# ViewBinding
어제 이어서 viewBinding을 구현해보고 있다.<br/>
viewBinding을 이제 쓸줄 알게 되었는데 binding을 하면서 onClick()을 override했을 때 여러 버튼을 한번에 처리하는 방법을 찾고 싶었다.
<p>예전엔 findViewById()를 가지고 id를 받아서 id 별로 처리하면 됐기 때문에 같은 로직으로 짰는데 작동을 안한다...

```kotlin
class _Fragment : Fragment(), View.OnClickListener {
    ...
    binding.option1.setOnClickListener { this }
    binding.option2.setOnClickListener { this }
    binding.option3.setOnClickListener { this }
    binding.option4.setOnClickListener { this }
    binding.btnBack.setOnClickListener { 
        navController.popBackStack()
     }
}

override fun onClick(v: View?) {
    when(v){
        binding.option1 -> { navigateWithIndex(1) }
        binding.option2 -> { navigateWithIndex(2) }
        binding.option3 -> { navigateWithIndex(3) }
        binding.option4 -> { navigateWithIndex(4) }
        binding.btnBack -> {
            navController.popBackStack()
            }
        }
}
```

## > 해결방법

# Bundle
Fragment의 결과 값을 다음 Fragment에 넘기고 싶을 때 값을 Bundle 객체를 만들어서 전달할 수 있다.
```kotlin
// 전달하는 클래스
val bundle = bundleOf("index" to index)
view.findNavController().navigate(R.id.FragmentSecond, bundle)
```
```kotlin
// 수신 대상 클래스
binding.tv.text = arguments?.getString("index")
```