# Binding
## ViewBinding
- ViewBinding을 쓰기전에는 findViewById라는 것을 이용해서 xml 뷰와 연결시켜주는 작업을 했어야 했는데 한 두개 면 상관없지만 한 페이지에 들어가는 뷰가 워낙 많아서  코드가 항상 더러워 진다

    **View Binding**은 **findViewById를 대체**할 수 있는 기능 이다.

1. findViewById와의 차이점
    - 뷰 바인딩을 사용하면 직접 id를 적고 타입을 정하고 이런 작업을 하지 않아도 된다
        자동으로 클래스 파일을 생성해주기 때문이다.
        그에 비해 findViewById의 문제점은 다음과 같다.

        - null 안정성 : 개발자가 실수로 유효하지 않는 아이디를 적으면 null오류가 발생할 수 있다.
        - Type 안정성 : textView의 타입을 imageView라고 잘못 적어서 캐스팅하면 cast exaption이 발생할 수 있다.
        - 속도가 상대적으로 느리다.

2. 사용법 
    - 1. gradle 추가
        ~~~ 
        android {
            ...
            buildFeatures {
                viewBinding = true
            }
        }
        ~~~

    - 2 - 1. Activity에서의 사용
        ~~~Kotlin
        private lateinit var binding: ActivityMainBinding
 
        class MainActivity : AppCompatActivity() {
            override fun onCreate(savedInstanceState: Bundle?) {
                super.onCreate(savedInstanceState)
                binding = ActivityMainBinding.inflate(layoutInflater)
                setContentView(binding.root)
        
                binding.textView.text = "안녕"
            }
        }
        ~~~

    - 2 - 2. Fragment에 적용
        ~~~Kotlin
        class BlankFragment : Fragment() {
            private var _binding: FragmentBlankBinding? = null
            private val binding get() = _binding!!
            override fun onCreateView(
                inflater: LayoutInflater,
                container: ViewGroup?,
                savedInstanceState: Bundle?
            ): View? {
                _binding = FragmentBlankBinding.inflate(inflater, container, false)
                val view = binding.root
                return view
            }
            override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
                super.onViewCreated(view, savedInstanceState)
                binding.textView.text = "안녕"
            }
            override fun onDestroyView() {
                super.onDestroyView()
                _binding = null
            }
        }
        ~~~
        다른 건 다 똑같지만 다른점은 **onDestroyView**에서 **binding에 null**을 집어넣아야 한다는 점이다.

        이유는 무엇이냐면 프레그먼트는 뷰보다 더 온래 삻아 남기 때문에   
        바인딩 클래스는 뷰에 대한 참조를 가지고 있는데  
        뷰가 제거될 때(= onDestroyView) 이 바인딩 클래스의 인스턴스도 같이 정리해주는 것이다.
* * *
## DataBinding
- Ui 요소와 데이터를 프로그램적 방식으로 연결하지 않고, 선언적 형식으로 결합할 수 있게 도와주는 라이브러리
#### DataBinding을 사용하는 이유
- **데이터 바인딩을 사용하면, 데이터를 Ui 요소에 연결하기 위해 필요한 코드를 최소화할 수 있다**

- 장점
    - findViewById()를 호출 하지 않아도, 자동으로 xml에 있는 View들을 만들어 준다.
    - RecyclerView에 각각의 item을 set 해주는 작업도 자동으로 진행된다.
    - data가 바뀌면 자동으로 View를 변경하게 할 수 있다.
    - xml 리소스만 보고도 View에 어떤 데이터가 들어가는지 파악 가능하다.
    - 코드 가독성이 좋아지고, 상대적으로 코드량이 줄어든다.  

     **또한, 데이터 바인딩은 MVP 또는 MVVM 패턴을 구현하기 위해 유용하게 사용된다**
- 단점
    - 클래스 파일이 많아진다.
    - 빌드 속도가 느려진다.

    그래서 data binding은 단독으로 사용하는 것보다 MVVM 또는 MVP 아키텍처과 함께 사용해야 빛을 발한다.

### 1. dataBinding 사용법
#### 1. gradle 파일에 dataBinding 요소를 추가한다.
 ~~~
    android {
        ...
        dataBinding{
            enabled = true
        }
    }
~~~

#### 2. \<layout> 태그에서 xml 레이아웃을 작성한다.
- data binding을 사용하는 xml 리소스는 \<layout> 루트 태그로 시작해야 한다.
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FPMqAR%2FbtqDSjtMLAl%2FUTLexkucS3Pgu9gi1nS5Z1%2Fimg.png">

    binding class 는 data binding을 사용하는 클래스의 이름에 따라 각 레이아웃 파일에 대해 뒤에 "Binding"이라는 suffix가 붙은 Camel Case로 자동으로 생성된다.

    위의 activity_main.xml에 대해 자동 생성된 데이터 클래스의 이름은 MainActivityBinding 이다.

#### 3. Activity에서 기존의 setContentView() 함수를 DataBindingUtil.setContentView()로 교체한다.
- DataBindingUtil class의 객체를 생성하고, 기존의 setContentView()를 DataBindingUtil.setContentView()로 대체한다.

    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsF7iD%2FbtqDRMiGDh3%2FBZDDSsuitCJeEhkoIjYuQ1%2Fimg.png">

#### 4. View에 보여줄 data class를 선언한다.
- data 객체를 view와 bind 하기 위해서 User라는 이름의 data class를 만들어줬다.

    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbW0YOx%2FbtqDPLsadN1%2FskxZ9GjiBT9q8O0gIByak1%2Fimg.png">

#### 5. xml 리소스의 \<layout> 태그 안에서 \<data> 요소를 작성해준다
- \<data> 태그는 \<layout>에서 사용할 변수를 정의하는데 사용된다.  
레이아웃 내의 표현식은 **"@{}"** 구문을 사용하여 속성(attribute properties)에서 작성된다.
    <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F2wQbT%2FbtqDQ9SYlWp%2FUdRxlbhOLvExTjEXrfX3R1%2Fimg.png">

#### 6. Activity에서 User 객체를 초기화 해준다.
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdwCvqu%2FbtqDQSRrJ6o%2FnpUIkSwZJt6dvGpfOcVI61%2Fimg.png">