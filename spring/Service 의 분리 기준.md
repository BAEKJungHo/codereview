# SectionService 는 분리하는게 맞는가?

> @ Reviewer : 카카오 모빌리티 최준우

### Q

구간을 만들때, REST API를 설계하는 것은 어렵지 않았습니다. 구간은 노선이 등록되고 나서야 등록 가능하기 때문에, `노선/노선ID/구간/구간ID` 형식으로 URI 를 표현하는게 맞다고 생각이 들었습니다.

문제는 Service 였는데,, 제가 한 고민은 아래와 같습니다.

1. LineService 에서 Section 의 등록, 삭제 등의 유스케이스를 나타내게 되면 기존에 없던 StationRepository 의존성이 생긴다고 판단하여 분리하였습니다.

2. 현재 서비스들이 application 패키지 밑에 존재하며, 실제로 하고 있는 역할도 도메인 서비스가 아닌 유스케이스를 구현하는 `애플리케이션 서비스`라고 생각합니다. 

애플리케이션 서비스이다보니, Presentation Layer (LineController) 에 있는 API 기능을 LineService 에 관리하도록 하는게 맞는것 같기도하고..

Line Entity 와 Section Entity 의 관게를 보면, Line Entity 에서 Section Entity 의 생명주기를 관리하고 있긴하지만,, 애플리케이션 서비스의 분리 기준이 생명주기로 하는게 맞는거가 의문이 들기도하였습니다.

- __가장 궁금한 내용 : 애플리케이션 서비스를 분리하는 기준__
    - 1. 각 애플리케이션 서비스의 Persistence Layer 에 대한 의존성을 고려하여 분리하는 지
    - 2. 유스케이스를 기준으로 분리하는 지
    - 3. 엔티티의 생명주기를 기준으로 분리하는 지

그 외에도 다양한 기준이 있을 것 같긴한데,, 

이에 대한 리뷰어님 생각은 어떠신지 궁금합니다.

### A

저 같은 경우는 유스케이스를 기준으로 분리를 하게되는 편입니다. 현재는 간단한 미션이지만 실무를 진행하면서 요구사항이 복잡해질수록 처음에는 UserService로 시작했다가, UserQueryService, UserSignUpService, UserUpdateService 등과 같이 나누어 지게 되더라구요............
정호님은 사용하셨을 때 어떤 방향이 가장 좋은 것 처럼 느껴지셨는지도 궁금하네요 👀

### A. 나의 답변

저는 주로 `Persistence Layer 에 대한 의존성을 고려하여 분리` 하였던거 같은데,,
저 기준을 사용한 제 나름대로의 이유는 아래와 같았습니다.

- 이미 Controller 의 핸들러메서드를 통해 자원과 행위가 표현되어 있기 때문에
`lines/{lilneId}/sections/{sectionId}` 를 보고 두 관계를 유추할 수 있다고 봅니다.

- LineService 에서 Section 을 관리하게되면 자연스럽게 StationRepository 의존성이
생기고, 지금은 섹션을 등록하기 위해서 StationService 에서 재사용해야하는 메서드가 `findStationById` 하나지만,, 이게 재사용해야하는 메서드가 많으면 많을 수록 누군가는 `Repository` 가 아닌 `Service` 에 의존하게끔 설계할 수도 있다고 생각합니다.

- 그렇게 되면 설계에 따라서 `순환참조` 발생가능성이 더 커진다고 봐서 저는 Persistence Layer 에 대한 의존성을 고려해서 분리했던것 같습니다.

- 만약에 LinService 에 명령과 쿼리로 분리되어 FindService, CommandService 이런식으로 나누어진다고 했을때, 과연 saveSection 은 어느 Service 에 들어가야 하는가를 생각해봤을때

- CommandService 에 들어가는 순간 FindService 에 있는 조회 메서드를 재사용할 수 없어서 메서드를 새로 만들어서 사용해야 하지 않을까 합니다. 또한 saveSection 에 대한 검증을 진행하기 위해서 CommandService 는 자연스럽게 StationRepository 의존성을 받게되므로, 누군가 __LineCommandService 를 보았을때, 노선을 등록하기 위해서 역에 의존하고 있다라고 보여질 수도 있다고 봅니다.__

- 이게 지금은 애플리케이션 서비스하나만 사용하고 있긴하지만, 도메인으로부터 파생된 로직을 `도메인 서비스`로 만들어서 관리하다고 했을때 아래와 같이 되지않을까 합니다.

```java
public class LineQueryService {
    private final LineRepository lineRepository;
    private final LineService lineService; // 도메인 서비스
    private final SectionService sectionService; // 도메인 서비스
    private final StationRepository stationRepository; // 구간 조회 시 필요

    // LineQueryService 에 현재 도메인서비스가 존재하는 이유는, 노선과 구간을 각각 조회할때 필요한 도메인 로직이 있다고 가정

    // 생략
}
```

- LineQueryService 에 벌써 많은 의존성이 생기고, 실제로 이렇게 되면 __필드 부분(어떤 의존성을 주입 받고 있는지)을 보더라도 라인과 구간의 관계를 명확하게 파악하기 힘들다고 보여집니다.__

> 라인과 구간의 관계는 REST API 설계와 엔티티간의 관계를 통해서 파악하는게 훨씬 빨라지게 된다고 생각합니다.
