---
layout: post
title: Java Indexof()方法
subtitle:
tags: [java]
---

今天遇到这样一个题目：

假定我们都知道非常高效的算法来检查一个单词是否为其他字符串的子串。请将这个算法编写成一个函数，给定两个字符串s1和s2，请编写代码检查s2是否为s1旋转而成，要求只能调用一次检查子串的函数。

给定两个字符串s1,s2,请返回bool值代表s2是否由s1旋转而成。字符串中字符为英文字母和空格，区分大小写，字符串长度小于等于1000。
测试样例：
~~~
"Hello world","worldhello "
返回：false
"waterbottle","erbottlewat"
返回：true
~~~
学到了一个新解法就是，重复一遍s1，检验s2是否为s1+s1的子序列即可，很灵巧！

使用String.indexof()即可！

```java
boolean isRotation(String s1,String s2) {
    return (s1.length() == s2.length()) && ((s1+s1).indexOf(s2) != -1);
}
```

Java中字符串中子串的查找共有四种方法，如下：
1.int indexOf(String str) ：返回第一次出现的指定子字符串在此字符串中的索引。
2.int indexOf(String str, int startIndex)：从指定的索引处开始，返回第一次出现的指定子字符串在此字符串中的索引。
3.int lastIndexOf(String str) ：返回在此字符串中最右边出现的指定子字符串的索引。
4.int lastIndexOf(String str, int startIndex) ：从指定的索引处开始向后搜索，返回在此字符串中最后一次出现的指定子字符串的索引。
