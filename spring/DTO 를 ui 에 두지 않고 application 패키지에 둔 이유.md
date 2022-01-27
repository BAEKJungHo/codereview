# DTO 를 ui 에 두지 않고 application 패키지에 둔 이유

> @Reviwer : 류성현(브라운)

- 각 Layer 간의 의존관계를 생각해서 설계함
- DTO 객체가 ui 에 있으면 Service 가 Controller 에 의존하게됨
  - 즉, 의존 관계의 방향이 역으로 흘러가게 된다.
- __DTO 가 application 패키지에 있으면 Presentation Layer 는 Application Layer 에 원래 의존 관계를 갖고 있기 때문에 의존 관계를 한 방향을 보게끔 설계__
