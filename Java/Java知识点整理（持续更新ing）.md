* ##### 1. HashMap 的查询时间复杂度

理想情况下是 O(1)的，但是实际中会出现 hash 碰撞，导致无法达到效果。

* ##### 2. LinkedList和ArrayList的区别

> • LinkedList 底层是基于双向链表实现的，而 ArrayList 底层是基于动态数组实现的；
• 查询的时候 LinkedList 的效率要低于 ArrayList，因为 LinkedList 需要遍历链表，而 ArrayList 底层数组根据下标直接获取数据。(数组是连续内存，可以通过移位直接找到内存地址，链表的内存节点是跳跃的，通过指针寻址)
• 插入删除数据的时候，LinkedList 效率比ArrayList 效率高，因为 ArrayList 在数据多的情况下会进行数组扩容或移动数组。

* ##### 3. 多进程与多线程区别

首先进程是资源分配的最小单元，线程是任务调度的最小单元

![多进程 vs 多线程](https://upload-images.jianshu.io/upload_images/1038472-c0d0a4fdda193499.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

