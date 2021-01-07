Kotlin에는 `run`, `with`, `apply`, `also`, `let`과 같은 유용한 Scope함수들이 있다. <br>
각각 상황에 따라 쓰임새가 다른데 **수신 객체**를 지정하는 `run`, `with`, `apply` 함수들은 사용함에 있어서 주의가 필요하다. <br>
<p>
무분별한 Scope 함수 남용은 오히려 코드 가독성을 해치는 결과와 예상치 못한 문제에 봉착할 수 있는데 아래와 같은 코드가 있다고 가정을 해보자. <br>

```xml
<layout xmlns:android="http://schemas.android.com/apk/res/android">

  <data>
    
    <variable
      name="handler"
      type="XXX"/>
      
    <variable
      name="viewModel"
      type="XXX"/>
  </data>

</layout>
```

```kotlin
with(binding) {
  setVariable(BR.handler, handler)
  setVariable(BR.viewModel, viewModel)
}
```

위 코드의 의도는 `viewBinding` 인스턴스인 `binding`에 연속적인 접근을 하기 때문에 `with` 함수로 감싸주고 <br>
DataBinding 연결을 위해 `setVariable`을 통해 `xml`에 정의되어 있는 `handler`, `viewModel` 변수에 <br>
`handler`, `viewModel` 인스턴스를 할당하려는 코드이다. <br>
<p>

위 코드에서는 `binding`에 존재하는 `handler`와 `handler` 인스턴스 변수의 이름이 같기 때문에 <br>
`with` 블록 내부에서는 `binding`의 `handler`를 우선해서 접근한다. <br>
<p>

그러므로 코드의 의도와는 다르게 잘못된 할당 동작이 이루어지고 이 문제는 컴파일 시점뿐 아니라 <br>
런타임에서도 제대로 인지를 못 할 가능성이 있다. <br>
<p>

위와 같은 경우 외에도 수신 객체를 지정하는 스코프 함수를 쓰게 될 때 종종 실수하는 경우가 있는데 <br>
this로 접근이 되는 수신 객체 지정 함수의 경우 항상 함수의 스코프에 대해서 신경을 쓰면서 사용해야 할 것 같다.
