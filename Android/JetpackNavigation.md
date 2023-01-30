# Jetpack Navigation
- Jetpack은 Android 개발을 빠르게 도와주는 컴포넌트 라이브러리
- Jetpack은 Ui를 통한 Navigation 편집이 가능하게 해주는 라이브러리로, 구글에서 권장하고 있는 네비게이션중 하나이다.
- 단순한 버튼 클릭부터 좀 더 복잡한(앱바, 탐색창)에 이르기까지 여러 가지 탐색을 구현하도록 도와준다.  
- 탐색 구성요소는 기존의 원칙을 준수하여 일관적이고 예측 가능한 사용자 환경을 보장한다.

## 1. Jetpack Navigation의 3가지 주요 구성요소
- 1. **탐색 그래프**    
: 모든 정보가 모여있는 Xml 리소스, 이용가능한 경로가 포함되어 있다.

- 2. **NavHost**    
: 탐색 그래프에서 대상을 표현시하는 빈 컨테이너,   
    프레그먼트 대상을 표시하는 기본 NavHost 구현인 NavHostFregment 가 포함된다.

- 3. **NavController**    
: NavHost 에서 탐색을 관리하는 객체.   
사용자가 앱 내에서 이동할 때 NavHost에서 대상 콘텐츠를 전환하는 작업을 한다.

## 2. 사용법 
### 1. res/navigation/nav_graph.xml 파일 생성
~~~Xml
<navigation xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/nav_graph"
    app:startDestination="@id/fragmentOne">

    <fragment
        android:id="@+id/fragmentOne"
        android:name="com.eun.myappkotlin02.nav.OneFragment"
        android:label="fragment_one"
        tools:layout="@layout/fragment_one" >
        <action
            android:id="@+id/action_fragmentOne_to_fragmentTwo"
            app:destination="@id/fragmentTwo" />
    </fragment>

    <fragment
        android:id="@+id/fragmentTwo"
        android:name="com.eun.myappkotlin02.nav.TwoFragment"
        android:label="fragment_two"
        tools:layout="@layout/fragment_two" >
        <action
            android:id="@+id/action_fragmentTwo_to_fragmentThree"
            app:destination="@id/fragmentThree" />
    </fragment>

    <fragment
        android:id="@+id/fragmentThree"
        android:name="com.eun.myappkotlin02.nav.ThreeFragment"
        android:label="fragment_three"
        tools:layout="@layout/fragment_three" >
        <action
            android:id="@+id/action_fragmentThree_to_fragmentOne"
            app:destination="@id/fragmentOne" />
    </fragment>

</navigation>
~~~

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fd8OXdu%2FbtrDaowErP4%2FkI8x30XBPkSSOzYVYYWt6K%2Fimg.png">

- fragment 안에는 action, argument, deeplink 태그를 가질 수 있다.  
action 은 화살표로 화면간의 이동을 나타내고, argument 는 데이터 전달 시 사용한다

### 2. NavHost 설정하기
- 이제 해당 activity에서 fregment를 보여줄 뷰를 추가해야 한다.   
원하는 activity레이아웃에 fregmentview를 추가해 주면 된다.
~~~Xml
<fragment 
	android:id="@+id/nav_host" 
	android:name="androidx.navigation.fragment.NavHostFragment" 
	android:layout_width="match_parent" 
	android:layout_height="match_parent" 
	app:defaultNavHost="true" 
	app:navGraph="@navigation/nav_graph" />
~~~
* _navGraph와 defaultNavHost설정은 필수적으로 해야 한다._

### 3. NavController 설정하기
- 해당 fregment 뷰가 있는 activity에서 설정해주면 된다.   
MainActivity에 fregmentView가 있다고 가정해보자.  
그럼 다음과 같이 navigationController을 설정해주면 된다.
~~~Kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
		
        
        val navHostFragment = supportFragmentManager.findFragmentById(R.id.nav_host_fragment) as NavHostFragment
        val navController = navHostFragment.navController

    }
}
~~~
* Activity에서 navigationController을 만들고, 이를 활용해 navigate를 수행해주면 된다

- 이제는 각 버튼에서 이동하는 법을 배워보자
~~~Kotlin
override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
    super.onViewCreated(view, savedInstanceState)

    val navController = Navigation.findNavController(view)
    btn.setOnClickListener {
        navController.navigate(R.id.action_fragment1_to_fragment2)
    }

}
~~~
- NavController을 선언하고 onViewCreated에서 받아온 view를 통해 navController을 정의한다.   
마지막으로 버튼 클릭시 navController의 navigate를 해당 action에 설정해준다.  
그러면 버튼 클릭시 해당 action을 통해 fregment2로 화면이 이동된다.