# 트리
## 트리의 개요
- 트리는 나무의 줄기에서 가지를 뻗어 나가는 것과 비슷하게 노드(자료)간 관계가 설정 되어 나가는 구조   
시스템 내부의 저장 장소를 할당하는데 있어서 중요한 역할을 담당하며 정열과 검색을 하는데 사용된다.

### 트리의 정의 
- 트리는 노드(node)라고 불리는 정점과 노드와 노드를 연결하는 간선(edge)으로 구성되고 트리를 형성하는 노드들 가운데에서 어떠한 두 정점 사이에도 사이클이 존재하지 않아야 한다.
### 트리의 용어
<img src="https://t1.daumcdn.net/cfile/tistory/23674B345724A80908">

## 이진 트리의 표현
### 이진 트리의 개념
- 이진 트리는 모든 노드가 두 개 이하의 자식 노드를 같=ㅈ는 순서화 된 트리이다.

### 이진 트리의 종류
- 이진 트리는 특별한 형태로 사향 이진 트리, 완전 이진 트리, 포화 이진 트리 등이 있다.

1. 사향 이진 트리
    - 루트를 제외한 모든 노드가 그 부모의 왼족 자식이기만 하거나 오른쪽 자식이기만 한 이진 트리 (a.k.a 한 쪽으로만 치우친 이진 트리)
<img src="https://velog.velcdn.com/images%2Fsungjun-jin%2Fpost%2Ff4a47917-65bd-40eb-bc2e-1b1cb77ac8b3%2Fimage.png">

2. 완전 이진 트리
    - 임의의 두 단말 노드의 레벨 차이가 1이하이며 왼쪽에서 오른쪽으로 채워진 이진 트리  
      
      전채 노드 수 = 2^n-1 (n은 최대 레밸)
<img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTzb3ZKBrZZzM6mnsM4tLzV83tSKfhtzAeKHQ&usqp=CAU">

3. 포화 이진 트리
    - 각 레밸의 모든 노드가 가득 차 있는 이진 트리
     
      전채 노드 수 = 2^n-1 (n은 최대 레밸)
<img src="https://t1.daumcdn.net/cfile/tistory/2153514657BBF9B428">

### 이진 트리의 표현
#### 1. 배열을 이용한 표현
- 포화 이진 트리는 루트를 정수 1에서 부터 시작하여 레벨이 증가하는 방향으로 같은 레벨에서는 왼쪽 노드에서 오른쪽 노드로 차례대로 정수를 각 노드의 번호로 부여할 수 있다.
<img src="https://t1.daumcdn.net/cfile/tistory/271D5133583844E634">  
트리의 깊이가 K이라고 할 때 배열 공간은 2^K-1 만큼이 필요하다.  
모든 이진 트리의 노드들은 그 위치에 따라 포화 이진 트리의 노드 들로 일대일 매핑이 가능하기 때문에 모든 이진 트리는 배열에 저장할 수 있다.
    - 모든 이진 트리는 1차원 배열을 이용하여 표현할 수 있다. 루트 노드의 위치를 1로 시작여 각 노드의 자료는 그 노드의 일년 번호에 해당하는 배열의 위치에 저장한다.

### 2. 노드 연결을 이용한 표현
- 트리를 구성하는 노드들 간의 다양한 관계는 부모-자식 관계에 기반한다.  
이진 트리의 경우 각 노드가 최대 두개의 자식 노드(왼쪽 자식 노드와 오른쪽 자식 노드)를 가질 수 있으므로, 노드별로 자료 저장 공간 이외에 두 자식 노드 각각에 대한 포인터 저장 공간을 설정, 활용할 수 있게 함으로 써 이진 트리를 표현할 수 있다.  

- 왼쪽 자식노드 포인터 | 자료 | 오른쪽 자식 노드 포인터
    ~~~C
    typedef struct TreeNode{
        int data;
        struct TreeNode *left_child;
        struct TreeNode *right_child;
    }TreeNode;
    TreeNode node;
    ~~~
- 이진 탐색 트리를 노드 연결 방식으로 표현하면 노드에서 왼족 자식 노드나 오른쪽 자식 노드가 없다면 해당 노드의 링크는 NULL값을 가지게 된다.
<img src="https://images.velog.io/images/yunsungyang-omc/post/e578778d-d1b8-46db-9d64-1b73ddb49099/Screen%20Shot%202021-03-06%20at%2011.38.17%20PM.jpg">

## 이진 트리의 순회
- 이진 트리의 순회란 트리 내의 각 노드를 한 번씩만 방문하여 저장된 정보를 확인하는 것을 말한다. 선형 자료 구조(스택, 큐, 리스트 등)의 경우 그 구조가 단순한 만큼 순회 방법 역시 단순하지만 계층 구조의 비선형 자료 구조인 이진 트리의 경우 순회 방법이 다양하다.

1. 전위 순회
    - 루트 노드를 먼저 방문한  다음 왼쪽 서브트리를 방문하고 마지막으로 오른쪽 서브트리를 방문한다.
<img src="https://mblogthumb-phinf.pstatic.net/20160413_226/shootingstar_romance_1460477282872LdVuj_PNG/2.%C0%FC%C0%A7%BC%F8%C8%B8.PNG?type=w2">

2. 중위 순회 
    - 왼쪽 서브 트리를 먼저 방문한 다음 루트 노드를 방문하고 마지막으로 오른쪽 서브 트리를 방문한다.
<img src="https://mblogthumb-phinf.pstatic.net/20160413_76/shootingstar_romance_1460477283167R3Dfg_PNG/2.%C1%DF%C0%A7%BC%F8%C8%B8.PNG?type=w2">

3. 후위 순회
    - 왼쪽 서브 트리를 먼저 방문한 다음 오른쪽 서브ㅡ리를 방문하고 마지막으로 루트 노드를 방문한다.
<img src="https://mblogthumb-phinf.pstatic.net/20160413_172/shootingstar_romance_1460477283316A2GTK_PNG/3.%C8%C4%C0%A7%BC%F8%C8%B8.PNG?type=w2">

### 이진 트리 순회 알고리즘
- 이진 트리의 순회는 순환 알고리즘과 반복 알고리즘을 사용하여 구현할 수 있다.  
1. 재귀 알고리즘을 사용한 이진 트리의 순회
    - 자기 자신을 다시 호출 하는 재귀 알고리즘은 이진 트리 순회 알고리즘을 단순하게 구현할 수 있다.
    ~~~C
    typedef struct TreeNode{
        int data;
        struct TreeNode *left, *right;
    }TreeNode;

    TreeNode n1 = {1, NULL, NULL};
    TreeNode n2 = {4, &n1, NULL};
    TreeNode n3 = {16, NULL, NULL};
    TreeNode n4 = {25, NULL, NULL};
    TreeNode n5 = {20, &n3, &n4};
    TreeNode n6 = {15, &n2, &n5};
    TreeNode *root = &n6;

    void inorder(TreeNode *root){
        if(root){
            inorder( root -> left);
            printf("%d",root -> left);
            inorder( root -> right);
        }
    }

    void preorder(TreeNode *root){
        if(root){
            printf(root -> data);
            preorder(root -> left);
            preorder(root -> right);
        }
    }

    void postorder(TreeNode *root){
        if(root){
            postorder(root -> left);
            postorder(root -> right);
            printf("%d", root -> data);
        }
    }

    int main(){
        inorder(root);
        preorder(root);
        postorder(root);

        return 0;
    }
    ~~~

## 이진 탐색 트리
- 이진 탐색 트리는 루트에 저장된 값보다 작은 값들만으로 구성 된 이진 탐색 트리를 왼쪽 서브 트리로 가지고 있고, 루트에 저장된 값보다 큰 값들만으로 구성된 이진 탐색 트리를 오른쪽 서브트리로 가지고 있는 이진 트리

1. 이진 탐색 트리의 정의
    - 이진 탐색 트리는 작업을 효율적으로 처리하기 위해 다음과 같이 정의된 자료 구조이다.  
        1. 모든 노드는 서로 다른 유일한 값을 갖는다.
        2. 왼쪽 서브 트리에 있는 노드들의 값은 그 루트의 값보다 작다.
        3. 오른쪽 서브 트리에 있는 노드들의 값은 그 루트의 값보다 크다.
        4. 왼쪽 서브 트리와 오른쪽 서브 ㅡ리도 이진 탐색 트리이다.
2. 이진 탐색 트리의 탐색
    - 이진 탐색 트리는 "왼쪽 서브 트리 값 < 루트 값 < 오른쪽 서브 트리 값"의 관계가 형성되기 때문에 중위 순회를 하게 되면 오름차순으로 정렬 된 값을 얻을 수 있다.
    ~~~
    이진 탐색 트리에서 값 X가 저장된 노드를 탐색하는 알고리즘
    1. X == 루트 노드 값 : 노드를 찾았으므로 탐색 종료
    2. X < 루트 노드 값 : 루트 노드의 왼쪽 서브트리에 대하여 값 X가 저장된 노드 탐색
    3. X > 루트 노드 값 : 루트 노드의 오른족 서브트리에 대하여 값 X가 저장된 노드 탐색
    ~~~

    - 코드 
    ~~~C
    Node *searchNode(Node *Tree, int findData){
        if(Tree == NULL){
            return NULL;
        }
        else if(Tree -> Data == FindData){
            return Tree;
        }
        else if ( Tree -> Data < findData){
            return searchNode(Tree -> Left, findData);
        }
        else if ( Tree -> Data > findData){
            return searchNode(Tree -> Right, findData);
        }
    }
    ~~~

3. 이진 탐색 트리에서의 삽입
    - 이진 탐색 트리에 새로운 원소를 삽입하기 위해서는 먼저 동일한 원소가 트리내에 있는지 확인하는 탐색이 필요하다. 만약 동일한 원소가 없다면 탐색이 실패하여 종료된 그 지점에 새로운 노드를 삽입한다.

4. 이진 탐색 트리에서의 삭제
    - 이진 탐색 트리에서의 삭제는 먼저 삭제할 노드를 찾은 다음 세 가지 경우로 나누어 삭제를 진행한다.
    1. 삭제할 노드가 단말 노드인 경우
        - 탐색을 통해 삭제할 노드의 위치를 찾은 노드를 삭제하고 부모 노드의 링크를 NULL 로 설정한다.
    2. 삭제할 노드가 하나의 자식 노드를 가진 경우
        - 해당 노드를 삭제하고 삭제한 노드의 자식 노드를 삭제한 노드의 위치로 이동한다.
    3. 삭제할 노드가 두 개의 자식 노드를 가진 경우
        - 해당 노드를 삭제한 후 삭제한 노드의 좌측 서브 트리에서 가장 값이 큰 노드나 또는 우측 서브 트리에서 가장 값이 작은 노드를 삭제한 노드의 위치로 이동한다.

