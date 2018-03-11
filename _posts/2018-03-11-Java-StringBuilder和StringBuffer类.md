---
layout: post
title: Java StringBuilder和StringBuffer类
subtitle:
#bigimg: /img/path.jpg
tags: [java]
---
## 转自[菜鸟教程]（http://www.runoob.com）
当对字符串进行修改的时候，需要使用 StringBuffer 和 StringBuilder 类。

和 String 类不同的是，StringBuffer 和 StringBuilder 类的对象能够被多次的修改，并且不产生新的未使用对象。

StringBuilder 类在 Java 5 中被提出，它和 StringBuffer 之间的最大不同在于 StringBuilder 的方法不是线程安全的（不能同步访问）。

由于 StringBuilder 相较于 StringBuffer 有速度优势，所以多数情况下建议使用 StringBuilder 类。然而在应用程序要求线程安全的情况下，则必须使用 StringBuffer 类。

```java
public class Test{
  public static void main(String args[]){
    StringBuffer sBuffer = new StringBuffer("菜鸟教程官网：");
    sBuffer.append("www");
    sBuffer.append(".runoob");
    sBuffer.append(".com");
    System.out.println(sBuffer);  
  }
}
```
以上实例编译运行结果如下：

~~~
菜鸟教程官网：www.runoob.com
~~~

总结如下：

* String 长度大小不可变
* StringBuffer 和 StringBuilder 长度可变
* StringBuffer 线程安全 StringBuilder 线程不安全
* StringBuilder 速度快
