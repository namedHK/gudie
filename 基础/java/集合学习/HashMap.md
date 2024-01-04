# HashMap源码分析

## 概念

### 散列表实现

![image-20221117211646229](/Users/kh/Library/Application Support/typora-user-images/image-20221117211646229.png)

```java
//序列化时不会对table序列化，会重写writeObject和readObject函数，减少存储开销
transient Node<K,V>[] table;
//链表
static class Node<K,V> implements Map.Entry<K,V> {
        final int hash;
        final K key;
        V value;
        Node<K,V> next;
}
```



### 扰动函数

低16位做异或运算，增加随机性

```java
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

### 初始化容量

```java
//默认16
DEFAULT_INITIAL_CAPACITY = 1<<<4
//map最大容量
static final int MAXIMUM_CAPACITY = 1 << 30;
```

如果传入 new HashMap(17) ,数组初始化大小为32，需要符合2的倍数，便于指定下标

```java
// 保证数组长度为2倍数，与运算时保证 n-1为 01111...，低位都为1,让下标有效填充
(n - 1) & hash
```

### 负载因子

```java
//当数组中存在75%下标有数据时，进行扩容，根据统计0.75时散列的使用率最高
DEFAULT_LOAD_FACTOR=0.75
```

### 元素扩容

扩容时，数组大小会翻倍。初始化时 使用 门槛 = 负载因子 * 门槛 ，后续 门槛 = 门槛 <<1

```java
final Node<K,V>[] resize() {
        Node<K,V>[] oldTab = table;
        int oldCap = (oldTab == null) ? 0 : oldTab.length;
        int oldThr = threshold;
        int newCap, newThr = 0;
        if (oldCap > 0) {
            if (oldCap >= MAXIMUM_CAPACITY) {
                threshold = Integer.MAX_VALUE;
                return oldTab;
            }
            else if ((newCap = oldCap << 1) < MAXIMUM_CAPACITY &&
                     oldCap >= DEFAULT_INITIAL_CAPACITY)
                newThr = oldThr << 1; // double threshold
        }
        else if (oldThr > 0) // initial capacity was placed in threshold
            newCap = oldThr;
        else {               // zero initial threshold signifies using defaults
            newCap = DEFAULT_INITIAL_CAPACITY;
            newThr = (int)(DEFAULT_LOAD_FACTOR * DEFAULT_INITIAL_CAPACITY);
        }
        if (newThr == 0) {
          //初始化时，门槛为负载因子*数组大小
            float ft = (float)newCap * loadFactor;
            newThr = (newCap < MAXIMUM_CAPACITY && ft < (float)MAXIMUM_CAPACITY ?
                      (int)ft : Integer.MAX_VALUE);
        }
        threshold = newThr;
        @SuppressWarnings({"rawtypes","unchecked"})
            Node<K,V>[] newTab = (Node<K,V>[])new Node[newCap];
        table = newTab;
        if (oldTab != null) {
            for (int j = 0; j < oldCap; ++j) {
                Node<K,V> e;
                if ((e = oldTab[j]) != null) {
                    oldTab[j] = null;
                    if (e.next == null)
                        newTab[e.hash & (newCap - 1)] = e;
                    else if (e instanceof TreeNode)
                        ((TreeNode<K,V>)e).split(this, newTab, j, oldCap);
                    else { // preserve order
                        Node<K,V> loHead = null, loTail = null;
                        Node<K,V> hiHead = null, hiTail = null;
                        Node<K,V> next;
                      //扩容后链表排列规则，hash&原数组大小 == 0 ? 位置不动 : hash|原数组大小 (偏移到原位置+扩容大小)
                        do {
                            next = e.next;
                            if ((e.hash & oldCap) == 0) {
                                if (loTail == null)
                                    loHead = e;
                                else
                                    loTail.next = e;
                                loTail = e;
                            }
                            else {
                                if (hiTail == null)
                                    hiHead = e;
                                else
                                    hiTail.next = e;
                                hiTail = e;
                            }
                        } while ((e = next) != null);
                        if (loTail != null) {
                            loTail.next = null;
                            newTab[j] = loHead;
                        }
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

### 链表转红黑树

需要满足链表长度>8 且 数组长度大于64(小于64优先扩容)

```java
   final V putVal(int hash, K key, V value, boolean onlyIfAbsent,
                   boolean evict) {
     .......
       //判断链表长度>8,进行树转换
 if (binCount >= TREEIFY_THRESHOLD - 1) // -1 for 1st
                            treeifyBin(tab, hash);
  

  final void treeifyBin(Node<K,V>[] tab, int hash) {
        int n, index; Node<K,V> e;
    //如果表长度
        if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY)
            resize();
```



- hashcode

> String 的 hashcode会 i*31 操作，31是因为jvm运行为2^5-1,位运算利于jvm计算 且 31碰撞的概率小性价比高

- 扰动函数，减少hash碰撞

```java
static final int hash(Object key) { int h;
return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16); }
```

![image-20221117100033273](/Users/kh/Library/Application Support/typora-user-images/image-20221117100033273.png)

- 初始化容量

hashmap需要定义初始化数组大小，数组为2的倍数，才可以除最高位都是1，来确定key在数组中的下标



- HasMap Java实现原理

  > 在JDK1.7中，hashmap使用数组和链表实现。jdk1.8中hashmap在链表元素超过8个后，转换成红黑树。
  >
  > hashmap的数据结构如下图：
  >
  > ![image-20200204104331914](/Users/kh/Library/Application Support/typora-user-images/image-20200204104331914.png)

- HashMap的主要成员变量

  ```java
   //定义了默认的数组初始化数组大小 ,2^4即16
   static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16
   //数组最大值不能超过2^30
   static final int MAXIMUM_CAPACITY = 1 << 30;
   /*默认的负载因子，当数组元素总数/数组大小 > 0.75时，数组会进行扩容，
    * 0.75因为经过大量测试，当超过0.75时，命中数组中空地址的可能性会降低很多
    */
   static final float DEFAULT_LOAD_FACTOR = 0.75f;
   //当链表长度大于等于8时，存储元素结构转换成红黑树，提高查找效率
   static final int TREEIFY_THRESHOLD = 8;
   //当链表长度小于等于6时，存储元素从红黑树转换回链表
   static final int UNTREEIFY_THRESHOLD = 6;
   //如果数组元素不超过64时，链表不直接树化，而是选择扩大数组
   static final int MIN_TREEIFY_CAPACITY = 64;
   //链表数组，存储hashmap数据的主要元素
   transient Node<K,V>[] table;
   //hashmap的键值对
    transient Set<Map.Entry<K,V>> entrySet;
    //实际元素个数
    transient int size;
    //实际修改次数，hashmap使用fast-fail机制，如果modcount和局部变量expectedModCount(迭代时)不一致
    //表示其他线程也在操作本对象，抛出ConcurrentModificationException。
    transient int modCount;
    //定义容器大小
    int threshold;
    //自定义负载因子
    final float loadFactor;
  
  ```

  