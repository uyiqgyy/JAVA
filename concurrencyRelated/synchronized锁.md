## Synchronized锁

> 2018/08/02

### 1. synchronized表现形式

1.1 Java中的每一个对象都可以作为锁。

JVM会为每个对象分配一个monitor，而同时只能有一个线程可以获得该对象monitor的所有权。在线程进入时通过monitorenter尝试取得对象monitor所有权，退出时通过monitorexit释放对象monitor所有权。

monitorenter与monitorexit在编译后对称插入代码。

* monitorenter: 被插入到同步代码块之前。
* monitorexit: 被插到同步代码块之后或异常处。

synchronized 

* 同步方法
* 同步代码块
* 同步方法块

1.2 锁是什么

* 实例对象
* Class对象

### 2.Java对象头

>HotSpot虚拟机中，对象在内存中存储的布局可以分为三块区域：对象头（Header）、实例数据（Instance Data）和对齐填充（Padding）.

1.1 对象头的长度

长度|内容|说明
---|---|---
32/64 bit|Mark Word|存储对象的hashCode或者锁信息
32/64 bit|Class Metadata Address|存储到对象类型数据的指针
32/32 bit|Array length|数组的长度（如果当前是数组）

HotSpot虚拟机的对象头(Object Header)包括两部分信息，第一部分用于存储对象自身的运行时数据， 如哈希码（HashCode）、GC分代年龄、锁状态标志、线程持有的锁、偏向线程ID、偏向时间戳等等，这部分数据的长度在32位和64位的虚拟机（暂 不考虑开启压缩指针的场景）中分别为32个和64个Bits，官方称它为“Mark Word”。

对象需要存储的运行时数据很多，其实已经超出了32、64位Bitmap结构所能记录的限度，但是对象头信息是与对象自身定义的数据无关的额 外存储成本，考虑到虚拟机的空间效率，Mark Word被设计成一个非固定的数据结构以便在极小的空间内存储尽量多的信息，它会根据对象的状态复用自己的存储空间。例如在32位的HotSpot虚拟机 中对象未被锁定的状态下，Mark Word的32个Bits空间中的25Bits用于存储对象哈希码（HashCode），4Bits用于存储对象分代年龄，2Bits用于存储锁标志 位，1Bit固定为0，在其他状态（轻量级锁定、重量级锁定、GC标记、可偏向）下对象的存储内容如下表所示。

对象头的另外一部分是类型指针，即是对象指向它的类的元数据的指针，虚拟机通过这个指针来确定这个对象是哪个类的实例。并不是所有的虚拟机实现都必须在对象数据上保留类型指针，换句话说查找对象的元数据信息并不一定要经过对象本身。另外，如果对象是一个Java数组，那在对象头中还必须有一块用于记录数组长度的数据，因为虚拟机可以通过普通Java对象的元数据信息确定Java对象的大小，但是从数组的元数据中无法确定数组的大小。    

1.2 对象头Mark Word的存储结构(32 bits)

![对象头Mark Word](https://wx3.sinaimg.cn/mw690/7968cbf7gy1ftvsr5kd9jj20rr09nq5z.jpg)


### 3 锁的升级和对比

锁的升级关系：

![锁的升级关系](https://blog.dreamtobe.cn/img/java_synchronized.png)


