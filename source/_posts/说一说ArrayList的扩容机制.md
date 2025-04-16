---
title: 说一说ArrayList的扩容机制
---

[ArrayList 扩容机制源码分析](https://github.com/Snailclimb/JavaGuide/blob/master/docs/java/collection/ArrayList-Grow.md)

-   通过ArrayList构造方法初始化时，可以带初始容量，则为ArrayList列表设置指定初始容量。
    
-   如果通过ArrayList构造方法初始化，不带初始容量参数，则ArrayList列表中的数据初始化为空的Object\[\]数组，其长度为0，即DEFEAULTCAPACITY\_ELEMENT\_DATA。之后在第一次调用add()方法添加元素时才会初始化数组容量为默认大小10.
    
-   当调用add()方法添加元素时，首先会比较扩容大小和默认大小10，如果是第一次扩容，则会在grow()方法中扩展数组大小为10。一般情况下，每次扩容时，都会将数组大小扩展为原来的1.5倍。
    
-   扩容时，会用到System.arrycopy方法。这里会新建一个扩容后的数组，将原来数组中的数据拷贝过去，在将ArrayList中elementData数组引用指向新数组，这样就完成了扩容。
    
-   在我们用add()方法添加大量元素之前，可以调用ensureCapacity()方法来扩容，以减少容量重新分配的次数。