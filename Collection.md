
1. Relationship - Collection, Map. - List,Set,Queue, Map.

![](https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1522341886&di=121ed8bcdb7f1cc655547803f51858fd&src=http://f.hiphotos.baidu.com/zhidao/pic/item/7dd98d1001e939017c5ba7c57bec54e737d196eb.jpg)
* 同步容器   
       主要代表有Vector和Hashtable，以及Collections.synchronizedXxx等。


       锁的粒度为当前对象整体。  


       迭代器是及时失败的，即在迭代的过程中发现被修改，就会抛出ConcurrentModificationException。  

* 并发容器  
       主要代表有ConcurrentHashMap、CopyOnWriteArrayList、ConcurrentSkipListMap、ConcurrentSkipListSet  


       锁的粒度是分散的、细粒度的，即读和写是使用不同的锁。  


       迭代器具有弱一致性，即可以容忍并发修改，不会抛出ConcurrentModificationException。  

* 阻塞队列  
        主要代表有LinkedBlockingQueue、ArrayBlockingQueue、PriorityBlockingQueue(Comparable,Comparator)、SynchronousQueue。  


        提供了可阻塞的put和take方法，以及支持定时的offer和poll方法。  


        适用于生产者、消费者模式（线程池和工作队列-Executor）  


        同时也是同步容器  

* 双端队列和工作密取 
           主要代表有ArrayDeque和LinkedBlockingDeque。  


           意义：正如阻塞队列适用于生产者消费者模式，双端队列同样适用与另一种模式，即工作密取。在生产者-消费者设计中，所有消费者共享一个工作队列， 而在工作密取中，每个消费者都有各自的双端队列。  

                   如果一个消费者完成了自己双端队列中的全部工作，那么他就可以从其他消费者的双端队列末尾秘密的获取工作。具有更好的可伸缩性，这是因为工作者线程不会在单个共享的任务队列上发生竞争 。  

                   在大多数时候，他们都只是访问自己的双端队列，从而极大的减少了竞争。当工作者线程需要访问另一个队列时，它会从队列的尾部而不是头部获取工作，因此进一步降低了队列上的竞争。  
2. Compare  
*ArrayList 是线性表（数组）
get() 直接读取第几个下标，复杂度 O(1)
add(E) 添加元素，直接在后面添加，复杂度O（1）
add(index, E) 添加元素，在第几个元素后面插入，后面的元素需要向后移动，复杂度O（n）
remove（）删除元素，后面的元素需要逐个移动，复杂度O（n)  .
*LinkedList 是链表的操作
get() 获取第几个元素，依次遍历，复杂度O(n)
add(E) 添加到末尾，复杂度O(1)
add(index, E) 添加第几个元素后，需要先查找到第几个元素，直接指针指向操作，复杂度O(n)
remove（）删除元素，直接指针指向操作，复杂度O(1)


2. BlockingQueue,Dequeue
* 阻塞队列（BlockingQueue）是一个支持两个附加操作的队列。这两个附加的操作是：在队列为空时，获取元素的线程会等待队列变为非空。当队列满时，存储元素的线程会等待队列可用。阻塞队列常用于生产者和消费者的场景，生产者是往队列里添加元素的线程，消费者是从队列里拿元素的线程。阻塞队列就是生产者存放元素的容器，而消费者也只从容器里拿元素。
ArrayBlockingQueue ：一个由数组结构组成的有界阻塞队列。  
LinkedBlockingQueue ：一个由链表结构组成的有界阻塞队列。  
PriorityBlockingQueue ：一个支持优先级排序的无界阻塞队列。  
DelayQueue：一个使用优先级队列实现的无界阻塞队列。  
SynchronousQueue：一个不存储元素的阻塞队列。  
LinkedTransferQueue：一个由链表结构组成的无界阻塞队列。  
LinkedBlockingDeque：一个由链表结构组成的双向阻塞队列  
* Deque的含义是“double ended queue”，即双端队列.
Deque是一种具有队列和栈的性质的数据结构.双端队列中的元素可以从两端弹出,其限定插入和删除操作在表的两端进行.

![](https://img-blog.csdn.net/20170318120808825?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbDU0MDY3NTc1OQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

3. Hash confilict , Tree set
4. [Collections](https://docs.oracle.com/javase/7/docs/api/java/util/Collections.html) & [Arrays](https://docs.oracle.com/javase/7/docs/api/java/util/Arrays.html)
5. some problems. 
Rotate Array - n rotate k. 
Find Min in Rotated Array   
Maximum Subarray   
Linked List Cycle.  
6. 数组实现Stack等
