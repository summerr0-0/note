# command-pattern
* 객체의 행동을 객체화 하는 디자인패턴
* 요청 자체를 캡슐화 해서 호출자와 수신자를 분리한다
* 확장성 : Command 클래스를 추가하는 방식으로 새로운 명령을 수비게 추가할 수 있다
* 분리 : 요청의 발생과 실행을 분리시켜 호출자는 수신자의 인터페이스를 알 필요 없이 명령을 실행할 수 있다

### 구성
Originator(발생자)
Command: 실행될 각 명령에 대한 인터페이스입니다. 이 인터페이스는 execute 메소드를 선언하여 명령을 수행하는데 사용합니다.

ConcreteCommand: Command 인터페이스를 구현하는 클래스로, 호출될 수신자의 특정 메소드를 호출하는 execute 메소드를 구현합니다.

Receiver: 요청에 대한 실제 작업을 수행하는 객체입니다. ConcreteCommand는 이 객체의 메소드를 호출하여 명령을 실행합니다.

Invoker: Command 객체를 사용하며, 요청을 수행하는 execute 메소드를 호출합니다. 이는 리모콘, 메뉴, 큐 등 여러 형태가 될 수 있습니다.

Client: Command 객체를 생성하고 수신자를 설정합니다. 또한 Invoker 객체에 Command 객체를 연결합니다

### 대표적 예시
* Runndable 인터페이스가 Command 역할
  * run메소드와 분리

