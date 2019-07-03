### 1. 编码
zset的编码有**ziplist**和**skiplist**两种。
底层分别使用**ziplist（压缩链表）**和**skiplist（跳表）**实现。

* ##### 什么时候使用ziplist什么时候使用skiplist？
当zset满足以下两个条件的时候，使用ziplist：
> 1. 保存的元素少于128个
> 2. 保存的所有元素大小都小于64字节

**不满足这两个条件则使用skiplist。**
（注意：这两个数值是可以通过```redis.conf```的```zset-max-ziplist-entries``` 和 ```zset-max-ziplist-value```选项 进行修改。）

***

### 2. 实现
* ##### ziplist编码

> 关于什么是ziplist（压缩链表），可以参见这篇文章：[Redis源码分析-压缩列表ziplist](https://www.jianshu.com/p/afaf78aaf615)

ziplist 编码的有序集合对象使用压缩列表作为底层实现，每个集合元素使用两个紧挨在一起的压缩列表节点来保存，第一个节点保存元素的成员，第二个节点保存元素的分值。并且压缩列表内的集合元素按分值从小到大的顺序进行排列，小的放置在靠近表头的位置，大的放置在靠近表尾的位置。
![从小到大排列](https://upload-images.jianshu.io/upload_images/1038472-ea9ab3a8c73124f7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![ziplist结构](https://upload-images.jianshu.io/upload_images/1038472-6f812227fb90b78d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
* ##### skiplist编码
skiplist 编码的有序集合对象使用 zet 结构作为底层实现，一个 zset 结构同时包含一个字典和一个跳表：
```
typedef struct zset{
     //跳跃表
     zskiplist *zsl;
     //字典
     dict *dice;
} zset;
```
　　字典的键保存元素的值，字典的值则保存元素的分值；跳跃表节点的 object 属性保存元素的成员，跳跃表节点的 score 属性保存元素的分值。

　　这两种数据结构会**通过指针来共享相同元素的成员和分值**，所以不会产生重复成员和分值，造成内存的浪费。

>**说明**：其实有序集合单独使用字典或跳跃表其中一种数据结构都可以实现，但是这里使用两种数据结构组合起来，原因是假如我们单独使用 字典，虽然能以 O(1) 的时间复杂度查找成员的分值，但是因为字典是以无序的方式来保存集合元素，所以**每次进行范围操作的时候都要进行排序**；假如我们单独使用跳跃表来实现，虽然能执行范围操作，**但是查找操作有 O(1)的复杂度变为了O(logN)**。因此**Redis使用了两种数据结构来共同实现有序集合。**
***
参考资料：
1. [Redis详解（五）------ redis的五大数据类型实现原理](https://www.cnblogs.com/ysocean/p/9102811.html)
2. [Redis源码分析-压缩列表ziplist](https://www.jianshu.com/p/afaf78aaf615)
