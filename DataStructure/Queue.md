# Queue
- 큐 = 먼저 들어온 데이터가 먼저 나가는 구조  (FIFO, First In First Out)
    - rear(tail) = 가장 최근 삽입된 값을 가르키는 포인터
    - front(head) = 삽입된지 가장 오래된 자료를 가르키는 포인터
## 선형 Queue와 원형 Queue
### 선형 큐
1. 정수를 저장할 수 있는 큐를 만들려면 먼저 정수의 1차원 배열을 정의하고 삽입, 삭제 를 위한 변수 fornt, rear를 만든다.
    - fornt = 큐의 첫번째 요소   
    - rear = 큐의 마지막 요소
    - fornt 와 rear의 초깃값은 -1이다.
- 선형 큐는 이해하기 쉽지만 문제점이 있다. front와 rear의 값이 계속 증가 하기 대문에 언젠가는 배열의 끝에 도달하게 되고 배열의 앞부분이 비어 있더라도 사용하지 못한다는 점이다.  
선형 큐의 문제점은 원형 큐로 생각하면 쉽게 해결 된다
### 원형 큐
- front와 rear의 값이 배열의 끝에 도달하면 다음에 증가되는 값은 0이 되도록 하는 것이다.

#### 원형 큐의 삽입 삭제 구현 
- front는 첫 번째 요소로부터 시계 방향으로 하나 앞을, rear는 마지막 요소를 가르킨다.  
- 삽입 할 때 rear를 먼저 하나 증가시키고 증가된 위치에 삽입한다.   
- 삭제 할 때 front를 증가시킨 다음, 그 위치에서 데이터를 꺼낸다.  

- front와 rear의 값이 같으면 원형큐가 비어있음을 나타낸다. 원형 큐에서는 하나의 자리는 항상 비워둔다. 왜냐하면 포화 상태와 공백 상태를 구별하기 위해서.  
만약 한 자리를 비우지 않는다면 공백 상태와 포화 상태를 구분 할 수 없게 된다.  

- 데이터 삽입 전에 front ==(rear+1)%MAX_SIZE 조건을 체크해 원형 큐가 포화 상태일 대 추가 삽입이 일어나지 않게 막아야 하고 데이터 삭제 전에 front == rear 조건을 체크해 원형큐가 공백인 상태일 때 ㄷ이터 삭제가 진행 되지 않게 해야 한다.

    ~~~ C
    #include <stdio.h>
    #define MAX_SIZE 100

    typedef int element;
    typedef struct {
        element queue[MAX_SIZE];
        int front, rear;
    }QueueType;

    void error(char *message){
        fprintf(stderr,"%s\n", message);
        exit(1)
    }

    void init(QueueType *q){
        p -> front = q -> rear = 0;
    }

    void enqueue(QueueType *q, element item){
        if((q -> rear + 1) % MAX_SIZE == q -> front){
            error("큐가 포화 상태입니다");
        }
        q -> rear = (q -> rear + 1) % MAX_SIZE;
        q -> queue[q -> rear] = item;
    }

    element dequeue(QueueType *q){
        if(q -> rear + 1 == q -> rear){
            error(" 큐가 공백 상태 입니다");
        }
        q -> front = (q -> front + 1) % MAX_SIZE;
        return q -> queue[q -> front];
     }
    ~~~