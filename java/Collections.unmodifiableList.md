# Collections.unmodifiableList

> @ Reviewer : 원유라

### Before

```java
public List<Section> getSections() {
    return sections;
}
```

외부에서 getSections() 를 사용하여 sections 의 값을 변경시킬 수 있다.

### After

```java
public List<Section> getSections() {
    return Collections.unmodifiableList(sections);
}
```

외부에서 getSections() 를 사용하더라도 읽기만 제공되기 때문에 안전하다.
