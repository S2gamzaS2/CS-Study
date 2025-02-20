# Blocking & Non-Blocking I/O

## I/O란?

I/O는 입력(Input)과 출력(Output)의 약자로 운영체제에서 I/O는 일반적으로 컴퓨터 시스템이 외부 입출력 장치들과 데이터를 주고 받는 것을 의미

> I/O 작업은 User레벨에서 직접 수행할 수 없고, 실제 I/O 작업을 수행하는 위치는 Kernel(운영체제)에서만 가능

<br>

## Blocking I/O

Blocking I/O는 I/O 작업이 진행되는 동안 User Process가 자신의 작업을 중단한 채, I/O가 끝날때까지 대기하는 방식

> 가장 기본적인 I/O 모델로, Linux에서의 모든 소켓 통신은 기본 Blocking으로 동작

![Blocking I/O](/Network/images/blockingio.png)

- **Blocking I/O의 동작 과정**

1. Process(Thread)가 Kernel에게 I/O를 요청하는 함수를 호출 (**제어권을 넘김**)

2. Process(Thread)는 작업 결과를 반환 받을 때까지 대기 (**제어권을 넘겨줬으므로 대기, 기존 작업 제어 불가**)

3. Kernel이 작업을 완료하면 작업 결과를 반환 받음 (**제어권을 다시 넘겨받음**)

<br>

이 경우, 말 그대로 Block이 되고, 애플리케이션에서 다른 작업을 수행하지 못하고 대기하게 되므로 Resource 낭비가 발생함

> 예를 들어, 카카오톡이 사용자가 메세지를 전송할 때까지 대기하고 있는거라고 생각하면 됨 (이 외에 다양한 기능이 많음에도 불구하고)

<br>

## Non-Blocking I/O

Non-Blocking I/O는 작업이 진행되는 동안 User Process의 작업을 중단하지 않고 I/O 호출에 대해 즉시 리턴하고, User Process가 이어서 다른 일을 수행할 수 있도록 하는 방식

> Blocking 방식의 비효율성을 극복하고자 도입된 방식

![Non-Blocking I/O](/Network/images/nonblockingio.png)

- **Non-Blocking I/O의 동작 과정**

1. Read I/O를 하기 위해 System Call 요청

2. Kernel의 I/O 작업 완료 여부와 관계없이 즉시 응답 (**입력 데이터가 없으면 결과 메세지(EWOULDBLOCK)을 반환**)

3. 애플리케이션은 I/O 작업이 완료되기 전에 다른 작업을 수행 가능 (**제어권을 넘겨주지 않았기 때문에 계속해서 작업이 가능**)

4. 애플리케이션은 다른 작업 수행 중간에 System Call을 보내 I/O가 완료되었는지 Kernel에 요청하고, 완료되면 I/O 작업을 완료

<br>

모든 작업 수행이 I/O의 진행 시간과는 관계없이 빠르게 동작하므로, User Process는 자신의 작업을 오랜시간 중지하지 않고도 I/O 처리를 수행할 수 있음

> 그러나 반복적으로 System Call이 발생하기 때문에 이것 또한 Resource 낭비가 발생함

<br>

**그래서 Non-Blocking I/O 문제인 반복적인 System Call 호출을 해결하기 위해 I/O 이벤트 통지 모델을 도입**

> Synchronous/Asynchronous 모델로 분류

<br>

### 참고자료

https://didu-story.tistory.com/307

https://velog.io/@octo__/BlockingNon-Blocking-IO-IO-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%ED%86%B5%EC%A7%80-%EB%AA%A8%EB%8D%B8

https://etloveguitar.tistory.com/140
