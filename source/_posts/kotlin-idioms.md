---
title: Kotlin习惯用语
---

### 创建DTOs(POJOs/POCOs)
```
data class Customer(val name: String, val email: String)
```
提供一个**Customer**类拥有以下功能:
- 所有属性的getters(var类型才有setters)
- equals()
- hashCode()
- toString()
- copy()
- component1(), component2(),...(component的数量和构造参数的数量一致)

### 带默认值的方法参数
```
fun foo(a: Int = 0, b: String = ""){ ... }
```
### 过滤list
```
// 格式为lambda表达式 该方法的作用是过滤掉所有小于等于0的值
val positives = list.filter { x -> x > 0 }
```
可以简写为:
```
// 使用隐参, 不清楚的可以参考gradle里面的it
val positives = list.filter { it > 0 }
```
### String占位符
原文标题为String Interpolation,这么翻译不知道有没有问题
```
println("Name $name")
```
### 实例检查
```
when (x) {
    is Foo -> ...
    is Bar -> ...
    else   -> ...
}
```
### 遍历map/list
```
for((k, v) in map) {
    println("$k -> $v")
}
```
k,v可以是任何类型
### 使用范围
```
for (i in 1..100) { ... }        // [1,100]
for (i in 1 until 100) { ... }   // [1,100)
for (x in 2..10 step 2) { ... }
for (x in 10 downTo 1) { ... }
if (x in 1..10) { ... }
```
### 只读list
```
val list = listOf("a", "b", "c")
```
### 只读map
```
val map = mapOf("a" to 1, "b" to 2, "c" to 3)
```
### 访问map
```
println(map["key"])
map["key"] = value
```
### lazy属性
```
var p: String by lazy {
}
```
### 扩展方法
```
// 这里照方法名的意思是将空格转成驼峰式命名
// 但是方法的返回值为Unit 所以第二行代码的结果应该是个Unit
// 按照方法名的实现参考代码吧
fun String.spaceToCamelcase() { ... }
"Convert this to camelcase".spaceToCamelcase()
```
### 创建一个单例
```
object Resource {
    val name = "Name"
}
```
### 非空速记
```
var files = File("Test").listFiles()
println(files?.size)
```
### 非空和else速记
```
var files = File("Test").listFiles()
println(files?.size ?: "empty")
```
### 执行一个可能为null的语句
```
val data = ...
val email = data["email"] ?: throw IllegalStateException("Email is missing!")
```
### 不为null时才执行
```
val data = ...
data?.let { ... }
```
### return时使用when语句
```
fun transform(color: String): Int {
    return when(color) {
        "Red"   -> 0
        "Green" -> 1
        "Blue"  -> 2
        else    -> throw IllegalArgumentException("invalid color param value")
    }
}
```
### try/catch表达式
```
fun test() {
    val result = try {
        count()
    }catch(e: ArithmeticExpection) {
        throw IllegalArgumentExpection(e)
    }
}
```
### if表达式
```
fun foo(param: Int) {
    val result = if (param == 1) {
        "One"
    } else if (param == 2) {
        "Two"
    } else {
        "Three"
    }
}
```
### Builder-style usage of methods that return Unit
```
fun arrayOfMinusOnes(size: Int): IntArray {
    // 初始化一个指定长度的数组并用-1填充它
    return IntArray(size).apply { fill(-1) }
}
```
### 单行表达式方法
```
fun theAnswer() = 42
```
等价
```
fun thAnswer(): Int {
    return 42
}
```
这种用法可以和其他的用法进行组合来使代码更短:
```
fun transform(color: String): Int = when (color) {
    "Red"   -> 0
    "Green" -> 1
    "Blue"  -> 2
    else    -> throw IllegalArgumentExpection("Invalid color param value")
}
```
### 在一个实例对象上调用多个方法('with')
```
class Turtle {
    fun penDown()
    fun penUp()
    fun turn(degrees: Double)
    fun forward(pixels: Double)
}

var myTurtle = Turtle()
with(myTurtle) {
    penDown()
    for (i in 1..4) {
        forward(100.0)
        turn(90.0)
    }
    penUp()
}
```
### 尝试使用Java 7
```
var stream = Files.newInputStream(Paths.get("/some/file.txt"))
stream.bufferred().reader().use { println(it.readText()) }
```
### 需要一般参数的一般方法的简便写法
```
//  public final class Gson {
//     ...
//     public <T> T fromJson(JsonElement json, Class<T> classOfT) throws JsonSyntaxException {
//     ...

inline fun <reified T: Any> Gson.fromJson(json): T = this.fromJson(json, T::class.java)
```
### 消费可能为null的Boolean
```
var b: Boolean ?= ...
if(b == true) {
    ...
} else {
    // b 为null或false
}
```
### 参考资料
- [本文翻译的文档](https://kotlinlang.org/docs/reference/idioms.html)
- [本文对应的代码](https://github.com/lazytes/HelloKotlin/blob/master/src/Idioms.kt)
- [Kotlin官方文档](http://kotlinlang.org/docs/reference/)
- [Kotlin在线编辑器](https://try.kotlinlang.org/#/Examples/Hello,%20world!/Simplest%20version/Simplest%20version.kt)



