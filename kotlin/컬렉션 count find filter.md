1. count() 데이터 개수 반환  
```kotlin   
fun main(args: Array<String>) {
  val list = arrayListOf<Int>(5, 6, 7, 8, 9)
  val arr = arrayListOf<Int>(10, 8, 7, 3, 2)
  
  println(list.count())
}
```
2. find() 조건에 만족하는 **가장 첫 번째 데이터** 반환  
```kotlin   
fun main(args: Array<String>) {
  val list = arrayListOf<Int>(5, 6, 7, 8, 9)
  val arr = arrayListOf<Int>(10, 8, 7, 3, 2)
  
  //8
  println(list.find { it > 7 })
}
```



























