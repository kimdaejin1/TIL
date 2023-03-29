#[DI] Hilt 라이브러리
## Hilt
- 2020년 6월 Google에서 발표한 Android 전용 DI 라이브러리
    > Hilt는 프로젝트의 모든 Android 클래스에 컨테이너를 제공하고 수명 주기를 자동으로 관리함으로써 애플리케이션에서DI를 사용하는 표준방법을 제공합니다  
    >Hilt는 Dagger가 제공하는 컴파일 시간 정확성, 런타임 성능, 확장성 및 Android 스튜디오 지원의 이점을 누리기 위해 인기 있는 DI 라이브러리 Dagger를 기반으로 빌드되었다.
    
    Android 공식문서의 Hilt에 대한 설명은 위와 같다.

    + 기존의 boiler plate code를 대량 줄이고, readablilty를 향상 -> 유지보수 good
    + Jetpack의 ViewModel에 대한 DI 또한 큰 비용 없이 구현

***

## Hilt Application Class
### **@HiltAndroidApp**
- Hilt를 사용하는 모든 앱은 이 주석이 Application 클래스를 포함해야함
>Appication-level dependency container 역할을 하는 애플리케이션의 기본 클래스를 비롯하여 Hilt의 코드 생성을 트리거 합니다.   
>생성된 이 Hilt 구성요소는 Application 객체의 수명 주기에 연결되며 이와 관련한 dependencies을 제공합니다.   
>이는 parent component of the app이므로 다른 구성요소는 이 상위 구성요소에서 제공하는 dependencies에 엑세스할 수 있습니다

+ @HiltAndroidApp 클래스 -> Application Container(어플리케이션 컨테이너)
+ 앱의 생명주기와 밀접하게 연결됨
+ 앱의 상위 컨테이너 이므로 다른 컨테이너가 이 컨테이너에서 제공하는 클래스에 엑세스 할 수 있음   
**?** 컨테이너(Container): 인스턴스를 저장하는 공간  
->컨테이너는 참조가 필요한 클래스의 인스턴스들을 생성해주고 수명주기를 관리해 인스턴스를 제공하는 역할 

***

## Android 클래스에 dependency injection
### **@AndroidEntryPoint
- Android lifecycle을 따르는 dependency container를 생성하는 어노테이션
>Application 클래스에 Hilt를 설정하고 application-level component를 사용할 수 있게 되면 Hilt는 @AndroidEntryPoint 주석이 있는 다른 Android 클래스에 dependencies를 제공할 수 있습니다.

Hilt가 지원하는 Android 클래스: **Appliction(@HiltAndroidApp), Activity, Fragment, View, Service, BrodcastReceiver**  
- Android 클래스에 이 주석을 사용하면 이 클래스에 종속된 Android 클래스에도 주석을 지정해야 함
- @AndroidEnterPoint는 프로젝트의 각 Android 클래스에 관한 개별 Hilt component를 생성함
- 상위 클래스에서 dependency component를 받을 수 있다.

### **@Inject**
- 클래스 필드 주입을 위한 어노테이션  
**X** private 한 필드에는 주입되지 않음
    ```Kotlin
    @AndroidEntryPoint
    class ExampleActivity : AppCompatActivity() {
        @Inject lateinit var analytics: AnalyticsAdapter
    }
    ```
    -> @AndroidEntryPoint 어노테이션이 있는 클래스의 dependency container에서 빌드한 인스턴스를 사용하여 해당 클래스의 수명주기 메서드 내에서 필드를 채움  
    -> @Inject 어노테이션이 있는 analytics라는 필드에는 dependency가 채워질 것

***
## Hilt binding
*바인딩(binding): 필드 주입을 위해서 Hilt에게 해당 인스턴스를 제공하는 방법을 알려주는 것*
* binding은 특정 타입의 인스턴스를 dependency로 제공하는데 필수적인 정보를 포함함
* Hilt에 바인딩 정보를 제공하는 방법: 생성자 주입, Hilt 모듈

### 1. 생성자 주입(constructor injeciton)
-> 클래스의 생성자에 @Inject 어노테이션을 사용하여 dependency를 생성

#### **@Inject**
클래스의 생성자에 사용하여 Hilt에게 클래스의 인스턴스를 제공하는 방법을 알려주는 어노테이션
- @Inject 어노테이션으로 dependency 인스턴스를 생성하고, 매개변수로 dependency 주입
- @AndroidEntryPoint에서의 @Inject와는 다르게 주입될 객체입을 알려줌   

```Kotlin
class AnalyticsAdapter @Inject constructor(
    private val service: AnalyticsService
){. . .}
```

### 2. Hilt모듈 생성(@Module)
생성자 주입을 사용할 수 없는 상황(ex. 인터페이스, 외부 라이브러리 클래스)
-> Hilt Module을 사용하여 Hilt에 binding 정보를 제공함

#### @Module
Hilt 모듈임을 알려주는 어노테이션
- Hilt 모듈은 특정 유형의 인스턴스를 제공하는 방법(binding 정보를 Hilt에게 알려줌
- 각 모듈을 사용하거나 설치할때 Hilt에게 알려야 함

#### @Installin
각 모듈을 사용하거나 설치할 Android 클래스를 Hilt에 알리는 어노테이션
- @Installin에 사용되는 Hilt component들은 각자의 생명주기를 갖고 있으며, 설정한 컴포넌트의 생명주기를 따라감
- Hilt 모듈에 제공하는 dependency는 Hilt 모듈을 설치하는 Android 클래스와 연결되어 있는 component에서 사용가능
- 일반적으로 하위 component의 바인딩은 상위 component의 바인딩에 dependency를 가질 수 있음   

Android|생성된 구성 요소|생성 위치|제거 위치|범위
:---|:---:|:---:|:---:|---:|
 Application|ApplicationComponent|Application#onCreate()|Application#onDestroy()|@Singleton  
 View Model|ActivityRetainedComponent|Activity#onCreate()|Activity#onDestory()|@ActivitiyRetainedScope  
 Activity|ActivityComponent|Activity#onCreate()|Activity#onDestroy()|@ActivityScoped  
 Fragment|FragmentComponent|Fragmet#onAttach()|Fragment#onDestroy()|@FragmentScoped  
 View|ViewComponent|View#super()|제거된 뷰|@ViewScoped  
@WithFragmentBindings 주석이 지정된 View|ViewWithFragmentComponent|View#super()|제거된 뷰|@ViewScoped   
Service|ServiceComponent|Service#onCreate()|Service#onDestroy()|@ServiceScoped
   
<Android 클래스와 Hilt component>

<img src="https://miro.medium.com/v2/resize:fit:720/format:webp/1*T6NuyyXR77HRIzdINQlXtw.png">  

<Hilt component 계층 구조>

### 2-1 *인터페이스*에서의 Hilt 모듈 사용
- 인터페이스에 인스턴스를 삽입하는 경우 사용 (외부 라이브러리에서는 사용 X)
- Hilt는 인터페이스를 implement된 타입이 constructor injection이 된 경우, 어디서 인스턴스를 생성해야 하는지 모름
- 따라서 **abstract class 타입의 모듈**을 생성하여 함수를 생서하는 방법으로 해결
- Hilt 모듈 내에 @Binds 어노테이션이 지정된 추상 함수를 생성하여 Hilt에 binidng 정보를 제공함

### **@Binds**

@Binds 어노테이션이 붙은 추상 함수는 Hilt에게  
- 반환 타입으로 함수가 어떤 인터페이스의 인스턴스를 제공하는지 알려줌
- 매개변수로 제공할 구현부을 알려줌

```Kotlin
interface AnalytucsService {
    fun analyticsMethods()
}

// Constructor-injected, because hilt needs to know how to
// provide instances of AnalyticsServiceImpl, too
class AnalyticsServiceImpl @Inject constructor(
    . . .
) : AnalyticsService {. . .}

@Module
@InstallIn(ActivityComponent::Class)
abstract class AnalyticsModule {

    @Binds
    abstract fun bindAnalyticsService(analyticsServiceImpl: AnalyticsServiceImpl): AnalyticsService
}
````
-> AnalyticsService 타입의 객체는 AnalyticsServiceImpl에서 객체를 생성해야 한다고 알려줌

### 2-2 *외부 라이브러리*에서의 Hilt 모듈 사용
- Retrofit, OkHttpClient, Room 등 외부 라이브러리에서 클래스가 제공되어 클래스를 소유하지 않은 경우에 사용
- 클래스를 직접 소유하지 않은 경우, Hilt 모듈 내에 함스를 생성하고 함수에 @Provides 어노테이션 지정

#### **@Provides**
**@Provides** 어노테이션이 붙은 함수는 Hiltdprp
- 반환 타입으로 함수가 어떤 타입의 인스턴스를 제공하는지 알려줌
- 매개 변수로 해당 타입의 dependency를 알려줌
- 본문으로 해당 타입의 인스턴스를 제공하는 방법을 알려줌
```Kotlin
@Module
@InstallIn(ActivityComponent::class)
object AnalyticsModule {
    @Singleton
    @Provides
    fun provideAnalyticsService(): AnalyticsService {
        return Retrofit.Builder()
            .baseUrl("https://example.com")
            .build()
            .create(AmalyticsService::class.java)
    }
}
```

### 2-3 *같은 타입의 객체*에 대한 여러 바인딩
- 동일한 타입의 객체가 여러개인 경우, 어떤 객체를 Inject할지 Hilt는 알 수 없음
- 따라서 그 식별을 위해 한정자(Qualifier)를 이용하여 동알한 타입에 대해 여러 결합(binding)을 정의할 수 있음
- @Provides와 @Binds와 함께 사용될 수 있음

#### @Qualifier
특정 타입에 대해 여러 결합이 정의되어 있을 때, 그 타입의 특정 결합을 식별하는데 사용하는 어노테이션
- 한정자와 함께 @Retention(AnnotationRetention.Binary)어노테이션을 붙여서 identifier를 지정함
- annotation class로 키워드의 이름을 정함
```Kotlin
@Qualifier
@Retention(AnnotationRetention.BINARY)
annotation class AuthInterceptorOkHttpClient

@Qualifier
@Retention(AnnotationRetention.BINARY)
annotation class OtherInterceptorOkHttpClient
````
- 각 한정자와 일치하는 타입의 인스턴스를 제공하는 방법을 Hilt에게 알림
- 아래의 두 메서드는 동알한 반환 타입을 갖지만, 한정자는 2가지의 서로 다른 바인딩으로 메서드에 라벨을 지정함
- 필드, 매개변수에 한정자를 어노테이션으로 하여 필요한 특정 유형을 삽입할 수도 있음

```Kotlin
@Module
@InstallIn(ApplicationComponnent::class)
object NetworkModule {
    @AuthInterceptorOkHttpClient
    @Provides
    fun provideAuthInterceptorOkHttpClient(
        authInterceptor: AuthInterceptor
    ): OkHttpClient{
        return OkHttpClient.Builder()
            .addInterceptor(authInterceptor)
            .build()
    }
}

// As a dependency of a constructor-injected class.
class ExampleServiceImpl @Inject constructor(
    @AuthInterceptorOkHttpClient private val okHttpClient: OkHttpCilent
): ...

// At field injection.
@AndroidEntryPoint
class ExampleActivity: AppCompatActivity() {

    @AuthInterceptorOkHttpClient
    @Inject lateinit var okHttpClient: OkHttpClient
}
```

#### Hilt가 기본으로 제공하는 한정자
ex.Application 또는 Activity의 Context 클래스가 필요할 수 있으므로 Hilt는 @ApplicationContext,@ActivityContext 한정자를 제공함

```Kotlin
class AnalyticsServiceImpl @Inject constructor(
    @ApplicationContext context: Context
): AnalyticsService { . . . }

// The Application binding is available without qualifiers.
class AnalyticsServiceImpl @Inject constructor(
    application: Application
): AnalyticsService { . . . }
```