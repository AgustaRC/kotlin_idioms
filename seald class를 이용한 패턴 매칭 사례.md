`sealed class`가 다양한 곳에서 쓰일 수 있지만<br>
Android 개발을 할 때 `sealed class`를 유용하게 쓰고 있는 부분이 있는데<br>
바로 `ListAdapter`의 `getItemViewType`이다.<br>
`UiModel <-> R.layout.xxx`를 1 대 1 매핑할 때 써먹고 있다.

```kotlin
sealed class UiModel
data class Layout1 : UiModel()
data class Layout2 : UiModel()
data class Layout3 : UiModel()
```
위와 같이 `UiModel` 역할을 하는 `data class`들이 존재한다고 가정하고 `ListAdapter`에서는<br>
`UiModel`을 `item`으로 다룬다고 했을 때 개별 `ListAdapter`에서 상속할 추상 클래스에서 다음과 같은 추상 메소드를 정의
```kotlin
abstract fun getItemViewType(item: T): Int
```
`getItemViewType`을 아래와 같은 형태로 호출
```kotlin
override fun getItemViewType(position: Int): Int {
    return getItemViewType(getItem(position))
}
```
실제 사용되는 코드는 아래와 같은 형태
```kotlin
override fun getItemViewType(item: UiModel) = when (item) {
  is Layout1 -> R.layout.item_layout_1
  is Layout2 -> R.layout.item_layout_2
  is Layout3 -> R.layout.item_layout_3
}
```
