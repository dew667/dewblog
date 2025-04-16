---
title: ConcurrentHashMap和HashTable的区别
---

-   ConcurrentHashMap和HashTable的区别  
    ConcurrentHashMap和Hashtable的主要区别在于实现线程安全性的方式不同。
    
-   底层数据结构：  
    JDK1.7的ConcurrentHashMap底层数据结构采用分段的数组+链表实现，JDK1.8的ConcurrentHashMap底层数据结构采用和HashMap一样的方式实现，即数组+链表/红黑树。Hashtable的底层数据结构则和JDK1.8之前的HashMap一样，采用数组+链表的方式。
     <!--more-->
-   实现线程安全性的方式不同：  
    **ConcurrentHashMap**：  
    JDK1.7的ConcurrentHashMap底层采用了桶数组分段加锁的方式，在不同线程访问不同分段资源的时候，不存在锁竞争，这样加大了并发访问效率。到了JDK1.8，ConcurrentHashMap抛弃了这种方式，采用数组+链表/红黑树的数据结构，并发访问控制采用Synchronized和CAS来操作，相当于线程安全的HashMap。  
    **Hashtable**：  
    Hashtable采用数组+链表的数据结构，并发控制采用synchronized关键字的方式，其并发访问效率较低。一个线程访问同步方法时，另一个线程无法访问该同步方法。
    
-   下面是两种集合线程安全性的示意图：  
    ![pic2](https://s1.ax1x.com/2020/07/08/UVTWV0.jpg)  
    ![pic3](https://s1.ax1x.com/2020/07/08/UVT2bq.jpg)  
    ![pic4](https://s1.ax1x.com/2020/07/08/UVTfaV.jpg)