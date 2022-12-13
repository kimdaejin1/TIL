# List
- 데이터 구조의 모든 형태는 실제적으로 여러 종류의 리스트이다.  

## 선형 리스트
- 선형 리스트는 매우 간단하고 일반적인 데이터 구조이며 리스트의 원소들이 기억장치의 저장 공간에 연속적으로 있다는 것을 의미한다.  
    - 장점: 기억 장소의 활용도가 높음, 자주 변하지 않는 데이터의 저장에 유리하다.
    - 단점: 삽입, 삭제 시에 데이터 이동 횟수가 많으며 여유분의 연속적인 기억 장소를 필요함

- 선형 리스트는 특정한 원소를 지정하기 위한 포인터를 포함하지 않음  
리스트의 시작과 연관해서 계산하면 원소의 주소(포인터)를 알 수 있기 때문
## 연결 리스트
- 연결 리스트는 자료를 기억하는 장소 이외에 다음 자료를 가리키는 포인터를 두어 자료의 삽입과 삭제 시 자료의 이동을 최소화할 수 있는 자료 구조이다.
    - 선형 리스트의 단점중 삽입 삭제 연산에 소요되는 시간을 절약 할 수 있음

- 노드 = 연결 리스트 구조를 표현하기 위해 하나의 원소를 기억하는 구조   
각 노드는 자료 부분과 연결 부분으로 구성되 노드의 주소, 즉 포인터가 기억된다.

- 단순 연결 리스트 = 연결 부분이 하나인 연결 리스트
<img src = "https://velog.velcdn.com/images%2Fsangh00n%2Fpost%2Fbfa73611-96a7-450a-9974-4b40fb70c2fd%2FlinkedLIst.png">
- 이중 연결 리스트 = 연결 부분이 두개인 연결 리스트
<img src = "https://velog.velcdn.com/images%2Fsangh00n%2Fpost%2Fa50a2ca1-9208-4b4f-9fd9-ee06dfc51a8d%2Fimage.png">
    - 이 두개의 연결 부분에는 각각 오른쪽 노드를 갈키는 포인터와 왼쪽 노드를 가리키는 포인터가 기억된다.  

### 연결 리스트를 논리적으로 표현하는 방법
- 연결 부분을 어떻게 표현하는가에 따라 두가지로 나뉨
    - 1. 화살표로 표현하는 방법
    <img src="https://blog.kakaocdn.net/dn/JXtCb/btqz6WhJeXn/rGXi1DFP40ZGxQA968R5p0/img.png">
    - 2. 주소를 직접 적어 표현하는 방법
    <img src="https://blog.kakaocdn.net/dn/2PTnh/btqzP5ORLWv/zrKYCCjIICsavZGwomKB9k/img.png">  

    첫 번째 노드를 가리키기 위해 헤드 노드를 둔다  
    마지막 노드를 가리키기 위해 null 연결 ^를 둔다   
    ^ 은 아무것도 가리키지 않는다.(null pointer Or tail pointer)

- 연결 리스트의 장점 :  
    1. 노드의 삽입과 삭제가 용이하다.
    2. 연속적인 기억 공간이 없어도 저장이 가능하다.
- 연결 리스트의 단점 :  
    1. 연결 부분의 사용으로 선형 리스트나 배열보다 많은 기억 공간을 필요로 한다.  
    2. 알고리즘 구현이 복잡하다.
    3. 마지막 노드의 연결 부분에 NULL이 없거나 어떤 노드의 포인터를 잃어버리면 다른 노드를 가리킬 수 없어서 특정 노드 검색 시 무한 루프에 빠질 수 있다.

## 단순 연결 리스트의 구현  
1. 구조체 정의  
    ~~~C 
    typedef int element;
    typedef struct ListNode{
        element Data;
        struct ListNode *link;
    }ListNode;
    ~~~
2. 헤드 노드 생성
    ~~~C
    ListNode *p1;
    P1 = (ListNode *)malloc(sizeof(ListNode));
    ~~~~
3. 노드 생성   
    ~~~C
    p1 -> data = 10;
    p1 -> link = null;
    ~~~

## 단순 연결 리스트의 삽입 연산 구현
- 단순 연결 리스트에서 노드를 삽입하는 것은 이미 존재하는 자료의 이동은 전혀 없이 단지 포인터의 변경만으로 간단히 해결된다.
<img src="https://jess2.github.io/images/content/list-1.png">
노드를 삽입하는 함수를 insert_node 함수라 하면 insert_node 함수는 다음의 3가지 인수를 받는다. 
    ~~~C
    void insert_node (ListNode *phead, ListNode *p, ListNode *new_node)
    ~~~
    - phead : 헤드 포인터(head에 대한 포인터)
    - p : 삽입될 위치의 앞노드를 가리키는 포인터
    - new_node : 새로운 노드를 가리키는 포인터  

    실제 삽입 연산을 구현할 때는 다음 3가지 경우로 나누어 알고리즘을 생각하여야 한다.

1. Head가 null인 경우
    - head가 null 이라면 현재 삽입하려는 노드가 첫 번째 노드가 된다.  
    따라서 head 값만 바꿔주면 됨
2. p 가 null인 경우
    - 선행 노드 p가 null 값일 때는 리스트의 맨 앞에 삽입한다.  
    먼저 new_node의 link가 head와 같은 값을 갖도록 하고 다음에 head가 new_node를 가리키게 한다.
3. head와 p가 null 이 아닌경우
    - new_node의 link에 p -> link값을 복사한 다음, p -> link가 new_node를 가리키게 한다.  
    **순서에 유의 해야 한다!!**

### 4. C언어로 삽입 함수 구현
- 위의 3가지 알고리즘에 따라서 순서대로 구현하면 다음과 같다.   
    ~~~C
    ListNode *insert_node(ListNode *phead,ListNode *p,ListNode *new_node){
        if(p==NULL){
            new_node -> link = phead;
            return new_node;
        }
        else{
            new_node -> link = p -> link;
            p -> link = new_node;
            return phead;
        }
    }

## 단순 연결 리스트의 삭제 연산 구현
- 노드를 삭제하는 함수는 remove_node 함수라 하면 remone_node 함수는 다음의 3가지 인수를 받는다.
    ~~~C
    void remove_node(ListNode *phead, ListNode *p,ListNode *removed)
    ~~~
    - phead : 헤드 포인터 (head에 대한 포인터)
    - p : 삭제될 노드의 앞 노드를 가리키는 포인터
    - removed : 삭제될 노드를 가리키는 포인터

    실제 삭제 연산을 구현할 때는 다음의 2가지 경우로 나누어 알고리즘을 생각하여야 한다.
 1. p가 null인 경우
    - 연결 리스트의 첫 번재 노드를 삭제한다. 헤드 포인터를 변경하고 removed 노드가 차지하고 있는 공간을 시스템에 반환한다.
 2. p가 null이 아닌 경우 
    - 먼저 removed 앞의 노드인 p의 link가 removed 다음 노드를 가리키도록 변경한다. 다음으로 removed 노드가 차지하고 있는 공간을 시스템에 반환한다.

### 3. C언어로 삭제 함수 구현  
~~~C
ListNode *removed_node(ListNode *phead, ListNode *p, ListNode *removed){
    ListNode *t = removed -> link;
    free(removed)
    if(p == null){
        return t;
    }
    p -> link = removed -> link;
    return phead;
}