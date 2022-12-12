# Stack
- Stack = 삽입과 삭제가 한쪽 끝에서만 발생하는 선형 리스트  
    - top = 삽입과 삭제가 일어나는 부분을 가르키는 포인터
    - bottom = 막혀있는 다른 한 쪽 끝을 가르키는 포인터
    - Stack의 연산 :  
        1. PUSH = 삽입
        2. POP = 삭제
        3. PEEK = top 포인터의 자료의 내용 조사

- Stack 을 배열로 구현 할 경우
    - 자료삽입은 top 포인터(가장 최근에 삽입된 자료가 저장되어 있는 자리의 저장된 값)가 가리키는 위치보다 1이 증가한 위치에 자료가 삽입되고, 삭제는 top 포인터가 가르키는 위치의 자료가 삭제된다.
      
             즉 PUSH는 top = top + 1이 된 후 자료가 삽입 됨  
            POP은 자료가 삭제 된 후 top = top - 1 이 됨  
          
             C 언어에서 배열은 0부터 지정되므로 스택을 선언할 때 top의  
             초깃값은 -1 이 되어야 한다.  
    스택에서 가장 나중에 삽입된 자료가 가장 먼저 출력되므로 후입선출 LIFO(Last In First Out)구조 라고도 함

## 배열을 이용한 스택의 구현
- 순차 자료 구조인 1차원 배열을 이용하여 스택을 구현해서 사용해보자.  

    ~~~ C
    #define STACK_SIZE 5
    typedef int element;
    element stack[STACK_SIZE];
    int top = -1;
    ~~~
    기초적인 배열이나 top 포인터를 설정해 주었다.
      

## Stack의 삽입 연산 (PUSH)
- PUSH 연산은 스택에 새로운 자료를 추가하는 연산  
이 연산은 스택의 맨 위에서만 수행됨
<img src="https://blog.kakaocdn.net/dn/cc3ECe/btrAmAyCbeo/XlmFSLB5Mk8gKxTgP7F6WK/img.png">
    
    ~~~ C
    void push(element item){
        if(top >= STACK_SIZE-1){
            printf("Stack is Full!\n");
            return;
        }
        else {
            stack[++top] = item;
        }
    }

## Stack의 삭제 연산 (POP)
- POP 연산이 수행되면 스택에 가장 최근에 삽입된 자료 하나가 스택에서 제거되고 해당 자료가 반환 해주는 연산  
이 연산은 삽입 연산과 마찬가지로 스택의 맨 위에서만 수행됨
<img src="https://blog.kakaocdn.net/dn/c5HrJN/btrAmGrTiwU/FEStXAXz6lIuELTtUTbENK/img.png">

    ~~~ C
    element pop(){
        if(top == -1){
            printf("Stack is Empty!!\n");
            return 0;
        }
        else{
            return stack[top--];
        }
    }
    ~~~

## Stack의 조회 연산 (PEEK)
- PEEK 연산은 POP 연산과 마찬가지로 스택의 가장 최근 자료를 반환하는 연산  
다만 기존 스택에서 자료를 제거하지 않으며 top의 값도 역시 바뀌지 않는다.

    ~~~ C
    element peek(){
        if(top == -1){
            printf("Stack is Empty!!\n");
            return 0;
        }
        else{
            return stack[top];
        }
    }