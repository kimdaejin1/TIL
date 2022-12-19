# RecyclerView
- 간단히 말해서 ListView와 비슷한데 비슷한 형식의 뷰만 몇 개만 그려놓고 스크롤을 내리면 위의 View를 없애고 미리 그려진 View에 데이터를 할당하는 방식
    - 데이터 집합을 각각의 개별 아이템 단위로 구성하여 화면에 출력해주는 ViewGrup.   
    동일한 형태를 갖는 수많은 리스트를 구현할 때 사용

    - RecyclerView는 ListView보다 재사용성이 좋다.
    - ListView는 Viewholder가 아니라 getView()를 이용하여 접근하는데 리스트가 많아지면 비효율적이다.
    - RecyclerView는 ViewHolder을 통해 만든 객체를 재사용하기 때문에 훨씬 효율적이다.

    > 1. RecyclerView를 레이아웃에 추가하고 각 아이템별 레이아웃 설정  
    > 2. 리스트의 각 아이템에 들어갈 데이터 결정 (data class)
    > 3. ViewHolder (어떤 데이터를 어디에 넣을건지)
    > 4. Adapter (RecyclerView에 데이터 연결)

## 1. 사용법
### 1. build.gradle(app)에 implement 추가
~~~
implementation 'androidx.recyclerview:recyclerview:1.1.0'
~~~

### 2. activity_main.xml 또는 recyclerview를 넣고 싶은 곳에 recyclerview 추가
~~~Xml
 <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:layout_width="match_parent"
        android:layout_height="0dp"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent"/>
~~~
### 3. RecycleView 안 각 아이템 배치할 레이아웃 만들기
- item_list.xml
~~~Xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="80dp"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:orientation="vertical">

    <ImageView
        android:id="@+id/foodPhotoImg"
        android:layout_width="60dp"
        android:layout_height="60dp"
        android:layout_margin="10dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:srcCompat="@mipmap/ic_launcher_round"
        app:layout_constraintTop_toTopOf="parent"/>

    <TextView
        android:id="@+id/foodCategory"
        android:layout_marginStart="20dp"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textStyle="bold"
        android:textSize="20sp"
        app:layout_constraintStart_toEndOf="@id/dogPhotoImg"
        app:layout_constraintTop_toTopOf="@id/dogPhotoImg"
        android:text="Breed"
        android:layout_marginLeft="20dp" />

    <TextView
        android:id="@+id/foodName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="16sp"
        app:layout_constraintBottom_toBottomOf="@id/dogPhotoImg"
        app:layout_constraintStart_toStartOf="@id/dogBreedTv"
        android:text="age"/>


</androidx.constraintlayout.widget.ConstraintLayout>
~~~

### 4. list_item에 넣을 data class 만들기
- FoodData.kt
~~~Kotlin
data class FoodData(
    val food_img : String,
    val food_name : String,
    val food_category : String
)
~~~

#### ViewHolder 만들기
> View Holder : 데이터가 들어갈수 있게 하는 기능 정의

- RecyclerViewAdapter.kt 클래스 안의 내부 클래스로 만들면 된다.  
내가 만들 ViewHolder에는 val 예약어로 바인딩을 전달 받아서 전역으로 사용한다.  
그리고 상속받는 ViewHolder 생성자에는 꼭 binding.root를 전달해야 한다.
~~~Kotlin
inner class MyViewHolder(private val binding: ItemListBinding): RecyclerView.ViewHolder(binding.root){
    fun bind(foodData : FoodData){
        binding.foodName.text=foodData.food_name
        binding.foodCategory.text=foodData.food_category
    }
}
~~~

### 5. RecyclerViewAdapter 만들기
> 어답터의 역할 : 간단히 말해서, 데이터 리스트를 실제 눈으로 볼 수 있게 item으로 변환하는 중간 다리 역할   
= 데이터를 받아오고 이를 레이아웃에 직접 연결하는 함수를 실행시키는 클래스  
= 미리 생성해둔 뷰홀더 객체에 사용자가 원하는 data list를 주입하고 data list의 변경 사항을 UI에 반영

- RecyclerViewAdapter에는 필수로 구현해야 하는 메서드 들이 있음   
-> OnCreateViewHolder(), getItemCount(), onBindViewHolder()

~~~Kotlin
class RecyclerViewAdapter: RecyclerView.Adapter<RecyclerViewAdapter.MyViewHolder>(){

    var datalsit = mutableListOf<FoodData>()

    inner class MyViewHolder(private val binding: ItemListBinding): RecyclerView.ViewHolder(binding.root){
            fun bind(foodData : FoodData){
            binding.foodName.text=foodData.food_name
            binding.foodCategory.text=foodData.food_category
        }
    }

    override fun onCreatedViewHolder(parent: ViewGrup, viewType: Int): MyViewHolder{
        val binding=ItemListBinding.inflate(LayoutInflater.from(parent.context), parent, false) 
        return MyViewHolder(binding)
    }

    override fun getItemCount() : Int = datalist.size

    override fun onBindViewHolder(holder: MyViewHolder, position: Int){
        holder.bind(datalist[position])
    }
}
~~~

### 6. MainActivity에 연결
~~~Kotlin
class MainActivity : AppCompatActivity(){

    private lateinit var binding:ActivityMainBinding
    private lateintit var adapter:RecyclerViewAdapter

    val mDatas=mutableListOf<FoodData>()

    override fun onCreate(savedInstanceState: Bundle?){
        super.onCreate(savedInstanceState)
        binding=ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        initializelist()
        initFoodRecyclerView()

    }

    fun initFoodRecyclerView(){
        val adapter=RecyclerViewAdapter()
        adapter.datalist=mDatas
        binding.recyclerView.adapter=adapter
        binding.recyclerView.layoutManager=LinerLayoutManager(this)
    }

    fun initializelist(){
        with(mDatas){
            add(FoodData("", "자장면","중식"))
            add(FoodData("", "스테이크","양식"))
            add(FoodData("", "초밥","일식"))
            add(FoodData("", "갈비찜","한식"))
            add(FoodData("", "탕수육","중식"))
            add(FoodData("", "토마토스파게티","양식"))
            add(FoodData("", "돈코츠 라멘","일식"))
            add(FoodData("", "돼지고기 김치찜","한식"))
        }
    }
}
~~~