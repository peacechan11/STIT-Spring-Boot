# Spring Boot Annotations and Dependency Injection Notes

# @Component

`@Component` က အခြေခံ Annotation ဖြစ်ပြီး Spring Bean အဖြစ် Register လုပ်ပေးသည်။

## Example

```java
@Component
public class EmailUtil {

    public void sendEmail() {
        System.out.println("Sending Email");
    }
}
```

## Usage

- Utility Classes
- Helper Classes
- Common Components

---

# @Service

`@Service` သည် Business Logic Layer အတွက် အသုံးပြုသည်။

## Example

```java
@Service
public class UserService {

    public String getUser() {
        return "John";
    }
}
```

## Responsibilities

- Business Rules
- Validation
- Calculations
- Processing Logic

---

# @Repository

`@Repository` သည် Database Access Layer အတွက် အသုံးပြုသည်။

## Example

```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> {

}
```

## Special Feature

Database Exception များကို Spring DataAccessException အဖြစ် Translate လုပ်ပေးနိုင်သည်။

---

# @Autowired

`@Autowired` သည် Dependency Injection (DI) Annotation ဖြစ်သည်။

Spring IoC Container ထဲရှိ Bean ကို အလိုအလျောက် Inject လုပ်ပေးသည်။

## Example

```java
@Service
public class UserService {

    @Autowired
    private UserRepository repository;
}
```

---

# Dependency Injection Types

## 1. Field Injection

```java
@Service
public class UserService {

    @Autowired
    private UserRepository repository;
}
```

### Pros

- Simple

### Cons

- Difficult to test
- Hidden dependencies

---

## 2. Setter Injection

```java
@Service
public class UserService {

    private UserRepository repository;

    @Autowired
    public void setRepository(UserRepository repository) {
        this.repository = repository;
    }
}
```

### Usage

Optional Dependencies

---

## 3. Constructor Injection (Recommended)

```java
@Service
public class UserService {

    private final UserRepository repository;

    public UserService(UserRepository repository) {
        this.repository = repository;
    }
}
```

### Advantages

- Immutable Objects
- Easier Unit Testing
- Better Design
- Detects Circular Dependencies

---

# IoC + DI + Autowired

## IoC (Inversion of Control)

Object Creation ကို Spring Container က ထိန်းချုပ်သည်။

```text
Developer
   ❌ new UserService()

Spring Container
   ✅ create UserService()
```

---

## DI (Dependency Injection)

လိုအပ်သော Dependency များကို Inject လုပ်ပေးခြင်း။

```text
UserService
     |
     v
UserRepository
```

---

## Autowired

Dependency Injection ကို အကောင်အထည်ဖော်ရန် အသုံးပြုသော Annotation ဖြစ်သည်။

```java
@Autowired
private UserRepository repository;
```

---

# Flow Diagram

```text
Client
   |
   v
Controller
   |
   v
@Service
UserService
   |
   v
@Repository
UserRepository
   |
   v
Database
```

---

# Comparison Table

| Annotation | Purpose |
|------------|----------|
| @Component | General Spring Bean |
| @Service | Business Logic Layer |
| @Repository | Database Access Layer |
| @Autowired | Dependency Injection |

---

# Interview Questions

## What is @Autowired?

Dependency Injection Annotation used to automatically inject Spring Beans.

## Can @Autowired work without Bean annotations?

No.

The target class must be a Spring Bean:

- @Component
- @Service
- @Repository
- @Controller

## Which Injection Type is Recommended?

Constructor Injection

## Why Constructor Injection?

- Easier Testing
- Immutable Dependencies
- Better Maintainability
- Spring Best Practice

---

# Key Point

```text
@Component
    ↑
    ├── @Service
    ├── @Repository
    └── @Controller
```

@All specialized annotations are based on @Component.

Modern Spring Boot projects generally prefer Constructor Injection over Field Injection.
