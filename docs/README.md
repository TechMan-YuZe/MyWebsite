# TechMan-YuZe's Website

> An awesome project.

## 工具

### 开发工具

![image-20220503110719304](C:\Users\TechMan-YuZe\AppData\Roaming\Typora\typora-user-images\image-20220503110719304.png)

#### JetBrains 全家桶

官网：https://jetbrains.com

中文官网：https://www.jetbrains.com/zh-cn/

```java
package com.yu.sharedModel;

import lombok.extern.slf4j.Slf4j;

import java.util.concurrent.locks.ReentrantLock;

@Slf4j(topic = "c.ReentrantLockTest")
public class ReentrantLockTest {
    public static ReentrantLock lock = new ReentrantLock();
    public static void main(String[] args) {
        ReentrantLockTest test = new ReentrantLockTest();
        lock.lock();
        try {
            log.debug("main");
            test.method1();
        } finally {
            lock.unlock();
        }
    }

    public void method1() {
        lock.lock();
        try {
            log.debug("method1");
            method2();
        } finally {
            lock.unlock();
        }
    }

    public void method2() {
        lock.lock();
        try {
            log.debug("method2");
            System.out.println("method2");
        } finally {
            lock.unlock();
        }
    }
}
```

#### 2. VS Code

#### 3. Typora

- [链接](https://www.baidu.com)

## 学习

## 实战

# Java 基础



# Java 集合

## HashMap

1. **HashMap 的底层数据结构是什么？**

在 JDK1.7 中和 JDK1.8 中有所区别：



在 JDK1.7 中，由“数组 + 链表”组成，数组是 HashMap 的主体，链表则是主要为了解决哈希冲突而存在的。



在 JDK1.8 中，有“数组 + 链表 + 红黑树”组成。当链表过长，则会严重影响 HashMap 的性能，红黑树搜索时间复杂度是 O(logn)，而链表是 O(n)。因此，JDK1.8 对数据结构做了进一步的优化，引入了红黑树，链表和红黑树在达到一定条件会进行转换：

- 当链表超过 8 且数组长度 (数据总量) 超过 64 才会转为红黑树
- 将链表转换成红黑树前会判断，如果当前数组的长度小于 64，那么会选择先进行数组扩容，而不是转换为红黑树，以减少搜索时间。
- 
- ![img](https://cdn.nlark.com/yuque/0/2022/png/602399/1648726849296-54956204-8001-46d6-82a2-b20a14a638c3.png?x-oss-process=image%2Fresize%2Cw_1018%2Climit_0)

**2. 说一下 HashMap 的特点**

- HashMap 存取是无序的
- 键和值位置都可以是 null，但是键位置只能是一个 null
- 键位置是唯一的，底层的数据结构是控制键的
- JDK1.8 前数据结构是：链表 + 数组 jdk1.8 之后是：数组 + 链表 + 红黑树
- 阈值（边界值）>8 并且数组长度大于 64，才将链表转换成红黑树，变成红黑树的目的是提高搜索速度，高效查询



**3. 解决 hash 冲突的办法有哪些？HashMap 用的哪种？**



解决 Hash 冲突方法有：开放定址法、再哈希法、链地址法（HashMap 中常见的拉链法）、建立公共溢出区。HashMap 中采用的是链地址法。

- 开放定址法也称为再散列法，基本思想就是，如果 p=H(key) 出现冲突时，则以 p 为基础，再次 hash，p1=H(p)，如果 p1 再次出现冲突，则以 p1 为基础，以此类推，直到找到一个不冲突的哈希地址 pi。因此开放定址法所需要的 hash 表的长度要大于等于所需要存放的元素，而且因为存在再次 hash，所以只能在删除的节点上做标记，而不能真正删除节点
- 再哈希法（双重散列，多重散列），提供多个不同的 hash 函数，R1=H1(key1) 发生冲突时，再计算 R2=H2（key1），直到没有冲突为止。这样做虽然不易产生堆集，但增加了计算的时间。
- 链地址法（拉链法），将哈希值相同的元素构成一个同义词的单链表，并将单链表的头指针存放在哈希表的第 i 个单元中，查找、插入和删除主要在同义词链表中进行，链表法适用于经常进行插入和删除的情况。
- 建立公共溢出区，将哈希表分为公共表和溢出表，当溢出发生时，将所有溢出数据统一放到溢出区



注意开放定址法和再哈希法的区别是

- 开放定址法只能使用同一种 hash 函数进行再次 hash，再哈希法可以调用多种不同的 hash 函数进行再次 hash



**4. 为什么要在数组长度大于 64 之后，链表才会进化为红黑树**



在数组比较小时如果出现红黑树结构，反而会降低效率，而红黑树需要进行左旋右旋，变色，这些操作来保持平衡，同时数组长度小于 64 时，搜索时间相对要快些，总之是为了加快搜索速度，提高性能

JDK1.8 以前 HashMap 的实现是数组 + 链表，即使哈希函数取得再好，也很难达到元素百分百均匀分布。当 HashMap 中有大量的元素都存放在同一个桶中时，这个桶下有一条长长的链表，此时 HashMap 就相当于单链表，假如单链表有 n 个元素，遍历的时间复杂度就从 O（1）退化成 O（n），完全失去了它的优势，为了解决此种情况，JDK1.8 中引入了红黑树（查找的时间复杂度为 O（logn））来优化这种问题



**5. 为什么加载因子设置为 0.75，初始化临界值是 12？**



HashMap 中的 threshold 是 HashMap 所能容纳键值对的最大值。计算公式为 length*LoadFactory。也就是说，在数组定义好长度之后，负载因子越大，所能容纳的键值对个数也越大

loadFactory 越趋近于 1，那么数组中存放的数据（entry 也就越来越多），数据也就越密集，也就会有更多的链表长度处于更长的数值，我们的查询效率就会越低，当我们添加数据，产生 hash 冲突的概率也会更高



默认的 loadFactory 是 0.75，loadFactory 越小，越趋近于 0，数组中个存放的数据 (entry) 也就越少，表现得更加稀疏



![img](https://cdn.nlark.com/yuque/0/2022/png/602399/1648726849291-27412f1b-0b08-4116-a02b-951f584f4c40.png)

0.75 是对空间和时间效率的一种平衡选择



如果负载因子小一些比如是 0.4，那么初始长度 16*0.4=6，数组占满 6 个空间就进行扩容，很多空间可能元素很少甚至没有元素，会造成大量的空间被浪费



如果负载因子大一些比如是 0.9，这样会导致扩容之前查找元素的效率非常低



loadfactory 设置为 0.75 是经过多重计算检验得到的可靠值，可以最大程度的减少 rehash 的次数，避免过多的性能消耗



**6. 哈希表底层采用何种算法计算 hash 值？还有哪些算法可以计算出 hash 值？**



hashCode 方法是 Object 中的方法，所有的类都可以对其进行使用，首先底层通过调用 hashCode 方法生成初始 hash 值 h1，然后将 h1 无符号右移 16 位得到 h2，之后将 h1 与 h2 进行按位异或（^）运算得到最终 hash 值 h3，之后将 h3 与 (length-1) 进行按位与（&）运算得到 hash 表索引



其他可以计算出 hash 值的算法有

- 平方取中法
- 取余数
- 伪随机数法



**7. 当两个对象的 hashCode 相等时会怎样**



hashCode 相等产生 hash 碰撞，hashCode 相等会调用 equals 方法比较内容是否相等，内容如果相等则会进行覆盖，内容如果不等则会连接到链表后方，链表长度超过 8 且数组长度超过 64，会转变成红黑树节点



**8. 何时发生哈希碰撞和什么是哈希碰撞，如何解决哈希碰撞****？**



只要两个元素的 key 计算的 hash 码值相同就会发生 hash 碰撞，jdk8 之前使用链表解决哈希碰撞，jdk8 之后使用链表 + 红黑树解决哈希碰撞



**9. HashMap 的 put 方法流程**



以 JDK 8 为例，简要流程如下：

- 首先根据 key 的值计算 hash 值，找到该元素在数组中存储的下标
- 如果数组是空的，则调用 resize 进行初始化；
- 如果没有哈希冲突直接放在对应的数组下标里
- 如果冲突了，且 key 已经存在，就覆盖掉 value
- 如果冲突后是链表结构，就判断该链表是否大于 8，如果大于 8 并且数组容量小于 64，就进行扩容；如果链表节点数量大于 8 并且数组的容量大于 64，则将这个结构转换成红黑树；否则，链表插入键值对，若 key 存在，就覆盖掉 value
- 如果冲突后，发现该节点是红黑树，就将这个节点挂在树上



![img](https://cdn.nlark.com/yuque/0/2022/png/602399/1648726849290-550db691-3885-48b2-8a5b-2ed24d879b48.png)

**10. HashMap 的扩容方式**

HashMap 在容量超过负载因子所定义的容量之后，就会扩容。Java 里的数组是无法自己扩容的，将 HashMap 的大小扩大为原来数组的两倍

我们来看 JDK 1.8 扩容的源码

```java
final Node<K,V>[] resize() {
        //oldTab：引用扩容前的哈希表
        Node<K,V>[] oldTab = table;
        //oldCap：表示扩容前的 table 数组的长度
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        //获得旧哈希表的扩容阈值
        int oldThr = threshold;
        //newCap:扩容之后 table 数组大小
        //newThr:扩容之后下次触发扩容的条件
        int newCap, newThr = 0;
        //条件成立说明 hashMap 中的散列表已经初始化过了，是一次正常扩容
        if (oldCap > 0) {
            //判断旧的容量是否大于等于最大容量，如果是，则无法扩容，并且设置扩容条件为 int 最大值，
            //这种情况属于非常少数的情况
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }//设置 newCap 新容量为 oldCap 旧容量的二倍（<<1）,并且<最大容量，而且>=16，则新阈值等于旧阈值的两倍
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
        }
        //如果 oldCap=0 并且边界值大于 0，说明散列表是 null，但此时 oldThr>0
        //说明此时 hashMap 的创建是通过指定的构造方法创建的，新容量直接等于阈值
        //1.new HashMap(intitCap,loadFactor)
        //2.new HashMap(initCap)
        //3.new HashMap(map)
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;
        //这种情况下 oldThr=0;oldCap=0，说明没经过初始化，创建 hashMap
        //的时候是通过 new HashMap() 的方式创建的
        else {               // zero initial threshold signifies using defaults
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        //newThr 为 0 时，通过 newCap 和 loadFactor 计算出一个 newThr
        if (newThr == 0) {
            //容量*0.75
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
                //根据上面计算出的结果创建一个更长更大的数组
            Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        //将 table 指向新创建的数组
        table = newTab;
        //本次扩容之前 table 不为 null
        if (oldTab != null) {
            //对数组中的元素进行遍历
            for (int j = 0; j < oldCap; ++j) {
                //设置 e 为当前 node 节点
                Node<K,V> e;
                //当前桶位数据不为空，但不能知道里面是单个元素，还是链表或红黑树，
                //e = oldTab[j]，先用 e 记录下当前元素
                if ((e = oldTab[j]) != null) {
                    //将老数组 j 桶位置为空，方便回收
                    oldTab[j] = null;
                    //如果 e 节点不存在下一个节点，说明 e 是单个元素，则直接放置在新数组的桶位
                    if (e.next == null)
                        newTab[e.hash & (newCap - 1)] = e;
                    //如果 e 是树节点，证明该节点处于红黑树中
                    else if (e instanceof TreeNode)
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    //e 为链表节点，则对链表进行遍历
                    else { // preserve order
                        //低位链表：存放在扩容之后的数组的下标位置，与当前数组下标位置一致
                        //loHead：低位链表头节点
                        //loTail 低位链表尾节点
                        Node<K,V> loHead = null, loTail = null;
                        //高位链表，存放扩容之后的数组的下标位置，=原索引 + 扩容之前数组容量
                        //hiHead:高位链表头节点
                        //hiTail:高位链表尾节点
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                        do {
                            next = e.next;
                            //oldCap 为 16:10000，与 e.hsah 做&运算可以得到高位为 1 还是 0
                            //高位为 0，放在低位链表
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    //loHead 指向 e
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            }
                            //高位为 1，放在高位链表
                            else {
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
                                hiTail = e;
                            }
                        } while ((e = next) != null);
                        //低位链表已成，将头节点 loHead 指向在原位
                        if (loTail != null) {
                            loTail.next = null;
                            newTab[j] = loHead;
                        }
                        //高位链表已成，将头节点指向新索引
                        if (hiTail != null) {
                            hiTail.next = null;
                            newTab[j + oldCap] = hiHead;
                        }
                    }
                }
            }
        }
        return newTab;
    }
```



扩容之后原位置的节点只有两种调整

- 保持原位置不动（新 bit 位为 0 时）
- 散列原索引 + 扩容大小的位置去（新 bit 位为 1 时）



扩容之后元素的散列设置的非常巧妙，节省了计算 hash 值的时间，我们来看一 下具体的实现

![img](https://cdn.nlark.com/yuque/0/2022/png/602399/1648726849468-aec6bf03-9f40-484c-8099-e420956831ca.png)



当数组长度从 16 到 32，其实只是多了一个 bit 位的运算，我们只需要在意那个多出来的 bit 为是 0 还是 1，是 0 的话索引不变，是 1 的话索引变为当前索引值 + 扩容的长度，比如 5 变成 5+16=21

![img](https://cdn.nlark.com/yuque/0/2022/png/602399/1648726849317-e51bfcaa-7611-4ceb-a530-894328f26608.png)



这样的扩容方式不仅节省了重新计算 hash 的时间，而且保证了当前桶中的元素总数一定小于等于原来桶中的元素数量，避免了更严重的 hash 冲突，均匀的把之前冲突的节点分散到新的桶中去



**11. 一般用什么作为 HashMap 的 key？**

一般用 Integer、String 这种不可变类当 HashMap 当 key

- 因为 String 是不可变的，当创建字符串时，它的 hashcode 被缓存下来，不需要再次计算，相对于其他对象更快
- 因为获取对象的时候要用到 equals() 和 hashCode() 方法，那么键对象正确的重写这两个方法是非常重要的，这些类很规范的重写了 hashCode() 以及 equals() 方法



**12. 为什么 Map 桶中节点个数超过 8 才转为红黑树？**



8 作为阈值作为 HashMap 的成员变量，在源码的注释中并没有说明阈值为什么是 8

树节点占用空间是普通 Node 的两倍，如果链表节点不够多却转换成红黑树，无疑会耗费大量的空间资源，并且在随机 hash 算法下的所有 bin 节点分布频率遵从泊松分布，链表长度达到 8 的概率只有 0.00000006，几乎是不可能事件，所以 8 的计算是经过重重科学考量的

- 从平均查找长度来看，红黑树的平均查找长度是 logn，如果长度为 8，则 logn=3，而链表的平均查找长度为 n/4，长度为 8 时，n/2=4，所以阈值 8 能大大提高搜索速度
- 当长度为 6 时红黑树退化为链表是因为 logn=log6 约等于 2.6，而 n/2=6/2=3，两者相差不大，而红黑树节点占用更多的内存空间，所以此时转换最为友好



**13. HashMap 为什么线程不安全？**

- 多线程下扩容死循环。JDK1.7 中的 HashMap 使用头插法插入元素，在多线程的环境下，扩容的时候有可能导致环形链表的出现，形成死循环。因此 JDK1.8 使用尾插法插入元素，在扩容时会保持链表元素原本的顺序，不会出现环形链表的问题
- 多线程的 put 可能导致元素的丢失。多线程同时执行 put 操作，如果计算出来的索引位置是相同的，那会造成前一个 key 被后一个 key 覆盖，从而导致元素的丢失。此问题在 JDK1.7 和 JDK1.8 中都存在
- put 和 get 并发时，可能导致 get 为 null。线程 1 执行 put 时，因为元素个数超出 threshold 而导致 rehash，线程 2 此时执行 get，有可能导致这个问题，此问题在 JDK1.7 和 JDK1.8 中都存在



**14. 计算 hash 值时为什么要让低 16bit 和高 16bit 进行异或处理**

- 我们计算索引需要将 hashCode 值与 length-1 进行按位与运算，如果数组长度很小，比如 16，这样的值和 hashCode 做异或实际上只有 hashCode 值的后 4 位在进行运算，hash 值是一个随机值，而如果产生的 hashCode 值高位变化很大，而低位变化很小，那么有很大概率造成哈希冲突，所以我们为了使元素更好的散列，将 hash 值的高位也利用起来



举个例子

如果我们不对 hashCode 进行按位异或，直接将 hash 和 length-1 进行按位与运算就有可能出现以下的情况

![img](https://cdn.nlark.com/yuque/0/2022/png/602399/1648726849848-fbd96c77-1c54-4014-a346-81cde6f36f2d.png)

如果下一次生成的 hashCode 值高位起伏很大，而低位几乎没有变化时，高位无法参与运算

![img](https://cdn.nlark.com/yuque/0/2022/png/602399/1648726849860-471049d3-dee0-4723-ad61-f272b8badaf7.png)

可以看到，两次计算出的 hash 相等，产生了 hash 冲突

所以无符号右移 16 位的目的是使高混乱度地区与地混乱度地区做一个中和，提高低位的随机性，减少哈希冲突

