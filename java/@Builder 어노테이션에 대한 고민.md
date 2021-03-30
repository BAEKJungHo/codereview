# @Builder 어노테이션에 대한 고민

```java
@ Reviewer : 카카오 박준영
```

- 원본 리뷰 내용


[sup2is](https://github.com/sup2is) 음 .. 저는 이 @builder 애너테이션에 대해서 조금 고민이 있는 편인데..
제가 생각하는 @builder의 단점은 객체를 생성할때 nullable한 필드를 포함해서 특정 필드를 생략하고 객체를 생성할 수 있다는것인데요
준영님은 혹시 이부분에 대해서 어떻게 생각하시는지 궁금합니다~~! 

[ces518](https://github.com/ces518) 말씀하신 부분은 충분히 고민할 수 있는 내용이고 저도 예전에 고민을 했던 부분인데요.
우선 기본적으로 Builder 는 저는 테스트용도나 혹은 DTO to Entity 와 같은 용도로 사용하는 곳을 제한합니다.
서비스코드에서 Builder를 직접 호출하다보면 말씀하신대로 휴먼에러가 발생할수도 있고, 의도한대로 생성이 되지 않을수도 있어요.
사실 그런 Null 관련 해서는 Assertion 을 해서 처리한다거나 할수도 있지만 대부분의 자바 메소드가 Null 에 취약해서 하나하나 다 체크하다보면 대부분의 코드가 Null 체크 관련으로 도배될수도 있어요.
어느정도 협의된 상황에서 호출 할것이다 라고 가정을 하고 설계를 하기 때문에 그런 부분은 크게 걱정하지 않으셔도 될것 같습니다 ~


- Example Source

```java
@JoinColumn(name = "menu_id")
private List<MenuOptionGroup> optionGroups = List.of();

@Builder
```


