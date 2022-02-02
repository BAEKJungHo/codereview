# entity에서 dto를 파라미터로 받아서 entity로 변환하는 작업

> reviewer : [원유라](https://github.com/dbeod2)

entity 에서 dto 를 파라미터로 받아서 entity로 변환하는 작업을 하고있는데요.
dto 는 view 요구사항에 맞추어 만들어진 객체이기 때문에 변경이 자주 일어 날 수 있는데요.
이때 entity 에서 dto 를 참조한다면 dto 의 변경이 있을때 entity 도 함께 변경되어야 할 수 있어요.
그렇기 때문에 `entity <> dto` 간의 변환은 `dto` 혹은 `converter` 레이어를 따로 두어서 처리하는 방식으로 처리해보면 어떨지 의견드립니다.🙏
