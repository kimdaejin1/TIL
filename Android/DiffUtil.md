# DiffUtil
- 안드로이드 앱을 개발하게 되면 필수적으로 쓰이는 *RecyclerView*사용에 있어서, 동적으로 데이터가 변경되는 경우 우리는 *notifyDataSetChanged()* 를 통해 갱신하곤 했다.   
만약 아이템을 딱 하나가 바뀌는 상황이라도 **귀찮아서** *notfyDataSetChanged()* 를 통해 갱신하곤 한다.
그러나 이러한 행위는 **성능에 치명적인 악영향**을 미치게 된다.  
~~사실 개발자에게는 굉장히 편하다~~

    *notifiyDataSetChanged()* 는 사실상 **리스트를 싹 지우고 다시 처음부터 끝가지 객체를 하나하나 만들어 새로 랜더링하는 과정**을 거치게 된다. 때문에 **비용이 매우 크게 발생**한다. 따라서 효율적으로 동적인 RecyclerView를 구성하는 방법이 필요했다.

    이러한 이유로 등장한 것이 *DiffUtil* 클래스 이다.**이전 데이터 상태와 현재 데이터의 상태 차이를 계산**하고, 반드시 업데이트해야 할 **최소한의 데이터에 대해서만 갱신**하게 된다. 데이터 업데이트 횟수를 최소한으로 가져가는 것이다.

> TMI   
두 데이터 간의 차이 계산에는 Eugene W.Myners의 Diff Algorithm이 사용되었다고 한다.

## 1. DiffUtil 사용법
- *DiffUtil*이 **원래 목록과 새로 들어온 목록간의 차이를 계산**하고 난 뒤 *DiffUtil.Callback*은 **4개의 추상 메소드와 1개의 일반 메소드**로 이루어져 있는데, 이러한 메소드를 오버라이딩 해서 사용한다.

> getOldListSize()   
이전(원래) 목록의 크기를 반환한다.

> getNewListSize()   
새로 들어온 목록의 크기를 반환한다.

> areItemTheSame(int oldItemPosition, int newItemPosition)  
두 아이템이 같은 객체인지 여부를 반환한다.

> areContentsTheSame(int oldItemPosition, int newItemPosition)  
두 아이템이 같은 데이터를 가지고 있는지 여부를 반환한다. *areItemTheSame()* **이 *true*를 반환할 때만 호출**된다. 애초에 객체가 다르다면 데이터를 비교하는 것은 의미가 없기 때문이다.

> getChangePlayload(int oldItemPosition, int newItemPosition)   
만약 ***areItemTheSame()* 이 *true* 를 반환하고, *areContentTheSame()* 이 *false*를 반환**했다면 새로운 녀석이 들어 왔다는 것으로 간주하고 해당 메소드가 호출되어 **변경 내용에 대한 페이로드**를 가져온다.   
해당 메소드는 추상메서드가 아니기 때문에 꼭 오버라이드 할 필요는 없다.

- 우선 ***DiffUtil.Callback*을 상속받는 콜백 클래스**를 만들어주고, 비교 대상을 지정해준다.
~~~Kotlin
class DiffUtilCallback(private val oldList: List<Any>, private val newList: List<Any>): DiffUtil.Callback(){
    override fun getOldListSize(): Int = oldList.Size

    override fun getNewListSize(): Int = newList.Size

    override fun areItemsTheSame(oldLtemPosition: Int, newItemPosition: Int): Boolean {
        val oldItem = oldList[oldItemPosition]
        val newItem = newList[newItemPosition]
        return if(oldItem is Person && newItem is Person){
            oldItem.id == newItem.id
        }
        else{
            false
        }
    }

    override fun areContentsTheSame(oldItemPositon: Int, newItemPosition: Int):Boolean = oldList[oldItemPosition] == newList[newItemPosition]
}
~~~
- 그러고 난 뒤 리사이클러뷰의 어답터에 아래와 같은 함수를 만들어두고 ***upDateList()*를 통해 들어온 데이터**를 집어 넣게 되면, *DiffUtil.calculateDiff*에 해당 데이터를 집어넣게 되고 인자로는 우리가 **구현한 콜백 클래스 객체**를 전달한다.

    이후 *diffResult.dispatchUpdatesTo(어뎁터)*를 호출하게 되면, 최소한의 업데이트 연산으로 리사이클러뷰를 갱신해 줄 수 있는 것이다.
~~~Kotlin
fun updateList(items: List<Person>?){
    item.let {
        val diffCallback = DiffUtilCallback(this.items, items)
        val diffResult = DiffUtil.calculateDiff(diffCallback)

        this.items.run {
            clear()
            addAll(items)
            diffResult.disapatchUpdateTo(this@Adapter)
        }
    }
}
~~~

## 2. 알아두어야 할 것들
### 1. RecyclerView.Adapter와 연관성
#### *DiffUtil*은 **리사이클러뷰 어뎁터**의 **다양한 데이터 업데이트 관련 메소드를 활용**한다.
- notifyItemMoved()
- notifyItemRangeChanged()
- notifyItemRangeInserted()
- notifyItemRangeRemoved()

### 2. 엄청 큰 데이터를 다루는 경우
- 목록이 매우 방대한 경우라면, ***DiffUtil* 연산이 오래 걸릴 수 있기 때문에 백그라운드 쓰레드**에서 이를 실행하고, **메인 쓰레드에서 *DiffUtil.DiffResult* 를 가져온 뒤 적용**하는 것을 권장한다.   
(목록의 최대 사이즈를 2^26개 이다.)

### 3. 예상 성능
- *DiffUtil*은 두 목록 간의 추가 및 제거 작업의 최소의 수를 찾기 위해 O(N) 공간이 필요하다. 예상되는 성능은 O(N+D^2)정도이다.
    - N : 추가 및 제거 된 항목의 총 수
    - D : 스크립트 길이
