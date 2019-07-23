# Java-
个人记录的一些Java重点和一些笔记 ，参考（CyC2018的GitHub）[https://github.com/CyC2018/CS-Notes/blob/master/notes/Java%20%E5%9F%BA%E7%A1%80.md#%E9%9A%90%E5%BC%8F%E7%B1%BB%E5%9E%8B%E8%BD%AC%E6%8D%A2]
# String

```java
public final class String
```
因为String类被声明为final，所以不能被继承，不能被重写。

---
字符串常量池（String Pool）
java中对String对象在内存区域分了两个模块，一块是存储普通字符串对象，另一块就是String Pool存储字符串常量。
字符串常量池保存着所有的字符串字面量（literal strings），这些字面量在编译时就已经确定。可以使用String的intern（）方法在运行过程中将字符串添加到String Pool中。
当一个字符串调用intern（）时，如果String Pool中已经存在一个字符串和该字符串相等（使用equals（）方法进行确定），那么就会返回String Pool中字符串的引用；否则就会在String Pool中新增一个新的字符串，并返回该字符串的应用。
实例
s1和s2采用的new String（）的方式创建了两个不同的字符串，s3和s4使用s1.intern（）方法取得一个字符串引用。

```java
String s1 = new String("aaa")；
String s2 = new String("aaa")；
System.out.printf( s1 = s2 );       //false
String s3 = s1.intern();
String s4 = s1.intern();
System.out.printf( s3 = s4 );       //true
```
之前没有"aaa"字符串，所以intern()首先把s1的引用放到String Pool中返回引用给s3；s1再次调用intern()时，在String Pool中已经存在了"aaa"字符串，所以直接返回引用给s4。因此s3和s4引用的是同一个字符串。
如果是采用字面量的形式创建字符串，会自动的将字符串放到String Pool中

```java
String s5 = "bbb";
String s6 = "bbb";
System.out.pringtf( s5 = s6 );      //true
```
在java 7 之前，String Pool被放在运行时常量池中，它属于永久代；而在java 7，String Pool被移到堆（heap）中，这是因为永久代的空间有限，在大量使用字符串的场景下会导致OutOfMemoryError错误。  
[深入解析String#intern](https://tech.meituan.com/2014/03/06/in-depth-understanding-string-intern.html)
# equals
Object常用方法  
```java
public boolean equals(Object obj)
public native int hashCode()
```
equals()  
1.等价关系  
- 自反性  
```java
x.equals(x);        //true
```
- 对称性  
```java
x.equals(y) == y.equals(x);         //true
```
- 传递性  
```java
x.equals(y) && y.equals(z);
x.equals(z);        //true
```
- 一致性  
```java
x.equals(y);     //true
x.equals(y);     //true
```
- 与null比较  
对任何不是null的对象调用equals结果都是false  
```java
x.equals(null);     //true
```
2.等价与相等  
- 对于基本类型，==判断两个值是否相等，基本类型没有equals()方法  
- 对于引用类型，==判断两个变量是否引用同一个对象；equals()判断引用的对象是否等价（即值是否相等）  
```java
Integer x == new Integer(1);
Integer y == new Integer(1);
System.out.println(x == y);         //false
System.out.println(x.equals(y));        //true
```
x == y结果是false的原因就是x和y分别创建了两个不同的变量分别引用各自的对象；x.equals(y)的结果为true的原因就是x和y所引用的对象值都是1
