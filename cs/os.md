## 소프트웨어 > 운영체제 > 프로그램 > 프로세스 > 스레드

### 프로그램
> 파일이 저장 장치에 저장되어 있지만 메모리에는 올라가 있지 않은 정적인 상태.
### 프로세스
> 운영체제로부터 자원을 할당받은 작업의 단위.
### 스레드
> 프로세스가 할당받은 자원을 이용하는 실행 흐름 단위.

1. 메모리에 올라가 있지 않은: 아직 운영체제가 프로그램에게 독립적인 메모리 공간을 할당해주지 않았다는 뜻.
모든 프로그램은 운영체제가 실행되기 위한 메모리 공간을 할당해줘야 실행될 수 있음.
2. 정적인 상태: 단어 그대로 움직이지 않는 상태. 아직 실행되지 않고 있는 상태.

프로그램이 실행되는 순간 해당 파일은 컴퓨터 메모리에 올라가게 되고, 이 상태를 동적인 상태라고 하며 이 상태의 프로그램에 정의된 프로세스들의 집합이 순차적으로 실행된다.
따라서 위키피디아에서는 프로세스에 대한 정의를 내릴 때 그냥 `실행되고 있는 컴퓨터 프로그램`이라고 정의 내린다.
스케줄링 단계에서의 `작업`과 같은 개념과 비슷하다고 이해하면 됨.

초기에는 프로그램을 실행할 때 시작부터 끝까지 프로세스 하나만을 사용해서 진행했다고 한다.
하지만 갈수록 프로그램은 복잡해졌기 때문에 하나의 프로세스만으로는 역부족.
쉽게 생각하면 `"프로세스를 여러 개 만들면 되지 않을까?"` 생각을 할 수 있지만 이는 불가능한 일이었다.
왜냐하면 운영체제는 안정성을 위해 프로세스마다 자신에게 할당된 메모리 내의 정보에만 접근할 수 있도록 제약을 두고 있고,
이를 벗어나는 정보에 접근하려면 오류를 발생시켰기 때문.

아무튼 프로세스와는 다른 더 작은 실행 단위 개념이 필요하게 되었고, 그게 바로 `스레드` 이다.

![프로세스](https://user-images.githubusercontent.com/18409941/185568171-1eb0062a-71e1-4e75-8eb9-03a406bf46e8.png)

스레드는 프로세스와는 다르게 스레드 간 메모리를 공유하며 작동한다. 스레드 끼리 프로세스의 자원을 공유하면서 프로세스 실행 흐름의 일부가 되는 것.
`스레드는 프로세스의 코드에 정의된 절차에 따라 실행되는 특정한 수행 경로`

### 면접에서는 이런걸 왜 물어볼까?
프로그램, 프로세스, 스레드에 대한 기본 개념을 이해하고있는지?
운영체제가 시스템 자원을 어떤 방식으로 할당하고 실제 프로그램은 이 자원을 어떤 방식으로 활용하여 작동되는지까지 알고 있어야 함.

더 깊이 들어가보자.

프로세스가 메모리에 올라갈 때 운영체제로부터 시스템 자원을 할당받는다고 언급했었다. 이 때 운영체제는 프로세스마다 각각 독립된 메모리 영역을 할당해준다.
![할당](https://user-images.githubusercontent.com/18409941/185569208-d5e14db4-7c33-4bd2-9c3b-57281ab34c3a.png)

스레드는 메모리를 서로 공유할 수 있다. 프로세스가 할당받은 메모리 영역 내에서 Stack 형식으로 할당된 메모리 영역은 따로 할당받고, 나머지 Code/Data/Heap 형식으로 할당된 메모리 영역을 공유한다.
따라서 각각의 스레드는 별도의 스택을 가지고 있지만 힙 메모리는 서로 읽고 쓸 수 있다.
![공유](https://user-images.githubusercontent.com/18409941/185569587-65fb52a0-188c-4544-908d-7a21df440675.png)

`여기서 프로세스와 스레드의 중요한 차이를 하나 더 알 수 있게 된다.`
만약 한 프로세스를 실행하다가 오류가 발생해서 프로세스가 강제로 종료된다면, 다른프로세스에게 어떤 영향이 있을까? 공유하고 있는 파일을 손상시키는 경우가 아니라면 아무런 영향을 주지 않는다.

그런데 스레드의 경우는 다르다. 스레드는 Code/Data/Heap 메모리 영역의 내용을 공유하기 때문에 어떤 스레드 하나에서 오류가 발생하면 같은 프로세스 내의 다른 스레드 모두가 강제 종료된다.

스레드를 코드(프로세스) 내에서의 함수(스레드)에 빗대어 표현해보면 이해가 쉽다.
오류가 어떤 함수에서 발생했든 해당코드는 다른 함수의 모두에 대한 작업을 중단하고 프로세스 실행을 끝내버린다.


[참고사이트](https://velog.io/@raejoonee/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4%EC%99%80-%EC%8A%A4%EB%A0%88%EB%93%9C%EC%9D%98-%EC%B0%A8%EC%9D%B4)