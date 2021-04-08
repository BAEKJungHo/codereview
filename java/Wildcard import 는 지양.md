# Wildcard import 는 지양

```java
@ Reviewer : 카카오 박준영
```

- 원본 리뷰 내용

wildcard import 는 지양해주세요 (테스트 코드 에선 예외로 허용) ~

- Example Source

```java
import lombok.*;
```

- 지양 해야하는 이유

다중 import 를 할 경우 인텔리제이가 와일드카드로 자동으로 변경시키곤 하는데 직접 변경해줘야 한다.

컴파일 타임이 늦어지는 경우도 있고, 클래스명이 동일한게 존재할 경우 충돌이 발생하는 이슈도 있기 때문이다.

