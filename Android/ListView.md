# ListView
- ListView는 스크롤 가능한 항목을 나타낼 때 사용하는 View 그룹
- ListView에 먼저 View를 배치한 다음, 데이터가 저장된 곳에서 데이터를 View형식에 맞게 변환하여 가져온다.


### 음식에 대한 리스트 뷰를 짜보자 

## 1. 데이터 클래스의 정의
- 음식의 목록을 만들기 위해 *Food.kt* 클래스 파일을 만들고, 음식의 구성요소로 name, category, photo를 설정한다.   
여기서 photo는 drawable에 들어갈 이미지 파일을 이름이며, 이후 변수를 통해 drawable 이미지를 불러올 수 있게 된다.

~~~Kotlin
/* Food */
class Food(val name : String, val category : String, val photo : String)
~~~
- 그리고, 실제 변수를 초기화한 후 음식의 목록을 가지고 있을 ArrayList를 MainActivity에 추가한다. 우선 빈 ArrayList를 추가한다.

~~~Kotlin
val foodList = arrayListof<Food>()
~~~

## 2. 레이아웃에 ListView 추가
~~~Xml
 <ListView
        android:id="@+id/mainListView"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent">
    </ListView>
~~~

## 3. Item 생성
~~~Xml
<ImageView
        android:id="@+id/foodPhotoImg"
        android:layout_width="52dp"
        android:layout_height="52dp"
        android:layout_marginBottom="4dp"
        android:layout_marginStart="8dp"
        android:layout_marginTop="4dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:srcCompat="@mipmap/ic_launcher_round" />

    <TextView
        android:id="@+id/foodName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginStart="16dp"
        android:textSize="20sp"
        android:textStyle="bold"
        app:layout_constraintStart_toEndOf="@+id/dogPhotoImg"
        app:layout_constraintTop_toTopOf="@+id/dogPhotoImg"
        tools:text="name"/>

    <TextView
        android:id="@+id/foodCategory"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textSize="16sp"
        app:layout_constraintBottom_toBottomOf="@+id/dogPhotoImg"
        app:layout_constraintStart_toStartOf="@+id/dogBreedTv"
        tools:text="category" />
~~~

## 4. 어댑터 생성
- ListView, ListView의 각 item 그리고 거기에 넣을 데이터까지 설정했다면 *Adapter*를 만들어야 한다.   
사진, 종류, 이름 등 어느 요소를 어느 view에 넣을 것인지 연결해주는 것이 Adapter의 역할이다.   
~~~Kotlin
class MainListAdapter(val context: Context, val foodList: ArrayList<Food>) : BaseAdapter(){

}
~~~
- 임의로 Custom한 Adpater Class는 *BaseAdapter()*를 상속받는다. 그렇기에 기본적으로 4개의 함수를 implement해야 한다. 

    Adapter에서 각 메소드가 담당하는 기능은 아래와 같다.
    - *getView(Int, View ViewGrup) : Xml 파일의 View와 데이터가 연결하는 핵심역할을 하는 메소드
    - getItem(Int) : 해당 위치의 item의 메소드, Int 형식으로 된 position을 파라미터로 갖는다.
    - getItemId(Int) : 해당 위치의 item id를 반환하는 메소드이다.
    - getCount() : ListView에 속한 item의 전체 수를 반환한다.

    이외에도 데이터 추가, 삭제나 변경 등의 기능을 하는 메소드도 추가할 수 있지만, 기본적으로 override 하는 메소드는 위의 4개이다.

~~~
class MainListAdapter (val context: Context, val dogList: ArrayList<Dog>) : BaseAdapter() {
    override fun getView(position: Int, convertView: View?, parent: ViewGroup?): View {
        val view: View = LayoutInflater.from(context).inflate(R.layout.main_lv_item, null)

        val foodPhoto = view.findViewById<ImageView>(R.id.foodPhotoImg)
        val foodCategory = view.findViewById<TextView>(R.id.foodCategory)
        val foodName = view.findViewById<TextView>(R.id.foodName)

        val food = foodList[position]
        val resourceId = context.resources.getIdentifier(food.photo, "drawable", context.packageName)
        dogPhoto.setImageResource(resourceId)
        foodCategory.text = food.category
        foodName.text = food.name

        return view
    }

      override fun getItem(position: Int): Any {
        return foodList[position]
    }

    override fun getItemId(position: Int): Long {
        return 0
    }

    override fun getCount(): Int {
        return foodList.size
    }

}
~~~

## 5. Adapter 설정
- 정성들여 Adapter을 만들면, MainActivity로 와서, Adapter를 초기화 해야 한다. 그 후 ListView와 Adapter을 연결한다.
~~~Kotlin
class MainActivity : AppCompatActivity() {

    var foodList = arrayListOf<Food>()

    override fun onCreate(saveInstanceState: Bundle?){
        super.onCreate(saveInsteanceState)
        setContentView(R.layout.activity_main)

        val foodAdapter = MainListAdapter(this, foodList)
        mainListView.adapter = foodAdapter
    }
}
~~~

- 이로써 ListView가 완성됐지만, 아직 리스트에 값을 넣지 않았기 때문에 
임의의 데이터를 넣어서 하드 코딩을 해보자.
~~~Kotlin
var foodList = arrayListof(Food){
    Food("돈까스", "양식", "foodimg1")
    Food("자장면", "중식", "foodimg2")
    Food("새우초밥", "일식", "foodimg3")
    Food("갈비찜", "한식", "foodimg4")
}
~~~

## 6. ListView의 단점
- ListView는 Adapter을 통해 *getView* 메소드를 호출하여 View를 만든다. 최초로 화면을 로딩한 후에도 스크롤을 움직이는 등 액션을 취하면 그때마다 *findViewById*를 통해 convertView에 들어갈 요소를 찾는다.   
스크롤 할때마다 View를 찾으면 리소스를 많이 사용하게 되고 속도가 느려진다.   

    스크롤링 할때 조금 더 자연스러운 뷰를 보여주고 싶다면 *View Holder*를 사용하거나, 그 View Holder를 사용하도록 설계된 *RecyclerView*가 권장된다.