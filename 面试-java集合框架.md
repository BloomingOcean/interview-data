# List

## 一、ArrayList和Vector的区别

线程安全：ArrayList是非线程安全的，Vector是线程安全的

性能：ArrayList的性能比Vector高（ArrayList不需要关心锁的问题）

动态扩容：Vector每次扩容都会扩容一倍的大小，而ArrayList只扩容50%

## 二、Array 和 ArrayList 有何区别？

存储：Array可以存储基本数据类型和对象，但ArrayList只能存储对象

容量：Array是固定大小的，但是ArrayList是可以动态扩容的

## 三、Arraylist 与 LinkedList 区别?

ArrayList底层使用的是Object[ ]，而LinkLeList底层使用的是双向链表。正由于他们的底层实现不同，导致他们对数据操作时有着区别。ArrayList因为可以随机访问数据，所以适合查找操作比较多的情况。LinkedList因为底层是链表，进行插入、删除操作时直接把需要操作的节点的指针替换就可以了，所以更加适合插入、删除操作。





# Set

## 一、HashSet、LinkedListSet、TreeSet的区别

HashSet实现的是Set接口，但是底层使用的是HashMap实现，可以存储null值。

LinkedHashSet是HashSet的子类，能够按照添加顺序排序。

TreeSet底层使用红黑树，能够对添加进Set内的元素进行遍历，排序有自然排序和定制排序。

## 二、Iterator 和 ListIterator 有什么区别？

Iterator 可以遍历 Set 和 List 集合，而 ListIterator 只能遍历 List。

 Iterator 只能单向遍历，而 ListIterator 可以双向遍历（向前/后遍历）。 

ListIterator 从 Iterator 接口继承，然后添加了一些额外的功能，比如添加一个元素、替换一个元素、获取前面或后面元素的索引位置。





# Map（红黑树）

## 一、HashMap和Hashtable（table是真的小写）（Dictionary是啥？）

不同：存储、线程安全、底层数据结构

存储：HashMap允许key和value为null，但是Hashtable不允许

线程安全：HashMap是线程不安全的，Hashtable是线程安全的（每个方法都加了synchronize）

底层数据结构：1.8之前HashMap是数组+链表的形式，1.8之后就是数组+链表+红黑树。而Hashtable就只是数组+链表

推荐使用：HashMap由于不需要考虑线程安全问题，所以效率比Hashtable高，在单线程操作的情况下推荐使用HashMap。Hashtable在类注释中可以看到这个类是一个保留类，不推荐使用，多线程情况下目前更多的还是使用ConcurrentHashMap。

## 二、HashMap实现原理

1.8之前：数组和链表结合在⼀起使⽤也就是链表散列。利用hash判断当前元素存放的位置，如果当前位置存在元素的话，就判断该元素与要存⼊的元素的hash值以及key是否相同，如果相同的话，直接覆盖，不相同就通过拉链法解决冲突。

注：所谓 “拉链法” 就是：将链表和数组相结合。也就是说创建⼀个链表数组，数组中每⼀格就是⼀个链表。若遇到哈希冲突，则将冲突的值加到链表中即可。

1.8之后：当链表⻓度⼤于阈值（默认为 8）（将链表转换成红⿊树前会判断，如果当前数组的⻓度⼩于 64，那么会选择先进⾏数组扩容，⽽不 是转换为红⿊树）时，将链表转化为红⿊树，以减少搜索时间。

注：TreeMap和TreeSet已经1.8之后的HashMap都使用了红黑树的结构

## 三、HashMap和TreeMap的区别

相同点：都继承了AbstructMap

不同点：TreeMap还继承了NavigableMap（元素搜索）和SortMap（排序）接口，这让TreeMap有了排序的能力

选择：所以HashMap更适合插入、删除、定位元素。而TreeMap更适合有排序需求的情形。

## 四、HashMap的长度为什么是2的幂次方

Hash值的取值范围是-2147483648到2147483647，总共大概40多亿大小。但是内存是放不了这么多东西的，所以这个hash散列值是不能直接使用的，而是要经过一步取模运算，得到的余数才能放入数组下标中。这个数组下标的计算方法是“（n-1）& hash  ” （n是数组长度，&是“与”操作）。

为什么不用%取余而使用&取余？

因为采用二进制操作符&的性能比取余%的快，所以HashMap使用2的幂次方更便于“与”运算。

## 五、HashMap遍历的四种方法（迭代器、for、lambda、stream）

https://mp.weixin.qq.com/s/Zz6mofCtmYpABDL1ap04ow

六、Hashtable和ConcurrentHashMap的区别

Hashtable：Hashtable由于只使用一把锁，所以并发操作的时候，都会把整个数组全部加锁，效率非常低

ConcurrentHashMap：

1.7：使用分段锁![image-20210617233218748](../AppData/Roaming/Typora/typora-user-images/image-20210617233218748.png)

将数据一段一段的存储，给每一段数据都加上一把锁（分段锁），这样就算有数据访问了其中一段数据，对于使用另一把锁的数据也是可以并发执行的。

1.8：取消了Segment分段锁，采用CAS和synchronized 来保证并发安全。它和Hashtable的区别是：synchronized 只锁定当前链表或红⿊⼆叉树的⾸节点，这样只要 hash 不冲突，就不会产⽣并发。

CAS（Compare And Swap 比较交换）：核心函数CAS(V,E,N)，（V表示要更新的变量，E表示预期值，N表示新值）。如果V值等于E值，则将V的值设为N。若V值和E值不同，则说明已经有其他线程做了更新，则当前线程什么都不做。并且CAS是系统原语，是一条CPU的原子指令，在执行过程中不允许被打断，保证了线程安全。