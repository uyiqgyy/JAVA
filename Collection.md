
1. Relationship - Collection, Map. - List,Set,Queue, Map.

![](https://ss0.bdstatic.com/94oJfD_bAAcT8t7mm9GUKT-xh_/timg?image&quality=100&size=b4000_4000&sec=1522341886&di=121ed8bcdb7f1cc655547803f51858fd&src=http://f.hiphotos.baidu.com/zhidao/pic/item/7dd98d1001e939017c5ba7c57bec54e737d196eb.jpg)

2. Compare  
ArrayList 是线性表（数组）
get() 直接读取第几个下标，复杂度 O(1)
add(E) 添加元素，直接在后面添加，复杂度O（1）
add(index, E) 添加元素，在第几个元素后面插入，后面的元素需要向后移动，复杂度O（n）
remove（）删除元素，后面的元素需要逐个移动，复杂度O（n)  .
LinkedList 是链表的操作
get() 获取第几个元素，依次遍历，复杂度O(n)
add(E) 添加到末尾，复杂度O(1)
add(index, E) 添加第几个元素后，需要先查找到第几个元素，直接指针指向操作，复杂度O(n)
remove（）删除元素，直接指针指向操作，复杂度O(1)
2. BlockingQueue,Dequeue
3. Hash confilict , Tree set
 https://blog.csdn.net/abcd1430/article/details/52745155
4. Collections & Arrays
5. some problems. 
Rotate Array - n rotate k. 
Find Min in Rotated Array   
Maximum Subarray   
Linked List Cycle.  
