# 함수형 프로그래밍(Functional Programming, FP) 기반의 소프트웨어 디자인과 개발을 Java와 Spring Boot에서 적용하기 위한방법

1.함수형 프로그래밍 개념: 함수형 프로그래밍의 핵심 원칙과 기본 개념들을 이해하는 것이 중요합니다. 불변성(Immutability), 순수 함수(Pure functions), 고차 함수(Higher-order functions), 커링(Currying) 등의 개념을 공부해보세요.

2.Java 8 이상의 함수형 프로그래밍 기능: Java 8부터 함수형 프로그래밍 지원이 개선되었습니다. 람다 표현식(Lambda expressions), 스트림 API(Stream API), Optional 클래스 등을 활용하여 코드를 더 간결하게 작성할 수 있습니다.

3.함수형 인터페이스(Functional Interfaces): Java에서는 함수형 인터페이스를 통해 람다 표현식을 사용할 수 있습니다. java.util.function 패키지의 함수형 인터페이스를 활용하거나, 필요한 경우 사용자 정의 함수형 인터페이스를 작성할 수 있습니다.

4.Spring Boot에서의 함수형 프로그래밍: Spring Framework 5와 Spring Boot 2부터는 리액티브 프로그래밍(Reactive Programming)과 함수형 프로그래밍을 지원합니다. WebFlux와 Reactor를 사용하여 리액티브 웹 애플리케이션을 구축하고, 함수형 라우팅과 함수형 스타일로 코드를 작성할 수 있습니다.

5.비동기 프로그래밍 및 병렬 처리: Java와 Spring Boot에서는 CompletableFuture, Parallel Stream 등을 활용하여 비동기 프로그래밍과 병렬 처리를 구현할 수 있습니다. 이를 통해 성능을 향상시키고, 응답성 있는 애플리케이션을 구축할 수 있습니다.


# 고차 함수(Higher-order functions)와 커링(Currying)

고차 함수는 다음 중 하나 이상을 수행하는 함수입니다.

하나 이상의 함수를 인자로 받습니다.  
함수를 결과로 반환합니다.  
커링은 여러 개의 인자를 받는 함수를 하나의 인자를 받는 함수로 변환하는 기법입니다.  
이렇게 하면 함수를 일부 인자에 대해 적용한 후, 다른 인자에 대해 나중에 적용할 수 있습니다.

Java에서 고차 함수와 커링 예제를 살펴보겠습니다.

```
import java.util.function.Function;

public class HigherOrderFunctionAndCurrying {

    public static void main(String[] args) {
        // 고차 함수 예제
        Function<Integer, Function<Integer, Integer>> add = x -> y -> x + y;
        Function<Integer, Integer> add5 = add.apply(5);
        System.out.println(add5.apply(3)); // 결과: 8

        // 커링 예제
        Function<Integer, Function<Integer, Integer>> multiply = x -> y -> x * y;
        Function<Integer, Integer> multiplyBy3 = multiply.apply(3);
        System.out.println(multiplyBy3.apply(4)); // 결과: 12
    }
}
```



함수 위임은 한 함수의 기능을 다른 함수에 위임하여 처리하는 것입니다.  
이를 통해 코드의 재사용성과 가독성을 높일 수 있습니다.

```
import java.util.function.Function;

public class FunctionDelegation {

    public static int square(int x) {
        return x * x;
    }

    public static int cube(int x) {
        return x * x * x;
    }

    public static int calculate(Function<Integer, Integer> function, int x) {
        return function.apply(x);
    }

    public static void main(String[] args) {
        System.out.println(calculate(FunctionDelegation::square, 5)); // 결과: 25
        System.out.println(calculate(FunctionDelegation::cube, 3)); // 결과: 27
    }
}
```


이 예제에서는 calculate 함수에 기능을 위임하고 있습니다.  
square 함수와 cube 함수를 인자로 전달하여 해당 함수를 호출하고 결과를 반환합니다.  
이를 통해 코드의 가독성과 유연성을 높일 수 있습니다.

# 함수형 프로그래밍(Functional Programming, FP) 기반으로 프로젝트 아키텍처 설계

```
sample_project
│
├── src
│   └── main
│       └── java
│           ├── model
│           │   ├── User.java
│           │   └── Post.java
│           │
│           ├── repository
│           │   ├── UserRepository.java
│           │   └── PostRepository.java
│           │
│           ├── service
│           │   ├── UserService.java
│           │   └── PostService.java
│           │
│           └── Application.java
│
└── pom.xml
```


모델(Model): User.java와 Post.java와 같은 도메인 모델을 불변 객체(Immutable Objects)로 정의합니다.  
이를 통해 상태 변화에 의한 부작용을 줄일 수 있습니다.

```
// User.java 예시
public final class User {
    private final String id;
    private final String name;

    public User(String id, String name) {
        this.id = id;
        this.name = name;
    }

    public String getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}
```


저장소(Repository): 저장소 계층에서는 불변 객체를 저장 및 조회하는 메서드를 제공합니다.  
순수 함수를 사용하여 사이드 이펙트를 최소화하고,  
고차 함수를 사용하여 코드 재사용성을 높입니다.

```
// UserRepository.java 예시
public interface UserRepository {
    Optional<User> findById(String id);
    List<User> findAll();
    User save(User user);
    void deleteById(String id);
}
```


서비스(Service): 서비스 계층에서는 비즈니스 로직을 구현합니다.  
순수 함수와 고차 함수를 사용하여 로직을 구현하고, 각 함수의 의존성을 최소화합니다.

```
// UserService.java 예시
public class UserService {
    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    public Optional<User> findUserById(String id) {
        return userRepository.findById(id);
    }

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    public User createUser(String id, String name) {
        User user = new User(id, name);
        return userRepository.save(user);
    }

    public void deleteUserById(String id) {
        userRepository.deleteById(id);
    }
}
```


애플리케이션(Application):  
Application.java에서 의존성 주입을 통해 서비스 객체를 생성하고 실행합니다.

```
public class Application {

    public static void main(String[] args) {

    }
}
```