# Case Study: Layered Architecture Implementation

## Overview Diagram

```
+---------------------------------------------+
|           Presentation Layer                |
|     (Controllers, Views, API Endpoints)     |
+---------------------------------------------+
|           Application Layer                 |
|      (Use Cases, Application Services)      |
+---------------------------------------------+
|             Domain Layer                    |
|  (Entities, Value Objects, Domain Services) |
+---------------------------------------------+
|          Infrastructure Layer               |
|   (Repositories, External Services, DB)     |
+---------------------------------------------+
```

---

## Package Structure

```
com.example.app/
+-- presentation/
|   +-- api/
|   |   +-- UserController.java
|   |   +-- dto/
|   |       +-- CreateUserRequest.java
|   |       +-- UserResponse.java
|   +-- validation/
+-- application/
|   +-- usecase/
|   |   +-- CreateUserUseCase.java
|   +-- command/
|       +-- CreateUserCommand.java
+-- domain/
|   +-- model/
|   |   +-- User.java
|   |   +-- UserId.java
|   |   +-- Email.java
|   +-- repository/
|   |   +-- UserRepository.java
|   +-- service/
|       +-- UserDomainService.java
+-- infrastructure/
    +-- persistence/
    |   +-- JpaUserRepository.java
    |   +-- entity/
    |       +-- UserEntity.java
    +-- external/
        +-- EmailServiceAdapter.java
```

---

## Implementation Examples

### Presentation Layer

```java
@RestController
@RequestMapping("/api/users")
class UserController {
    private final CreateUserUseCase createUser;

    @PostMapping
    Response create(@Valid @RequestBody CreateUserRequest request) {
        CreateUserCommand command = request.toCommand();
        User user = createUser.execute(command);
        return Response.created(UserResponse.from(user));
    }
}
```

### Application Layer

```java
class CreateUserUseCase {
    private final UserRepository userRepository;
    private final EventPublisher events;

    @Transactional
    User execute(CreateUserCommand command) {
        Email email = new Email(command.email());

        if (userRepository.existsByEmail(email)) {
            throw new EmailAlreadyExistsException(email);
        }

        User user = User.create(command.name(), email);
        User saved = userRepository.save(user);

        events.publish(new UserCreatedEvent(saved));
        return saved;
    }
}
```

### Domain Layer

```java
// Entity
class User {
    private UserId id;
    private Name name;
    private Email email;
    private UserStatus status;

    static User create(String name, Email email) {
        return new User(
            UserId.generate(),
            new Name(name),
            email,
            UserStatus.PENDING_VERIFICATION
        );
    }

    void verify() {
        if (status != UserStatus.PENDING_VERIFICATION) {
            throw new InvalidStateException("User already verified");
        }
        this.status = UserStatus.ACTIVE;
    }
}

// Value Object
record Email(String value) {
    Email {
        if (!isValid(value)) {
            throw new InvalidEmailException(value);
        }
    }
}

// Repository Interface (in domain layer)
interface UserRepository {
    Optional<User> findById(UserId id);
    User save(User user);
}
```

### Infrastructure Layer

```java
@Repository
class JpaUserRepository implements UserRepository {
    private final JpaUserEntityRepository jpa;
    private final UserMapper mapper;

    @Override
    public Optional<User> findById(UserId id) {
        return jpa.findById(id.value())
            .map(mapper::toDomain);
    }

    @Override
    public User save(User user) {
        UserEntity entity = mapper.toEntity(user);
        UserEntity saved = jpa.save(entity);
        return mapper.toDomain(saved);
    }
}
```
