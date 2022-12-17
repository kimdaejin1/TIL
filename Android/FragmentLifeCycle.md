# FragmentLifeCycle
- 안드로이드 공식 사이트에 있는 생명주기
<img src= "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcDyVCU%2Fbtq9CtTEtoA%2FkpOuUqYRAw8aVmbyKT7jpk%2Fimg.png">
Activity의 생명주기에 따른 콜백 함수와 비교해봤을 때 생성시는 onViewCreated() - onViewStateRestored()기 추가 되어 잇고 종료 시에는 onSaveInstanceState() - onDestroyView() 가 추가 되었다.

- Activity도 마찬가지지만 기본적으로 lifecycle은 위에서 아래 방향으로 진행 된다.

- Fragment가 백스택에 최상단으로 올라왔을 경우에는 생명주기가   
Created - Started - Resumed 순으로 진행되고   
반대로 POP 되었을 때 Resumed - Started - Created - Destroyed 순으로 진행됨

- 그림을 잘 보면 FragmentLifecycle과 ViewLifecycle이 이상한데 Fragment의 Lifecycle이 변화 되는 순간 Fragment Callback 함수를 호출하게 되고, 해당 콜백 함수가 종료되는 시점에 View의 LifeCycle 에 이벤트를 전달하게 된다.

## FragmentLifeCycle의 함수

### 1. onCreate()
- 먼저, Fragment만 Create된 상태  
- Fragment Manager에 add 됐을때 도달하며 onCreate() 콜백 함수를 호출한다.  
    * 주의 할 점은 onCreate() 이전에  onAttach()가 먼저 호출된다는 것  
    생각보다 onAttach()가 먼저 호출 된다는 것을 쉽게 잊어버리기 때문에 잘 기억해 두자.

    - **이 시점에는 아직 FragmentView가 생성되지 않았기 때문에 Fragment의 View와 관련되 작업을 두기에 적절하지 않다.**   

    onCreate() 콜백 시점에는 Bundle 타입으로 savedInstanceState 파라미터가 함께 제공되는데   
    이는 onSaveInstaceState() 콜백 함수에 의에 저장된 Bundle 값이다.

    - savedInstanceState 파라미터는 **Fregment가 처음 생성되었을 때만 null**로 넘어오며,   
    onSaveInstanceState() 함수를 재정의 하지 않아도 그 이후 재생성부터는 non - null 값으로 넘어 온다.   

### 2. onCreateView(), onViewCreated()
- onCreate() 다음에는 onCreateView()와 onViewCreated()가 이어서 호출 된다.  
onCreate()의 반환 값으로 정상적인 Fragment View 객체를 제공했을 때만 Fragment View의 LifeCycle이 생성됨

- onCreateView()를 재정의하여 Fragment View를 직접 생성하고 inflate 할 수 있지만  
LayoutId를 받는 Fragment의 생성자를 사용하여 해당 리소스 Id 값을 통해 onCreateView() 재정의 없이도 Fragment View를 생성할 수 도 있다.

- onCreateView()를 통해 반환된 View 객체는 onViewCreated()의 파라미터로 전달되는 데, 이 시점부터는 FragmentView의 LifeCycle이 INITIALIZED 상태로 업데이트 되었기 때문에   
**View의 초깃값 설정 또는 LiveData 옵저빙, RecyclerView 또는 ViewPager2에 사용될 Adapter 세팅 등은 onViewCreated() 에서 해주는 것이 적절하다.**

### 3. onViewStateRestored()
- onViewStateRestored() 콜백 함수는 저장해둔 모든 state값이 Fragment의 View 계층 구조에 복원되었을 때 호출된다.  
    - 여기서 부터는 체크박스 위젯이 현제 체크 되어있는지 등 각 뷰의 상태값을 확인할 수 있다.
- View lifecycle owner 는 이때 INITIALIZED 상태에서 CREATED 상태로 변경됐음을 알린다.
### 4. onStart()
- Fragment가 사용자에게 보여질 수 있을때 호출 된다.  
이는 주로 Fragment가 attach 되어 있는 Activity의 onStart() 시점과 유사하다.  
이 시점부터는 Fragment의 child FragmentManager을 통해 FragmentTranscation을 안전하게 수행 가능하다.
- Fragment의 LifeCycle이 STARTED로 이동한 후에 Fragment View의 LifeCycle 또한 STARTED로 변환 된다.

### 5. onResume()
- Fragment가 보이는 상태에서 모든 Animator와 Transition 효과가 종료되고, Fragment가 사용자와 상호작용 할 수 있을 때 onResume() 함수가 콜백 된다.   
onStart()와 마찬가지로 Activity의 onResume()의 시점과 유사하다.

- Resumed 상태가 됐다는 것은 사용자가 Fragment와 상호작용 하기에 적절한 상태가 됐다고 하지만 **onResume()이 호출되지 않은 시점에서는 입력을 시도하거나 포커스를 설정하는 등의 작업을 임의로 하면 안된다는 것을 의미**한다.

### 6. onPause()
- 사용자가 Fragment를 떠나기 시작했지만 Fragment는 여전히 visible(보이는 상태)일 경우 onPause() 가 호출 된다.

- **Fragment와 View의 LifeCycle이 PAUSED 가 아닌 STARTED 상태가 된다는 점을 주목 하자** 엄밀히는 LifeCycle에는 PAUSED와 STOP에 해당하는 상태가 없다.

### 7. onStop()
- Fragment가 더이상 화면에 보이지 않으면 Fragment와 View의 LifeCycle은 CREATED 상태가 되고, onStop()함수가 호출 되게 된다.   
(이 상태는 부모 Activity나 Fragment가 중단 됐을 때 뿐만 아니라 부모 Activity나 Fragment의 상태가 저장될 때도 호출 된다.)

- Fragment의 onStop()의 주의 할 점
    - **API 28 버전을 기점으로 onSaveInstanceState()와 onStop() 함수의 호출 순서가 달라 졌다.**
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbC4Zkm%2Fbtq9DwbxrgQ%2FIl287fhextuJbiCRZtZde1%2Fimg.png">
    사진을 보면 onSaveInstanceState() 함수보다 먼저 호출 함으로써 onStop()이 FragmentTransaction을 안전하게 실행할 수 있는 마지막 지점이 되었다.

### 8. onDestroyView()
- 모든 exit animation과 transition이 완료되고, Fragment 가 화면으로부터 벗어났을 경우 FragmentView의 LifeCycle은 DESTROYED가 되고 onDestroy()가 호출된다.   
(이 시점부터는 getViewLifecycleOwnerLiveData()의 리턴 값이 Null이 반환 된다.)
- **해당 시점에서는 가비지 컬랙터에 의해 수거될 수 있도록 Fragment View에 대한 모든 참조가 제거 되어 있어야 한다**

### 9. onDestroy()
- Fragment가 제거되거나 FragmentManager가 destroy됐을 경우, Fragment의 LifeCycle은 DESTROYED 상태가 되고, onDestroy 콜백 함수가 호출된다.
(이 시점부터 FragmentLifecycle이 끝남을 알린다. 그리고 onAttach()가 onCreate() 이전에 호출 됐던 것처럼 onDeath() 또한 onDestroy() 다음에 호출 된다.)
