# BaseEntity 에 Serializable 을 구현한 이유?

> @ Reviewer : 카카오 박준영

우선 JPA 표준 스펙이 Serializable 을 구현해야 하는 것으로 알고 있고, JPA 구현체로 하이버네이트를 사용할텐데, 하이버네이트가 엔티티 생성 및 관리하는 복잡한 사이클을
단순화 하기 위해 임시로 `직렬화`한 뒤 메모리에 캐싱해뒀다 사용하는 케이스가 발생해서, 구현하는 방향이 좋아요.

## JPA Entity에 Serializable을 꼭 상속해야 할까 ?

직렬화는 JVM 메모리에 상주되어 있는 객체 데이터를 그대로 영속화 할 때 사용된다. 시스템이 종료 되더라도 없어지지 않고 영속화 되어 네트워크로 전송도 가능하다.

따라서, 이 객체를 어딘가로 전송하거나 세션에 기록하거나 등등 정말 serialization 을 위한 용도가 아니라면, Hibernate 상에서는 굳이 Serializable 을 구현하지 않아도 되지만, 그래도 Serializable 을 구현하는 것이 좋은 습관이라고 한다.

> [Do JPA entities have to be Serializable](https://bvaisakh.wordpress.com/2015/03/04/do-jpa-entities-have-to-be-serializable/)
