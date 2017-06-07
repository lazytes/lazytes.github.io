---
title: Hello, Kotlin!
---

### 1. Packages的定义
在源文件的顶端声明: 
```
package my.demo

import java.util.*
```
**注意：Kotlin与Java不同，声明`package my.demo`的文件在Java中必须放在`my.demo`包下，在Kotlin中可以放在任意包下**
### 2. Fuctions的定义
拥有2个Int参数且返回值为Int的方法:
```
fun sum(a: Int,b: Int): Int {
    return a+b
}
```
可简写为:
```
fun sum(a: Int,b: Int) = a + b
```
此时的返回值类型自动推测为Int
拥有2个Int参数且无返回值**(对应Java中的void)**的方法:
```
fun sum(a: Int,b: Int): Unit {
    println("sum of $a and $b is ${a + b}");
}
```
**Unit** 可以省略:
```
fun sum(a: Int,b: Int) {
    println("sum of $a and $b is ${a + b}");
}
```
### 3. 本地变量的定义
只读变量:
```
val a: Int = 1
val b = 2       // 此时b的类型根据赋值2自动推测为Int
val c: Int      // 没有默认值 必须声明变量类型 
c = 3           // 结合上面一行, 这种写法必须在方法里面，外面会报错哦
```
可变变量:
```
var x = 5
x += 1
```
### 4. 注释
支持Java相同的注释,**Kotlin的注释块(/\* \*/)是可以嵌套的**
### 5. 使用字符串模板
```
var a = 1
val s1 = "a is $a" // 最简单的用法

a = 2
val s2 = "${s1.replace("is","was")}, but now is $a" // 使用表达式
```
### 6. 使用条件表达式
```
fun maxOf(a: Int,b: Int): Int {
    if(a > b){
        return a
    } else {
        return b
    }
}
```
使用**if**表达式后:
```
fun maxOf(a: Int,b: Int) = if(a > b) a else b
```
### 7. 使用可能为null的值并检查是否为null
定义一个方法, 如果不能转换成Integer就返回null:
```
fun parseInt(str: String): Int? {
    // 官方文档中这里没有写实现方法
    // 使用自己的判断, 我在接口中看到有现成的就直接用了
    // 具体使用?可以参考该方法的源码
    return str.toIntOrNull() 
}
```
使用这个可能返回null的方法:
```
fun printProduction(arg1: String, arg2: String) {
    val x = parseInt(arg1) // 等价val x = arg1.toIntOrNull()
    val y = parseInt(arg2) // 等价val y = arg2.toIntOrNull()
    
    if(x != null && y != null) println(x * y)
    else println("either ‘$arg1’ or '$arg2' is a number")
}
```
### 8. 使用类型检查和自动转换
**is**等价Java的**instanceof**:
```
fun getStringLength(obj: Any): Int? {
    if(obj is String) {
        return obj.length
    }
    return null
}
```
换种写法:
```
fun getStringLength(obj: Any): Int? {
    if(obj !is String) return null
    return obj.length
}
```
### 9. 使用for循环
```
val items = listOf("apple", "banana", "kiwi")
for(item in items) println(item)
```
也可以这么写:
```
val items = listOf("apple", "banana", "kiwi")
for(index in items.indices) println("item at $index is ${items[index]}")
```
### 10. 使用while循环
```
val items = listOf("apple", "banana", "kiwi")
var index = 0
while(index < items.size) {
    println("item at $index is ${items[index}")
    index++
}
```
### 11. 使用when表达式
```
fun describe(obj: Any): String =
when(obj) { // 吐槽: 在这里idea的缩进真难看
    1           -> "One"
    "Hello"     ->"Greeting"
    is Long     ->"Long"
    !is String  ->"Not a String"
    else        ->"Unknown"
}
```
### 12. 使用ranges(不知道怎么翻比较好)
使用**in**操作符判断数字是否在一个范围之内:
```
val x = 10
val y = 9
if(x in 1..y + 1) println("fit in ranges") // x在1-10之内就打印
```
检查数字是否超出范围:
```
var list = listOf("a", "b", "c")
if(-1 !in 0..list.lastIndex) println("-1 is out of range")
if(list.size !in list.indices) {
    println("list size is out of valid list indices range too")
}
```
迭代一个范围:
```
for(x in 1..5) println(x)
```
or over a progression(不知道怎么翻比较好):
```
/**
 * 等价 for(int i = 1; i < 11; i += 2) {
 *          print(i);
 *      }
 */
for(x in 1..10 step 2) print(x) // 从1开始每次增加2直到10
/**
 * 等价 for(int i = 9; i > -1; i -= 3) {
 *          print(x);
 *      }
 */
for(x in 9 downTo 0 step 3) print(x) // 从9开始每次减少3直到0
```
### 13. 使用集合
迭代一个集合:
```
val items = listOf("apple", "banana", "kiwi")
for(item in items) println(item)
```
判断集合内的对象是否在操作符中使用:
```
when {
    "orange" in items -> "juicy"
    "apple"  in items -> "apple is fine too"
}
```
使用拉姆达表达式并转换集合内的对象:
```
fruits
    // 这里的it应该是和gradle里面一样的隐参
    // 过滤集合, 条件为所有不是以a开头的全部过滤掉
    .filter { it.startsWith("a") } 
    // 将集合内的对象全部排序
    // 这里是String, 应该是先按第一个字母排序，再按第二个字母排序
    .sortedBy { it }
    // 全部转换为大写(这里的map和RxJava好像)
    .map { it.toUpperCase() }
    // 打印出来
    .forEach { println(it) }
```
### 参考资料
[本文翻译的文档](http://kotlinlang.org/docs/reference/basic-syntax.html)
[Kotlin官方文档](http://kotlinlang.org/docs/reference/)
[Kotlin在线编辑器](https://try.kotlinlang.org/#/Examples/Hello,%20world!/Simplest%20version/Simplest%20version.kt)





