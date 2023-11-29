# java集合

> 2023.11.5

* 集合

1. 可以动态的保存任意多个对象
2. 提供一系列的操作对象方法
3. 使用集合添加，删除新元素，简洁

## 集合的框架体系图

![image-20231105112651586](https://boimage.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20231105112651586.png)

![image-20231105112717885](https://boimage.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20231105112717885.png)

> 1. Iterator 对象称为迭代器，主要用于遍历 Collection 集合中的元素
> 2. 所有实现了 Collection接口的的集合类都有一个 iterator() 方法， 用以返回一个实现了 Iterator 接口的对象，既可以返回一个迭代器
> 3. Iterator 结构图
> 4. Iterator 仅用于遍历集合，Iterator 本身并不存放对象

* 迭代器的原理

```java
Iterator iterator = coll.iterator(); // 得到一个集合迭代器
// hasNext() : 判断是否有下一个元素
while(iterator.hasNext()){
    // next() 作用：下移 2：将下移以后集合位置上的元素返回
    System.out.println(iterator.next());
}

/*
	在调用 iterator.next() 的时候必须先调用 iterator.hasNext() 进行检测，若不调用，
	且下一条记录无效，直接调用 it.next() 会抛出 NoSuchElementException 异常
*/

// 显示所有的快捷键的快捷键  ctrl + j
```

# List

## List接口和常用方法

List接口是 Collection 接口的子类

1. List集合中的元素有序（即添加顺序和取出顺序一致），且可以重复
2. 每个元素都有其对应的顺序索引，即支持索引
3. List容器中的元素都对应一个整型的序号记载其在容器中的位置，可以根据序号取出容器中的元素
4. JDK API 中 List 接口的实现类  ArrayList  LinkedList  Vector

## ArrayList 底层结构源码分析

1. ArrayList 中 维护了一个 Object 类型的数组 elementData [debug 看源码]
2. 创建 ArrayList  对象时，如果使用的是无参构造器，则初始elemantData 容量 为 0 ，第一次参加则扩容  elementData 为 10， 如需要再次扩容，则扩容 elementData 为 1.5 倍
3. 如果使用的是指定大小的构造器，则初始 elementData 的容量为指定大小，如果需要扩容，则直接扩容 elementData 的大小为 1.5 倍

## LinkedList 的底层结构源码分析

1. LinkedList 底层实现了双向链表和双端队列的特点
2. 可以添加任意元素（元素可以重复），包括 null
3. 线程不安全，没有实现同步

* LinkedList的底层操作机制
  1. LinkedList 底层维护了了一个双向链表
  2.  LinkedList 中维护了两个属性 first 和 last 分别指向首节点和尾结点
  3. 每个节点 （Node对象）里面维护了 prev next item  三个属性，其中通过prev指向前一个，通过 next 指向后一个，最终实现了双向链表
  4. 所以LinkedList 的元素的添加和删除，不是通过数组完成的，相对来说效率较高
  5. 模拟一个简单的双向链表 （走代码） 

## ArrayList 和 LinkedList  的比较

|            | 底层结构 | 增删的效率           | 改查的效率 |
| ---------- | -------- | -------------------- | ---------- |
| ArrayList  | 可变数组 | 较低   数组扩容      | 较高       |
| LinkedList | 双向链表 | 较高，通过链表的追加 | 较低       |

* 如何选择 ArrayList 和 LinkedList:
  1. 如果我们改查的操作多 选择 ArrayList
  2. 如果我们增删的操作多 选择 LinkedList
  3. 一般来说，在程序中 80% - 90% 都是查询，因此大部分情况下选择 ArrayList
  4. 在一个项目中，根据业务灵活选择，也可能这样，一个模块使用的是 ArrayList,另一个模块使用的 LinkedList,也就是所 根据业务来选择

# Set

## set接口常用的方法

* Set接口的介绍

1. 无序（添加和取出的顺序不一致），没有索引
2. 不允许重复元素，所以最多包含一个 null
3. jdk API 中 set接口的实现类有 ![image-20231116141936283](https://boimage.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20231116141936283.png)

* Set 接口得常用方法

  跟List接口一样，Set也是collection 的 子接口 ，因此常用方法和 Collection 接口一样

* Set接口的遍历方式

​		同Collection的遍历方式一样，因为 Set 接口是 collection 的子接口

1. 可以使用迭代器
2. 增强 for
3. 不能使用索引的方式来获取

* HashSet底层机制说明

> 1. HashSet 底层是 HashMap
> 2. 添加一个元素时，先得到一个 hash 值 会转成 -> 索引值
> 3. 找到存储数据表table，看这个索引位置是否已经存放元素
> 4. 如果没有直接加入
> 5. 如果有调用 equals 比较，如果相同就放弃添加，如果不同则添加到最后
> 6. 在java中，如果有一条链表的元素个数达到 TREEIFY_THRESHOLD (默认是 8 )，并且table 的大小 >= MIN_TREEIFY_CAPACITY(默认值是 64)，就会进行树化 （红黑树）

> 1. 先获取元素的哈希值（hashCode 方法）
>
> 2. 对哈希值进行运算，得出一个索引值即为要存放在哈希表中的位置号
>
> 3. 如果该位置上没有其他元素，则直接存放
>
>    如果该位置上已经有其他元素，则需要进行 equals 判断，如果相等，则不在添加，如果不相等，则以链表的方式添加

## 分析 hashSet 扩容和转 红黑树 的机制

1. hashSet 底层是 HashMap ，第一添加时， table 数组扩容到 16，临界值（threshold） 是 16*加载因子（loadFactor）是 0.75 = 12
2. 如果 table 水族使用到了临界值 12，就会扩容到 16* 2 = 32. 新的临界值 就是 32*0.75 = 24  题词类推
3. 在 java8 中 如果一条链表的元素个数到达 TREEIFY_THRESHOLD(默认是 8)， 并且table 的大小  >= MIN_TRREEIFY_CAPACITY(默认是 64)， 就会进行树化（红黑树）， 否则仍然采用数组扩容机制

​	

## Set 接口实现子类 - LinkedHashSet

> 1. LinkedHashSet 是 HashSet 的子类
> 2. LinkedHashSet 底层是一个 LinkedHashMap， 底层维护了一个数组 + 双向链表
> 3. LinkedHashSet 根据元素的 hashCode 值来决定元素存储的位置，同时使用链表维护元素的次序（图），这使得元素看起来是以插入顺序保存的
> 4. LinkedHashSet 不允许重复添加元素

# Map接口

## Map接口和常用方法[很实用]

> 注意： 这里讲的是 JDK 8 的 Map  接口的特点

1. Map 与 Collection 并列存在，用于保存具有映射关系的数据： Key - Value
2. Map 中的 key 和 value 可以是引用数据类型， 会封装待 HashMap$Node 对象中
3. Map 中的 key 不允许重复， 原因和 HashSet 一样
4. Map 中的 Value 可以重复
5. Map 的 key 可以为 null， value  也就可以为 null，注意 key  为 null ，只能有一个，value 为 null 可以有多个
6. 常用的 String 类作为 Map 的 key
7. key 和 value 之间存在单向一对一的关系，即通过指定的 key 总能找到对应的 value
8. map 存放的数据结构 key - value示意图， 一堆k-v 放在 Node 中，有因为 Node 实现了 Entry 接口，有些书上说， 一对k-v就是一个 entry 【代码演示】

![image-20231122215332099](https://boimage.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20231122215332099.png)

```java
// 解读
// 1. k-v 最后是 HashMap$Node node = newNode(hash, key, value, null)
// 2. k-v 为了方便程序员的遍历，还会创建 EntrySet 集合， 该集合存放的元素类型是 Entry, 而一个 Entry 对象
//	  就有 k-v  EntrySet<Entry <k,v>> 即： transient Set<Map.Entry<k,v>> entrySet;
// 3. entrySet 中， 定义类型是 Map.Entry, 但实际上存放的还是 HashMap$Node
//     这是因为 HashMap$Node implements Map.Entry
// 4. 这样当把 HashMasp$Node 存放到  entrySet 之后就方便我们进行遍历
```

#### Map接口常用方法

1. put ： 添加
2. remove ： 根据键删除映射关系
3. get：根据键获取值
4. size: 获取元素个数
5. isEmpty: 判断个数是否为 0
6. clear : 清除
7. containsKey : 查找键是否存在

##### HashMap 小结

```java
// 1. Map 接口常用的实现类 ： HashMap， Hashtable ， Properties
// 2. HashMap 是Map 接口 使用频率最高的实现鳄梨
// 3. HashMap 是 以 key-val 对的方式来存储数据
// 4. key 不能重复，但是值可以重复允许使用空键和空值
// 5. 如果添加相同的 key ，则会覆盖原来的 key - val，等同于修改
// 6. 与HashSet 一样，不保证映射的顺序， 因为底层谁以 hash 标的方式来存储
// 7. HashMap 没有实现同步，因此线程不安全， 方法上没有做同步互斥的操作 没有 synchronized
```

### HashMap 的底层机制及源码分析

```java
// 1. HashMap 底层维护了Node 类型的数组table， 默认为 null
// 2. 当创建对象时， 将加载因子（loaddactor） 初始化为 0,75
// 3. 当创建 key - val 时， 通过key 的哈希值得到table 标的索引，然后判断该索引处是否有元素，如果没有元素直接添加，如果该索引处有元素，继续判断该元素的 key 是否和准备加入的key 相等，如果相等， 则直接替换 val 如果不相等需要判断是树结构还是链表结构，做出相应的处理，如果添加时容量不够，则需要进行扩容
// 4. 第一次添加，则需扩容 table 容量为 16， 临界值为 （threhold） 为 12
// 5. 以后如果在需要扩容，则需要扩容 table 容量为原来的 2 倍，临界值为原来的 2 被， 即24， 以此类推
// 6. 在java8 中，如果一条链表的元素个数超过 TREEIFY_THRESHOLD(默认是 8) 并且table 的大小 >= MIN_TREEIFY_CAPACITY(默认是 64)， 就会进行树化
```



### Map 接口的实现类 HashTable

* 基本介绍

1. 存放的是键值对： 即 k - v
2. hashtable 的键和值都不能为 null， 否则会抛出 NullPointException
3. hashTable  使用的方法和 HashMap 一样
4. hashTable 是线程安全的 （synchronized）， hashMap 是线程不安全的
5. 简单看下 HashTable 的底层结构

### Map 接口的实现类 Properties

1. Properties 类，继承自 Hashtable 类并实现了 Map 接口，也是一种键值对的形式保存数据
2. 他的使用特点和 Hashtable 类似
3. Properties 还可以用于 xxx.properties 文件中 ， 加载数据到 Properties 类对象并进行读取和修改
4. 说明： 工作后 XXX.properties 文件通常作为配置文件，这知识点在 IO 流 ，中举例 ，有兴趣可先看文章

## 总结

```java
// 开发中如何选择集合实现类
// 1. 先判断存储类型（一组独像（单列） 或  一组键值对（双列））
// 2. 一组对象（单列）： Collection 接口
//		允许重复： list
// 			增删多： LinkedList (底层维护了一个双向链表)
//			改查多： ArrayList 【底层维护 Object 类型的可变数组】
//		不允许重复： Set
//			无序： HashSet 【底层是 HashMap， 维护了一个哈希表 ， 即 （数组+链表 + 红黑树）】
//			排序： TreeSet
// 			插入和取出顺序一致： LinkedHashSet， 维护数据  + 双向链表
// 3. 一组键值对： Map
//		键无序： HashMap 【底层是： 哈希表， jdk7：数组+链表， jdk8： 数组+链表+红黑树】
//		键排序： TreeMap
//		键插入和取出顺序一致： LinkedHashMap
//		读取文件： Properries
```

## Collection 工具类

Collection 工具类介绍

1）Collection 是一个操作 Set List Map 等集合的工具类

2）Collection 中提供了一系列**静态**方法对集合元素进行排序，查询和修好的操作

1. reverse(List) : 反转 List 中元素的顺序
2. shuffle（List）：对 List 集合元素进行随机排序
3. sort（List）：根据元素的自然顺序 对指定的 List 集合按升序进行排序
4. sort(List, Comparator) : 根据指定的 Compaartor 产生的顺序对 List 集合元素进行排序
5. swap（List， int， int） ： 将指定 list 集合中 i 处元素和 j 出的元素进行交换

### HashSet 的去重机制

HashSet 的去重机制： hashCode() + equals(), 底层先通过存入对象， 进行运算得到一个 hash 值， 通过hash 值得到对应的索引，如果发现table 索引所在的位置没有数据， 就直接存放， 如果有数据，就进行equals 的比较[遍历比较] 如果比较后， 不相同就加入，否则就不假如

### TreeSet 去重机制

TreeSet 的去重机制： 如果传入了一个 Comparator 匿名对象，就用实现了 Compare 去重， 如果方法返回 0， 就认为 是相同的元素/数据，就不添加， 如果没有传入 Comparator 匿名对象，则就以你添加的对象去实现Compareable 接口的 compareTo去重
