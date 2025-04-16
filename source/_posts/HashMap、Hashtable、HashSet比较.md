---
title: HashMap、Hashtable、HashSet比较
---

### HashMap Hashtable HashSet继承实现关系

下面是三者的继承实现关系示意图

![pic1](https://s1.ax1x.com/2020/07/07/UFZ5Fg.jpg)

### HashMap 和HashTable 的区别.

-   线程安全: HashMap不是线程安全的，HashTable 是线程安全的。HashTable 中方法都有synchronized 修饰。如果要用线程安全的集合，可以考虑ConcurrentHashMap。
    
-   效率：因为线程安全的原因，HashTable 的效率要低于HashMap。
    
-   是否支持Null Key 和Null Value：HashMap支持为Null 的Key，但一个HashMap集合只能包含一个，同时HashMap支持使用多个值为Null的Value。HashTable则不行，若put进入Null则会报错：NullPointerException.
    
-   底层数据结构：1.8版本开始，HashMap底层数据结构为数组承载的链表或者红黑树，当链表长度大于阈值时，链表会转换为红黑树。HashTable中没有这样的机制。
    
-   初始容量和扩容：HashTable若不指定初始容量，则默认初始容量为11，以后每次扩容，都变为2n+1. HashMap若不指定初始容量，则默认容量为16，每次扩容都为原来两倍。HashTable 若指定初始容量，则初始容量为指定值。HashMap 若指定初始值，则初始容量为2的幂次。
     <!--more-->

### HashMap 和HashSet 的区别

HashSet底层都是通过HashMap实现的。

**HashMap**

**HashSet**

实现了Map接口

实现了Set接口

存储键值对

存储对象元素

put添加键值对

add添加元素

HashMap使用Key计算hashcode

HashSet使用成员对象来计算hashcode