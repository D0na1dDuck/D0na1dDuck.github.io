# Java集合


### 1. 从Java中的数组谈起
#### 1.1 使用数组
数组：是用于储存多个相同类型数据的集合。
定义一个数组类型的变量，使用数组类型`“类型[]”`，例如，`int[]`。和单个基本类型变量不同，数组变量初始化必须使用`new int[5]`表示创建一个可容纳5个int元素的数组。
```
// 定义长度为5的整型数组
int[] array = new int[5];
```
也可以在定义数组时直接指定初始化的元素，这样就不必写出数组大小，而是由编译器自动推算数组大小。例如：
```
int[] array = new int[]{1,2,3,4,5};
// 或
int[] array = {1, 2, 3, 4, 5};
```   
要访问数组中的某一个元素，需要使用索引。数组索引从0开始，例如，5个元素的数组，索引范围是0~4。
可以修改数组中的某一个元素，使用赋值语句，例如，`array[1] = 79;。`
可以用数组变量`.length`获取数组大小：
```
// 定义长度为5的整型数组
int[] array = new int[5];
System.out.println(array.length);
```
数组是引用类型，在使用索引访问数组元素时，如果索引超出范围，运行时将报错：`java.lang.ArrayIndexOutOfBoundsException`
```
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
```
int[] array = {1, 2, 3, 4, 5};
for (int i = 0; i < array.length; i++) {
    System.out.println(array[i]);
}
```
```
int[] array = {1, 2, 3, 4, 5};
for (int i : array) {
    System.out.println(i);
}
```
##### 排序
冒泡排序：
```
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

#### 2.4 Collection子接口之一：List接口

##### 2.4.1 List实现类之一：ArrayList
##### 2.4.2 List实现类之二：LinkedList
##### 2.4.3 List实现类之三：Vector

#### 2.5 Collection子接口之二：Set接口

##### 2.5.1 Set实现类之一：HashSet
##### 2.5.2 Set实现类之一：LinkedHashSet
##### 2.5.3 Set实现类之一：TreeSet

#### 2.6 Map接口

##### 2.6.1 Map实现类之一：HashMap
##### 2.6.2 Map实现类之二：LinkedHashMap
##### 2.6.3 Map实现类之三：TreeMap 
##### 2.6.4 Map实现类之四：Hashtable
##### 2.6.5 Map实现类之五：Properties

####  2.7 Collections工具类

