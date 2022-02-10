# DTO 에 로직 처리 코드가 존재하면 안된다

> @ Reviewer : 우아한형제들 강현구

DTO 에 아래와 같은 코드가 있었다. 역을 정렬하여 응답으로 내보내기 위한 로직이다.

```java
public static List<StationResponse> toStations(final Sections sections) {
    List<Station> stations = isRequiredReOrdering(sections)
            ? createStations(createReOrderedSections(sections))
            : createStations(sections.getSections());

    return stations.stream()
            .map(StationResponse::of)
            .sorted(Comparator.comparing(StationResponse::getId))
            .collect(Collectors.toList());
}
```

## Feedback

> https://github.com/next-step/atdd-subway-favorite/pull/158

- 1차 피드백

```
dto는 단순 값만 옮기는 클래스입니다.
삼항연산자나 if, 비교문 같은 로직 처리가 있으면 안 됩니다.
이 메서드를 포함해서 isRequiredReOrdering, createReOrderedSections, reOrderSections, createStations 메서드를 적절한 도메인 클래스로 옮겨보시기 바랍니다.
```

- 나의 답변

```
dto는 단순 값만 옮기는 클래스입니다.

말씀하신 dto 에 대한 정의는 동의합니다. 제가 저 코드를 DTO 에 넣은 이유는, 노선을 조회할 때, StationResponse 목록을 응답에 담에서 반환하는데요,,

역 응답 정보를 생성하는 로직 은 엔티티보다 DTO 에 정의하는 것이 더 역할과 책임을 잘 구분한게 아닌가 생각이 듭니다.

도메인 클래스로 역에 대한 응답 정보를 생성하는 로직을 넣게되면, 응답 정보 생성 로직이 바뀔 때마다, 도메인이 수정되어야 한다고 생각이 드는데

isRequiredReOrdering, createReOrderedSections, reOrderSections, createStations 해당 메서드들이 응답 생성 로직 뿐만 아니라 다른 곳에서도 사용이 된다고 하면, 그때 리팩토링을 고민해도 될 것 같다고 저는 생각합니다.

현구님 생각은 어떠신가요?
```

- 2차 피드백

```
제가 보기엔 역 응답 정보를 생성하는게 아니라 정렬하는 로직으로 보이네요.
응답 정보를 생성한다는 범위를 넓게 잡으셔서 dto에 추가하신 것 같습니다.
지하철 역 순서를 정하는 것은 뷰의 책임이 아닌 도메인이 책임을 가져가는 게 맞습니다. 정렬 로직이 바뀌면 당연히 도메인 객체도 수정해야하구요.
프로그램을 http가 아닌 콘솔이나 모바일로 확장한다고 했을 때 dto에 정렬 로직을 두면 http, 콘솔, 모바일 세 곳에 중복 코드가 발생합니다.
```

## 정리

- 리뷰어님이 DTO 에 삼항연산자나 if, 비교문 같은 로직 처리가 있으면 안 된다고 한 이유는 다음과 같다.
- DTO 에 로직들이 존재하는 경우, 프로그램을 콘솔이나 모바일로 확장한다고 했을 때 DTO 에 로직을 두면, http, 콘솔, 모바일 세 곳에 중복 코드가 발생하기 때문이다.
