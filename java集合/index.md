# Java集合


### 1. 从Java中的数组谈起
#### 1.1 使用数组
数组：是用于储存多个相同类型数据的集合。
定义一个数组类型的变量，使用数组类型`“类型[]”`，例如，`int[]`。和单个基本类型变量不同，数组变量初始化必须使用`new int[5]`表示创建一个可容纳5个int元素的数组。
```Java
// 定义长度为5的整型数组
int[] array = new int[5];
```
也可以在定义数组时直接指定初始化的元素，这样就不必写出数组大小，而是由编译器自动推算数组大小。例如：
```Java
int[] array = new int[]{1,2,3,4,5};
// 或
int[] array = {1, 2, 3, 4, 5};
```   
要访问数组中的某一个元素，需要使用索引。数组索引从0开始，例如，5个元素的数组，索引范围是0~4。
可以修改数组中的某一个元素，使用赋值语句，例如，`array[1] = 79;。`
可以用数组变量`.length`获取数组大小：
```Java
// 定义长度为5的整型数组
int[] array = new int[5];
System.out.println(array.length);
```
数组是引用类型，在使用索引访问数组元素时，如果索引超出范围，运行时将报错：`java.lang.ArrayIndexOutOfBoundsException`
```Java
// 定义长度为5的整型数组
int[] array = new int[5];
System.out.println(array[5]);
```    
__Java中的数组有几个特点：__
* 数组中的元素是有序的
* 数组所有元素初始化为默认值，整型都是0，浮点型是0.0，布尔型是false；
* 数组一旦创建后，类型不可改变，大小不可改变；
* 数组元素可以是值类型（如int）或引用类型（如String），但数组本身是引用类型。   
#### 1.2 处理数组
##### 遍历
数组的元素类型和数组的大小都是确定的，所以当处理数组元素时候，我们通常使用基本循环或者 For-Each 循环。
```Java
int[] array = {1, 2, 3, 4, 5};
for (int i = 0; i < array.length; i++) {
    System.out.println(array[i]);
}
```
```Java
int[] array = {1, 2, 3, 4, 5};
for (int i : array) {
    System.out.println(i);
}
```
##### 排序
冒泡排序：
```Java
public void arrayTest() {
    int[] ns = { 28, 12, 89, 73, 65, 18, 96, 50, 8, 36 };
    // 排序前:
    System.out.println(Arrays.toString(ns));
    for (int i = 0; i < ns.length - 1; i++) {
        for (int j = 0; j < ns.length - i - 1; j++) {
            if (ns[j] > ns[j+1]) {
                // 交换ns[j]和ns[j+1]:
                int tmp = ns[j];
                ns[j] = ns[j+1];
                ns[j+1] = tmp;
            }
        }
    }
    // 排序后:
    System.out.println(Arrays.toString(ns));
}
```
##### Arrays工具类
Arrays为Java中内置的专门用于处理数组的工具类，位置`java.util.Arrays`。
Arrays常用方法：
| 方法 | 作用 | 参数 | 返回值 |
| --- | --- | --- | --- |
| toString | 返回指定数组内容的字符串表示形式 | `int[] a` | `String` |
| binarySearch | 使用二分法搜索指定数组的指定值 | `int[] a, int b` | `index，查询不到返回 -1` |
| sort | 按升序对指定数组进行排序 | `int[] a` | `排序后的数组` |
| equals | 判断两个数组是否相等 | `int[] a, int[] b` | `boolean` |

__数组再存储数组方面的弊端：__
* 数组初始化以后，长度就不可以改变了，不便于扩展；
* 数组中提供的属性和方法少，不便于进行添加、删除、插入等操作，且效率不高，同时无法获取存储元素的个数；
* 数组存储的数据是有序的、可重复的；--->存储数据的特点单一
* 数组只能按索引顺序存取。
---
### 2. Java中的集合
#### 2.1 概览
![集合框架图](../../static/images/basic-markdown-syntax/Collection01.gif)
![集合框架简化图](../../static/images/basic-markdown-syntax/Collection02.jpg)   

从上面的图中可以看到，Java集合框架主要包括两种类型的容器，一种是集合（Collection），存储一个元素集合；另一个是图（Map)，存储键/值对映射。Collection接口又有三种子类型：List、Set和Queue，再下面是一些抽象类，最后是具体实现类，常用的有ArrayList、LinkedList、HashSet、LinkedHashSet、TreeSet、HashMap、LinkedHashMap、TreeMap等等。     

集合框架是一个用来代表和操纵集合的统一架构。所有的集合框架都包含如下内容：
* 接口：是代表集合的抽象数据类型。例如 Collection、List、Set、Map 等。之所以定义多个接口，是为了以不同的方式操作集合对象
* 实现（类）：是集合接口的具体实现。从本质上讲，它们是可重复使用的数据结构，例如：ArrayList、LinkedList、HashSet、HashMap。
* 算法：是实现集合接口的对象里的方法执行的一些有用的计算，例如：搜索和排序。这些算法被称为多态，那是因为相同的方法可以在相似的接口上有着不同的实现。   

除了集合，该框架也定义了几个 Map 接口和类。Map 里存储的是键/值对。尽管 Map 不是集合，但是它们完全整合在集合中。

> _**所有集合类都位于java.util包下，支持泛型，Java集合使用统一的Iterator遍历，尽量不要使用遗留接口。**_

Java的java.util包主要提供了以下三种类型的集合（Collection和Map两种体系）：
* Collection接口：单列数据，定义了一组存取对象的方法的集合
  * List: 元素有序、可重复的集合 ---> 动态数组
  * Set: 元素无序、不可重复的集合 ---> 数学中的集合
* Map接口: 双列数据，保存具有映射关系的“key-value”的集合 ---> 函数：y = f(x)

#### 2.2 Collection接口
* Collection接口是List、Set、和Queue接口的父接口，该接口定义的方法既可用于操作Set集合，也可用于操作List和Queue集合。
* JDK不提供此接口的任何直接实现，而是提供更具体的子接口（如：Set和List）实现。
* 在Java5之前，Java集合会丢失容器中所有对象的数据类型，把所有对象都当作Object类型处理；从JDK5.0增加了__泛型__以后，Java集合可以记住容器中对象的数据类型。
##### Collection接口方法
1. 添加  
    * `add(Object obj)`
    * `addAll(Collection coll) `
2. 获取有效元素的个数
    * `int size()`
3. 清空集合
    * `void clear()`
4. 是否清空集合
    * `boolean isEmpty()`
5. 是否包含某个元素
    * `boolean contains(Object obj)`:通过元素的equals方法来判断是否是同一个对象
    * `boolean containsAll(Collection c)`:也是调用元素的equals方法来比较的。拿两个集合的元素挨个比较。
6. 删除
    * `boolean remove(Object obj)`:通过元素的equals方法判断是否是 要删除的那个元素。只会删除找到的第一个元素
    * `boolean removeAll(Collection coll)`:取当前集合的差集。
7. 取两个集合的交集
    *  `boolean retainAll(Collection c)`：把交集的结果存在当前集合中，不 影响c 
8. 集合是否相等
    * `boolean equals(Object obj)`
9. 转成对象数组
    * `Object[] toArray()`
10. 获取集合对象的哈希值
    * `hashCode()` 
11. 遍历
    * `iterator()`：返回迭代器对象，用于集合遍历

#### 2.3 Iterable接口
Collection接口实现了Iterable接口，该接口有一个iterator方法，返回一个Iterator对象。
Iterator是一种抽象的数据访问模型。使用Iterator模式进行迭代的好处有：
* 对任何集合都采用同一种访问模型；
* 调用者对集合内部结构一无所知；
* 集合类返回的Iterator对象知道如何迭代。   

Java提供了标准的迭代器模型，即集合类实现java.util.Iterable接口，返回java.util.Iterator实例。
```Java
        Collection collection = new ArrayList();
        collection.add(1);
        collection.add(2);
        collection.add(2);
        collection.add(3);
        // 使用迭代器遍历集合
        Iterator itr = collection.iterator();
        while(itr.hasNext()){
            System.out.println(itr.next());
        }
```

#### 2.4 Collection子接口之一：List接口
在集合类中，List是最基础的一种集合：它是一种有序列表。
List的行为和数组几乎完全相同：List内部按照放入元素的先后顺序存放，每个元素都可以通过索引确定自己的位置，List的索引和数组一样，从0开始。鉴于Java中数组用来存储数据的局限性，我们通常使用List替代数组。JDK API中List接口的实现类常用的有：ArrayList、LinkedList和Vector。
List可以和Array相互转换。
##### 2.4.1 List实现类之一：ArrayList
* 本质上，ArrayList是对象引用的一个”变长”数组 
* ArrayList的JDK1.8之前与之后的实现区别？ 
    *  JDK1.7：ArrayList像饿汉式，直接创建一个初始容量为10的数组 
    *  JDK1.8：ArrayList像懒汉式，一开始创建一个长度为0的数组，当添加第一个元 素时再创建一个始容量为10的数组

在实际应用中，需要增删元素的有序列表，我们使用最多的是ArrayList。实际上，ArrayList在内部使用了数组来存储所有元素。ArrayList把添加和删除的操作封装起来，让我们操作List类似于操作数组，却不用关心内部元素如何移动。通常情况下，我们总是优先使用ArrayList。
##### 2.4.2 List实现类之二：LinkedList
双向链表：对于频繁的插入或删除元素的操作，建议使用LinkedList类，效率较高。
内部没有声明数组，而是定义了Node类型的first和last， 用于记录首末元素。同时，定义内部类Node，作为LinkedList中保存数据的基 本结构。Node除了保存数据，还定义了两个变量： 
*  prev变量记录前一个元素的位置 
*  next变量记录下一个元素的位置

##### 2.4.3 List实现类之三：Vector
* Vector 是一个古老的集合，JDK1.0就有了。大多数操作与ArrayList 相同，区别之处在于Vector是线程安全的。 
* 在各种list中，最好把ArrayList作为缺省选择。当插入、删除频繁时， 使用LinkedList；Vector总是比ArrayList慢，所以尽量避免使用。 
> 请问ArrayList/LinkedList/Vector的异同？谈谈你的理解？ArrayList底层 是什么？扩容机制？Vector和ArrayList的最大区别?
> * ArrayList和LinkedList的异同
  二者都线程不安全，相对线程安全的Vector，执行效率高。 此外，ArrayList是实现了基于动态数组的数据结构，LinkedList基于链表的数据结构。对于 随机访问get和set，ArrayList觉得优于LinkedList，因为LinkedList要移动指针。对于新增 和删除操作add(特指插入)和remove，LinkedList比较占优势，因为ArrayList要移动数据。
> * ArrayList和Vector的区别
Vector和ArrayList几乎是完全相同的,唯一的区别在于Vector是同步类(synchronized)，属于 强同步类。因此开销就比ArrayList要大，访问要慢。正常情况下,大多数的Java程序员使用 ArrayList而不是Vector,因为同步完全可以由程序员自己来控制。Vector每次扩容请求其大 小的2倍空间，而ArrayList是1.5倍。Vector还有一个子类Stack。

#### 2.5 Collection子接口之二：
* Set接口是Collection的子接口，set接口没有提供额外的方法
* Set接口 Set 集合不允许包含相同的元素，如果试把两个相同的元素加入同一个 Set 集合中，则添加操作失败。
* Set 判断两个对象是否相同不是使用 == 运算符，而是根据 equals() 方法
##### 2.5.1 Set实现类之一：HashSet
* HashSet 是 Set 接口的典型实现，大多数时候使用 Set 集合时都使用这个实现类。 
* HashSet 按 Hash 算法来存储集合中的元素，因此具有很好的存取、查找、删除 性能。 
HashSet 具有以下特点： 
* 不能保证元素的排列顺序 
* HashSet 不是线程安全的 
* 集合元素可以是 null 
* HashSet 集合判断两个元素相等的标准：两个对象通过 hashCode() 方法比较相 等，并且两个对象的 equals() 方法返回值也相等。 
* __对于存放在Set容器中的对象，对应的类一定要重写equals()和hashCode(Object obj)方法，以实现对象相等规则。即：“相等的对象必须具有相等的散列码”。__

##### 2.5.2 Set实现类之一：LinkedHashSet
* LinkedHashSet 是 HashSet 的子类 
* LinkedHashSet 根据元素的 hashCode 值来决定元素的存储位置， 但它同时使用双向链表维护元素的次序，这使得元素看起来是以插入 顺序保存的。 
* LinkedHashSet插入性能略低于 HashSet，但在迭代访问 Set 里的全 部元素时有很好的性能。 
* LinkedHashSet 不允许集合元素重复。

##### 2.5.3 Set实现类之一：TreeSet
* TreeSet 是 SortedSet 接口的实现类，TreeSet 可以确保集合元素处于排序状态。 
* TreeSet底层使用红黑树结构存储数据 
* TreeSet 两种排序方法：自然排序和定制排序。默认情况下，TreeSet 采用自然排序。

#### 2.6 Map接口
* Map与Collection并列存在。用于保存具有映射关系的数据:key-value 
*  Map 中的 key 和 value 都可以是任何引用类型的数据 
*  __Map 中的 key 用Set来存放，不允许重复，即同一个 Map 对象所对应 的类，须重写hashCode()和equals()方法__
*  常用String类作为Map的“键”
*  key 和 value 之间存在单向一对一关系，即通过指定的 key 总能找到 唯一的、确定的 value 
*  Map接口的常用实现类：HashMap、TreeMap、LinkedHashMap和 Properties。其中，HashMap是 Map 接口使用频率最高的实现类
##### 2.6.1 Map实现类之一：HashMap
* HashMap是 Map 接口使用频率最高的实现类。 
* 允许使用null键和null值，与HashSet一样，不保证映射的顺序。 
* 所有的key构成的集合是Set:无序的、不可重复的。所以，key所在的类要重写： equals()和hashCode() 
* 所有的value构成的集合是Collection:无序的、可以重复的。所以，value所在的类 要重写：equals() 
* 一个key-value构成一个entry 
* 所有的entry构成的集合是Set:无序的、不可重复的 
* HashMap 判断两个 key 相等的标准是：两个 key 通过 equals() 方法返回 true， hashCode 值也相等。 
* HashMap 判断两个 value相等的标准是：两个 value 通过 equals() 方法返回 true    

> JDK 7及以前版本：HashMap是数组+链表结构(即为链地址法) 
JDK 8版本发布以后：HashMap是数组+链表+红黑树实现    


##### 2.6.2 Map实现类之二：LinkedHashMap
* LinkedHashMap 是 HashMap 的子类
* 在HashMap存储结构的基础上，使用了一对双向链表来记录添加
元素的顺序
* 与LinkedHashSet类似，LinkedHashMap 可以维护 Map 的迭代 顺序：迭代顺序与 Key-Value 对的插入顺序一
##### 2.6.3 Map实现类之三：TreeMap 
* *TreeMap存储 Key-Value 对时，需要根据 key-value 对进行排序。 TreeMap 可以保证所有的 Key-Value 对处于有序状态。 
* TreeSet底层使用红黑树结构存储数据 
* TreeMap 的 Key 的排序： 
    * 自然排序：TreeMap 的所有的 Key 必须实现 Comparable 接口，而且所有 的 Key 应该是同一个类的对象，否则将会抛出 ClasssCastException 
    * 定制排序：创建 TreeMap 时，传入一个 Comparator 对象，该对象负责对 TreeMap 中的所有 key 进行排序。此时不需要 Map 的 Key 实现 Comparable 接口 
* TreeMap判断两个key相等的标准：两个key通过compareTo()方法或 者compare()方法返回0
##### 2.6.4 Map实现类之四：Hashtable
* Hashtable是个古老的 Map 实现类，JDK1.0就提供了。不同于HashMap， Hashtable是线程安全的。 
* Hashtable实现原理和HashMap相同，功能相同。底层都使用哈希表结构，查询 速度快，很多情况下可以互用。 
* 与HashMap不同，Hashtable 不允许使用 null 作为 key 和 value 
* 与HashMap一样，Hashtable 也不能保证其中 Key-Value 对的顺序 
* Hashtable判断两个key相等、两个value相等的标准，与HashMap一致
##### 2.6.5 Map实现类之五：Properties
* Properties 类是 Hashtable 的子类，该对象用于处理属性文件 
* 由于属性文件里的 key、value 都是字符串类型，所以 Properties 里的 key 和 value 都是字符串类型 
* 存取数据时，建议使用setProperty(String key,String value)方法和 getProperty(String key)方法
####  2.7 Collections工具类
* Collections 是一个操作 Set、List 和 Map 等集合的工具类 
* Collections 中提供了一系列静态的方法对集合元素进行排序、查询和修改等操作， 还提供了对集合对象设置不可变、对集合对象实现同步控制等方法 

