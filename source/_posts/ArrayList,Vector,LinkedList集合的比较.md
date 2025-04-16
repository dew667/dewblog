---
title: ArrayList,Vector,LinkedList集合的比较
---

### ArrayList、Vector、LinkedList三者的继承实现关系

ArrayList、LinkedList、Vector都实现了List接口，而LinkedList还实现了Queue接口。因而，ArrayList、Vector都有列表的基本方法add、remove、get、set等，LinkedList除具有List的基本方法外还有队列的一些方法如poll、peek等。

下图展示了三者的继承实现关系：  
![pic1](https://s1.ax1x.com/2020/07/06/UixDjH.jpg)

### ArrayList 与LinkedList 的区别

-   是否线程安全：由于ArrayList 和Linkedlist 都不是同步的，所以不是保证线程安全的。
    
-   底层数据结构：ArrayList 底层数据结构采用的是Object数组，而LinkedList底层数据结构采用的是双向链表（JDK1.7之前为双向循环链表，1.7开始取消循环）。
    
-   删除和插入的时间复杂度：ArrayList由于采用数组作为底层数据结构，所以在中间某一位置删除或插入元素，其时间复杂度为O(n-i)，因为插入时需要将i位置后面n-i个元素后移，删除时则需要前移。LinkedList由于采用双向链表作为底层数据结构，所以插入和删除元素不受位置因素影响，时间复杂度都是O(1)。
    
-   是否支持随机访问：ArrayList支持随机访问而LinkedList不支持随机访问，因为ArrayList采用数组，数组是天然支持随机访问的，链表就不行，访问元素需要遍历。因此，ArrayList实现了RandomAccess这一接口，而LinkedList没有。RandomAccess接口就是一种随机访问的标记。
    
-   内存空间占用：ArrayList内存占用冗余之处体现在需要预留一定的数组末尾空间，而LinkedList内存占用体现在每一个链表节点都有前驱、后继和数据。
<!--more-->

**_附_** _RandomAceess与循环遍历方法_  
实现了RandomAccess接口的list，优先选择普通for 循环， 其次foreach  
未实现RandomAccess接口的list，优先选择iterator 遍历(foreach底层也是通过iterator实现的)，大size的数据，千万不要用for 循环  
采用上述遍历策略才能节省时间

    if (list instanceof RandomAccess){
                System.out.println("实现了RandomAccess接口，不使用迭代器");
    
                for (int i = 0;i < list.size();i++){
                    System.out.println(list.get(i));
                }
    
            }else{
                System.out.println("没实现RandomAccess接口，使用迭代器");
    
                Iterator it = list.iterator();
                while(it.hasNext()){
                    System.out.println(it.next());
                }
    
            }

### ArrayList与Vector的区别？为什么要用ArrayList取代Vector呢？

-   Vector类中的所有方法都是同步的，所以可以由两个或多个线程安全的访问一个Vector对象，但是一个线程访问Vector的话要在同步操作上耗费大量的时间。
    
-   ArrayList不是同步的，所以在不需要保证线程安全的情况下建议使用ArrayList。
    
-   ArrayList与Vector初始容量都为10，但在容量不足的情况下，ArrayList缺省情况下自动增长50%，而Vector在缺省情况下自动增长1倍。