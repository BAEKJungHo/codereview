# Service 인터페이스 추상화

> @ Reviewer : [우아한형제들 강현구](https://github.com/kang-hyungu)

내 피드백은 아니고, 다른분 피드백에 좋은 내용이 있어서 가져왔다.

```java
public class UserDetailsServiceImpl implements UserDetailService {

// 생략

```

## Feedback

- 구현체가 하나밖에 없는데 왜 인터페이스로 추상화하셨나요?
- 매번 수정사항이 생길때마다 인터페이스도 같이 수정해야되겠네요.
- 언제 인터페이스가 필요해질지 모르는데 미리 만들어둘 필요는 없어보입니다.
