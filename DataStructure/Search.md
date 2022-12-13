# 탐색
- 여러 원소로 구성된 데이터에서 원하는 원소를 찾는 것
## 1. 순차 탐색
- 순차 탐색은 리스트 형태로 주어진 원소들을 처음부터 하나씩 차례로 살펴보며 원하는 킷값을 갖는 원소를 순차적으로 찾는 탐색 방식.
<img src="https://miro.medium.com/max/1400/1*Tt7ykGnFSFgFjNd45UW1hg.png">

### 순차 탐색의 알고리즘
~~~C    
SequentiaSearch(A[ ],n,k){
    for(int i=0; i < n; i++){
        if(A[i] == k){
            return i;
        }
        return n;
    }
}
~~~

## 2. 이진 탐색
- 이진 탐색은 정렬된 데이터 형태로 주어진 원소들을 절반씩 나누며 가운데 원소를 보면서 원하는 킷값을 갖는 원소를 찾는 탐색 방식
- 이진 탐색은 정렬된 데이터 형태여야만 적용이 가능한 탐색 알고리즘이다.
<img src="https://t1.daumcdn.net/cfile/tistory/262CCF4657D6D0352E">

### 이진 탐색의 알고리즘
~~~C
BinarySearch(A[ ], n, k){
    LEFT = 0;
    RIGHT = n-1;
    while(LEFT <= RIGHT){
        MID = [(RIGHT - LEFT + 1)/2]+LEFT
        if(A[MID] == k){
            return MID;
        }
        else if(A[MID] < k){
            LEFT = MID + 1;
        }
        else{
            RIGHT  = MID - 1;
        }
    }

    return n;
}
~~~


