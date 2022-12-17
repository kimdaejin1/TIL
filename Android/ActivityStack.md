# Activity Stack
- Activity는 Stack형태로 관리된다.
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbF9xOn%2FbtqBZraP29L%2F3kCwC9b9iHqUr86J8G4dz1%2Fimg.jpg">
OneActivity에서 TwoActivity를 부르면 위에 쌓이고  
TwoActivity에서 ThreeActivity를 부르면 위에 쌓이는 Stack의 형식으로 위와 같이 만들어 진다.
    - 후입 선출(LIFO)구조로 가장 나중에 들어온 (top)의 위치한다.  
    가장 top에 있는 Activity에서 뒤로가기 버튼을 누르면 top에서 pop된다.

## Activity Stack 확인하기
- 안드로이드 스튜디오에서 Tools >Layout Inspector >실행시킨 avd or 모바일 기기 를 선택하면 내가 실행시킨 Activity의 스택을 확인할 수 있다.  
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbTN1go%2FbtqB1mT770W%2FeGn5sjGhvsQ5YFj24Uij21%2Fimg.png">

## Activity Stack 관리하기
- Stack을 관리하는 방법은 2가지가 있다.

### 1. AndroidManifest의 LaunchMode의 옵션으로 관리
~~~
    android:launchMode="standard"
    : 새 인스턴스를 생성하고 여러 번 인스턴스화 될 수 있고,
      각 인스턴스는 서로 다른 작업에 속 할 수 있으며, 
      여러개의 인스턴스가 있을 수 있다.
~~~
- ex) OneActivity 가 single 모드로 선언되어 있으면  
OneActivity -> TwoActivity-> ThreeActivity -> ThreeActivity
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FtvSvA%2FbtqBZgOhIA0%2F6jTDofKV6KyWNIKJjWpFG1%2Fimg.jpg">
위의 그림처럼 된다.      

~~~
    android:launchMode="singleTop"
    : 활동의 인스턴스가 이미 현재 작업 맨 위에 있으면 시스템은  
      활동의 새 인스턴스를 생성하지 않고 기존 인스턴스로 라우팅한다.
~~~
- ex) ThreeActivity=singleTop  
OneActivity -> TwoActivity->ThreeActivity-> ~~ThreeActivity~~
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCNp8z%2FbtqBZr221wv%2FXcAghVL0bkZpyKXgFikKt0%2Fimg.jpg">

~~~
    android:launchMode="singletask"
    : 활동의 인스턴스가 이미 작업에 있다면 시스템은 새 인스턴스를 
      생성하지 않고 기존 인스턴스로 라우팅한다.
      활동의 인스턴스가 한 번에 하나만 존재할 수 있다.
      루트 Activity로만 존재한다. 
      위에 다른 Activity를 쌓을 수 있다.
~~~
- ex) ThreeActivity=singletask 
OneActivity -> TwoActivity->ThreeActivity  
Stack은 ThreeActivity 하나
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbjLCD0%2FbtqBZMFVnXe%2FYgfAKoCCATKQd7bwrOTDk0%2Fimg.jpg">

~~~
    android:launchMode="singleInstance"
    : singletask와 동일하지만 활동은 항상 자체 작업의 
      단 하나의 유일한 멤버이다.
      위의 다른 액티비티를 올릴 수 있다.
~~~
- ex) ThreeActivity=singleInstance 
OneActivity -> TwoActivity->ThreeActivity -> OneOneActivity 
Stack은 ThreeActivity 하나 그리고 마지막에 OneOneActivity
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fr6Dhe%2FbtqB0gz3SIc%2FzlIwWx17O4BDUh7bKqqw5K%2Fimg.jpg">

### 2. Intent Flag로 관리
- startActivity()에 전달하는 Intent Flag에 아래와 같은 Flag를 주어 Stack을 관리한다.

    - FLAG ACTIVITY NEW TASK  
    : singleTask와 유사하다. 활동을 새 작업에서 시작한다.  
      이미 실행중인 작업이 있다면 그 작업을 마지막으로 포그라운드로 이동한다.

    - FLAG ACTIVITY SINGLE TOP  
    : 새롭게 부른 Activity가 가장 top에 있는 Activity라면 새 인스턴스가 생성되는 대신 기존 인스턴스가 호출을 수신한다.  
    singleTop과 유사하다.

    - FLAG ACTIVITY CLEAR TOP 
    : 새롭게 부른 Activity가 현제 Stack에서 실행 중이면 활동의 새 인스턴스가 실행되는 대신 해당 Activity와 Stack을 제거하고 새로운 Activity를 가장 top으로 만든다.