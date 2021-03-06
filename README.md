# 자동차 경주 게임

## 진행 방법

* 자동차 경주 게임 요구사항을 파악한다.
* 요구사항에 대한 구현을 완료한 후 자신의 github 아이디에 해당하는 브랜치에 Pull Request(이하 PR)를 통해 코드 리뷰 요청을 한다.
* 코드 리뷰 피드백에 대한 개선 작업을 하고 다시 PUSH한다.
* 모든 피드백을 완료하면 다음 단계를 도전하고 앞의 과정을 반복한다.

## 온라인 코드 리뷰 과정

* [텍스트와 이미지로 살펴보는 온라인 코드 리뷰 과정](https://github.com/next-step/nextstep-docs/tree/master/codereview)

---

## 미션 분석

### 3단계

#### 요구사항 정리

- 입력은 자동차의 수, 이동 횟수
- 출력 시점은 정해져있지 않으며, 라인에 따라 '-'으로 나타낸다.
- 자동차는 무작위 결과에 따라 이동할 수도, 가만히 있을 수도 있다.

#### 구현할 사항

- 입력
    - [x] 자동차의 수, 움직임의 수를 각각 정수로 입력
    - [x] 음수, 정수가 아닌 문자, null 등 validation 처리
    - [x] 프롬프트를 차례대로 띄워주어야 함
- 출력
    - [x] 자동차의 실시간 위치 출력
- 자동차
    - [x] 위치 정보
    - [x] 이동 기능
        - [x] 무작위로 이동
    - [x] 이동 경로를 생성

#### 기타 요구사항 적용

- IntelliJ google-java-format 플러그인 사용

#### 1차 리뷰 결과

- `Car` 클래스
    - `Car` 클래스의 이동거리 상태 메소드 점검
        - 불필요한 If-statement
        - 확장 가능성 고려
            - 이동거리를 나타내는 Symbol 이 변경될 수 있다.
    - `SYMBOL` 의 접근 제어자 점검
    - `RandomlyMovingCar` 클래스에 사용한 `Random` 멤버의 `static` 선언 여부
    - `Car` 클래스가 스스로 자신의 움직임을 결정
        - 움직이는 방식에 따라 클래스가 확장되고 있음
- `Racing` 클래스
    - `ResultView` 클래스를 주입받아야만 경주를 시작할 수 있음
- `ResultView` 클래스
    - `Racing` 클래스와의 결합도
- `InteractiveInputView` 클래스
    - Primitive 타입들을 표현하기 위해 유틸리티 클래스로 사용하는 것을 고려

#### 개선할 내용 (중요도 순서로)

- [x] `Racing` 클래스와 `ResultView` 클래스의 결합도 낮추기
- [x] `ResultView` 에게 출력과 관련된 모든 역할을 위임
    - [x] 자동차를 표현할 형태가 변할 경우를 고려
- [x] `Car` 클래스의 이동 방법 개선
    - [x] `Random` 인스턴스 `static` 선언
- [x] `InputView` 의 유틸리티 클래스 정의

### 4단계

#### 요구사항 정리

- 자동차에 이름을 부여한다.
  - 이름은 최대 다섯 글자이다.
  - 사용자로부터 이름을 입력받을 때는 쉼표를 기준으로 구분한다.
- 자동차의 상태를 출력할 때, 자동차 이름도 같이 출력한다.
- 게임이 완료되면 우승자를 출력한다.
  - 우승자는 여러 명일 수 있다.

#### 구현할 기능

- 입력
  - [x] 자동차의 수 대신 이름을 입력받는다.
    - [x] 이름은 최대 다섯 글자까지 허용한다.
- 출력
  - [x] 자동차의 이름과 위치를 모두 출력한다.
  - [x] 경주가 끝나면 우승자를 출력한다.
    - [x] 우승자가 여러 명일 경우도 고려한다.

### 5단계

#### 요구사항 정리

- 비즈니스 로직을 가진 객체는 domain 패키지에, UI와 관련된 객체는 view 패키지에 구현한다.
- view 패키지의 객체가 domain 패키지의 객체에 의존할 수 있다.
- domain 패키지의 객체는 view 패키지에 의존하면 안된다.

#### 4단계 리뷰

- `Racing` 이 사용할 자동차 목록을 `Racing` 외부에 노출하지 않도록 개선
- 출력에 사용하는 자동차 목록은 불변 객체로 변경

#### 구현할 기능

- [x] `Racing` 이 사용하는 자동차 목록을 외부에 노출하지 않는다.
- [x] `Racing` 이 반환하는 객체와, 거기에 속한 멤버를 불변 객체로 만든다.
- [x] view - domain 패키지의 의존성을 확인하고, domain -> view 의존성이 존재하면 제거한다.
