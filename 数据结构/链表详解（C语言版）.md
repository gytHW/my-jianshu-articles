&emsp;&emsp;链表和线性表相对，指的是在物理内存上并非相连线性存储的一种数据结构。链表中的每个元素都是分散存储的。因为分散存储，为了能够体现出数据元素之间的逻辑关系，每个数据元素在存储的同时，要配备一个指针，用于指向它的直接后继元素，即每一个数据元素都指向下一个数据元素（最后一个指向NULL(空))。

## 链表中数据元素的构成
每个元素本身由两部分组成：
>* 本身的信息，称为**“数据域”**；
>* 指向直接后继的指针，称为**“指针域”**。

![节点的构成](https://upload-images.jianshu.io/upload_images/1038472-894c089ac6ff4b8a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这两部分信息组成数据元素的存储结构，称之为“结点”。n个结点通过指针域相互链接，组成一个链表。
![含有n个节点的链表](https://upload-images.jianshu.io/upload_images/1038472-b232311a88a87544.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图中，由于每个结点中只包含一个指针域，生成的链表又被称为 **线性链表** 或 **单链表**。
链表中存放的不是基本数据类型，需要用结构体自定义实现：
```c
typedef struct Link{
    char elem;//代表数据域
    struct Link * next;//代表指针域，指向直接后继元素
}link;
```
***
## 头结点、头指针和首元结点
* **头结点**： 有时，在链表的第一个结点之前会额外增设一个结点，结点的数据域一般不存放数据（有些情况下也可以存放链表的长度等信息），此结点被称为头结点。
> 若头结点的指针域为空（NULL），表明链表是空表。头结点对于链表来说，**不是必须的**，在处理某些问题时，**给链表添加头结点会使问题变得简单**。
* **首元结点**：链表中**第一个元素**所在的结点，它是头结点后边的第一个结点。
* **头指针**：永远指向链表中第一个结点的位置（如果链表有头结点，头指针指向头结点；否则，头指针指向首元结点）。
> 头结点和头指针的**区别**：头指针是一个指针，头指针指向链表的头结点或者首元结点；头结点是一个实际存在的结点，它包含有数据域和指针域。两者在程序中的直接体现就是：**头指针只声明而没有分配存储空间，头结点进行了声明并分配了一个结点的实际物理内存。**

![头结点、首元结点和头指针](https://upload-images.jianshu.io/upload_images/1038472-243d165e52b00d1c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

>**注意：单链表中可以没有头结点，但是不能没有头指针！**
***
##链表的创建和遍历
万事开头难，初始化链表首先要做的就是创建链表的头结点或者首元结点。创建的同时，要保证有一个指针永远指向的是链表的表头，这样做不至于丢失链表。
例如创建一个链表（1，2，3，4）:
```
link * initLink(){
    link * p=(link*)malloc(sizeof(link));//创建一个头结点
    link * temp=p;//声明一个指针指向头结点，用于遍历链表
    //生成链表
    for (int i=1; i<5; i++) {
        link *a=(link*)malloc(sizeof(link));
        a->elem=i;
        a->next=NULL;
        temp->next=a; //这里*temp是一个指向头结点的指针，所以temp->next指的是头结点的next域
        temp=temp->next;
    }
    return p;
}
```
这里是尾插法建立链表，建立链表有两种方法：**头插法**和**尾插法**
![头插法和尾插法的区别](https://upload-images.jianshu.io/upload_images/1038472-bd404830d0897afe.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

***
## 链表中查找某节点
一般情况下，链表只能通过头结点或者头指针进行访问，所以实现查找某结点最常用的方法就是对链表中的结点进行逐个遍历。

实现代码：
```
int selectElem(link * p,int elem){
    link * t=p;
    int i=1;
    while (t->next) {
        t=t->next;
        if (t->elem==elem) {
            return i;
        }
        i++;
    }
    return -1;
}
```
## 更改某节点的数据域
直接遍历链表，找到节点，然后直接更改其数据域即可。
实现代码：
```
//更新函数，其中，pos表示更改结点在链表中的位置，newElem 为新的数据域的值
int *amendElem(link * p, int pos, int newElem){
    link * temp = p;
     temp=temp->next;//在遍历之前，temp指向首元结点
    //遍历到被删除结点
    for (int i=1; i<pos; i++) {
        temp=temp->next;
    }
    temp->elem=newElem;
    return p;
}
```
***
## 向链表中插入结点
链表中插入头结点，根据插入位置的不同，分为3种：
* 插入到链表的首部，也就是头结点和首元结点中间；
* 插入到链表中间的某个位置；
* 插入到链表最末端；

![向链表中插入节点5](https://upload-images.jianshu.io/upload_images/1038472-3042d424f02069d4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

虽然插入位置有区别，都使用相同的插入手法。分为两步，如上图所示：
> 1. 将新结点的next指针指向插入位置后的结点；
> 2. 将插入位置前的结点的next指针指向插入结点；

提示：在做插入操作时，首先要找到插入位置的上一个结点，图4中，也就是找到结点 1，相应的结点 2 可通过结点 1 的 next 指针表示，这样，先进行步骤 1，后进行步骤 2，实现过程中不需要添加其他辅助指针。
要先执行1再2，不能颠倒，如果先执行2，则后续节点就丢失了无法找到。

实现代码：
```
link * insertElem(link * p,int elem,int add){
    link * temp=p;//创建临时结点temp
    //首先找到要插入位置的上一个结点
    for (int i=1; i<add; i++) {
        if (temp==NULL) {
            printf("插入位置无效\n");
            return p;
        }
        temp=temp->next;
    }    
    //创建插入结点c
    link * c=(link*)malloc(sizeof(link));
    c->elem=elem;
    //向链表中插入结点
    c->next=temp->next;
    temp->next=c;
    return  p;
}
```
注意：首先要保证插入位置的可行性，例如图 5 中，原本只有 5 个结点，插入位置可选择的范围为：1-6，如果超过6，本身不具备任何意义，程序提示插入位置无效。
***
##从链表中删除节点
当需要从链表中删除某个结点时，需要进行两步操作：
>* 将结点从链表中摘下来;
>* 手动释放掉结点，回收被结点占用的内存空间;

>使用malloc函数申请的空间，一定要注意手动free掉。否则在程序运行的整个过程中，申请的内存空间不会自己释放（只有当整个程序运行完了以后，这块内存才会被回收），造成内存泄漏，别把它当成是小问题。

实现代码：
```
link * delElem(link * p,int add){
    link * temp=p;
    //temp指向被删除结点的上一个结点
    for (int i=1; i<add; i++) {
        temp=temp->next;
    }
    link * del=temp->next;//单独设置一个指针指向被删除结点，以防丢失
    temp->next=temp->next->next;//删除某个结点的方法就是更改前一个结点的指针域
    free(del);//手动释放该结点，防止内存泄漏
    return p;
}
```
