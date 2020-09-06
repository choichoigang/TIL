# Process & Thread

## Process

명령어 코드 등의 정적인 데이터들의 집합인 프로그램을 실행시키고 동작하는 행동을 Process라고 하며 Process는 운영체제로부터 시스템 자원을 할당 받아서 작업을 진행하게 됩니다.

![스크린샷 2020-09-07 오전 12 37 12](https://user-images.githubusercontent.com/49897409/92330092-3fdeeb80-f0a7-11ea-9ae1-c94a11e8b3fb.png)

프로세스는 위와 같이 크게 4개의 영역으로 구성이 되어 있습니다. 각각의 영역은 아래와 같은 의미와 역할을 합니다.

1. Code : Program의 코드가 담긴 영역
2. Data : 전역 변수 영역
3. Heap : 동적으로 할당되는 메모리의 영역
4. Stack : 매개변수 , 지역 변수 등 임시적인 데이터의 영역

![스크린샷 2020-09-07 오전 12 46 04](https://user-images.githubusercontent.com/49897409/92330098-49685380-f0a7-11ea-96c7-4b531c1cbf2d.png)

운영체제 아래서 여러 프로세스가 동작하며 각 프로세스의 실행은 동시성과 병렬적으로 작동합니다.
또한 각각의 프로세스는 운영체제 아래에서 PCB(Process Control Block) 라고 불립니다.

- PID(Process ID) : 프로세스를 식별할 수 있는 ID
- 프로세스 상태 : new , ready , running , waiting , halted 로 프로세스의 상태를 표현합니다.
- 프로그램 카운터 : 다음에 실핼될 명령어의 주소
- 스케줄링 정보 : 우선순위

**동시성 (Context Switching)** : 여러 프로세스를 돌아다니면서 작업을 진행하는 일을 뜻합니다. 이 진행이 매우 빨라서 사용자의 입장에서는 동시에 여러가지 작업이 발생하는 듯 한 경험을 하게 됩니다.

**병렬적** : 요즘 CPU의 옥타코어 쿼드코어 등은 하나의 CPU에 여러 코어를 추가해서 그 코어들이 프로세서 안에서 여러가지 작업을 병렬적으로 실행할 수 있습니다.

## Thread

![스크린샷 2020-09-07 오전 12 53 55](https://user-images.githubusercontent.com/49897409/92330107-5422e880-f0a7-11ea-90ff-861e8a8620dc.png)

Thread는 프로세스 내부에서 실행되는 흐름 입니다.
큰 특징은 Text , data , Heap을 공유하고 각각의 Thread는 별도로 Stack을 가진다는 특징이 있습니다.

여러 자원을 공유하는 만큼 Thread-safe 하게 상태를 관리해야 한다는 특징이 있습니다. 하지만 Process와 다르게 자원을 공유하는 특징은Process 에서 발생하는 Context Switching 만큼 많은 자원을 소모하지 않는다는 특징 또한 가지고 있습니다.

## 차이와 장단점은 ?

- Process는 독립적인 데이터를 가집니다. 그래서 Multi-Process를 진행할 경우에 하나의 Process에 문제가 생겨도 다른 Process에는 영향을 주지 않습니다. 하지만 Multi-Thread 같은 경우에는 자원을 공유하는 만큼 Thread-safe하게 상태를 유지하지 못한다면 연쇄적으로 다른 Thread에 영향을 줄 수 있습니다.
- Process는 **Context Switching** 비용이 큰 반면 Thread는 Process에 비해서 적은 비용을 가집니다.
