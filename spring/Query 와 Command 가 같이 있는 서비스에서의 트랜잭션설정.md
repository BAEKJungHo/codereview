# Query 와 Command 가 같이 있는 서비스에서 트랜잭션을 설정하는 방법

```java
@ Reviewer : 카카오 박준영
```

- Example Source

```java
@RequiredArgsConstructor
@Service
public class UserService {

  // command
  
  // query
  
  ...

}
```

위와 같은 구조일 때 `@Transactional` 설정을 기본을 ReadOnly 로 잡고 Command 를 수행하는 메서드에 Transacational 을 선언해주는것이 잘못된 트랜잭션 경계를 선언하는걸 방지할 수 있다.

