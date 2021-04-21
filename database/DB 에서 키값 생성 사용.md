# DB 에서 키값 생성 사용

```java
@ Reviewer : 백정호
```

- DB 벤더마다 sequence, auto-increment 등 키 값을 생성하는 방법이 다르다.
- Mybatis 를 사용하여 개발하는 경우 MAX+1 은 절대 사용하면 안된다.
  - 많은 사람이 신청하는 프로그램 같은 경우, 동시성 문제가 발생할 수 있다.
  - 같은 키 값이 물리게 되는 경우 내 개인정보에 다른 사람 개인정보로 업데이트 될 수도 있다.
  - 따라서, sequence 나 auto-increment 를 사용해야 한다.


- MAX+1 사용 금지
  - 이렇게 사용하는 경우는 아마 DB 벤더마다 키 값 생성하는 방법이 다르기 때문에 SQL 을 바꿔줘야해서 귀찮아서 이렇게 쓴 것이다.
 
```xml
<selectKey keyProperty="articleSeq" resultType="Integer" order="BEFORE">
    SELECT
    IFNULL(MAX(ARTICLE_SEQ),0) +1
    FROM
    MEC_C_ARTICLE
</selectKey>
```
