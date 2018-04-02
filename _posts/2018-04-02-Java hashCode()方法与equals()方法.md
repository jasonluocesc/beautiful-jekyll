---
layout: post
title: Java hashCode()方法与equals()方法
subtitle:
tags: [java]
---
## 部分转自[cnblog瞄了个咪](https://www.cnblogs.com/Qian123/p/5703507.html)
### Hashset、Hashmap、Hashtable与hashcode()和equals()的密切关系
Hashset是继承Set接口，Set接口又实现Collection接口，这是层次关系。那么Hashset、Hashmap、Hashtable中的存储操作是根据什么原理来存取对象的呢？

下面以HashSet为例进行分析，我们都知道：在hashset中不允许出现重复对象，元素的位置也是不确定的。在hashset中又是怎样判定元素是否重复的呢？在java的集合中，判断两个对象是否相等的规则是：
1.判断两个对象的hashCode是否相等
如果不相等，认为两个对象也不相等，完毕
如果相等，转入2
（这一点只是为了提高存储效率而要求的，其实理论上没有也可以，但如果没有，实际使用时效率会大大降低，所以我们这里将其做为必需的。）

2.判断两个对象用equals运算是否相等
如果不相等，认为两个对象也不相等
如果相等，认为两个对象相等（equals()是判断两个对象是否相等的关键）
为什么是两条准则，难道用第一条不行吗？不行，因为前面已经说了，hashcode()相等时，equals()方法也可能不等，所以必须用第2条准则进行限制，才能保证加入的为非重复元素。

例1：
```Java
package com.bijian.study;

import java.util.HashSet;
import java.util.Iterator;
import java.util.Set;

public class HashSetTest {

    public static void main(String args[]) {
        String s1 = new String("aaa");
        String s2 = new String("aaa");
        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
        System.out.println(s1.hashCode());
        System.out.println(s2.hashCode());
        Set hashset = new HashSet();
        hashset.add(s1);
        hashset.add(s2);
        Iterator it = hashset.iterator();
        while (it.hasNext()) {
            System.out.println(it.next());
        }
    }
}
```
运行结果：
```Java
false
true
96321
96321
aaa
```
这是因为String类已经重写了equals()方法和hashcode()方法，所以hashset认为它们是相等的对象，进行了重复添加。

例2：
```Java
package com.bijian.study;

import java.util.HashSet;
import java.util.Iterator;

public class HashSetTest {

    public static void main(String[] args) {
        HashSet hs = new HashSet();
        hs.add(new Student(1, "zhangsan"));
        hs.add(new Student(2, "lisi"));
        hs.add(new Student(3, "wangwu"));
        hs.add(new Student(1, "zhangsan"));

        Iterator it = hs.iterator();
        while (it.hasNext()) {
            System.out.println(it.next());
        }
    }
}

class Student {
    int num;
    String name;

    Student(int num, String name) {
        this.num = num;
        this.name = name;
    }

    public String toString() {
        return num + ":" + name;
    }
}
```
运行结果：
1:zhangsan  
3:wangwu  
2:lisi  
1:zhangsan

为什么hashset添加了相等的元素呢，这是不是和hashset的原则违背了呢？回答是：没有。因为在根据hashcode()对两次建立的new Student(1,“zhangsan”)对象进行比较时，生成的是不同的哈希码值，所以hashset把他当作不同的对象对待了，当然此时的equals()方法返回的值也不等。

为什么会生成不同的哈希码值呢？上面我们在比较s1和s2的时候不是生成了同样的哈希码吗？原因就在于我们自己写的Student类并没有重新自己的hashcode()和equals()方法，所以在比较时，是继承的object类中的hashcode()方法，而object类中的hashcode()方法是一个本地方法，比较的是对象的地址（引用地址），使用new方法创建对象，两次生成的当然是不同的对象了，造成的结果就是两个对象的hashcode()返回的值不一样，所以Hashset会把它们当作不同的对象对待。

怎么解决这个问题呢？答案是：在Student类中重新hashcode()和equals()方法。
