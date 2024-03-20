# **Java 面试题整理**



## **基础篇**

### **1**、 **Java** 语言有哪些特点

1、简单易学、有丰富的类库

2、面向对象（ Java 最重要的特性，让程序耦合度更低，内聚性更高）

3、与平台无关性（ JVM 是 Java 跨平台使用的根本）

4、可靠安全

5、支持多线程



### **2**、面向对象和面向过程的区别

**面向过程**：是分析解决问题的步骤，然后用函数把这些步骤一步一步地实现，然后在使用的时候一一调用则可。性能较高，所以单片机、嵌入式开发等一般采用面向过程开发

**面向对象**：是把构成问题的事务分解成各个对象，而建立对象的目的也不是为了完成一个个步骤，而是为了描述某个事物在解决整个问题的过程中所发生的行为。面向对象有 **封装、继承、多态** 的特性，所以易维护、易复用、易扩展。可以设计出低耦合的系统。 但是性能上来说，比面向过程要低。



### **3** **、八种基本数据类型的大小，以及他们的封装类**

| **基本类型** | **大小（字节）** | **默认值**       | **封装类** |
| ------------ | ---------------- | ---------------- | ---------- |
| byte         | 1                | （ byte ）0      | Byte       |
| short        | 2                | （ short ）0     | Short      |
| int          | 4                | 0                | Integer    |
| long         | 8                | 0 L              | Long       |
| float        | 4                | 0.0 f            | Float      |
| double       | 8                | 0.0 d            | Double     |
| boolean      | -                | false            | Boolean    |
| char         | 2                | \u0000（ null ） | Character  |

注：

- int 是基本数据类型，Integer 是 int 的封装类，是引用类型。int 默认值是0，而 Integer 默认值是null，所以Integer能区分出0和null的情况。一旦java看到null，就知道这个引用还没有指向某个对象，再任何引用使用前，必须为其指定一个对象，否则会报错。

- 基本数据类型在声明时系统会自动给它分配空间，而引用类型声明时只是分配了引用空间，必须通过实例化开辟数据空间之后才可以赋值。数组对象也是一个引用对象，将一个数组赋值给另一个数组时只是复制了一个引用，所以通过某一个数组所做的修改在另一个数组中也看的见。

虽然定义了 boolean 这种数据类型，但是只对它提供了非常有限的支持。在 Java 虚拟机中没有任何供 boolean 值专用的字节码指令，Java 语言表达式所操作的 boolean 值，在编译之后都使用 Java 虚拟机中的 int 数据类型来代替，而 boolean 数组将会被编码成 Java 虚拟机的 byte 数组，每个元素 boolean 元素占 8 位。这样我们可以得出 boolean 类型占了单独使用是 4 个字节，在数组中又是 1 个字

节。使用 int 的原因是，对于当下 32 位的处理器（ CPU ）来说，一次处理数据是 32 位（这里不是指的是 32 / 64 位系统，而是指 CPU 硬件层面），具有高效存取的特点。



### **4**、标识符的命名规则

**标识符的含义：** 是指在程序中，我们自己定义的内容，譬如，类的名字，方法名称以及变量名称等等，都是标识符。

**命名规则：（硬性要求）** 标识符可以包含英文字母，0 - 9 的数字，$ 以及_ 标识符不能以数字开头 标识符不是关键字

**命名规范：（非硬性要求）** 类名规范：首字符大写，后面每个单词首字母大写（大驼峰式）。 变量名规范：首字母小写，后面每个单词首字母大写（小驼峰式）。 方法名规范：同变量名。



### **5**、instanceof **关键字的作用**

instanceof  严格来说是 Java 中的一个双目运算符，用来测试一个对象是否为一个类的实例，用法为：

```java
boolean result = obj instanceof Class
```

其中 obj 为一个对象，Class 表示一个类或者一个接口，当 obj 为 Class 的对象，或者是其直接或间接子类，或者是其接口的实现类，结果 result 都返回 true，否则返回 false 。

注意：编译器会检查 obj 是否能转换成右边的 class 类型，如果不能转换则直接报错，如果不能确定类型，则通过编译，具体看运行时定。

```java
int i = 0;
System.out.println(i instanceof Integer);//编译不通过 i 必须是引用类型，不能是基本类型
System.out.println(i instanceof Object);//编译不通过
```

```java
Integer integer = new Integer(1);
System.out.println(integer instanceof Integer);//true
```

```java
//false ,在 JavaSE 规范 中对 instanceof 运算符的规定就是：如果 obj 为 null，那么将返回 false。
System.out.println(null instanceof Object);
```



### **6**、Java 自动装箱与拆箱

**装箱就是自动将基本数据类型转换为包装器类型（ int --> Integer ），调用方法：Integer 的 valueOf (int) 方法**

**拆箱就是自动将包装器类型转换为基本数据类型（ Integer --> int），调用方法：Integer 的 intValue 方法**

在 Java SE5 之前，如果要生成一个数值为 10 的 Integer 对象，必须这样进行：

```java
Integer i = new Integer(10);
```

而在从 Java SE5 开始就提供了自动装箱的特性，如果要生成一个数值为 10 的 Integer 对象，只需要这样就可以了：

```java
Integer i = 10;
```

**面试题 1 ： 以下代码会输出什么？**

```java
public class Main {
 	public static void main(String[] args) {
 
 	Integer i1 = 100;
 	Integer i2 = 100;
	Integer i3 = 200;
 	Integer i4 = 200;
 
 	System.out.println(i1==i2);
 	System.out.println(i3==i4);
 	}
}
```

运行结果：

```java
true
false
```

为什么会出现这样的结果？输出结果表明 i1 和 i2 指向的是同一个对象，而 i3 和 i4 指向的是不同的对

象。此时只需一看源码便知究竟，下面这段代码是 Integer 的 valueOf 方法的具体实现：

```java
public static Integer valueOf(int i) {
 	if(i >= -128 && i <= IntegerCache.high)
 	return IntegerCache.cache[i + 128];
 	else
 	return new Integer(i);
 }
```

其中 IntegerCache 类的实现为：

```java
private static class IntegerCache {
 	static final int high;
 	static final Integer cache[];
	static {
    final int low = -128;
 	// high value may be configured by property
 	int h = 127;
 	if (integerCacheHighPropValue != null) {
 	// Use Long.decode here to avoid invoking methods that
 	// require Integer's autoboxing cache to be initialized
 	int i = Long.decode(integerCacheHighPropValue).intValue();
 	i = Math.max(i, 127);
 	// Maximum array size is Integer.MAX_VALUE
	h = Math.min(i, Integer.MAX_VALUE - -low);
 	}
 	high = h;
 	cache = new Integer[(high - low) + 1];
 	int j = low;
 	for(int k = 0; k < cache.length; k++)
 	cache[k] = new Integer(j++);
 	}
 	private IntegerCache() {}
 }
```

从这2段代码可以看出，在通过 valueOf 方法创建 Integer 对象的时候，如果数值在 [-128,127] 之间，便返回指向 IntegerCache.cache 中已经存在的对象的引用；否则创建一个新的 Integer 对象。

上面的代码中 i1 和 i2 的数值为 100 ，因此会直接从 cache 中取已经存在的对象，所以 i1 和 i2 指向的是同一个对象，而 i3 和 i4 则是分别指向不同的对象。

**面试题 2 ：以下代码输出什么运行结果：**

```java
public class Main {
 	public static void main(String[] args) {
 
 	Double i1 = 100.0;
 	Double i2 = 100.0;
 	Double i3 = 200.0;
 	Double i4 = 200.0;
 
 	System.out.println(i1==i2);
 	System.out.println(i3==i4);
 	}
}
```

运行结果：

```java
false
false
```

原因： 在某个范围内的整型数值的个数是有限的，而浮点数却不是。



### **7**、 重载和重写的区别

**重写 (Override)**

从字面上看，重写就是重新写一遍的意思。其实就是在子类中把父类本身有的方法重新写一遍。子类继承了父类原有的方法，但有时子类并不想原封不动的继承父类中的某个方法，所以在方法名，参数列表，返回类型(除过子类中方法的返回值是父类中方法返回值的子类时)都相同的情况下， 对方法体进行修改或重写，这就是重写。但要注意子类函数的访问修饰权限不能少于父类的。

```java
public class Father {
 	public static void main(String[] args) {
 	// TODO Auto-generated method stub
 	Son s = new Son();
 	s.sayHello();
 	}
 	public void sayHello() {
 	System.out.println("Hello");
 	}
}
class Son extends Father{
	@Override
 	public void sayHello() {
 	// TODO Auto-generated method stub
 	System.out.println("hello by ");
 	}
}
```

**重写 总结：** 1.发生在父类与子类之间 2.方法名，参数列表，返回类型（除过子类中方法的返回类型是父类中返回类型的子类）必须相同 3.访问修饰符的限制一定要大于被重写方法的访问修饰符（ public > protected > default > private ) 4.重写方法一定不能抛出新的检查异常或者比被重写方法申明更加宽泛的检查型异常

**重载（ Overload ）**

在一个类中，同名的方法如果有不同的参数列表（**参数类型不同、参数个数不同甚至是参数顺序不同**）则视为重载。同时，重载对返回类型没有要求，可以相同也可以不同，但**不能通过返回类型是否相同来判断重载**。

```java
public class Father {
 	public static void main(String[] args) {
 		// TODO Auto-generated method stub
 		Father s = new Father();
 		s.sayHello();
 		s.sayHello("wintershii");
 	}
 	public void sayHello() {
 		System.out.println("Hello");
 	}
 	public void sayHello(String name) {
 		System.out.println("Hello" + " " + name);
 	}
}
```

**重载 总结：** 1.重载 Overload 是一个类中多态性的一种表现 2.重载要求同名方法的参数列表不同(参数类型，参数个数甚至是参数顺序) 3.重载的时候，返回值类型可以相同也可以不相同。无法以返回型别作为重载函数的区分标准

### 8、equals 与 == 的区别

**==** **：**

== 比较的是变量(栈)内存中存放的对象的(堆)内存地址，用来判断两个对象的地址是否相同，即是否是指相同一个对象。比较的是真正意义上的指针操作。

1、比较的是操作符两端的操作数是否是同一个对象。 2、两边的操作数必须是同一类型的（可以是父子类之间）才能编译通过。 3、比较的是地址，如果是具体的阿拉伯数字的比较，值相等则为 true，如： int a = 10 与 long b = 10 L 与 double c = 10.0 都是相同的（为true），因为他们都指向地址为 10 的堆。

**equals**：

equals用来比较的是两个对象的内容是否相等，由于所有的类都是继承自 java.lang.Object 类的，所以适用于所有对象，如果没有对该方法进行覆盖的话，调用的仍然是 Object 类中的方法，而 Object 中的 equals 方法返回的却是 == 的判断。

总结：

所有比较是否相等时，都是用 equals 并且在对常量相比较时，把常量写在前面，因为使用 object 的 equals object 可能为 null 则空指针

在阿里的代码规范中只使用 equals，阿里插件默认会识别，并可以快速修改，推荐安装阿里插件来排查老代码使用 “==” ，替换成 equals



### **9、Hashcode 的作用**

java 的集合有两类，一类是 List ，还有一类是 Set 。前者有序可重复，后者无序不重复。当我们在 set 中插入的时候怎么判断是否已经存在该元素呢，可以通过 equals 方法。但是如果元素太多，用这样的方法就会比较满。

于是有人发明了哈希算法来提高集合中查找元素的效率。 这种方式将集合分成若干个存储区域，每个对象可以计算出一个哈希码，可以将哈希码分组，每组分别对应某个存储区域，根据一个对象的哈希码就可以确定该对象应该存储的那个区域。

hashCode 方法可以这样理解：它返回的就是根据对象的内存地址换算出的一个值。这样一来，当集合要添加新的元素时，先调用这个元素的 hashCode 方法，就一下子能定位到它应该放置的物理位置上。如果这个位置上没有元素，它就可以直接存储在这个位置上，不用再进行任何比较了；如果这个位置上已经有元素了，就调用它的 equals 方法与新元素进行比较，相同的话就不存了，不相同就散列其它的地址。这样一来实际调用 equals 方法的次数就大大降低了，几乎只需要一两次。



### 10、String、StringBuffer 和 StringBuilder 的区别是什么?

String 是只读字符串，它并不是基本数据类型，而是一个对象。从底层源码来看是一个 final 类型的字符数组，所引用的字符串不能被改变，一经定义，无法再增删改。每次对 String 的操作都会生成新的 String 对象。

```java
private final char value[];//dk1.8 及以前 String 底层使用的是 char 数组
private final byte[] value;//jdk 1.9 及以后使用的是 byte 数组
```

**jdk1.8 及以前 String 底层使用的是 char 数组，jdk 1.9 及以后使用的是 byte 数组。**

每次 + 操作 ： 隐式在堆上 new 了一个跟原字符串相同的 StringBuilder 对象，再调用 append 方法拼接 + 后面的字符。

StringBuffer 和 StringBuilder 他们两都继承了 AbstractStringBuilder 抽象类，从 AbstractStringBuilder 抽象类中我们可以看到

```java
/**
* 基于1.8
* The value is used for character storage.
*/
char[] value;
```

他们的底层都是可变的字符数组，所以在进行频繁的字符串操作时，建议使用 StringBuffer 和 StringBuilder 来进行操作。 另外StringBuffer 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。StringBuilder 并没有对方法进行加同步锁，所以是非线程安全的。



### 11、ArrayList 和 linkedList 的区别

**ArrayList**

- **优点**：ArrayList 是实现了基于动态数组的数据结构，因为地址连续，一旦数据存储好了，查询操作效率会比较高（在内存里是连着放的）。

- **缺点**：因为地址连续，ArrayList 要移动数据，所以插入和删除操作效率比较低。

**LinkedList**

- **优点**：LinkedList 基于链表的数据结构，地址是任意的，所以在开辟内存空间的时候不需要等一个连续的地址。对于新增和删除操作，LinkedList 比较占优势。LinkedList 适用于要头尾操作或插入指定位置的场景。

- **缺点**：因为 LinkedList 要移动指针，所以查询操作性能比较低。

**适用场景分析**

- 当需要对数据进行对随机访问的时候，选用 ArrayList。

- 当需要对数据进行多次增加删除修改时，采用 LinkedList。

如果容量固定，并且只会添加到尾部，不会引起扩容，优先采用 ArrayList。

当然，绝大数业务的场景下，使用 ArrayList 就够了，但需要注意避免 ArrayList 的扩容，以及非顺序的插入。



 **Array （数组）是基于索引（index）的数据结构，它使用索引在数组中搜索和读取数据是很快的。**

Array 获取数据的时间复杂度是 O ( 1 ) ,但是要删除数据却是开销很大，因为这需要重排数组中的所有数据（因为删除数据以后, 需要把后面所有的数据前移）

**缺点：**数组初始化必须指定初始化的长度, 否则报错

例如：

```java
int[] a = new int[4];//推介使用int[] 这种方式初始化
int c[] = {23,43,56,78};//长度：4，索引范围：[0,3]
```

**List 是一个有序的集合，可以包含重复的元素，提供了按索引访问的方式，它继承 Collection。**

**List有两个重要的实现类：ArrayList 和 LinkedList**

**ArrayList:** **可以看作是能够自动增长容量的数组**

**ArrayList 的 toArray 方法返回一个数组**

**ArrayList 的 asList 方法返回一个列表**

ArrayList底层的实现是 Array, 数组扩容实现

**LinkList 是一个双链表 ，在添加和删除元素时具有比 ArrayList 更好的性能；但在 get 与 set 方面弱于ArrayList。当然，这些对比都是指数据量很大或者操作很频繁。**



### 12、HashMap 和 HashTable 的区别

HashTable 是 java 一开始发布时就提供的键值映射的数据结构，而 HashMap 产生于 JDK 1.2。虽然 HashTable 比 HashMap 出现的早一些，**但是现在 HashTable 基本上已经被弃用了**。而 HashMap 已经成为应用最为广泛的一种数据类型了。

#### 1、两者父类不同

HashMap 是继承自 AbstractMap 类，而 HashTable 是继承自 Dictionary 类。不过它们同时实现了 map、Cloneable（可复制）、Serializable（可序列化）这三个接口。

#### 2、对外提供的接口不同

HashTable 比 HashMap 多提供了 elments() 和 contains() 两个方法。elments() 方法继承自 HashTable 的父类 Dictionnary。elements() 方法用于返回此 HashTable 中的 value 的枚举。

contains() 方法判断该 HashTable 是否包含传入的 value。它的作用与 containsValue() 一致。事实上，contansValue() 就只是调用了一下 contains() 方法。

#### 3、对 null 的支持不同

Hashtable：key 和 value 都不能为 null。

HashMap：key 可以为 null，但是这样的 key 只能有一个，因为必须保证 key 的唯一性；可以有多个 key 值对应的 value 为 null。

#### 4、安全性不同

HashMap 是线程不安全的，在多线程并发的环境下，可能会产生死锁等问题，因此需要开发人员自己处理多线程的安全问题。

Hashtable 是线程安全的，它的每个方法上都有 synchronized  关键字，因此可直接用于多线程中。

虽然 HashMap 是线程不安全的，但是它的效率远远高于 Hashtable，这样设计是合理的，因为大部分的使用场景都是单线程。当需要多线程操作的时候可以使用线程安全的 ConcurrentHashMap 。

ConcurrentHashMap 虽然也是线程安全的，但是它的效率比Hashtable要高好多倍。因为 ConcurrentHashMap 使用了分段锁，并不对整个数据进行锁定。



**第二种答案：**

1. 出生的版本不一样，Hashtable 出生于 Java 发布的第一版本 JDK 1.0，HashMap 出生于 JDK 1.2。

2. 都实现了 Map、Cloneable、Serializable（ 当前 JDK 版本 1.8 ）。
3. HashMap 继承的是 AbstractMap，并且 AbstractMap 也实现了 Map 接口。Hashtable 继承 Dictionary。

4. Hashtable 中大部分 public 修饰普通方法都是 synchronized 字段修饰的，是线程安全的，HashMap 是非线程安全的。

5. Hashtable 的 key 不能为 null，value 也不能为 null，这个可以从 Hashtable 源码中的 put 方法看到，判断如果 value 为 null 就直接抛出空指针异常，在 put 方法中计算 key 的 hash 值之前并没有判断 key 为 null 的情况，那说明，这时候如果 key 为空，照样会抛出空指针异常。

6. HashMap 的 key 和 value 都可以为 null。在计算 hash 值的时候，有判断，如果 key==null ，则其 hash=0 ；至于 value 是否为 null，根本没有判断过。

7. Hashtable 直接使用对象的 hash 值。hash 值是 JDK 根据对象的地址或者字符串或者数字算出来的 int 类型的数值。然后再使用除留余数法来获得最终的位置。然而除法运算是非常耗费时间的，效率很低。HashMap 为了提高计算效率，将哈希表的大小固定为了 2 的幂，这样在取模预算时，不需要做除法，只需要做位运算。位运算比除法的效率要高很多。

8. Hashtable、HashMap 都使用了 Iterator。而由于历史原因，Hashtable 还使用了 Enumeration 的方式。

9. 默认情况下，初始容量不同，Hashtable 的初始长度是 11，之后每次扩充容量变为之前的 2n+1（n 为上一次的长度）而 HashMap 的初始长度为 16，之后每次扩充变为原来的两倍。

   另外在 Hashtable 源码注释中有这么一句话：

   ```
   Hashtable is synchronized. If a thread-safe implementation is not needed, it is
   recommended to use HashMap in place of Hashtable . If a thread-safe highlyconcurrent implementation is desired, then it is recommended to use
   ConcurrentHashMap in place of Hashtable.
   ```

大致意思：Hashtable 是线程安全，推荐使用 HashMap 代替 Hashtable；如果需要线程安全高并发的话，推荐使用 ConcurrentHashMap 代替 Hashtable。

这个回答完了，面试官可能会继续问：HashMap 是线程不安全的，那么在需要线程安全的情况下还要考虑性能，有什么解决方式？

这里最好的选择就是 ConcurrentHashMap 了，但面试官肯定会叫你继续说一下 ConcurrentHashMap 数据结构以及底层原理等。



**拓展：什么是分段锁？**

ConcurrentHashMap 中的分段锁称为 Segment，它的内部结构是维护一个 HashEntry 数组，同时 Segment 还继承了 ReentrantLock。

当需要 put 元素的时候，并不是对整个 ConcurrentHashMap 进行加锁，而是先通过 hashcode 来判断它放在哪一个分段中，然后对该分段进行加锁。所以当多线程 put 的时候，只要不是放在同一个分段中，就可以实现并行的插入。分段锁的设计目的就是为了细化锁的粒度，从而提高并发能力。

jdk 1.8 中的 ConcurrentHashMap 中废弃了 Segment 锁，直接使用了数组元素，数组中的每个元素都可以作为一个锁。在元素中没有值的情况下，可以直接通过 CAS 操作来设值，同时保证并发安全；如果元素里面已经存在值的话，那么就使用 synchronized 关键字对元素加锁，再进行之后的 hash 冲突处理。jdk1.8 的 ConcurrentHashMap 加锁粒度比 jdk 1.7 里的 Segment 来加锁粒度更细，并发性能更好。

#### 5、初始容量大小和每次扩充容量大小不同

HashMap 的初始容量为：16，Hashtable 初始容量为：11，两者的负载因子默认都是：0.75

- HashMap 每次扩充，容量变为原来的 2 倍（ 2 n ）；
- HashTable 每次扩充，容量会变为原来的 2 倍 + 1（ 2 n + 1 ）；

下面给出 HashMap 中的源码：

```java
    /**
     * The default initial capacity - MUST be a power of two.
     */
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16

    /**
     * The maximum capacity, used if a higher value is implicitly specified
     * by either of the constructors with arguments.
     * MUST be a power of two <= 1<<30.
     */
    static final int MAXIMUM_CAPACITY = 1 << 30;

    /**
     * The load factor used when none specified in constructor.
     */
    static final float DEFAULT_LOAD_FACTOR = 0.75f;
```

**拓展：什么是负载因子？**

负载因子 loadFactor = 哈希表的有效元素个数 / 哈希表长度

这个值越大就说明冲突越严重一些
这个值越小说明冲突越小，数组利用率越低
扩容：采用整表扩容的方式什么时候需要对数组扩容？

扩容与否就根据负载因子来决定，当数组长度 * 负载因子 <=  有效元素个数就需要扩容

HashMap中的源码：

```
    /**
     * Returns a power of two size for the given target capacity.
     */
    static final int tableSizeFor(int cap) {
        int n = cap - 1;
        n |= n >>> 1;
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
```



#### 6、计算 hash 值的方法不同

为了得到元素的位置，首先需要根据元素的 KEY 计算出一个 hash 值，然后再用这个 hash 值来计算得到最终的位置。

Hashtable 中 hash 的计算方法为：直接使用对象的 hashCode()。
HashMap 中 hash 的计算方法为：key 的 hash 值高 16 位不变，低 16 位与高 16 位异或作为 key 最终的 hash 值。（h>>>16，表示无符号右移 16 位，高位补 0 ）

**HashTable：**

```java
    /**
     * Maps the specified <code>key</code> to the specified
     * <code>value</code> in this hashtable. Neither the key nor the
     * value can be <code>null</code>. <p>
     *
     * The value can be retrieved by calling the <code>get</code> method
     * with a key that is equal to the original key.
     *
     * @param      key     the hashtable key
     * @param      value   the value
     * @return     the previous value of the specified key in this hashtable,
     *             or <code>null</code> if it did not have one
     * @exception  NullPointerException  if the key or value is
     *               <code>null</code>
     * @see     Object#equals(Object)
     * @see     #get(Object)
     */
    public synchronized V put(K key, V value) {
        // Make sure the value is not null
        if (value == null) {
            throw new NullPointerException();
        }

        // Makes sure the key is not already in the hashtable.
        Entry<?,?> tab[] = table;
        int hash = key.hashCode();
        int index = (hash & 0x7FFFFFFF) % tab.length;
        @SuppressWarnings("unchecked")
        Entry<K,V> entry = (Entry<K,V>)tab[index];
        for(; entry != null ; entry = entry.next) {
            if ((entry.hash == hash) && entry.key.equals(key)) {
                V old = entry.value;
                entry.value = value;
                return old;
            }
        }

        addEntry(hash, key, value, index);
        return null;
    }
```

**HashMap：**

```java
    /**
     * Computes key.hashCode() and spreads (XORs) higher bits of hash
     * to lower.  Because the table uses power-of-two masking, sets of
     * hashes that vary only in bits above the current mask will
     * always collide. (Among known examples are sets of Float keys
     * holding consecutive whole numbers in small tables.)  So we
     * apply a transform that spreads the impact of higher bits
     * downward. There is a tradeoff between speed, utility, and
     * quality of bit-spreading. Because many common sets of hashes
     * are already reasonably distributed (so don't benefit from
     * spreading), and because we use trees to handle large sets of
     * collisions in bins, we just XOR some shifted bits in the
     * cheapest possible way to reduce systematic lossage, as well as
     * to incorporate impact of the highest bits that would otherwise
     * never be used in index calculations because of table bounds.
     */
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }

```

**拓展：什么是hashCode？**

hashCode 是 JDK 根据对象的地址或者字符串或者数字算出来的 int 类型的数值。

**两者为啥 hash 算法不一样？**

Hashtable 在计算元素的位置时使用除留余数法来获得存储的最终的位置，而除法运算是比较耗时的。

HashMap 为了提高计算效率，将哈希表的大小固定为了 2 的倍数，这样在取模运算时，不需要做除法，只需要做位运算（左移一位就是乘以 2 ）。位运算比除法的效率要高很多。

HashMap 的效率虽然提高了，但是 hash 冲突却也增加了。因为它得出的 hash 值的低位相同的概率比较高。

为了解决这个问题，HashMap 重新根据 hashcode 计算 hash 值后，又将 hash 值无符号右移 16 位，使得运算出来所取得的位置分散到高低位中，从而减少了 hash 冲突。HashMap 中采取的这种简单位运算操作，不会把使用 2 的幂次方带来的效率提升给抵消掉。


#### 7. 迭代器内部实现不同

Hashtable、HashMap 都使用了 Iterator。Hashtable 还使用了 Enumeration 的方式 。

Hashtable中 的 Enumerator 类，实现了 Enumeration 接口和 Iterator 接口：

```java
    /**
     * A hashtable enumerator class.  This class implements both the
     * Enumeration and Iterator interfaces, but individual instances
     * can be created with the Iterator methods disabled.  This is necessary
     * to avoid unintentionally increasing the capabilities granted a user
     * by passing an Enumeration.
     */
    private class Enumerator<T> implements Enumeration<T>, Iterator<T> {
        Entry<?,?>[] table = Hashtable.this.table;
        int index = table.length;
        Entry<?,?> entry;
        Entry<?,?> lastReturned;
        int type;

        /**
         * Indicates whether this Enumerator is serving as an Iterator
         * or an Enumeration.  (true -> Iterator).
         */
        boolean iterator;

        /**
         * The modCount value that the iterator believes that the backing
         * Hashtable should have.  If this expectation is violated, the iterator
         * has detected concurrent modification.
         */
        protected int expectedModCount = modCount;

        Enumerator(int type, boolean iterator) {
            this.type = type;
            this.iterator = iterator;
        }

        public boolean hasMoreElements() {
            Entry<?,?> e = entry;
            int i = index;
            Entry<?,?>[] t = table;
            /* Use locals for faster loop iteration */
            while (e == null && i > 0) {
                e = t[--i];
            }
            entry = e;
            index = i;
            return e != null;
        }

        @SuppressWarnings("unchecked")
        public T nextElement() {
            Entry<?,?> et = entry;
            int i = index;
            Entry<?,?>[] t = table;
            /* Use locals for faster loop iteration */
            while (et == null && i > 0) {
                et = t[--i];
            }
            entry = et;
            index = i;
            if (et != null) {
                Entry<?,?> e = lastReturned = entry;
                entry = e.next;
                return type == KEYS ? (T)e.key : (type == VALUES ? (T)e.value : (T)e);
            }
            throw new NoSuchElementException("Hashtable Enumerator");
        }

        // Iterator methods
        public boolean hasNext() {
            return hasMoreElements();
        }

        public T next() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
            return nextElement();
        }

        public void remove() {
            if (!iterator)
                throw new UnsupportedOperationException();
            if (lastReturned == null)
                throw new IllegalStateException("Hashtable Enumerator");
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();

            synchronized(Hashtable.this) {
                Entry<?,?>[] tab = Hashtable.this.table;
                int index = (lastReturned.hash & 0x7FFFFFFF) % tab.length;

                @SuppressWarnings("unchecked")
                Entry<K,V> e = (Entry<K,V>)tab[index];
                for(Entry<K,V> prev = null; e != null; prev = e, e = e.next) {
                    if (e == lastReturned) {
                        modCount++;
                        expectedModCount++;
                        if (prev == null)
                            tab[index] = e.next;
                        else
                            prev.next = e.next;
                        count--;
                        lastReturned = null;
                        return;
                    }
                }
                throw new ConcurrentModificationException();
            }
        }
    }
```

HashMap 中的 Iterator ：

```java
  /* ------------------------------------------------------------ */
    // iterators

    abstract class HashIterator {
        Node<K,V> next;        // next entry to return
        Node<K,V> current;     // current entry
        int expectedModCount;  // for fast-fail
        int index;             // current slot

        HashIterator() {
            expectedModCount = modCount;
            Node<K,V>[] t = table;
            current = next = null;
            index = 0;
            if (t != null && size > 0) { // advance to first entry
                do {} while (index < t.length && (next = t[index++]) == null);
            }
        }

        public final boolean hasNext() {
            return next != null;
        }

        final Node<K,V> nextNode() {
            Node<K,V>[] t;
            Node<K,V> e = next;
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
            if (e == null)
                throw new NoSuchElementException();
            if ((next = (current = e).next) == null && (t = table) != null) {
                do {} while (index < t.length && (next = t[index++]) == null);
            }
            return e;
        }

        public final void remove() {
            Node<K,V> p = current;
            if (p == null)
                throw new IllegalStateException();
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
            current = null;
            K key = p.key;
            removeNode(hash(key), key, null, false, false);
            expectedModCount = modCount;
        }
    }
```

**拓展：JDK 8 之后，HashMap 和 Hashtable 的 Iterator 都有 fast-fail 机制。**

当有其它线程修改了 HashMap 的结构时，将会抛出 ConcurrentModificationException 异常。

注：结构修改是指改变 HashMap 中的映射数量或以其他方式修改其内部结构 (如，重新哈希，增加，删除，修改元素)。

**什么是 fast-fail 机制？**

![](https://dream-syz.github.io/1-1.png)

例如，通常不允许一个线程在另一个线程迭代 Collection 时修改它。

通常，迭代的结果在这些情况下是没有定义的。如果检测到此行为，一些 Iterator 实现(包括JRE提供的所有通用集合实现)可能会选择抛出此异常。这样做的迭代器被称为快速失败迭代器，因为它们快速而干净地失败，而不是冒着在未来不确定的时间发生任意、不确定行为的风险。

迭代器中的 modCount 变量，类似于并发编程中的 CAS（Compare and Swap）技术。我们可以看到这个方法中，每次在发生增删改的时候都会出现 modCount++ 的动作。

![](https://dream-syz.github.io/1-2.png)

而 modcount 可以理解为是当前 hashtable 的状态。每发生一次操作，状态就向前走一步。设置这个状态，主要是由于 hashtable 等容器类在迭代时，判断数据是否过时时使用的。尽管 hashtable 采用了原生的同步锁来保护数据安全。但是在出现迭代数据的时候，则无法保证边迭代，边正确操作。于是使用这个值来标记状态。一旦在迭代的过程中状态发生了改变，则会快速抛出一个异常，终止迭代行为。



### 13、 Collection 包结构与 Collections 的区别

Collection 是集合类的上级接口，子接口有 Set、List、LinkedList、ArrayList、Vector、Stack、Set；

Collections 是集合类的一个帮助类， 它包含有各种有关集合操作的静态多态方法，用于实现对各种集合的搜索、排序、线程安全化等操作。此类不能实例化，就像一个工具类，服务于 Java 的 Collection 框架。



### 14、 Java 的四种引用，强弱软虚

#### 1、强引用

强引用是平常中使用最多的引用，强引用在程序内存不足（OOM）的时候也不会被回收，使用方式：

```java
String str = new String("str");
System.out.println(str);
```

#### 2、软引用

软引用在程序内存不足时，会被回收，使用方式：

```java
// 注意：wrf 这个引用也是强引用，它是指向 SoftReference 这个对象的，
// 这里的软引用指的是指向 new String("str") 的引用，也就是 SoftReference 类中 T
SoftReference<String> wrf = new SoftReference<String>(new String("str"));
```

可用场景： 创建缓存的时候，创建的对象放进缓存中，当内存不足时，JVM 就会回收早先创建的对象。

#### 3、弱引用

弱引用就是只要 JVM 垃圾回收器发现了它，就会将之回收，他的强度比软引用更低一点，弱引用的对象下一次 GC 的时候一定会被回收，而不管内存是否足够。使用方式：

```java
WeakReference<String> wrf = new WeakReference<String>(str);
```

**可用场景：** Java 源码中的 `java.util.WeakHashMap` 中的 `key` 就是使用弱引用，我的理解就是，一旦我不需要某个引用，JVM 会自动帮我处理它，这样我就不需要做其它操作。

#### 4、虚引用

虚引用的回收机制跟弱引用差不多，但是它被回收之前，会被放入 `ReferenceQueue` 中。注意哦，其它引用是被 JVM 回收后才被传入 `ReferenceQueue` 中的。由于这个机制，所以虚引用大多被用于引用销毁前的处理工作。还有就是，虚引用创建的时候，必须带有 `ReferenceQueue` ，同样的当发生GC的时候，虚引用也会被回收。可以用虚引用来管理堆外内存

使用例子：

```java
PhantomReference<String> prf = new PhantomReference<String>(new String("str"),
new ReferenceQueue<>());
```

可用场景： 对象销毁前的一些操作，比如说资源释放等。 `Object.finalize()` 虽然也可以做这类动作，但是这个方式即不安全又低效

上诉所说的几类引用，都是指对象本身的引用，而不是指 Reference 的四个子类的引用（ SoftReference 等）。



### 15、 泛型常用特点

泛型是 Java SE 1.5 之后的特性， 《Java 核心技术》中对泛型的定义是：

“泛型” 意味着编写的代码可以被不同类型的对象所重用。

“泛型”，顾名思义，“泛指的类型”。我们提供了泛指的概念，但具体执行的时候却可以有具体的规则来约束，比如我们用的非常多的ArrayList 就是个泛型类，ArrayList作为集合可以存放各种元素，如 Integer， String，自定义的各种类型等，但在我们使用的时候通过具体的规则来约束，如我们可以约束集合中只存放Integer类型的元素，如

```java
List<Integer> iniData = new ArrayList<>()
```

使用泛型的好处？

以集合来举例，使用泛型的好处是我们不必因为添加元素类型的不同而定义不同类型的集合，如整型集合类，浮点型集合类，字符串集合类，我们可以定义一个集合来存放整型、浮点型，字符串型数据，而这并不是最重要的，因为我们只要把底层存储设置了 Object 即可，添加的数据全部都可向上转型为 Object。 更重要的是我们可以通过规则按照自己的想法控制存储的数据类型。



### 16、 Java 创建对象有几种方式？

#### **1、new 关键字**

使用 new 关键字创建对象，这是我们最常见的也是最简单的创建对象的方式，通过这种方式我们还可以调用任意的构造器（无参的和有参的）。

```java
ClassName myClass = new ClassName();
```

**注意：**

- 当我们使用 `new` 创建了一个对象时，会在栈中创建 `myClass` 这个引用，并且在堆中开辟一块空间存放对象的值，然后让 `myClass` 这个引用指向堆中新建的 `ClassName` 对象的值；

- 不管每次创建的对象的值是否相同，每次用 `new` 创建对象时，在栈中创建的引用都是不一样的，即地址都是不一样的。

#### **2、Class.newInstance**

这是我们运用 **反射** 创建对象时最常用的方法。

`Class` 类的 `newInstance` 使用的是类的 `public` 的 **无参** 构造方法。因此也就是说使用此方法创建对象的前提是必须有 `public` 的无参构造器才行：

```java
ClassName myClass = ClassName.class.newInstance();
ClassName myClass = (ClassName)Class.foeName("ClassName").newInstance();
```

#### 3、Constructor.newInstance

该方法和 `Class` 类的 `newInstance` 方法很像，但是比它强大很多。

`java.lang.relect.Constructor` 类里也有一个 `newInstance` 方法可以创建对象。

我们可以通过这个 `newInstance` 方法调用 **有参数**（不再必须是无参）的和 **私有的** 构造函数（不再必须是 `public` ）。

```java
Constructor<ClassName> constructor = ClassName.class.getConstructor();
ClassName class = constructor.newInstance();
```

#### 两种 newInstance 方法的区别

- `Class` 类位于 `java` 的 `lang` 包中，
  `Constructor` 是 `java` 反射机制的一部分

- `Class` 类的 `newInstance` 只能触发 **无参数** 的构造方法创建对象，
  `Constructor` 类的 `newInstance` 能触发 **有参数** 或者 **任意参数** 的构造方法来创建对象。
- `Class` 类的 `newInstance` 需要其构造方法是 `public` 的或者对调用方法可见的，
  `Constructor` 类的 `newInstance` 可以在特定环境下调用 **私有构造方法** 来创建对象。

- `Class` 类的 `newInstance` 抛出类构造函数的异常，
  `Constructor` 类的 `newInstance` 包装了一个 `InvocationTargetException` 异常。

#### 4、Clone 方法

无论何时我们调用一个对象的 `clone` 方法，`JVM` 就会创建一个新的对象，将前面的对象的内容全部拷贝进去，用 `clone` 方法创建对象并不会调用任何构造函数。

要使用 `clone` 方法，我们必须先实现 `Cloneable` 接口并复写 `Object` 的 `clone` 方法（因为 `Object` 的这个方法是 `protected` 的，若不复写，外部也调用不了）。

    public class ClassName implements Cloneable {
    	...
    	// 访问权限写为public，并且返回值写为myclass
        @Override
        public ClassName clone() throws CloneNotSupportedException {
            return (ClassName) super.clone();
        }
        ...
    }
    
    public class Main {
    	public static void main(String[] args) throws Exception {
       		ClassName myClass = new ClassName();
        	Object clone = myclass.clone();
        	//ClassName clone = (ClassName)myClass.clone();
    	}
    }

**注意：**

- `clone()` 方法只会进行 **浅复制**，也就是说只会在栈中再创建一个引用指向原来的对象的值所处的堆空间中，堆中的对象的值还是原来的并没有重新创建；

- 使用 `clone()` 方法并不需要调用造函数

#### 5、反序列化

要想通过反序列化创建对象，就必须现将某个对象序列化

**概念：**

1.**序列化：将 `java` 对象转化为字节流或字符流的过程**

**作用：**

（1）便于在网络上进行传输；

（2）将 `java` 字节序列永久保存在硬盘上，通常放在文件中，所以序列化也可以叫做持久化。

2.**反序列化：将字节流或字符流对象转化成 `java` 对象的过程**

```java
/**
 * 使用反序列化创建对象
 * 当序列化和反序列化一个对象时，JVM会创建一个单独的对象。
 * 在反序列化时，JVM 创建对象并不会调用任何构造函数，
 * 为了反序列化一个对象，需要让我们的 ClassName 类实现 Serializable 接口
 */
@Test
public void function() {
    String filePath = "F:\\Tests\\data.obj";
    try {
	    //序列化过程
        ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(filePath));
        objectOutputStream.writeObject(new ClassName());
        objectOutputStream.close();
 
        //反序列化过程
		ObjectInputStream inputStream = new ObjectInputStream(new FileInputStream(filePath));
		ClassName myClass = (ClassName) inputStream.readObject();
		inputStream.close();
 
	} catch (IOException | ClassNotFoundException e) {
		e.printStackTrace();
	}
}
```

**备注**：JDK 序列化、反序列化特别特别耗内存。

#### Java 创建实例对象是不是必须要通过构造函数/构造器/构造方法？

| 创建对象方式            | 是否调用了构造器 |
| ----------------------- | ---------------- |
| new关键字               | 是               |
| Class.newInstance       | 是               |
| Constructor.newInstance | 是               |
| Clone                   | 否               |
| 反序列化                | 否               |

**答案：**Java 创建实例对象，并不一定必须要调用构造函数/构造器/构造方法的。



### 17 、有没有可能两个不相等的对象有相同的 hashcode

有可能.在产生 hash 冲突时,两个不相等的对象就会有相同的 hashcode 值。当 hash 冲突产生时,一般有以下几种方式来处理：

- 拉链法：每个哈希表节点都有一个 next 指针，多个哈希表节点可以用 next 指针构成一个单向链表，被分配到同一个索引上的多个节点可以用这个单向链表进行存储

- 开放定址法：一旦发生了冲突,就去寻找下一个空的散列地址，只要散列表足够大，空的散列地址总能找到，并将记录存入
- 再哈希：又叫双哈希法，有多个不同的 Hash 函数。当发生冲突时，使用第二个，第三个….等哈希函数计算地址，直到无冲突

#### 为什么 java 的指针压缩：

在 Java 中，指针压缩（Pointer Compression）是一种优化技术，用于减少对象指针的内存占用和提高内存访问效率。

在 32 位的 Java 虚拟机中，每个对象引用（指针）通常占用4字节的内存空间。然而，在实际的应用程序中，大部分的堆对象的内存地址范围并不需要使用整个 32 位空间。因此，指针压缩的目标是通过缩小指针的有效位数来减少内存消耗。

指针压缩的具体实现方式是通过将堆对象的内存地址空间划分为几个区域，其中一部分用于存储对象的数据，另一部分用于存储指针。指针压缩将指针的有效位数缩小，从而减少了指针的内存占用。例如，可以将指针大小压缩到3字节或2字节。

指针压缩的好处是可以减少内存消耗，并且在一定程度上提高内存访问效率。较小的指针大小意味着可以在更短的时间内读取或写入指针所指向的对象。此外，指针压缩还可以减少垃圾回收器的工作量，提高垃圾回收的效率。

需要注意的是，指针压缩只在 32 位的 Java 虚拟机中有效，因为在 64 位的虚拟机中，由于地址空间的扩展，指针大小通常为 8 字节，已经足够存储大量的对象引用。

#### 哈希冲突的产生原因及解决方法

##### 哈希冲突的产生

哈希表是根据关键码的值而直接进行访问的数据结构。

哈希法又称散列法、杂凑法以及关键字地址计算法等，相应的表称为哈希表。

哈希表中关键码就是数组的索引下标，然后通过下标直接访问数组中的元素。（哈希表可以用来快速判断一个元素是否出现在集合里）

哈希法法的基本思想是：首先在元素的关键字 k 和元素的存储位置 p 之间建立一个对应关系 f ，使得 p = f ( k ) ，f称为哈希函数。创建哈希表时，把关键字为 k 的元素直接存入地址为 f ( k ) 的单元；以后当查找关键字为 k 的元素时，再利用哈希函数计算出该元素的存储位置p = f ( k ) ，从而达到按关键字直接存取元素的目的。

当关键字集合很大时，关键字值不同的元素可能会映象到哈希表的同一地址上，即 k1 ≠ k2 ，但 H（k1）= H（k2），这种现象称为冲突，此时称 k1 和 k2 为同义词。实际中，冲突是不可避免的，只能通过改进哈希函数的性能来减少冲突。

综上所述，哈希法主要包括以下两方面的内容：

 1）如何构造哈希函数

 2）如何处理冲突

##### **产生哈希冲突的影响因素**

装填因子（ 装填因子 = 数据总数 / 哈希表长 ）、哈希函数、处理冲突的方法

##### **哈希冲突解决办法**

###### 1、开放定址法（再散列法）

基本思想：当关键字 key 的哈希地址 p = H（key）出现冲突时，以 p 为基础，产生另一个哈希地址 p1，如果 p1 仍然冲突，再以 p 为基础，产生另一个哈希地址 p2 ，…，直到找出一个不冲突的哈希地址 pi ，将相应元素存入其中。

（1）线性探测

按顺序决定值时，如果某数据的值已经存在，则在原来值的基础上往后加一个单位，直至不发生哈希冲突。

（2）再平方探测

按顺序决定值时，如果某数据的值已经存在，则在原来值的基础上先加1的平方个单位，若仍然存在则减 1 的平方个单位。随之是 2 的平方，3 的平方等等。直至不发生哈希冲突。

（3）伪随机探测

按顺序决定值时，如果某数据已经存在，通过随机函数随机生成一个数，在原来值的基础上加上随机数，直至不发生哈希冲突。

###### 2、链地址法（拉链法：**HashMap 的哈希冲突解决方法**）

基本思想：以数组为基本单元，将所有的哈希地址为 i 的元素构成一个称为同义词链的单链表，并将单链表的头指针存在哈希表的第 i 个单元中，因而查找、插入和删除主要在同义词链中进行。

链地址法适用于经常进行插入和删除的情况。

**优点：**

（1）拉链法处理冲突简单，且无堆积现象，即非同义词决不会发生冲突，因此平均查找长度较短；

（2）由于拉链法中各链表上的结点空间是动态申请的，故它更适合于造表前无法确定表长的情况；

（3）开放定址法为减少冲突，要求装填因子 α 较小，故当结点规模较大时会浪费很多空间。而拉链法中可取 α ≥ 1 ，且结点较大时，拉链法中增加的指针域可忽略不计，因此节省空间；

（4）在用拉链法构造的散列表中，删除结点的操作易于实现。只要简单地删去链表上相应的结点即可。

**缺点：**

指针占用较大空间时，会造成空间浪费，若空间用于增大散列表规模进而提高开放地址法的效率。

###### 3、再哈希法

基本思想：同时构造多个不同的哈希函数，在发生冲突的时候再用另外一个哈希函数算出哈希值，直到算出的哈希值不同为止。

###### 4、建立公共溢出区

基本思想：将哈希表分为基本表和溢出表两部分，凡是和基本表发生冲突的元素，一律填入溢出表。查表时，先去基本表查，查不到再去溢出区查找。



### 18、深拷贝和浅拷贝的区别是什么？

- 浅拷贝：被复制对象的所有变量都含有与原来的对象相同的值，而所有的对其他对象的引用仍然指向原来的对象。换言之，**浅拷贝仅仅复制所考虑的对象，而不复制它所引用的对象**

- 深拷贝：被复制对象的所有变量都含有与原来的对象相同的值，而那些引用其他对象的变量将指向被复制过的新对象，而不再是原有的那些被引用的对象。换言之，**深拷贝把要复制的对象所引用的对象都复制了一遍**



### 19、final 有哪些用法?

- 被 final 修饰的类不可以被继承

- 被 final 修饰的方法不可以被重写

- 被 final 修饰的变量不可以被改变。如果修饰引用，那么表示引用不可变，引用指向的内容可变

- 被 final 修饰的方法，JVM 会尝试将其内联，以提高运行效率

- 被 final 修饰的常量，在编译阶段会存入常量池中

除此之外,编译器对 final 域要遵守的两个重排序规则更好：

在构造函数内对一个 final 域的写入，与随后把这个被构造对象的引用赋值给一个引用变量，这两个操作之间不能重排序，初次读一个包含final域的对象的引用，与随后初次读这个 final 域，这两个操作之间不能重排序.



### 20、static 都有哪些用法？

所有的人都知道 static 关键字这两个基本的用法：静态变量和静态方法。也就是被 static 所修饰的变量 / 方法都属于类的静态资源，类实例所共享.

除了静态变量和静态方法之外，static 也用于静态块，多用于初始化操作：

```java
public calss PreCache{
 static{
 //执行相关操作
 }
}
```

此外 static 也多用于修饰内部类,此时称之为静态内部类

最后一种用法就是静态导包，即 `import static` .import static是在  JDK 1.5 之后引入的新特性，可以用来指定导入某个类中的静态资源,并且不需要使用类名,可以直接使用资源名，比如：

```java
import static java.lang.Math.*;
public class Test{
 public static void main(String[] args){
 //System.out.println(Math.sin(20));传统做法
 System.out.println(sin(20));
 }
}
```



### 21、 3 * 0.1 **==**  0.3 返回值是什么

false，因为有些浮点数不能完全精确的表示出来



### 22、a = a + b 与 a + = b 有什么区别吗?

`+=` 操作符会进行隐式自动类型转换，此处 a + = b 隐式的将加操作的结果类型强制转换为持有结果的类型,而 a = a + b 则不会自动进行类型转换。如：

```java
byte a = 127;
byte b = 127;
b = a + b; // 报编译错误:cannot convert from int to byte
b += a;
```

以下代码是否有错,有的话怎么改？

```java
short s1= 1;
s1 = s1 + 1;
```

有错误。short 类型在进行运算时会自动提升为 int 类型，也就是说 s1 + 1 的运算结果是 int 类型而 s1 是 short 类型，此时编译器会报错

正确写法：

```JAVA
short s1= 1;
s1 += 1;
```

`+=` 操作符会对右边的表达式结果强转匹配左边的数据类型，所以没错



### 23、try catch finally，try 里有 return ，finally 还执行么？

执行，并且 finally 的执行早于 try 里面的 return。如果 finally 块中存在 return 语句，它会覆盖 try 块或 catch 块中的 return 语句，并成为最终的返回值。

结论：

1、不管有木有出现异常，finally 块中代码都会执行；

2、当 try 和 catch 中有 return 时，finally 仍然会执行；

3、finally 是在 return 后面的表达式运算后执行的（此时并没有返回运算后的值，而是先把要返回的值保存起来，管 finally 中的代码怎么样，返回的值都不会改变，仍然是之前保存的值），所以函数返回值是在 finally 执行前确定的；

4、finally 中最好不要包含 return，否则程序会提前退出，返回值不是 try 或 catch 中保存的返回值。



### 24、Excption 与 Error 包结构

Java 可抛出 ( Throwable ) 的结构分为三种类型：被检查的异常 ( CheckedException )，运行时异常 (RuntimeException )，错误( Error )

#### 1、运行时异常

定义：RuntimeException 及其子类都被称为运行时异常。

特点：Java 编译器不会检查它。也就是说，当程序中可能出现这类异常时，倘若既"没有通过 throws 声明抛出它"，也"没有用 try-catch语句捕获它"，还是会编译通过。例如，除数为零时产生的 ArithmeticException 异常，数组越界时产生的 IndexOutOfBoundsException异常，fail - fast 机制产生的 ConcurrentModificationException 异常（java.util 包下面的所有的集合类都是快速失败的，“快速失败”也就是 fail-fast，它是 Java 集合的一种错误检测机制。当多个线程对集合进行结构上的改变的操作时，有可能会产生fail-fast机制。记住是有可能，而不是一定。例如：假设存在两个线程（线程1、线程2），线程 1 通过 Iterator 在遍历集合 A 中的元素，在某个时候线程 2 修改了集合 A 的结构（是结构上面的修改，而不是简单的修改集合元素的内容），那么这个时候程序就会抛出ConcurrentModificationException 异常，从而产生 fail-fast 机制，这个错叫并发修改异常。Failsafe，java.util.concurrent 包下面的所有的类都是安全失败的，在遍历过程中，如果已经遍历的数组上的内容变化了，迭代器不会抛出 ConcurrentModificationException 异常。如果未遍历的数组上的内容发生了变化，则有可能反映到迭代过程中。这就是 ConcurrentHashMap 迭代器弱一致的表现。ConcurrentHashMap 的弱一致性主要是为了提升效率，是一致性与效率之间的一种权衡。要成为强一致性，就得到处使用锁，甚至是全局锁，这就与 Hashtable 和同步的 HashMap 一样了。）等，都属于运行时异常。

常见的五种运行时异常：

ClassCastException（类转换异常）

IndexOutOfBoundsException（数组越界）

NullPointerException（空指针异常）

ArrayStoreException（数据存储异常，操作数组是类型不一致）

BufferOverflowException

#### 2、被检查异常

定义：Exception 类本身，以及 Exception 的子类中除了"运行时异常"之外的其它子类都属于被检查异常。

特点：Java 编译器会检查它。 此类异常，要么通过 throws 进行声明抛出，要么通过 try - catch 进行捕获处理，否则不能通过编译。例如，CloneNotSupportedException 就属于被检查异常。当通过 clone() 接口去克隆一个对象，而该对象对应的类没有实现 Cloneable 接口，就会抛出 CloneNotSupportedException 异常。被检查异常通常都是可以恢复的。 如：

IOException

FileNotFoundException

SQLException

被检查的异常适用于那些不是因程序引起的错误情况，比如：读取文件时文件不存在引发的 FileNotFoundException 。然而，不被检查的异常通常都是由于糟糕的编程引起的，比如：在对象引用时没有确保对象非空而引起的 NullPointerException 。

#### 3、错误

定义：Error 类及其子类。

特点：和运行时异常一样，编译器也不会对错误进行检查。

当资源不足、约束失败、或是其它程序无法继续运行的条件发生时，就产生错误。程序本身无法修复这些错误的。例如，VirtualMachineError 就属于错误。出现这种错误会导致程序终止运行。

OutOfMemoryError、ThreadDeath。

Java 虚拟机规范规定 JVM 的内存分为了好几块，比如堆，栈，程序计数器，方法区等



### 25、OOM 你遇到过哪些情况，SOF 你遇到过哪些情况

#### **OOM**：

##### 1、OutOfMemoryError 异常

除了程序计数器外，虚拟机内存的其他几个运行时区域都有发生 OutOfMemoryError ( OOM ) 异常的可能。

Java Heap 溢出：

一般的异常信息：java.lang.OutOfMemoryError:Java heap spacess。

java 堆用于存储对象实例，我们只要不断的创建对象，并且保证 GC Roots 到对象之间有可达路径来避免垃圾回收机制清除这些对象，就会在对象数量达到最大堆容量限制后产生内存溢出异常。

出现这种异常，一般手段是先通过内存映像分析工具 ( 如 Eclipse Memory Analyzer ) 对 dump 出来的堆转存快照进行分析，重点是确认内存中的对象是否是必要的，先分清是因为内存泄漏 ( Memory Leak ) 还是内存溢出 ( Memory Overflow )。

如果是内存泄漏，可进一步通过工具查看泄漏对象到 GCRoots 的引用链。于是就能找到泄漏对象是通过怎样的路径与 GC Roots 相关联并导致垃圾收集器无法自动回收。

如果不存在泄漏，那就应该检查虚拟机的参数  ( -Xmx 与 -Xms ) 的设置是否适当。

##### 2、虚拟机栈和本地方法栈溢出

如果线程请求的栈深度大于虚拟机所允许的最大深度，将抛出 StackOverflowError 异常。

如果虚拟机在扩展栈时无法申请到足够的内存空间，则抛出 OutOfMemoryError异常

这里需要注意当栈的大小越大可分配的线程数就越少。

##### 3、运行时常量池溢出

异常信息：java.lang.OutOfMemoryError:PermGenspace

如果要向运行时常量池中添加内容，最简单的做法就是使用 String.intern() 这个 Native 方法。该方法的作用是：如果池中已经包含一个等于此 String 的字符串，则返回代表池中这个字符串的String对象；否则，将此 String 对象包含的字符串添加到常量池中，并且返回此String 对象的引用。由于常量池分配在方法区内，我们可以通过 -XX:PermSize 和 -XX:MaxPermSize 限制方法区的大小，从而间接限制其中常量池的容量。

##### 4、方法区溢出

方法区用于存放 Class 的相关信息，如类名、访问修饰符、常量池、字段描述、方法描述等。也有可能是方法区中保存的 class 对象没有被及时回收掉或者class信息占用的内存超过了我们配置。

异常信息：java.lang.OutOfMemoryError:PermGenspace

方法区溢出也是一种常见的内存溢出异常，一个类如果要被垃圾收集器回收，判定条件是很苛刻的。在经常动态生成大量 Class 的应用中，要特别注意这点。

#### SOF（堆栈溢出 StackOverflow）：

StackOverflowError 的定义：当应用程序递归太深而发生堆栈溢出时，抛出该错误。

因为栈一般默认为 1 - 2 m，一旦出现死循环或者是大量的递归调用，在不断的压栈过程中，造成栈容量超过 1 m 而导致溢出。

栈溢出的原因：递归调用，大量循环或死循环，全局变量是否过多，数组、List、map数据过大。



### 26、 简述线程、程序、进程的基本概念。以及他们之间关系是什么?

**线程**与进程相似，但线程是一个比进程更小的执行单位。一个进程在其执行的过程中可以产生多个线程。与进程不同的是同类的多个线程共享同一块内存空间和一组系统资源，所以系统在产生一个线程，或是在各个线程之间作切换工作时，负担要比进程小得多，也正因为如此，线程也被称为轻量级进程。

**程序**是含有指令和数据的文件，被存储在磁盘或其他的数据存储设备中，也就是说程序是静态的代码。

**进程**是程序的一次执行过程，是系统运行程序的基本单位，因此进程是动态的。系统运行一个程序即是一个进程从创建，运行到消亡的过程。简单来说，一个进程就是一个执行中的程序，它在计算机中一个指令接着一个指令地执行着，同时，每个进程还占有某些系统资源如 CPU 时间，内存空间，文件，输入输出设备的使用权等等。换句话说，当程序在执行时，将会被操作系统载入内存中。 线程是进程划分成的更小的运行单位。线程和进程最大的不同在于基本上各进程是独立的，而各线程则不一定，因为同一进程中的线程极有可能会相互影响。从另一角度来说，进程属于操作系统的范畴，主要是同一段时间内，可以同时执行一个以上的程序，而线程则是在同一程序内几乎同时执行一个以上的程序段。



### 27、Java 序列化中如果有些字段不想进行序列化，怎么办？

对于不想进行序列化的变量，使用 transient 关键字修饰。

transient 关键字的作用是：阻止实例中那些用此关键字修饰的的变量序列化；当对象被反序列化时，被 transient 修饰的变量值不会被持久化和恢复。transient 只能修饰变量，不能修饰类和方法。



### 28、说说 Java 中 IO 流

#### Java 中 IO 流分为几种?

- 按照流的流向分，可以分为输入流和输出流；

- 按照操作单元划分，可以划分为字节流和字符流；

- 按照流的角色划分为节点流和处理流。

Java IO 流共涉及 40 多个类，这些类看上去很杂乱，但实际上很有规则，而且彼此之间存在非常紧密的联系， Java IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的。

InputStream / Reader：所有的输入流的基类，前者是字节输入流，后者是字符输入流。

OutputStream / Writer：所有输出流的基类，前者是字节输出流，后者是字符输出流。



### 29、 Java IO 与 NIO 的区别（补充）

NIO 即 New IO，这个库是在 JDK1.4 中才引入的。NIO 和 IO 有相同的作用和目的，但实现方式不同，NIO 主要用到的是块，所以 NIO 的效率要比 IO 高很多。在 Java API 中提供了两套 NIO，一套是针对标准输入输出 NIO，另一套就是网络编程 NIO。



### 30、java 反射的作用于原理

#### 1、定义：

反射机制是在运行时，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意个对象，都能够调用它的任意一个方法。在 java中，只要给定类的名字，就可以通过反射机制来获得类的所有信息。

**这种动态获取的信息以及动态调用对象的方法的功能称为 Java 语言的反射机制。**

#### 2、哪里会用到反射机制？

jdbc 就是典型的反射

```java
Class.forName('com.mysql.jdbc.Driver.class');//加载 MySQL 的驱动类
```

这就是反射。如 hibernate，struts 等框架使用反射实现的。

#### 3、反射的实现方式：

第一步：获取 Class 对象，有 4 中方法： 

1）Class.forName (“类的路径”)； 

2）类名.class 

3）对象名.getClass() 

4）基本类型的包装类，可以调用包装类的 Type 属性来获得该包装类的 Class 对象

#### 4、实现 Java 反射的类：

1）Class：表示正在运行的 Java 应用程序中的类和接口。注意： 所有获取对象的信息都需要 Class 类来实现。 

2）Field：提供有关类和接口的属性信息，以及对它的动态访问权限。 

3）Constructor：提供关于类的单个构造方法的信息以及它的访问权限 

4）Method：提供类或接口中某个方法的信息

#### 5、反射机制的优缺点：

**优点：** 

1）能够运行时动态获取类的实例，提高灵活性

2）与动态编译结合 

**缺点：** 

1）使用反射性能较低，需要解析字节码，将内存中的对象进行解析。 

解决方案： 

1、通过 setAccessible ( true ) 关闭  JDK 的安全检查来提升反射速度； 

2、多次创建一个类的实例时，有缓存会快很多 

3、ReflectASM 工具类，通过字节码生成的方式加快反射速度

2）相对不安全，破坏了封装性（ 因为通过反射可以获得私有方法和属性 ）



### 31、说说 List，Set，Map 三者的区别？

- List（对付顺序的好帮手）：List 接口存储一组不唯一（可以有多个元素引用相同的对象），有序的对象

- Set（注重独一无二的性质）：不允许重复的集合。不会有多个元素引用相同的对象。

- Map（用 Key 来搜索的专家）：使用键值对存储。Map 会维护与 Key 有关联的值。两个 Key 可以引用相同的对象，但 Key 不能重复，典型的 Key 是 String 类型，但也可以是任何对象。



### 32、Object 有哪些常用方法？

java.lang.Object

**clone** **方法**

保护方法，实现对象的浅复制，只有实现了 Cloneable 接口才可以调用该方法，否则抛出 CloneNotSupportedException 异常，深拷贝也需要实现 Cloneable，同时其成员变量为引用类型的也需要实现 Cloneable，然后重写 clone 方法。

**finalize** **方法**

该方法和垃圾收集器有关系，判断一个对象是否可以被回收的最后一步就是判断是否重写了此方法。

**equals** **方法**

该方法使用频率非常高。一般 equals 和 == 是不一样的，但是在 Object 中两者是一样的。子类一般都要重写这个方法。

**hashCode** **方法**

该方法用于哈希查找，重写了 equals 方法一般都要重写 hashCode 方法，这个方法在一些具有哈希功能的 Collection 中用到。

一般必须满足 obj1.equals(obj2) == true 。可以推出 obj1.hashCode() == obj2.hashCode() ，但是 hashCode 相等不一定就满足 equals。不过为了提高效率，应该尽量使上面两个条件接近等价。

JDK 1.6、1.7  默认是返回随机数；

JDK 1.8  默认是通过和当前线程有关的一个随机数 + 三个确定值，运用 Marsaglia’s xorshift scheme 随机数算法得到的一个随机数。

**wait** **方法**

配合 synchronized 使用，wait 方法就是使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。wait() 方法一直等待，直到获得锁或者被中断。wait ( long timeout ) 设定一个超时间隔，如果在规定时间内没有获得锁就返回。

调用该方法后当前线程进入睡眠状态，直到以下事件发生。

1. 其他线程调用了该对象的 notify 方法；
2. 其他线程调用了该对象的 notifyAll 方法；
3. 其他线程调用了 interrupt 中断该线程；
4. 时间间隔到了。

此时该线程就可以被调度了，如果是被中断的话就抛出一个 InterruptedException 异常。

**notify** **方法**

配合 synchronized 使用，该方法唤醒在该对象上**等待队列**中的某个线程（同步队列中的线程是给抢占 CPU 的线程，等待队列中的线程指的是等待唤醒的线程）。

**notifyAll** **方法**

配合 synchronized 使用，该方法唤醒在该对象上等待队列中的所有线程。

**总结**

只要把上面几个方法熟悉就可以了，toString 和 getClass 方法可以不用去讨论它们。该题目考察的是对 Object 的熟悉程度，平时用的很多方法并没看其定义但是也在用，比如说：wait() 方法，equals() 方法等。

```
Class Object is the root of the class hierarchy.Every class has Object as a
superclass. All objects, including arrays, implement the methods of this class.
```

大致意思：Object 是所有类的根，是所有类的父类，所有对象包括数组都实现了 Object 的方法。



### 33、获取一个类 Class 对象的方式有哪些？

搞清楚类对象和实例对象，但都是对象。

第一种：通过类对象的 getClass() 方法获取，细心点的都知道，这个 getClass 是 Object 类里面的方法。

```java
User user=new User();
//clazz就是一个User的类对象
Class<?> clazz=user.getClass()
```

第二种：通过类的静态成员表示，每个类都有隐含的静态成员 class。

```java
//clazz就是一个User的类对象
Class<?> clazz=User.class;
```

第三种：通过 Class 类的静态方法 forName() 方法获取。

```java
Class<?> clazz = Class.forName("com.tian.User");
```



### 34、**用过** **ArrayList** 吗？说一下它有什么特点？

Java 集合框架中的一种存放相同类型的元素数据，是一种变长的集合类，基于定长数组实现，当加入数据达到一定程度后，会实行自动扩容，即扩大数组大小。

底层是使用数组实现，添加元素。

如果 add ( o ) ，添加到的是数组的尾部，如果要增加的数据量很大，应该使用 ensureCapacity() 方法，该方法的作用是预先设置 ArrayList 的大小，这样可以大大提高初始化速度。

如果使用 add ( int , o ) ，添加到某个位置，那么可能会挪动大量的数组元素，并且可能会触发扩容机制。

高并发的情况下，线程不安全。多个线程同时操作 ArrayList，会引发不可预知的异常或错误。

ArrayList 实现了 Cloneable 接口，标识着它可以被复制。注意：ArrayList 里面的 clone() 复制其实是浅复制。

**有数组了为什么还要搞个** **ArrayList** **呢**

通常我们在使用的时候，如果在不明确要插入多少数据的情况下，普通数组就很尴尬了，因为你不知道需要初始化数组大小为多少，而 ArrayList 可以使用默认的大小，当元素个数到达一定程度后，会自动扩容。

可以这么来理解：我们常说的数组是定死的数组，ArrayList 却是动态数组



### 35、说说什么是 fail - fast ？

fail - fast 机制是 Java 集合（Collection）中的一种错误机制。当多个线程对同一个集合的内容进行操作时，就可能会产生 fail - fast 事件。

例如：当某一个线程 A 通过 iterator 去遍历某集合的过程中，若该集合的内容被其他线程所改变了，那么线程 A 访问集合时，就会抛出 ConcurrentModificationException 异常，产生 fail - fast 事件。这里的操作主要是指 add、remove 和 clear，对集合元素个数进行修改。

解决办法：建议使用“ java.util.concurrent 包下的类” 去取代 “ java.util 包下的类”。

可以这么理解：在遍历之前，把 modCount 记下来 expectModCount，后面 expectModCount 去和 modCount 进行比较，如果不相等了，证明已并发了，被修改了，于是抛出 ConcurrentModificationException 异常。



### 36、HashMap 中的 key 我们可以使用任何类作为 key 吗？

平时可能大家使用的最多的就是使用 String 作为 HashMap 的 key，但是现在我们想使用某个自定义类作为 HashMap 的 key，那就需要注意以下几点：

- 如果类重写了 equals 方法，它也应该重写 hashCode 方法。

- 类的所有实例需要遵循与 equals 和 hashCode 相关的规则。

- 如果一个类没有使用 equals，你不应该在 hashCode 中使用它。

- 咱们自定义 key 类的最佳实践是使之为不可变的，这样，hashCode 值可以被缓存起来，拥有更好的性能。不可变的类也可以确保 hashCode 和 equals 在未来不会改变，这样就会解决与可变相关的问题了。



### 37、HashMap 的长度为什么是 2 的 N 次方呢？

为了能让 HashMap 存数据和取数据的效率高，尽可能地减少 hash 值的碰撞，也就是说尽量把数据能均匀的分配，每个链表或者红黑树长度尽量相等。

我们首先可能会想到 % 取模的操作来实现。

下面是回答的重点哟：

取余（%）操作中如果除数是 2 的幂次，则等价于与其除数减一的与（&）操作（也就是说 hash % length == hash & ( length - 1 ) 的前提是 length 是 2 的 n 次方）。并且，采用二进制位操作 & ，相对于 % 能够提高运算效率。

这就是为什么 HashMap 的长度需要 2 的 N 次方了



### 38、HashMap 与 ConcurrentHashMap 的异同

1. 都是 key - value 形式的存储数据；
2. HashMap 是线程不安全的，ConcurrentHashMap 是 JUC 下的线程安全的；
3. HashMap 底层数据结构是数组 + 链表（ JDK 1.8 之前）。JDK 1.8 之后是数组 + 链表 + 红黑树。当链表中元素个数达到 8 的时候，链表的查询速度不如红黑树快，链表会转为红黑树，红黑树查询速度快；

4. HashMap 初始数组大小为 16（默认），当出现扩容的时候，以 0.75 * 数组大小的方式进行扩容；

5. ConcurrentHashMap 在 JDK 1.8 之前是采用分段锁来现实的 Segment + HashEntry，Segment 数组大小默认是 16，2 的 n 次方；JDK 1.8 之后，采用 Node + CAS + Synchronized来保证并发安全进行实现。



### 39、红黑树有哪几个特征？

- 每个节点是黑色或红色

- 根节点是黑色

- 每个叶子节点都是黑色（指向空的叶子节点）

- 如果一个叶子节点是红色，那么其子节点必须都是黑色的

- 从一个节点到该节点的子孙节点的所有路径上包含相同数目的黑节点



### 40、**说说你平时是怎么处理** **Java** 异常的

try - catch - finally

- try 块负责监控可能出现异常的代码

- catch 块负责捕获可能出现的异常，并进行处理

- finally 块负责清理各种资源，不管是否出现异常都会执行

- 其中 try 块是必须的，catch 和 finally 至少存在一个标准异常处理流程

在开发过程中会使用到自定义异常，在通常情况下，程序很少会自己抛出异常，因为异常的类名通常也包含了该异常的有用信息，所以在选择抛出异常的时候，应该选择合适的异常类，从而可以明确地描述该异常情况，所以这时候往往都是自定义异常。

自定义异常通常是通过继承 java.lang.Exception 类，如果想自定义 Runtime 异常的话，可以继承 java.lang.RuntimeException 类，实现一个无参构造和一个带字符串参数的有参构造方法。

在业务代码里，可以针对性的使用自定义异常。比如说：该用户不具备某某权限、余额不足等





## JVM 篇

### 1、知识点汇总

JVM 是 Java 运行基础，面试时一定会遇到 JVM 的有关问题，内容相对集中，但对只是深度要求较高

![](https://dream-syz.github.io/2-1.png)

其中内存模型、类加载机制、GC 是重点方面。性能调优部分更偏向应用。重点突出实践能力，编译器优化和执行模式部分偏向于理论基础,重点掌握知识点。

需了解 **内存模型** 各部分作用，保存哪些数据。

**类加载** 双亲委派加载机制，常用加载器分别加载哪种类型的类。

**GC** 分代回收的思想和依据以及不同垃圾回收算法的回收思路和适合场景。

**性能调优** 常有 JVM 优化参数作用，参数调优的依据，常用的 JVM 分析工具能分析哪些问题以及使用方法。

**执行模式 ** 解释 / 编译 / 混合模式的优缺点，Java 7 提供的分层编译技术，JIT 即时编译技术，OSR 栈上替换，C1 / C2 编译器针对的场景，C2 针对的是 server 模式，优化更激进。新技术方面 Java 10 的 graal 编译器

**编译器优化** javac 的编译过程，ast 抽象语法树，编译器优化和运行器优化.



### 2、知识点详解

#### 1、JVM 内存模型：

线程独占：栈，本地方法栈，程序计数器 

线程共享：堆，方法区

#### 2、栈：

又称方法栈，线程私有的，线程执行方法是都会创建一个栈阵，用来存储局部变量表、操作栈、动态链接、方法出口等信息。调用方法时执行入栈，方法返回时执行出栈。

#### 3、本地方法栈

与栈类似，也是用来保存执行方法的信息。执行 Java 方法是使用栈，执行 Native 方法时使用本地方法栈。

#### 4、程序计数器

保存着当前线程执行的字节码位置，每个线程工作时都有独立的计数器，只为执行 Java 方法服务，执行 Native 方法时，程序计数器为空

#### 5、堆

JVM 内存管理最大的一块，它被线程共享，目的是存放对象的实例，几乎所有的对象实例都会放在这里，当堆没有可用空间时，会抛出OOM 异常。根据对象的存活周期不同，JVM 把对象进行分代管理，由垃圾回收器进行垃圾的回收管理

#### 6、方法区：

又称非堆区，用于存储已被虚拟机加载的类信息、常量、静态变量，即时编译器优化后的代码等数据。1.7 的永久代和 1.8 的元空间都是方法区的一种实现

#### 7、JVM 内存可见性

![](https://dream-syz.github.io/2-2.png)

JMM 是定义程序中变量的访问规则，线程对于变量的操作只能在自己的工作内存中进行,而不能直接对主内存操作。由于指令重排序，读写的顺序会被打乱，因此 JMM 需要提供原子性、可见性、有序性保证。

![](https://dream-syz.github.io/2-3.png)



### 3、说说类加载与卸载

#### **1、加载过程**

![image-20230814093140833](https://dream-syz.github.io/2-4.png)

其中 **验证**，**准备**，**解析** 合称链接

**加载** 通过类的完全限定名，查找此类字节码文件，利用字节码文件创建Class对象。

**验证** 确保 Class 文件符合当前虚拟机的要求，不会危害到虚拟机自身安全。

**准备** 进行内存分配，为 static 修饰的类变量分配内存，并设置初始值 ( 0 或 null )。不包含 final 修饰的静态变量，因为 final 变量在编译时分配。

**解析 **将常量池中的符号引用替换为直接引用的过程，直接引用为直接指向目标的指针或者相对偏移量等。

**初始化** 主要完成静态块执行以及静态变量的赋值，先初始化父类，再初始化当前类，只有对类主动使用时才会初始化。

触发条件包括，创建类的实例时，访问类的静态方法或静态变量的时候，使用 `Class.forName` 反射类的时候，或者某个子类初始化的时候。

Java 自带的加载器加载的类，在虚拟机的生命周期中是不会被卸载的，只有用户自定义的加载器加载的类才可以被卸

#### 2、加载机制 - 双亲委派模式

![image-20230814094411338](https://dream-syz.github.io/2-5.png)



双亲委派模式，即加载器加载类时先把请求委托给自己的父类加载器执行，直到顶层的启动类加载器。

父类加载器能够完成加载则成功返回，不能则子类加载器才自己尝试加载。

**优点：**

1. 避免类的重复加载
2. 避免 Java 的核心 API 被篡改

#### 3、分代回收

分代回收基于两个事实:大部分对象很快就不使用了,还有一部分不会立即无用,但也不会持续很长时间。 

| 堆分代 |                     |                     |                     |
| ------ | ------------------- | ------------------- | ------------------- |
| 年轻代 | Dden                | Survivor1           | Survivor2           |
| 老年代 | Tebured             | Tenured             | Tenured             |
| 永久代 | PremGen / MetaSpace | PremGen / MetaSpace | PremGen / MetaSpace |

年轻代 ——> 标记 - 复制 老年代 ——> 标记 - 清除

#### **4**、回收算法

##### *a*、*G1*算法

1.9 后默认的垃圾回收算法，特点保持高回收率的同时减少停顿。采用每次只清理一部分，而不是清理全部的增量式清理，以保证停顿时间不会过长

其取消了年轻代与老年代的物理划分，但仍属于分代收集器，算法将堆分为若干个逻辑区域 ( region ) ，一部分用作年轻代，一部分用作老年代，还有用来存储巨型对象的分区。

同 CMS 相同，会遍历所有对象，标记引用情况，清除对象后会对区域进行复制移动，以整合碎片空间。

年轻代回收：并行复制采用复制算法，并行收集，会 StopTheWorld。

老年代回收：会对年轻代一并回收

初始标记完成堆 root 对象的标记，会 StopTheWorld。并发标记 GC 线程和应用线程并发执行。 最终标记完成三色标记周期，会StopTheWorld。复制 / 清楚会优先对可回收空间加大的区域进行回收

##### *b*、*ZGC*算法

前面提供的高效垃圾回收算法，针对大堆内存设计，可以处理 TB 级别的堆，可以做到 10 ms 以下的回收停顿时间

**GC roots 标记 ——> 并发标记 ——> 清除 ——> 并发重定位 <—— root 重定位**

- 着色指针

- 读屏障

- 并发处理

- 基于 region

- 内存压缩 ( 整理 )

roots 标记：标记 root 对象，会发生 StopTheWorld.。并发标记：利用读屏障与应用线程一起运行标记，可能会发生 StopTheWorld。清除会清理标记为不可用的对象。 roots 重定位：是对存活的对象进行移动，以腾出大块内存空间，减少碎片产生。重定位最开始会StopTheWorld，取决于重定位集与对象总活动集的比例。并发重定位与并发标记类似



### 4、简述一下 JVM 的内存模型

#### 1.JVM 内存模型简介

JVM 定义了不同运行时数据区，他们是用来执行应用程序的。某些区域随着 JVM 启动及销毁，另外一些区域的数据是线程性独立的，随着线程创建和销毁。JVM 内存模型总体架构图如下：（ 摘自 oracle 官方网站 ）

![](https://dream-syz.github.io/2-6.png)

JVM 在执行 Java 程序时，会把它管理的内存划分为若干个的区域，每个区域都有自己的用途和创建销毁时间。如下图所示，可以分为两大部分，线程私有区和共享区。下图是根据自己理解画的一个 JVM 内存模型架构图

![](https://dream-syz.github.io/2-7.png)

JVM 内存分为线程私有区和线程共享区

##### **线程私有区**

###### 1、程序计数器

当同时进行的线程数超过 CPU 数或其内核数时，就要通过时间片轮询分派 CPU 的时间资源，不免发生线程切换。这时，每个线程就需要一个属于自己的计数器来记录下一条要运行的指令。如果执行的是 JAVA 方法，计数器记录正在执行的 java 字节码地址，如果执行的是native 方法，则计数器为空。

###### 2、虚拟机栈

线程私有的，与线程在同一时间创建。管理 JAVA 方法执行的内存模型。每个方法执行时都会创建一个桢栈来存储方法的的变量表、操作数栈、动态链接方法、返回值、返回地址等信息。栈的大小决定了方法调用的可达深度（ 递归多少层次，或嵌套调用多少层其他方法，   -Xss 参数可以设置虚拟机栈大小）。栈的大小可以是固定的，或者是动态扩展的。如果请求的栈深度大于最大可用深度，则抛出stackOverflowError；如果栈是可动态扩展的，但没有内存空间支持扩展，则抛出 OutofMemoryError。 使用 jclasslib 工具可以查看class 类文件的结构。下图为栈帧结构图：

![](https://dream-syz.github.io/2-8.png)

###### 3、本地方法栈

与虚拟机栈作用相似。但它不是为 Java 方法服务的，而是本地方法（ C 语言）。由于规范对这块没有强制要求，不同虚拟机实现方法不同。

##### **线程共享区**

###### 1、方法区

线程共享的，用于存放被虚拟机加载的类的元数据信息，如常量、静态变量和即时编译器编译后的代码。若要分代，算是永久代（老年代），以前类大多 `static` 的，很少被卸载或收集，现回收废弃常量和无用的类。其中运行时常量池存放编译生成的各种常量。（如果hotspot 虚拟机确定一个类的定义信息不会被使用，也会将其回收。回收的基本条件至少有：所有该类的实例被回收，而且装载该类的ClassLoader 被回收）

###### 2、堆

存放对象实例和数组，是垃圾回收的主要区域，分为新生代和老年代。刚创建的对象在新生代的 Eden 区中，经过 GC 后进入新生代的 S0 区中，再经过 GC 进入新生代的 S1 区中，15 次 GC 后仍存在就进入老年代。这是按照一种回收机制进行划分的，不是固定的。若堆的空间不够实例分配，则 OutOfMemoryError。

![](https://dream-syz.github.io/2-9.png)

```
Young Generation 即图中的 Eden + From Space（s0） + To Space(s1)
Eden 存放新生的对象
Survivor Space 有两个，存放每次垃圾回收后存活的对象(s0+s1)
Old Generation Tenured Generation 即图中的Old Space
 主要存放应用程序中生命周期长的存活对象
```



### 5、说说堆和栈的区别

栈是运行时单位，代表着逻辑，内含基本数据类型和堆中对象引用，所在区域连续，没有碎片；堆是存储单位，代表着数据，可被多个栈共享（包括成员中基本数据类型、引用和引用对象），所在区域不连续，会有碎片。

#### 1、功能不同

栈内存用来存储 **局部变量** 和 **方法调用**，而堆内存用来存储 Java 中的对象。无论是成员变量，局部变量，还是类变量，它们指向的对象都存储在堆内存中。

#### 2、共享性不同

栈内存是线程私有的。 堆内存是所有线程共有的。

#### 3、异常错误不同

如果栈内存或者堆内存不足都会抛出异常。 

栈空间不足：java.lang.StackOverFlowError。 

堆空间不足：java.lang.OutOfMemoryError。

#### 4、空间大小

栈的空间大小远远小于堆的。



### 6、什么时候会触发 Full GC

除直接调用 System.gc 外，触发 Full GC 执行的情况有如下四种。

#### 1、旧生代空间不足 

旧生代空间只有在新生代对象转入及创建为大对象、大数组时才会出现不足的现象，当执行 Full GC 后空间仍然不足，则抛出如下错误： `java.lang.OutOfMemoryError: Java heap space` 为避免以上两种状况引起的 Full GC，调优时应尽量做到让对象在 Minor GC 阶段被回收、让对象在新生代多存活一段时间及不要创建过大的对象及数组。

#### 2、Permanet Generation 空间满

Permanet Generation中 存放的为一些 class 的信息等，当系统中要加载的类、反射的类和调用的方法较多时，Permanet Generation 可能会被占满，在未配置为采用 CMS GC 的情况下会执行 Full GC。如果经过 Full GC 仍然回收不了，那么 JVM 会抛出如下错误信息： `java.lang.OutOfMemoryError: PermGen space` 为避免 Perm Gen 占满造成 Full GC 现象，可采用的方法为增大 Perm Gen 空间或转为使用 CMS GC。

#### 3、CMS GC 时出现 promotion failed 和 concurrent mode failure 

对于采用 CMS 进行旧生代 GC 的程序而言，尤其要注意 GC 日志中是否有 promotion failed 和 concurrent mode failure 两种状况，当这两种状况出现时可能会触发 Full GC。 promotion failed 是在进行 Minor GC 时，survivor space 放不下、对象只能放入旧生代，而此时旧生代也放不下造成的；concurrent mode failure 是在执行 CMS GC 的过程中同时有对象要放入旧生代，而此时旧生代空间不足造成的。 应对措施为：增大 survivor space、旧生代空间或调低触发并发 GC 的比率，但在 JDK 5.0 +、6.0 + 的版本中有可能会由于 JDK 的bug29 导致 CMS 在 remark 完毕后很久才触发 sweeping 动作。对于这种状况，可通过设置 -XX:CMSMaxAbortablePrecleanTime=5（单位为ms）来避免。

#### 4、统计得到的 Minor GC 晋升到旧生代的平均大小大于旧生代的剩余空间

这是一个较为复杂的触发情况，Hotspot 为了避免由于新生代对象晋升到旧生代导致旧生代空间不足的现象，在进行 Minor GC 时，做了一个判断，如果之前统计所得到的 Minor GC 晋升到旧生代的平均大小大于旧生代的剩余空间，那么就直接触发 Full GC。 例如程序第一次触发 Minor GC 后，有 6 MB 的对象晋升到旧生代，那么当下一次 Minor GC 发生时，首先检查旧生代的剩余空间是否大于 6 MB，如果小于 6 MB，则执行 Full GC。 当新生代采用 PS GC 时，方式稍有不同，PS GC 是在 Minor GC 后也会检查，例如上面的例子中第一次 Minor GC 后，PS GC 会检查此时旧生代的剩余空间是否大于 6 MB，如小于，则触发对旧生代的回收。 除了以上4种状况外，对于使用RMI来进行RPC或管理的Sun JDK应用而言，默认情况下会一小时执行一次Full GC。可通过在启动时通过

`-java Dsun.rmi.dgc.client.gcInterval=3600000`来设置Full GC执行的间隔时间或通过

`-XX:+DisableExplicitGC`来禁止 RMI 调用 System.gc



### 7、什么是 Java 虚拟机？为什么 Java 被称作是“平台无关的编程语言“？

Java 虚拟机是一个可以执行 Java 字节码的虚拟机进程。Java 源文件被编译成能被 Java 虚拟机执行的字节码文件。 Java 被设计成允许应用程序可以运行在任意的平台，而不需要程序员为每一个平台单独重写或者是重新编译。Java 虚拟机让这个变为可能，因为它知道底层硬件平台的指令长度和其他特性。



### 8、Java 内存结构

![](https://dream-syz.github.io/2-10.png)

方法区和堆是所有线程共享的内存区域；而 java 栈、本地方法栈和程序员计数器是运行是线程私有的内存区域。

- Java 堆（ Heap ）是 Java 虚拟机所管理的内存中最大的一块。Java 堆是被所有线程共享的一块内存区域，在虚拟机启动时创建。此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例都在这里分配内存。

- 方法区（ Method Area ）与 Java 堆一样，是各个线程共享的内存区域，它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。

- 程序计数器（ Program Counter Register ）是一块较小的内存空间，它的作用可以看做是当前线程所执行的字节码的行号指示器。

- JVM 栈（JVM Stacks）与程序计数器一样，Java 虚拟机栈（ Java Virtual Machine Stacks ）也是线程私有的，它的生命周期与线程相同。虚拟机栈描述的是 Java 方法执行的内存模型：每个方法被执行的时候都会同时创建一个栈帧（ Stack Frame ）用于存储局部变量表、操作栈、动态链接、方法出口等信息。每一个方法被调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中从入栈到出栈的过程。

- 本地方法栈（Native Method Stacks）与虚拟机栈所发挥的作用是非常相似的，其区别不过是虚拟机栈为虚拟机执行 Java 方法（也就是字节码）服务，而本地方法栈则是为虚拟机使用到的 Native 方法服务。



### 9、说说对象分配规则

- 对象优先分配在 Eden 区，如果 Eden 区没有足够的空间时，虚拟机执行一次 Minor GC。

- 大对象直接进入老年代（大对象是指需要大量连续内存空间的对象）。这样做的目的是避免在Eden区和两个 Survivor 区之间发生大量的内存拷贝（新生代采用复制算法收集内存）。

- 长期存活的对象进入老年代。虚拟机为每个对象定义了一个年龄计数器，如果对象经过了 1 次 Minor GC 那么对象会进入 Survivor区，之后每经过一次 Minor GC 那么对象的年龄加 1，知道达到阀值对象进入老年区。

- 动态判断对象的年龄。如果 Survivor 区中相同年龄的所有对象大小的总和大于 Survivor 空间的一半，年龄大于或等于该年龄的对象可以直接进入老年代。

- 空间分配担保。每次进行 Minor GC 时，JVM 会计算 Survivor 区移至老年区的对象的平均大小，如果这个值大于老年区的剩余值大小则进行一次 Full GC，如果小于检查 HandlePromotionFailure 设置，如果 true 则只进行 Monitor GC，如果 false 则进行 Full GC。



### 10、描述一下 JVM 加载 class 文件的原理机制？

JVM 中类的装载是由类加载器（ ClassLoader ）和它的子类来实现的，Java 中的类加载器是一个重要的 Java 运行时系统组件，它负责在运行时查找和装入类文件中的类。 由于 Java 的跨平台性，经过编译的 Java 源程序并不是一个可执行程序，而是一个或多个类文件。当Java 程序需要使用某个类时，JVM 会确保这个类已经被 **加载、连接（验证、准备和解析）和初始化**。类的加载是指把类的 `.class` 文件中的数据读入到内存中，通常是创建一个字节数组读入 `.class` 文件，然后产生与所加载类对应的 `Class` 对象。加载完成后，`Class` 对象还不完整，所以此时的类还不可用。当类被加载后就进入连接阶段，这一阶段包括 **验证、准备（为静态变量分配内存并设置默认的初始值）和解析（将符号引用替换为直接引用）**三个步骤。

最后JVM对类进行初始化，包括：

1) 如果类存在直接的父类并且这个类还没有被初始化，那么就先初始化父类；

2) 如果类中存在初始化语句，就依次执行这些初始化语句。 类的加载是由类加载器完成的，类加载器包括：根加载器（BootStrap）、扩展加载器（Extension）、系统加载器（System）和用户自定义类加载器（java.lang.ClassLoader的子类）。从 Java 2（JDK 1.2）开始，类加载过程采取了父亲委托机制（PDM）。PDM 更好的保证了 Java 平台的安全性，在该机制中，JVM 自带的 Bootstrap 是根加载器，其他的加载器都有且仅有一个父类加载器。类的加载首先请求父类加载器加载，父类加载器无能为力时才由其子类加载器自行加载。JVM 不会向 Java 程序提供对 Bootstrap 的引用。下面是关于几个类加载器的说明：

   - Bootstrap：一般用本地代码实现，负责加载 JVM 基础核心类库（rt.jar）；

   - Extension：从 `java.ext.dirs` 系统属性所指定的目录中加载类库，它的父加载器是 Bootstrap；

   - System：又叫应用类加载器，其父类是 Extension。它是应用最广泛的类加载器。它从环境变量 classpath 或者系统属性java.class.path 所指定的目录中记载类，是用户自定义加载器的默认父加载器。



### 11、说说 Java 对象创建过程

1. JVM 遇到一条新建对象的指令时首先去检查这个指令的参数是否能在常量池中定义到一个类的符号引用。然后加载这个类（类加载过程在后边讲）

2. 为对象分配内存。一种办法 “指针碰撞” 、一种办法 “空闲列表” ，最终常用的办法 “本地线程缓冲分配(TLAB)”

3. 将除对象头外的对象内存空间初始化为 0

4. 对对象头进行必要设置



### 12、知道类的生命周期吗？

类的生命周期包括这几个部分，加载、连接、初始化、使用和卸载，其中前三部是类的加载的过程，如下图；

![](https://dream-syz.github.io/2-11.png)

- 加载，查找并加载类的二进制数据，在 Java 堆中也创建一个 `java.lang.Class` 类的对象

- 连接，连接又包含三块内容：验证、准备、初始化。 1）验证，文件格式、元数据、字节码、符号引用验证； 2）准备，为类的静态变量分配内存，并将其初始化为默认值； 3）解析，把类中的符号引用转换为直接引用

- 初始化，为类的静态变量赋予正确的初始值

- 使用，new 出对象程序中使用

- 卸载，执行垃圾回收



### 13、简述 Java 的对象结构

Java 对象由三个部分组成：对象头、实例数据、对齐填充。

- 对象头由两部分组成，第一部分存储对象自身的运行时数据：哈希码、GC 分代年龄、锁标识状态、线程持有的锁、偏向线程 ID（一般占 32 / 64 bit ）。第二部分是指针类型，指向对象的类元数据类型（即对象代表哪个类）。如果是数组对象，则对象头中还有一部分用来记录数组长度。

- 实例数据用来存储对象真正的有效信息（包括父类继承下来的和自己定义的）

- 对齐填充：JVM 要求对象起始地址必须是 8 字节的整数倍（8 字节对齐）



### 14、如何判断对象可以被回收？

判断对象是否存活一般有两种方式：

引用计数：每个对象有一个引用计数属性，新增一个引用时计数加 1，引用释放时计数减 1，计数为 0 时可以回收。此方法简单，无法解决对象相互循环引用的问题。

可达性分析（Reachability Analysis）：从 GC Roots 开始向下搜索，搜索所走过的路径称为引用链。当一个对象到 GC Roots 没有任何引用链相连时，则证明此对象是不可用的，不可达对象。



### 15、JVM 的永久代中会发生垃圾回收么？

垃圾回收不会发生在永久代，如果永久代满了或者是超过了临界值，会触发完全垃圾回收（Full GC）。如果你仔细查看垃圾收集器的输出信息，就会发现永久代也是被回收的。这就是为什么正确的永久代大小对避免Full GC是非常重要的原因。请参考下 Java 8：从永久代到元数据区（注：Java 8 中已经移除了永久代，新加了一个叫做元数据区的 native 内存区）



### 16、你知道哪些垃圾收集算法

GC 最基础的算法有三种： 标记 - 清除算法、复制算法、标记 - 压缩算法，我们常用的垃圾回收器一般都采用分代收集算法。

- 标记 - 清除算法，“标记 - 清除”（ Mark - Sweep ）算法，如它的名字一样，算法分为“标记”和“清除”两个阶段：首先标记出所有需要回收的对象，在标记完成后统一回收掉所有被标记的对象。

- 复制算法，“复制”（ Copying ）的收集算法，它将可用内存按容量划分为大小相等的两块，每次只使用其中的一块。当这一块的内存用完了，就将还存活着的对象复制到另外一块上面，然后再把已使用过的内存空间一次清理掉。

- 标记 - 压缩算法，标记过程仍然与“标记 - 清除”算法一样，但后续步骤不是直接对可回收对象进行清理，而是让所有存活的对象都向一端移动，然后直接清理掉端边界以外的内存

- 分代收集算法，“分代收集”（ Generational Collection ）算法，把 Java 堆分为新生代和老年代，这样就可以根据各个年代的特点采用最适当的收集算法。



### 17、调优命令有哪些？

Sun JDK 监控和故障处理命令有 jps jstat jmap jhat jstack jinfo

- jps，JVM Process Status Tool，显示指定系统内所有的 HotSpot 虚拟机进程。

- jstat，JVM statistics Monitoring 是用于监视虚拟机运行时状态信息的命令，它可以显示出虚拟机进程中的类装载、内存、垃圾收集、JIT 编译等运行数据。

- jmap，JVM Memory Map 命令用于生成 heap dump 文件

- jhat，JVM Heap Analysis Tool 命令是与 jmap 搭配使用，用来分析 jmap 生成的 dump，jhat 内置了一个微型的 HTTP / HTML 服务器，生成 dump 的分析结果后，可以在浏览器中查看

- jstack，用于生成 java 虚拟机当前时刻的线程快照。

- jinfo，JVM Configuration info 这个命令作用是实时查看和调整虚拟机运行参数。



### 18、常见调优工具有哪些

常用调优工具分为两类，jdk 自带监控工具：jconsole 和 jvisualvm，第三方有：MAT（Memory Analyzer Tool）、GChisto。

jconsole，Java Monitoring and Management Console 是从 java5 开始，在 JDK 中自带的 java 监控和管理控制台，用于对 JVM 中内存，线程和类等的监控

jvisualvm，jdk 自带全能工具，可以分析内存快照、线程快照；监控内存变化、GC 变化等。

MAT，Memory Analyzer Tool，一个基于 Eclipse 的内存分析工具，是一个快速、功能丰富的 Java heap 分析工具，它可以帮助我们查找内存泄漏和减少内存消耗

GChisto，一款专业分析 gc 日志的工具



### 19、Minor GC 与 Full GC 分别在什么时候发生？

新生代内存不够用时候发生 MGC 也叫 YGC，JVM 内存不够的时候发生 FGC



### 20、你知道哪些 JVM 性能调优参数？

**简单回答：**

- 设定堆内存大小

	   -Xmx 堆内存最大限制。

- 设定新生代大小。 新生代不宜太小，否则会有大量对象涌入老年代

		-XX:NewSize 新生代大小
		
		-XX:NewRatio 新生代和老生代占比
		
		-XX:SurvivorRatio 伊甸园空间和幸存者空间的占比

- 设定垃圾回收器 年轻代用 -XX:+UseParNewGC 年老代用 -XX:+UseConcMarkSweepGC

**具体回答：**

#### **「堆栈内存相关」**

-Xms 设置初始堆的大小

-Xmx 设置最大堆的大小

-Xmn 设置年轻代大小，相当于同时配置 -XX:NewSize 和 -XX:MaxNewSize 为一样的值

-Xss 每个线程的堆栈大小

-XX:NewSize 设置年轻代大小（ for 1.3 / 1.4 ）

-XX:MaxNewSize 年轻代最大值（ for 1.3 / 1.4 ）

-XX:NewRatio 年轻代与年老代的比值（除去持久代）

-XX:SurvivorRatio Eden 区与 Survivor 区的的比值

-XX:PretenureSizeThreshold 当创建的对象超过指定大小时，直接把对象分配在老年代。

-XX:MaxTenuringThreshold 设定对象在 Survivor 复制的最大年龄阈值，超过阈值转移到老年代

#### **「垃圾收集器相关」**

-XX:+UseParallelGC 选择垃圾收集器为并行收集器。

-XX:ParallelGCThreads=20 配置并行收集器的线程数

-XX:+UseConcMarkSweepGC 设置年老代为并发收集。

-XX:CMSFullGCsBeforeCompaction=5 由于并发收集器不对内存空间进行压缩、整理，所以运行一段时间以后会产生“碎片”，使得运行效率降低。此值设置运行 5 次 GC 以后对内存空间进行压缩、整理。

-XX:+UseCMSCompactAtFullCollection 打开对年老代的压缩。可能会影响性能，但是可以消除碎片

#### **「辅助信息相关」**

-XX:+PrintGCDetails 打印 GC 详细信息

-XX:+HeapDumpOnOutOfMemoryError 让 JVM 在发生内存溢出的时候自动生成内存快照，排查问题用

-XX:+DisableExplicitGC 禁止系统 System.gc()，防止手动误触发 FGC 造成问题.

-XX:+PrintTLAB 查看 TLAB 空间的使用情况



### 21、 对象一定分配在堆中吗？有没有了解逃逸分析技术？

**「对象一定分配在堆中吗？」** 不一定的，JVM 通过**「逃逸分析」**，那些逃不出方法的对象会在栈上分配。

**「什么是逃逸分析？」**

逃逸分析（ Escape Analysis ），是一种可以有效减少 Java 程序中同步负载和内存堆分配压力的跨函数全局数据流分析算法。通过逃逸分析，Java Hotspot 编译器能够分析出一个新的对象的引用的使用范围，从而决定是否要将这个对象分配到堆上。

**逃逸分析** 是指分析指针动态范围的方法，它同编译器优化原理的指针分析和外形分析相关联。当变量（或者对象）在方法中分配后，其指针有可能被返回或者被全局引用，这样就会被其他方法或者线程所引用，这种现象称作指针（或者引用）的逃逸（ Escape ）。通俗点讲，如果一个对象的指针被多个方法或者线程引用时，那么我们就称这个对象的指针发生了逃逸。

**「逃逸分析的好处」**

- 栈上分配，可以降低垃圾收集器运行的频率。

- 同步消除，如果发现某个对象只能从一个线程可访问，那么在这个对象上的操作可以不需要同步。

- 标量替换，把对象分解成一个个基本类型，并且内存分配不再是分配在堆上，而是分配在栈上。这样的好处有，一、减少内存使用，因为不用生成对象头。二、程序内存回收效率高，并且 GC 频率也会减少。



### 22、虚拟机为什么使用元空间替换了永久代？

**「什么是元空间？什么是永久代？为什么用元空间代替永久代？」** 我们先回顾一下**「方法区」**吧,看看虚拟机运行时数据内存图，如下:

![](https://dream-syz.github.io/2-12.png)

方法区和堆一样，是各个线程共享的内存区域，它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译后的代码等数据。

**「什么是永久代？它和方法区有什么关系呢？」**

如果在 HotSpot 虚拟机上开发、部署，很多程序员都把方法区称作永久代。可以说方法区是规范，永久代是 Hotspot 针对该规范进行的实现。在 Java7 及以前的版本，方法区都是永久代实现的。

**「什么是元空间？它和方法区有什么关系呢？」**

对于 Java8，HotSpots 取消了永久代，取而代之的是元空间（Metaspace）。换句话说，就是方法区还是在的，只是实现变了，从永久代变为元空间了。

**「为什么使用元空间替换了永久代？」**

- 永久代的方法区，和堆使用的物理内存是连续的。

![](https://dream-syz.github.io/2-13.png)

**「永久代」**是通过以下这两个参数配置大小的~

- -XX:PremSize：设置永久代的初始大小

- -XX:MaxPermSize: 设置永久代的最大值，默认是 64 M

对于**「永久代」**，如果动态生成很多 class 的话，就很可能出现**「 java.lang.OutOfMemoryError: PermGen space 错误」**，因为永久代空间配置有限嘛。最典型的场景是，在 web 开发比较多 jsp 页面的时候。

- JDK 8 之后，方法区存在于元空间（ Metaspace）。物理内存不再与堆连续，而是直接存在于本地内存中，理论上机器**「内存有多大，元空间就有多大」**。

![](https://dream-syz.github.io/2-14.png)

可以通过以下的参数来设置元空间的大小：

- -XX:MetaspaceSize，初始空间大小，达到该值就会触发垃圾收集进行类型卸载，同时 GC 会对该值进行调整：如果释放了大量的空间，就适当降低该值；如果释放了很少的空间，那么在不超过 MaxMetaspaceSize 时，适当提高该值。

- -XX:MaxMetaspaceSize，最大空间，默认是没有限制的。

- -XX:MinMetaspaceFreeRatio，在 GC 之后，最小的 Metaspace 剩余空间容量的百分比，减少为分配空间所导致的垃圾收集

- -XX:MaxMetaspaceFreeRatio，在 GC 之后，最大的 Metaspace 剩余空间容量的百分比，减少为释放空间所导致的垃圾收集

**「所以，为什么使用元空间替换永久代？」**

表面上看是为了避免 OOM 异常。因为通常使用 PermSize 和 MaxPermSize 设置永久代的大小就决定了永久代的上限，但是不是总能知道应该设置为多大合适, 如果使用默认值很容易遇到 OOM 错误。当使用元空间时，可以加载多少类的元数据就不再由 MaxPermSize 控制, 而由系统的实际可用空间来控制啦。



### 23、什么是 Stop The World ？什么是 OopMap ？什么是安全点？

进行垃圾回收的过程中，会涉及对象的移动。为了保证对象引用更新的正确性，必须暂停所有的用户线程，像这样的停顿，虚拟机设计者形象描述为**「 Stop The World 」**。也简称为 STW。

在 HotSpot 中，有个数据结构（映射表）称为**「 OopMap 」**。一旦类加载动作完成的时候，HotSpot 就会把对象内什么偏移量上是什么类型的数据计算出来，记录到 OopMap。在即时编译过程中，也会在**「特定的位置」**生成 OopMap，记录下栈上和寄存器里哪些位置是引用。

这些特定的位置主要在：

1.循环的末尾（非 counted 循环）

2.方法临返回前 / 调用方法的 call 指令后

3.可能抛异常的位置

这些位置就叫作**「安全点（ safepoint）」** 用户程序执行时并非在代码指令流的任意位置都能够在停顿下来开始垃圾收集，而是必须是执行到安全点才能够暂停。



### 24、说一下 JVM 的主要组成部分及其作用？

![](https://dream-syz.github.io/2-15.png)

JVM 包含两个子系统和两个组件，分别为

- Class loader（类装载子系统）

- Execution engine（执行引擎子系统）

- Runtime data area（运行时数据区组件）

- Native Interface（本地接口组件）

- **「 Class loader（类装载）」**： 根据给定的全限定名类名（如：java.lang.Object）来装载 class 文件到运行时数据区的方法区中。

- **「 Execution engine（执行引擎）」**：执行 class 的指令。

- **「 Native Interface（本地接口）」**： 与 native lib 交互，是其它编程语言交互的接口。

- **「 Runtime data area（运行时数据区域）」**：即我们常说的 JVM 的内存。

首先通过编译器把 Java 源代码转换成字节码，Class loader（类装载）再把字节码加载到内存中，将其放在运行时数据区的方法区内，而字节码文件只是 JVM 的一套指令集规范，并不能直接交给底层操作系统去执行，因此需要特定的命令解析器执行引擎（Execution Engine），将字节码翻译成底层系统指令，再交由 CPU 去执行，而这个过程中需要调用其他语言的本地库接口（Native Interface）来实现整个程序的功能。



### 25、什么是指针碰撞？

一般情况下，JVM 的对象都放在堆内存中（发生逃逸分析除外）。当类加载检查通过后，Java 虚拟机开始为新生对象分配内存。如果 Java 堆中内存是绝对规整的，所有被使用过的的内存都被放到一边，空闲的内存放到另外一边，中间放着一个指针作为分界点的指示器，所分配内存仅仅是把那个指针向空闲空间方向挪动一段与对象大小相等的实例，这种分配方式就是 **指针碰撞**。

![image-20230816094955691](https://dream-syz.github.io/2-16.png)



### 26、什么是空闲列表？

如果 Java 堆内存中的内存并不是规整的，已被使用的内存和空闲的内存相互交错在一起，不可以进行指针碰撞啦，虚拟机必须维护一个列表，记录哪些内存是可用的，在分配的时候从列表找到一块大的空间分配给对象实例，并更新列表上的记录，这种分配方式就是空闲列表。



### 27、什么是 TLAB ？

可以把内存分配的动作按照线程划分在不同的空间之中进行，每个线程在 Java 堆中预先分配一小块内存，这就是 TLAB（Thread Local Allocation Buffer，本地线程分配缓存） 。虚拟机通过 -XX:UseTLAB 设定它的。



### 28、对象头具体都包含哪些内容？

在我们常用的 Hotspot 虚拟机中，对象在内存中布局实际包含 3 个部分：

1. 对象头

2. 实例数据
3. 对齐填充

而对象头包含两部分内容，Mark Word 中的内容会随着锁标志位而发生变化，所以只说存储结构就好了。

1. 对象自身运行时所需的数据，也被称为 Mark Word，也就是用于轻量级锁和偏向锁的关键点。具体的内容包含对象的 hashcode、分代年龄、轻量级锁指针、重量级锁指针、GC 标记、偏向锁线程 ID、偏向锁时间戳。

2. 存储类型指针，也就是指向类的元数据的指针，通过这个指针才能确定对象是属于哪个类的实例。

如果是数组的话，则还包含了数组的长度。

![](https://dream-syz.github.io/2-17.png)



### 29、说一下 JVM 有哪些垃圾回收器？

如果说垃圾收集算法是内存回收的方法论，那么垃圾收集器就是内存回收的具体实现。下图展示了 7 种作用于不同分代的收集器，其中用于回收新生代的收集器包括 Serial、PraNew、Parallel Scavenge，回收老年代的收集器包括 Serial Old、Parallel Old、CMS，还有用于回收整个 Java 堆的 G1收集器。不同收集器之间的连线表示它们可以搭配使用。

![](https://dream-syz.github.io/2-18.png)

- Serial 收集器（复制算法）：新生代单线程收集器，标记和清理都是单线程，优点是简单高效；

- ParNew 收集器 （复制算法） ：新生代收并行集器，实际上是 Serial 收集器的多线程版本，在多核 CPU 环境下有着比 Serial 更好的表现；

- Parallel Scavenge 收集器（复制算法）：新生代并行收集器，追求高吞吐量，高效利用 CPU。吞吐量 = 用户线程时间 /（用户线程时间 + GC 线程时间），高吞吐量可以高效率的利用 CPU 时间，尽快完成程序的运算任务，适合后台应用等对交互相应要求不高的场景；

- Serial Old 收集器 （标记 - 整理算法）： 老年代单线程收集器，Serial 收集器的老年代版本；

- Parallel Old 收集器（标记 - 整理算法）： 老年代并行收集器，吞吐量优先，Parallel Scavenge 收集器的老年代版本；

- CMS（ Concurrent Mark Sweep ）收集器（标记 - 清除算法）： 老年代并行收集器，以获取最短回收停顿时间为目标的收集器，具有高并发、低停顿的特点，追求最短 GC 回收停顿时间。

- G1（ Garbage First ）收集器（标记 - 整理算法）： Java 堆并行收集器，G1 收集器是 JDK 1.7 提供的一个新收集器，G1 收集器基于“标记 - 整理”算法实现，也就是说不会产生内存碎片。此外，G1 收集器不同于之前的收集器的一个重要特点是：G1 回收的范围是整个 Java堆（包括新生代，老年代），而前六种收集器回收的范围仅限于新生代或老年代。

- ZGC （ Z Garbage Collector ）是一款由 Oracle 公司研发的，以低延迟为首要目标的一款垃圾收集器。它是基于动态 Region 内存布局，（暂时）不设年龄分代，使用了读屏障、染色指针和内存多重映射等技术来实现可并发的标记 - 整理算法的收集器。在  JDK 11 新加入，还在实验阶段，主要特点是：回收 TB 级内存（最大 4 T ），停顿时间不超过 10 ms。**优点**：低停顿，高吞吐量， ZGC 收集过程中额外耗费的内存小。**缺点**：浮动垃圾目前使用的非常少，真正普及还是需要写时间的。

**新生代收集器**：Serial、 ParNew 、 Parallel Scavenge

**老年代收集器**：CMS 、Serial Old、Parallel Old

**整堆收集器**：G1 ，ZGC （因为不涉年代不在图中）



### 30、如何选择垃圾收集器？

1. 如果你的堆大小不是很大（比如 100 MB ），选择串行收集器一般是效率最高的。

   参数： -XX:+UseSerialGC 。

2. 如果你的应用运行在单核的机器上，或者你的虚拟机核数只有单核，选择串行收集器依然是合适的，这时候启用一些并行收集器没有任何收益。

   参数： -XX:+UseSerialGC 。

3. 如果你的应用是“吞吐量”优先的，并且对较长时间的停顿没有什么特别的要求。选择并行收集器是比较好的。

   参数： -XX:+UseParallelGC 。

4. 如果你的应用对响应时间要求较高，想要较少的停顿。甚至 1 秒的停顿都会引起大量的请求失败，那么选择 G1 、ZGC 、CMS 都是合理的。虽然这些收集器的 GC 停顿通常都比较短，但它需要一些额外的资源去处理这些工作，通常吞吐量会低一些。

   参数：

   -XX:+UseConcMarkSweepGC 、

   -XX:+UseG1GC 、

   -XX:+UseZGC 等。

从上面这些出发点来看，我们平常的 Web 服务器，都是对响应性要求非常高的。选择性其实就集中在 CMS 、 G1 、 ZGC 上。而对于某些定时任务，使用并行收集器，是一个比较好的选择。



### 31、 什么是类加载器？

类加载器是一个用来加载类文件的类。Java 源代码通过 javac 编译器编译成类文件。然后 JVM 来执行类文件中的字节码来执行程序。类加载器负责加载文件系统、网络或其他来源的类文件。



### 32、什么是 tomcat 类加载机制？

在 tomcat 中类的加载稍有不同，如下图：

![](https://dream-syz.github.io/2-19.png)

当 tomcat 启动时，会创建几种类加载器： **Bootstrap** **引导类加载器** 加载 JVM 启动所需的类，以及标准扩展类（位于 `jre/lib/ext` 下） **System** **系统类加载器** 加载 tomcat 启动的类，比如 bootstrap.jar，通常在 catalina.bat 或者 `catalina.sh` 中指定。位于 `CATALINA_HOME/bin` 下。

![](https://dream-syz.github.io/2-20.png)

**Common** **通用类加载器**



## 多线程 & 并发篇

### 1、说说 Java 中实现多线程有几种方法

**剖其底层 Runnable** 

创建线程的常用四种方式：

1. 继承 Thread 类，重写 run 方法
2. 实现 Runnable 接口，重写 run 方法
3. 实现 Callable 接口（  JDK 1.5 >= ），重写 call 方法，配合 FutureTask（有返回结果的非阻塞执行）
4. 线程池方式创建

lambda 方式

```
Thread t2 = new Thread(()->{
	for (int i = 0; i < 100; i++) {
		system.out.println( "lambda: " +i);
	}
});
```

通过继承 Thread 类或者实现 Runnable 接口、Callable 接口都可以实现多线程，不过实现 Runnable 接口与实现 Callable 接口的方式基本相同，只是 Callable 接口里定义的方法返回值，可以声明抛出异常而已。因此将实现 Runnable 接口和实现 Callable 接口归为一种方式。这种方式与继承 Thread 方式之间的主要差别如下。

**采用实现 Runnable、Callable 接口的方式创建线程的优缺点**

**优点**：线程类只是实现了 Runnable 或者 Callable 接口，还可以继承其他类。这种方式下，多个线程可以共享一个 target 对象，所以非常适合多个相同线程来处理同一份资源的情况，从而可以将 CPU、代码和数据分开，形成清晰的模型，较好的体现了面向对象的思想。

**缺点**：编程稍微复杂一些，如果需要访问当前线程，则必须使用 `Thread.currentThread()` 方法

**采用继承 Thread 类的方式创建线程的优缺点**

**优点**：编写简单，如果需要访问当前线程，则无需使用 `Thread.currentThread()` 方法，直接使用 this 即可获取当前线程

**缺点**：因为线程类已经继承了 Thread 类，Java 语言是单继承的，所以就不能再继承其他父类了。



### 2、如何停止一个正在运行的线程

1、使用退出标志，使线程正常退出，也就是当 run 方法完成后线程终止。

2、使用 stop 方法强行终止，但是不推荐这个方法，因为 stop 和 suspend 及 resume 一样都是过期作废的方法。

3、使用 interrupt 方法中断线程。

```java
class MyThread extends Thread {
 	volatile boolean stop = false;
 	public void run() {
      while (!stop) {
 		System.out.println(getName() + " is running");
 		try {
 			sleep(1000);
 		} catch (InterruptedException e) {
 			System.out.println("week up from blcok...");
 			stop = true; // 在异常处理代码中修改共享变量的状态
 		}
	  }
      System.out.println(getName() + " is exiting...");
 	}
}

class InterruptThreadDemo3 {
  public static void main(String[] args) throws InterruptedException {
    MyThread m1 = new MyThread();
 	System.out.println("Starting thread...");
 	m1.start();
 	Thread.sleep(3000);
 	System.out.println("Interrupt thread...: " + m1.getName());
 	m1.stop = true; // 设置共享变量为true
 	m1.interrupt(); // 阻塞时退出阻塞状态
 	Thread.sleep(3000); // 主线程休眠3秒以便观察线程m1的中断情况
 	System.out.println("Stopping application...");
  }
}
```



### 3、notify() 和 notifyAll() 有什么区别？

notify 可能会导致死锁，而 notifyAll 则不会

任何时候只有一个线程可以获得锁，也就是说只有一个线程可以运行 synchronized 中的代码使用 notifyAll，可以唤醒所有处于 wait 状态的线程，使其重新进入锁的争夺队列中，而 notify 只能唤醒一个。

wait() 应配合 while 循环使用，不应使用 if，务必在 wait() 调用前后都检查条件，如果不满足，必须调用 notify() 唤醒另外的线程来处理，自己继续 wait() 直至条件满足再往下执行。

notify() 是对 notifyAll() 的一个优化，但它有很精确的应用场景，并且要求正确使用。不然可能导致死锁。正确的场景应该是 WaitSet 中等待的是相同的条件，唤醒任一个都能正确处理接下来的事项，如果唤醒的线程无法正确处理，务必确保继续 notify() 下一个线程，并且自身需要重新回到 WaitSet 中.



### 4、sleep() 和 wait() 有什么区别？

对于 sleep() 方法，我们首先要知道该方法是属于 Thread 类中的。而 wait() 方法，则是属于 Object 类中的。

sleep() 方法导致了程序暂停执行指定的时间，让出 cpu 该其他线程，但是他的监控状态依然保持者，当指定的时间到了又会自动恢复运行状态。在调用 sleep() 方法的过程中，线程不会释放对象锁。

当调用 wait() 方法的时候，线程会放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象调用 notify() 方法后本线程才进入对象锁定池准备，获取对象锁进入运行状态。



### 5、volatile 是什么？可以保证有序性吗？

一旦一个共享变量（类的成员变量、类的静态成员变量）被 volatile 修饰之后，那么就具备了两层语义：

1）保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的，volatile 关键字会强制将修改的值立即写入主存。

2）禁止进行指令重排序。

volatile 不是原子性操作

什么叫保证部分有序性？

当程序执行到 volatile 变量的读操作或者写操作时，在其前面的操作的更改肯定全部已经进行，且结果已经对后面的操作可见；在其后面的操作肯定还没有进行；

```
x = 2; //语句1
y = 0; //语句2
flag = true; //语句3
x = 4; //语句4
y = -1; //语句5
```

由于 flag 变量为 volatile 变量，那么在进行指令重排序的过程的时候，不会将语句 3 放到语句 1、语句 2 前面，也不会将语句 3 放到语句4、语句 5 后面。但是要注意语句 1 和语句 2 的顺序、语句 4 和语句 5 的顺序是不作任何保证的。

**使用 volatile 一般用于状态标记量和单例模式的双检锁。**



### 6、Thread 类中的 start() 和 run() 方法有什么区别？

start() 方法被用来启动新创建的线程，而且 start() 内部调用了 run() 方法，这和直接调用 run() 方法的效果不一样。当你调用 run() 方法的时候，只会是在原来的线程中调用，没有新的线程启动，start() 方法才会启动新线程。



### 7、为什么 wait，notify 和 notifyAll 这些方法不在 thread 类里面？

明显的原因是 JAVA 提供的锁是对象级的而不是线程级的，每个对象都有锁，通过线程获得。如果线程需要等待某些锁那么调用对象中的wait() 方法就有意义了。如果 wait() 方法定义在 Thread 类中，线程正在等待的是哪个锁就不明显了。简单的说，由于 wait，notify 和notifyAll 都是锁级别的操作，所以把他们定义在 Object 类中因为锁属于对象。



### 8、为什么 wait 和 notify 方法要在同步块中调用？

1. 只有在调用线程拥有某个对象的独占锁时，才能够调用该对象的 wait()，notify() 和 notifyAll() 方法。

2. 如果你不这么做，你的代码会抛出 IllegalMonitorStateException 异常。
3. 还有一个原因是为了避免 wait 和 notify 之间产生竞态条件。

wait() 方法强制当前线程释放对象锁。这意味着在调用某对象的 wait() 方法之前，当前线程必须已经获得该对象的锁。因此，线程必须在某个对象的同步方法或同步代码块中才能调用该对象的 wait() 方法。

在调用对象的 notify() 和 notifyAll() 方法之前，调用线程必须已经得到该对象的锁。因此，必须在某个对象的同步方法或同步代码块中才能调用该对象的 notify() 或 notifyAll() 方法。

调用 wait() 方法的原因通常是，调用线程希望某个特殊的状态（或变量）被设置之后再继续执行。调用 notify() 或 notifyAll() 方法的原因通常是，调用线程希望告诉其他等待中的线程："特殊状态已经被设置"。这个状态作为线程间通信的通道，它必须是一个可变的共享状态（或变量）	

![](https://dream-syz.github.io/3-1.png)		



### 9、Java 中 interrupted 和 isInterrupted 方法的区别？

![](https://dream-syz.github.io/3-2.png)

interrupted() 和 isInterrupted() 的主要区别是前者会将中断状态清除而后者不会。Java 多线程的中断机制是用内部标识来实现的，调用`Thread.interrupt()` 来中断一个线程就会设置中断标识为 true。当中断线程调用静态方法 `Thread.interrupted()` 来检查中断状态时，中断状态会被清零。而非静态方法 isInterrupted() 用来查询其它线程的中断状态且不会改变中断状态标识。简单的说就是任何抛出InterruptedException 异常的方法都会将中断状态清零。无论如何，一个线程的中断状态有有可能被其它线程调用中断来改变。



### 10、Java 中 synchronized 和 ReentrantLock 有什么不同？

相似点：

这两种同步方式有很多相似之处，它们都是加锁方式同步，而且都是阻塞式的同步，也就是说当如果一个线程获得了对象锁，进入了同步块，其他访问该同步块的线程都必须阻塞在同步块外面等待，而进行线程阻塞和唤醒的代价是比较高的.

区别：

这两种方式最大区别就是对于 Synchronized 来说，它是 java 语言的关键字，是原生语法层面的互斥，需要 jvm 实现。而 ReentrantLock 它是 JDK 1.5 之后提供的 API 层面的互斥锁，需要 lock() 和 unlock() 方法配合 try / finally 语句块来完成。

Synchronized 进过编译，会在同步块的前后分别形成 monitorenter 和 monitorexit 这个两个字节码指令。在执行 monitorenter 指令时，首先要尝试获取对象锁。如果这个对象没被锁定，或者当前线程已经拥有了那个对象锁，把锁的计算器加 1，相应的，在执行monitorexit 指令时会将锁计算器就减 1，当计算器为 0 时，锁就被释放了。如果获取对象锁失败，那当前线程就要阻塞，直到对象锁被另一个线程释放为止。

由于 ReentrantLock 是 `java.util.concurrent` 包下提供的一套互斥锁，相比 Synchronized，ReentrantLock 类提供了一些高级功能，主要有以下3项：

1.等待可中断，持有锁的线程长期不释放的时候，正在等待的线程可以选择放弃等待，这相当于Synchronized 来说可以避免出现死锁的情况。

2.公平锁，多个线程等待同一个锁时，必须按照申请锁的时间顺序获得锁，Synchronized 锁非公平锁，ReentrantLock 默认的构造函数是创建的非公平锁，可以通过参数 true 设为公平锁，但公平锁表现的性能不是很好。

3.锁绑定多个条件，一个 ReentrantLock 对象可以同时绑定多个对象。			



### 11、有三个线程 T1、T2、T3 如何保证顺序执行？		

在多线程中有多种方法让线程按特定顺序执行，你可以用线程类的 join() 方法在一个线程中启动另一个线程，另外一个线程完成该线程继续执行。为了确保三个线程的顺序你应该先启动最后一个（ T3 调用 T2，T2调用 T1），这样 T1 就会先完成而 T3 最后完成。

实际上先启动三个线程中哪一个都行， 因为在每个线程的run方法中用join方法限定了三个线程的执行顺序。

```java
public class JoinTest2 {
 // 1.现在有T1、T2、T3三个线程，你怎样保证T2在T1执行完后执行，T3在T2执行完后执行
   public static void main(String[] args) {
     final Thread t1 = new Thread(new Runnable() {
       @Override
       public void run() {
 		 System.out.println("t1");
 	   }
     });
   final Thread t2 = new Thread(new Runnable() {
 	@Override
	 public void run() {
 	   try {
 		// 引用t1线程，等待t1线程执行完
 		t1.join();
 	   } catch (InterruptedException e) {
 		e.printStackTrace();
 	   }
 	 System.out.println("t2");
 	}
   });
   Thread t3 = new Thread(new Runnable() {
 	 @Override
 	 public void run() {
 	   try {
 	      // 引用t2线程，等待t2线程执行完
 		  t2.join();
 	   } catch (InterruptedException e) {
 		  e.printStackTrace();
 	   }
 	   System.out.println("t3");
 	 }
   });
   t3.start();//这里三个线程的启动顺序可以任意，大家可以试下！
   t2.start();
   t1.start();
  }
}
```



### 12、SynchronizedMap 和 ConcurrentHashMap 有什么区别？

SynchronizedMap() 和 Hashtable 一样，实现上在调用 map 所有方法时，都对整个 map 进行同步。而 ConcurrentHashMap 的实现却更加精细，它对 map 中的所有桶加了锁。所以，只要有一个线程访问 map，其他线程就无法进入 map，而如果一个线程在访问ConcurrentHashMap 某个桶时，其他线程，仍然可以对 map 执行某些操作。

所以，ConcurrentHashMap 在性能以及安全性方面，明显比 Collections.synchronizedMap() 更加有优势。同时，同步操作精确控制到桶，这样，即使在遍历 map 时，如果其他线程试图对 map 进行数据修改，也不会抛出 ConcurrentModificationException。											



### 13、什么是线程安全

线程安全就是说多线程访问同一段代码，不会产生不确定的结果。

又是一个理论的问题，各式各样的答案有很多，我给出一个个人认为解释地最好的：**如果你的代码在多线程下执行和在单线程下执行永远都能获得一样的结果，那么你的代码就是线程安全的**。

这个问题有值得一提的地方，就是线程安全也是有几个级别的：

（1）不可变

像 String、Integer、Long 这些，都是 final 类型的类，任何一个线程都改变不了它们的值，要改变除非新创建一个，因此这些不可变对象不需要任何同步手段就可以直接在多线程环境下使用

（2）绝对线程安全

不管运行时环境如何，调用者都不需要额外的同步措施。要做到这一点通常需要付出许多额外的代价，Java 中标注自己是线程安全的类，实际上绝大多数都不是线程安全的，不过绝对线程安全的类，Java 中也有，比方说 CopyOnWriteArrayList、CopyOnWriteArraySet

（3）相对线程安全

相对线程安全也就是我们通常意义上所说的线程安全，像 Vector 这种，add、remove 方法都是原子操作，不会被打断，但也仅限于此，如果有个线程在遍历某个 Vector、有个线程同时在 add 这个 Vector，99 % 的情况下都会出现 ConcurrentModificationException，也就是**fail-fast 机制**。

（4）线程非安全

这个就没什么好说的了，ArrayList、LinkedList、HashMap 等都是线程非安全的类



### 14、Thread 类中的 yield 方法有什么作用？

Yield 方法可以暂停当前正在执行的线程对象，让其它有相同优先级的线程执行。它是一个静态方法而且只保证当前线程放弃 CPU 占用而不能保证使其它线程一定能占用 CPU，执行 yield() 的线程有可能在进入到暂停状态后马上又被执行。



### 15、Java 线程池中 submit() 和 execute() 方法有什么区别？

两个方法都可以向线程池提交任务，execute() 方法的返回类型是 void，它定义在 Executor 接口，b而 submit() 方法可以返回持有计算结果的 Future 对象，它定义在 ExecutorService 接口中，它扩展了 Executor 接口，其它线程池类像 ThreadPoolExecutor 和ScheduledThreadPoolExecutor 都有这些方法。



### 16、说一说自己对于 synchronized 关键字的了解

synchronized 关键字解决的是多个线程之间访问资源的同步性，synchronized 关键字可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。 另外，在 Java 早期版本中，synchronized 属于重量级锁，效率低下，因为监视器锁（monitor）是依赖于底层的操作系统的 Mutex Lock 来实现的，Java 的线程是映射到操作系统的原生线程之上的。如果要挂起或者唤醒一个线程，都需要操作系统帮忙完成，而操作系统实现线程之间的切换时需要从用户态转换到内核态，这个状态之间的转换需要相对比较长的时间，时间成本相对较高，这也是为什么早期的 synchronized 效率低的原因。庆幸的是在 Java 6 之后 Java 官方对从 JVM 层面对 synchronized 做了较大优化，所以现在的 synchronized 锁效率也优化得很不错了。JDK1.6 对锁的实现引入了大量的优化，如自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁等技术来减少锁操作的开销。



### 17、说说自己是怎么使用 synchronized 关键字？

**修饰实例方法**:：作用于当前对象实例加锁，进入同步代码前要获得当前对象实例的锁 

**修饰静态方法**：也就是给当前类加锁，会作用于类的所有对象实例，因为静态成员不属于任何一个实例对象，是类成员（ static 表明这是该类的一个静态资源，不管 new 了多少个对象，只有一份）。所以如果一个线程 A 调用一个实例对象的非静态 synchronized 方法，而线程 B 需要调用这个实例对象所属类的静态 synchronized 方法，是允许的，不会发生互斥现象，**因为访问静态** **synchronized** **方法占用的锁是当前类的锁，而访问非静态** **synchronized** **方法占用的锁是当前实例对象锁。 **

**修饰代码块**：指定加锁对象，对给定对象加锁，进入同步代码库前要获得给定对象的锁。

**总结**： synchronized 关键字加到 static 静态方法和 synchronized(class) 代码块上都是是给 Class 类上锁。synchronized 关键字加到实例方法上是给对象实例上锁。尽量不要使用 synchronized(String a) 因为 JVM 中，字符串常量池具有缓存功能！会导致加锁在同一个对象，必然会阻塞



### 18、什么是线程安全？Vector 是一个线程安全类吗？

如果你的代码所在的进程中有多个线程在同时运行，而这些线程可能会同时运行这段代码。如果每次运行结果和单线程运行的结果是一样的，而且其他的变量的值也和预期的是一样的，就是线程安全的。一个线程安全的计数器类的同一个实例对象在被多个线程使用的情况下也不会出现计算失误。很显然你可以将集合类分成两组，线程安全和非线程安全的。Vector 是用同步方法来实现线程 安全的, 而和它相似的 ArrayList 不是线程安全的。



### 19、volatile 关键字的作用？

一旦一个共享变量（类的成员变量、类的静态成员变量）被 volatile 修饰之后，那么就具备了两层语义：

- 保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。

- 禁止进行指令重排序。

volatile  与 synchronized  区别（5 个）：

- volatile 本质是在告诉 jvm 当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取；

  synchronized 则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。

- volatile 仅能使用在变量级别；

  synchronized 则可以使用在变量、方法、和类级别的。

- volatile 仅能实现变量的修改可见性，并不能保证原子性；

  synchronized 则可以保证变量的修改可见性和原子性。

- volatile 不会造成线程的阻塞；

  synchronized 可能会造成线程的阻塞。

- volatile 标记的变量不会被编译器优化；

  synchronized标记的变量可以被编译器优化。



### 20、常用的线程池有哪些？

- newSingleThreadExecutor：创建一个单线程的线程池，此线程池保证所有任务的执行顺序按照任务的提交顺序执行。

- newFixedThreadPool：创建固定大小的线程池，它的核心线程数和最大线程数是一样的，每次提交一个任务就创建一个线程，直到线程达到线程池的最大大小。

- newCachedThreadPool：创建一个可缓存的线程池，它的特点就是可以无限的缓存线程任务，最大可以达到 Integer.MAX_VALUE，为 2^31-1，此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说JVM）能够创建的最大线程大小。
- newScheduledThreadPool：创建一个大小无限的线程池，此线程池支持定时以及周期性执行任务的需求。

- newSingleThreadScheduledExecutor：创建一个单线程的线程池。此线程池支持定时以及周期性执行任务的需求



### 21、简述一下你对线程池的理解

（如果问到了这样的问题，可以展开的说一下线程池如何用、线程池的好处、线程池的启动策略）

合理利用线程池能够带来三个好处。

第一：降低资源消耗。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。

第二：提高响应速度。当任务到达时，任务可以不需要等到线程创建就能立即执行。

第三：提高线程的可管理性。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。



### 22、Java 程序是如何执行的

我们日常的工作中都使用开发工具（IntelliJ IDEA 或 Eclipse 等）可以很方便的调试程序，或者是通过打包工具把项目打包成 jar 包或者 war 包，放入 Tomcat 等 Web 容器中就可以正常运行了，但你有没有想过 Java 程序内部是如何执行的？其实不论是在开发工具中运行还是在 Tomcat 中运行，Java 程序的执行流程基本都是相同的，它的执行流程如下：

- 先把 Java 代码编译成字节码，也就是把 .java 类型的文件编译成 .class 类型的文件。这个过程的大致执行流程：Java 源代码 -> 词法分析器 -> 语法分析器 -> 语义分析器 -> 字符码生成器 ->最终生成字节码，其中任何一个节点执行失败就会造成编译失败；

- 把 class 文件放置到 Java 虚拟机，这个虚拟机通常指的是 Oracle 官方自带的 Hotspot JVM；

- Java 虚拟机使用类加载器（Class Loader）装载 class 文件；

- 类加载完成之后，会进行字节码效验，字节码效验通过之后 JVM 解释器会把字节码翻译成机器码交由操作系统执行。但不是所有代码都是解释执行的，JVM 对此做了优化，比如，以 Hotspot 虚拟机来说，它本身提供了 JIT（Just In Time）也就是我们通常所说的动态编译器，它能够在运行时将热点代码编译为机器码，这个时候字节码就变成了编译执行。Java 程序执行流程图如下：

![](https://dream-syz.github.io/3-3.png)



### 23、锁的优化机制了解吗？

从 JDK 1.6 版本之后，synchronized 本身也在不断优化锁的机制，有些情况下他并不会是一个很重量级的锁了。优化机制包括自适应锁、自旋锁、锁消除、锁粗化、轻量级锁和偏向锁。

锁的状态从低到高依次为 **无锁** -> **偏向锁** -> **轻量级锁** -> **重量级锁**，升级的过程就是从低到高，降级在一定条件也是有可能发生的。

**自旋锁**：由于大部分时候，锁被占用的时间很短，共享变量的锁定时间也很短，所有没有必要挂起线程，用户态和内核态的来回上下文切换严重影响性能。自旋的概念就是让线程执行一个忙循环，可以理解为就是啥也不干，防止从用户态转入内核态，自旋锁可以通过设置    -XX:+UseSpining 来开启，自旋的默认次数是 10 次，可以使用 -XX:PreBlockSpin 设置。

**自适应锁**：自适应锁就是自适应的自旋锁，自旋的时间不是固定时间，而是由前一次在同一个锁上的自旋时间和锁的持有者状态来决定。

**锁消除**：锁消除指的是 JVM 检测到一些同步的代码块，完全不存在数据竞争的场景，也就是不需要加锁，就会进行锁消除。

**锁粗化**：锁粗化指的是有很多操作都是对同一个对象进行加锁，就会把锁的同步范围扩展到整个操作序列之外。

**偏向锁**：当线程访问同步块获取锁时，会在对象头和栈帧中的锁记录里存储偏向锁的线程 ID，之后这个线程再次进入同步块时都不需要CAS 来加锁和解锁了，偏向锁会永远偏向第一个获得锁的线程，如果后续没有其他线程获得过这个锁，持有锁的线程就永远不需要进行同步，反之，当有其他线程竞争偏向锁时，持有偏向锁的线程就会释放偏向锁。可以用过设置 -XX:+UseBiasedLocking 开启偏向锁。

**轻量级锁**：JVM 的对象的对象头中包含有一些锁的标志位，代码进入同步块的时候，JVM 将会使用 CAS 方式来尝试获取锁，如果更新成功则会把对象头中的状态位标记为轻量级锁，如果更新失败，当前线程就尝试自旋来获得锁。整个锁升级的过程非常复杂，我尽力去除一些无用的环节，简单来描述整个升级的机制。简单点说，偏向锁就是通过对象头的偏向线程 ID 来对比，甚至都不需要 CAS 了，而轻量级锁主要就是通过 CAS 修改对象头锁记录和自旋来实现，重量级锁则是除了拥有锁的线程其他全部阻塞。

![](https://dream-syz.github.io/3-4.png)



### 24、说说进程和线程的区别？

1. 进程是一个“执行中的程序”，是系统进行资源分配和调度的一个独立单位。
2. 线程是进程的一个实体，一个进程中拥有多个线程，线程之间共享地址空间和其它资源（所以通信和同步等操作线程比进程更加容易）

3. 线程上下文的切换比进程上下文切换要快很多。

- （1）进程切换时，涉及到当前进程的 CPU 环境的保存和新被调度运行进程的 CPU 环境的设置。

- （2）线程切换仅需要保存和设置少量的寄存器内容，不涉及存储管理方面的操作。



### 25、产生死锁的四个必要条件？

1. 互斥条件：一个资源每次只能被一个线程使用
2. 请求与保持条件：一个线程因请求资源而阻塞时，对已获得的资源保持不放
3. 不剥夺条件：进程已经获得的资源，在未使用完之前，不能强行剥夺
4. 循环等待条件：若干线程之间形成一种头尾相接的循环等待资源关系



### 26、如何避免死锁？

指定获取锁的顺序，举例如下：

1. 比如某个线程只有获得 A 锁和 B 锁才能对某资源进行操作，在多线程条件下，如何避免死锁？
2. 获得锁的顺序是一定的，比如规定，只有获得 A 锁的线程才有资格获取 B 锁，按顺序获取锁就可以避免死锁！！！



### 27、线程池核心线程数怎么设置呢？

分为 CPU 密集型和 IO 密集型

**CPU**

这种任务消耗的主要是 CPU 资源，可以将线程数设置为 N（CPU 核心数）+1，比 CPU 核心数多出来的一个线程是为了防止线程偶发的缺页中断，或者其它原因导致的任务暂停而带来的影响。一旦任务暂停，CPU 就会处于空闲状态，而在这种情况下多出来的一个线程就可以充分利用 CPU 的空闲时间。

**IO 密集型**

这种任务应用起来，系统会用大部分的时间来处理 I/O 交互，而线程在处理 I/O 的时间段内不会占用 CPU 来处理，这时就可以将 CPU 交出给其它线程使用。因此在 I/O 密集型任务的应用中，我们可以多配置一些线程，具体的计算方法是：核心线程数 = CPU 核心数量 * 2。



### 28、Java 线程池中队列常用类型有哪些？

- ArrayBlockingQueue 是一个基于数组结构的 **有界阻塞队列**，此队列按 FIFO（先进先出）原则对元素进行排序。

- LinkedBlockingQueue 一个基于链表结构的 **阻塞队列**，此队列按 FIFO（先进先出） 排序元素，吞吐量通常要高于 ArrayBlockingQueue 。

- SynchronousQueue 一个不存储元素的 **阻塞队列**。

- PriorityBlockingQueue 一个具有优先级的 **无限阻塞队列**。PriorityBlockingQueue 也是 **基于最小二叉堆实现**

- DelayQueue 
  - 只有当其指定的延迟时间到了，才能够从队列中获取到该元素。
  - DelayQueue 是一个没有大小限制的队列， 
  - 因此往队列中插入数据的操作（生产者）永远不会被阻塞，而只有获取数据的操作（消费者）才会被阻塞。

这里能说出前三种也就差不多了，如果能说全那是最好。



### 29、线程安全需要保证几个基本特征？

**原子性**，简单说就是相关操作不会中途被其他线程干扰，一般通过同步机制实现。

**可见性**，是一个线程修改了某个共享变量，其状态能够立即被其他线程知晓，通常被解释为将线程本地状态反映到主内存上，volatile 就是负责保证可见性的。

**有序性**，是保证线程内串行语义，避免指令重排等。



### 30、说一下线程之间是如何通信的？

线程之间的通信有两种方式：共享内存和消息传递。

**共享内存**

在共享内存的并发模型里，线程之间共享程序的公共状态，线程之间通过写-读内存中的公共状态来隐式进行通信。典型的共享内存通信方式，就是通过共享对象进行通信。

例如上图线程 A 与 线程 B 之间如果要通信的话，那么就必须经历下面两个步骤：

1. 线程 A 把本地内存 A 更新过得共享变量刷新到主内存中去。
2. 线程 B 到主内存中去读取线程 A 之前更新过的共享变量。

**消息传递**

在消息传递的并发模型里，线程之间没有公共状态，线程之间必须通过明确的发送消息来显式进行通信。在 Java 中典型的消息传递方式，就是 wait() 和 notify() ，或者 BlockingQueue 。



### 31、CAS 的原理呢？

CAS 叫做 CompareAndSwap，比较并交换，主要是通过处理器的指令来保证操作的原子性，它包含三个操作数：

1. 变量内存地址，V 表示
2. 旧的预期值，A 表示
3. 准备设置的新值，B 表示

当执行 CAS 指令时，只有当 V 等于 A 时，才会用 B 去更新 V 的值，否则就不会执行更新操作。



### 32、CAS 有什么缺点吗？

CAS 的缺点主要有 3 点：

**ABA 问题**：ABA 的问题指的是在 CAS 更新的过程中，当读取到的值是 A，然后准备赋值的时候仍然是 A，但是实际上有可能 A 的值被改成了 B，然后又被改回了 A，这个 CAS 更新的漏洞就叫做 ABA。只是 ABA 的问题大部分场景下都不影响并发的最终效果。

Java 中有 AtomicStampedReference 来解决这个问题，他加入了预期标志和更新后标志两个字段，更新时不光检查值，还要检查当前的标志是否等于预期标志，全部相等的话才会更新。

**循环时间长开销大**：自旋 CAS 的方式如果长时间不成功，会给 CPU 带来很大的开销。

**只能保证一个共享变量的原子操作**：只对一个共享变量操作可以保证原子性，但是多个则不行，多个可以通过 AtomicReference 来处理或者使用锁 synchronized 实现。



### 33、**说说** ThreadLocal 原理？

ThreadLocal 可以理解为线程本地变量，他会在每个线程都创建一个副本，那么在线程之间访问内部副本变量就行了，做到了线程之间互相隔离，相比于 synchronized 的做法是用空间来换时间。

ThreadLocal 有一个静态内部类 ThreadLocalMap，ThreadLocalMap 又包含了一个 Entry 数组，Entry 本身是一个弱引用，它的 key 是指向 ThreadLocal 的弱引用，Entry 具备了保存 key value 键值对的能。

弱引用的目的是为了防止内存泄露，如果是强引用那么 ThreadLocal 对象除非线程结束否则始终无法被回收，弱引用则会在下一次 GC 的时候被回收。

但是这样还是会存在内存泄露的问题，假如 key 和 ThreadLocal 对象被回收之后，entry 中就存在 key 为 null，但是 value 有值的 entry对象，但是永远没办法被访问到，同样除非线程结束运行。

但是只要 ThreadLocal 使用恰当，在使用完之后调用 remove 方法删除 Entry 对象，实际上是不会出现这个问题的。

![](https://dream-syz.github.io/3-5.png)



### 34、线程池原理知道吗？以及核心参数

首先线程池有几个核心的参数概念：

1. 最大线程数 maximumPoolSize
2. 核心线程数 corePoolSize
3. 活跃时间 keepAliveTime
4. 阻塞队列 workQueue
5. 拒绝策略 RejectedExecutionHandler

当提交一个新任务到线程池时，具体的执行流程如下：

1. 当我们提交任务，线程池会根据 corePoolSize 大小创建若干任务数量线程执行任务
2. 当任务的数量超过 corePoolSize 数量，后续的任务将会进入阻塞队列阻塞排队
3. 当阻塞队列也满了之后，那么将会继续创建（maximumPoolSize-corePoolSize）个数量的线程来执行任务，如果任务处理完成，maximumPoolSize-corePoolSize 额外创建的线程等待 keepAliveTime 之后被自动销毁

4. 如果达到 maximumPoolSize，阻塞队列还是满的状态，那么将根据不同的拒绝策略对应处理

![](https://dream-syz.github.io/3-6.png)



### 35、 线程池的拒绝策略有哪些？

主要有 4 种拒绝策略：

1. AbortPolicy：直接丢弃任务，抛出异常，这是默认策略
2. CallerRunsPolicy：只用调用者所在的线程来处理任务
3. DiscardOldestPolicy：丢弃等待队列中最旧的任务，并执行当前任务
4. DiscardPolicy：直接丢弃任务，也不抛出异常



### 36、说说你对 JMM 内存模型的理解？为什么需要 JMM ？

随着 CPU 和内存的发展速度差异的问题，导致 CPU 的速度远快于内存，所以现在的 CPU 加入了高速缓存，高速缓存一般可以分为 L1、L2、L3 三级缓存。基于上面的例子我们知道了这导致了缓存一致性的问题，所以加入了缓存一致性协议，同时导致了内存可见性的问题，而编译器和 CPU 的重排序导致了原子性和有序性的问题，JMM 内存模型正是对多线程操作下的一系列规范约束，因为不可能让陈雇员的代码去兼容所有的 CPU，通过 JMM 我们才屏蔽了不同硬件和操作系统内存的访问差异，这样保证了 Java 程序在不同的平台下达到一致的内存访问效果，同时也是保证在高效并发的时候程序能够正确执行。

![](https://dream-syz.github.io/3-7.png)

**原子性**：Java 内存模型通过 read、load、assign、use、store、write 来保证原子性操作，此外还有 lock 和 unlock，直接对应着synchronized 关键字的 monitorenter 和 monitorexit 字节码指令。

**可见性**：可见性的问题在上面的回答已经说过，Java 保证可见性可以认为通过 volatile、synchronized、final 来实现。

**有序性**：由于处理器和编译器的重排序导致的有序性问题，Java 通过 volatile、synchronized 来保证。

**happen-before 规则**

虽然指令重排提高了并发的性能，但是 Java 虚拟机会对指令重排做出一些规则限制，并不能让所有的指令都随意的改变执行位置，主要有以下几点：

1. 单线程每个操作，happen-before 于该线程中任意后续操作
2. volatile 写 happen-before 与后续对这个变量的读
3. synchronized 解锁 happen-before 后续对这个锁的加锁
4. final 变量的写 happen-before 于 final 域对象的读，happen-before 后续对 final 变量的读
5. 传递性规则，A 先于 B，B 先于 C，那么 A 一定先于 C 发生

**说了半天，到底工作内存和主内存是什么？**

主内存可以认为就是物理内存，Java 内存模型中实际就是虚拟机内存的一部分。而工作内存就是 CPU 缓存，他有可能是寄存器也有可能是 L1 \ L2 \ L3 缓存，都是有可能的。



### 37、多线程有什么用？

一个可能在很多人看来很扯淡的一个问题：我会用多线程就好了，还管它有什么用？在我看来，这个回答更扯淡。所谓"知其然知其所以然"，"会用"只是"知其然"，"为什么用"才是"知其所以然"，只有达到"知其然知其所以然"的程度才可以说是把一个知识点运用自如。OK，下面说说我对这个问题的看法：

#### （1）发挥多核 CPU 的优势

随着工业的进步，现在的笔记本、台式机乃至商用的应用服务器至少也都是双核的，4 核、8 核甚至 16 核的也都不少见，如果是单线程的程序，那么在双核 CPU 上就浪费了 50 %，在 4 核 CPU 上就浪费了 75 %。**单核 CPU 上所谓的“多线程”那是假的多线程，同一时间处理器只会处理一段逻辑，只不过线程之间切换得比较快，看着像多个线程"同时"运行罢了**。多核 CPU 上的多线程才是真正的多线程，它能让你的多段逻辑同时工作，多线程，可以真正发挥出多核 CPU 的优势来，达到充分利用 CPU 的目的。

#### （2）防止阻塞

从程序运行效率的角度来看，单核 CPU 不但不会发挥出多线程的优势，反而会因为在单核 CPU 上运行多线程导致线程上下文的切换，而降低程序整体的效率。但是单核 CPU 我们还是要应用多线程，就是为了防止阻塞。试想，如果单核 CPU 使用单线程，那么只要这个线程阻塞了，比方说远程读取某个数据吧，对端迟迟未返回又没有设置超时时间，那么你的整个程序在数据返回回来之前就停止运行了。多线程可以防止这个问题，多条线程同时运行，哪怕一条线程的代码执行读取数据阻塞，也不会影响其它任务的执行。

#### （3）便于建模

这是另外一个没有这么明显的优点了。假设有一个大的任务 A，单线程编程，那么就要考虑很多，建立整个程序模型比较麻烦。但是如果把这个大的任务 A 分解成几个小任务，任务 B、任务 C、任务 D，分别建立程序模型，并通过多线程分别运行这几个任务，那就简单很多了。



### 38、说说 CyclicBarrier 和 CountDownLatch 的区别？

两个看上去有点像的类，都在 `java.util.concurrent` 下，都可以用来表示代码运行到某个点上，二者的区别在于：

（1）CyclicBarrier 的某个线程运行到某个点上之后，该线程即停止运行，直到所有的线程都到达了这个点，所有线程才重新运行；             

          CountDownLatch 则不是，某线程运行到某个点上之后，只是给某个数值 -1 而已，该线程继续运行

（2）CyclicBarrier 只能唤起一个任务，CountDownLatch 可以唤起多个任务

（3）CyclicBarrier 可重用，CountDownLatch 不可重用，计数值为 0 该 CountDownLatch 就不可再用了



### 39、什么是 AQS ？

简单说一下 AQS，AQS 全称为 AbstractQueuedSychronizer，翻译过来应该是 **抽象队列同步器**。如果说 `java.util.concurrent` 的基础是 CAS 的话，那么 AQS 就是整个 Java 并发包的核心了，ReentrantLock、CountDownLatch、Semaphore 等等都用到了它。AQS 实际上以双向队列的形式连接所有的 Entry，比方说 ReentrantLock，所有等待的线程都被放在一个 Entry 中并连成双向队列，前面一个线程使用 ReentrantLock 好了，则双向队列实际上的第一个 Entry 开始运行。

AQS 定义了对双向队列所有的操作，而只开放了 tryLock 和 tryRelease 方法给开发者使用，开发者可以根据自己的实现重写 tryLock 和tryRelease 方法，以实现自己的并发功能。



### 40、了解 Semaphore 吗？

Semaphore 就是一个信号量，它的作用是 **限制某段代码块的并发数**。Semaphore 有一个构造函数，可以传入一个 int 型整数 n，表示某段代码最多只有 n 个线程可以访问，如果超出了 n，那么请等待，等到某个线程执行完毕这段代码块，下一个线程再进入。由此可以看出如果 Semaphore 构造函数中传入的 int 型整数 n = 1，相当于变成了一个 synchronized 了。



### 41、什么是 Callable 和 Future？

Callable 接口类似于 Runnable，从名字就可以看出来了，但是 Runnable 不会返回结果，并且无法抛出返回结果的异常，而 Callable 功能更强大一些，被线程执行后，可以返回值，这个返回值可以被 Future 拿到，也就是说，Future 可以拿到异步执行任务的返回值。可以认为是带有回调的 Runnable。

Future 接口表示异步任务，是还没有完成的任务给出的未来结果。所以说 Callable 用于产生结果，Future 用于获取结果。



### 42、什么是阻塞队列？阻塞队列的实现原理是什么？如何使用阻塞队列来实现生产者 - 消费者模型？

阻塞队列（BlockingQueue）是一个支持两个附加操作的队列。

这两个附加的操作是：在队列为空时，获取元素的线程会等待队列变为非空。当队列满时，存储元素的线程会等待队列可用。

阻塞队列常用于生产者和消费者的场景，生产者是往队列里添加元素的线程，消费者是从队列里拿元素的线程。阻塞队列就是生产者存放元素的容器，而消费者也只从容器里拿元素。

JDK 7 提供了 7 个阻塞队列。分别是：

- ArrayBlockingQueue ：一个由数组结构组成的有界阻塞队列。

- LinkedBlockingQueue ：一个由链表结构组成的有界阻塞队列。

- PriorityBlockingQueue ：一个支持优先级排序的无界阻塞队列。

- DelayQueue：一个使用优先级队列实现的无界阻塞队列。

- SynchronousQueue：一个不存储元素的阻塞队列。

- LinkedTransferQueue：一个由链表结构组成的无界阻塞队列。

- LinkedBlockingDeque：一个由链表结构组成的双向阻塞队列。

Java 5 之前实现同步存取时，可以使用普通的一个集合，然后在使用线程的协作和线程同步可以实现生产者 - 消费者模式，主要的技术就是用好 wait，notify，notifyAll，sychronized 这些关键字。而在 java 5 之后，可以使用阻塞队列来实现，此方式大大简少了代码量，使得多线程编程更加容易，安全方面也有保障。

BlockingQueue 接口是 Queue 的子接口，它的主要用途并不是作为容器，而是作为线程同步的的工具，因此他具有一个很明显的特性，当生产者线程试图向 BlockingQueue 放入元素时，如果队列已满，则线程被阻塞，当消费者线程试图从中取出一个元素时，如果队列为空，则该线程会被阻塞，正是因为它所具有这个特性，所以在程序中多个线程交替向 BlockingQueue 中放入元素，取出元素，它可以很好的控制线程之间的通信。

阻塞队列使用最经典的场景就是 socket 客户端数据的读取和解析，读取数据的线程不断将数据放入队列，然后解析线程不断从队列取数据解析。



### 43、什么是多线程中的上下文切换？

在上下文切换过程中，CPU 会停止处理当前运行的程序，并保存当前程序运行的具体位置以便之后继续运行。从这个角度来看，上下文切换有点像我们同时阅读几本书，在来回切换书本的同时我们需要记住每本书当前读到的页码。

在程序中，上下文切换过程中的“页码”信息是保存在进程控制块（PCB）中的。PCB 还经常被称作“切换桢”（switchframe）。“页码”信息会一直保存到 CPU 的内存中，直到他们被再次使用。

上下文切换是存储和恢复 CPU 状态的过程，它使得线程执行能够从中断点恢复执行。上下文切换是多任务操作系统和多线程环境的基本特征。



### 44、什么是 Daemon 线程？它有什么意义？

所谓后台（daemon）线程，也叫守护线程，是指在程序运行的时候在后台提供一种通用服务的线程，并且这个线程并不属于程序中不可或缺的部分。

因此，当所有的非后台线程结束时，程序也就终止了，同时会杀死进程中的所有后台线程。反过来说， 只要有任何非后台线程还在运行，程序就不会终止。

必须在线程启动之前调用 setDaemon() 方法，才能把它设置为后台线程。注意：后台进程在不执行 finally 子句的情况下就会终止其 run() 方法。

比如：JVM 的垃圾回收线程就是 Daemon 线程，Finalizer 也是守护线程。



### 45、乐观锁和悲观锁的理解及如何实现，有哪些实现方式？

**悲观锁：**总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁。

传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁。再比如 Java 里面的同步原语 synchronized 关键字的实现也是悲观锁。

**乐观锁：**顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。

乐观锁适用于多读的应用类型，这样可以提高吞吐量，像数据库提供的类似于 write_condition 机制，其实都是提供的乐观锁。在 Java 中 `java.util.concurrent.atomic` 包下面的原子变量类就是使用了乐观锁的一种实现方式 CAS 实现的。

**乐观锁的实现方式：**

1、使用版本标识来确定读到的数据与提交时的数据是否一致。提交后修改版本标识，不一致时可以采取丢弃和再次尝试的策略。

2、java 中的 Compare and Swap 即 CAS，当多个线程尝试使用 CAS 同时更新同一个变量时，只有其中一个线程能更新变量的值，而其它线程都失败，失败的线程并不会被挂起，而是被告知这次竞争中失败，并可以再次尝试。 CAS 操作中包含三个操作数 —— 需要读写的内存位置（V）、进行比较的预期原值（A）和拟写入的新值(B)。如果内存位置 V 的值与预期原值 A 相匹配，那么处理器会自动将该位置值更新为新值 B。否则处理器不做任何操作。

**CAS 缺点：**

1. **ABA 问题：**比如说一个线程 one 从内存位置 V 中取出 A，这时候另一个线程 two 也从内存中取出 A，并且 two 进行了一些操作变成了 B，然后 two 又将 V 位置的数据变成 A，这时候线程 one 进行 CAS 操作发现内存中仍然是 A，然后 one 操作成功。尽管线程 one的 CAS 操作成功，但可能存在潜藏的问题。从 Java 1.5 开始 JDK 的 atomic 包里提供了一个类 `AtomicStampedReference` 来解决ABA 问题。

2. **循环时间长开销大：**对于资源竞争严重（线程冲突严重）的情况，CAS 自旋的概率会比较大，从而浪费更多的 CPU 资源，效率低于 synchronized。

3. **只能保证一个共享变量的原子操作：**当对一个共享变量执行操作时，我们可以使用循环 CAS 的方式来保证原子操作，但是对多个共享变量操作时，循环 CAS 就无法保证操作的原子性，这个时候就可以用锁。



### 46、CopyOnWriteArrayList 应用场景

1、**读多写少的场景**：由于在写操作时需要复制一个新的数组，因此写的性能较差。而读操作则不会影响原来的数组，所以性能很高。适合于读多写少的场景。

2、**读操作优先场景**：由于 CopyOnWriteArrayList 在写操作时，所有读操作都不受到影响和阻塞。因此，当对数据的读操作次数比较多时，可以使用 CopyOnWriteArrayList 来提升系统的性能。

3、**数据更新要求不频繁的场景**: 在 CopyOnWriteArrayList 上，每次添加、修改或删除列表中的元素时，都需要重新创建一个新的底层数组，因此在实现上会消耗更多的内存空间。如果数据经常需要被更新，则建议使用普通的 ArrayList。

4、**互斥访问数据不方便的场景**: 在多线程环境下，如果对一个 ArrayList 实例进行访问，需要加锁保证数据一致性。但是，在某些场景下，加锁会给程序带来额外的复杂度和延迟。在这种情况下可以考虑使用 CopyOnWriteArrayList。

5、**高并发场景**：CopyOnWriteArrayList 在写操作时候有很高的并发度，不会阻塞其他的读操作。因此非常适合用于读多写少的场景下，可以提高系统的并发性能。

需要注意的是，CopyOnWriteArrayList 并不能解决所有多线程问题，它主要是针对读多写少的应用场景，所以开发人员在选择使用 CopyOnWriteArrayList 时，一定要结合实际需求，如果业务场景不适合使用该类，建议使用其它的集合类，或者自己实现一些更加适用的线程安全集合类。

总之，CopyOnWriteArrayList 适合于读多写少，读优先的场景，需要更新频率较低的数据，而且有运行效率限制的场景。因为它的底层实现方式比较特殊，它的读性能非常高，而写性能相对较差。对于需要快速修改的应用场景，可以考虑使用其他的 List 类来替代 CopyOnWriteArrayList 。



### 47、说说线程池的常用构建方式

在Java中，可以使用`ExecutorService`接口来创建线程池。`ExecutorService`是`Executor`的子接口，它提供了更多的方法来管理和控制线程池的执行。

以下是几种常见的建立线程池的方式：

1. 使用`Executors`类的静态方法创建线程池：
```java
ExecutorService executor = Executors.newFixedThreadPool(10); // 创建固定大小的线程池，最多同时执行10个任务
ExecutorService executor = Executors.newSingleThreadExecutor(); // 创建单个线程的线程池
ExecutorService executor = Executors.newCachedThreadPool(); // 创建可缓存的线程池，根据需要自动调整线程数量
```

2. 使用`ThreadPoolExecutor`类的构造函数创建线程池：
```java
int corePoolSize = 10; // 核心线程数
int maximumPoolSize = 20; // 最大线程数
long keepAliveTime = 60; // 线程空闲时间
TimeUnit unit = TimeUnit.SECONDS; // 空闲时间的单位

ExecutorService executor = new ThreadPoolExecutor(corePoolSize, maximumPoolSize, keepAliveTime, unit,
        new LinkedBlockingQueue<>()); // 创建自定义的线程池，使用有界队列作为任务队列
```

3. 使用`ThreadPoolExecutor`类的`ThreadPoolExecutor.AbortPolicy`等内置拒绝策略创建线程池：
```java
int corePoolSize = 10; // 核心线程数
int maximumPoolSize = 20; // 最大线程数
long keepAliveTime = 60; // 线程空闲时间
TimeUnit unit = TimeUnit.SECONDS; // 空闲时间的单位
BlockingQueue<Runnable> workQueue = new LinkedBlockingQueue<>(); // 任务队列
RejectedExecutionHandler handler = new ThreadPoolExecutor.AbortPolicy(); // 拒绝策略

ExecutorService executor = new ThreadPoolExecutor(corePoolSize, maximumPoolSize, keepAliveTime, unit,
        workQueue, handler); // 创建自定义的线程池，指定拒绝策略
```

以上是几种常见的建立线程池的方式，根据具体的需求选择适合的方式来创建线程池。



## Spring 篇

### 1、什么是 spring?

Spring 是个 java 企业级应用的开源开发框架。Spring 主要用来开发 Java 应用，但是有些扩展是针对构建 J2EE 平台的 web 应用。Spring 框架目标是简化 Java 企业级应用开发，并通过 POJO 为基础的编程模型促进良好的编程习惯。



### 2、你们项目中为什么使用 Spring 框架？

这么问的话，就直接说 Spring 框架的好处就可以了。比如说 Spring 有以下特点：

- **轻量：**Spring 是轻量的，基本的版本大约 2 MB。

- **控制反转：**Spring 通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们。

- **面向切面的编程 (AOP) ：**Spring 支持面向切面的编程，并且把应用业务逻辑和系统服务分开。

- **容器：**Spring 包含并管理应用中对象的生命周期和配置。

- **MVC 框架**：Spring 的 WEB 框架是个精心设计的框架，是 Web 框架的一个很好的替代品。

- **事务管理：**Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）。

- **异常处理：**Spring 提供方便的 API 把具体技术相关的异常（比如由 JDBC，Hibernate or JDO 抛出的）转化为一致的 unchecked 异常。



### 3、Autowired 和 Resource 关键字的区别？

@Resource 和 @Autowired 都是做 bean 的注入时使用，其实 @Resource 并不是 Spring 的注解，它的包是`javax.annotation.Resource`，需要导入，但是 Spring 支持该注解的注入。

1、共同点

两者都可以写在字段和 setter 方法上。两者如果都写在字段上，那么就不需要再写 setter 方法。

2、不同点

（1）@Autowired

@Autowired 为 Spring 提供的注解，需要导入包 `org.springframework.beans.factory.annotation.Autowired`；只按照 byType 注入。

```
public class TestServiceImpl {
 // 下面两种@Autowired只要使用一种即可
 @Autowired
 private UserDao userDao; // 用于字段上
 
 @Autowired
 public void setUserDao(UserDao userDao) { // 用于属性的方法上
 this.userDao = userDao;
 }
}
```

@Autowired 注解是按照类型（byType）装配依赖对象，默认情况下它要求依赖对象必须存在，如果允许 null 值，可以设置它的required 属性为 false。如果我们想使用按照名称（byName）来装配，可以结合 @Qualifier 注解一起使用。如下：

```
public class TestServiceImpl {
 @Autowired
 @Qualifier("userDao")
 private UserDao userDao;
}
```

（2）@Resource

@Resource 默认按照 ByName 自动注入，由 J2EE 提供，需要导入包 `javax.annotation.Resource`。

@Resource 有两个重要的属性：name 和 type，而 Spring 将 @Resource 注解的 name 属性解析为 bean 的名字，而 type 属性则解析为 bean 的类型。所以，如果使用 name 属性，则使用 byName 的自动注入策略，而使用 type 属性时则使用 byType 自动注入策略。如果既不制定 name 也不制定 type 属性，这时将通过反射机制使用 byName 自动注入策略。

```
public class TestServiceImpl {
 // 下面两种@Resource只要使用一种即可
 @Resource(name="userDao")
 private UserDao userDao; // 用于字段上
 
 @Resource(name="userDao")
 public void setUserDao(UserDao userDao) { // 用于属性的setter方法上
 this.userDao = userDao;
 }
}
```

注：最好是将 @Resource 放在 setter 方法上，因为这样更符合面向对象的思想，通过 set、get 去操作属性，而不是直接去操作属性。

**@Resource 装配顺序：**

①如果同时指定了 name 和 type，则从 Spring 上下文中找到唯一匹配的 bean 进行装配，找不到则抛出异常。

②如果指定了 name，则从上下文中查找名称（id）匹配的 bean 进行装配，找不到则抛出异常。

③如果指定了 type，则从上下文中找到类似匹配的唯一 bean 进行装配，找不到或是找到多个，都会抛出异常。

④如果既没有指定 name，又没有指定 type，则自动按照 byName 方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配。

@Resource 的作用相当于 @Autowired，只不过 @Autowired 按照 byType 自动注入。



### 4、依赖注入的方式有几种，各是什么？

#### **1、构造器注入** 

将被依赖对象通过构造函数的参数注入给依赖对象，并且在初始化对象的时候注入。

**优点：**对象初始化完成后便可获得可使用的对象。

**缺点：**当需要注入的对象很多时，构造器参数列表将会很长； 不够灵活。若有多种注入方式，每种方式只需注入指定几个依赖，那么就需要提供多个重载的构造函数，麻烦。

#### 2、setter 方法注入

IOC Service Provider 通过调用成员变量提供的 setter 函数将被依赖对象注入给依赖类。

**优点：**灵活。可以选择性地注入需要的对象。

**缺点：**依赖对象初始化完成后由于尚未注入被依赖对象，因此还不能使用。

#### 3、接口注入

依赖类必须要实现指定的接口，然后实现该接口中的一个函数，该函数就是用于依赖注入。该函数的参数就是要注入的对象。

**优点：**接口注入中，接口的名字、函数的名字都不重要，只要保证函数的参数是要注入的对象类型即可。

**缺点：**侵入行太强，不建议使用。

PS：什么是侵入行？ 如果类 A 要使用别人提供的一个功能，若为了使用这功能，需要在自己的类中增加额外的代码，这就是侵入性。



### 5、讲一下什么是 Spring

Spring 是一个轻量级的 IOC 和 AO P容器框架。是为 Java 应用程序提供基础性服务的一套框架，目的是用于简化企业应用程序的开发，它使得开发者只需要关心业务需求。常见的配置方式有三种：基于 XML 的配置、基于注解的配置、基于 Java 的配置。

主要由以下几个模块组成：

Spring Core：核心类库，提供 IOC 服务；

Spring Context：提供框架式的 Bean 访问方式，以及企业级功能（ JNDI、定时任务等）；

Spring AOP：AOP 服务；

Spring DAO：对 JDBC 的抽象，简化了数据访问异常的处理；

Spring ORM：对现有的 ORM 框架的支持；

Spring Web：提供了基本的面向 Web 的综合特性，例如多方文件上传；

Spring MVC：提供面向 Web 应用的 Model - View - Controller 实现。


### 6、说说你对 Spring MVC 的理解

**什么是 MVC 模式**

MVC：MVC 是一种设计模式

MVC 的原理图：

![](https://dream-syz.github.io/4-1.png)

<img src="C:\Users\FAN\AppData\Roaming\Typora\typora-user-images\image-20230824015211162.png" alt="image-20230824015211162" style="zoom: 67%;" />

**分析：**

M - Model 模型（完成业务逻辑：有 javaBean 构成，service + dao + entity）

V - View 视图（做界面的展示 jsp，html……）

C - Controller 控制器（接收请求 —> 调用模型 —> 根据结果派发页面）

SpringMVC 是一个 MVC 的开源框架，SpringMVC = struts2 + spring，SpringMVC 就相当于是 Struts2 加上 spring 的整合，但是这里有一个疑惑就是，SpringMVC 和 spring 是什么样的关系呢？这个在百度百科上有一个很好的解释：意思是说，SpringMVC 是 spring 的一个后续产品，其实就是 spring 在原有基础上，又提供了 web 应用的 MVC 模块，可以简单的把 SpringMVC 理解为是 spring 的一个模块

（类似 AOP，IOC 这样的模块），网络上经常会说 SpringMVC 和 spring 无缝集成，其实 SpringMVC 就是 spring 的一个子模块，所以根本不需要同 spring 进行整合。

工作原理：

![](https://dream-syz.github.io/4-2.png)

1、 用户发送请求至前端控制器 DispatcherServlet。

2、 DispatcherServlet 收到请求调用 HandlerMapping 处理器映射器。

3、 处理器映射器找到具体的处理器（可以根据 xml 配置、注解进行查找），生成处理器对象及处理器拦截器（如果有则生成）一并返回给 DispatcherServlet。

4、 DispatcherServlet 调用 HandlerAdapter 处理器适配器。

5、 HandlerAdapter 经过适配调用具体的处理器（Controller，也叫后端控制器）。

6、 Controller 执行完成返回 ModelAndView。

7、 HandlerAdapter 将 controller 执行结果 ModelAndView 返回给 DispatcherServlet。

8、 DispatcherServlet 将 ModelAndView 传给 ViewReslover 视图解析器。

9、 ViewReslover 解析后返回具体 View。

10、DispatcherServlet 根据 View 进行渲染视图（即将模型数据填充至视图中）。

11、 DispatcherServlet 响应用户。

**组件说明** 以下组件通常使用框架提供实现：

DispatcherServlet：作为前端控制器，整个流程控制的中心，控制其它组件执行，统一调度，降低组件之间的耦合性，提高每个组件的扩展性。

HandlerMapping：通过扩展处理器映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

HandlAdapter：通过扩展处理器适配器，支持更多类型的处理器。

ViewResolver：通过扩展视图解析器，支持更多类型的视图解析，例如：jsp、freemarker、pdf、excel 等。

**组件：** **1、前端控制器 DispatcherServlet（不需要工程师开发），由框架提供** 作用：接收请求，响应结果，相当于转发器，中央处理器。有了 DispatcherServlet 减少了其它组件之间的耦合度。 用户请求到达前端控制器，它就相当于 mvc 模式中的 c，DispatcherServlet 是整个流程控制的中心，由它调用其它组件处理用户的请求，DispatcherServlet 的存在降低了组件之间的耦合性。

**2、处理器映射器 HandlerMapping（不需要工程师开发），由框架提供** 作用：根据请求的 url 查找 Handler，HandlerMapping 负责根据用户请求找到 Handler 即处理器，springmvc 提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

**3、处理器适配器 HandlerAdapter** 作用：按照特定规则（HandlerAdapter 要求的规则）去执行 Handler，通过 HandlerAdapter 对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。

**4、处理器 Handler（需要工程师开发）注意：编写 Handler 时按照 HandlerAdapter 的要求去做，这样适配器才可以去正确执行Handler** Handler 是继 DispatcherServlet 前端控制器的后端控制器，在 DispatcherServlet 的控制下 Handler 对具体的用户请求进行处理。 由于 Handler 涉及到具体的用户业务请求，所以一般情况需要工程师根据业务需求开发 Handler。

**5、视图解析器 View resolver（不需要工程师开发），由框架提供** 作用：进行视图解析，根据逻辑视图名解析成真正的视图（view） View Resolver 负责将处理结果生成 View 视图，View Resolver 首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成 View视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户。 springmvc 框架提供了很多的 View 视图类型，包括：jstlView、freemarkerView、pdfView 等。 一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由工程师根据业务需求开发具体的页面。

**6、视图 View（需要工程师开发 jsp...）** View 是一个接口，实现类支持不同的 View 类型（jsp、freemarker、pdf...）

**核心架构的具体流程步骤如下：** 1、首先用户发送请求 ——> DispatcherServlet，前端控制器收到请求后自己不进行处理，而是委托给其他的解析器进行处理，作为统一访问点，进行全局的流程控制； 2、DispatcherServlet ——> HandlerMapping， HandlerMapping 将会把请求映射为 HandlerExecutionChain 对象（包含一个 Handler 处理器（页面控制器）对象、多个 HandlerInterceptor 拦截器）对象，通过这种策略模式，很容易添加新的映射策略； 3、DispatcherServlet ——> HandlerAdapter，HandlerAdapter 将会把处理器包装为适配器，从而支持多种类型的处理器，即适配器设计模式的应用，从而很容易支持很多类型的处理器； 4、HandlerAdapter ——> 处理器功能处理方法的调用，HandlerAdapter 将会根据适配的结果调用真正的处理器的功能处理方法，完成功能处理；并返回一个ModelAndView 对象（包含模型数据、逻辑视图名）； 5、ModelAndView 的逻辑视图名 ——> ViewResolver， ViewResolver 将把逻辑视图名解析为具体的 View，通过这种策略模式，很容易更换其他视图技术； 6、View ——> 渲染，View 会根据传进来的 Model 模型数据进行渲染，此处的 Model 实际是一个 Map 数据结构，因此很容易支持其他视图技术； 7、返回控制权给 DispatcherServlet，由DispatcherServlet 返回响应给用户，到此一个流程结束。

看到这些步骤我相信大家很感觉非常的乱，这是正常的，但是这里主要是要大家理解 springMVC 中的几个组件：

前端控制器（DispatcherServlet）：接收请求，响应结果，相当于电脑的 CPU。

处理器映射器（HandlerMapping）：根据 URL 去查找处理器。

处理器（Handler）：需要程序员去写代码处理逻辑的。

处理器适配器（HandlerAdapter）：会把处理器包装成适配器，这样就可以支持多种类型的处理器，类比笔记本的适配器（适配器模式的应用）。

视图解析器（ViewResovler）：进行视图解析，多返回的字符串，进行处理，可以解析成对应的页面



### 7、SpringMV 常用的注解有哪些？

@RequestMapping：用于处理请求 url 映射的注解，可用于类或方法上。用于类上，则表示类中的所有响应请求的方法都是以该地址作为父路径。

@RequestBody：注解实现接收 http 请求的 json 数据，将 json 转换为 java 对象。

@ResponseBody：注解实现将 conreoller 方法返回对象转化为 json 对象响应给客户。



### 8、 谈谈你对 Spring AOP 的理解

AOP（Aspect - Oriented Programming，面向切面编程）能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可扩展性和可维护性。

Spring AOP是基于动态代理的，如果要代理的对象实现了某个接口，那么 Spring AOP 就会使用 JDK 动态代理去创建代理对象；而对于没有实现接口的对象，就无法使用 JDK 动态代理，转而使用 CGlib 动态代理生成一个被代理对象的子类来作为代理。

![](https://dream-syz.github.io/4-3.png)

注意：图中的 implements 和 extend。即一个是接口，一个是实现类。

当然也可以使用 AspectJ，Spring AOP 中已经集成了 AspectJ，AspectJ 应该算得上是 Java 生态系统中最完整的 AOP 框架了。使用 AOP 之后我们可以把一些通用功能抽象出来，在需要用到的地方直接使用即可，这样可以大大简化代码量。我们需要增加新功能也方便，提高了系统的扩展性。日志功能、事务管理和权限管理等场景都用到了 AOP。

这里只要你提到了 AspectJ，那么面试官很有可能会继续问：



### 9、Spring AOP 和 AspectJ AOP 有什么区别？

Spring AOP 是属于运行时增强，而 AspectJ 是编译时增强。Spring AOP 基于代理（Proxying），而 AspectJ 基于字节码操作（Bytecode Manipulation）。

Spring AOP 已经集成了 AspectJ，AspectJ 应该算得上是 Java 生态系统中最完整的 AOP 框架了。

AspectJ 相比于 Spring AOP 功能更加强大，但是 Spring AOP 相对来说更简单。

如果我们的切面比较少，那么两者性能差异不大。但是，当切面太多的话，最好选择 AspectJ，它比 Spring AOP 快很多。

可能还会继续问：

#### **在 Spring AOP 中，关注点和横切关注的区别是什么？**

关注点是应用中一个模块的行为，一个关注点可能会被定义成一个我们想实现的一个功能。 横切关注点是一个关注点，此关注点是整个应用都会使用的功能，并影响整个应用，比如日志，安全和数据传输，几乎应用的每个模块都需要的功能。因此这些都属于横切关注点。

那什么是连接点呢？连接点代表一个应用程序的某个位置，在这个位置我们可以插入一个 AOP 切面，它实际上是个应用程序执行 Spring AOP 的位置。切入点是什么？切入点是一个或一组连接点，通知将在这些位置执行。可以通过表达式或匹配的方式指明切入点。

#### **什么是通知呢？有哪些类型呢？**

通知是个在方法执行前或执行后要做的动作，实际上是程序执行时要通过 Spring AOP 框架触发的代码段。

Spring 切面可以应用五种类型的通知：

- **before：**前置通知，在一个方法执行前被调用。

- **after：**在方法执行之后调用的通知，无论方法执行是否成功。

- **after-returning：**仅当方法成功完成后执行的通知。

- **after-throwing：**在方法抛出异常退出时执行的通知。

- **around：**在方法执行之前和之后调用的通知。



### 10、说说你对 Spring IOC 是怎么理解的？

（1）IOC 就是控制反转，是指创建对象的控制权的转移。以前创建对象的主动权和时机是由自己把控的，而现在这种权力转移到 Spring容器中，并由容器根据配置文件去创建实例和管理各个实例之间的依赖关系。对象与对象之间松散耦合，也利于功能的复用。DI 依赖注入，和控制反转是同一个概念的不同角度的描述，即应用程序在运行时依赖 IOC 容器来动态注入对象需要的外部资源。

（2）最直观的表达就是，IOC 让对象的创建不用去 new 了，可以由 spring 自动生产，使用 java 的反射机制，根据配置文件在运行时动态的去创建对象以及管理对象，并调用对象的方法的。

（3）Spring 的 IOC 有三种注入方式 ：构造器注入、setter方法注入、根据注解注入。

IOC 让相互协作的组件保持松散的耦合，而 AOP 编程允许你把遍布于应用各层的功能分离出来形成可重用的功能组件。



### 11、解释一下 spring bean 的生命周期

首先说一下 Servlet 的生命周期：实例化，初始 init，接收请求 service，销毁 destroy；

Spring 上下文中的 Bean 生命周期也类似，如下：

#### （1）实例化 Bean：

对于 BeanFactory 容器，当客户向容器请求一个尚未初始化的 bean 时，或初始化 bean 的时候需要注入另一个尚未初始化的依赖时，容器就会调用 createBean 进行实例化。对于 ApplicationContext 容器，当容器启动结束后，通过获取 BeanDefinition 对象中的信息，实例化所有的 bean。

#### （2）设置对象属性（依赖注入）：

实例化后的对象被封装在 BeanWrapper 对象中，紧接着，Spring 根据 BeanDefinition 中的信息以及通过 BeanWrapper 提供的设置属性的接口完成依赖注入。

#### （3）处理 Aware 接口：

接着，Spring 会检测该对象是否实现了 xxxAware 接口，并将相关的 xxxAware 实例注入给 Bean：

①如果这个 Bean 已经实现了 BeanNameAware 接口，会调用它实现的 setBeanName(StringbeanId) 方法，此处传递的就是 Spring 配置文件中 Bean 的 id 值；

②如果这个 Bean 已经实现了 BeanFactoryAware 接口，会调用它实现的 setBeanFactory() 方法，传递的是 Spring 工厂自身。

③如果这个 Bean 已经实现了 ApplicationContextAware 接口，会调用 setApplicationContext(ApplicationContext) 方法，传入 Spring 上下文；

#### （4）BeanPostProcessor：

如果想对 Bean 进行一些自定义的处理，那么可以让 Bean 实现了 BeanPostProcessor 接口，那将会调用postProcessBeforeInitialization(Object obj, String s) 方法。

#### （5）InitializingBean 与 init - method：

如果 Bean 在 Spring 配置文件中配置了 init - method 属性，则会自动调用其配置的初始化方法。

#### （6）如果这个Bean实现了 BeanPostProcessor 接口：

将会调用 postProcessAfterInitialization(Object obj, String s) 方法；由于这个方法是在 Bean 初始化结束时调用的，所以可以被应用于内存或缓存技术；

以上几个步骤完成后，Bean就已经被正确创建了，之后就可以使用这个 Bean 了。

#### （7）DisposableBean：

当 Bean 不再需要时，会经过清理阶段，如果 Bean 实现了 DisposableBean 这个接口，会调用其实现的 destroy() 方法；

#### （8）destroy - method：

最后，如果这个 Bean 的 Spring 配置中配置了 destroy - method 属性，会自动调用其配置的销毁方法。

![](https://dream-syz.github.io/4-4.png)



### 12、解释 Spring 支持的几种 bean 的作用域？

Spring 容器中的 bean 可以分为5个范围：

（1）singleton：默认，每个容器中只有一个bean 的实例，单例的模式由 BeanFactory 自身来维护。

（2）prototype：为每一个 bean 请求提供一个实例。

（3）request：为每一个网络请求创建一个实例，在请求完成以后，bean 会失效并被垃圾回收器回收。

（4）session：与 request 范围类似，确保每个 session 中有一个 bean 的实例，在 session 过期后，bean 会随之失效。

（5）global - session：全局作用域，global - session 和 Portlet 应用相关。当你的应用部署在 Portlet 容器中工作时，它包含很多portlet。如果你想要声明让所有的 portlet 共用全局的存储变量的话，那么这全局变量需要存储在 global - session 中。全局作用域与Servlet 中的 session 作用域效果相同。



### 13、Spring 基于 xml 注入 bean 的几种方式？

（1）Set 方法注入；

（2）构造器注入：①通过 index 设置参数的位置；②通过 type 设置参数类型；

（3）静态工厂注入；

（4）实例工厂；

通常回答前面两种即可，因为后面两种很多人都不太会，不会的就不要说出来，不然问到你不会就尴尬了。



### 14、Spring 框架中都用到了哪些设计模式？

这是一道相对有难度的题目，你不仅要回设计模式，还要知道每个设计模式在 Spring 中是如何使用的。

**简单工厂模式**：Spring 中的 BeanFactory 就是简单工厂模式的体现。根据传入一个唯一的标识来获得 Bean 对象，但是在传入参数后创建还是传入参数前创建，要根据具体情况来定。

**工厂模式**：Spring 中的 FactoryBean 就是典型的工厂方法模式，实现了 FactoryBean 接口的 bean 是一类叫做 factory 的 bean。其特点是，spring 在使用 getBean() 调用获得该 bean 时，会自动调用该 bean 的 getObject() 方法，所以返回的不是 factory 这个 bean，而是这个 bean.getOjbect() 方法的返回值。

**单例模式**：在 spring 中用到的单例模式有： scope="singleton" ，注册式单例模式，bean 存放于 Map 中。bean name 当做 key，bean 当做 value。

**原型模式**：在 spring 中用到的原型模式有： scope="prototype" ，每次获取的是通过克隆生成的新实例，对其进行修改时对原有实例对象不造成任何影响。

**迭代器模式**：在 Spring 中有个 CompositeIterator 实现了 Iterator，Iterable 接口和 Iterator 接口，这两个都是迭代相关的接口。可以这么认为，实现了 Iterable 接口，则表示某个对象是可被迭代的。Iterator 接口相当于是一个迭代器，实现了 Iterator 接口，等于具体定义了这个可被迭代的对象时如何进行迭代的。

**代理模式**：Spring 中经典的 AOP，就是使用动态代理实现的，分 JDK 和 CGlib 动态代理。

**适配器模式**：Spring 中的 AOP 中 `AdvisorAdapter` 类，它有三个实现：

`MethodBeforAdviceAdapter`、`AfterReturnningAdviceAdapter`、`ThrowsAdviceAdapter`。Spring 会根据不同的 AOP 配置来使用对应的 Advice，与策略模式不同的是，一个方法可以同时拥有多个 Advice。Spring 存在很多以 Adapter 结尾的，大多数都是适配器模式。

**观察者模式**：Spring 中的 Event 和 Listener。spring 事件：ApplicationEvent，该抽象类继承了 EventObject 类，JDK 建议所有的事件都应该继承自 EventObject。spring 事件监听器：ApplicationListener，该接口继承了 EventListener 接口，JDK 建议所有的事件监听器都应该继承 EventListener。

**模板模式**：Spring 中的 org.springframework.jdbc.core.JdbcTemplate 就是非常经典的模板模式的应用，里面的 execute 方法，把整个算法步骤都定义好了。

**责任链模式**：DispatcherServlet 中的 doDispatch() 方法中获取与请求匹配的处理器 HandlerExecutionChain，this.getHandler() 方法的处理使用到了责任链模式。

**注意**：这里只是列举了部分设计模式，其实里面用到了还有享元模式、建造者模式等。可选择性的回答，主要是怕你回答了迭代器模式，然后继续问你，结果你一问三不知，那就尴了尬了。



### 15、说说 Spring 中 ApplicationContext 和 BeanFactory 的区别

**类图**

![](https://dream-syz.github.io/4-5.png)

**包目录不同**

spring-beans.jar 中 org.springframework.beans.factory.BeanFactory

spring-context.jar 中 org.springframework.context.ApplicationContext

**国际化**

BeanFactory 是不支持国际化功能的，因为 BeanFactory 没有扩展 Spring 中 MessageResource 接口。相反，由于 ApplicationContext 扩展了 MessageResource 接口，因而具有消息处理的能力（i18N）。

**强大的事件机制（Event）**

基本上牵涉到事件（Event）方面的设计，就离不开观察者模式，ApplicationContext 的事件机制主要通过 ApplicationEvent 和 ApplicationListener 这两个接口来提供的，和 Java swing 中的事件机制一样。即当 ApplicationContext 中发布一个事件时，所有扩展了 ApplicationListener 的 Bean 都将接受到这个事件，并进行相应的处理。

**底层资源的访问**

ApplicationContext 扩展了 ResourceLoader（资源加载器）接口，从而可以用来加载多个 Resource，而 BeanFactory 是没有扩展 ResourceLoader。

**对** **Web** **应用的支持**

与 BeanFactory 通常以编程的方式被创建，ApplicationContext 能以声明的方式创建，如使用 ContextLoader。当然你也可以使用 ApplicationContext 的实现方式之一，以编程的方式创建 ApplicationContext 实例。

**延迟加载**

1. BeanFactroy 采用的是延迟加载形式来注入 Bean 的，即只有在使用到某个 Bean 时(调用 getBean())，才对该 Bean 进行加载实例化。这样，我们就不能发现一些存在的 spring 的配置问题。而 ApplicationContext 则相反，它是在容器启动时，一次性创建了所有的 Bean。这样，在容器启动时，我们就可以发现 Spring 中存在的配置错误。

2. BeanFactory 和 ApplicationContext 都支持 BeanPostProcessor、BeanFactoryPostProcessor 的使用。两者之间的区别是：BeanFactory 需要手动注册，而 ApplicationContext 则是自动注册。

可以看到，ApplicationContext 继承了 BeanFactory，BeanFactory 是 Spring 中比较原始的 Factory，它不支持 AOP、Web 等 Spring 插件。而 ApplicationContext 不仅包含了 BeanFactory 的所有功能，还支持 Spring 的各种插件，还以一种面向框架的方式工作以及对上下文进行分层和实现继承。

BeanFactory 是 Spring 框架的基础设施，面向 Spring 本身；而 ApplicationContext 面向使用 Spring 的开发者，相比 BeanFactory 提供了更多面向实际应用的功能，几乎所有场合都可以直接使用 ApplicationContext，而不是底层的 BeanFactory。

**常用容器**

BeanFactory 类型的有 XmlBeanFactory，它可以根据 XML 文件中定义的内容，创建相应的 Bean。

ApplicationContext 类型的常用容器有：

1. ClassPathXmlApplicationContext：从 ClassPath 的 XML 配置文件中读取上下文，并生成上下文定义。应用程序上下文从程序环境变量中取得。

2. FileSystemXmlApplicationContext：由文件系统中的 XML 配置文件读取上下文。
3. XmlWebApplicationContext：由 Web 应用的 XML 文件读取上下文。例如我们在 Spring MVC 使用的情况。



### 16、Spring 框架中的单例 Bean 是线程安全的么？

Spring 框架并没有对单例 Bean 进行任何多线程的封装处理。

关于单例 Bean 的线程安全和并发问题，需要开发者自行去搞定。

单例的线程安全问题，并不是 Spring 应该去关心的。Spring 应该做的是，提供根据配置，创建单例 Bean 或多例 Bean 的功能。当然，但实际上，大部分的 Spring Bean 并没有可变的状态，所以在某种程度上说 Spring 的单例 Bean 是线程安全的。如果你的 Bean 有多种状态的话，就需要自行保证线程安全。最浅显的解决办法，就是将多态 Bean 的作用域（Scope）由 Singleton 变更为 Prototype。



### 17、Spring 是怎么解决循环依赖的？

**循环依赖**

**定义：**循环依赖其实就是循环引用，也就是两个或两个以上的 bean 对象互相持有对方，最终形成闭环。比如 A 依赖 B，B 依赖 C，C 又依赖 A，形成循环依赖

**出现场景：**1、构造器的循环依赖 2、Filed 属性的循环依赖

**如何检测：**在创建 Bean 的时候可以给该 Bean 打标签，如果递归调用回来发现正在创建中的话，即说明发生了循环依赖

**Spring 单例对象初始化步骤：**

1、createBeanInstance 实例化：调用对象的构造方法实例化对象

2、populateBean 属性填充：这一步主要是对多 Bean 的依赖属性进行填充

3、initializeBean 初始化：调用applicationContext.xml 中的初始化方法

**如何解决：**

使用三级缓存

singletonFactories：单例对象工厂的 cache 缓存
earlySingletonObjects：提前曝光的单例对象的 cache 缓存
singletonObjects：单例对象的 cache 缓存

Spring 首先从一级缓存 singletonObjects 中获取对象，如果获取不到并且对象正在创建中，就再从二级存 earlySingletonObiects 中获取，如果还是获取不到且允许 singletonFactories 通过 getObiect() 获取，就从三级缓存 singletonFactory 中获取，如果获取到了就从singletonFactories 三级缓存中移除掉，并放入 earlySingletonObiects 中，其实也就是从三级缓存移到了二级缓存中

**构造器的循环依赖问题无法解决，需通过懒加载**

整个流程大致如下：

1. 首先 A 完成初始化第一步并将自己 **提前曝光** 出来（通过 ObjectFactory 将自己提前曝光），在初始化的时候，发现自己依赖对象 B，此时就会去尝试 get(B)，这个时候发现 B 还没有被创建出来；

2. 然后 B 就走创建流程，在 B 初始化的时候，同样发现自己依赖 C，C 也没有被创建出来；
3. 这个时候 C 又开始初始化进程，但是在初始化的过程中发现自己依赖 A，于是尝试 get(A)。这个时候由于 A 已经添加至缓存中（一般都是添加至三级缓存 singletonFactories），通过 ObjectFactory 提前曝光，所以可以通过 ObjectFactory#getObject() 方法来拿到 A 对象。C 拿到 A 对象后顺利完成初始化，然后将自己添加到一级缓存中；

4. 回到 B，B 也可以拿到 C 对象，完成初始化，A 可以顺利拿到 B 完成初始化。到这里整个链路就已经完成了初始化过程了。

关键字：三级缓存，提前曝光。



### 18、说说事务的隔离级别

未提交读（Read Uncommitted）：允许脏读，也就是可能读取到其他会话中未提交事务修改的数据

提交读（Read Committed）：只能读取到已经提交的数据。Oracle 等多数数据库默认都是该级别 (不重复读)

可重复读（Repeated Read）：在同一个事务内的查询都是事务开始时刻一致的，MySQL 的 InnoDB 默认级别。在 SQL 标准中，该隔离级别消除了不可重复读，但是还存在幻读（多个事务同时修改同一条记录，事务之间不知道彼此存在，当事务提交之后，后面的事务修改的数据将会覆盖前事务，前一个事务就像发生幻觉一样）

可串行化（Serializable）：完全串行化的读，每次读都需要获得表级共享锁，读写相互都会阻塞。

| 事务隔离级别 | 脏读 | 不可重复读 | 幻读 |
| ------------ | ---- | ---------- | ---- |
| 读未提交     | 允许 | 允许       | 允许 |
| 读已提交     | 禁止 | 允许       | 允许 |
| 可重复读     | 禁止 | 禁止       | 允许 |
| 顺序读       | 禁止 | 禁止       | 禁止 |

不可重复读和幻读的区别主要是：解决不可重复读需要锁定当前满足条件的记录，而解决幻读需要锁定当前满足条件的记录及相近的记录。比如查询某个商品的信息，可重复读事务隔离级别可以保证当前商品信息被锁定，解决不可重复读；但是如果统计商品个数，中途有记录插入，可重复读事务隔离级别就不能保证两个事务统计的个数相同。



### 19、说说事务的传播级别

Spring 事务定义了 7 种传播机制：

1. PROPAGATION_REQUIRED：默认的 Spring 事物传播级别，若当前存在事务，则加入该事务，若不存在事务，则新建一个事务。

2. PAOPAGATION_REQUIRE_NEW：若当前没有事务，则新建一个事务。若当前存在事务，则新建一个事务，新老事务相互独立。外部事务抛出异常回滚不会影响内部事务的正常提交。

3. PROPAGATION_NESTED：如果当前存在事务，则嵌套在当前事务中执行。如果当前没有事务，则新建一个事务，类似于REQUIRE_NEW。

4. PROPAGATION_SUPPORTS：支持当前事务，若当前不存在事务，以非事务的方式执行。
5. PROPAGATION_NOT_SUPPORTED：以非事务的方式执行，若当前存在事务，则把当前事务挂起。

6. PROPAGATION_MANDATORY：强制事务执行，若当前不存在事务，则抛出异常.
7. PROPAGATION_NEVER：以非事务的方式执行，如果当前存在事务，则抛出异常。

Spring 事务传播级别一般不需要定义，默认就是 PROPAGATION_REQUIRED，除非在嵌套事务的情况下需要重点了解。



### 20、Spring 事务实现方式

编程式事务管理：这意味着你可以通过编程的方式管理事务，这种方式带来了很大的灵活性，但很难维护。

声明式事务管理：这种方式意味着你可以将事务管理和业务代码分离。你只需要通过注解或者 XML 配置管理事务。



### 21、Spring 框架的事务管理有哪些优点

它为不同的事务 API（如 JTA、 JDBC、Hibernate、 JPA 和 JDO）提供了统一的编程模型。它为编程式事务管理提供了一个简单的 API 而非一系列复杂的事务 API（如 JTA），它支持声明式事务管理。它可以和 Spring 的多种数据访问技术很好的融合。



### 22、事务三要素是什么？

**数据源**：表示具体的事务性资源，是事务的真正处理者，如 MySQL 等。

**事务管理器**：像一个大管家，从整体上管理事务的处理过程，如打开、提交、回滚等。

**事务应用和属性配置**：像一个标识符，表明哪些方法要参与事务，如何参与事务，以及一些相关属性如隔离级别、超时时间等。



### 23、 事务注解的本质是什么？

@Transactional 这个注解仅仅是一些（和事务相关的）元数据，在运行时被事务基础设施读取消费，并**使用这些元数据来配置 bean 的事务行为**。 大致来说具有两方面功能，**一是表明该方法要参与事务，二是配置相关属性来定制事务的参与方式和运行行为** 

声明式事务主要是得益于 Spring AOP。使用一个事务拦截器，在方法调用的前后 / 周围进行事务性增强（advice），来驱动事务完成。

@Transactional 注解既可以标注在类上，也可以标注在方法上。当在类上时，默认应用到类里的所有方法。如果此时方法上也标注了，则方法上的优先级高。 另外注意方法一定要是 public 的。





## MyBatis 篇

### 1、什么是 MyBatis

（1）Mybatis 是一个半 ORM（对象关系映射）框架，它内部封装了 JDBC，开发时只需要关注 SQL 语句本身，不需要花费精力去处理加载驱动、创建连接、创建 statement 等繁杂的过程。程序员直接编写原生态 sql，可以严格控制 sql 执行性能，灵活度高。

（2）MyBatis 可以使用 XML 或注解来配置和映射原生信息，将 POJO 映射成数据库中的记录，避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。

（3）通过 xml 文件或注解的方式将要执行的各种 statement 配置起来，并通过 java 对象和 statement 中 sql 的动态参数进行映射生成最终执行的 sql 语句，最后由 mybatis 框架执行 sql 并将结果映射为 java 对象并返回。（从执行 sql 到返回 result 的过程）。



### 2、说说 MyBatis 的优点和缺点

**优点：**

（1）基于 SQL 语句编程，相当灵活，不会对应用程序或者数据库的现有设计造成任何影响，SQL 写在 XML 里，解除 sql 与程序代码的耦合，便于统一管理；提供 XML 标签，支持编写动态 SQL 语句，并可重用。

（2）与 JDBC 相比，减少了 50 % 以上的代码量，消除了 JDBC 大量冗余的代码，不需要手动开关连接；

（3）很好的与各种数据库兼容（因为 MyBatis 使用 JDBC 来连接数据库，所以只要 JDBC 支持的数据库 MyBatis 都支持）。

（4）能够与 Spring 很好的集成；

（5）提供映射标签，支持对象与数据库的 ORM 字段关系映射；提供对象关系映射标签，支持对象关系组件维护。

**缺点**

（1）SQL 语句的编写工作量较大，尤其当字段多、关联表多时，对开发人员编写 SQL 语句的功底有一定要求。

（2）SQL 语句依赖于数据库，导致数据库移植性差，不能随意更换数据库。



### 3、#{} 和 ${} 的区别是什么？

\#{} 是预编译处理，${} 是字符串替换。

Mybatis 在处理 #{} 时，会将 sql 中的 #{} 替换为 ? 号，调用 PreparedStatement 的 set 方法来赋值；

Mybatis 在处理 ${} 时，就是把 ${} 替换成变量的值。

使用 #{} 可以有效的防止 SQL 注入，提高系统安全性。



### 4、当实体类中的属性名和表中的字段名不一样 ，怎么办 ？

第 1 种：通过在查询的 sql 语句中定义字段名的别名，让字段名的别名和实体类的属性名一致。

```xml
<select id=”selectorder” parametertype=”int” resultetype=”me.gacl.domain.order”>
  select order_id id, order_no orderno ,order_price price form orders where order_id=#{id};
</select>
```

第 2 种：通过来映射字段名和实体类属性名的一一对应的关系。

```xml
 <select id="getOrder" parameterType="int" resultMap="orderresultmap">
   select * from orders where order_id=#{id}
 </select>
 <resultMap type=”me.gacl.domain.order” id=”orderresultmap”>
 <!–用id属性来映射主键字段–>
 <id property=”id” column=”order_id”>
 <!–用result属性来映射非主键字段，property为实体类属性名，column为数据表中的属性–>
 <result property = “orderno” column =”order_no”/>
 <result property=”price” column=”order_price” />
 </reslutMap>
```



### 5、Mybatis 是如何进行分页的？分页插件的原理是什么？

Mybatis 使用 RowBounds 对象进行分页，它是针对 ResultSet 结果集执行的内存分页，而非物理分页。可以在 sql 内直接拼写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页，比如：MySQL 数据的时候，在原有 SQL 后面拼写 limit。

分页插件的基本原理是使用 Mybatis 提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的 sql，然后重写 sql，根据dialect 方言，添加对应的物理分页语句和物理分页参数。



### 6、Mybatis 是如何将 sql 执行结果封装为目标对象并返回的？都有哪些映射形式？

第一种是使用标签，逐一定义数据库列名和对象属性名之间的映射关系。

第二种是使用 sql 列的别名功能，将列的别名书写为对象属性名。

有了列名与属性名的映射关系后，Mybatis 通过反射创建对象，同时使用反射给对象的属性逐一赋值并返回，那些找不到映射关系的属性，是无法完成赋值的。



### 7、 如何执行批量插入？

首先,创建一个简单的 insert 语句:

```xml
<insert id=”insertname”>
 insert into names (name) values (#{value})
</insert>
```

然后在 java 代码中像下面这样执行批处理插入:

```java
list<string> names = new arraylist();
 names.add(“fred”);
 names.add(“barney”);
 names.add(“betty”);
 names.add(“wilma”);
 // 注意这里 executortype.batch
 sqlsession sqlsession = sqlsessionfactory.opensession(executortype.batch);
 try {
 	namemapper mapper = sqlsession.getmapper(namemapper.class);
 	for (string name : names) {
 		mapper.insertname(name);
 	}
	 sqlsession.commit();
 }catch(Exception e){
	 e.printStackTrace();
 	 sqlSession.rollback(); 
	 throw e; 
 }
 finally {
 	sqlsession.close();
 }
```



### 8、Xml 映射文件中，除了常见的 select|insert|updae|delete 标签之外，还有哪些标签？

加上动态 sql 的 9 个标签，其中为 sql 片段标签，通过标签引入 sql 片段，为不支持自增的主键生成策略标签。



### 9、MyBatis 实现一对一有几种方式？具体怎么操作的？

有联合查询和嵌套查询，联合查询是几个表联合查询，只查询一次，通过在 resultMap 里面配置 association 节点配置一对一的类就可以完成；

嵌套查询是先查一个表，根据这个表里面的结果的外键 id，去再另外一个表里面查询数据,也是通过 association 配置，但另外一个表的查询通过 select 属性配置。



### 10、Mybatis 是否支持延迟加载？如果支持，它的实现原理是什么？

Mybatis 仅支持 association 关联对象和 collection 关联集合对象的延迟加载，association 指的就是一对一，collection 指的就是一对多查询。在 Mybatis 配置文件中，可以配置是否启用延迟加载 lazyLoadingEnabled = true|false。

它的原理是，使用 CGLIB 创建目标对象的代理对象，当调用目标方法时，进入拦截器方法，比如调用 a.getB().getName()，拦截器invoke() 方法发现 a.getB() 是 null 值，那么就会单独发送事先保存好的查询关联 B 对象的 sql，把 B 查询上来，然后调用 a.setB(b)，于是 a 的对象 b 属性就有值了，接着完成 a.getB().getName() 方法的调用。这就是延迟加载的基本原理。

当然了，不光是 Mybatis，几乎所有的包括 Hibernate，支持延迟加载的原理都是一样的。



### 11、说说 Mybatis 的缓存机制

Mybatis 整体：

![](https://dream-syz.github.io/5-1.png)

**一级缓存 localCache**

在应用运行过程中，我们有可能在一次数据库会话中，执行多次查询条件完全相同的 SQL，MyBatis 提供了一级缓存的方案优化这部分场景，如果是相同的 SQL 语句，会优先命中一级缓存，避免直接对数据库进行查询，提高性能。

每个 SqlSession 中持有了 Executor，每个 Executor 中有一个 LocalCache。当用户发起查询时，MyBatis 根据当前执行的语句生成 MappedStatement，在 Local Cache 进行查询，如果缓存命中的话，直接返回结果给用户，如果缓存没有命中的话，查询数据库，结果写入 Local Cache，最后返回结果给用户。具体实现类的类关系图如下图所示：

![](https://dream-syz.github.io/5-2.png)

<img src="D:\文档\WeChat Files\wxid_hrkocnb2rcbm22\FileStorage\Temp\1692984622141.jpg" alt="1692984622141" style="zoom:80%;" />

1. MyBatis 一级缓存的生命周期和 SqlSession 一致。
2. MyBatis 一级缓存内部设计简单，只是一个没有容量限定的 HashMap，在缓存的功能性上有所欠缺。

3. MyBatis 的一级缓存最大范围是 SqlSession 内部，有多个 SqlSession 或者分布式的环境下，数据库写操作会引起脏数据，建议设定缓存级别为 Statement。

**二级缓存**

在上文中提到的一级缓存中，其最大的共享范围就是一个 SqlSession 内部，如果多个 SqlSession之间需要共享缓存，则需要使用到二级缓存。开启二级缓存后，会使用 CachingExecutor 装饰 Executor，进入一级缓存的查询流程前，先在 CachingExecutor 进行二级缓存的查询，具体的工作流程如下所示。

![](https://dream-syz.github.io/5-3.png)

二级缓存开启后，同一个 namespace 下的所有操作语句，都影响着同一个 Cache，即二级缓存被多个 SqlSession 共享，是一个全局的变量。

当开启缓存后，数据的查询执行的流程为：

二级缓存 -> 一级缓存 -> 数据库

1. MyBatis 的二级缓存相对于一级缓存来说，实现了 SqlSession 之间缓存数据的共享，同时粒度更加细，能够到 namespace 级别，通过 Cache 接口实现类不同的组合，对 Cache 的可控性也更强。

2. MyBatis 在多表查询时，极大可能会出现脏数据，有设计上的缺陷，安全使用二级缓存的条件比较苛刻。

3. 在分布式环境下，由于默认的 MyBatis Cache 实现都是基于本地的，分布式环境下必然会出现读取到脏数据，需要使用集中式缓存将 MyBatis 的 Cache 接口实现，有一定的开发成本，直接使用 Redis、Memcached 等分布式缓存可能成本更低，安全性也更高。



### 12、JDBC 编程有哪些步骤？

1. 装载相应的数据库的 JDBC 驱动并进行初始化：

```java
Class.forName("com.mysql.jdbc.Driver");
```

2. 建立 JDBC 和数据库之间的 Connection 连接：

```java
Connection c = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/test?
characterEncoding=UTF-8", "root", "123456");
```

3. 创建 Statement 或者 PreparedStatement 接口，执行 SQL 语句。

4. 处理和显示结果。

5. 释放资源。



### 13、MyBatis 中见过什么设计模式？

![](https://dream-syz.github.io/5-4.png)



**建造者模式：**例如 SqlSessionFactoryBuilder、XMLConfigBuilder、XMLMapperBuilder、XMLStatementBuilder、 CacheBuilder ;

**单例模式：**例如 ErrorContext 和 LogFactory ；

**代理模式：**Mybatis 实现的核心，比如 Mapperroxy、ConnectionLogger，用的 JDK 的动态代理；还有 executor.loader 包使用了 CGlib 或者 javassist 达到延迟加载的效果；

**组合模式：**例如 SqlNode 和各个子类 ChooseSqlNode 等 ;

**模板方法模式：**例如 BaseExecutor 和 SimpleExecutor，还有 BaseTypeHandler 和所有的子类例 IntegerTypeHandler ;

**适配器模式：**例如 Log 的 Mybatis 接口和它对 JDBC、log4j 等各种日志框架的适配实现；

**装饰者模式：**例如 Cache 包中的 cache,decorators 子包中等各个装饰者的实现；

**迭代器模式：**例如迭代器模式 PropertyTokenizer ;

**工厂模式：**例如 SqlSessionFactory、ObjectFactory、MapperProxyFactory ；



### 14、MyBatis 中比如 `UserMapper.java` 是接口，为什么没有实现类还能调用？

使用 JDK 动态代理 + MapperProxy。本质上调用的是 MapperProxy 的 invoke 方法。



## SpringBoot 篇

### 1、为什么要用 SpringBoot

Spring Boot 优点非常多，如：

一、独立运行

Spring Boot 而且内嵌了各种 servlet 容器，Tomcat、Jetty 等，现在不再需要打成 war 包部署到容器中，Spring Boot 只要打成一个可执行的 jar 包就能独立运行，所有的依赖包都在一个 jar 包内。

二、简化配置

spring - boot - starter - web 启动器自动依赖其他组件，简少了 maven 的配置。

三、自动配置 Spring Boot 能根据当前类路径下的类、jar 包来自动配置 bean，如添加一个 `spring-boot-starter-web` 启动器就能拥有 web 的功能，无需其他配置。

四、无代码生成和 XML 配置

Spring Boo t配置过程中无代码生成，也无需 XML 配置文件就能完成所有配置工作，这一切都是借助于条件注解完成的，这也是Spring4.x 的核心功能之一。

五、应用监控

Spring Boot 提供一系列端点可以监控服务及应用，做健康检测。



### 2、Spring Boot 的核心注解是哪个？它主要由哪几个注解组成的？

启动类上面的注解是 **@SpringBootApplication**，它也是 Spring Boot 的核心注解，主要组合包含了以下 3 个注解：

**@SpringBootConfiguration**：组合了 @Configuration 注解，实现配置文件的功能。

**@EnableAutoConfiguration**：打开自动配置的功能，也可以关闭某个自动配置的选项，如关闭数据源自动配置功能： @SpringBootApplication(exclude = { DataSourceAutoConfiguration.class})。

**@ComponentScan**：Spring 组件扫描。



### 3、运行 Spring Boot 有哪几种方式？

1）打包用命令或者放到容器中运行

2）用 Maven / Gradle 插件运行

3）直接执行 main 方法运行



### 4、如何理解 Spring Boot 中的 Starters？

Starters 是什么：

Starters 可以理解为启动器，它包含了一系列可以集成到应用里面的依赖包，你可以一站式集成 Spring 及其他技术，而不需要到处找示例代码和依赖包。如你想使用 Spring JPA 访问数据库，只要加入 `spring-boot-starter-data-jpa` 启动器依赖就能使用了。Starters 包含了许多项目中需要用到的\依赖，它们能快速持续的运行，都是一系列得到支持的管理传递性依赖。

Starters 命名：Spring Boot 官方的启动器都是以 `spring-boot-starter-` 命名的，代表了一个特定的应用类型。第三方的启动器不能以 `spring-boot` 开头命名，它们都被 Spring Boot 官方保留。一般一个第三方的应该这样命名，像 mybatis 的 `mybatis-spring-boot-starter`。

Starters 分类：

1. Spring Boot 应用类启动器

| 启动器名称             | 功能描述                                                     |
| ---------------------- | ------------------------------------------------------------ |
| spring-boot-stater     | 包含自动配置、日志、YAML 的支持                              |
| spring-boot-stater-web | 使用 Spring MVC 构建 web 工程，包含 restful，默认使用 Tomcat 容器 |

2. Spring Boot 生产启动器

| 启动器名称                  | 功能描述                         |
| --------------------------- | -------------------------------- |
| spring-boot-stater-actuator | 提供生产环境特性，能监控管理应用 |

3. Spring Boot 技术类启动器

| 启动器名称                 | 功能描述                           |
| -------------------------- | ---------------------------------- |
| spring-boot-stater-json    | 提供对 JSON 的读写支持             |
| spring-boot-stater-logging | 默认的日志启动器，默认使用 Logback |



### 5、 如何在 Spring Boot 启动的时候运行一些特定的代码？

如果你想在 Spring Boot 启动的时候运行一些特定的代码，你可以实现接口 **ApplicationRunner** 或者 **CommandLineRunner**，这两个接口实现方式一样，它们都只提供了一个 run 方法。

**CommandLineRunner**：启动获取命令行参数



### 6、Spring Boot 需要独立的容器运行吗？

可以不需要，内置了 Tomcat / Jetty 等容器。



### 7、Spring Boot 中的监视器是什么？

Spring boot actuator 是 spring 启动框架中的重要功能之一。Spring boot 监视器可帮助您访问生产环境中正在运行的应用程序的当前状态。有几个指标必须在生产环境中进行检查和监控。即使一些外部应用程序可能正在使用这些服务来向相关人员触发警报消息。监视器模块公开了一组可直接作为 HTTP URL 访问的 REST 端点来检查状态。



### 8、 如何使用 Spring Boot 实现异常处理？

Spring 提供了一种使用 ControllerAdvice 处理异常的非常有用的方法。 我们通过实现一个 ControlerAdvice 类，来处理控制器类抛出的所有异常。



### 9、 你如何理解 Spring Boot 中的 Starters ？

Starters 可以理解为启动器，它包含了一系列可以集成到应用里面的依赖包，你可以一站式集成 Spring 及其他技术，而不需要到处找示例代码和依赖包。如你想使用 Spring JPA 访问数据库，只要加入 `spring-boot-starter-data-jpa` 启动器依赖就能使用了。



### 10、springboot 常用的 starter 有哪些

`spring-boot-starter-web` 嵌入 tomcat 和 web 开发需要 servlet 与 jsp 支持

`spring-boot-starter-data-jpa` 数据库支持

`spring-boot-starter-data-redis` redis 数据库支持

`spring-boot-starter-data-solr` solr 支持

`mybatis-spring-boot-starter` 第三方的 mybatis 集成 starter



### 11、SpringBoot 实现热部署有哪几种方式？

主要有两种方式：

- **Spring Loaded**

- **Spring-boot-devtools**



### 12、 如何理解 Spring Boot 配置加载顺序？

在 Spring Boot 里面，可以使用以下几种方式来加载配置。

1）properties 文件；

2）YAML 文件；

3）系统环境变量；

4）命令行参数；



### 13、Spring Boot 的核心配置文件有哪几个？它们的区别是什么？

Spring Boot 的核心配置文件是 application 和 bootstrap 配置文件。

application 配置文件这个容易理解，主要用于 Spring Boot 项目的自动化配置。

bootstrap 配置文件有以下几个应用场景。

- 使用 Spring Cloud Config 配置中心时，这时需要在 bootstrap 配置文件中添加连接到配置中心的配置属性来加载外部配置中心的配置信息；

- 一些固定的不能被覆盖的属性；

- 一些加密 / 解密的场景；



### 14、如何集成 Spring Boot 和 ActiveMQ ？

集成 Spring Boot 和 ActiveMQ，我们使用 `spring-boot-starter-activemq` 依赖关系。 它只需要很少的配置，并且不需要样板代码。





## MySQL 篇

### 1、数据库的三范式是什么

第一范式：列不可再分 

第二范式：行可以唯一区分，主键约束 

第三范式：表的非主属性不能依赖与其他表的非主属性外键约束且三大范式是一级一级依赖的，第二范式建立在第一范式上，第三范式建立第一第二范式上。



### 2、MySQL 数据库引擎有哪些

如何查看 MySQL 提供的所有存储引擎

```mysql
mysql> show engines;
```



![](https://dream-syz.github.io/6-1.png)

![](https://dream-syz.github.io/6-2.png)

MySQL 常用引擎包括：MYISAM、Innodb、Memory、MERGE

- MYISAM：全表锁，拥有较高的执行速度，不支持事务，不支持外键，并发性能差，占用空间相对较小，对事务完整性没有要求，以select、insert 为主的应用基本上可以使用这引擎

- Innodb：行级锁，提供了具有提交、回滚和崩溃回复能力的事务安全，支持自动增长列，支持外键约束，并发能力强，占用空间是MYISAM 的 2.5 倍，处理效率相对会差一些

- Memory：全表锁，存储在内容中，速度快，但会占用和数据量成正比的内存空间且数据在 MySQL 重启时会丢失，默认使用 HASH索引，检索效率非常高，但不适用于精确查找，主要用于那些内容变化不频繁的代码表

- MERGE：是一组 MYISAM 表的组合



### 3、说说 InnoDB 与 MyISAM 的区别

在 MySQL 5.5 及之前的版本中，MyISAM 是默认的存储引擎，而在 MySQL 5.5 版本以后，默认使用 InnoDB 存储引擎。

1. InnoDB 支持事务，MyISAM 不支持，对于 InnoDB 每一条 SQL 语言都默认封装成事务，自动提交，这样会影响速度，所以最好把多条 SQL 语言放在 begin 和 commit 之间，组成一个事务；InnoDB 需要更多存储空间，会在内存中建立其专用的缓冲池用于高速缓冲数据和索引。InnoDB 支持自动奔溃恢复特性。
2. InnoDB 支持外键，而 MyISAM 不支持。对一个包含外键的 InnoDB 表转为 MYISAM 会失败；
3. InnoDB 是聚集索引，数据文件是和索引绑在一起的，必须要有主键，通过主键索引效率很高。但是辅助索引需要两次查询，先查询到主键，然后再通过主键查询到数据。因此，主键不应该过大，因为主键太大，其他索引也都会很大。而 MyISAM 是非聚集索引，数据文件是分离的，索引保存的是数据文件的指针。主键索引和辅助索引是独立的。
4. InnoDB 不保存表的具体行数，执行 select count(*) from table 时需要全表扫描。而 MyISAM 用一个变量保存了整个表的行数，MyISAM 不支持行级锁，执行上述语句时只需要读出该变量即可，速度很快；
5. Innodb 不支持全文索引，而 MyISAM 支持全文索引，查询效率上 MyISAM 要高；



### 4、数据库的事务

**什么是事务？：** 多条 sql 语句，要么全部成功，要么全部失败。

**事务的特性：**

**数据库事务特性：原子性（Atomic）、一致性（Consistency）、隔离性（Isolation）、持久性（Durabiliy）。简称 ACID。**

- 原子性：组成一个事务的多个数据库操作是一个不可分割的原子单元，只有所有操作都成功，整个事务才会提交。任何一个操作失败，已经执行的任何操作都必须撤销，让数据库返回初始状态。

- 一致性：事务操作成功后，数据库所处的状态和它的业务规则是一致的。即数据不会被破坏。如 A 转账 100 元给 B，不管操作是否成功，A 和 B 的账户总额是不变的。

- 隔离性：在并发数据操作时，不同的事务拥有各自的数据空间，它们的操作不会对彼此产生干扰

- 持久性：一旦事务提交成功，事务中的所有操作都必须持久化到数据库中。



### 5、索引是什么

官方介绍索引是帮助 MySQL **高效获取数据 **的 **数据结构**。更通俗的说，数据库索引好比是一本书前面的目录，能 **加快数据库的查询速度**。

一般来说索引本身也很大，不可能全部存储在内存中，因此 **索引往往是存储在磁盘上的文件中的**（可能存储在单独的索引文件中，也可能和数据一起存储在数据文件中）。

**我们通常所说的索引，包括聚集索引、覆盖索引、组合索引、前缀索引、唯一索引等，没有特别说明，默认都是使用 B+ 树结构组织（多路搜索树，并不一定是二叉的）的索引。**



### 6、SQL 优化手段有哪些

1、查询语句中不要使用 select *

2、尽量减少子查询，使用关联查询（left join，right join，inner join）替代

3、减少使用 IN 或者 NOT IN，使用 exists，not exists 或者关联查询语句替代

4、or 的查询尽量用 union 或者 union all 代替（在确认没有重复数据或者不用剔除重复数据时，union all 会更好）

5、应尽量避免在 where 子句中使用 != 或 <> 操作符，否则将引擎放弃使用索引而进行全表扫描。

6、应尽量避免在 where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用索引而进行全表扫描，如： select id from t where num is null 可以在 num 上设置默认值 0，确保表中 num 列没有 null 值，然后这样查询： select id from t where num = 0



### 7、简单说一说 drop、delete 与 truncate 的区别

SQL 中的 drop、delete、truncate 都表示删除，但是三者有一些差别 

delete 和 truncate 只删除表的数据不删除表的结构 速度，一般来说：drop > truncate > delete delete 语句是 dml，这个操作会放到rollback segement 中,事务提交之后才生效；如果有相应的 trigger，执行的时候将被触发。 truncate，drop 是 ddl， 操作立即生效，原数据不放到 rollback segment中，不能回滚操作不触发 trigger。



### 8、什么是视图

视图是一种虚拟的表，具有和物理表相同的功能。可以对视图进行增，改，查，操作，试图通常是有一个表或者多个表的行或列的子集。对视图的修改不影响基本表。它使得我们获取数据更容易，相比多表查询。



### 9、 什么是内联接、左外联接、右外联接？

- 内联接（Inner Join）：匹配2张表中相关联的记录。

- 左外联接（Left Outer Join）：除了匹配2张表中相关联的记录外，还会匹配左表中剩余的记录，右表中未匹配到的字段用 NULL 表示。

- 右外联接（Right Outer Join）：除了匹配 2 张表中相关联的记录外，还会匹配右表中剩余的记录，左表中未匹配到的字段用 NULL 表示。在判定左表和右表时，要根据表名出现在 Outer Join 的左右位置关系。



### 10、并发事务带来哪些问题？

在典型的应用程序中，多个事务并发运行，经常会操作相同的数据来完成各自的任务（多个用户对同一数据进行操作）。并发虽然是必须的，但可能会导致以下的问题。

- **脏读（Dirty read）:** 当一个事务正在访问数据并且对数据进行了修改，而这种修改还没有提交到数据库中，这时另外一个事务也访问了这个数据，然后使用了这个数据。因为这个数据是还没有提交的数据，那么另外一个事务读到的这个数据是“脏数据”，依据“脏数据”所做的操作可能是不正确的。

- **丢失修改（Lost to modify）:** 指在一个事务读取一个数据时，另外一个事务也访问了该数据，那么在第一个事务中修改了这个数据后，第二个事务也修改了这个数据。这样第一个事务内的修改结果就被丢失，因此称为丢失修改。 例如：事务 1 读取某表中的数据 A = 20，事务 2 也读取 A = 20，事务 1 修改 A = A - 1，事务 2 也修改 A = A - 1，最终结果 A = 19，事务 1 的修改被丢失。

- **不可重复读（Unrepeatableread）:** 指在一个事务内多次读同一数据。在这个事务还没有结束时，另一个事务也访问该数据。那么，在第一个事务中的两次读数据之间，由于第二个事务的修改导致第一个事务两次读取的数据可能不太一样。这就发生了在一个事务内两次读到的数据是不一样的情况，因此称为不可重复读。

- **幻读（Phantom read）:** 幻读与不可重复读类似。它发生在一个事务（T1）读取了几行数据，接着另一个并发事务（T2）插入了一些数据时。在随后的查询中，第一个事务（T1）就会发现多了一些原本不存在的记录，就好像发生了幻觉一样，所以称为幻读。

**不可重复读和幻读区别：**

不可重复读的重点是修改比如多次读取一条记录发现其中某些列的值被修改，幻读的重点在于新增或者删除比如多次读取一条记录发现记录增多或减少了。



### 11、事务隔离级别有哪些？MySQL 的默认隔离级别是？

**SQL** **标准定义了四个隔离级别：**

- **READ-UNCOMMITTED（读取未提交)：** 最低的隔离级别，允许读取尚未提交的数据变更，**可能会导致脏读、幻读或不可重复读**。

- **READ-COMMITTED（读取已提交）：** 允许读取并发事务已经提交的数据，**可以阻止脏读，但是幻读或不可重复读仍有可能发生**。

- **REPEATABLE-READ（可重复读）：** 对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，**可以阻止脏读和不可重复读，但幻读仍有可能发生**。

- **SERIALIZABLE（可串行化）：** 最高的隔离级别，完全服从 ACID 的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，**该级别可以防止脏读、不可重复读以及幻读**。

| **隔离级别**     | **脏读** | **不可重复读** | 幻**影**读 |
| ---------------- | -------- | -------------- | ---------- |
| READ-UNCOMMITTED | √        | √              | √          |
| READ-COMMITTED   | ×        | √              | √          |
| REPEATABLE-READ  | ×        | ×              | √          |
| SERIALIZABLE     | ×        | ×              | ×          |

MySQL InnoDB 存储引擎的默认支持的隔离级别是 **REPEATABLE-READ（可重读）**。我们可以通过 SELECT @@tx_isolation；命令来查看

```mysql
mysql> SELECT @@tx_isolation;
+-----------------+
| @@tx_isolation |
+-----------------+
| REPEATABLE-READ |
+-----------------+
```

这里需要注意的是：与 SQL 标准不同的地方在于 InnoDB 存储引擎在 **REPEATABLE-READ（可重读）** 事务隔离级别下使用的是 Next-Key Lock 锁算法，因此可以避免幻读的产生，这与其他数据库系统（如 SQL Server）是不同的。所以说 InnoDB 存储引擎的默认支持的隔离级别是 **REPEATABLEREAD（可重读）** 已经可以完全保证事务的隔离性要求，即达到了  SQL 标准的 **SERIALIZABLE（可串行化）隔离级别。因为隔离级别越低，事务请求的锁越少，所以大部分数据库系统的隔离级别都是 READ-COMMITTED（读取提交内容）** ，但是你要知道的是 InnoDB 存储引擎默认使用(**REPEATABLE-READ（可重读）** 并不会有任何性能损失。

InnoDB 存储引擎在 **分布式事务** 的情况下一般会用到 **SERIALIZABLE（可串行化）** 隔离级别。



### 12、大表如何优化？

当 MySQL 单表记录数过大时，数据库的 CRUD 性能会明显下降，一些常见的优化措施如下：

**1.** **限定数据的范围**

务必禁止不带任何限制数据范围条件的查询语句。比如：我们当用户在查询订单历史的时候，我们可以控制在一个月的范围内；

**2.** **读 / 写分离**

经典的数据库拆分方案，主库负责写，从库负责读；

**3.** **垂直分区**

**根据数据库里面数据表的相关性进行拆分。** 例如，用户表中既有用户的登录信息又有用户的基本信息，可以将用户表拆分成两个单独的表，甚至放到单独的库做分库。

**简单来说垂直拆分是指数据表列的拆分，把一张列比较多的表拆分为多张表。** 如下所示，这样来说大家应该就更容易理解了。

参考链接：https://github.com/gsjqwyl/JavaInterview

- **垂直拆分的优点：** 可以使得列数据变小，在查询时减少读取的 Block 数，减少 I / O 次数。此外，垂直分区可以简化表的结构，易于维护。

- **垂直拆分的缺点：** 主键会出现冗余，需要管理冗余列，并会引起 Join 操作，可以通过在应用层进行 Join 来解决。此外，垂直分区会让事务变得更加复杂；

**4.** **水平分区**

**保持数据表结构不变，通过某种策略存储数据分片。这样每一片数据分散到不同的表或者库中，达到了分布式的目的。 水平拆分可以支撑非常大的数据量。**

水平拆分是指数据表行的拆分，表的行数超过 200 万行时，就会变慢，这时可以把一张的表的数据拆成多张表来存放。举个例子：我们可以将用户信息表拆分成多个用户信息表，这样就可以避免单一表数据量过大对性能造成影响。

水平拆分可以支持非常大的数据量。需要注意的一点是：分表仅仅是解决了单一表数据过大的问题，但由于表的数据还是在同一台机器上，其实对于提升 MySQL 并发能力没有什么意义，所以 **水平拆分最好分库** 。

水平拆分能够 **支持非常大的数据量存储，应用端改造也少**，但 **分片事务难以解决** ，跨节点 Join 性能较差，逻辑复杂。《Java工程师修炼之道》的作者推荐 **尽量不要对数据进行分片，因为拆分会带来逻辑、部署、运维的各种复杂度** ，一般的数据表在优化得当的情况下支撑千万以下的数据量是没有太大问题的。如果实在要分片，尽量选择客户端分片架构，这样可以减少一次和中间件的网络 I/O。

**下面补充一下数据库分片的两种常见方案：**

- **客户端代理： 分片逻辑在应用端，封装在 jar 包中，通过修改或者封装 JDBC 层来实现。** 当当网的 **Sharding-JDBC** 、阿里的 TDD L是两种比较常用的实现。

- **中间件代理： 在应用和数据中间加了一个代理层。分片逻辑统一维护在中间件服务中。** 我们现在谈的 **Mycat** 、360 的 Atlas、网易的DDB 等等都是这种架构的实现。

详细内容可以参考： MySQL 大表优化方案: https://segmentfault.com/a/1190000006158186 【值得细看】





### 13、分库分表之后， id 主键如何处理？

因为要是分成多个表之后，每个表都是从 1 开始累加，这样是不对的，我们需要一个全局唯一的 id来支持。

生成全局 id 有下面这几种方式：

- **UUID**：不适合作为主键，因为太长了，并且无序不可读，查询效率低。比较适合用于生成唯一的名字的标示比如文件的名字。

- **数据库自增** **id** : 两台数据库分别设置不同步长，生成不重复ID的策略来实现高可用。这种方式生成的 id 有序，但是需要独立部署数据库实例，成本高，还会有性能瓶颈。

- **利用** **redis** **生成** **id :** 性能比较好，灵活方便，不依赖于数据库。但是，引入了新的组件造成系统更加复杂，可用性降低，编码更加复杂，增加了系统成本。

- **Twitter 的 snowflake 算法** ：Github 地址：https://github.com/twitter-archive/snowflake。

- **美团的 Leaf 分布式 ID 生成系统** ：Leaf 是美团开源的分布式 ID 生成器，能保证全局唯一性、趋势递增、单调递增、信息安全，里面也提到了几种分布式方案的对比，但也需要依赖关系数据库、Zookeeper 等中间件。感觉还不错。美团技术团队的一篇文章：https://tech.meituan.com/2017/04/21/mt-leaf.html 。



### 14、 说说在 MySQL 中一条查询 SQL 是如何执行的？

比如下面这条 SQL 语句：

```mysql
select name from t_user where id=1
```

1. **取得链接**，使用到 MySQL 中的连接器。

2. **查询缓存**，key 为 SQL 语句，value 为查询结果，如果查到就直接返回。不建议使用次缓存，在 MySQL 8.0 版本已经将查询缓存删除，也就是说 MySQL 8.0 版本后不存在此功能。

3. **分析器**，分为词法分析和语法分析。此阶段只是做一些 SQL 解析，语法校验。所以一般语法错误在此阶段。

4. **优化器**，是在表里有多个索引的时候，决定使用哪个索引；或者一个语句中存在多表关联的时候（join），决定各个表的连接顺序。

5. **执行器**，通过分析器让 SQL 知道你要干啥，通过优化器知道该怎么做，于是开始执行语句。执行语句的时候还要判断是否具备此权限，没有权限就直接返回提示没有权限的错误；有权限则打开表，根据表的引擎定义，去使用这个引擎提供的接口，获取这个表的第一行，判断 id 是都等于 1。如果是，直接返回；如果不是继续调用引擎接口去下一行，重复相同的判断，直到取到这个表的最后一行，最后返回。



### 15、索引有什么优缺点？

![](https://dream-syz.github.io/6-3.png)

### 16、MySQL 中 varchar 与 char 的区别？ varchar(30)  中的 30 代表的涵义？

- varchar 与 char 的区别，char 是一种固定长度的类型，varchar 则是一种可变长度的类型。

- CHAR 和 VARCHAR 类型在存储和检索方面有所不同

  CHAR 列长度固定为创建表时声明的长度，长度值范围是 1 到 255；当 CHAR 值被存储时，它们被用空格填充到特定长度，检索 CHAR 值时需删除尾随空格。

- varchar(30) 中 30 的涵义最多存放 30 个字符。varchar(30) 和 (130) 存储 hello 所占空间一样，但后者在排序时会消耗更多内存，因为 ORDER BY col 采用 fixed_length 计算 col 长度（memory 引擎也一样）。

- 对效率要求高用 char，对空间使用要求高用 varchar。



### 17、int(11) 中的 11 代表什么涵义？

int(11) 中的 11，不影响字段存储的范围，只影响展示效果。



### 18、 为什么 SELECT COUNT(\*) FROM table 在 InnoDB 比 MyISAM 慢？

*对于 SELECT COUNT(*) FROM table 语句，在没有 WHERE 条件的情况下，InnoDB 比 MyISAM 可能会慢很多，尤其在大表的情况下。因为，InnoDB 是去实时统计结果，会 **全表扫描**；而 MyISAM 内部维持了一个计数器，预存了结果，所以直接返回即可。



### 19、MySQL索引类型有哪些？

**主键索引**

索引列中的值必须是唯一的，不允许有空值。

**普通索引**

MySQL 中基本索引类型，没有什么限制，允许在定义索引的列中插入重复值和空值。

**唯一索引**

索引列中的值必须是唯一的，但是允许为空值。

**全文索引**

只能在文本类型 CHAR，VARCHAR，TEXT 类型字段上创建全文索引。字段长度比较大时，如果创建普通索引，在进行 like 模糊查询时效率比较低，这时可以创建全文索引。MyISAM 和 InnoDB 中都可以使用全文索引。

**空间索引**

MySQL 在 5.7 之后的版本支持了空间索引，而且支持 OpenGIS 几何数据模型。MySQL 在空间索引这方面遵循 OpenGIS 几何数据模型规则。

**前缀索引**

在文本类型如 CHAR，VARCHAR，TEXT 类列上创建索引时，可以指定索引列的长度，但是数值类型不能指定。其他（按照索引列数量分类）

1. 单列索引
2. 组合索引

组合索引的使用，需要遵循 **最左前缀匹配原则（最左匹配原则）**。一般情况下在条件允许的情况下使用组合索引替代多个单列索引使用。



### 20、什么时候不要使用索引？

1. 经常增删改的列不要建立索引；
2. 有大量重复的列不建立索引；
3. 表记录太少不要建立索引。



### 21、说说什么是 MVCC？

多版本并发控制（MVCC=Multi-Version Concurrency Control），是一种用来解决读 - 写冲突的无锁并发控制。也就是为事务分配单向增长的时间戳，为每个修改保存一个版本。版本与事务时间戳关联，读操作只读该事务开始前的数据库的快照（复制了一份数据）。这样在读操作不用阻塞写操作，写操作不用阻塞读操作的同时，避免了脏读和不可重复读。

#### MVCC 可以为数据库解决什么问题？

在并发读写数据库时，可以做到在读操作时不用阻塞写操作，写操作也不用阻塞读操作，提高了数据库并发读写的性能。同时还可以解决脏读、幻读、不可重复读等事务隔离问题，但不能解决更新丢失问题。

#### 说说 MVCC 的实现原理

MVCC 的目的就是多版本并发控制，在数据库中的实现，就是为了解决读写冲突，它的实现原理主要是依赖记录中的 3 个隐式字段、undo 日志、Read View 来实现的。

##### **三个隐式字段：**

**DB_ROW_ID**：隐含的自增 ID（隐藏主键）

**DB_TRX_ID**：记录最近修改这条记录的事务 ID

**DB_ROLL_PTR**：回滚指针，指向这条记录的上一个版本



### 22、 请说说 MySQL 数据库的锁？

关于 MySQL 的锁机制，可能会问很多问题，不过这也得看面试官在这方面的知识储备。

MySQL 中有共享锁和排它锁，也就是读锁和写锁。

1. 共享锁：不堵塞，多个用户可以同一时刻读取同一个资源，相互之间没有影响。
2. 排它锁：一个写操作阻塞其他的读锁和写锁，这样可以只允许一个用户进行写入，防止其他用户读取正在写入的资源。

3. 表锁：系统开销最小，会锁定整张表，MyISAM 使用表锁。
4. 行锁：容易出现死锁，发生冲突概率低，并发高，InnoDB 支持行锁（必须有索引才能实现，否则会自动锁全表，那么就不是行锁了）。



### 23、说说什么是锁升级？

MySQL 行锁只能加在索引上，如果操作不走索引，就会升级为表锁。因为 InnoDB 的行锁是加在索引上的，如果不走索引，自然就没法使用行锁了，原因是 InnoDB 是将 primary key index 和相关的行数据共同放在 B+ 树的叶节点。InnoDB 一定会有一个 primary key，secondary index 查找的时候，也是通过找到对应的 primary，再找对应的数据行。

当非唯一索引上记录数超过一定数量时，行锁也会升级为表锁。测试发现 **当非唯一索引相同的内容不少于整个表记录的二分之一时会升级为表锁**。因为当非唯一索引相同的内容达到整个记录的二分之一时，索引需要的性能比全文检索还要大，查询语句优化时会选择不走索引，造成索引失效，行锁自然就会升级为表锁。



### 24、说说悲观锁和乐观锁

**悲观锁**

说的是数据库被外界（包括本系统当前的其他事物以及来自外部系统的事务处理）修改保持着保守态度，因此在整个数据修改过程中，将数据处于锁状态。悲观的实现往往是依靠数据库提供的锁机制，也只有数据库层面提供的锁机制才能真正保证数据访问的排他性，否则，即使在本系统汇总实现了加锁机制，也是没有办法保证系统不会修改数据。

在悲观锁的情况下，为了保证事务的隔离性，就需要一致性锁定读。读取数据时给加锁，其它事务无法修改这些数据。修改删除数据时也要加锁，其它事务无法读取这些数据。

**乐观锁**

相对悲观锁而言，乐观锁机制采取了更加宽松的加锁机制。悲观锁大多数情况下依靠数据库的锁机制实现，以保证操作最大程度的独占性。但随之而来的就是数据库性能的大量开销，特别是对长事务而言，这样的开销往往无法承受。

而乐观锁机制在一定程度上解决了这个问题。乐观锁，大多是基于数据版本（Version）记录机制实现。何谓数据版本？即为数据增加一个版本标识，在基于数据库表的版本解决方案中，一般是通过为数据库表增加一个“version”字段来实现。读取出数据时，将此版本号一同读出，之后更新时，对此版本号加一。此时，将提交数据的版本数据与数据库表对应记录的当前版本信息进行比对，如果提交的数据版本号大于数据库表当前版本号，则予以更新，否则认为是过期数据。



### 25、怎样尽量避免死锁的出现？

1. 设置获取锁的超时时间，至少能保证最差情况下，可以退出程序，不至于一直等待导致死锁；
2. 设置按照同一顺序访问资源，类似于串行执行；
3. 避免事务中的用户交叉；
4. 保持事务简短并在一个批处理中；
5. 使用低隔离级别；
6. 使用绑定链接。



### 26、使用 MySQL 的索引应该注意些什么？

![](https://dream-syz.github.io/6-4.png)



### 27、主键和候选键有什么区别？

表格的每一行都由主键唯一标识，一个表只有一个主键。主键也是候选键。按照惯例，候选键可以被指定为主键，并且可以用于任何外 键引用。



### 28、主键与索引有什么区别？

主键一 **定会创建一个唯一索引，但是有唯一索引的列不一定是主键；**

主键不允许为空值，唯一索引列允许空值；

一个表只能有一个主键，但是可以有多个唯一索引；

主键可以被 **其他表引用为外键，唯一索引列不可以；**

主键是一种约束，而唯一索引是一种索引，是表的冗余数据结构



### 29、MySQL 如何做到高可用方案？

MySQL 高可用，意味着不能一台 MySQL 出了问题，就不能访问了。

1. MySQL 高可用：分库分表，通过 MyCat 连接多个 MySQL
2. MyCat 也得高可用：Haproxy，连接多个 MyCat
3. Haproxy 也得高可用：通过 keepalived 辅助 Haproxy









## SpringCloud 篇

### 1、什么是 SpringCloud

Spring cloud 流应用程序启动器是基于 Spring Boot 的 Spring 集成应用程序，提供与外部系统的集成。Spring cloud Task，一个生命周期短暂的微服务框架，用于快速构建执行有限数据处理的应用程序。



### 2、什么是微服务

微服务架构是一种架构模式或者说是一种架构风格，它提倡将单一应用程序划分为一组小的服务，每个服务运行在其独立的自己的进程中，服务之间相互协调、互相配合，为用户提供最终价值。服务之间采用轻量级的通信机制互相沟通（通常是基于 HTTP 的 RESTful API）,每个服务都围绕着具体的业务进行构建，并且能够被独立的构建在生产环境、类生产环境等。另外，应避免统一的、集中式的服务管理机制，对具体的一个服务而言，应根据业务上下文，选择合适的语言、工具对其进行构建，可以有一个非常轻量级的集中式管理来协调这些服务，可以使用不同的语言来编写服务，也可以使用不同的数据存储。



### 3、SpringCloud 有什么优势

使用 Spring Boot 开发分布式微服务时，我们面临以下问题

（1）与分布式系统相关的复杂性-这种开销包括网络问题，延迟开销，带宽问题，安全问题。

（2）服务发现-服务发现工具管理群集中的流程和服务如何查找和互相交谈。它涉及一个服务目录，在该目录中注册服务，然后能够查找并连接到该目录中的服务。

（3）冗余-分布式系统中的冗余问题。

（4）负载平衡 --负载平衡改善跨多个计算资源的工作负荷，诸如计算机，计算机集群，网络链路，中央处理单元，或磁盘驱动器的分布。

（5）性能-问题 由于各种运营开销导致的性能问题。

（6）部署复杂性-Devops 技能的要求。



### 4、 什么是服务熔断？什么是服务降级？

**熔断机制是应对雪崩效应的一种微服务链路保护机制**。当某个微服务不可用或者响应时间太长时，会进行服务降级，进而熔断该节点微服务的调用，快速返回“错误”的响应信息。当检测到该节点微服务调用响应正常后恢复调用链路。在 SpringCloud 框架里熔断机制通过Hystrix 实现，Hystrix 会监控微服务间调用的状况，当失败的调用到一定阈值，缺省是 5 秒内调用 20 次，如果失败，就会启动熔断机制。服务降级，一般是从整体负荷考虑。就是当某个服务熔断之后，服务器将不再被调用，此时客户端可以自己准备一个本地的 fallback回调，返回一个缺省值。这样做，虽然水平下降，但好歹可用，比直接挂掉强。

**Hystrix 相关注解** @EnableHystrix：开启熔断 @HystrixCommand(fallbackMethod=”XXX”)：声明一个失败回滚处理函数 XXX，当被注解的方法执行超时（默认是 1000 毫秒），就会执行 fallback 函数，返回错误提示。



### 5、Eureka 和 zookeeper 都可以提供服务注册与发现的功能，请说说两个的区别？

Zookeeper 保证了CP（C：一致性，P：分区容错性），Eureka 保证了AP（A：高可用，P：分区容错性） 

1.当向注册中心查询服务列表时，我们可以容忍注册中心返回的是几分钟以前的信息，但不能容忍直接 down 掉不可用。也就是说，服务注册功能对高可用性要求比较高，但 zk 会出现这样一种情况，当 master 节点因为网络故障与其他节点失去联系时，剩余节点会重新选 leader。问题在于，选取 leader 时间过长，30 ~ 120 s，且选取期间 zk 集群都不可用，这样就会导致选取期间注册服务瘫痪。在云部署的环境下，因网络问题使得zk集群失去master节点是较大概率会发生的事，虽然服务能够恢复，但是漫长的选取时间导致的注册长期不可用是不能容忍的。

2.Eureka 保证了可用性，Eureka 各个节点是平等的，几个节点挂掉不会影响正常节点的工作，剩余的节点仍然可以提供注册和查询服务。而 Eureka 的客户端向某个 Eureka 注册或发现时发生连接失败，则会自动切换到其他节点，只要有一台 Eureka 还在，就能保证注册服务可用，只是查到的信息可能不是最新的。除此之外，Eureka 还有自我保护机制，如果在 15 分钟内超过 85% 的节点没有正常的心跳，那么 Eureka 就认为客户端与注册中心发生了网络故障，此时会出现以下几种情况： 

①、Eureka 不在从注册列表中移除因为长时间没有收到心跳而应该过期的服务。 

②、Eureka 仍然能够接受新服务的注册和查询请求，但是不会被同步到其他节点上（即保证当前节点仍然可用）

③、当网络稳定时，当前实例新的注册信息会被同步到其他节点。

因此，Eureka 可以很好的应对因网络故障导致部分节点失去联系的情况，而不会像 Zookeeper 那样使整个微服务瘫痪



### 6、SpringBoot 和 SpringCloud 的区别？

SpringBoot 专注于快速方便的开发单个个体微服务。

SpringCloud 是关注全局的微服务协调整理治理框架，它将 SpringBoot 开发的一个个单体微服务整合并管理起来，为各个微服务之间提供，配置管理、服务发现、断路器、路由、微代理、事件总线、全局锁、决策竞选、分布式会话等等集成服务

SpringBoot 可以离开 SpringCloud 独立使用开发项目， 但是 SpringCloud 离不开 SpringBoot ，属于依赖的关系。SpringBoot 专注于快速、方便的开发单个微服务个体，SpringCloud 关注全局的服务治理框架。



### 7、负载平衡的意义什么？

在计算中，负载平衡可以改善跨计算机，计算机集群，网络链接，中央处理单元或磁盘驱动器等多种计算资源的工作负载分布。负载平衡旨在优化资源使用，最大化吞吐量，最小化响应时间并避免任何单一资源的过载。使用多个组件进行负载平衡而不是单个组件可能会通过冗余来提高可靠性和可用性。负载平衡通常涉及专用软件或硬件，例如多层交换机或域名系统服务器进程。



### 8、什么是 Hystrix ？它如何实现容错？

Hystrix 是一个延迟和容错库，旨在隔离远程系统，服务和第三方库的访问点，当出现不可避免的故障时，停止级联故障并在复杂的分布式系统中实现弹性。

通常对于使用微服务架构开发的系统，涉及到许多微服务。这些微服务彼此协作。思考以下微服务

![](https://dream-syz.github.io/7-1.png)

假设如果上图中的微服务 9 失败了，那么使用传统方法我们将传播一个异常。但这仍然会导致整个系统崩溃。

随着微服务数量的增加，这个问题变得更加复杂。微服务的数量可以高达 1000。这是 hystrix 出现的地方，我们将使用 Hystrix 在这种情况下的 Fallback 方法功能。我们有两个服务 employee-consumer 使用由 employee-consumer 公开的服务。

简化图如下所示

![](https://dream-syz.github.io/7-2.png)

现在假设由于某种原因，employee-producer 公开的服务会抛出异常。我们在这种情况下使用 Hystrix 定义了一个回退方法。这种后备方法应该具有与公开服务相同的返回类型。如果暴露服务中出现异常，则回退方法将返回一些值。



### 9、什么是 Hystrix 断路器？我们需要它吗？

由于某些原因，employee-consumer 公开服务会引发异常。在这种情况下使用 Hystrix 我们定义了一个回退方法。如果在公开服务中发生异常，则回退方法返回一些默认值

![](https://dream-syz.github.io/7-3.png)

如果 firstPage method() 中的异常继续发生，则 Hystrix 电路将中断，并且员工使用者将一起跳过 firtsPage 方法，并直接调用回退方法。 断路器的目的是给第一页方法或第一页方法可能调用的其他方法留出时间，并导致异常恢复。可能发生的情况是，在负载较小的情况下，导致异常的问题有更好的恢复机会 。

![](https://dream-syz.github.io/7-4.png)



### 10、说说 RPC 的实现原理

首先需要有处理网络连接通讯的模块，负责连接建立、管理和消息的传输。其次需要有编解码的模块，因为网络通讯都是传输的字节码，需要将我们使用的对象序列化和反序列化。剩下的就是客户端和服务器端的部分，服务器端暴露要开放的服务接口，客户调用服务接口的一个代理实现，这个代理实现负责收集数据、编码并传输给服务器然后等待结果返回。



### 11、Eureka 自我保护机制是什么?

当 Eureka Server 节点在短时间内丢失了过多实例的连接时（比如网络故障或频繁启动关闭客户端）节点会进入自我保护模式，保护注册信息，不再删除注册数据，故障恢复时，自动退出自我保护模式。



### 12、什么是 Ribbon ？

ribbon 是一个负载均衡客户端，可以很好的控制 http 和 tcp 的一些行为。feign 默认集成了 ribbon。



### 13、什么是 feigin ？它的优点是什么？

1.feign 采用的是基于接口的注解 

2.feign 整合了 ribbon，具有负载均衡的能力 

3.整合了 Hystrix，具有熔断的能力

使用: 1.添加 pom 依赖。 2.启动类添加 @EnableFeignClients 3.定义一个接口 @FeignClient(name=“xxx”) 指定调用哪个服务



### 14、Ribbon 和 Feign 的区别？

1.Ribbon 都是调用其他服务的，但方式不同。

2.启动类注解不同，Ribbon 是 @RibbonClient；feign 的是 @EnableFeignClients 

3.服务指定的位置不同，Ribbon 是在 @RibbonClient 注解上声明，Feign则是在定义抽象方法的接口中使用 @FeignClient 声明。 

4.调用方式不同，Ribbon 需要自己构建 http 请求，模拟 http 请求



### 15、如何设计一个注册中心？

![](https://dream-syz.github.io/7-5.png)

- 服务注册

- 注册表结构设计服务发现

- 服务订阅

- 服务推送

- 健康检查

- 集群同步：设计到数据同步，数据同步我们有哪些协议 raft 、distro、ZAB



### 16、Nacos1.x 作为注册中心的原理？

1、使用 HTTP 发送注册

2、查询服务提供方列表

3、定时拉取（每 10 秒）

4、检测到服务提供者异常，基于 UDP 协议推送更新

5、定时心跳（5 秒），检测服务状态

6、定时心跳任务检查

7、集群数据同步任务使用 Distro

![](https://dream-syz.github.io/7-6.png)



### 17、Nacos 服务领域模型有哪些？

| **模型名称** | **解释**                                                |
| ------------ | ------------------------------------------------------- |
| NameSpace    | 实现环境隔离，默认值 public                             |
| Group        | 不同的 service 可以组成一个 Group，默认值 Default-Group |
| Service      | 服务名称                                                |
| Cluster      | 对指定的微服务虚拟划分，默认值Default                   |
| Instance     | 某个服务的具体实例                                      |

Nacos 服务注册中心于发现的领域模型的最佳实践。

![](https://dream-syz.github.io/7-7.png)



### 18、Nacos中的Distro协议

- Nacos 每个节点自己负责部分的写请求。

- 每个节点会把自己负责的新增数据同步给其他节点。

- 每个节点定时发送自己负责数据的校验值到其他节点来保持数据一致性。

- 每个节点独立处理读请求，及时从本地发出响应。

- 新加入的 Distro 节点会进行全量数据拉取。（具体操作是轮询所有的 Distro节点，通过向其他的机器发送请求拉取全量数据。）

- ![](https://dream-syz.github.io/7-8.png)



### 19、配置中心的技术选型

| 功能点              | **Spring Cloud Config**  | **Apollo**                 | **Nacos**                            |
| ------------------- | ------------------------ | -------------------------- | ------------------------------------ |
| **版本管理**        | 支持（Git）              | 支持                       | 支持                                 |
| **配置实时推送**    | 支持（Spring Cloud Bus） | 支持（HTTP 长轮询 1 s 内） | 支持（HTTP 长轮询 1 s 内 或者 grpc） |
| **配置回滚**        | 支持（Git）              | 支持                       | 支持                                 |
| **灰度发布**        | 支持（调用机器接口）     | 支持                       | **不支持**                           |
| **权限管理**        | 支持（依赖 Git）         | 支持                       | 支持                                 |
| **配置生效时间**    | **重启**                 | 实时                       | 实时                                 |
| 审计                | 支持（依赖 Git）         | 支持                       | 不支持                               |
| **多集群**          | 支持                     | 支持                       | 支持                                 |
| 多环境              | 支持                     | 支持                       | 支持                                 |
| **client 本地缓存** | **不支持**               | 支持                       | 支持                                 |
| 监听查询            | 支持                     | 支持                       | 支持                                 |
| 运维成本            | 高                       | 中等                       | 较低                                 |
| **多语言**          | 仅 Java                  | 主流语言，提供了 Open API  | 主流语言，提供了 Open API            |
| **配置项维护**      | 基于 Git                 | 统一界面                   | 统一界面                             |



### 20、Nacos1.x 配置中心长轮询机制？

客户端会轮询向服务端发出一个长连接请求，这个长连接最多 30 s 就会超时，服务端收到客户端的请求会先判断当前是否有配置更新，有则立即返回，如果没有服务端会将这个请求拿住“hold” 29.5 s 加入队列，最后 0.5 s 再检测配置文件无论有没有更新都进行正常返回，但等待的 29.5 s 期间有配置更新可以提前结束并返回。



### 21、Nacos 配置中心配置优先级？

```
#作用：顺序
#${application.name}-${profile}.${file- extension} nacos-config-prod.yaml
#${application.name}.${file-extension} nacos-config.yaml
#${application.name} nacos-config
#extensionConfigs 扩展配置文件
#sharedConfigs 多个微服务公共配置 redis
```

```
spring.application.name=nacos-config
server.port=8081
#nacos地址
spring.cloud.nacos.config.server-addr=localhost:8848
#1、只有上面的配置的时候他默认加载文件为：${application.name} nacos-config
#2、指定文件后缀名称
#加载文件为：${application.name}.${file-extension}
#nacos-config.yaml
#spring.cloud.nacos.config.file-extension=yaml
##3、profile： 指定环境 文件名：${application.name}-${profile}.${file-extension}
##nacos-config-prod.yaml
#spring.profiles.active=prod
#4、nacos自己提供的环境隔离 ，这里是开发环境下的
#spring.cloud.nacos.config.namespace=ff02931a-6fdb-4681-ac37-2f6d9a0596f8
#5、 自定义 group 配置，这里也可以设置为数据库配置组，中间件配置组，但是一般不用，
# 配置中心淡化了组的概念，使用默认值DEFAULT_GROUP
#spring.cloud.nacos.config.group=DEFAULT_GROUP
#
#6、自定义Data Id的配置 共享配置（sharedConfigs）
#spring.cloud.nacos.config.shared-configs[0].data-id= common.yaml
##可以不配置，使用默认
#spring.cloud.nacos.config.shared-configs[0].group=DEFAULT_GROUP
## 这里需要设置为true，动态可以刷新，默认为false
#spring.cloud.nacos.config.shared-configs[0].refresh=true
# 7、扩展配置(extensionConfigs)
# 支持一个应用有多个DataId配置，mybatis.yaml datasource.yaml
#spring.cloud.nacos.config.extension-configs[0].data-id=datasource.yaml
#spring.cloud.nacos.config.extension-configs[0].group=DEFAULT_GROUP
#spring.cloud.nacos.config.extension-configs[0].refresh=true

#作用：顺序
#${application.name}-${profile}.${file- extension} msb-edu-prod.yaml
#${application.name}.${file-extension} nacos-config.yaml
#${application.name} nacos-config
#extensionConfigs 扩展配置文件
#sharedConfigs 多个微服务公共配置 redis
```



### 22、Nacos 2.x 客户端探活机制？

Nacos 2.x 引入了客户端探活机制，该机制主要用于检测服务实例的健康状态。具体来说，Nacos 2.x 客户端探活机制包括以下几个步骤：

1. 注册服务实例：服务提供者在启动时将自己的实例信息注册到 Nacos Server 中，包括 IP 地址、端口号和健康状态等。

2. 心跳检测：Nacos Server 会周期性地向服务实例发送心跳请求，以检测实例是否存活。默认情况下，心跳检测间隔为 5 秒。

3. 健康检测：服务实例在接收到 Nacos Server 的心跳请求后，会返回一个健康状态给 Nacos Server。如果服务实例长时间未返回健康状态或返回的状态为不健康，Nacos Server 会认为该实例不可用。

4. 健康状态更新：服务实例的健康状态可能会因为各种原因发生变化，例如网络故障或服务异常。当服务实例的健康状态发生变化时，Nacos Server 会及时更新实例的健康状态。

通过客户端探活机制，Nacos 可以动态地监测和管理服务实例的健康状态，以确保服务的可用性和稳定性。



### 23、Ribbon 底层怎样实现不同服务的不同配置

Ribbon 是一个负载均衡客户端，它的底层实现主要依赖于以下几个组件：

1. 服务发现：Ribbon 通过与服务注册中心（如 Eureka、Consul 或 Nacos）进行集成，获取可用的服务实例列表。通过服务发现，Ribbon 可以动态地获取不同服务的实例信息。

2. 负载均衡策略：Ribbon 支持多种负载均衡策略，例如轮询、随机、加权随机、加权轮询等。根据不同的负载均衡策略，Ribbon 可以在不同的服务实例之间进行请求的分发。

3. 配置管理：Ribbon 可以通过配置文件或配置中心（如 Spring Cloud Config 或 Nacos）获取服务的相关配置信息。这些配置信息可以包括连接超时时间、重试次数、连接池大小等。通过配置管理，Ribbon 可以根据不同的服务设置不同的配置。

通过以上组件的协作，Ribbon 可以根据服务的不同配置来进行请求的负载均衡。例如，可以针对不同的服务设置不同的负载均衡策略或超时时间，以满足不同服务的需求。



### 24、为什么 Feign 第一次调用耗时很长？

在使用 Feign 进行第一次调用时，可能会出现较长的耗时。这是由于以下原因：

1. 服务发现和负载均衡：Feign 在第一次调用时需要进行服务发现和负载均衡操作，以确定要调用的服务实例。这涉及到与服务注册中心通信、获取可用实例列表并选择一个实例等步骤，这些操作可能会花费一些时间。

2. 远程调用初始化：在进行第一次远程调用时，Feign 需要初始化相关的 HTTP 连接和连接池等资源。这包括建立 TCP 连接、进行 TLS 握手（如果使用 HTTPS）以及创建 HTTP 请求等。这些初始化过程可能会导致一定的延迟。

3. 服务实例的冷启动：如果服务实例处于空闲状态，可能会因为冷启动而导致第一次调用的耗时较长。冷启动时，服务实例需要进行初始化和加载相关资源，这可能会花费更多的时间。

**为了减少 Feign 第一次调用的耗时，可以采取以下措施：**

1. 启用连接池：可以通过配置 Feign 使用连接池来复用 TCP 连接，减少连接的建立和释放开销，提高性能。

2. 预热服务实例：可以通过定时任务或定时健康检查等方式，定期发送心跳请求或调用服务实例，使其保持活跃状态，避免冷启动带来的延迟。

3. 调整超时时间：根据实际情况，可以适当调整 Feign 的超时时间设置，以确保在网络状况较差或服务响应较慢时，不会因超时导致请求失败。

需要注意的是，Feign 的第一次调用耗时问题可能是正常现象，因为在微服务架构中，服务之间的通信和调用是不可避免的，而这些初始化和准备过程都需要一定的时间。



### 25、Ribbon 的属性配置和类配置优先级

在 Ribbon 中，属性配置和类配置有不同的优先级。具体来说，优先级从高到低如下：

1. 属性配置优先级高于类配置：如果在属性配置和类配置中同时设置了相同的属性，属性配置会覆盖类配置。属性配置通常是通过配置文件（如 `application.properties` 或 `application.yml`）中的属性来进行设置。

2. 类配置：类配置是通过编程方式进行设置，通常是通过创建 `RibbonClientConfiguration` 的子类并覆盖其中的方法来进行自定义配置。类配置中可以设置一些默认的属性值，这些属性值将被应用于所有的 Ribbon 客户端。

3. 全局默认配置：Ribbon 还提供了一些全局默认配置，对所有的 Ribbon 客户端都生效。可以通过在配置文件中设置以"ribbon."开头的属性来进行全局默认配置。

总的来说，属性配置具有最高的优先级，会覆盖类配置和全局默认配置。类配置的优先级次之，而全局默认配置的优先级最低。通过这种优先级的设置，可以在不同的层级上对 Ribbon 进行灵活的配置和定制。



### 26、Feign 的性能优化？

要优化 Feign 的性能，可以考虑以下几个方面：

1. 启用连接池：通过配置 Feign 使用连接池来复用 TCP 连接，减少连接的建立和释放开销，提高性能。可以通过配置 `httpclient`的相关属性，如 `maxTotalConnections` 和 `maxConnectionsPerRoute` 来控制连接池的大小。

2. 调整超时时间：根据实际情况，可以适当调整Feign的超时时间设置，以确保在网络状况较差或服务响应较慢时，不会因超时导致请求失败。可以通过配置 `connectTimeout` 和 `readTimeout` 来设置连接超时和读取超时时间。

3. 批量请求：如果有多个独立的请求需要发送给同一个服务，可以考虑使用 Feign 的批量请求功能，将多个请求合并为一个批量请求发送，减少网络开销和连接建立的次数，提高性能。

4. 启用 GZIP 压缩：可以通过配置 Feign 启用 GZIP 压缩来减少网络传输的数据量，提高性能。可以通过设置 `Content-Encoding` 为`gzip`来启用 GZIP 压缩。

5. 启用 HTTP / 2：如果服务端支持 HTTP / 2 协议，可以通过配置 Feign 使用 HTTP / 2 来提高性能。可以通过配置 `httpclient` 的相关属性，如 `sslContextBuilder` 和 `alpnProtocols` 来启用 HTTP / 2。

6. 缓存：对于一些不经常变动的数据，可以考虑在客户端使用缓存，减少对服务端的请求，提高性能。可以使用 Spring Cache 等缓存框架来实现。

7. 并发请求：如果有多个独立的请求需要发送给不同的服务，可以考虑使用并发请求的方式，同时发送多个请求，减少请求的等待时间，提高性能。可以使用异步方式发送请求，或使用并发工具类如 CompletableFuture 来实现。

注意，优化 Feign 的性能需要根据具体场景和需求进行调整和测试。不同的应用场景可能需要不同的优化策略，因此需要根据实际情况选择合适的优化方法。



Feign 底层默认是 JDK 自带的 HttpURLConnection，它是单线程发送 HTTP 请求的，不能配置线程池，我们使用 OKHttp 或者 HttpClient 来发送 HTTP 请求，并且它们两个都支持线程池。

常见HTTP客户端

**HttpClient**

HttpClient 是 Apache Jakarta Common 下的子项目，用来提供高效的、最新的、功能丰富的支持 HTTP 协 议的客户端编程工具包，并且它支持 HTTP 协议最新版本和建议。HttpClient 相比传统  JDK 自带的 URLConnection，提升了易用性和灵活性，使客户端发送 HTTP 请求变得容易，提高了开发的效率。

**Okhttp**

一个处理网络请求的开源项目，是安卓端最火的轻量级框架，由 Square 公司贡献，用于替代 HttpUrlConnection 和 Apache HttpClient。OKHttp 拥有简洁的 API、高效的性能，并支持多种协议 （HTTP/2 和 SPDY）。

**HttpURLConnection**

HttpURLConnection 是 Java 的标准类，它继承自 URLConnection，可用于向指定网站发送  GET 请求、 POST 请求。HttpURLConnection 使用比较复杂，不像 HttpClient 那样容易使用。

**RestTemplate**

RestTemplate 是 Spring 提供的用于访问 Rest 服务的客户端，RestTemplate 提供了多种便捷访问远程 HTTP 服务的方法，能够大大提高客户端的编写效率。



### 27、Feign 怎样实现认证的传递？

在 Feign 中实现认证的传递，可以通过以下几种方式：

1. 使用请求头传递认证信息：可以在 Feign 的请求方法上添加 `@RequestHeader` 注解，将认证信息作为请求头的一部分进行传递。例如：

```java
@FeignClient(name = "example-service")
public interface ExampleServiceClient {
    @GetMapping("/api/example")
    String getExampleData(@RequestHeader("Authorization") String token);
}
```

在调用 `getExampleData` 方法时，可以将认证信息作为参数传入，Feign 会将其放入请求头中。

2. 使用请求参数传递认证信息：可以在 Feign 的请求方法上添加`@RequestParam`注解，将认证信息作为请求参数进行传递。例如：

```java
@FeignClient(name = "example-service")
public interface ExampleServiceClient {
    @GetMapping("/api/example")
    String getExampleData(@RequestParam("token") String token);
}
```

在调用 `getExampleData` 方法时，可以将认证信息作为参数传入，Feign 会将其作为请求参数进行传递。

3. 使用请求体传递认证信息：可以在Feign的请求方法上添加 `@RequestBody` 注解，将认证信息作为请求体的一部分进行传递。例如：

```java
@FeignClient(name = "example-service")
public interface ExampleServiceClient {
    @PostMapping("/api/example")
    String postExampleData(@RequestBody AuthRequest request);
}

public class AuthRequest {
    private String token;
    // getters and setters
}
```

在调用`postExampleData`方法时，可以创建一个包含认证信息的请求体对象，并作为参数传入。

需要注意的是，以上方法仅适用于将认证信息传递给后端服务。在后端服务中，需要进行相应的认证验证和处理。另外，认证信息的传递方式应根据具体的认证机制和安全要求进行选择和配置。



### 28、谈谈 Sentienl 中使用的限流算法

Sentinel 是一个开源的分布式系统的流量防控组件，它提供了实时的流量控制、熔断降级、系统负载保护等功能。在 Sentienl 中使用的限流算法主要有以下几种：

1. 固定窗口计数器（Fixed Window Counter）：该算法是最简单的限流算法之一，它将请求按照时间窗口进行计数，当窗口内的请求数超过阈值时进行限流。例如，可以设置每秒钟最多处理 100 个请求，超过该阈值的请求将被限流。

2. 滑动窗口计数器（Sliding Window Counter）：该算法是固定窗口计数器的改进版，它不仅考虑了窗口内的请求数，还考虑了窗口内的时间分布。例如，可以将时间窗口划分为若干个小窗口，每个小窗口内都有一个请求数的阈值，当整个时间窗口内的请求数超过阈值时进行限流。

3. 令牌桶算法（Token Bucket）：该算法是一种基于令牌的限流算法，它维护一个固定容量的令牌桶，每个令牌代表一个请求的权限。当请求到达时，需要从令牌桶中获取一个令牌，如果令牌桶为空，则表示无法获取令牌，请求将被限流。令牌桶算法可以平滑处理请求的流量。

4. 漏桶算法（Leaky Bucket）：该算法是一种基于漏桶的限流算法，它将请求看作水滴，漏桶作为一个固定容量的桶，请求以恒定的速率流入桶中，如果桶满了，则表示请求无法处理，被限流。漏桶算法可以平滑处理突发的请求流量。

以上是 Sentienl 中使用的常见限流算法，根据具体的应用场景和需求，可以选择合适的算法进行限流。



### 29、谈谈 Sentienl 服务熔断过程

Sentinel 是一个流量控制组件，除了限流功能外，还提供了熔断降级的功能。在 Sentinel 中，服务熔断是一种保护机制，用于防止故障服务对整个系统的影响。下面是 Sentinel 服务熔断的一般过程：

1. 监控服务调用：Sentinel 会监控服务的调用情况，包括请求的成功率、响应时间等指标。通过实时收集和统计这些指标，Sentinel 可以判断服务是否正常运行。

2. 熔断条件判断：当服务的某些指标超过预设的阈值时，Sentinel 就会触发熔断条件判断。例如，可以设置当服务的请求错误率超过 50% 时触发熔断。

3. 进入熔断状态：一旦触发熔断条件，Sentinel 会将服务切换到熔断状态。在熔断状态下，Sentinel 会拒绝对该服务的请求，并返回预设的错误信息或者空响应。这样可以避免故障服务对整个系统的影响。

4. 熔断恢复：在熔断状态下，Sentinel 会定时尝试恢复服务。它会允许一部分请求通过，以测试服务的可用性。如果这些请求成功率达到一定的阈值，Sentinel 就会认为服务已经恢复正常，将服务切换回正常状态。

5. 熔断保护：为了防止熔断状态频繁切换，Sentinel 还提供了熔断保护机制。当服务切换回正常状态后，Sentinel 会暂时禁止对该服务的请求，以避免服务被过多的请求压垮。

通过以上的熔断过程，Sentinel 可以有效地保护系统免受故障服务的影响，提高系统的可用性和稳定性。



### 30、在 Gateway 中怎样实现服务平滑迁移？

在 Gateway 中实现服务平滑迁移可以采用以下步骤：

1. 配置多个后端服务：在 Gateway 的配置中，同时配置新旧两个版本的后端服务。可以使用不同的路由规则或者负载均衡策略来将请求分发到不同的后端服务。
2. 分流策略设置：在 Gateway 中设置分流策略，逐渐将流量从旧版本的后端服务迁移到新版本的后端服务上。可以根据实际需求逐步提高新版本服务的流量占比，例如每天增加一定比例的流量。
3. 监控和调整：监控新版本服务的性能和稳定性，确保新版本服务能够正常运行。如果发现问题，可以及时调整配置或者回滚迁移操作。
4. 逐步迁移：在经过一段时间的监控和调整后，当新版本服务被充分验证并且稳定运行时，可以逐步将流量从旧版本服务全面切换到新版本服务上。
5. 清理旧版本：在所有流量都已经迁移到新版本服务后，可以将旧版本服务从 Gateway 的配置中移除，并进行清理操作。

需要注意的是，在服务平滑迁移过程中，要确保新版本服务和旧版本服务的接口和功能兼容，以避免对客户端的影响。此外，还应该做好监控和回滚准备，以应对可能出现的问题。



### 31、Seata 支持那些事务模式？

概念：Seata 是一款开源的分布式事务解决方案，致力于提供高性能和简单易用的分布式事务服务。Seata 将为用户提供了 AT、TCC、SAGA 和 XA 事务模式，为用户打造一站式的分布式解决方案。

1. AT（Atomikos）模式：AT 模式是 Seata 的默认事务模式，它通过在业务代码中嵌入事务上下文，实现分布式事务的一致性。在 AT 模式下，Seata 会对参与分布式事务的数据库进行事务协调和管理，保证所有数据库的数据一致性。提供无侵入自动补偿的事务模式 【这里是基于本地能支持事务的关系型数据库，然后 java 代码可以通过 JDBC 访问数据库， 这里的无侵入：我们只需要加上对应的注解就可以开启全局事务】
2. TCC（Try-Confirm-Cancel）模式：TCC 模式是一种补偿性事务模式，它通过定义业务的 Try、Confirm 和 Cancel 三个阶段来实现分布式事务的一致性。在 TCC 模式下，Seata 会根据业务代码中定义的 Try、Confirm 和 Cancel 方法来执行事务操作，保证事务的一致性。
3. SAGA（Saga）模式：SAGA 模式是一种长事务模式，它通过定义一系列的事务步骤来实现分布式事务的一致性。在 SAGA 模式下，Seata 会按照业务代码中定义的事务步骤顺序执行，每个步骤都有对应的补偿操作，以保证整个事务的一致性。
4. XA（X/Open XA）模式：XA 模式是一种基于两阶段提交（2 PC）的事务模式，它通过协调参与分布式事务的资源管理器（如数据库）来实现事务的一致性。在 XA 模式下，Seata 会与各个资源管理器进行协调，确保所有资源的提交或回滚操作都能正确执行，以保证分布式事务的一致性。



### 32、请简述 2 PC 流程以及优缺点

2 PC（Two-Phase Commit）是一种经典的分布式事务协议，它通过两个阶段的协调来实现分布式事务的一致性。以下是 2 PC 的基本流程：

1. 准备阶段（Prepare Phase）：
   - 协调者（Coordinator）向参与者（Participant）发送事务准备请求。
   - 参与者执行事务操作，并将事务日志和准备完成的消息发送给协调者。
   - 协调者收到所有参与者的准备完成消息后，进入下一阶段。如果有任何一个参与者发生错误或者超时，协调者将发送回滚请求给所有参与者。

2. 提交阶段（Commit Phase）：
   - 协调者向所有参与者发送事务提交请求。
   - 参与者执行事务的最终提交操作，并将提交完成的消息发送给协调者。
   - 协调者收到所有参与者的提交完成消息后，向所有参与者发送事务完成的消息，事务完成。

2 PC 的优点：

- 数据一致性：2 PC 可以保证所有参与者的数据在事务提交时保持一致，即要么全部提交成功，要么全部回滚。
- 简单易用：2 PC 是一个相对简单的分布式事务协议，易于理解和实现。

2 PC 的缺点：

- 同步阻塞：在 2 PC 的过程中，所有参与者都需要等待协调者的指令，这会导致整个事务的执行过程存在阻塞，影响性能和吞吐量。
- 单点故障：2 PC 中的协调者是一个单点，如果协调者发生故障，整个事务的执行将受到影响。
- 阻塞风暴：如果在准备阶段中的任何一个参与者发生故障或者超时，协调者将发送回滚请求给所有参与者，导致参与者之间的通信产生大量的回滚请求，可能引发阻塞风暴。

综上所述，2 PC 是一种经典的分布式事务协议，能够保证数据一致性，但存在同步阻塞和单点故障等问题。因此，在实际应用中，可以根据具体的业务需求和性能要求选择合适的分布式事务协议。



### 33、Seata中xid怎样通过 Feign 进行全局传递

在 Seata 中，XID（全局事务 ID）是用来标识一个全局事务的唯一标识符。在使用 Feign 进行服务之间的远程调用时，可以通过以下方式将 XID 传递给下游服务：

1. 通过请求头传递：在发起 Feign 请求时，可以将 XID 设置为请求头的一部分，并在下游服务中解析该请求头获取 XID。例如，可以使用 `@RequestHeader` 注解将 XID 传递给下游服务。

```java
@FeignClient(name = "serviceB")
public interface ServiceBClient {
    @RequestMapping(value = "/api/serviceB", method = RequestMethod.GET)
    String getServiceB(@RequestHeader("XID") String xid);
}
```

2. 通过请求参数传递：将 XID 作为请求参数传递给下游服务。在定义 Feign 接口方法时，将 XID 作为方法的参数，并在请求中传递给下游服务。下游服务通过解析请求参数获取 XID。

```java
@FeignClient(name = "serviceB")
public interface ServiceBClient {
    @RequestMapping(value = "/api/serviceB", method = RequestMethod.GET)
    String getServiceB(@RequestParam("XID") String xid);
}
```

通过以上方式，在 Feign 远程调用中传递 XID，可以确保全局事务上下文在服务之间的传递和一致性。同时，下游服务需要正确解析和使用 XID，以保证参与到全局事务中。



### 34、分布式事务应用的典型场景

**1、多服务**

随着互联网快速发展，微服务，SOA 等服务架构模式正在被大规模的使用，会出现很多分布式事务，问题如下：

![](https://dream-syz.github.io/7-9.png)



**2、多数据源**

**2.1 跨库**

跨库事务指的是，一个应用某个功能需要操作多个库，如下图：

![](https://dream-syz.github.io/7-10.png)



**2.2 分库分表**

通常一个库数据量比较大或者预期未来的数据量比较大，都会进行水平拆分，也就是分库分表。如下图，将订单数据库拆分成了 4 个库：

![](https://dream-syz.github.io/7-11.png)



### 35、请说一下 CAP 和 BASE 理论

CAP 和 BASE 是分布式必备理论基础

**1、CAP理论**

这个定理的内容是指的是在一个分布式系统中、Consistency（一致性）、 Availability（可用性）、Partition tolerance（分区容错性），三者不可得兼。

- 一致性（C）

一致性意思就是写操作之后进行读操作无论在哪个节点都需要返回写操作的值

- 可用性（A）

非故障的节点在合理的时间内返回合理的响应

- 分区容错性（P）

当出现网络分区后，系统能够继续工作。打个比方，这里个集群有多台机器，有台机器网络出现了问题，但是这个集群仍然可以正常工作。在分布式系统中，网络无法 100% 可靠，分区其实是一个必然现象，如果我们选择了 CA 而放弃了 P，那么当发生分区现象时，为了保证一致性，这个时候必须拒绝请求，但是 A 又不允许，所以分布式系统理论上不可能选择CA架构，只能选择 CP 或者 AP 架构。

对于CP来说，放弃可用性，追求一致性和分区容错性，我们的 zookeeper 其实就是追求的强一致。

对于 AP 来说，放弃一致性（这里说的一致性是强一致性），追求分区容错性和可用性，Nacos 就是 AP 模式，这是很多分布式系统设计时的选择，后面的 BASE 也是根据 AP 来扩展。

**2、BASE理论**

BASE 是 Basically Available（基本可用）、Soft state（软状态）和 Eventually consistent（最终一致性）三个短语的缩写。BASE 理论是对 CAP 中一致性和可用性权衡的结果，其来源于对大规模互联网系统分布式实践的总结， 是基于 CAP 定理逐步演化而来的。BASE 理论的核心思想是：**即使无法做到强一致性，但每个应用都可以根据自身业务特点，采用适当的方式来使系统达到最终一致性。**

- 基本可用

基本可用是指分布式系统在出现不可预知故障的时候，允许损失部分可用性—-注意，这绝不等价于系统不可用。比如：

（1）**响应时间上的损失**。正常情况下，一个在线搜索引擎需要在0.5秒之内返回给用户相应的查询结果，但由于出现故障，查询结果的响应时间增加了 1~2 秒

（2）**系统功能上的损失**：正常情况下，在一个电子商务网站上进行购物的时候，消费者几乎能够顺利完成每一笔订单，但是在一些节日大促购物高峰的时候，由于消费者的购物行为激增，为了保护购物系统的稳定性，部分消费者可能会被引导到一个降级页面

- 软状态

软状态指允许系统中的数据存在中间状态，并认为该中间状态的存在不会影响系统的整体可用性，即允许系统在不同节点的数据副本之间进行数据同步的过程存在延时

- 最终一致性

最终一致性强调的是所有的数据副本，在经过一段时间的同步之后，最终都能够达到一个一致的状态。因此，最终一致性的本质是需要系统保证最终数据能够达到一致，而不需要实时保证系统数据的强一致性。



### 36、简述 Seata 的 AT 模式两阶段过程

Seata的AT（Atomikos）模式是一种基于应用程序代码的分布式事务处理模式。它通过两阶段提交（Two-Phase Commit，2PC）的方式来实现分布式事务的一致性。

下面是Seata AT模式的两阶段过程：

1. 阶段一：准备阶段（Prepare Phase）
   - 应用程序发起一个分布式事务，并向Seata事务协调器注册该事务。
   - Seata 事务协调器将该事务注册到全局事务日志中，并生成一个全局事务ID（Global Transaction ID）。
   - 应用程序在本地事务执行前，会先向Seata事务协调器发送分支事务的注册请求。
   - Seata 事务协调器会将分支事务注册到全局事务中，并生成一个分支事务ID（Branch Transaction ID）。
   - 应用程序执行本地事务，并将事务执行结果（成功或失败）发送给 Seata 事务协调器。

2. 阶段二：提交/回滚阶段（Commit/Rollback Phase）
   - 如果所有分支事务的本地事务都执行成功，Seata 事务协调器会发送一个全局提交请求给各个分支事务参与者。
   - 分支事务参与者接收到全局提交请求后，会执行本地事务的提交操作，并将提交结果发送给 Seata 事务协调器。
   - 如果任何一个分支事务的本地事务执行失败，Seata 事务协调器会发送一个全局回滚请求给各个分支事务参与者。
   - 分支事务参与者接收到全局回滚请求后，会执行本地事务的回滚操作，并将回滚结果发送给 Seata 事务协调器。

通过这个两阶段的过程，Seata AT 模式可以确保分布式事务的一致性。如果所有分支事务的本地事务执行成功，那么全局提交请求将被发送，所有分支事务都会提交。如果任何一个分支事务的本地事务执行失败，那么全局回滚请求将被发送，所有分支事务都会回滚。



### 37、简述 Eureka 自我保护机制

Eureka 服务端会检查最近 15 分钟内所有 Eureka 实例正常心跳占比，如果低于 85% 就会触发自我保护机制。触发了保护机制，Eureka将暂时把这些失效的服务保护起来，不让其过期，但这些服务也并不是永远不会过期。Eureka 在启动完成后，每隔 60 秒会检查一次服务健康状态，如果这些被保护起来失效的服务过一段时间后（默认 90 秒）还是没有恢复，就会把这些服务剔除。如果在此期间服务恢复了并且实例心跳占比高于 85% 时，就会自动关闭自我保护机制。

阈值的计算方式：

每分钟 能接受最少的续租次数 = 微服务实例总数 * （60 s / 实例的续约时间间隔：30s ） * 有效的心跳比率 默认 85%

100 乘以（60 /30） 乘以 85% = 170

自我保护机制更改条件： 注册、服务下架、 服务初始化、15 分钟更新一次

自我保护机制的触发位置： 服务剔除



### 38、简述 Eureka 集群架构

参考官网：https://github.com/Netflix/eureka/wiki/Eureka-at-a-glance

- Register（服务注册）：把自己的 IP 和端口注册给 Eureka。

- Renew（服务续约）：发送心跳包，每 30 秒发送一次。告诉 Eureka 自己还活着。

- Cancel（服务下线）：当 provider 关闭时会向 Eureka 发送消息，把自己从服务列表中删除。防止 consumer 调用到不存在的服务。

- Get Registry（获取服务注册列表）：获取其他服务列表。

- Make Remote Call（远程调用）：完成服务的远程调用。

- Replicate（集群中数据同步）：eureka 集群中的数据复制与同步。



### 39、从 Eureka 迁移到 Nacos 的解决方案

版本依赖关系：

https://github.com/alibaba/spring-cloud-alibaba/wiki/%E7%89%88%E6%9C%AC%E8%AF%B4%E6%98%8E

![](https://dream-syz.github.io/7-12.png)

- 换依赖

  ```
   <spring-cloud.alibaba.version>2.2.9.RELEASE</spring-cloud.alibaba.version>
  <dependency>
   <groupId>com.alibaba.cloud</groupId>
   <artifactId>spring-cloud-alibaba-dependencies</artifactId>
   <version>${spring-cloud.alibaba.version}</version>
   <type>pom</type>
   <scope>import</scope>
  </dependency>
  
  <dependencies>
   <dependency>
   <groupId>com.alibaba.cloud</groupId>
   <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId>
   </dependency>
  </dependencies>
  ```

- 配置换一下

  ```
  spring:
   cloud:
   nacos:
   discovery:
   server-addr: localhost:8848
   service: msb-order
  ```

- 使用 `@EnableDiscoveryClient` 开启注册发现

  ```
  @EnableDiscoveryClient // 这是官方提供的 ,我们以后可能切换其他的注册中心比如说 nacos，那我们就直接切换就行了
  
  //@EnableEurekaClient // 是 netflix 提供的，如果用这个注解就只能服务于 eureka
  ```

- 梳理项目关系

		只提供调用的基础服务先进行更改，进行迁移



### 40、Apollo的整体架构

![](https://dream-syz.github.io/7-13.png)

- Config Service 提供配置的读取、推送等功能，服务对象是 Apollo 客户端。

- Admin Service 提供配置的修改、发布等功能，服务对象是 Apollo Portal（管理界面）。

- Config Service 和 Admin Service 都是多实例、无状态部署，所以需要将自己注册到 Eureka 中并保持心跳。

- 在 Eureka 之上我们架了一层 Meta Serve r用于封装 Eureka 的服务发现接口。

- Client 通过域名访问 Meta Server 获取 Config Service 服务列表（IP+Port），而后直接通过 IP+Port 访问服务，同时在 Client 侧会做 load balance、错误重试。

- Portal 通过域名访问 Meta Server 获取 Admin Service 服务列表（IP+Port），而后直接通过 IP+Port 访问服务，同时在 Portal 侧会做 load balance、错误重试。

为了简化部署，我们实际上会把 Config Service、Eureka 和 Meta Server 三个逻辑角色部署在同一个 JVM 进程中。



### 41、Apollo的整体架构可靠性分析

| 场景                     | 影响                                  | 降级                                                         |
| ------------------------ | ------------------------------------- | ------------------------------------------------------------ |
| 某台 Config Service 下线 | 无影响                                |                                                              |
| 所有 Config Service 下线 | 客户端无法读取最新配置，Portal 无影响 | 客户端重启时，可以读取本地缓存配置文件。如果是新扩容的机器，可以从其它机器上获取已缓存的配置文件，具体信息可以参考 Java 客户端使用指南 - 1.2.3 本地缓存路径 |
| 某台 Admin Service下线   | 无影响                                |                                                              |
| 所有 Admin Service下线   | 客户端无影响，Portal无法更新配置      |                                                              |
| 某台 Portal 下线         | 无影响                                |                                                              |
| 全部 Portal 下线         | 客户端无影响，Portal无法更新配置      |                                                              |
| 某个数据中心下线         | 无影响                                |                                                              |
| 数据库宕机               | 客户端无影响，Portal无法更新配置      | Config Service 开启配置缓存后，对配置的读取不受数据库宕机影响 |



### 42、Apollo配置发布后的实时推送设计

在配置中心中，一个重要的功能就是配置发布后实时推送到客户端。下面我们简要看一下这块是怎么设计实现的。

![](https://dream-syz.github.io/7-14.png)

上图简要描述了配置发布的大致过程：

1. 用户在 Portal 操作配置发布

2. Portal 调用 Admin Service 的接口操作发布

3. Admin Service 发布配置后，发送 `ReleaseMessage` 给各个Config Service

4. Config Service收到 `ReleaseMessage` 后，通知对应的客户端

   ![](https://dream-syz.github.io/7-15.png)



### 43、Apollo 客户端设计

![](https://dream-syz.github.io/7-16.png)

上图简要描述了 Apollo 客户端的实现原理：

1. 客户端和服务端保持了一个长连接，从而能第一时间获得配置更新的推送。（通过Http Long Polling实现）

2. 客户端还会定时从Apollo配置中心服务端拉取应用的最新配置。
3. 客户端从Apollo配置中心服务端获取到应用的最新配置后，会保存在内存中
4. 客户端会把从服务端获取到的配置在本地文件系统缓存一份，在遇到服务不可用，或网络不通的时候，依然能从本地恢复配置

5. 应用程序可以从 Apollo 客户端获取最新的配置、订阅配置更新通知



### 44、Zuul 有几种过滤器类型？分别是？

Zuul 有四种过滤器类型，分别是：

1. 前置过滤器（Pre Filter）：在请求被路由之前执行。可以用来做身份验证、请求参数校验、记录调试信息等操作。

2. 路由过滤器（Route Filter）：用于将请求转发到目标服务实例。可以修改请求的路由规则、请求头等信息。

3. 这种过滤器用于构建发送给微服务的请求，并使用 Apache HttpClient 或 Netfilx feign 请求

4. 后置过滤器（Post Filter）：在目标服务实例处理完请求并返回响应之后执行。可用来为响应添加标准的HTTP Header、收

   集统计信息和指标、将响应从微服务发送给客户端，可以对响应进行处理，如添加响应头、修改响应内容等。

5. 错误过滤器（Error Filter）：在整个请求生命周期中发生错误时执行。可以对错误进行处理，如记录错误日志、返回自定义错误信息等。

通过使用这四种过滤器类型，可以在Zuul网关中实现各种功能，如安全认证、请求转发、日志记录等。开发人员可以自定义各种过滤器，并将它们按照需要配置到Zuul网关中，以实现特定的功能和需求。





## Dubbo 篇

其实关于 Dubbo 的面试题，我觉得最好的文档应该还是官网，因为官网有中文版，照顾了很多阅读英文文档吃力的小伙伴。但是官网内容挺多的，于是这里就结合官网和平时面试被问的相对较多的题目整理了一下。

### 1、 说说一次 Dubbo 服务请求流程？

基本工作流程：

![](https://dream-syz.github.io/8-1.png)

上图中角色说明：

| 节点      | 角色说明                               |
| --------- | -------------------------------------- |
| Provider  | 暴露服务的服务提供方                   |
| Consumer  | 调用远程服务的服务消费方               |
| Registry  | 服务注册与发现的注册中心               |
| Monitor   | 统计服务的调用次数和调用时间的监控中心 |
| Container | 服务运行容器                           |



### 2、说说 Dubbo 工作原理

工作原理分 10 层：

- 第一层：service 层，接口层，给服务提供者和消费者来实现的（留给开发人员来实现）；

- 第二层：config 层，配置层，主要是对 Dubbo 进行各种配置的，Dubbo 相关配置；

- 第三层：proxy 层，服务代理层，透明生成客户端的 stub 和服务单的 skeleton，调用的是接口，实现类没有，所以得生成代理，代理之间再进行网络通讯、负责均衡等；

- 第四层：registry 层，服务注册层，负责服务的注册与发现；
- 第五层：cluster 层，集群层，封装多个服务提供者的路由以及负载均衡，将多个实例组合成一个服务；

- 第六层：monitor 层，监控层，对 rpc 接口的调用次数和调用时间进行监控；

- 第七层：protocol 层，远程调用层，封装 rpc 调用；

- 第八层：exchange 层，信息交换层，封装请求响应模式，同步转异步；

- 第九层：transport 层，网络传输层，抽象 mina 和 netty 为统一接口；

- 第十层：serialize 层，数据序列化层。

这是个很坑爹的面试题，但是很多面试官又喜欢问，你真的要背么？你能背那还是不错的，我建议不要背，你就想想 Dubbo 服务调用过程中应该会涉及到哪些技术，把这些技术串起来就 OK 了。

**面试扩散**

如果让你设计一个 RPC 框架，你会怎么做？其实你就把上面这个工作原理中涉及的到技术点总结一下就行了。



### 3、Dubbo 支持哪些协议？

![](https://dream-syz.github.io/8-2.png)

还有三种，混个眼熟就行：Memcached 协议、Redis 协议、Rest 协议。

上图基本上把序列化的方式也罗列出来了。

详细请参考：Dubbo 官网：http://dubbo.apache.org/zh-cn/docs/user/references/protocol/dubbo.html。



### 4、注册中心挂了，consumer 还能不能调用 provider？

可以。因为刚开始初始化的时候，consumer 会将需要的所有提供者的地址等信息拉取到本地缓存，所以注册中心挂了可以继续通信。但是 provider 挂了，那就没法调用了。

![](https://dream-syz.github.io/8-3.png)

关键字：consumer 本地缓存服务列表



### 5、怎么实现动态感知服务下线的呢？

![](https://dream-syz.github.io/8-4.png)

服务订阅通常有 pull 和 push 两种方式：

- pull 模式需要客户端定时向注册中心拉取配置；

- push 模式采用注册中心主动推送数据给客户端。

Dubbo ZooKeeper 注册中心采用是事件通知与客户端拉取方式。服务第一次订阅的时候将会拉取对应目录下全量数据，然后在订阅的节点注册一个 watcher。一旦目录节点下发生任何数据变化，ZooKeeper 将会通过 watcher 通知客户端。客户端接到通知，将会重新拉取该目录下全量数据，并重新注册 watcher。利用这个模式，Dubbo 服务就可以做到服务的动态发现。

注意：ZooKeeper 提供了“心跳检测”功能，它会定时向各个服务提供者发送一个请求（实际上建立的是一个 socket 长连接），如果长期没有响应，服务中心就认为该服务提供者已经“挂了”，并将其剔除。



### 6、Dubbo 负载均衡策略？

- 随机（默认）：随机来

- 轮训：一个一个来

- 活跃度：机器活跃度来负载

- 一致性 hash：落到同一台机器上



### 7、Dubbo 容错策略

**failover cluster** **模式**

provider 宕机重试以后，请求会分到其他的 provider 上，默认两次，可以手动设置重试次数，建议把写操作重试次数设置成 0。

**failback** **模式**

失败自动恢复会在调用失败后，返回一个空结果给服务消费者。并通过定时任务对失败的调用进行重试，适合执行消息通知等操作。

**failfast cluster** **模式**

快速失败只会进行一次调用，失败后立即抛出异常。适用于幂等操作、写操作，类似于 failover cluster 模式中重试次数设置为 0 的情况。

**failsafe cluster** **模式**

失败安全是指，当调用过程中出现异常时，仅会打印异常，而不会抛出异常。适用于写入审计日志等操作。

**forking cluster** **模式**

并行调用多个服务器，只要一个成功即返回。通常用于实时性要求较高的读操作，但需要浪费更多服务资源。可通过 forks="2" 来设置最大并行数。

**broadcacst cluster** **模式**

广播调用所有提供者，逐个调用，任意一台报错则报错。通常用于通知所有提供者更新缓存或日志等本地资源信息。



### 8、Dubbo 动态代理策略有哪些？

默认使用 javassist 动态字节码生成，创建代理类，但是可以通过 SPI 扩展机制配置自己的动态代理策略。



### 9、说说 Dubbo 与 Spring Cloud 的区别？

这是很多面试官喜欢问的问题，本人认为其实他们没什么关联之处，但是硬是要问区别，那就说说吧。

回答的时候主要围绕着四个关键点来说：通信方式、注册中心、监控、断路器，其余像 Spring 分布式配置、服务网关肯定得知道。

**通信方式**

Dubbo 使用的是 RPC 通信；Spring Cloud 使用的是 HTTP RestFul 方式。

**注册中心**

Dubbo 使用 ZooKeeper（官方推荐），还有 Redis、Multicast、Simple 注册中心，但不推荐。

Spring Cloud 使用的是 Spring Cloud Netflix Eureka。

**监控**

Dubbo 使用的是 Dubbo-monitor；Spring Cloud 使用的是 Spring Boot admin。

**断路器**

Dubbo 在断路器这方面还不完善，Spring Cloud 使用的是 Spring Cloud Netflix Hystrix。

分布式配置、网关服务、服务跟踪、消息总线、批量任务等。

Dubbo 目前可以说还是空白，而 Spring Cloud 都有相应的组件来支撑。



### 10、Zookeeper 和 Dubbo 的关系？

**Zookeeper 的作用**

zookeeper 用来注册服务和进行负载均衡，哪一个服务由哪一个机器来提供必需让调用者知道，简单来说就是 **ip 地址和服务名称的对应关系**。当然也可以通过硬编码的方式把这种对应关系在调用方业务代码中实现，但是如果提供服务的机器挂掉调用者无法知晓，如果不更改代码会继续请求挂掉的机器提供服务。zookeeper 通过心跳机制可以检测挂掉的机器并将挂掉机器的 ip 和服务对应关系从列表中删除。至于支持高并发，简单来说就是横向扩展，在不更改代码的情况通过添加机器来提高运算能力。通过添加新的机器向 zookeeper 注册服务，服务的提供者多了能服务的客户就多了。

**dubbo**

是管理中间层的工具，在业务层到数据仓库间有非常多服务的接入和服务提供者需要调度，dubbo 提供一个框架解决这个问题。 注意这里的 dubbo 只是一个框架，至于你架子上放什么是完全取决于你的，就像一个汽车骨架，你需要配你的轮子引擎。这个框架中要完成调度必须要有一个分布式的注册中心，储存所有服务的元数据，你可以用 zk，也可以用别的，只是大家都用 zk。

**zookeeper 和 dubbo 的关系**

Dubbo 将注册中心进行抽象，它可以外接不同的存储媒介给注册中心提供服务，有 ZooKeeper，Memcached，Redis 等。

引入了 ZooKeeper 作为存储媒介，也就把 ZooKeeper 的特性引进来。首先是负载均衡，单注册中心的承载能力是有限的，在流量达到一定程度的时候就需要分流，负载均衡就是为了分流而存在的，一个 ZooKeeper 群配合相应的 Web 应用就可以很容易达到负载均衡；资源同步，单有负载均衡还不够，节点之间的数据和资源需要同步，ZooKeeper 集群就天然具备有这样的功能；命名服务，将树状结构用于维护全局的服务地址列表，服务提供者在启动的时候，向 ZooKeeper 上的指定节点 /dubbo/${serviceName}/providers 目录下写入自己的 URL 地址，这个操作就完成了服务的发布。 其他特性还有 Master 选举，分布式锁等。

![](https://dream-syz.github.io/8-5.png)





## Nginx 篇

### 1、简述一下什么是 Nginx，它有什么优势和功能？

Nginx 是一个 web 服务器和反向代理服务器，用于 HTTP、HTTPS、SMTP、POP3 和 IMAP 协议。因它的稳定性、丰富的功能集、示例配置文件和低系统资源的消耗而闻名。

> Nginx---Ngine X，是一款免费的、自由的、开源的、高性能 HTTP 服务器和反向代理服务器；也是一个 IMAP、POP3、SMTP 代理服务器；Nginx 以其高性能、稳定性、丰富的功能、简单的配置和低资源消耗而闻名。
>
> 也就是说 Nginx 本身就可以托管网站（类似于 Tomcat 一样），进行 Http 服务处理，也可以作为反向代理服务器 、负载均衡器和HTTP 缓存。
>
> Nginx 解决了服务器的 C10K（就是在一秒之内连接客户端的数目为 10k 即 1 万）问题。它的设计不像传统的服务器那样使用线程处理请求，而是一个更加高级的机制—事件驱动机制，是一种异步事件驱动结构。

#### **优点：**

**（1）更快** 这表现在两个方面：一方面，在正常情况下，单次请求会得到更快的响应；另一方面，在高峰期（如有数以万计的并发请求），Nginx 可以比其他 Web 服务器更快地响应请求。

**（2）高扩展性，跨平台** Nginx 的设计极具扩展性，它完全是由多个不同功能、不同层次、不同类型且耦合度极低的模块组成。因此，当对某一个模块修复 Bug 或进行升级时，可以专注于模块自身，无须在意其他。而且在 HTTP 模块中，还设计了 HTTP 过滤器模块：一个正常的 HTTP 模块在处理完请求后，会有一串 HTTP 过滤器模块对请求的结果进行再处理。这样，当我们开发一个新的 HTTP 模块时，不但可以使用诸如 HTTP 核心模块、events 模块、log 模块等不同层次或者不同类型的模块，还可以原封不动地复用大量已有的 HTTP 过滤器模块。这种低耦合度的优秀设计，造就了 Nginx 庞大的第三方模块，当然，公开的第三方模块也如官方发布的模块一样容易使用。 Nginx 的模块都是嵌入到二进制文件中执行的，无论官方发布的模块还是第三方模块都是如此。这使得第三方模块一样具备极其优秀的性能，充分利用 Nginx 的高并发特性，因此，许多高流量的网站都倾向于开发符合自己业务特性的定制模块。

**（3）高可靠性：用于反向代理，宕机的概率微乎其微** 高可靠性是我们选择 Nginx 的最基本条件，因为 Nginx 的可靠性是大家有目共睹的，很多家高流量网站都在核心服务器上大规模使用 Nginx。Nginx 的高可靠性来自于其核心框架代码的优秀设计、模块设计的简单性；另外，官方提供的常用模块都非常稳定，每个 worker 进程相对独立，master 进程在 1 个 worker 进程出错时可以快速“拉起”新的 worker子进程提供服务。

**（4）低内存消耗** 一般情况下，10 000 个非活跃的 HTTP Keep-Alive 连接在 Nginx 中仅消耗 2.5 MB 的内存，这是 Nginx 支持高并发连接的基础。 

**（5）单机支持 10 万以上的并发连接** 这是一个非常重要的特性！随着互联网的迅猛发展和互联网用户数量的成倍增长，各大公司、网站都需要应付海量并发请求，一个能够在峰值期顶住 10 万以上并发请求的 Server，无疑会得到大家的青睐。理论上，Nginx 支持的并发连接上限取决于内存，10 万远未封顶。当然，能够及时地处理更多的并发请求，是与业务特点紧密相关的。

**（6）热部署** master 管理进程与 worker 工作进程的分离设计，使得 Nginx 能够提供热部署功能，即可以在 7×24 小时不间断服务的前提下，升级 Nginx 的可执行文件。当然，它也支持不停止服务就更新配置项、更换日志文件等功能。

**（7）最自由的 BSD 许可协议** 这是 Nginx 可以快速发展的强大动力。BSD 许可协议不只是允许用户免费使用 Nginx，它还允许用户在自己的项目中直接使用或修改 Nginx 源码，然后发布。这吸引了无数开发者继续为 Nginx 贡献自己的智慧。 

以上 7 个特点当然不是 Nginx的全部，拥有无数个官方功能模块、第三方功能模块使得 Nginx 能够满足绝大部分应用场景，这些功能模块间可以叠加以实现更加强大、复杂的功能，有些模块还支持 Nginx 与 Perl、Lua 等脚本语言集成工作，大大提高了开发效率。这些特点促使用户在寻找一个 Web 服务器时更多考虑 Nginx。 选择 Nginx 的核心理由还是它能在支持高并发请求的同时保持高效的服务。



### 2、Nginx 是如何处理一个 HTTP 请求的呢？

Nginx 是一个高性能的 Web 服务器，能够同时处理大量的并发请求。它结合多进程机制和异步机制，异步机制使用的是异步非阻塞方式，接下来就给大家介绍一下 Nginx 的多线程机制和异步非阻塞机制 。

**1、多进程机制**

服务器每当收到一个客户端时，就有服务器主进程 （ master process ）生成一个 子进程（worker process ）出来和客户端建立连接进行交互，直到连接断开，该子进程就结束了。

使用进程的好处是各个进程之间相互独立，不需要加锁，减少了使用锁对性能造成影响，同时降低编程的复杂度，降低开发成本。其次，采用独立的进程，可以让进程互相之间不会影响 ，如果一个进程发生异常退出时，其它进程正常工作， master 进程则很快启动新的 worker 进程，确保服务不会中断，从而将风险降到最低。

缺点是操作系统生成一个子进程需要进行内存复制等操作，在资源和时间上会产生一定的开销。当有大量请求时，会导致系统性能下降。

**2、异步非阻塞机制**

每个工作进程使用异步非阻塞方式 ，可以处理多个客户端请求 。

当某个工作进程接收到客户端的请求以后，调用 IO 进行处理，如果不能立即得到结果，就去处理其他请求 （即为非阻塞 ）；而客户端 在此期间也无需等待响应 ，可以去处理其他事情（即为异步 ）。

当 IO 返回时，就会通知此工作进程 ；该进程得到通知，暂时挂起当前处理的事务去响应客户端请求 。



### 3、列举一些 Nginx 的特性

Nginx 服务器的特性包括：

1. 反向代理 / L7 负载均衡器

2. 嵌入式 Perl 解释器

3. 动态二进制升级

4. 可用于重新编写 URL，具有非常好的 PCRE 支持

   

### 4、请列举 Nginx 和 Apache 之间的不同点

| Nginx                                                       | Apache                                                       |
| ----------------------------------------------------------- | ------------------------------------------------------------ |
| Nginx 是一个基于事件的 Web 服务器，所有请求都由一个线程处理 | Apache  是一个基于流程的服务器，单个线程处理单个请求         |
| Nginx  避免子进程的概念，Nginx  类似于速度                  | Apache  基于子进程，Apache  类似于功率                       |
| Nginx  在内存消耗和连接方面比较好                           | Apache  内存消耗和连接上并没有提高                           |
| Nginx  在负载均衡方面表现较好                               | 当流量达到进程的极限时，Apache  将拒绝新的连接               |
| 对于 PHP 来说，Nginx 可能更可取，因为它支持 PHP             | Apache 支持的 PHP、Python、Perl 和其他语言使用插件，当应用程序基于Python 或 Ruby 时，它非常有用 |
| Nginx 不支持像 IBMi 和 OpenVMS 一样的 OS                    | Apache 支持更多的 OS                                         |
| Nginx 只具有核心功能                                        | Apache 提供了比 Nginx 更多的功能                             |
| Nginx 的性能和可伸缩性不依赖于硬件                          | Apache 依赖于 CPU 和内存等硬件组件                           |



### 5、在 Nginx 中，如何使用未定义的服务器名称来阻止处理请求？

只需将请求删除的服务器定义为：

```properties
Server{
	listen 80;
	server_name "";
	return 444;
}
```

这里，服务器名被保留为一个空字符串，它将在没有“主机”头字段的情况下匹配请求，而一个特殊的 Nginx 的非标准代码 444 被返回，从而终止连接。

**一般推荐 worker 进程数与 CPU 内核数一致，这样一来不存在大量的子进程生成和管理任务，避免了进程之间竞争 CPU 资源和进程切换的开销**。而且 Nginx 为了更好的利用多核特性 ，提供了 CPU 亲缘性的绑定选项，我们可以将某一个进程绑定在某一个核上，这样就不会因为进程的切换带来 Cache 的失效。

**对于每个请求，有且只有一个工作进程对其处理**。首先，每个 worker 进程都是从 master 进程 fork 过来。在 master 进程里面，先建立好需要 listen 的 socket（listenfd） 之后，然后再 fork 出多个 worker 进程。所有 worker 进程的 listenfd 会在新连接到来时变得可读，为保证只有一个进程处理该连接，所有 worker 进程在注册 listenfd 读事件前抢占 accept_mutex ，抢到互斥锁的那个进程注册 listenfd 读事件 ，在读事件里调用 accept 接受该连接。

当一个 worker 进程在 accept 这个连接之后，就开始读取请求、解析请求、处理请求，产生数据后，再返回给客户端 ，最后才断开连接。这样一个完整的请求就是这样的了。我们可以看到，一个请求，完全由 worker 进程来处理，而且只在一个 worker 进程中处理。

在 Nginx 服务器的运行过程中， 主进程和工作进程需要进程交互。**交互依赖于 Socket 实现的管道来实现**。



### 6、请解释 Nginx 服务器上的 Master 和 Worker 进程分别是什么?

主程序 Master process 启动后，通过一个 for 循环来接收和处理外部信号 ；

主进程通过 fork() 函数产生 worker 子进程 ，每个子进程执行一个 for 循环来实现 Nginx 服务器对事件的接收和处理 。



### 7、请解释代理中的正向代理和反向代理

首先，代理服务器一般指局域网内部的机器通过代理服务器发送请求到互联网上的服务器，代理服务器一般作用在客户端。例如：GoAgent 翻墙软件。我们的客户端在进行翻墙操作的时候，我们使用的正是正向代理，通过正向代理的方式，在我们的客户端运行一个软件，将我们的 HTTP 请求转发到其他不同的服务器端，实现请求的分发。

反向代理服务器作用在服务器端，它在服务器端接收客户端的请求，然后将请求分发给具体的服务器进行处理，然后再将服务器的相应结果反馈给客户端。Nginx 就是一个反向代理服务器软件。

可以看出：客户端必须设置正向代理服务器，当然前提是要知道正向代理服务器的 IP 地址，还有代理程序的端口。 反向代理正好与正向代理相反，对于客户端而言代理服务器就像是原始服务器，并且客户端不需要进行任何特别的设置。客户端向反向代理的命名空间（name-space）中的内容发送普通请求，接着反向代理将判断向何处（原始服务器）转交请求，并将获得的内容返回给客户端。



### 8、解释 Nginx 用途

Nginx 服务器的最佳用法是在网络上部署动态 HTTP 内容，使用 SCGI、WSGI 应用程序服务器、用于脚本的 FastCGI 处理程序。它还可以作为负载均衡器。





## **MQ** 篇

### 1、为什么要使用 MQ

**核心：解耦，异步，削峰**

**（1）解耦：**A 系统发送数据到 BCD 三个系统，通过接口调用发送。如果 E 系统也要这个数据呢？那如果 C 系统现在不需要了呢？A 系统负责人几乎崩溃......A 系统跟其它各种乱七八糟的系统严重耦合，A 系统产生一条比较关键的数据，很多系统都需要 A 系统将这个数据发送过来。如果使用 MQ，A 系统产生一条数据，发送到 MQ 里面去，哪个系统需要数据自己去 MQ 里面消费。如果新系统需要数据，直接从 MQ 里消费即可；如果某个系统不需要这条数据了，就取消对 MQ 消息的消费即可。这样下来，A 系统压根儿不需要去考虑要给谁发送数据，不需要维护这个代码，也不需要考虑人家是否调用成功、失败超时等情况。

就是一个系统或者一个模块，调用了多个系统或者模块，互相之间的调用很复杂，维护起来很麻烦。但是其实这个调用是不需要直接同步调用接口的，如果用 MQ 给它异步化解耦。

**（2）异步：**A 系统接收一个请求，需要在自己本地写库，还需要在 BCD 三个系统写库，自己本地写库要 3ms，BCD 三个系统分别写库要 300ms、450ms、200ms。最终请求总延时是 3 + 300 + 450 + 200 = 953ms，接近 1s，用户感觉搞个什么东西，慢死了慢死了。用户通过浏览器发起请求。如果使用 MQ，那么 A 系统连续发送 3 条消息到 MQ 队列中，假如耗时 5ms，A 系统从接受一个请求到返回响应给用户，总时长是 3 + 5 = 8ms。

**（3）削峰：**减少高峰时期对服务器压力。



### 2、MQ 有什么优缺点

优点上面已经说了，就是在特殊场景下有其对应的好处，**解耦、异步、削峰**。

#### **缺点有以下几个：**

**系统可用性降低** 系统引入的外部依赖越多，越容易挂掉。万一 MQ 挂了，MQ 一挂，整套系统崩溃，你不就完了？

**系统复杂度提高** 硬生生加个 MQ 进来，你怎么保证消息没有重复消费？怎么处理消息丢失的情况？怎么保证消息传递的顺序性？问题一大堆。

**一致性问题** A 系统处理完了直接返回成功了，人都以为你这个请求就成功了；但是问题是，要是

BCD 三个系统那里，BD 两个系统写库成功了，结果 C 系统写库失败了，咋整？你这数据就不一致了。



### 3、Kafka、ActiveMQ、RabbitMQ、RocketMQ 都有什么区别？

对于吞吐量来说 kafka 和 RocketMQ 支撑高吞吐，ActiveMQ 和 RabbitMQ 比他们低一个数量级。对于延迟量来说 RabbitMQ 是最低的。

**1.从社区活跃度**

按照目前网络上的资料，RabbitMQ 、activeMQ 、ZeroMQ 三者中，综合来看，RabbitMQ 是首选。

**2.持久化消息比较**

ActiveMQ 和 RabbitMQ 都支持。持久化消息主要是指我们机器在不可抗力因素等情况下挂掉了，消息不会丢失的机制。

**3.综合技术实现**

可靠性、灵活的路由、集群、事务、高可用的队列、消息排序、问题追踪、可视化管理工具、插件系统等等。

RabbitMQ / Kafka 最好，ActiveMQ 次之，ZeroMQ 最差。当然 ZeroMQ 也可以做到，不过自己必须手动写代码实现，代码量不小。尤其是可靠性中的：持久性、投递确认、发布者证实和高可用性。

**4.高并发**

毋庸置疑，RabbitMQ 最高，原因是它的实现语言是天生具备高并发高可用的 erlang 语言。

**5.比较关注的比较，** **RabbitMQ** **和** **Kafka**

RabbitMQ 比 Kafka 成熟，在可用性上，稳定性上，可靠性上， RabbitMQ 胜于 Kafka （理论上）。

另外，Kafka 的定位主要在日志等方面， 因为 Kafka 设计的初衷就是处理日志的，可以看做是一个日志（消息）系统一个重要组件，针对性很强，所以如果业务方面还是建议选择 RabbitMq 。

还有就是，Kafka 的性能（吞吐量、TPS ）比 RabbitMq 要高出来很多。



### 4、如何保证高可用的？

RabbitMQ 是比较有代表性的，因为是 **基于主从**（非分布式）做高可用性的，我们就以 RabbitMQ为例子讲解第一种 MQ 的高可用性怎么实现。**RabbitMQ 有三种模式：单机模式、普通集群模式、镜像集群模式**。

单机模式，就是 Demo 级别的，一般就是你本地启动了玩玩儿的?，没人生产用单机模式

普通集群模式，意思就是在多台机器上启动多个 RabbitMQ 实例，每个机器启动一个。你 **创建的 queue，只会放在一个 RabbitMQ** **实例上**，但是每个实例都同步 queue 的元数据（元数据可以认为是 queue 的一些配置信息，通过元数据，可以找到 queue 所在实例）。你消费的时候，实际上如果连接到了另外一个实例，那么那个实例会从 queue 所在实例上拉取数据过来。**这方案主要是提高吞吐量的**，就让集群中多个节点来服务某个 queue 的读写操作。镜像集群模式：这种模式，才是所谓的 RabbitMQ 的高可用模式。

跟普通集群模式不一样的是，在镜像集群模式下，你创建的 queue，无论元数据还是 queue 里的消息都会**存在于多个实例上**，就是说，每个 RabbitMQ 节点都有这个 queue 的一个完整镜像，包含 queue 的全部数据的意思。然后每次你写消息到 queue 的时候，都会自动把消息同步到多个实例的 queue 上。RabbitMQ 有很好的管理控制台，就是在后台新增一个策略，这个策略是镜像集群模式的策略，指定的时候是可以要求数据同步到所有节点的，也可以要求同步到指定数量的节点，再次创建 queue 的时候，应用这个策略，就会自动将数据同步到其他的节点上去了。这样的话，好处在于，你任何一个机器宕机了，没事儿，其它机器（节点）还包含了这个 queue 的完整数据，别的 consumer 都可以到其它节点上去消费数据。坏处在于，第一，这个性能开销也太大了吧，消息需要同步到所有机器上，导致网络带宽压力和消耗很重！RabbitMQ 一个 queue 的数据都是放在一个节点里的，镜像集群下，也是每个节点都放这个 queue 的完整数据。

Kafka 一个最基本的架构认识：由多个 broker 组成，每个 broker 是一个节点；你创建一个 topic，这个 topic 可以划分为多 partition，每个 partition 可以存在于不同的 broker 上，每个 partition 就放一部分数据。这就是天然的分布式消息队列，就是说一个 topic 的数据，是**分散放在多个机器上的，每个机器就放一部分数据**。Kafka 0.8 以后，提供了 HA 机制，就是 replica（复制品） 副本机制。每个 partition 的数据都会同步到其它机器上，形成自己的多个 replica 副本。所有 replica 会选举一个 leader 出来，那么生产和消费都跟这个 leader 打交道，然后其他 replica 就是 follower。写的时候，leader 会负责把数据同步到所有 follower 上去，读的时候就直接读 leader上的数据即可。只能读写 leader？很简单，要是你可以随意读写每个 follower，那么就要 care 数据一致性的问题，系统复杂度太高，很容易出问题。Kafka 会均匀地将一个 partition 的所有 replica 分布在不同的机器上，这样才可以提高容错性。因为如果某个 broker 宕机了，没事儿，那个 broker上面的 partition 在其他机器上都有副本的，如果这上面有某个 partition 的 leader，那么此时会从 follower 中重新选举一个新的 leader 出来，大家继续读写那个新的 leader 即可。这就有所谓的高可用性了。写数据的时候，生产者就写 leader，然后 leader 将数据落地写本地磁盘，接着其他 follower 自己主动从 leader 来 pull 数据。一旦所有 follower 同步好数据了，就会发送 ack 给 leader，leader 收到所有 follower 的 ack 之后，就会返回写成功的消息给生产者。（当然，这只是其中一种模式，还可以适当调整这个行为）消费的时候，只会从 leader 去读，但是只有当一个消息已经被所有 follower 都同步成功返回 ack 的时候，这个消息才会被消费者读到。



### 5、如何保证消息的可靠传输？如果消息丢了怎么办

数据的丢失问题，可能出现在生产者、MQ、消费者中生产者丢失：生产者将数据发送到 RabbitMQ 的时候，可能数据就在半路给搞丢了，因为网络问题啥的，都有可能。此时可以选择用 RabbitMQ 提供的事务功能，就是生产者发送数据之前开启 RabbitMQ 事务`channel.txSelect`，然后发送消息，如果消息没有成功被 RabbitMQ 接收到，那么生产者会收到异常报错，此时就可以回滚事务`channel.txRollback`，然后重试发送消息；如果收到了消息，那么可以提交事务 `channel.txCommit`。吞吐量会下来，因为太耗性能。所以一般来说，如果你要确保说写 RabbitMQ 的消息别丢，可以开启 confirm 模式，在生产者那里设置开启 confirm 模式之后，你每次写的消息都会分配一个唯一的 id，然后如果写入了 RabbitMQ 中，RabbitMQ 会给你回传一个 ack 消息，告诉你说这个消息 ok 了。如果 RabbitMQ 没能处理这个消息，会回调你一个 nack 接口，告诉你这个消息接收失败，你可以重试。而且你可以结合这个机制自己在内存里维护每个消息 id 的状态，如果超过一定时间还没接收到这个消息的回调，那么你可以重发。事务机制和 cnofirm 机制最大的不同在于，事务机制是同步的，你提交一个事务之后会阻塞在那儿，但是 **confirm 机制是异步** 的，你发送个消息之后就可以发送下一个消息，然后那个消息 RabbitMQ 接收了之后会异步回调你一个接口通知你这个消息接收到了。所以一般在生产者这块避免数据丢失，都是用confirm机制的。

MQ中丢失：就是 RabbitMQ 自己弄丢了数据，这个你必须开启 RabbitMQ 的持久化，就是消息写入之后会持久化到磁盘，哪怕是 RabbitMQ 自己挂了，恢复之后会自动读取之前存储的数据，一般数据不会丢。设置持久化有两个步骤：创建 queue 的时候将其设置为持久化，这样就可以保证 RabbitMQ 持久化 queue 的元数据，但是不会持久化 queue 里的数据。第二个是发送消息的时候将消息的 deliveryMode 设置为 2，就是将消息设置为持久化的，此时 RabbitMQ 就会将消息持久化到磁盘上去。必须要同时设置这两个持久化才行，RabbitMQ 哪怕是挂了，再次重启，也会从磁盘上重启恢复 queue，恢复这个 queue 里的数据。持久化可以跟生产者那边的confirm 机制配合起来，只有消息被持久化到磁盘之后，才会通知生产者 ack 了，所以哪怕是在持久化到磁盘之前，RabbitMQ 挂了，数据丢了，生产者收不到ack，你也是可以自己重发的。注意，哪怕是你给 RabbitMQ 开启了持久化机制，也有一种可能，就是这个消息写到了 RabbitMQ 中，但是还没来得及持久化到磁盘上，结果不巧，此时 RabbitMQ 挂了，就会导致内存里的一点点数据丢失。

消费端丢失：你消费的时候，刚消费到，还没处理，结果进程挂了，比如重启了，那么就尴尬了，RabbitMQ 认为你都消费了，这数据就丢了。这个时候得用 RabbitMQ 提供的 ack 机制，简单来说，就是你关闭 RabbitMQ 的自动 ack，可以通过一个 api 来调用就行，然后每次你自己代码里确保处理完的时候，再在程序里 ack一把。这样的话，如果你还没处理完，不就没有 ack？那 RabbitMQ 就认为你还没处理完，这个时候 RabbitMQ 会把这个消费分配给别的 consumer 去处理，消息是不会丢的。

![](https://dream-syz.github.io/9-1.png)



### 6、如何保证消息的顺序性

先看看顺序会错乱的场景：RabbitMQ：一个 queue，多个 consumer，这不明显乱了；

![](https://dream-syz.github.io/9-2.png)

解决：





### 7、 如何解决消息队列的延时以及过期失效问题？消息队列满了以后该怎么处理？有几百万消息持续积压几小时，说说怎么解决？

消息积压处理办法：临时紧急扩容：

先修复 consumer 的问题，确保其恢复消费速度，然后将现有 cnosumer 都停掉。 新建一个 topic，partition 是原来的 10 倍，临时建立好原先 10 倍的 queue 数量。 然后写一个临时的分发数据的 consumer 程序，这个程序部署上去消费积压的数据，消费之后不做耗时的处理，直接均匀轮询写入临时建立好的 10 倍数量的 queue。 接着临时征用 10 倍的机器来部署 consumer，每一批 consumer 消费一个临时 queue 的数据。这种做法相当于是临时将 queue 资源和 consumer 资源扩大 10 倍，以正常的 10 倍速度来消费数据。 等快速消费完积压数据之后，得恢复原先部署的架构，重新用原先的 consumer 机器来消费消息。 

MQ中消息失效：假设你用的是 RabbitMQ，RabbtiMQ 是可以设置过期时间的，也就是 TTL。如果消息在 queue 中积压超过一定的时间就会被 RabbitMQ 给清理掉，这个数据就没了。那这就是第二个坑了。这就不是说数据会大量积压在 mq 里，而是大量的数据会直接搞丢。我们可以采取一个方案，就是**批量重导**，这个我们之前线上也有类似的场景干过。就是大量积压的时候，我们当时就直接丢弃数据了，然后等过了高峰期以后，比如大家一起喝咖啡熬夜到晚上 12 点以后，用户都睡觉了。这个时候我们就开始写程序，将丢失的那批数据，写个临时程序，一点一点的查出来，然后重新灌入 mq 里面去，把白天丢的数据给他补回来。也只能是这样了。假设 1 万个订单积压在 mq 里面，没有处理，其中 1000 个订单都丢了，你只能手动写程序把那 1000 个订单给查出来，手动发到 mq 里去再补一次。

mq 消息队列快满了：如果消息积压在 mq 里，你很长时间都没有处理掉，此时导致 mq 都快写满了，咋办？这个还有别的办法吗？没有，谁让你第一个方案执行的太慢了，你临时写程序，接入数据来消费，消费一个丢弃一个，都不要了，快速消费掉所有的消息。然后走第二个方案，到了晚上再补数据吧。



### 8、让你来设计一个消息队列，你会怎么设计

比如说这个消息队列系统，我们从以下几个角度来考虑一下：

首先这个 mq 得支持可伸缩性吧，就是需要的时候快速扩容，就可以增加吞吐量和容量，那怎么搞？设计个分布式的系统呗，参照一下 kafka 的设计理念，broker -> topic -> partition，每个 partition 放一个机器，就存一部分数据。如果现在资源不够了，简单啊，给 topic 增加 partition，然后做数据迁移，增加机器，不就可以存放更多数据，提供更高的吞吐量了？

其次你得考虑一下这个 mq 的数据要不要落地磁盘吧？那肯定要了，落磁盘才能保证别进程挂了数据就丢了。那落磁盘的时候怎么落啊？顺序写，这样就没有磁盘随机读写的寻址开销，磁盘顺序读写的性能是很高的，这就是 kafka 的思路。

再次你考虑一下你的 mq 的可用性啊？这个事儿，具体参考之前可用性那个环节讲解的 kafka 的高可用保障机制。多副本 -> leader & follower -> broker 挂了重新选举 leader 即可对外服务。

能不能支持数据 0 丢失啊？可以的，参考我们之前说的那个 kafka 数据零丢失方案。（题 5）





## 数据结构与算法篇

在另外两本小册子里。



## Linux 篇

### 1、 绝对路径用什么符号表示？当前目录、上层目录用什么表示？主目录用什么表示? 切换目录用什么命令？

绝对路径： 如 /etc/init.d

当前目录和上层目录： ./ ../

主目录： ~/

切换目录： cd



### 2、 怎么查看当前进程？怎么执行退出？怎么查看当前路径？

查看当前进程： ps

> ps -l 列出与本次登录有关的进程信息； ps -aux 查询内存中进程信息； ps -aux | grep **查询 **进程的详细信息； top 查看内存中进程的动态信息； kill -9 pid 杀死进程。

执行退出： exit

查看当前路径： pwd



### 3、查看文件有哪些命令

```
vi 文件名 #编辑方式查看，可修改
cat 文件名 #显示全部文件内容
more 文件名 #分页显示文件内容
less 文件名 #与 more 相似，更好的是可以往前翻页
tail 文件名 #仅查看尾部，还可以指定行数
head 文件名 #仅查看头部,还可以指定行数
```



### 4、列举几个常用的 Linux 命令

- 列出文件列表：ls【参数 -a -l】

- 创建目录和移除目录：mkdir rmdir

- 用于显示文件后几行内容：tail，例如： tail -n 1000：显示最后1000行

- 打包：tar -xvf

- 打包并压缩：tar -zcvf

- 查找字符串：grep

- 显示当前所在目录：pwd创建空文件：touch

- 编辑器：vim vi



### 5、你平时是怎么查看日志的？

Linux 查看日志的命令有多种: tail、cat、tac、head、echo 等，本文只介绍几种常用的方法。

#### **1、tail**

最常用的一种查看方式

**命令格式**: tail [ 必要参数 ] [ 选择参数] [ 文件 ] 

-f 循环读取 -q 不显示处理信息 -v 显示详细的处理信息 -c<数目> 显示的字节数 -n<行数> 显示行数 -q, --quiet, --silent 从不输出给出文件名的首部 -s, --sleep-interval=S 与 -f 合用,表示在每次反复的间隔休眠S秒

例如：

```
tail -n 10 test.log 查询日志尾部最后 10 行的日志;
tail -n +10 test.log 查询 10 行之后的所有日志;
tail -fn 10 test.log 循环实时查看最后 1000 行记录(最常用的)
```

一般还会配合着 grep 搜索用，例如 :

```
tail -fn 1000 test.log | grep '关键字'
```

如果一次性查询的数据量太大，可以进行翻页查看，例如:

```
tail -n 4700 aa.log |more -1000 可以进行多屏显示(ctrl + f 或者 空格键可以快捷键)
```

#### **2、head**

跟 tail 是相反的 head 是看前多少行日志

```
head -n 10 test.log 查询日志文件中的头 10 行日志;
head -n -10 test.log 查询日志文件除了最后 10 行的其他所有日志;
```

head 其他参数参考 tail

#### 3、cat

cat 是由第一行到最后一行连续显示在屏幕上

一次显示整个文件 ：

```
$ cat filename
```

从键盘创建一个文件 :

```
$ cat > filename
```

将几个文件合并为一个文件：

```
$ cat file1 file2 > file 只能创建新文件,不能编辑已有文件
```

将一个日志文件的内容追加到另外一个 ：

```
$ cat -n textfile1 > textfile2
```

清空一个日志文件:

```
$ cat : >textfile2
```

注意：> 意思是创建，>> 是追加。千万不要弄混了。

cat 其他参数参考 tail

#### 4、more

more 命令是一个基于 vi 编辑器文本过滤器，它以全屏幕的方式按页显示文本文件的内容，支持 vi 中的关键字定位操作。more 名单中内置了若干快捷键，常用的有 H（获得帮助信息），Enter（向下翻滚一行），空格（向下滚动一屏），Q（退出命令）。more 命令从前向后读取文件，因此在启动时就加载整个文件。

该命令一次显示一屏文本，满屏后停下来，并且在屏幕的底部出现一个提示信息，给出至今己显示的该文件的百分比：–More–（XX%）

- more 的语法：more 文件名

- Enter 向下 n 行，需要定义，默认为 1 行

- Ctrl f 向下滚动一屏

- 空格键 向下滚动一屏

- Ctrl b 返回上一屏

- = 输出当前行的行号

- :f 输出文件名和当前行的行号

- v 调用 vi 编辑器

- ! 命令 调用 Shell，并执行命令

- q 退出 more

#### 5、sed

这个命令可以查找日志文件特定的一段 , 根据时间的一个范围查询，可以按照行号和时间范围查询

按照行号

```
sed -n '5,10p' filename 这样你就可以只查看文件的第 5 行到第 10 行。
```

**按照时间段**

```cmake
sed -n '/2014-12-17 16:17:20/,/2014-12-17 16:17:36/p' test.log
```

#### 6、less

less 命令在查询日志时，一般流程是这样的

```
less log.log
shift + G 命令到文件尾部 然后输入 ？加上你要搜索的关键字例如 ？1213
按 n 向上查找关键字
shift + n 反向查找关键字
less 与 more 类似，使用 less 可以随意浏览文件，而 more 仅能向前移动，不能向后移动，而且 less 在查看之前不会加载整个文件。
less log2013.log 查看文件
ps -ef | less ps 查看进程信息并通过 less 分页显示
history | less 查看命令历史使用记录并通过 less 分页显示
less log2013.log log2014.log 浏览多个文件
```

常用命令参数：

```
-b <缓冲区大小> 设置缓冲区的大小
-g 只标志最后搜索的关键词
-i 忽略搜索时的大小写
-m 显示类似 more 命令的百分比
-N 显示每行的行号
-o <文件名> 将 less 输出的内容在指定文件中保存起来
-Q 不使用警告音
-s 显示连续空行为一行
/ 字符串：向下搜索"字符串"的功能
? 字符串：向上搜索"字符串"的功能
n：重复前一个搜索（与 / 或 ? 有关）
N：反向重复前一个搜索（与 / 或 ? 有关）
b 向后翻一页
h 显示帮助界面
q 退出 less 命令
```

**一般查日志配合应用的其他命令**

```
history // 所有的历史记录
history | grep XXX // 历史记录中包含某些指令的记录
history | more // 分页查看记录
history -c // 清空所有的历史记录
!! 重复执行上一个命令
查询出来记录后选中 : !323
```





## Zookeeper 篇

### 1、说说 Zookeeper 是什么？

**直译：**从名字上直译就是动物管理员，动物指的是 Hadoop 一类的分布式软件，管理员三个字体现了 **ZooKeeper 的特点：维护、协调、管理、监控**。

**简述：**有些软件你想做成集群或者分布式，你可以用 ZooKeeper 帮你来辅助实现。

**特点：**

- 最终一致性：客户端看到的数据最终是一致的。

- 可靠性：服务器保存了消息，那么它就一直都存在。

- 实时性：ZooKeeper 不能保证两个客户端同时得到刚更新的数据。

- 独立性（等待无关）：不同客户端直接互不影响。

- 原子性：更新要不成功要不失败，没有第三个状态。

**注意**：回答面试题，切忌只是简单一句话回答，可以将你对概念的理解，特点等多个方面描述一下，哪怕你自己认为不完全切中题意的也可以说说，面试官不喜欢会打断你的，你的目的是让面试官认为你是好沟通的。当然了，如果不会可别装作会，说太多不专业的想法。



### 2、ZooKeeper 有哪些应用场景？

#### **数据发布与订阅**

发布与订阅即所谓的配置管理，顾名思义就是将数据发布到 ZooKeeper 节点上，供订阅者动态获取数据，实现配置信息的集中式管理和动态更新。例如全局的配置信息，地址列表等就非常适合使用。数据发布 / 订阅的一个常见的场景是配置中心，发布者把数据发布到 ZooKeeper 的一个或一系列的节点上，供订阅者进行数据订阅，达到动态获取数据的目的。

配置信息一般有几个特点:

1. 数据量小的 KV
2. 数据内容在运行时会发生动态变化
3. 集群机器共享，配置一致

![](https://dream-syz.github.io/10-1.png)

ZooKeeper 采用的是推拉结合的方式。

1. 推: 服务端会推给注册了监控节点的客户端 Wathcer 事件通知
2. 拉: 客户端获得通知后，然后主动到服务端拉取最新的数据

#### **命名服务**

作为分布式命名服务，命名服务是指通过指定的名字来获取资源或者服务的地址，利用 ZooKeeper 创建一个全局的路径，这个路径就可以作为一个名字，指向集群中的集群，提供的服务的地址，或者一个远程的对象等等。

统一命名服务的命名结构图如下所示：

![](https://dream-syz.github.io/10-2.png)

1、在分布式环境下，经常需要对应用/服务进行统一命名，便于识别不同服务。类似于域名与 IP 之间对应关系，IP 不容易记住，而域名容易记住。通过名称来获取资源或服务的地址，提供者等信息。

2、按照层次结构组织服务 / 应用名称。可将服务名称以及地址信息写到 ZooKeeper 上，客户端通过 ZooKeeper 获取可用服务列表类。

#### **配置管理**

程序分布式的部署在不同的机器上，将程序的配置信息放在 ZooKeeper 的 znode 下，当有配置发生改变时，也就是 znode 发生变化时，可以通过改变 zk 中某个目录节点的内容，利用 watch 通知给各个客户端，从而更改配置。

ZooKeeper 配置管理结构图如下所示：

<img src="https://dream-syz.github.io/10-3.png" style="zoom:80%;" />

1、分布式环境下，配置文件管理和同步是一个常见问题。

- 一个集群中，所有节点的配置信息是一致的，比如 Hadoop 集群。
- 对配置文件修改后，希望能够快速同步到各个节点上。

2、配置管理可交由 ZooKeeper 实现。

- 可将配置信息写入 ZooKeeper 上的一个 Znode。

- 各个节点监听这个 Znode。

- 一旦 Znode 中的数据被修改，ZooKeeper 将通知各个节点。

#### **集群管理**

所谓集群管理就是：是否有机器退出和加入、选举 master。

**集群管理主要指集群监控和集群控制两个方面。前者侧重于集群运行时的状态的收集，后者则是对集群进行操作与控制。**开发和运维中，面对集群，经常有如下需求:

1. 希望知道集群中究竟有多少机器在工作
2. 对集群中的每台机器的运行时状态进行数据收集
3. 对集群中机器进行上下线的操作

集群管理结构图如下所示：

<img src="https://dream-syz.github.io/10-4.png" style="zoom:80%;" />

1、分布式环境中，实时掌握每个节点的状态是必要的，可根据节点实时状态做出一些调整。

2、可交由 ZooKeeper 实现。

- 可将节点信息写入 ZooKeeper 上的一个 Znode。

- 监听这个 Znode 可获取它的实时状态变化。

3、典型应用 

- Hbase 中 Master 状态监控与选举。

利用 ZooKeeper 的强一致性，能够保证在分布式高并发情况下节点创建的全局唯一性，即：同时有多个客户端请求创建 /currentMaster 节点，最终一定只有一个客户端请求能够创建成功

#### **分布式通知与协调**

1、分布式环境中，经常存在一个服务需要知道它所管理的子服务的状态。

a）NameNode 需知道各个 Datanode 的状态。

b）JobTracker 需知道各个 TaskTracker 的状态。

2、心跳检测机制可通过 ZooKeeper 来实现。

3、信息推送可由 ZooKeeper 来实现，ZooKeeper 相当于一个发布/订阅系统。

#### **分布式锁**

处于不同节点上不同的服务，它们可能需要顺序的访问一些资源，这里需要一把分布式的锁。

分布式锁具有以下特性：写锁、读锁、时序锁。

**写锁**：在 zk 上创建的一个临时的无编号的节点。由于是无序编号，在创建时不会自动编号，导致只能客户端有一个客户端得到锁，然后进行写入。

**读锁**：在 zk 上创建一个临时的有编号的节点，这样即使下次有客户端加入是同时创建相同的节点时，他也会自动编号，也可以获得锁对象，然后对其进行读取。

**时序锁**：在 zk 上创建的一个临时的有编号的节点根据编号的大小控制锁。

#### **分布式队列**

##### 分布式队列分为两种：

1、当一个队列的成员都聚齐时，这个队列才可用，否则一直等待所有成员到达，这种是同步队列。

a）一个 job 由多个 task 组成，只有所有任务完成后，job 才运行完成。

b）可为 job 创建一个 /job 目录，然后在该目录下，为每个完成的 task 创建一个临时的 Znode，一旦临时节点数目达到 task 总数，则表明 job 运行完成。

2、队列按照 FIFO 方式进行入队和出队操作，例如实现生产者和消费者模型。



### 3、说说 Zookeeper 的工作原理？

Zookeeper 的 **核心是原子广播**，这个机制保证了各个 Server 之间的同步。实现这个机制的协议叫做 Zab 协议。Zab 协议有两种模式，它们分别是恢复模式（选主）和广播模式（同步）。

Zab 协议 的全称是 Zookeeper Atomic Broadcast（Zookeeper 原子广播）。Zookeeper 是通过Zab 协议来保证分布式事务的最终一致性。Zab协议要求每个 Leader 都要经历三个阶段：**发现，同步，广播**。

当服务启动或者在领导者崩溃后，Zab 就进入了恢复模式，当领导者被选举出来，且大多数 Server 完成了和 leader 的状态同步以后，恢复模式就结束了。状态同步保证了 leader 和 Server 具有相同的系统状态。

为了保证事务的顺序一致性，zookeeper 采用了递增的事务 id 号（zxid）来标识事务。所有的提议（proposal）都在被提出的时候加上了 zxid。实现中 zxid 是一个 64 位的数字，它高 32 位是 epoch 用来标识 leader 关系是否改变，每次一个 leader 被选出来，它都会有一 个新的 epoch，标识当前属于那个 leader 的统治时期。低 32 位用于递增计数。

epoch：可以理解为皇帝的年号，当新的皇帝 leader 产生后，将有一个新的 epoch 年号。

每个 Server 在工作过程中有四种状态：

- LOOKING：当前 Server 不知道 leader 是谁，正在搜寻。

- LEADING：当前 Server 即为选举出来的 leader。

- FOLLOWING：leader 已经选举出来，当前 Server 与之同步
- OBSERVING：观察者状态



### 4、请描述一下 Zookeeper 的通知机制是什么？

Zookeeper 允许客户端向服务端的某个 znode 注册一个 Watcher 监听，当服务端的一些指定事件触发了这个 Watcher ，服务端会向指定客户端发送一个事件通知来实现分布式的通知功能，然后客户端根据 Watcher 通知状态和事件类型做出业务上的改变。

大致分为三个步骤：

- **客户端注册 Watcher**

1、调用 getData、getChildren、exist 三个 API ，传入Watcher 对象。

2、标记请求 request ，封装 Watcher 到 WatchRegistration 。

3、封装成 Packet 对象，发服务端发送 request 。

4、收到服务端响应后，将 Watcher 注册到 ZKWatcherManager 中进行管理。

5、请求返回，完成注册。

- **服务端处理 Watcher**

1、服务端接收 Watcher 并存储。 

2、Watcher 触发 

3、调用 process 方法来触发 Watcher 。

- **客户端回调 Watcher**

1，客户端 SendThread 线程接收事件通知，交由 EventThread 线程回调Watcher 。 

2，客户端的 Watcher 机制同样是一次性的，一旦被触发后，该 Watcher 就失效了。

> client 端会对某个 znode 建立一个 watcher 事件，当该 znode 发生变化时，这些 client 会收到 zk 的通知，然后 client 可以根据 znode 变化来做出业务上的改变等。
>



### 5、Zookeeper 对节点的 **watch** **监听通知是永久的吗？**

不是，是**一次性 **的。无论是服务端还是客户端，一旦一个 Watcher 被触发， Zookeeper 都会将其从相应的存储中移除。这样的设计有效的减轻了服务端的压力，不然对于更新非常频繁的节点，服务端会不断的向客户端发送事件通知，无论对于网络还是服务端的压力都非常大。



### 6、Zookeeper 集群中有哪些角色？

![](https://dream-syz.github.io/10-5.png)

在一个集群中，最少需要 3 台。或者保证 2N + 1 台，即奇数。为什么保证奇数？主要是为了选举算法。



### 7、Zookeeper 集群中 Server 有哪些工作状态？

**LOOKING**

寻找 Leader 状态；当服务器处于该状态时，它会认为当前集群中没有 Leader ，因此需要进入 Leader 选举状态

**FOLLOWING**

跟随者状态；表明当前服务器角色是 Follower

**LEADING**

领导者状态；表明当前服务器角色是 Leader

**OBSERVING**

观察者状态；表明当前服务器角色是 Observer



### 8、Zookeeper 集群中是怎样选举 leader 的？

当 Leader 崩溃了，或者失去了大多数的 Follower，这时候 Zookeeper 就进入 **恢复模式**，恢复模式需要重新选举出一个新的 Leader，让所有的 Server 都恢复到一个状态 **LOOKING** 。

Zookeeper 有两种选举算法：基于 basic paxos 实现和基于 fast paxos 实现。默认为 fast paxos

由于篇幅问题，这里推荐：选举流程



### 9、 Zookeeper 是如何保证事务的顺序一致性的呢？

Zookeeper 采用了递增的事务 id 来识别，所有的 proposal （提议）都在被提出的时候加上了 zxid 。 zxid 实际上是一个 64 位数字。

高 32 位是 epoch 用来标识 Leader 是否发生了改变，如果有新的 Leader 产生出来， epoch 会自增。 低 32 位用来递增计数。 当新产生的 proposal 的时候，会依据数据库的两阶段过程，首先会向其他的 Server 发出事务执行请求，如果超过半数的机器都能执行并且能够成功，那么就会开始执行。



### 10、ZooKeepe **集群中个服务器之间是怎样通信的？**

Leader 服务器会和每一个 Follower / Observer 服务器都建立 TCP 连接，同时为每个 Follower / Observer 都创建一个叫做 LearnerHandler 的实体。

- LearnerHandler 主要负责 Leader 和 Follower / Observer 之间的网络通讯，包括数据同步，请求转发和 proposal 提议的投票等。

- Leader 服务器保存了所有 Follower / Observer 的 LearnerHandler 。



### **11、 ZooKeeper** 分布式锁怎么实现的？

如果有客户端 1、客户端 2 等 N 个客户端争抢一个 Zookeeper 分布式锁。大致如下：

1. 大家都是上来直接创建一个锁节点下的一个接一个的临时有序节点
2. 如果自己不是第一个节点，就对自己上一个节点加监听器
3. 只要上一个节点释放锁，自己就排到前面去了，相当于是一个排队机制。

而且用临时顺序节点的另外一个用意就是，如果某个客户端创建临时顺序节点之后，不小心自己宕机了也没关系， Zookeeper 感知到那个客户端宕机，会自动删除对应的临时顺序节点，相当于自动释放锁，或者是自动取消自己的排队。

本地锁，可以用 JDK 实现，但是分布式锁就必须要用到分布式的组件。比如 ZooKeeper、Redis。

网上代码一大段，面试一般也不要写，我这说一些关键点。

几个需要注意的地方如下。

- 死锁问题：锁不能因为意外就变成死锁，所以要用 ZK 的临时节点，客户端连接失效了，锁就自动释放了。

- 锁等待问题：锁有排队的需求，所以要 ZK 的顺序节点。

- 锁管理问题：一个使用释放了锁，需要通知其他使用者，所以需要用到监听。

- 监听的羊群效应：比如有 1000 个锁竞争者，锁释放了，1000 个竞争者就得到了通知，然后判断，最终序号最小的那个拿到了锁。其它 999 个竞争者重新注册监听。这就是羊群效应，出点事，就会惊动整个羊群。应该每个竞争者只监听自己前面的那个节点。比如 2 号释放了锁，那么只有 3 号得到了通知。



### 12、了解 Zookeeper 的系统架构吗？

![](https://dream-syz.github.io/10-6.png)

ZooKeeper 的架构图中我们需要了解和掌握的主要有：

（1）ZooKeeper分为服务器端（Server） 和客户端（Client），客户端可以连接到整个 ZooKeeper 服务的任意服务器上（除非 leaderServes 参数被显式设置， leader 不允许接受客户端连接）。

（2）客户端使用并维护一个 TCP 连接，通过这个连接发送请求、接受响应、获取观察的事件以及发送心跳。如果这个 TCP 连接中断，客户端将自动尝试连接到另外的 ZooKeeper 服务器。客户端第一次连接到 ZooKeeper 服务时，接受这个连接的 ZooKeeper 服务器会为这个客户端建立一个会话。当这个客户端连接到另外的服务器时，这个会话会被新的服务器重新建立。

（3）上图中每一个 Server 代表一个安装 Zookeeper 服务的机器，即是整个提供 Zookeeper 服务的集群（或者是由伪集群组成）；

（4）组成 ZooKeeper 服务的服务器必须彼此了解。它们维护一个内存中的状态图像，以及持久存储中的事务日志和快照， 只要大多数服务器可用，ZooKeeper 服务就可用；

（5）ZooKeeper 启动时，将从实例中选举一个 leader，Leader 负责处理数据更新等操作，一个更新操作成功的标志是当且仅当大多数Server 在内存中成功修改数据。每个 Server 在内存中存储了一份数据。

（6）Zookeeper 是可以集群复制的，集群间通过 Zab 协议（Zookeeper Atomic Broadcast）来保持数据的一致性；

（7）Zab协议包含两个阶段：leader election 阶段和 Atomic Brodcast 阶段。

- a) 集群中将选举出一个 leader，其他的机器则称为 follower，所有的写操作都被传送给 leader，并通过 brodcast 将所有的更新告诉给follower。

- b) 当 leader 崩溃或者 leader 失去大多数的 follower 时，需要重新选举出一个新的 leader，让所有的服务器都恢复到一个正确的状态。

- c) 当 leader 被选举出来，且大多数服务器完成了 和 leader 的状态同步后，leadder election 的过程就结束了，就将会进入到 Atomic brodcast 的过程。

- d) Atomic Brodcast 同步 leader 和 follower 之间的信息，保证 leader 和 follower 具有形同的系统状态。



### 13、Zookeeper 为什么要这么设计？

ZooKeeper 设计的目的是提供高性能、高可用、顺序一致性的分布式协调服务、保证数据最终一致性。

**高性能（简单的数据模型）**

1. 采用树形结构组织数据节点；
2. 全量数据节点，都存储在内存中；
3. Follower 和 Observer 直接处理非事务请求；

**高可用（构建集群）**

1. 半数以上机器存活，服务就能正常运行
2. 自动进行 Leader 选举

**顺序一致性（事务操作的顺序）**

1. 每个事务请求，都会转发给 Leader 处理
2. 每个事务，会分配全局唯一的递增 id（zxid，64位：epoch + 自增 id）

**最终一致性**

1. 通过提议投票方式，保证事务提交的可靠性
2. 提议投票方式，只能保证 Client 收到事务提交成功后，半数以上节点能够看到最新数据



### 14、你知道 Zookeeper 中有哪些角色？

#### 系统模型：

![](https://dream-syz.github.io/10-7.png)

**领导者（leader）**

Leader 服务器为客户端提供读服务和写服务。负责进行投票的发起和决议，更新系统状态。

**学习者（learner）**

- 跟随者（ follower ） Follower服务器为客户端提供读服务，参与Leader选举过程，参与写操作“过半写成功”策略。

- 观察者（ observer ） Observer服务器为客户端提供读服务，不参与Leader选举过程，不参与写操作“过半写成功”策略。用于在不影响写性能的前提下提升集群的读性能。

**客户端（client）** 

服务请求发起方。



### 15、你熟悉 Zookeeper 节点 ZNode 和相关属性吗？

**节点有哪些类型？**

Znode 两种类型 ：

- 持久的（persistent）：客户端和服务器端断开连接后，创建的节点不删除（默认）。

- 短暂的（ephemeral）：客户端和服务器端断开连接后，创建的节点自己删除。

Znode 有四种形式 ：

- 持久化目录节点（PERSISTENT）：客户端与 Zookeeper 断开连接后，该节点依旧存在持久化顺序编号目录节点（PERSISTENT_SEQUENTIAL）

- 客户端与 Zookeeper 断开连接后，该节点依旧存在，只是 Zookeeper 给该节点名称进行顺序编号：临时目录节点（EPHEMERAL）

- 客户端与 Zookeeper 断开连接后，该节点被删除：临时顺序编号目录节点（EPHEMERAL_SEQUENTIAL）

- 客户端与 Zookeeper 断开连接后，该节点被删除，只是 Zookeeper 给该节点名称进行顺序编号

**「注意」**：创建vZNodev时设置顺序标识，ZNodev名称后会附加一个值，顺序号是一个单调递增的计数器，由父节点维护。

**节点属性有哪些**

一个 znode 节点不仅可以存储数据，还有一些其他特别的属性。接下来我们创建一个 /test 节点分析一下它各个属性的含义。

```
 [zk: localhost:2181(CONNECTED) 6] get /test
 456
 cZxid = 0x59ac //
 ctime = Mon Mar 30 15:20:08 CST 2020
 mZxid = 0x59ad
 mtime = Mon Mar 30 15:22:25 CST 2020
 pZxid = 0x59ac
 cversion = 0
 dataVersion = 2
 aclVersion = 0
 ephemeralOwner = 0x0
 dataLength = 3
 numChildren = 0
```

属性说明：

| 节点属性         | 注解                                                         |
| ---------------- | ------------------------------------------------------------ |
| cZxid            | 该数据节点被创建时的事务 id                                  |
| mZxid            | 该数据节点被修改时最新的事务 id                              |
| pZxid            | 当前节点的父级节点事务 id                                    |
| cTime            | 该数据节点创建时间                                           |
| mTime            | 该数据节点最后修改时间                                       |
| dataVersion      | 当前节点版本号（每修改一次值 + 1 递增）                      |
| cVersion         | 子节点版本号（子节点修改次数，每修改一次值 + 1 递增）        |
| aclVersion       | 当前节点 acl 版本号（节点被修改 acl 权限，每修改一次值 + 1 递增） |
| ephemeral())wner | 临时节点标识，当前节点如果是临时节点，则存储的创建者的会话 id(sessionl，如果不是，那么值 = 0 |
| dataLength       | 当前节点所存储的数据长度                                     |
| numChildren      | 当前节点下子节点的个数                                       |



### 16、请简述 Zookeeper 的选主流程

Zookeeper 的核心是原子广播，这个机制保证了各个 Server 之间的同步。实现这个机制的协议叫做 Zab 协议。Zab 协议有两种模式，它们分别是恢复模式（选主）和广播模式（同步）。当服务启动或者在领导者崩溃后，Zab 就进入了恢复模式，当领导者被选举出来，且大多数 Serve r完成了和 leader 的状态同步以后，恢复模式就结束了。状态同步保证了 leader 和 Server 具有相同的系统状态。leader 选举是保证分布式数据一致性的关键。

出现选举主要是两种场景：初始化、leader 不可用。

当 zk 集群中的一台服务器出现以下两种情况之一时，就会开始 leader 选举。

（1）服务器初始化启动。

（2）服务器运行期间无法和 leader 保持连接。

而当一台机器进入 leader 选举流程时，当前集群也可能处于以下两种状态。

（1）集群中本来就已经存在一个 leader。

（2）集群中确实不存在 leader。

首先第一种情况，通常是集群中某一台机器启动比较晚，在它启动之前，集群已经正常工作，即已经存在一台 leader 服务器。当该机器试图去选举 leader 时，会被告知当前服务器的 leader 信息，它仅仅需要和 leader 机器建立连接，并进行状态同步即可。

重点是 leader 不可用了，此时的选主制度。

投票信息中包含两个最基本的信息。

sid ：即 server id，用来标识该机器在集群中的机器序号。

zxid ：即 zookeeper 事务 id 号。

ZooKeeper 状态的每一次改变, 都对应着一个递增的 Transaction id,，该 id 称为 zxid.，由于 zxid 的递增性质, 如果 zxid 1 小于 zxid 2，那么 zxid 1肯定先于 zxid 2 发生。创建任意节点，或者更新任意节点的数据， 或者删除任意节点，都会导致 Zookeeper 状态发生改变，从而导致 zxid 的值增加。

以（sid，zxid）的形式来标识一次投票信息。

例如：如果当前服务器要推举 sid 为 1，zxid 为 8 的服务器成为 leader，那么投票信息可以表示为（1，8）集群中的每台机器发出自己的投票后，也会接受来自集群中其他机器的投票。每台机器都会根据一定的规则，来处理收到的其他机器的投票，以此来决定是否需要变更自己的投票。

规则如下 ：

（1）初始阶段，都会给自己投票。

（2）当接收到来自其他服务器的投票时，都需要将别人的投票和自己的投票进行pk，规则如下：

优先检查 zxid。zxid 比较大的服务器优先作为 leader。如果 zxid 相同的话，就比较 sid，sid 比较大的服务器作为 leader。

**所有服务启动时候的选举流程**：

三台服务器 server 1、server 2、server 3：

1. server 1 启动，一台机器不会选举。
2. server 2 启动，server 1 和 server 2 的状态改为 looking，广播投票
3. server 3 启动，状态改为 looking，加入广播投票。
4. 初识状态，互不认识，大家都认为自己是王者，投票也投自己为 Leader。
5. 投票信息说明，票信息本来为五元组，这里为了逻辑清晰，简化下表达。

初识 zxid = 0，sid 是每个节点的名字，这个 sid 在 zoo.cfg 中配置，不会重复。

| **节点** | **sid** |
| -------- | ------- |
| server 1 | 1       |
| server 2 | 2       |
| server 3 | 3       |

1. 初始 zxid = 0，server 1 投票（1，0），server 2 投票（2，0），server 3 投票（3，0）
2. server 1 收到 投票（2，0）时，会先验证投票的合法性，然后自己的票进行 pk，pk 的逻辑是先比较 zxid，server 1（zxid）=server 2（zxid）=0，zxid 相等再比较 sid，server 1（sid）< server 2(sid)，pk 结果为 server 2 的投票获胜。server 1 更新自己的投票为 （2，0），server 1 重新投票。

3. TODO 这里最终是 2 还是 3，需要做实验确定。
4. server 2 收到 server 1 投票，会先验证投票的合法性，然后 pk，自己的票获胜，server 不用更新自己的票，pk 后，重新在发送一次投票。

5. 统计投票，pk 后会统计投票，如果半数以上的节点投出相同的票，确定选出了 Leader。
6. 选举结束，被选中节点的状态由 LOOKING 变成 LEADING，其他参加选举的节点由 LOOKING 变成 FOLLOWING。如果有 Observer 节点，如果 Observer 不参与选举，所以选举前后它的状态一直是 OBSERVING，没有变化。

**简单地说**

开始投票 -> 节点状态变成 LOOKING  ->  每个节点选自己 ->  收到票进行 PK -> sid 大的获胜 -> 更新选票 -> 再次投票 -> 统计选票，选票过半数选举结果 -> 节点状态更新为自己的角色状态。



### 17、为什么 Zookeeper 集群的数目，一般为奇数个？

首先需要明确 zookeeper 选举的规则：leader 选举，要求 可用节点数量 > 总节点数量 / 2 。

比如：标记一个写是否成功是要在超过一半节点发送写请求成功时才认为有效。同样，Zookeeper 选择领导者节点也是在超过一半节点同意时才有效。最后，Zookeeper 是否正常是要根据是否超过一半的节点正常才算正常。这是基于 CAP 的一致性原理。

zookeeper 有这样一个特性：集群中只要有过半的机器是正常工作的，那么整个集群对外就是可用的。

也就是说如果有 2 个 zookeeper，那么只要有 1 个死了 zookeeper 就不能用了，因为 1 没有过半，所以 2 个 zookeeper 的死亡容忍度为 0；

同理，要是有 3 个 zookeeper，一个死了，还剩下 2 个正常的，过半了，所以 3 个 zookeeper 的容忍度为 1；

同理：

- 2->0；两个 zookeeper，最多 0 个 zookeeper 可以不可用。

- 3->1；三个 zookeeper，最多 1 个 zookeeper 可以不可用。

- 4->1；四个 zookeeper，最多 1 个 zookeeper 可以不可用。

- 5->2；五个 zookeeper，最多 2 个 zookeeper 可以不可用。

- 6->2；两个 zookeeper，最多 0 个 zookeeper 可以不可用。

....

会发现一个规律，2n 和 2n-1 的容忍度是一样的，都是 n-1，所以为了更加高效，何必增加那一个不必要的 zookeeper 呢。

zookeeper 的选举策略也是需要半数以上的节点同意才能当选 leader，如果是偶数节点可能导致票数相同的情况。



### 18、知道 Zookeeper 监听器的原理吗？

![](https://dream-syz.github.io/10-8.png)

1. 创建一个 Main()  线程。
2. 在 Main()  线程中创建两个线程，一个负责网络连接通信（connect），一个负责监听（listener）。
3.  通过 connect 线程将注册的监听事件发送给 Zookeeper。

4. 将注册的监听事件添加到 Zookeeper 的注册监听器列表中。
5. Zookeeper 监听到有数据或路径发生变化时，把这条消息发送给 Listener 线程。
6. Listener 线程内部调用 process() 方法



### 19、说说 Zookeeper 中的 ACL 权限控制机制

UGO（User/Group/Others）

目前在 Linux / Unix 文件系统中使用，也是使用最广泛的权限控制方式。是一种粗粒度的文件系统权限控制模式。

ACL（Access Control List）访问控制列表

包括三个方面：

权限模式（Scheme）

（1）IP：从 IP 地址粒度进行权限控制

（2）Digest：最常用，用类似于 username:password 的权限标识来进行权限配置，便于区分不同应用来进行权限控制

（3）World：最开放的权限控制方式，是一种特殊的 digest 模式，只有一个权限标识  “world:anyone”

（4）Super：超级用户

授权对象

授权对象指的是权限赋予的用户或一个指定实体，例如 IP 地址或是机器灯。

权限 Permission

（1）CREATE：数据节点创建权限，允许授权对象在该 Znode 下创建子节点

（2）DELETE：子节点删除权限，允许授权对象删除该数据节点的子节点

（3）READ：数据节点的读取权限，允许授权对象访问该数据节点并读取其数据内容或子节点列等

（4）WRITE：数据节点更新权限，允许授权对象对该数据节点进行更新操作

（5）ADMIN：数据节点管理权限，允许授权对象对该数据节点进行 ACL 相关设置操作

​	

### 20、Zookeeper 有哪几种几种部署模式？

Zookeeper 有三种部署模式：

1. 单机部署：一台集群上运行；
2. 集群部署：多台集群运行；
3. 伪集群部署：一台集群启动多个 Zookeeper 实例运行。



### 21、Zookeeper 集群支持动态添加机器吗？

其实就是水平扩容了，Zookeeper 在这方面不太好。两种方式：

全部重启：关闭所有 Zookeeper 服务，修改配置之后启动。不影响之前客户端的会话。

逐个重启：在过半存活即可用的原则下，一台机器重启不影响整个集群对外提供服务。这是比较常用的方式。

3.5 版本开始支持动态扩容。



### 22、描述一下 ZAB 协议

ZAB 协议是 ZooKeeper 自己定义的协议，全名 ZooKeeper 原子广播协议。

**ZAB** **协议有两种模式：**Leader 节点崩溃了如何恢复和消息如何广播到所有节点。

整个 ZooKeeper 集群没有 Leader 节点的时候，属于崩溃的情况。比如集群启动刚刚启动，这时节点们互相不认识。比如运作 Leader 节点宕机了，又或者网络问题，其他节点 Ping 不通 Leader 节点了。这时就需要 ZAB 中的节点崩溃协议，所有节点进入选举模式，选举出新的 Leader。整个选举过程就是通过广播来实现的。选举成功后，一切都需要以 Leader 的数据为准，那么就需要进行数据同步了。



### 23、ZAB 和 Paxos 算法的联系与区别？

相同点：

（1）两者都存在一个类似于 Leader 进程的角色，由其负责协调多个 Follower 进程的运行

（2）Leader 进程都会等待超过半数的 Follower 做出正确的反馈后，才会将一个提案进行提交

（3）ZAB 协议中，每个 Proposal 中都包含一个 epoch 值来代表当前的 Leader周期，Paxos 中名字为 Ballot

不同点：

ZAB 用来构建高可用的分布式数据主备系统（Zookeeper），Paxos 是用来构建分布式一致性状态机系统。



### 24、ZooKeeper 宕机如何处理？

ZooKeeper 本身也是集群，推荐配置奇数个服务器。因为宕机就需要选举，选举需要半数 +1 票才能通过，为了避免打成平手。进来不用偶数个服务器。如果是 Follower 宕机了，没关系不影响任何使用。用户无感知。如果 Leader 宕机，集群就得停止对外服务，开始选举，选举出一个 Leader 节点后，进行数据同步，保证所有节点数据和 Leader 统一，然后开始对外提供服务。为啥投票需要半数 +1，如果半数就可以的话，网络的问题可能导致集群选举出来两个 Leader，各有一半的小弟支持，这样数据也就乱套了。



### 25、 描述一下 ZooKeeper 的 session 管理的思想？

**分桶策略：**

简单地说，就是不同的会话过期可能都有时间间隔，比如 15 秒过期、15.1 秒过期、15.8 秒过期，ZooKeeper 统一让这些 session 16 秒过期。这样非常方便管理，看下面的公式，过期时间总是 ExpirationInterval 的整数倍。

计算公式：

```
ExpirationTime = currentTime + sessionTimeout
ExpirationTime = (ExpirationTime / ExpirationInrerval + 1) * ExpirationInterval ,
```

见图片：

![](https://dream-syz.github.io/10-9.png)

默认配置的 session 超时时间是在 2 tickTime ~ 20  tickTime。



### 26、ZooKeeper 负载均衡和 Nginx 负载均衡有什么区别？

**ZooKeeper：**

- 不存在单点问题，zab 机制保证单点故障可重新选举一个 Leader

- 只负责服务的注册与发现，不负责转发，减少一次数据交换（消费方与服务方直接通信）

- 需要自己实现相应的负载均衡算法

**Nginx：**

- 存在单点问题，单点负载高数据量大，需要通过 KeepAlived 辅助实现高可用

- 每次负载，都充当一次中间人转发角色，本身是个反向代理服务器

- 自带负载均衡算法



### 27、说说 ZooKeeper 的序列化

序列化：

- 内存数据，保存到硬盘需要序列化。

- 内存数据，通过网络传输到其他节点，需要序列化。

ZK 使用的序列化协议是 Jute，Jute 提供了 Record 接口。接口提供了两个方法：

- serialize 序列化方法

- deserialize 反序列化方法

要系列化的方法，在这两个方法中存入到流对象中即可。



### 28、在 Zookeeper 中 Zxid 是什么，有什么作用？

Zxid，也就是事务 id，为了保证事务的顺序一致性，ZooKeeper 采用了递增的事务 Zxid 来标识事务。proposal 都会加上了 Zxid。Zxid 是一个 64 位的数字，它高 32 位是 Epoch 用来标识朝代变化，比如每次选举 Epoch 都会加改变。低 32 位用于递增计数。

Epoch：可以理解为当前集群所处的年代或者周期，每个 Leader 就像皇帝，都有自己的年号，所以每次改朝换代，Leader 变更之后，都会在前一个年代的基础上加 1。这样就算旧的 Leader 崩溃恢复之后，也没有人听它的了，因为 Follower 只听从当前年代的 Leader 的命令。



### 29、讲解一下 ZooKeeper 的持久化机制

**什么是持久化？**

- 数据，存到磁盘或者文件当中。

- 机器重启后，数据不会丢失。内存 -> 磁盘的映射，和序列化有些像。

**ZooKeeper** **的持久化：**

- SnapShot 快照，记录内存中的全量数据

- TxnLog 增量事务日志，记录每一条增删改记录（查不是事务日志，不会引起数据变化）

**为什么持久化这么麻烦，一个不可用吗？**

快照的缺点，文件太大，而且快照文件不会是最新的数据。 增量事务日志的缺点，运行时间长了，

日志太多了，加载太慢。二者结合最好。

**快照模式：**

- 将 ZooKeeper 内存中以 DataTree 数据结构存储的数据定期存储到磁盘中。

- 由于快照文件是定期对数据的全量备份，所以快照文件中数据通常不是最新的。

见图片：

![](https://dream-syz.github.io/10-10.png)



### 30、Zookeeper 选举中投票信息的五元组是什么？

- Leader：被选举的 Leader 的 SID

- Zxid：被选举的 Leader 的事务 ID

- Sid：当前服务器的 SID

- electionEpoch：当前投票的轮次

- peerEpoch：当前服务器的 Epoch

**Epoch > Zxid > Sid**

Epoch，Zxid 都可能一致，但是 Sid 一定不一样，这样两张选票一定会 PK 出结果。



### 31、说说 Zookeeper 中的脑裂？

简单点来说，脑裂（Split-Brain）就是比如当你的 cluster 里面有两个节点，它们都知道在这个 cluster 里需要选举出一个 master。那么当它们两个之间的通信完全没有问题的时候，就会达成共识，选出其中一个作为 master。但是如果它们之间的通信出了问题，那么两个结点都会觉得现在没有 master，所以每个都把自己选举成 master，于是 cluster 里面就会有两个 master。

对于 Zookeeper 来说有一个很重要的问题，就是到底是根据一个什么样的情况来判断一个节点死亡 down 掉了？在分布式系统中这些都是有监控者来判断的，但是监控者也很难判定其他的节点的状态，唯一一个可靠的途径就是心跳，Zookeeper也是使用心跳来判断客户端是否仍然活着。

使用 ZooKeeper 来做 Leader HA 基本都是同样的方式：每个节点都尝试注册一个象征 leader 的临时节点，其他没有注册成功的则成为follower，并且通过 watch 机制监控着 leader 所创建的临时节点，Zookeeper 通过内部心跳机制来确定 leader 的状态，一旦 leader 出现意外 Zookeeper 能很快获悉并且通知其他的 follower，其他 flower 在之后作出相关反应，这样就完成了一个切换，这种模式也是比较通用的模式，基本大部分都是这样实现的。但是这里面有个很严重的问题，如果注意不到会导致短暂的时间内系统出现脑裂，因为心跳出现超时可能是 leader 挂了，但是也可能是 zookeeper 节点之间网络出现了问题，导致 leader 假死的情况，leader 其实并未死掉，但是与 ZooKeeper 之间的网络出现问题导致 Zookeeper 认为其挂掉了然后通知其他节点进行切换，这样 follower 中就有一个成为了leader，但是原本的 leader 并未死掉，这时候 client 也获得 leader 切换的消息，但是仍然会有一些延时，zookeeper 需要通讯需要一个一个通知，这时候整个系统就很混乱可能有一部分 client 已经通知到了连接到新的 leader 上去了，有的 client 仍然连接在老的 leader上，如果同时有两个 client 需要对 leader 的同一个数据更新，并且刚好这两个 client 此刻分别连接在新老的 leader 上，就会出现很严重问题。

这里做下小总结： 

**假死**：由于心跳超时（网络原因导致的）认为 leader 死了，但其实 leader 还存活着。

**脑裂**：由于假死会发起新的 leader 选举，选举出一个新的 leader，但旧的 leader 网络又通了，导致出现了两个 leader ，有的客户端连接到老的 leader，而有的客户端则连接到新的 leader。



### 32、Zookeeper 脑裂是什么原因导致的？

主要原因是 Zookeeper 集群和 Zookeeper client 判断超时并不能做到完全同步，也就是说可能一前一后，如果是集群先于 client 发现，那就会出现上面的情况。同时，在发现并切换后通知各个客户端也有先后快慢。一般出现这种情况的几率很小，需要 leader 节点与Zookeeper 集群网络断开，但是与其他集群角色之间的网络没有问题，还要满足上面那些情况，但是一旦出现就会引起很严重的后果，数据不一致。



### 33、Zookeeper 是如何解决脑裂问题的？

要解决 Split-Brain 脑裂的问题，一般有下面几种种方法： Quorums (法定人数) 方式：比如 3 个节点的集群，Quorums = 2, 也就是说集群可以容忍 1 个节点失效，这时候还能选举出 1 个 lead，集群还可用。比如 4 个节点的集群，它的 Quorums = 3，Quorums 要超过 3，相当于集群的容忍度还是 1，如果 2 个节点失效，那么整个集群还是无效的。这是 zookeeper 防止"脑裂"默认采用的方法。

采用 Redundant communications （冗余通信）方式：集群中采用多种通信方式，防止一种通信方式失效导致集群中的节点无法通信。

Fencing (共享资源) 方式：比如能看到共享资源就表示在集群中，能够获得共享资源的锁的就是 Leader，看不到共享资源的，就不在集群中。

要想避免 zookeeper "脑裂"情况其实也很简单，在 follower 节点切换的时候不在检查到老的 leader 节点出现问题后马上切换，而是在休眠一段足够的时间，确保老的 leader 已经获知变更并且做了相关的 shutdown 清理工作了然后再注册成为 master 就能避免这类问题了，这个休眠时间一般定义为与 zookeeper 定义的超时时间就够了，但是这段时间内系统可能是不可用的，但是相对于数据不一致的后果来说还是值得的。

1、zooKeeper 默认采用了 Quorums 这种方式来防止"脑裂"现象。即只有集群中超过半数节点投票、才能选举出 Leader。这样的方式可以确保 leader 的唯一性,要么选出唯一的一个 leader，要么选举失败。在 zookeeper 中 Quorums 作用如下：

- 集群中最少的节点数用来选举 leader 保证集群可用。

- 通知客户端数据已经安全保存前集群中最少数量的节点数已经保存了该数据。一旦这些节点保存了该数据，客户端将被通知已经安全保存了，可以继续其他任务。而集群中剩余的节点将会最终也保存了该数据。

假设某个 leader 假死，其余的 followers 选举出了一个新的 leader。这时，旧的 leader 复活并且仍然认为自己是 leader，这个时候它向其他 followers 发出写请求也是会被拒绝的。因为每当新 leader 产生时，会生成一个 epoch 标号（标识当前属于那个leader的统治时期），这个 epoch 是递增的，followers 如果确认了新的 leader 存在，知道其 epoch，就会拒绝 epoch 小于现任 leader epoch 的所有请求。那有没有 follower 不知道新的leader存在呢，有可能，但肯定不是大多数，否则新 leader 无法产生。Zookeeper 的写也遵循quorum 机制，因此，得不到大多数支持的写是无效的，旧 leader 即使各种认为自己是 leader，依然没有什么作用。

zookeeper 除了可以采用上面默认的 Quorums 方式来避免出现"脑裂"，还可以可采用下面的预防措施：

2、添加冗余的心跳线，例如双线条线，尽量减少“裂脑”发生机会。 

3、启用磁盘锁。正在服务一方锁住共享磁盘，"裂脑"发生时，让对方完全"抢不走"共享磁盘资源。但使用锁磁盘也会有一个不小的问题，如果占用共享盘的一方不主动"解锁"，另一方就永远得不到共享磁盘。现实中假如服务节点突然死机或崩溃，就不可能执行解锁命令。后备节点也就接管不了共享资源和应用服务。于是有人在HA中设计了"智能"锁。即正在服务的一方只在发现心跳线全部断开（察觉不到对端）时才启用磁盘锁。平时就不上锁了。 

4、设置仲裁机制。例如设置参考 IP（如网关IP），当心跳线完全断开时，2 个节点都各自 ping 一下参考 IP，不通则表明断点就出在本端，不仅"心跳"、还兼对外"服务"的本端网络链路断了，即使启动（或继续）应用服务也没有用了，那就主动放弃竞争，让能够 ping 通参考 IP 的一端去起服务。更保险一些，ping 不通参考IP的一方干脆就自我重启，以彻底释放有可能还占用着的那些共享资源。



### 34、说说 Zookeeper 的 CAP 问题上做的取舍？

**一致性 C：**Zookeeper 是强一致性系统，为了保证较强的可用性，“一半以上成功即成功”的数据同步方式可能会导致部分节点的数据不一致。所以 Zookeeper 还提供了 sync() 操作来做所有节点的数据同步，这就关于 C 和 A 的选择问题交给了用户，因为使用 sync() 势必会延长同步时间，可用性会有一些损失。

**可用性 A ：**Zookeeper 数据存储在内存中，且各个节点都可以相应读请求，具有好的响应性能。Zookeeper 保证了数据总是可用的，没有锁。并且有一大半的节点所拥有的数据是最新的。

**分区容忍性 P：**Follower 节点过多会导致增大数据同步的延时（需要半数以上 follower 写完提交）。同时选举过程的收敛速度会变慢，可用性降低。Zookeeper 通过引入 observer 节点缓解了这个问题，增加 observer 节点后集群可接受 client 请求的节点多了，而且 observer 不参与投票，可以提高可用性和扩展性，但是节点多数据同步总归是个问题，所以一致性会有所降低。



### 35、watch 监听为什么是一次性的？

如果服务端变动频繁，而监听的客户端很多情况下，每次变动都要通知到所有的客户端，给网络和服务器造成很大压力。

一般是客户端执行 getData（节点 A,true) ，如果节点 A 发生了变更或删除，客户端会得到它的 watch 事件，但是在之后节点 A 又发生了变更，而客户端又没有设置 watch 事件，就不再给客户端发送。

在实际应用中，很多情况下，我们的客户端不需要知道服务端的每一次变动，我只要最新的数据即可。





## Redis 篇

### 1、Redis 为什么快

1. C 语言实现，效率高
2. 纯内存操作
3. 基于非阻塞的 IO 复用模型机制
4. 单线程的话就能避免多线程的频繁上下文切换问题
5. 丰富的数据结构（全称采用 hash 结构，读取速度非常快，对数据存储进行了一些优化，比如亚索表，跳表等）

##### 什么是 Redis?

Redis 是一个开源（BSD 许可）、基于内存、支持多种数据结构的存储系统，可以作为数据库、缓存和消息中间件。它支持的数据结构有字符串（strings）、哈希（hashes）、列表（lists）、集合（sets）、有序集合（sorted sets）等，除此之外还支持 bitmaps、hyperloglogs 和地理空间（geospatial ）索引半径查询等功能。

它内置了复制（Replication）、LUA 脚本（Lua scripting）、LRU 驱动事件（LRU eviction）、事务（Transactions）和不同级别的磁盘持久化（persistence）功能，并通过 Redis 哨兵（哨兵）和集群（Cluster）保证缓存的高可用性（High availability）。



**跳表**：将有序链表改造为支持近似“折半查找”算法，可以进行快速的插入、删除、查找操作

1. **纯内存 KV 操作**

Redis 的操作都是基于内存的，CPU不是 Redis 性能瓶颈,，Redis 的瓶颈是机器内存和网络带宽。

在计算机的世界中，CPU 的速度是远大于内存的速度的，同时内存的速度也是远大于硬盘的速度。Redis 的操作都是基于内存的，绝大部分请求是纯粹的内存操作，非常迅速。

2. **单线程操作**

使用单线程可以省去多线程时 CPU 上下文会切换的时间，也不用去考虑各种锁的问题，不存在加锁释放锁操作，没有死锁问题导致的性能消耗。对于内存系统来说，多次读写都是在一个 CPU 上，没有上下文切换效率就是最高的！既然单线程容易实现，而且 CPU 不会成为瓶颈，那就顺理成章的采用单线程的方案了

Redis 单线程指的是网络请求模块使用了一个线程，即一个线程处理所有网络请求，其他模块该使用多线程，仍会使用了多个线程。

3.  **I/O 多路复用**

为什么 Redis 中要使用 I/O 多路复用这种技术呢？

首先，Redis 是跑在单线程中的，所有的操作都是按照顺序线性执行的，但是由于读写操作等待用户输入或输出都是阻塞的，所以 I/O 操作在一般情况下往往不能直接返回，这会导致某一文件的 I/O 阻塞导致整个进程无法对其它客户提供服务，而 I/O 多路复用就是为了解决这个问题而出现的

#### 如何实现IO多路复用？

##### 在  Java 中，可以使用以下三种方式来实现 IO 多路复用：

1. 使用 Selector 和 Channel：Java NIO 提供了 Selector 和 Channel 的机制，可以实现 IO 多路复用。Selector 是一个选择器，通过它可以注册多个 Channel，并监听这些 Channel 上的事件。当某个 Channel 上有读或写事件发生时，Selector 会通知应用程序进行相应的处理。这样就可以使用单个线程来处理多个 Channel，实现 IO 的多路复用。具体步骤如下：
   - 创建 Selector 对象。
   - 将需要进行 IO 操作的 Channel 注册到 Selector上，可以通过调用 Channel 的 register() 方法来实现。
   - 调用 Selector 的 select() 方法进行阻塞，等待事件发生。
   - 当有事件发生时，通过调用Selector的selectedKeys()方法获取到发生事件的Channel集合。
   - 遍历事件集合，根据事件类型进行相应的处理。

2. 使用 `java.nio.channels.Selector` 类：Java NIO 提供了 Selector 类，可以实现 IO 多路复用。具体步骤如下：
   - 创建 Selector 对象。
   - 将需要进行 IO 操作的 Channel 注册到 Selector 上，可以通过调用 Channel 的 configureBlocking(false) 方法将 Channel 设置为非阻塞模式，然后调用 Selector 的 register() 方法将 Channel 注册到 Selector 上。
   - 调用 Selector 的 select() 方法进行阻塞，等待事件发生。
   - 当有事件发生时，通过调用 Selector 的 selectedKeys() 方法获取到发生事件的 SelectionKey 集合。
   - 遍历SelectionKey集合，根据事件类型进行相应的处理。

3. 使用 `java.nio.channels.Selector` 类和 `java.nio.channels.ServerSocketChannel` 类：这种方式主要用于实现基于 NIO 的服务器。具体步骤如下：
   - 创建 Selector 对象。
   - 创建ServerSocketChannel对象，并将其绑定到指定的端口上。
   - 将ServerSocketChannel设置为非阻塞模式，并将其注册到Selector上。
   - 调用Selector的select()方法进行阻塞，等待连接事件发生。
   - 当有连接事件发生时，通过调用Selector的selectedKeys()方法获取到发生事件的SelectionKey集合。
   - 遍历SelectionKey集合，根据事件类型进行相应的处理。

以上是三种常用的实现IO多路复用的方式，在实际应用中可以根据具体的需求和场景选择合适的方式。

#### redis 中的 IO 多路复用是怎么实现的?

在 Redis 中，实现 IO 多路复用是通过使用事件驱动的网络库来实现的。Redis 使用了多个事件库，其中最常用的是基于 epoll 系统调用的事件库。

具体来说，Redis 使用了以下几个关键组件来实现IO多路复用：

1. 文件事件（File Event）：Redis 通过文件事件来处理网络 IO 操作。每个文件事件都与一个套接字（socket）关联，当套接字就绪时，相应的文件事件就会被触发。文件事件包含了读事件和写事件两种类型。

2. 事件处理器（Event Loop）：事件处理器是 Redis 的核心组件，负责监听并处理文件事件。事件处理器使用 IO 多路复用技术来监听多个文件事件，一旦有就绪事件，就会调用相应的事件处理函数进行处理。事件处理器是 Redis 单线程模型的核心。

3. 多路复用器（Multiplexer）：Redis 使用多路复用器来实现 IO 多路复用。多路复用器可以同时监听多个文件描述符，等待其中任何一个文件描述符变为可读或可写状态，从而实现对多个文件描述符的复用。

4. epoll系统调用：在 Linux 系统上，Redis 使用 epoll 系统调用来实现 IO 多路复用。epoll 系统调用在内核中管理一组文件描述符，并通过监听这些文件描述符的状态变化来实现 IO 多路复用。

Redis 的事件驱动模型可以保证高效的 IO 操作和响应速度，因为它可以在一个线程中同时处理多个客户端的请求，而无需为每个客户端创建一个线程。这种单线程的设计使得 Redis 能够高效地处理大量的并发请求，并且具有较低的系统开销。

#### 简述 select，poll，epoll

select、poll 和 epoll 都是用于实现 IO 多路复用的机制，常用于高并发的网络编程中。

1. select：
select 是最早的一种实现 IO 多路复用的机制，它通过 select 系统调用来监听多个文件描述符的状态变化。select 采用的是轮询的方式，即遍历所有的文件描述符，检查是否有 IO 事件发生。但是 select 有一些限制，其中最主要的是每次调用 select 时，都需要将所有的文件描述符集合传递给内核，而内核会返回就绪的文件描述符集合，这个过程会带来一定的开销。

2. poll：
poll 是在 select 的基础上进行改进的一种 IO 多路复用机制。与 select 相比，poll 的改进主要体现在解决了select的文件描述符集合大小限制问题。poll使用一个pollfd结构体的数组来保存待监听的文件描述符，每次调用poll时，只需要将这个数组传递给内核，内核会返回就绪的文件描述符集合。

3. epoll：
epoll 是 Linux 特有的一种高效的 IO 多路复用机制，它在 select 和 poll 的基础上进行了进一步的改进。epoll 使用了事件驱动的方式，当某个文件描述符就绪时，内核会通知应用程序，而不需要应用程序进行轮询。这样就避免了select和poll中需要遍历所有文件描述符的开销。另外，epoll使用了一个事件表来保存待监听的文件描述符，这个事件表在内核中维护，可以支持大量的并发连接。

总结来说，select、poll 和 epoll 都是实现 IO 多路复用的机制，它们的主要区别在于实现方式和性能表现。在Linux系统中，由于epoll的高性能和可扩展性，一般推荐使用epoll来实现高并发的网络编程。

#### Java 中 NIO 的三剑客是啥

1. Java中的 NIO（New Input/Output）的三剑客是指 Selector、Channel 和 Buffer。

   1. Selector（选择器）：用于检测一个或多个NIO Channel的状态是否处于可读、可写等事件，从而实现非阻塞IO操作。可以通过一个线程管理多个Channel，提高系统资源利用率。

   2. Channel（通道）：类似于传统IO中的流，用于读取和写入数据。不同的是，Channel可以同时进行读写操作，而流只能单向操作。常见的Channel类型有FileChannel、SocketChannel、ServerSocketChannel和DatagramChannel等。

   3. Buffer（缓冲区）：用于临时存储数据。在 NIO 中，数据读取和写入都是通过Buffer来进行的。Buffer提供了不同类型的缓冲区，如ByteBuffer、CharBuffer、ShortBuffer、IntBuffer、LongBuffer、FloatBuffer和DoubleBuffer等，用于存储不同类型的数据。

   这三个组件共同协作，实现了高效的非阻塞 IO 操作。Selector 负责监听 Channel 的事件，Channel 负责读写数据，而Buffer则作为数据的中转站，存储和提供数据。

4. **Reactor 设计模式**

Redis 基于 Reactor 模式开发了自己的网络事件处理器，称之为文件事件处理器(File Event Hanlder)。

- 纯内存访问，不需要访问磁盘

- 单线程避免上下文切换

- 渐进式 ReHash、缓存时间戳

  为了实现从键到值的快速访问，Redis 使用了一个哈希表来保存所有键值对。一个哈希表，其实就是一个数组，数组的每个元素称为一个哈希桶。所以，我们常说，一个哈希表是由多个哈希桶组成的，每个哈希桶中保存了键值对数据。

  **Key：**一般是 String 类型

  **Value：**基本数据类型 String、List、Hash、Set、ZSet

  **高级数据类型（待整理）**

  Redis 解决哈希冲突的方式，就是链式哈希。链式哈希也很容易理解，就是指同一个哈希桶中的多个元素用一个链表来保存，它们之间依次用指针连接。
  当然如果这个数组一直不变，那么 hash 冲突会变很多，这个时候检索效率会大打折扣，所以 Redis 就需要把数组进行扩容(一般是扩大到原来的两倍)，但是问题来了，扩容后每个 hash 桶的数据会分散到不同的位置，这里设计到元素的移动，必定会阻塞 IO，所以这个 ReHash 过程会导致很多请求阻塞。

  **为了避免这个问题，Redis 采用了渐进式rehash。**
  首先、Redis 默认使用了两个全局哈希表：哈希表 1 和哈希表 2。一开始，当你刚插入数据时，默认使用哈希表 1，此时的哈希表 2 并没有被分配空间。随着数据逐步增多，Redis 开始执行 rehash。

  1、给哈希表 2 分配更大的空间，例如是当前哈希表 1 大小的两倍

  2、把哈希表 1中的数据重新映射并拷贝到哈希表 2 中

  3、释放哈希表 1的空间

  在上面的第二步涉及大量的数据拷贝，如果一次性把哈希表 1 中的数据都迁移完，会造成 Redis 线程阻塞，无法服务其他请求。此时，Redis 就无法快速访问数据了。

  在Redis 开始执行 rehash，Redis 仍然正常处理客户端请求，但是要加入一个额外的处理：处理第1个请求时，把哈希表 1 中的第 1 个索引位置上的所有 entries 拷贝到哈希表 2 中处理第 2 个请求时，把哈希表 1 中的第 2 个索引位置上的所有 entries 拷贝到哈希表 2中如此循环，直到把所有的索引位置的数据都拷贝到哈希表 2 中。
  这样就巧妙地把一次性大量拷贝的开销，分摊到了多次处理请求的过程中，避免了耗时操作，保证了数据的快速访问。
  所以这里基本上也可以确保根据 key 找 value 的操作在O (1)左右。
  不过这里要注意，如果 Redis 中有海量的 key 值的话，这个Rehash过程会很长很长，虽然采用渐进式 Rehash，但在Rehash的过程中还是会导致请求有不小的卡顿。并且像一些统计命令也会非常卡顿: 比如 keys
  按照 Redis 的配置每个实例能存储的最大的 key 的数量为 2 的 32 次方,即 2.5 亿，但是尽量把 key 的数量控制在千万以下，这样就可以避免 Rehash 导致的卡顿问题，如果数量确实比较多，建议采用分区 hash 存储。

  **为什么 Redis 要缓存系统时间戳**

  我们平时使用系统时间截时，常常是不假思索地使用 System.currentTimelnMillis 或者 time.time() 来获取系统的毫秒时间戳。Redis 不能这样，因为每一次获取系统时间截都是一次系统调用，系统调用相对来说是比较费时间的，作为单线程的 Redis 承受不起，所以它需要对时间进行缓存，由一个定时任务，每毫秒更新一次时间缓存，获取时间都是从缓存中直接拿.

  

### 2、Redis 合适的应用场景

- 缓存
- 计数器
- 分布式会话
- 排行榜
- 最新列表
- 分布式锁
- 消息队列

1、会话缓存（Session Cache）最常用的一种使用 Redis 的情景是会话缓存（session cache），用 Redis 缓存会话比其他存储（如Memcached）的优势在于：Redis 提供持久化。当维护一个不是严格要求一致性的缓存时，如果用户的购物车信息全部丢失，大部分人都会不高兴的，现在，他们还会这样吗？幸运的是，随着 Redis 这些年的改进，很容易找到怎么恰当的使用 Redis 来缓存会话的文档。甚至广为人知的商业平台 Magento 也提供 Redis 的插件。

2、全页缓存（FPC）除基本的会话 token 之外，Redis 还提供很简便的 FPC 平台。回到一致性问题，即使重启了 Redis 实例，因为有磁盘的持久化，用户也不会看到页面加载速度的下降，这是一个极大改进，类似 PHP 本地 FPC。再次以 Magento 为例，Magento 提供一个插件来使用 Redis 作为全页缓存后端。此外，对 WordPress 的用户来说，Pantheon 有一个非常好的插件 wp-redis，这个插件能帮助你以最快速度加载你曾浏览过的页面。

3、队列 Redis 在内存存储引擎领域的一大优点是提供 list 和 set 操作，这使得 Redis 能作为一个很好的消息队列平台来使用。Redis 作为队列使用的操作，就类似于本地程序语言（如 Python）对 list 的 push/pop操作。如果你快速的在 Google 中搜索“Redis queues”，你马上就能找到大量的开源项目，这些项目的目的就是利用 Redis 创建非常好的后端工具，以满足各种队列需求。例如，Celery 有一个后台就是使用Redis 作为 broker，你可以从这里去查看。

4、排行榜/计数器Redis 在内存中对数字进行递增或递减的操作实现的非常好。集合（Set）和有序集合（Sorted Set）也使得我们在执行这些操作的时候变的非常简单，Redis 只是正好提供了这两种数据结构。微信搜索公众号：Java专栏，获取最新面试手册所以，我们要从排序集合中获取到排名最靠前的 10 个用户–我们称之为“user_scores”，我们只需要像下面一样执行即可：当然，这是假定你是根据你用户的分数做递增的排序。如果你想返回用户及用户的分数，你需要这样执行：ZRANGE user_scores 0 10 WITHSCORESAgora Games 就是一个很好的例子，用 Ruby 实现的，它的排行榜就是使用 Redis 来存储数据的，你可以在这里看到。

5、发布/订阅最后（但肯定不是最不重要的）是 Redis 的发布/订阅功能。发布/订阅的使用场景确实非常多。我已看见人们在社交网络连接中使用，还可作为基于发布/订阅的脚本触发器，甚至用 Redis 的发布/订阅功能来建立聊天系统！

https://blog.csdn.net/MinggeQingchun/article/details/130533914

https://blog.51cto.com/u_14402/6371239



### 3、Redis 6.0 之前为什么一直不使用多线程？

- 使用 Redis、CPU 不是瓶颈，受制于内存、网络

- 提高 Redis，Pipeline (命令批量) 每秒 100 万个请求

- 单线程，内部维护比较低

- 如果是多线程（线程切换、加锁\解锁、导致死锁问题）
- 惰性 ReHash（渐进式的 ReHash）

一般的公司，单线程 Redis 就够了

官方曾做过类似问题的回复：使用 Redis 时，几乎不存在 CPU 成为瓶颈的情况，

Redis主要受限于内存和网络。例如在一个普通的 Linux 系统上，Redis通过使用 pipelining 每秒可以处理 100 万个请求，所以如果应用程序主要使用O(N)或O(log(N))的命令，它几乎不会占用太多 CPU。

使用了单线程后，可维护性高。多线程模型虽然在某些方面表现优异，但是它却引入了程序执行顺序的不确定性，带来了并发读写的一系列问题，增加了系统复杂度、同时可能存在线程切换、甚至加锁解锁、死锁造成的性能损耗。Redis 通过 AE 事件模型以及 IO 多路复用等技术，处理性能非常高，因此没有必要使用多线程。单线程机制使得 Redis 内部实现的复杂度大大降低，Hash 的惰性 Rehash、Lpush 等等,“线程不安全” 的命令都可以无锁进行。



### 4、Redis 6.0 为什么要引入多线程？

1、单线程就够了。数据->内存 响应时间 100 纳秒，比较小的数据包，8 w~10 w QPS（极限值）

2、大的公司，需要更大 QPS，IO 多线程（内部执行命令还是单线程）多线程任务分摊到 Redis 同步 IO 中读写 负载

3、为什么不采用分布式架构------很大的缺点

服务数量多，维护成本高

Redis 命令不适用数据分区

数据分区，无法解决热点读/写的问题

数据倾斜、重新分配、扩容、缩容，更加复杂



Redis 将所有数据放在内存中，内存的响应时长大约为 100 纳秒，对于小数据包，Redis服务器可以处理 80,000 到 100,000 QPS，这也是Redis处理的极限了，对于 80% 的公司来说，单线程的 Redis 已经足够使用了。

但随着越来越复杂的业务场景，有些公司动不动就上亿的交易量，因此需要更大的 QPS，IO 的多线程（内部执行命令还是单线程）。常见的解决方案是在分布式架构中对数据进行分区并采用多个服务器，但该方案有非常大的缺点，例如要管理的 Redis 服务器太多，维护代价大；某些适用于单个 Redis 服务器的命令不适用于数据分区；数据分区无法解决热点读/写问题；数据偏斜，重新分配和放大/缩小变得更加复杂等等。



### 5、Redis 有哪些高级功能？

消息队列、自动过期删除、事务、数据持久化、分布式锁、附近的人、慢查询分析、Sentinel 和集群等多项功能。



### 6、为什么要用 Redis？

- 高性能

- 高并发

使用缓存的目的就是提升读写性能。而实际业务场景下，更多的是为了提升读性能，带来更好的性能，带来更高的并发量。Redis 的读写性能比 MySQL 好的多，我们就可以把 MySQL 中的热点数据缓存到 Redis 中，提升读取性能，同时也减轻了 MySQL 的读取压力

#### 使用 Redis 有哪些好处？

具有以下好处：

- 读取速度快，因为数据存在内存中，所以数据获取快；

- 支持多种数据结构，包括字符串、列表、集合、有序集合、哈希等；

- 支持事务，且操作遵守原子性，即对数据的操作要么都执行，要么都不支持；

- 还拥有其他丰富的功能，队列、主从复制、集群、数据持久化等功能。

#### Redis 优缺点：

**优点**

- 速度快：因为数据存在内存中，类似于 HashMap ， HashMap 的优势就是查找和操作的时间复杂度都是 O (1) 。

- 支持丰富的数据结构：支持 String ，List，Set，Sorted Set，Hash 五种基础的数据结构。

- 持久化存储：Redis 提供 RDB 和 AOF 两种数据的持久化存储方案，解决内存数据库最担心的万一 Redis 挂掉，数据会消失掉

- 高可用：内置 Redis Sentinel ，提供高可用方案，实现主从故障自动转移。 内置 Redis Cluster ，提供集群方案，实现基于槽的分片方案，从而支持更大的 Redis 规模。

- 丰富的特性：Key 过期、计数、分布式锁、消息队列等。

**缺点**

- 由于 Redis 是内存数据库，所以，单台机器，存储的数据量，跟机器本身的内存大小。虽然 Redis 本身有 Key 过期策略，但是还是需要提前预估和节约内存。如果内存增长过快，需要定期删除数据。

- 如果进行完整重同步，由于需要生成 RDB 文件，并进行传输，会占用主机的 CPU ，并会消耗现网的带宽。不过 Redis 2.8 版本，已经有部分重同步的功能，但是还是有可能有完整重同步的。比如，新上线的备机。

- 修改配置文件，进行重启，将硬盘中的数据加载进内存，时间比较久。在这个过程中， Redis 不能提供服务。



### 7、Redis 与 Memcached 相对有哪些优势？

|              | Redis                                                        | Memcached                                       |
| ------------ | ------------------------------------------------------------ | ----------------------------------------------- |
| 整体类型     | 支持内存、非关系型数据库                                     | 支持内存、key-value                             |
| 数据类型     | String、List、Set、ZSet、Hash                                | 文本型、二进制类型                              |
| 操作类型     | 单个操作、批量操作、事务支持（弱事务、结合 LUA）、每个类型不同的 CRUD | CRUD、少量其命令                                |
| 附加功能     | 发布\订阅、主从高可用（哨兵 故障转移）、序列化支持、支持 LUA 脚本 | 多线程服务支持                                  |
| 网络 IO 模型 | 执行命令-单线程、网络操作-多线程                             | 多线程、非阻塞的 IO 模式                        |
| 持久化       | RDB、AOF                                                     | 纯粹内存，不支持持久化                          |
| 内存管理     | 过期、内存淘汰策略，适合做数据存储                           | 预分配池的管理，内存 slab 块 增长因子，适合缓存 |

1、 memcached 所有的值均是简单的字符串，Redis 作为其替代者，支持更为丰富的数据类型

2、 Redis 的速度比 memcached 快很多

3、 Redis 可以持久化其数据

4、Redis 支持数据的备份，即 master-slave 模式的数据备份。

5、使用底层模型不同，它们之间底层实现方式以及与客户端之间通信的应用协议不一样。Redis直接自己构建了 VM 机制 ，因为一般的系统调用系统函数的话，会浪费一定的时间去移动和请求。

6、value 大小：Redis 最大可以达到 1 GB，而 memcache 只有 1 MB

#### Redis 和 Memcached 的区别与联系：

- Redis 和 Memcache 都是将数据存放在内存中，都是内存数据库。不过 Memcached 还可用于缓存其他东西，例如图片、视频等等。

- Memcached 仅支持 key-value 结构的数据类型，Redis 不仅仅支持简单的 key-value 类型的数据，同时还提供 list，set，hash 等数据结构的存储。

- 虚拟内存– Redis 当物理内存用完时，可以将一些很久没用到的 value 交换到磁盘
- 分布式–设定 Memcached 集群，利用 magent 做一主多从； Redis 可以做一主多从。都可以一主一从

- 存储数据安全– Memcached 挂掉后，数据没了； Redis 可以定期保存到磁盘（持久化）

- Memcached 的单个 value 最大 1 m ， Redis 的单个 value 最大 512 m 。

- 灾难恢复– Memcached 挂掉后，数据不可恢复； Redis 数据丢失后可以通过 aof 恢复

- Redis 原生就支持集群模式， Redis3.0 版本中，官方便能支持 Cluster 模式了， Memcached 没有原生的集群模式，需要依赖客户端来实现，然后往集群中分片写入数据。

- Memcached 网络 IO 模型是多线程，非阻塞 IO 复用的网络模型，原型上接近于 nignx 。而 Redis 使用单线程的IO复用模型，自己封装了一个简单的 AeEvent 事件处理框架，主要实现类 epoll，kqueue 和 select ，更接近于 Apache 早期的模式。



### 8、怎么理解Redis中事务？

事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行，事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。事务是一个原子操作：事务中的命令要么全部被执行，要么全部都不执行。不过 Redis 的是弱事物。

事务是 Redis 实现在服务器端的行为，用户执行 MULTI 命令时，服务器会将对应这个用户的客户端对象设置为一个特殊的状态，在这个状态下后续用户执行的查询命令不会被真的执行，而是被服务器缓存起来，直到用户执行 EXEC 命令为止，服务器会将这个用户对应的客户端对象中缓存的命令按照提交的顺序依次执行。

Redis 提供了简单的事务，之所以说它简单，主要是因为它不支持事务中的回滚特性,同时无法实现命令之间的逻辑关系计算，当然也体现了 Redis 的 “keep it simple” 的特性



### 9、Redis的过期策略以及内存淘汰机制？

Redis 采用的是定期删除 + 惰性删除策略。 为什么不用定时删除策略？定时删除，用一个定时器来负责监视 key，过期则自动删除。虽然内存及时释放，但是十分消耗 CPU 资源。在大并发请求下，CPU 要将时间应用在处理请求，而不是删除 key,因此没有采用这一策略. 定期删除+惰性删除是如何工作的呢? 定期删除，Redis 默认每个100 ms 检查，是否有过期的key,有过期 key 则删除。需要说明的是，Redis 不是每个 100 ms 将所有的 key 检查一次，而是随机抽取进行检查(如果每隔 100 ms,全部key进行检查，Redis 岂不是卡死)。因此，如果只采用定期删除策略，会导致很多 key 到时间没有删除。 于是，惰性删除派上用场。也就是说在你获取某个 key 的时候，Redis 会检查一下，这个 key 如果设置了过期时间那么是否过期了？如果过期了此时就会删除

1. noeviction：返回错误当内存限制达到，并且客户端尝试执行会让更多内存被使用的命令。

2. allkeys-lru： 尝试回收最少使用的键（LRU），使得新添加的数据有空间存放。

3. volatile-lru: 尝试回收最少使用的键（LRU），但仅限于在过期集合的键,使得新添加的数据有空间存放。

4. allkeys-random: 回收随机的键使得新添加的数据有空间存放。

5. volatile-random: 回收随机的键使得新添加的数据有空间存放，但仅限于在过期集合的键。

6. volatile-ttl: 回收在过期集合的键，并且优先回收存活时间（TTL）较短的键,使得新添加的数据有空间存放。



### 10、什么是缓存穿透？如何避免？

**缓存穿透：**指查询一个一定不存在的数据，如果从存储层查不到数据则不写入缓存，这将导致这个不存在的数据每次请求都要到 DB 去查询，可能导致 DB 挂掉。

 解决方案：1.查询返回的数据为空，仍把这个空结果进行缓存，但过期时间会比较短；2.布隆过滤器：将所有可能存在的数据哈希到一个足够大的 bitmap 中，一个一定不存在的数据会被这个 bitmap 拦截掉，从而避免了对 DB 的查询。

**布隆过滤器**

- 用来判断一个元素是否在一个集合中（一个二进制数组和一个 Hash 算法）

**误判问题：**

- 通过 hash 计算在数组上不一定在集合

- 本质是 hash 冲突

- 通过 hash 计算不在数组的就一定不在集合

**优化方案：**

- 增大数组（预估合适值）

- 增加 hash 函数



### 11、什么是缓存雪崩？如何避免？

- Redis 故障——解决-集群（主从、多级缓存如 EH Catch、限流组件 hystrix）

- Redis 中大量 key 的 ttl （失效时间）过期——解决：ttl 岔开

缓存雪崩：设置缓存时采用了相同的过期时间，导致缓存在某一时刻同时失效，请求全部转发到 DB，DB 瞬时压力过重雪崩。与缓存击穿的区别：雪崩是很多 key，击穿是某一个key 缓存。

解决方案：将缓存失效时间分散开，比如可以在原有的失效时间基础上增加一个随机值，比如 1-5 分钟随机，这样每一个缓存的过期时间的重复率就会降低，就很难引发集体失效的事件。



### 12、使用 Redis 如何设计分布式锁？

基于 Redis 实现的分布式锁，一个严谨的的流程如下：

1、加锁

SET lock_key $unique_id EX $expire_time NX

2、操作共享资源

3、释放锁：LUA 脚本，先 GET 判断锁是否归属自己，再 DEL 释放锁

**分布式锁加入看门狗**

加锁时，先设置一个过期时间，然后开启一个 [ 守护线程 ] ，定时去检测这个锁的失效时间，如果锁快要过期，操作共享资源还未完成，那么自动对锁进行 [ 续期 ] ，重新设置过期时间。

这个守护线程我们一般也把它叫做 [ 看门狗 ] 线程。



### 13、怎么使用 Redis 实现消息队列？

- **基于 List 的 LPUSH + BRPOP 的实现**

足够简单，消费消息延迟几乎为零，但是需要处理空闲连接的问题。

如果线程一直阻塞在那里，Redis 客户端的连接就成了闲置连接，闲置过久，服务器一般会主动断开连接，减少闲置资源占用，这个时候blpop 和 brpop 或抛出异常，所以在编写客户端消费者的时候要小心，如果捕获到异常，还有重试。

其他缺点包括：

做消费者确认 ACK 麻烦，不能保证消费者消费消息后是否成功处理的问题（宕机或处理异常等），通常需要维护一个 Pending ( PEL ) 列表，保证消息处理确认；不能做广播模式，如 pub / sub，消息发布 / 订阅模型；不能重复消费，一旦消费就会被删除；不支持分组消费。

- **基于 Sorted - Set 的实现**

多用来实现延迟队列，当然也可以实现有序的普通的消息队列，但是消费者无法阻塞的获取消息，只能轮询，不允许重复消息。

- **PUB / SUB，订阅 / 发布模式**

优点：

典型的广播模式，一个消息可以发布到多个消费者；多信道订阅，消费者可以同时订阅多个信道，从而接收多类消息；消息即时发送，消息不用等待消费者读取，消费者会自动接收到信道发布的消息。

缺点：

消息一旦发布，不能接收。换句话就是发布时若客户端不在线，则消息丢失，不能寻回；不能保证每个消费者接收的时间是一致的；若消费者客户端出现消息积压，到一定程度，会被强制断开，导致消息意外丢失。通常发生在消息的生产远大于消费速度时；可见，Pub/Sub 模式不适合做消息存储，消息积压类的业务，而是擅长处理广播，即时通讯，即时反馈的业务。

- **基于Stream类型的实现**

Redis 5.0 最大的新特性就是多出了一个数据结构 Stream，它是一个新的强大的支持多播的可持久化的消息队列，借鉴了 Kafka 的设计

Stream 已经具备了一个消息队列的基本要素，生产者 API 、消费者 API ，消息 Broker ，消息的确认机制等等，所以在使用消息中间件中产生的问题，这里一样也会遇到。

**Stream 消息太多怎么办?**
要是消息积累太多，Stream 的链表岂不是很长内容会不会爆掉? xdel 指令又不会删除消息，它只是给消息做了个标志位。
Redis 自然考虑到了这一点，所以它提供了一个定长 Stream 功能在 xadd 的指令提供一个定长长度 maxlen ,就可以将老的消息干掉，确保最多不超过指定长度。

**消息如果忘记 ACK 会怎样?**
Stream 在每个消费者结构中保存了正在处理中的消息 ID 列表 PEL，如果消费者收到了消息处理完了但是没有回复 ACK ，就会导致 PEL 列表不断增长，如果有很多消费组的话，那么这个 PEL 占用的内存就会放大。所以消息要尽可能的快速消费并确认。

基本上已经有了一个消息中间件的雏形，可以考虑在生产过程中使用。

**PEL 如何避免消息丢失?**
在客户端消费者读取 Stream 消息时，Redis 服务器将消息回复给客户端的过程中，客户端突然断开了连接，消息就丢失了。但是 PEL 里已经保存了发出去的消息 ID 。待客户端重新连上之后，可以再次收到 PEL 中的消息D不过此时 xreadgroup 的起始消息 ID 不能为参数>，而必须是任意有效的消息 ID ，一般将参数设为 0 - 0，表示读取所有的PEL消息以及自 last_delivered_id 之后的新消息。

**死信问题**
如果某个消息，不能被消费者处理，也就是不能被 XACK，这是要长时间处于 Pending 列表中，即使被反复的转移给各个消费者也是如此。此时该消息的 delivery counter (通过 XPENDING 可以查询到)就会累加，当累加到某个我们预设的临界值时，我们就认为是坏消息(也叫死信，DeadLetter，无法投递的消息)，由于有了判定条件，我们将坏消息外理掉即可，删除即可。删除一个消息，使用 XDEL 语法，注意，这个命令并没有删除 Pending 中的消息，因此查看 Pending ，消息还会在，可以在执行执行 XDEL 之后，XACK 这个消息标识其处理完毕。

### 14、什么是 bigkey？会有什么影响？

bigkey 是指 key 对应的 value 所占的内存空间比较大，例如一个字符串类型的 value 可以最大存到 512 MB，一个列表类型的 value 最多可以存储 23 - 1 个元素。

如果按照数据结构来细分的话，一般分为字符串类型 bigkey 和非字符串类型 bigkey。

字符串类型：体现在单个 value 值很大，一般认为超过 10 KB 就是 bigkey，但这个值和具体的 OPS 相关。

非字符串类型：哈希、列表、集合、有序集合，体现在元素个数过多。

bigkey 无论是空间复杂度和时间复杂度都不太友好，下面我们将介绍它的危害。

**bigkey 的危害**

bigkey 的危害体现在三个方面：

1、内存空间不均匀.(平衡)：例如在 Redis Cluster 中，bigkey 会造成节点的内存空间使用不均匀。

2、超时阻塞：由于 Redis 单线程的特性，操作 bigkey 比较耗时，也就意味着阻塞 Redis 可能性增大。

3、网络拥塞：每次获取 bigkey 产生的网络流量较大

假设一个 bigkey 为 1 MB，每秒访问量为 1000，那么每秒产生 1000 MB 的流量,对于普通的千兆网卡(按照字节算是 128 MB/s )的服务器来说简直是灭顶之灾，而且一般服务器会采用单机多实例的方式来部署，也就是说一个 bigkey 可能会对其他实例造成影响，其后果不堪设想。

对 value 值作拆分

### 15、Redis 如何解决 key 冲突？

1、业务隔离

业务 A Set A 1，key 区分出来

2、key 的设计

业务模块 + 系统名称 + 关键字 （ biz- pay- orderId -11）

3、分布式锁

多个客户端，并发写 key 原有值 1 ，修改顺序，2 -> 3 -> 4，2

客户端拿到锁，才能进行操作

时间戳 set key 7 : 00  set key 6 : 00



遇到 hash 冲突采用链表进行处理



### 16、怎么提高缓存命中率？

1、提前加载

2、增加缓存的存储空间，提高缓存的数据，提高命中率

3、调整缓存的存储类型

4、提升缓存的更新频次

需要在业务需求，缓存粒度，缓存策略，技术选型等各个方面去通盘考虑并做权衡。尽可能的聚焦在高频访问且时效性要求不高的热点业务上，通过缓存预加载（预热）、增加存储容量、调整缓存粒度、更新缓存等手段来提高命中率。



### 17、Redis 持久化方式有哪些？有什么区别？

RDB、AOF、混合持久化。

RDB 的优缺点：

优点：RDB 持久化文件，速度比较快，而且存储的是一个二进制文件，传输起来很方便。

缺点：RDB 无法保证数据的绝对安全，有时候就是 1 s 也会有很大的数据丢失。

AOF 的优缺点：

优点：AOF 相对 RDB 更加安全，一般不会有数据的丢失或者很少，官方推荐同时开启 AOF 和 RDB。

缺点：AOF 持久化的速度，相对于 RDB 较慢，存储的是一个文本文件，到了后期文件会比较大，传输困难。



#### Redis 提供两种持久化机制 RDB 和 AOF 机制:

**RDB** **持久化方式**

是指用数据集快照的方式半持久化模式记录 redis 数据库的所有键值对，在某个时间点将数据写入一个临时文件，持久化结束后，用这个临时文件替换上次持久化的文件，达到数据恢复。

**优点：**

- 只有一个文件 dump.rdb ，方便持久化。

- 容灾性好，一个文件可以保存到安全的磁盘。

- 性能最大化，fork 子进程来完成写操作，让主进程继续处理命令，所以是 IO 最大化。使用单独子进程来进行持久化，主进程不会进行任何 IO 操作，保证了 Redis 的高性能

- 相对于数据集大时，比 AOF 的启动效率更高。

**缺点：**

- 数据安全性低。 RDB 是间隔一段时间进行持久化，如果持久化之间 Redis 发生故障，会发生数据丢失。所以这种方式更适合数据要求不严谨的时候

**AOF=Append****-****only file** **持久化方式**是指所有的命令行记录以 Redis 命令请求协议的格式完全持久化存储，保存为 AOF 文件。

**优点：**

（1）数据安全， AOF 持久化可以配置 appendfsync 属性，有 always，每进行一次命令操作就记录

到 AOF 文件中一次。

（2）通过 append 模式写文件，即使中途服务器宕机，可以通过 redis-check-aof 工具解决数据

一致性问题。

（3） AOF 机制的 rewrite 模式。 AOF 文件没被 rewrite 之前（文件过大时会对命令进行合并重

写），可以删除其中的某些命令（比如误操作的 flushall )

**缺点：**

（1） AOF 文件比 RDB 文件大，且恢复速度慢。

（2）数据集大的时候，比 RDB 启动效率低。

### 18、为什么 Redis 需要把所有数据放到内存中？

Redis 为了达到最快的读写速度，将数据都读到内存中，并通过异步的方式将数据写入磁盘，所以 Redis 具有快速和数据持久化的特征。如果不将数据放在内存中，磁盘 I/O 速度为严重影响 Redis 的性能。



### 19、如何保证缓存与数据库双写时的数据一致性

第一种方案：采用延时双删策略

具体的步骤就是：

先删除缓存；

再写数据库；

休眠 500 毫秒；

再次删除缓存。

第二种方案：异步更新缓存(基于订阅 binlog 的同步机制)

技术整体思路：

MySQL binlog 增量订阅消费 + 消息队列 + 增量数据更新到 Redis



### 20、Redis 集群方案应该怎么做？

**Redis集群**
Redis Cluster 是 Redis 的分布式解决方案，在 3.0 版本正式推出，有效地解决了 Redis 分布式方面的需求。当遇到单机内存、并发、流量等瓶颈时，可以采用 Cluster 架构方案达到负载均衡的目的。之前，Redis 分布式方案一般有两种：
1、客户端分区方案，优点是分区逻辑可控，缺点是需要自己处理数据路由、高可用、故障转移等问题。
2、代理方案，优点是简化客户端分布式逻辑和升级维护便利，缺点是加重架构部署复杂度和性能损耗。现在官方为我们提供了专有的集群方案：Redis Cluster，它非常优雅地解决了 Redis 集群方面的问题，因此理解应用好 Redis Cluster 将极大地解放我们使用分布式 Redis 的工作量。

**虚拟槽分区**
Redis 则是利用了虚拟槽分区，可以算上面虚拟一致性哈希分区的变种，它使用分散度良好的哈希函数把所有数据映射到一个固定范围的整数集合中，整数定义为槽( slot )。这个范围一般远大于节点数，比如 Redis Cluster 槽范围是 0~16383 。槽是集群内数据管理和迁移的基本单位。采用大范围槽的主要目的是为了方便数据拆分和集群扩展。每个节点会负责一定数量的槽。
比如集群有 3 个节点，则每个节点平均大约负责 5460 个槽。由于采用高质量的哈希算法，每个槽所映射的数据通常比较均匀，将数据平均划分到 5 个节点进行数据分区 Redis Cluster 就是采用虚拟槽分区下面就介绍 Redis 数据分区方法

**集群功能限制**
Redis 集群相对单机在功能上存在一些限制需要开发人员提前了解，在使用时做好规避。限制如下
1、key 批量操作支持有限。如 mset、mget，目前只支持具有相同slot值的key执行批量操作。对于映射为不同 slot 值的 key 由于执行mget、mget 等操作可能存在于多个节点上因此不被支持
2、key 事务操作支持有限。同理只支持多 key 在同一节点上的事务操作，当多个 key 分布在不同的节点上时无法使用事务功能。
3、key 作为数据分区的最小粒度，因此不能将一个大的键值对象如 hash、list 等映射到不同的节点
4、不支持多数据库空间。单机下的 Redis 可以支持 16 个数据库，集群模式下只能使用一个数据库空间，即 db 0
复制结构只支持一层，从节点只能复制主节点，不支持嵌套树状复制结构。

**搭建集群**
介绍完 Redis 集群分区规则之后，下面我们开始搭建 Redis 集群。搭建集群有几种方式

1、依照 Redis 协议手工搭建，使用cluster meet、cluster addslots、cluster replicate 命令

2、5.0 之前使用由 ruby 语言编写的 redis-trib.rb，在使用前需要安装 ruby 语言环境。

3、5.0 及其之后 redis 摒弃了 redis-trib.rb，将搭建集群的功能合并到了 redis-cli。
我们简单点，采用第三种方式搭建。集群中至少应该有奇数个节点，所以至少有三个节点，官方推荐三主三从的配置方式，我们就来搭建一个三主三从的集群。

**Gossip 消息**
Gossip 协议的主要职责就是信息交换。信息交换的载体就是节点彼此发送的 Gossip 消息，了解这些消息有助于我们理解集群如何完成信息交换
常用的 Gossip 消息可分为：ping 消息、pong 消息、meet消息、fail 消息等 

**meet 消息**
用于通知新节点加入。消息发送者通知接收者加入到当前集群，meet 消息通信正常完成后，接收节点会加入到群中并进行周期性的ping、pong 消息交换。
**ping消息**
集群内交换最频繁的消息，集群内每个节点每秒向多个其他节点发送ping消息用于检测节点是否在线和交换彼止状态信息。ping 消息发送封装了自身节点和部分其他节点的状态数据
**pong消息**
当接收到 ping、meet 消息时，作为响应消息回复给发送方确认消息正常通信。pong 消息内部封装了自身状态据。节点也可以向集群内广播自身的 pong 消息来通知整个集群对自身状态进行更新。
**fail消息**
当节点判定集群内另一个节点下线时，会向集群内广播一个 fail 消息,其他节点接收到 fail 消息之后把对应节点更新为下线状态。

1. Redis Sentinel体量较小时，选择 Redis Sentinel，单主 Redis 足以支撑业务。
2. Redis Cluster Redis 官方提供的集群化方案，体量较大时，选择 Redis Cluster，通过分片，使用更多内存。
3. Twemprox 是 Twtter 开源的一个 Redis和 Memcached 代理服务器，主要用于管理 Redis 和 Memcached 集群，减少与Cache 服务器直接连接的数量。
4. CodisCodis 是一个代理中间件，当客户端向 Codis 发送指令时，Codis 负责将指令转发到后面的 Redis 来执行，并将结果返回给客户端。一个 Codis 实例可以连接多个 Redis 实例，也可以启动多个 Codis 实例来支撑，每个 Codis 节点都是对等的，这样可以增加整体的 QPS 需求，还能起到容灾功能。
5. 客户端分片在Redis Cluster还没出现之前使用较多，现在基本很少热你使用了，在业务代码层实现，起几个毫无关联的Redis实例，在代码层，对 Key 进行 hash 计算，然后去对应的 Redis实例操作数据。这种方式对 hash 层代码要求比较高，考虑部分包括，节点失效后的替代算法方案，数据震荡后的自动脚本恢复，实例的监控，等等。



### 21、Redis 集群方案什么情况下会导致整个集群不可用？

**集群不可用判定**
为了保证集群完整性，默认情况下当集群 16384 个槽任何一个没有指派到节点时整个集群不可用。执行任何键命令返回 ( error ) CLUSTERDOWN Hash slot not served 错误。这是对集群完整性的一种保护措施，保证所有的槽都指派给在线的节点。但是当持有槽的主节点下线时，从故障发现到自动完成转移期间整个集群是不可用状态，对于大多数业务无法容忍这种情况，因此可以将参数 cluster-require-full-coverage 配置为 no，当主节点故障时只影响它负责槽的相关命令执行，不会影响其他主节点的可用性。

但是从集群故障转移的原理来说，集群会出现不可用，当：

1.当访问一个 Master 和 Slave 节点都挂了的槽的时候，cluster-require-full-coverage = yes，会报槽无法获取。

2、集群主库半数宕机（根据 fail over 原理，fail 掉一个主需要一半以上主都投票通过才可以）。

另外，当集群 Master 节点个数小于 3 个的时候，或者集群可用节点个数为偶数的时候，基于 fail 的这种选举机制的自动主从切换过程可能会不能正常工作，一个是标记 fall 的过程，一个是选举新的 master 的过程，都有可能异常。



### 22、说一说 Redis 哈希槽的概念？

**数据分布理论**
分布式数据库首先要解决把整个数据集按照分区规则映射到多个节点的问题，即把数据集划分到多个节点上，每个节点负责整体数据的一个子集。
需要重点关注的是数据分区规则。常见的分区规则有哈希分区和顺序分区两种，哈希分区离散度好、数据分布业务无关、无法顺序访问，顺序分区离散度易倾斜、数据分布业务相关、可顺序访问。

**节点取余分区**
使用特定的数据，如 Redis 的键或用户 ID，再根据节点数量 N 使用公式：hash(key) % N 计算出哈希值，用来决定数据映射到哪一个节点上。这种方案存在一个问题:当节点数量变化时，如扩容或收缩节点，数据节点映射关系需要重新计算，会导致数据的重新迁移
这种方式的突出优点是简单性，常用于数据库的分库分表规则，一般采用预分区的方式，提前根据数据量规划好分区数，比如划分为 512或 1024 张表，保证可支撑未来一段时间的数据量,再根据负载情况将表迁移到其他数据库中。扩容时通常采用翻倍扩容，避免数据映射全部被打乱导致全量迁移的情况,如图 10-2 所示。

**一致性哈希分区**
一致性哈希分区（ Distributed Hash Table ）实现思路是为系统中每个节点分配一个 token，范围一般在 0~23，这些 token 构成一个哈希环。数据读写执行节点查找操作时，先根据 key 计算 hash 值，然后顺时针找到第一个大于等于该哈希值的 token 节点

这种方式相比节点取余最大的好处在于加入和删除节点只影响哈希环中相邻的节点，对其他节点无影响。但一致性哈希分区存在几个问题：
1，当使用少量节点时，节点变化将大范围影响哈希环中数据映射，因此这种方式不适合少量数据节点的分布式方案。
2、增加节点只能对下一个相邻节点有比较好的负载分担效果，对集群中其他的节点基本没有起到负载分担的效果，类似地，删除节点会导致下一个相邻节点负载增加，而其他节点却不能有效分担负载压力。

正因为一致性哈希分区的这些缺点，一些分布式系统采用虚拟槽对一致性哈希进行改进，比如虚拟一致性哈希分区

**虚拟槽分区**
Redis 则是利用了虚拟槽分区，可以算上面虚拟一致性哈希分区的变种，它使用分散度良好的哈希函数把所有数据映射到一个固定范围的整数集合中，整数定义为槽 （slot）。这个范围一般远远大于节点数，比如 Redis Cluster 槽范围是 0~16383 。槽是集群内数据管理和迁移的基本单位。采用大范围槽的主要目的是为了方便数据拆分和集群扩展。每个节点会负责一定数量的槽。
比如集群有 3 个节点，则每个节点平均大约负责 5460 个槽。由于采用高质量的哈希算法，每个槽所映射的数据通常比较均匀，将数据平均划分到 5 个节点进行数据分区。Redis Cluster 就是采用虚拟槽分区下面就介绍 Redis 数据分区方法

slot：称为哈希槽

Redis 集群中内置了 16384 个哈希槽，当需要在 Redis 集群中放置一个 key-value 时，Redis 先对 key 使用 crc 16 算法算出一个结果，然后把结果对 16384 求余数，这样每个 key 都会对应一个编号在 0-16383 之间的哈希槽，Redis 会根据节点数量大致均等的将哈希槽映射到不同的节点。

使用哈希槽的好处就在于可以方便的添加或移除节点。

当需要增加节点时，只需要把其他节点的某些哈希槽挪到新节点就可以了；

当需要移除节点时，只需要把移除节点上的哈希槽挪到其他节点就行了；



### 23、Redis 集群会有写操作丢失吗？为什么？

Redis 并不能保证数据的强一致性，所以在特定条件下可能会丢失数据，比如 

1、最大内存不足（再次写入返回报错） 

2、未开启持久化的 master 故障重启，slave 再次同步 

3、网络波动，哨兵主动选举，主从切换的期间



以下情况可能导致写操作丢失：

过期 key 被清理

最大内存不足，导致 Redis 自动清理部分 key 以节省空间

主库故障后自动重启，从库自动同步

单独的主备方案，网络不稳定触发哨兵的自动切换主从节点，切换期间会有数据丢失



### 24、Redis 常见性能问题和解决方案有哪些？

1、持久化性能问题

全量复制、部分复制（一台机器）

主从 主不做持久化，从节点做持久化

2、数据比较重要，AOF 持久化 slave 开启 AOF，策略每秒同步

3、主从复制 流畅，同一个局域网

4、尽量减少主库压力，增加从库

5、主从复制 不要采用网状结构，可用线性结构减轻主库压力

一、缓存穿透：就是查询一个压根就不存在的数据，即缓存中没有，数据库中也没有

解决方案：使用布隆过滤器，把数据先加载到布隆过滤器中，访问前先判断是否存在于布隆过滤器中，不存在代表这笔数据压根就不存在。

缺点：布隆过滤器是不可变的，可能一开始过滤器和数据库数据时一致的，后面数据库数据变了，或变多或变少，而对应的布隆过滤器的数据也要改变，这时会比较麻烦。

二、缓存击穿：数据库中有，缓存中没有。缓存击穿实际就是一个并发问题，一般来说查询数据，先查询缓存，有直接返回，没有再查询数据库并放到缓存中之后返回，但这种场景在并发情况下就会有问题，假设同时又 100 个请求执行上面

逻辑的代码，则可能会出现多个请求都查询数据库，因为大家同时执行，都查到了缓存中没有数据。

解决方案：加锁。如果是单机部署，则可以使用 JVM 级别的锁，如 lock、synchronized。如果是集群部署，则需要使用分布式锁，如基于Redis、zookeeper、MySQL 等实现的分布式锁。

三、缓存雪崩：大部分数据同时失效、过期，新的缓存又没来，导致大量的请求都去访问数据库而导致的服务器压力过大、宕机、系统崩溃。

解决方案：搭建高可用的 Redis 集群，避免压力集中于一个节点；缓存失效时间错开，避免缓存同时失效而都去请求数据库。



### 25、热点数据和冷数据是什么

对于冷数据而言，大部分数据可能还没有再次访问到就已经被挤出内存，不仅占用内存，而且价值不大。频繁修改的数据，看情况考虑使用缓存。例如寿星列表、导航信息都存在一个特点，就是信息修改频率不高，读取通常非常高的场景。

对于热点数据，比如我们的某 IM 产品，生日祝福模块，当天的寿星列表，缓存以后可能读取数十万次。 再举个例子，某导航产品，我们将导航信息，缓存以后可能读取数百万次。Redis 更新之前至少读取两次才放缓存



### 26、什么情况下可能会导致 Redis 阻塞？

1、客户端阻塞 命令 keys *、Hgetall、smembers 时间复杂度 O(n)

2、bigkey 删除 ZSet（100 万的元素，删除 2 s ）

3、清空库 flushdb、flushall

4、AOF 日志同步写 一个同步写磁盘耗时 1~2 ms

5、从库 加载 RDB 文件

数据集中过期

不合理地使用 API 或数据结构

CPU 饱和

持久化阻塞



### 27、什么时候选择 Redis，什么时候选择 Memcached？

实际业务分析

如果业务中更加侧重性能的⾼效性，对持久化要求不⾼，那么应该优先选择 Memcached。

如果业务中对持久化有需求或者对数据涉及到存储、排序等一系列复杂的操作，比如业务中有排⾏榜类应⽤、社交关系存储、数据排重、实时配置等功能，那么应该优先选择 Redis



### 28、Redis 过期策略都有哪些？LRU 算法知道吗？

1. noeviction：返回错误当内存限制达到，并且客户端尝试执行会让更多内存被使用的命令。

2. allkeys-lru： 尝试回收最少使用的键（LRU），使得新添加的数据有空间存放。

3. volatile-lru：尝试回收最少使用的键（LRU），但仅限于在过期集合的键，使得新添加的数据有空间存放。

4. allkeys-random：回收随机的键使得新添加的数据有空间存放。

5. volatile-random：回收随机的键使得新添加的数据有空间存放，但仅限于在过期集合的键。

6. volatile-ttl：回收在过期集合的键，并且优先回收存活时间（TTL）较短的键，使得新添加的数据有空间存放。

**LRU 算法**

实现 LRU 算法除了需要 key / value 字典外，还需要附加一个链表，链表的元素按照一定的顺序进行排列。当空间满的时候，会踢掉链表尾部的元素。当字典的某个元素被访问时，它在链表中的位置会被移动到表头。所以链表的元素排列顺序就是元素最近被访问的时间顺序。

**近似 LRU 算法**

**LFU 算法**

#### Redis 缓存刷新策略有哪些？

![](https://dream-syz.github.io/11-2.png)





### 29、说说 Redis 的线程模型

```
这问题是因为前面回答问题的时候提到了 Redis 是基于非阻塞的 IO 复用模型。如果这个问题回答不上来，就相当于前面的回答是给自己挖坑，因为你答不上来，面试官对你的印象可能就要打点折扣了。
```

Redis 内部使用 **文件事件处理器**  file event handler ，这个文件事件处理器是 **单线程的**，所以 Redis 才叫做单线程的模型。它采用 IO 多路复用机制同时监听多个 socket ，根据 socket 上的事件来选择对应的事件处理器进行处理。

文件事件处理器的结构包含 4 个部分：

1. 多个 socket 。
2. IO 多路复用程序。
3. 文件事件分派器。
4. 事件处理器（连接应答处理器、命令请求处理器、命令回复处理器）。

多个 socket 可能会并发产生不同的操作，每个操作对应不同的文件事件，但是 IO 多路复用程序会监听多个 socket，会将 socket 产生的事件放入队列中排队，事件分派器每次从队列中取出一个事件，把该事件交给对应的事件处理器进行处理。

来看客户端与 Redis 的一次通信过程：

![](https://dream-syz.github.io/11-1.png)

下面来大致说一下这个图：

1. 客户端 Socket01 向 Redis 的 Server Socket 请求建立连接，此时 Server Socket 会产生一个 AE_READABLE 事件，IO 多路复用程序监听到 server socket 产生的事件后，将该事件压入队列中。文件事件分派器从队列中获取该事件，交给连接应答处理器。连接应答处理器会创建一个能与客户端通信的 Socket 01，并将该 Socket 01 的 AE_READABLE 事件与命令请求处理器关联。

2. 假设此时客户端发送了一个 set key value 请求，此时 Redis 中的 Socket 01 会产生 AE_READABLE 事件，IO 多路复用程序将事件压入队列，此时事件分派器从队列中获取到该事件，由于前面 Socket 01 的 AE_READABLE 事件已经与命令请求处理器关联，因此事件分派器将事件交给命令请求处理器来处理。命令请求处理器读取 Socket 01 的 set key value 并在自己内存中完成 set key value 的设置。操作完成后，它会将 Socket01 的 AE_WRITABLE 事件与令回复处理器关联。

3. 如果此时客户端准备好接收返回结果了，那么 Redis 中的 Socket 01 会产生一个 AE_WRITABLE 事件，同样压入队列中，事件分派器找到相关联的命令回复处理器，由命令回复处理器对 Socket 01 输入本次操作的一个结果，比如 ok ，之后解除 Socket 01 的AE_WRITABLE 事件与命令回复处理器的关联。

这样便完成了一次通信。 不要怕这段文字，结合图看，一遍不行两遍，实在不行可以网上查点资料结合着看，一定要搞清楚，否则前面吹的牛逼就白费了。



### 30、为什么 Redis 需要把所有数据放到内存中？

Redis 将数据放在内存中有一个好处，那就是可以实现最快的对数据读取，如果数据存储在硬盘中，磁盘 I / O 会严重影响 Redis 的性能。而且 Redis 还提供了数据持久化功能，不用担心服务器重启对内存中数据的影响。其次现在硬件越来越便宜的情况下，Redis 的使用也被应用得越来越多，使得它拥有很大的优势。



### 31、Redis 的同步机制了解是什么？

Redis 支持主从同步、从从同步。如果是第一次进行主从同步，主节点需要使用 bgsave 命令，再将后续修改操作记录到内存的缓冲区，等 RDB 文件全部同步到复制节点，复制节点接受完成后将 RDB 镜像记载到内存中。等加载完成后，复制节点通知主节点将复制期间修改的操作记录同步到复制节点，即可完成同步过程。



### 32、 pipeline 有什么好处，为什么要用 pipeline？

使用 pipeline（管道）的好处在于可以将多次 I / O 往返的时间缩短为一次，但是要求管道中执行的指令间没有因果关系。

用 pipeline 的原因在于可以实现请求/响应服务器的功能，当客户端尚未读取旧响应时，它也可以处理新的请求。如果客户端存在多个命令发送到服务器时，那么客户端无需等待服务端的每次响应才能执行下个命令，只需最后一步从服务端读取回复即可。



 







## 分布式篇

### 1、分布式幂等性如何设计？

在高并发场景的架构里，幂等性是必须得保证的。比如说支付功能，用户发起支付，如果后台没有做幂等校验，刚好用户手抖多点了几下，于是后台就可能多次受到同一个订单请求，不做幂等很容易就让用户重复支付了，这样用户是肯定不能忍的。

**解决方案：**

1、查询和删除不在幂等讨论范围，查询肯定没有幂等的说，删除：第一次删除成功后，后面来删除直接返回 0，也是返回成功。

2、建唯一索引：唯一索引或唯一组合索引来防止新增数据存在脏数据 （当表存在唯一索引，并发时新增异常时，再查询一次就可以了，数据应该已经存在了，返回结果即可）。

3、token机制：由于重复点击或者网络重发，或者 nginx 重发等情况会导致数据被重复提交。前端在数据提交前要向后端服务的申请 token，token 放到 Redis 或 JVM 内存，token 有效时间。提交后后台校验 token，同时删除 token，生成新的 token 返回。redis 要用删除操作来判断 token，删除成功代表 token 校验通过，如果用 select + delete 来校验 token，存在并发问题，不建议使用。

4、悲观锁：悲观锁使用时一般伴随事务一起使用，数据锁定时间可能会很长，根据实际情况选用（另外还要考虑 id 是否为主键，如果 id不是主键或者不是 InnoDB 存储引擎，那么就会出现锁全表）。for update

5、乐观锁，给数据库表增加一个 version 字段，可以通过这个字段来判断是否已经被修改了

6、分布式锁，比如 Redis 、 Zookeeper 的分布式锁。单号为 key，然后给 Key 设置有效期（防止支付失败后，锁一直不释放），来一个请求使用订单号生成一把锁，业务代码执行完成后再释放锁。

7、保底方案，先查询是否存在此单，不存在进行支付，存在就直接返回支付结果。



### 2、说说你对分布式事务的理解？

**场景**：多个服务或者多个库，要保持在一个事务中。

分布式事务是企业集成中的一个技术难点，也是每一个分布式系统架构中都会涉及到的一个东西，特别是在微服务架构中，几乎可以说是无法避免。

**理论**：ACID、CAP、BASE。

**ACID**

指数据库事务正确执行的四个基本要素：

1. 原子性（Atomicity）
2. 一致性（Consistency）
3. 隔离性（Isolation）
4. 持久性（Durability）

**CAP：cp，ap。**

CAP 原则又称 CAP 定理，指的是在一个分布式系统中，一致性（Consistency）、可用性（Availability）、分区容忍性（Partition tolerance）。CAP 原则指的是，这三个要素最多只能同时实现两点，不可能三者兼顾。

- 一致性：在分布式系统中的所有数据备份，在同一时刻是否同样的值。

- 可用性：在集群中一部分节点故障后，集群整体是否还能响应客户端的读写请求。

- 分区容忍性：以实际效果而言，分区相当于对通信的时限要求。系统如果不能在时限内达成数据一致性，就意味着发生了分区的情况，必须就当前操作在 C 和 A 之间做出选择。

**BASE 理论**

BASE 理论是对 CAP 中的一致性和可用性进行一个权衡的结果，理论的核心思想就是：我们无法做到强一致，但每个应用都可以根据自身的业务特点，采用适当的方式来使系统达到最终一致性。

Basically Available（基本可用）

Soft state（软状态）

Eventually consistent（最终一致性）

**解决方案**：seata，消息队列 + 本地事件表，事务消息，最大努力通知方案，tcc



### 3、什么是两阶段提交？

两阶段提交 2 PC 是分布式事务中最强大的事务类型之一

**流程：**

两段提交就是分两个阶段提交：

**第一阶段** 询问各个事务数据源是否准备好，投票阶段。

**第二阶段** 才真正将数据提交给事务数据源。

为了保证该事务可以满足 ACID，就要引入一个协调者（Cooradinator）。其他的节点被称为参与者（Participant）。协调者负责调度参与者的行为，并最终决定这些参与者是否要把事务进行提交。

处理流程如下：

阶段一

a) 协调者向所有参与者发送事务内容，询问是否可以提交事务，并等待答复。

b) 各参与者执行事务操作，将 undo 和 redo 信息记入事务日志中（但不提交事务）。

c) 如参与者执行成功，给协调者反馈 yes，否则反馈 no。

阶段二

如果协调者收到了参与者的失败消息或者超时，直接给每个参与者发送回滚(rollback)消息；否则，发送提交(commit)消息。

两种情况处理如下：

情况1：当所有参与者均反馈 yes，提交事务

a) 协调者向所有参与者发出正式提交事务的请求（即 commit 请求）。

b) 参与者执行 commit 请求，并释放整个事务期间占用的资源。

c) 各参与者向协调者反馈 ack(应答)完成的消息。

d) 协调者收到所有参与者反馈的 ack 消息后，即完成事务提交。

情况2：当有一个参与者反馈 no，回滚事务

a) 协调者向所有参与者发出回滚请求（即 rollback 请求）。

b) 参与者使用阶段 1 中的 undo 信息执行回滚操作，并释放整个事务期间占用的资源。

c) 各参与者向协调者反馈 ack 完成的消息。

d) 协调者收到所有参与者反馈的 ack 消息后，即完成事务。

**问题：# **Java 面试题整理**



## **基础篇**

### **1**、 **Java** 语言有哪些特点

1、简单易学、有丰富的类库

2、面向对象（ Java 最重要的特性，让程序耦合度更低，内聚性更高）

3、与平台无关性（ JVM 是 Java 跨平台使用的根本）

4、可靠安全

5、支持多线程



### **2**、面向对象和面向过程的区别

**面向过程**：是分析解决问题的步骤，然后用函数把这些步骤一步一步地实现，然后在使用的时候一一调用则可。性能较高，所以单片机、嵌入式开发等一般采用面向过程开发

**面向对象**：是把构成问题的事务分解成各个对象，而建立对象的目的也不是为了完成一个个步骤，而是为了描述某个事物在解决整个问题的过程中所发生的行为。面向对象有**封装、继承、多态**的特性，所以易维护、易复用、易扩展。可以设计出低耦合的系统。 但是性能上来说，比面向过程要低。



### **3** **、八种基本数据类型的大小，以及他们的封装类**

| **基本类型** | **大小（字节）** | **默认值**       | **封装类** |
| ------------ | ---------------- | ---------------- | ---------- |
| byte         | 1                | （ byte ）0      | Byte       |
| short        | 2                | （ short ）0     | Short      |
| int          | 4                | 0                | Integer    |
| long         | 8                | 0 L              | Long       |
| float        | 4                | 0.0 f            | Float      |
| double       | 8                | 0.0 d            | Double     |
| boolean      | -                | false            | Boolean    |
| char         | 2                | \u0000（ null ） | Character  |

注：

- int 是基本数据类型，Integer 是 int 的封装类，是引用类型。int 默认值是0，而 Integer 默认值是null，所以Integer能区分出0和null的情况。一旦java看到null，就知道这个引用还没有指向某个对象，再任何引用使用前，必须为其指定一个对象，否则会报错。

- 基本数据类型在声明时系统会自动给它分配空间，而引用类型声明时只是分配了引用空间，必须通过实例化开辟数据空间之后才可以赋值。数组对象也是一个引用对象，将一个数组赋值给另一个数组时只是复制了一个引用，所以通过某一个数组所做的修改在另一个数组中也看的见。

虽然定义了 boolean 这种数据类型，但是只对它提供了非常有限的支持。在 Java 虚拟机中没有任何供 boolean 值专用的字节码指令，Java 语言表达式所操作的 boolean 值，在编译之后都使用 Java 虚拟机中的 int 数据类型来代替，而 boolean 数组将会被编码成 Java 虚拟机的 byte 数组，每个元素 boolean 元素占 8 位。这样我们可以得出 boolean 类型占了单独使用是 4 个字节，在数组中又是 1 个字

节。使用 int 的原因是，对于当下 32 位的处理器（ CPU ）来说，一次处理数据是 32 位（这里不是指的是 32 / 64 位系统，而是指 CPU 硬件层面），具有高效存取的特点。



### **4**、标识符的命名规则

**标识符的含义：** 是指在程序中，我们自己定义的内容，譬如，类的名字，方法名称以及变量名称等等，都是标识符。

**命名规则：（硬性要求）** 标识符可以包含英文字母，0 - 9 的数字，$ 以及_ 标识符不能以数字开头 标识符不是关键字

**命名规范：（非硬性要求）** 类名规范：首字符大写，后面每个单词首字母大写（大驼峰式）。 变量名规范：首字母小写，后面每个单词首字母大写（小驼峰式）。 方法名规范：同变量名。



### **5**、instanceof **关键字的作用**

instanceof  严格来说是 Java 中的一个双目运算符，用来测试一个对象是否为一个类的实例，用法为：

```java
boolean result = obj instanceof Class
```

其中 obj 为一个对象，Class 表示一个类或者一个接口，当 obj 为 Class 的对象，或者是其直接或间接子类，或者是其接口的实现类，结果 result 都返回 true，否则返回 false 。

注意：编译器会检查 obj 是否能转换成右边的 class 类型，如果不能转换则直接报错，如果不能确定类型，则通过编译，具体看运行时定。

```java
int i = 0;
System.out.println(i instanceof Integer);//编译不通过 i 必须是引用类型，不能是基本类型
System.out.println(i instanceof Object);//编译不通过
```

```java
Integer integer = new Integer(1);
System.out.println(integer instanceof Integer);//true
```

```java
//false ,在 JavaSE 规范 中对 instanceof 运算符的规定就是：如果 obj 为 null，那么将返回 false。
System.out.println(null instanceof Object);
```



### **6**、Java 自动装箱与拆箱

**装箱就是自动将基本数据类型转换为包装器类型（ int --> Integer ），调用方法：Integer 的 valueOf (int) 方法**

**拆箱就是自动将包装器类型转换为基本数据类型（ Integer --> int），调用方法：Integer 的 intValue 方法**

在 Java SE5 之前，如果要生成一个数值为 10 的 Integer 对象，必须这样进行：

```java
Integer i = new Integer(10);
```

而在从 Java SE5 开始就提供了自动装箱的特性，如果要生成一个数值为 10 的 Integer 对象，只需要这样就可以了：

```java
Integer i = 10;
```

**面试题 1 ： 以下代码会输出什么？**

```java
public class Main {
 	public static void main(String[] args) {
 
 	Integer i1 = 100;
 	Integer i2 = 100;
	Integer i3 = 200;
 	Integer i4 = 200;
 
 	System.out.println(i1==i2);
 	System.out.println(i3==i4);
 	}
}
```

运行结果：

```java
true
false
```

为什么会出现这样的结果？输出结果表明 i1 和 i2 指向的是同一个对象，而 i3 和 i4 指向的是不同的对

象。此时只需一看源码便知究竟，下面这段代码是 Integer 的 valueOf 方法的具体实现：

```java
public static Integer valueOf(int i) {
 	if(i >= -128 && i <= IntegerCache.high)
 	return IntegerCache.cache[i + 128];
 	else
 	return new Integer(i);
 }
```

其中 IntegerCache 类的实现为：

```java
private static class IntegerCache {
 	static final int high;
 	static final Integer cache[];
	static {
    final int low = -128;
 	// high value may be configured by property
 	int h = 127;
 	if (integerCacheHighPropValue != null) {
 	// Use Long.decode here to avoid invoking methods that
 	// require Integer's autoboxing cache to be initialized
 	int i = Long.decode(integerCacheHighPropValue).intValue();
 	i = Math.max(i, 127);
 	// Maximum array size is Integer.MAX_VALUE
	h = Math.min(i, Integer.MAX_VALUE - -low);
 	}
 	high = h;
 	cache = new Integer[(high - low) + 1];
 	int j = low;
 	for(int k = 0; k < cache.length; k++)
 	cache[k] = new Integer(j++);
 	}
 	private IntegerCache() {}
 }
```

从这2段代码可以看出，在通过 valueOf 方法创建 Integer 对象的时候，如果数值在 [-128,127] 之间，便返回指向 IntegerCache.cache 中已经存在的对象的引用；否则创建一个新的 Integer 对象。

上面的代码中 i1 和 i2 的数值为 100 ，因此会直接从 cache 中取已经存在的对象，所以 i1 和 i2 指向的是同一个对象，而 i3 和 i4 则是分别指向不同的对象。

**面试题 2 ：以下代码输出什么运行结果：**

```java
public class Main {
 	public static void main(String[] args) {
 
 	Double i1 = 100.0;
 	Double i2 = 100.0;
 	Double i3 = 200.0;
 	Double i4 = 200.0;
 
 	System.out.println(i1==i2);
 	System.out.println(i3==i4);
 	}
}
```

运行结果：

```java
false
false
```

原因： 在某个范围内的整型数值的个数是有限的，而浮点数却不是。



### **7**、 重载和重写的区别

**重写 (Override)**

从字面上看，重写就是重新写一遍的意思。其实就是在子类中把父类本身有的方法重新写一遍。子类继承了父类原有的方法，但有时子类并不想原封不动的继承父类中的某个方法，所以在方法名，参数列表，返回类型(除过子类中方法的返回值是父类中方法返回值的子类时)都相同的情况下， 对方法体进行修改或重写，这就是重写。但要注意子类函数的访问修饰权限不能少于父类的。

```java
public class Father {
 	public static void main(String[] args) {
 	// TODO Auto-generated method stub
 	Son s = new Son();
 	s.sayHello();
 	}
 	public void sayHello() {
 	System.out.println("Hello");
 	}
}
class Son extends Father{
	@Override
 	public void sayHello() {
 	// TODO Auto-generated method stub
 	System.out.println("hello by ");
 	}
}
```

**重写 总结：** 1.发生在父类与子类之间 2.方法名，参数列表，返回类型（除过子类中方法的返回类型是父类中返回类型的子类）必须相同 3.访问修饰符的限制一定要大于被重写方法的访问修饰符（ public > protected > default > private ) 4.重写方法一定不能抛出新的检查异常或者比被重写方法申明更加宽泛的检查型异常

**重载（ Overload ）**

在一个类中，同名的方法如果有不同的参数列表（**参数类型不同、参数个数不同甚至是参数顺序不同**）则视为重载。同时，重载对返回类型没有要求，可以相同也可以不同，但**不能通过返回类型是否相同来判断重载**。

```java
public class Father {
 	public static void main(String[] args) {
 		// TODO Auto-generated method stub
 		Father s = new Father();
 		s.sayHello();
 		s.sayHello("wintershii");
 	}
 	public void sayHello() {
 		System.out.println("Hello");
 	}
 	public void sayHello(String name) {
 		System.out.println("Hello" + " " + name);
 	}
}
```

**重载 总结：** 1.重载 Overload 是一个类中多态性的一种表现 2.重载要求同名方法的参数列表不同(参数类型，参数个数甚至是参数顺序) 3.重载的时候，返回值类型可以相同也可以不相同。无法以返回型别作为重载函数的区分标准

### 8、equals 与 == 的区别

**==** **：**

== 比较的是变量(栈)内存中存放的对象的(堆)内存地址，用来判断两个对象的地址是否相同，即是否是指相同一个对象。比较的是真正意义上的指针操作。

1、比较的是操作符两端的操作数是否是同一个对象。 2、两边的操作数必须是同一类型的（可以是父子类之间）才能编译通过。 3、比较的是地址，如果是具体的阿拉伯数字的比较，值相等则为 true，如： int a = 10 与 long b = 10 L 与 double c = 10.0 都是相同的（为true），因为他们都指向地址为 10 的堆。

**equals**：

equals用来比较的是两个对象的内容是否相等，由于所有的类都是继承自 java.lang.Object 类的，所以适用于所有对象，如果没有对该方法进行覆盖的话，调用的仍然是 Object 类中的方法，而 Object 中的 equals 方法返回的却是 == 的判断。

总结：

所有比较是否相等时，都是用 equals 并且在对常量相比较时，把常量写在前面，因为使用 object 的 equals object 可能为 null 则空指针

在阿里的代码规范中只使用 equals，阿里插件默认会识别，并可以快速修改，推荐安装阿里插件来排查老代码使用 “==” ，替换成 equals



### **9、Hashcode 的作用**

java 的集合有两类，一类是 List ，还有一类是 Set 。前者有序可重复，后者无序不重复。当我们在 set 中插入的时候怎么判断是否已经存在该元素呢，可以通过 equals 方法。但是如果元素太多，用这样的方法就会比较满。

于是有人发明了哈希算法来提高集合中查找元素的效率。 这种方式将集合分成若干个存储区域，每个对象可以计算出一个哈希码，可以将哈希码分组，每组分别对应某个存储区域，根据一个对象的哈希码就可以确定该对象应该存储的那个区域。

hashCode 方法可以这样理解：它返回的就是根据对象的内存地址换算出的一个值。这样一来，当集合要添加新的元素时，先调用这个元素的 hashCode 方法，就一下子能定位到它应该放置的物理位置上。如果这个位置上没有元素，它就可以直接存储在这个位置上，不用再进行任何比较了；如果这个位置上已经有元素了，就调用它的 equals 方法与新元素进行比较，相同的话就不存了，不相同就散列其它的地址。这样一来实际调用 equals 方法的次数就大大降低了，几乎只需要一两次。



### 10、String、StringBuffer 和 StringBuilder 的区别是什么?

String 是只读字符串，它并不是基本数据类型，而是一个对象。从底层源码来看是一个 final 类型的字符数组，所引用的字符串不能被改变，一经定义，无法再增删改。每次对 String 的操作都会生成新的 String 对象。

```java
private final char value[];//dk1.8 及以前 String 底层使用的是 char 数组
private final byte[] value;//jdk 1.9 及以后使用的是 byte 数组
```

**jdk1.8 及以前 String 底层使用的是 char 数组，jdk 1.9 及以后使用的是 byte 数组。**

每次 + 操作 ： 隐式在堆上 new 了一个跟原字符串相同的 StringBuilder 对象，再调用 append 方法拼接 + 后面的字符。

StringBuffer 和 StringBuilder 他们两都继承了 AbstractStringBuilder 抽象类，从 AbstractStringBuilder 抽象类中我们可以看到

```java
/**
* 基于1.8
* The value is used for character storage.
*/
char[] value;
```

他们的底层都是可变的字符数组，所以在进行频繁的字符串操作时，建议使用 StringBuffer 和 StringBuilder 来进行操作。 另外StringBuffer 对方法加了同步锁或者对调用的方法加了同步锁，所以是线程安全的。StringBuilder 并没有对方法进行加同步锁，所以是非线程安全的。



### 11、ArrayList 和 linkedList 的区别

**ArrayList**

- **优点**：ArrayList 是实现了基于动态数组的数据结构，因为地址连续，一旦数据存储好了，查询操作效率会比较高（在内存里是连着放的）。

- **缺点**：因为地址连续，ArrayList 要移动数据，所以插入和删除操作效率比较低。

**LinkedList**

- **优点**：LinkedList 基于链表的数据结构，地址是任意的，所以在开辟内存空间的时候不需要等一个连续的地址。对于新增和删除操作，LinkedList 比较占优势。LinkedList 适用于要头尾操作或插入指定位置的场景。

- **缺点**：因为 LinkedList 要移动指针，所以查询操作性能比较低。

**适用场景分析**

- 当需要对数据进行对随机访问的时候，选用 ArrayList。

- 当需要对数据进行多次增加删除修改时，采用 LinkedList。

如果容量固定，并且只会添加到尾部，不会引起扩容，优先采用 ArrayList。

当然，绝大数业务的场景下，使用 ArrayList 就够了，但需要注意避免 ArrayList 的扩容，以及非顺序的插入。



 **Array （数组）是基于索引（index）的数据结构，它使用索引在数组中搜索和读取数据是很快的。**

Array 获取数据的时间复杂度是 O ( 1 ) ,但是要删除数据却是开销很大，因为这需要重排数组中的所有数据（因为删除数据以后, 需要把后面所有的数据前移）

**缺点：**数组初始化必须指定初始化的长度, 否则报错

例如：

```java
int[] a = new int[4];//推介使用int[] 这种方式初始化
int c[] = {23,43,56,78};//长度：4，索引范围：[0,3]
```

**List 是一个有序的集合，可以包含重复的元素，提供了按索引访问的方式，它继承 Collection。**

**List有两个重要的实现类：ArrayList 和 LinkedList**

**ArrayList:** **可以看作是能够自动增长容量的数组**

**ArrayList 的 toArray 方法返回一个数组**

**ArrayList 的 asList 方法返回一个列表**

ArrayList底层的实现是Array, 数组扩容实现

**LinkList 是一个双链表 ，在添加和删除元素时具有比 ArrayList 更好的性能；但在 get 与 set 方面弱于ArrayList。当然，这些对比都是指数据量很大或者操作很频繁。**



### 12、HashMap 和 HashTable 的区别

HashTable 是 java 一开始发布时就提供的键值映射的数据结构，而 HashMap 产生于 JDK 1.2。虽然 HashTable 比 HashMap 出现的早一些，**但是现在 HashTable 基本上已经被弃用了**。而 HashMap 已经成为应用最为广泛的一种数据类型了。

#### 1、两者父类不同

HashMap 是继承自 AbstractMap 类，而 HashTable 是继承自 Dictionary 类。不过它们都实现了同时

实现了 map、Cloneable（可复制）、Serializable（可序列化）这三个接口。

#### 2、对外提供的接口不同

HashTable 比 HashMap 多提供了 elments() 和 contains() 两个方法。elments() 方法继承自

HashTable 的父类 Dictionnary。elements() 方法用于返回此 HashTable 中的 value 的枚举。

contains() 方法判断该 HashTable 是否包含传入的 value。它的作用与 containsValue() 一致。事实

上，contansValue() 就只是调用了一下 contains() 方法。

#### 3、对 null 的支持不同

Hashtable：key 和 value 都不能为 null。

HashMap：key 可以为 null，但是这样的 key 只能有一个，因为必须保证 key 的唯一性；可以有多个

key值对应的 value 为 null。

#### 4、安全性不同

HashMap 是线程不安全的，在多线程并发的环境下，可能会产生死锁等问题，因此需要开发人员自

己处理多线程的安全问题。

Hashtable是线程安全的，它的每个方法上都有 synchronized  关键字，因此可直接用于多线程中。

虽然 HashMap是线程不安全的，但是它的效率远远高于 Hashtable，这样设计是合理的，因为大部

分的使用场景都是单线程。当需要多线程操作的时候可以使用线程安全的 ConcurrentHashMap 。

ConcurrentHashMap 虽然也是线程安全的，但是它的效率比Hashtable要高好多倍。因为

ConcurrentHashMap 使用了分段锁，并不对整个数据进行锁定。



**第二种答案：**

1. 出生的版本不一样，Hashtable 出生于 Java 发布的第一版本 JDK 1.0，HashMap 出生于 JDK 1.2。

2. 都实现了 Map、Cloneable、Serializable（ 当前 JDK 版本 1.8 ）。
3. HashMap 继承的是 AbstractMap，并且 AbstractMap 也实现了 Map 接口。Hashtable 继承 Dictionary。

4. Hashtable 中大部分 public 修饰普通方法都是 synchronized 字段修饰的，是线程安全的，HashMap 是非线程安全的。

5. Hashtable 的 key 不能为 null，value 也不能为 null，这个可以从 Hashtable 源码中的 put 方法看到，判断如果 value 为 null 就直接抛出空指针异常，在 put 方法中计算 key 的 hash 值之前并没有判断 key 为 null 的情况，那说明，这时候如果 key 为空，照样会抛出空指针异常。

6. HashMap 的 key 和 value 都可以为 null。在计算 hash 值的时候，有判断，如果 key==null ，则其 hash=0 ；至于 value 是否为 null，根本没有判断过。

7. Hashtable 直接使用对象的 hash 值。hash 值是 JDK 根据对象的地址或者字符串或者数字算出来的 int 类型的数值。然后再使用除留余数法来获得最终的位置。然而除法运算是非常耗费时间的，效率很低。HashMap 为了提高计算效率，将哈希表的大小固定为了 2 的幂，这样在取模预算时，不需要做除法，只需要做位运算。位运算比除法的效率要高很多。

8. Hashtable、HashMap 都使用了 Iterator。而由于历史原因，Hashtable 还使用了 Enumeration 的方式。

9. 默认情况下，初始容量不同，Hashtable 的初始长度是 11，之后每次扩充容量变为之前的 2n+1（n 为上一次的长度）而 HashMap 的初始长度为 16，之后每次扩充变为原来的两倍。

   另外在 Hashtable 源码注释中有这么一句话：

   ```
   Hashtable is synchronized. If a thread-safe implementation is not needed, it is
   recommended to use HashMap in place of Hashtable . If a thread-safe highlyconcurrent implementation is desired, then it is recommended to use
   ConcurrentHashMap in place of Hashtable.
   ```

大致意思：Hashtable 是线程安全，推荐使用 HashMap 代替 Hashtable；如果需要线程安全高并发的话，推荐使用 ConcurrentHashMap 代替 Hashtable。

这个回答完了，面试官可能会继续问：HashMap 是线程不安全的，那么在需要线程安全的情况下还要考虑性能，有什么解决方式？

这里最好的选择就是 ConcurrentHashMap 了，但面试官肯定会叫你继续说一下 ConcurrentHashMap 数据结构以及底层原理等。



**拓展：什么是分段锁？**

ConcurrentHashMap 中的分段锁称为 Segment，它的内部结构是维护一个 HashEntry 数组，同时 Segment 还继承了 ReentrantLock。

当需要 put 元素的时候，并不是对整个 ConcurrentHashMap 进行加锁，而是先通过 hashcode 来判断它放在哪一个分段中，然后对该分段进行加锁。所以当多线程 put 的时候，只要不是放在同一个分段中，就可以实现并行的插入。分段锁的设计目的就是为了细化锁的粒度，从而提高并发能力。

jdk 1.8 中的 ConcurrentHashMap 中废弃了 Segment 锁，直接使用了数组元素，数组中的每个元素都可以作为一个锁。在元素中没有值的情况下，可以直接通过 CAS 操作来设值，同时保证并发安全；如果元素里面已经存在值的话，那么就使用 synchronized 关键字对元素加锁，再进行之后的 hash 冲突处理。jdk1.8 的 ConcurrentHashMap 加锁粒度比 jdk 1.7 里的 Segment 来加锁粒度更细，并发性能更好。

#### 5、初始容量大小和每次扩充容量大小不同

HashMap 的初始容量为：16，Hashtable 初始容量为：11，两者的负载因子默认都是：0.75

- HashMap 每次扩充，容量变为原来的 2 倍（ 2 n ）；
- HashTable 每次扩充，容量会变为原来的 2 倍 + 1（ 2 n + 1 ）；

下面给出HashMap中的源码：

```java
    /**
     * The default initial capacity - MUST be a power of two.
     */
    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4; // aka 16

    /**
     * The maximum capacity, used if a higher value is implicitly specified
     * by either of the constructors with arguments.
     * MUST be a power of two <= 1<<30.
     */
    static final int MAXIMUM_CAPACITY = 1 << 30;

    /**
     * The load factor used when none specified in constructor.
     */
    static final float DEFAULT_LOAD_FACTOR = 0.75f;
```

**拓展：什么是负载因子？**

负载因子 loadFactor = 哈希表的有效元素个数 / 哈希表长度

这个值越大就说明冲突越严重一些
这个值越小说明冲突越小，数组利用率越低
扩容：采用整表扩容的方式什么时候需要对数组扩容？

扩容与否就根据负载因子来决定，当数组长度 * 负载因子 <=  有效元素个数就需要扩容

HashMap中的源码：

```
    /**
     * Returns a power of two size for the given target capacity.
     */
    static final int tableSizeFor(int cap) {
        int n = cap - 1;
        n |= n >>> 1;
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
```



#### 6、计算 hash 值的方法不同

为了得到元素的位置，首先需要根据元素的 KEY 计算出一个hash值，然后再用这个 hash 值来计算得到最终的位置。

Hashtable 中 hash 的计算方法为：直接使用对象的 hashCode()。
HashMap 中 hash 的计算方法为：key 的 hash 值高 16 位不变，低 16 位与高 16 位异或作为 key 最终的 hash 值。（h>>>16，表示无符号右移 16 位，高位补 0 ）

**HashTable：**

```java
    /**
     * Maps the specified <code>key</code> to the specified
     * <code>value</code> in this hashtable. Neither the key nor the
     * value can be <code>null</code>. <p>
     *
     * The value can be retrieved by calling the <code>get</code> method
     * with a key that is equal to the original key.
     *
     * @param      key     the hashtable key
     * @param      value   the value
     * @return     the previous value of the specified key in this hashtable,
     *             or <code>null</code> if it did not have one
     * @exception  NullPointerException  if the key or value is
     *               <code>null</code>
     * @see     Object#equals(Object)
     * @see     #get(Object)
     */
    public synchronized V put(K key, V value) {
        // Make sure the value is not null
        if (value == null) {
            throw new NullPointerException();
        }

        // Makes sure the key is not already in the hashtable.
        Entry<?,?> tab[] = table;
        int hash = key.hashCode();
        int index = (hash & 0x7FFFFFFF) % tab.length;
        @SuppressWarnings("unchecked")
        Entry<K,V> entry = (Entry<K,V>)tab[index];
        for(; entry != null ; entry = entry.next) {
            if ((entry.hash == hash) && entry.key.equals(key)) {
                V old = entry.value;
                entry.value = value;
                return old;
            }
        }

        addEntry(hash, key, value, index);
        return null;
    }
```

**HashMap：**

```java
    /**
     * Computes key.hashCode() and spreads (XORs) higher bits of hash
     * to lower.  Because the table uses power-of-two masking, sets of
     * hashes that vary only in bits above the current mask will
     * always collide. (Among known examples are sets of Float keys
     * holding consecutive whole numbers in small tables.)  So we
     * apply a transform that spreads the impact of higher bits
     * downward. There is a tradeoff between speed, utility, and
     * quality of bit-spreading. Because many common sets of hashes
     * are already reasonably distributed (so don't benefit from
     * spreading), and because we use trees to handle large sets of
     * collisions in bins, we just XOR some shifted bits in the
     * cheapest possible way to reduce systematic lossage, as well as
     * to incorporate impact of the highest bits that would otherwise
     * never be used in index calculations because of table bounds.
     */
    static final int hash(Object key) {
        int h;
        return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
    }

```

**拓展：什么是hashCode？**

hashCode 是 JDK 根据对象的地址或者字符串或者数字算出来的 int 类型的数值。

**两者为啥 hash 算法不一样？**

Hashtable 在计算元素的位置时使用除留余数法来获得存储的最终的位置，而除法运算是比较耗时的。

HashMap 为了提高计算效率，将哈希表的大小固定为了 2 的倍数，这样在取模运算时，不需要做除法，只需要做位运算（左移一位就是乘以 2 ）。位运算比除法的效率要高很多。

HashMap 的效率虽然提高了，但是 hash 冲突却也增加了。因为它得出的 hash 值的低位相同的概率比较高。

为了解决这个问题，HashMap 重新根据 hashcode 计算 hash 值后，又将 hash 值无符号右移 16 位，使得运算出来所取得的位置分散到高低位中，从而减少了 hash 冲突。HashMap 中采取的这种简单位运算操作，不会把使用 2 的幂次方带来的效率提升给抵消掉。


#### 7. 迭代器内部实现不同

Hashtable、HashMap都使用了 Iterator。Hashtable还使用了Enumeration的方式 。

Hashtable中的 Enumerator类，实现了Enumeration接口和Iterator接口：

```java
    /**
     * A hashtable enumerator class.  This class implements both the
     * Enumeration and Iterator interfaces, but individual instances
     * can be created with the Iterator methods disabled.  This is necessary
     * to avoid unintentionally increasing the capabilities granted a user
     * by passing an Enumeration.
     */
    private class Enumerator<T> implements Enumeration<T>, Iterator<T> {
        Entry<?,?>[] table = Hashtable.this.table;
        int index = table.length;
        Entry<?,?> entry;
        Entry<?,?> lastReturned;
        int type;

        /**
         * Indicates whether this Enumerator is serving as an Iterator
         * or an Enumeration.  (true -> Iterator).
         */
        boolean iterator;

        /**
         * The modCount value that the iterator believes that the backing
         * Hashtable should have.  If this expectation is violated, the iterator
         * has detected concurrent modification.
         */
        protected int expectedModCount = modCount;

        Enumerator(int type, boolean iterator) {
            this.type = type;
            this.iterator = iterator;
        }

        public boolean hasMoreElements() {
            Entry<?,?> e = entry;
            int i = index;
            Entry<?,?>[] t = table;
            /* Use locals for faster loop iteration */
            while (e == null && i > 0) {
                e = t[--i];
            }
            entry = e;
            index = i;
            return e != null;
        }

        @SuppressWarnings("unchecked")
        public T nextElement() {
            Entry<?,?> et = entry;
            int i = index;
            Entry<?,?>[] t = table;
            /* Use locals for faster loop iteration */
            while (et == null && i > 0) {
                et = t[--i];
            }
            entry = et;
            index = i;
            if (et != null) {
                Entry<?,?> e = lastReturned = entry;
                entry = e.next;
                return type == KEYS ? (T)e.key : (type == VALUES ? (T)e.value : (T)e);
            }
            throw new NoSuchElementException("Hashtable Enumerator");
        }

        // Iterator methods
        public boolean hasNext() {
            return hasMoreElements();
        }

        public T next() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
            return nextElement();
        }

        public void remove() {
            if (!iterator)
                throw new UnsupportedOperationException();
            if (lastReturned == null)
                throw new IllegalStateException("Hashtable Enumerator");
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();

            synchronized(Hashtable.this) {
                Entry<?,?>[] tab = Hashtable.this.table;
                int index = (lastReturned.hash & 0x7FFFFFFF) % tab.length;

                @SuppressWarnings("unchecked")
                Entry<K,V> e = (Entry<K,V>)tab[index];
                for(Entry<K,V> prev = null; e != null; prev = e, e = e.next) {
                    if (e == lastReturned) {
                        modCount++;
                        expectedModCount++;
                        if (prev == null)
                            tab[index] = e.next;
                        else
                            prev.next = e.next;
                        count--;
                        lastReturned = null;
                        return;
                    }
                }
                throw new ConcurrentModificationException();
            }
        }
    }
```

HashMap 中的 Iterator ：

```java
  /* ------------------------------------------------------------ */
    // iterators

    abstract class HashIterator {
        Node<K,V> next;        // next entry to return
        Node<K,V> current;     // current entry
        int expectedModCount;  // for fast-fail
        int index;             // current slot

        HashIterator() {
            expectedModCount = modCount;
            Node<K,V>[] t = table;
            current = next = null;
            index = 0;
            if (t != null && size > 0) { // advance to first entry
                do {} while (index < t.length && (next = t[index++]) == null);
            }
        }

        public final boolean hasNext() {
            return next != null;
        }

        final Node<K,V> nextNode() {
            Node<K,V>[] t;
            Node<K,V> e = next;
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
            if (e == null)
                throw new NoSuchElementException();
            if ((next = (current = e).next) == null && (t = table) != null) {
                do {} while (index < t.length && (next = t[index++]) == null);
            }
            return e;
        }

        public final void remove() {
            Node<K,V> p = current;
            if (p == null)
                throw new IllegalStateException();
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
            current = null;
            K key = p.key;
            removeNode(hash(key), key, null, false, false);
            expectedModCount = modCount;
        }
    }
```

**拓展：JDK 8 之后，HashMap 和 Hashtable 的 Iterator 都有 fast-fail 机制。**

当有其它线程修改了 HashMap 的结构时，将会抛出 ConcurrentModificationException 异常。

注：结构修改是指改变 HashMap 中的映射数量或以其他方式修改其内部结构 (如，重新哈希，增加，删除，修改元素)。

**什么是 fast-fail 机制？**

![](https://dream-syz.github.io/1-1.png)

例如，通常不允许一个线程在另一个线程迭代 Collection 时修改它。

通常，迭代的结果在这些情况下是没有定义的。如果检测到此行为，一些 Iterator 实现(包括JRE提供的所有通用集合实现)可能会选择抛出此异常。这样做的迭代器被称为快速失败迭代器，因为它们快速而干净地失败，而不是冒着在未来不确定的时间发生任意、不确定行为的风险。

迭代器中的 modCount 变量，类似于并发编程中的 CAS（Compare and Swap）技术。我们可以看到这个方法中，每次在发生增删改的时候都会出现 modCount++ 的动作。

![](https://dream-syz.github.io/1-2.png)

而 modcount 可以理解为是当前 hashtable 的状态。每发生一次操作，状态就向前走一步。设置这个状态，主要是由于 hashtable 等容器类在迭代时，判断数据是否过时时使用的。尽管 hashtable 采用了原生的同步锁来保护数据安全。但是在出现迭代数据的时候，则无法保证边迭代，边正确操作。于是使用这个值来标记状态。一旦在迭代的过程中状态发生了改变，则会快速抛出一个异常，终止迭代行为。



### 13、 Collection 包结构与 Collections 的区别

Collection 是集合类的上级接口，子接口有 Set、List、LinkedList、ArrayList、Vector、Stack、Set；

Collections 是集合类的一个帮助类， 它包含有各种有关集合操作的静态多态方法，用于实现对各种集合的搜索、排序、线程安全化等操作。此类不能实例化，就像一个工具类，服务于 Java 的 Collection 框架。



### 14、 Java 的四种引用，强弱软虚

#### 1、强引用

强引用是平常中使用最多的引用，强引用在程序内存不足（OOM）的时候也不会被回收，使用方式：

```java
String str = new String("str");
System.out.println(str);
```

#### 2、软引用

软引用在程序内存不足时，会被回收，使用方式：

```java
// 注意：wrf 这个引用也是强引用，它是指向 SoftReference 这个对象的，
// 这里的软引用指的是指向 new String("str") 的引用，也就是 SoftReference 类中 T
SoftReference<String> wrf = new SoftReference<String>(new String("str"));
```

可用场景： 创建缓存的时候，创建的对象放进缓存中，当内存不足时，JVM 就会回收早先创建的对象。

#### 3、弱引用

弱引用就是只要 JVM 垃圾回收器发现了它，就会将之回收，他的强度比软引用更低一点，弱引用的对象下一次 GC

的时候一定会被回收，而不管内存是否足够。使用方式：

```java
WeakReference<String> wrf = new WeakReference<String>(str);
```

**可用场景：** Java 源码中的 `java.util.WeakHashMap` 中的 `key` 就是使用弱引用，我的理解就是，一旦我不需要某个引用，JVM 会自动帮我处理它，这样我就不需要做其它操作。

#### 4、虚引用

虚引用的回收机制跟弱引用差不多，但是它被回收之前，会被放入 `ReferenceQueue` 中。注意哦，其它引用是被 JVM 回收后才被传入 `ReferenceQueue` 中的。由于这个机制，所以虚引用大多被用于引用销毁前的处理工作。还有就是，虚引用创建的时候，必须带有 `ReferenceQueue` ，同样的当发生GC的时候，虚引用也会被回收。可以用虚引用来管理堆外内存

使用例子：

```java
PhantomReference<String> prf = new PhantomReference<String>(new String("str"),
new ReferenceQueue<>());
```

可用场景： 对象销毁前的一些操作，比如说资源释放等。 `Object.finalize()` 虽然也可以做这类动作，但是这个方式即不安全又低效

上诉所说的几类引用，都是指对象本身的引用，而不是指 Reference 的四个子类的引用（ SoftReference 等）。



### 15、 泛型常用特点

泛型是 Java SE 1.5 之后的特性， 《Java 核心技术》中对泛型的定义是：

“泛型” 意味着编写的代码可以被不同类型的对象所重用。

“泛型”，顾名思义，“泛指的类型”。我们提供了泛指的概念，但具体执行的时候却可以有具体的规则来约束，比如我们用的非常多的ArrayList 就是个泛型类，ArrayList作为集合可以存放各种元素，如 Integer， String，自定义的各种类型等，但在我们使用的时候通过具体的规则来约束，如我们可以约束集合中只存放Integer类型的元素，如

```java
List<Integer> iniData = new ArrayList<>()
```

使用泛型的好处？

以集合来举例，使用泛型的好处是我们不必因为添加元素类型的不同而定义不同类型的集合，如整型集合类，浮点型集合类，字符串集合类，我们可以定义一个集合来存放整型、浮点型，字符串型数据，而这并不是最重要的，因为我们只要把底层存储设置了 Object 即可，添加的数据全部都可向上转型为 Object。 更重要的是我们可以通过规则按照自己的想法控制存储的数据类型。



### 16、 Java 创建对象有几种方式？

#### **1、new 关键字**

使用 new 关键字创建对象，这是我们最常见的也是最简单的创建对象的方式，通过这种方式我们还可以调用任意的构造器（无参的和有参的）。

```java
ClassName myClass = new ClassName();
```

**注意：**

- 当我们使用 `new` 创建了一个对象时，会在栈中创建 `myClass` 这个引用，并且在堆中开辟一块空间存放对象的值，然后让 `myClass` 这个引用指向堆中新建的 `ClassName` 对象的值；

- 不管每次创建的对象的值是否相同，每次用 `new` 创建对象时，在栈中创建的引用都是不一样的，即地址都是不一样的。

#### **2、Class.newInstance**

这是我们运用 **反射** 创建对象时最常用的方法。

`Class` 类的 `newInstance` 使用的是类的 `public` 的 **无参** 构造方法。因此也就是说使用此方法创建对象的前提是必须有 `public` 的无参构造器才行：

```java
ClassName myClass = ClassName.class.newInstance();
ClassName myClass = (ClassName)Class.foeName("ClassName").newInstance();
```

#### 3、Constructor.newInstance

该方法和 `Class` 类的 `newInstance` 方法很像，但是比它强大很多。

`java.lang.relect.Constructor` 类里也有一个 `newInstance` 方法可以创建对象。

我们可以通过这个 `newInstance` 方法调用 **有参数**（不再必须是无参）的和 **私有的** 构造函数（不再必须是 `public` ）。

```java
Constructor<ClassName> constructor = ClassName.class.getConstructor();
ClassName class = constructor.newInstance();
```

#### 两种 newInstance 方法的区别

- `Class` 类位于 `java` 的 `lang` 包中，
  `Constructor` 是 `java` 反射机制的一部分

- `Class` 类的 `newInstance` 只能触发 **无参数** 的构造方法创建对象，
  `Constructor` 类的 `newInstance` 能触发 **有参数** 或者 **任意参数** 的构造方法来创建对象。
- `Class` 类的 `newInstance` 需要其构造方法是 `public` 的或者对调用方法可见的，
  `Constructor` 类的 `newInstance` 可以在特定环境下调用 **私有构造方法** 来创建对象。

- `Class` 类的 `newInstance` 抛出类构造函数的异常，
  `Constructor` 类的 `newInstance` 包装了一个 `InvocationTargetException` 异常。

#### 4、Clone 方法

无论何时我们调用一个对象的 `clone` 方法，`JVM` 就会创建一个新的对象，将前面的对象的内容全部拷贝进去，用 `clone` 方法创建对象并不会调用任何构造函数。

要使用 `clone` 方法，我们必须先实现 `Cloneable` 接口并复写 `Object` 的 `clone` 方法（因为 `Object` 的这个方法是 `protected` 的，若不复写，外部也调用不了）。

    public class ClassName implements Cloneable {
    	...
    	// 访问权限写为public，并且返回值写为myclass
        @Override
        public ClassName clone() throws CloneNotSupportedException {
            return (ClassName) super.clone();
        }
        ...
    }
    
    public class Main {
    	public static void main(String[] args) throws Exception {
       		ClassName myClass = new ClassName();
        	Object clone = myclass.clone();
        	//ClassName clone = (ClassName)myClass.clone();
    	}
    }

**注意：**

- `clone()` 方法只会进行 **浅复制**，也就是说只会在栈中再创建一个引用指向原来的对象的值所处的堆空间中，堆中的对象的值还是原来的并没有重新创建；

- 使用 `clone()` 方法并不需要调用造函数

#### 5、反序列化

要想通过反序列化创建对象，就必须现将某个对象序列化

**概念：**

1.**序列化：将 `java` 对象转化为字节流或字符流的过程**

**作用：**

（1）便于在网络上进行传输；

（2）将 `java` 字节序列永久保存在硬盘上，通常放在文件中，所以序列化也可以叫做持久化。

2.**反序列化：将字节流或字符流对象转化成 `java` 对象的过程**

```java
/**
 * 使用反序列化创建对象
 * 当序列化和反序列化一个对象时，JVM会创建一个单独的对象。
 * 在反序列化时，JVM 创建对象并不会调用任何构造函数，
 * 为了反序列化一个对象，需要让我们的 ClassName 类实现 Serializable 接口
 */
@Test
public void function() {
    String filePath = "F:\\Tests\\data.obj";
    try {
	    //序列化过程
        ObjectOutputStream objectOutputStream = new ObjectOutputStream(new FileOutputStream(filePath));
        objectOutputStream.writeObject(new ClassName());
        objectOutputStream.close();
 
        //反序列化过程
		ObjectInputStream inputStream = new ObjectInputStream(new FileInputStream(filePath));
		ClassName myClass = (ClassName) inputStream.readObject();
		inputStream.close();
 
	} catch (IOException | ClassNotFoundException e) {
		e.printStackTrace();
	}
}
```

**备注**：JDK 序列化、反序列化特别特别耗内存。

#### Java 创建实例对象是不是必须要通过构造函数/构造器/构造方法？

| 创建对象方式            | 是否调用了构造器 |
| ----------------------- | ---------------- |
| new关键字               | 是               |
| Class.newInstance       | 是               |
| Constructor.newInstance | 是               |
| Clone                   | 否               |
| 反序列化                | 否               |

**答案：**Java 创建实例对象，并不一定必须要调用构造函数/构造器/构造方法的。



### 17 、有没有可能两个不相等的对象有相同的 hashcode

有可能.在产生 hash 冲突时,两个不相等的对象就会有相同的 hashcode 值。当 hash 冲突产生时,一般有以下几种方式来处理：

- 拉链法：每个哈希表节点都有一个 next 指针，多个哈希表节点可以用 next 指针构成一个单向链表，被分配到同一个索引上的多个节点可以用这个单向链表进行存储

- 开放定址法：一旦发生了冲突,就去寻找下一个空的散列地址，只要散列表足够大，空的散列地址总能找到，并将记录存入
- 再哈希：又叫双哈希法，有多个不同的 Hash 函数。当发生冲突时，使用第二个，第三个….等哈希函数计算地址，直到无冲突

#### 哈希冲突的产生原因及解决方法

##### 哈希冲突的产生

哈希表是根据关键码的值而直接进行访问的数据结构。

哈希法又称散列法、杂凑法以及关键字地址计算法等，相应的表称为哈希表。

哈希表中关键码就是数组的索引下标，然后通过下标直接访问数组中的元素。（哈希表可以用来快速判断一个元素是否出现在集合里）

哈希法法的基本思想是：首先在元素的关键字 k 和元素的存储位置 p 之间建立一个对应关系 f ，使得 p = f ( k ) ，f称为哈希函数。创建哈希表时，把关键字为 k 的元素直接存入地址为 f ( k ) 的单元；以后当查找关键字为 k 的元素时，再利用哈希函数计算出该元素的存储位置p = f ( k ) ，从而达到按关键字直接存取元素的目的。

当关键字集合很大时，关键字值不同的元素可能会映象到哈希表的同一地址上，即 k1 ≠ k2 ，但 H（k1）= H（k2），这种现象称为冲突，此时称 k1 和 k2 为同义词。实际中，冲突是不可避免的，只能通过改进哈希函数的性能来减少冲突。

综上所述，哈希法主要包括以下两方面的内容：

 1）如何构造哈希函数

 2）如何处理冲突

##### **产生哈希冲突的影响因素**

装填因子（ 装填因子 = 数据总数 / 哈希表长 ）、哈希函数、处理冲突的方法

##### **哈希冲突解决办法**

###### 1、开放定址法（再散列法）

基本思想：当关键字 key 的哈希地址 p = H（key）出现冲突时，以 p 为基础，产生另一个哈希地址 p1，如果 p1 仍然冲突，再以 p 为基础，产生另一个哈希地址 p2 ，…，直到找出一个不冲突的哈希地址 pi ，将相应元素存入其中。

（1）线性探测

按顺序决定值时，如果某数据的值已经存在，则在原来值的基础上往后加一个单位，直至不发生哈希冲突。

（2）再平方探测

按顺序决定值时，如果某数据的值已经存在，则在原来值的基础上先加1的平方个单位，若仍然存在则减 1 的平方个单位。随之是 2 的平方，3 的平方等等。直至不发生哈希冲突。

（3）伪随机探测

按顺序决定值时，如果某数据已经存在，通过随机函数随机生成一个数，在原来值的基础上加上随机数，直至不发生哈希冲突。

###### 2、链地址法（拉链法：**HashMap 的哈希冲突解决方法**）

基本思想：以数组为基本单元，将所有的哈希地址为i的元素构成一个称为同义词链的单链表，并将单链表的头指针存在哈希表的第 i 个单元中，因而查找、插入和删除主要在同义词链中进行。

链地址法适用于经常进行插入和删除的情况。

**优点：**

（1）拉链法处理冲突简单，且无堆积现象，即非同义词决不会发生冲突，因此平均查找长度较短；

（2）由于拉链法中各链表上的结点空间是动态申请的，故它更适合于造表前无法确定表长的情况；

（3）开放定址法为减少冲突，要求装填因子 α 较小，故当结点规模较大时会浪费很多空间。而拉链法中可取 α ≥ 1 ，且结点较大时，拉链法中增加的指针域可忽略不计，因此节省空间；

（4）在用拉链法构造的散列表中，删除结点的操作易于实现。只要简单地删去链表上相应的结点即可。

**缺点：**

指针占用较大空间时，会造成空间浪费，若空间用于增大散列表规模进而提高开放地址法的效率。

###### 3、再哈希法

基本思想：同时构造多个不同的哈希函数，在发生冲突的时候再用另外一个哈希函数算出哈希值，直到算出的哈希值不同为止。

###### 4、建立公共溢出区

基本思想：将哈希表分为基本表和溢出表两部分，凡是和基本表发生冲突的元素，一律填入溢出表。查表时，先去基本表查，查不到再去溢出区查找。



### 18、深拷贝和浅拷贝的区别是什么？

- 浅拷贝：被复制对象的所有变量都含有与原来的对象相同的值，而所有的对其他对象的引用仍然指向原来的对象。换言之，**浅拷贝仅仅复制所考虑的对象，而不复制它所引用的对象**

- 深拷贝：被复制对象的所有变量都含有与原来的对象相同的值，而那些引用其他对象的变量将指向被复制过的新对象，而不再是原有的那些被引用的对象。换言之，**深拷贝把要复制的对象所引用的对象都复制了一遍**



### 19、final 有哪些用法?

- 被 final 修饰的类不可以被继承

- 被 final 修饰的方法不可以被重写

- 被 final 修饰的变量不可以被改变。如果修饰引用，那么表示引用不可变，引用指向的内容可变

- 被 final 修饰的方法，JVM 会尝试将其内联，以提高运行效率

- 被 final 修饰的常量，在编译阶段会存入常量池中

除此之外,编译器对 final 域要遵守的两个重排序规则更好：

在构造函数内对一个 final 域的写入，与随后把这个被构造对象的引用赋值给一个引用变量，这两个操作之间不能重排序，初次读一个包含final域的对象的引用，与随后初次读这个 final 域，这两个操作之间不能重排序.



### 20、static 都有哪些用法？

所有的人都知道 static 关键字这两个基本的用法：静态变量和静态方法。也就是被 static 所修饰的变量 / 方法都属于类的静态资源，类实例所共享.

除了静态变量和静态方法之外，static 也用于静态块，多用于初始化操作：

```java
public calss PreCache{
 static{
 //执行相关操作
 }
}
```

此外 static 也多用于修饰内部类,此时称之为静态内部类

最后一种用法就是静态导包，即 `import static` .import static是在 JDK 1.5 之后引入的新特性,可以用来指定导入某个类中的静态资源,并且不需要使用类名,可以直接使用资源名，比如：

```java
import static java.lang.Math.*;
public class Test{
 public static void main(String[] args){
 //System.out.println(Math.sin(20));传统做法
 System.out.println(sin(20));
 }
}
```



### 21、 3 * 0.1 **==**  0.3 返回值是什么

false，因为有些浮点数不能完全精确的表示出来



### 22、a = a + b 与 a + = b 有什么区别吗?

`+=` 操作符会进行隐式自动类型转换，此处 a + = b 隐式的将加操作的结果类型强制转换为持有结果的类型,而 a = a + b 则不会自动进行类型转换。如：

```java
byte a = 127;
byte b = 127;
b = a + b; // 报编译错误:cannot convert from int to byte
b += a;
```

以下代码是否有错,有的话怎么改？

```java
short s1= 1;
s1 = s1 + 1;
```

有错误。short 类型在进行运算时会自动提升为 int 类型，也就是说 s1 + 1 的运算结果是 int 类型而 s1 是 short 类型，此时编译器会报错

正确写法：

```JAVA
short s1= 1;
s1 += 1;
```

`+=` 操作符会对右边的表达式结果强转匹配左边的数据类型，所以没错



### 23、try catch finally，try 里有 return ，finally 还执行么？

执行，并且 finally 的执行早于 try 里面的 return。如果 finally 块中存在 return 语句，它会覆盖 try 块或 catch 块中的 return 语句，并成为最终的返回值。

结论：

1、不管有木有出现异常，finally 块中代码都会执行；

2、当 try 和 catch 中有 return 时，finally 仍然会执行；

3、finally 是在 return 后面的表达式运算后执行的（此时并没有返回运算后的值，而是先把要返回的值保存起来，管 finally 中的代码怎么样，返回的值都不会改变，仍然是之前保存的值），所以函数返回值是在 finally 执行前确定的；

4、finally 中最好不要包含 return，否则程序会提前退出，返回值不是 try 或 catch 中保存的返回值。



### 24、Excption 与 Error 包结构

Java 可抛出 ( Throwable ) 的结构分为三种类型：被检查的异常 ( CheckedException )，运行时异常 (RuntimeException )，错误( Error )

#### 1、运行时异常

定义：RuntimeException 及其子类都被称为运行时异常。

特点：Java 编译器不会检查它。也就是说，当程序中可能出现这类异常时，倘若既"没有通过 throws 声明抛出它"，也"没有用 try-catch语句捕获它"，还是会编译通过。例如，除数为零时产生的 ArithmeticException 异常，数组越界时产生的 IndexOutOfBoundsException异常，fail - fast 机制产生的 ConcurrentModificationException 异常（java.util 包下面的所有的集合类都是快速失败的，“快速失败”也就是 fail-fast，它是 Java 集合的一种错误检测机制。当多个线程对集合进行结构上的改变的操作时，有可能会产生fail-fast机制。记住是有可能，而不是一定。例如：假设存在两个线程（线程1、线程2），线程 1 通过 Iterator 在遍历集合 A 中的元素，在某个时候线程 2 修改了集合 A 的结构（是结构上面的修改，而不是简单的修改集合元素的内容），那么这个时候程序就会抛出ConcurrentModificationException 异常，从而产生 fail-fast 机制，这个错叫并发修改异常。Failsafe，java.util.concurrent 包下面的所有的类都是安全失败的，在遍历过程中，如果已经遍历的数组上的内容变化了，迭代器不会抛出 ConcurrentModificationException 异常。如果未遍历的数组上的内容发生了变化，则有可能反映到迭代过程中。这就是 ConcurrentHashMap 迭代器弱一致的表现。ConcurrentHashMap 的弱一致性主要是为了提升效率，是一致性与效率之间的一种权衡。要成为强一致性，就得到处使用锁，甚至是全局锁，这就与 Hashtable 和同步的 HashMap 一样了。）等，都属于运行时异常。

常见的五种运行时异常：

ClassCastException（类转换异常）

IndexOutOfBoundsException（数组越界）

NullPointerException（空指针异常）

ArrayStoreException（数据存储异常，操作数组是类型不一致）

BufferOverflowException

#### 2、被检查异常

定义：Exception 类本身，以及 Exception 的子类中除了"运行时异常"之外的其它子类都属于被检查异常。

特点：Java 编译器会检查它。 此类异常，要么通过 throws 进行声明抛出，要么通过 try - catch 进行捕获处理，否则不能通过编译。例如，CloneNotSupportedException 就属于被检查异常。当通过 clone() 接口去克隆一个对象，而该对象对应的类没有实现 Cloneable 接口，就会抛出 CloneNotSupportedException 异常。被检查异常通常都是可以恢复的。 如：

IOException

FileNotFoundException

SQLException

被检查的异常适用于那些不是因程序引起的错误情况，比如：读取文件时文件不存在引发的 FileNotFoundException 。然而，不被检查的异常通常都是由于糟糕的编程引起的，比如：在对象引用时没有确保对象非空而引起的 NullPointerException 。

#### 3、错误

定义：Error 类及其子类。

特点：和运行时异常一样，编译器也不会对错误进行检查。

当资源不足、约束失败、或是其它程序无法继续运行的条件发生时，就产生错误。程序本身无法修复这些错误的。例如，VirtualMachineError 就属于错误。出现这种错误会导致程序终止运行。

OutOfMemoryError、ThreadDeath。

Java虚拟机规范规定JVM的内存分为了好几块，比如堆，栈，程序计数器，方法区等



### 25、OOM 你遇到过哪些情况，SOF 你遇到过哪些情况

#### **OOM**：

##### 1、OutOfMemoryError 异常

除了程序计数器外，虚拟机内存的其他几个运行时区域都有发生 OutOfMemoryError ( OOM ) 异常的可能。

Java Heap 溢出：

一般的异常信息：java.lang.OutOfMemoryError:Java heap spacess。

java 堆用于存储对象实例，我们只要不断的创建对象，并且保证 GC Roots 到对象之间有可达路径来避免垃圾回收机制清除这些对象，就会在对象数量达到最大堆容量限制后产生内存溢出异常。

出现这种异常，一般手段是先通过内存映像分析工具 ( 如 Eclipse Memory Analyzer ) 对 dump 出来的堆转存快照进行分析，重点是确认内存中的对象是否是必要的，先分清是因为内存泄漏 ( Memory Leak ) 还是内存溢出 ( Memory Overflow )。

如果是内存泄漏，可进一步通过工具查看泄漏对象到 GCRoots 的引用链。于是就能找到泄漏对象是通过怎样的路径与 GC Roots 相关联并导致垃圾收集器无法自动回收。

如果不存在泄漏，那就应该检查虚拟机的参数  ( -Xmx 与 -Xms ) 的设置是否适当。

##### 2、虚拟机栈和本地方法栈溢出

如果线程请求的栈深度大于虚拟机所允许的最大深度，将抛出 StackOverflowError 异常。

如果虚拟机在扩展栈时无法申请到足够的内存空间，则抛出 OutOfMemoryError异常

这里需要注意当栈的大小越大可分配的线程数就越少。

##### 3、运行时常量池溢出

异常信息：java.lang.OutOfMemoryError:PermGenspace

如果要向运行时常量池中添加内容，最简单的做法就是使用 String.intern() 这个 Native 方法。该方法的作用是：如果池中已经包含一个等于此 String 的字符串，则返回代表池中这个字符串的String对象；否则，将此 String 对象包含的字符串添加到常量池中，并且返回此String 对象的引用。由于常量池分配在方法区内，我们可以通过 -XX:PermSize 和 -XX:MaxPermSize 限制方法区的大小，从而间接限制其中常量池的容量。

##### 4、方法区溢出

方法区用于存放 Class 的相关信息，如类名、访问修饰符、常量池、字段描述、方法描述等。也有可能是方法区中保存的 class 对象没有被及时回收掉或者class信息占用的内存超过了我们配置。

异常信息：java.lang.OutOfMemoryError:PermGenspace

方法区溢出也是一种常见的内存溢出异常，一个类如果要被垃圾收集器回收，判定条件是很苛刻的。在经常动态生成大量 Class 的应用中，要特别注意这点。

#### SOF（堆栈溢出 StackOverflow）：

StackOverflowError 的定义：当应用程序递归太深而发生堆栈溢出时，抛出该错误。

因为栈一般默认为 1 - 2 m，一旦出现死循环或者是大量的递归调用，在不断的压栈过程中，造成栈容量超过 1 m 而导致溢出。

栈溢出的原因：递归调用，大量循环或死循环，全局变量是否过多，数组、List、map数据过大。



### 26、 简述线程、程序、进程的基本概念。以及他们之间关系是什么?

**线程**与进程相似，但线程是一个比进程更小的执行单位。一个进程在其执行的过程中可以产生多个线程。与进程不同的是同类的多个线程共享同一块内存空间和一组系统资源，所以系统在产生一个线程，或是在各个线程之间作切换工作时，负担要比进程小得多，也正因为如此，线程也被称为轻量级进程。

**程序**是含有指令和数据的文件，被存储在磁盘或其他的数据存储设备中，也就是说程序是静态的代码。

**进程**是程序的一次执行过程，是系统运行程序的基本单位，因此进程是动态的。系统运行一个程序即是一个进程从创建，运行到消亡的过程。简单来说，一个进程就是一个执行中的程序，它在计算机中一个指令接着一个指令地执行着，同时，每个进程还占有某些系统资源如 CPU 时间，内存空间，文件，输入输出设备的使用权等等。换句话说，当程序在执行时，将会被操作系统载入内存中。 线程是进程划分成的更小的运行单位。线程和进程最大的不同在于基本上各进程是独立的，而各线程则不一定，因为同一进程中的线程极有可能会相互影响。从另一角度来说，进程属于操作系统的范畴，主要是同一段时间内，可以同时执行一个以上的程序，而线程则是在同一程序内几乎同时执行一个以上的程序段。



### 27、Java 序列化中如果有些字段不想进行序列化，怎么办？

对于不想进行序列化的变量，使用 transient 关键字修饰。

transient 关键字的作用是：阻止实例中那些用此关键字修饰的的变量序列化；当对象被反序列化时，被 transient 修饰的变量值不会被持久化和恢复。transient 只能修饰变量，不能修饰类和方法。



### 28、说说 Java 中 IO 流

#### Java 中 IO 流分为几种?

- 按照流的流向分，可以分为输入流和输出流；

- 按照操作单元划分，可以划分为字节流和字符流；

- 按照流的角色划分为节点流和处理流。

Java IO 流共涉及 40 多个类，这些类看上去很杂乱，但实际上很有规则，而且彼此之间存在非常紧密的联系， Java IO 流的 40 多个类都是从如下 4 个抽象类基类中派生出来的。

InputStream / Reader：所有的输入流的基类，前者是字节输入流，后者是字符输入流。

OutputStream / Writer：所有输出流的基类，前者是字节输出流，后者是字符输出流。



### 29、 Java IO 与 NIO 的区别（补充）

NIO 即 New IO，这个库是在 JDK1.4 中才引入的。NIO 和 IO 有相同的作用和目的，但实现方式不同，NIO 主要用到的是块，所以 NIO 的效率要比 IO 高很多。在 Java API 中提供了两套 NIO，一套是针对标准输入输出 NIO，另一套就是网络编程 NIO。



### 30、java 反射的作用于原理

#### 1、定义：

反射机制是在运行时，对于任意一个类，都能够知道这个类的所有属性和方法；对于任意个对象，都能够调用它的任意一个方法。在 java中，只要给定类的名字，就可以通过反射机制来获得类的所有信息。

**这种动态获取的信息以及动态调用对象的方法的功能称为 Java 语言的反射机制。**

#### 2、哪里会用到反射机制？

jdbc 就是典型的反射

```java
Class.forName('com.mysql.jdbc.Driver.class');//加载 MySQL 的驱动类
```

这就是反射。如 hibernate，struts 等框架使用反射实现的。

#### 3、反射的实现方式：

第一步：获取 Class 对象，有 4 中方法： 

1）Class.forName (“类的路径”)； 

2）类名.class 

3）对象名.getClass() 

4）基本类型的包装类，可以调用包装类的 Type 属性来获得该包装类的 Class 对象

#### 4、实现 Java 反射的类：

1）Class：表示正在运行的Java应用程序中的类和接口。注意： 所有获取对象的信息都需要 Class 类来实现。 

2）Field：提供有关类和接口的属性信息，以及对它的动态访问权限。 

3）Constructor：提供关于类的单个构造方法的信息以及它的访问权限 

4）Method：提供类或接口中某个方法的信息

#### 5、反射机制的优缺点：

**优点：** 

1）能够运行时动态获取类的实例，提高灵活性

2）与动态编译结合 

**缺点：** 

1）使用反射性能较低，需要解析字节码，将内存中的对象进行解析。 

解决方案： 

1、通过 setAccessible ( true ) 关闭  JDK 的安全检查来提升反射速度； 

2、多次创建一个类的实例时，有缓存会快很多 

3、ReflectASM 工具类，通过字节码生成的方式加快反射速度

2）相对不安全，破坏了封装性（ 因为通过反射可以获得私有方法和属性 ）



### 31、说说 List，Set，Map 三者的区别？

- List（对付顺序的好帮手）：List 接口存储一组不唯一（可以有多个元素引用相同的对象），有序的对象

- Set（注重独一无二的性质）：不允许重复的集合。不会有多个元素引用相同的对象。

- Map（用 Key 来搜索的专家）：使用键值对存储。Map 会维护与 Key 有关联的值。两个 Key 可以引用相同的对象，但 Key 不能重复，典型的 Key 是 String 类型，但也可以是任何对象。



### 32、Object 有哪些常用方法？

java.lang.Object

**clone** **方法**

保护方法，实现对象的浅复制，只有实现了 Cloneable 接口才可以调用该方法，否则抛出 CloneNotSupportedException 异常，深拷贝也需要实现 Cloneable，同时其成员变量为引用类型的也需要实现 Cloneable，然后重写 clone 方法。

**finalize** **方法**

该方法和垃圾收集器有关系，判断一个对象是否可以被回收的最后一步就是判断是否重写了此方法。

**equals** **方法**

该方法使用频率非常高。一般 equals 和 == 是不一样的，但是在 Object 中两者是一样的。子类一般都要重写这个方法。

**hashCode** **方法**

该方法用于哈希查找，重写了 equals 方法一般都要重写 hashCode 方法，这个方法在一些具有哈希功能的 Collection 中用到。

一般必须满足 obj1.equals(obj2) == true 。可以推出 obj1.hashCode() == obj2.hashCode() ，但是 hashCode 相等不一定就满足 equals。不过为了提高效率，应该尽量使上面两个条件接近等价。

JDK 1.6、1.7  默认是返回随机数；

JDK 1.8  默认是通过和当前线程有关的一个随机数 + 三个确定值，运用 Marsaglia’s xorshift scheme 随机数算法得到的一个随机数。

**wait** **方法**

配合 synchronized 使用，wait 方法就是使当前线程等待该对象的锁，当前线程必须是该对象的拥有者，也就是具有该对象的锁。wait() 方法一直等待，直到获得锁或者被中断。wait ( long timeout ) 设定一个超时间隔，如果在规定时间内没有获得锁就返回。

调用该方法后当前线程进入睡眠状态，直到以下事件发生。

1. 其他线程调用了该对象的 notify 方法；
2. 其他线程调用了该对象的 notifyAll 方法；
3. 其他线程调用了 interrupt 中断该线程；
4. 时间间隔到了。

此时该线程就可以被调度了，如果是被中断的话就抛出一个 InterruptedException 异常。

**notify** **方法**

配合 synchronized 使用，该方法唤醒在该对象上**等待队列**中的某个线程（同步队列中的线程是给抢占 CPU 的线程，等待队列中的线程指的是等待唤醒的线程）。

**notifyAll** **方法**

配合 synchronized 使用，该方法唤醒在该对象上等待队列中的所有线程。

**总结**

只要把上面几个方法熟悉就可以了，toString 和 getClass 方法可以不用去讨论它们。该题目考察的是对 Object 的熟悉程度，平时用的很多方法并没看其定义但是也在用，比如说：wait() 方法，equals() 方法等。

```
Class Object is the root of the class hierarchy.Every class has Object as a
superclass. All objects, including arrays, implement the methods of this class.
```

大致意思：Object 是所有类的根，是所有类的父类，所有对象包括数组都实现了 Object 的方法。



### 33、获取一个类 Class 对象的方式有哪些？

搞清楚类对象和实例对象，但都是对象。

第一种：通过类对象的 getClass() 方法获取，细心点的都知道，这个 getClass 是 Object 类里面的方法。

```java
User user=new User();
//clazz就是一个User的类对象
Class<?> clazz=user.getClass()
```

第二种：通过类的静态成员表示，每个类都有隐含的静态成员 class。

```java
//clazz就是一个User的类对象
Class<?> clazz=User.class;
```

第三种：通过 Class 类的静态方法 forName() 方法获取。

```java
Class<?> clazz = Class.forName("com.tian.User");
```



### 34、**用过** **ArrayList** 吗？说一下它有什么特点？

Java 集合框架中的一种存放相同类型的元素数据，是一种变长的集合类，基于定长数组实现，当加入数据达到一定程度后，会实行自动扩容，即扩大数组大小。

底层是使用数组实现，添加元素。

如果 add ( o ) ，添加到的是数组的尾部，如果要增加的数据量很大，应该使用 ensureCapacity() 方法，该方法的作用是预先设置 ArrayList 的大小，这样可以大大提高初始化速度。

如果使用 add ( int , o ) ，添加到某个位置，那么可能会挪动大量的数组元素，并且可能会触发扩容机制。

高并发的情况下，线程不安全。多个线程同时操作 ArrayList，会引发不可预知的异常或错误。

ArrayList 实现了 Cloneable 接口，标识着它可以被复制。注意：ArrayList 里面的 clone() 复制其实是浅复制。

**有数组了为什么还要搞个** **ArrayList** **呢**

通常我们在使用的时候，如果在不明确要插入多少数据的情况下，普通数组就很尴尬了，因为你不知道需要初始化数组大小为多少，而 ArrayList 可以使用默认的大小，当元素个数到达一定程度后，会自动扩容。

可以这么来理解：我们常说的数组是定死的数组，ArrayList 却是动态数组



### 35、说说什么是 fail - fast ？

fail - fast 机制是 Java 集合（Collection）中的一种错误机制。当多个线程对同一个集合的内容进行操作时，就可能会产生 fail - fast 事件。

例如：当某一个线程 A 通过 iterator 去遍历某集合的过程中，若该集合的内容被其他线程所改变了，那么线程 A 访问集合时，就会抛出 ConcurrentModificationException 异常，产生 fail - fast 事件。这里的操作主要是指 add、remove 和 clear，对集合元素个数进行修改。

解决办法：建议使用“ java.util.concurrent 包下的类” 去取代 “ java.util 包下的类”。

可以这么理解：在遍历之前，把 modCount 记下来 expectModCount，后面 expectModCount 去和 modCount 进行比较，如果不相等了，证明已并发了，被修改了，于是抛出 ConcurrentModificationException 异常。



### 36、HashMap 中的 key 我们可以使用任何类作为 key 吗？

平时可能大家使用的最多的就是使用 String 作为 HashMap 的 key，但是现在我们想使用某个自定义类作为 HashMap 的 key，那就需要注意以下几点：

- 如果类重写了 equals 方法，它也应该重写 hashCode 方法。

- 类的所有实例需要遵循与 equals 和 hashCode 相关的规则。

- 如果一个类没有使用 equals，你不应该在 hashCode 中使用它。

- 咱们自定义 key 类的最佳实践是使之为不可变的，这样，hashCode 值可以被缓存起来，拥有更好的性能。不可变的类也可以确保 hashCode 和 equals 在未来不会改变，这样就会解决与可变相关的问题了。



### 37、HashMap 的长度为什么是 2 的 N 次方呢？

为了能让 HashMap 存数据和取数据的效率高，尽可能地减少 hash 值的碰撞，也就是说尽量把数据能均匀的分配，每个链表或者红黑树长度尽量相等。

我们首先可能会想到 % 取模的操作来实现。

下面是回答的重点哟：

取余（%）操作中如果除数是 2 的幂次，则等价于与其除数减一的与（&）操作（也就是说 hash % length == hash & ( length - 1 ) 的前提是 length 是 2 的 n 次方）。并且，采用二进制位操作 & ，相对于 % 能够提高运算效率。

这就是为什么 HashMap 的长度需要 2 的 N 次方了



### 38、HashMap 与 ConcurrentHashMap 的异同

1. 都是 key - value 形式的存储数据；
2. HashMap 是线程不安全的，ConcurrentHashMap 是 JUC 下的线程安全的；
3. HashMap 底层数据结构是数组 + 链表（ JDK 1.8 之前）。JDK 1.8 之后是数组 + 链表 + 红黑树。当链表中元素个数达到 8 的时候，链表的查询速度不如红黑树快，链表会转为红黑树，红黑树查询速度快；

4. HashMap 初始数组大小为 16（默认），当出现扩容的时候，以 0.75 * 数组大小的方式进行扩容；

5. ConcurrentHashMap 在 JDK 1.8 之前是采用分段锁来现实的 Segment + HashEntry，Segment 数组大小默认是 16，2 的 n 次方；JDK 1.8 之后，采用 Node + CAS + Synchronized来保证并发安全进行实现。



### 39、红黑树有哪几个特征？

- 每个节点是黑色或红色

- 根节点是黑色

- 每个叶子节点都是黑色（指向空的叶子节点）

- 如果一个叶子节点是红色，那么其子节点必须都是黑色的

- 从一个节点到该节点的子孙节点的所有路径上包含相同数目的黑节点



### 40、**说说你平时是怎么处理** **Java** 异常的

try - catch - finally

- try 块负责监控可能出现异常的代码

- catch 块负责捕获可能出现的异常，并进行处理

- finally 块负责清理各种资源，不管是否出现异常都会执行

- 其中 try 块是必须的，catch 和 finally 至少存在一个标准异常处理流程

在开发过程中会使用到自定义异常，在通常情况下，程序很少会自己抛出异常，因为异常的类名通常也包含了该异常的有用信息，所以在选择抛出异常的时候，应该选择合适的异常类，从而可以明确地描述该异常情况，所以这时候往往都是自定义异常。

自定义异常通常是通过继承 java.lang.Exception 类，如果想自定义 Runtime 异常的话，可以继承 java.lang.RuntimeException 类，实现一个无参构造和一个带字符串参数的有参构造方法。

在业务代码里，可以针对性的使用自定义异常。比如说：该用户不具备某某权限、余额不足等





## JVM 篇

### 1、知识点汇总

JVM 是 Java 运行基础，面试时一定会遇到 JVM 的有关问题，内容相对集中，但对只是深度要求较高

![](https://dream-syz.github.io/2-1.png)

其中内存模型、类加载机制、GC 是重点方面。性能调优部分更偏向应用。重点突出实践能力，编译器优化和执行模式部分偏向于理论基础,重点掌握知识点。

需了解 **内存模型** 各部分作用，保存哪些数据。

**类加载** 双亲委派加载机制，常用加载器分别加载哪种类型的类。

**GC** 分代回收的思想和依据以及不同垃圾回收算法的回收思路和适合场景。

**性能调优** 常有 JVM 优化参数作用，参数调优的依据，常用的 JVM 分析工具能分析哪些问题以及使用方法。

**执行模式 ** 解释 / 编译 / 混合模式的优缺点，Java 7 提供的分层编译技术，JIT 即时编译技术，OSR 栈上替换，C1 / C2 编译器针对的场景，C2 针对的是 server 模式，优化更激进。新技术方面 Java 10 的 graal 编译器

**编译器优化** javac 的编译过程，ast 抽象语法树，编译器优化和运行器优化.



### 2、知识点详解

#### 1、JVM 内存模型：

线程独占：栈，本地方法栈，程序计数器 

线程共享：堆，方法区

#### 2、栈：

又称方法栈，线程私有的，线程执行方法是都会创建一个栈阵，用来存储局部变量表、操作栈、动态链接、方法出口等信息。调用方法时执行入栈，方法返回时执行出栈。

#### 3、本地方法栈

与栈类似，也是用来保存执行方法的信息。执行 Java 方法是使用栈，执行 Native 方法时使用本地方法栈。

#### 4、程序计数器

保存着当前线程执行的字节码位置，每个线程工作时都有独立的计数器，只为执行 Java 方法服务，执行 Native 方法时，程序计数器为空

#### 5、堆

JVM 内存管理最大的一块，它被线程共享，目的是存放对象的实例，几乎所有的对象实例都会放在这里，当堆没有可用空间时，会抛出OOM 异常。根据对象的存活周期不同，JVM 把对象进行分代管理，由垃圾回收器进行垃圾的回收管理

#### 6、方法区：

又称非堆区，用于存储已被虚拟机加载的类信息、常量、静态变量，即时编译器优化后的代码等数据。1.7 的永久代和 1.8 的元空间都是方法区的一种实现

#### 7、JVM 内存可见性

![](https://dream-syz.github.io/2-2.png)

JMM 是定义程序中变量的访问规则，线程对于变量的操作只能在自己的工作内存中进行,而不能直接对主内存操作。由于指令重排序，读写的顺序会被打乱，因此 JMM 需要提供原子性、可见性、有序性保证。

![](https://dream-syz.github.io/2-3.png)



### 3、说说类加载与卸载

#### **1、加载过程**

![image-20230814093140833](https://dream-syz.github.io/2-4.png)

其中 **验证**，**准备**，**解析** 合称链接

**加载** 通过类的完全限定名，查找此类字节码文件，利用字节码文件创建Class对象。

**验证** 确保 Class 文件符合当前虚拟机的要求，不会危害到虚拟机自身安全。

**准备** 进行内存分配，为 static 修饰的类变量分配内存，并设置初始值 ( 0 或 null )。不包含 final 修饰的静态变量，因为 final 变量在编译时分配。

**解析 **将常量池中的符号引用替换为直接引用的过程，直接引用为直接指向目标的指针或者相对偏移量等。

**初始化** 主要完成静态块执行以及静态变量的赋值，先初始化父类，再初始化当前类，只有对类主动使用时才会初始化。

触发条件包括，创建类的实例时，访问类的静态方法或静态变量的时候，使用 `Class.forName` 反射类的时候，或者某个子类初始化的时候。

Java 自带的加载器加载的类，在虚拟机的生命周期中是不会被卸载的，只有用户自定义的加载器加载的类才可以被卸

#### 2、加载机制 - 双亲委派模式

![image-20230814094411338](https://dream-syz.github.io/2-5.png)



双亲委派模式，即加载器加载类时先把请求委托给自己的父类加载器执行，直到顶层的启动类加载器。

父类加载器能够完成加载则成功返回，不能则子类加载器才自己尝试加载。

**优点：**

1. 避免类的重复加载
2. 避免 Java 的核心 API 被篡改

#### 3、分代回收

分代回收基于两个事实:大部分对象很快就不使用了,还有一部分不会立即无用,但也不会持续很长时间。 

| 堆分代 |                     |                     |                     |
| ------ | ------------------- | ------------------- | ------------------- |
| 年轻代 | Dden                | Survivor1           | Survivor2           |
| 老年代 | Tebured             | Tenured             | Tenured             |
| 永久代 | PremGen / MetaSpace | PremGen / MetaSpace | PremGen / MetaSpace |

年轻代 ——> 标记 - 复制 老年代 ——> 标记 - 清除

#### **4**、回收算法

##### *a*、*G1*算法

1.9 后默认的垃圾回收算法，特点保持高回收率的同时减少停顿。采用每次只清理一部分，而不是清理全部的增量式清理，以保证停顿时间不会过长

其取消了年轻代与老年代的物理划分，但仍属于分代收集器，算法将堆分为若干个逻辑区域 ( region ) ，一部分用作年轻代，一部分用作老年代，还有用来存储巨型对象的分区。

同 CMS 相同，会遍历所有对象，标记引用情况，清除对象后会对区域进行复制移动，以整合碎片空间。

年轻代回收：并行复制采用复制算法，并行收集，会 StopTheWorld。

老年代回收：会对年轻代一并回收

初始标记完成堆 root 对象的标记，会 StopTheWorld。并发标记 GC 线程和应用线程并发执行。 最终标记完成三色标记周期，会StopTheWorld。复制 / 清楚会优先对可回收空间加大的区域进行回收

##### *b*、*ZGC*算法

前面提供的高效垃圾回收算法，针对大堆内存设计，可以处理 TB 级别的堆，可以做到 10 ms 以下的回收停顿时间

**GC roots 标记 ——> 并发标记 ——> 清除 ——> 并发重定位 <—— root 重定位**

- 着色指针

- 读屏障

- 并发处理

- 基于 region

- 内存压缩 ( 整理 )

roots 标记：标记 root 对象，会发生 StopTheWorld.。并发标记：利用读屏障与应用线程一起运行标记，可能会发生 StopTheWorld。清除会清理标记为不可用的对象。 roots 重定位：是对存活的对象进行移动，以腾出大块内存空间，减少碎片产生。重定位最开始会StopTheWorld，取决于重定位集与对象总活动集的比例。并发重定位与并发标记类似



### 4、简述一下 JVM 的内存模型

#### 1.JVM 内存模型简介

JVM 定义了不同运行时数据区，他们是用来执行应用程序的。某些区域随着 JVM 启动及销毁，另外一些区域的数据是线程性独立的，随着线程创建和销毁。JVM 内存模型总体架构图如下：（ 摘自 oracle 官方网站 ）

![](https://dream-syz.github.io/2-6.png)

JVM 在执行 Java 程序时，会把它管理的内存划分为若干个的区域，每个区域都有自己的用途和创建销毁时间。如下图所示，可以分为两大部分，线程私有区和共享区。下图是根据自己理解画的一个 JVM 内存模型架构图

![](https://dream-syz.github.io/2-7.png)

JVM 内存分为线程私有区和线程共享区

##### **线程私有区**

###### 1、程序计数器

当同时进行的线程数超过 CPU 数或其内核数时，就要通过时间片轮询分派 CPU 的时间资源，不免发生线程切换。这时，每个线程就需要一个属于自己的计数器来记录下一条要运行的指令。如果执行的是 JAVA 方法，计数器记录正在执行的 java 字节码地址，如果执行的是native 方法，则计数器为空。

###### 2、虚拟机栈

线程私有的，与线程在同一时间创建。管理 JAVA 方法执行的内存模型。每个方法执行时都会创建一个桢栈来存储方法的的变量表、操作数栈、动态链接方法、返回值、返回地址等信息。栈的大小决定了方法调用的可达深度（ 递归多少层次，或嵌套调用多少层其他方法，   -Xss 参数可以设置虚拟机栈大小）。栈的大小可以是固定的，或者是动态扩展的。如果请求的栈深度大于最大可用深度，则抛出stackOverflowError；如果栈是可动态扩展的，但没有内存空间支持扩展，则抛出 OutofMemoryError。 使用 jclasslib 工具可以查看class 类文件的结构。下图为栈帧结构图：

![](https://dream-syz.github.io/2-8.png)

###### 3、本地方法栈

与虚拟机栈作用相似。但它不是为 Java 方法服务的，而是本地方法（ C 语言）。由于规范对这块没有强制要求，不同虚拟机实现方法不同。

##### **线程共享区**

###### 1、方法区

线程共享的，用于存放被虚拟机加载的类的元数据信息，如常量、静态变量和即时编译器编译后的代码。若要分代，算是永久代（老年代），以前类大多 `static` 的，很少被卸载或收集，现回收废弃常量和无用的类。其中运行时常量池存放编译生成的各种常量。（如果hotspot 虚拟机确定一个类的定义信息不会被使用，也会将其回收。回收的基本条件至少有：所有该类的实例被回收，而且装载该类的ClassLoader 被回收）

###### 2、堆

存放对象实例和数组，是垃圾回收的主要区域，分为新生代和老年代。刚创建的对象在新生代的 Eden 区中，经过 GC 后进入新生代的 S0 区中，再经过 GC 进入新生代的 S1 区中，15 次 GC 后仍存在就进入老年代。这是按照一种回收机制进行划分的，不是固定的。若堆的空间不够实例分配，则 OutOfMemoryError。

![](https://dream-syz.github.io/2-9.png)

```
Young Generation 即图中的 Eden + From Space（s0） + To Space(s1)
Eden 存放新生的对象
Survivor Space 有两个，存放每次垃圾回收后存活的对象(s0+s1)
Old Generation Tenured Generation 即图中的Old Space
 主要存放应用程序中生命周期长的存活对象
```



### 5、说说堆和栈的区别

栈是运行时单位，代表着逻辑，内含基本数据类型和堆中对象引用，所在区域连续，没有碎片；堆是存储单位，代表着数据，可被多个栈共享（包括成员中基本数据类型、引用和引用对象），所在区域不连续，会有碎片。

#### 1、功能不同

栈内存用来存储 **局部变量** 和 **方法调用**，而堆内存用来存储 Java 中的对象。无论是成员变量，局部变量，还是类变量，它们指向的对象都存储在堆内存中。

#### 2、共享性不同

栈内存是线程私有的。 堆内存是所有线程共有的。

#### 3、异常错误不同

如果栈内存或者堆内存不足都会抛出异常。 

栈空间不足：java.lang.StackOverFlowError。 

堆空间不足：java.lang.OutOfMemoryError。

#### 4、空间大小

栈的空间大小远远小于堆的。



### 6、什么时候会触发 Full GC

除直接调用 System.gc 外，触发 Full GC 执行的情况有如下四种。

#### 1、旧生代空间不足 

旧生代空间只有在新生代对象转入及创建为大对象、大数组时才会出现不足的现象，当执行 Full GC 后空间仍然不足，则抛出如下错误： `java.lang.OutOfMemoryError: Java heap space` 为避免以上两种状况引起的 Full GC，调优时应尽量做到让对象在 Minor GC 阶段被回收、让对象在新生代多存活一段时间及不要创建过大的对象及数组。

#### 2、Permanet Generation 空间满

Permanet Generation中 存放的为一些 class 的信息等，当系统中要加载的类、反射的类和调用的方法较多时，Permanet Generation 可能会被占满，在未配置为采用 CMS GC 的情况下会执行 Full GC。如果经过 Full GC 仍然回收不了，那么 JVM 会抛出如下错误信息： `java.lang.OutOfMemoryError: PermGen space` 为避免 Perm Gen 占满造成 Full GC 现象，可采用的方法为增大 Perm Gen 空间或转为使用 CMS GC。

#### 3、CMS GC 时出现 promotion failed 和 concurrent mode failure 

对于采用 CMS 进行旧生代 GC 的程序而言，尤其要注意 GC 日志中是否有 promotion failed 和 concurrent mode failure 两种状况，当这两种状况出现时可能会触发 Full GC。 promotion failed 是在进行 Minor GC 时，survivor space 放不下、对象只能放入旧生代，而此时旧生代也放不下造成的；concurrent mode failure 是在执行 CMS GC 的过程中同时有对象要放入旧生代，而此时旧生代空间不足造成的。 应对措施为：增大 survivor space、旧生代空间或调低触发并发 GC 的比率，但在 JDK 5.0 +、6.0 + 的版本中有可能会由于 JDK 的bug29 导致 CMS 在 remark 完毕后很久才触发 sweeping 动作。对于这种状况，可通过设置 -XX:CMSMaxAbortablePrecleanTime=5（单位为ms）来避免。

#### 4、统计得到的 Minor GC 晋升到旧生代的平均大小大于旧生代的剩余空间

这是一个较为复杂的触发情况，Hotspot 为了避免由于新生代对象晋升到旧生代导致旧生代空间不足的现象，在进行 Minor GC 时，做了一个判断，如果之前统计所得到的 Minor GC 晋升到旧生代的平均大小大于旧生代的剩余空间，那么就直接触发 Full GC。 例如程序第一次触发 Minor GC 后，有 6 MB 的对象晋升到旧生代，那么当下一次 Minor GC 发生时，首先检查旧生代的剩余空间是否大于 6 MB，如果小于 6 MB，则执行 Full GC。 当新生代采用 PS GC 时，方式稍有不同，PS GC 是在 Minor GC 后也会检查，例如上面的例子中第一次 Minor GC 后，PS GC 会检查此时旧生代的剩余空间是否大于 6 MB，如小于，则触发对旧生代的回收。 除了以上4种状况外，对于使用RMI来进行RPC或管理的Sun JDK应用而言，默认情况下会一小时执行一次Full GC。可通过在启动时通过

`-java Dsun.rmi.dgc.client.gcInterval=3600000`来设置Full GC执行的间隔时间或通过

`-XX:+DisableExplicitGC`来禁止 RMI 调用 System.gc



### 7、什么是 Java 虚拟机？为什么 Java 被称作是“平台无关的编程语言“？

Java 虚拟机是一个可以执行 Java 字节码的虚拟机进程。Java 源文件被编译成能被 Java 虚拟机执行的字节码文件。 Java 被设计成允许应用程序可以运行在任意的平台，而不需要程序员为每一个平台单独重写或者是重新编译。Java 虚拟机让这个变为可能，因为它知道底层硬件平台的指令长度和其他特性。



### 8、Java 内存结构

![](https://dream-syz.github.io/2-10.png)

方法区和堆是所有线程共享的内存区域；而 java 栈、本地方法栈和程序员计数器是运行是线程私有的内存区域。

- Java 堆（ Heap ）是 Java 虚拟机所管理的内存中最大的一块。Java 堆是被所有线程共享的一块内存区域，在虚拟机启动时创建。此内存区域的唯一目的就是存放对象实例，几乎所有的对象实例都在这里分配内存。

- 方法区（ Method Area ）与 Java 堆一样，是各个线程共享的内存区域，它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译器编译后的代码等数据。

- 程序计数器（ Program Counter Register ）是一块较小的内存空间，它的作用可以看做是当前线程所执行的字节码的行号指示器。

- JVM 栈（JVM Stacks）与程序计数器一样，Java 虚拟机栈（ Java Virtual Machine Stacks ）也是线程私有的，它的生命周期与线程相同。虚拟机栈描述的是 Java 方法执行的内存模型：每个方法被执行的时候都会同时创建一个栈帧（ Stack Frame ）用于存储局部变量表、操作栈、动态链接、方法出口等信息。每一个方法被调用直至执行完成的过程，就对应着一个栈帧在虚拟机栈中从入栈到出栈的过程。

- 本地方法栈（Native Method Stacks）与虚拟机栈所发挥的作用是非常相似的，其区别不过是虚拟机栈为虚拟机执行 Java 方法（也就是字节码）服务，而本地方法栈则是为虚拟机使用到的 Native 方法服务。



### 9、说说对象分配规则

- 对象优先分配在 Eden 区，如果 Eden 区没有足够的空间时，虚拟机执行一次 Minor GC。

- 大对象直接进入老年代（大对象是指需要大量连续内存空间的对象）。这样做的目的是避免在Eden区和两个 Survivor 区之间发生大量的内存拷贝（新生代采用复制算法收集内存）。

- 长期存活的对象进入老年代。虚拟机为每个对象定义了一个年龄计数器，如果对象经过了 1 次 Minor GC 那么对象会进入 Survivor区，之后每经过一次 Minor GC 那么对象的年龄加 1，知道达到阀值对象进入老年区。

- 动态判断对象的年龄。如果 Survivor 区中相同年龄的所有对象大小的总和大于 Survivor 空间的一半，年龄大于或等于该年龄的对象可以直接进入老年代。

- 空间分配担保。每次进行 Minor GC 时，JVM 会计算 Survivor 区移至老年区的对象的平均大小，如果这个值大于老年区的剩余值大小则进行一次 Full GC，如果小于检查 HandlePromotionFailure 设置，如果 true 则只进行 Monitor GC，如果 false 则进行 Full GC。



### 10、描述一下 JVM 加载 class 文件的原理机制？

JVM 中类的装载是由类加载器（ ClassLoader ）和它的子类来实现的，Java 中的类加载器是一个重要的 Java 运行时系统组件，它负责在运行时查找和装入类文件中的类。 由于 Java 的跨平台性，经过编译的 Java 源程序并不是一个可执行程序，而是一个或多个类文件。当Java 程序需要使用某个类时，JVM 会确保这个类已经被 **加载、连接（验证、准备和解析）和初始化**。类的加载是指把类的 `.class` 文件中的数据读入到内存中，通常是创建一个字节数组读入 `.class` 文件，然后产生与所加载类对应的 `Class` 对象。加载完成后，`Class` 对象还不完整，所以此时的类还不可用。当类被加载后就进入连接阶段，这一阶段包括 **验证、准备（为静态变量分配内存并设置默认的初始值）和解析（将符号引用替换为直接引用）**三个步骤。

最后JVM对类进行初始化，包括：

1) 如果类存在直接的父类并且这个类还没有被初始化，那么就先初始化父类；

2) 如果类中存在初始化语句，就依次执行这些初始化语句。 类的加载是由类加载器完成的，类加载器包括：根加载器（BootStrap）、扩展加载器（Extension）、系统加载器（System）和用户自定义类加载器（java.lang.ClassLoader的子类）。从 Java 2（JDK 1.2）开始，类加载过程采取了父亲委托机制（PDM）。PDM 更好的保证了 Java 平台的安全性，在该机制中，JVM 自带的 Bootstrap 是根加载器，其他的加载器都有且仅有一个父类加载器。类的加载首先请求父类加载器加载，父类加载器无能为力时才由其子类加载器自行加载。JVM 不会向 Java 程序提供对 Bootstrap 的引用。下面是关于几个类加载器的说明：

   - Bootstrap：一般用本地代码实现，负责加载 JVM 基础核心类库（rt.jar）；

   - Extension：从 `java.ext.dirs` 系统属性所指定的目录中加载类库，它的父加载器是 Bootstrap；

   - System：又叫应用类加载器，其父类是 Extension。它是应用最广泛的类加载器。它从环境变量 classpath 或者系统属性java.class.path 所指定的目录中记载类，是用户自定义加载器的默认父加载器。



### 11、说说 Java 对象创建过程

1. JVM 遇到一条新建对象的指令时首先去检查这个指令的参数是否能在常量池中定义到一个类的符号引用。然后加载这个类（类加载过程在后边讲）

2. 为对象分配内存。一种办法 “指针碰撞” 、一种办法 “空闲列表” ，最终常用的办法 “本地线程缓冲分配(TLAB)”

3. 将除对象头外的对象内存空间初始化为 0

4. 对对象头进行必要设置



### 12、知道类的生命周期吗？

类的生命周期包括这几个部分，加载、连接、初始化、使用和卸载，其中前三部是类的加载的过程，如下图；

![](https://dream-syz.github.io/2-11.png)

- 加载，查找并加载类的二进制数据，在 Java 堆中也创建一个 `java.lang.Class` 类的对象

- 连接，连接又包含三块内容：验证、准备、初始化。 1）验证，文件格式、元数据、字节码、符号引用验证； 2）准备，为类的静态变量分配内存，并将其初始化为默认值； 3）解析，把类中的符号引用转换为直接引用

- 初始化，为类的静态变量赋予正确的初始值

- 使用，new 出对象程序中使用

- 卸载，执行垃圾回收



### 13、简述 Java 的对象结构

Java 对象由三个部分组成：对象头、实例数据、对齐填充。

- 对象头由两部分组成，第一部分存储对象自身的运行时数据：哈希码、GC 分代年龄、锁标识状态、线程持有的锁、偏向线程 ID（一般占 32 / 64 bit ）。第二部分是指针类型，指向对象的类元数据类型（即对象代表哪个类）。如果是数组对象，则对象头中还有一部分用来记录数组长度。

- 实例数据用来存储对象真正的有效信息（包括父类继承下来的和自己定义的）

- 对齐填充：JVM 要求对象起始地址必须是 8 字节的整数倍（8 字节对齐）



### 14、如何判断对象可以被回收？

判断对象是否存活一般有两种方式：

引用计数：每个对象有一个引用计数属性，新增一个引用时计数加 1，引用释放时计数减 1，计数为 0 时可以回收。此方法简单，无法解决对象相互循环引用的问题。

可达性分析（Reachability Analysis）：从 GC Roots 开始向下搜索，搜索所走过的路径称为引用链。当一个对象到 GC Roots 没有任何引用链相连时，则证明此对象是不可用的，不可达对象。



### 15、JVM 的永久代中会发生垃圾回收么？

垃圾回收不会发生在永久代，如果永久代满了或者是超过了临界值，会触发完全垃圾回收（Full GC）。如果你仔细查看垃圾收集器的输出信息，就会发现永久代也是被回收的。这就是为什么正确的永久代大小对避免Full GC是非常重要的原因。请参考下 Java 8：从永久代到元数据区（注：Java 8 中已经移除了永久代，新加了一个叫做元数据区的 native 内存区）



### 16、你知道哪些垃圾收集算法

GC 最基础的算法有三种： 标记 - 清除算法、复制算法、标记 - 压缩算法，我们常用的垃圾回收器一般都采用分代收集算法。

- 标记 - 清除算法，“标记 - 清除”（ Mark - Sweep ）算法，如它的名字一样，算法分为“标记”和“清除”两个阶段：首先标记出所有需要回收的对象，在标记完成后统一回收掉所有被标记的对象。

- 复制算法，“复制”（ Copying ）的收集算法，它将可用内存按容量划分为大小相等的两块，每次只使用其中的一块。当这一块的内存用完了，就将还存活着的对象复制到另外一块上面，然后再把已使用过的内存空间一次清理掉。

- 标记 - 压缩算法，标记过程仍然与“标记 - 清除”算法一样，但后续步骤不是直接对可回收对象进行清理，而是让所有存活的对象都向一端移动，然后直接清理掉端边界以外的内存

- 分代收集算法，“分代收集”（ Generational Collection ）算法，把 Java 堆分为新生代和老年代，这样就可以根据各个年代的特点采用最适当的收集算法。



### 17、调优命令有哪些？

Sun JDK 监控和故障处理命令有 jps jstat jmap jhat jstack jinfo

- jps，JVM Process Status Tool，显示指定系统内所有的 HotSpot 虚拟机进程。

- jstat，JVM statistics Monitoring 是用于监视虚拟机运行时状态信息的命令，它可以显示出虚拟机进程中的类装载、内存、垃圾收集、JIT 编译等运行数据。

- jmap，JVM Memory Map 命令用于生成 heap dump 文件

- jhat，JVM Heap Analysis Tool 命令是与 jmap 搭配使用，用来分析 jmap 生成的 dump，jhat 内置了一个微型的 HTTP / HTML 服务器，生成 dump 的分析结果后，可以在浏览器中查看

- jstack，用于生成 java 虚拟机当前时刻的线程快照。

- jinfo，JVM Configuration info 这个命令作用是实时查看和调整虚拟机运行参数。



### 18、常见调优工具有哪些

常用调优工具分为两类，jdk 自带监控工具：jconsole 和 jvisualvm，第三方有：MAT（Memory Analyzer Tool）、GChisto。

jconsole，Java Monitoring and Management Console 是从 java5 开始，在 JDK 中自带的 java 监控和管理控制台，用于对 JVM 中内存，线程和类等的监控

jvisualvm，jdk 自带全能工具，可以分析内存快照、线程快照；监控内存变化、GC 变化等。

MAT，Memory Analyzer Tool，一个基于 Eclipse 的内存分析工具，是一个快速、功能丰富的 Java heap 分析工具，它可以帮助我们查找内存泄漏和减少内存消耗

GChisto，一款专业分析 gc 日志的工具



### 19、Minor GC 与 Full GC 分别在什么时候发生？

新生代内存不够用时候发生 MGC 也叫 YGC，JVM 内存不够的时候发生 FGC



### 20、你知道哪些 JVM 性能调优参数？

**简单回答：**

- 设定堆内存大小

	   -Xmx 堆内存最大限制。

- 设定新生代大小。 新生代不宜太小，否则会有大量对象涌入老年代

		-XX:NewSize 新生代大小
		
		-XX:NewRatio 新生代和老生代占比
		
		-XX:SurvivorRatio 伊甸园空间和幸存者空间的占比

- 设定垃圾回收器 年轻代用 -XX:+UseParNewGC 年老代用 -XX:+UseConcMarkSweepGC

**具体回答：**

#### **「堆栈内存相关」**

-Xms 设置初始堆的大小

-Xmx 设置最大堆的大小

-Xmn 设置年轻代大小，相当于同时配置 -XX:NewSize 和 -XX:MaxNewSize 为一样的值

-Xss 每个线程的堆栈大小

-XX:NewSize 设置年轻代大小（ for 1.3 / 1.4 ）

-XX:MaxNewSize 年轻代最大值（ for 1.3 / 1.4 ）

-XX:NewRatio 年轻代与年老代的比值（除去持久代）

-XX:SurvivorRatio Eden 区与 Survivor 区的的比值

-XX:PretenureSizeThreshold 当创建的对象超过指定大小时，直接把对象分配在老年代。

-XX:MaxTenuringThreshold 设定对象在 Survivor 复制的最大年龄阈值，超过阈值转移到老年代

#### **「垃圾收集器相关」**

-XX:+UseParallelGC 选择垃圾收集器为并行收集器。

-XX:ParallelGCThreads=20 配置并行收集器的线程数

-XX:+UseConcMarkSweepGC 设置年老代为并发收集。

-XX:CMSFullGCsBeforeCompaction=5 由于并发收集器不对内存空间进行压缩、整理，所以运行一段时间以后会产生“碎片”，使得运行效率降低。此值设置运行 5 次 GC 以后对内存空间进行压缩、整理。

-XX:+UseCMSCompactAtFullCollection 打开对年老代的压缩。可能会影响性能，但是可以消除碎片

#### **「辅助信息相关」**

-XX:+PrintGCDetails 打印 GC 详细信息

-XX:+HeapDumpOnOutOfMemoryError 让 JVM 在发生内存溢出的时候自动生成内存快照,排查问题用

-XX:+DisableExplicitGC 禁止系统 System.gc()，防止手动误触发 FGC 造成问题.

-XX:+PrintTLAB 查看 TLAB 空间的使用情况



### 21、 对象一定分配在堆中吗？有没有了解逃逸分析技术？

**「对象一定分配在堆中吗？」** 不一定的，JVM 通过**「逃逸分析」**，那些逃不出方法的对象会在栈上分配。

**「什么是逃逸分析？」**

逃逸分析（ Escape Analysis ），是一种可以有效减少 Java 程序中同步负载和内存堆分配压力的跨函数全局数据流分析算法。通过逃逸分析，Java Hotspot 编译器能够分析出一个新的对象的引用的使用范围，从而决定是否要将这个对象分配到堆上。

**逃逸分析** 是指分析指针动态范围的方法，它同编译器优化原理的指针分析和外形分析相关联。当变量（或者对象）在方法中分配后，其指针有可能被返回或者被全局引用，这样就会被其他方法或者线程所引用，这种现象称作指针（或者引用）的逃逸（ Escape ）。通俗点讲，如果一个对象的指针被多个方法或者线程引用时，那么我们就称这个对象的指针发生了逃逸。

**「逃逸分析的好处」**

- 栈上分配，可以降低垃圾收集器运行的频率。

- 同步消除，如果发现某个对象只能从一个线程可访问，那么在这个对象上的操作可以不需要同步。

- 标量替换，把对象分解成一个个基本类型，并且内存分配不再是分配在堆上，而是分配在栈上。这样的好处有，一、减少内存使用，因为不用生成对象头。二、程序内存回收效率高，并且 GC 频率也会减少。



### 22、虚拟机为什么使用元空间替换了永久代？

**「什么是元空间？什么是永久代？为什么用元空间代替永久代？」** 我们先回顾一下**「方法区」**吧,看看虚拟机运行时数据内存图，如下:

![](https://dream-syz.github.io/2-12.png)

方法区和堆一样，是各个线程共享的内存区域，它用于存储已被虚拟机加载的类信息、常量、静态变量、即时编译后的代码等数据。

**「什么是永久代？它和方法区有什么关系呢？」**

如果在 HotSpot 虚拟机上开发、部署，很多程序员都把方法区称作永久代。可以说方法区是规范，永久代是 Hotspot 针对该规范进行的实现。在 Java7 及以前的版本，方法区都是永久代实现的。

**「什么是元空间？它和方法区有什么关系呢？」**

对于 Java8，HotSpots 取消了永久代，取而代之的是元空间（Metaspace）。换句话说，就是方法区还是在的，只是实现变了，从永久代变为元空间了。

**「为什么使用元空间替换了永久代？」**

- 永久代的方法区，和堆使用的物理内存是连续的。

![](https://dream-syz.github.io/2-13.png)

**「永久代」**是通过以下这两个参数配置大小的~

- -XX:PremSize：设置永久代的初始大小

- -XX:MaxPermSize: 设置永久代的最大值，默认是 64 M

对于**「永久代」**，如果动态生成很多 class 的话，就很可能出现**「 java.lang.OutOfMemoryError: PermGen space 错误」**，因为永久代空间配置有限嘛。最典型的场景是，在 web 开发比较多 jsp 页面的时候。

- JDK 8 之后，方法区存在于元空间（ Metaspace）。物理内存不再与堆连续，而是直接存在于本地内存中，理论上机器**「内存有多大，元空间就有多大」**。

![](https://dream-syz.github.io/2-14.png)

可以通过以下的参数来设置元空间的大小：

- -XX:MetaspaceSize，初始空间大小，达到该值就会触发垃圾收集进行类型卸载，同时 GC 会对该值进行调整：如果释放了大量的空间，就适当降低该值；如果释放了很少的空间，那么在不超过 MaxMetaspaceSize 时，适当提高该值。

- -XX:MaxMetaspaceSize，最大空间，默认是没有限制的。

- -XX:MinMetaspaceFreeRatio，在 GC 之后，最小的 Metaspace 剩余空间容量的百分比，减少为分配空间所导致的垃圾收集

- -XX:MaxMetaspaceFreeRatio，在 GC 之后，最大的 Metaspace 剩余空间容量的百分比，减少为释放空间所导致的垃圾收集

**「所以，为什么使用元空间替换永久代？」**

表面上看是为了避免 OOM 异常。因为通常使用 PermSize 和 MaxPermSize 设置永久代的大小就决定了永久代的上限，但是不是总能知道应该设置为多大合适, 如果使用默认值很容易遇到 OOM 错误。当使用元空间时，可以加载多少类的元数据就不再由 MaxPermSize 控制, 而由系统的实际可用空间来控制啦。



### 23、什么是 Stop The World ？什么是 OopMap ？什么是安全点？

进行垃圾回收的过程中，会涉及对象的移动。为了保证对象引用更新的正确性，必须暂停所有的用户线程，像这样的停顿，虚拟机设计者形象描述为**「 Stop The World 」**。也简称为 STW。

在 HotSpot 中，有个数据结构（映射表）称为**「 OopMap 」**。一旦类加载动作完成的时候，HotSpot 就会把对象内什么偏移量上是什么类型的数据计算出来，记录到 OopMap。在即时编译过程中，也会在**「特定的位置」**生成 OopMap，记录下栈上和寄存器里哪些位置是引用。

这些特定的位置主要在：

1.循环的末尾（非 counted 循环）

2.方法临返回前 / 调用方法的 call 指令后

3.可能抛异常的位置

这些位置就叫作**「安全点（ safepoint）」** 用户程序执行时并非在代码指令流的任意位置都能够在停顿下来开始垃圾收集，而是必须是执行到安全点才能够暂停。



### 24、说一下 JVM 的主要组成部分及其作用？

![](https://dream-syz.github.io/2-15.png)

JVM 包含两个子系统和两个组件，分别为

- Class loader（类装载子系统）

- Execution engine（执行引擎子系统）

- Runtime data area（运行时数据区组件）

- Native Interface（本地接口组件）

- **「 Class loader（类装载）」**： 根据给定的全限定名类名（如：java.lang.Object）来装载 class 文件到运行时数据区的方法区中。

- **「 Execution engine（执行引擎）」**：执行 class 的指令。

- **「 Native Interface（本地接口）」**： 与 native lib 交互，是其它编程语言交互的接口。

- **「 Runtime data area（运行时数据区域）」**：即我们常说的 JVM 的内存。

首先通过编译器把 Java 源代码转换成字节码，Class loader（类装载）再把字节码加载到内存中，将其放在运行时数据区的方法区内，而字节码文件只是 JVM 的一套指令集规范，并不能直接交给底层操作系统去执行，因此需要特定的命令解析器执行引擎（Execution Engine），将字节码翻译成底层系统指令，再交由 CPU 去执行，而这个过程中需要调用其他语言的本地库接口（Native Interface）来实现整个程序的功能。



### 25、什么是指针碰撞？

一般情况下，JVM 的对象都放在堆内存中（发生逃逸分析除外）。当类加载检查通过后，Java 虚拟机开始为新生对象分配内存。如果 Java 堆中内存是绝对规整的，所有被使用过的的内存都被放到一边，空闲的内存放到另外一边，中间放着一个指针作为分界点的指示器，所分配内存仅仅是把那个指针向空闲空间方向挪动一段与对象大小相等的实例，这种分配方式就是 指针碰撞。

![image-20230816094955691](https://dream-syz.github.io/2-16.png)



### 26、什么是空闲列表？

如果 Java 堆内存中的内存并不是规整的，已被使用的内存和空闲的内存相互交错在一起，不可以进行指针碰撞啦，虚拟机必须维护一个列表，记录哪些内存是可用的，在分配的时候从列表找到一块大的空间分配给对象实例，并更新列表上的记录，这种分配方式就是空闲列表。



### 27、什么是 TLAB ？

可以把内存分配的动作按照线程划分在不同的空间之中进行，每个线程在 Java 堆中预先分配一小块内存，这就是 TLAB（Thread Local Allocation Buffer，本地线程分配缓存） 。虚拟机通过 -XX:UseTLAB 设定它的。



### 28、对象头具体都包含哪些内容？

在我们常用的 Hotspot 虚拟机中，对象在内存中布局实际包含 3 个部分：

1. 对象头

2. 实例数据
3. 对齐填充

而对象头包含两部分内容，Mark Word 中的内容会随着锁标志位而发生变化，所以只说存储结构就好了。

1. 对象自身运行时所需的数据，也被称为 Mark Word，也就是用于轻量级锁和偏向锁的关键点。具体的内容包含对象的 hashcode、分代年龄、轻量级锁指针、重量级锁指针、GC 标记、偏向锁线程 ID、偏向锁时间戳。

2. 存储类型指针，也就是指向类的元数据的指针，通过这个指针才能确定对象是属于哪个类的实例。

如果是数组的话，则还包含了数组的长度。

![](https://dream-syz.github.io/2-17.png)



### 29、说一下 JVM 有哪些垃圾回收器？

如果说垃圾收集算法是内存回收的方法论，那么垃圾收集器就是内存回收的具体实现。下图展示了 7 种作用于不同分代的收集器，其中用于回收新生代的收集器包括 Serial、PraNew、Parallel Scavenge，回收老年代的收集器包括 Serial Old、Parallel Old、CMS，还有用于回收整个 Java 堆的 G1收集器。不同收集器之间的连线表示它们可以搭配使用。

![](https://dream-syz.github.io/2-18.png)

- Serial 收集器（复制算法）：新生代单线程收集器，标记和清理都是单线程，优点是简单高效；

- ParNew 收集器 （复制算法） ：新生代收并行集器，实际上是 Serial 收集器的多线程版本，在多核 CPU 环境下有着比 Serial 更好的表现；

- Parallel Scavenge 收集器（复制算法）：新生代并行收集器，追求高吞吐量，高效利用 CPU。吞吐量 = 用户线程时间 /（用户线程时间 + GC 线程时间），高吞吐量可以高效率的利用 CPU 时间，尽快完成程序的运算任务，适合后台应用等对交互相应要求不高的场景；

- Serial Old 收集器 （标记 - 整理算法）： 老年代单线程收集器，Serial 收集器的老年代版本；

- Parallel Old 收集器（标记 - 整理算法）： 老年代并行收集器，吞吐量优先，Parallel Scavenge 收集器的老年代版本；

- CMS（ Concurrent Mark Sweep ）收集器（标记 - 清除算法）： 老年代并行收集器，以获取最短回收停顿时间为目标的收集器，具有高并发、低停顿的特点，追求最短 GC 回收停顿时间。

- G1（ Garbage First ）收集器（标记 - 整理算法）： Java 堆并行收集器，G1 收集器是 JDK 1.7 提供的一个新收集器，G1 收集器基于“标记 - 整理”算法实现，也就是说不会产生内存碎片。此外，G1 收集器不同于之前的收集器的一个重要特点是：G1 回收的范围是整个 Java堆（包括新生代，老年代），而前六种收集器回收的范围仅限于新生代或老年代。

- ZGC （ Z Garbage Collector ）是一款由 Oracle 公司研发的，以低延迟为首要目标的一款垃圾收集器。它是基于动态 Region 内存布局，（暂时）不设年龄分代，使用了读屏障、染色指针和内存多重映射等技术来实现可并发的标记 - 整理算法的收集器。在  JDK 11 新加入，还在实验阶段，主要特点是：回收 TB 级内存（最大 4 T ），停顿时间不超过 10 ms。**优点**：低停顿，高吞吐量， ZGC 收集过程中额外耗费的内存小。**缺点**：浮动垃圾目前使用的非常少，真正普及还是需要写时间的。

**新生代收集器**：Serial、 ParNew 、 Parallel Scavenge

**老年代收集器**：CMS 、Serial Old、Parallel Old

**整堆收集器**：G1 ，ZGC （因为不涉年代不在图中）



### 30、如何选择垃圾收集器？

1. 如果你的堆大小不是很大（比如 100 MB ），选择串行收集器一般是效率最高的。

   参数： -XX:+UseSerialGC 。

2. 如果你的应用运行在单核的机器上，或者你的虚拟机核数只有单核，选择串行收集器依然是合适的，这时候启用一些并行收集器没有任何收益。

   参数： -XX:+UseSerialGC 。

3. 如果你的应用是“吞吐量”优先的，并且对较长时间的停顿没有什么特别的要求。选择并行收集器是比较好的。

   参数： -XX:+UseParallelGC 。

4. 如果你的应用对响应时间要求较高，想要较少的停顿。甚至 1 秒的停顿都会引起大量的请求失败，那么选择 G1 、ZGC 、CMS 都是合理的。虽然这些收集器的 GC 停顿通常都比较短，但它需要一些额外的资源去处理这些工作，通常吞吐量会低一些。

   参数：

   -XX:+UseConcMarkSweepGC 、

   -XX:+UseG1GC 、

   -XX:+UseZGC 等。

从上面这些出发点来看，我们平常的 Web 服务器，都是对响应性要求非常高的。选择性其实就集中在 CMS 、 G1 、 ZGC 上。而对于某些定时任务，使用并行收集器，是一个比较好的选择。



### 31、 什么是类加载器？

类加载器是一个用来加载类文件的类。Java 源代码通过 javac 编译器编译成类文件。然后 JVM 来执行类文件中的字节码来执行程序。类加载器负责加载文件系统、网络或其他来源的类文件。



### 32、什么是 tomcat 类加载机制？

在 tomcat 中类的加载稍有不同，如下图：

![](https://dream-syz.github.io/2-19.png)

当 tomca t启动时，会创建几种类加载器： **Bootstrap** **引导类加载器** 加载 JVM 启动所需的类，以及标准扩展类（位于 `jre/lib/ext` 下） **System** **系统类加载器** 加载 tomcat 启动的类，比如 bootstrap.jar，通常在 catalina.bat 或者 `catalina.sh` 中指定。位于 `CATALINA_HOME/bin` 下。

![](https://dream-syz.github.io/2-20.png)

**Common** **通用类加载器**



## 多线程 & 并发篇

### 1、说说 Java 中实现多线程有几种方法

**剖其底层 Runnable** 

创建线程的常用四种方式：

1. 继承 Thread 类，重写 run 方法
2. 实现 Runnable 接口，重写 run 方法
3. 实现 Callable 接口（  JDK 1.5 >= ），重写 call 方法，配合 FutureTask（有返回结果的非阻塞执行）
4. 线程池方式创建

lambda 方式

```
Thread t2 = new Thread(()->{
	for (int i = 0; i < 100; i++) {
		system.out.println( "lambda: " +i);
	}
});
```

通过继承 Thread 类或者实现 Runnable 接口、Callable 接口都可以实现多线程，不过实现 Runnable 接口与实现 Callable 接口的方式基本相同，只是 Callable 接口里定义的方法返回值，可以声明抛出异常而已。因此将实现 Runnable 接口和实现 Callable 接口归为一种方式。这种方式与继承 Thread 方式之间的主要差别如下。

**采用实现 Runnable、Callable 接口的方式创建线程的优缺点**

**优点**：线程类只是实现了 Runnable 或者 Callable 接口，还可以继承其他类。这种方式下，多个线程可以共享一个 target 对象，所以非常适合多个相同线程来处理同一份资源的情况，从而可以将 CPU、代码和数据分开，形成清晰的模型，较好的体现了面向对象的思想。

**缺点**：编程稍微复杂一些，如果需要访问当前线程，则必须使用 `Thread.currentThread()` 方法

**采用继承 Thread 类的方式创建线程的优缺点**

**优点**：编写简单，如果需要访问当前线程，则无需使用 `Thread.currentThread()` 方法，直接使用 this 即可获取当前线程

**缺点**：因为线程类已经继承了 Thread 类，Java 语言是单继承的，所以就不能再继承其他父类了。



### 2、如何停止一个正在运行的线程

1、使用退出标志，使线程正常退出，也就是当 run 方法完成后线程终止。

2、使用 stop 方法强行终止，但是不推荐这个方法，因为 stop 和 suspend 及 resume 一样都是过期作废的方法。

3、使用 interrupt 方法中断线程。

```java
class MyThread extends Thread {
 	volatile boolean stop = false;
 	public void run() {
      while (!stop) {
 		System.out.println(getName() + " is running");
 		try {
 			sleep(1000);
 		} catch (InterruptedException e) {
 			System.out.println("week up from blcok...");
 			stop = true; // 在异常处理代码中修改共享变量的状态
 		}
	  }
      System.out.println(getName() + " is exiting...");
 	}
}

class InterruptThreadDemo3 {
  public static void main(String[] args) throws InterruptedException {
    MyThread m1 = new MyThread();
 	System.out.println("Starting thread...");
 	m1.start();
 	Thread.sleep(3000);
 	System.out.println("Interrupt thread...: " + m1.getName());
 	m1.stop = true; // 设置共享变量为true
 	m1.interrupt(); // 阻塞时退出阻塞状态
 	Thread.sleep(3000); // 主线程休眠3秒以便观察线程m1的中断情况
 	System.out.println("Stopping application...");
  }
}
```



### 3、notify() 和 notifyAll() 有什么区别？

notify 可能会导致死锁，而 notifyAll 则不会

任何时候只有一个线程可以获得锁，也就是说只有一个线程可以运行 synchronized 中的代码使用 notifyAll，可以唤醒所有处于 wait 状态的线程，使其重新进入锁的争夺队列中，而 notify 只能唤醒一个。

wait() 应配合 while 循环使用，不应使用 if，务必在 wait() 调用前后都检查条件，如果不满足，必须调用 notify() 唤醒另外的线程来处理，自己继续 wait() 直至条件满足再往下执行。

notify() 是对 notifyAll() 的一个优化，但它有很精确的应用场景，并且要求正确使用。不然可能导致死锁。正确的场景应该是 WaitSet 中等待的是相同的条件，唤醒任一个都能正确处理接下来的事项，如果唤醒的线程无法正确处理，务必确保继续 notify() 下一个线程，并且自身需要重新回到 WaitSet 中.



### 4、sleep() 和 wait() 有什么区别？

对于 sleep() 方法，我们首先要知道该方法是属于 Thread 类中的。而 wait() 方法，则是属于 Object 类中的。

sleep() 方法导致了程序暂停执行指定的时间，让出 cpu 该其他线程，但是他的监控状态依然保持者，当指定的时间到了又会自动恢复运行状态。在调用 sleep() 方法的过程中，线程不会释放对象锁。

当调用 wait() 方法的时候，线程会放弃对象锁，进入等待此对象的等待锁定池，只有针对此对象调用 notify() 方法后本线程才进入对象锁定池准备，获取对象锁进入运行状态。



### 5、volatile 是什么？可以保证有序性吗？

一旦一个共享变量（类的成员变量、类的静态成员变量）被 volatile 修饰之后，那么就具备了两层语义：

1）保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的，volatile 关键字会强制将修改的值立即写入主存。

2）禁止进行指令重排序。

volatile 不是原子性操作

什么叫保证部分有序性？

当程序执行到 volatile 变量的读操作或者写操作时，在其前面的操作的更改肯定全部已经进行，且结果已经对后面的操作可见；在其后面的操作肯定还没有进行；

```
x = 2; //语句1
y = 0; //语句2
flag = true; //语句3
x = 4; //语句4
y = -1; //语句5
```

由于 flag 变量为 volatile 变量，那么在进行指令重排序的过程的时候，不会将语句 3 放到语句 1、语句 2 前面，也不会将语句 3 放到语句4、语句 5 后面。但是要注意语句 1 和语句 2 的顺序、语句 4 和语句 5 的顺序是不作任何保证的。

**使用 volatile 一般用于状态标记量和单例模式的双检锁。**



### 6、Thread 类中的 start() 和 run() 方法有什么区别？

start() 方法被用来启动新创建的线程，而且 start() 内部调用了 run() 方法，这和直接调用 run() 方法的效果不一样。当你调用 run() 方法的时候，只会是在原来的线程中调用，没有新的线程启动，start() 方法才会启动新线程。



### 7、为什么 wait，notify 和 notifyAll 这些方法不在 thread 类里面？

明显的原因是 JAVA 提供的锁是对象级的而不是线程级的，每个对象都有锁，通过线程获得。如果线程需要等待某些锁那么调用对象中的wait() 方法就有意义了。如果 wait() 方法定义在 Thread 类中，线程正在等待的是哪个锁就不明显了。简单的说，由于 wait，notify 和notifyAll 都是锁级别的操作，所以把他们定义在 Object 类中因为锁属于对象。



### 8、为什么 wait 和 notify 方法要在同步块中调用？

1. 只有在调用线程拥有某个对象的独占锁时，才能够调用该对象的 wait()，notify() 和 notifyAll() 方法。

2. 如果你不这么做，你的代码会抛出 IllegalMonitorStateException 异常。
3. 还有一个原因是为了避免 wait 和 notify 之间产生竞态条件。

wait() 方法强制当前线程释放对象锁。这意味着在调用某对象的 wait() 方法之前，当前线程必须已经获得该对象的锁。因此，线程必须在某个对象的同步方法或同步代码块中才能调用该对象的 wait() 方法。

在调用对象的 notify() 和 notifyAll() 方法之前，调用线程必须已经得到该对象的锁。因此，必须在某个对象的同步方法或同步代码块中才能调用该对象的 notify() 或 notifyAll() 方法。

调用 wait() 方法的原因通常是，调用线程希望某个特殊的状态（或变量）被设置之后再继续执行。调用 notify() 或 notifyAll() 方法的原因通常是，调用线程希望告诉其他等待中的线程："特殊状态已经被设置"。这个状态作为线程间通信的通道，它必须是一个可变的共享状态（或变量）	

![](https://dream-syz.github.io/3-1.png)		



### 9、Java 中 interrupted 和 isInterrupted 方法的区别？

![](https://dream-syz.github.io/3-2.png)

interrupted() 和 isInterrupted() 的主要区别是前者会将中断状态清除而后者不会。Java 多线程的中断机制是用内部标识来实现的，调用`Thread.interrupt()` 来中断一个线程就会设置中断标识为 true。当中断线程调用静态方法 `Thread.interrupted()` 来检查中断状态时，中断状态会被清零。而非静态方法 isInterrupted() 用来查询其它线程的中断状态且不会改变中断状态标识。简单的说就是任何抛出InterruptedException 异常的方法都会将中断状态清零。无论如何，一个线程的中断状态有有可能被其它线程调用中断来改变。



### 10、Java 中 synchronized 和 ReentrantLock 有什么不同？

相似点：

这两种同步方式有很多相似之处，它们都是加锁方式同步，而且都是阻塞式的同步，也就是说当如果一个线程获得了对象锁，进入了同步块，其他访问该同步块的线程都必须阻塞在同步块外面等待，而进行线程阻塞和唤醒的代价是比较高的.

区别：

这两种方式最大区别就是对于 Synchronized 来说，它是 java 语言的关键字，是原生语法层面的互斥，需要 jvm 实现。而 ReentrantLock 它是 JDK 1.5 之后提供的 API 层面的互斥锁，需要 lock() 和 unlock() 方法配合 try / finally 语句块来完成。

Synchronized 进过编译，会在同步块的前后分别形成 monitorenter 和 monitorexit 这个两个字节码指令。在执行 monitorenter 指令时，首先要尝试获取对象锁。如果这个对象没被锁定，或者当前线程已经拥有了那个对象锁，把锁的计算器加 1，相应的，在执行monitorexit 指令时会将锁计算器就减 1，当计算器为 0 时，锁就被释放了。如果获取对象锁失败，那当前线程就要阻塞，直到对象锁被另一个线程释放为止。

由于 ReentrantLock 是 `java.util.concurrent` 包下提供的一套互斥锁，相比 Synchronized，ReentrantLock 类提供了一些高级功能，主要有以下3项：

1.等待可中断，持有锁的线程长期不释放的时候，正在等待的线程可以选择放弃等待，这相当于Synchronized 来说可以避免出现死锁的情况。

2.公平锁，多个线程等待同一个锁时，必须按照申请锁的时间顺序获得锁，Synchronized 锁非公平锁，ReentrantLock 默认的构造函数是创建的非公平锁，可以通过参数 true 设为公平锁，但公平锁表现的性能不是很好。

3.锁绑定多个条件，一个 ReentrantLock 对象可以同时绑定多个对象。			



### 11、有三个线程 T1、T2、T3 如何保证顺序执行？		

在多线程中有多种方法让线程按特定顺序执行，你可以用线程类的 join() 方法在一个线程中启动另一个线程，另外一个线程完成该线程继续执行。为了确保三个线程的顺序你应该先启动最后一个（ T3 调用 T2，T2调用 T1），这样 T1 就会先完成而 T3 最后完成。

实际上先启动三个线程中哪一个都行， 因为在每个线程的run方法中用join方法限定了三个线程的执行顺序。

```java
public class JoinTest2 {
 // 1.现在有T1、T2、T3三个线程，你怎样保证T2在T1执行完后执行，T3在T2执行完后执行
   public static void main(String[] args) {
     final Thread t1 = new Thread(new Runnable() {
       @Override
       public void run() {
 		 System.out.println("t1");
 	   }
     });
   final Thread t2 = new Thread(new Runnable() {
 	@Override
	 public void run() {
 	   try {
 		// 引用t1线程，等待t1线程执行完
 		t1.join();
 	   } catch (InterruptedException e) {
 		e.printStackTrace();
 	   }
 	 System.out.println("t2");
 	}
   });
   Thread t3 = new Thread(new Runnable() {
 	 @Override
 	 public void run() {
 	   try {
 	      // 引用t2线程，等待t2线程执行完
 		  t2.join();
 	   } catch (InterruptedException e) {
 		  e.printStackTrace();
 	   }
 	   System.out.println("t3");
 	 }
   });
   t3.start();//这里三个线程的启动顺序可以任意，大家可以试下！
   t2.start();
   t1.start();
  }
}
```



### 12、SynchronizedMap 和 ConcurrentHashMap 有什么区别？

SynchronizedMap() 和 Hashtable 一样，实现上在调用 map 所有方法时，都对整个 map 进行同步。而 ConcurrentHashMap 的实现却更加精细，它对 map 中的所有桶加了锁。所以，只要有一个线程访问 map，其他线程就无法进入 map，而如果一个线程在访问ConcurrentHashMap 某个桶时，其他线程，仍然可以对 map 执行某些操作。

所以，ConcurrentHashMap 在性能以及安全性方面，明显比 Collections.synchronizedMap() 更加有优势。同时，同步操作精确控制到桶，这样，即使在遍历 map 时，如果其他线程试图对 map 进行数据修改，也不会抛出 ConcurrentModificationException。											



### 13、什么是线程安全

线程安全就是说多线程访问同一段代码，不会产生不确定的结果。

又是一个理论的问题，各式各样的答案有很多，我给出一个个人认为解释地最好的：**如果你的代码在多线程下执行和在单线程下执行永远都能获得一样的结果，那么你的代码就是线程安全的**。

这个问题有值得一提的地方，就是线程安全也是有几个级别的：

（1）不可变

像 String、Integer、Long 这些，都是 final 类型的类，任何一个线程都改变不了它们的值，要改变除非新创建一个，因此这些不可变对象不需要任何同步手段就可以直接在多线程环境下使用

（2）绝对线程安全

不管运行时环境如何，调用者都不需要额外的同步措施。要做到这一点通常需要付出许多额外的代价，Java 中标注自己是线程安全的类，实际上绝大多数都不是线程安全的，不过绝对线程安全的类，Java 中也有，比方说 CopyOnWriteArrayList、CopyOnWriteArraySet

（3）相对线程安全

相对线程安全也就是我们通常意义上所说的线程安全，像 Vector 这种，add、remove 方法都是原子操作，不会被打断，但也仅限于此，如果有个线程在遍历某个 Vector、有个线程同时在 add 这个 Vector，99 % 的情况下都会出现 ConcurrentModificationException，也就是**fail-fast 机制**。

（4）线程非安全

这个就没什么好说的了，ArrayList、LinkedList、HashMap 等都是线程非安全的类



### 14、Thread 类中的 yield 方法有什么作用？

Yield 方法可以暂停当前正在执行的线程对象，让其它有相同优先级的线程执行。它是一个静态方法而且只保证当前线程放弃 CPU 占用而不能保证使其它线程一定能占用 CPU，执行 yield() 的线程有可能在进入到暂停状态后马上又被执行。



### 15、Java 线程池中 submit() 和 execute() 方法有什么区别？

两个方法都可以向线程池提交任务，execute() 方法的返回类型是 void，它定义在 Executor 接口，b而 submit() 方法可以返回持有计算结果的 Future 对象，它定义在 ExecutorService 接口中，它扩展了 Executor 接口，其它线程池类像 ThreadPoolExecutor 和ScheduledThreadPoolExecutor 都有这些方法。



### 16、说一说自己对于 synchronized 关键字的了解

synchronized 关键字解决的是多个线程之间访问资源的同步性，synchronized 关键字可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。 另外，在 Java 早期版本中，synchronized 属于重量级锁，效率低下，因为监视器锁（monitor）是依赖于底层的操作系统的 Mutex Lock 来实现的，Java 的线程是映射到操作系统的原生线程之上的。如果要挂起或者唤醒一个线程，都需要操作系统帮忙完成，而操作系统实现线程之间的切换时需要从用户态转换到内核态，这个状态之间的转换需要相对比较长的时间，时间成本相对较高，这也是为什么早期的 synchronized 效率低的原因。庆幸的是在 Java 6 之后 Java 官方对从 JVM 层面对 synchronized 做了较大优化，所以现在的 synchronized 锁效率也优化得很不错了。JDK1.6 对锁的实现引入了大量的优化，如自旋锁、适应性自旋锁、锁消除、锁粗化、偏向锁、轻量级锁等技术来减少锁操作的开销。



### 17、说说自己是怎么使用 synchronized 关键字？

**修饰实例方法**:：作用于当前对象实例加锁，进入同步代码前要获得当前对象实例的锁 

**修饰静态方法**：也就是给当前类加锁，会作用于类的所有对象实例，因为静态成员不属于任何一个实例对象，是类成员（ static 表明这是该类的一个静态资源，不管 new 了多少个对象，只有一份）。所以如果一个线程 A 调用一个实例对象的非静态 synchronized 方法，而线程 B 需要调用这个实例对象所属类的静态 synchronized 方法，是允许的，不会发生互斥现象，**因为访问静态** **synchronized** **方法占用的锁是当前类的锁，而访问非静态** **synchronized** **方法占用的锁是当前实例对象锁。 **

**修饰代码块**：指定加锁对象，对给定对象加锁，进入同步代码库前要获得给定对象的锁。

**总结**： synchronized 关键字加到 static 静态方法和 synchronized(class) 代码块上都是是给 Class 类上锁。synchronized 关键字加到实例方法上是给对象实例上锁。尽量不要使用 synchronized(String a) 因为 JVM 中，字符串常量池具有缓存功能！会导致加锁在同一个对象，必然会阻塞



### 18、什么是线程安全？Vector 是一个线程安全类吗？

如果你的代码所在的进程中有多个线程在同时运行，而这些线程可能会同时运行这段代码。如果每次运行结果和单线程运行的结果是一样的，而且其他的变量的值也和预期的是一样的，就是线程安全的。一个线程安全的计数器类的同一个实例对象在被多个线程使用的情况下也不会出现计算失误。很显然你可以将集合类分成两组，线程安全和非线程安全的。Vector 是用同步方法来实现线程 安全的, 而和它相似的 ArrayList 不是线程安全的。



### 19、volatile 关键字的作用？

一旦一个共享变量（类的成员变量、类的静态成员变量）被 volatile 修饰之后，那么就具备了两层语义：

- 保证了不同线程对这个变量进行操作时的可见性，即一个线程修改了某个变量的值，这新值对其他线程来说是立即可见的。

- 禁止进行指令重排序。

volatile  与 synchronized  区别（5 个）：

- volatile 本质是在告诉 jvm 当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取；

  synchronized 则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。

- volatile 仅能使用在变量级别；

  synchronized 则可以使用在变量、方法、和类级别的。

- volatile 仅能实现变量的修改可见性，并不能保证原子性；

  synchronized 则可以保证变量的修改可见性和原子性。

- volatile 不会造成线程的阻塞；

  synchronized 可能会造成线程的阻塞。

- volatile 标记的变量不会被编译器优化；

  synchronized标记的变量可以被编译器优化。



### 20、常用的线程池有哪些？

- newSingleThreadExecutor：创建一个单线程的线程池，此线程池保证所有任务的执行顺序按照任务的提交顺序执行。

- newFixedThreadPool：创建固定大小的线程池，它的核心线程数和最大线程数是一样的，每次提交一个任务就创建一个线程，直到线程达到线程池的最大大小。

- newCachedThreadPool：创建一个可缓存的线程池，它的特点就是可以无限的缓存线程任务，最大可以达到 Integer.MAX_VALUE，为 2^31-1，此线程池不会对线程池大小做限制，线程池大小完全依赖于操作系统（或者说JVM）能够创建的最大线程大小。
- newScheduledThreadPool：创建一个大小无限的线程池，此线程池支持定时以及周期性执行任务的需求。

- newSingleThreadScheduledExecutor：创建一个单线程的线程池。此线程池支持定时以及周期性执行任务的需求



### 21、简述一下你对线程池的理解

（如果问到了这样的问题，可以展开的说一下线程池如何用、线程池的好处、线程池的启动策略）

合理利用线程池能够带来三个好处。

第一：降低资源消耗。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。

第二：提高响应速度。当任务到达时，任务可以不需要等到线程创建就能立即执行。

第三：提高线程的可管理性。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。



### 22、Java 程序是如何执行的

我们日常的工作中都使用开发工具（IntelliJ IDEA 或 Eclipse 等）可以很方便的调试程序，或者是通过打包工具把项目打包成 jar 包或者 war 包，放入 Tomcat 等 Web 容器中就可以正常运行了，但你有没有想过 Java 程序内部是如何执行的？其实不论是在开发工具中运行还是在 Tomcat 中运行，Java 程序的执行流程基本都是相同的，它的执行流程如下：

- 先把 Java 代码编译成字节码，也就是把 .java 类型的文件编译成 .class 类型的文件。这个过程的大致执行流程：Java 源代码 -> 词法分析器 -> 语法分析器 -> 语义分析器 -> 字符码生成器 ->最终生成字节码，其中任何一个节点执行失败就会造成编译失败；

- 把 class 文件放置到 Java 虚拟机，这个虚拟机通常指的是 Oracle 官方自带的 Hotspot JVM；

- Java 虚拟机使用类加载器（Class Loader）装载 class 文件；

- 类加载完成之后，会进行字节码效验，字节码效验通过之后 JVM 解释器会把字节码翻译成机器码交由操作系统执行。但不是所有代码都是解释执行的，JVM 对此做了优化，比如，以 Hotspot 虚拟机来说，它本身提供了 JIT（Just In Time）也就是我们通常所说的动态编译器，它能够在运行时将热点代码编译为机器码，这个时候字节码就变成了编译执行。Java 程序执行流程图如下：

![](https://dream-syz.github.io/3-3.png)



### 23、锁的优化机制了解吗？

从 JDK 1.6 版本之后，synchronized 本身也在不断优化锁的机制，有些情况下他并不会是一个很重量级的锁了。优化机制包括自适应锁、自旋锁、锁消除、锁粗化、轻量级锁和偏向锁。

锁的状态从低到高依次为 **无锁** -> **偏向锁** -> **轻量级锁** -> **重量级锁**，升级的过程就是从低到高，降级在一定条件也是有可能发生的。

**自旋锁**：由于大部分时候，锁被占用的时间很短，共享变量的锁定时间也很短，所有没有必要挂起线程，用户态和内核态的来回上下文切换严重影响性能。自旋的概念就是让线程执行一个忙循环，可以理解为就是啥也不干，防止从用户态转入内核态，自旋锁可以通过设置    -XX:+UseSpining 来开启，自旋的默认次数是 10 次，可以使用 -XX:PreBlockSpin 设置。

**自适应锁**：自适应锁就是自适应的自旋锁，自旋的时间不是固定时间，而是由前一次在同一个锁上的自旋时间和锁的持有者状态来决定。

**锁消除**：锁消除指的是 JVM 检测到一些同步的代码块，完全不存在数据竞争的场景，也就是不需要加锁，就会进行锁消除。

**锁粗化**：锁粗化指的是有很多操作都是对同一个对象进行加锁，就会把锁的同步范围扩展到整个操作序列之外。

**偏向锁**：当线程访问同步块获取锁时，会在对象头和栈帧中的锁记录里存储偏向锁的线程 ID，之后这个线程再次进入同步块时都不需要CAS 来加锁和解锁了，偏向锁会永远偏向第一个获得锁的线程，如果后续没有其他线程获得过这个锁，持有锁的线程就永远不需要进行同步，反之，当有其他线程竞争偏向锁时，持有偏向锁的线程就会释放偏向锁。可以用过设置 -XX:+UseBiasedLocking 开启偏向锁。

**轻量级锁**：JVM 的对象的对象头中包含有一些锁的标志位，代码进入同步块的时候，JVM 将会使用 CAS 方式来尝试获取锁，如果更新成功则会把对象头中的状态位标记为轻量级锁，如果更新失败，当前线程就尝试自旋来获得锁。整个锁升级的过程非常复杂，我尽力去除一些无用的环节，简单来描述整个升级的机制。简单点说，偏向锁就是通过对象头的偏向线程 ID 来对比，甚至都不需要 CAS 了，而轻量级锁主要就是通过 CAS 修改对象头锁记录和自旋来实现，重量级锁则是除了拥有锁的线程其他全部阻塞。

![](https://dream-syz.github.io/3-4.png)



### 24、说说进程和线程的区别？

1. 进程是一个“执行中的程序”，是系统进行资源分配和调度的一个独立单位。
2. 线程是进程的一个实体，一个进程中拥有多个线程，线程之间共享地址空间和其它资源（所以通信和同步等操作线程比进程更加容易）

3. 线程上下文的切换比进程上下文切换要快很多。

- （1）进程切换时，涉及到当前进程的 CPU 环境的保存和新被调度运行进程的 CPU 环境的设置。

- （2）线程切换仅需要保存和设置少量的寄存器内容，不涉及存储管理方面的操作。



### 25、产生死锁的四个必要条件？

1. 互斥条件：一个资源每次只能被一个线程使用
2. 请求与保持条件：一个线程因请求资源而阻塞时，对已获得的资源保持不放
3. 不剥夺条件：进程已经获得的资源，在未使用完之前，不能强行剥夺
4. 循环等待条件：若干线程之间形成一种头尾相接的循环等待资源关系



### 26、如何避免死锁？

指定获取锁的顺序，举例如下：

1. 比如某个线程只有获得 A 锁和 B 锁才能对某资源进行操作，在多线程条件下，如何避免死锁？
2. 获得锁的顺序是一定的，比如规定，只有获得 A 锁的线程才有资格获取 B 锁，按顺序获取锁就可以避免死锁！！！



### 27、线程池核心线程数怎么设置呢？

分为 CPU 密集型和 IO 密集型

**CPU**

这种任务消耗的主要是 CPU 资源，可以将线程数设置为 N（CPU 核心数）+1，比 CPU 核心数多出来的一个线程是为了防止线程偶发的缺页中断，或者其它原因导致的任务暂停而带来的影响。一旦任务暂停，CPU 就会处于空闲状态，而在这种情况下多出来的一个线程就可以充分利用 CPU 的空闲时间。

**IO 密集型**

这种任务应用起来，系统会用大部分的时间来处理 I/O 交互，而线程在处理 I/O 的时间段内不会占用 CPU 来处理，这时就可以将 CPU 交出给其它线程使用。因此在 I/O 密集型任务的应用中，我们可以多配置一些线程，具体的计算方法是：核心线程数 = CPU 核心数量 * 2。



### 28、Java 线程池中队列常用类型有哪些？

- ArrayBlockingQueue 是一个基于数组结构的 **有界阻塞队列**，此队列按 FIFO（先进先出）原则对元素进行排序。

- LinkedBlockingQueue 一个基于链表结构的 **阻塞队列**，此队列按 FIFO（先进先出） 排序元素，吞吐量通常要高于 ArrayBlockingQueue 。

- SynchronousQueue 一个不存储元素的 **阻塞队列**。

- PriorityBlockingQueue 一个具有优先级的 **无限阻塞队列**。PriorityBlockingQueue 也是 **基于最小二叉堆实现**

- DelayQueue 
  - 只有当其指定的延迟时间到了，才能够从队列中获取到该元素。
  - DelayQueue 是一个没有大小限制的队列， 
  - 因此往队列中插入数据的操作（生产者）永远不会被阻塞，而只有获取数据的操作（消费者）才会被阻塞。

这里能说出前三种也就差不多了，如果能说全那是最好。



### 29、线程安全需要保证几个基本特征？

**原子性**，简单说就是相关操作不会中途被其他线程干扰，一般通过同步机制实现。

**可见性**，是一个线程修改了某个共享变量，其状态能够立即被其他线程知晓，通常被解释为将线程本地状态反映到主内存上，volatile 就是负责保证可见性的。

**有序性**，是保证线程内串行语义，避免指令重排等。



### 30、说一下线程之间是如何通信的？

线程之间的通信有两种方式：共享内存和消息传递。

**共享内存**

在共享内存的并发模型里，线程之间共享程序的公共状态，线程之间通过写-读内存中的公共状态来隐式进行通信。典型的共享内存通信方式，就是通过共享对象进行通信。

例如上图线程 A 与 线程 B 之间如果要通信的话，那么就必须经历下面两个步骤：

1. 线程 A 把本地内存 A 更新过得共享变量刷新到主内存中去。
2. 线程 B 到主内存中去读取线程 A 之前更新过的共享变量。

**消息传递**

在消息传递的并发模型里，线程之间没有公共状态，线程之间必须通过明确的发送消息来显式进行通信。在 Java 中典型的消息传递方式，就是 wait() 和 notify() ，或者 BlockingQueue 。



### 31、CAS 的原理呢？

CAS 叫做 CompareAndSwap，比较并交换，主要是通过处理器的指令来保证操作的原子性，它包含三个操作数：

1. 变量内存地址，V 表示
2. 旧的预期值，A 表示
3. 准备设置的新值，B 表示

当执行 CAS 指令时，只有当 V 等于 A 时，才会用 B 去更新 V 的值，否则就不会执行更新操作。



### 32、CAS 有什么缺点吗？

CAS 的缺点主要有 3 点：

**ABA 问题**：ABA 的问题指的是在 CAS 更新的过程中，当读取到的值是 A，然后准备赋值的时候仍然是 A，但是实际上有可能 A 的值被改成了 B，然后又被改回了 A，这个 CAS 更新的漏洞就叫做 ABA。只是 ABA 的问题大部分场景下都不影响并发的最终效果。

Java 中有 AtomicStampedReference 来解决这个问题，他加入了预期标志和更新后标志两个字段，更新时不光检查值，还要检查当前的标志是否等于预期标志，全部相等的话才会更新。

**循环时间长开销大**：自旋 CAS 的方式如果长时间不成功，会给 CPU 带来很大的开销。

**只能保证一个共享变量的原子操作**：只对一个共享变量操作可以保证原子性，但是多个则不行，多个可以通过 AtomicReference 来处理或者使用锁 synchronized 实现。



### 33、**说说** ThreadLocal 原理？

ThreadLocal 可以理解为线程本地变量，他会在每个线程都创建一个副本，那么在线程之间访问内部副本变量就行了，做到了线程之间互相隔离，相比于 synchronized 的做法是用空间来换时间。

ThreadLocal 有一个静态内部类 ThreadLocalMap，ThreadLocalMap 又包含了一个 Entry 数组，Entry 本身是一个弱引用，它的 key 是指向 ThreadLocal 的弱引用，Entry 具备了保存 key value 键值对的能。

弱引用的目的是为了防止内存泄露，如果是强引用那么 ThreadLocal 对象除非线程结束否则始终无法被回收，弱引用则会在下一次 GC 的时候被回收。

但是这样还是会存在内存泄露的问题，假如 key 和 ThreadLocal 对象被回收之后，entry 中就存在 key 为 null，但是 value 有值的 entry对象，但是永远没办法被访问到，同样除非线程结束运行。

但是只要 ThreadLocal 使用恰当，在使用完之后调用 remove 方法删除 Entry 对象，实际上是不会出现这个问题的。

![](https://dream-syz.github.io/3-5.png)



### 34、线程池原理知道吗？以及核心参数

首先线程池有几个核心的参数概念：

1. 最大线程数 maximumPoolSize
2. 核心线程数 corePoolSize
3. 活跃时间 keepAliveTime
4. 阻塞队列 workQueue
5. 拒绝策略 RejectedExecutionHandler

当提交一个新任务到线程池时，具体的执行流程如下：

1. 当我们提交任务，线程池会根据 corePoolSize 大小创建若干任务数量线程执行任务
2. 当任务的数量超过 corePoolSize 数量，后续的任务将会进入阻塞队列阻塞排队
3. 当阻塞队列也满了之后，那么将会继续创建（maximumPoolSize-corePoolSize）个数量的线程来执行任务，如果任务处理完成，maximumPoolSize-corePoolSize 额外创建的线程等待 keepAliveTime 之后被自动销毁

4. 如果达到 maximumPoolSize，阻塞队列还是满的状态，那么将根据不同的拒绝策略对应处理

![](https://dream-syz.github.io/3-6.png)



### 35、 线程池的拒绝策略有哪些？

主要有 4 种拒绝策略：

1. AbortPolicy：直接丢弃任务，抛出异常，这是默认策略
2. CallerRunsPolicy：只用调用者所在的线程来处理任务
3. DiscardOldestPolicy：丢弃等待队列中最旧的任务，并执行当前任务
4. DiscardPolicy：直接丢弃任务，也不抛出异常



### 36、说说你对 JMM 内存模型的理解？为什么需要 JMM ？

随着 CPU 和内存的发展速度差异的问题，导致 CPU 的速度远快于内存，所以现在的 CPU 加入了高速缓存，高速缓存一般可以分为 L1、L2、L3 三级缓存。基于上面的例子我们知道了这导致了缓存一致性的问题，所以加入了缓存一致性协议，同时导致了内存可见性的问题，而编译器和 CPU 的重排序导致了原子性和有序性的问题，JMM 内存模型正是对多线程操作下的一系列规范约束，因为不可能让陈雇员的代码去兼容所有的 CPU，通过 JMM 我们才屏蔽了不同硬件和操作系统内存的访问差异，这样保证了 Java 程序在不同的平台下达到一致的内存访问效果，同时也是保证在高效并发的时候程序能够正确执行。

![](https://dream-syz.github.io/3-7.png)

**原子性**：Java 内存模型通过 read、load、assign、use、store、write 来保证原子性操作，此外还有 lock 和 unlock，直接对应着synchronized 关键字的 monitorenter 和 monitorexit 字节码指令。

**可见性**：可见性的问题在上面的回答已经说过，Java 保证可见性可以认为通过 volatile、synchronized、final 来实现。

**有序性**：由于处理器和编译器的重排序导致的有序性问题，Java 通过 volatile、synchronized 来保证。

**happen-before 规则**

虽然指令重排提高了并发的性能，但是 Java 虚拟机会对指令重排做出一些规则限制，并不能让所有的指令都随意的改变执行位置，主要有以下几点：

1. 单线程每个操作，happen-before 于该线程中任意后续操作
2. volatile 写 happen-before 与后续对这个变量的读
3. synchronized 解锁 happen-before 后续对这个锁的加锁
4. final 变量的写 happen-before 于 final 域对象的读，happen-before 后续对 final 变量的读
5. 传递性规则，A 先于 B，B 先于 C，那么 A 一定先于 C 发生

**说了半天，到底工作内存和主内存是什么？**

主内存可以认为就是物理内存，Java 内存模型中实际就是虚拟机内存的一部分。而工作内存就是 CPU 缓存，他有可能是寄存器也有可能是 L1 \ L2 \ L3 缓存，都是有可能的。



### 37、多线程有什么用？

一个可能在很多人看来很扯淡的一个问题：我会用多线程就好了，还管它有什么用？在我看来，这个回答更扯淡。所谓"知其然知其所以然"，"会用"只是"知其然"，"为什么用"才是"知其所以然"，只有达到"知其然知其所以然"的程度才可以说是把一个知识点运用自如。OK，下面说说我对这个问题的看法：

#### （1）发挥多核 CPU 的优势

随着工业的进步，现在的笔记本、台式机乃至商用的应用服务器至少也都是双核的，4 核、8 核甚至 16 核的也都不少见，如果是单线程的程序，那么在双核 CPU 上就浪费了 50 %，在 4 核 CPU 上就浪费了 75 %。**单核 CPU 上所谓的“多线程”那是假的多线程，同一时间处理器只会处理一段逻辑，只不过线程之间切换得比较快，看着像多个线程"同时"运行罢了**。多核 CPU 上的多线程才是真正的多线程，它能让你的多段逻辑同时工作，多线程，可以真正发挥出多核 CPU 的优势来，达到充分利用 CPU 的目的。

#### （2）防止阻塞

从程序运行效率的角度来看，单核 CPU 不但不会发挥出多线程的优势，反而会因为在单核 CPU 上运行多线程导致线程上下文的切换，而降低程序整体的效率。但是单核 CPU 我们还是要应用多线程，就是为了防止阻塞。试想，如果单核 CPU 使用单线程，那么只要这个线程阻塞了，比方说远程读取某个数据吧，对端迟迟未返回又没有设置超时时间，那么你的整个程序在数据返回回来之前就停止运行了。多线程可以防止这个问题，多条线程同时运行，哪怕一条线程的代码执行读取数据阻塞，也不会影响其它任务的执行。

#### （3）便于建模

这是另外一个没有这么明显的优点了。假设有一个大的任务 A，单线程编程，那么就要考虑很多，建立整个程序模型比较麻烦。但是如果把这个大的任务 A 分解成几个小任务，任务 B、任务 C、任务 D，分别建立程序模型，并通过多线程分别运行这几个任务，那就简单很多了。



### 38、说说 CyclicBarrier 和 CountDownLatch 的区别？

两个看上去有点像的类，都在 `java.util.concurrent` 下，都可以用来表示代码运行到某个点上，二者的区别在于：

（1）CyclicBarrier 的某个线程运行到某个点上之后，该线程即停止运行，直到所有的线程都到达了这个点，所有线程才重新运行；             

          CountDownLatch 则不是，某线程运行到某个点上之后，只是给某个数值 -1 而已，该线程继续运行

（2）CyclicBarrier 只能唤起一个任务，CountDownLatch 可以唤起多个任务

（3）CyclicBarrier 可重用，CountDownLatch 不可重用，计数值为 0 该 CountDownLatch 就不可再用了



### 39、什么是 AQS ？

简单说一下 AQS，AQS 全称为 AbstractQueuedSychronizer，翻译过来应该是 **抽象队列同步器**。如果说 `java.util.concurrent` 的基础是 CAS 的话，那么 AQS 就是整个 Java 并发包的核心了，ReentrantLock、CountDownLatch、Semaphore 等等都用到了它。AQS 实际上以双向队列的形式连接所有的 Entry，比方说 ReentrantLock，所有等待的线程都被放在一个 Entry 中并连成双向队列，前面一个线程使用 ReentrantLock 好了，则双向队列实际上的第一个 Entry 开始运行。

AQS 定义了对双向队列所有的操作，而只开放了 tryLock 和 tryRelease 方法给开发者使用，开发者可以根据自己的实现重写 tryLock 和tryRelease 方法，以实现自己的并发功能。



### 40、了解 Semaphore 吗？

Semaphore 就是一个信号量，它的作用是 **限制某段代码块的并发数**。Semaphore 有一个构造函数，可以传入一个 int 型整数 n，表示某段代码最多只有 n 个线程可以访问，如果超出了 n，那么请等待，等到某个线程执行完毕这段代码块，下一个线程再进入。由此可以看出如果 Semaphore 构造函数中传入的 int 型整数 n = 1，相当于变成了一个 synchronized 了。



### 41、什么是 Callable 和 Future？

Callable 接口类似于 Runnable，从名字就可以看出来了，但是 Runnable 不会返回结果，并且无法抛出返回结果的异常，而 Callable 功能更强大一些，被线程执行后，可以返回值，这个返回值可以被 Future 拿到，也就是说，Future 可以拿到异步执行任务的返回值。可以认为是带有回调的 Runnable。

Future 接口表示异步任务，是还没有完成的任务给出的未来结果。所以说 Callable 用于产生结果，Future 用于获取结果。



### 42、什么是阻塞队列？阻塞队列的实现原理是什么？如何使用阻塞队列来实现生产者 - 消费者模型？

阻塞队列（BlockingQueue）是一个支持两个附加操作的队列。

这两个附加的操作是：在队列为空时，获取元素的线程会等待队列变为非空。当队列满时，存储元素的线程会等待队列可用。

阻塞队列常用于生产者和消费者的场景，生产者是往队列里添加元素的线程，消费者是从队列里拿元素的线程。阻塞队列就是生产者存放元素的容器，而消费者也只从容器里拿元素。

JDK 7 提供了 7 个阻塞队列。分别是：

- ArrayBlockingQueue ：一个由数组结构组成的有界阻塞队列。

- LinkedBlockingQueue ：一个由链表结构组成的有界阻塞队列。

- PriorityBlockingQueue ：一个支持优先级排序的无界阻塞队列。

- DelayQueue：一个使用优先级队列实现的无界阻塞队列。

- SynchronousQueue：一个不存储元素的阻塞队列。

- LinkedTransferQueue：一个由链表结构组成的无界阻塞队列。

- LinkedBlockingDeque：一个由链表结构组成的双向阻塞队列。

Java 5 之前实现同步存取时，可以使用普通的一个集合，然后在使用线程的协作和线程同步可以实现生产者 - 消费者模式，主要的技术就是用好 wait，notify，notifyAll，sychronized 这些关键字。而在 java 5 之后，可以使用阻塞队列来实现，此方式大大简少了代码量，使得多线程编程更加容易，安全方面也有保障。

BlockingQueue 接口是 Queue 的子接口，它的主要用途并不是作为容器，而是作为线程同步的的工具，因此他具有一个很明显的特性，当生产者线程试图向 BlockingQueue 放入元素时，如果队列已满，则线程被阻塞，当消费者线程试图从中取出一个元素时，如果队列为空，则该线程会被阻塞，正是因为它所具有这个特性，所以在程序中多个线程交替向 BlockingQueue 中放入元素，取出元素，它可以很好的控制线程之间的通信。

阻塞队列使用最经典的场景就是 socket 客户端数据的读取和解析，读取数据的线程不断将数据放入队列，然后解析线程不断从队列取数据解析。



### 43、什么是多线程中的上下文切换？

在上下文切换过程中，CPU 会停止处理当前运行的程序，并保存当前程序运行的具体位置以便之后继续运行。从这个角度来看，上下文切换有点像我们同时阅读几本书，在来回切换书本的同时我们需要记住每本书当前读到的页码。

在程序中，上下文切换过程中的“页码”信息是保存在进程控制块（PCB）中的。PCB 还经常被称作“切换桢”（switchframe）。“页码”信息会一直保存到 CPU 的内存中，直到他们被再次使用。

上下文切换是存储和恢复 CPU 状态的过程，它使得线程执行能够从中断点恢复执行。上下文切换是多任务操作系统和多线程环境的基本特征。



### 44、什么是 Daemon 线程？它有什么意义？

所谓后台（daemon）线程，也叫守护线程，是指在程序运行的时候在后台提供一种通用服务的线程，并且这个线程并不属于程序中不可或缺的部分。

因此，当所有的非后台线程结束时，程序也就终止了，同时会杀死进程中的所有后台线程。反过来说， 只要有任何非后台线程还在运行，程序就不会终止。

必须在线程启动之前调用 setDaemon() 方法，才能把它设置为后台线程。注意：后台进程在不执行 finally 子句的情况下就会终止其 run() 方法。

比如：JVM 的垃圾回收线程就是 Daemon 线程，Finalizer 也是守护线程。



### 45、乐观锁和悲观锁的理解及如何实现，有哪些实现方式？

**悲观锁：**总是假设最坏的情况，每次去拿数据的时候都认为别人会修改，所以每次在拿数据的时候都会上锁，这样别人想拿这个数据就会阻塞直到它拿到锁。

传统的关系型数据库里边就用到了很多这种锁机制，比如行锁，表锁等，读锁，写锁等，都是在做操作之前先上锁。再比如 Java 里面的同步原语 synchronized 关键字的实现也是悲观锁。

**乐观锁：**顾名思义，就是很乐观，每次去拿数据的时候都认为别人不会修改，所以不会上锁，但是在更新的时候会判断一下在此期间别人有没有去更新这个数据，可以使用版本号等机制。

乐观锁适用于多读的应用类型，这样可以提高吞吐量，像数据库提供的类似于 write_condition 机制，其实都是提供的乐观锁。在 Java 中 `java.util.concurrent.atomic` 包下面的原子变量类就是使用了乐观锁的一种实现方式 CAS 实现的。

**乐观锁的实现方式：**

1、使用版本标识来确定读到的数据与提交时的数据是否一致。提交后修改版本标识，不一致时可以采取丢弃和再次尝试的策略。

2、java 中的 Compare and Swap 即 CAS，当多个线程尝试使用 CAS 同时更新同一个变量时，只有其中一个线程能更新变量的值，而其它线程都失败，失败的线程并不会被挂起，而是被告知这次竞争中失败，并可以再次尝试。 CAS 操作中包含三个操作数 —— 需要读写的内存位置（V）、进行比较的预期原值（A）和拟写入的新值(B)。如果内存位置 V 的值与预期原值 A 相匹配，那么处理器会自动将该位置值更新为新值 B。否则处理器不做任何操作。

**CAS 缺点：**

1. **ABA 问题：**比如说一个线程 one 从内存位置 V 中取出 A，这时候另一个线程 two 也从内存中取出 A，并且 two 进行了一些操作变成了 B，然后 two 又将 V 位置的数据变成 A，这时候线程 one 进行 CAS 操作发现内存中仍然是 A，然后 one 操作成功。尽管线程 one的 CAS 操作成功，但可能存在潜藏的问题。从 Java 1.5 开始 JDK 的 atomic 包里提供了一个类 `AtomicStampedReference` 来解决ABA 问题。

2. **循环时间长开销大：**对于资源竞争严重（线程冲突严重）的情况，CAS 自旋的概率会比较大，从而浪费更多的 CPU 资源，效率低于 synchronized。

3. **只能保证一个共享变量的原子操作：**当对一个共享变量执行操作时，我们可以使用循环 CAS 的方式来保证原子操作，但是对多个共享变量操作时，循环 CAS 就无法保证操作的原子性，这个时候就可以用锁。



### 46、CopyOnWriteArrayList 应用场景

1、**读多写少的场景**：由于在写操作时需要复制一个新的数组，因此写的性能较差。而读操作则不会影响原来的数组，所以性能很高。适合于读多写少的场景。

2、**读操作优先场景**：由于 CopyOnWriteArrayList 在写操作时，所有读操作都不受到影响和阻塞。因此，当对数据的读操作次数比较多时，可以使用 CopyOnWriteArrayList 来提升系统的性能。

3、**数据更新要求不频繁的场景**: 在 CopyOnWriteArrayList 上，每次添加、修改或删除列表中的元素时，都需要重新创建一个新的底层数组，因此在实现上会消耗更多的内存空间。如果数据经常需要被更新，则建议使用普通的 ArrayList。

4、**互斥访问数据不方便的场景**: 在多线程环境下，如果对一个 ArrayList 实例进行访问，需要加锁保证数据一致性。但是，在某些场景下，加锁会给程序带来额外的复杂度和延迟。在这种情况下可以考虑使用 CopyOnWriteArrayList。

5、**高并发场景**：CopyOnWriteArrayList 在写操作时候有很高的并发度，不会阻塞其他的读操作。因此非常适合用于读多写少的场景下，可以提高系统的并发性能。

需要注意的是，CopyOnWriteArrayList 并不能解决所有多线程问题，它主要是针对读多写少的应用场景，所以开发人员在选择使用 CopyOnWriteArrayList 时，一定要结合实际需求，如果业务场景不适合使用该类，建议使用其它的集合类，或者自己实现一些更加适用的线程安全集合类。

总之，CopyOnWriteArrayList 适合于读多写少，读优先的场景，需要更新频率较低的数据，而且有运行效率限制的场景。因为它的底层实现方式比较特殊，它的读性能非常高，而写性能相对较差。对于需要快速修改的应用场景，可以考虑使用其他的 List 类来替代 CopyOnWriteArrayList 。





## Spring 篇

### 1、什么是 spring?

Spring 是个 java 企业级应用的开源开发框架。Spring 主要用来开发 Java 应用，但是有些扩展是针对构建 J2EE 平台的 web 应用。Spring 框架目标是简化 Java 企业级应用开发，并通过 POJO 为基础的编程模型促进良好的编程习惯。



### 2、你们项目中为什么使用 Spring 框架？

这么问的话，就直接说 Spring 框架的好处就可以了。比如说 Spring 有以下特点：

- **轻量：**Spring 是轻量的，基本的版本大约 2 MB。

- **控制反转：**Spring 通过控制反转实现了松散耦合，对象们给出它们的依赖，而不是创建或查找依赖的对象们。

- **面向切面的编程 (AOP) ：**Spring 支持面向切面的编程，并且把应用业务逻辑和系统服务分开。

- **容器：**Spring 包含并管理应用中对象的生命周期和配置。

- **MVC 框架**：Spring 的 WEB 框架是个精心设计的框架，是 Web 框架的一个很好的替代品。

- **事务管理：**Spring 提供一个持续的事务管理接口，可以扩展到上至本地事务下至全局事务（JTA）。

- **异常处理：**Spring 提供方便的 API 把具体技术相关的异常（比如由 JDBC，Hibernate or JDO 抛出的）转化为一致的 unchecked 异常。



### 3、Autowired 和 Resource 关键字的区别？

@Resource 和 @Autowired 都是做 bean 的注入时使用，其实 @Resource 并不是 Spring 的注解，它的包是`javax.annotation.Resource`，需要导入，但是 Spring 支持该注解的注入。

1、共同点

两者都可以写在字段和 setter 方法上。两者如果都写在字段上，那么就不需要再写 setter 方法。

2、不同点

（1）@Autowired

@Autowired 为 Spring 提供的注解，需要导入包 `org.springframework.beans.factory.annotation.Autowired`；只按照 byType 注入。

```
public class TestServiceImpl {
 // 下面两种@Autowired只要使用一种即可
 @Autowired
 private UserDao userDao; // 用于字段上
 
 @Autowired
 public void setUserDao(UserDao userDao) { // 用于属性的方法上
 this.userDao = userDao;
 }
}
```

@Autowired 注解是按照类型（byType）装配依赖对象，默认情况下它要求依赖对象必须存在，如果允许 null 值，可以设置它的required 属性为 false。如果我们想使用按照名称（byName）来装配，可以结合 @Qualifier 注解一起使用。如下：

```
public class TestServiceImpl {
 @Autowired
 @Qualifier("userDao")
 private UserDao userDao;
}
```

（2）@Resource

@Resource 默认按照 ByName 自动注入，由 J2EE 提供，需要导入包 `javax.annotation.Resource`。

@Resource 有两个重要的属性：name 和 type，而 Spring 将 @Resource 注解的 name 属性解析为 bean 的名字，而 type 属性则解析为 bean 的类型。所以，如果使用 name 属性，则使用 byName 的自动注入策略，而使用 type 属性时则使用 byType 自动注入策略。如果既不制定 name 也不制定 type 属性，这时将通过反射机制使用 byName 自动注入策略。

```
public class TestServiceImpl {
 // 下面两种@Resource只要使用一种即可
 @Resource(name="userDao")
 private UserDao userDao; // 用于字段上
 
 @Resource(name="userDao")
 public void setUserDao(UserDao userDao) { // 用于属性的setter方法上
 this.userDao = userDao;
 }
}
```

注：最好是将 @Resource 放在 setter 方法上，因为这样更符合面向对象的思想，通过 set、get 去操作属性，而不是直接去操作属性。

**@Resource 装配顺序：**

①如果同时指定了 name 和 type，则从 Spring 上下文中找到唯一匹配的 bean 进行装配，找不到则抛出异常。

②如果指定了 name，则从上下文中查找名称（id）匹配的 bean 进行装配，找不到则抛出异常。

③如果指定了 type，则从上下文中找到类似匹配的唯一 bean 进行装配，找不到或是找到多个，都会抛出异常。

④如果既没有指定 name，又没有指定 type，则自动按照 byName 方式进行装配；如果没有匹配，则回退为一个原始类型进行匹配，如果匹配则自动装配。

@Resource 的作用相当于 @Autowired，只不过 @Autowired 按照 byType 自动注入。



### 4、依赖注入的方式有几种，各是什么？

#### **1、构造器注入** 

将被依赖对象通过构造函数的参数注入给依赖对象，并且在初始化对象的时候注入。

**优点：**对象初始化完成后便可获得可使用的对象。

**缺点：**当需要注入的对象很多时，构造器参数列表将会很长； 不够灵活。若有多种注入方式，每种方式只需注入指定几个依赖，那么就需要提供多个重载的构造函数，麻烦。

#### 2、setter 方法注入

IOC Service Provider 通过调用成员变量提供的 setter 函数将被依赖对象注入给依赖类。

**优点：**灵活。可以选择性地注入需要的对象。

**缺点：**依赖对象初始化完成后由于尚未注入被依赖对象，因此还不能使用。

#### 3、接口注入

依赖类必须要实现指定的接口，然后实现该接口中的一个函数，该函数就是用于依赖注入。该函数的参数就是要注入的对象。

**优点：**接口注入中，接口的名字、函数的名字都不重要，只要保证函数的参数是要注入的对象类型即可。

**缺点：**侵入行太强，不建议使用。

PS：什么是侵入行？ 如果类 A 要使用别人提供的一个功能，若为了使用这功能，需要在自己的类中增加额外的代码，这就是侵入性。



### 5、讲一下什么是 Spring

Spring 是一个轻量级的 IOC 和 AO P容器框架。是为 Java 应用程序提供基础性服务的一套框架，目的是用于简化企业应用程序的开发，它使得开发者只需要关心业务需求。常见的配置方式有三种：基于 XML 的配置、基于注解的配置、基于 Java 的配置。

主要由以下几个模块组成：

Spring Core：核心类库，提供 IOC 服务；

Spring Context：提供框架式的 Bean 访问方式，以及企业级功能（ JNDI、定时任务等）；

Spring AOP：AOP 服务；

Spring DAO：对 JDBC 的抽象，简化了数据访问异常的处理；

Spring ORM：对现有的 ORM 框架的支持；

Spring Web：提供了基本的面向 Web 的综合特性，例如多方文件上传；

Spring MVC：提供面向 Web 应用的 Model - View - Controller 实现。


### 6、说说你对 Spring MVC 的理解

**什么是 MVC 模式**

MVC：MVC 是一种设计模式

MVC 的原理图：

![](https://dream-syz.github.io/4-1.png)

<img src="C:\Users\FAN\AppData\Roaming\Typora\typora-user-images\image-20230824015211162.png" alt="image-20230824015211162" style="zoom: 67%;" />

**分析：**

M - Model 模型（完成业务逻辑：有 javaBean 构成，service + dao + entity）

V - View 视图（做界面的展示 jsp，html……）

C - Controller 控制器（接收请求 —> 调用模型 —> 根据结果派发页面）

SpringMVC 是一个 MVC 的开源框架，SpringMVC = struts2 + spring，SpringMVC 就相当于是 Struts2 加上 spring 的整合，但是这里有一个疑惑就是，SpringMVC 和 spring 是什么样的关系呢？这个在百度百科上有一个很好的解释：意思是说，SpringMVC 是 spring 的一个后续产品，其实就是 spring 在原有基础上，又提供了 web 应用的 MVC 模块，可以简单的把 SpringMVC 理解为是 spring 的一个模块

（类似 AOP，IOC 这样的模块），网络上经常会说 SpringMVC 和 spring 无缝集成，其实 SpringMVC 就是 spring 的一个子模块，所以根本不需要同 spring 进行整合。

工作原理：

![](https://dream-syz.github.io/4-2.png)

1、 用户发送请求至前端控制器 DispatcherServlet。

2、 DispatcherServlet 收到请求调用 HandlerMapping 处理器映射器。

3、 处理器映射器找到具体的处理器（可以根据 xml 配置、注解进行查找），生成处理器对象及处理器拦截器（如果有则生成）一并返回给 DispatcherServlet。

4、 DispatcherServlet 调用 HandlerAdapter 处理器适配器。

5、 HandlerAdapter 经过适配调用具体的处理器（Controller，也叫后端控制器）。

6、 Controller 执行完成返回 ModelAndView。

7、 HandlerAdapter 将 controller 执行结果 ModelAndView 返回给 DispatcherServlet。

8、 DispatcherServlet 将 ModelAndView 传给 ViewReslover 视图解析器。

9、 ViewReslover 解析后返回具体 View。

10、DispatcherServlet 根据 View 进行渲染视图（即将模型数据填充至视图中）。

11、 DispatcherServlet 响应用户。

**组件说明** 以下组件通常使用框架提供实现：

DispatcherServlet：作为前端控制器，整个流程控制的中心，控制其它组件执行，统一调度，降低组件之间的耦合性，提高每个组件的扩展性。

HandlerMapping：通过扩展处理器映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

HandlAdapter：通过扩展处理器适配器，支持更多类型的处理器。

ViewResolver：通过扩展视图解析器，支持更多类型的视图解析，例如：jsp、freemarker、pdf、excel 等。

**组件：** **1、前端控制器 DispatcherServlet（不需要工程师开发），由框架提供** 作用：接收请求，响应结果，相当于转发器，中央处理器。有了 DispatcherServlet 减少了其它组件之间的耦合度。 用户请求到达前端控制器，它就相当于 mvc 模式中的 c，DispatcherServlet 是整个流程控制的中心，由它调用其它组件处理用户的请求，DispatcherServlet 的存在降低了组件之间的耦合性。

**2、处理器映射器 HandlerMapping（不需要工程师开发），由框架提供** 作用：根据请求的 url 查找 Handler，HandlerMapping 负责根据用户请求找到 Handler 即处理器，springmvc 提供了不同的映射器实现不同的映射方式，例如：配置文件方式，实现接口方式，注解方式等。

**3、处理器适配器 HandlerAdapter** 作用：按照特定规则（HandlerAdapter 要求的规则）去执行 Handler，通过 HandlerAdapter 对处理器进行执行，这是适配器模式的应用，通过扩展适配器可以对更多类型的处理器进行执行。

**4、处理器 Handler（需要工程师开发）注意：编写 Handler 时按照 HandlerAdapter 的要求去做，这样适配器才可以去正确执行Handler** Handler 是继 DispatcherServlet 前端控制器的后端控制器，在 DispatcherServlet 的控制下 Handler 对具体的用户请求进行处理。 由于 Handler 涉及到具体的用户业务请求，所以一般情况需要工程师根据业务需求开发 Handler。

**5、视图解析器 View resolver（不需要工程师开发），由框架提供** 作用：进行视图解析，根据逻辑视图名解析成真正的视图（view） View Resolver 负责将处理结果生成 View 视图，View Resolver 首先根据逻辑视图名解析成物理视图名即具体的页面地址，再生成 View视图对象，最后对 View 进行渲染将处理结果通过页面展示给用户。 springmvc 框架提供了很多的 View 视图类型，包括：jstlView、freemarkerView、pdfView 等。 一般情况下需要通过页面标签或页面模版技术将模型数据通过页面展示给用户，需要由工程师根据业务需求开发具体的页面。

**6、视图 View（需要工程师开发 jsp...）** View 是一个接口，实现类支持不同的 View 类型（jsp、freemarker、pdf...）

**核心架构的具体流程步骤如下：** 1、首先用户发送请求 ——> DispatcherServlet，前端控制器收到请求后自己不进行处理，而是委托给其他的解析器进行处理，作为统一访问点，进行全局的流程控制； 2、DispatcherServlet ——> HandlerMapping， HandlerMapping 将会把请求映射为 HandlerExecutionChain 对象（包含一个 Handler 处理器（页面控制器）对象、多个 HandlerInterceptor 拦截器）对象，通过这种策略模式，很容易添加新的映射策略； 3、DispatcherServlet ——> HandlerAdapter，HandlerAdapter 将会把处理器包装为适配器，从而支持多种类型的处理器，即适配器设计模式的应用，从而很容易支持很多类型的处理器； 4、HandlerAdapter ——> 处理器功能处理方法的调用，HandlerAdapter 将会根据适配的结果调用真正的处理器的功能处理方法，完成功能处理；并返回一个ModelAndView 对象（包含模型数据、逻辑视图名）； 5、ModelAndView 的逻辑视图名 ——> ViewResolver， ViewResolver 将把逻辑视图名解析为具体的 View，通过这种策略模式，很容易更换其他视图技术； 6、View ——> 渲染，View 会根据传进来的 Model 模型数据进行渲染，此处的 Model 实际是一个 Map 数据结构，因此很容易支持其他视图技术； 7、返回控制权给 DispatcherServlet，由DispatcherServlet 返回响应给用户，到此一个流程结束。

看到这些步骤我相信大家很感觉非常的乱，这是正常的，但是这里主要是要大家理解 springMVC 中的几个组件：

前端控制器（DispatcherServlet）：接收请求，响应结果，相当于电脑的 CPU。

处理器映射器（HandlerMapping）：根据 URL 去查找处理器。

处理器（Handler）：需要程序员去写代码处理逻辑的。

处理器适配器（HandlerAdapter）：会把处理器包装成适配器，这样就可以支持多种类型的处理器，类比笔记本的适配器（适配器模式的应用）。

视图解析器（ViewResovler）：进行视图解析，多返回的字符串，进行处理，可以解析成对应的页面



### 7、SpringMV 常用的注解有哪些？

@RequestMapping：用于处理请求 url 映射的注解，可用于类或方法上。用于类上，则表示类中的所有响应请求的方法都是以该地址作为父路径。

@RequestBody：注解实现接收 http 请求的 json 数据，将 json 转换为 java 对象。

@ResponseBody：注解实现将 conreoller 方法返回对象转化为 json 对象响应给客户。



### 8、 谈谈你对 Spring AOP 的理解

AOP（Aspect - Oriented Programming，面向切面编程）能够将那些与业务无关，却为业务模块所共同调用的逻辑或责任（例如事务处理、日志管理、权限控制等）封装起来，便于减少系统的重复代码，降低模块间的耦合度，并有利于未来的可扩展性和可维护性。

Spring AOP是基于动态代理的，如果要代理的对象实现了某个接口，那么 Spring AOP 就会使用 JDK 动态代理去创建代理对象；而对于没有实现接口的对象，就无法使用 JDK 动态代理，转而使用 CGlib 动态代理生成一个被代理对象的子类来作为代理。

![](https://dream-syz.github.io/4-3.png)

注意：图中的 implements 和 extend。即一个是接口，一个是实现类。

当然也可以使用 AspectJ，Spring AOP 中已经集成了 AspectJ，AspectJ 应该算得上是 Java 生态系统中最完整的 AOP 框架了。使用 AOP 之后我们可以把一些通用功能抽象出来，在需要用到的地方直接使用即可，这样可以大大简化代码量。我们需要增加新功能也方便，提高了系统的扩展性。日志功能、事务管理和权限管理等场景都用到了 AOP。

这里只要你提到了 AspectJ，那么面试官很有可能会继续问：



### 9、Spring AOP 和 AspectJ AOP 有什么区别？

Spring AOP 是属于运行时增强，而 AspectJ 是编译时增强。Spring AOP 基于代理（Proxying），而 AspectJ 基于字节码操作（Bytecode Manipulation）。

Spring AOP 已经集成了 AspectJ，AspectJ 应该算得上是 Java 生态系统中最完整的 AOP 框架了。

AspectJ 相比于 Spring AOP 功能更加强大，但是 Spring AOP 相对来说更简单。

如果我们的切面比较少，那么两者性能差异不大。但是，当切面太多的话，最好选择 AspectJ，它比 Spring AOP 快很多。

可能还会继续问：

#### **在 Spring AOP 中，关注点和横切关注的区别是什么？**

关注点是应用中一个模块的行为，一个关注点可能会被定义成一个我们想实现的一个功能。 横切关注点是一个关注点，此关注点是整个应用都会使用的功能，并影响整个应用，比如日志，安全和数据传输，几乎应用的每个模块都需要的功能。因此这些都属于横切关注点。

那什么是连接点呢？连接点代表一个应用程序的某个位置，在这个位置我们可以插入一个 AOP 切面，它实际上是个应用程序执行 Spring AOP 的位置。切入点是什么？切入点是一个或一组连接点，通知将在这些位置执行。可以通过表达式或匹配的方式指明切入点。

#### **什么是通知呢？有哪些类型呢？**

通知是个在方法执行前或执行后要做的动作，实际上是程序执行时要通过 Spring AOP 框架触发的代码段。

Spring 切面可以应用五种类型的通知：

- **before：**前置通知，在一个方法执行前被调用。

- **after：**在方法执行之后调用的通知，无论方法执行是否成功。

- **after-returning：**仅当方法成功完成后执行的通知。

- **after-throwing：**在方法抛出异常退出时执行的通知。

- **around：**在方法执行之前和之后调用的通知。



### 10、说说你对 Spring IOC 是怎么理解的？

（1）IOC 就是控制反转，是指创建对象的控制权的转移。以前创建对象的主动权和时机是由自己把控的，而现在这种权力转移到 Spring容器中，并由容器根据配置文件去创建实例和管理各个实例之间的依赖关系。对象与对象之间松散耦合，也利于功能的复用。DI 依赖注入，和控制反转是同一个概念的不同角度的描述，即应用程序在运行时依赖 IOC 容器来动态注入对象需要的外部资源。

（2）最直观的表达就是，IOC 让对象的创建不用去 new 了，可以由 spring 自动生产，使用 java 的反射机制，根据配置文件在运行时动态的去创建对象以及管理对象，并调用对象的方法的。

（3）Spring 的 IOC 有三种注入方式 ：构造器注入、setter方法注入、根据注解注入。

IOC 让相互协作的组件保持松散的耦合，而 AOP 编程允许你把遍布于应用各层的功能分离出来形成可重用的功能组件。



### 11、解释一下 spring bean 的生命周期

首先说一下 Servlet 的生命周期：实例化，初始 init，接收请求 service，销毁 destroy；

Spring 上下文中的 Bean 生命周期也类似，如下：

#### （1）实例化 Bean：

对于 BeanFactory 容器，当客户向容器请求一个尚未初始化的 bean 时，或初始化 bean 的时候需要注入另一个尚未初始化的依赖时，容器就会调用 createBean 进行实例化。对于 ApplicationContext 容器，当容器启动结束后，通过获取 BeanDefinition 对象中的信息，实例化所有的 bean。

#### （2）设置对象属性（依赖注入）：

实例化后的对象被封装在 BeanWrapper 对象中，紧接着，Spring 根据 BeanDefinition 中的信息以及通过 BeanWrapper 提供的设置属性的接口完成依赖注入。

#### （3）处理 Aware 接口：

接着，Spring 会检测该对象是否实现了 xxxAware 接口，并将相关的 xxxAware 实例注入给 Bean：

①如果这个 Bean 已经实现了 BeanNameAware 接口，会调用它实现的 setBeanName(StringbeanId) 方法，此处传递的就是 Spring 配置文件中 Bean 的 id 值；

②如果这个 Bean 已经实现了 BeanFactoryAware 接口，会调用它实现的 setBeanFactory() 方法，传递的是 Spring 工厂自身。

③如果这个 Bean 已经实现了 ApplicationContextAware 接口，会调用 setApplicationContext(ApplicationContext) 方法，传入 Spring 上下文；

#### （4）BeanPostProcessor：

如果想对 Bean 进行一些自定义的处理，那么可以让 Bean 实现了 BeanPostProcessor 接口，那将会调用postProcessBeforeInitialization(Object obj, String s) 方法。

#### （5）InitializingBean 与 init - method：

如果 Bean 在 Spring 配置文件中配置了 init - method 属性，则会自动调用其配置的初始化方法。

#### （6）如果这个Bean实现了 BeanPostProcessor 接口：

将会调用 postProcessAfterInitialization(Object obj, String s) 方法；由于这个方法是在 Bean 初始化结束时调用的，所以可以被应用于内存或缓存技术；

以上几个步骤完成后，Bean就已经被正确创建了，之后就可以使用这个 Bean 了。

#### （7）DisposableBean：

当 Bean 不再需要时，会经过清理阶段，如果 Bean 实现了 DisposableBean 这个接口，会调用其实现的 destroy() 方法；

#### （8）destroy - method：

最后，如果这个 Bean 的 Spring 配置中配置了 destroy - method 属性，会自动调用其配置的销毁方法。

![](https://dream-syz.github.io/4-4.png)



### 12、解释 Spring 支持的几种 bean 的作用域？

Spring 容器中的 bean 可以分为5个范围：

（1）singleton：默认，每个容器中只有一个bean 的实例，单例的模式由 BeanFactory 自身来维护。

（2）prototype：为每一个 bean 请求提供一个实例。

（3）request：为每一个网络请求创建一个实例，在请求完成以后，bean 会失效并被垃圾回收器回收。

（4）session：与 request 范围类似，确保每个 session 中有一个 bean 的实例，在 session 过期后，bean 会随之失效。

（5）global - session：全局作用域，global - session 和 Portlet 应用相关。当你的应用部署在 Portlet 容器中工作时，它包含很多portlet。如果你想要声明让所有的 portlet 共用全局的存储变量的话，那么这全局变量需要存储在 global - session 中。全局作用域与Servlet 中的 session 作用域效果相同。



### 13、Spring 基于 xml 注入 bean 的几种方式？

（1）Set 方法注入；

（2）构造器注入：①通过 index 设置参数的位置；②通过 type 设置参数类型；

（3）静态工厂注入；

（4）实例工厂；

通常回答前面两种即可，因为后面两种很多人都不太会，不会的就不要说出来，不然问到你不会就尴尬了。



### 14、Spring 框架中都用到了哪些设计模式？

这是一道相对有难度的题目，你不仅要回设计模式，还要知道每个设计模式在 Spring 中是如何使用的。

**简单工厂模式**：Spring 中的 BeanFactory 就是简单工厂模式的体现。根据传入一个唯一的标识来获得 Bean 对象，但是在传入参数后创建还是传入参数前创建，要根据具体情况来定。

**工厂模式**：Spring 中的 FactoryBean 就是典型的工厂方法模式，实现了 FactoryBean 接口的 bean 是一类叫做 factory 的 bean。其特点是，spring 在使用 getBean() 调用获得该 bean 时，会自动调用该 bean 的 getObject() 方法，所以返回的不是 factory 这个 bean，而是这个 bean.getOjbect() 方法的返回值。

**单例模式**：在 spring 中用到的单例模式有： scope="singleton" ，注册式单例模式，bean 存放于 Map 中。bean name 当做 key，bean 当做 value。

**原型模式**：在 spring 中用到的原型模式有： scope="prototype" ，每次获取的是通过克隆生成的新实例，对其进行修改时对原有实例对象不造成任何影响。

**迭代器模式**：在 Spring 中有个 CompositeIterator 实现了 Iterator，Iterable 接口和 Iterator 接口，这两个都是迭代相关的接口。可以这么认为，实现了 Iterable 接口，则表示某个对象是可被迭代的。Iterator 接口相当于是一个迭代器，实现了 Iterator 接口，等于具体定义了这个可被迭代的对象时如何进行迭代的。

**代理模式**：Spring 中经典的 AOP，就是使用动态代理实现的，分 JDK 和 CGlib 动态代理。

**适配器模式**：Spring 中的 AOP 中 `AdvisorAdapter` 类，它有三个实现：

`MethodBeforAdviceAdapter`、`AfterReturnningAdviceAdapter`、`ThrowsAdviceAdapter`。Spring 会根据不同的 AOP 配置来使用对应的 Advice，与策略模式不同的是，一个方法可以同时拥有多个 Advice。Spring 存在很多以 Adapter 结尾的，大多数都是适配器模式。

**观察者模式**：Spring 中的 Event 和 Listener。spring 事件：ApplicationEvent，该抽象类继承了 EventObject 类，JDK 建议所有的事件都应该继承自 EventObject。spring 事件监听器：ApplicationListener，该接口继承了 EventListener 接口，JDK 建议所有的事件监听器都应该继承 EventListener。

**模板模式**：Spring 中的 org.springframework.jdbc.core.JdbcTemplate 就是非常经典的模板模式的应用，里面的 execute 方法，把整个算法步骤都定义好了。

**责任链模式**：DispatcherServlet 中的 doDispatch() 方法中获取与请求匹配的处理器 HandlerExecutionChain，this.getHandler() 方法的处理使用到了责任链模式。

**注意**：这里只是列举了部分设计模式，其实里面用到了还有享元模式、建造者模式等。可选择性的回答，主要是怕你回答了迭代器模式，然后继续问你，结果你一问三不知，那就尴了尬了。



### 15、说说 Spring 中 ApplicationContext 和 BeanFactory 的区别

**类图**

![](https://dream-syz.github.io/4-5.png)

**包目录不同**

spring-beans.jar 中 org.springframework.beans.factory.BeanFactory

spring-context.jar 中 org.springframework.context.ApplicationContext

**国际化**

BeanFactory 是不支持国际化功能的，因为 BeanFactory 没有扩展 Spring 中 MessageResource 接口。相反，由于 ApplicationContext 扩展了 MessageResource 接口，因而具有消息处理的能力（i18N）。

**强大的事件机制（Event）**

基本上牵涉到事件（Event）方面的设计，就离不开观察者模式，ApplicationContext 的事件机制主要通过 ApplicationEvent 和 ApplicationListener 这两个接口来提供的，和 Java swing 中的事件机制一样。即当 ApplicationContext 中发布一个事件时，所有扩展了 ApplicationListener 的 Bean 都将接受到这个事件，并进行相应的处理。

**底层资源的访问**

ApplicationContext 扩展了 ResourceLoader（资源加载器）接口，从而可以用来加载多个 Resource，而 BeanFactory 是没有扩展 ResourceLoader。

**对** **Web** **应用的支持**

与 BeanFactory 通常以编程的方式被创建，ApplicationContext 能以声明的方式创建，如使用 ContextLoader。当然你也可以使用 ApplicationContext 的实现方式之一，以编程的方式创建 ApplicationContext 实例。

**延迟加载**

1. BeanFactroy 采用的是延迟加载形式来注入 Bean 的，即只有在使用到某个 Bean 时(调用 getBean())，才对该 Bean 进行加载实例化。这样，我们就不能发现一些存在的 spring 的配置问题。而 ApplicationContext 则相反，它是在容器启动时，一次性创建了所有的 Bean。这样，在容器启动时，我们就可以发现 Spring 中存在的配置错误。

2. BeanFactory 和 ApplicationContext 都支持 BeanPostProcessor、BeanFactoryPostProcessor 的使用。两者之间的区别是：BeanFactory 需要手动注册，而 ApplicationContext 则是自动注册。

可以看到，ApplicationContext 继承了 BeanFactory，BeanFactory 是 Spring 中比较原始的 Factory，它不支持 AOP、Web 等 Spring 插件。而 ApplicationContext 不仅包含了 BeanFactory 的所有功能，还支持 Spring 的各种插件，还以一种面向框架的方式工作以及对上下文进行分层和实现继承。

BeanFactory 是 Spring 框架的基础设施，面向 Spring 本身；而 ApplicationContext 面向使用 Spring 的开发者，相比 BeanFactory 提供了更多面向实际应用的功能，几乎所有场合都可以直接使用 ApplicationContext，而不是底层的 BeanFactory。

**常用容器**

BeanFactory 类型的有 XmlBeanFactory，它可以根据 XML 文件中定义的内容，创建相应的 Bean。

ApplicationContext 类型的常用容器有：

1. ClassPathXmlApplicationContext：从 ClassPath 的 XML 配置文件中读取上下文，并生成上下文定义。应用程序上下文从程序环境变量中取得。

2. FileSystemXmlApplicationContext：由文件系统中的 XML 配置文件读取上下文。
3. XmlWebApplicationContext：由 Web 应用的 XML 文件读取上下文。例如我们在 Spring MVC 使用的情况。



### 16、Spring 框架中的单例 Bean 是线程安全的么？

Spring 框架并没有对单例 Bean 进行任何多线程的封装处理。

关于单例 Bean 的线程安全和并发问题，需要开发者自行去搞定。

单例的线程安全问题，并不是 Spring 应该去关心的。Spring 应该做的是，提供根据配置，创建单例 Bean 或多例 Bean 的功能。当然，但实际上，大部分的 Spring Bean 并没有可变的状态，所以在某种程度上说 Spring 的单例 Bean 是线程安全的。如果你的 Bean 有多种状态的话，就需要自行保证线程安全。最浅显的解决办法，就是将多态 Bean 的作用域（Scope）由 Singleton 变更为 Prototype。



### 17、Spring 是怎么解决循环依赖的？

**循环依赖**

**定义：**循环依赖其实就是循环引用，也就是两个或两个以上的 bean 对象互相持有对方，最终形成闭环。比如 A 依赖 B，B 依赖 C，C 又依赖 A，形成循环依赖

**出现场景：**1、构造器的循环依赖 2、Filed 属性的循环依赖

**如何检测：**在创建 Bean 的时候可以给该 Bean 打标签，如果递归调用回来发现正在创建中的话，即说明发生了循环依赖

**Spring 单例对象初始化步骤：**

1、createBeanInstance 实例化：调用对象的构造方法实例化对象

2、populateBean 属性填充：这一步主要是对多 Bean 的依赖属性进行填充

3、initializeBean 初始化：调用applicationContext.xml 中的初始化方法

**如何解决：**

使用三级缓存

singletonFactories：单例对象工厂的 cache 缓存
earlySingletonObjects：提前曝光的单例对象的 cache 缓存
singletonObjects：单例对象的 cache 缓存

Spring 首先从一级缓存 singletonObjects 中获取对象，如果获取不到并且对象正在创建中，就再从二级存 earlySingletonObiects 中获取，如果还是获取不到且允许 singletonFactories 通过 getObiect() 获取，就从三级缓存 singletonFactory 中获取，如果获取到了就从singletonFactories 三级缓存中移除掉，并放入 earlySingletonObiects 中，其实也就是从三级缓存移到了二级缓存中

**构造器的循环依赖问题无法解决，需通过懒加载**

整个流程大致如下：

1. 首先 A 完成初始化第一步并将自己 **提前曝光** 出来（通过 ObjectFactory 将自己提前曝光），在初始化的时候，发现自己依赖对象 B，此时就会去尝试 get(B)，这个时候发现 B 还没有被创建出来；

2. 然后 B 就走创建流程，在 B 初始化的时候，同样发现自己依赖 C，C 也没有被创建出来；
3. 这个时候 C 又开始初始化进程，但是在初始化的过程中发现自己依赖 A，于是尝试 get(A)。这个时候由于 A 已经添加至缓存中（一般都是添加至三级缓存 singletonFactories），通过 ObjectFactory 提前曝光，所以可以通过 ObjectFactory#getObject() 方法来拿到 A 对象。C 拿到 A 对象后顺利完成初始化，然后将自己添加到一级缓存中；

4. 回到 B，B 也可以拿到 C 对象，完成初始化，A 可以顺利拿到 B 完成初始化。到这里整个链路就已经完成了初始化过程了。

关键字：三级缓存，提前曝光。



### 18、说说事务的隔离级别

未提交读（Read Uncommitted）：允许脏读，也就是可能读取到其他会话中未提交事务修改的数据

提交读（Read Committed）：只能读取到已经提交的数据。Oracle 等多数数据库默认都是该级别 (不重复读)

可重复读（Repeated Read）：在同一个事务内的查询都是事务开始时刻一致的，MySQL 的 InnoDB 默认级别。在 SQL 标准中，该隔离级别消除了不可重复读，但是还存在幻读（多个事务同时修改同一条记录，事务之间不知道彼此存在，当事务提交之后，后面的事务修改的数据将会覆盖前事务，前一个事务就像发生幻觉一样）

可串行化（Serializable）：完全串行化的读，每次读都需要获得表级共享锁，读写相互都会阻塞。

| 事务隔离级别 | 脏读 | 不可重复读 | 幻读 |
| ------------ | ---- | ---------- | ---- |
| 读未提交     | 允许 | 允许       | 允许 |
| 读已提交     | 禁止 | 允许       | 允许 |
| 可重复读     | 禁止 | 禁止       | 允许 |
| 顺序读       | 禁止 | 禁止       | 禁止 |

不可重复读和幻读的区别主要是：解决不可重复读需要锁定当前满足条件的记录，而解决幻读需要锁定当前满足条件的记录及相近的记录。比如查询某个商品的信息，可重复读事务隔离级别可以保证当前商品信息被锁定，解决不可重复读；但是如果统计商品个数，中途有记录插入，可重复读事务隔离级别就不能保证两个事务统计的个数相同。



### 19、说说事务的传播级别

Spring 事务定义了 7 种传播机制：

1. PROPAGATION_REQUIRED：默认的 Spring 事物传播级别，若当前存在事务，则加入该事务，若不存在事务，则新建一个事务。

2. PAOPAGATION_REQUIRE_NEW：若当前没有事务，则新建一个事务。若当前存在事务，则新建一个事务，新老事务相互独立。外部事务抛出异常回滚不会影响内部事务的正常提交。

3. PROPAGATION_NESTED：如果当前存在事务，则嵌套在当前事务中执行。如果当前没有事务，则新建一个事务，类似于REQUIRE_NEW。

4. PROPAGATION_SUPPORTS：支持当前事务，若当前不存在事务，以非事务的方式执行。
5. PROPAGATION_NOT_SUPPORTED：以非事务的方式执行，若当前存在事务，则把当前事务挂起。

6. PROPAGATION_MANDATORY：强制事务执行，若当前不存在事务，则抛出异常.
7. PROPAGATION_NEVER：以非事务的方式执行，如果当前存在事务，则抛出异常。

Spring 事务传播级别一般不需要定义，默认就是 PROPAGATION_REQUIRED，除非在嵌套事务的情况下需要重点了解。



### 20、Spring 事务实现方式

编程式事务管理：这意味着你可以通过编程的方式管理事务，这种方式带来了很大的灵活性，但很难维护。

声明式事务管理：这种方式意味着你可以将事务管理和业务代码分离。你只需要通过注解或者 XML 配置管理事务。



### 21、Spring 框架的事务管理有哪些优点

它为不同的事务 API（如 JTA、 JDBC、Hibernate、 JPA 和 JDO）提供了统一的编程模型。它为编程式事务管理提供了一个简单的 API 而非一系列复杂的事务 API（如 JTA），它支持声明式事务管理。它可以和 Spring 的多种数据访问技术很好的融合。



### 22、事务三要素是什么？

**数据源**：表示具体的事务性资源，是事务的真正处理者，如 MySQL 等。

**事务管理器**：像一个大管家，从整体上管理事务的处理过程，如打开、提交、回滚等。

**事务应用和属性配置**：像一个标识符，表明哪些方法要参与事务，如何参与事务，以及一些相关属性如隔离级别、超时时间等。



### 23、 事务注解的本质是什么？

@Transactional 这个注解仅仅是一些（和事务相关的）元数据，在运行时被事务基础设施读取消费，并**使用这些元数据来配置 bean 的事务行为**。 大致来说具有两方面功能，**一是表明该方法要参与事务，二是配置相关属性来定制事务的参与方式和运行行为** 

声明式事务主要是得益于 Spring AOP。使用一个事务拦截器，在方法调用的前后 / 周围进行事务性增强（advice），来驱动事务完成。

@Transactional 注解既可以标注在类上，也可以标注在方法上。当在类上时，默认应用到类里的所有方法。如果此时方法上也标注了，则方法上的优先级高。 另外注意方法一定要是 public 的。





## MyBatis 篇

### 1、什么是 MyBatis

（1）Mybatis 是一个半 ORM（对象关系映射）框架，它内部封装了 JDBC，开发时只需要关注 SQL 语句本身，不需要花费精力去处理加载驱动、创建连接、创建 statement 等繁杂的过程。程序员直接编写原生态 sql，可以严格控制 sql 执行性能，灵活度高。

（2）MyBatis 可以使用 XML 或注解来配置和映射原生信息，将 POJO 映射成数据库中的记录，避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集。

（3）通过 xml 文件或注解的方式将要执行的各种 statement 配置起来，并通过 java 对象和 statement 中 sql 的动态参数进行映射生成最终执行的 sql 语句，最后由 mybatis 框架执行 sql 并将结果映射为 java 对象并返回。（从执行 sql 到返回 result 的过程）。



### 2、说说 MyBatis 的优点和缺点

**优点：**

（1）基于 SQL 语句编程，相当灵活，不会对应用程序或者数据库的现有设计造成任何影响，SQL 写在 XML 里，解除 sql 与程序代码的耦合，便于统一管理；提供 XML 标签，支持编写动态 SQL 语句，并可重用。

（2）与 JDBC 相比，减少了 50 % 以上的代码量，消除了 JDBC 大量冗余的代码，不需要手动开关连接；

（3）很好的与各种数据库兼容（因为 MyBatis 使用 JDBC 来连接数据库，所以只要 JDBC 支持的数据库 MyBatis 都支持）。

（4）能够与 Spring 很好的集成；

（5）提供映射标签，支持对象与数据库的 ORM 字段关系映射；提供对象关系映射标签，支持对象关系组件维护。

**缺点**

（1）SQL 语句的编写工作量较大，尤其当字段多、关联表多时，对开发人员编写 SQL 语句的功底有一定要求。

（2）SQL 语句依赖于数据库，导致数据库移植性差，不能随意更换数据库。



### 3、#{} 和 ${} 的区别是什么？

\#{} 是预编译处理，${} 是字符串替换。

Mybatis 在处理 #{} 时，会将 sql 中的 #{} 替换为 ? 号，调用 PreparedStatement 的 set 方法来赋值；

Mybatis 在处理 ${} 时，就是把 ${} 替换成变量的值。

使用 #{} 可以有效的防止 SQL 注入，提高系统安全性。



### 4、当实体类中的属性名和表中的字段名不一样 ，怎么办 ？

第 1 种：通过在查询的 sql 语句中定义字段名的别名，让字段名的别名和实体类的属性名一致。

```xml
<select id=”selectorder” parametertype=”int” resultetype=”me.gacl.domain.order”>
  select order_id id, order_no orderno ,order_price price form orders where order_id=#{id};
</select>
```

第 2 种：通过来映射字段名和实体类属性名的一一对应的关系。

```xml
 <select id="getOrder" parameterType="int" resultMap="orderresultmap">
   select * from orders where order_id=#{id}
 </select>
 <resultMap type=”me.gacl.domain.order” id=”orderresultmap”>
 <!–用id属性来映射主键字段–>
 <id property=”id” column=”order_id”>
 <!–用result属性来映射非主键字段，property为实体类属性名，column为数据表中的属性–>
 <result property = “orderno” column =”order_no”/>
 <result property=”price” column=”order_price” />
 </reslutMap>
```



### 5、Mybatis 是如何进行分页的？分页插件的原理是什么？

Mybatis 使用 RowBounds 对象进行分页，它是针对 ResultSet 结果集执行的内存分页，而非物理分页。可以在 sql 内直接拼写带有物理分页的参数来完成物理分页功能，也可以使用分页插件来完成物理分页，比如：MySQL 数据的时候，在原有 SQL 后面拼写 limit。

分页插件的基本原理是使用 Mybatis 提供的插件接口，实现自定义插件，在插件的拦截方法内拦截待执行的 sql，然后重写 sql，根据dialect 方言，添加对应的物理分页语句和物理分页参数。



### 6、Mybatis 是如何将 sql 执行结果封装为目标对象并返回的？都有哪些映射形式？

第一种是使用标签，逐一定义数据库列名和对象属性名之间的映射关系。

第二种是使用 sql 列的别名功能，将列的别名书写为对象属性名。

有了列名与属性名的映射关系后，Mybatis 通过反射创建对象，同时使用反射给对象的属性逐一赋值并返回，那些找不到映射关系的属性，是无法完成赋值的。



### 7、 如何执行批量插入？

首先,创建一个简单的 insert 语句:

```xml
<insert id=”insertname”>
 insert into names (name) values (#{value})
</insert>
```

然后在 java 代码中像下面这样执行批处理插入:

```java
list<string> names = new arraylist();
 names.add(“fred”);
 names.add(“barney”);
 names.add(“betty”);
 names.add(“wilma”);
 // 注意这里 executortype.batch
 sqlsession sqlsession = sqlsessionfactory.opensession(executortype.batch);
 try {
 	namemapper mapper = sqlsession.getmapper(namemapper.class);
 	for (string name : names) {
 		mapper.insertname(name);
 	}
	 sqlsession.commit();
 }catch(Exception e){
	 e.printStackTrace();
 	 sqlSession.rollback(); 
	 throw e; 
 }
 finally {
 	sqlsession.close();
 }
```



### 8、Xml 映射文件中，除了常见的 select|insert|updae|delete 标签之外，还有哪些标签？

加上动态 sql 的 9 个标签，其中为 sql 片段标签，通过标签引入 sql 片段，为不支持自增的主键生成策略标签。



### 9、MyBatis 实现一对一有几种方式？具体怎么操作的？

有联合查询和嵌套查询，联合查询是几个表联合查询，只查询一次，通过在 resultMap 里面配置 association 节点配置一对一的类就可以完成；

嵌套查询是先查一个表，根据这个表里面的结果的外键 id，去再另外一个表里面查询数据,也是通过 association 配置，但另外一个表的查询通过 select 属性配置。



### 10、Mybatis 是否支持延迟加载？如果支持，它的实现原理是什么？

Mybatis 仅支持 association 关联对象和 collection 关联集合对象的延迟加载，association 指的就是一对一，collection 指的就是一对多查询。在 Mybatis 配置文件中，可以配置是否启用延迟加载 lazyLoadingEnabled = true|false。

它的原理是，使用 CGLIB 创建目标对象的代理对象，当调用目标方法时，进入拦截器方法，比如调用 a.getB().getName()，拦截器invoke() 方法发现 a.getB() 是 null 值，那么就会单独发送事先保存好的查询关联 B 对象的 sql，把 B 查询上来，然后调用 a.setB(b)，于是 a 的对象 b 属性就有值了，接着完成 a.getB().getName() 方法的调用。这就是延迟加载的基本原理。

当然了，不光是 Mybatis，几乎所有的包括 Hibernate，支持延迟加载的原理都是一样的。



### 11、说说 Mybatis 的缓存机制

Mybatis 整体：

![](https://dream-syz.github.io/5-1.png)

**一级缓存 localCache**

在应用运行过程中，我们有可能在一次数据库会话中，执行多次查询条件完全相同的 SQL，MyBatis 提供了一级缓存的方案优化这部分场景，如果是相同的 SQL 语句，会优先命中一级缓存，避免直接对数据库进行查询，提高性能。

每个 SqlSession 中持有了 Executor，每个 Executor 中有一个 LocalCache。当用户发起查询时，MyBatis 根据当前执行的语句生成 MappedStatement，在 Local Cache 进行查询，如果缓存命中的话，直接返回结果给用户，如果缓存没有命中的话，查询数据库，结果写入 Local Cache，最后返回结果给用户。具体实现类的类关系图如下图所示：

![](https://dream-syz.github.io/5-2.png)

<img src="D:\文档\WeChat Files\wxid_hrkocnb2rcbm22\FileStorage\Temp\1692984622141.jpg" alt="1692984622141" style="zoom:80%;" />

1. MyBatis 一级缓存的生命周期和 SqlSession 一致。
2. MyBatis 一级缓存内部设计简单，只是一个没有容量限定的 HashMap，在缓存的功能性上有所欠缺。

3. MyBatis 的一级缓存最大范围是 SqlSession 内部，有多个 SqlSession 或者分布式的环境下，数据库写操作会引起脏数据，建议设定缓存级别为 Statement。

**二级缓存**

在上文中提到的一级缓存中，其最大的共享范围就是一个 SqlSession 内部，如果多个 SqlSession之间需要共享缓存，则需要使用到二级缓存。开启二级缓存后，会使用 CachingExecutor 装饰 Executor，进入一级缓存的查询流程前，先在 CachingExecutor 进行二级缓存的查询，具体的工作流程如下所示。

![](https://dream-syz.github.io/5-3.png)

二级缓存开启后，同一个 namespace 下的所有操作语句，都影响着同一个 Cache，即二级缓存被多个 SqlSession 共享，是一个全局的变量。

当开启缓存后，数据的查询执行的流程为：

二级缓存 -> 一级缓存 -> 数据库

1. MyBatis 的二级缓存相对于一级缓存来说，实现了 SqlSession 之间缓存数据的共享，同时粒度更加细，能够到 namespace 级别，通过 Cache 接口实现类不同的组合，对 Cache 的可控性也更强。

2. MyBatis 在多表查询时，极大可能会出现脏数据，有设计上的缺陷，安全使用二级缓存的条件比较苛刻。

3. 在分布式环境下，由于默认的 MyBatis Cache 实现都是基于本地的，分布式环境下必然会出现读取到脏数据，需要使用集中式缓存将 MyBatis 的 Cache 接口实现，有一定的开发成本，直接使用 Redis、Memcached 等分布式缓存可能成本更低，安全性也更高。



### 12、JDBC 编程有哪些步骤？

1. 装载相应的数据库的 JDBC 驱动并进行初始化：

```java
Class.forName("com.mysql.jdbc.Driver");
```

2. 建立 JDBC 和数据库之间的 Connection 连接：

```java
Connection c = DriverManager.getConnection("jdbc:mysql://127.0.0.1:3306/test?
characterEncoding=UTF-8", "root", "123456");
```

3. 创建 Statement 或者 PreparedStatement 接口，执行 SQL 语句。

4. 处理和显示结果。

5. 释放资源。



### 13、MyBatis 中见过什么设计模式？

![](https://dream-syz.github.io/5-4.png)



**建造者模式：**例如 SqlSessionFactoryBuilder、XMLConfigBuilder、XMLMapperBuilder、XMLStatementBuilder、 CacheBuilder ;

**单例模式：**例如 ErrorContext 和 LogFactory ；

**代理模式：**Mybatis 实现的核心，比如 Mapperroxy、ConnectionLogger，用的 JDK 的动态代理；还有 executor.loader 包使用了 CGlib 或者 javassist 达到延迟加载的效果；

**组合模式：**例如 SqlNode 和各个子类 ChooseSqlNode 等 ;

**模板方法模式：**例如 BaseExecutor 和 SimpleExecutor，还有 BaseTypeHandler 和所有的子类例 IntegerTypeHandler ;

**适配器模式：**例如 Log 的 Mybatis 接口和它对 JDBC、log4j 等各种日志框架的适配实现；

**装饰者模式：**例如 Cache 包中的 cache,decorators 子包中等各个装饰者的实现；

**迭代器模式：**例如迭代器模式 PropertyTokenizer ;

**工厂模式：**例如 SqlSessionFactory、ObjectFactory、MapperProxyFactory ；



### 14、MyBatis 中比如 `UserMapper.java` 是接口，为什么没有实现类还能调用？

使用 JDK 动态代理 + MapperProxy。本质上调用的是 MapperProxy 的 invoke 方法。



## SpringBoot 篇

### 1、为什么要用 SpringBoot

Spring Boot 优点非常多，如：

一、独立运行

Spring Boot 而且内嵌了各种 servlet 容器，Tomcat、Jetty 等，现在不再需要打成 war 包部署到容器中，Spring Boot 只要打成一个可执行的 jar 包就能独立运行，所有的依赖包都在一个 jar 包内。

二、简化配置

spring - boot - starter - web 启动器自动依赖其他组件，简少了 maven 的配置。

三、自动配置 Spring Boot 能根据当前类路径下的类、jar 包来自动配置 bean，如添加一个 `spring-boot-starter-web` 启动器就能拥有 web 的功能，无需其他配置。

四、无代码生成和 XML 配置

Spring Boo t配置过程中无代码生成，也无需 XML 配置文件就能完成所有配置工作，这一切都是借助于条件注解完成的，这也是Spring4.x 的核心功能之一。

五、应用监控

Spring Boot 提供一系列端点可以监控服务及应用，做健康检测。



### 2、Spring Boot 的核心注解是哪个？它主要由哪几个注解组成的？

启动类上面的注解是 **@SpringBootApplication**，它也是 Spring Boot 的核心注解，主要组合包含了以下 3 个注解：

**@SpringBootConfiguration**：组合了 @Configuration 注解，实现配置文件的功能。

**@EnableAutoConfiguration**：打开自动配置的功能，也可以关闭某个自动配置的选项，如关闭数据源自动配置功能： @SpringBootApplication(exclude = { DataSourceAutoConfiguration.class})。

**@ComponentScan**：Spring 组件扫描。



### 3、运行 Spring Boot 有哪几种方式？

1）打包用命令或者放到容器中运行

2）用 Maven / Gradle 插件运行

3）直接执行 main 方法运行



### 4、如何理解 Spring Boot 中的 Starters？

Starters 是什么：

Starters 可以理解为启动器，它包含了一系列可以集成到应用里面的依赖包，你可以一站式集成 Spring 及其他技术，而不需要到处找示例代码和依赖包。如你想使用 Spring JPA 访问数据库，只要加入 `spring-boot-starter-data-jpa` 启动器依赖就能使用了。Starters 包含了许多项目中需要用到的\依赖，它们能快速持续的运行，都是一系列得到支持的管理传递性依赖。

Starters 命名：Spring Boot 官方的启动器都是以 `spring-boot-starter-` 命名的，代表了一个特定的应用类型。第三方的启动器不能以 `spring-boot` 开头命名，它们都被 Spring Boot 官方保留。一般一个第三方的应该这样命名，像 mybatis 的 `mybatis-spring-boot-starter`。

Starters 分类：

1. Spring Boot 应用类启动器

| 启动器名称             | 功能描述                                                     |
| ---------------------- | ------------------------------------------------------------ |
| spring-boot-stater     | 包含自动配置、日志、YAML 的支持                              |
| spring-boot-stater-web | 使用 Spring MVC 构建 web 工程，包含 restful，默认使用 Tomcat 容器 |

2. Spring Boot 生产启动器

| 启动器名称                  | 功能描述                         |
| --------------------------- | -------------------------------- |
| spring-boot-stater-actuator | 提供生产环境特性，能监控管理应用 |

3. Spring Boot 技术类启动器

| 启动器名称                 | 功能描述                           |
| -------------------------- | ---------------------------------- |
| spring-boot-stater-json    | 提供对 JSON 的读写支持             |
| spring-boot-stater-logging | 默认的日志启动器，默认使用 Logback |



### 5、 如何在 Spring Boot 启动的时候运行一些特定的代码？

如果你想在 Spring Boot 启动的时候运行一些特定的代码，你可以实现接口 **ApplicationRunner** 或者 **CommandLineRunner**，这两个接口实现方式一样，它们都只提供了一个 run 方法。

**CommandLineRunner**：启动获取命令行参数



### 6、Spring Boot 需要独立的容器运行吗？

可以不需要，内置了 Tomcat / Jetty 等容器。



### 7、Spring Boot 中的监视器是什么？

Spring boot actuator 是 spring 启动框架中的重要功能之一。Spring boot 监视器可帮助您访问生产环境中正在运行的应用程序的当前状态。有几个指标必须在生产环境中进行检查和监控。即使一些外部应用程序可能正在使用这些服务来向相关人员触发警报消息。监视器模块公开了一组可直接作为 HTTP URL 访问的 REST 端点来检查状态。



### 8、 如何使用 Spring Boot 实现异常处理？

Spring 提供了一种使用 ControllerAdvice 处理异常的非常有用的方法。 我们通过实现一个 ControlerAdvice 类，来处理控制器类抛出的所有异常。



### 9、 你如何理解 Spring Boot 中的 Starters ？

Starters 可以理解为启动器，它包含了一系列可以集成到应用里面的依赖包，你可以一站式集成 Spring 及其他技术，而不需要到处找示例代码和依赖包。如你想使用 Spring JPA 访问数据库，只要加入 `spring-boot-starter-data-jpa` 启动器依赖就能使用了。



### 10、springboot 常用的 starter 有哪些

`spring-boot-starter-web` 嵌入 tomcat 和 web 开发需要 servlet 与 jsp 支持

`spring-boot-starter-data-jpa` 数据库支持

`spring-boot-starter-data-redis` redis 数据库支持

`spring-boot-starter-data-solr` solr 支持

`mybatis-spring-boot-starter` 第三方的 mybatis 集成 starter



### 11、SpringBoot 实现热部署有哪几种方式？

主要有两种方式：

- **Spring Loaded**

- **Spring-boot-devtools**



### 12、 如何理解 Spring Boot 配置加载顺序？

在 Spring Boot 里面，可以使用以下几种方式来加载配置。

1）properties 文件；

2）YAML 文件；

3）系统环境变量；

4）命令行参数；



### 13、Spring Boot 的核心配置文件有哪几个？它们的区别是什么？

Spring Boot 的核心配置文件是 application 和 bootstrap 配置文件。

application 配置文件这个容易理解，主要用于 Spring Boot 项目的自动化配置。

bootstrap 配置文件有以下几个应用场景。

- 使用 Spring Cloud Config 配置中心时，这时需要在 bootstrap 配置文件中添加连接到配置中心的配置属性来加载外部配置中心的配置信息；

- 一些固定的不能被覆盖的属性；

- 一些加密 / 解密的场景；



### 14、如何集成 Spring Boot 和 ActiveMQ ？

集成 Spring Boot 和 ActiveMQ，我们使用 `spring-boot-starter-activemq` 依赖关系。 它只需要很少的配置，并且不需要样板代码。





## MySQL 篇

### 1、数据库的三范式是什么

第一范式：列不可再分 

第二范式：行可以唯一区分，主键约束 

第三范式：表的非主属性不能依赖与其他表的非主属性外键约束且三大范式是一级一级依赖的，第二范式建立在第一范式上，第三范式建立第一第二范式上。



### 2、MySQL 数据库引擎有哪些

如何查看 MySQL 提供的所有存储引擎

```mysql
mysql> show engines;
```



![](https://dream-syz.github.io/6-1.png)

![](https://dream-syz.github.io/6-2.png)

MySQL 常用引擎包括：MYISAM、Innodb、Memory、MERGE

- MYISAM：全表锁，拥有较高的执行速度，不支持事务，不支持外键，并发性能差，占用空间相对较小，对事务完整性没有要求，以select、insert 为主的应用基本上可以使用这引擎

- Innodb：行级锁，提供了具有提交、回滚和崩溃回复能力的事务安全，支持自动增长列，支持外键约束，并发能力强，占用空间是MYISAM 的 2.5 倍，处理效率相对会差一些

- Memory：全表锁，存储在内容中，速度快，但会占用和数据量成正比的内存空间且数据在 MySQL 重启时会丢失，默认使用 HASH索引，检索效率非常高，但不适用于精确查找，主要用于那些内容变化不频繁的代码表

- MERGE：是一组 MYISAM 表的组合



### 3、说说 InnoDB 与 MyISAM 的区别

在 MySQL 5.5 及之前的版本中，MyISAM 是默认的存储引擎，而在 MySQL 5.5 版本以后，默认使用 InnoDB 存储引擎。

1. InnoDB 支持事务，MyISAM 不支持，对于 InnoDB 每一条 SQL 语言都默认封装成事务，自动提交，这样会影响速度，所以最好把多条 SQL 语言放在 begin 和 commit 之间，组成一个事务；InnoDB 需要更多存储空间，会在内存中建立其专用的缓冲池用于高速缓冲数据和索引。InnoDB 支持自动奔溃恢复特性。
2. InnoDB 支持外键，而 MyISAM 不支持。对一个包含外键的 InnoDB 表转为 MYISAM 会失败；
3. InnoDB 是聚集索引，数据文件是和索引绑在一起的，必须要有主键，通过主键索引效率很高。但是辅助索引需要两次查询，先查询到主键，然后再通过主键查询到数据。因此，主键不应该过大，因为主键太大，其他索引也都会很大。而 MyISAM 是非聚集索引，数据文件是分离的，索引保存的是数据文件的指针。主键索引和辅助索引是独立的。
4. InnoDB 不保存表的具体行数，执行 select count(*) from table 时需要全表扫描。而 MyISAM 用一个变量保存了整个表的行数，MyISAM 不支持行级锁，执行上述语句时只需要读出该变量即可，速度很快；
5. Innodb 不支持全文索引，而 MyISAM 支持全文索引，查询效率上 MyISAM 要高；

InnoDB 和 MyISAM 是使用 MySQL 时最常用的两种引擎类型，我们重点来看下两者区别。

* 事务和外键
  InnoDB 支持事务和外键，具有安全性和完整性，适合大量 insert 或 update 操作
  MyISAM 不支持事务和外键，它提供高速存储和检索，适合大量的 select 查询操作
* 锁机制
  InnoDB 支持行级锁，锁定指定记录。基于索引来加锁实现。
  MyISAM 支持表级锁，锁定整张表。
* 索引结构
  InnoDB 使用聚集索引（聚簇索引），索引和记录在一起存储，既缓存索引，也缓存记录。
  MyISAM 使用非聚集索引（非聚簇索引），索引和记录分开。
* 并发处理能力
  MyISAM 使用表锁，会导致写操作并发率低，读之间并不阻塞，读写阻塞。
  InnoDB 读写阻塞可以与隔离级别有关，可以采用多版本并发控制（MVCC）来支持高并发
* 存储文件
  InnoDB 表对应两个文件，一个.frm 表结构文件，一个.ibd 数据文件。InnoDB 表最大支持 64 TB；
  MyISAM 表对应三个文件，一个.frm 表结构文件，一个 MYD 表数据文件，一个 .MYI 索引文件。从 MySQL5.0 开始默认限制是256 TB。

![](https://dream-syz.github.io/6-30.png)

MyISAM 适用场景

* 不需要事务支持（不支持）
* 并发相对较低（锁定机制问题）
* 数据修改相对较少，以读为主
* 数据一致性要求不高

InnoDB 适用场景

* 需要事务支持（具有较好的事务特性）
* 行级锁定对高并发有很好的适应能力
* 数据更新较为频繁的场景
* 数据一致性要求较高
* 硬件设备内存较大，可以利用 InnoDB 较好的缓存能力来提高内存利用率，减少磁盘 IO

两种引擎该如何选择？

* 是否需要事务？有，InnoDB
* 是否存在并发修改？有，InnoDB
* 是否追求快速查询，且数据修改少？是，MyISAM
* 在绝大多数情况下，推荐使用 InnoDB

扩展资料：各个存储引擎特性对比

![](https://dream-syz.github.io/6-31.png)



### 4、数据库的事务

**什么是事务？：** 多条 sql 语句，要么全部成功，要么全部失败。

**事务的特性：**

**数据库事务特性：原子性（Atomic）、一致性（Consistency）、隔离性（Isolation）、持久性（Durabiliy）。简称 ACID。**

- 原子性：组成一个事务的多个数据库操作是一个不可分割的原子单元，只有所有操作都成功，整个事务才会提交。任何一个操作失败，已经执行的任何操作都必须撤销，让数据库返回初始状态。

- 一致性：事务操作成功后，数据库所处的状态和它的业务规则是一致的。即数据不会被破坏。如 A 转账 100 元给 B，不管操作是否成功，A 和 B 的账户总额是不变的。

- 隔离性：在并发数据操作时，不同的事务拥有各自的数据空间，它们的操作不会对彼此产生干扰

- 持久性：一旦事务提交成功，事务中的所有操作都必须持久化到数据库中。



### 5、索引是什么

官方介绍索引是帮助 MySQL **高效获取数据 **的 **数据结构**。更通俗的说，数据库索引好比是一本书前面的目录，能 **加快数据库的查询速度**。

一般来说索引本身也很大，不可能全部存储在内存中，因此 **索引往往是存储在磁盘上的文件中的**（可能存储在单独的索引文件中，也可能和数据一起存储在数据文件中）。

**我们通常所说的索引，包括聚集索引、覆盖索引、组合索引、前缀索引、唯一索引等，没有特别说明，默认都是使用 B+ 树结构组织（多路搜索树，并不一定是二叉的）的索引。**



### 6、SQL 优化手段有哪些

1、查询语句中不要使用 select *

2、尽量减少子查询，使用关联查询（left join，right join，inner join）替代

3、减少使用 IN 或者 NOT IN，使用 exists，not exists 或者关联查询语句替代

4、or 的查询尽量用 union 或者 union all 代替（在确认没有重复数据或者不用剔除重复数据时，union all 会更好）

5、应尽量避免在 where 子句中使用 != 或 <> 操作符，否则将引擎放弃使用索引而进行全表扫描。

6、应尽量避免在 where 子句中对字段进行 null 值判断，否则将导致引擎放弃使用索引而进行全表扫描，如： select id from t where num is null 可以在 num 上设置默认值 0，确保表中 num 列没有 null 值，然后这样查询： select id from t where num = 0



### 7、简单说一说 drop、delete 与 truncate 的区别

SQL 中的 drop、delete、truncate 都表示删除，但是三者有一些差别 

delete 和 truncate 只删除表的数据不删除表的结构 速度，一般来说：drop > truncate > delete delete 语句是 dml，这个操作会放到rollback segement 中,事务提交之后才生效；如果有相应的 trigger，执行的时候将被触发。 truncate，drop 是 ddl， 操作立即生效，原数据不放到 rollback segment中，不能回滚操作不触发 trigger。



### 8、什么是视图

视图是一种虚拟的表，具有和物理表相同的功能。可以对视图进行增，改，查，操作，试图通常是有一个表或者多个表的行或列的子集。对视图的修改不影响基本表。它使得我们获取数据更容易，相比多表查询。



### 9、 什么是内联接、左外联接、右外联接？

- 内联接（Inner Join）：匹配2张表中相关联的记录。

- 左外联接（Left Outer Join）：除了匹配2张表中相关联的记录外，还会匹配左表中剩余的记录，右表中未匹配到的字段用 NULL 表示。

- 右外联接（Right Outer Join）：除了匹配 2 张表中相关联的记录外，还会匹配右表中剩余的记录，左表中未匹配到的字段用 NULL 表示。在判定左表和右表时，要根据表名出现在 Outer Join 的左右位置关系。



### 10、并发事务带来哪些问题？

在典型的应用程序中，多个事务并发运行，经常会操作相同的数据来完成各自的任务（多个用户对同一数据进行操作）。并发虽然是必须的，但可能会导致以下的问题。

- **脏读（Dirty read）:** 当一个事务正在访问数据并且对数据进行了修改，而这种修改还没有提交到数据库中，这时另外一个事务也访问了这个数据，然后使用了这个数据。因为这个数据是还没有提交的数据，那么另外一个事务读到的这个数据是“脏数据”，依据“脏数据”所做的操作可能是不正确的。

- **丢失修改（Lost to modify）:** 指在一个事务读取一个数据时，另外一个事务也访问了该数据，那么在第一个事务中修改了这个数据后，第二个事务也修改了这个数据。这样第一个事务内的修改结果就被丢失，因此称为丢失修改。 例如：事务 1 读取某表中的数据 A = 20，事务 2 也读取 A = 20，事务 1 修改 A = A - 1，事务 2 也修改 A = A - 1，最终结果 A = 19，事务 1 的修改被丢失。

- **不可重复读（Unrepeatableread）:** 指在一个事务内多次读同一数据。在这个事务还没有结束时，另一个事务也访问该数据。那么，在第一个事务中的两次读数据之间，由于第二个事务的修改导致第一个事务两次读取的数据可能不太一样。这就发生了在一个事务内两次读到的数据是不一样的情况，因此称为不可重复读。

- **幻读（Phantom read）:** 幻读与不可重复读类似。它发生在一个事务（T1）读取了几行数据，接着另一个并发事务（T2）插入了一些数据时。在随后的查询中，第一个事务（T1）就会发现多了一些原本不存在的记录，就好像发生了幻觉一样，所以称为幻读。

**不可重复读和幻读区别：**

不可重复读的重点是修改比如多次读取一条记录发现其中某些列的值被修改，幻读的重点在于新增或者删除比如多次读取一条记录发现记录增多或减少了。



### 11、事务隔离级别有哪些？MySQL 的默认隔离级别是？

**SQL** **标准定义了四个隔离级别：**

- **READ-UNCOMMITTED（读取未提交)：** 最低的隔离级别，允许读取尚未提交的数据变更，**可能会导致脏读、幻读或不可重复读**。

- **READ-COMMITTED（读取已提交）：** 允许读取并发事务已经提交的数据，**可以阻止脏读，但是幻读或不可重复读仍有可能发生**。

- **REPEATABLE-READ（可重复读）：** 对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，**可以阻止脏读和不可重复读，但幻读仍有可能发生**。

- **SERIALIZABLE（可串行化）：** 最高的隔离级别，完全服从 ACID 的隔离级别。所有的事务依次逐个执行，这样事务之间就完全不可能产生干扰，也就是说，**该级别可以防止脏读、不可重复读以及幻读**。

| **隔离级别**     | **脏读** | **不可重复读** | 幻**影**读 |
| ---------------- | -------- | -------------- | ---------- |
| READ-UNCOMMITTED | √        | √              | √          |
| READ-COMMITTED   | ×        | √              | √          |
| REPEATABLE-READ  | ×        | ×              | √          |
| SERIALIZABLE     | ×        | ×              | ×          |

MySQL InnoDB 存储引擎的默认支持的隔离级别是 **REPEATABLE-READ（可重读）**。我们可以通过 SELECT @@tx_isolation；命令来查看

```mysql
mysql> SELECT @@tx_isolation;
+-----------------+
| @@tx_isolation |
+-----------------+
| REPEATABLE-READ |
+-----------------+
```

这里需要注意的是：与 SQL 标准不同的地方在于 InnoDB 存储引擎在 **REPEATABLE-READ（可重读）** 事务隔离级别下使用的是 Next-Key Lock 锁算法，因此可以避免幻读的产生，这与其他数据库系统（如 SQL Server）是不同的。所以说 InnoDB 存储引擎的默认支持的隔离级别是 **REPEATABLEREAD（可重读）** 已经可以完全保证事务的隔离性要求，即达到了  SQL 标准的 **SERIALIZABLE（可串行化）隔离级别。因为隔离级别越低，事务请求的锁越少，所以大部分数据库系统的隔离级别都是 READ-COMMITTED（读取提交内容）** ，但是你要知道的是 InnoDB 存储引擎默认使用(**REPEATABLE-READ（可重读）** 并不会有任何性能损失。

InnoDB 存储引擎在 **分布式事务** 的情况下一般会用到 **SERIALIZABLE（可串行化）** 隔离级别。



### 12、大表如何优化？

当 MySQL 单表记录数过大时，数据库的 CRUD 性能会明显下降，一些常见的优化措施如下：

**1.** **限定数据的范围**

务必禁止不带任何限制数据范围条件的查询语句。比如：我们当用户在查询订单历史的时候，我们可以控制在一个月的范围内；

**2.** **读 / 写分离**

经典的数据库拆分方案，主库负责写，从库负责读；

**3.** **垂直分区**

**根据数据库里面数据表的相关性进行拆分。** 例如，用户表中既有用户的登录信息又有用户的基本信息，可以将用户表拆分成两个单独的表，甚至放到单独的库做分库。

**简单来说垂直拆分是指数据表列的拆分，把一张列比较多的表拆分为多张表。** 如下所示，这样来说大家应该就更容易理解了。

参考链接：https://github.com/gsjqwyl/JavaInterview

- **垂直拆分的优点：** 可以使得列数据变小，在查询时减少读取的 Block 数，减少 I / O 次数。此外，垂直分区可以简化表的结构，易于维护。

- **垂直拆分的缺点：** 主键会出现冗余，需要管理冗余列，并会引起 Join 操作，可以通过在应用层进行 Join 来解决。此外，垂直分区会让事务变得更加复杂；

**4.** **水平分区**

**保持数据表结构不变，通过某种策略存储数据分片。这样每一片数据分散到不同的表或者库中，达到了分布式的目的。 水平拆分可以支撑非常大的数据量。**

水平拆分是指数据表行的拆分，表的行数超过 200 万行时，就会变慢，这时可以把一张的表的数据拆成多张表来存放。举个例子：我们可以将用户信息表拆分成多个用户信息表，这样就可以避免单一表数据量过大对性能造成影响。

水平拆分可以支持非常大的数据量。需要注意的一点是：分表仅仅是解决了单一表数据过大的问题，但由于表的数据还是在同一台机器上，其实对于提升 MySQL 并发能力没有什么意义，所以 **水平拆分最好分库** 。

水平拆分能够 **支持非常大的数据量存储，应用端改造也少**，但 **分片事务难以解决** ，跨节点 Join 性能较差，逻辑复杂。《Java工程师修炼之道》的作者推荐 **尽量不要对数据进行分片，因为拆分会带来逻辑、部署、运维的各种复杂度** ，一般的数据表在优化得当的情况下支撑千万以下的数据量是没有太大问题的。如果实在要分片，尽量选择客户端分片架构，这样可以减少一次和中间件的网络 I/O。

**下面补充一下数据库分片的两种常见方案：**

- **客户端代理： 分片逻辑在应用端，封装在 jar 包中，通过修改或者封装 JDBC 层来实现。** 当当网的 **Sharding-JDBC** 、阿里的 TDD L是两种比较常用的实现。

- **中间件代理： 在应用和数据中间加了一个代理层。分片逻辑统一维护在中间件服务中。** 我们现在谈的 **Mycat** 、360 的 Atlas、网易的DDB 等等都是这种架构的实现。

详细内容可以参考： MySQL 大表优化方案: https://segmentfault.com/a/1190000006158186 【值得细看】





### 13、分库分表之后， id 主键如何处理？

因为要是分成多个表之后，每个表都是从 1 开始累加，这样是不对的，我们需要一个全局唯一的 id来支持。

生成全局 id 有下面这几种方式：

- **UUID**：不适合作为主键，因为太长了，并且无序不可读，查询效率低。比较适合用于生成唯一的名字的标示比如文件的名字。

- **数据库自增** **id** : 两台数据库分别设置不同步长，生成不重复ID的策略来实现高可用。这种方式生成的 id 有序，但是需要独立部署数据库实例，成本高，还会有性能瓶颈。

- **利用** **redis** **生成** **id :** 性能比较好，灵活方便，不依赖于数据库。但是，引入了新的组件造成系统更加复杂，可用性降低，编码更加复杂，增加了系统成本。

- **Twitter 的 snowflake 算法** ：Github 地址：https://github.com/twitter-archive/snowflake。

- **美团的 Leaf 分布式 ID 生成系统** ：Leaf 是美团开源的分布式 ID 生成器，能保证全局唯一性、趋势递增、单调递增、信息安全，里面也提到了几种分布式方案的对比，但也需要依赖关系数据库、Zookeeper 等中间件。感觉还不错。美团技术团队的一篇文章：https://tech.meituan.com/2017/04/21/mt-leaf.html 。



### 14、 说说在 MySQL 中一条查询 SQL 是如何执行的？

比如下面这条 SQL 语句：

```mysql
select name from t_user where id=1
```

1. **取得链接**，使用到 MySQL 中的连接器。

2. **查询缓存**，key 为 SQL 语句，value 为查询结果，如果查到就直接返回。不建议使用次缓存，在 MySQL 8.0 版本已经将查询缓存删除，也就是说 MySQL 8.0 版本后不存在此功能。

3. **分析器**，分为词法分析和语法分析。此阶段只是做一些 SQL 解析，语法校验。所以一般语法错误在此阶段。

4. **优化器**，是在表里有多个索引的时候，决定使用哪个索引；或者一个语句中存在多表关联的时候（join），决定各个表的连接顺序。

5. **执行器**，通过分析器让 SQL 知道你要干啥，通过优化器知道该怎么做，于是开始执行语句。执行语句的时候还要判断是否具备此权限，没有权限就直接返回提示没有权限的错误；有权限则打开表，根据表的引擎定义，去使用这个引擎提供的接口，获取这个表的第一行，判断 id 是都等于 1。如果是，直接返回；如果不是继续调用引擎接口去下一行，重复相同的判断，直到取到这个表的最后一行，最后返回。



### 15、索引有什么优缺点？

![](https://dream-syz.github.io/6-3.png)

### 16、MySQL 中 varchar 与 char 的区别？ varchar(30)  中的 30 代表的涵义？

- varchar 与 char 的区别，char 是一种固定长度的类型，varchar 则是一种可变长度的类型。

- CHAR 和 VARCHAR 类型在存储和检索方面有所不同

  CHAR 列长度固定为创建表时声明的长度，长度值范围是 1 到 255；当 CHAR 值被存储时，它们被用空格填充到特定长度，检索 CHAR 值时需删除尾随空格。

- varchar(30) 中 30 的涵义最多存放 30 个字符。varchar(30) 和 (130) 存储 hello 所占空间一样，但后者在排序时会消耗更多内存，因为 ORDER BY col 采用 fixed_length 计算 col 长度（memory 引擎也一样）。

- 对效率要求高用 char，对空间使用要求高用 varchar。



### 17、int(11) 中的 11 代表什么涵义？

int(11) 中的 11，不影响字段存储的范围，只影响展示效果。



### 18、 为什么 SELECT COUNT(\*) FROM table 在 InnoDB 比 MyISAM 慢？

*对于 SELECT COUNT(*) FROM table 语句，在没有 WHERE 条件的情况下，InnoDB 比 MyISAM 可能会慢很多，尤其在大表的情况下。因为，InnoDB 是去实时统计结果，会 **全表扫描**；而 MyISAM 内部维持了一个计数器，预存了结果，所以直接返回即可。



### 19、MySQL索引类型有哪些？

**主键索引**

索引列中的值必须是唯一的，不允许有空值。

**普通索引**

MySQL 中基本索引类型，没有什么限制，允许在定义索引的列中插入重复值和空值。

**唯一索引**

索引列中的值必须是唯一的，但是允许为空值。

**全文索引**

只能在文本类型 CHAR，VARCHAR，TEXT 类型字段上创建全文索引。字段长度比较大时，如果创建普通索引，在进行 like 模糊查询时效率比较低，这时可以创建全文索引。MyISAM 和 InnoDB 中都可以使用全文索引。

**空间索引**

MySQL 在 5.7 之后的版本支持了空间索引，而且支持 OpenGIS 几何数据模型。MySQL 在空间索引这方面遵循 OpenGIS 几何数据模型规则。

**前缀索引**

在文本类型如 CHAR，VARCHAR，TEXT 类列上创建索引时，可以指定索引列的长度，但是数值类型不能指定。其他（按照索引列数量分类）

1. 单列索引
2. 组合索引

组合索引的使用，需要遵循 **最左前缀匹配原则（最左匹配原则）**。一般情况下在条件允许的情况下使用组合索引替代多个单列索引使用。



### 20、什么时候不要使用索引？

1. 经常增删改的列不要建立索引；
2. 有大量重复的列不建立索引；
3. 表记录太少不要建立索引。



### 21、说说什么是 MVCC？

多版本并发控制（MVCC=Multi-Version Concurrency Control），是一种用来解决读 - 写冲突的无锁并发控制。也就是为事务分配单向增长的时间戳，为每个修改保存一个版本。版本与事务时间戳关联，读操作只读该事务开始前的数据库的快照（复制了一份数据）。这样在读操作不用阻塞写操作，写操作不用阻塞读操作的同时，避免了脏读和不可重复读。

#### MVCC 可以为数据库解决什么问题？

在并发读写数据库时，可以做到在读操作时不用阻塞写操作，写操作也不用阻塞读操作，提高了数据库并发读写的性能。同时还可以解决脏读、幻读、不可重复读等事务隔离问题，但不能解决更新丢失问题。

#### 说说 MVCC 的实现原理

MVCC 的目的就是多版本并发控制，在数据库中的实现，就是为了解决读写冲突，它的实现原理主要是依赖记录中的 3 个隐式字段、undo 日志、Read View 来实现的。

##### **三个隐式字段：**

**DB_ROW_ID**：隐含的自增 ID（隐藏主键）

**DB_TRX_ID**：记录最近修改这条记录的事务 ID

**DB_ROLL_PTR**：回滚指针，指向这条记录的上一个版本



### 22、 请说说 MySQL 数据库的锁？

关于 MySQL 的锁机制，可能会问很多问题，不过这也得看面试官在这方面的知识储备。

MySQL 中有共享锁和排它锁，也就是读锁和写锁。

1. 共享锁：不堵塞，多个用户可以同一时刻读取同一个资源，相互之间没有影响。
2. 排它锁：一个写操作阻塞其他的读锁和写锁，这样可以只允许一个用户进行写入，防止其他用户读取正在写入的资源。

3. 表锁：系统开销最小，会锁定整张表，MyISAM 使用表锁。
4. 行锁：容易出现死锁，发生冲突概率低，并发高，InnoDB 支持行锁（必须有索引才能实现，否则会自动锁全表，那么就不是行锁了）。



### 23、说说什么是锁升级？

MySQL 行锁只能加在索引上，如果操作不走索引，就会升级为表锁。因为 InnoDB 的行锁是加在索引上的，如果不走索引，自然就没法使用行锁了，原因是 InnoDB 是将 primary key index 和相关的行数据共同放在 B+ 树的叶节点。InnoDB 一定会有一个 primary key，secondary index 查找的时候，也是通过找到对应的 primary，再找对应的数据行。

当非唯一索引上记录数超过一定数量时，行锁也会升级为表锁。测试发现 **当非唯一索引相同的内容不少于整个表记录的二分之一时会升级为表锁**。因为当非唯一索引相同的内容达到整个记录的二分之一时，索引需要的性能比全文检索还要大，查询语句优化时会选择不走索引，造成索引失效，行锁自然就会升级为表锁。



### 24、说说悲观锁和乐观锁

**悲观锁**

说的是数据库被外界（包括本系统当前的其他事物以及来自外部系统的事务处理）修改保持着保守态度，因此在整个数据修改过程中，将数据处于锁状态。悲观的实现往往是依靠数据库提供的锁机制，也只有数据库层面提供的锁机制才能真正保证数据访问的排他性，否则，即使在本系统汇总实现了加锁机制，也是没有办法保证系统不会修改数据。

在悲观锁的情况下，为了保证事务的隔离性，就需要一致性锁定读。读取数据时给加锁，其它事务无法修改这些数据。修改删除数据时也要加锁，其它事务无法读取这些数据。

**乐观锁**

相对悲观锁而言，乐观锁机制采取了更加宽松的加锁机制。悲观锁大多数情况下依靠数据库的锁机制实现，以保证操作最大程度的独占性。但随之而来的就是数据库性能的大量开销，特别是对长事务而言，这样的开销往往无法承受。

而乐观锁机制在一定程度上解决了这个问题。乐观锁，大多是基于数据版本（Version）记录机制实现。何谓数据版本？即为数据增加一个版本标识，在基于数据库表的版本解决方案中，一般是通过为数据库表增加一个“version”字段来实现。读取出数据时，将此版本号一同读出，之后更新时，对此版本号加一。此时，将提交数据的版本数据与数据库表对应记录的当前版本信息进行比对，如果提交的数据版本号大于数据库表当前版本号，则予以更新，否则认为是过期数据。



### 25、怎样尽量避免死锁的出现？

1. 设置获取锁的超时时间，至少能保证最差情况下，可以退出程序，不至于一直等待导致死锁；
2. 设置按照同一顺序访问资源，类似于串行执行；
3. 避免事务中的用户交叉；
4. 保持事务简短并在一个批处理中；
5. 使用低隔离级别；
6. 使用绑定链接。



### 26、使用 MySQL 的索引应该注意些什么？

![](https://dream-syz.github.io/6-4.png)



### 27、主键和候选键有什么区别？

表格的每一行都由主键唯一标识，一个表只有一个主键。主键也是候选键。按照惯例，候选键可以被指定为主键，并且可以用于任何外 键引用。



### 28、主键与索引有什么区别？

主键一 **定会创建一个唯一索引，但是有唯一索引的列不一定是主键；**

主键不允许为空值，唯一索引列允许空值；

一个表只能有一个主键，但是可以有多个唯一索引；

主键可以被 **其他表引用为外键，唯一索引列不可以；**

主键是一种约束，而唯一索引是一种索引，是表的冗余数据结构



### 29、MySQL 如何做到高可用方案？

MySQL 高可用，意味着不能一台 MySQL 出了问题，就不能访问了。

1. MySQL 高可用：分库分表，通过 MyCat 连接多个 MySQL
2. MyCat 也得高可用：Haproxy，连接多个 MyCat
3. Haproxy 也得高可用：通过 keepalived 辅助 Haproxy



### 30、什么是 BufferPool？

**Buffer Pool 基本概念**

Buffer Pool：缓冲池，简称 BP。其作用是用来缓存表数据与索引数据，减少磁盘 IO 操作，提升效率。

Buffer Pool由 **缓存数据页(Page)** 和 对缓存数据页进行描述的 **控制块** 组成, 控制块中存储着对应缓存页的所属的	表空间、数据页的编号、以及对应缓存页在 Buffer Pool 中的地址等信息.

Buffer Pool 默认大小是 128 M, 以 Page 页为单位，Page 页默认大小 16K，而控制块的大小约为数据页的 5%，大概是 800 字节。

![](https://dream-syz.github.io/6-5.png)

> 注: Buffer Pool 大小为 128 M 指的就是缓存页的大小，控制块则一般占 5 %，所以每次会多申请 6 M 的内存空间用于存放控制块

**如何判断一个页是否在 BufferPool 中缓存 ?**

MySQL 中有一个哈希表数据结构，它使用表空间号 + 数据页号，作为一个 key，然后缓冲页对应的控制块作为 value。

![](https://dream-syz.github.io/6-6.png)

* **当需要访问某个页的数据时，先从哈希表中根据表空间号 + 页号看看是否存在对应的缓冲页。**
* **如果有，则直接使用；如果没有，就从free链表中选出一个空闲的缓冲页，然后把磁盘中对应的页加载到该缓冲页的位置**



### 31、InnoDB 如何管理 Page 页？

**Page页分类**

BP 的底层采用链表数据结构管理 Page。在 InnoDB 访问表记录和索引时会在 Page 页中缓存，以后使用可以减少磁盘 IO 操作，提升效率。

Page 根据状态可以分为三种类型：

![](https://dream-syz.github.io/6-7.png)

* free page ：空闲 page，未被使用
* clean page：被使用 page，数据没有被修改过
* dirty page：脏页，被使用 page，数据被修改过，Page 页中数据和磁盘的数据产生了不一致

Page 页如何管理

针对上面所说的三种 page 类型，InnoDB 通过三种链表结构来维护和管理

1. free list：表示空闲缓冲区，管理 free page

* free 链表是把所有空闲的缓冲页对应的控制块作为一个个的节点放到一个链表中，这个链表便称之为 free 链表
* 基节点:  free 链表中只有一个基节点是不记录缓存页信息（单独申请空间），它里面就存放了 free 链表的头节点的地址，尾节点的地址，还有 free 链表里当前有多少个节点。

![](https://dream-syz.github.io/6-8.png)

2.flush list：表示需要刷新到磁盘的缓冲区，管理 dirty page，内部 page 按修改时间排序。

* InnoDB 引擎为了提高处理效率，在每次修改缓冲页后，并不是立刻把修改刷新到磁盘上，而是在未来的某个时间点进行刷新操作. 所以需要使用到 flush 链表存储脏页，凡是被修改过的缓冲页对应的控制块都会作为节点加入到 flush 链表.
* flush 链表的结构与 free 链表的结构相似

![](https://dream-syz.github.io/6-9.png)

**3.lru list**：表示正在使用的缓冲区，管理 clean page 和 dirty page，缓冲区以 midpoint 为基点，前面链表称为 new 列表区，存放经常访问的数据，占 63 %；后面的链表称为 old 列表区，存放使用较少数据，占 37 %

![](https://dream-syz.github.io/6-10.png)



### 32、为什么写缓冲区，仅适用于非唯一普通索引页？

**change Buffer 基本概念**

Change Buffer：写缓冲区，是针对二级索引（辅助索引）页的更新优化措施。

作用:  在进行 DML 操作时，如果请求的辅助索引（二级索引）没有在缓冲池中时，并不会立刻将磁盘页加载到缓冲池，而是在 CB 记录缓冲变更，等未来数据被读取时，再将数据合并恢复到 BP 中。

![](https://dream-syz.github.io/6-11.png)

1. ChangeBuffer 用于存储 SQL 变更操作，比如 Insert/Update/Delete 等 SQL 语句
2. ChangeBuffer 中的每个变更操作都有其对应的数据页，并且该数据页未加载到缓存中；
3. 当 ChangeBuffer 中变更操作对应的数据页加载到缓存中后，InnoDB 会把变更操作 Merge 到数据页上；
4. InnoDB 会定期加载 ChangeBuffer 中操作对应的数据页到缓存中，并 Merge 变更操作；

**change buffer更新流程**

![](https://dream-syz.github.io/6-12.png)

写缓冲区，仅适用于非唯一普通索引页，为什么？

* **如果在索引设置唯一性，在进行修改时，InnoDB 必须要做唯一性校验，因此必须查询磁盘，做一次 IO 操作。会直接将记录查询到BufferPool 中，然后在缓冲池修改，不会在 ChangeBuffer 操作。**



### 33、MySQL 为什么改进 LRU 算法？

**普通 LRU 算法**

LRU = Least Recently Used（最近最少使用）：就是末尾淘汰法，新数据从链表头部加入，释放空间时从末尾淘汰.

![](https://dream-syz.github.io/6-13.png)

1. 当要访问某个页时，如果不在 Buffer Pool，需要把该页加载到缓冲池,并且把该缓冲页对应的控制块作为节点添加到 LRU 链表的头部。
2. 当要访问某个页时，如果在 Buffer Pool 中，则直接把该页对应的控制块移动到 LRU 链表的头部
3. 当需要释放空间时，从最末尾淘汰

**普通 LRU 链表的优缺点**

优点

* 所有最近使用的数据都在链表表头，最近未使用的数据都在链表表尾，保证热数据能最快被获取到。

缺点

* 如果发生全表扫描（比如：没有建立合适的索引 or 查询时使用 select * 等），则有很大可能将真正的热数据淘汰掉.
* 由于 MySQL 中存在预读机制，很多预读的页都会被放到 LRU 链表的表头。如果这些预读的页都没有用到的话，这样，会导致很多尾部的缓冲页很快就会被淘汰。

![](https://dream-syz.github.io/6-14.png)

**改进型 LRU 算法**

改进型 LRU：将链表分为 new 和 old 两个部分，加入元素时并不是从表头插入，而是从中间 midpoint 位置插入（就是说从磁盘中新读出的数据会放在冷数据区的头部），如果数据很快被访问，那么 page 就会向 new 列表头部移动，如果数据没有被访问，会逐步向 old 尾部移动，等待淘汰。

![](https://dream-syz.github.io/6-15.png)

冷数据区的数据页什么时候会被转到到热数据区呢 ?

1. 如果该数据页在 LRU 链表中存在时间超过 1 s，就将其移动到链表头部  ( 链表指的是整个 LRU 链表)
2. 如果该数据页在 LRU 链表中存在的时间短于 1 s，其位置不变（由于全表扫描有一个特点，就是它对某个页的频繁访问总耗时会很短）
3. 1 s 这个时间是由参数 `innodb_old_blocks_time` 控制的



### 34、使用索引一定可以提升效率吗？

索引就是排好序的,帮助我们进行快速查找的数据结构.

简单来讲，索引就是一种将数据库中的记录按照特殊形式存储的数据结构。通过索引，能够显著地提高数据查询的效率，从而提升服务器的性能.

索引的优势与劣势

- 优点

  - 提高数据检索的效率，降低数据库的 IO 成本
  - 通过索引列对数据进行排序，降低数据排序的成本，降低了 CPU 的消耗

- 缺点

  - 创建索引和维护索引要耗费时间，这种时间随着数据量的增加而增加
  - 索引需要占物理空间，除了数据表占用数据空间之外，每一个索引还要占用一定的物理空间
  - 当对表中的数据进行增加、删除和修改的时候，索引也要动态的维护，降低了数据的维护速度

- 创建索引的原则

  - 在经常需要搜索的列上创建索引，可以加快搜索的速度；
  - 在作为主键的列上创建索引，强制该列的唯一性和组织表中数据的排列结构；
  - 在经常用在连接的列上，这些列主要是一些外键，可以加快连接的速度；
  - 在经常需要根据范围进行搜索的列上创建索引，因为索引已经排序，其指定的范围是连续的；
  - 在经常需要排序的列上创建索引，因为索引已经排序，这样查询可以利用索引的排序，加快排序查询时间；
  - 在经常使用在 WHERE 子句中的列上面创建索引，加快条件的判断速度。

  

### 35、介绍一下 Page 页的结构？

Page 是整个 InnoDB 存储的最基本构件，也是 InnoDB 磁盘管理的最小单位，与数据库相关的所有内容都存储在这种 Page 结构里。

Page 分为几种类型，常见的页类型有数据页（B+tree Node）Undo 页（Undo Log Page）系统页（System Page） 事务数据页（Transaction System Page）等

![](https://dream-syz.github.io/6-16.png)

**Page 各部分说明**

| **名称**               | **占用大小** | **说明**                                |
| ---------------------- | ------------ | --------------------------------------- |
| **File Header**        | **38字节**   | **文件头, 描述页信息**                  |
| **Page Header**        | **56字节**   | **页头,页的状态**                       |
| **Infimum + Supremum** | **26字节**   | **最大和最小记录,这是两个虚拟的行记录** |
| **User Records**       | **不确定**   | **用户记录,存储数据行记录**             |
| **Free Space**         | **不确定**   | **空闲空间,页中还没有被使用的空间**     |
| **Page Directory**     | **不确定**   | **页目录,存储用户记录的相对位置**       |
| **File Trailer**       | **8字节**    | **文件尾,校验页是否完整**               |

* File Header 字段用于记录 Page 的头信息，其中比较重要的是 FIL_PAGE_PREV 和 FIL_PAGE_NEXT 字段，通过这两个字段，我们可以找到该页的上一页和下一页，实际上所有页通过两个字段可以形成一条双向链表
* Page Header 字段用于记录 Page 的状态信息。
* Infimum 和 Supremum 是两个伪行记录，Infimum（下确界）记录比该页中任何主键值都要小的值，Supremum （上确界）记录比该页中任何主键值都要大的值，这个伪记录分别构成了页中记录的边界。
* User Records 中存放的是实际的数据行记录
* Free Space 中存放的是空闲空间，被删除的行记录会被记录成空闲空间
* Page Directory 记录着与二叉查找相关的信息
* File Trailer 存储用于检测数据完整性的校验和等数据。

**页结构整体上可以分为三大部分，分别为通用部分(文件头、文件尾)、存储记录空间、索引部分。**

1) 通用部分 (File Header&File Trailer )

通用部分：主要指文件头和文件尾，将页的内容进行封装，通过文件头和文件尾校验的 CheckSum 方式来确保页的传输是完整的。

其中比较重要的是在文件头中的 `FIL_PAGE_PREV` 和 `FIL_PAGE_NEXT` 字段，通过这两个字段，我们可以找到该页的上一页和下一页，实际上所有页通过两个字段可以形成一条双向链表

![](https://dream-syz.github.io/6-17.png)

2) 记录部分(User Records&Free Space)

页的主要作用是存储记录，所以“最小和最大记录”和“用户记录”部分占了页结构的主要空间。另外空闲空间是个灵活的部分，当有新的记录插入时，会从空闲空间中进行分配用于存储新记录

![](https://dream-syz.github.io/6-18.png)



**3)数据目录部分 (Page Directory)**

数据页中行记录按照主键值由小到大顺序串联成一个单链表(**页中记录是以单向链表的形式进行存储的**)，且单链表的链表头为最小记录，链表尾为最大记录。并且为了更快速地定位到指定的行记录，通过 `Page Directory`实现目录的功能，借助 `Page Directory`使用二分法快速找到需要查找的行记录。

![](https://dream-syz.github.io/6-19.png)



### 36、说一下聚簇索引与非聚簇索引？

聚集索引与非聚集索引的区别是：叶节点是否存放一整行记录

* **聚簇索引**：将数据存储与索引放到了一块,索引结构的叶子节点保存了行数据.
* **非聚簇索引**：将数据与索引分开存储，索引结构的叶子节点指向了数据对应的位置.

InnoDB 主键使用的是聚簇索引，MyISAM 不管是主键索引，还是二级索引使用的都是非聚簇索引。

在InnoDB引擎中，主键索引采用的就是聚簇索引结构存储。

聚簇索引（聚集索引）

* 聚簇索引是一种数据存储方式，InnoDB 的聚簇索引就是按照主键顺序构建 B+Tree结构。B+Tree 的叶子节点就是行记录，行记录和主键值紧凑地存储在一起。 这也意味着 InnoDB 的主键索引就是数据表本身，它按主键顺序存放了整张表的数据，占用的空间就是整个表数据量的大小。通常说的主键索引就是聚集索引。
* InnoDB 的表要求 **必须要有聚簇索引**：
  * 如果表定义了主键，则主键索引就是聚簇索引
  * 如果表没有定义主键，则第一个非空 unique 列作为聚簇索引
  * 否则 InnoDB 会从建一个隐藏的 row-id 作为聚簇索引
* 辅助索引
  InnoDB 辅助索引，也叫作二级索引，是根据索引列构建 B+Tree 结构。但在 B+Tree 的叶子节点中只存了索引列和主键的信息。二级索引占用的空间会比聚簇索引小很多， 通常创建辅助索引就是为了提升查询效率。一个表 InnoDB 只能创建一个聚簇索引，但可以创建多个辅助索引。

![](https://dream-syz.github.io/6-20.png)



**非聚簇索引**

与 InnoDB 表存储不同，MyISM 使用的是非聚簇索引，非聚簇索引的两棵 B+ 树看上去没什么不同 ，节点的结构完全一致只是存储的内容不同而已，主键索引 B+ 树的节点存储了主键，辅助键索引 B+ 树存储了辅助键。

表数据存储在独立的地方，这两颗 B+ 树的叶子节点都使用一个地址指向真正的表数据，对于表数据来说，这两个键没有任何差别。由于 索引树是独立的，通过辅助键检索无需访问主键的索引树 。

![](https://dream-syz.github.io/6-21.png)

**聚簇索引的优点**

1. 当你需要取出一定范围内的数据时，用聚簇索引也比用非聚簇索引好。
2. 当通过聚簇索引查找目标数据时理论上比非聚簇索引要快，因为非聚簇索引定位到对应主键时还要多一次目标记录寻址,即多一次I/O。
3. 使用覆盖索引扫描的查询可以直接使用页节点中的主键值。

**聚簇索引的缺点**

1. 插入速度严重依赖于插入顺序 。
2. 更新主键的代价很高，因为将会导致被更新的行移动 。
3. 二级索引访问需要两次索引查找，第一次找到主键值，第二次根据主键值找到行数据。



### 37、索引有哪几种类型？

**1）普通索引**

* **这是最基本的索引类型，基于普通字段建立的索引，没有任何限制。**

```
CREATE INDEX <索引的名字> ON tablename (字段名);
ALTER TABLE tablename ADD INDEX [索引的名字] (字段名);
CREATE TABLE tablename ( [...], INDEX [索引的名字] (字段名) );
```

**2）唯一索引**

* **与"普通索引"类似，不同的就是：索引字段的值必须唯一，但允许有空值 。**

```
CREATE UNIQUE INDEX <索引的名字> ON tablename (字段名);
ALTER TABLE tablename ADD UNIQUE INDEX [索引的名字] (字段名);
CREATE TABLE tablename ( [...], UNIQUE [索引的名字] (字段名) ;
```

**3）主键索引**

* **它是一种特殊的唯一索引，不允许有空值。在创建或修改表时追加主键约束即可，每个表只能有一个主键。**

```sql
CREATE TABLE tablename ( [...], PRIMARY KEY (字段名) );
ALTER TABLE tablename ADD PRIMARY KEY (字段名);
```

**4）复合索引**

* **用户可以在多个列上建立索引，这种索引叫做组复合索引（组合索引）。复合索引可以代替多个单一索引，相比多个单一索引复合索引所需的开销更小。**

```sql
CREATE INDEX <索引的名字> ON tablename (字段名1，字段名2...);

ALTER TABLE tablename ADD INDEX [索引的名字] (字段名1，字段名2...);

CREATE TABLE tablename ( [...], INDEX [索引的名字] (字段名1，字段名2...) );
```

* **复合索引使用注意事项：**
  * **何时使用复合索引，要根据where条件建索引，注意不要过多使用索引，过多使用会对更新操作效率有很大影响。**
  * **如果表已经建立了(col1，col2)，就没有必要再单独建立（col1）；如果现在有(col1)索引，如果查询需要col1和col2条件，可以建立(col1,col2)复合索引，对于查询有一定提高。**

**5) 全文索引**

查询操作在数据量比较少时，可以使用 like 模糊查询，但是对于大量的文本数据检索，效率很低。如果使用全文索引，查询速度会比 like快很多倍。

在 MySQL 5.6 以前的版本，只有 MyISAM 存储引擎支持全文索引，从 MySQL 5.6 开始 MyISAM 和 InnoDB 存储引擎均支持。

```
CREATE FULLTEXT INDEX <索引的名字> ON tablename (字段名);

ALTER TABLE tablename ADD FULLTEXT [索引的名字] (字段名);

CREATE TABLE tablename ( [...], FULLTEXT KEY [索引的名字] (字段名) ;
```

全文索引方式有自然语言检索 `IN NATURAL LANGUAGE MODE`和布尔检索 `IN BOOLEAN MODE`两种和常用的 like 模糊查询不同，全文索引有自己的语法格式，使用 match 和 against 关键字，比如

```
SELECT * FROM users3 WHERE MATCH(NAME) AGAINST('aabb');

-- * 表示通配符,只能在词的后面
SELECT * FROM users3 WHERE MATCH(NAME) AGAINST('aa*'  IN BOOLEAN MODE);
```

全文索引使用注意事项：

* 全文索引必须在字符串、文本字段上建立。
* 全文索引字段值必须在最小字符和最大字符之间的才会有效。（innodb：3-84；myisam：4-84）



### 38、介绍一下最佳左前缀法则？

1)最佳左前缀法则

最佳左前缀法则:  如果创建的是联合索引，就要遵循该法则。 使用索引时，where 后面的条件需要从索引的最左前列开始使用，并且不能跳过索引中的列使用。

* 场景1:  按照索引字段顺序使用，三个字段都使用了索引,没有问题。

  ```
  EXPLAIN SELECT * FROM users WHERE user_name = 'tom' 
  AND user_age = 17 AND user_level = 'A';
  ```

* 场景2: 直接跳过 user_name 使用索引字段，索引无效，未使用到索引。

  ```
  EXPLAIN SELECT * FROM users WHERE user_age = 17 AND user_level = 'A';
  ```

* 场景3:  不按照创建联合索引的顺序,使用索引

  ```
  EXPLAIN SELECT * FROM users WHERE 
  user_age = 17 AND user_name = 'tom' AND user_level = 'A';
  ```

  where后面查询条件顺序是 `user_age`、`user_level`、`user_name`与我们创建的索引顺序 `user_name`、`user_age`、`user_level`不一致，为什么还是使用了索引，原因是因为MySql底层优化器对其进行了优化。

* 最佳左前缀底层原理
  MySQL创建联合索引的规则是: 首先会对联合索引最左边的字段进行排序( 例子中是 `user_name` ), 在第一个字段的基础之上 再对第二个字段进行排序 ( 例子中是 `user_age` ) .

  ![](https://dream-syz.github.io/6-22.png)

* 最佳左前缀原则其实是和 B+ 树的结构有关系，最左字段肯定是有序的，第二个字段则是无序的(联合索引的排序方式是：先按照第一个字段进行排序，如果第一个字段相等再根据第二个字段排序)。所以如果直接使用第二个字段 `user_age` 通常是使用不到索引的。



### 39、什么是索引下推？

索引下推（index condition pushdown ）简称 ICP，在 Mysql 5.6 的版本上推出，用于优化查询。

需求：查询 users 表中 "名字第一个字是张，年龄为 10 岁的所有记录"。

```
SELECT * FROM users WHERE user_name LIKE '张%' AND user_age = 10;
```

根据最左前缀法则，该语句在搜索索引树的时候，只能匹配到名字第一个字是‘张’的记录，接下来是怎么处理的呢？当然就是从该记录开始，逐个回表，到主键索引上找出相应的记录，再比对 `age` 这个字段的值是否符合。

图1: 在 (name,age) 索引里面特意去掉了 age 的值，这个过程 InnoDB 并不会去看 age 的值，只是按顺序把“name 第一个字是’张’”的记录一条条取出来回表。因此，需要回表 4 次

![](https://dream-syz.github.io/6-23.png)

MySQL 5.6 引入了索引下推优化，可以在索引遍历过程中，对索引中包含的字段先做判断，过滤掉不符合条件的记录，减少回表次数。

图2: InnoDB 在 (name,age) 索引内部就判断了 age 是否等于 10，对于不等于 10 的记录，直接判断并跳过,减少回表次数.

![](https://dream-syz.github.io/6-24.png)

总结

如果没有索引下推优化（或称 ICP 优化），当进行索引查询时，首先根据索引来查找记录，然后再根据 where 条件来过滤记录；

在支持 ICP 优化后，MySQL 会在取出索引的同时，判断是否可以进行 where 条件过滤再进行索引查询，也就是说提前执行 where 的部分过滤操作，在某些场景下，可以大大减少回表次数，从而提升整体性能。



### 40、什么是自适应哈希索引？

自适应 Hash 索引（Adatptive Hash Index，内部简称 AHI）是 InnoDB 的三大特性之一，还有两个是 Buffer Pool 简称 BP、双写缓冲区（Doublewrite Buffer）。

1、自适应即我们不需要自己处理，当 InnoDB 引擎根据查询统计发现某一查询满足 hash 索引的数据结构特点，就会给其建立一个 hash索引；

2、hash 索引底层的数据结构是散列表（Hash 表），其数据特点就是比较适合在内存中使用，自适应 Hash 索引存在于 InnoDB 架构中的缓存中（不存在于磁盘架构中），见下面的 InnoDB 架构图。

3、自适应 hash 索引只适合搜索等值的查询，如 select * from table where index_col='xxx'，而对于其他查找类型，如范围查找，是不能使用的；

![](https://dream-syz.github.io/6-25.png)

Adaptive Hash Index 是针对 B+ 树 Search Path 的优化，因此所有会涉及到 Search Path 的操作，均可使用此 Hash 索引进行优化.

![](https://dream-syz.github.io/6-26.png)

根据索引键值(前缀)快速定位到叶子节点满足条件记录的 Offset，减少了 B+ 树 Search Path 的代价，将 B+ 树从 Root 节点至 Leaf 节点的路径定位，优化为 Hash Index 的快速查询。

InnoDB 的自适应 Hash 索引是默认开启的，可以通过配置下面的参数设置进行关闭。

```
innodb_adaptive_hash_index = off
```

自适应 Hash 索引使用分片进行实现的，分片数可以使用配置参数设置：

```
innodb_adaptive_hash_index_parts = 8
```



### 41、为什么 LIKE 以 % 开头索引会失效？

like 查询为范围查询，% 出现在左边，则索引失效。% 出现在右边索引未失效.

场景1: 两边都有 % 或者 字段左边有 %,索引都会失效。

```sql
EXPLAIN SELECT * FROM users WHERE user_name LIKE '%tom%';

EXPLAIN SELECT * FROM users WHERE user_name LIKE '%tom';
```

场景2: 字段右边有 %,索引生效

```sql
EXPLAIN SELECT * FROM users WHERE user_name LIKE 'tom%';
```

**解决 % 出现在左边索引失效的方法，使用覆盖索引<创建 包含所有查询列如：user_name,user_age,user_level 的所索引>**

```sql
EXPLAIN SELECT user_name FROM users WHERE user_name LIKE '%jack%';

EXPLAIN SELECT user_name,user_age,user_level FROM users WHERE user_name LIKE '%jack%';
```

对比场景 1 可以知道, 通过使用覆盖索引 `type = index`,并且 `extra = Using index`，从全表扫描变成了全索引扫描.

**like 失效的原因**

1. **%号在右:** 由于 B+ 树的索引顺序，是按照首字母的大小进行排序，% 号在右的匹配又是匹配首字母。所以可以在 B+ 树上进行有序的查找，查找首字母符合要求的数据。所以有些时候可以用到索引.
2. **%号在左:**  是匹配字符串尾部的数据，我们上面说了排序规则，尾部的字母是没有顺序的，所以不能按照索引顺序查询，就用不到索引.
3. **两个 %% 号:**  这个是查询任意位置的字母满足条件即可，只有首字母是进行索引排序的，其他位置的字母都是相对无序的，所以查找任意位置的字母是用不上索引的.



### 42、自增还是 UUID？数据库主键的类型该如何选择？

auto_increment 的优点：

1. 字段长度较 uuid 小很多，可以是 bigint 甚至是 int 类型，这对检索的性能会有所影响。
2. 在写的方面，因为是自增的，所以主键是趋势自增的，也就是说新增的数据永远在后面，这点对于性能有很大的提升。
3. 数据库自动编号，速度快，而且是增量增长，按顺序存放，对于检索非常有利。
4. 数字型，占用空间小，易排序，在程序中传递也方便。

auto_increment 的缺点：

1. 由于是自增，很容易通过网络爬虫知晓当前系统的业务量。
2. 高并发的情况下，竞争自增锁会降低数据库的吞吐能力。
3. 数据迁移或分库分表场景下，自增方式不再适用。

UUID 的优点：

1. 不会冲突。进行数据拆分、合并存储的时候，能保证主键全局的唯一性
2. 可以在应用层生成，提高数据库吞吐能力

UUID 的缺点：

1. 影响插入速度， 并且造成硬盘使用率低。与自增相比，最大的缺陷就是随机io，下面我们会去具体解释
2. 字符串类型相比整数类型肯定更消耗空间，而且会比整数类型操作慢。

**uuid 和自增 id 的索引结构对比**

1、**使用自增 id 的内部结构**

![](https://dream-syz.github.io/6-27.png)

自增的主键的值是顺序的，所以 InnoDB 把每一条记录都存储在一条记录的后面。

* 当达到页面的最大填充因子时候（InnoDB 默认的最大填充因子是页大小的 15/16，会留出 1/16 的空间留作以后的修改）。
* 下一条记录就会写入新的页中，一旦数据按照这种顺序的方式加载，主键页就会近乎于顺序的记录填满，提升了页面的最大填充率，不会有页的浪费。
* 新插入的行一定会在原有的最大数据行下一行，MySQL 定位和寻址很快，不会为计算新行的位置而做出额外的消耗。减少了页分裂和碎片的产生。

2、**使用 uuid 的索引内部结构**

插入UUID： 新的记录可能会插入之前记录的中间，因此需要移动之前的记录

![](https://dream-syz.github.io/6-28.png)

被写满已经刷新到磁盘上的页可能会被重新读取

![](https://dream-syz.github.io/6-29.png)

因为 uuid 相对顺序的自增 id 来说是毫无规律可言的，新行的值不一定要比之前的主键的值要大，所以 innodb 无法做到总是把新行插入到索引的最后，而是需要为新行寻找新的合适的位置从而来分配新的空间。

这个过程需要做很多额外的操作，数据的毫无顺序会导致数据分布散乱，将会导致以下的问题：

1. 写入的目标页很可能已经刷新到磁盘上并且从缓存上移除，或者还没有被加载到缓存中，innodb 在插入之前不得不先找到并从磁盘读取目标页到内存中，这将导致大量的随机 IO。
2. 因为写入是乱序的，innodb 不得不频繁的做页分裂操作，以便为新的行分配空间，页分裂导致移动大量的数据，一次插入最少需要修改三个页以上。
3. 由于频繁的页分裂，页会变得稀疏并被不规则的填充，最终会导致数据会有碎片。
4. 在把随机值（uuid 和雪花 id）载入到聚簇索引（InnoDB 默认的索引类型）以后，有时候会需要做一次 OPTIMEIZE TABLE 来重建表并优化页的填充，这将又需要一定的时间消耗。

结论：使用 InnoDB 应该尽可能的按主键的自增顺序插入，并且尽可能使用单调的增加的聚簇键的值来插入新行。如果是分库分表场景下，分布式主键 ID 的生成方案 优先选择雪花算法生成全局唯一主键（雪花算法生成的主键在一定程度上是有序的）。





### 43、B 树和 B+ 树的区别是什么？

**1）B-Tree 介绍**

B-Tree 是一种平衡的多路查找树，B 树允许一个节点存放多个数据。这样可以在尽可能减少树的深度的同时，存放更多的数据(把瘦高的树变的矮胖).

B-Tree 中所有节点的子树个数的最大值称为 B-Tree 的阶，用 m 表示。一颗 m 阶的 B 树，如果不为空，就必须满足以下条件。

m 阶的 B-Tree 满足以下条件:

1. 每个节点最多拥有 m-1 个关键字(根节点除外)，也就是 m 个子树
2. 根节点至少有两个子树(可以没有子树,有就必须是两个)
3. 分支节点至少有（m/2）颗子树 (除去根节点和叶子节点其他都是分支节点)
4. 所有叶子节点都在同一层,并且以升序排序

![](https://dream-syz.github.io/6-32.png)

**什么是 B-Tree 的阶 ?**
所有节点中，节点【60,70,90】拥有的子节点数目最多，四个子节点（灰色节点），所以上面的 B-Tree 为 4 阶 B 树。

**B-Tree 结构存储索引的特点**

为了描述 B-Tree 首先定义一条记录为一个键值对[key, data] ，key 为记录的键值，对应表中的主键值(聚簇索引)，data 为一行记录中除主键外的数据。对于不同的记录，key值互不相同

* 索引值和 data 数据分布在整棵树结构中
* 白色块部分是指针，存储着子节点的地址信息。
* 每个节点可以存放多个索引值及对应的 data 数据
* 树节点中的多个索引值从左到右升序排列

![](https://dream-syz.github.io/6-33.png)

**B-Tree 的查找操作**

B-Tree 的每个节点的元素可以视为一次 I/O 读取，树的高度表示最多的 I/O 次数，在相同数量的总元素个数下，每个节点的元素个数越多，高度越低，查询所需的 I/O 次数越少.

**B-Tree 总结**

* 优点: B 树可以在内部节点存储键值和相关记录数据，因此把频繁访问的数据放在靠近根节点的位置将大大提高热点数据的查询效率。
* 缺点: B 树中每个节点不仅包含数据的 key 值，还有 data 数据.。所以当 data 数据较大时，会导致每个节点存储的 key 值减少，并且导致 B 树的层数变高。增加查询时的 IO 次数。
* 使用场景: B 树主要应用于文件系统以及部分数据库索引，如 MongoDB，大部分关系型数据库索引则是使用 B+ 树实现

**2）B+Tree**

B+Tree 是在 B-Tree 基础上的一种优化，使其更适合实现存储索引结构，InnoDB 存储引擎就是用 B+Tree 实现其索引结构。

**B+Tree 的特征**

- 非叶子节点只存储键值信息.
- 所有叶子节点之间都有一个链指针.
- 数据记录都存放在叶子节点中.

![](https://dream-syz.github.io/6-34.png)

**B+Tree 的优势**

1. B+Tree 是 B Tree 的变种，B Tree 能解决的问题，B+Tree 也能够解决（降低树的高度，增大节点存储数据量）
2. B+Tree 扫库和扫表能力更强，如果我们要根据索引去进行数据表的扫描，对 B Tree 进行扫描，需要把整棵树遍历一遍，而 B+Tree只需要遍历他的所有叶子节点即可（叶子节点之间有引用）。
3. B+Tree 磁盘读写能力更强，他的根节点和支节点不保存数据区，所有根节点和支节点同样大小的情况下，保存的关键字要比 B Tree要多。而叶子节点不保存子节点引用。所以，B+Tree 读写一次磁盘加载的关键字比 B Tree 更多。
4. B+Tree 排序能力更强，如上面的图中可以看出，B+Tree 天然具有排序功能。
5. B+Tree 查询效率更加稳定，每次查询数据，查询 IO 次数一定是稳定的。当然这个每个人的理解都不同，因为在 B Tree 如果根节点命中直接返回，确实效率更高。



### 44、一个 B+ 树中大概能存放多少条索引记录？

MySQL 设计者将一个 B+Tree 的节点的大小设置为等于一个页. (这样做的目的是每个节点只需要一次 I/O 就可以完全载入)， InnoDB 的一个页的大小是 16 KB，所以每个节点的大小也是 16KB, 并且 B+Tree 的根节点是保存在内存中的，子节点才是存储在磁盘上。

![](https://dream-syz.github.io/6-35.png)

**假设一个 B+ 树高为 2，即存在一个根节点和若干个叶子节点，那么这棵 B+ 树的存放总记录数为：**

**根节点指针数 单个叶子节点记录行数.**

* **计算根节点指针数**: 假设表的主键为 INT 类型,占用的就是 4 个字节,或者是 BIGINT 占用 8 个字节, 指针大小为 6 个字节,那么一个页(就是 B+Tree 中的一个节点) ,大概可以存储: 16384B / (4B+6B) = 1638 ,一个节点最多可以存储 1638  个索引指针.
* **计算每个叶子节点的记录数**:我们假设一行记录的数据大小为1k,那么一页就可以存储16行数据,16KB / 1KB = 16.
* **一颗高度为 2 的 B+Tree 可以存放的记录数为**: 1638 * 16=26208 条数据记录, 同样的原理可以推算出一个高度 3 的 B+Tree 可以存放: 1638 * 1638 * 16 = 42928704 条这样的记录.

**所以 InnoDB 中的 B+Tree 高度一般为 1-3 层,就可以满足千万级别的数据存储**,在查找数据时一次页的查找代表一次 IO，所以通过主键索引查询通常只需要 1-3 次 IO 操作即可查找到数据。



### 45、explain 用过吗，有哪些主要字段？

使用 `EXPLAIN` 关键字可以模拟优化器来执行SQL查询语句，从而知道 MySQL 是如何处理我们的 SQL 语句的。分析出查询语句或是表结构的性能瓶颈。

**MySQL 查询过程**

![](https://dream-syz.github.io/6-36.png)

**通过 explain 我们可以获得以下信息：**

* **表的读取顺序**
* **数据读取操作的操作类型**
* **哪些索引可以被使用**
* **哪些索引真正被使用**
* **表的直接引用**
* **每张表的有多少行被优化器查询了**

Explain使用方式: **explain+sql 语句**, 通过执行 explain 可以获得 sql 语句执行的相关信息

```
explain select * from users;
```



### 46、type 字段中有哪些常见的值？

**type 字段在 MySQL 官网文档描述如下：**

> **The join type. For descriptions of the different types.**

**type 字段显示的是连接类型 ( join type 表示的是用什么样的方式来获取数据)，它描述了找到所需数据所使用的扫描方式, 是较为重要的一个指标。**

**下面给出各种连接类型,按照从最佳类型到最坏类型进行排序:**

```
-- 完整的连接类型比较多
system > const > eq_ref > ref > fulltext > ref_or_null > index_merge > unique_subquery > index_subquery > range > index > ALL

-- 简化之后,我们可以只关注一下几种
system > const > eq_ref > ref > range > index > ALL
```

> **一般来说,需要保证查询至少达到 range 级别,最好能到 ref,否则就要就行 SQL 的优化调整**

下面介绍 type 字段不同值表示的含义:

| **type类 型** | **解释**                                                     |
| ------------- | ------------------------------------------------------------ |
| **system**    | **不进行磁盘 IO,查询系统表,仅仅返回一条数据**                |
| **const**     | **查找主键索引,最多返回1条或0条数据. 属于精确查找**          |
| **eq_ref**    | **查找唯一性索引,返回数据最多一条, 属于精确查找**            |
| **ref**       | **查找非唯一性索引,返回匹配某一条件的多条数据,属于精确查找,数据返回可能是多条.** |
| **range**     | **查找某个索引的部分索引,只检索给定范围的行,属于范围查找. 比如: > 、 < 、in 、between** |
| **index**     | **查找所有索引树,比ALL快一些,因为索引文件要比数据文件小.**   |
| **ALL**       | **不使用任何索引,直接进行全表扫描**                          |
|               |                                                              |



### 47、Extra 有哪些主要指标，各自的含义是什么？

Extra 是 EXPLAIN 输出中另外一个很重要的列，该列显示 MySQL 在查询过程中的一些详细信息

| **extra类型**             | **解释**                                                     |
| ------------------------- | ------------------------------------------------------------ |
| **Using filesort**        | **MySQL中无法利用索引完成的排序操作称为  “文件排序”**        |
| **Using index**           | **表示直接访问索引就能够获取到所需要的数据（覆盖索引），不需要通过索引回表** |
| **Using index condition** | **搜索条件中虽然出现了索引列，但是有部分条件无法使用索引，** **会根据能用索引的条件先搜索一遍再匹配无法使用索引的条件。** |
| **Using join buffer**     | **使用了连接缓存, 会显示join连接查询时,MySQL选择的查询算法** |
| **Using temporary**       | **表示MySQL需要使用临时表来存储结果集，常见于排序和分组查询** |
| **Using where**           | **意味着全表扫描或者在查找使用索引的情况下，但是还有查询条件不在索引字段当中** |



### 48、如何进行分页查询优化？

- 一般性分页

  一般的分页查询使用简单的 limit 子句就可以实现。limit格式如下：

  ```
  SELECT * FROM 表名 LIMIT [offset,] rows
  ```

  - 第一个参数指定第一个返回记录行的偏移量，注意从0开始；
  - 第二个参数指定返回记录行的最大数目；
  - 如果只给定一个参数，它表示返回最大的记录行数目；

  **思考1：如果偏移量固定，返回记录量对执行时间有什么影响？**

  ```
  select * from user limit 10000,1;
  select * from user limit 10000,10;
  select * from user limit 10000,100;
  select * from user limit 10000,1000;
  select * from user limit 10000,10000;
  ```

  结果：在查询记录时，返回记录量低于100条，查询时间基本没有变化，差距不大。随着查询记录量越大，所花费的时间也会越来越多。

  **思考2：如果查询偏移量变化，返回记录数固定对执行时间有什么影响？**

  ```
  select * from user limit 1,100;
  select * from user limit 10,100;
  select * from user limit 100,100;
  select * from user limit 1000,100;
  select * from user limit 10000,100;
  ```

  结果：在查询记录时，如果查询记录量相同，偏移量超过100后就开始随着偏移量增大，查询时间急剧的增加。（这种分页查询机制，每次都会从数据库第一条记录开始扫描，越往后查询越慢，而且查询的数据越多，也会拖慢总查询速度。）

- 分页优化方案

  **优化1: 通过索引进行分页**

  直接进行 limit 操作 会产生全表扫描，速度很慢. Limit 限制的是从结果集的 M 位置处取出 N 条输出，其余抛弃.

  假设 ID 是连续递增的，我们根据查询的页数和查询的记录数可以算出查询的 id 的范围，然后配合 limit 使用

  ```
  EXPLAIN SELECT * FROM user WHERE id  >= 100001 LIMIT 100;
  ```

  **优化2：利用子查询优化**

  ```
  -- 首先定位偏移位置的id
  SELECT id FROM user_contacts LIMIT 100000,1;
  
  -- 根据获取到的id值向后查询.
  EXPLAIN SELECT * FROM user_contacts WHERE id >=
  (SELECT id FROM user_contacts LIMIT 100000,1) LIMIT 100;
  ```

  原因：使用了 id 做主键比较 (id>=)，并且子查询使用了覆盖索引进行优化。

  

### 49、如何做慢查询优化？

**MySQL 慢查询的相关参数解释：**

* **slow_query_log**：是否开启慢查询日志，`ON(1)`表示开启,
  `OFF(0)` 表示关闭。
* **slow-query-log-file**：新版（5.6 及以上版本）MySQL 数据库慢查询日志存储路径。
* **long_query_time**： 慢查询 **阈值**，当查询时间多于设定的阈值时，记录日志。

**慢查询配置方式**

1. **默认情况下 slow_query_log 的值为 OFF，表示慢查询日志是禁用的**

```
mysql> show variables like '%slow_query_log%';
+---------------------+------------------------------+
| Variable_name       | Value                        |
+---------------------+------------------------------+
| slow_query_log      | ON                           |
| slow_query_log_file | /var/lib/mysql/test-slow.log |
+---------------------+------------------------------+
```

2. **可以通过设置 slow_query_log 的值来开启**

```
mysql> set global slow_query_log=1;
```

3. **使用** `set global slow_query_log=1`  开启了慢查询日志只对当前数据库生效，MySQL 重启后则会失效。如果要永久生效，就必须修改配置文件 my.cnf（其它系统变量也是如此）

```
-- 编辑配置
vim /etc/my.cnf

-- 添加如下内容
slow_query_log =1
slow_query_log_file=/var/lib/mysql/ruyuan-slow.log

-- 重启MySQL
service mysqld restart

mysql> show variables like '%slow_query%';
+---------------------+--------------------------------+
| Variable_name       | Value                          |
+---------------------+--------------------------------+
| slow_query_log      | ON                             |
| slow_query_log_file | /var/lib/mysql/ruyuan-slow.log |
+---------------------+--------------------------------+
```

4. 那么开启了慢查询日志后，什么样的 SQL 才会记录到慢查询日志里面呢？ 这个是由参数 `long_query_time`控制，默认情况下long_query_time 的值为 10 秒.

```sql
mysql> show variables like 'long_query_time';
+-----------------+-----------+
| Variable_name   | Value     |
+-----------------+-----------+
| long_query_time | 10.000000 |
+-----------------+-----------+

mysql> set global long_query_time=1;
Query OK, 0 rows affected (0.00 sec)

mysql>  show variables like 'long_query_time';
+-----------------+-----------+
| Variable_name   | Value     |
+-----------------+-----------+
| long_query_time | 10.000000 |
+-----------------+-----------+
```

5. **修改了变量 long_query_time，但是查询变量 long_query_time 的值还是10，难道没有修改到呢？注意：使用命令 set global long_query_time=1 修改后，需要重新连接或新开一个会话才能看到修改值。**

```
mysql> show variables like 'long_query_time';
+-----------------+----------+
| Variable_name   | Value    |
+-----------------+----------+
| long_query_time | 1.000000 |
+-----------------+----------+
```

6. `log_output` 参数是指定日志的存储方式。`log_output='FILE'`  表示将日志存入文件，默认值是'FILE'。`log_output='TABLE'` 表示将日志存入数据库，这样日志信息就会被写入到 mysql.slow_log 表中。

```
mysql> SHOW VARIABLES LIKE '%log_output%';
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| log_output    | FILE  |
+---------------+-------+
```

> **MySQL 数据库支持同时两种日志存储方式，配置的时候以逗号隔开即可，如：log_output='FILE,TABLE'。日志记录到系统的专用日志表中，要比记录到文件耗费更多的系统资源，因此对于需要启用慢查询日志，又需要能够获得更高的系统性能，那么建议优先记录到文件.**

7. 系统变量 `log-queries-not-using-indexes`：未使用索引的查询也被记录到慢查询日志中（可选项）。如果调优的话，建议开启这个选项。

```
mysql> show variables like 'log_queries_not_using_indexes';
+-------------------------------+-------+
| Variable_name                 | Value |
+-------------------------------+-------+
| log_queries_not_using_indexes | OFF   |
+-------------------------------+-------+

mysql> set global log_queries_not_using_indexes=1;
Query OK, 0 rows affected (0.00 sec)

mysql> show variables like 'log_queries_not_using_indexes';
+-------------------------------+-------+
| Variable_name                 | Value |
+-------------------------------+-------+
| log_queries_not_using_indexes | ON    |
+-------------------------------+-------+
```

**3) 慢查询测试**

1. **执行 test_index.sql 脚本,监控慢查询日志内容**

```
[root@localhost mysql]# tail -f /var/lib/mysql/ruyuan-slow.log 
/usr/sbin/mysqld, Version: 5.7.30-log (MySQL Community Server (GPL)). started with:
Tcp port: 0  Unix socket: /var/lib/mysql/mysql.sock
Time                 Id Command    Argument
```

2. **执行下面的 SQL，执行超时 (超过1秒) 我们去查看慢查询日志**

```
SELECT * FROM test_index WHERE  
hobby = '20009951' OR hobby = '10009931' OR hobby = '30009931' 
OR dname = 'name4000' OR dname = 'name6600' ;
```

3. **日志内容**

**我们得到慢查询日志后，最重要的一步就是去分析这个日志。我们先来看下慢日志里到底记录了哪些内容。**

**如下图是慢日志里其中一条 SQL 的记录内容，可以看到有时间戳，用户，查询时长及具体的 SQL 等信息.**

```
# Time: 2022-02-23T13:50:45.005959Z
# User@Host: root[root] @ localhost []  Id:     3
# Query_time: 3.724273  Lock_time: 0.000371 Rows_sent: 5  Rows_examined: 5000000
SET timestamp=1645624245;
select * from test_index where hobby = '20009951' or hobby = '10009931' or hobby = '30009931' or dname = 'name4000' or dname = 'name6600';
```

* **Time: 执行时间**
* **User: 用户信息 ,Id信息**
* **Query_time: 查询时长**
* **Lock_time: 等待锁的时长**
* **Rows_sent:查询结果的行数**
* **Rows_examined: 查询扫描的行数**
* **SET timestamp: 时间戳**
* **SQL的具体信息**

**慢查询 SQL 优化思路**

**1) SQL性能下降的原因**

在日常的运维过程中，经常会遇到 DBA 将一些执行效率较低的 SQL 发过来找开发人员分析，当我们拿到这个 SQL 语句之后，在对这些SQL 进行分析之前，需要明确可能导致 SQL 执行性能下降的原因进行分析，执行性能下降可以体现在以下两个方面：

- **等待时间长**

  ```
  锁表导致查询一直处于等待状态，后续我们从 MySQL 锁的机制去分析 SQL 执行的原理
  ```

- **执行时间长**

  ```
  1.查询语句写的烂
  2.索引失效 
  3.关联查询太多join 
  4.服务器调优及各个参数的设置
  ```

**2) 慢查询优化思路**

1. 优先选择优化高并发执行的 SQL，因为高并发的 SQL 发生问题带来后果更严重.

   ```
   比如下面两种情况:
      SQL1: 每小时执行10000次, 每次20个IO 优化后每次18个IO,每小时节省2万次IO
      SQL2: 每小时10次,每次20000个IO,每次优化减少2000个IO,每小时节省2万次IO
      SQL2更难优化,SQL1更好优化.但是第一种属于高并发SQL,更急需优化 成本更低
   ```

2. 定位优化对象的性能瓶颈(在优化之前了解性能瓶颈在哪)

   ```
   在去优化 SQL 时,选择优化分方向有三个: 
     1.IO(数据访问消耗的了太多的时间,查看是否正确使用了索引) , 
     2.CPU(数据运算花费了太多时间, 数据的运算分组 排序是不是有问题) 
     3.网络带宽(加大网络带宽)
   ```

3. 明确优化目标

   ```
   需要根据数据库当前的状态
   数据库中与该条SQL的关系
   当前SQL的具体功能
   最好的情况消耗的资源,最差情况下消耗的资源,优化的结果只有一个给用户一个好的体验
   ```

4. 从 explain 执行计划入手

   ```
   只有 explain 能告诉你当前 SQL 的执行状态
   ```

5. 永远用小的结果集驱动大的结果集

   ```java
   小的数据集驱动大的数据集,减少内层表读取的次数
   
   类似于嵌套循环
   for(int i = 0; i < 5; i++){
   	for(int i = 0; i < 1000; i++){
   
   	}
   }
   如果小的循环在外层,对于数据库连接来说就只连接5次,进行5000次操作,如果1000在外,则需要进行1000次数据库连接,从而浪费资源，增加消耗.这就是为什么要小表驱动大表。
   ```

6. 尽可能在索引中完成排序

   ```
   排序操作用的比较多,order by 后面的字段如果在索引中,索引本来就是排好序的,所以速度很快,没有索引的话,就需要从表中拿数据,在内存中进行排序,如果内存空间不够还会发生落盘操作
   ```

7. 只获取自己需要的列

   ```
   不要使用select  * ,select * 很可能不走索引,而且数据量过大
   ```

8. 只使用最有效的过滤条件

   ```
   误区 where后面的条件越多越好,但实际上是应该用最短的路径访问到数据
   ```

9. 尽可能避免复杂的join和子查询

   ```
   每条SQL的JOIN操作 建议不要超过三张表
   将复杂的SQL, 拆分成多个小的SQL 单个表执行,获取的结果 在程序中进行封装
   如果join占用的资源比较多,会导致其他进程等待时间变长
   ```

10. 合理设计并利用索引

    ```
    如何判定是否需要创建索引?
     1.较为频繁的作为查询条件的字段应该创建索引.
     2.唯一性太差的字段不适合单独创建索引，即使频繁作为查询条件.（唯一性太差的字段主要是指哪些呢？如状态字段，类型字段等等这些字段中的数据可能总共就是那么几个几十个数值重复使用）（当一条Query所返回的数据超过了全表的15%的时候，就不应该再使用索引扫描来完成这个Query了）.
     3.更新非常频繁的字段不适合创建索引.（因为索引中的字段被更新的时候，不仅仅需要更新表中的数据，同时还要更新索引数据，以确保索引信息是准确的）.
     4.不会出现在WHERE子句中的字段不该创建索引.
    
    如何选择合适索引?
     1.对于单键索引，尽量选择针对当前Query过滤性更好的索引.
     2.选择联合索引时,当前Query中过滤性最好的字段在索引字段顺序中排列要靠前.
     3.选择联合索引时,尽量索引字段出现在w中比较多的索引.
    ```



### 50、Hash 索引有哪些优缺点？

MySQL 中索引的常用数据结构有两种: 一种是 B+Tree，另一种则是 Hash.

Hash 底层实现是由 Hash 表来实现的，是根据键值 <key,value> 存储数据的结构。非常适合根据 key 查找 value 值，也就是单个 key 查询，或者说等值查询。

![](https://dream-syz.github.io/6-37.png)

对于每一行数据，存储引擎都会对所有的索引列计算一个哈希码，哈希码是一个较小的值,如果出现哈希码值相同的情况会拉出一条链表.

Hsah索引的优点

* 因为索引自身只需要存储对应的 Hash 值,所以索引结构非常紧凑, 只需要做等值比较查询，而不包含排序或范围查询的需求，都适合使用哈希索引 .
* 没有哈希冲突的情况下,等值查询访问哈希索引的数据非常快.(如果发生 Hash 冲突,存储引擎必须遍历链表中的所有行指针,逐行进行比较,直到找到所有符合条件的行).

Hash 索引的缺点

* 哈希索引只包含哈希值和行指针，而不存储字段值，所以不能使用索引中的值来避免读取行。
* 哈希索引只支持等值比较查询。不支持任何范围查询和部分索引列匹配查找。
* 哈希索引数据并不是按照索引值顺序存储的，所以也就无法用于排序。



### 51、说一下 InnoDB 内存相关的参数优化？

Buffer Pool 参数优化

1.1 缓冲池内存大小配置

一个大的日志缓冲区允许大量的事务在提交之前不写日志到磁盘。因此，如果你有很多事务的更新，插入或删除操作，通过设置这个参数会大量的减少磁盘 I/O 的次数数。
建议：在专用数据库服务器上，可以将缓冲池大小设置为服务器物理内存的 60 % - 80 %.

* **查看缓冲池大小**

  ```
  mysql> show variables like '%innodb_buffer_pool_size%';
  +-------------------------+-----------+
  | Variable_name           | Value     |
  +-------------------------+-----------+
  | innodb_buffer_pool_size | 134217728 |
  +-------------------------+-----------+
  
  mysql> select 134217728 / 1024 / 1024;
  +-------------------------+
  | 134217728 / 1024 / 1024 |
  +-------------------------+
  |            128.00000000 |
  +-------------------------+
  ```

* **在线调整 InnoDB 缓冲池大小**
  **innodb_buffer_pool_size 可以动态设置，允许在不重新启动服务器的情况下调整缓冲池的大小.**

  ```
  mysql> SET GLOBAL innodb_buffer_pool_size = 268435456; -- 512
  Query OK, 0 rows affected (0.10 sec)
  
  mysql> show variables like '%innodb_buffer_pool_size%';
  +-------------------------+-----------+
  | Variable_name           | Value     |
  +-------------------------+-----------+
  | innodb_buffer_pool_size | 268435456 |
  +-------------------------+-----------+
  ```

  **监控在线调整缓冲池的进度**

  ```
  mysql> SHOW STATUS WHERE Variable_name='InnoDB_buffer_pool_resize_status';
  +----------------------------------+----------------------------------------------------------------------+
  | Variable_name                    | Value                                                        |
  +----------------------------------+----------------------------------------------------------------------+
  | Innodb_buffer_pool_resize_status | Size did not change (old size = new size = 268435456. Nothing to do. |
  +----------------------------------+----------------------------------------------------------------------+
  ```

**1.3 InnoDB 缓存性能评估**

当前配置的 innodb_buffer_pool_size 是否合适，可以通过分析 InnoDB 缓冲池的缓存命中率来验证。

* 以下公式计算 InnoDB buffer pool 命中率:

  ```
  命中率 = innodb_buffer_pool_read_requests / (innodb_buffer_pool_read_requests+innodb_buffer_pool_reads)* 100
  
  参数1: innodb_buffer_pool_reads：表示InnoDB缓冲池无法满足的请求数。需要从磁盘中读取。
  参数2: innodb_buffer_pool_read_requests：表示从内存中读取页的请求数。
  ```

  ```
  mysql> show status like 'innodb_buffer_pool_read%';
  +---------------------------------------+-------+
  | Variable_name                         | Value |
  +---------------------------------------+-------+
  | Innodb_buffer_pool_read_ahead_rnd     | 0     |
  | Innodb_buffer_pool_read_ahead         | 0     |
  | Innodb_buffer_pool_read_ahead_evicted | 0     |
  | Innodb_buffer_pool_read_requests      | 12701 |
  | Innodb_buffer_pool_reads              | 455   |
  +---------------------------------------+-------+
  
  -- 此值低于90%，则可以考虑增加innodb_buffer_pool_size。
  mysql> select 12701 / (455 + 12701) * 100 ;
  +-----------------------------+
  | 12701 / (455 + 12701) * 100 |
  +-----------------------------+
  |                     96.5415 |
  +-----------------------------+
  ```

**1.4 Page 管理相关参数**

**查看 Page 页的大小(默认16KB),**`innodb_page_size`只能在初始化 MySQL 实例之前配置，不能在之后修改。如果没有指定值，则使用默认页面大小初始化实例。

```
mysql> show variables like '%innodb_page_size%'; 
+------------------+-------+
| Variable_name    | Value |
+------------------+-------+
| innodb_page_size | 16384 |
+------------------+-------+
```

**Page 页管理状态相关参数**

```
mysql> show global status like '%innodb_buffer_pool_pages%';
+----------------------------------+-------+
| Variable_name                    | Value |
+----------------------------------+-------+
| Innodb_buffer_pool_pages_data    | 515   |
| Innodb_buffer_pool_pages_dirty   | 0     |
| Innodb_buffer_pool_pages_flushed | 334   |
| Innodb_buffer_pool_pages_free    | 15868 |
| Innodb_buffer_pool_pages_misc    | 0     |
| Innodb_buffer_pool_pages_total   | 16383 |
+----------------------------------+-------+
```

**pages_data**: InnoDB 缓冲池中包含数据的页数。 该数字包括脏页面和干净页面。

**pages_dirty**: 显示在内存中修改但尚未写入数据文件的 InnoDB 缓冲池数据页的数量（脏页刷新）。

**pages_flushed**: 表示从 InnoDB 缓冲池中刷新脏页的请求数。

**pages_free**: 显示 InnoDB 缓冲池中的空闲页面

**pages_misc**: 缓存池中当前已经被用作管理用途或 hash index 而不能用作为普通数据页的数目

**pages_total**: 缓存池的页总数目。单位是 page。



### 52、InnoDB 日志相关的参数优化了解过吗？

**1.日志缓冲区相关参数配置**

**日志缓冲区的大小。一般默认值 16MB 是够用的，但如果事务之中含有 blog/text 等大字段，这个缓冲区会被很快填满会引起额外的IO 负载。配置更大的日志缓冲区,可以有效的提高 MySQL 的效率.**

* **innodb_log_buffer_size 缓冲区大小**

  ```
  mysql> show variables like 'innodb_log_buffer_size';
  +------------------------+----------+
  | Variable_name          | Value    |
  +------------------------+----------+
  | innodb_log_buffer_size | 16777216 |
  +------------------------+----------+
  ```

* **innodb_log_files_in_group 日志组文件个数**
  **日志组根据需要来创建。而日志组的成员则需要至少 2 个，实现循环写入并作为冗余策略。**

  ```
  mysql> show variables like 'innodb_log_files_in_group';
  +---------------------------+-------+
  | Variable_name             | Value |
  +---------------------------+-------+
  | innodb_log_files_in_group | 2     |
  +---------------------------+-------+
  ```

* **innodb_log_file_size 日志文件大小**
  **参数innodb_log_file_size 用于设定 MySQL 日志组中每个日志文件的大小(默认 48M)。此参数是一个全局的静态参数，不能动态修改。**
  **参数 innodb_log_file_size 的最大值，二进制日志文件大小（innodb_log_file_size * innodb_log_files_in_group）不能超过512GB.所以单个日志文件的大小不能超过 256G.**

  ```
  mysql> show variables like 'innodb_log_file_size';
  +----------------------+----------+
  | Variable_name        | Value    |
  +----------------------+----------+
  | innodb_log_file_size | 50331648 |
  +----------------------+----------+
  ```

**2.日志文件参数优化**

首先我们先来看一下日志文件大小设置对性能的影响

* 设置过小
  1. 参数 `innodb_log_file_size`设置太小，就会导致 MySQL 的日志文件( redo log）频繁切换，频繁的触发数据库的检查点（Checkpoint），导致刷新脏页到磁盘的次数增加。从而影响 IO 性能。
  2. 处理大事务时，将所有的日志文件写满了，事务内容还没有写完，这样就会导致日志不能切换.
* 设置过大
  参数 `innodb_log_file_size`如果设置太大，虽然可以提升 IO 性能，但是当 MySQL 由于意外宕机时，二进制日志很大，那么恢复的时间必然很长。而且这个恢复时间往往不可控，受多方面因素影响。

**优化建议:**

如何设置合适的日志文件大小 ?

* 根据实际生产场景的优化经验,一般是计算一段时间内生成的事务日志（redo log）的大小， 而 MySQL 的日志文件的大小最少应该承载一个小时的业务日志量(官网文档中有说明)。

想要估计一下 InnoDB redo log 的大小，需要抓取一段时间内 Log SequenceNumber（日志顺序号）的数据,来计算一小时内产生的日志大小.

> **Log sequence number**
>
> **自系统修改开始，就不断的生成 redo 日志。为了记录一共生成了多少日志，于是 mysql 设计了全局变量 log sequence number，简称 lsn，但不是从 0 开始，是从 8704 字节开始。**

```
-- pager分页工具, 只获取 sequence的信息
mysql> pager grep sequence;
PAGER set to 'grep sequence'

-- 查询状态,并倒计时一分钟
mysql> show engine innodb status\G select sleep(60);
Log sequence number 5399154
1 row in set (0.00 sec)

1 row in set (1 min 0.00 sec)

-- 一分时间内所生成的数据量 5406150
mysql> show engine innodb status\G;
Log sequence number 5406150

-- 关闭pager
mysql> nopager;
PAGER set to stdout
```

有了一分钟的日志量,据此推算一小时内的日志量

```
mysql> select (5406150 - 5399154) / 1024 as kb_per_min;
+------------+
| kb_per_min |
+------------+
|     6.8320 |
+------------+

mysql> select (5406150 - 5399154) / 1024 * 60 as kb_per_min;
+------------+
| kb_per_min |
+------------+
|   409.9219 |
+------------+
```

太大的缓冲池或非常不正常的业务负载可能会计算出非常大(或非常小)的日志大小。这也是公式不足之处，需要根据判断和经验。但这个计算方法是一个很好的参考标准。



### 53、InnoDB IO 线程相关参数优化了解过吗？

数据库属于 IO 密集型的应用程序，其主要职责就是数据的管理及存储工作。从内存中读取一个数据库数据的时间是微秒级别，而从一块普通硬盘上读取一个 IO 是在毫秒级别。要优化数据库，IO 操作是必须要优化的，尽可能将磁盘 IO 转化为内存 IO。

**1) 参数: query_cache_size&have_query_cache**
MySQL 查询缓存会保存查询返回的完整结果。当查询命中该缓存，会立刻返回结果，跳过了解析，优化和执行阶段。
查询缓存会跟踪查询中涉及的每个表，如果这些表发生变化，那么和这个表相关的所有缓存都将失效。

1. 查看查询缓存是否开启

```
-- 查询是否支持查询缓存
mysql> show variables like 'have_query_cache';
+------------------+-------+
| Variable_name    | Value |
+------------------+-------+
| have_query_cache | YES   |
+------------------+-------+

-- 查询是否开启查询缓存 默认关闭
mysql> show variables like '%query_cache_type%';
+------------------+-------+
| Variable_name    | Value |
+------------------+-------+
| query_cache_type | OFF   |
+------------------+-------+
```

2. 开启缓存,在 my.ini 中添加下面一行参数

```
query_cache_size=128M
query_cache_type=1

query_cache_type:
设置为0，OFF,缓存禁用
设置为1，ON,缓存所有的结果
设置为2，DENAND,只缓存在select语句中通过SQL_CACHE指定需要缓存的查询
```

3. 测试能否缓存查询

```
  mysql> show status like '%Qcache%';
  +-------------------------+---------+
  | Variable_name           | Value   |
  +-------------------------+---------+
  | Qcache_free_blocks      | 1       |
  | Qcache_free_memory      | 1031832 |
  | Qcache_hits             | 0       |
  | Qcache_inserts          | 0       |
  | Qcache_lowmem_prunes    | 0       |
  | Qcache_not_cached       | 1       |
  | Qcache_queries_in_cache | 0       |
  | Qcache_total_blocks     | 1       |
  +-------------------------+---------+
```

* **Qcache_free_blocks**:缓存中目前剩余的 blocks 数量（如果值较大，则查询缓存中的内存碎片过多）
* **Qcache_free_memory**:空闲缓存的内存大小
* **Qcache_hits**:命中缓存次数
* **Qcache_inserts**: 未命中然后进行正常查询
* **Qcache_lowmem_prunes**:查询因为内存不足而被移除出查询缓存记录
* **Qcache_not_cached**: 没有被缓存的查询数量
* **Qcache_queries_in_cache**:当前缓存中缓存的查询数量
* **Qcache_total_blocks**:当前缓存的block数量

**优化建议**: Query Cache 的使用需要多个参数配合，其中最为关键的是 query_cache_size 和 query_cache_type ，前者设置用于缓存 ResultSet 的内存大小，后者设置在何场景下使用 Query Cache。

MySQL 数据库数据变化相对不多，query_cache_size  一般设置为 256MB 比较合适 ,也可以通过计算 Query Cache 的命中率来进行调整

```
( Qcache_hits / ( Qcache_hits + Qcache_inserts ) * 100) )
```

2) 参数: innodb_max_dirty_pages_pct 该参数是 InnoDB 存储引擎用来控制 buffer pool 中脏页的百分比，当脏页数量占比超过这个参数设置的值时，InnoDB 会启动刷脏页的操作。

```
-- innodb_max_dirty_pages_pct 参数可以动态调整，最小值为0， 最大值为99.99，默认值为 75。
mysql> show variables like 'innodb_max_dirty_pages_pct';
+----------------------------+-----------+
| Variable_name              | Value     |
+----------------------------+-----------+
| innodb_max_dirty_pages_pct | 75.000000 |
+----------------------------+-----------+
```

**优化建议**: 该参数比例值越大，从内存到磁盘的写入操作就会相对减少，所以能够一定程度下减少写入操作的磁盘 IO。但是，如果这个比例值过大，当数据库 Crash 之后重启的时间可能就会很长，因为会有大量的事务数据需要从日志文件恢复出来写入数据文件中.最大不建议超过90,一般重启恢复的数据在超过 1G B的话,启动速度就会变慢.

**3) 参数: innodb_old_blocks_pct&innodb_old_blocks_time**
`innodb_old_blocks_pct` 用来确定 LRU 链表中 old sublist 所占比例,默认占用 37%

```
mysql> show variables like '%innodb_old_blocks_pct%';
+-----------------------+-------+
| Variable_name         | Value |
+-----------------------+-------+
| innodb_old_blocks_pct | 37    |
+-----------------------+-------+
```

`innodb_old_blocks_time`  用来控制 old sublist 中 page 的转移策略，新的 page 页在进入 LRU 链表中时，会先插入到 old sublist 的头部，然后 page 需要在 old sublist 中停留 innodb_old_blocks_time 这么久后，下一次对该 page 的访问才会使其移动到 new sublist 的头部，默认值 1 秒.

```
mysql> show variables like '%innodb_old_blocks_time%';
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| innodb_old_blocks_time | 1000  |
+------------------------+-------+
```

**优化建议**: 在没有大表扫描的情况下，并且数据多为频繁使用的数据时，我们可以增加 innodb_old_blocks_pct 的值，并且减小innodb_old_blocks_time 的值。让数据页能够更快和更多的进入的热点数据区。



### 54、什么是写失效？

InnoDB 的页和操作系统的页大小不一致，InnoDB 页大小一般为 16K，操作系统页大小为 4 K，InnoDB 的页写入到磁盘时，一个页需要分 4 次写。

如果存储引擎正在写入页的数据到磁盘时发生了宕机，可能出现页只写了一部分的情况，比如只写了 4 K，就宕机了，这种情况叫做部分写失效（partial page write），可能会导致数据丢失。

![](https://dream-syz.github.io/6-38.png)

**双写缓冲区 Doublewrite Buffer**

为了解决写失效问题，InnoDB 实现了 double write buffer Files, 它位于系统表空间，是一个存储区域。

在 BufferPool 的 page 页刷新到磁盘真正的位置前，会先将数据存在 Doublewrite 缓冲区。这样在宕机重启时，如果出现数据页损坏，那么在应用 redo log 之前，需要通过该页的副本来还原该页，然后再进行 redo log 重做，double write 实现了 InnoDB 引擎数据页的可靠性.

默认情况下启用双写缓冲区，如果要禁用 Doublewrite 缓冲区，可以将 `innodb_doublewrite`设置为 0。

```sql
mysql> show variables like '%innodb_doublewrite%';
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| innodb_doublewrite | ON    |
+--------------------+-------+
1 row in set (0.01 sec)
```

数据双写流程

![](https://dream-syz.github.io/6-39.png)

* **step1**：当进行缓冲池中的脏页刷新到磁盘的操作时,并不会直接写磁盘,每次脏页刷新必须要先写 double write .
* **step2**：通过 memcpy 函数将脏页复制到内存中的 double write buffer .
* **step3**:  double write buffer 再分两次、每次 1 MB, 顺序写入共享表空间的物理磁盘上, **第一次写**.
* **step4**:  在完成 double write 页的写入后，再将 double wirite buffer 中的页写入各个表的 **独立表空间 **文件中(数据文件 .ibd), **第二次写**。

**为什么写两次 ?**

可能有的同学会有疑问，为啥写两次，刷一次数据文件保存数据不就可以了，为什么还要写共享表空间 ?其实是因为共享表空间是在ibdbata 文件中划出 2 M 连续的空间，专门给 double write 刷脏页用的, 由于在这个过程中，**double write 页的存储是连续的，因此写入磁盘为顺序写，性能很高**；完成 double write 后，再将脏页写入实际的各个表空间文件，这时写入就是离散的了.



### 55、什么是行溢出？

**行记录格式**

**1) 行格式分类**

表的行格式决定了它的行是如何物理存储的，这反过来又会影响查询和 DML 操作的性能。如果在单个 page 页中容纳更多行，查询和索引查找可以更快地工作，缓冲池中所需的内存更少，写入更新时所需的 I/O 更少。

InnoDB 存储引擎支持四种行格式：Redundant、Compact、Dynamic 和 Compressed .

查询 MySQL 使用的行格式，默认为: dynamic

```sql
mysql> show variables like 'innodb_default_row_format';
+---------------------------+---------+
| Variable_name             | Value   |
+---------------------------+---------+
| innodb_default_row_format | dynamic |
+---------------------------+---------+
```

指定行格式语法

```sql
CREATE TABLE <table_name(column_name)> ROW_FORMAT=行格式名称
ALTER TABLE <table_name> ROW_FORMAT=行格式名称
```

**2) COMPACT 行记录格式**

Compact 设计目标是高效地存储数据，一个页中存放的行数据越多，其性能就越高。

Compact行记录由两部分组成: 记录放入额外信息 和  记录的真实数据.

![](https://dream-syz.github.io/6-40.png)

**记录额外信息部分**

服务器为了描述一条记录而添加了一些额外信息(元数据信息)，这些额外信息分为 3 类，分别是: 变长字段长度列表、NULL 值列表和记录头信息.

- **变长字段长度列表**

  MySQL 支持一些变长的数据类型，比如 VARCHAR(M)、VARBINARY(M)、各种 TEXT 类型，各种 BLOB 类型，这些变长的数据类型占用的存储空间分为两部分：

  1. 真正的数据内容
  2. 占用的字节数

  变长字段的长度是不固定的，所以在存储数据的时候要把这些数据占用的字节数也存起来，读取数据的时候才能根据这个长度列表去读取对应长度的数据。

  在 `Compact`行格式中，把所有变长类型的列的长度都存放在记录的开头部位形成一个列表，按照列的顺序逆序存放,这个列表就是 **变长字段长度列表**。

- **NULL 值列表**

  表中的某些列可能会存储 NULL 值，如果把这些 NULL 值都放到记录的真实数据中会比较浪费空间，所以 Compact 行格式把这些值为 NULL 的列存储到 NULL 值列表中。( 如果表中所有列都不允许为 NULL，就不存在 NULL 值列表 )

- **记录头信息**

  记录头信息是由固定的 5 个字节组成，5 个字节也就是 40 个二进制位，不同的位代表不同的意思，这些头信息会在后面的一些功能中看到。

  | 名称         | 大小(单位:bit) | 描述                                                         |
  | ------------ | -------------- | ------------------------------------------------------------ |
  | 预留位1      | 1              | 没有使用                                                     |
  | 预留位2      | 1              | 没有使用                                                     |
  | delete_mask  | 1              | 标记该记录是否被删除                                         |
  | min_rec_mask | 1              | 标记该记录是否是本层 B+ 树的非叶子节点中的最小记录           |
  | n_owned      | 4              | 表示当前分组中管理的记录数                                   |
  | heap_no      | 13             | 表示当前记录在记录堆中的位置信息                             |
  | record_type  | 3              | 表示当前记录的类型:<br />0 表示普通记录,<br />1 表示 B+ 树非叶子节点记录,<br />2 表示最小记录,3表示最大记录 |
  | next_record  | 16             | 表示下一条记录的相对位置                                     |


  1. delete_mask

     这个属性标记着当前记录是否被删除，占用 1 个二进制位，值为 0 的时候代表记录并没有被删除，为 1 的时候代表记录被删除掉了

  2. min_rec_mask

     B+ 树的每层非叶子节点中的最小记录都会添加该标记。

  3. n_owned

     代表每个分组里，所拥有的记录的数量，一般是分组里主键最大值才有的。

  4. heap_no

     在数据页的 User Records 中插入的记录是一条一条紧凑的排列的，这种紧凑排列的结构又被称为堆。为了便于管理这个堆，把记录在堆中的相对位置给定一个编号——heap_no。所以 heap_no 这个属性表示当前记录在本页中的位置。

  5. record_type

     这个属性表示当前记录的类型，一共有 4 种类型的记录， 0 表示普通用户记录， 1 表示 B+ 树非叶节点记录， 2 表示最小记录， 3 表示最大记录。

  6. next_record

     表示从当前记录的真实数据到下一条记录的真实数据的地址偏移量，可以理解为指向下一条记录地址的指针。值为正数说明下一条记录在当前记录后面，为负数说明下一条记录在当前记录的前面。

- **记录真实数据部分**

  记录的真实数据除了插入的那些列的数据，MySQL 会为每个记录默认的添加一些列（也称为隐藏列），具体的列如下：<**MVCC 中的三个隐式字段**>

  ![](https://dream-syz.github.io/6-41.png)

  | 列名           | 是否必须 | 占用空间 | 描述                   |
  | -------------- | -------- | -------- | ---------------------- |
  | row_id         | 否       | 6字节    | 行 ID,唯一标识一条记录 |
  | transaction_id | 是       | 6字节    | 事务 ID                |
  | roll_pointer   | 是       | 7字节    | 回滚指针               |

  生成隐藏主键列的方式有:


    1. 服务器会在内存中维护一个全局变量，每当向某个包含隐藏的row_id列的表中插入一条记录时，就会把该变量的值当作新记录的row_id列的值，并且把该变量自增1。
    2. 每当这个变量的值为256的倍数时，就会将该变量的值刷新到系统表空间的页号为7的页面中一个Max Row ID的属性处。
    3. 当系统启动时，会将页中的Max Row ID属性加载到内存中，并将该值加上256之后赋值给全局变量，因为在上次关机时该全局变量的值可能大于页中Max Row ID属性值。
       4.

  **3) Compact 中的行溢出机制**

  **什么是行溢出 ?**

  MySQL 中是以页为基本单位,进行磁盘与内存之间的数据交互的,我们知道一个页的大小是16 KB,16 KB = 16384 字节.而一个 varchar(m) 类型列最多可以存储 65532 个字节,一些大的数据类型比如 TEXT 可以存储更多.

  如果一个表中存在这样的大字段,那么一个页就无法存储一条完整的记录.这时就会发生行溢出,多出的数据就会存储在另外的溢出页中.

  总结: 如果某些字段信息过长，无法存储在B树节点中，这时候会被单独分配空间，此时被称为溢出页，该字段被称为页外列。

  **Compact 中的行溢出机制**

  InnoDB 规定一页至少存储两条记录(B+ 树特点)，如果页中只能存放下一条记录，InnoDB 存储引擎会自动将行数据存放到溢出页中.
  当发生行溢出时，数据页只保存了前 768 字节的前缀数据，接着是 20 个字节的偏移量，指向行溢出页.

  ![](https://dream-syz.github.io/6-42.png)



### 56、如何进行 JOIN 优化？

JOIN 是 MySQL 用来进行联表操作的，用来匹配两个表的数据，筛选并合并出符合我们要求的结果集。

JOIN 操作有多种方式，取决于最终数据的合并效果。常用连接方式的有以下几种:

![](https://dream-syz.github.io/6-43.png)

什么是驱动表 ?

* 多表关联查询时,第一个被处理的表就是驱动表,使用驱动表去关联其他表.
* 驱动表的确定非常的关键,会直接影响多表关联的顺序,也决定后续关联查询的性能

驱动表的选择要遵循一个规则:

* 在对最终的结果集没有影响的前提下,优先选择结果集最小的那张表作为驱动表

**3) 三种 JOIN算法**

1.Simple Nested-Loop Join（ 简单的嵌套循环连接 )

* **简单来说嵌套循环连接算法就是一个双层for 循环 ，通过循环外层表的行数据，逐个与内层表的所有行数据进行比较来获取结果.**

* **这种算法是最简单的方案，性能也一般。对内循环没优化。**

* **例如有这样一条SQL:**

  ```
  -- 连接用户表与订单表 连接条件是 u.id = o.user_id
  select * from user t1 left join order t2 on t1.id = t2.user_id;
  -- user表为驱动表,order表为被驱动表
  ```

* 转换成代码执行时的思路是这样的:

  ```
  for(user表行 uRow : user表){
      for(Order表的行 oRow : order表){
          if(uRow.id = oRow.user_id){
              return uRow;
          }
      }
  }
  ```

* **匹配过程如下图**

  ![](https://dream-syz.github.io/6-44.png)

* **SNL 的特点**

  * **简单粗暴容易理解，就是通过双层循环比较数据来获得结果**
  * **查询效率会非常慢,假设 A 表有 N 行，B 表有 M 行。SNL 的开销如下：**
    * **A 表扫描 1 次。**
    * **B 表扫描 M 次。**
    * **一共有 N 个内循环，每个内循环要 M 次，一共有内循环 N * M 次**

**2) Index Nested-Loop Join（ 索引嵌套循环连接 ）**

* Index Nested-Loop Join 其优化的思路:  **主要是为了减少内层表数据的匹配次数** , 最大的区别在于，用来进行 join 的字段已经在被驱动表中建立了索引。

* 从原来的  `匹配次数 = 外层表行数 * 内层表行数` , 变成了  `匹配次数 = 外层表的行数 * 内层表索引的高度`  ，极大的提升了 join的性能。

* 当 `order`  表的   `user_id`  为索引的时候执行过程会如下图：

  ![](https://dream-syz.github.io/6-45.png)

**注意：使用Index Nested-Loop Join 算法的前提是匹配的字段必须建立了索引。**

**3) Block Nested-Loop Join( 块嵌套循环连接 )**

如果 join 的字段有索引，MySQL 会使用 INL 算法。如果没有的话，MySQL 会如何处理？

因为不存在索引了，所以被驱动表需要进行扫描。这里 MySQL 并不会简单粗暴的应用 SNL 算法，而是加入了 buffer 缓冲区，降低了内循环的个数，也就是被驱动表的扫描次数。

![](https://dream-syz.github.io/6-46.png)

* 在外层循环扫描 user表中的所有记录。扫描的时候，会把需要进行 join 用到的列都缓存到 buffer 中。buffer 中的数据有一个特点，里面的记录不需要一条一条地取出来和 order 表进行比较，而是整个 buffer 和 order表进行批量比较。

* 如果我们把 buffer 的空间开得很大，可以容纳下 user 表的所有记录，那么 order 表也只需要访问一次。

* MySQL 默认 buffer 大小 256K，如果有 n 个 join 操作，会生成 n-1 个 join buffer。

  ```
  mysql> show variables like '%join_buffer%';
  +------------------+--------+
  | Variable_name    | Value  |
  +------------------+--------+
  | join_buffer_size | 262144 |
  +------------------+--------+
  
  mysql> set session join_buffer_size=262144;
  Query OK, 0 rows affected (0.00 sec)
  ```

**4) JOIN 优化总结**

1. 永远用小结果集驱动大结果集(其本质就是减少外层循环的数据数量)
2. 为匹配的条件增加索引(减少内层表的循环匹配次数)
3. 增大 join buffer size 的大小（一次缓存的数据越多，那么内层包的扫表次数就越少）
4. 减少不必要的字段查询（字段越少，join buffer 所缓存的数据就越多



### 57、索引哪些情况下会失效？

1. 查询条件包含 or，会导致索引失效。
2. 隐式类型转换，会导致索引失效，例如 age 字段类型是 int，我们 where age = “1”，这样就会触发隐式类型转换
3. like 通配符会导致索引失效，注意:”ABC%” 不会失效，会走 range 索引，”% ABC” 索引会失效
4. 联合索引，查询时的条件列不是联合索引中的第一个列，索引失效。
5. 对索引字段进行函数运算。
6. 对索引列运算（如，+、-、*、/），索引失效。
7. 索引字段上使用（!= 或者 < >，not in）时，会导致索引失效。
8. 索引字段上使用 is null， is not null，可能导致索引失效。
9. 相 join 的两个表的字符编码不同，不能命中索引，会导致笛卡尔积的循环计算
10. mysql 估计使用全表扫描要比使用索引快，则不使用索引。



### 58、什么是覆盖索引？

**覆盖索引是一种避免回表查询的优化策略:  只需要在一棵索引树上就能获取 SQL 所需的所有列数据，无需回表，速度更快。**

具体的实现方式:

* *将被查询的字段建立普通索引或者联合索引*，这样的话就可以直接返回索引中的的数据，不需要再通过聚集索引去定位行记录，避免了回表的情况发生。

```
EXPLAIN SELECT user_name,user_age,user_level FROM users 
WHERE user_name = 'tom' AND user_age = 17;
```

![](https://dream-syz.github.io/6-47.png)

覆盖索引的定义与注意事项:

* 如果一个索引包含了 所有需要查询的字段的值 (不需要回表)，这个索引就是覆盖索引。
* MySQL 只能使用 B+Tree 索引做覆盖索引 (因为只有 B+ 树能存储索引列值)
* 在 explain 的 Extra 列, 如果出现 `Using index`  表示 使用到了覆盖索引 , 所取的数据完全在索引中就能拿到



### 59、介绍一下 MySQL 中事务的特性？

在关系型数据库管理系统中，一个逻辑工作单元要成为事务，必须满足这 4 个特性，即所谓的 ACID：原子性（Atomicity）、一致性（Consistency）、隔离性（Isolation）和持久性（Durability）。

1）原子性

原子性：事务作为一个整体被执行，包含在其中的对数据库的操作要么全部被执行，要么都不执行。

InnoDB 存储引擎提供了两种事务日志：redo log(重做日志)和undo log(回滚日志)。其中 redo log 用于保证事务持久性；undo log 则是事务原子性和隔离性实现的基础。

![](https://dream-syz.github.io/6-48.png)

每写一个事务,都会修改 Buffer Pool,从而产生相应的 Redo/Undo日志:

- 如果要回滚事务，那么就基于 undo log 来回滚就可以了，把之前对缓存页做的修改都给回滚了就可以了。
- 如果事务提交之后，redo log 刷入磁盘，结果 MySQL 宕机了，是可以根据 redo log 恢复事务修改过的缓存数据的。

实现原子性的关键，是当事务回滚时能够撤销所有已经成功执行的sql语句。

InnoDB 实现回滚，靠的是 undo log ：当事务对数据库进行修改时，InnoDB 会生成对应的 undo log  ；如果事务执行失败或调用了rollback ，导致事务需要回滚，便可以利用 undo log 中的信息将数据回滚到修改之前的样子。

![](https://dream-syz.github.io/6-49.png)

2）一致性

一致性：事务应确保数据库的状态从一个一致状态转变为另一个一致状态。*一致状态*的含义是数据库中的数据应满足完整性约束。

- 约束一致性：创建表结构时所指定的外键、唯一索引等约束。
- 数据一致性：是一个综合性的规定，因为它是由原子性、持久性、隔离性共同保证的结果，而不是单单依赖于某一种技术。

![](https://dream-syz.github.io/6-50.png)

3）隔离性

隔离性：指的是一个事务的执行不能被其他事务干扰，即一个事务内部的操作及使用的数据对其他的并发事务是隔离的。

不考虑隔离性会引发的问题:

- **脏读** : 一个事务读取到了另一个事务修改但未提交的数据。
- **不可重复读**: 一个事务中多次读取同一行记录的结果不一致，后面读取的跟前面读取的结果不一致。
- **幻读** : 一个事务中多次按相同条件查询，结果不一致。后续查询的结果和面前查询结果不同，多了或少了几行记录。

数据库事务的隔离级别有 4 个，由低到高依次为Read uncommitted 、Read committed、Repeatable read 、Serializable ，这四个级别可以逐个解决脏读 、不可重复读 、幻读 这几类问题。

4）持久性

持久性：指的是一个事务一旦提交，它对数据库中数据的改变就应该是永久性的，后续的操作或故障不应该对其有任何影响，不会丢失。

MySQL 事务的持久性保证依赖的日志文件: `redo log`

* redo log 也包括两部分：一是内存中的日志缓冲(redo log buffer)，该部分日志是易失性的；二是磁盘上的重做日志文件(redo log file)，该部分日志是持久的。redo log 是物理日志，记录的是数据库中物理页的情况 。
* 当数据发生修改时，InnoDB 不仅会修改 Buffer Pool 中的数据，也会在 redo log buffer 记录这次操作；当事务提交时，会对 redo log buffer 进行刷盘，记录到 redo log file 中。如果 MySQL 宕机，重启时可以读取 redo log file 中的数据，对数据库进行恢复。这样就不需要每次提交事务都实时进行刷脏了。

![](https://dream-syz.github.io/6-51.png)

5）ACID 总结

- 事务的持久化是为了应对系统崩溃造成的数据丢失.
- 只有保证了事务的一致性，才能保证执行结果的正确性
- 在非并发状态下，事务间天然保证隔离性，因此只需要保证事务的原子性即可保证一致性.
- 在并发状态下，需要严格保证事务的原子性、隔离性。

![](https://dream-syz.github.io/6-52.png)



### 60、MySQL 的可重复读怎么实现的？

可重复读（repeatable read）定义： 一个事务执行过程中看到的数据，总是跟这个事务在启动时看到的数据是一致的。

MVCC

* MVCC，多版本并发控制, 用于实现**读已提交**和**可重复读**隔离级别。
* MVCC 的核心就是 Undo log 多版本链 + Read view，“MV”就是通过 Undo log来保存数据的历史版本，实现多版本的管理，“CC”是通过 Read-view来实现管理，通过 Read-view原则来决定数据是否显示。同时针对不同的隔离级别， Read view 的生成策略不同，也就实现了不同的隔离级别。

**Undo log 多版本链**

每条数据都有三个隐藏字段:

| 列名           | 是否必须 | 占用空间 | 描述                                      |
| -------------- | -------- | -------- | ----------------------------------------- |
| row_id         | 否       | 6字节    | 行 ID,唯一标识一条记录                    |
| transaction_id | 是       | 6字节    | 事务 ID，记录最近一次更新这条数据的事务id |
| roll_pointer   | 是       | 7字节    | 回滚指针，指向之前生成的 undo log         |

![](https://dream-syz.github.io/6-53.png)

每一条数据都有多个版本,版本之间通过 undo log 链条进行连接通过这样的设计方式，可以保证每个事务提交的时候，一旦需要回滚操作,可以保证同一个事务只能读取到比当前版本更早提交的值,不能看到更晚提交的值。

**ReadView**

Read View是 InnoDB 在实现 MVCC 时用到的一致性读视图，即 consistent read view，用于支持 RC（Read Committed，读提交）和 RR（Repeatable Read，可重复读）隔离级别的实现.

Read View简单理解就是对数据在某个时刻的状态拍成照片记录下来。那么之后获取某时刻的数据时就还是原来的照片上的数据，是不会变的.

Read View中比较重要的字段有4个:

* `m_ids` : 用来表示MySQL中哪些事务正在执行,但是没有提交.
* `min_trx_id`: 就是m_ids里最小的值.
* `max_trx_id` : 下一个要生成的事务id值,也就是最大事务id
* `creator_trx_id`: 就是你这个事务的id

![](https://dream-syz.github.io/6-54.png)

当一个事务第一次执行查询 sql 时，会生成一致性视图 read-view（快照），查询时从 undo log 中最新的一条记录开始跟 read-view 做对比，如果不符合比较规则，就根据回滚指针回滚到上一条记录继续比较，直到得到符合比较条件的查询结果。

**Read View判断记录某个版本是否可见的规则如下**

![](https://dream-syz.github.io/6-55.png)

1.如果当前记录的事务id落在绿色部分（trx_id < min_id），表示这个版本是已提交的事务生成的，可读。
2.如果当前记录的事务id落在红色部分（trx_id > max_id），表示这个版本是由将来启动的事务生成的，不可读。

3. 如果当前记录的事务id落在黄色部分（min_id <= trx_id <= max_id），则分为两种情况：

4. 若当前记录的事务id在未提交事务的数组中，则此条记录不可读；
5. 若当前记录的事务id不在未提交事务的数组中，则此条记录可读。

RC 和 RR 隔离级别都是由 MVCC 实现，区别在于：

1. 读取的一致性：在 RC 隔离级别下，读取的数据是提交事务时的最新版本，即读取已经提交的数据。而在 RR 隔离级别下，读取的数据是事务开始时的快照版本，即读取事务开始时已经存在的数据。在 RR 级别下，事务在读取数据时，即使其他事务对数据进行了修改，也不会看到这些修改，保证了读取的一致性。
2. 读取的锁定：在 RC 隔离级别下，读取的数据不会被锁定，即其他事务可以修改被读取的数据。而在 RR 隔离级别下，读取的数据会被共享锁定，即其他事务不能修改被读取的数据，从而避免了脏读和不可重复读的问题。
3. 并发冲突：由于 RR 隔离级别下的读取会锁定数据，可能会导致更多的并发冲突和锁竞争，从而影响性能。而在 RC 隔离级别下，读取不会锁定数据，可以提高并发性能。



### 61、Repeatable Read 解决了幻读问题吗？

可重复读（repeatable read）定义： 一个事务执行过程中看到的数据，总是跟这个事务在启动时看到的数据是一致的。

不过理论上会出现幻读，简单的说幻读指的的当用户读取某一范围的数据行时，另一个事务又在该范围插入了新行，当用户在读取该范围的数据时会发现有新的幻影行。

**注意在可重复读隔离级别下，普通的查询是快照读，是不会看到别的事务插入的数据的。因此， 幻读在“当前读”下才会出现（查询语句添加for update，表示当前读）；**

在 MVCC 并发控制中，读操作可以分为两类: 快照读（`Snapshot Read`）与当前读 （`Current Read`）。

* 快照读
  快照读是指读取数据时不是读取最新版本的数据，而是基于历史版本读取的一个快照信息（mysql 读取 undo log 历史版本) ，快照读可以使普通的 SELECT 读取数据时不用对表数据进行加锁，从而解决了因为对数据库表的加锁而导致的两个如下问题
  1. 解决了因加锁导致的修改数据时无法对数据读取问题.
  2. 解决了因加锁导致读取数据时无法对数据进行修改的问题.
* 当前读
  当前读是读取的数据库最新的数据，当前读和快照读不同，因为要读取最新的数据而且要保证事务的隔离性，所以当前读是需要对数据进行加锁的（`插入/更新/删除操作，属于当前读，需要加锁`   , `select for update` 为当前读）

表结构

| id   | key  | value |
| ---- | ---- | ----- |
| 0    | 0    | 0     |
| 1    | 1    | 1     |

假设 select * from where value=1 for update，只在这一行加锁（注意这只是假设），其它行不加锁，那么就会出现如下场景：

![](https://dream-syz.github.io/6-56.png)

Session A 的三次查询 Q1-Q3 都是 select * from where value=1 for update，查询的 value=1 的所有 row。

* T1：Q1 只返回一行(1,1,1)；
* T2：session B 更新 id=0 的 value 为 1，此时表 t 中 value=1 的数据有两行
* T3：Q2 返回两行(0,0,1),(1,1,1)
* T4：session C 插入一行(6,6,1)，此时表 t 中 value=1 的数据有三行
* T5：Q3 返回三行(0,0,1),(1,1,1),(6,6,1)
* T6：session A 事物 commit。

其中 Q3 读到 value=1 这一样的现象，就称之为幻读，**幻读指的是一个事务在前后两次查询同一个范围的时候，后一次查询看到了前一次查询没有看到的行**。

先对“幻读”做出如下解释：

* 要讨论「可重复读」隔离级别的幻读现象，是要建立在「当前读」的情况下，而不是快照读,因为在可重复读隔离级别下，普通的查询是快照读，是不会看到别的事务插入的数据的。

**Next-key Lock 锁**

产生幻读的原因是，行锁只能锁住行，但是新插入记录这个动作，要更新的是记录之间的“间隙”。因此，Innodb 引擎为了解决「可重复读」隔离级别使用「当前读」而造成的幻读问题，就引出了 next-key 锁，就是记录锁和间隙锁的组合。

* RecordLock 锁：锁定单个行记录的锁。（记录锁，RC、RR 隔离级别都支持）
* GapLock 锁：间隙锁，锁定索引记录间隙(不包括记录本身)，确保索引记录的间隙不变。（范围锁，RR 隔离级别支持）
* Next-key Lock 锁：记录锁和间隙锁组合，同时锁住数据，并且锁住数据前后范围。（记录锁+范围锁，RR 隔离级别支持）

![](https://dream-syz.github.io/6-57.png)

**总结**

* RR 隔离级别下间隙锁才有效，RC 隔离级别下没有间隙锁；
* RR 隔离级别下为了解决“幻读”问题：“快照读”依靠 MVCC 控制，“当前读”通过间隙锁解决；
* 间隙锁和行锁合称 next-key lock，每个 next-key lock 是前开后闭区间；
* 间隙锁的引入，可能会导致同样语句锁住更大的范围，影响并发度。



### 62、请说一下数据库锁的种类？

**MySQL 数据库由于其自身架构的特点,存在多种数据存储引擎, MySQL 中不同的存储引擎支持不同的锁机制。**

* **MyISAM **和 **MEMORY** 存储引擎采用的表级锁，
* **InnoDB** 存储引擎既支持行级锁，也支持表级锁，默认情况下采用行级锁。
* **BDB** 采用的是页面锁，也支持表级锁

**按照数据操作的类型分**

* 读锁（共享锁）：针对同一份数据，多个读操作可以同时进行而不会互相影响。
* 写锁（排他锁）：当前写操作没有完成前，它会阻断其他写锁和读锁。

**按照数据操作的粒度分**

* 表级锁：开销小，加锁快；不会出现死锁；锁定粒度大，发生锁冲突的概率最高，并发度最低。
* 行级锁： 开销大，加锁慢；会出现死锁；锁定粒度最小，发生锁冲突的概率最低，并发度也最高。
* 页面锁：开销和加锁时间界于表锁和行锁之间；会出现死锁；锁定粒度界于表锁和行锁之间，并发度一般

**按照操作性能可分为乐观锁和悲观锁**

* 乐观锁：一般的实现方式是对记录数据版本进行比对，在数据更新提交的时候才会进行冲突检测，如果发现冲突了，则提示错误信息。
* 悲观锁：在对一条数据修改的时候，为了避免同时被其他人修改，在修改数据之前先锁定，再修改的控制方式。共享锁和排他锁是悲观锁的不同实现，但都属于悲观锁范畴。



### 63、请说一下共享锁和排他锁？

**行级锁分为共享锁和排他锁两种。**

行锁的是 mysql 锁中粒度最小的一种锁，因为锁的粒度很小，所以发生资源争抢的概率也最小，并发性能最大，但是也会造成死锁，每次加锁和释放锁的开销也会变大。

**使用 MySQL 行级锁的两个前提**

* 使用 innoDB 引擎
* 开启事务 (隔离级别为 `Repeatable Read`)

**InnoDB行锁的类型**

* **共享锁**（S）：当事务对数据加上共享锁后, 其他用户可以并发读取数据，但任何事务都不能对数据进行修改（获取数据上的排他锁），直到已释放所有共享锁。
* **排他锁**（X）：如果事务 T 对数据 A 加上排他锁后，则其他事务不能再对数据 A 加任任何类型的封锁。获准排他锁的事务既能读数据，又能修改数据。

**加锁的方式**

* InnoDB 引擎默认更新语句，**update,delete,insert 都会自动给涉及到的数据加上排他锁**，select 语句默认不会加任何锁类型，如果要加可以使用下面的方式:
* 加共享锁（S）：select * from table_name where ... **lock in share mode**;
* 加排他锁（x）：select * from table_name where ... **for update**;

**锁兼容**

* 共享锁只能兼容共享锁, 不兼容排它锁
* 排它锁互斥共享锁和其它排它锁

![](https://dream-syz.github.io/6-58.png)



### 64、InnoDB 的行锁是怎么实现的？

**InnoDB 行锁是通过对索引数据页上的记录加锁实现的**，主要实现算法有 3 种：Record Lock、Gap Lock 和 Next-key Lock。

* **RecordLock 锁：锁定单个行记录的锁。（记录锁，RC、RR 隔离级别都支持）**
* **GapLock 锁：间隙锁，锁定索引记录间隙，确保索引记录的间隙不变。（范围锁，RR 隔离级别支持）**
* **Next-key Lock 锁：记录锁和间隙锁组合，同时锁住数据，并且锁住数据前后范围。（记录锁+范围锁，RR 隔离级别支持）**

> 注意： InnoDB 这种行锁实现特点意味着：只有通过索引条件检索数据，InnoDB 才使用行级锁，否则，InnoDB 将使用表锁

**在 RR 隔离级别，InnoDB 对于记录加锁行为都是先采用 Next-Key Lock，但是当 SQL 操作含有唯一索引时，Innodb 会对 Next-Key Lock 进行优化，降级为 RecordLock，仅锁住索引本身而非范围。**

**各种操作加锁的特点**

1）select ... from 语句：InnoDB 引擎采用 MVCC 机制实现非阻塞读，所以对于普通的 select 语句，InnoDB 不加锁

2）select ... from lock in share mode语句：追加了共享锁，InnoDB 会使用 Next-Key Lock 锁进行处理，如果扫描发现唯一索引，可以降级为 RecordLock 锁。

3）select ... from for update 语句：追加了排他锁，InnoDB 会使用 Next-Key Lock 锁进行处理，如果扫描发现唯一索引，可以降级为RecordLock 锁。

4）update ... where 语句：InnoDB 会使用 Next-Key Lock 锁进行处理，如果扫描发现唯一索引，可以降级为 RecordLock锁。

5）delete ... where 语句：InnoDB 会使用 Next-Key Lock 锁进行处理，如果扫描发现唯一索引，可以降级为 RecordLock锁。

6）insert语句：InnoDB 会在将要插入的那一行设置一个排他的 RecordLock 锁。

**下面以“update t1 set name=‘lisi’ where id=10”操作为例，举例子分析下 InnoDB 对不同索引的加锁行为，以RR隔离级别为例。**

1. 主键加锁

   加锁行为：仅在id=10的主键索引记录上加X锁。

   ![](https://dream-syz.github.io/6-59.png)

2. 唯一键加锁

   加锁行为：现在唯一索引 id 上加 X 锁，然后在 id=10 的主键索引记录上加 X 锁。

   ![](https://dream-syz.github.io/6-60.png)

3. 非唯一键加锁

   加锁行为：对满足id=10条件的记录和主键分别加X锁，然后在(6,c)-(10,b)、(10,b)-(10,d)、(10,d)-(11,f)范围分别加Gap Lock。

   ![](https://dream-syz.github.io/6-61.png)

4. 无索引加锁

   加锁行为：表里所有行和间隙都会加X锁。（当没有索引时，会导致全表锁定，因为 InnoDB 引擎锁机制是基于索引实现的记录锁定）。

   ![](https://dream-syz.github.io/6-62.png)



### 65、并发事务会产生哪些问题

事务并发处理可能会带来一些问题，如下：

* 更新丢失
  当两个或多个事务更新同一行记录，会产生更新丢失现象。可以分为回滚覆盖和提交覆盖。
  * 回滚覆盖：一个事务回滚操作，把其他事务已提交的数据给覆盖了。
  * 提交覆盖：一个事务提交操作，把其他事务已提交的数据给覆盖了。
* 脏读
  一个事务读取到了另一个事务修改但未提交的数据。
* 不可重复读
  一个事务中多次读取同一行记录不一致，后面读取的跟前面读取的不一致。
* 幻读
  一个事务中多次按相同条件查询，结果不一致。后续查询的结果和面前查询结果不同，多了或少了几行记录。

“更新丢失”、”脏读”、“不可重复读”和“幻读”等并发事务问题，其实都是数据库一致性问题，为了解决这些问题，MySQL数据库是通过事务隔离级别来解决的，数据库系统提供了以下 4 种事务隔离级别供用户选择。

| 事务隔离级别 | 回滚覆盖 | 脏读     | 不可重复读 | 提交覆盖 | 幻读     |
| ------------ | -------- | -------- | ---------- | -------- | -------- |
| 读未提交     | x        | 可能发生 | 可能发生   | 可能发生 | 可能发生 |
| 读已提交     | x        | x        | 可能发生   | 可能发生 | 可能发生 |
| 可重复读     | x        | x        | x          | x        | 可能发生 |
| 串行化       | x        | x        | x          | x        | x        |

* **读未提交**
  Read Uncommitted 读未提交：解决了回滚覆盖类型的更新丢失，但可能发生脏读现象，也就是可能读取到其他会话中未提交事务修改的数据。
* **已提交读**
  Read Committed 读已提交：只能读取到其他会话中已经提交的数据，解决了脏读。但可能发生不可重复读现象，也就是可能在一个事务中两次查询结果不一致。
* **可重复度**
  Repeatable Read 可重复读：解决了不可重复读，它确保同一事务的多个实例在并发读取数据时，会看到同样的数据行。不过理论上会出现幻读，简单的说幻读指的的当用户读取某一范围的数据行时，另一个事务又在该范围插入了新行，当用户在读取该范围的数据时会发现有新的幻影行。
* **可串行化**
  所有的增删改查串行执行。它通过强制事务排序，解决相互冲突，从而解决幻度的问题。这个级别可能导致大量的超时现象的和锁竞争，效率低下。

数据库的事务隔离级别越高，并发问题就越小，但是并发处理能力越差（代价）。读未提交隔离级别最低，并发问题多，但是并发处理能力好。以后使用时，可以根据系统特点来选择一个合适的隔离级别，比如对不可重复读和幻读并不敏感，更多关心数据库并发处理能力，此时可以使用 Read Commited 隔离级别。

事务隔离级别，针对 Innodb 引擎，支持事务的功能。像 MyISAM 引擎没有关系。

**事务隔离级别和锁的关系**

1）事务隔离级别是 SQL92 定制的标准，相当于事务并发控制的整体解决方案，本质上是对锁和 MVCC 使用的封装，隐藏了底层细节。

2）锁是数据库实现并发控制的基础，事务隔离性是采用锁来实现，对相应操作加不同的锁，就可以防止其他事务同时对数据进行读写操作。

3）对用户来讲，首先选择使用隔离级别，当选用的隔离级别不能解决并发问题或需求时，才有必要在开发中手动的设置锁。

MySQL 默认隔离级别：可重复读

Oracle、SQLServer 默认隔离级别：读已提交

一般使用时，建议采用默认隔离级别，然后存在的一些并发问题，可以通过悲观锁、乐观锁等实现处理。



### 66、说一下MVCC内部细节

**MVCC概念**

MVCC（Multi Version Concurrency Control）被称为多版本并发控制，是指在数据库中为了实现高并发的数据访问，对数据进行多版本处理，并通过事务的可见性来保证事务能看到自己应该看到的数据版本。

> MVCC 最大的好处是读不加锁，读写不冲突。在读多写少的系统应用中，读写不冲突是非常重要的，极大的提升系统的并发性能，这也是为什么现阶段几乎所有的关系型数据库都支持 MVCC 的原因，不过目前 MVCC 只在 Read Commited 和 Repeatable Read 两种隔离级别下工作。

回答这个面试题时，主要介绍以下的几个关键内容：

**1）行记录的三个隐藏字段**

![](https://dream-syz.github.io/6-63.png)

![i

* `DB_ROW_ID` : 如果没有为表显式的定义主键，并且表中也没有定义唯一索引，那么 InnoDB 会自动为表添加一个 row_id 的隐藏列作为主键。
* `DB_TRX_ID` : 事务中对某条记录做增删改时,就会将这个事务的事务 ID 写入到 trx_id 中.
* `DB_ROLL_PTR`: 回滚指针,指向 undo log 的指针

**2）Undo log 多版本链**

举例：事务 T-100 和 T-120 对表中 id = 1 的数据行做 update 操作，事务 T-130 进行 select 操作，即使 T-100 已经提交修改，三次 select 语句的结果都是“lisi”。

![](https://dream-syz.github.io/6-64.png)

* 每一条数据都有多个版本,版本之间通过 undo log 链条进行连接

![](https://dream-syz.github.io/6-65.png)

**3）ReadView**

Read View是 InnoDB 在实现 MVCC 时用到的一致性读视图，即 consistent read view，用于支持 RC（Read Committed，读提交）和 RR（Repeatable Read，可重复读）隔离级别的实现.

Read View 简单理解就是对数据在每个时刻的状态拍成照片记录下来。那么之后获取某时刻的数据时就还是原来的照片上的数据，是不会变的.

Read View中比较重要的字段有 4 个:

* `m_ids` : 用来表示 MySQL 中哪些事务正在执行,但是没有提交.
* `min_trx_id`: 就是 m_ids 里最小的值.
* `max_trx_id` : 下一个要生成的事务 id 值,也就是最大事务 id
* `creator_trx_id`: 就是你这个事务的 id

**通过 Read View 判断记录的某个版本是否可见的方式总结:**

* trx_id = creator_trx_id
  如果被访问版本的trx_id,与readview中的creator_trx_id值相同,表明当前事务在访问自己修改过的记录,该版本可以被当前事务访问.
* trx_id < min_trx_id
  如果被访问版本的trx_id,小于readview中的min_trx_id值,表明生成该版本的事务在当前事务生成readview前已经提交,该版本可以被当前事务访问.
* trx_id >= max_trx_id
  如果被访问版本的trx_id,大于或等于readview中的max_trx_id值,表明生成该版本的事务在当前事务生成readview后才开启,该版本不可以被当前事务访问.
* trx_id > min_trx_id && trx_id < max_trx_id
  如果被访问版本的trx_id,值在readview的min_trx_id和max_trx_id之间，就需要判断trx_id属性值是不是在m_ids列表中？
  * 在：说明创建readview时生成该版本的事务还是活跃的,该版本不可以被访问
  * 不在：说明创建readview时生成该版本的事务已经被提交,该版本可以被访问

**何时生成 ReadView 快照**

* 在 **读已提交（Read Committed， 简称RC）** 隔离级别下，**每一次 **读取数据前都生成一个ReadVIew。
* 在 **可重复读** （Repeatable Read，简称RR）隔离级别下，在一个事务中，只在 **第一次**读取数据前生成一个 ReadVIew。

**4）快照读（`Snapshot Read`）与当前读 （`Current Read`）**

在 MVCC 并发控制中，读操作可以分为两类: 快照读（`Snapshot Read`）与当前读 （`Current Read`）。

* 快照读
  快照读是指读取数据时不是读取最新版本的数据，而是基于历史版本读取的一个快照信息（mysql读取undo log历史版本) ，快照读可以使普通的SELECT 读取数据时不用对表数据进行加锁，从而解决了因为对数据库表的加锁而导致的两个如下问题
  1. 解决了因加锁导致的修改数据时无法对数据读取问题.
  2. 解决了因加锁导致读取数据时无法对数据进行修改的问题.
* 当前读
  当前读是读取的数据库最新的数据，当前读和快照读不同，因为要读取最新的数据而且要保证事务的隔离性，所以当前读是需要对数据进行加锁的（`Update delete insert select ....lock in share mode`   , `select for update` 为当前读）

**总结一下**

* 并发环境下，写-写操作有加锁解决方案，但为了提高性能，InnoDB 存储引擎提供 MVCC，目的是为了解决读-写，写-读操作下不加锁仍能安全进行。
* MVCC 的过程，本质就是访问版本链，并判断哪个版本可见的过程。该判断算法是通过版本上的 trx_id 与快照 ReadView 的若干个信息进行对比。
* 快照生成的时机因隔离级别不同，读已提交隔离级别下，每一次读取前都会生成一个快照 ReadView；而可重复读则仅在一个事务中，第一次读取前生成一个快照。



### 67、说一下 MySQL 死锁的原因和处理方法

**1) 表的死锁**

**产生原因:**

用户A访问表A（锁住了表A），然后又访问表B；另一个用户B访问表B（锁住了表B），然后企图访问表A；这时用户A由于用户B已经锁住表B，它必须等待用户B释放表B才能继续，同样用户B要等用户A释放表A才能继续，这就死锁就产生了。

用户A--》A表（表锁）--》B表（表锁）

用户B--》B表（表锁）--》A表（表锁）

**解决方案：**

这种死锁比较常见，是由于程序的BUG产生的，除了调整的程序的逻辑没有其它的办法。

仔细分析程序的逻辑，对于数据库的多表操作时，尽量按照相同的顺序进行处理，尽量避免同时锁定两个资源，如操作A和B两张表时，总是按先A后B的顺序处理， 必须同时锁定两个资源时，要保证在任何时刻都应该按照相同的顺序来锁定资源。

**2) 行级锁死锁**

**产生原因1：**

如果在事务中执行了一条没有索引条件的查询，引发全表扫描，把行级锁上升为全表记录锁定（等价于表级锁），多个这样的事务执行后，就很容易产生死锁和阻塞，最终应用系统会越来越慢，发生阻塞或死锁。

**解决方案1：**

SQL语句中不要使用太复杂的关联多表的查询；使用explain“执行计划"对SQL语句进行分析，对于有全表扫描和全表锁定的SQL语句，建立相应的索引进行优化。

**产生原因2：**

- 两个事务分别想拿到对方持有的锁，互相等待，于是产生死锁

  ![](https://dream-syz.github.io/6-66.png)

**产生原因3**：每个事务只有一个SQL,但是有些情况还是会发生死锁.

1. 事务1,从name索引出发 , 读到的[hdc, 1],  [hdc, 6]均满足条件, 不仅会加name索引上的记录X锁, 而且会加聚簇索引上的记录X锁, 加锁顺序为先[1,hdc,100], 后[6,hdc,10]
2. 事务2，从pubtime索引出发，[10,6],[100,1]均满足过滤条件，同样也会加聚簇索引上的记录X锁，加锁顺序为[6,hdc,10]，后[1,hdc,100]。
3. 但是加锁时发现跟事务1的加锁顺序正好相反，两个Session恰好都持有了第一把锁，请求加第二把锁，死锁就发生了。

![](https://dream-syz.github.io/6-67.png)

**解决方案:** 如上面的原因2和原因3,  对索引加锁顺序的不一致很可能会导致死锁，所以如果可以，尽量以相同的顺序来访问索引记录和表。在程序以批量方式处理数据的时候，如果事先对数据排序，保证每个线程按固定的顺序来处理记录，也可以大大降低出现死锁的可能；



### 68、介绍一下 MySQL 的体系架构？

![](https://dream-syz.github.io/6-68.png)

**MySQL Server架构自顶向下大致可以分网络连接层、服务层、存储引擎层和系统文件层。**

**一、网络连接层**

* **客户端连接器（Client Connectors）：提供与MySQL服务器建立的支持。目前几乎支持所有主流的服务端编程技术，例如常见的 Java、C、Python、.NET等，它们通过各自API技术与MySQL建立连接。**

**二、服务层（MySQL Server）**

**服务层是MySQL Server的核心，主要包含系统管理和控制工具、连接池、SQL接口、解析器、查询优化器和缓存六个部分。**

* **连接池（Connection Pool）**：负责存储和管理客户端与数据库的连接，一个线程负责管理一个连接。

* **系统管理和控制工具（Management Services & Utilities）**：例如备份恢复、安全管理、集群管理等

* **SQL接口（SQL Interface）**：用于接受客户端发送的各种SQL命令，并且返回用户需要查询的结果。比如DML、DDL、存储过程、视图、触发器等。

* **解析器（Parser）**：负责将请求的SQL解析生成一个"解析树"。然后根据一些MySQL规则进一步检查解析树是否合法。

* **查询优化器（Optimizer）**：当“解析树”通过解析器语法检查后，将交由优化器将其转化成执行计划，然后与存储引擎交互。

  > **select uid,name from user where gender=1;**
  >
  > **选取--》投影--》联接 策略**
  >
  > **1）select先根据where语句进行选取，并不是查询出全部数据再过滤**
  >
  > **2）select查询根据uid和name进行属性投影，并不是取出所有字段**
  >
  > **3）将前面选取和投影联接起来最终生成查询结果**

* **缓存（Cache&Buffer）**： 缓存机制是由一系列小缓存组成的。比如表缓存，记录缓存，权限缓存，引擎缓存等。如果查询缓存有命中的查询结果，查询语句就可以直接去查询缓存中取数据。

**三、存储引擎层（Pluggable Storage Engines）**

**存储引擎负责MySQL中数据的存储与提取，与底层系统文件进行交互。MySQL存储引擎是插件式的，服务器中的查询执行引擎通过接口与存储引擎进行通信，接口屏蔽了不同存储引擎之间的差异 。现在有很多种存储引擎，各有各的特点，最常见的是MyISAM和InnoDB。**

**四、系统文件层（File System）**

**该层负责将数据库的数据和日志存储在文件系统之上，并完成与存储引擎的交互，是文件的物理存储层。主要包含日志文件，数据文件，配置文件，pid 文件，socket 文件等。**

* **日志文件**
  * **错误日志（Error log）**
    **默认开启，show variables like '%log_error%'**
  * **通用查询日志（General query log）**
    **记录一般查询语句，show variables like '%general%';**
  * **二进制日志（binary log）**
    **记录了对MySQL数据库执行的更改操作，并且记录了语句的发生时间、执行时长；但是它不记录select、show等不修改数据库的SQL。主要用于数据库恢复和主从复制。**
    **show variables like '%log_bin%'; //是否开启**
    **show variables like '%binlog%'; //参数查看**
    **show binary logs;//查看日志文件**
  * **慢查询日志（Slow query log）**
    **记录所有执行时间超时的查询SQL，默认是10秒。**
    **show variables like '%slow_query%'; //是否开启**
    **show variables like '%long_query_time%'; //时长**
* **配置文件**
  **用于存放MySQL所有的配置信息文件，比如my.cnf、my.ini等。**
* **数据文件**
  * **db.opt 文件：记录这个库的默认使用的字符集和校验规则。**
  * **frm 文件：存储与表相关的元数据（meta）信息，包括表结构的定义信息等，每一张表都会有一个frm 文件。**
  * **MYD 文件：MyISAM 存储引擎专用，存放 MyISAM 表的数据（data)，每一张表都会有一个 .MYD 文件。**
  * **MYI 文件：MyISAM 存储引擎专用，存放 MyISAM 表的索引相关信息，每一张 MyISAM 表对应一个 .MYI 文件。**
  * **ibd文件和 IBDATA 文件：存放 InnoDB 的数据文件（包括索引）。InnoDB 存储引擎有两种表空间方式：独享表空间和共享表空间。独享表空间使用 .ibd 文件来存放数据，且每一张 InnoDB 表对应一个 .ibd 文件。共享表空间使用 .ibdata 文件，所有表共同使用一个（或多个，自行配置）.ibdata 文件。**
  * **ibdata1 文件：系统表空间数据文件，存储表元数据、Undo日志等 。**
  * **ib_logfile0、ib_logfile1 文件：Redo log 日志文件。**
* **pid 文件**
  **pid 文件是 mysqld 应用程序在 Unix/Linux 环境下的一个进程文件，和许多其他 Unix/Linux 服务端程序一样，它存放着自己的进程 id。**
* **socket 文件**
  **socket 文件也是在 Unix/Linux 环境下才有的，用户在 Unix/Linux 环境下客户端连接可以不通过 TCP/IP 网络而直接使用 Unix Socket 来连接 MySQL。**



### 69、undo log、redo log、 bin log 的作用是什么？

**undo log 基本概念**

* undo log是一种用于撤销回退的日志，在数据库事务开始之前，MySQL 会先记录更新前的数据到 undo log 日志文件里面，当事务回滚时或者数据库崩溃时，可以利用 undo log 来进行回退。
* Undo Log产生和销毁：Undo Log 在事务开始前产生；事务在提交时，并不会立刻删除 undo log，innodb 会将该事务对应的 undo log放入到删除列表中，后面会通过后台线程 purge thread 进行回收处理。

**注意: undo log 也会产生 redo log，因为 undo log 也要实现持久性保护。**

**undo log 的作用**

1. **提供回滚操作【undo log 实现事务的原子性】**
   在数据修改的时候，不仅记录了 redo log，还记录了相对应的 undo log，如果因为某些原因导致事务执行失败了，可以借助 undo log进行回滚。
   *undo log 和 redo log 记录物理日志不一样，它是*逻辑日志。可以认为当 delete 一条记录时，undo log 中会记录一条对应的 insert 记录，反之亦然，当 update 一条记录时，它记录一条对应相反的 update 记录。
2. **提供多版本控制(MVCC)【*undo log实现多版本并发控制（MVCC）*】**
   MVCC，即多版本控制。在 MySQL 数据库 InnoDB 存储引擎中，用 undo Log 来实现多版本并发控制（MVCC）。当读取的某一行被其他事务锁定时，它可以从 undo log 中分析出该行记录以前的数据版本是怎样的，从而让用户能够读取到当前事务操作之前的数据【快照读】。

**redo log 基本概念**

* InnoDB 引擎对数据的更新，是先将更新记录写入 redo log 日志，然后会在系统空闲的时候或者是按照设定的更新策略再将日志中的内容更新到磁盘之中。这就是所谓的预写式技术（Write Ahead logging）。这种技术可以大大减少 IO 操作的频率，提升数据刷新的效率。
* redo log：被称作重做日志, 包括两部分：一个是内存中的日志缓冲： `redo log buffer`，另一个是磁盘上的日志文件： `redo log file` 。

**redo log的作用**

* mysql 每执行一条 DML 语句，先将记录写入 redo log buffer 。后续某个时间点再一次性将多个操作记录写到 redo log file 。当故障发生致使内存数据丢失后，InnoDB 会在重启时，经过重放 redo，将 Page 恢复到崩溃之前的状态 **通过 Redo log 可以实现事务的持久性 。**

**bin log 基本概念**

* binlog 是一个二进制格式的文件，用于记录用户对数据库更新的 SQL 语句信息，例如更改数据库表和更改内容的SQL语句都会记录到binlog里，但是不会记录SELECT和SHOW这类操作。
* binlog在MySQL的Server层实现(引擎共用)
* binlog为逻辑日志,记录的是一条SQL语句的原始逻辑
  * binlog不限制大小,追加写入,不会覆盖以前的日志.
  * 默认情况下，binlog日志是二进制格式的，不能使用查看文本工具的命令（比如，cat，vi等）查看，而使用mysqlbinlog解析查看。

**bin log 的作用**

1. 主从复制：在主库中开启 Binlog 功能，这样主库就可以把 Binlog 传递给从库，从库拿到Binlog后实现数据恢复达到主从数据一致性。
2. 数据恢复：通过mysqlbinlog工具来恢复数据。



### 70、redo log 与 undo log 的持久化策略？

**redo log持久化**

缓冲区数据一般情况下是无法直接写入磁盘的，中间必须经过操作系统缓冲区( OS Buffer )。因此， redo log buffer 写入 redo logfile 实际上是先写入 OS Buffer，然后再通过系统调用 fsync() 将其刷到 redo log file.

Redo Buffer 持久化到 redo log 的策略，可通过 `Innodb_flush_log_at_trx_commit` 设置：

| **参数值**         | 含义                                                         |
| ------------------ | ------------------------------------------------------------ |
| 0 (延迟写)         | 事务提交时不会将 `redo log buffer`中日志写入到 `os buffer`， 而是每秒写入 `os buffer`并调用 `fsync()`写入到 `redo log file`中。 也就是说设置为0时是(大约)每秒刷新写入到磁盘中的，当系统崩溃，会丢失1秒钟的数据。 |
| 1  (实时写,实时刷) | 事务每次提交都会将 `redo log buffer`中的日志写入 `os buffer`并 调用 `fsync()`刷到 `redo log file`中。这种方式即使系统崩溃也不会丢失任何数据，但是因为每次提交都写入磁盘，IO的性能较差。 |
| 2 (实时写, 延时刷) | 每次提交都仅写入到 `os buffer`，然后是每秒调用 `fsync()`将 `os buffer`中的日志写入到 `redo log file`。 |



![](https://dream-syz.github.io/6-69.png)

一般建议选择取值2，因为 MySQL 挂了数据没有损失，整个服务器挂了才会损失1秒的事务提交数据

**undo log持久化**

MySQL中的Undo Log严格的讲不是Log，而是数据，因此他的管理和落盘都跟数据是一样的：

* Undo的磁盘结构并不是顺序的，而是像数据一样按Page管理
* Undo写入时，也像数据一样产生对应的Redo Log (因为undo也是对页面的修改，记录undo这个操作本身也会有对应的redo)。
* Undo的Page也像数据一样缓存在Buffer Pool中，跟数据Page一起做LRU换入换出，以及刷脏。Undo Page的刷脏也像数据一样要等到对应的Redo Log 落盘之后

当事务提交的时候，innodb不会立即删除undo log，因为后续还可能会用到undo log，如隔离级别为repeatable read时，事务读取的都是开启事务时的最新提交行版本，只要该事务不结束，该行版本就不能删除，即undo log不能删除。

但是在事务提交的时候，会将该事务对应的undo log放入到删除列表中，未来通过purge来删除。并且提交事务时，还会判断undo log分配的页是否可以重用，如果可以重用，则会分配给后面来的事务，避免为每个独立的事务分配独立的undo log页而浪费存储空间和性能。



### 71、bin log 与 undo log 的区别？

**1）redo log 是 InnoDB 引擎特有的；binlog是MySQL的Server层实现的，所有引擎都可以使用。**

**2）redo log是物理日志，记录的是“在XXX数据页上做了XXX修改”；binlog 是逻辑日志，记录的是原始逻辑，其记录是对应的SQL语句。**

* 物理日志: 记录的是每一个page页中具体存储的值是多少，在这个数据页上做了什么修改.  比如: 某个事物将系统表空间中的第100个页面中偏移量为1000处的那个字节的值1改为2.
* 逻辑日志: 记录的是每一个page页面中具体数据是怎么变动的，它会记录一个变动的过程或SQL语句的逻辑, 比如: 把一个page页中的一个数据从1改为2，再从2改为3,逻辑日志就会记录1->2,2->3这个数据变化的过程.

**3）redo log是循环写的，空间一定会用完，需要write pos和check point搭配；binlog是追加写，写到一定大小会切换到下一个，并不会覆盖以前的日志**

* Redo Log 文件内容是以顺序循环的方式写入文件，写满时则回溯到第一个文件，进行覆盖写。

![](https://dream-syz.github.io/6-70.png)

* **write pos**: 表示日志当前记录的位置，当ib_logfile_4写满后，会从ib_logfile_1从头开始记录；
* **check point**: 表示将日志记录的修改写进磁盘，完成数据落盘，数据落盘后checkpoint会将日志上的相关记录擦除掉，即 `write pos -> checkpoint`  之间的部分是redo log空着的部分，用于记录新的记录，`checkpoint -> write pos` 之间是redo log 待落盘的数据修改记录
* 如果 write pos 追上 checkpoint，表示写满，这时候不能再执行新的更新，得停下来先擦掉一些记录，把 checkpoint 推进一下。

**3）Redo Log 作为服务器异常宕机后事务数据自动恢复使用，Binlog 可以作为主从复制和数据恢复使用。Binlog没有自动crash-safe能力**

CrashSafe指MySQL服务器宕机重启后，能够保证：

* 所有已经提交的事务的数据仍然存在。
* 所有没有提交的事务的数据自动回滚。



### 72、MySQL 的 binlog 有几种日志格式？分别有什么区别？

binlog日志有三种模式

1）ROW（row-based replication, RBR）：日志中会记录每一行数据被修改的情况，然后在slave端对相同的数据进行修改。

* 优点：能清楚记录每一个行数据的修改细节，能完全实现主从数据同步和数据的恢复。而且不会出现某些特定情况下存储过程或function无法被正确复制的问题。
* 缺点：批量操作，会产生大量的日志，尤其是alter table会让日志量暴涨。

2）STATMENT（statement-based replication, SBR）：记录每一条修改数据的SQL语句（批量修改时，记录的不是单条SQL语句，而是批量修改的SQL语句事件）, slave在复制的时候SQL进程会解析成和原来master端执行过的相同的SQL再次执行。简称SQL语句复制。

* 优点：日志量小，减少磁盘IO，提升存储和恢复速度

* 缺点：在某些情况下会导致主从数据不一致，比如last_insert_id()、now()等函数。

  ![](https://dream-syz.github.io/6-71.png)

3）MIXED（mixed-based replication, MBR）：以上两种模式的混合使用，一般会使用 STATEMENT 模式保存 binlog，对于STATEMENT模式无法复制的操作使用ROW模式保存binlog，MySQL会根据执行的SQL语句选择写入模式。

企业场景如何选择binlog的模式

1. 如果生产中使用MySQL的特殊功能相对少（存储过程、触发器、函数）。选择默认的语句模式，Statement。
2. 如果生产中使用MySQL的特殊功能较多的，可以选择Mixed模式。
3. 如果生产中使用MySQL的特殊功能较多，又希望数据最大化一致，此时最好Row 模式；但是要注意，该模式的binlog日志量增长非常快.



### 73、MySQL 线上修改大表结构有哪些风险？

在线修改大表的可能影响

* 在线修改大表的表结构执行时间往往不可预估，一般时间较长。
* 由于修改表结构是表级锁，因此在修改表结构时，影响表写入操作。
* 如果长时间的修改表结构，中途修改失败，由于修改表结构是一个事务，因此失败后会还原表结构，在这个过程中表都是锁着不可写入。
* 修改大表结构容易导致数据库 CPU、IO 等性能消耗，使 MySQL 服务器性能降低。
* 在线修改大表结构容易导致主从延时，从而影响业务读取。

修改方式：

1. 对表加锁(表此时只读)
2. 复制原表物理结构
3. 修改表的物理结构
4. 把原表数据导入中间表中 ，数据同步完后，**锁定中间表，并删除原表
5. rename中间表为原表
6. 刷新数据字典，并释放锁

使用工具： **online-schema-change** ，是 percona 推出的一个针对 mysql 在线 ddl 的工具。percona 是一个mysql分支维护公司，专门提供mysql技术服务的。



### 74、count(列名)、count(1) 和 count(*) 有什么区别?

**进行统计操作时,count 中的统计条件可以三种选择:**

```
EXPLAIN  SELECT COUNT(*) FROM user;

EXPLAIN  SELECT COUNT(列名) FROM user;

EXPLAIN  SELECT COUNT(1) FROM user;
```

执行效果上：

* count(*) 包括了所有的列,在统计时 不会忽略列值为null的数据。
* count(1) 用1表示代码行,在统计时,不会忽略列值为null的数据。
* count(列名)在统计时,会忽略列值为空的数据,就是说某个字段的值为null时不统计。

执行效率上：

* InnoDB引擎：count（字段) < count(1) = count(*)
  * InnoDB通过遍历最小的可用二级索引来处理select count(*) 语句，除非索引或优化器提示指示优化器使用不同的索引。如果二级索引不存在，则通过扫描聚集索引来处理。
  * InnoDB已同样的方式处理count(1)和count(*)
* MyISAM引擎：count（字段) < count(1) <= count(*)
  * MyISAM存储了数据的准确行数，使用 `count(*)`会直接读取该行数， 只有当第一列定义为NOT NULL时，count（1），才会执行该操作，所以优先选择 `count(*)`
* count(列名) 会遍历整个表，但不同的是，它会先获取列，然后判断是否为空，然后累加，因此count(列名)性能不如前两者。

注意：count(*)，这是 SQL92 定义的标准统计行数的语法，跟数据库无关，与NULL也无关。而count(列名) 是统计列值数量，不计NULL，相同列值算一个。



### 75、什么是分库分表？什么时候进行分库分表？

**什么是分库分表**

简单来说，就是指通过某种特定的条件，将我们存放在同一个数据库中的数据分散存放到多个数据库（主机）上面，以达到分散单台设备负载的效果。

![](https://dream-syz.github.io/6-72.png)

- 分库分表解决的问题

  **分库分表的目的是为了解决由于数据量过大而导致数据库性能降低的问题，将原来单体服务的数据库进行拆分.将数据大表拆分成若干数据表组成，使得单一数据库、单一数据表的数据量变小，从而达到提升数据库性能的目的。**

- 什么情况下需要分库分表

  - **单机存储容量遇到瓶颈.**
  - **连接数,处理能力达到上限.**

> 注意:
>
> 分库分表之前,要根据项目的实际情况 确定我们的数据量是不是够大,并发量是不是够大,来决定是否分库分表.
>
> 数据量不够就不要分表,单表数据量超过1000万或100G的时候, 速度就会变慢(官方测试),

分库分表包括： 垂直分库、垂直分表、水平分库、水平分表 四种方式。

**垂直分库**

- 数据库中不同的表对应着不同的业务，垂直切分是指按照业务的不同将表进行分类,分布到不同的数据库上面

  - 将数据库部署在不同服务器上，从而达到多个服务器共同分摊压力的效果

  ![](https://dream-syz.github.io/6-73.png)

**垂直分表**

表中字段太多且包含大字段的时候，在查询时对数据库的IO、内存会受到影响，同时更新数据时，产生的binlog文件会很大，MySQL在主从同步时也会有延迟的风险

- 将一个表按照字段分成多表，每个表存储其中一部分字段。
- 对职位表进行垂直拆分, 将职位基本信息放在一张表, 将职位描述信息存放在另一张表

![](https://dream-syz.github.io/6-74.png)

- 垂直拆分带来的一些提升
  - 解决业务层面的耦合，业务清晰
  - 能对不同业务的数据进行分级管理、维护、监控、扩展等
  - 高并发场景下，垂直分库一定程度的提高访问性能
- 垂直拆分没有彻底解决单表数据量过大的问题

**水平分库**

- 将单张表的数据切分到多个服务器上去，每个服务器具有相应的库与表，只是表中数据集合不同。 水平分库分表能够有效的缓解单机和单库的性能瓶颈和压力，突破IO、连接数、硬件资源等的瓶颈.

- 简单讲就是根据表中的数据的逻辑关系，将同一个表中的数据按照某种条件拆分到多台数据库（主机）上面, 例如将订单表 按照id是奇数还是偶数, 分别存储在不同的库中。

  ![](https://dream-syz.github.io/6-75.png)

**水平分表**

- 针对数据量巨大的单张表（比如订单表），按照规则把一张表的数据切分到多张表里面去。 但是这些表还是在同一个库中，所以库级别的数据库操作还是有IO瓶颈。

  ![](https://dream-syz.github.io/6-76.png)

- 总结

  - **垂直分表**: 将一个表按照字段分成多表，每个表存储其中一部分字段。
  - **垂直分库**: 根据表的业务不同,分别存放在不同的库中,这些库分别部署在不同的服务器.
  - **水平分库**: 把一张表的数据按照一定规则,分配到**不同的数据库**,每一个库只有这张表的部分数据.
  - **水平分表**: 把一张表的数据按照一定规则,分配到**同一个数据库的多张表中**,每个表只有这个表的部分数据.



### 76、说说 MySQL 的主从复制？

**主从复制的用途**

- 实时灾备，用于故障切换
- 读写分离，提供查询服务
- 备份，避免影响业务

**主从部署必要条件**

- 主库开启 binlog 日志（设置log-bin参数）
- 主从server-id不同
- 从库服务器能连通主库

**主从复制的原理**

- Mysql 中有一种日志叫做 bin 日志（二进制日志）。这个日志会记录下所有修改了数据库的SQL 语句（insert,update,delete,create/alter/drop table, grant 等等）。
- 主从复制的原理其实就是把主服务器上的 bin 日志复制到从服务器上执行一遍，这样从服务器上的数据就和主服务器上的数据相同了。

![](https://dream-syz.github.io/6-77.png)

1. 主库db的更新事件(update、insert、delete)被写到binlog
2. 主库创建一个binlog dump thread，把binlog的内容发送到从库
3. 从库启动并发起连接，连接到主库
4. 从库启动之后，创建一个I/O线程，读取主库传过来的binlog内容并写入到relay log
5. 从库启动之后，创建一个SQL线程，从relay log里面读取内容，执行读取到的更新事件，将更新内容写入到slave的db



### 77、说一下 MySQL 执行一条查询语句的内部执行过程？

![](https://dream-syz.github.io/6-78.png)

* ①建立连接（Connectors&Connection Pool），通过客户端/服务器通信协议与MySQL建立连接。MySQL 客户端与服务端的通信方式是 “ 半双工 ”。对于每一个 MySQL 的连接，时刻都有一个线程状态来标识这个连接正在做什么。
  通讯机制：

  * 全双工：能同时发送和接收数据，例如平时打电话。
  * 半双工：指的某一时刻，要么发送数据，要么接收数据，不能同时。例如早期对讲机
  * 单工：只能发送数据或只能接收数据。例如单行道

  线程状态：
  show processlist; //查看用户正在运行的线程信息，root用户能查看所有线程，其他用户只能看自己的

  * id：线程ID，可以使用kill xx；
  * user：启动这个线程的用户
  * Host：发送请求的客户端的IP和端口号
  * db：当前命令在哪个库执行
  * Command：该线程正在执行的操作命令
    * Create DB：正在创建库操作
    * Drop DB：正在删除库操作
    * Execute：正在执行一个PreparedStatement
    * Close Stmt：正在关闭一个PreparedStatement
    * Query：正在执行一个语句
    * Sleep：正在等待客户端发送语句
    * Quit：正在退出
    * Shutdown：正在关闭服务器
  * Time：表示该线程处于当前状态的时间，单位是秒
  * State：线程状态
    * Updating：正在搜索匹配记录，进行修改
    * Sleeping：正在等待客户端发送新请求
    * Starting：正在执行请求处理
    * Checking table：正在检查数据表
    * Closing table : 正在将表中数据刷新到磁盘中
    * Locked：被其他查询锁住了记录
    * Sending Data：正在处理Select查询，同时将结果发送给客户端
  * Info：一般记录线程执行的语句，默认显示前100个字符。想查看完整的使用show full processlist;

* ②查询缓存（Cache&Buffer），这是MySQL的一个可优化查询的地方，如果开启了查询缓存且在查询缓存过程中查询到完全相同的SQL语句，则将查询结果直接返回给客户端；如果没有开启查询缓存或者没有查询到完全相同的 SQL 语句则会由解析器进行语法语义解析，并生成“解析树”。

  * 缓存Select查询的结果和SQL语句
  * 执行Select查询时，先查询缓存，判断是否存在可用的记录集，要求是否完全相同（包括参数值），这样才会匹配缓存数据命中。
  * 即使开启查询缓存，以下SQL也不能缓存
    * 查询语句使用SQL_NO_CACHE
    * 查询的结果大于query_cache_limit设置
    * 查询中有一些不确定的参数，比如now()
  * show variables like '%query_cache%'; //查看查询缓存是否启用，空间大小，限制等
  * show status like 'Qcache%'; //查看更详细的缓存参数，可用缓存空间，缓存块，缓存多少等

* ③解析器（Parser）将客户端发送的 SQL 进行语法解析，生成"解析树"。预处理器根据一些 MySQL 规则进一步检查“解析树”是否合法，例如这里将检查数据表和数据列是否存在，还会解析名字和别名，看看它们是否有歧义，最后生成新的“解析树”。

* ④查询优化器（Optimizer）根据“解析树”生成最优的执行计划。MySQL 使用很多优化策略生成最优的执行计划，可以分为两类：静态优化（编译时优化）、动态优化（运行时优化）。

  * 等价变换策略
    * 5=5 and a>5 改成 a > 5
    * a < b and a=5 改成b>5 and a=5
    * 基于联合索引，调整条件位置等
  * 优化count、min、max等函数
    * InnoDB引擎min函数只需要找索引最左边
    * InnoDB引擎max函数只需要找索引最右边
    * MyISAM引擎count(*)，不需要计算，直接返回
  * 提前终止查询
    * 使用了limit查询，获取 limit 所需的数据，就不在继续遍历后面数据
  * in的优化
    * MySQL 对 in 查询，会先进行排序，再采用二分法查找数据。比如 where id in (2,1,3)，变成 in (1,2,3)

* ⑤查询执行引擎负责执行 SQL 语句，此时查询执行引擎会根据 SQL 语句中表的存储引擎类型，以及对应的API接口与底层存储引擎缓存或者物理文件的交互，得到查询结果并返回给客户端。若开启用查询缓存，这时会将 SQL 语句和结果完整地保存到查询缓存（Cache&Buffer）中，以后若有相同的 SQL 语句执行则直接返回结果。

  * 如果开启了查询缓存，先将查询结果做缓存操作
  * 返回结果过多，采用增量模式返回
  
  

### 78、Mysql 内部支持缓存查询吗？

使用缓存的好处：当 MySQL 接收到客户端的查询 SQL 之后，仅仅只需要对其进行相应的权限验证之后，就会通过 Query Cache 来查找结果，甚至都不需要经过 Optimizer 模块进行执行计划的分析优化，更不需要发生任何存储引擎的交互.

mysql 5.7 支持内部缓存，8.0 之后已废弃

mysql 缓存的限制

1. mysql 基本没有手段灵活的管理缓存失效和生效，尤其对于频繁更新的表
2. SQL 必须完全一致才会导致 cache 命中
3. 为了节省内存空间，太大的 result set 不会被 cache (< query_cache_limit)；
4. MySQL 缓存在分库分表环境下是不起作用的；
5. 执行 SQL 里有触发器,自定义函数时，MySQL 缓存也是不起作用的；
6. 在表的结构或数据发生改变时，基于该表相关 cache 立即全部失效。

替代方案

* 应用层组织缓存，最简单的是使用 redis，ehcached 等



### 79、ORM 可以防止 SQL 注入攻击吗?

ORM（对象关系映射）框架本身并不能完全防止 SQL 注入攻击，但它可以在一定程度上减少 SQL 注入的风险。下面是一些ORM框架如何帮助减少SQL注入攻击的方式：

1. 参数化查询：ORM 框架通常会使用参数化查询来执行数据库操作，而不是直接将用户输入的数据拼接到SQL语句中。参数化查询可以将用户输入的数据作为参数传递给SQL语句，而不是将其直接嵌入到 SQL 语句中，从而避免了 SQL 注入攻击。

2. 自动转义：一些 ORM 框架会自动对用户输入的数据进行转义，以确保输入数据中的特殊字符不会被误认为是SQL语句的一部分。这样可以防止恶意的SQL注入攻击。

3. 输入验证和过滤：ORM 框架通常提供了输入验证和过滤的功能，可以对用户输入的数据进行检查和过滤，以确保输入的数据符合预期的格式和类型。这样可以防止一些常见的SQL注入攻击。

尽管 ORM 框架可以提供一些保护机制来减少SQL注入攻击的风险，但仍然需要开发人员自己保证应用程序的安全性。开发人员应该遵循安全的编码实践，对用户输入的数据进行充分验证和过滤，以防止 SQL 注入攻击。





## SpringCloud 篇

### 1、什么是 SpringCloud

Spring cloud 流应用程序启动器是基于 Spring Boot 的 Spring 集成应用程序，提供与外部系统的集成。Spring cloud Task，一个生命周期短暂的微服务框架，用于快速构建执行有限数据处理的应用程序。



### 2、什么是微服务

微服务架构是一种架构模式或者说是一种架构风格，它提倡将单一应用程序划分为一组小的服务，每个服务运行在其独立的自己的进程中，服务之间相互协调、互相配合，为用户提供最终价值。服务之间采用轻量级的通信机制互相沟通（通常是基于 HTTP 的 RESTful API）,每个服务都围绕着具体的业务进行构建，并且能够被独立的构建在生产环境、类生产环境等。另外，应避免统一的、集中式的服务管理机制，对具体的一个服务而言，应根据业务上下文，选择合适的语言、工具对其进行构建，可以有一个非常轻量级的集中式管理来协调这些服务，可以使用不同的语言来编写服务，也可以使用不同的数据存储。



### 3、SpringCloud 有什么优势

使用 Spring Boot 开发分布式微服务时，我们面临以下问题

（1）与分布式系统相关的复杂性-这种开销包括网络问题，延迟开销，带宽问题，安全问题。

（2）服务发现-服务发现工具管理群集中的流程和服务如何查找和互相交谈。它涉及一个服务目录，在该目录中注册服务，然后能够查找并连接到该目录中的服务。

（3）冗余-分布式系统中的冗余问题。

（4）负载平衡 --负载平衡改善跨多个计算资源的工作负荷，诸如计算机，计算机集群，网络链路，中央处理单元，或磁盘驱动器的分布。

（5）性能-问题 由于各种运营开销导致的性能问题。

（6）部署复杂性-Devops 技能的要求。



### 4、 什么是服务熔断？什么是服务降级？

**熔断机制是应对雪崩效应的一种微服务链路保护机制**。当某个微服务不可用或者响应时间太长时，会进行服务降级，进而熔断该节点微服务的调用，快速返回“错误”的响应信息。当检测到该节点微服务调用响应正常后恢复调用链路。在 SpringCloud 框架里熔断机制通过Hystrix 实现，Hystrix 会监控微服务间调用的状况，当失败的调用到一定阈值，缺省是 5 秒内调用 20 次，如果失败，就会启动熔断机制。服务降级，一般是从整体负荷考虑。就是当某个服务熔断之后，服务器将不再被调用，此时客户端可以自己准备一个本地的 fallback回调，返回一个缺省值。这样做，虽然水平下降，但好歹可用，比直接挂掉强。

**Hystrix 相关注解** @EnableHystrix：开启熔断 @HystrixCommand(fallbackMethod=”XXX”)：声明一个失败回滚处理函数 XXX，当被注解的方法执行超时（默认是 1000 毫秒），就会执行 fallback 函数，返回错误提示。



### 5、Eureka 和 zookeeper 都可以提供服务注册与发现的功能，请说说两个的区别？

Zookeeper 保证了CP（C：一致性，P：分区容错性），Eureka 保证了AP（A：高可用，P：分区容错性） 

1.当向注册中心查询服务列表时，我们可以容忍注册中心返回的是几分钟以前的信息，但不能容忍直接 down 掉不可用。也就是说，服务注册功能对高可用性要求比较高，但 zk 会出现这样一种情况，当 master 节点因为网络故障与其他节点失去联系时，剩余节点会重新选 leader。问题在于，选取 leader 时间过长，30 ~ 120 s，且选取期间 zk 集群都不可用，这样就会导致选取期间注册服务瘫痪。在云部署的环境下，因网络问题使得zk集群失去master节点是较大概率会发生的事，虽然服务能够恢复，但是漫长的选取时间导致的注册长期不可用是不能容忍的。

2.Eureka 保证了可用性，Eureka 各个节点是平等的，几个节点挂掉不会影响正常节点的工作，剩余的节点仍然可以提供注册和查询服务。而 Eureka 的客户端向某个 Eureka 注册或发现时发生连接失败，则会自动切换到其他节点，只要有一台 Eureka 还在，就能保证注册服务可用，只是查到的信息可能不是最新的。除此之外，Eureka 还有自我保护机制，如果在 15 分钟内超过 85% 的节点没有正常的心跳，那么 Eureka 就认为客户端与注册中心发生了网络故障，此时会出现以下几种情况： 

①、Eureka 不在从注册列表中移除因为长时间没有收到心跳而应该过期的服务。 

②、Eureka 仍然能够接受新服务的注册和查询请求，但是不会被同步到其他节点上（即保证当前节点仍然可用）

③、当网络稳定时，当前实例新的注册信息会被同步到其他节点。

因此，Eureka 可以很好的应对因网络故障导致部分节点失去联系的情况，而不会像 Zookeeper 那样使整个微服务瘫痪



### 6、SpringBoot 和 SpringCloud 的区别？

SpringBoot 专注于快速方便的开发单个个体微服务。

SpringCloud 是关注全局的微服务协调整理治理框架，它将 SpringBoot 开发的一个个单体微服务整合并管理起来，为各个微服务之间提供，配置管理、服务发现、断路器、路由、微代理、事件总线、全局锁、决策竞选、分布式会话等等集成服务

SpringBoot 可以离开 SpringCloud 独立使用开发项目， 但是 SpringCloud 离不开 SpringBoot ，属于依赖的关系。SpringBoot 专注于快速、方便的开发单个微服务个体，SpringCloud 关注全局的服务治理框架。



### 7、负载平衡的意义什么？

在计算中，负载平衡可以改善跨计算机，计算机集群，网络链接，中央处理单元或磁盘驱动器等多种计算资源的工作负载分布。负载平衡旨在优化资源使用，最大化吞吐量，最小化响应时间并避免任何单一资源的过载。使用多个组件进行负载平衡而不是单个组件可能会通过冗余来提高可靠性和可用性。负载平衡通常涉及专用软件或硬件，例如多层交换机或域名系统服务器进程。



### 8、什么是 Hystrix ？它如何实现容错？

Hystrix 是一个延迟和容错库，旨在隔离远程系统，服务和第三方库的访问点，当出现不可避免的故障时，停止级联故障并在复杂的分布式系统中实现弹性。

通常对于使用微服务架构开发的系统，涉及到许多微服务。这些微服务彼此协作。思考以下微服务

![](https://dream-syz.github.io/7-1.png)

假设如果上图中的微服务 9 失败了，那么使用传统方法我们将传播一个异常。但这仍然会导致整个系统崩溃。

随着微服务数量的增加，这个问题变得更加复杂。微服务的数量可以高达 1000。这是 hystrix 出现的地方，我们将使用 Hystrix 在这种情况下的 Fallback 方法功能。我们有两个服务 employee-consumer 使用由 employee-consumer 公开的服务。

简化图如下所示

![](https://dream-syz.github.io/7-2.png)

现在假设由于某种原因，employee-producer 公开的服务会抛出异常。我们在这种情况下使用 Hystrix 定义了一个回退方法。这种后备方法应该具有与公开服务相同的返回类型。如果暴露服务中出现异常，则回退方法将返回一些值。



### 9、什么是 Hystrix 断路器？我们需要它吗？

由于某些原因，employee-consumer 公开服务会引发异常。在这种情况下使用 Hystrix 我们定义了一个回退方法。如果在公开服务中发生异常，则回退方法返回一些默认值

![](https://dream-syz.github.io/7-3.png)

如果 firstPage method() 中的异常继续发生，则 Hystrix 电路将中断，并且员工使用者将一起跳过 firtsPage 方法，并直接调用回退方法。 断路器的目的是给第一页方法或第一页方法可能调用的其他方法留出时间，并导致异常恢复。可能发生的情况是，在负载较小的情况下，导致异常的问题有更好的恢复机会 。

![](https://dream-syz.github.io/7-4.png)



### 10、说说 RPC 的实现原理

首先需要有处理网络连接通讯的模块，负责连接建立、管理和消息的传输。其次需要有编解码的模块，因为网络通讯都是传输的字节码，需要将我们使用的对象序列化和反序列化。剩下的就是客户端和服务器端的部分，服务器端暴露要开放的服务接口，客户调用服务接口的一个代理实现，这个代理实现负责收集数据、编码并传输给服务器然后等待结果返回。



### 11、Eureka 自我保护机制是什么?

当 Eureka Server 节点在短时间内丢失了过多实例的连接时（比如网络故障或频繁启动关闭客户端）节点会进入自我保护模式，保护注册信息，不再删除注册数据，故障恢复时，自动退出自我保护模式。



### 12、什么是 Ribbon ？

ribbon 是一个负载均衡客户端，可以很好的控制 http 和 tcp 的一些行为。feign 默认集成了 ribbon。



### 13、什么是 feigin ？它的优点是什么？

1.feign 采用的是基于接口的注解 

2.feign 整合了 ribbon，具有负载均衡的能力 

3.整合了 Hystrix，具有熔断的能力

使用: 1.添加 pom 依赖。 2.启动类添加 @EnableFeignClients 3.定义一个接口 @FeignClient(name=“xxx”) 指定调用哪个服务



### 14、Ribbon 和 Feign 的区别？

1.Ribbon 都是调用其他服务的，但方式不同。

2.启动类注解不同，Ribbon 是 @RibbonClient；feign 的是 @EnableFeignClients 

3.服务指定的位置不同，Ribbon 是在 @RibbonClient 注解上声明，Feign则是在定义抽象方法的接口中使用 @FeignClient 声明。 

4.调用方式不同，Ribbon 需要自己构建 http 请求，模拟 http 请求





## Dubbo 篇

其实关于 Dubbo 的面试题，我觉得最好的文档应该还是官网，因为官网有中文版，照顾了很多阅读英文文档吃力的小伙伴。但是官网内容挺多的，于是这里就结合官网和平时面试被问的相对较多的题目整理了一下。

### 1、 说说一次 Dubbo 服务请求流程？

基本工作流程：

![](https://dream-syz.github.io/8-1.png)

上图中角色说明：

| 节点      | 角色说明                               |
| --------- | -------------------------------------- |
| Provider  | 暴露服务的服务提供方                   |
| Consumer  | 调用远程服务的服务消费方               |
| Registry  | 服务注册与发现的注册中心               |
| Monitor   | 统计服务的调用次数和调用时间的监控中心 |
| Container | 服务运行容器                           |



### 2、说说 Dubbo 工作原理

工作原理分 10 层：

- 第一层：service 层，接口层，给服务提供者和消费者来实现的（留给开发人员来实现）；

- 第二层：config 层，配置层，主要是对 Dubbo 进行各种配置的，Dubbo 相关配置；

- 第三层：proxy 层，服务代理层，透明生成客户端的 stub 和服务单的 skeleton，调用的是接口，实现类没有，所以得生成代理，代理之间再进行网络通讯、负责均衡等；

- 第四层：registry 层，服务注册层，负责服务的注册与发现；
- 第五层：cluster 层，集群层，封装多个服务提供者的路由以及负载均衡，将多个实例组合成一个服务；

- 第六层：monitor 层，监控层，对 rpc 接口的调用次数和调用时间进行监控；

- 第七层：protocol 层，远程调用层，封装 rpc 调用；

- 第八层：exchange 层，信息交换层，封装请求响应模式，同步转异步；

- 第九层：transport 层，网络传输层，抽象 mina 和 netty 为统一接口；

- 第十层：serialize 层，数据序列化层。

这是个很坑爹的面试题，但是很多面试官又喜欢问，你真的要背么？你能背那还是不错的，我建议不要背，你就想想 Dubbo 服务调用过程中应该会涉及到哪些技术，把这些技术串起来就 OK 了。

**面试扩散**

如果让你设计一个 RPC 框架，你会怎么做？其实你就把上面这个工作原理中涉及的到技术点总结一下就行了。



### 3、Dubbo 支持哪些协议？

![](https://dream-syz.github.io/8-2.png)

还有三种，混个眼熟就行：Memcached 协议、Redis 协议、Rest 协议。

上图基本上把序列化的方式也罗列出来了。

详细请参考：Dubbo 官网：http://dubbo.apache.org/zh-cn/docs/user/references/protocol/dubbo.html。



### 4、注册中心挂了，consumer 还能不能调用 provider？

可以。因为刚开始初始化的时候，consumer 会将需要的所有提供者的地址等信息拉取到本地缓存，所以注册中心挂了可以继续通信。但是 provider 挂了，那就没法调用了。

![](https://dream-syz.github.io/8-3.png)

关键字：consumer 本地缓存服务列表



### 5、怎么实现动态感知服务下线的呢？

![](https://dream-syz.github.io/8-4.png)

服务订阅通常有 pull 和 push 两种方式：

- pull 模式需要客户端定时向注册中心拉取配置；

- push 模式采用注册中心主动推送数据给客户端。

Dubbo ZooKeeper 注册中心采用是事件通知与客户端拉取方式。服务第一次订阅的时候将会拉取对应目录下全量数据，然后在订阅的节点注册一个 watcher。一旦目录节点下发生任何数据变化，ZooKeeper 将会通过 watcher 通知客户端。客户端接到通知，将会重新拉取该目录下全量数据，并重新注册 watcher。利用这个模式，Dubbo 服务就可以做到服务的动态发现。

注意：ZooKeeper 提供了“心跳检测”功能，它会定时向各个服务提供者发送一个请求（实际上建立的是一个 socket 长连接），如果长期没有响应，服务中心就认为该服务提供者已经“挂了”，并将其剔除。



### 6、Dubbo 负载均衡策略？

- 随机（默认）：随机来

- 轮训：一个一个来

- 活跃度：机器活跃度来负载

- 一致性 hash：落到同一台机器上



### 7、Dubbo 容错策略

**failover cluster** **模式**

provider 宕机重试以后，请求会分到其他的 provider 上，默认两次，可以手动设置重试次数，建议把写操作重试次数设置成 0。

**failback** **模式**

失败自动恢复会在调用失败后，返回一个空结果给服务消费者。并通过定时任务对失败的调用进行重试，适合执行消息通知等操作。

**failfast cluster** **模式**

快速失败只会进行一次调用，失败后立即抛出异常。适用于幂等操作、写操作，类似于 failover cluster 模式中重试次数设置为 0 的情况。

**failsafe cluster** **模式**

失败安全是指，当调用过程中出现异常时，仅会打印异常，而不会抛出异常。适用于写入审计日志等操作。

**forking cluster** **模式**

并行调用多个服务器，只要一个成功即返回。通常用于实时性要求较高的读操作，但需要浪费更多服务资源。可通过 forks="2" 来设置最大并行数。

**broadcacst cluster** **模式**

广播调用所有提供者，逐个调用，任意一台报错则报错。通常用于通知所有提供者更新缓存或日志等本地资源信息。



### 8、Dubbo 动态代理策略有哪些？

默认使用 javassist 动态字节码生成，创建代理类，但是可以通过 SPI 扩展机制配置自己的动态代理策略。



### 9、说说 Dubbo 与 Spring Cloud 的区别？

这是很多面试官喜欢问的问题，本人认为其实他们没什么关联之处，但是硬是要问区别，那就说说吧。

回答的时候主要围绕着四个关键点来说：通信方式、注册中心、监控、断路器，其余像 Spring 分布式配置、服务网关肯定得知道。

**通信方式**

Dubbo 使用的是 RPC 通信；Spring Cloud 使用的是 HTTP RestFul 方式。

**注册中心**

Dubbo 使用 ZooKeeper（官方推荐），还有 Redis、Multicast、Simple 注册中心，但不推荐。

Spring Cloud 使用的是 Spring Cloud Netflix Eureka。

**监控**

Dubbo 使用的是 Dubbo-monitor；Spring Cloud 使用的是 Spring Boot admin。

**断路器**

Dubbo 在断路器这方面还不完善，Spring Cloud 使用的是 Spring Cloud Netflix Hystrix。

分布式配置、网关服务、服务跟踪、消息总线、批量任务等。

Dubbo 目前可以说还是空白，而 Spring Cloud 都有相应的组件来支撑。



### 10、Zookeeper 和 Dubbo 的关系？

**Zookeeper 的作用**

zookeeper 用来注册服务和进行负载均衡，哪一个服务由哪一个机器来提供必需让调用者知道，简单来说就是 **ip 地址和服务名称的对应关系**。当然也可以通过硬编码的方式把这种对应关系在调用方业务代码中实现，但是如果提供服务的机器挂掉调用者无法知晓，如果不更改代码会继续请求挂掉的机器提供服务。zookeeper 通过心跳机制可以检测挂掉的机器并将挂掉机器的 ip 和服务对应关系从列表中删除。至于支持高并发，简单来说就是横向扩展，在不更改代码的情况通过添加机器来提高运算能力。通过添加新的机器向 zookeeper 注册服务，服务的提供者多了能服务的客户就多了。

**dubbo**

是管理中间层的工具，在业务层到数据仓库间有非常多服务的接入和服务提供者需要调度，dubbo 提供一个框架解决这个问题。 注意这里的 dubbo 只是一个框架，至于你架子上放什么是完全取决于你的，就像一个汽车骨架，你需要配你的轮子引擎。这个框架中要完成调度必须要有一个分布式的注册中心，储存所有服务的元数据，你可以用 zk，也可以用别的，只是大家都用 zk。

**zookeeper 和 dubbo 的关系**

Dubbo 将注册中心进行抽象，它可以外接不同的存储媒介给注册中心提供服务，有 ZooKeeper，Memcached，Redis 等。

引入了 ZooKeeper 作为存储媒介，也就把 ZooKeeper 的特性引进来。首先是负载均衡，单注册中心的承载能力是有限的，在流量达到一定程度的时候就需要分流，负载均衡就是为了分流而存在的，一个 ZooKeeper 群配合相应的 Web 应用就可以很容易达到负载均衡；资源同步，单有负载均衡还不够，节点之间的数据和资源需要同步，ZooKeeper 集群就天然具备有这样的功能；命名服务，将树状结构用于维护全局的服务地址列表，服务提供者在启动的时候，向 ZooKeeper 上的指定节点 /dubbo/${serviceName}/providers 目录下写入自己的 URL 地址，这个操作就完成了服务的发布。 其他特性还有 Master 选举，分布式锁等。

![](https://dream-syz.github.io/8-5.png)





## Nginx 篇

### 1、简述一下什么是 Nginx，它有什么优势和功能？

Nginx 是一个 web 服务器和反向代理服务器，用于 HTTP、HTTPS、SMTP、POP3 和 IMAP 协议。因它的稳定性、丰富的功能集、示例配置文件和低系统资源的消耗而闻名。

> Nginx---Ngine X，是一款免费的、自由的、开源的、高性能 HTTP 服务器和反向代理服务器；也是一个 IMAP、POP3、SMTP 代理服务器；Nginx 以其高性能、稳定性、丰富的功能、简单的配置和低资源消耗而闻名。
>
> 也就是说 Nginx 本身就可以托管网站（类似于 Tomcat 一样），进行 Http 服务处理，也可以作为反向代理服务器 、负载均衡器和HTTP 缓存。
>
> Nginx 解决了服务器的 C10K（就是在一秒之内连接客户端的数目为 10k 即 1 万）问题。它的设计不像传统的服务器那样使用线程处理请求，而是一个更加高级的机制—事件驱动机制，是一种异步事件驱动结构。

#### **优点：**

**（1）更快** 这表现在两个方面：一方面，在正常情况下，单次请求会得到更快的响应；另一方面，在高峰期（如有数以万计的并发请求），Nginx 可以比其他 Web 服务器更快地响应请求。

**（2）高扩展性，跨平台** Nginx 的设计极具扩展性，它完全是由多个不同功能、不同层次、不同类型且耦合度极低的模块组成。因此，当对某一个模块修复 Bug 或进行升级时，可以专注于模块自身，无须在意其他。而且在 HTTP 模块中，还设计了 HTTP 过滤器模块：一个正常的 HTTP 模块在处理完请求后，会有一串 HTTP 过滤器模块对请求的结果进行再处理。这样，当我们开发一个新的 HTTP 模块时，不但可以使用诸如 HTTP 核心模块、events 模块、log 模块等不同层次或者不同类型的模块，还可以原封不动地复用大量已有的 HTTP 过滤器模块。这种低耦合度的优秀设计，造就了 Nginx 庞大的第三方模块，当然，公开的第三方模块也如官方发布的模块一样容易使用。 Nginx 的模块都是嵌入到二进制文件中执行的，无论官方发布的模块还是第三方模块都是如此。这使得第三方模块一样具备极其优秀的性能，充分利用 Nginx 的高并发特性，因此，许多高流量的网站都倾向于开发符合自己业务特性的定制模块。

**（3）高可靠性：用于反向代理，宕机的概率微乎其微** 高可靠性是我们选择 Nginx 的最基本条件，因为 Nginx 的可靠性是大家有目共睹的，很多家高流量网站都在核心服务器上大规模使用 Nginx。Nginx 的高可靠性来自于其核心框架代码的优秀设计、模块设计的简单性；另外，官方提供的常用模块都非常稳定，每个 worker 进程相对独立，master 进程在 1 个 worker 进程出错时可以快速“拉起”新的 worker子进程提供服务。

**（4）低内存消耗** 一般情况下，10 000 个非活跃的 HTTP Keep-Alive 连接在 Nginx 中仅消耗 2.5 MB 的内存，这是 Nginx 支持高并发连接的基础。 

**（5）单机支持 10 万以上的并发连接** 这是一个非常重要的特性！随着互联网的迅猛发展和互联网用户数量的成倍增长，各大公司、网站都需要应付海量并发请求，一个能够在峰值期顶住 10 万以上并发请求的 Server，无疑会得到大家的青睐。理论上，Nginx 支持的并发连接上限取决于内存，10 万远未封顶。当然，能够及时地处理更多的并发请求，是与业务特点紧密相关的。

**（6）热部署** master 管理进程与 worker 工作进程的分离设计，使得 Nginx 能够提供热部署功能，即可以在 7×24 小时不间断服务的前提下，升级 Nginx 的可执行文件。当然，它也支持不停止服务就更新配置项、更换日志文件等功能。

**（7）最自由的 BSD 许可协议** 这是 Nginx 可以快速发展的强大动力。BSD 许可协议不只是允许用户免费使用 Nginx，它还允许用户在自己的项目中直接使用或修改 Nginx 源码，然后发布。这吸引了无数开发者继续为 Nginx 贡献自己的智慧。 

以上 7 个特点当然不是 Nginx的全部，拥有无数个官方功能模块、第三方功能模块使得 Nginx 能够满足绝大部分应用场景，这些功能模块间可以叠加以实现更加强大、复杂的功能，有些模块还支持 Nginx 与 Perl、Lua 等脚本语言集成工作，大大提高了开发效率。这些特点促使用户在寻找一个 Web 服务器时更多考虑 Nginx。 选择 Nginx 的核心理由还是它能在支持高并发请求的同时保持高效的服务。



### 2、Nginx 是如何处理一个 HTTP 请求的呢？

Nginx 是一个高性能的 Web 服务器，能够同时处理大量的并发请求。它结合多进程机制和异步机制，异步机制使用的是异步非阻塞方式，接下来就给大家介绍一下 Nginx 的多线程机制和异步非阻塞机制 。

**1、多进程机制**

服务器每当收到一个客户端时，就有服务器主进程 （ master process ）生成一个 子进程（worker process ）出来和客户端建立连接进行交互，直到连接断开，该子进程就结束了。

使用进程的好处是各个进程之间相互独立，不需要加锁，减少了使用锁对性能造成影响，同时降低编程的复杂度，降低开发成本。其次，采用独立的进程，可以让进程互相之间不会影响 ，如果一个进程发生异常退出时，其它进程正常工作， master 进程则很快启动新的 worker 进程，确保服务不会中断，从而将风险降到最低。

缺点是操作系统生成一个子进程需要进行内存复制等操作，在资源和时间上会产生一定的开销。当有大量请求时，会导致系统性能下降。

**2、异步非阻塞机制**

每个工作进程使用异步非阻塞方式 ，可以处理多个客户端请求 。

当某个工作进程接收到客户端的请求以后，调用 IO 进行处理，如果不能立即得到结果，就去处理其他请求 （即为非阻塞 ）；而客户端 在此期间也无需等待响应 ，可以去处理其他事情（即为异步 ）。

当 IO 返回时，就会通知此工作进程 ；该进程得到通知，暂时挂起当前处理的事务去响应客户端请求 。



### 3、列举一些 Nginx 的特性

Nginx 服务器的特性包括：

1. 反向代理 / L7 负载均衡器

2. 嵌入式 Perl 解释器

3. 动态二进制升级

4. 可用于重新编写 URL，具有非常好的 PCRE 支持

   

### 4、请列举 Nginx 和 Apache 之间的不同点

| Nginx                                                       | Apache                                                       |
| ----------------------------------------------------------- | ------------------------------------------------------------ |
| Nginx 是一个基于事件的 Web 服务器，所有请求都由一个线程处理 | Apache  是一个基于流程的服务器，单个线程处理单个请求         |
| Nginx  避免子进程的概念，Nginx  类似于速度                  | Apache  基于子进程，Apache  类似于功率                       |
| Nginx  在内存消耗和连接方面比较好                           | Apache  内存消耗和连接上并没有提高                           |
| Nginx  在负载均衡方面表现较好                               | 当流量达到进程的极限时，Apache  将拒绝新的连接               |
| 对于 PHP 来说，Nginx 可能更可取，因为它支持 PHP             | Apache 支持的 PHP、Python、Perl 和其他语言使用插件，当应用程序基于Python 或 Ruby 时，它非常有用 |
| Nginx 不支持像 IBMi 和 OpenVMS 一样的 OS                    | Apache 支持更多的 OS                                         |
| Nginx 只具有核心功能                                        | Apache 提供了比 Nginx 更多的功能                             |
| Nginx 的性能和可伸缩性不依赖于硬件                          | Apache 依赖于 CPU 和内存等硬件组件                           |



### 5、在 Nginx 中，如何使用未定义的服务器名称来阻止处理请求？

只需将请求删除的服务器定义为：

```properties
Server{
	listen 80;
	server_name "";
	return 444;
}
```

这里，服务器名被保留为一个空字符串，它将在没有“主机”头字段的情况下匹配请求，而一个特殊的 Nginx 的非标准代码 444 被返回，从而终止连接。

**一般推荐 worker 进程数与 CPU 内核数一致，这样一来不存在大量的子进程生成和管理任务，避免了进程之间竞争 CPU 资源和进程切换的开销**。而且 Nginx 为了更好的利用多核特性 ，提供了 CPU 亲缘性的绑定选项，我们可以将某一个进程绑定在某一个核上，这样就不会因为进程的切换带来 Cache 的失效。

**对于每个请求，有且只有一个工作进程对其处理**。首先，每个 worker 进程都是从 master 进程 fork 过来。在 master 进程里面，先建立好需要 listen 的 socket（listenfd） 之后，然后再 fork 出多个 worker 进程。所有 worker 进程的 listenfd 会在新连接到来时变得可读，为保证只有一个进程处理该连接，所有 worker 进程在注册 listenfd 读事件前抢占 accept_mutex ，抢到互斥锁的那个进程注册 listenfd 读事件 ，在读事件里调用 accept 接受该连接。

当一个 worker 进程在 accept 这个连接之后，就开始读取请求、解析请求、处理请求，产生数据后，再返回给客户端 ，最后才断开连接。这样一个完整的请求就是这样的了。我们可以看到，一个请求，完全由 worker 进程来处理，而且只在一个 worker 进程中处理。

在 Nginx 服务器的运行过程中， 主进程和工作进程需要进程交互。**交互依赖于 Socket 实现的管道来实现**。



### 6、请解释 Nginx 服务器上的 Master 和 Worker 进程分别是什么?

主程序 Master process 启动后，通过一个 for 循环来接收和处理外部信号 ；

主进程通过 fork() 函数产生 worker 子进程 ，每个子进程执行一个 for 循环来实现 Nginx 服务器对事件的接收和处理 。



### 7、请解释代理中的正向代理和反向代理

首先，代理服务器一般指局域网内部的机器通过代理服务器发送请求到互联网上的服务器，代理服务器一般作用在客户端。例如：GoAgent 翻墙软件。我们的客户端在进行翻墙操作的时候，我们使用的正是正向代理，通过正向代理的方式，在我们的客户端运行一个软件，将我们的 HTTP 请求转发到其他不同的服务器端，实现请求的分发。

反向代理服务器作用在服务器端，它在服务器端接收客户端的请求，然后将请求分发给具体的服务器进行处理，然后再将服务器的相应结果反馈给客户端。Nginx 就是一个反向代理服务器软件。

可以看出：客户端必须设置正向代理服务器，当然前提是要知道正向代理服务器的 IP 地址，还有代理程序的端口。 反向代理正好与正向代理相反，对于客户端而言代理服务器就像是原始服务器，并且客户端不需要进行任何特别的设置。客户端向反向代理的命名空间（name-space）中的内容发送普通请求，接着反向代理将判断向何处（原始服务器）转交请求，并将获得的内容返回给客户端。



### 8、解释 Nginx 用途

Nginx 服务器的最佳用法是在网络上部署动态 HTTP 内容，使用 SCGI、WSGI 应用程序服务器、用于脚本的 FastCGI 处理程序。它还可以作为负载均衡器。





## **MQ** 篇

### 1、为什么要使用 MQ

**核心：解耦，异步，削峰**

**（1）解耦：**A 系统发送数据到 BCD 三个系统，通过接口调用发送。如果 E 系统也要这个数据呢？那如果 C 系统现在不需要了呢？A 系统负责人几乎崩溃......A 系统跟其它各种乱七八糟的系统严重耦合，A 系统产生一条比较关键的数据，很多系统都需要 A 系统将这个数据发送过来。如果使用 MQ，A 系统产生一条数据，发送到 MQ 里面去，哪个系统需要数据自己去 MQ 里面消费。如果新系统需要数据，直接从 MQ 里消费即可；如果某个系统不需要这条数据了，就取消对 MQ 消息的消费即可。这样下来，A 系统压根儿不需要去考虑要给谁发送数据，不需要维护这个代码，也不需要考虑人家是否调用成功、失败超时等情况。

就是一个系统或者一个模块，调用了多个系统或者模块，互相之间的调用很复杂，维护起来很麻烦。但是其实这个调用是不需要直接同步调用接口的，如果用 MQ 给它异步化解耦。

**（2）异步：**A 系统接收一个请求，需要在自己本地写库，还需要在 BCD 三个系统写库，自己本地写库要 3ms，BCD 三个系统分别写库要 300ms、450ms、200ms。最终请求总延时是 3 + 300 + 450 + 200 = 953ms，接近 1s，用户感觉搞个什么东西，慢死了慢死了。用户通过浏览器发起请求。如果使用 MQ，那么 A 系统连续发送 3 条消息到 MQ 队列中，假如耗时 5ms，A 系统从接受一个请求到返回响应给用户，总时长是 3 + 5 = 8ms。

**（3）削峰：**减少高峰时期对服务器压力。



### 2、MQ 有什么优缺点

优点上面已经说了，就是在特殊场景下有其对应的好处，**解耦、异步、削峰**。

#### **缺点有以下几个：**

**系统可用性降低** 系统引入的外部依赖越多，越容易挂掉。万一 MQ 挂了，MQ 一挂，整套系统崩溃，你不就完了？

**系统复杂度提高** 硬生生加个 MQ 进来，你怎么保证消息没有重复消费？怎么处理消息丢失的情况？怎么保证消息传递的顺序性？问题一大堆。

**一致性问题** A 系统处理完了直接返回成功了，人都以为你这个请求就成功了；但是问题是，要是

BCD 三个系统那里，BD 两个系统写库成功了，结果 C 系统写库失败了，咋整？你这数据就不一致了。



### 3、Kafka、ActiveMQ、RabbitMQ、RocketMQ 都有什么区别？

对于吞吐量来说 kafka 和 RocketMQ 支撑高吞吐，ActiveMQ 和 RabbitMQ 比他们低一个数量级。对于延迟量来说 RabbitMQ 是最低的。

**1.从社区活跃度**

按照目前网络上的资料，RabbitMQ 、activeMQ 、ZeroMQ 三者中，综合来看，RabbitMQ 是首选。

**2.持久化消息比较**

ActiveMQ 和 RabbitMQ 都支持。持久化消息主要是指我们机器在不可抗力因素等情况下挂掉了，消息不会丢失的机制。

**3.综合技术实现**

可靠性、灵活的路由、集群、事务、高可用的队列、消息排序、问题追踪、可视化管理工具、插件系统等等。

RabbitMQ / Kafka 最好，ActiveMQ 次之，ZeroMQ 最差。当然 ZeroMQ 也可以做到，不过自己必须手动写代码实现，代码量不小。尤其是可靠性中的：持久性、投递确认、发布者证实和高可用性。

**4.高并发**

毋庸置疑，RabbitMQ 最高，原因是它的实现语言是天生具备高并发高可用的 erlang 语言。

**5.比较关注的比较，** **RabbitMQ** **和** **Kafka**

RabbitMQ 比 Kafka 成熟，在可用性上，稳定性上，可靠性上， RabbitMQ 胜于 Kafka （理论上）。

另外，Kafka 的定位主要在日志等方面， 因为 Kafka 设计的初衷就是处理日志的，可以看做是一个日志（消息）系统一个重要组件，针对性很强，所以如果业务方面还是建议选择 RabbitMq 。

还有就是，Kafka 的性能（吞吐量、TPS ）比 RabbitMq 要高出来很多。



### 4、如何保证高可用的？

RabbitMQ 是比较有代表性的，因为是 **基于主从**（非分布式）做高可用性的，我们就以 RabbitMQ为例子讲解第一种 MQ 的高可用性怎么实现。**RabbitMQ 有三种模式：单机模式、普通集群模式、镜像集群模式**。

单机模式，就是 Demo 级别的，一般就是你本地启动了玩玩儿的?，没人生产用单机模式

普通集群模式，意思就是在多台机器上启动多个 RabbitMQ 实例，每个机器启动一个。你 **创建的 queue，只会放在一个 RabbitMQ** **实例上**，但是每个实例都同步 queue 的元数据（元数据可以认为是 queue 的一些配置信息，通过元数据，可以找到 queue 所在实例）。你消费的时候，实际上如果连接到了另外一个实例，那么那个实例会从 queue 所在实例上拉取数据过来。**这方案主要是提高吞吐量的**，就让集群中多个节点来服务某个 queue 的读写操作。镜像集群模式：这种模式，才是所谓的 RabbitMQ 的高可用模式。

跟普通集群模式不一样的是，在镜像集群模式下，你创建的 queue，无论元数据还是 queue 里的消息都会**存在于多个实例上**，就是说，每个 RabbitMQ 节点都有这个 queue 的一个完整镜像，包含 queue 的全部数据的意思。然后每次你写消息到 queue 的时候，都会自动把消息同步到多个实例的 queue 上。RabbitMQ 有很好的管理控制台，就是在后台新增一个策略，这个策略是镜像集群模式的策略，指定的时候是可以要求数据同步到所有节点的，也可以要求同步到指定数量的节点，再次创建 queue 的时候，应用这个策略，就会自动将数据同步到其他的节点上去了。这样的话，好处在于，你任何一个机器宕机了，没事儿，其它机器（节点）还包含了这个 queue 的完整数据，别的 consumer 都可以到其它节点上去消费数据。坏处在于，第一，这个性能开销也太大了吧，消息需要同步到所有机器上，导致网络带宽压力和消耗很重！RabbitMQ 一个 queue 的数据都是放在一个节点里的，镜像集群下，也是每个节点都放这个 queue 的完整数据。

Kafka 一个最基本的架构认识：由多个 broker 组成，每个 broker 是一个节点；你创建一个 topic，这个 topic 可以划分为多 partition，每个 partition 可以存在于不同的 broker 上，每个 partition 就放一部分数据。这就是天然的分布式消息队列，就是说一个 topic 的数据，是**分散放在多个机器上的，每个机器就放一部分数据**。Kafka 0.8 以后，提供了 HA 机制，就是 replica（复制品） 副本机制。每个 partition 的数据都会同步到其它机器上，形成自己的多个 replica 副本。所有 replica 会选举一个 leader 出来，那么生产和消费都跟这个 leader 打交道，然后其他 replica 就是 follower。写的时候，leader 会负责把数据同步到所有 follower 上去，读的时候就直接读 leader上的数据即可。只能读写 leader？很简单，要是你可以随意读写每个 follower，那么就要 care 数据一致性的问题，系统复杂度太高，很容易出问题。Kafka 会均匀地将一个 partition 的所有 replica 分布在不同的机器上，这样才可以提高容错性。因为如果某个 broker 宕机了，没事儿，那个 broker上面的 partition 在其他机器上都有副本的，如果这上面有某个 partition 的 leader，那么此时会从 follower 中重新选举一个新的 leader 出来，大家继续读写那个新的 leader 即可。这就有所谓的高可用性了。写数据的时候，生产者就写 leader，然后 leader 将数据落地写本地磁盘，接着其他 follower 自己主动从 leader 来 pull 数据。一旦所有 follower 同步好数据了，就会发送 ack 给 leader，leader 收到所有 follower 的 ack 之后，就会返回写成功的消息给生产者。（当然，这只是其中一种模式，还可以适当调整这个行为）消费的时候，只会从 leader 去读，但是只有当一个消息已经被所有 follower 都同步成功返回 ack 的时候，这个消息才会被消费者读到。



### 5、如何保证消息的可靠传输？如果消息丢了怎么办

数据的丢失问题，可能出现在生产者、MQ、消费者中生产者丢失：生产者将数据发送到 RabbitMQ 的时候，可能数据就在半路给搞丢了，因为网络问题啥的，都有可能。此时可以选择用 RabbitMQ 提供的事务功能，就是生产者发送数据之前开启 RabbitMQ 事务`channel.txSelect`，然后发送消息，如果消息没有成功被 RabbitMQ 接收到，那么生产者会收到异常报错，此时就可以回滚事务`channel.txRollback`，然后重试发送消息；如果收到了消息，那么可以提交事务 `channel.txCommit`。吞吐量会下来，因为太耗性能。所以一般来说，如果你要确保说写 RabbitMQ 的消息别丢，可以开启 confirm 模式，在生产者那里设置开启 confirm 模式之后，你每次写的消息都会分配一个唯一的 id，然后如果写入了 RabbitMQ 中，RabbitMQ 会给你回传一个 ack 消息，告诉你说这个消息 ok 了。如果 RabbitMQ 没能处理这个消息，会回调你一个 nack 接口，告诉你这个消息接收失败，你可以重试。而且你可以结合这个机制自己在内存里维护每个消息 id 的状态，如果超过一定时间还没接收到这个消息的回调，那么你可以重发。事务机制和 cnofirm 机制最大的不同在于，事务机制是同步的，你提交一个事务之后会阻塞在那儿，但是 **confirm 机制是异步** 的，你发送个消息之后就可以发送下一个消息，然后那个消息 RabbitMQ 接收了之后会异步回调你一个接口通知你这个消息接收到了。所以一般在生产者这块避免数据丢失，都是用confirm机制的。

MQ中丢失：就是 RabbitMQ 自己弄丢了数据，这个你必须开启 RabbitMQ 的持久化，就是消息写入之后会持久化到磁盘，哪怕是 RabbitMQ 自己挂了，恢复之后会自动读取之前存储的数据，一般数据不会丢。设置持久化有两个步骤：创建 queue 的时候将其设置为持久化，这样就可以保证 RabbitMQ 持久化 queue 的元数据，但是不会持久化 queue 里的数据。第二个是发送消息的时候将消息的 deliveryMode 设置为 2，就是将消息设置为持久化的，此时 RabbitMQ 就会将消息持久化到磁盘上去。必须要同时设置这两个持久化才行，RabbitMQ 哪怕是挂了，再次重启，也会从磁盘上重启恢复 queue，恢复这个 queue 里的数据。持久化可以跟生产者那边的confirm 机制配合起来，只有消息被持久化到磁盘之后，才会通知生产者 ack 了，所以哪怕是在持久化到磁盘之前，RabbitMQ 挂了，数据丢了，生产者收不到ack，你也是可以自己重发的。注意，哪怕是你给 RabbitMQ 开启了持久化机制，也有一种可能，就是这个消息写到了 RabbitMQ 中，但是还没来得及持久化到磁盘上，结果不巧，此时 RabbitMQ 挂了，就会导致内存里的一点点数据丢失。

消费端丢失：你消费的时候，刚消费到，还没处理，结果进程挂了，比如重启了，那么就尴尬了，RabbitMQ 认为你都消费了，这数据就丢了。这个时候得用 RabbitMQ 提供的 ack 机制，简单来说，就是你关闭 RabbitMQ 的自动 ack，可以通过一个 api 来调用就行，然后每次你自己代码里确保处理完的时候，再在程序里 ack一把。这样的话，如果你还没处理完，不就没有 ack？那 RabbitMQ 就认为你还没处理完，这个时候 RabbitMQ 会把这个消费分配给别的 consumer 去处理，消息是不会丢的。

![](https://dream-syz.github.io/9-1.png)



### 6、如何保证消息的顺序性

先看看顺序会错乱的场景：RabbitMQ：一个 queue，多个 consumer，这不明显乱了；

![](https://dream-syz.github.io/9-2.png)

解决：





### 7、 如何解决消息队列的延时以及过期失效问题？消息队列满了以后该怎么处理？有几百万消息持续积压几小时，说说怎么解决？

消息积压处理办法：临时紧急扩容：

先修复 consumer 的问题，确保其恢复消费速度，然后将现有 cnosumer 都停掉。 新建一个 topic，partition 是原来的 10 倍，临时建立好原先 10 倍的 queue 数量。 然后写一个临时的分发数据的 consumer 程序，这个程序部署上去消费积压的数据，消费之后不做耗时的处理，直接均匀轮询写入临时建立好的 10 倍数量的 queue。 接着临时征用 10 倍的机器来部署 consumer，每一批 consumer 消费一个临时 queue 的数据。这种做法相当于是临时将 queue 资源和 consumer 资源扩大 10 倍，以正常的 10 倍速度来消费数据。 等快速消费完积压数据之后，得恢复原先部署的架构，重新用原先的 consumer 机器来消费消息。 

MQ中消息失效：假设你用的是 RabbitMQ，RabbtiMQ 是可以设置过期时间的，也就是 TTL。如果消息在 queue 中积压超过一定的时间就会被 RabbitMQ 给清理掉，这个数据就没了。那这就是第二个坑了。这就不是说数据会大量积压在 mq 里，而是大量的数据会直接搞丢。我们可以采取一个方案，就是**批量重导**，这个我们之前线上也有类似的场景干过。就是大量积压的时候，我们当时就直接丢弃数据了，然后等过了高峰期以后，比如大家一起喝咖啡熬夜到晚上 12 点以后，用户都睡觉了。这个时候我们就开始写程序，将丢失的那批数据，写个临时程序，一点一点的查出来，然后重新灌入 mq 里面去，把白天丢的数据给他补回来。也只能是这样了。假设 1 万个订单积压在 mq 里面，没有处理，其中 1000 个订单都丢了，你只能手动写程序把那 1000 个订单给查出来，手动发到 mq 里去再补一次。

mq 消息队列快满了：如果消息积压在 mq 里，你很长时间都没有处理掉，此时导致 mq 都快写满了，咋办？这个还有别的办法吗？没有，谁让你第一个方案执行的太慢了，你临时写程序，接入数据来消费，消费一个丢弃一个，都不要了，快速消费掉所有的消息。然后走第二个方案，到了晚上再补数据吧。



### 8、让你来设计一个消息队列，你会怎么设计

比如说这个消息队列系统，我们从以下几个角度来考虑一下：

首先这个 mq 得支持可伸缩性吧，就是需要的时候快速扩容，就可以增加吞吐量和容量，那怎么搞？设计个分布式的系统呗，参照一下 kafka 的设计理念，broker -> topic -> partition，每个 partition 放一个机器，就存一部分数据。如果现在资源不够了，简单啊，给 topic 增加 partition，然后做数据迁移，增加机器，不就可以存放更多数据，提供更高的吞吐量了？

其次你得考虑一下这个 mq 的数据要不要落地磁盘吧？那肯定要了，落磁盘才能保证别进程挂了数据就丢了。那落磁盘的时候怎么落啊？顺序写，这样就没有磁盘随机读写的寻址开销，磁盘顺序读写的性能是很高的，这就是 kafka 的思路。

再次你考虑一下你的 mq 的可用性啊？这个事儿，具体参考之前可用性那个环节讲解的 kafka 的高可用保障机制。多副本 -> leader & follower -> broker 挂了重新选举 leader 即可对外服务。

能不能支持数据 0 丢失啊？可以的，参考我们之前说的那个 kafka 数据零丢失方案。（题 5）





## 数据结构与算法篇

在另外两本小册子里。



## Linux 篇

### 1、 绝对路径用什么符号表示？当前目录、上层目录用什么表示？主目录用什么表示? 切换目录用什么命令？

绝对路径： 如 /etc/init.d

当前目录和上层目录： ./ ../

主目录： ~/

切换目录： cd



### 2、 怎么查看当前进程？怎么执行退出？怎么查看当前路径？

查看当前进程： ps

> ps -l 列出与本次登录有关的进程信息； ps -aux 查询内存中进程信息； ps -aux | grep **查询 **进程的详细信息； top 查看内存中进程的动态信息； kill -9 pid 杀死进程。

执行退出： exit

查看当前路径： pwd



### 3、查看文件有哪些命令

```
vi 文件名 #编辑方式查看，可修改
cat 文件名 #显示全部文件内容
more 文件名 #分页显示文件内容
less 文件名 #与 more 相似，更好的是可以往前翻页
tail 文件名 #仅查看尾部，还可以指定行数
head 文件名 #仅查看头部,还可以指定行数
```



### 4、列举几个常用的 Linux 命令

- 列出文件列表：ls【参数 -a -l】

- 创建目录和移除目录：mkdir rmdir

- 用于显示文件后几行内容：tail，例如： tail -n 1000：显示最后1000行

- 打包：tar -xvf

- 打包并压缩：tar -zcvf

- 查找字符串：grep

- 显示当前所在目录：pwd创建空文件：touch

- 编辑器：vim vi



### 5、你平时是怎么查看日志的？

Linux 查看日志的命令有多种: tail、cat、tac、head、echo 等，本文只介绍几种常用的方法。

#### **1、tail**

最常用的一种查看方式

**命令格式**: tail [ 必要参数 ] [ 选择参数] [ 文件 ] 

-f 循环读取 -q 不显示处理信息 -v 显示详细的处理信息 -c<数目> 显示的字节数 -n<行数> 显示行数 -q, --quiet, --silent 从不输出给出文件名的首部 -s, --sleep-interval=S 与 -f 合用,表示在每次反复的间隔休眠S秒

例如：

```
tail -n 10 test.log 查询日志尾部最后 10 行的日志;
tail -n +10 test.log 查询 10 行之后的所有日志;
tail -fn 10 test.log 循环实时查看最后 1000 行记录(最常用的)
```

一般还会配合着 grep 搜索用，例如 :

```
tail -fn 1000 test.log | grep '关键字'
```

如果一次性查询的数据量太大，可以进行翻页查看，例如:

```
tail -n 4700 aa.log |more -1000 可以进行多屏显示(ctrl + f 或者 空格键可以快捷键)
```

#### **2、head**

跟 tail 是相反的 head 是看前多少行日志

```
head -n 10 test.log 查询日志文件中的头 10 行日志;
head -n -10 test.log 查询日志文件除了最后 10 行的其他所有日志;
```

head 其他参数参考 tail

#### 3、cat

cat 是由第一行到最后一行连续显示在屏幕上

一次显示整个文件 ：

```
$ cat filename
```

从键盘创建一个文件 :

```
$ cat > filename
```

将几个文件合并为一个文件：

```
$ cat file1 file2 > file 只能创建新文件,不能编辑已有文件
```

将一个日志文件的内容追加到另外一个 ：

```
$ cat -n textfile1 > textfile2
```

清空一个日志文件:

```
$ cat : >textfile2
```

注意：> 意思是创建，>> 是追加。千万不要弄混了。

cat 其他参数参考 tail

#### 4、more

more 命令是一个基于 vi 编辑器文本过滤器，它以全屏幕的方式按页显示文本文件的内容，支持 vi 中的关键字定位操作。more 名单中内置了若干快捷键，常用的有 H（获得帮助信息），Enter（向下翻滚一行），空格（向下滚动一屏），Q（退出命令）。more 命令从前向后读取文件，因此在启动时就加载整个文件。

该命令一次显示一屏文本，满屏后停下来，并且在屏幕的底部出现一个提示信息，给出至今己显示的该文件的百分比：–More–（XX%）

- more 的语法：more 文件名

- Enter 向下 n 行，需要定义，默认为 1 行

- Ctrl f 向下滚动一屏

- 空格键 向下滚动一屏

- Ctrl b 返回上一屏

- = 输出当前行的行号

- :f 输出文件名和当前行的行号

- v 调用 vi 编辑器

- ! 命令 调用 Shell，并执行命令

- q 退出 more

#### 5、sed

这个命令可以查找日志文件特定的一段 , 根据时间的一个范围查询，可以按照行号和时间范围查询

按照行号

```
sed -n '5,10p' filename 这样你就可以只查看文件的第 5 行到第 10 行。
```

**按照时间段**

```cmake
sed -n '/2014-12-17 16:17:20/,/2014-12-17 16:17:36/p' test.log
```

#### 6、less

less 命令在查询日志时，一般流程是这样的

```
less log.log
shift + G 命令到文件尾部 然后输入 ？加上你要搜索的关键字例如 ？1213
按 n 向上查找关键字
shift + n 反向查找关键字
less 与 more 类似，使用 less 可以随意浏览文件，而 more 仅能向前移动，不能向后移动，而且 less 在查看之前不会加载整个文件。
less log2013.log 查看文件
ps -ef | less ps 查看进程信息并通过 less 分页显示
history | less 查看命令历史使用记录并通过 less 分页显示
less log2013.log log2014.log 浏览多个文件
```

常用命令参数：

```
-b <缓冲区大小> 设置缓冲区的大小
-g 只标志最后搜索的关键词
-i 忽略搜索时的大小写
-m 显示类似 more 命令的百分比
-N 显示每行的行号
-o <文件名> 将 less 输出的内容在指定文件中保存起来
-Q 不使用警告音
-s 显示连续空行为一行
/ 字符串：向下搜索"字符串"的功能
? 字符串：向上搜索"字符串"的功能
n：重复前一个搜索（与 / 或 ? 有关）
N：反向重复前一个搜索（与 / 或 ? 有关）
b 向后翻一页
h 显示帮助界面
q 退出 less 命令
```

**一般查日志配合应用的其他命令**

```
history // 所有的历史记录
history | grep XXX // 历史记录中包含某些指令的记录
history | more // 分页查看记录
history -c // 清空所有的历史记录
!! 重复执行上一个命令
查询出来记录后选中 : !323
```





## Zookeeper 篇

### 1、说说 Zookeeper 是什么？

**直译：**从名字上直译就是动物管理员，动物指的是 Hadoop 一类的分布式软件，管理员三个字体现了 **ZooKeeper 的特点：维护、协调、管理、监控**。

**简述：**有些软件你想做成集群或者分布式，你可以用 ZooKeeper 帮你来辅助实现。

**特点：**

- 最终一致性：客户端看到的数据最终是一致的。

- 可靠性：服务器保存了消息，那么它就一直都存在。

- 实时性：ZooKeeper 不能保证两个客户端同时得到刚更新的数据。

- 独立性（等待无关）：不同客户端直接互不影响。

- 原子性：更新要不成功要不失败，没有第三个状态。

**注意**：回答面试题，切忌只是简单一句话回答，可以将你对概念的理解，特点等多个方面描述一下，哪怕你自己认为不完全切中题意的也可以说说，面试官不喜欢会打断你的，你的目的是让面试官认为你是好沟通的。当然了，如果不会可别装作会，说太多不专业的想法。



### 2、ZooKeeper 有哪些应用场景？

#### **数据发布与订阅**

发布与订阅即所谓的配置管理，顾名思义就是将数据发布到 ZooKeeper 节点上，供订阅者动态获取数据，实现配置信息的集中式管理和动态更新。例如全局的配置信息，地址列表等就非常适合使用。数据发布 / 订阅的一个常见的场景是配置中心，发布者把数据发布到 ZooKeeper 的一个或一系列的节点上，供订阅者进行数据订阅，达到动态获取数据的目的。

配置信息一般有几个特点:

1. 数据量小的 KV
2. 数据内容在运行时会发生动态变化
3. 集群机器共享，配置一致

![](https://dream-syz.github.io/10-1.png)

ZooKeeper 采用的是推拉结合的方式。

1. 推: 服务端会推给注册了监控节点的客户端 Wathcer 事件通知
2. 拉: 客户端获得通知后，然后主动到服务端拉取最新的数据

#### **命名服务**

作为分布式命名服务，命名服务是指通过指定的名字来获取资源或者服务的地址，利用 ZooKeeper 创建一个全局的路径，这个路径就可以作为一个名字，指向集群中的集群，提供的服务的地址，或者一个远程的对象等等。

统一命名服务的命名结构图如下所示：

![](https://dream-syz.github.io/10-2.png)

1、在分布式环境下，经常需要对应用/服务进行统一命名，便于识别不同服务。类似于域名与 IP 之间对应关系，IP 不容易记住，而域名容易记住。通过名称来获取资源或服务的地址，提供者等信息。

2、按照层次结构组织服务 / 应用名称。可将服务名称以及地址信息写到 ZooKeeper 上，客户端通过 ZooKeeper 获取可用服务列表类。

#### **配置管理**

程序分布式的部署在不同的机器上，将程序的配置信息放在 ZooKeeper 的 znode 下，当有配置发生改变时，也就是 znode 发生变化时，可以通过改变 zk 中某个目录节点的内容，利用 watch 通知给各个客户端，从而更改配置。

ZooKeeper 配置管理结构图如下所示：

<img src="https://dream-syz.github.io/10-3.png" style="zoom:80%;" />

1、分布式环境下，配置文件管理和同步是一个常见问题。

- 一个集群中，所有节点的配置信息是一致的，比如 Hadoop 集群。
- 对配置文件修改后，希望能够快速同步到各个节点上。

2、配置管理可交由 ZooKeeper 实现。

- 可将配置信息写入 ZooKeeper 上的一个 Znode。

- 各个节点监听这个 Znode。

- 一旦 Znode 中的数据被修改，ZooKeeper 将通知各个节点。

#### **集群管理**

所谓集群管理就是：是否有机器退出和加入、选举 master。

**集群管理主要指集群监控和集群控制两个方面。前者侧重于集群运行时的状态的收集，后者则是对集群进行操作与控制。**开发和运维中，面对集群，经常有如下需求:

1. 希望知道集群中究竟有多少机器在工作
2. 对集群中的每台机器的运行时状态进行数据收集
3. 对集群中机器进行上下线的操作

集群管理结构图如下所示：

<img src="https://dream-syz.github.io/10-4.png" style="zoom:80%;" />

1、分布式环境中，实时掌握每个节点的状态是必要的，可根据节点实时状态做出一些调整。

2、可交由 ZooKeeper 实现。

- 可将节点信息写入 ZooKeeper 上的一个 Znode。

- 监听这个 Znode 可获取它的实时状态变化。

3、典型应用 

- Hbase 中 Master 状态监控与选举。

利用 ZooKeeper 的强一致性，能够保证在分布式高并发情况下节点创建的全局唯一性，即：同时有多个客户端请求创建 /currentMaster 节点，最终一定只有一个客户端请求能够创建成功

#### **分布式通知与协调**

1、分布式环境中，经常存在一个服务需要知道它所管理的子服务的状态。

a）NameNode 需知道各个 Datanode 的状态。

b）JobTracker 需知道各个 TaskTracker 的状态。

2、心跳检测机制可通过 ZooKeeper 来实现。

3、信息推送可由 ZooKeeper 来实现，ZooKeeper 相当于一个发布/订阅系统。

#### **分布式锁**

处于不同节点上不同的服务，它们可能需要顺序的访问一些资源，这里需要一把分布式的锁。

分布式锁具有以下特性：写锁、读锁、时序锁。

**写锁**：在 zk 上创建的一个临时的无编号的节点。由于是无序编号，在创建时不会自动编号，导致只能客户端有一个客户端得到锁，然后进行写入。

**读锁**：在 zk 上创建一个临时的有编号的节点，这样即使下次有客户端加入是同时创建相同的节点时，他也会自动编号，也可以获得锁对象，然后对其进行读取。

**时序锁**：在 zk 上创建的一个临时的有编号的节点根据编号的大小控制锁。

#### **分布式队列**

##### 分布式队列分为两种：

1、当一个队列的成员都聚齐时，这个队列才可用，否则一直等待所有成员到达，这种是同步队列。

a）一个 job 由多个 task 组成，只有所有任务完成后，job 才运行完成。

b）可为 job 创建一个 /job 目录，然后在该目录下，为每个完成的 task 创建一个临时的 Znode，一旦临时节点数目达到 task 总数，则表明 job 运行完成。

2、队列按照 FIFO 方式进行入队和出队操作，例如实现生产者和消费者模型。



### 3、说说 Zookeeper 的工作原理？

Zookeeper 的 **核心是原子广播**，这个机制保证了各个 Server 之间的同步。实现这个机制的协议叫做 Zab 协议。Zab 协议有两种模式，它们分别是恢复模式（选主）和广播模式（同步）。

Zab 协议 的全称是 Zookeeper Atomic Broadcast（Zookeeper 原子广播）。Zookeeper 是通过Zab 协议来保证分布式事务的最终一致性。Zab协议要求每个 Leader 都要经历三个阶段：**发现，同步，广播**。

当服务启动或者在领导者崩溃后，Zab 就进入了恢复模式，当领导者被选举出来，且大多数 Server 完成了和 leader 的状态同步以后，恢复模式就结束了。状态同步保证了 leader 和 Server 具有相同的系统状态。

为了保证事务的顺序一致性，zookeeper 采用了递增的事务 id 号（zxid）来标识事务。所有的提议（proposal）都在被提出的时候加上了 zxid。实现中 zxid 是一个 64 位的数字，它高 32 位是 epoch 用来标识 leader 关系是否改变，每次一个 leader 被选出来，它都会有一 个新的 epoch，标识当前属于那个 leader 的统治时期。低 32 位用于递增计数。

epoch：可以理解为皇帝的年号，当新的皇帝 leader 产生后，将有一个新的 epoch 年号。

每个 Server 在工作过程中有四种状态：

- LOOKING：当前 Server 不知道 leader 是谁，正在搜寻。

- LEADING：当前 Server 即为选举出来的 leader。

- FOLLOWING：leader 已经选举出来，当前 Server 与之同步
- OBSERVING：观察者状态



### 4、请描述一下 Zookeeper 的通知机制是什么？

Zookeeper 允许客户端向服务端的某个 znode 注册一个 Watcher 监听，当服务端的一些指定事件触发了这个 Watcher ，服务端会向指定客户端发送一个事件通知来实现分布式的通知功能，然后客户端根据 Watcher 通知状态和事件类型做出业务上的改变。

大致分为三个步骤：

- **客户端注册 Watcher**

1、调用 getData、getChildren、exist 三个 API ，传入Watcher 对象。

2、标记请求 request ，封装 Watcher 到 WatchRegistration 。

3、封装成 Packet 对象，发服务端发送 request 。

4、收到服务端响应后，将 Watcher 注册到 ZKWatcherManager 中进行管理。

5、请求返回，完成注册。

- **服务端处理 Watcher**

1、服务端接收 Watcher 并存储。 

2、Watcher 触发 

3、调用 process 方法来触发 Watcher 。

- **客户端回调 Watcher**

1，客户端 SendThread 线程接收事件通知，交由 EventThread 线程回调Watcher 。 

2，客户端的 Watcher 机制同样是一次性的，一旦被触发后，该 Watcher 就失效了。

> client 端会对某个 znode 建立一个 watcher 事件，当该 znode 发生变化时，这些 client 会收到 zk 的通知，然后 client 可以根据 znode 变化来做出业务上的改变等。
>



### 5、Zookeeper 对节点的 **watch** **监听通知是永久的吗？**

不是，是**一次性 **的。无论是服务端还是客户端，一旦一个 Watcher 被触发， Zookeeper 都会将其从相应的存储中移除。这样的设计有效的减轻了服务端的压力，不然对于更新非常频繁的节点，服务端会不断的向客户端发送事件通知，无论对于网络还是服务端的压力都非常大。



### 6、Zookeeper 集群中有哪些角色？

![](https://dream-syz.github.io/10-5.png)

在一个集群中，最少需要 3 台。或者保证 2N + 1 台，即奇数。为什么保证奇数？主要是为了选举算法。



### 7、Zookeeper 集群中 Server 有哪些工作状态？

**LOOKING**

寻找 Leader 状态；当服务器处于该状态时，它会认为当前集群中没有 Leader ，因此需要进入 Leader 选举状态

**FOLLOWING**

跟随者状态；表明当前服务器角色是 Follower

**LEADING**

领导者状态；表明当前服务器角色是 Leader

**OBSERVING**

观察者状态；表明当前服务器角色是 Observer



### 8、Zookeeper 集群中是怎样选举 leader 的？

当 Leader 崩溃了，或者失去了大多数的 Follower，这时候 Zookeeper 就进入 **恢复模式**，恢复模式需要重新选举出一个新的 Leader，让所有的 Server 都恢复到一个状态 **LOOKING** 。

Zookeeper 有两种选举算法：基于 basic paxos 实现和基于 fast paxos 实现。默认为 fast paxos

由于篇幅问题，这里推荐：选举流程



### 9、 Zookeeper 是如何保证事务的顺序一致性的呢？

Zookeeper 采用了递增的事务 id 来识别，所有的 proposal （提议）都在被提出的时候加上了 zxid 。 zxid 实际上是一个 64 位数字。

高 32 位是 epoch 用来标识 Leader 是否发生了改变，如果有新的 Leader 产生出来， epoch 会自增。 低 32 位用来递增计数。 当新产生的 proposal 的时候，会依据数据库的两阶段过程，首先会向其他的 Server 发出事务执行请求，如果超过半数的机器都能执行并且能够成功，那么就会开始执行。



### 10、ZooKeepe **集群中个服务器之间是怎样通信的？**

Leader 服务器会和每一个 Follower / Observer 服务器都建立 TCP 连接，同时为每个 Follower / Observer 都创建一个叫做 LearnerHandler 的实体。

- LearnerHandler 主要负责 Leader 和 Follower / Observer 之间的网络通讯，包括数据同步，请求转发和 proposal 提议的投票等。

- Leader 服务器保存了所有 Follower / Observer 的 LearnerHandler 。



### **11、 ZooKeeper** 分布式锁怎么实现的？

如果有客户端 1、客户端 2 等 N 个客户端争抢一个 Zookeeper 分布式锁。大致如下：

1. 大家都是上来直接创建一个锁节点下的一个接一个的临时有序节点
2. 如果自己不是第一个节点，就对自己上一个节点加监听器
3. 只要上一个节点释放锁，自己就排到前面去了，相当于是一个排队机制。

而且用临时顺序节点的另外一个用意就是，如果某个客户端创建临时顺序节点之后，不小心自己宕机了也没关系， Zookeeper 感知到那个客户端宕机，会自动删除对应的临时顺序节点，相当于自动释放锁，或者是自动取消自己的排队。

本地锁，可以用 JDK 实现，但是分布式锁就必须要用到分布式的组件。比如 ZooKeeper、Redis。

网上代码一大段，面试一般也不要写，我这说一些关键点。

几个需要注意的地方如下。

- 死锁问题：锁不能因为意外就变成死锁，所以要用 ZK 的临时节点，客户端连接失效了，锁就自动释放了。

- 锁等待问题：锁有排队的需求，所以要 ZK 的顺序节点。

- 锁管理问题：一个使用释放了锁，需要通知其他使用者，所以需要用到监听。

- 监听的羊群效应：比如有 1000 个锁竞争者，锁释放了，1000 个竞争者就得到了通知，然后判断，最终序号最小的那个拿到了锁。其它 999 个竞争者重新注册监听。这就是羊群效应，出点事，就会惊动整个羊群。应该每个竞争者只监听自己前面的那个节点。比如 2 号释放了锁，那么只有 3 号得到了通知。



### 12、了解 Zookeeper 的系统架构吗？

![](https://dream-syz.github.io/10-6.png)

ZooKeeper 的架构图中我们需要了解和掌握的主要有：

（1）ZooKeeper分为服务器端（Server） 和客户端（Client），客户端可以连接到整个 ZooKeeper 服务的任意服务器上（除非 leaderServes 参数被显式设置， leader 不允许接受客户端连接）。

（2）客户端使用并维护一个 TCP 连接，通过这个连接发送请求、接受响应、获取观察的事件以及发送心跳。如果这个 TCP 连接中断，客户端将自动尝试连接到另外的 ZooKeeper 服务器。客户端第一次连接到 ZooKeeper 服务时，接受这个连接的 ZooKeeper 服务器会为这个客户端建立一个会话。当这个客户端连接到另外的服务器时，这个会话会被新的服务器重新建立。

（3）上图中每一个 Server 代表一个安装 Zookeeper 服务的机器，即是整个提供 Zookeeper 服务的集群（或者是由伪集群组成）；

（4）组成 ZooKeeper 服务的服务器必须彼此了解。它们维护一个内存中的状态图像，以及持久存储中的事务日志和快照， 只要大多数服务器可用，ZooKeeper 服务就可用；

（5）ZooKeeper 启动时，将从实例中选举一个 leader，Leader 负责处理数据更新等操作，一个更新操作成功的标志是当且仅当大多数Server 在内存中成功修改数据。每个 Server 在内存中存储了一份数据。

（6）Zookeeper 是可以集群复制的，集群间通过 Zab 协议（Zookeeper Atomic Broadcast）来保持数据的一致性；

（7）Zab协议包含两个阶段：leader election 阶段和 Atomic Brodcast 阶段。

- a) 集群中将选举出一个 leader，其他的机器则称为 follower，所有的写操作都被传送给 leader，并通过 brodcast 将所有的更新告诉给follower。

- b) 当 leader 崩溃或者 leader 失去大多数的 follower 时，需要重新选举出一个新的 leader，让所有的服务器都恢复到一个正确的状态。

- c) 当 leader 被选举出来，且大多数服务器完成了 和 leader 的状态同步后，leadder election 的过程就结束了，就将会进入到 Atomic brodcast 的过程。

- d) Atomic Brodcast 同步 leader 和 follower 之间的信息，保证 leader 和 follower 具有形同的系统状态。



### 13、Zookeeper 为什么要这么设计？

ZooKeeper 设计的目的是提供高性能、高可用、顺序一致性的分布式协调服务、保证数据最终一致性。

**高性能（简单的数据模型）**

1. 采用树形结构组织数据节点；
2. 全量数据节点，都存储在内存中；
3. Follower 和 Observer 直接处理非事务请求；

**高可用（构建集群）**

1. 半数以上机器存活，服务就能正常运行
2. 自动进行 Leader 选举

**顺序一致性（事务操作的顺序）**

1. 每个事务请求，都会转发给 Leader 处理
2. 每个事务，会分配全局唯一的递增 id（zxid，64位：epoch + 自增 id）

**最终一致性**

1. 通过提议投票方式，保证事务提交的可靠性
2. 提议投票方式，只能保证 Client 收到事务提交成功后，半数以上节点能够看到最新数据



### 14、你知道 Zookeeper 中有哪些角色？

#### 系统模型：

![](https://dream-syz.github.io/10-7.png)

**领导者（leader）**

Leader 服务器为客户端提供读服务和写服务。负责进行投票的发起和决议，更新系统状态。

**学习者（learner）**

- 跟随者（ follower ） Follower服务器为客户端提供读服务，参与Leader选举过程，参与写操作“过半写成功”策略。

- 观察者（ observer ） Observer服务器为客户端提供读服务，不参与Leader选举过程，不参与写操作“过半写成功”策略。用于在不影响写性能的前提下提升集群的读性能。

**客户端（client）** 

服务请求发起方。



### 15、你熟悉 Zookeeper 节点 ZNode 和相关属性吗？

**节点有哪些类型？**

Znode 两种类型 ：

- 持久的（persistent）：客户端和服务器端断开连接后，创建的节点不删除（默认）。

- 短暂的（ephemeral）：客户端和服务器端断开连接后，创建的节点自己删除。

Znode 有四种形式 ：

- 持久化目录节点（PERSISTENT）：客户端与 Zookeeper 断开连接后，该节点依旧存在持久化顺序编号目录节点（PERSISTENT_SEQUENTIAL）

- 客户端与 Zookeeper 断开连接后，该节点依旧存在，只是 Zookeeper 给该节点名称进行顺序编号：临时目录节点（EPHEMERAL）

- 客户端与 Zookeeper 断开连接后，该节点被删除：临时顺序编号目录节点（EPHEMERAL_SEQUENTIAL）

- 客户端与 Zookeeper 断开连接后，该节点被删除，只是 Zookeeper 给该节点名称进行顺序编号

**「注意」**：创建vZNodev时设置顺序标识，ZNodev名称后会附加一个值，顺序号是一个单调递增的计数器，由父节点维护。

**节点属性有哪些**

一个 znode 节点不仅可以存储数据，还有一些其他特别的属性。接下来我们创建一个 /test 节点分析一下它各个属性的含义。

```
 [zk: localhost:2181(CONNECTED) 6] get /test
 456
 cZxid = 0x59ac //
 ctime = Mon Mar 30 15:20:08 CST 2020
 mZxid = 0x59ad
 mtime = Mon Mar 30 15:22:25 CST 2020
 pZxid = 0x59ac
 cversion = 0
 dataVersion = 2
 aclVersion = 0
 ephemeralOwner = 0x0
 dataLength = 3
 numChildren = 0
```

属性说明：

| 节点属性         | 注解                                                         |
| ---------------- | ------------------------------------------------------------ |
| cZxid            | 该数据节点被创建时的事务 id                                  |
| mZxid            | 该数据节点被修改时最新的事务 id                              |
| pZxid            | 当前节点的父级节点事务 id                                    |
| cTime            | 该数据节点创建时间                                           |
| mTime            | 该数据节点最后修改时间                                       |
| dataVersion      | 当前节点版本号（每修改一次值 + 1 递增）                      |
| cVersion         | 子节点版本号（子节点修改次数，每修改一次值 + 1 递增）        |
| aclVersion       | 当前节点 acl 版本号（节点被修改 acl 权限，每修改一次值 + 1 递增） |
| ephemeral())wner | 临时节点标识，当前节点如果是临时节点，则存储的创建者的会话 id(sessionl，如果不是，那么值 = 0 |
| dataLength       | 当前节点所存储的数据长度                                     |
| numChildren      | 当前节点下子节点的个数                                       |



### 16、请简述 Zookeeper 的选主流程

Zookeeper 的核心是原子广播，这个机制保证了各个 Server 之间的同步。实现这个机制的协议叫做 Zab 协议。Zab 协议有两种模式，它们分别是恢复模式（选主）和广播模式（同步）。当服务启动或者在领导者崩溃后，Zab 就进入了恢复模式，当领导者被选举出来，且大多数 Serve r完成了和 leader 的状态同步以后，恢复模式就结束了。状态同步保证了 leader 和 Server 具有相同的系统状态。leader 选举是保证分布式数据一致性的关键。

出现选举主要是两种场景：初始化、leader 不可用。

当 zk 集群中的一台服务器出现以下两种情况之一时，就会开始 leader 选举。

（1）服务器初始化启动。

（2）服务器运行期间无法和 leader 保持连接。

而当一台机器进入 leader 选举流程时，当前集群也可能处于以下两种状态。

（1）集群中本来就已经存在一个 leader。

（2）集群中确实不存在 leader。

首先第一种情况，通常是集群中某一台机器启动比较晚，在它启动之前，集群已经正常工作，即已经存在一台 leader 服务器。当该机器试图去选举 leader 时，会被告知当前服务器的 leader 信息，它仅仅需要和 leader 机器建立连接，并进行状态同步即可。

重点是 leader 不可用了，此时的选主制度。

投票信息中包含两个最基本的信息。

sid ：即 server id，用来标识该机器在集群中的机器序号。

zxid ：即 zookeeper 事务 id 号。

ZooKeeper 状态的每一次改变, 都对应着一个递增的 Transaction id,，该 id 称为 zxid.，由于 zxid 的递增性质, 如果 zxid 1 小于 zxid 2，那么 zxid 1肯定先于 zxid 2 发生。创建任意节点，或者更新任意节点的数据， 或者删除任意节点，都会导致 Zookeeper 状态发生改变，从而导致 zxid 的值增加。

以（sid，zxid）的形式来标识一次投票信息。

例如：如果当前服务器要推举 sid 为 1，zxid 为 8 的服务器成为 leader，那么投票信息可以表示为（1，8）集群中的每台机器发出自己的投票后，也会接受来自集群中其他机器的投票。每台机器都会根据一定的规则，来处理收到的其他机器的投票，以此来决定是否需要变更自己的投票。

规则如下 ：

（1）初始阶段，都会给自己投票。

（2）当接收到来自其他服务器的投票时，都需要将别人的投票和自己的投票进行pk，规则如下：

优先检查 zxid。zxid 比较大的服务器优先作为 leader。如果 zxid 相同的话，就比较 sid，sid 比较大的服务器作为 leader。

**所有服务启动时候的选举流程**：

三台服务器 server 1、server 2、server 3：

1. server 1 启动，一台机器不会选举。
2. server 2 启动，server 1 和 server 2 的状态改为 looking，广播投票
3. server 3 启动，状态改为 looking，加入广播投票。
4. 初识状态，互不认识，大家都认为自己是王者，投票也投自己为 Leader。
5. 投票信息说明，票信息本来为五元组，这里为了逻辑清晰，简化下表达。

初识 zxid = 0，sid 是每个节点的名字，这个 sid 在 zoo.cfg 中配置，不会重复。

| **节点** | **sid** |
| -------- | ------- |
| server 1 | 1       |
| server 2 | 2       |
| server 3 | 3       |

1. 初始 zxid = 0，server 1 投票（1，0），server 2 投票（2，0），server 3 投票（3，0）
2. server 1 收到 投票（2，0）时，会先验证投票的合法性，然后自己的票进行 pk，pk 的逻辑是先比较 zxid，server 1（zxid）=server 2（zxid）=0，zxid 相等再比较 sid，server 1（sid）< server 2(sid)，pk 结果为 server 2 的投票获胜。server 1 更新自己的投票为 （2，0），server 1 重新投票。

3. TODO 这里最终是 2 还是 3，需要做实验确定。
4. server 2 收到 server 1 投票，会先验证投票的合法性，然后 pk，自己的票获胜，server 不用更新自己的票，pk 后，重新在发送一次投票。

5. 统计投票，pk 后会统计投票，如果半数以上的节点投出相同的票，确定选出了 Leader。
6. 选举结束，被选中节点的状态由 LOOKING 变成 LEADING，其他参加选举的节点由 LOOKING 变成 FOLLOWING。如果有 Observer 节点，如果 Observer 不参与选举，所以选举前后它的状态一直是 OBSERVING，没有变化。

**简单地说**

开始投票 -> 节点状态变成 LOOKING  ->  每个节点选自己 ->  收到票进行 PK -> sid 大的获胜 -> 更新选票 -> 再次投票 -> 统计选票，选票过半数选举结果 -> 节点状态更新为自己的角色状态。



### 17、为什么 Zookeeper 集群的数目，一般为奇数个？

首先需要明确 zookeeper 选举的规则：leader 选举，要求 可用节点数量 > 总节点数量 / 2 。

比如：标记一个写是否成功是要在超过一半节点发送写请求成功时才认为有效。同样，Zookeeper 选择领导者节点也是在超过一半节点同意时才有效。最后，Zookeeper 是否正常是要根据是否超过一半的节点正常才算正常。这是基于 CAP 的一致性原理。

zookeeper 有这样一个特性：集群中只要有过半的机器是正常工作的，那么整个集群对外就是可用的。

也就是说如果有 2 个 zookeeper，那么只要有 1 个死了 zookeeper 就不能用了，因为 1 没有过半，所以 2 个 zookeeper 的死亡容忍度为 0；

同理，要是有 3 个 zookeeper，一个死了，还剩下 2 个正常的，过半了，所以 3 个 zookeeper 的容忍度为 1；

同理：

- 2->0；两个 zookeeper，最多 0 个 zookeeper 可以不可用。

- 3->1；三个 zookeeper，最多 1 个 zookeeper 可以不可用。

- 4->1；四个 zookeeper，最多 1 个 zookeeper 可以不可用。

- 5->2；五个 zookeeper，最多 2 个 zookeeper 可以不可用。

- 6->2；两个 zookeeper，最多 0 个 zookeeper 可以不可用。

....

会发现一个规律，2n 和 2n-1 的容忍度是一样的，都是 n-1，所以为了更加高效，何必增加那一个不必要的 zookeeper 呢。

zookeeper 的选举策略也是需要半数以上的节点同意才能当选 leader，如果是偶数节点可能导致票数相同的情况。



### 18、知道 Zookeeper 监听器的原理吗？

![](https://dream-syz.github.io/10-8.png)

1. 创建一个 Main()  线程。
2. 在 Main()  线程中创建两个线程，一个负责网络连接通信（connect），一个负责监听（listener）。
3.  通过 connect 线程将注册的监听事件发送给 Zookeeper。

4. 将注册的监听事件添加到 Zookeeper 的注册监听器列表中。
5. Zookeeper 监听到有数据或路径发生变化时，把这条消息发送给 Listener 线程。
6. Listener 线程内部调用 process() 方法



### 19、说说 Zookeeper 中的 ACL 权限控制机制

UGO（User/Group/Others）

目前在 Linux / Unix 文件系统中使用，也是使用最广泛的权限控制方式。是一种粗粒度的文件系统权限控制模式。

ACL（Access Control List）访问控制列表

包括三个方面：

权限模式（Scheme）

（1）IP：从 IP 地址粒度进行权限控制

（2）Digest：最常用，用类似于 username:password 的权限标识来进行权限配置，便于区分不同应用来进行权限控制

（3）World：最开放的权限控制方式，是一种特殊的 digest 模式，只有一个权限标识  “world:anyone”

（4）Super：超级用户

授权对象

授权对象指的是权限赋予的用户或一个指定实体，例如 IP 地址或是机器灯。

权限 Permission

（1）CREATE：数据节点创建权限，允许授权对象在该 Znode 下创建子节点

（2）DELETE：子节点删除权限，允许授权对象删除该数据节点的子节点

（3）READ：数据节点的读取权限，允许授权对象访问该数据节点并读取其数据内容或子节点列等

（4）WRITE：数据节点更新权限，允许授权对象对该数据节点进行更新操作

（5）ADMIN：数据节点管理权限，允许授权对象对该数据节点进行 ACL 相关设置操作

​	

### 20、Zookeeper 有哪几种几种部署模式？

Zookeeper 有三种部署模式：

1. 单机部署：一台集群上运行；
2. 集群部署：多台集群运行；
3. 伪集群部署：一台集群启动多个 Zookeeper 实例运行。



### 21、Zookeeper 集群支持动态添加机器吗？

其实就是水平扩容了，Zookeeper 在这方面不太好。两种方式：

全部重启：关闭所有 Zookeeper 服务，修改配置之后启动。不影响之前客户端的会话。

逐个重启：在过半存活即可用的原则下，一台机器重启不影响整个集群对外提供服务。这是比较常用的方式。

3.5 版本开始支持动态扩容。



### 22、描述一下 ZAB 协议

ZAB 协议是 ZooKeeper 自己定义的协议，全名 ZooKeeper 原子广播协议。

**ZAB** **协议有两种模式：**Leader 节点崩溃了如何恢复和消息如何广播到所有节点。

整个 ZooKeeper 集群没有 Leader 节点的时候，属于崩溃的情况。比如集群启动刚刚启动，这时节点们互相不认识。比如运作 Leader 节点宕机了，又或者网络问题，其他节点 Ping 不通 Leader 节点了。这时就需要 ZAB 中的节点崩溃协议，所有节点进入选举模式，选举出新的 Leader。整个选举过程就是通过广播来实现的。选举成功后，一切都需要以 Leader 的数据为准，那么就需要进行数据同步了。



### 23、ZAB 和 Paxos 算法的联系与区别？

相同点：

（1）两者都存在一个类似于 Leader 进程的角色，由其负责协调多个 Follower 进程的运行

（2）Leader 进程都会等待超过半数的 Follower 做出正确的反馈后，才会将一个提案进行提交

（3）ZAB 协议中，每个 Proposal 中都包含一个 epoch 值来代表当前的 Leader周期，Paxos 中名字为 Ballot

不同点：

ZAB 用来构建高可用的分布式数据主备系统（Zookeeper），Paxos 是用来构建分布式一致性状态机系统。



### 24、ZooKeeper 宕机如何处理？

ZooKeeper 本身也是集群，推荐配置奇数个服务器。因为宕机就需要选举，选举需要半数 +1 票才能通过，为了避免打成平手。进来不用偶数个服务器。如果是 Follower 宕机了，没关系不影响任何使用。用户无感知。如果 Leader 宕机，集群就得停止对外服务，开始选举，选举出一个 Leader 节点后，进行数据同步，保证所有节点数据和 Leader 统一，然后开始对外提供服务。为啥投票需要半数 +1，如果半数就可以的话，网络的问题可能导致集群选举出来两个 Leader，各有一半的小弟支持，这样数据也就乱套了。



### 25、 描述一下 ZooKeeper 的 session 管理的思想？

**分桶策略：**

简单地说，就是不同的会话过期可能都有时间间隔，比如 15 秒过期、15.1 秒过期、15.8 秒过期，ZooKeeper 统一让这些 session 16 秒过期。这样非常方便管理，看下面的公式，过期时间总是 ExpirationInterval 的整数倍。

计算公式：

```
ExpirationTime = currentTime + sessionTimeout
ExpirationTime = (ExpirationTime / ExpirationInrerval + 1) * ExpirationInterval ,
```

见图片：

![](https://dream-syz.github.io/10-9.png)

默认配置的 session 超时时间是在 2 tickTime ~ 20  tickTime。



### 26、ZooKeeper 负载均衡和 Nginx 负载均衡有什么区别？

**ZooKeeper：**

- 不存在单点问题，zab 机制保证单点故障可重新选举一个 Leader

- 只负责服务的注册与发现，不负责转发，减少一次数据交换（消费方与服务方直接通信）

- 需要自己实现相应的负载均衡算法

**Nginx：**

- 存在单点问题，单点负载高数据量大，需要通过 KeepAlived 辅助实现高可用

- 每次负载，都充当一次中间人转发角色，本身是个反向代理服务器

- 自带负载均衡算法



### 27、说说 ZooKeeper 的序列化

序列化：

- 内存数据，保存到硬盘需要序列化。

- 内存数据，通过网络传输到其他节点，需要序列化。

ZK 使用的序列化协议是 Jute，Jute 提供了 Record 接口。接口提供了两个方法：

- serialize 序列化方法

- deserialize 反序列化方法

要系列化的方法，在这两个方法中存入到流对象中即可。



### **28、在 Zookeeper 中 Zxid **是什么，有什么作用？

Zxid，也就是事务 id，为了保证事务的顺序一致性，ZooKeeper 采用了递增的事务 Zxid 来标识事务。proposal 都会加上了 Zxid。Zxid 是一个 64 位的数字，它高 32 位是 Epoch 用来标识朝代变化，比如每次选举 Epoch 都会加改变。低 32 位用于递增计数。

Epoch：可以理解为当前集群所处的年代或者周期，每个 Leader 就像皇帝，都有自己的年号，所以每次改朝换代，Leader 变更之后，都会在前一个年代的基础上加 1。这样就算旧的 Leader 崩溃恢复之后，也没有人听它的了，因为 Follower 只听从当前年代的 Leader 的命令。



### 29、讲解一下 ZooKeeper 的持久化机制

**什么是持久化？**

- 数据，存到磁盘或者文件当中。

- 机器重启后，数据不会丢失。内存 -> 磁盘的映射，和序列化有些像。

**ZooKeeper** **的持久化：**

- SnapShot 快照，记录内存中的全量数据

- TxnLog 增量事务日志，记录每一条增删改记录（查不是事务日志，不会引起数据变化）

**为什么持久化这么麻烦，一个不可用吗？**

快照的缺点，文件太大，而且快照文件不会是最新的数据。 增量事务日志的缺点，运行时间长了，

日志太多了，加载太慢。二者结合最好。

**快照模式：**

- 将 ZooKeeper 内存中以 DataTree 数据结构存储的数据定期存储到磁盘中。

- 由于快照文件是定期对数据的全量备份，所以快照文件中数据通常不是最新的。

见图片：

![](https://dream-syz.github.io/10-10.png)



### 30、Zookeeper 选举中投票信息的五元组是什么？

- Leader：被选举的 Leader 的 SID

- Zxid：被选举的 Leader 的事务 ID

- Sid：当前服务器的 SID

- electionEpoch：当前投票的轮次

- peerEpoch：当前服务器的 Epoch

**Epoch > Zxid > Sid**

Epoch，Zxid 都可能一致，但是 Sid 一定不一样，这样两张选票一定会 PK 出结果。



### 31、说说 Zookeeper 中的脑裂？

简单点来说，脑裂（Split-Brain）就是比如当你的 cluster 里面有两个节点，它们都知道在这个 cluster 里需要选举出一个 master。那么当它们两个之间的通信完全没有问题的时候，就会达成共识，选出其中一个作为 master。但是如果它们之间的通信出了问题，那么两个结点都会觉得现在没有 master，所以每个都把自己选举成 master，于是 cluster 里面就会有两个 master。

对于 Zookeeper 来说有一个很重要的问题，就是到底是根据一个什么样的情况来判断一个节点死亡 down 掉了？在分布式系统中这些都是有监控者来判断的，但是监控者也很难判定其他的节点的状态，唯一一个可靠的途径就是心跳，Zookeeper也是使用心跳来判断客户端是否仍然活着。

使用 ZooKeeper 来做 Leader HA 基本都是同样的方式：每个节点都尝试注册一个象征 leader 的临时节点，其他没有注册成功的则成为follower，并且通过 watch 机制监控着 leader 所创建的临时节点，Zookeeper 通过内部心跳机制来确定 leader 的状态，一旦 leader 出现意外 Zookeeper 能很快获悉并且通知其他的 follower，其他 flower 在之后作出相关反应，这样就完成了一个切换，这种模式也是比较通用的模式，基本大部分都是这样实现的。但是这里面有个很严重的问题，如果注意不到会导致短暂的时间内系统出现脑裂，因为心跳出现超时可能是 leader 挂了，但是也可能是 zookeeper 节点之间网络出现了问题，导致 leader 假死的情况，leader 其实并未死掉，但是与 ZooKeeper 之间的网络出现问题导致 Zookeeper 认为其挂掉了然后通知其他节点进行切换，这样 follower 中就有一个成为了leader，但是原本的 leader 并未死掉，这时候 client 也获得 leader 切换的消息，但是仍然会有一些延时，zookeeper 需要通讯需要一个一个通知，这时候整个系统就很混乱可能有一部分 client 已经通知到了连接到新的 leader 上去了，有的 client 仍然连接在老的 leader上，如果同时有两个 client 需要对 leader 的同一个数据更新，并且刚好这两个 client 此刻分别连接在新老的 leader 上，就会出现很严重问题。

这里做下小总结： 

**假死**：由于心跳超时（网络原因导致的）认为 leader 死了，但其实 leader 还存活着。

**脑裂**：由于假死会发起新的 leader 选举，选举出一个新的 leader，但旧的 leader 网络又通了，导致出现了两个 leader ，有的客户端连接到老的 leader，而有的客户端则连接到新的 leader。



### 32、Zookeeper 脑裂是什么原因导致的？

主要原因是 Zookeeper 集群和 Zookeeper client 判断超时并不能做到完全同步，也就是说可能一前一后，如果是集群先于 client 发现，那就会出现上面的情况。同时，在发现并切换后通知各个客户端也有先后快慢。一般出现这种情况的几率很小，需要 leader 节点与Zookeeper 集群网络断开，但是与其他集群角色之间的网络没有问题，还要满足上面那些情况，但是一旦出现就会引起很严重的后果，数据不一致。



### 33、Zookeeper 是如何解决脑裂问题的？

要解决 Split-Brain 脑裂的问题，一般有下面几种种方法： Quorums (法定人数) 方式：比如 3 个节点的集群，Quorums = 2, 也就是说集群可以容忍 1 个节点失效，这时候还能选举出 1 个 lead，集群还可用。比如 4 个节点的集群，它的 Quorums = 3，Quorums 要超过 3，相当于集群的容忍度还是 1，如果 2 个节点失效，那么整个集群还是无效的。这是 zookeeper 防止"脑裂"默认采用的方法。

采用 Redundant communications （冗余通信）方式：集群中采用多种通信方式，防止一种通信方式失效导致集群中的节点无法通信。

Fencing (共享资源) 方式：比如能看到共享资源就表示在集群中，能够获得共享资源的锁的就是 Leader，看不到共享资源的，就不在集群中。

要想避免 zookeeper "脑裂"情况其实也很简单，在 follower 节点切换的时候不在检查到老的 leader 节点出现问题后马上切换，而是在休眠一段足够的时间，确保老的 leader 已经获知变更并且做了相关的 shutdown 清理工作了然后再注册成为 master 就能避免这类问题了，这个休眠时间一般定义为与 zookeeper 定义的超时时间就够了，但是这段时间内系统可能是不可用的，但是相对于数据不一致的后果来说还是值得的。

1、zooKeeper 默认采用了 Quorums 这种方式来防止"脑裂"现象。即只有集群中超过半数节点投票、才能选举出 Leader。这样的方式可以确保 leader 的唯一性,要么选出唯一的一个 leader，要么选举失败。在 zookeeper 中 Quorums 作用如下：

- 集群中最少的节点数用来选举 leader 保证集群可用。

- 通知客户端数据已经安全保存前集群中最少数量的节点数已经保存了该数据。一旦这些节点保存了该数据，客户端将被通知已经安全保存了，可以继续其他任务。而集群中剩余的节点将会最终也保存了该数据。

假设某个 leader 假死，其余的 followers 选举出了一个新的 leader。这时，旧的 leader 复活并且仍然认为自己是 leader，这个时候它向其他 followers 发出写请求也是会被拒绝的。因为每当新 leader 产生时，会生成一个 epoch 标号（标识当前属于那个leader的统治时期），这个 epoch 是递增的，followers 如果确认了新的 leader 存在，知道其 epoch，就会拒绝 epoch 小于现任 leader epoch 的所有请求。那有没有 follower 不知道新的leader存在呢，有可能，但肯定不是大多数，否则新 leader 无法产生。Zookeeper 的写也遵循quorum 机制，因此，得不到大多数支持的写是无效的，旧 leader 即使各种认为自己是 leader，依然没有什么作用。

zookeeper 除了可以采用上面默认的 Quorums 方式来避免出现"脑裂"，还可以可采用下面的预防措施：

2、添加冗余的心跳线，例如双线条线，尽量减少“裂脑”发生机会。 

3、启用磁盘锁。正在服务一方锁住共享磁盘，"裂脑"发生时，让对方完全"抢不走"共享磁盘资源。但使用锁磁盘也会有一个不小的问题，如果占用共享盘的一方不主动"解锁"，另一方就永远得不到共享磁盘。现实中假如服务节点突然死机或崩溃，就不可能执行解锁命令。后备节点也就接管不了共享资源和应用服务。于是有人在HA中设计了"智能"锁。即正在服务的一方只在发现心跳线全部断开（察觉不到对端）时才启用磁盘锁。平时就不上锁了。 

4、设置仲裁机制。例如设置参考 IP（如网关IP），当心跳线完全断开时，2 个节点都各自 ping 一下参考 IP，不通则表明断点就出在本端，不仅"心跳"、还兼对外"服务"的本端网络链路断了，即使启动（或继续）应用服务也没有用了，那就主动放弃竞争，让能够 ping 通参考 IP 的一端去起服务。更保险一些，ping 不通参考IP的一方干脆就自我重启，以彻底释放有可能还占用着的那些共享资源。



### 34、说说 Zookeeper 的 CAP 问题上做的取舍？

**一致性 C：**Zookeeper 是强一致性系统，为了保证较强的可用性，“一半以上成功即成功”的数据同步方式可能会导致部分节点的数据不一致。所以 Zookeeper 还提供了 sync() 操作来做所有节点的数据同步，这就关于 C 和 A 的选择问题交给了用户，因为使用 sync() 势必会延长同步时间，可用性会有一些损失。

**可用性 A ：**Zookeeper 数据存储在内存中，且各个节点都可以相应读请求，具有好的响应性能。Zookeeper 保证了数据总是可用的，没有锁。并且有一大半的节点所拥有的数据是最新的。

**分区容忍性 P：**Follower 节点过多会导致增大数据同步的延时（需要半数以上 follower 写完提交）。同时选举过程的收敛速度会变慢，可用性降低。Zookeeper 通过引入 observer 节点缓解了这个问题，增加 observer 节点后集群可接受 client 请求的节点多了，而且 observer 不参与投票，可以提高可用性和扩展性，但是节点多数据同步总归是个问题，所以一致性会有所降低。



### 35、watch 监听为什么是一次性的？

如果服务端变动频繁，而监听的客户端很多情况下，每次变动都要通知到所有的客户端，给网络和服务器造成很大压力。

一般是客户端执行 getData（节点 A,true) ，如果节点 A 发生了变更或删除，客户端会得到它的 watch 事件，但是在之后节点 A 又发生了变更，而客户端又没有设置 watch 事件，就不再给客户端发送。

在实际应用中，很多情况下，我们的客户端不需要知道服务端的每一次变动，我只要最新的数据即可。





## Redis 篇

### 1、Redis 为什么快

1. C 语言实现，效率高
2. 纯内存操作
3. 基于非阻塞的 IO 复用模型机制
4. 单线程的话就能避免多线程的频繁上下文切换问题
5. 丰富的数据结构（全称采用 hash 结构，读取速度非常快，对数据存储进行了一些优化，比如亚索表，跳表等）

##### 什么是 Redis?

Redis 是一个开源（BSD 许可）、基于内存、支持多种数据结构的存储系统，可以作为数据库、缓存和消息中间件。它支持的数据结构有字符串（strings）、哈希（hashes）、列表（lists）、集合（sets）、有序集合（sorted sets）等，除此之外还支持 bitmaps、hyperloglogs 和地理空间（geospatial ）索引半径查询等功能。

它内置了复制（Replication）、LUA 脚本（Lua scripting）、LRU 驱动事件（LRU eviction）、事务（Transactions）和不同级别的磁盘持久化（persistence）功能，并通过 Redis 哨兵（哨兵）和集群（Cluster）保证缓存的高可用性（High availability）。



**跳表**：将有序链表改造为支持近似“折半查找”算法，可以进行快速的插入、删除、查找操作

1. **纯内存 KV 操作**

Redis的操作都是基于内存的，CPU不是 Redis 性能瓶颈,，Redis 的瓶颈是机器内存和网络带宽。

在计算机的世界中，CPU 的速度是远大于内存的速度的，同时内存的速度也是远大于硬盘的速度。Redis 的操作都是基于内存的，绝大部分请求是纯粹的内存操作，非常迅速。

2. **单线程操作**

使用单线程可以省去多线程时 CPU 上下文会切换的时间，也不用去考虑各种锁的问题，不存在加锁释放锁操作，没有死锁问题导致的性能消耗。对于内存系统来说，多次读写都是在一个 CPU 上，没有上下文切换效率就是最高的！既然单线程容易实现，而且 CPU 不会成为瓶颈，那就顺理成章的采用单线程的方案了

Redis 单线程指的是网络请求模块使用了一个线程，即一个线程处理所有网络请求，其他模块该使用多线程，仍会使用了多个线程。

3.  **I/O 多路复用**

为什么 Redis 中要使用 I/O 多路复用这种技术呢？

首先，Redis 是跑在单线程中的，所有的操作都是按照顺序线性执行的，但是由于读写操作等待用户输入或输出都是阻塞的，所以 I/O 操作在一般情况下往往不能直接返回，这会导致某一文件的 I/O 阻塞导致整个进程无法对其它客户提供服务，而 I/O 多路复用就是为了解决这个问题而出现的

4. **Reactor 设计模式**

Redis基于Reactor模式开发了自己的网络事件处理器，称之为文件事件处理器(File Event Hanlder)。

- 纯内存访问，不需要访问磁盘

- 单线程避免上下文切换

- 渐进式 ReHash、缓存时间戳

  为了实现从键到值的快速访问，Redis 使用了一个哈希表来保存所有键值对。一个哈希表，其实就是一个数组，数组的每个元素称为一个哈希桶。所以，我们常说，一个哈希表是由多个哈希桶组成的，每个哈希桶中保存了键值对数据。

  **Key：**一般是 String 类型

  **Value：**基本数据类型 String、List、Hash、Set、ZSet

  **高级数据类型（待整理）**

  Redis 解决哈希冲突的方式，就是链式哈希。链式哈希也很容易理解，就是指同一个哈希桶中的多个元素用一个链表来保存，它们之间依次用指针连接。
  当然如果这个数组一直不变，那么 hash 冲突会变很多，这个时候检索效率会大打折扣，所以 Redis 就需要把数组进行扩容(一般是扩大到原来的两倍)，但是问题来了，扩容后每个 hash 桶的数据会分散到不同的位置，这里设计到元素的移动，必定会阻塞 IO，所以这个 ReHash 过程会导致很多请求阻塞。

  **为了避免这个问题，Redis 采用了渐进式rehash。**
  首先、Redis 默认使用了两个全局哈希表：哈希表 1 和哈希表 2。一开始，当你刚插入数据时，默认使用哈希表 1，此时的哈希表 2 并没有被分配空间。随着数据逐步增多，Redis 开始执行 rehash。

  1、给哈希表 2 分配更大的空间，例如是当前哈希表 1 大小的两倍

  2、把哈希表 1中的数据重新映射并拷贝到哈希表 2 中

  3、释放哈希表 1的空间

  在上面的第二步涉及大量的数据拷贝，如果一次性把哈希表 1 中的数据都迁移完，会造成 Redis 线程阻塞，无法服务其他请求。此时，Redis 就无法快速访问数据了。

  在Redis 开始执行 rehash，Redis 仍然正常处理客户端请求，但是要加入一个额外的处理：处理第1个请求时，把哈希表 1 中的第 1 个索引位置上的所有 entries 拷贝到哈希表 2 中处理第 2 个请求时，把哈希表 1 中的第 2 个索引位置上的所有 entries 拷贝到哈希表 2中如此循环，直到把所有的索引位置的数据都拷贝到哈希表 2 中。
  这样就巧妙地把一次性大量拷贝的开销，分摊到了多次处理请求的过程中，避免了耗时操作，保证了数据的快速访问。
  所以这里基本上也可以确保根据 key 找 value 的操作在O (1)左右。
  不过这里要注意，如果 Redis 中有海量的 key 值的话，这个Rehash过程会很长很长，虽然采用渐进式 Rehash，但在Rehash的过程中还是会导致请求有不小的卡顿。并且像一些统计命令也会非常卡顿: 比如 keys
  按照 Redis 的配置每个实例能存储的最大的 key 的数量为 2 的 32 次方,即 2.5 亿，但是尽量把 key 的数量控制在千万以下，这样就可以避免 Rehash 导致的卡顿问题，如果数量确实比较多，建议采用分区 hash 存储。

  **为什么 Redis 要缓存系统时间戳**

  我们平时使用系统时间截时，常常是不假思索地使用 System.currentTimelnMillis 或者 time.time() 来获取系统的毫秒时间戳。Redis 不能这样，因为每一次获取系统时间截都是一次系统调用，系统调用相对来说是比较费时间的，作为单线程的 Redis 承受不起，所以它需要对时间进行缓存，由一个定时任务，每毫秒更新一次时间缓存，获取时间都是从缓存中直接拿.

  

### 2、Redis 合适的应用场景

- 缓存
- 计数器
- 分布式会话
- 排行榜
- 最新列表
- 分布式锁
- 消息队列

1、会话缓存（Session Cache）最常用的一种使用 Redis 的情景是会话缓存（session cache），用 Redis 缓存会话比其他存储（如Memcached）的优势在于：Redis 提供持久化。当维护一个不是严格要求一致性的缓存时，如果用户的购物车信息全部丢失，大部分人都会不高兴的，现在，他们还会这样吗？幸运的是，随着 Redis 这些年的改进，很容易找到怎么恰当的使用 Redis 来缓存会话的文档。甚至广为人知的商业平台 Magento 也提供 Redis 的插件。

2、全页缓存（FPC）除基本的会话 token 之外，Redis 还提供很简便的 FPC 平台。回到一致性问题，即使重启了 Redis 实例，因为有磁盘的持久化，用户也不会看到页面加载速度的下降，这是一个极大改进，类似 PHP 本地 FPC。再次以 Magento 为例，Magento 提供一个插件来使用 Redis 作为全页缓存后端。此外，对 WordPress 的用户来说，Pantheon 有一个非常好的插件 wp-redis，这个插件能帮助你以最快速度加载你曾浏览过的页面。

3、队列 Redis 在内存存储引擎领域的一大优点是提供 list 和 set 操作，这使得 Redis 能作为一个很好的消息队列平台来使用。Redis 作为队列使用的操作，就类似于本地程序语言（如 Python）对 list 的 push/pop操作。如果你快速的在 Google 中搜索“Redis queues”，你马上就能找到大量的开源项目，这些项目的目的就是利用 Redis 创建非常好的后端工具，以满足各种队列需求。例如，Celery 有一个后台就是使用Redis 作为 broker，你可以从这里去查看。

4、排行榜/计数器Redis 在内存中对数字进行递增或递减的操作实现的非常好。集合（Set）和有序集合（Sorted Set）也使得我们在执行这些操作的时候变的非常简单，Redis 只是正好提供了这两种数据结构。微信搜索公众号：Java专栏，获取最新面试手册所以，我们要从排序集合中获取到排名最靠前的 10 个用户–我们称之为“user_scores”，我们只需要像下面一样执行即可：当然，这是假定你是根据你用户的分数做递增的排序。如果你想返回用户及用户的分数，你需要这样执行：ZRANGE user_scores 0 10 WITHSCORESAgora Games 就是一个很好的例子，用 Ruby 实现的，它的排行榜就是使用 Redis 来存储数据的，你可以在这里看到。

5、发布/订阅最后（但肯定不是最不重要的）是 Redis 的发布/订阅功能。发布/订阅的使用场景确实非常多。我已看见人们在社交网络连接中使用，还可作为基于发布/订阅的脚本触发器，甚至用 Redis 的发布/订阅功能来建立聊天系统！

https://blog.csdn.net/MinggeQingchun/article/details/130533914

https://blog.51cto.com/u_14402/6371239



### 3、Redis 6.0 之前为什么一直不使用多线程？

- 使用 Redis、CPU 不是瓶颈，受制于内存、网络

- 提高 Redis，Pipeline (命令批量) 每秒 100 万个请求

- 单线程，内部维护比较低

- 如果是多线程（线程切换、加锁\解锁、导致死锁问题）
- 惰性 ReHash（渐进式的 ReHash）

一般的公司，单线程 Redis 就够了

官方曾做过类似问题的回复：使用 Redis 时，几乎不存在 CPU 成为瓶颈的情况，

Redis主要受限于内存和网络。例如在一个普通的 Linux 系统上，Redis通过使用 pipelining 每秒可以处理 100 万个请求，所以如果应用程序主要使用O(N)或O(log(N))的命令，它几乎不会占用太多 CPU。

使用了单线程后，可维护性高。多线程模型虽然在某些方面表现优异，但是它却引入了程序执行顺序的不确定性，带来了并发读写的一系列问题，增加了系统复杂度、同时可能存在线程切换、甚至加锁解锁、死锁造成的性能损耗。Redis 通过 AE 事件模型以及 IO 多路复用等技术，处理性能非常高，因此没有必要使用多线程。单线程机制使得 Redis 内部实现的复杂度大大降低，Hash 的惰性 Rehash、Lpush 等等,“线程不安全” 的命令都可以无锁进行。



### 4、Redis 6.0 为什么要引入多线程？

1、单线程就够了。数据->内存 响应时间 100 纳秒，比较小的数据包，8 w~10 w QPS（极限值）

2、大的公司，需要更大 QPS，IO 多线程（内部执行命令还是单线程）多线程任务分摊到 Redis 同步 IO 中读写 负载

3、为什么不采用分布式架构------很大的缺点

服务数量多，维护成本高

Redis 命令不适用数据分区

数据分区，无法解决热点读/写的问题

数据倾斜、重新分配、扩容、缩容，更加复杂



Redis 将所有数据放在内存中，内存的响应时长大约为 100 纳秒，对于小数据包，Redis服务器可以处理 80,000 到 100,000 QPS，这也是Redis处理的极限了，对于 80% 的公司来说，单线程的 Redis 已经足够使用了。

但随着越来越复杂的业务场景，有些公司动不动就上亿的交易量，因此需要更大的 QPS，IO 的多线程（内部执行命令还是单线程）。常见的解决方案是在分布式架构中对数据进行分区并采用多个服务器，但该方案有非常大的缺点，例如要管理的 Redis 服务器太多，维护代价大；某些适用于单个 Redis 服务器的命令不适用于数据分区；数据分区无法解决热点读/写问题；数据偏斜，重新分配和放大/缩小变得更加复杂等等。



### 5、Redis 有哪些高级功能？

消息队列、自动过期删除、事务、数据持久化、分布式锁、附近的人、慢查询分析、Sentinel 和集群等多项功能。



### 6、为什么要用 Redis？

- 高性能

- 高并发

使用缓存的目的就是提升读写性能。而实际业务场景下，更多的是为了提升读性能，带来更好的性能，带来更高的并发量。Redis 的读写性能比 MySQL 好的多，我们就可以把 MySQL 中的热点数据缓存到 Redis 中，提升读取性能，同时也减轻了 MySQL 的读取压力

#### 使用 Redis 有哪些好处？

具有以下好处：

- 读取速度快，因为数据存在内存中，所以数据获取快；

- 支持多种数据结构，包括字符串、列表、集合、有序集合、哈希等；

- 支持事务，且操作遵守原子性，即对数据的操作要么都执行，要么都不支持；

- 还拥有其他丰富的功能，队列、主从复制、集群、数据持久化等功能。

#### Redis 优缺点：

**优点**

- 速度快：因为数据存在内存中，类似于 HashMap ， HashMap 的优势就是查找和操作的时间复杂度都是 O (1) 。

- 支持丰富的数据结构：支持 String ，List，Set，Sorted Set，Hash 五种基础的数据结构。

- 持久化存储：Redis 提供 RDB 和 AOF 两种数据的持久化存储方案，解决内存数据库最担心的万一 Redis 挂掉，数据会消失掉

- 高可用：内置 Redis Sentinel ，提供高可用方案，实现主从故障自动转移。 内置 Redis Cluster ，提供集群方案，实现基于槽的分片方案，从而支持更大的 Redis 规模。

- 丰富的特性：Key 过期、计数、分布式锁、消息队列等。

**缺点**

- 由于 Redis 是内存数据库，所以，单台机器，存储的数据量，跟机器本身的内存大小。虽然 Redis 本身有 Key 过期策略，但是还是需要提前预估和节约内存。如果内存增长过快，需要定期删除数据。

- 如果进行完整重同步，由于需要生成 RDB 文件，并进行传输，会占用主机的 CPU ，并会消耗现网的带宽。不过 Redis 2.8 版本，已经有部分重同步的功能，但是还是有可能有完整重同步的。比如，新上线的备机。

- 修改配置文件，进行重启，将硬盘中的数据加载进内存，时间比较久。在这个过程中， Redis 不能提供服务。



### 7、Redis 与 Memcached 相对有哪些优势？

|              | Redis                                                        | Memcached                                       |
| ------------ | ------------------------------------------------------------ | ----------------------------------------------- |
| 整体类型     | 支持内存、非关系型数据库                                     | 支持内存、key-value                             |
| 数据类型     | String、List、Set、ZSet、Hash                                | 文本型、二进制类型                              |
| 操作类型     | 单个操作、批量操作、事务支持（弱事务、结合 LUA）、每个类型不同的 CRUD | CRUD、少量其命令                                |
| 附加功能     | 发布\订阅、主从高可用（哨兵 故障转移）、序列化支持、支持 LUA 脚本 | 多线程服务支持                                  |
| 网络 IO 模型 | 执行命令-单线程、网络操作-多线程                             | 多线程、非阻塞的 IO 模式                        |
| 持久化       | RDB、AOF                                                     | 纯粹内存，不支持持久化                          |
| 内存管理     | 过期、内存淘汰策略，适合做数据存储                           | 预分配池的管理，内存 slab 块 增长因子，适合缓存 |

1、 memcached 所有的值均是简单的字符串，Redis 作为其替代者，支持更为丰富的数据类型

2、 Redis 的速度比 memcached 快很多

3、 Redis 可以持久化其数据

4、Redis 支持数据的备份，即 master-slave 模式的数据备份。

5、使用底层模型不同，它们之间底层实现方式以及与客户端之间通信的应用协议不一样。Redis直接自己构建了 VM 机制 ，因为一般的系统调用系统函数的话，会浪费一定的时间去移动和请求。

6、value 大小：Redis 最大可以达到 1 GB，而 memcache 只有 1 MB

#### Redis 和 Memcached 的区别与联系：

- Redis 和 Memcache 都是将数据存放在内存中，都是内存数据库。不过 Memcached 还可用于缓存其他东西，例如图片、视频等等。

- Memcached 仅支持 key-value 结构的数据类型，Redis 不仅仅支持简单的 key-value 类型的数据，同时还提供 list，set，hash 等数据结构的存储。

- 虚拟内存– Redis 当物理内存用完时，可以将一些很久没用到的 value 交换到磁盘
- 分布式–设定 Memcached 集群，利用 magent 做一主多从； Redis 可以做一主多从。都可以一主一从

- 存储数据安全– Memcached 挂掉后，数据没了； Redis 可以定期保存到磁盘（持久化）

- Memcached 的单个 value 最大 1 m ， Redis 的单个 value 最大 512 m 。

- 灾难恢复– Memcached 挂掉后，数据不可恢复； Redis 数据丢失后可以通过 aof 恢复

- Redis 原生就支持集群模式， Redis3.0 版本中，官方便能支持 Cluster 模式了， Memcached 没有原生的集群模式，需要依赖客户端来实现，然后往集群中分片写入数据。

- Memcached 网络 IO 模型是多线程，非阻塞 IO 复用的网络模型，原型上接近于 nignx 。而 Redis 使用单线程的IO复用模型，自己封装了一个简单的 AeEvent 事件处理框架，主要实现类 epoll，kqueue 和 select ，更接近于 Apache 早期的模式。



### 8、怎么理解Redis中事务？

事务是一个单独的隔离操作：事务中的所有命令都会序列化、按顺序地执行，事务在执行的过程中，不会被其他客户端发送来的命令请求所打断。事务是一个原子操作：事务中的命令要么全部被执行，要么全部都不执行。不过 Redis 的是弱事物。

事务是 Redis 实现在服务器端的行为，用户执行 MULTI 命令时，服务器会将对应这个用户的客户端对象设置为一个特殊的状态，在这个状态下后续用户执行的查询命令不会被真的执行，而是被服务器缓存起来，直到用户执行 EXEC 命令为止，服务器会将这个用户对应的客户端对象中缓存的命令按照提交的顺序依次执行。

Redis 提供了简单的事务，之所以说它简单，主要是因为它不支持事务中的回滚特性,同时无法实现命令之间的逻辑关系计算，当然也体现了 Redis 的 “keep it simple” 的特性



### 9、Redis的过期策略以及内存淘汰机制？

Redis 采用的是定期删除 + 惰性删除策略。 为什么不用定时删除策略？定时删除，用一个定时器来负责监视 key，过期则自动删除。虽然内存及时释放，但是十分消耗 CPU 资源。在大并发请求下，CPU 要将时间应用在处理请求，而不是删除 key,因此没有采用这一策略. 定期删除+惰性删除是如何工作的呢? 定期删除，Redis 默认每个100 ms 检查，是否有过期的key,有过期 key 则删除。需要说明的是，Redis 不是每个 100 ms 将所有的 key 检查一次，而是随机抽取进行检查(如果每隔 100 ms,全部key进行检查，Redis 岂不是卡死)。因此，如果只采用定期删除策略，会导致很多 key 到时间没有删除。 于是，惰性删除派上用场。也就是说在你获取某个 key 的时候，Redis 会检查一下，这个 key 如果设置了过期时间那么是否过期了？如果过期了此时就会删除

1. noeviction：返回错误当内存限制达到，并且客户端尝试执行会让更多内存被使用的命令。

2. allkeys-lru： 尝试回收最少使用的键（LRU），使得新添加的数据有空间存放。

3. volatile-lru: 尝试回收最少使用的键（LRU），但仅限于在过期集合的键,使得新添加的数据有空间存放。

4. allkeys-random: 回收随机的键使得新添加的数据有空间存放。

5. volatile-random: 回收随机的键使得新添加的数据有空间存放，但仅限于在过期集合的键。

6. volatile-ttl: 回收在过期集合的键，并且优先回收存活时间（TTL）较短的键,使得新添加的数据有空间存放。



### 10、什么是缓存穿透？如何避免？

**缓存穿透：**指查询一个一定不存在的数据，如果从存储层查不到数据则不写入缓存，这将导致这个不存在的数据每次请求都要到 DB 去查询，可能导致 DB 挂掉。

 解决方案：1.查询返回的数据为空，仍把这个空结果进行缓存，但过期时间会比较短；2.布隆过滤器：将所有可能存在的数据哈希到一个足够大的 bitmap 中，一个一定不存在的数据会被这个 bitmap 拦截掉，从而避免了对 DB 的查询。

**布隆过滤器**

- 用来判断一个元素是否在一个集合中（一个二进制数组和一个 Hash 算法）

**误判问题：**

- 通过 hash 计算在数组上不一定在集合

- 本质是 hash 冲突

- 通过 hash 计算不在数组的就一定不在集合

**优化方案：**

- 增大数组（预估合适值）

- 增加 hash 函数



### 11、什么是缓存雪崩？如何避免？

- Redis 故障——解决-集群（主从、多级缓存如 EH Catch、限流组件 hystrix）

- Redis 中大量 key 的 ttl （失效时间）过期——解决：ttl 岔开

缓存雪崩：设置缓存时采用了相同的过期时间，导致缓存在某一时刻同时失效，请求全部转发到 DB，DB 瞬时压力过重雪崩。与缓存击穿的区别：雪崩是很多 key，击穿是某一个key 缓存。

解决方案：将缓存失效时间分散开，比如可以在原有的失效时间基础上增加一个随机值，比如 1-5 分钟随机，这样每一个缓存的过期时间的重复率就会降低，就很难引发集体失效的事件。



### 12、使用 Redis 如何设计分布式锁？

基于 Redis 实现的分布式锁，一个严谨的的流程如下：

1、加锁

SET lock_key $unique_id EX $expire_time NX

2、操作共享资源

3、释放锁：LUA 脚本，先 GET 判断锁是否归属自己，再 DEL 释放锁

**分布式锁加入看门狗**

加锁时，先设置一个过期时间，然后开启一个 [ 守护线程 ] ，定时去检测这个锁的失效时间，如果锁快要过期，操作共享资源还未完成，那么自动对锁进行 [ 续期 ] ，重新设置过期时间。

这个守护线程我们一般也把它叫做 [ 看门狗 ] 线程。



### 13、怎么使用 Redis 实现消息队列？

- **基于 List 的 LPUSH + BRPOP 的实现**

足够简单，消费消息延迟几乎为零，但是需要处理空闲连接的问题。

如果线程一直阻塞在那里，Redis 客户端的连接就成了闲置连接，闲置过久，服务器一般会主动断开连接，减少闲置资源占用，这个时候blpop 和 brpop 或抛出异常，所以在编写客户端消费者的时候要小心，如果捕获到异常，还有重试。

其他缺点包括：

做消费者确认 ACK 麻烦，不能保证消费者消费消息后是否成功处理的问题（宕机或处理异常等），通常需要维护一个 Pending ( PEL ) 列表，保证消息处理确认；不能做广播模式，如 pub / sub，消息发布 / 订阅模型；不能重复消费，一旦消费就会被删除；不支持分组消费。

- **基于 Sorted - Set 的实现**

多用来实现延迟队列，当然也可以实现有序的普通的消息队列，但是消费者无法阻塞的获取消息，只能轮询，不允许重复消息。

- **PUB / SUB，订阅 / 发布模式**

优点：

典型的广播模式，一个消息可以发布到多个消费者；多信道订阅，消费者可以同时订阅多个信道，从而接收多类消息；消息即时发送，消息不用等待消费者读取，消费者会自动接收到信道发布的消息。

缺点：

消息一旦发布，不能接收。换句话就是发布时若客户端不在线，则消息丢失，不能寻回；不能保证每个消费者接收的时间是一致的；若消费者客户端出现消息积压，到一定程度，会被强制断开，导致消息意外丢失。通常发生在消息的生产远大于消费速度时；可见，Pub/Sub 模式不适合做消息存储，消息积压类的业务，而是擅长处理广播，即时通讯，即时反馈的业务。

- **基于Stream类型的实现**

Redis 5.0 最大的新特性就是多出了一个数据结构 Stream，它是一个新的强大的支持多播的可持久化的消息队列，借鉴了 Kafka 的设计

Stream 已经具备了一个消息队列的基本要素，生产者 API 、消费者 API ，消息 Broker ，消息的确认机制等等，所以在使用消息中间件中产生的问题，这里一样也会遇到。

**Stream 消息太多怎么办?**
要是消息积累太多，Stream 的链表岂不是很长内容会不会爆掉? xdel 指令又不会删除消息，它只是给消息做了个标志位。
Redis 自然考虑到了这一点，所以它提供了一个定长 Stream 功能在 xadd 的指令提供一个定长长度 maxlen ,就可以将老的消息干掉，确保最多不超过指定长度。

**消息如果忘记 ACK 会怎样?**
Stream 在每个消费者结构中保存了正在处理中的消息 ID 列表 PEL，如果消费者收到了消息处理完了但是没有回复 ACK ，就会导致 PEL 列表不断增长，如果有很多消费组的话，那么这个 PEL 占用的内存就会放大。所以消息要尽可能的快速消费并确认。

基本上已经有了一个消息中间件的雏形，可以考虑在生产过程中使用。

**PEL 如何避免消息丢失?**
在客户端消费者读取 Stream 消息时，Redis 服务器将消息回复给客户端的过程中，客户端突然断开了连接，消息就丢失了。但是 PEL 里已经保存了发出去的消息 ID 。待客户端重新连上之后，可以再次收到 PEL 中的消息D不过此时 xreadgroup 的起始消息 ID 不能为参数>，而必须是任意有效的消息 ID ，一般将参数设为 0 - 0，表示读取所有的PEL消息以及自 last_delivered_id 之后的新消息。

**死信问题**
如果某个消息，不能被消费者处理，也就是不能被 XACK，这是要长时间处于 Pending 列表中，即使被反复的转移给各个消费者也是如此。此时该消息的 delivery counter (通过 XPENDING 可以查询到)就会累加，当累加到某个我们预设的临界值时，我们就认为是坏消息(也叫死信，DeadLetter，无法投递的消息)，由于有了判定条件，我们将坏消息外理掉即可，删除即可。删除一个消息，使用 XDEL 语法，注意，这个命令并没有删除 Pending 中的消息，因此查看 Pending ，消息还会在，可以在执行执行 XDEL 之后，XACK 这个消息标识其处理完毕。

**一般使用 list 结构作为队列**， rpush 生产消息， lpop 消费消息。当 lpop 没有消息的时候，要适当 sleep 一会再重试。

- 面试官可能会问可不可以不用 sleep 呢？list 还有个指令叫 blpop ，在没有消息的时候，它会阻塞住直到消息到来。

- 面试官可能还问能不能生产一次消费多次呢？使用 pub / sub 主题订阅者模式，可以实现 1:N 的消息队列。

- 面试官可能还问 pub / sub 有什么缺点？在消费者下线的情况下，生产的消息会丢失，得使用专业的消息队列如 rabbitmq 等。
- 面试官可能还问 Redis 如何 **实现延时队列**？我估计现在你很想把面试官一棒打死如果你手上有一根棒球棍的话，怎么问的这么详细。但是你很克制，然后神态自若的回答道：使用 sortedset ，拿时间戳作为 score ，消息内容作为 key 调用 zadd 来生产消息，消费者用 zrangebyscore 指令获取 N 秒之前的数据轮询进行处理。

**面试扩散**：很多面试官上来就直接这么问： Redis 如何 **实现延时队列**？



### 14、什么是 bigkey？会有什么影响？

bigkey 是指 key 对应的 value 所占的内存空间比较大，例如一个字符串类型的 value 可以最大存到 512 MB，一个列表类型的 value 最多可以存储 23 - 1 个元素。

如果按照数据结构来细分的话，一般分为字符串类型 bigkey 和非字符串类型 bigkey。

字符串类型：体现在单个 value 值很大，一般认为超过 10 KB 就是 bigkey，但这个值和具体的 OPS 相关。

非字符串类型：哈希、列表、集合、有序集合，体现在元素个数过多。

bigkey 无论是空间复杂度和时间复杂度都不太友好，下面我们将介绍它的危害。

**bigkey 的危害**

bigkey 的危害体现在三个方面：

1、内存空间不均匀.(平衡)：例如在 Redis Cluster 中，bigkey 会造成节点的内存空间使用不均匀。一个 bigkey 存储数据量比较大，同一个 key 在同一个节点或服务器中存

储，会造成一定影响

2、超时阻塞：由于 Redis 单线程的特性，操作 bigkey 比较耗时，也就意味着阻塞 Redis 可能性增大。因为 bigkey 占用的空间比较大，所以操作起来效率会比较低，导致出现阻塞的可能性增加

3、网络拥塞：每次获取 bigkey 产生的网络流量较大，会增加带宽的压力。

假设一个 bigkey 为 1 MB，每秒访问量为 1000，那么每秒产生 1000 MB 的流量,对于普通的千兆网卡(按照字节算是 128 MB/s )的服务器来说简直是灭顶之灾，而且一般服务器会采用单机多实例的方式来部署，也就是说一个 bigkey 可能会对其他实例造成影响，其后果不堪设想。

对 value 值作拆分



### 15、Redis 如何解决 key 冲突？

1、业务隔离

业务 A Set A 1，key 区分出来

2、key 的设计

业务模块 + 系统名称 + 关键字 （ biz- pay- orderId -11）

3、分布式锁

多个客户端，并发写 key 原有值 1 ，修改顺序，2 -> 3 -> 4，2

客户端拿到锁，才能进行操作

时间戳 set key 7 : 00  set key 6 : 00



遇到 hash 冲突采用链表进行处理



### 16、怎么提高缓存命中率？

1、提前加载

2、增加缓存的存储空间，提高缓存的数据，提高命中率

3、调整缓存的存储类型

4、提升缓存的更新频次

需要在业务需求，缓存粒度，缓存策略，技术选型等各个方面去通盘考虑并做权衡。尽可能的聚焦在高频访问且时效性要求不高的热点业务上，通过缓存预加载（预热）、增加存储容量、调整缓存粒度、更新缓存等手段来提高命中率。



### 17、Redis 持久化方式有哪些？有什么区别？

RDB、AOF、混合持久化。

RDB 的优缺点：

优点：RDB 持久化文件，速度比较快，而且存储的是一个二进制文件，传输起来很方便。

缺点：RDB 无法保证数据的绝对安全，有时候就是 1 s 也会有很大的数据丢失。

AOF 的优缺点：

优点：AOF 相对 RDB 更加安全，一般不会有数据的丢失或者很少，官方推荐同时开启 AOF 和 RDB。

缺点：AOF 持久化的速度，相对于 RDB 较慢，存储的是一个文本文件，到了后期文件会比较大，传输困难。



#### Redis 提供两种持久化机制 RDB 和 AOF 机制:

**RDB** **持久化方式**

是指用数据集快照的方式半持久化模式记录 redis 数据库的所有键值对，在某个时间点将数据写入一个临时文件，持久化结束后，用这个临时文件替换上次持久化的文件，达到数据恢复。

**优点：**

- 只有一个文件 dump.rdb ，方便持久化。

- 容灾性好，一个文件可以保存到安全的磁盘。

- 性能最大化，fork 子进程来完成写操作，让主进程继续处理命令，所以是 IO 最大化。使用单独子进程来进行持久化，主进程不会进行任何 IO 操作，保证了 Redis 的高性能

- 相对于数据集大时，比 AOF 的启动效率更高。

**缺点：**

- 数据安全性低。 RDB 是间隔一段时间进行持久化，如果持久化之间 Redis 发生故障，会发生数据丢失。所以这种方式更适合数据要求不严谨的时候

**AOF=Appen-only file** **持久化方式**

是指所有的命令行记录以 Redis 命令请求协议的格式完全持久化存储，保存为 AOF 文件。

**优点：**

（1）数据安全， AOF 持久化可以配置 appendfsync 属性，有 always，每进行一次命令操作就记录到 AOF 文件中一次。

（2）通过 append 模式写文件，即使中途服务器宕机，可以通过 redis-check-aof 工具解决数据一致性问题。

（3） AOF 机制的 rewrite 模式。 AOF 文件没被 rewrite 之前（文件过大时会对命令进行合并重写），可以删除其中的某些命令（比如误操作的 flushall )

**缺点：**

（1） AOF 文件比 RDB 文件大，且恢复速度慢。

（2）数据集大的时候，比 RDB 启动效率低。



#### **持久化有两种，那应该怎么选择呢？**

1. 不要仅仅使用 RDB ，因为那样会导致你丢失很多数据。
2. 也不要仅仅使用 AOF ，因为那样有两个问题，第一，你通过 AOF 做冷备没有 RDB 做冷备的恢复速度更快；第二， RDB 每次简单粗暴生成数据快照，更加健壮，可以避免 AOF 这种复杂的备份和恢复机制的 bug 。

3. Redis 支持同时开启两种持久化方式，我们可以综合使用 AOF 和 RDB 两种持久化机制，用 AOF 来保证数据不丢失，作为数据恢复的第一选择; 用 RDB 来做不同程度的冷备，在 AOF 文件都丢失或损坏不可用的时候，还可以使用 RDB 来进行快速的数据恢复。

4. 如果同时使用 RDB 和 AOF 两种持久化机制，那么在 Redis 重启的时候，会使用 AOF 来重新构建数据，因为 AOF 中的数据更加完整。



### 18、为什么 Redis 需要把所有数据放到内存中？

Redis 为了达到最快的读写速度，将数据都读到内存中，并通过异步的方式将数据写入磁盘，所以 Redis 具有快速和数据持久化的特征。如果不将数据放在内存中，磁盘 I/O 速度为严重影响 Redis 的性能。



### 19、如何保证缓存与数据库双写时的数据一致性

第一种方案：采用延时双删策略

具体的步骤就是：

先删除缓存；

再写数据库；

休眠 500 毫秒；

再次删除缓存。

第二种方案：异步更新缓存(基于订阅 binlog 的同步机制)

技术整体思路：

MySQL binlog 增量订阅消费 + 消息队列 + 增量数据更新到 Redis



### 20、Redis 集群方案应该怎么做？

**Redis集群**
Redis Cluster 是 Redis 的分布式解决方案，在 3.0 版本正式推出，有效地解决了 Redis 分布式方面的需求。当遇到单机内存、并发、流量等瓶颈时，可以采用 Cluster 架构方案达到负载均衡的目的。之前，Redis 分布式方案一般有两种：
1、客户端分区方案，优点是分区逻辑可控，缺点是需要自己处理数据路由、高可用、故障转移等问题。
2、代理方案，优点是简化客户端分布式逻辑和升级维护便利，缺点是加重架构部署复杂度和性能损耗。现在官方为我们提供了专有的集群方案：Redis Cluster，它非常优雅地解决了 Redis 集群方面的问题，因此理解应用好 Redis Cluster 将极大地解放我们使用分布式 Redis 的工作量。

**虚拟槽分区**
Redis 则是利用了虚拟槽分区，可以算上面虚拟一致性哈希分区的变种，它使用分散度良好的哈希函数把所有数据映射到一个固定范围的整数集合中，整数定义为槽( slot )。这个范围一般远大于节点数，比如 Redis Cluster 槽范围是 0~16383 。槽是集群内数据管理和迁移的基本单位。采用大范围槽的主要目的是为了方便数据拆分和集群扩展。每个节点会负责一定数量的槽。
比如集群有 3 个节点，则每个节点平均大约负责 5460 个槽。由于采用高质量的哈希算法，每个槽所映射的数据通常比较均匀，将数据平均划分到 5 个节点进行数据分区 Redis Cluster 就是采用虚拟槽分区下面就介绍 Redis 数据分区方法

**集群功能限制**
Redis 集群相对单机在功能上存在一些限制需要开发人员提前了解，在使用时做好规避。限制如下
1、key 批量操作支持有限。如 mset、mget，目前只支持具有相同slot值的key执行批量操作。对于映射为不同 slot 值的 key 由于执行mget、mget 等操作可能存在于多个节点上因此不被支持
2、key 事务操作支持有限。同理只支持多 key 在同一节点上的事务操作，当多个 key 分布在不同的节点上时无法使用事务功能。
3、key 作为数据分区的最小粒度，因此不能将一个大的键值对象如 hash、list 等映射到不同的节点
4、不支持多数据库空间。单机下的 Redis 可以支持 16 个数据库，集群模式下只能使用一个数据库空间，即 db 0
复制结构只支持一层，从节点只能复制主节点，不支持嵌套树状复制结构。

**搭建集群**
介绍完 Redis 集群分区规则之后，下面我们开始搭建 Redis 集群。搭建集群有几种方式

1、依照 Redis 协议手工搭建，使用cluster meet、cluster addslots、cluster replicate 命令

2、5.0 之前使用由 ruby 语言编写的 redis-trib.rb，在使用前需要安装 ruby 语言环境。

3、5.0 及其之后 redis 摒弃了 redis-trib.rb，将搭建集群的功能合并到了 redis-cli。
我们简单点，采用第三种方式搭建。集群中至少应该有奇数个节点，所以至少有三个节点，官方推荐三主三从的配置方式，我们就来搭建一个三主三从的集群。

**Gossip 消息**
Gossip 协议的主要职责就是信息交换。信息交换的载体就是节点彼此发送的 Gossip 消息，了解这些消息有助于我们理解集群如何完成信息交换
常用的 Gossip 消息可分为：ping 消息、pong 消息、meet消息、fail 消息等 

**meet 消息**
用于通知新节点加入。消息发送者通知接收者加入到当前集群，meet 消息通信正常完成后，接收节点会加入到群中并进行周期性的ping、pong 消息交换。
**ping消息**
集群内交换最频繁的消息，集群内每个节点每秒向多个其他节点发送ping消息用于检测节点是否在线和交换彼止状态信息。ping 消息发送封装了自身节点和部分其他节点的状态数据
**pong消息**
当接收到 ping、meet 消息时，作为响应消息回复给发送方确认消息正常通信。pong 消息内部封装了自身状态据。节点也可以向集群内广播自身的 pong 消息来通知整个集群对自身状态进行更新。
**fail消息**
当节点判定集群内另一个节点下线时，会向集群内广播一个 fail 消息,其他节点接收到 fail 消息之后把对应节点更新为下线状态。

1. Redis Sentinel体量较小时，选择 Redis Sentinel，单主 Redis 足以支撑业务。
2. Redis Cluster Redis 官方提供的集群化方案，体量较大时，选择 Redis Cluster，通过分片，使用更多内存。
3. Twemprox 是 Twtter 开源的一个 Redis和 Memcached 代理服务器，主要用于管理 Redis 和 Memcached 集群，减少与Cache 服务器直接连接的数量。
4. CodisCodis 是一个代理中间件，当客户端向 Codis 发送指令时，Codis 负责将指令转发到后面的 Redis 来执行，并将结果返回给客户端。一个 Codis 实例可以连接多个 Redis 实例，也可以启动多个 Codis 实例来支撑，每个 Codis 节点都是对等的，这样可以增加整体的 QPS 需求，还能起到容灾功能。
5. 客户端分片在Redis Cluster还没出现之前使用较多，现在基本很少热你使用了，在业务代码层实现，起几个毫无关联的Redis实例，在代码层，对 Key 进行 hash 计算，然后去对应的 Redis实例操作数据。这种方式对 hash 层代码要求比较高，考虑部分包括，节点失效后的替代算法方案，数据震荡后的自动脚本恢复，实例的监控，等等。



### 21、Redis 集群方案什么情况下会导致整个集群不可用？

**集群不可用判定**
为了保证集群完整性，默认情况下当集群 16384 个槽任何一个没有指派到节点时整个集群不可用。执行任何键命令返回 ( error ) CLUSTERDOWN Hash slot not served 错误。这是对集群完整性的一种保护措施，保证所有的槽都指派给在线的节点。但是当持有槽的主节点下线时，从故障发现到自动完成转移期间整个集群是不可用状态，对于大多数业务无法容忍这种情况，因此可以将参数 cluster-require-full-coverage 配置为 no，当主节点故障时只影响它负责槽的相关命令执行，不会影响其他主节点的可用性。

但是从集群故障转移的原理来说，集群会出现不可用，当：

1.当访问一个 Master 和 Slave 节点都挂了的槽的时候，cluster-require-full-coverage = yes，会报槽无法获取。

2、集群主库半数宕机（根据 fail over 原理，fail 掉一个主需要一半以上主都投票通过才可以）。

另外，当集群 Master 节点个数小于 3 个的时候，或者集群可用节点个数为偶数的时候，基于 fail 的这种选举机制的自动主从切换过程可能会不能正常工作，一个是标记 fall 的过程，一个是选举新的 master 的过程，都有可能异常。



Redis 没有使用哈希一致性算法，而是使用哈希槽。Redis 中的哈希槽一共有 16384 个，计算给定密钥的哈希槽，我们只需要对密钥的 CRC16 取摸 16384。假设集群中有 A、B、C 三个集群节点，不存在复制模式下，每个集群的节点包含的哈希槽如下：

- 节点 A 包含从 0 到 5500 的哈希槽；

- 节点 B 包含从 5501 到 11000 的哈希槽；

- 节点 C 包含从 11001 到 16383 的哈希槽；

- 这时，如果节点 B 出现故障，整个集群就会出现缺少 5501 到 11000 的哈希槽范围而不可用。



### 22、说一说 Redis 哈希槽的概念？

Redis 集群并没有使用一致性 hash，而是引入了哈希槽的概念。Redis 集群有 16384（2^14）个哈希槽，每个 key 通过 CRC16 校验后对 16384 取模来决定放置哪个槽，集群的每个节点负责一部分 hash 槽。

**数据分布理论**
分布式数据库首先要解决把整个数据集按照分区规则映射到多个节点的问题，即把数据集划分到多个节点上，每个节点负责整体数据的一个子集。
需要重点关注的是数据分区规则。常见的分区规则有哈希分区和顺序分区两种，哈希分区离散度好、数据分布业务无关、无法顺序访问，顺序分区离散度易倾斜、数据分布业务相关、可顺序访问。

**节点取余分区**
使用特定的数据，如 Redis 的键或用户 ID，再根据节点数量 N 使用公式：hash(key) % N 计算出哈希值，用来决定数据映射到哪一个节点上。这种方案存在一个问题:当节点数量变化时，如扩容或收缩节点，数据节点映射关系需要重新计算，会导致数据的重新迁移
这种方式的突出优点是简单性，常用于数据库的分库分表规则，一般采用预分区的方式，提前根据数据量规划好分区数，比如划分为 512或 1024 张表，保证可支撑未来一段时间的数据量,再根据负载情况将表迁移到其他数据库中。扩容时通常采用翻倍扩容，避免数据映射全部被打乱导致全量迁移的情况,如图 10-2 所示。

**一致性哈希分区**
一致性哈希分区（ Distributed Hash Table ）实现思路是为系统中每个节点分配一个 token，范围一般在 0~23，这些 token 构成一个哈希环。数据读写执行节点查找操作时，先根据 key 计算 hash 值，然后顺时针找到第一个大于等于该哈希值的 token 节点

这种方式相比节点取余最大的好处在于加入和删除节点只影响哈希环中相邻的节点，对其他节点无影响。但一致性哈希分区存在几个问题：
1，当使用少量节点时，节点变化将大范围影响哈希环中数据映射，因此这种方式不适合少量数据节点的分布式方案。
2、增加节点只能对下一个相邻节点有比较好的负载分担效果，对集群中其他的节点基本没有起到负载分担的效果，类似地，删除节点会导致下一个相邻节点负载增加，而其他节点却不能有效分担负载压力。

正因为一致性哈希分区的这些缺点，一些分布式系统采用虚拟槽对一致性哈希进行改进，比如虚拟一致性哈希分区

**虚拟槽分区**
Redis 则是利用了虚拟槽分区，可以算上面虚拟一致性哈希分区的变种，它使用分散度良好的哈希函数把所有数据映射到一个固定范围的整数集合中，整数定义为槽 （slot）。这个范围一般远远大于节点数，比如 Redis Cluster 槽范围是 0~16383 。槽是集群内数据管理和迁移的基本单位。采用大范围槽的主要目的是为了方便数据拆分和集群扩展。每个节点会负责一定数量的槽。
比如集群有 3 个节点，则每个节点平均大约负责 5460 个槽。由于采用高质量的哈希算法，每个槽所映射的数据通常比较均匀，将数据平均划分到 5 个节点进行数据分区。Redis Cluster 就是采用虚拟槽分区下面就介绍 Redis 数据分区方法

slot：称为哈希槽

Redis 集群中内置了 16384 个哈希槽，当需要在 Redis 集群中放置一个 key-value 时，Redis 先对 key 使用 crc 16 算法算出一个结果，然后把结果对 16384 求余数，这样每个 key 都会对应一个编号在 0-16383 之间的哈希槽，Redis 会根据节点数量大致均等的将哈希槽映射到不同的节点。

使用哈希槽的好处就在于可以方便的添加或移除节点。

当需要增加节点时，只需要把其他节点的某些哈希槽挪到新节点就可以了；

当需要移除节点时，只需要把移除节点上的哈希槽挪到其他节点就行了；



### 23、Redis 集群会有写操作丢失吗？为什么？

Redis 并不能保证数据的强一致性，所以在特定条件下可能会丢失数据，比如 

1、最大内存不足（再次写入返回报错） 

2、未开启持久化的 master 故障重启，slave 再次同步 

3、网络波动，哨兵主动选举，主从切换的期间



以下情况可能导致写操作丢失：

过期 key 被清理

最大内存不足，导致 Redis 自动清理部分 key 以节省空间

主库故障后自动重启，从库自动同步

单独的主备方案，网络不稳定触发哨兵的自动切换主从节点，切换期间会有数据丢失



### 24、Redis 常见性能问题和解决方案有哪些？

1、持久化性能问题

全量复制、部分复制（一台机器）

主从 主不做持久化，从节点做持久化

2、数据比较重要，AOF 持久化 slave 开启 AOF，策略每秒同步

3、主从复制 流畅，同一个局域网

4、尽量减少主库压力，增加从库

5、主从复制 不要采用网状结构，可用线性结构减轻主库压力

一、缓存穿透：就是查询一个压根就不存在的数据，即缓存中没有，数据库中也没有

解决方案：使用布隆过滤器，把数据先加载到布隆过滤器中，访问前先判断是否存在于布隆过滤器中，不存在代表这笔数据压根就不存在。

缺点：布隆过滤器是不可变的，可能一开始过滤器和数据库数据时一致的，后面数据库数据变了，或变多或变少，而对应的布隆过滤器的数据也要改变，这时会比较麻烦。

二、缓存击穿：数据库中有，缓存中没有。缓存击穿实际就是一个并发问题，一般来说查询数据，先查询缓存，有直接返回，没有再查询数据库并放到缓存中之后返回，但这种场景在并发情况下就会有问题，假设同时又 100 个请求执行上面

逻辑的代码，则可能会出现多个请求都查询数据库，因为大家同时执行，都查到了缓存中没有数据。

解决方案：加锁。如果是单机部署，则可以使用 JVM 级别的锁，如 lock、synchronized。如果是集群部署，则需要使用分布式锁，如基于Redis、zookeeper、MySQL 等实现的分布式锁。

三、缓存雪崩：大部分数据同时失效、过期，新的缓存又没来，导致大量的请求都去访问数据库而导致的服务器压力过大、宕机、系统崩溃。

解决方案：搭建高可用的 Redis 集群，避免压力集中于一个节点；缓存失效时间错开，避免缓存同时失效而都去请求数据库。



**简而言之：**

Redis 常见性能问题和解决方案如下：

- Master 最好不要做任何持久化工作，如 RDB 内存快照和 AOF 日志文件；

- 如果数据比较重要，某个 Slave 开启 AOF 备份数据，策略设置为每秒同步一次；

- 为了主从复制的速度和连接的稳定性，Master 和 Slave 最好在同一个局域网内；

- 尽量避免在压力很大的主库上增加从库；

- 主从复制不要用图状结构，用单向链表结构更为稳定，即：Master <- Slave1 <- Slave2 <-Slave3….；这样的结构方便解决单点故障问题，实现 Slave 对 Master 的替换。如果 Master 挂了，可以立刻启用 Slave1 做 Master，其他不变。



### 25、热点数据和冷数据是什么

对于冷数据而言，大部分数据可能还没有再次访问到就已经被挤出内存，不仅占用内存，而且价值不大。频繁修改的数据，看情况考虑使用缓存。例如寿星列表、导航信息都存在一个特点，就是信息修改频率不高，读取通常非常高的场景。

对于热点数据，比如我们的某 IM 产品，生日祝福模块，当天的寿星列表，缓存以后可能读取数十万次。 再举个例子，某导航产品，我们将导航信息，缓存以后可能读取数百万次。Redis 更新之前至少读取两次才放缓存



### 26、什么情况下可能会导致 Redis 阻塞？

1、客户端阻塞 命令 keys *、Hgetall、smembers 时间复杂度 O(n)

2、bigkey 删除 ZSet（100 万的元素，删除 2 s ）

3、清空库 flushdb、flushall

4、AOF 日志同步写 一个同步写磁盘耗时 1~2 ms

5、从库 加载 RDB 文件

数据集中过期

不合理地使用 API 或数据结构

CPU 饱和

持久化阻塞



Redis 产生阻塞的原因主要有内部和外部两个原因导致：

**内部原因**

- 如果 Redis 主机的 CPU 负载过高，也会导致系统崩溃；

- 数据持久化占用资源过多；

- 对 Redis 的 API 或指令使用不合理，导致 Redis 出现问题。

**外部原因**

- 外部原因主要是服务器的原因，例如服务器的 CPU 线程在切换过程中竞争过大，内存出现问题、网络问题等。



### 27、什么时候选择 Redis，什么时候选择 Memcached？

实际业务分析

如果业务中更加侧重性能的⾼效性，对持久化要求不⾼，那么应该优先选择 Memcached。

如果业务中对持久化有需求或者对数据涉及到存储、排序等一系列复杂的操作，比如业务中有排⾏榜类应⽤、社交关系存储、数据排重、实时配置等功能，那么应该优先选择 Redis



### 28、Redis 过期策略都有哪些？LRU 算法知道吗？

1. noeviction：返回错误当内存限制达到，并且客户端尝试执行会让更多内存被使用的命令。

2. allkeys-lru： 尝试回收最少使用的键（LRU），使得新添加的数据有空间存放。

3. volatile-lru：尝试回收最少使用的键（LRU），但仅限于在过期集合的键，使得新添加的数据有空间存放。

4. allkeys-random：回收随机的键使得新添加的数据有空间存放。

5. volatile-random：回收随机的键使得新添加的数据有空间存放，但仅限于在过期集合的键。

6. volatile-ttl：回收在过期集合的键，并且优先回收存活时间（TTL）较短的键，使得新添加的数据有空间存放。

**LRU 算法**

实现 LRU 算法除了需要 key / value 字典外，还需要附加一个链表，链表的元素按照一定的顺序进行排列。当空间满的时候，会踢掉链表尾部的元素。当字典的某个元素被访问时，它在链表中的位置会被移动到表头。所以链表的元素排列顺序就是元素最近被访问的时间顺序。

**近似 LRU 算法**

**LFU 算法**

#### Redis 缓存刷新策略有哪些？

![](https://dream-syz.github.io/11-2.png)



### 29、说说 Redis 的线程模型

```
这问题是因为前面回答问题的时候提到了 Redis 是基于非阻塞的 IO 复用模型。如果这个问题回答不上来，就相当于前面的回答是给自己挖坑，因为你答不上来，面试官对你的印象可能就要打点折扣了。
```

Redis 内部使用 **文件事件处理器**  file event handler ，这个文件事件处理器是 **单线程的**，所以 Redis 才叫做单线程的模型。它采用 IO 多路复用机制同时监听多个 socket ，根据 socket 上的事件来选择对应的事件处理器进行处理。

文件事件处理器的结构包含 4 个部分：

1. 多个 socket 。
2. IO 多路复用程序。
3. 文件事件分派器。
4. 事件处理器（连接应答处理器、命令请求处理器、命令回复处理器）。

多个 socket 可能会并发产生不同的操作，每个操作对应不同的文件事件，但是 IO 多路复用程序会监听多个 socket，会将 socket 产生的事件放入队列中排队，事件分派器每次从队列中取出一个事件，把该事件交给对应的事件处理器进行处理。

来看客户端与 Redis 的一次通信过程：

![](https://dream-syz.github.io/11-1.png)

下面来大致说一下这个图：

1. 客户端 Socket01 向 Redis 的 Server Socket 请求建立连接，此时 Server Socket 会产生一个 AE_READABLE 事件，IO 多路复用程序监听到 server socket 产生的事件后，将该事件压入队列中。文件事件分派器从队列中获取该事件，交给连接应答处理器。连接应答处理器会创建一个能与客户端通信的 Socket 01，并将该 Socket 01 的 AE_READABLE 事件与命令请求处理器关联。

2. 假设此时客户端发送了一个 set key value 请求，此时 Redis 中的 Socket 01 会产生 AE_READABLE 事件，IO 多路复用程序将事件压入队列，此时事件分派器从队列中获取到该事件，由于前面 Socket 01 的 AE_READABLE 事件已经与命令请求处理器关联，因此事件分派器将事件交给命令请求处理器来处理。命令请求处理器读取 Socket 01 的 set key value 并在自己内存中完成 set key value 的设置。操作完成后，它会将 Socket01 的 AE_WRITABLE 事件与令回复处理器关联。

3. 如果此时客户端准备好接收返回结果了，那么 Redis 中的 Socket 01 会产生一个 AE_WRITABLE 事件，同样压入队列中，事件分派器找到相关联的命令回复处理器，由命令回复处理器对 Socket 01 输入本次操作的一个结果，比如 ok ，之后解除 Socket 01 的AE_WRITABLE 事件与命令回复处理器的关联。

这样便完成了一次通信。 不要怕这段文字，结合图看，一遍不行两遍，实在不行可以网上查点资料结合着看，一定要搞清楚，否则前面吹的牛逼就白费了。



### 30、为什么 Redis 需要把所有数据放到内存中？

Redis 将数据放在内存中有一个好处，那就是可以实现最快的对数据读取，如果数据存储在硬盘中，磁盘 I / O 会严重影响 Redis 的性能。而且 Redis 还提供了数据持久化功能，不用担心服务器重启对内存中数据的影响。其次现在硬件越来越便宜的情况下，Redis 的使用也被应用得越来越多，使得它拥有很大的优势。



### 31、Redis 的同步机制了解是什么？

Redis 支持主从同步、从从同步。如果是第一次进行主从同步，主节点需要使用 bgsave 命令，再将后续修改操作记录到内存的缓冲区，等 RDB 文件全部同步到复制节点，复制节点接受完成后将 RDB 镜像记载到内存中。等加载完成后，复制节点通知主节点将复制期间修改的操作记录同步到复制节点，即可完成同步过程。



### 32、 pipeline 有什么好处，为什么要用 pipeline？

使用 pipeline（管道）的好处在于可以将多次 I / O 往返的时间缩短为一次，但是要求管道中执行的指令间没有因果关系。

用 pipeline 的原因在于可以实现请求/响应服务器的功能，当客户端尚未读取旧响应时，它也可以处理新的请求。如果客户端存在多个命令发送到服务器时，那么客户端无需等待服务端的每次响应才能执行下个命令，只需最后一步从服务端读取回复即可。



### 33、说说你对 Redis 事务的理解？

**什么是** **Redis** **事务？原理是什么？**

Redis 中的事务是一组命令的集合，是 Redis 的最小执行单位。它可以保证一次执行多个命令，每个事务是一个单独的隔离操作，事务中的所有命令都会序列化、按顺序地执行。服务端在执行事务的过程中，不会被其他客户端发送来的命令请求打断。

它的原理是先将属于一个事务的命令发送给 Redis，然后依次执行这些命令。

**Redis** **事务的注意点有哪些？**

需要注意的点有：

- Redis 事务是不支持回滚的，不像 MySQL 的事务一样，要么都执行要么都不执行；

- Redis 服务端在执行事务的过程中，不会被其他客户端发送来的命令请求打断。直到事务命令全部执行完毕才会执行其他客户端的命令。

**Redis** **事务为什么不支持回滚？**

Redis 的事务不支持回滚，但是执行的命令有语法错误，Redis 会执行失败，这些问题可以从程序层面捕获并解决。但是如果出现其他问题，则依然会继续执行余下的命令。这样做的原因是因为回滚需要增加很多工作，而不支持回滚则可以保持简单、快速的特性。

**简述：**

Redis 事务功能是通过 MULTI、EXEC、DISCARD 和 WATCH 四个原语实现的 Redis 会将一个事务中的所有命令序列化，然后按顺序执行。 1.redis 不支持回滚“ Redis 在事务失败时不进行回滚，而是继续执行余下的命令”， 所以 Redis 的内部可以保持简单且快速。 2.如果在一个事务中的命令出现错误，那么所有的命令都不会执行； 3.如果在一个事务中出现运行错误，那么正确的命令会被执行。

1）MULTI 命令用于开启一个事务，它总是返回 OK。 MULTI 执行之后，客户端可以继续向服务器发送任意多条命令，这些命令不会立即被执行，而是被放到一个队列中，当 EXEC 命令被调用时，所有队列中的命令才会被执行。 2）EXEC：执行所有事务块内的命令。返回事务块内所有命令的返回值，按命令执行的先后顺序排列。 当操作被打断时，返回空值 nil 。 3）通过调用 DISCARD，客户端可以清空事务队列，并放弃执行事务， 并且客户端会从事务状态中退出。 4）WATCH 命令可以为 Redis 事务提供 check-and-set （CAS）行为。 可以监控一个或多个键，一旦其中有一个键被修改（或删除），之后的事务就不会执行，监控一直持续到 EXEC 命令。



### 34、熟悉哪些 Redis 集群模式？

1. Redis Sentinel

	体量较小时，选择 Redis Sentinel ，单主 Redis 足以支撑业务。

2. Redis Cluster

	Redis 官方提供的集群化方案，体量较大时，选择 Redis Cluster ，通过分片，使用更多内存。

3. Twemprox

	Twemprox 是 Twtter 开源的一个 Redis 和 Memcached 代理服务器，主要用于管理 Redis 和 Memcached 集群，减少与 Cache 服务	器直接连接的数量。

4. Codis

	Codis 是一个代理中间件，当客户端向 Codis 发送指令时， Codis 负责将指令转发到后面的 Redis 来执行，并将结果返回给客户端。一	个 Codis 实例可以连接多个 Redis 实例，也可以启动多个 Codis 实例来支撑，每个 Codis 节点都是对等的，这样可以增加整体的 QPS 	需求，还能起到容灾功能。

5. 客户端分片

	在 Redis Cluster 还没出现之前使用较多，现在基本很少热你使用了，在业务代码层实现，起几个毫无关联的 Redis 实例，在代码层，	对 Key 进行 hash 计算，然后去对应的 Redis 实例操作数据。这种方式对 hash 层代码要求比较高，考虑部分包括，节点失效后的替代	算法方案，数据震荡后的自动脚本恢复，实例的监控，等等。



### 35、是否使用过 Redis Cluster 集群，集群的原理是什么？

使用过 Redis 集群，它的原理是：

- 所有的节点相互连接

- 集群消息通信通过集群总线通信，集群总线端口大小为客户端服务端口 + 10000（固定值）

- 节点与节点之间通过二进制协议进行通信

- 客户端和集群节点之间通信和通常一样，通过文本协议进行

- 集群节点不会代理查询

- 数据按照 Slot 存储分布在多个 Redis 实例上

- 集群节点挂掉会自动故障转移

- 可以相对平滑扩/缩容节点

Redis 集群中内置了 16384 个哈希槽，当需要在 Redis 集群中放置一个 key-value 时，redis 先对key 使用 crc16 算法算出一个结果，然后把结果对 16384 求余数，这样每个 key 都会对应一个编号在 0~16383 之间的哈希槽，redis 会根据节点数量大致均等的将哈希槽映射到不同的节点。



### 36、假如 **Redis** 里面有 1 亿个 key，其中有 10w **个** **key** 是以某个固定的已知的前缀开头的，如果将它们全部找出来？

我们可以使用 keys 命令和 scan 命令，但是会发现使用 scan 更好。

**1.** **使用** **keys** **命令**

直接使用 keys 命令查询，但是如果是在生产环境下使用会出现一个问题，keys 命令是遍历查询的，查询的时间复杂度为 O(n)，数据量越大查询时间越长。而且 Redis 是单线程，keys 指令会导致线程阻塞一段时间，会导致线上 Redis 停顿一段时间，直到 keys 执行完毕才能恢复。这在生产环境是不允许的。除此之外，需要注意的是，这个命令没有分页功能，会一次性查询出所有符合条件的 key 值，会发现查询结果非常大，输出的信息非常多。所以不推荐使用这个命令。

**2.** **使用** **scan** **命令**

scan 命令可以实现和 keys 一样的匹配功能，但是 scan 命令在执行的过程不会阻塞线程，并且查找的数据可能存在重复，需要客户端操作去重。因为 scan 是通过游标方式查询的，所以不会导致 Redis 出现假死的问题。Redis 查询过程中会把游标返回给客户端，单次返回空值且游标不为 0，则说明遍历还没结束，客户端继续遍历查询。scan 在检索的过程中，被删除的元素是不会被查询出来的，但是如果在迭代过程中有元素被修改，scan 不能保证查询出对应元素。相对来说，scan 指令查找花费的时间会比 keys 指令长。



### 37、Redis 报内存不足怎么处理？

Redis 内存不足可以这样处理：

- 修改配置文件 redis.conf 的 maxmemory 参数，增加 Redis 可用内存；

- 设置缓存淘汰策略，提高内存的使用效率；
-  使用 Redis 集群模式，提高存储量。



### 38、缓存雪崩、缓存穿透、缓存预热、缓存更新、缓存降级等问题汇总

**一、缓存雪崩**

我们可以简单的理解为：由于原有缓存失效，新缓存未到期间（例如：我们设置缓存时采用了相同的过期时间，在同一时刻出现大面积的缓存过期），所有原本应该访问缓存的请求都去查询数据库了，而对数据库 CPU 和内存造成巨大压力，严重的会造成数据库宕机。从而形成一系列连锁反应，造成整个系统崩溃。

 **解决办法：** 大多数系统设计者考虑用加锁（ 最多的解决方案）或者队列的方式保证来保证不会有大量的线程对数据库一次性进行读写，从而避免失效时大量的并发请求落到底层存储系统上。还有一个简单方案就时讲缓存失效时间分散开。

**二、缓存穿透** 

缓存穿透是指用户查询数据，在数据库没有，自然在缓存中也不会有。这样就导致用户查询的时候，在缓存中找不到，每次都要去数据库再查询一遍，然后返回空（相当于进行了两次无用的查询）。这样请求就绕过缓存直接查数据库，这也是经常提的缓存命中率问题。

 **解决办法：**

最常见的则是采用 **布隆过滤器**，将所有可能存在的数据哈希到一个足够大的 bitmap 中，一个一定不存在的数据会被这个 bitmap 拦截掉，从而避免了对底层存储系统的查询压力。 另外也有一个更为 **简单粗暴的方法**，如果一个查询返回的数据为空（不管是数据不存在，还是系统故障），我们仍然把这个空结果进行缓存，但它的过期时间会很短，最长不超过五分钟。通过这个直接设置的默认值存放到缓存，这样第二次到缓冲中获取就有值了，而不会继续访问数据库，这种办法最简单粗暴。

5 TB 的硬盘上放满了数据，请写一个算法将这些数据进行排重。如果这些数据是一些32bit大小的数据该如何解决？如果是 64 bit 的呢？对于空间的利用到达了一种极致，那就是 Bitmap 和布隆过滤器(Bloom Filter)。 Bitmap： 典型的就是哈希表 缺点是，Bitmap 对于每个元素只能记录 1 bit 信息，如果还想完成额外的功能，恐怕只能靠牺牲更多的空间、时间来完成了。

**布隆过滤器（推荐）** 

布隆过滤器是一种概率型数据结构，用于判断一个元素是否可能存在于一个集合中。它通过使用一个位数组和多个哈希函数来实现。当一个元素被插入到布隆过滤器中时，通过对元素进行多次哈希，将对应的位数组位置标记为 1。当需要查询一个元素是否存在时，同样对元素进行多次哈希，如果对应的位数组位置有任意一位为 0，则可以确定元素一定不存在于集合中;如果所有位数组位置都为 1，则元素可能存在于集合中，但存在一定的误判率。布隆过滤器适用于对查询速度要求较高，可以容忍一定的误判率的场景，如缓存系统、垃圾邮件过滤等。

就是引入了  k(k>1)k(k>1) 个相互独立的哈希函数，保证在给定的空间、误判率下，完成元素判重的过程。 它的优点是空间效率和查询时间都远远超过一般的算法，缺点是有一定的误识别率和删除困难。 Bloom-Filter 算法的核心思想就是利用多个不同的Hash函数来解决“冲突”。 Hash存在一个冲突（碰撞）的问题，用同一个 Hash 得到的两个 URL 的值有可能相同。为了减少冲突，我们可以多引入几个Hash，如果通过其中的一个 Hash 值我们得出某元素不在集合中，那么该元素肯定不在集合中。只有在所有的 Hash 函数告诉我们该元素在集合中时，才能确定该元素存在于集合中。这便是 Bloom-Filter 的基本思想。 Bloom-Filter 一般用于在大数据量的集合中判定某元素是否存在。

**布谷鸟过滤器(Cuckoo Filter)：**布隆过滤器的增强版

**三、缓存预热** 

缓存预热这个应该是一个比较常见的概念，相信很多小伙伴都应该可以很容易的理解，缓存预热就是系统上线后，将相关的缓存数据直接加载到缓存系统。这样就可以避免在用户请求的时候，先查询数据库，然后再将数据缓存的问题！用户直接查询事先被预热的缓存数据！ 解决思路： 

1、直接写个缓存刷新页面，上线时手工操作下；

2、数据量不大，可以在项目启动的时候自动进行加载；

3、定时刷新缓存；

**四、缓存更新** 

除了缓存服务器自带的缓存失效策略之外（Redis默认的有6中策略可供选择），我们还可以根据具体的业务需求进行自定义的缓存淘汰，常见的策略有两种： （1）定时去清理过期的缓存； （2）当有用户请求过来时，再判断这个请求所用到的缓存是否过期，过期的话就去底层系统得到新数据并更新缓存。 两者各有优劣，第一种的缺点是维护大量缓存的 key 是比较麻烦的，第二种的缺点就是每次用户请求过来都要判断缓存失效，逻辑相对比较复杂！具体用哪种方案，大家可以根据自己的应用场景来权衡。

**五、缓存降级**

当访问量剧增、服务出现问题（如响应时间慢或不响应）或非核心服务影响到核心流程的性能时，仍然需要保证服务还是可用的，即使是有损服务。系统可以根据一些关键数据进行自动降级，也可以配置开关实现人工降级。 降级的最终目的是保证核心服务可用，即使是有损的。而且有些服务是无法降级的（如加入购物车、结算）。 以参考日志级别设置预案： （1）一般：比如有些服务偶尔因为网络抖动或者服务正在上线而超时，可以自动降级； （2）警告：有些服务在一段时间内成功率有波动（如在 95 ~ 100 % 之间），可以自动降级或人工降级，并发送告警； （3）错误：比如可用率低于 90 %，或者数据库连接池被打爆了，或者访问量突然猛增到系统能承受的最大阀值，此时可以根据情况自动降级或者人工降级； （4）严重错误：比如因为特殊原因数据错误了，此时需要紧急人工降级。

服务降级的目的，是为了防止 Redis 服务故障，导致数据库跟着一起发生雪崩问题。因此，对于不重要的缓存数据，可以采取服务降级策略，例如一个比较常见的做法就是，Redis 出现问题，不去数据库查询，而是直接返回默认值给用户。



### 39、Redis 的数据类型及使用场景

**String**

最常规的 set / get 操作，Value 可以是 String 也可以是数字。一般做一些复杂的计数功能的缓存。

**Hash**

这里 Value 存放的是结构化的对象，比较方便的就是操作其中的某个字段。我在做单点登录的时候，就是用这种数据结构存储用户信息，以 CookieId 作为 Key，设置 30 分钟为缓存过期时间，能很好的模拟出类似 Session 的效果。

**List**

使用 List 的数据结构，可以做简单的消息队列的功能。另外，可以利用 lrange 命令，做基于 Redis 的分页功能，性能极佳，用户体验好。

**Set**

因为 Set 堆放的是一堆不重复值的集合。所以可以做全局去重的功能。我们的系统一般都是集群部署，使用 JVM 自带的 Set 比较麻烦。另外，就是利用交集、并集、差集等操作，可以计算共同喜好，全部的喜好，自己独有的喜好等功能。

**Sorted Set**

Sorted Set 多了一个权重参数 Score，集合中的元素能够按 Score 进行排列。可以做排行榜应用，取 TOP(N) 操作。Sorted Set 可以用来做延时任务。





## 分布式篇

### 1、分布式幂等性如何设计？

在高并发场景的架构里，幂等性是必须得保证的。比如说支付功能，用户发起支付，如果后台没有做幂等校验，刚好用户手抖多点了几下，于是后台就可能多次受到同一个订单请求，不做幂等很容易就让用户重复支付了，这样用户是肯定不能忍的。

**解决方案：**

1、查询和删除不在幂等讨论范围，查询肯定没有幂等的说，删除：第一次删除成功后，后面来删除直接返回 0，也是返回成功。

2、建唯一索引：唯一索引或唯一组合索引来防止新增数据存在脏数据 （当表存在唯一索引，并发时新增异常时，再查询一次就可以了，数据应该已经存在了，返回结果即可）。

3、token机制：由于重复点击或者网络重发，或者 nginx 重发等情况会导致数据被重复提交。前端在数据提交前要向后端服务的申请 token，token 放到 Redis 或 JVM 内存，token 有效时间。提交后后台校验 token，同时删除 token，生成新的 token 返回。redis 要用删除操作来判断 token，删除成功代表 token 校验通过，如果用 select + delete 来校验 token，存在并发问题，不建议使用。

4、悲观锁：悲观锁使用时一般伴随事务一起使用，数据锁定时间可能会很长，根据实际情况选用（另外还要考虑 id 是否为主键，如果 id不是主键或者不是 InnoDB 存储引擎，那么就会出现锁全表）。

```mysql
select id ,name from table_# where id='##' for update;
```

5、乐观锁，给数据库表增加一个 version 字段，可以通过这个字段来判断是否已经被修改了

```mysql
update table_xxx set name=#name#,version=version+1 where version=#version#
```

6、分布式锁，比如 Redis 、 Zookeeper 的分布式锁。单号为 key，然后给 Key 设置有效期（防止支付失败后，锁一直不释放），来一个请求使用订单号生成一把锁，业务代码执行完成后再释放锁。

7、保底方案，先查询是否存在此单，不存在进行支付，存在就直接返回支付结果。



### 2、简单一次完整的 HTTP 请求所经历的步骤？

1、 DNS 解析(通过访问的域名找出其 IP 地址，递归搜索)。

2、HTTP 请求，当输入一个请求时，建立一个 Socket 连接发起 TCP的 3 次握手。

> 如果是 HTTPS 请求，会略微有不同。

3.1、客户端向服务器发送请求命令（一般是 GET 或 POST 请求）。

> 这个是补充内容，面试一般不用回答。
>
> 客户端的网络层不用关心应用层或者传输层的东西，主要做的是通过查找路由表确定如何到达服务器，期间可能经过多个路由器，这些都是由路由器来完成的工作，我不作过多的描述，无非就是通过查找路由表决定通过那个路径到达服务器。
>
> 客户端的链路层，包通过链路层发送到路由器，通过邻居协议查找给定 IP 地址的 MAC 地址，然后发送 ARP 请求查找目的地址，如果得到回应后就可以使用 ARP 的请求应答交换的 IP 数据包现在就可以传输了，然后发送 IP 数据包到达服务器的地址。

3.2、客户端发送请求头信息和数据。

4.1、服务器发送应答头信息。

4.2、服务器向客户端发送数据。

5、服务器关闭 TCP 连接（4次挥手）。

> 这里是否关闭 TCP 连接，也根据 HTTP Keep-Alive 机制有关。同时，客户端也可以主动发起关闭 TCP 连接。

6、客户端根据返回的 HTML 、 CSS 、 JS 进行渲染。

![](https://dream-syz.github.io/12-1.png)



### 3、说说你对分布式事务的理解？

**场景**：多个服务或者多个库，要保持在一个事务中。

分布式事务是企业集成中的一个技术难点，也是每一个分布式系统架构中都会涉及到的一个东西，特别是在微服务架构中，几乎可以说是无法避免。

**理论**：ACID、CAP、BASE。

**ACID**

指数据库事务正确执行的四个基本要素：

1. 原子性（Atomicity）
2. 一致性（Consistency）
3. 隔离性（Isolation）
4. 持久性（Durability）

**CAP：cp，ap。**

CAP 原则又称 CAP 定理，指的是在一个分布式系统中，一致性（Consistency）、可用性（Availability）、分区容忍性（Partition tolerance）。CAP 原则指的是，这三个要素最多只能同时实现两点，不可能三者兼顾。

- 一致性：在分布式系统中的所有数据备份，在同一时刻是否同样的值。

- 可用性：在集群中一部分节点故障后，集群整体是否还能响应客户端的读写请求。

- 分区容忍性：以实际效果而言，分区相当于对通信的时限要求。系统如果不能在时限内达成数据一致性，就意味着发生了分区的情况，必须就当前操作在 C 和 A 之间做出选择。

**BASE 理论**

BASE 理论是对 CAP 中的一致性和可用性进行一个权衡的结果，理论的核心思想就是：我们无法做到强一致，但每个应用都可以根据自身的业务特点，采用适当的方式来使系统达到最终一致性。

Basically Available（基本可用）

Soft state（软状态）

Eventually consistent（最终一致性）

**解决方案**：**seata**，消息队列 + 本地事件表，事务消息，最大努力通知方案，tcc

SAGA



### 4、你知道哪些分布式事务解决方案？

我目前知道的有五种：

1. 两阶段提交（2 PC）
2. 三阶段提交（3 PC）
3. 补偿事务（TCC = Try-Confirm-Cancel）
4. 本地消息队列表（MQ）
5. Sagas 事务模型（最终一致性）

说完上面五种，面试官一般都会继续问下面这几个问题（可能就问一两个，也可能全部问）。



### 5、什么是两阶段提交？

两阶段提交 2 PC 是分布式事务中最强大的事务类型之一

**流程：**

两段提交就是分两个阶段提交：

- **第一阶段** 询问各个事务数据源是否准备好，投票阶段。

- **第二阶段** 才真正将数据提交给事务数据源。

为了保证该事务可以满足 ACID，就要引入一个协调者（Cooradinator）。其他的节点被称为参与者（Participant）。协调者负责调度参与者的行为，并最终决定这些参与者是否要把事务进行提交。

处理流程如下：

![](https://dream-syz.github.io/12-2.png)

**阶段一**

a) 协调者向所有参与者发送事务内容，询问是否可以提交事务，并等待答复。

b) 各参与者执行事务操作，将 undo 和 redo 信息记入事务日志中（但不提交事务）。

c) 如参与者执行成功，给协调者反馈 yes，否则反馈 no。

**阶段二**

如果协调者收到了参与者的失败消息或者超时，直接给每个参与者发送回滚(rollback)消息；否则，发送提交(commit)消息。

两种情况处理如下：

**情况1：**当所有参与者均反馈 yes，提交事务

a) 协调者向所有参与者发出正式提交事务的请求（即 commit 请求）。

b) 参与者执行 commit 请求，并释放整个事务期间占用的资源。

c) 各参与者向协调者反馈 ack（应答）完成的消息。

d) 协调者收到所有参与者反馈的 ack 消息后，即完成事务提交。

**情况2：**当有一个参与者反馈 no，回滚事务

a) 协调者向所有参与者发出回滚请求（即 rollback 请求）。

b) 参与者使用阶段 1 中的 undo 信息执行回滚操作，并释放整个事务期间占用的资源。

c) 各参与者向协调者反馈 ack 完成的消息。

d) 协调者收到所有参与者反馈的 ack 消息后，即完成事务。

**问题：**

1. 性能问题：所有参与者在事务提交阶段处于同步阻塞状态，占用系统资源，容易导致性能瓶颈。

2) 可靠性问题：如果协调者存在单点故障问题，或出现故障，提供者将一直处于锁定状态。
3) 数据一致性问题：在阶段 2 中，如果出现协调者和参与者都挂了的情况，有可能导致数据不一致。

优点：尽量保证了数据的强一致，适合对数据强一致要求很高的关键领域。（其实也不能 100 % 保证强一致）。

缺点：实现复杂，牺牲了可用性，对性能影响较大，不适合高并发高性能场景。

**优化：**案例：seata，lcn，tcc

1. 性能问题：所有参与者在事务提交阶段处于同步阻塞状态，占用系统资源，容易导致性能瓶颈。

2) 可靠性问题：如果协调者存在单点故障问题，或出现故障，提供者将一直处于锁定状态。
3) 数据一致性问题：在阶段 2 中，如果出现协调者和参与者都挂了的情况，有可能导致数据不一致。

优点：尽量保证了数据的强一致，适合对数据强一致要求很高的关键领域。（其实也不能 100 % 保证强一致）。

缺点：实现复杂，牺牲了可用性，对性能影响较大，不适合高并发高性能场景。



### 6、什么是三阶段提交？

三阶段提交是在二阶段提交上的改进版本，3 PC 最关键要解决的就是协调者和参与者同时挂掉的问题，所以 3 PC 把 2 PC 的准备阶段再次一分为二，这样三阶段提交。

处理流程如下 ：

![](https://dream-syz.github.io/12-3.png)

**阶段一**

a) 协调者向所有参与者发出包含事务内容的 canCommit 请求，询问是否可以提交事务，并等待所有参与者答复。

b) 参与者收到 canCommit 请求后，如果认为可以执行事务操作，则反馈 yes 并进入预备状态，否则反馈 no。

**阶段二**

协调者根据参与者响应情况，有以下两种可能。

**情况 1：**所有参与者均反馈 yes，协调者预执行事务

a) 协调者向所有参与者发出 preCommit 请求，进入准备阶段。

b) 参与者收到 preCommit 请求后，执行事务操作，将 undo 和 redo 信息记入事务日志中（但不提交事务）。

c) 各参与者向协调者反馈 ack 响应或 no 响应，并等待最终指令。

**情况 2：**只要有一个参与者反馈 no，或者等待超时后协调者尚无法收到所有提供者的反馈，即中断事务

a) 协调者向所有参与者发出 abort 请求。

b) 无论收到协调者发出的 abort 请求，或者在等待协调者请求过程中出现超时，参与者均会中断事务。

**阶段三**

该阶段进行真正的事务提交，也可以分为以下两种情况。

**情况 1：**所有参与者均反馈 ack 响应，执行真正的事务提交

a) 如果协调者处于工作状态，则向所有参与者发出 do Commit 请求。

b) 参与者收到 do Commit 请求后，会正式执行事务提交，并释放整个事务期间占用的资源。

c) 各参与者向协调者反馈 ack 完成的消息。

d) 协调者收到所有参与者反馈的 ack 消息后，即完成事务提交。

**情况 2：**只要有一个参与者反馈 no，或者等待超时后协调组尚无法收到所有提供者的反馈，即回滚事务。

a) 如果协调者处于工作状态，向所有参与者发出 rollback 请求。

b) 参与者使用阶段 1 中的 undo 信息执行回滚操作，并释放整个事务期间占用的资源。

c) 各参与者向协调组反馈 ack 完成的消息。

d) 协调组收到所有参与者反馈的 ack 消息后，即完成事务回滚。

**优点：**相比二阶段提交，三阶段提交降低了阻塞范围，在等待超时后协调者或参与者会中断事务。避免了协调者单点问题。阶段 3 中协调者出现问题时，参与者会继续提交事务。

**缺点：**数据不一致问题依然存在，当在参与者收到 preCommit 请求后等待 do commite 指令时，此时如果协调者请求中断事务，而协调者无法与参与者正常通信，会导致参与者继续提交事务，造成数据不一致。



### 7、什么是补偿事务？

TCC （Try Confirm Cancel）是服务化的二阶段编程模型，采用的补偿机制：

![](https://dream-syz.github.io/12-4.png)

TCC 其实就是采用的补偿机制，其核心思想是：针对每个操作，都要注册一个与其对应的确认和补

偿（撤销）操作。

它分为三个步骤：

- Try 阶段主要是对业务系统做检测及资源预留。

- Confirm 阶段主要是对业务系统做确认提交，Try阶段执行成功并开始执行 Confirm阶段时，默认 Confirm阶段是不会出错的。即：只要Try成功，Confirm一定成功。

- Cancel 阶段主要是在业务执行错误，需要回滚的状态下执行的业务取消，预留资源释放。

举个例子，假入你要向 老田 转账，思路大概是： 我们有一个本地方法，里面依次调用步骤： 1、首先在 Try 阶段，要先调用远程接口把 你 和 老田 的钱给冻结起来。 2、在 Confirm 阶段，执行远程调用的转账的操作，转账成功进行解冻。 3、如果第2步执行成功，那么转账成功，如果第二步执行失败，则调用远程冻结接口对应的解冻方法 (Cancel)。

**优点：**

性能提升：具体业务来实现控制资源锁的粒度变小，不会锁定整个资源。

数据最终一致性：基于 Confirm 和 Cancel 的幂等性，保证事务最终完成确认或者取消，保证数据的一致性。

可靠性：解决了 XA 协议的协调者单点故障问题，由主业务方发起并控制整个业务活动，业务活动管理器也变成多点，引入集群。

**缺点：**TCC 的 Try、Confirm 和 Cancel 操作功能要按具体业务来实现，业务耦合度较高，提高了开发成本。



### 8、消息队列是怎么实现的？

**本地消息表（异步确保）**

本地消息表这种实现方式应该是业界使用最多的，其核心思想是将分布式事务拆分成本地事务进行处理，这种思路是来源于 ebay。我们可以从下面的流程图中看出其中的一些细节：

![](https://dream-syz.github.io/12-5.png)

基本思路就是：

消息生产方，需要额外建一个消息表，并记录消息发送状态。消息表和业务数据要在一个事务里提交，也就是说他们要在一个数据库里面。然后消息会经过MQ发送到消息的消费方。如果消息发送失败，会进行重试发送。

消息消费方，需要处理这个消息，并完成自己的业务逻辑。此时如果本地事务处理成功，表明已经处理成功了，如果处理失败，那么就会重试执行。如果是业务上面的失败，可以给生产方发送一个业务补偿消息，通知生产方进行回滚等操作。

生产方和消费方定时扫描本地消息表，把还没处理完成的消息或者失败的消息再发送一遍。如果有靠谱的自动对账补账逻辑，这种方案还是非常实用的。

这种方案遵循 **BASE 理论**，采用的是最终一致性，笔者认为是这几种方案里面比较适合实际业务场景的，即不会出现像 2 PC 那样复杂的实现(当调用链很长的时候，2 PC 的可用性是非常低的)，也不会像 TCC 那样可能出现确认或者回滚不了的情况。

**优点：** 一种非常经典的实现，避免了分布式事务，实现了最终一致性。在 .NET中 有现成的解决方案。

**缺点：** 消息表会耦合到业务系统中，如果没有封装好的解决方案，会有很多杂活需要处理。

**MQ** **事务消息**有一些第三方的MQ是支持事务消息的，比如RocketMQ，他们支持事务消息的方式也是类似于采用的二阶段提交，但是市面上一些主流的MQ都是不支持事务消息的，比如 RabbitMQ 和 Kafka 都不支持。

以阿里的 RocketMQ 中间件为例，其思路大致为：

第一阶段 Prepared 消息，会拿到消息的地址。 第二阶段执行本地事务，第三阶段通过第一阶段拿到的地址去访问消息，并修改状态。

也就是说在业务方法内要想消息队列提交两次请求，一次发送消息和一次确认消息。如果确认消息发送失败了 RocketMQ 会定期扫描消息集群中的事务消息，这时候发现了 Prepared 消息，它会向消息发送者确认，所以生产方需要实现一个 check 接口，RocketMQ 会根据发送端设置的策略来决定是回滚还是继续发送确认消息。这样就保证了消息发送与本地事务同时成功或同时失败

![](https://dream-syz.github.io/12-6.png)

遗憾的是，RocketMQ 并没有 .NET 客户端。

**优点：** 实现了最终一致性，不需要依赖本地数据库事务。

**缺点：** 实现难度大，主流MQ不支持，没有.NET客户端，RocketMQ 事务消息部分代码也未开源。



### 9、说说 Saga 事务模型

Saga 模式是一种分布式异步事务，一种最终一致性事务，是一种柔性事务，有两种不同的方式来实现 saga 事务，最流行的两种方式是：

**一、 事件 / 编排 Choreography ：没有中央协调器（没有单点风险）时，每个服务产生并聆听其他服务的事件，并决定是否应采取行动。**

该实现第一个服务执行一个事务，然后发布一个事件。该事件被一个或多个服务进行监听，这些服务再执行本地事务并发布（或不发布）新的事件，当最后一个服务执行本地事务并且不发布任何事件时，意味着分布式事务结束，或者它发布的事件没有被任何 Saga 参与者听到都意味着事务结束。

![](https://dream-syz.github.io/12-7.png)

**处理流程说明：**

订单服务保存新订单，将状态设置为 pengding 挂起状态，并发布名为 ORDER_CREATED_EVENT的事件。

支付服务监听 ORDER_CREATED_EVENT，并公布事件 BILLED_ORDER_EVENT。

库存服务监听 BILLED_ORDER_EVENT，更新库存，并发布 ORDER_PREPARED_EVENT。

货运服务监听 ORDER_PREPARED_EVENT，然后交付产品。最后，它发布 ORDER_DELIVERED_EVENT。

最后，订单服务侦听 ORDER_DELIVERED_EVENT 并设置订单的状态为 concluded 完成。

假设库存服务在事务过程中失败了。进行回滚：

库存服务产生 PRODUCT_OUT_OF_STOCK_EVENT 订购服务和支付服务会监听到上面库存服务的这一事件：

①支付服务会退款给客户。

②订单服务将订单状态设置为失败。

**优点：**事件 / 编排是实现 Saga 模式的自然方式； 它很简单，容易理解，不需要太多的努力来构建，所有参与者都是松散耦合的，因为他们彼此之间没有直接的耦合。如果您的事务涉及 2 至 4 个步骤，则可能是非常合适的。

**二、 命令 / 协调 orchestrator：中央协调器负责集中处理事件的决策和业务逻辑排序。**

saga 协调器 orchestrator 以命令 / 回复的方式与每项服务进行通信，告诉他们应该执行哪些操作。

![](https://dream-syz.github.io/12-8.png)

订单服务保存pending状态，并要求订单Saga协调器（简称OSO）开始启动订单事务。

OSO向收款服务发送执行收款命令，收款服务回复Payment Executed消息。

OSO向库存服务发送准备订单命令，库存服务将回复OrderPrepared消息。

OSO向货运服务发送订单发货命令，货运服务将回复Order Delivered消息。

OSO订单 Saga 协调器必须事先知道执行“创建订单”事务所需的流程(通过读取 BPM 业务流程 XML 配置获得)。如果有任何失败，它还负责通过向每个参与者发送命令来撤销之前的操作来协调分布式的回滚。当你有一个中央协调器协调一切时，回滚要容易得多，因为协调器默认是执行正向流程，回滚时只要执行反向流程即可。

**优点：**

避免服务之间的循环依赖关系，因为 saga 协调器会调用 saga 参与者，但参与者不会调用协调器。

集中分布式事务的编排。

只需要执行命令/回复(其实回复消息也是一种事件消息)，降低参与者的复杂性。

在添加新步骤时，事务复杂性保持线性，回滚更容易管理。如果在第一笔交易还没有执行完，想改变有第二笔事务的目标对象，则可以轻松地将其暂停在协调器上，直到第一笔交易结束。



### 10、Java 中分布式 ID 的设计方案

**系统环境：**

- JDK 版本：1.8

**示例地址：**

- [Java 中的 Snowflake 生成分布式 ID 示例](https://github.com/my-dlq/blog-example/tree/master/java/java-distributed-id-snowflake)

**参考地址：**

- [滴滴 Tinyid](https://github.com/didi/tinyid)
- [美团 Leaf](https://github.com/Meituan-Dianping/Leaf)
- [百度 Uid-Generator](https://github.com/baidu/uid-generator)
- [百度百科-UUID](https://baike.baidu.com/item/UUID/5921266?fr=aladdin)
- [雪花算法 Java 实现](https://github.com/beyondfengyu/SnowFlake)
- [美团分布式ID生成服务开源 Leaf](https://tech.meituan.com/2019/03/07/open-source-project-leaf.html)
- [大型互联网公司分布式 ID 方案总结](https://www.cnblogs.com/nicerblog/p/11466442.html)



#### 1、什么是分布式 ID

分布式 ID 是指，在分布式环境下可用于对数据进行标识且易存储的全局唯一的 ID 标识。



#### 2、为什么需要分布式 ID

在互联网业务中，一套系统往往都是非常复杂的，很多时候都需要对大量数据进行处理与标识。例如，支付宝、美团、大众点评、京东商城、淘宝商城等等，都需要关联商品编号、用户下单单号、快递的快递号这样的数据，都需要使用一个唯一的 ID 进行标识。不仅如此，也随着分布式系统的流行，ID 号也是向这个方向分布式方向靠拢。

如何生成一个全局唯一的，且有序的 ID 对这些数据进行标识，成为分布式系统中很重要的考虑内容。



#### 3、分布式 ID 需要满足的条件

分布式 ID 是我们在非常多的场景下用到的组件，对其要求比较高，其一般需要满足以下条件：

- **① 唯一性：** 必须保证 ID 是全局性唯一的，这是基本要求。
- **② 高性能：** 高可用低延时，ID 生成速度要快，否则反倒会成为业务瓶颈；
- **③ 高可用：** 尽量保证服务的可用性，多实例化，避免因一个实例挂掉影响整个业务应用的运行。
- **④ 容易接入：** 要秉着拿来即用的设计原则，在系统设计和实现上要尽可能的简单，避免增加开发人员的使用成本。
- **⑤ 趋势递增：** 最好趋势递增，这样方便进行数据排序、过滤，当然这个要求还需要根据具体的业务场景作出安排。
- **⑥ 信息安全：** 如果 ID 是连续递增的，恶意用户就可以很容易的推测出订单号的规则，从而猜出下一个订单号，如果是竞争对手，就可以直接知道我们一天的订单量。所以在某些场景下，需要 ID 无规则来保证安全性。



#### 4、常用分布式 ID 生成方案

下面列出的这几种方案都是生成 ID 的常用方法：

- **(1)、使用 UUID 生成 ID；**
- **(2)、使用数据库单机自增生成 ID；**
- **(3)、使用数据库集群模式自增生成 ID；**
- **(4)、使用数据库号段模式生成 ID；**
- **(5)、使用 Redis 单节点实现生成 ID；**
- **(6)、使用 Redis 集群模式实现生成 ID；**
- **(7)、根据 Snowflake 算法生成 ID；**
- **(8)、使用 Zookeeper 生成 ID；**
- **(9)、使用 MongoDB 创建 ObjectID 生成 ID；**

每种方案的生成 ID 的性能、可用性、有序性等都不相同，接下来将对这些方案进行简单介绍，只有了解其原理后才能结合实际业务需求，找到适合的解决方案。



#### 方案一：使用 UUID 生成 ID

##### UUID 是什么

UUID 是通用唯一识别码 Universally Unique Identifier 的缩写，开放软件基金会（OSF）对其规范进行定义，规定该组成元素中可以包含网卡 MAC 地址、时间戳、名字空间、随机或伪随机数、时序等元素，利用这些元素组合进而生成 UUID。



##### UUID 的结构组成

UUID 一般是由一组 32 位数的 16 进制数字所构成，常包含时间戳和 MAC 地址信息这些元素，以连字号 "-" 分隔成五组进行显示， 标准的 UUID 格式为：

```bash
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

即"8-4-4-4-12"共 32 个英数字母和 4 个连字号"-"组成，下面的字符串就是一个 UUID 的样例：

```bash
550e8400-e29b-41d4-a716-446655440000
```



##### Java 中使用 UUID 工具生成 ID

在 Java 中对 UUID 的生成也有默认实现的方法，如下代码：

```java
public class UUIDTest {

    public static void main(String[] args) {
        // 生成 UUID
        String uuid = UUID.randomUUID().toString();
        // 输出 UUID 串
        System.out.println(uuid);
    }

}
```



##### UUID 作为分布式 ID 的优缺点

**优点：**

- 高性能
- 实现简单
- 不需要数据库等第三方组件依赖

**缺点：**

- 并不是趋势递增，不方便排序
- 生成的 ID 只能用字符串类型存储，占用空间大
- 生成的串没有规律，出问题时不易根据 ID 进行排查



#### 方案二：使用数据库单机自增生成 ID

##### 单机数据库是如何生成 ID

数据库自增 ID 是依赖数据库中提供的自动增量功能实现，这种生成 ID 的方案比较容易实现与使用。在这种方案中，为了存储生成的 ID 值，往往我们会单独创建立一张专用于存储生成 ID 的表，然后往表中插入数据替换旧数据，过程中 ID 会递增，我们只需要查询该递增的 ID 值，然后再与时间戳、随机值等元素进行组合处理，生成分布式 ID。



##### 数据库实现 ID 递增生成的过程

如果使用的数据库是 Mysql，则可以按下面步骤进行 ID 自增。

![](https://dream-syz.github.io/12-9.png)

创建用于存储 ID 的表，其结构如下：

- id：自增生成的 ID 值；
- stub：用于记录 ID 是归属于业务的；

```sql
CREATE TABLE `myid` (
    id bigint(20) unsigned NOT NULL auto_increment,
    stub char(1) NOT NULL default '',
    PRIMARY KEY (id)
)
```

每当我们的应用需要 ID 的时，就插入一条数据使其自增 ID，然后替换旧数据，最后读取新生成的 ID 值：

```sql
begin;
REPLACE INTO `myid`(stub) VALUES ('a');
SELECT LAST_INSERT_ID();
commit;
```

等取到 ID 值后我们在让其与"时间戳"、"随机值"、"业务码"等组合，生成与业务挂钩的分布式 ID 串，一般时候我们生成的串都不会超过 64 位，以方便用 long 类型存储该串；



##### 数据库生成分布式 ID 方案的优缺点

**优点：**

- 实现简单；
- 有序递增；

**缺点：**

- 性能较差，只能并发量小的业务需求；
- 存在单点问题，如果数据库不可用将导致依赖它的服务不能正常执行逻辑。



#### 方案三：使用数据库集群模式自增生成 ID

##### 数据库集群模式实现分布式 ID

前面讲述了单机数据库方式通过自增方式生成 ID，这种方式由于单机部署，不论是性能还是可用性都无法得到保障。故而往往都不会直接采用该方案，而是对其进行改动，将其改为使用多主的集群模式部署，利用多个数据库来进行自增生成 ID。

使用多台数据库会导致每个数据库的 ID 都是从 1 开始递增，且递增步长为 1，在这种情况下一定会生成重复的 ID 值。解决这种 ID 重复生成的问题也很简单，只需要对每个数据库都提前配置好其初始值（auto_increment_increment），以数据库个数充当自增长步长（auto_increment_offset），这样每个库中增长的 ID 就不会重复了。

![](https://dream-syz.github.io/12-10.png)

例如，在存在多个 Mysql 数据库，下面根据主节点格式分配"初始值"和"增长步长"的表如下：

| 参数/库个数 | 2个主数据库          | 3个主数据库                     |
| ----------- | -------------------- | ------------------------------- |
| 初始化值    | **db1=1**、**db2=2** | **db1=1**、**db2=2**、**db3=3** |
| 增长步长    | **2**                | **3**                           |

- 初始值： 1~n（数据库个数）
- 增长步长： n（数据库个数）



##### 双主数据库配置示例

比如，在使用双主的数据库中，两个数据库可以按下面进行数据库配置"初始值"和"递增步长"，如下：

```sql
## 第1个 Mysql 数据库中配置如下
SET @@auto_increment_offset = 1;     -- 初始化值
SET @@auto_increment_increment = 2;  -- 增长步长

## 第2个 Mysql 数据库中配置如下
SET @@auto_increment_offset = 2;     -- 初始化值
SET @@auto_increment_increment = 2;  -- 增长步长
```

这样两个 MySQL 实例的自增 ID 分别就是：

```bash
## 第1个数据库：
1,3,5,7,9

## 第2个数据库：
2,4,6,8,10
```



##### 使用数据库集群模式自增生成 ID 的优缺点

**优点：**

- 高可用；
- 趋势递增；

**缺点：**

- 性能一般，只能并发量小的业务需求；
- 水平扩展比较麻烦，需要手动调整集群数据库中的初始值与步长；



#### 方案四：使用数据库号段模式自增生成 ID

##### 数据库号段模式生成 ID 如何实现

号段模式一般也是基于数据库自增实现分布式 ID 的一种方式，是当下分布式 ID 生成方式中比较流行的一种，其使用可以简单理解为每次从数据库中获取生成的 ID 号段范围，将范围数据获取到应用本地后，在范围内循递增生成一批 ID，然后将这批数据存入缓存。

每次应用需要获取 ID 时，这时就候就可以从缓存中读取 ID 数据，当缓存中的 ID 消耗到一定数目时候，这时再去从数据库中读取一个号段范围，再执行生成一批 ID 操作存入缓存，这是一个重复循环的过程，这样重复操作每次都只是从数据库中获取待生成的 ID 号段范围，而不是一次次获取数据库中生成的递增 ID，这样减少对数据库的访问次数，大大提高了 ID 的生成效率。

![](https://dream-syz.github.io/12-11.png)

##### 数据库号段模式生成 ID 实现流程

在使用号码模式时，我们通常会先建立一张表用于记录上述的 ID 号段范围，一般表内容如下：

```sql
CREATE TABLE `myid` (
  id int(10) NOT NULL AUTO_INCREMENT,
  max_id bigint(20) NOT NULL,
  step int(20) NOT NULL,
  biz_type int(20) NOT NULL,   
  version int(20) NOT NULL,  
  PRIMARY KEY (`id`)
) 
```

- max_id：当前最大可用 ID。
- step：号段的步长。
- bit_type：业务类型。
- version：记录更新的版本号，主要作用是乐观锁，每次更新时都会更新该值，以保证并发时数据的正确性。

每次从数据库中获取号段 ID 的范围时，都会执行更新语句，其中计算新号段范围最大值 `max_id` 的公式是 `max_id + step` 组成，所以 SQL 中设置 `max_id` = `max_id+step` 来执行更新语句，更新数据库中这个范围最大值 `max_id`，然后再通过查询语句查询更新后 ID 最大值，再根据最大值 `max_id` 与步长 `step` 计算出待生成的 ID 的范围，其中操作的 SQL 如下：

```sql
begin
UPDATE `myid` SET `max_id` = max_id + step, `version` = version + 1 WHERE `version` = {执行更新的版本号} AND `biz_type` = {业务类型}
SELECT `max_id`, `step`, `version` FROM `myid` WHERE `biz_type` = {业务类型}
commit
```

其中数据库表中内容大同小异，如下是某个测试数据的样式：

| id   | max_id | step | biz_type      | version |
| ---- | ------ | ---- | ------------- | ------- |
| 1    | 2000   | 1000 | pay_ordertag  | 2       |
| 2    | 10000  | 2000 | test_ordertag | 5       |



##### 数据库号段模式生成 ID 过程描述

例如，某个业务需要批量获取 ID，首先它往数据库 `myid` 中插入一条初始化值，设置 `max_id=0` 和步长 `step=1000` 及使用该 ID 的业务标识 `biz_type=test` 与版本 `version=0`，如下：

```sql
INSERT INTO `myid`(`max_id`,`step`,`biz_type`,`version`) VALUES(0,1000,"test",0) 
```

然后可以观察到数据库中多了一条数据：

| id   | max_id | step | biz_type | version |
| ---- | ------ | ---- | -------- | ------- |
| 1    | **0**  | 1000 | test     | 0       |

然后执行获取分布式 ID 的方法，方法中应执行下面语句进行号段更新，方便生成新的一批号段：

```sql
begin
UPDATE `myid` SET `max_id` = max_id + step, `version` = version + 1 WHERE `version` = 0 AND `biz_type` = test
SELECT `max_id`, `step`, `version` FROM `myid` WHERE `biz_type` = test
commit
```

这时候数据库中的值为：

| id   | max_id   | step | biz_type | version |
| ---- | -------- | ---- | -------- | ------- |
| 1    | **1000** | 1000 | test     | 1       |

然后以 `biz_type` 作为筛选条件，从数据库 `myid` 中读取 `max_id` 与 `step` 的值:

```bash
- max_id：0
- step：1000
```

通过这两个值可以知道号段范围为 `(0,1000]`，生成该批 ID 存入到缓存中，那么这时候缓存大小为：

- 缓存大小：1000

每次都从缓存中取值，创建一个监听器用于监听缓存中 ID 消耗比例，设置阈值，判断如果取值超过的阈值后就进行数据库号段更新操作，跟上面第一次执行更新时候一样，也是执行下面的更新 SQL 语句：

```sql
begin
UPDATE `myid` SET `max_id` = max_id + step, `version` = version + 1 WHERE `version` = 0 AND `biz_type` = test
SELECT `max_id`, `step`, `version` FROM `myid` WHERE `biz_type` = test
commit
```

比如，设置阈值为 `50%`，当缓存中存在 `1000` 个 ID，监听器监听到业务应用已经消耗到 `500` 个后（超过阈值），创建一个新的线程去执行上面的更新 SQL 语句，让数据库中号段范围按照设置的 `step` 扩大，然后获取新的号段最大值，应用中再生成一批范围为 `(1001,2000]` 范围的 ID 存入缓存供应用使用，这时候缓存中数据大小为：

- 缓存大小：2000（已经使用 500，可用 1500）

过程是个循环的过程，每到消耗到一定数据后就会生成新的一批。这里只是对其进行了简单介绍，很多时候为了保证数据库可用性都会采用集群模式，现在通过号码模式生成 ID 的开源框架有很多，比如：

- 美团开源的 [Leaf](https://github.com/Meituan-Dianping/Leaf)
- 滴滴开源的 [TinyId](https://github.com/didi/tinyid)



##### 数据库号段模式生成 ID 的优缺点

**优点：**

- 趋势递增；
- 使用缓存机制，容灾性高，即使数据库不可用还能撑一段时间。
- 可以自定义每次扩展的大小，控制 ID 生成速度；
- 可以设置生成 ID 的初始范围，方便业务从原有的 ID 方式上迁移过来。

**缺点：**

- 数据库宕机会造成整个系统不可用。
- ID 号码不够随机，可能够泄露发号数量的信息，不太安全。

所以，采用这种方案我们也经常使用数据库多主模式，保证数据库的高可用性。



#### 方案五：使用 Redis 单节点实现分布式 ID

##### Redis 中实现分布式 ID 方法

Redis 中存在原子操作指令 INCR 或 INCRBY，执行后可用于创建初始化值或者在原有数字基础上增加指定数字，并返回执行 INCR 命令之后 key 的值，这样就可以很方便的创建有序递增的 ID。

![](https://dream-syz.github.io/12-12.png)

- INCR： 将 key 中储存的数字值增一，如果 key 不存在，那么 key 的值会先被初始化为 0 ，然后再执行 INCR 操作。
- INCRBY： 将 key 中储存的数字加上指定的增量值，如果 key 不存在，那么 key 的值会先被初始化为 0 ，然后再执行 INCR 操作。

获取到有序递增的值后，我们会对该值进行"时间戳"+"随机值"+"Redis递增编号"处理，组合方式多种多样，这里说的只是其中一种方式。



##### Java 中操作 Redis 生成 ID 示例

在 Java 中可以使用 Jedis 组件操作 Redis，可以 Maven 中引入 Jedis 相关包：

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.3.0</version>
</dependency>
```

然后使用 INCR 命令生成 ID：

```java
public class RedisTest {

    public static void main(String[] args) {
        // 创建 Jedis 客户端
        Jedis jedis = new Jedis("127.0.0.1", 6379);
        // 执行 INCR 并获取 ID 值
        long id = jedis.incr("myid");
        // 输出 ID 值（正常情况下会执行'时间戳'+'随机值'+'Redis ID'进行组合使用)
        System.out.println(id);
        // 关闭连接
        jedis.close();
    }

}
```

这里只是简单介绍如何获取 INCR 命令自增的 ID 值，拿到 ID 后如何与时间戳、随机值等拼接组合，这点还需要考虑。



##### Redis 单节点实现分布式 ID 的优缺点

**优点：**

- 实现简单；
- 有序递增，方便排序；

**缺点：**

- 强依赖于 Redis，可能存在单点问题；
- 如果 Redis 超时，可能会对业务造成影响；
- 占用宽带，而且需要考虑网络延时等问题带来地性能冲击。



#### 方案六：使用 Redis 集群实现分布式 ID

##### 为什么使用 Redis 集群模式

使用 Redis 单机生成 ID 存在性能瓶颈，无法满足高并发的业务需求，且一旦 Redis 崩溃或者服务器宕机，那么将导致整个基于它的服务不可用，这是业务中难以忍受的，所以一般时候会用集群的方式来实现 Redis 的分布式 ID 方案。



##### Redis 集群模式下如何实现分布式 ID

![](https://dream-syz.github.io/12-13.png)



使用集群的方式需要设置提前设置 `初始值` 和 `步长` 来保证每个节点增加的 ID 不会冲突，正常做法每个节点都配置一个跟节点挂钩的 `Lua` 脚本，脚本内容中设置好对应节点的 `初始值` 和 `步长`，其中初始值是按照节点个数从 1 开始递增分配，而步长则是等于集群中 Master 节点的个数。按照这种方式生成 ID 并获取后，后面的执行逻辑跟单节点 Redis 一样，都是对 ID 进行加工处理操作。

假如 Redis 集群中存在 3 台 Master 节点，那么就可以针对每个节点的 Lua 脚本中配置初始值和步长，如下：

| 节点名称       | 初始值 | 步长 |
| -------------- | ------ | ---- |
| Redis-Master-1 | 1      | 3    |
| Redis-Master-2 | 2      | 3    |
| Redis-Master-3 | 3      | 3    |

初始化每台 Redis 的值分别是 1、2、3，然后步长都是 3（步长=集群节点数），那么各个 Redis 生成的 ID 为：

- A：1,4,7,10,...
- B：2,5,8,11,...
- C：3,6,9,12,...

然后应用中获取到该 ID 值后与时间戳、随机值等组合处理，生成一个唯一 ID。



##### Redis 集群模式下生成分布式 ID 的优缺点

Redis 集群方案和单机比，ID 的有序递增变为趋势递增，且集群模式保证了可用性。

**优点：**

- 集群模式，高可用；
- 趋势递增，方便分类、排序；

**缺点：**

- 如果 Redis 超时，可能会对业务造成影响；
- 存在网络开销，集群模式需要数据同步，对性能有影响；
- 集群规模固定后，改动规则影响很大，所以扩展比较困难；



#### 方案七：利用 Snowflake 算法实现 ID 生成

##### Snowflake 算法是什么

Snowflake 算法是 Twitter 开源的分布式 ID 生成算法，我们常称其为雪花算法。其生产的结果 ID 是一个 64bit 的 ID，能够很方便的使用 long 类型进行存储。其核心组成是使用 41bit 作为毫秒数，10bit 作为机器的 ID（5个bit是数据中心，5个bit的机器ID），12bit 作为毫秒内的序列号。



##### Snowflake 算法的组成

![](https://dream-syz.github.io/12-14.png)



其结构组成：

- **1 位的正负标识位：** 由于 long 基本类型在 Java 中是带符号的，整数为 0 负数为 1，一般生成的 ID 都为正数，所以固定为0;
- **41 位的时间戳：** 该时间戳为毫秒级，时间戳不是存储当前时间的时间戳，而是存储时间的差值（当前时间-固定的开始时间），这里的的开始时间戳为我们的ID生成器开始使用的时间，通过计算（1L << 41) / (1000L * 60 * 60 * 24 * 365）得出69，说明该算法生成的 ID 足够我们使用69年。
- **10 位的 WorkerID：** 一般是5位数据中心标识与 5 位机器标识共同组成，以区分不同的集群节点，相当于能在1024个节点上生成 ID。
- **12 位的序列号：** 在同一机器同一毫秒内可生成不同的序列号，12位支持最多能生成4096个。

雪花算法效率很高，理论上其生成 ID 的 QPS 约为 409.6w/s，这种分配方式可以保证在任何一个机房的任何一台机器在任意毫秒内生成的 ID 都是不同的。



##### Snowflake 算法的扩展位

在实际使用过程中，我们往往都会根据具体的业务对雪花算法的组成进行改动，常改动的是10bit的 WorkerID 位置，该位置由5位数据中心标识与5位机器标识共同组成，那么这时候可以：

- 如果部署的服务都在同一个数据中心，即不考虑数据中心概念，可以将5bit数据中心为替换成我们的业务编码。
- 如果数据中心不是很多，这时候可以将5bit数据中心位拆成3bit+2bit，其中3bit为数据中心标识，2bit为业务编码，可以设置该值为随机值，放置别人猜测 ID 号。

还有很多拆分方法，这里省略，请大家根据业务需求进行拆分。



##### Snowflake 算法的不足点

根据上面介绍，已经对雪花算法有了大概的了解，不过雪花算法中部分由时间戳组成，所以其强依赖机器时钟，如果机器上时钟回拨，会导致发号重复或者服务会处于不可用状态。

为了解决这个问题，网上给出了很多方案：

- **① 关闭时钟同步：** 将ID生成交给少量服务器，并关闭时钟同步；
- **② 抛出异常：** 直接抛出异常，交给上层业务处理。
- **③ 短时间等待：** 如果回拨时间较短，在耗时要求内，比如 5ms，那么可以让时钟等待一小段时间，时间到达后再次进行判断，如果已经超过回拨前的时间则正常执行逻辑，否则接着抛出异常。
- **④ 使用扩展位预防时钟回拨：** 如果回拨时间很长，那么无法等待，可以调整算法占用的64位，将 1~2位作为回拨位，一旦时钟回拨将回拨位+1，可得到不一样的ID，2位回拨位允许标记三次时钟回拨，基本够使用。如果超出了，再选择抛出异常。

其中比较推荐的就是使用上面介绍的雪花算法扩展位，如利用 WorkerID 作为扩展位，可以让这 10bit 预留出 2bit，让其作为回滚的标识，当发生时钟回拨时候使其值 +1，由于是 2bit 预留位，所以支持最多三次回拨，一般来说够用，毕竟时钟回拨几率比较小，当然如果还发生了，且超过三次后只能抛出进行处理了。



##### Snowflake 的 Java 实现示例

这里提供两种方式在 Java 中使用 Snowflake 生成分布式 ID，第一种是使用现成封装好的工具 Hutool，其对 Snowflake 进行了封装，可以直接使用。另一种是自己写代码实现 Snowflake，这种方式可以灵活配置其中的位数分配。

**方式一：使用 Hutool 工具封装的 Snowflake 工具**

通过 Maven 引入 Hutool 工具包：

```xml
<dependency>
    <groupId>cn.hutool</groupId>
    <artifactId>hutool-all</artifactId>
    <version>5.4.2</version>
</dependency>
```



使用 Hutool 中提供的 Snowflake 工具：

```java
public class SnowflakeHutool {

    public static void main(String[] args) {
        // 实例化生成 ID 工具对象
        Snowflake snowflake = IdUtil.getSnowflake(1, 3);
        long id = snowflake.nextId();
    }

}
```



**方式二：自己写代码实现 Snowflake 生成 ID 工具**

```java
import java.util.ArrayList;
import java.util.HashSet;
import java.util.List;

/**
 * 手动实现 Snowflake 生成 ID 逻辑
 *
 * @author mydlq
 */
public class Snowflake {

    /** 机器id（5位）*/
    private final long machineId;
    /** 数据中心id（5位）*/
    private final long datacenterId;
    /** 序列号（12位） */
    private long sequence = 0L;

    /** 初始时间戳 */
    private final long INIT_TIMESTAMP = 1288834974657L;
    /** 机器id位数 */
    private final long MAX_MACHINE_ID_BITS = 5L;
    /** 数据中心id位数 */
    private final long DATACENTER_ID_BITS = 5L;

    /** 机器id最大值 */
    private final long MAX_MACHINE_Id = -1L ^ (-1L << MAX_MACHINE_ID_BITS);
    /** 数据中心id最大值 */
    private final long MAX_DATACENTER_ID = -1L ^ (-1L << DATACENTER_ID_BITS);
    /** 序列号id最大值 */
    private final long SEQUENCE_BITS = 12L;
    /** 序列号最大值 */
    private final long sequenceMask = -1L ^ (-1L << SEQUENCE_BITS);

    /** workerid需要左移的位数（12位） */
    private final long WORKER_ID_SHIFT = SEQUENCE_BITS;
    /** 数据id需要左移位数(12序列号)+(5机器id)共17位 */
    private final long DATACENTER_ID_SHIFT = SEQUENCE_BITS + MAX_MACHINE_ID_BITS;
    /** 时间戳需要左移位数(12序列号)+(5机器id)+(5数据中心id)共22位 */
    private final long TIMESTAMP_LEFT_SHIFT = SEQUENCE_BITS + MAX_MACHINE_ID_BITS + DATACENTER_ID_BITS;

    /** 上次时间戳，初始值为负数 */
    private long lastTimestamp = -1L;

    /**
     * 构造方法，进行初始化检测
     *
     * @param machineId    机器ID
     * @param datacenterId 数据ID
     */
    public Snowflake(long machineId, long datacenterId) {
        // 检查数(机器ID)是否大于5或者小于0
        if (machineId > MAX_MACHINE_Id || machineId < 0) {
            throw new IllegalArgumentException(String.format("机器id不能大于 %d 或者小于 0", MAX_MACHINE_Id));
        }
        // 检查数(据中心ID)是否大于5或者小于0
        if (datacenterId > MAX_DATACENTER_ID || datacenterId < 0) {
            throw new IllegalArgumentException(String.format("数据中心id不能大于 %d 或者小于 0", MAX_DATACENTER_ID));
        }
        // 配置参数
        this.machineId = machineId;
        this.datacenterId = datacenterId;
    }

    /**
     * 获取下一个生成的分布式 ID
     *
     * @return 分布式 ID
     */
    public synchronized long nextId() {
        // 获取当前时间戳
        long currentTimestamp = timeGen();
        //获取当前时间戳如果小于上次时间戳，则表示时间戳获取出现异常
        if (currentTimestamp < lastTimestamp) {
            // 等待 10ms，如果时间回拨时间短，能在 10ms 内恢复，则正常生产 ID，否则抛出异常
            long offset = lastTimestamp - currentTimestamp;
            if (offset <= 10) {
                try {
                    wait(offset << 1);
                    if (currentTimestamp < lastTimestamp) {
                        throw new RuntimeException("系统时间被回调，无法生成ID");
                    }
                } catch (InterruptedException e) {
                    Thread.currentThread().interrupt();
                    throw new RuntimeException("系统时间被回调，无法生成ID，且等待中断");
                }
            }
        }
        // 判断当前时间戳是否等于上次生成ID的时间戳（同1ms内），是则进行序列号递增+1，如果递增到设置的最大值（默认4096）则等待
        if (lastTimestamp == currentTimestamp) {
            sequence = (sequence + 1) & sequenceMask;
            if (sequence == 0) {
                currentTimestamp = tilNextMillis(lastTimestamp);
            }
        }
        // 如果当前时间戳大于上次生成ID的时间戳，说明已经进入下一毫秒，则设置序列化ID为0
        else {
            sequence = 0;
        }
        // 设置最后时间戳为当前时间戳
        lastTimestamp = currentTimestamp;
        // 生成 ID 并返回结果：
        // (currStamp - INIT_TIMESTAMP) << TIMESTAMP_LEFT_SHIFT     时间戳部分
        // datacenterId << DATACENTER_ID_SHIFT                      数据中心部分
        // machineId << WORKER_ID_SHIFT                             机器标识部分
        // sequence                                                 序列号部分
        return ((currentTimestamp - INIT_TIMESTAMP) << TIMESTAMP_LEFT_SHIFT) |
                (datacenterId << DATACENTER_ID_SHIFT) |
                (machineId << WORKER_ID_SHIFT) |
                sequence;
    }

    /**
     * 当某一毫秒时间内产生的ID数超过最大值则进入等待，
     * 循环判断当前时间戳是否已经变更到下一毫秒，
     * 是则返回最新的时间戳
     *
     * @param lastTimestamp 待比较的时间戳
     * @return 当前时间戳
     */
    private long tilNextMillis(long lastTimestamp) {
        long timestamp = timeGen();
        while (timestamp <= lastTimestamp) {
            timestamp = timeGen();
        }
        return timestamp;
    }

    /**
     * 获取系统当前时间
     *
     * @return 系统当前时间（毫秒）
     */
    private long timeGen() {
        return System.currentTimeMillis();
    }

    /**
     * 测试 main 方法
     */
    public static void main(String[] args) {
        // 实例化生成 ID 工具对象
        Snowflake worker = new Snowflake(1, 3);
        // 创建用于存储 id 的集合
        List<Long> idList = new ArrayList<>();
        // 标记开始时间
        long start = System.currentTimeMillis();
        // 设置 1000ms 内循环生成 ID
        while (System.currentTimeMillis() - start <= 1000) {
            // 生成 ID 加入集合
            idList.add(worker.nextId());
        }
        // 输出1s内生成ID的数量
        System.out.println("生成 ID 总数量：" + idList.size());
    }

}
```



##### Snowflake 生成分布式 ID 的优缺点

**优点：**

- 高性能；
- 趋势递增；
- 可以灵活调整结构；
- 不需要第数据库等第三方组件依赖；

**缺点：**

- 强依赖时钟，可能发生时钟回拨导致生成的 ID 重复。



#### 方案八：使用 Zookeeper 生成 ID

##### Zookeeper 中如何生成 ID 值

在 Zookeeper 中主要通过节点数据版本号来生成序列号，可以生成 32 位和 64 位的数据版本号，客户端可以使用这个版本号来作为唯一的序列号。在 Zookeeper 中本身就是支持集群模式，所以能保证高可用性，且生成的 ID 为趋势递增且有序，不过在实际使用中很少用 Zookeeper 来充当 ID 生成器，因为 Zookeeper 中存在强一致性，在高并发场景下其性能可能很难满足需求。

![](https://dream-syz.github.io/12-15.png)



不过由于使用 Zookeeper 节点的版本号来充当 ID 号是比较繁琐，需要创建节点获取生成的 ID，然后去掉节点命令前缀，只截取数字部分，最后还要异步执行删除节点（启动新的线程执行删除节点操作，防止占用生成ID线程执行的实际）。过程比较耗时且繁琐，所以，在操作 Zookeeper 时经经常不会采用该方案，常使用 Curator 客户端提供的基于乐观锁的计数器来自增实现 ID 生成，这个过程和数据库自增生成 ID 类似。



##### Java 操作 Zookeeper 生成 ID 的实现

在 Java 中可以使用 Curator 包操作 Zookeeper，引入的 Maven 包如下：

```xml
<dependency>
    <groupId>org.apache.curator</groupId>
    <artifactId>curator-recipes</artifactId>
    <version>5.0.0</version>
</dependency>
<!-- 创建 Zookeeper 客户端依赖，一定要和 Zookeeper Server 版本保持一致 -->
<dependency>
    <groupId>org.apache.zookeeper</groupId>
    <artifactId>zookeeper</artifactId>
    <version>3.6.0</version>
</dependency>
```

然后使用 curator 工具提供的 increment 方法，使计数器递增，获取递增后的值：

```java
public class ZookeeperIdExample {

    /** 
     * 原子递增器对象 
     */
    private DistributedAtomicLong distributedAtomicLong;

    /**
     * 初始化操作
     */
    @PostConstruct
    public void init() {
        // 初始化参数（Zookeeper 地址、Session 超时时间、节点路径、重试策略）
        String zkServer = "127.0.0.1:2181";
        int sessionTimeoutMs = 10000;
        String counterNode = "/distribute_id";
        RetryPolicy retryPolicy = new ExponentialBackoffRetry(1000, 10);
        // 创建 CuratorFramework 对象并启动客户端
        CuratorFramework cf = CuratorFrameworkFactory.builder()
                .connectString(zkServer)
                .sessionTimeoutMs(sessionTimeoutMs)
                .retryPolicy(retryPolicy)
                .build();
        cf.start();
        // 初始化原子递增器对象
        distributedAtomicLong = new DistributedAtomicLong(cf, counterNode, retryPolicy);
    }

    /**
     * 获取 ID 号
     */
    public Long generateId() {
        Long generateId = null;
        try {
            // 计数器 +1
            AtomicValue<Long> sequence = distributedAtomicLong.increment();
            // 判断获取序列号是否是成功的
            if (sequence.succeeded()) {
                generateId = sequence.postValue();
            } else {
                throw new RuntimeException("生成 ID 异常");
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        // 返回 ID 号
        return generateId;
    }

    public static void main(String[] args) {
        // 方法类初始化
        ZookeeperIdExample zookeeperIdExample = new ZookeeperIdExample();
        zookeeperIdExample.init();
        // 设置存储生成的 ID 的 List 集合
        List<Long> idList = new ArrayList<>();
        // 设置开始时间，该时间作为起始时间
        long start = System.currentTimeMillis();
        // 统计 10s 内生成的 ID 数量
        while (System.currentTimeMillis() - start < 10000){
            // 生成 ID
            Long id = zookeeperIdExample.generateId();
            System.out.println("生成的 ID = " + id);
            // 加入集合
            idList.add(id);
        }
        System.out.println("大小：" + idList.size());
    }

}
```



##### 使用 Zookeeper 生成 ID 的优缺点

**优点：**

- 高可用；
- 趋势递增；

**缺点：**

- 性能差；
- 定期删除之前生成的节点，比较繁琐；



#### 方案九：使用 MongoDB 创建 ObjectID 生成 ID

##### MongoDB 中如何生成 ID 值

在 MongoDB 中每插入一条数据且没有指定 ID 就会生成一个 `_id` 键作为唯一标识，该键默认是 ObjectID 串，常常可以类似于像数据库插入数据一样往 MongoDB 中插入数据，获取其默认生成的 ObjectID 值来充当分布式 ID。

##### MongoDB 的 ObjectId 的组成

在 MongoDB 中默认生成的 ObjectId（十六进制）是由一个 12bit 组成的BSON，ObjectID 是一个 12bit 的 BSON 类型数据，组成类似于雪花算法，如下图就是一个 ObjectID 的组成图：

![](https://dream-syz.github.io/12-16.png)



结构组成如下：

- 4字节时间戳，以 Unix 纪元以来的秒数为单位（精确到秒)。
- 5字节随机数。
- 3字节递增计数器，初始化为随机值，它能确保相同进程同一秒产生的 ObjectId 也是不一样的。同一秒钟最多允许每个进程拥有2563（16 777 216）个不同的 ObjectId。

如下，就是一个十六进制组成的 ObjectID 示例：

```bash
5f55e4dddd2ab03204e13167
```

其中 16 进制转 10 进制如下：

| 组成             | 十六进制   | 十进制       |
| ---------------- | ---------- | ------------ |
| 时间戳           | 5f55e4d    | 1599464669   |
| 随机数           | dd2ab03204 | 949903962628 |
| 递增计算器随机值 | e13167     | 14758247     |



##### Java 中操作 MongoDB 生成 ID 的实现

在 Java 中可以使用 MongoDB 官方提供的 MongoDB Java 驱动操作 MongoDB，可以如下引入 Maven 包：

```xml
<dependency>
    <groupId>org.mongodb</groupId>
    <artifactId>mongo-java-driver</artifactId>
    <version>3.12.7</version>
</dependency>
```

然后使用插入一条数据生成 ID：

```java
public class MongoExample {

    public static void main(String[] args) {
        //连接 MongoDB 服务器，端口号为 27017
        MongoClient mongoClient = new MongoClient("127.0.0.1", 27017);
        // 获取数据库（如果就创建不存在就创建）
        MongoDatabase dbTest = mongoClient.getDatabase("test");
        // 插入一条文档数据（如果就创建不存在就创建）
        Document doc = new Document();
        dbTest.getCollection("myid").insertOne(doc);
        // 获取 ID 值
        ObjectId id = (ObjectId) doc.get("_id");
        // 输出 ID 值
        System.out.println(id);
    }

}
```



##### 使用 MongoDB 的生成分布式 ID 优缺点

**优点：**

- 实现简单；
- 集群模式易于扩展，没有单点问题；

**缺点：**

- 性能一般，只能并发量小的业务需求；



#### 5、流行的分布式 ID 开源框架

现在网上有很多分布式 ID 开源框架，这里比较常用的有：

- **滴滴 Tinyid：** https://github.com/didi/tinyid
- **美团 Leaf：** https://github.com/Meituan-Dianping/Leaf
- **百度 Uid-Generator：** https://github.com/baidu/uid-generator

这里个开源框架中，大都是按照上面介绍的这几种分布式 ID 生成方案原理开发的，如：

- **滴滴 Tinyid：** 数据库号段模式
- **百度 Uid-Generator：** 数据库 + Snowflake
- **美团 leaf：** 数据库号段模式、Zookeeper + Snowflake

这几款性能都不错，他们的 github 和相关博客中都有详细的介绍，这里我就不过多叙述，感兴趣的博友自行去查阅相关资料。根据本人分析，这几张流行的开源分布式 ID 的实现都做了如下操作：

- (1)、减少网络延迟，没有使用 Zookeeper、Redis 等作为分布式锁的核心组件；
- (2)、可以灵活配置生成的 ID，可以在其中添加跟业务挂钩的业务号，以满足不同业务需求；
- (3)、大部分考虑的是高可用方案，组成统一分布式 ID 分发组件，且组成集群模式，保证可用性；
- (4)、将生成的 ID 存入缓存，这样相当于提前往缓存中存入了一批数据，能防止并发突增导致 ID 需求大，也能防止数据库突然不可用。
- (5)、都会设置一个监控器和异步更新缓存中分布式 ID 的多个线程，监控器会监控缓存中的使用比例，达到一定比例后会通知更新缓存的线程执行更新分布式 ID 任务，这样会再往缓存中放入一批可以数据。



#### 6、总结：各种方案的优缺点对比

对部分方案进行了简单测试，由于没有精细的配置组件环境和参数所以这里的数据不一定准确，只供参考：

| 方案名称                                 | 性能   | ID生成速度(单位：s) |
| ---------------------------------------- | ------ | ------------------- |
| 使用数据库号段模式生成 ID                | 非常高 | 100000000+          |
| 使用 Snowflake 生成 ID                   | 很高   | 4000000+            |
| 使用 UUID 生成 ID                        | 高     | 710000+             |
| 使用 MongoDB 创建 ObjectID 生成 ID       | 一般   | 1500+               |
| 使用 Redis 的 INCR 或 INCRBY 命令生成 ID | 一般   | 2000+               |
| 使用 Zookeeper 的节点 ID                 | 差     | 600+                |
| 使用数据库自增生成 ID                    | 差     | 300+                |

根据上面比较，还是比较推荐使用 `号段模式` 与 `Snowflake` 两种方案用于生成分布式 ID，具体还是得根据业务实际来选择不同方案。



### 11、幂等解决方法有哪些？

什么是幂等？

- 常见描述：对于相同的请求应该返回相同的结果，所以查询类接口是天然的幂等性接口。

- 真正的回答方式：幂等指的是相同请求（identical request）执行一次或者多次所带来的副作用（side-effects）是一样的。

什么常见会出现幂等？

- 前端调后端接口发起支付超时，然后再次发起重试。可能会导致多次支付。

- Dubbo 中也有重试机制。

- 页面上多次点击。

我们想要的是：接口的幂等性实际上就是接口可重复调用，在调用方多次调用的情况下，接口最终得到的结果是一致的 。

解决方案

在插入数据的时候，插入去重表，利用数据库的唯一索引特性，保证唯一的逻辑。

悲观锁，select for update，整个执行过程中锁定该订单对应的记录。注意：这种在 DB 读大于写的情况下尽量少用。先查询后修改数据，并发不高的后台系统，或者一些任务 JOB，为了支持幂等，支持重复执行，简单的处理方法是，先查询下一些关键数据，判断是否已经执行过，在进行业务处理，就可以了。注意：核心高并发流程不要用这种方法。

状态机幂等，在设计单据相关的业务，或者是任务相关的业务，肯定会涉及到状态机，就是业务单据上面有个状态，状态在不同的情况下会发生变更，一般情况下存在有限状态机，这时候，如果状态机已经处于下一个状态，这时候来了一个上一个状态的变更，理论上是不能够变更的，这样的话，保证了有限状态机的幂等。

token机制，防止页面重复提交：

集群环境：采用 token 加 redis（redis 单线程的，处理需要排队）或者单 JVM 环境：采用 token 加 redis 或 token 加 jvm 内存数据提交前要向服务的申请 token，token 放到 redis 或 jvm 内存，设置 token 有效时间，提交后后台校验 token，同时删除 token，生成新的token 返回。token 特点：要申请，一次有效性，可以限流。

全局唯一 ID，如果使用全局唯一 ID，就是根据业务的操作和内容生成一个全局 ID，在执行操作前先根据这个全局唯一 ID 是否存在，来判断这个操作是否已经执行。如果不存在则把全局 ID，存储到存储系统中，比如数据库、redis 等。如果存在则表示该方法已经执行。



### 12、常见负载均衡算法有哪些？

- 轮询

- 加权轮询

- 随机

- 最小连接

- 源地址 Hash



### 13、你知道哪些限流算法？

限流算法有四种常见算法：

#### **计数器算法（固定窗口）**

计数器算法是使用计数器在周期内累加访问次数，当达到设定的限流值时，触发限流策略。下一个周期开始时，进行清零，重新计数。

此算法在单机还是分布式环境下实现都非常简单，使用redis的incr原子自增性和线程安全即可轻松实现。

![](https://dream-syz.github.io/12-17.png)

这个算法通常用于 QPS 限流和统计总访问量，对于秒级以上的时间周期来说，会存在一个非常严重的问题，那就是临界问题，如下图：

![](https://dream-syz.github.io/12-18.png)

假设 1 min 内服务器的负载能力为 100，因此一个周期的访问量限制在 100，然而在第一个周期的最后 5 秒和下一个周期的开始 5 秒时间段内，分别涌入100 的访问量，虽然没有超过每个周期的限制量，但是整体上10 秒内已达到 200 的访问量，已远远超过服务器的负载能力，由此可见，计数器算法方式限流对于周期比较长的限流，存在很大的弊端。



#### 滑动窗口

滑动窗口算法是将时间周期分为 N 个小周期，分别记录每个小周期内访问次数，并且根据时间滑动删除过期的小周期。

如下图，假设时间周期为 1 min，将 1 min 再分为 2 个小周期，统计每个小周期的访问数量，则可以看到，第一个时间周期内，访问数量为 75，第二个时间周期内，访问数量为 100，超过 100 的访问则被限流掉了

![](https://dream-syz.github.io/12-19.png)

由此可见，当滑动窗口的格子划分的越多，那么滑动窗口的滚动就越平滑，限流的统计就会越精确。

此算法可以很好的解决固定窗口算法的临界问题。

#### 漏桶算法

漏桶算法是访问请求到达时直接放入漏桶，如当前容量已达到上限（限流值），则进行丢弃（触发限流策略）。漏桶以固定的速率进行释放访问请求（即请求通过），直到漏桶为空。

![](https://dream-syz.github.io/12-20.png)

#### 令牌桶算法

令牌桶算法是程序以r（r= 时间周期/限流值）的速度向令牌桶中增加令牌，直到令牌桶满，请求到达时向令牌桶请求令牌，如获取到令牌则通过请求，否则触发限流策略

![](https://dream-syz.github.io/12-21.png)



### 14、数据库如何处理海量数据？

对数据库进行：分库分表，主从架构，读写分离。

水平分库 / 分表，垂直分库 / 分表。

- 水平分库/表，各个库和表的结构一模一样。

- 垂直分库/表，各个库和表的结构不一样。

读写分离：主机负责写，从机负责读。



### 15、如何将长链接转换成短链接，并发送短信？

短 URL 从生成到使用分为以下几步：

- 有一个服务,将要发送给你的长 URL 对应到一个短 URL 上.例如 www.baidu.com -> www.t.cn/1。

- 把短 url 拼接到短信等的内容上发送。

- 用户点击短 URL，浏览器用 301 / 302 进行重定向,访问到对应的长 URL。

- 展示对应的内容。



#### 长链接和短链接如何互相转换？

思路是建立一个发号器。每次有一个新的长 URL 进来，我们就增加一。并且将新的数值返回.第一个来的 url 返回 "www.x.cn/0"，第二个返回 "www.x.cn/1"。



#### 长链接和短链接的对应关系如何存储？

如果数据量小且 QPS 低，直接使用数据库的自增主键就可以实现。 还可以将最近/最热门的对应关系存储在 K-V 数据库中,这样子可以节省空间的同时,加快响应速度。



### 16、如何提高系统的并发能力？

- 使用分布式系统。

- 部署多台服务器，并做负载均衡。

- 使用缓存（Redis）集群。

- 数据库分库分表 + 读写分离。

- 引入消息中间件集群。



## 网络篇

### 1、HTTP 响应码有哪些？分别代表什么含义？

- 200：成功，Web 服务器成功处理了客户端的请求。

- 301：永久重定向，当客户端请求一个网址的时候，Web 服务器会将当前请求重定向到另一个网址，搜索引擎会抓取重定向后网页的内容并且将旧的网址替换为重定向后的网址。

- 302：临时重定向，搜索引擎会抓取重定向后网页的内容而保留旧的网址，因为搜索引擎认为重定向后的网址是暂时的。

- 400：客户端请求错误，多为参数不合法导致 Web 服务器验参失败。

- 404：未找到，Web 服务器找不到资源。

- 500：Web 服务器错误，服务器处理客户端请求的时候发生错误。

- 503：服务不可用，服务器停机。

- 504：网关超时。



### 2、**Forward** **和 **Redirect 的区别？

**浏览器 URL 地址：**Forward 是服务器内部的重定向，服务器内部请求某个 servlet，然后获取响应的内容，浏览器的 URL 地址是不会变化的；Redirect 是客户端请求服务器，然后服务器给客户端返回了一个 302 状态码和新的 location，客户端重新发起 HTTP 请求，服务器给客户端响应 location 对应的 URL 地址，浏览器的 URL 地址发生了变化。

**数据的共享：**Forward 是服务器内部的重定向，request 在整个重定向过程中是不变的，request 中的信息在 servlet 间是共享的。Redirect 发起了两次 HTTP 请求分别使用不同的 request。

**请求的次数：**Forward 只有一次请求；Redirect 有两次请求。



### 3、 Get 和 Post 请求有哪些区别？

用途：

- get 请求用来从服务器获取资源

- post 请求用来向服务器提交数据

表单的提交方式：

- get 请求直接将表单数据以 name1=value1&name2=value2 的形式拼接到 URL 上（http://wwwbaidu.com/action?name1=value1&name2=value2），多个参数参数值需要用 & 连接起来并且用 ? 拼接到 action 后面；

- post 请求将表单数据放到请求头或者请求的消息体中。

传输数据的大小限制：

- get 请求传输的数据受到 URL 长度的限制，而 URL 长度是由浏览器决定的；

- post 请求传输数据的大小理论上来说是没有限制的。

参数的编码：

- get 请求的参数会在地址栏明文显示，使用 URL 编码的文本格式传递参数；

- post 请求使用二进制数据多重编码传递参数。

缓存：

- get 请求可以被浏览器缓存被收藏为标签；

- post 请求不会被缓存也不能被收藏为标签。



### 4、**说说** **TCP** **与** **UDP** 的区别，以及各自的优缺点

1、TCP 面向连接（如打电话要先拨号建立连接）：UDP 是无连接的，即发送数据之前不需要建立连接。

2、TCP 提供可靠的服务。也就是说，通过 TCP 连接传送的数据，无差错，不丢失，不重复，且按序到达；UDP 尽最大努力交付，即不保证可靠交付。tcp 通过校验和，重传控制，序号标识，滑动窗口、确认应答实现可靠传输。如丢包时的重发控制，还可以对次序乱掉的分包进行顺序控制。

3、UDP 具有较好的实时性，工作效率比 TCP 高，适用于对高速传输和实时性有较高的通信或广播通信。

4、每一条 TCP 连接只能是点到点的；UDP 支持一对一，一对多，多对一和多对多的交互通信 

5、TCP 对系统资源要求较多，UDP 对系统资源要求较少。



### 5、说一下 HTTP 和 HTTPS 的区别

端口不同：HTTP 和 HTTPS 的连接方式不同没用的端口也不一样，HTTP 是 80， HTTPS 用的是 443

消耗资源：和 HTTP 相比，HTTPS 通信会因为加解密的处理消耗更多的 CPU 和内存资源。

开销： HTTPS 通信需要证书，这类证书通常需要向认证机构申请或者付费购买。



### 6、说说 HTTP、TCP、Socket 的关系是什么？

- TCP / IP 代表传输控制协议 / 网际协议，指的是一系列协议族。

- HTTP 本身就是一个协议，是从 Web 服务器传输超文本到本地浏览器的传送协议。

- Socket 是 TCP/IP 网络的 API ，其实就是一个门面模式，它把复杂的 TCP/IP 协议族隐藏在 Socket 接口后面。对用户来说，一组简单的接口就是全部，让 Socket 去组织数据，以符合指定的协议。

综上所述：

- 需要 IP 协议来连接网络

- TCP 是一种允许我们安全传输数据的机制，使用 TCP 协议来传输数据的 HTTP 是 Web 服务器和客户端使用的特殊协议。

- HTTP 基于 TCP 协议，所以可以使用 Socket 去建立一个 TCP 连接。



### 7、说一下 HTTP 的长连接与短连接的区别

HTTP 协议的长连接和短连接，实质上是 TCP 协议的长连接和短连接。

**短连接**

在 HTTP / 1.0 中默认使用短链接，也就是说，浏览器和服务器每进行一次 HTTP 操作，就建立一次连接，但任务结束就中断连接。如果客户端访问的某个 HTML 或其他类型的 Web 资源，如 JavaScript 文件、图像文件、 CSS 文件等。当浏览器每遇到这样一个Web资源，就会建立一个 HTTP 会话.

**长连接**

从 HTTP / 1.1 起，默认使用长连接，用以保持连接特性。在使用长连接的情况下，当一个网页打开完成后，客户端和服务器之间用于传输 HTTP 数据的 TCP 连接不会关闭。如果客户端再次访问这个服务器上的网页，会继续使用这一条已经建立的连接。Keep-Alive 不会永久保持连接，它有一个保持时间，可以在不同的服务器软件（如 Apache）中设定这个时间。



### 8、TCP 为什么要三次握手，两次不行吗？为什么

- TCP 客户端和服务端建立连接需要三次握手，首先服务端需要开启监听，等待客户端的连接请求，这个时候服务端处于“收听”状态；

- 客户端向服务端发起连接，选择 seq=x 的初始序列号，此时客户端处于“同步已发送”的状态；

- 服务端收到客户端的连接请求，同意连接并向客户端发送确认，确认号是 ack=x+1 表示客户端可以发送下一个数据包序号从 x+1 开始，同时选择 seq=y 的初始序列号，此时服务端处于“同步收到”状态；

- 客户端收到服务端的确认后，向服务端发送确认信息，确认号是 ack=y+1 表示服务端可以发送下一个数据包序号从 y+1 开始，此时客户端处于“已建立连接”的状态；
- 服务端收到客户端的确认后，也进入“已建立连接”的状态。

从三次握手的过程可以看出如果只有两次握手，那么客户端的起始序列号可以确认，服务端的起始序列号将得不到确认。

![](https://dream-syz.github.io/13-1.png)



### 9、说一下 TCP 粘包是怎么产生的？怎么解决粘包问题的？第二种情况发生了明显的粘包现象，这种情况对于数据接收方来说很难处理。

上文中讲 TCP 和 UDP 区别的时候提到 TCP 传输数据基于字节流，从应用层到 TCP 传输层的多个数据包是一连串的字节流是没有边界的，而且 TCP 首部并没有记录数据包的长度，所以 TCP 传输数据的时候可能会发送粘包和拆包的问题；而 UDP 是基于数据报传输数据的，UDP 首部也记录了数据报的长度，可以轻易的区分出不同的数据包的边界。

接下来看下 TCP 传输数据的几种情况，首先第一种情况是正常的，既没有发送粘包也没有发生拆包。

![](https://dream-syz.github.io/13-2.png)

第二种情况发生了明显的粘包现象，这种情况对于数据接收方来说很难处理。

![](https://dream-syz.github.io/13-3.png)

接下来的两种情况发生了粘包和拆包的现象，接收端收到的数据包要么是不完整的要么是多出来一块儿。

![](https://dream-syz.github.io/13-4.png)

造成粘包和拆包现象的原因：

- TCP 发送缓冲区剩余空间不足以发送一个完整的数据包，将发生拆包；

- 要发送的数据超过了最大报文长度的限制，TCP 传输数据时进行拆包；

- 要发送的数据包小于 TCP 发送缓冲区剩余空间，TCP 将多个数据包写满发送缓冲区一次发送出去，将发生粘包；

- 接收端没有及时读取 TCP 发送缓冲区中的数据包，将会发生粘包。

  

粘包拆包的解决方法：

- 发送端给数据包添加首部，首部中添加数据包的长度属性，这样接收端通过首部中的长度字段就可以知道数据包的实际长度啦；

- 针对发送的数据包小于缓冲区大小的情况，发送端可以将不同的数据包规定成同样的长度，不足这个长度的补充 0，接收端从缓冲区读取固定的长度数据这样就可以区分不同的数据包；

- 发送端通过给不同的数据包添加间隔符合确定边界，接收端通过这个间隔符合就可以区分不同的数据包。



### 10、TCP 如何保证可靠性

**序列号和确认号机制：**

TCP 发送端发送数据包的时候会选择一个 seq 序列号，接收端收到数据包后会检测数据包的完整性，如果检测通过会响应一个 ack 确认号表示收到了数据包。

**超时重发机制：**

TCP 发送端发送了数据包后会启动一个定时器，如果一定时间没有收到接受端的确认后，将会重新发送该数据包。

**对乱序数据包重新排序：**

从 IP 网络层传输到 TCP 层的数据包可能会乱序，TCP 层会对数据包重新排序再发给应用层。

**丢弃重复数据：**

从 IP 网络层传输到 TCP 层的数据包可能会重复，TCP 层会丢弃重复的数据包。

**流量控制：**

TCP 发送端和接收端都有一个固定大小的缓冲空间，为了防止发送端发送数据的速度太快导致接收端缓冲区溢出，发送端只能发送接收端可以接纳的数据，为了达到这种控制效果，TCP 用了流量控制协议（可变大小的滑动窗口协议）来实现。



### 11、OSI 的七层模型都有哪些？

OSI七层模型一般指开放系统互连参考模型（Open System Interconnect 简称OSI）是国际标准化组织（ISO）和国际电报电话咨询委员会（CCITT）联合制定的开放系统互连参考模型,为开放式互连信息系统提供了一种功能结构的框架。

- 应用层：各种应用程序协议，比如 HTTP、HTTPS、FTP、SOCKS 安全套接字协议、DNS 域名系统、GDP 网关发现协议等等。

- 表示层：加密解密、转换翻译、压缩解压缩，比如 LPP 轻量级表示协议。

- 会话层：不同机器上的用户建立和管理会话，比如 SSL 安全套接字层协议、TLS 传输层安全协议、RPC 远程过程调用协议等等。传输层：接受上一层的数据，在必要的时候对数据进行分割，并将这些数据交给网络层，保证这些数据段有效到达对端，比如 TCP 传输控制协议、UDP 数据报协议。

- 网络层：控制子网的运行：逻辑编址、分组传输、路由选择，比如 IP、IPV6、SLIP 等等。

- 数据链路层：物理寻址，同时将原始比特流转变为逻辑传输路线，比如 XTP 压缩传输协议、PPTP 点对点隧道协议等等。

- 物理层：机械、电子、定时接口通信信道上的原始比特流传输，比如 IEEE802.2 等等。

#### OSI 这样分层有什么好处？

OSI 分层的好处可以从五个方面讲：

1. 人们可以很容易的讨论和学习协议的规范细节。
2. 层间的标准接口方便了工程模块化。
3. 创建了一个更好的互连环境。
4. 降低了复杂度，使程序更容易修改，产品开发的速度更快。
5. 每层利用紧邻的下层服务，更容易记住个层的功能。



### 12、浏览器中输入：“www.woaijava.com” 之后都发生了什么？请详细阐述

- 由域名 → IP 地址 寻找IP地址的过程依次经过了浏览器缓存、系统缓存、hosts 文件、路由器缓存、 递归搜索根域名服务器。

- 建立 TCP / IP连接（三次握手具体过程）

- 由浏览器发送一个 HTTP 请求

- 经过路由器的转发，通过服务器的防火墙，该 HTTP 请求到达了服务器

- 服务器处理该 HTTP 请求，返回一个 HTML 文件浏览器解析该 HTML 文件，并且显示在浏览器端

这里需要注意：

- HTTP 协议是一种基于 TCP / IP 的应用层协议，进行 HTTP 数据请求必须先建立 TCP / IP 连接可以这样理解：HTTP 是轿车，提供了封装或者显示数据的具体形式；Socket 是发动机，提供了网络通信的能力。

- 两个计算机之间的交流无非是两个端口之间的数据通信，具体的数据会以什么样的形式展现是以不同的应用层协议来定义的。



### 13、如何实现跨域？

当浏览器执行 JS 脚本的时候，会检测脚本要访问的协议、域名、端口号是不是和当前网址一致，如果不一致就是跨域。跨域是不允许的，这种限制叫做浏览器的同源策略，简单点的说法就是浏览器不允许一个源中加载脚本与其他源中的资源进行交互。那么如何实现跨域呢？

JSONP、CORS方式、代理方式

**1 JSONP** **方式**

script、img、iframe、link、video、audio 等带有 src 属性的标签可以跨域请求和执行资源，JSONP 利用这一点 “漏洞” 实现跨域。

```html
<script>
 var scriptTag = document.createElement('script');
 scriptTag.type = "text/javascript";
 scriptTag.src = "http://10.10.0.101:8899/jsonp?callback=f";
 document.head.appendChild(scriptTag);
</script>
```

再看下 jQuery 的写法。

```html
$.ajax({
 // 请求域名
 url:'http://10.10.0.101:8899/login',
 // 请求方式
 type:'GET',
 // 数据类型选择 jsonp
 dataType:'jsonp',
 // 回调方法名
 jsonpCallback:'callback',
});
// 回调方法
function callback(response) {
 console.log(response);
}
```

**2 CORS** **方式**

CORS（Cross-Origin Resource Sharing）即跨域资源共享，需要浏览器和服务器同时支持，这种请求方式分为简单请求和非简单请求。

当浏览器发出的 XMLHttpRequest 请求的请求方式是 POST 或者 GET，请求头中只包含 Accept、Accept-Language、Content-Language、Last-Event-ID、Content-Type（application/x-www form-urlencoded、multipart/form-data、text/plain）时那么这个请求就是一个简单请求。

对于简单的请求，浏览器会在请求头中添加 Origin 属性，标明本次请求来自哪个源（协议 + 域名 + 端口）。

```
GET
// 标明本次请求来自哪个源（协议+域名+端口）
Origin: http://127.0.0.1:8080
// IP
Host: 127.0.0.1:8080
// 长连接
Connection: keep-alive
Content-Type: text/plain
```

如果 Origin 标明的域名在服务器许可范围内，那么服务器就会给出响应：

```
 // 该值上文提到过，表示允许浏览器指定的域名访问，要么为浏览器传入的 origin，要么为 * 表示
所有域名都可以访问
 Access-Control-Allow-Origin: http://127.0.0.1:8080
 // 表示服务器是否同意浏览器发送 cookie
 Access-Control-Allow-Credentials: true
 // 指定 XMLHttpRequest#getResponseHeader() 方法可以获取到的字段
 Access-Control-Expose-Headers: xxx
 Content-Type: text/html; charset=utf-8
```

Access-Control-Allow-Credentials: true 表示服务器同意浏览器发送 cookie，另外浏览器也需要设置支持发送 cookie，否则就算服务器支持浏览器也不会发送

```
var xhr = new XMLHttpRequest();
// 设置发送的请求是否带 cookie
xhr.withCredentials = true;
xhr.open('post', 'http://10.10.0.101:8899/login', true);
xhr.setRequestHeader('Content-Type', 'text/plain');
```

另外一种是非简单请求，请求方式是 PUT 或 DELETE，或者请求头中添加了 ContentType:application/json 属性和属性值的请求。

这种请求在浏览器正式发出 XMLHttpRequest 请求前会先发送一个预检 HTTP 请求，询问服务器当前网页的域名是否在服务器的许可名单之中，只有得到服务器的肯定后才会正式发出通信请求。预检请求的头信息：

```
 // 预检请求的请求方式是 OPTIONS
 OPTIONS
 // 标明本次请求来自哪个源（协议+域名+端口）
 Origin: http://127.0.0.1:8080
 // 标明接下来的 CORS 请求要使用的请求方式
 Access-Control-Request-Method: PUT
 // 标明接下来的 CORS 请求要附加发送的头信息属性
 Access-Control-Request-Headers: X-Custom-Header
 // IP
 Host: 127.0.0.1:8080
 // 长连接
 Connection: keep-alive
```

如果服务器回应预检请求的响应头中没有任何 CORS 相关的头信息的话表示不支持跨域，如果允许跨域就会做出响应，响应头信息如下：

```
HTTP/1.1 200 OK
// 该值上文提到过，表示允许浏览器指定的域名访问，要么为浏览器传入的 origin，要么为 * 表示所
有域名都可以访问
Access-Control-Allow-Origin:http://127.0.0.1:8080
// 服务器支持的所有跨域请求方式，为了防止浏览器发起多次预检请求把所有的请求方式返回给浏览器
Access-Control-Allow-Methods: GET, POST, PUT
// 服务器支持预检请求头信息中的 Access-Control-Request-Headers 属性值
Access-Control-Allow-Headers: X-Custom-Header
// 服务器同意浏览器发送 cookie
Access-Control-Allow-Credentials: true
// 指定预检请求的有效期是 20 天，期间不必再次发送另一个预检请求
Access-Control-Max-Age:1728000
Content-Type: text/html; charset=utf-8
Keep-Alive: timeout=2, max=100
// 长连接
Connection: Keep-Alive
Content-Type: text/plain
```

接着浏览器会像简单请求一样，发送一个 CORS 请求，请求头中一定包含 Origin 属性，服务器的响应头中也一定得包含 Access-Control-Allow-Origin 属性。

**3** **代理方式**

跨域限制是浏览器的同源策略导致的，使用 nginx 当做服务器访问别的服务的 HTTP 接口是不需要执行 JS 脚步不存在同源策略限制的，所以可以利用 Nginx 创建一个代理服务器，这个代理服务器的域名跟浏览器要访问的域名一致，然后通过这个代理服务器修改 cookie 中的域名为要访问的 HTTP 接口的域名，通过反向代理实现跨域。

Nginx 的配置信息：

```properties
server {
 # 代理服务器的端口
 listen 88;
 # 代理服务器的域名
 server_name http://127.0.0.1;
 location / {
 # 反向代理服务器的域名+端口
 proxy_pass http://127.0.0.2:89;
 # 修改cookie里域名
 proxy_cookie_domain http://127.0.0.2 http://127.0.0.1;
 index index.html index.htm;
 # 设置当前代理服务器允许浏览器跨域
 add_header Access-Control-Allow-Origin http://127.0.0.1;
 # 设置当前代理服务器允许浏览器发送 cookie
 add_header Access-Control-Allow-Credentials true;
 }
}
```

前端代码：

```
var xhr = new XMLHttpRequest();
// 设置浏览器允许发送 cookie
xhr.withCredentials = true;
// 访问 nginx 代理服务器
xhr.open('get', 'http://127.0.0.1:88', true);
xhr.send();
```



### 14、TCP 为什么要三次握手，两次不行吗？为什么

- CP 客户端和服务端建立连接需要三次握手，首先服务端需要开启监听，等待客户端的连接请求，这个时候服务端处于“收听”状态；

- 客户端向服务端发起连接，选择 seq=x 的初始序列号，此时客户端处于“同步已发送”的状态；

- 服务端收到客户端的连接请求，同意连接并向客户端发送确认，确认号是 ack=x+1 表示客户端可以发送下一个数据包序号从 x+1 开始，同时选择 seq=y 的初始序列号，此时服务端处于“同步收到”状态；

- 客户端收到服务端的确认后，向服务端发送确认信息，确认号是 ack=y+1 表示服务端可以发送下一个数据包序号从 y+1 开始，此时客户端处于“已建立连接”的状态；

- 服务端收到客户端的确认后，也进入“已建立连接”的状态。

从三次握手的过程可以看出如果只有两次握手，那么客户端的起始序列号可以确认，服务端的起始序列号将得不到确认。

![](https://dream-syz.github.io/13-5.png)



### 15、说一下 **TCP** **粘包是怎么产生的？怎么解决粘包问题的？**

上文中讲 TCP 和 UDP 区别的时候提到 TCP 传输数据基于字节流，从应用层到 TCP 传输层的多个数据包是一连串的字节流是没有边界的，而且 TCP 首部并没有记录数据包的长度，所以 TCP 传输数据的时候可能会发送粘包和拆包的问题；而 UDP 是基于数据报传输数据的，UDP 首部也记录了数据报的长度，可以轻易的区分出不同的数据包的边界。

![](https://dream-syz.github.io/13-6.png)

造成粘包和拆包现象的原因：

- TCP 发送缓冲区剩余空间不足以发送一个完整的数据包，将发生拆包；

- 要发送的数据超过了最大报文长度的限制，TCP 传输数据时进行拆包；

- 要发送的数据包小于 TCP 发送缓冲区剩余空间，TCP 将多个数据包写满发送缓冲区一次发送出去，将发生粘包；

- 接收端没有及时读取 TCP 发送缓冲区中的数据包，将会发生粘包。

粘包拆包的解决方法：

- 发送端给数据包添加首部，首部中添加数据包的长度属性，这样接收端通过首部中的长度字段就可以知道数据包的实际长度啦；

- 针对发送的数据包小于缓冲区大小的情况，发送端可以将不同的数据包规定成同样的长度，不足这个长度的补充 0，接收端从缓冲区读取固定的长度数据这样就可以区分不同的数据包；

- 发送端通过给不同的数据包添加间隔符合确定边界，接收端通过这个间隔符合就可以区分不同的数据包



### 16、HTTP1.0、HTTP1.1、HTTP2.0 的关系和区别

**一，对比**

| HTTP1.0 | 无状态、无连接                                               |
| ------- | ------------------------------------------------------------ |
| HTTP1.1 | 持久连接，请求管道化，增加缓存处理（新的字段如 catch-control ），增加 Host 字段，支持断点传输等（把文件分成几部分） |
| HTTP2.0 | 二进制分帧，多路复用（或连接共享），头部压缩，服务器推送     |

**二、HTTP1.0：**

浏览器的每次请求都需要与服务器建立一个 TCP 连接，服务器处理完成后立即断开 TCP 连接（无连接），服务器不跟踪每个客户端也不记录过去的请求（无状态）。

**三、HTTP1.1：**

HTTP / 1.0 中默认使用 Connection: close。在 HTTP / 1.1 中已经默认使用 Connection: keep-alive，避免了连接建立和释放的开销，但服务器必须按照客户端请求的先后顺序依次回送相应的结果，以保证客户端能够区分出每次请求的响应内容。通过 Content-Length 字段来判断当前请求的数据是否已经全部接收。不允许同时存在两个并行的响应。

**四、HTTP2.0：**

HTTP / 2 引入二进制数据帧和流的概念，其中帧对数据进行顺序标识，如下图所示，这样浏览器收到数据之后，就可以按照序列对数据进行合并，而不会出现合并后数据错乱的情况。同样是因为有了序列，服务器就可以并行的传输数据，这就是流所做的事情。

流（stream） 已建立连接上的双向字节流 消息 与逻辑消息对应的完整的一系列数据帧 帧 HTTP2.0 通信的最小单位，每个帧包含帧头部，至少也会标识出当前帧所属的流（stream id）。 多路复用：

1、所有的HTTP2.0通信都在一个TCP连接上完成，这个连接可以承载任意数量的双向数据流。

2、每个数据流以消息的形式发送，而消息由一或多个帧组成。这些帧可以乱序发送，然后再根据

每个帧头部的流标识符（stream id）重新组装。举个例子，每个请求是一个数据流，数据流以消息的方式发送，而消息又分为多个帧，帧头部记录着 stream id 用来标识所属的数据流，不同属的帧可以在连接中随机混杂在一起。接收方可以根据 stream id 将帧再归属到各自不同的请求当中去。

3、另外，多路复用（连接共享）可能会导致关键请求被阻塞。HTTP2.0 里每个数据流都可以设置优先级和依赖，优先级高的数据流会被服务器优先处理和返回给客户端，数据流还可以依赖其他的子数据流。

4、可见，HTTP 2.0 实现了真正的并行传输，它能够在一个 TCP 上进行任意数量 HTTP 请求。而这个强大的功能则是基于“二进制分帧”的特性。

头部压缩

在 HTTP1.x 中，头部元数据都是以纯文本的形式发送的，通常会给每个请求增加 500~800 字节的负荷。

HTTP 2.0 使用 encoder 来减少需要传输的 header 大小，通讯双方各自 cache 一份 header fields 表，既避免了重复header的传输，又减小了需要传输的大小。高效的压缩算法可以很大的压缩 header，减少发送包的数量从而降低延迟。

服务器推送：

服务器除了对最初请求的响应外，服务器还可以额外的向客户端推送资源，而无需客户端明确的请求。



### 17、说说 HTTP 协议与 TCP / IP 协议的关系

HTTP 的长连接和短连接本质上是 TCP 长连接和短连接。

HTTP 属于应用层协议，在传输层使用 TCP 协议，在网络层使用IP协议。

IP 协议主要解决网络路由和寻址问题，

TCP 协议主要解决如何在IP层之上可靠地传递数据包，使得网络上接收端收到发送端所发出的所有包，并且顺序与发送顺序一致。TCP 协议是可靠的、面向连接的。



### 18、如何理解 HTTP 协议是无状态的？

HTTP 协议是无状态的，指的是协议对于事务处理没有记忆能力，服务器不知道客户端是什么状态。

也就是说，打开一个服务器上的网页和上一次打开这个服务器上的网页之间没有任何联系。HTTP是一个无状态的面向连接的协议，无状态不代表 HTTP 不能保持 TCP 连接，更不能代表 HTTP 使用的是 UDP 协议（无连接）。



### 19、什么是长连接和短连接？

在 HTTP / 1.0 中默认使用短连接。也就是说，客户端和服务器每进行一次 HTTP 操作，就建立一次连接，任务结束就中断连接。当客户端浏览器访问的某个 HTML 或其他类型的 Web 页中包含有其他的 Web 资源（如 JavaScript 文件、图像文件、CSS 文件等），每遇到这样一个Web资源，浏览器就会重新建立一个 HTTP 会话。

而从 HTTP / 1.1 起，默认使用长连接，用以保持连接特性。使用长连接的HTTP协议，会在响应头加入这行代码：

```
Connection:keep-alive
```

在使用长连接的情况下，当一个网页打开完成后，客户端和服务器之间用于传输 HTTP 数据的 TCP 连接不会关闭，客户端再次访问这个服务器时，会继续使用这一条已经建立的连接。Keep-Alive 不会永久保持连接，它有一个保持时间，可以在不同的服务器软件（如Apache）中设定这个时间。实现长连接需要客户端和服务端都支持长连接。

HTTP 协议的长连接和短连接，实质上是 TCP 协议的长连接和短连接。



### 20、长连接和短连接的优缺点？

长连接可以省去较多的 TCP 建立和关闭的操作，减少浪费，节约时间 。对于频繁请求资源的客户来说，较适用长连接。不过这里存在一个问题，存活功能的探测周期太长，还有就是它只是探测 TCP 连接的存活，属于比较斯文的做法，遇到恶意的连接时，保活功能就不够使了。在长连接的应用场景下，client 端一般不会主动关闭它们之间的连接，Client 与 server 之间的连接如果一直不关闭的话，会存在一个问题，随着客户端连接越来越多，server 早晚有扛不住的时候，这时候 server 端需要采取一些策略，如关闭一些长时间没有读写事件发生的连接，这样可 以避免一些恶意连接导致 server 端服务受损；如果条件再允许就可以以客户端机器为颗粒度，限制每个客户端的最大长连接数，这样可以完全避免某个蛋疼的客户端连累后端服务。

短连接对于服务器来说管理较为简单，存在的连接都是有用的连接，不需要额外的控制手段。但如果客户请求频繁，将在 TCP 的建立和关闭操作上浪费时间和带宽



### 21、说说长连接短连接的操作过程

短连接的操作步骤是：建立连接——数据传输——关闭连接...建立连接——数据传输——关闭连接长连接的操作步骤是：建立连接——数据传输...（保持连接）...数据传输——关闭连接



### 22、说说 TCP 三次握手和四次挥手的全过程

**三次握手**

第一次握手：客户端发送 syn 包（syn=x）到服务器，并进入 SYN_SEND 状态，等待服务器确认； 第二次握手：服务器收到 syn 包，必须确认客户的SYN（ack=x+1），同时自己也发送一个 SYN 包（syn=y），即 SYN+ACK 包，此时服务器进入 SYN_RECV 状态； 第三次握手：客户端收到服务器的 SYN＋ACK 包，向服务器发送确认包 ACK（ack=y+1），此包发送完毕，客户端和服务器进入 ESTABLISHED 状态，完成三次握手。 握手过程中传送的包里不包含数据，三次握手完毕后，客户端与服务器才正式开始传送数据。理想状态下，TCP连接一旦建立，在通信双方中的任何一方主动关闭连接之前，TCP 连接都将被一直保持下去。

**四次挥手**

与建立连接的“三次握手”类似，断开一个 TCP 连接则需要“四次握手”。 第一次挥手：主动关闭方发送一个 FIN，用来关闭主动方到被动关闭方的数据传送，也就是主动关闭方告诉被动关闭方：我已经不会再给你发数据了(当然，在fin包之前发送出去的数据，如果没有收到对应的ack确认报文，主动关闭方依然会重发这些数据)，但是，此时主动关闭方还可 以接受数据。 第二次挥手：被动关闭方收到 FIN 包后，发送一个 ACK 给对方，确认序号为收到序号 +1（与 SYN 相同，一个FIN占用一个序号）。 第三次挥手：被动关闭方发送一个 FIN，用来关闭被动关闭方到主动关闭方的数据传送，也就是告诉主动关闭方，我的数据也发送完了，不会再给你发数据了。 第四次挥手：主动关闭方收到 FIN 后，发送一个 ACK 给被动关闭方，确认序号为收到序号 +1，至此，完成四次挥手。



### 23、说说 TCP / IP 四层网络模型

TCP / IP分层模型（TCP/IP Layening Model）被称作因特网分层模型(Internet Layering Model)、因特网参考模型(Internet Reference Model)。下图表示了 TCP / IP 分层模型的四层。

![](https://dream-syz.github.io/13-7.png)

TCP / IP 协议被组织成四个概念层，其中有三层对应于 ISO 参考模型中的相应层。TCP / IP 协议族并不包含物理层和数据链路层，因此它不能独立完成整个计算机网络系统的功能，必须与许多其他的协议协同工作。 TCP / IP 分层模型的四个协议层分别完成以下的功能：

**第一层 网络接口层**

网络接口层包括用于协作IP数据在已有网络介质上传输的协议。

协议：ARP,RARP

**第二层 网络层**

网络层对应于 OSI 七层参考模型的网络层。负责数据的包装、寻址和路由。同时还包含网络控制报文协议（Internet Control Message Protocol,ICMP）用来提供网络诊断信息。

协议：本层包含 IP 协议、RIP 协议（Routing Information Protocol，路由信息协议），ICMP 协议。

**第三层 传输层**

传输层对应于 OSI 七层参考模型的传输层，它提供两种端到端的通信服务。

其中 TCP 协议(Transmission Control Protocol)提供可靠的数据流运输服务，UDP 协议(Use Datagram Protocol)提供不可靠的用户数据报服务。

**第四层 应用层**

应用层对应于 OSI 七层参考模型的应用层和表达层。因特网的应用层协议包括 Finger、Whois、FTP(文件传输协议)、Gopher、HTTP(超文本传输协议)、Telent(远程终端协议)、SMTP(简单邮件传送协议)、IRC(因特网中继会话)、NNTP（网络新闻传输协议）等。



### 24、 说说域名解析详细过程？

1. 浏览器访问 www.baidu.com，询问本地 DNS 服务器是否缓存了该网址解析后的 IP 地址。
2. 如果本地 DNS 服务器没有缓存的话，就去 root-servers.net 根服务器查询该网址对应的 IP 地址。

3. 根服务器返回顶级域名服务器的网址 gtld-servers.net，然后本地 DNS 服务器去顶级域名服务器查询该网址对应的 IP 地址。

4. 顶级域名服务器返回 www.baidu.com 主区域服务器的地址，然后本地 DNS 服务器去 www.baidu.com 主区域服务器查询此域名对应的 IP 地址。

5. 本地 DNS 服务器拿到 www.baidu.com 解析后的 IP 地址后，缓存起来以便备查，然后把解析后的 IP 地址返回给浏览器。



### 25、 **IP** 地址分为几类，每类都代表什么，私网是哪些？

大致上分为公共地址和私有地址两大类，公共地址可以在外网中随意访问，私有地址只能在内网访问只有通过代理服务器才可以和外网通信。

公共地址：

```
1.0.0.1～126.255.255.254
128.0.0.1～191.255.255.254
192.0.0.1～223.255.255.254
224.0.0.1～239.255.255.254
240.0.0.1～255.255.255.254
```

私有地址：

```
10.0.0.0～10.255.255.255
172.16.0.0～172.31.255.255
192.168.0.0～192.168.255.255
```

- 0.0.0.0 路由器转发使用

- 127.x.x.x 保留

- 255.255.255.255 局域网下的广播地址



### 26、说说 TCP 如何保证可靠性的？

**序列号和确认号机制：**

TCP 发送端发送数据包的时候会选择一个 seq 序列号，接收端收到数据包后会检测数据包的完整性，如果检测通过会响应一个 ack 确认号表示收到了数据包。

**超时重发机制：**

TCP 发送端发送了数据包后会启动一个定时器，如果一定时间没有收到接受端的确认后，将会重新发送该数据包。

**对乱序数据包重新排序：**

从 IP 网络层传输到 TCP 层的数据包可能会乱序，TCP 层会对数据包重新排序再发给应用层。

**丢弃重复数据：**

从 IP 网络层传输到 TCP 层的数据包可能会重复，TCP 层会丢弃重复的数据包。

**流量控制：**

TCP 发送端和接收端都有一个固定大小的缓冲空间，为了防止发送端发送数据的速度太快导致接收端缓冲区溢出，发送端只能发送接收端可以接纳的数据，为了达到这种控制效果，TCP 用了流量控制协议（可变大小的滑动窗口协议）来实现。





## 设计模式

创建型模式：单例模式、原型模式、工厂方法模式、抽象工厂模式、建造者模式

结构型模式：代理模式、适配器模式、桥接模式、装饰器模式、外观（门面）模式、享元模式、组合

行为型模式：模板方法模式、策略模式、命令模式、职责链模式、状态模式、观察者模式、中介者模式、迭代器模式、访问者模式、备忘录模式、解耦器模式

设计模式（Design Pattern）是前辈们对代码开发经验的总结，是解决特定问题的一系列套路。它不是语法规定，而是一套用来提高代码可复用性、可维护性、可读性、稳健性以及安全性的解决方案。 11995 年，GoF（Gang of Four，四人组/四人帮）合作出版了《设计模式：可复用面向对象软件的基础》一书，共收录了 23 种设计模式，从此树立了软件设计模式领域的里程碑，人称「GoF设计模式」。

通常面试中会问：说一下你知道哪些设计模式？

这时候，就得需要平时积累下来的经验了，肯定回答自己会的，你只是知道名词是没用用的。从难易程度和常用情况来看可以这么回答：

单例模式、代理模式、模板方法模式、装饰器模式、工厂模式、责任链模式、观察者模式、原型模式。

通常你能回答这么多就已经 ok 了。但是其他模式，可以适当的了解了解，不然面试官问你还有其他设计模式吗？

这时候，你就可以回答名词了，他再问，你就说这些设计模式只是了解过，在工作中用的不是很多。

### 1、说说什么是单例模式

答：单例模式是一种常用的软件设计模式，在应用这个模式时，单例对象的类必须保证只有一个实例存在，整个系统只能使用一个对象实例。

优点：不会频繁地创建和销毁对象，浪费系统资源。

可能这会需要你手写一个单例模式，这就得自己去学了，因为单例模式有很多种写法，懒汉模式，

饿汉模式，双重检查模式等。懒汉模式就是用的时候再去创建对象，饿汉模式就是提前就已经加载好的静态 static 对象，双重检查模式就是两次检查避免多线程造成创建了多个对象。

单例模式有很多种的写法，我总结一下：

（1）饿汉式单例模式的写法：线程安全

（2）懒汉式单例模式的写法：非线程安全

（3）双检锁单例模式的写法：线程安全



### 2、说说你对代理模式的理解

代理模式是给某一个对象提供一个代理，并由代理对象控制对原对象的引用。

**优点**：

- 代理模式能够协调调用者和被调用者，在一定程度上降低了系统的耦合度；

- 可以灵活地隐藏被代理对象的部分功能和服务，也增加额外的功能和服务。

**缺点**：

- 由于使用了代理模式，因此程序的性能没有直接调用性能高；

使用代理模式提高了代码的复杂度。

黄牛卖火车票：没有流行网络购票的年代是很喜欢找黄牛买火车票的，因为工作忙的原因，没时间去买票，然后就托黄牛给你买张回家过年的火车票。这个过程中黄牛就是代理人，火车票就是被代理的对象。

婚姻介绍所：婚姻介绍所的工作人员，搜集单身人士信息，婚介所的工作人员为这个单身人士找对象，这个过程也是代理模式的生活案例。对象就是被代理的对象。

注意了，问代理模式的时候，很有可能会问：动态代理。在 Spring 篇中已经讲述过，如果你把动态代理讲了后，很有可能还会问什么是静态代理？一个洞一个是静，大致也能才出来，就是中间代理层使我们手动写的，通常说的代理模式就是静态代理。



### 3、说说工厂模式

答：简单工厂模式又叫静态工厂方法模式，就是建立一个工厂类，对实现了同一接口的一些类进行实例的创建。比如，一台咖啡机就可以理解为一个工厂模式，你只需要按下想喝的咖啡品类的按钮（摩卡或拿铁），它就会给你生产一杯相应的咖啡，你不需要管它内部的具体实现，只要告诉它你的需求即可。

**优点**：

- 工厂类含有必要的判断逻辑，可以决定在什么时候创建哪一个产品类的实例，客户端可以免除直接创建产品对象的责任，而仅仅“消费”产品；简单工厂模式通过这种做法实现了对责任的割，它提供了专门的工厂类用于创建对象；

- 客户端无须知道所创建的具体产品类的类名，只需要知道具体产品类所对应的参数即可，对于一些复杂的类名，通过简单工厂模式可以减少使用者的记忆量；

- 通过引入配置文件，可以在不修改任何客户端代码的情况下更换和增加新的具体产品类，在一定程度上提高了系统的灵活性。

**缺点**：

- 不易拓展，一旦添加新的产品类型，就不得不修改工厂的创建逻辑；

- 产品类型较多时，工厂的创建逻辑可能过于复杂，一旦出错可能造成所有产品的创建失败，不利于系统的维护。



### 4、抽象工厂模式

答：抽象工厂模式是在简单工厂的基础上将未来可能需要修改的代码抽象出来，通过继承的方式让子类去做决定。比如，以上面的咖啡工厂为例，某天我的口味突然变了，不想喝咖啡了想喝啤酒，这个时候如果直

接修改简单工厂里面的代码，这种做法不但不够优雅，也不符合软件设计的“开闭原则”，因为每次新增品类都要修改原来的代码。这个时候就可以使用抽象工厂类了，抽象工厂里只声明方法，具体的实现交给子类（子工厂）去实现，这个时候再有新增品类的需求，只需要新创建代码即可。



### 5、装饰器模式是什么

答：装饰器模式是指动态地给一个对象增加一些额外的功能，同时又不改变其结构。

优点：装饰类和被装饰类可以独立发展，不会相互耦合，装饰模式是继承的一个替代模式，装饰模式可以动态扩展一个实现类的功能。

装饰器模式的关键：装饰器中使用了被装饰的对象。

比如，创建一个对象 “laowang”，给对象添加不同的装饰，穿上夹克、戴上帽子......，这个执行过程就是装饰者模式。

一句名言：人靠衣裳马靠鞍。都是装饰器模式的生活案列。



### 6、代理模式和装饰器模式有什么区别？

答：都是结构型模式，代理模式重在访问权限的控制，而装饰器模式重在功能的加强。



### 7、模板方法模式

答：模板方法模式是指定义一个算法骨架，将具体内容延迟到子类去实现。

**优点**：

提高代码复用性：将相同部分的代码放在抽象的父类中，而将不同的代码放入不同的子类中；

实现了反向控制：通过一个父类调用其子类的操作，通过对子类的具体实现扩展不同的行为，实现了反向控制并且符合开闭原则。

喝茶茶：烧水----放入茶叶---喝茶。放入的茶叶每个人自己的喜好不一样，有的是普洱、有的是铁观音等。

每日工作：上班打卡----工作---下班打卡。每个人工作的内容不一样，后端开发的、前端开发、测试、产品每个人的工作内容不一样。



### 8、知道享元模式吗？

答：顾名思义就是被共享的单元。享元模式的意图是复用对象，节省内存，前提是享元对象是不可变对象。具体来讲，当一个系统中存在大量重复对象的时候，如果这些重复的对象是不可变对象，我们就可以利用享元模式将对象设计成享元，在内存中只保留一份实例，供多处代码引用。这样可以减少内存中对象的数量，起到节省内存的目的。

典型的使用场景：Integer 中 cache，就是享元模式很经典的实现。

怎么看起来享元模式和单例模式是一毛一样的？面试官很有可能会继续问：



### 9、享元模式和单例模式的区别？

答：单例模式是创建型模式，重在只能有一个对象。而享元模式是结构型模式，重在节约内存使用，提升程序性能。

享元模式：把一个或者多可对象缓存起来，用的时候，直接从缓存里获取。也就是说享元模式不一定只有一个对象。



### 10、说说策略模式在我们生活的场景？

答：策略模式是指定义一系列算法，将每个算法都封装起来，并且使他们之间可以相互替换。

**优点**：遵循了开闭原则，扩展性良好。

**缺点**：随着策略的增加，对外暴露越来越多。

条条大路通罗马，条条大路通北京。

我们去北京的交通方式（策略）很多，比如说坐飞机、坐高铁、自己开车等方式。每一种方式就可以理解为每一种策略。

这就是生活中的策略模式。



### 11、知道责任链模式吗？

答：是行为型设计模式之一，其将链中每一个节点看作是一个对象，每个节点处理的请求均不同，且内部自动维护一个下一节点对象。当一个请求从链式的首端发出时，会沿着链的路径依次传递给每一个节点对象，直至有对象处理这个请求为止。

**优点**

- 解耦了请求与处理；

- 请求处理者（节点对象）只需关注自己感兴趣的请求进行处理即可，对于不感兴趣的请求，直接转发给下一级节点对象；

- 具备链式传递处理请求功能，请求发送者无需知晓链路结构，只需等待请求处理结果；

- 链路结构灵活，可以通过改变链路结构动态地新增或删减责任；

- 易于扩展新的请求处理类（节点），符合 开闭原则；

**缺点**

- 责任链路过长时，可能对请求传递处理效率有影响；

- 如果节点对象存在循环引用时，会造成死循环，导致系统崩溃；

生活案列：我们在公司内部发起一个OA审批流程，项目经理审批、部门经理审批。老板审批、人力审批。这就是生活中的责任链模式，每个角色的责任是不同。

SpringMVC 中的拦截器和 Mybatis 中的插件机制，都是拦截器经典实现。



### 12、了解过适配器模式么？

答：适配器模式是将一个类的接口变成客户端所期望的另一种接口，从而使原本因接口不匹配而无法一起工作的两个类能够在一起工作。

**优点**：

- 可以让两个没有关联的类一起运行，起着中间转换的作用；

- 灵活性好，不会破坏原有的系统。

**缺点**：

- 过多地使用适配器，容易使代码结构混乱，如明明看到调用的是 A 接口，内部调用的却是 B 接口的实现。

生活中的插座，为了适应各种插头，然后上面有两个孔的，三个空的，基本都能适应。还有万能充电器、USB 接口等。这些都是生活中的适配器模式。



### 13、知道观察者模式吗？

答：观察者模式是定义对象间的一种一对多依赖关系，使得每当一个对象状态发生改变时，其相关依赖对象皆得到通知并被自动更新。观察者模式又叫做发布-订阅（Publish/Subscribe）模式、模型-视图（Model/View）模式、源-监听器（Source/Listener）模式或从属者（Dependents）模式。

 **优点**：

- 观察者模式可以实现表示层和数据逻辑层的分离，并定义了稳定的消息更新传递机制，抽象了更新接口，使得可以有各种各样不同的表示层作为具体观察者角色；

- 观察者模式在观察目标和观察者之间建立一个抽象的耦合；

- 观察者模式支持广播通信；

- 观察者模式符合开闭原则（对拓展开放，对修改关闭）的要求。

**缺点**：

- 如果一个观察目标对象有很多直接和间接的观察者的话，将所有的观察者都通知到会花费很多时间；

- 如果在观察者和观察目标之间有循环依赖的话，观察目标会触发它们之间进行循环调用，可能导致系统崩溃；

- 观察者模式没有相应的机制让观察者知道所观察的目标对象是怎么发生变化的，而仅仅只是知道观察目标发生了变化。

  

在观察者模式中有如下角色：

- Subject：抽象主题（抽象被观察者），抽象主题角色把所有观察者对象保存在一个集合里，每个主题都可以有任意数量的观察者，抽象主题提供一个接口，可以增加和删除观察者对象；

- ConcreteSubject：具体主题（具体被观察者），该角色将有关状态存入具体观察者对象，在具体主题的内部状态发生改变时，给所有注册过的观察者发送通知；

- Observer：抽象观察者，是观察者者的抽象类，它定义了一个更新接口，使得在得到主题更改通知时更新自己；

- ConcrereObserver：具体观察者，实现抽象观察者定义的更新接口，以便在得到主题更改通知时更新自身的状态。

在 Spring 中大量的使用的观察者模式，只要看到是以 Event 结尾或者 Publish 开头的基本上都是观察者模式。

上面一共说了 11 种设计模式，这些设计模式能应对绝大多数人和面试场景来说。建议私下自己用代码实现一番，便于更好地理解。



## maven 篇

### 1、什么是 maven ？

maven 主要服务于基于 java 平台的项目构建，依赖管理和项目信息管理。

maven 项目对象模型（POM），可以通过一小段描述信息来管理项目的构建，报告和文档的项目管理工具软件。它包含了一个项目对象模型，一组标准集合，一个项目生命周期，一个依赖管理系统和用来运行定义在生命周期阶段中插件目标的逻辑。当使用 Maven 的时候，你用一个明确定义的项对象模型来描述你的项目，然后 Maven 可以应用横切的逻辑，这些逻辑来自于一组共享的（或自定义的）插件。



### 2、Maven 能为我们解决什么问题？

①添加第三方 jar 包

按照最原始的做法，我们是手动复制 jar 包到项目 WEB-INF / lib 下，每个项目都会有一份，造成大量重复文件。而 Maven 将 jar 包放在本地仓库中统一管理，需要jar包只需要用坐标的方式引用即可。

②jar 包之间的依赖关系

jar 包之间往往不是独立的，很多 jar 需要在其他jar包的支持下才能够正常工作，称为 jar 包之间的依赖关系。如果我们手动去导入，要知道 jar 包之间的依赖关系并一一导入是及其麻烦而且容易出错的。如果使用 Maven，它能够将当前 jar 包所依赖的其他所有 jar 包全部导入。

③获取第三方 jar 包开发过程中我们需要用到很多 jar 包，每个jar包在官网获取的方式不尽相同，给工作带来了额外困难。但是使用Maven 可以以坐标的方式依赖一个 jar 包，Maven 从中央仓库进行下载，并同时下载这个 jar 包依赖的其他 jar 包。

④将项目拆分为多个工程模块

项目的规模越来越大，已经不可能通过 package 结构来划分模块，必须将项目拆分为多个工程协同开发。



### 3、说说 maven 有什么优缺点？

**优点**

- 简化了项目依赖管理

- 易于上手，对于新手来说了解几个常用命令即可满足日常工作

- 便于与持续集成工具（jenkins）整合

- 便于项目升级，无论是项目本身还是项目使用的依赖

- maven 有很多插件，便于功能扩展，比如生产站点，自动发布版本等

**缺点**

- Maven 是一个庞大的构建系统，学习难度大。（很多都可以这样说，入门容易[优点]但是精通难[缺点]）

- Maven采用约定约定优于配置的策略，虽然上手容易但是一旦出现问题，难于调试中网络环境较差，很多 repository 无法访问



### 4、什么是 Maven 的坐标？

Maven 其中一个核心的作用就是管理项目的依赖，引入我们所需的各种 jar 包等。为了能自动化的解析任何一个 Java 构件，Maven 必须将这些Jar包或者其他资源进行唯一标识，这是管理项目的依赖的基础，也就是我们要说的坐标。包括我们自己开发的项目，也是要通过坐标进行唯一标识的，这样才能才其它项目中进行依赖引用。

maven 的坐标通过 groupId，artifactId，version 唯一标志一个构件。groupId 通常为公司或组织名字，artifactId 通常为项目名称，versionId 为版本号。



### 5、讲一下 maven 的生命周期

Maven 的生命周期：从我们的项目构建，一直到项目发布的这个过程。

![](https://dream-syz.github.io/14-1.png)

每个阶段的说明

| 阶段     | 处理     | 描述                                                     |
| -------- | -------- | -------------------------------------------------------- |
| validate | 验证项目 | 验证项目是否正确且所有必须信息是可用的                   |
| compile  | 执行编译 | 源代码编译在此阶段完成                                   |
| test     | 测试     | 使用适当的单元测试框架(（例 JUnit）运行测试。            |
| package  | 打包     | 创建 JAR / WAR 包如在 pom.xml 中定义提及的包             |
| verify   | 检查     | 对集成测试的结果进行检查，以保证质量达标                 |
| install  | 安装     | 安装打包的项目到本地仓库，以供其他项目使用               |
| deploy   | 部署     | 拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程 |



### 6、说说你熟悉哪些 maven 命令？

mvn archetype:generate 创建 Maven 项目

mvn compile 编译源代码

mvn deploy 发布项目

mvn test-compile 编译测试源代码

mvn test 运行应用程序中的单元测试

mvn site 生成项目相关信息的网站

mvn clean 清除项目目录中的生成结果

mvn package 根据项目生成的jar

mvn install 在本地 Repository 中安装 jar

mvn eclipse:eclipse 生成 eclipse 项目文件

mvnjetty:run 启动 jetty 服务

mvntomcat:run 启动 tomcat 服务

mvn clean package -Dmaven.test.skip=true: 清除以前的包后重新打包，跳过测试类



### 7、如何解决依赖传递引起的版本冲突？

可通过 dependency 的 exclusion 元素排除掉依赖。



### 8、说说 maven 的依赖原则

- 最短路径原则（依赖传递的路径越短越优先）

- pom 文件申明顺序优先（路径长度一样，则先申明的优先）
- 覆写原则（当前pom文件里申明的直接覆盖父工程传过来的）
- 父级依赖比子级高



### 9、说说依赖的解析机制？

当依赖的范围是 system 的时候，Maven 直接从本地文件系统中解析构件。

根据依赖坐标计算仓库路径，尝试直接从本地仓库寻找构件，如果发现对应的构件，就解析成功。

如果在本地仓库不存在相应的构件，就遍历所有的远程仓库，发现后，下载并解析使用。

如果依赖的版本是 RELEASE 或 LATEST，就基于更新策略读取所有远程仓库的元数据文件（groupId/artifactId/maven-metadata.xml），将其与本地仓库的对应元合并后，计算出 RELEASE 或者 LATEST 真实的值，然后基于该值检查本地仓库，或者从远程仓库下载。

如果依赖的版本是 SNAPSHOT，就基于更新策略读取所有远程仓库的元数据文件，将它与本地仓库

对应的元数据合并，得到最新快照版本的值，然后根据该值检查本地仓库，或从远程仓库下载。

如果最后解析得到的构件版本包含有时间戳，先将该文件下载下来，再将文件名中时间戳信息删除，剩下 SNAPSHOT 并使用（以非时间戳的形式使用）。



### 10、**说说插件的解析机制**

与依赖的构件一样，插件也是基于坐标保存在 Maven 仓库中。在用到插件的时候会先从本地仓库查找插件，如果本地仓库没有则从远程仓库查找插件并下载到本地仓库。与普通的依赖构件不同的是，Maven 会区别对待普通依赖的远程仓库与插件的远程仓库。前面提到的配置远程仓库只会对普通的依赖有效果。当 Maven 需要的插件在本地仓库不存在时是不会去我们以前配置的远程仓库查找插件的，而是需要有专门的插件远程仓库。



## **ElasticSearch** 篇

### 1、谈谈分词与倒排索引的原理

首先说分词是给检索用的。

英文：一个单词一个词，很简单。I am a student，词与词之间空格分隔。

中文：我是学生，就不能一个字一个字地分，我-是-学生。这是好分的。还有歧义的，使用户放心，使用-户，使-用户。人很容易看出，机器就难多了。所以市面上有各种各样的分词器，一个强调的效率一个强调的准确率。

**倒排索引：倒排针对的是正排。**

1. 正排就是我记得我电脑有个文档，讲了 ES 的常见问题总结。那么我就找到文档，从上往下翻页，找到 ES 的部分。通过文档找文档内容。
2.  倒排：一个 txt 文件 ES 的常见问题 -> D:/分布式问题总结.doc。

所以倒排就是文档内容找文档。当然内容不是全部的，否则也不需要找文档了，内容就是几个分词而已。这里的 txt 就是搜索引擎。



### 2、说说分段存储的思想

Lucene 是著名的搜索开源软件，ElasticSearch 和 Solr 底层用的都是它。

分段存储是 Lucene 的思想。

早期，都是一个整个文档建立一个大的倒排索引。简单，快速，但是问题随之而来。

文档有个很小的改动，整个索引需要重新建立，速度慢，成本高，为了提高速度，定期更新那么时效性就差。

现在一个索引文件，拆分为多个子文件，每个子文件是段。修改的数据不影响的段不必做处理。



### 3、谈谈你对段合并的策略思想的认识

分段的思想大大的提高了维护索引的效率。但是随之就有了新的问题。

每次新增数据就会新增加一个段，时间久了，一个文档对应的段非常多。段多了，也就影响检索性能了。

检索过程：

1. 查询所有短中满足条件的数据
2. 对每个段的结果集合并

所以，定期的对段进行合理是很必要的。真是天下大势，分久必合合久必分。

策略：将段按大小排列分组，大到一定程度的不参与合并。小的组内合并。整体维持在一个合理的大小范围。当然这个大到底应该是多少，是用户可配置的。这也符合设计的思想。



### 4、了解文本相似度 TF-IDF 吗

简单地说，就是你检索一个词，匹配出来的文章，网页太多了。比如 1000 个，这些内容再该怎么呈现，哪些在前面哪些在后面。这需要也有个对匹配度的评分。

TF-IDF 就是干这个的。

- TF = Term Frequency 词频，一个词在这个文档中出现的频率。值越大，说明这文档越匹配，正向指标。

- IDF = Inverse Document Frequency 反向文档频率，简单点说就是一个词在所有文档中都出现，那么这个词不重要。比如“的、了、我、好”这些词所有文档都出现，对检索毫无帮助。反向指标。

```
TF-IDF = TF / IDF
```

复杂的公式，就不写了，主要理解他的思想即可。



### 5、能说说 ElasticSearch 写索引的逻辑吗？

ElasticSearch 是集群的 = 主分片 + 副本分片。

写索引只能写主分片，然后主分片同步到副本分片上。但主分片不是固定的，可能网络原因，之前还是 Node1 是主分片，后来就变成了 Node2 经过选举成了主分片了。

客户端如何知道哪个是主分片呢？ 看下面过程。

1. 客户端向某个节点 NodeX 发送写请求
2. NodeX 通过文档信息，请求会转发到主分片的节点上
3. 主分片处理完，通知到副本分片同步数据，向 Nodex 发送成功信息。
4. Nodex 将处理结果返回给客户端。



### 6、熟悉 ElasticSearch **集群中搜索数据的过程吗？**

1. 客户端向集群发送请求，集群随机选择一个 NodeX 处理这次请求。
2. Nodex 先计算文档在哪个主分片上，比如是主分片 A，它有三个副本 A1，A2，A3。那么请求会轮询三个副本中的一个完成请求。

3. 如果无法确认分片，比如检索的不是一个文档，就遍历所有分片。

补充一点，一个节点的存储量是有限的，于是有了分片的概念。但是分片可能有丢失，于是有了副本的概念。

比如：

ES 集群有 3 个分片，分片 A、分片 B、分片 C，那么分片 A + 分片 B + 分片 C = 所有数据，每个分片只有大概 1/3。分片 A 又有副本 A1 A2 A3，数据都是一样的。



### 7、了解 ElasticSearch 深翻页的问题及解决吗？

Elasticsearch 是一个分布式的开源搜索和分析引擎，可以用于全文搜索、日志分析、数据可视化等应用。在使用 Elasticsearch 进行深翻页时，可能会遇到一些问题，下面是一些常见问题及解决方法：

1. 性能问题：当需要翻到非常深的页数时，性能可能会变得很差。这是因为 Elasticsearch 默认情况下只会返回前 10,000 条结果，如果需要获取更多的结果，可以使用 `from` 和 `size` 参数进行分页查询。但是，随着 `from` 的增加，性能会逐渐下降。为了解决这个问题，可以考虑使用 `search_after` 参数，该参数可以通过上一次查询的最后一个结果来进行分页，避免性能下降。
2. 内存消耗问题：当需要翻到非常深的页数时，Elasticsearch 可能会消耗大量的内存。这是因为 Elasticsearch 默认会缓存查询结果，以提高性能。当需要获取大量结果时，这些缓存可能会占用大量的内存。为了解决这个问题，可以通过设置 `request_cache` 参数为 `false` 来禁用缓存，从而减少内存消耗。
3. 数据一致性问题：当进行深翻页时，可能会遇到数据一致性问题。这是因为 Elasticsearch 是一个分布式系统，不同节点之间的数据可能会有一定的延迟。当进行深翻页时，可能会看到旧的或者不一致的数据。为了解决这个问题，可以使用 `search_type` 参数设置为 `dfs_query_then_fetch`，该参数会在查询之前先进行分布式的查询，以确保数据的一致性。

总的来说，要解决 Elasticsearch 深翻页的问题，可以考虑使用 `search_after` 参数进行分页查询，禁用缓存来减少内存消耗，以及使用 `search_type` 参数来确保数据的一致性。另外，还可以调整 Elasticsearch 集群的配置，以提高性能和稳定性。



### 8、熟悉 ElasticSearch 性能优化

**1.批量提交**

背景是大量的写操作，每次提交都是一次网络开销。网络永久是优化要考虑的重点。

**2.** **优化硬盘**

索引文件需要落地硬盘，段的思想又带来了更多的小文件，磁盘 IO 是 ES 的性能瓶颈。一个固态硬盘比普通硬盘好太多。

**3.** **减少副本数量**

副本可以保证集群的可用性，但是严重影响了 写索引的效率。写索引时不只完成写入索引，还要完成索引到副本的同步。ES 不是存储引擎，不要考虑数据丢失，性能更重要。 如果是批量导入，建议就关闭副本。



### **9、ElasticSearch** **查询优化手段有哪些？**

**设计阶段调优**

（1）根据业务增量需求，采取基于日期模板创建索引，通过 roll over API 滚动索引；

（2）使用别名进行索引管理；

（3）每天凌晨定时对索引做 force_merge 操作，以释放空间；

（4）采取冷热分离机制，热数据存储到 SSD，提高检索效率；冷数据定期进行 shrink操作，以缩

减存储；

（5）采取 curator 进行索引的生命周期管理；

（6）仅针对需要分词的字段，合理的设置分词器；

（7）Mapping 阶段充分结合各个字段的属性，是否需要检索、是否需要存储等。……..

**写入调优**

（1）写入前副本数设置为 0；

（2）写入前关闭 refresh_interval 设置为-1，禁用刷新机制；

（3）写入过程中：采取 bulk 批量写入；

（4）写入后恢复副本数和刷新间隔；

（5）尽量使用自动生成的 id。

**查询调优**

（1）禁用 wildcard；

（2）禁用批量 terms（成百上千的场景）；

（3）充分利用倒排索引机制，能 keyword 类型尽量 keyword；

（4）数据量大时候，可以先基于时间敲定索引再检索；

（5）设置合理的路由机制。

**其他调优**

部署调优，业务调优等。

上面的提及一部分，面试者就基本对你之前的实践或者运维经验有所评估了。



### **10、elasticsearch **是如何实现 master 选举的？

Elasticsearch 使用一种基于 ZooKeeper 或者内置的 Zen Discovery 机制来实现主节点选举。主节点负责集群的管理和协调工作，如分配分片、索引的创建和删除等操作。

在基于 ZooKeeper 的实现中，Elasticsearch 集群中的每个节点都会与 ZooKeeper 进行通信，并尝试创建一个临时的有序节点（ephemeral sequential node）。这些节点按照节点的 ID 和创建顺序进行排序，其中 ID 是一个唯一标识符，可以是节点的名称或者 IP 地址。节点创建临时节点后，它会监视自己前面的一个节点，并在该节点消失时尝试成为主节点。一旦一个节点成为主节点，它将负责管理集群，并在需要时进行主节点选举。

在基于 Zen Discovery 的实现中，每个节点都会与其他节点进行通信，并进行协商以决定主节点。节点通过互相交换信息，比较自己的节点 ID 和其他节点的节点 ID，然后选择具有最低节点 ID 的节点作为主节点。如果主节点失去连接或者宕机，其他节点将重新进行协商，以选择新的主节点。

无论是基于 ZooKeeper 还是基于 Zen Discovery，**Elasticsearch 都使用了一种类似于 Paxos 或 Raft 算法的分布式一致性协议来确保主节点选举的正确性和可靠性。**这些协议可以确保只有一个主节点在任何时候管理集群，并且在主节点失效时能够快速选举新的主节点。主节点选举的过程是自动进行的，不需要用户干预。

总的来说，Elasticsearch 使用基于 ZooKeeper 或者 Zen Discovery 的机制来实现主节点选举，确保集群的稳定性和可靠性。这些机制依赖于分布式一致性协议，能够自动进行主节点选举并处理主节点的故障。



### 11、elasticsearch 索引数据多了怎么办，如何调优，部署？

当 Elasticsearch 索引数据量增加时，可能会遇到性能下降和资源消耗增加的问题。为了调优和部署 Elasticsearch，可以考虑以下几个方面：

1. 硬件升级：增加硬件资源是提高 Elasticsearch 性能的一种有效方法。可以考虑增加更多的节点、更高性能的硬盘、更大的内存等。
2. 分片和副本设置：合理设置分片和副本的数量。如果索引数据量很大，可以考虑增加分片的数量，以便更好地分布和并行处理数据。但要注意，分片数量过多可能会导致性能下降和资源消耗增加。副本可以提高查询性能和故障容错能力，但也会增加资源消耗。
3. 索引设计和映射优化：合理设计索引和映射，以提高查询性能和减少资源消耗。可以考虑使用合适的数据类型、字段类型和分词器，避免不必要的字段和索引，以及优化查询的过滤条件和聚合操作。
4. 缓存和缓存策略：Elasticsearch 提供了缓存机制，可以通过合理配置缓存来提高性能。可以考虑启用查询缓存、字段数据缓存和过滤器缓存，并根据实际情况调整缓存的大小和过期策略。
5. JVM 调优：Elasticsearch 是基于 Java 的应用程序，可以通过调整 JVM 的参数来优化性能。可以考虑调整堆内存大小、垃圾回收策略、线程池参数等。
6. 集群和节点配置：合理配置集群和节点的参数，以提高性能和可靠性。可以考虑调整集群的并发连接数、线程池大小、网络参数等。
7. 监控和日志：定期监控 Elasticsearch 集群的性能指标，如查询延迟、吞吐量、CPU 和内存使用等。通过日志记录和分析，可以及时发现和解决问题。

总的来说，调优和部署 Elasticsearch 需要综合考虑硬件资源、分片和副本设置、索引设计、缓存、JVM 调优、集群和节点配置等方面。根据实际情况进行适当的调整和优化，以提高性能和可靠性。



### 12、说说你们公司 es 的集群架构，索引数据大小，分片有多少？

面试官：想了解应聘者之前公司接触的 ES 使用场景、规模，有没有做过比较大规模的索引设计、规划、调优。

解答：如实结合自己的实践场景回答即可。

比如：ES 集群架构 13 个节点，索引根据通道不同共 20+索引，根据日期，每日递增 20+，索引：10 分片，每日递增 1 亿+数据，每个通道每天索引大小控制：150GB 之内。



### 13、什么是 ElasticSearch？

Elasticsearch 是一个开源的分布式搜索和分析引擎，构建在 Apache Lucene 基础之上。它提供了一个高度可扩展的、实时的搜索和分析解决方案，可以用于全文搜索、日志分析、数据可视化等各种应用场景。

Elasticsearch 以分布式的方式存储数据，将数据分成多个分片（shard），并分布在不同的节点上，以实现高可用性和横向扩展性。每个分片都是一个完整的索引，包含了数据的一部分。通过分片和副本（replica）的机制，Elasticsearch 可以在节点之间自动分配和复制数据，以提供高性能和容错能力。

Elasticsearch 提供了一个基于 JSON 的 RESTful API，使得数据的索引、搜索和分析变得非常简单。它支持复杂的查询语法，可以进行全文搜索、过滤、聚合和排序等操作。同时，Elasticsearch 还提供了强大的分布式聚合功能，可以对大规模数据进行复杂的统计和分析。

除了搜索和分析功能，Elasticsearch 还具备实时性能和可扩展性。它支持实时索引和搜索，可以在数据发生变化时立即更新索引并提供搜索结果。同时，通过添加新的节点，Elasticsearch 可以轻松地扩展以处理大规模的数据和高并发的请求。

总的来说，Elasticsearch 是一个功能强大、易于使用和高度可扩展的搜索和分析引擎，可以满足各种应用场景的需求。它已经被广泛应用于全文搜索、日志分析、数据可视化、推荐系统等领域。



### 14、ElasticSearch 中的集群、节点、索引、文档、类型是什么？

在 Elasticsearch 中，有以下几个重要的概念：

1. 集群（Cluster）：一个 Elasticsearch 集群由一个或多个节点组成，它们共同合作来存储和处理数据。集群有一个唯一的名字，用于区分不同的集群。
2. 节点（Node）：一个节点是集群中的一个单独的服务器实例，它存储数据并参与集群的协调和搜索功能。每个节点都有一个唯一的名称，并且可以具有不同的角色，如主节点、数据节点、协调节点等。
3. 索引（Index）：索引是 Elasticsearch 中存储和组织数据的逻辑容器。它类似于关系数据库中的数据库，用于存储和索引一组具有相似特征的文档。索引可以被分成多个分片，并且每个分片可以有多个副本。
4. 文档（Document）：文档是 Elasticsearch 中最小的基本数据单元。它是一个 JSON 格式的数据对象，可以是结构化的或者半结构化的。每个文档都属于一个索引，并且有一个唯一的 ID 来标识。
5. 类型（Type）：在 Elasticsearch 6.x 及更高版本中，类型的概念已经被弃用。在早期的版本中，一个索引可以包含多个类型，每个类型定义了一组具有相似结构的文档。然而，从 Elasticsearch 7.x 开始，一个索引只能包含一个类型，而且该类型的名称默认为 "_doc"。

总的来说，Elasticsearch 中的集群由多个节点组成，每个节点可以存储和处理数据。数据以文档的形式存储在索引中，每个文档属于一个索引，并且可以通过唯一的 ID 进行标识。类型定义了文档的结构，但在最新版本中已经被弃用，一个索引只能包含一个类型。



### 15、ElasticSearch 中的分片是什么？

在 Elasticsearch 中，分片（Shard）是将索引的数据分割成多个部分的方式。每个分片都是一个完整的索引，包含了数据的一部分。通过将索引分成多个分片，可以将数据分布在不同的节点上，以实现数据的分布式存储和处理。

分片在 Elasticsearch 集群中起到了重要的作用，它们提供了以下几个关键的功能：

1. 横向扩展性：通过分片，Elasticsearch 可以在多个节点上并行处理数据，从而实现横向扩展。当数据量增加时，可以简单地增加节点和分片，以增加集群的处理能力。

2. 高可用性：通过复制分片，Elasticsearch 提供了高可用性和故障容错能力。每个分片可以有多个副本，这些副本分布在不同的节点上。当一个节点故障时，副本可以接管工作，保证数据的可用性。

3. 并行搜索和聚合：通过将查询分发到不同的分片上进行并行处理，Elasticsearch 可以实现高性能的搜索和聚合操作。查询可以同时在多个分片上执行，并将结果合并返回给用户。

4. 数据均衡：Elasticsearch 会自动将分片分配到不同的节点上，并尽量保持每个节点上的分片数量均衡。这样可以确保数据在集群中均匀分布，避免出现热点或单点故障。

分片数量是在创建索引时指定的，并且在索引创建后不能更改。分片的数量应根据数据量、查询负载和硬件资源进行合理的规划和配置。过少的分片可能会导致性能瓶颈和资源利用不充分，而过多的分片可能会导致性能下降和资源消耗增加。

总的来说，分片是 Elasticsearch 中将索引数据分割和分布式存储的方式，它提供了横向扩展性、高可用性、并行处理能力和数据均衡等重要功能。



### 16、ElasticSearch 中的副本是什么？

在Elasticsearch中，副本（replica）是主分片（primary shard）的一个复制品。主分片是数据的原始分片，而副本是主分片的一个备份。副本的存在有以下几个目的：

1. 提高数据的可用性：副本可以在主分片不可用时提供数据的访问。如果主分片发生故障或不可用，副本可以接管请求并继续提供服务。

2. 提高读取性能：副本可以用于处理读取请求，从而分担主分片的负载。当有多个副本时，读取请求可以并行处理，提高了读取的性能。

3. 分布式搜索：副本可以分布在不同的节点上，从而实现分布式搜索。当一个搜索请求到达时，可以同时在主分片和副本上执行搜索操作，从而提高搜索的效率。

副本的数量可以根据需求进行配置。默认情况下，每个主分片有一个副本，即总共有两个副本。可以根据数据的重要性和可用性需求来增加或减少副本的数量。



### 17、ElasticSearch 中的分析器是什么？

在Elasticsearch中，分析器（analyzer）是用于将文本数据进行分词和标准化的组件。它是索引和搜索过程中的一个重要部分。

分析器由以下三个主要组件组成：

1. 字符过滤器（Character Filters）：用于对原始文本进行预处理，例如去除HTML标签、转换字符编码等。

2. 分词器（Tokenizer）：将文本按照一定的规则进行分词，将文本拆分为一个个单词或词条。常见的分词器有标准分词器（Standard Tokenizer）、关键字分词器（Keyword Tokenizer）、边界分词器（Pattern Tokenizer）等。

3. 词条过滤器（Token Filters）：对分词结果进行一系列的标准化和处理操作，例如转换为小写、去除停用词、词干提取等。

分析器的作用是将原始文本转换为可被索引和搜索的标准化文本。在索引过程中，分析器被应用于将文本分析为一系列的词条，并将这些词条存储在倒排索引中。在搜索过程中，分析器被应用于对查询进行相同的分析和标准化操作，以便与索引中的词条进行匹配。

Elasticsearch 提供了丰富的内置分析器，同时也支持自定义分析器，可以根据具体需求来选择和配置合适的分析器。



### 18、es node 有关

当涉及到 Elasticsearch 节点的面试题时，以下是一些常见的问题和回答：

1. 什么是 Elasticsearch 节点？
答：Elasticsearch 节点是 Elasticsearch 集群中的一个实例，它可以是一个物理服务器、虚拟机或容器。每个节点都是一个独立的进程，具有自己的内存、处理能力和存储空间。节点之间通过内部通信进行协调和数据同步。

2. 节点的类型有哪些？
答：Elasticsearch 节点可以分为主节点（master node）和数据节点（data node）。主节点负责集群管理和协调，如创建和删除索引、分配和迁移分片等。数据节点负责存储和处理数据，执行索引和搜索操作。

3. 如何配置节点类型？
答：节点的类型是通过配置文件 elasticsearch.yml 进行配置的。可以通过设置 node.master 和 node.data 参数来指定节点的类型。设置 node.master=true 表示节点是主节点，设置 node.data=true 表示节点是数据节点。

4. 节点之间如何通信和协调？
答：节点之间通过内部的 TCP/IP 通信进行协调和数据同步。主节点负责集群管理和决策，数据节点负责存储和处理数据。节点之间使用选举算法来选举主节点，并通过心跳机制进行健康检查和节点状态的监控。

5. 如何添加或删除节点？
答：可以通过启动或停止 Elasticsearch 进程来添加或删除节点。添加节点时，新节点会自动加入到集群中，并根据配置的节点类型进行分配和协调。删除节点时，集群会重新分配数据和分片，以保持集群的健康状态。

这些问题涵盖了 Elasticsearch 节点的基本概念和配置。当面试时，可能会进一步深入探讨节点的角色、负载均衡、高可用性等方面的问题。

### 19、es node 角色有关

当涉及到 Elasticsearch 节点角色的面试题时，以下是一些相关问题和回答：

1. 什么是 Elasticsearch 节点角色？
答：Elasticsearch 节点角色定义了一个节点在集群中的功能和职责。主要的节点角色包括主节点（master node）、数据节点（data node）和协调节点（coordinating node）。

2. 主节点的作用是什么？
答：主节点负责集群管理和协调工作。它们负责创建和删除索引、分配和迁移分片、维护集群状态等。主节点还负责选举和监控其他节点的健康状态，并在需要时进行重新选举。

3. 数据节点的作用是什么？
答：数据节点负责存储和处理数据。它们执行索引和搜索操作，并维护分片的复制和同步。数据节点负责将数据分布在多个分片上，以实现数据的水平扩展和负载均衡。

4. 协调节点的作用是什么？
答：协调节点充当客户端请求的代理，负责将请求路由到适当的数据节点。它们接收来自客户端的请求，并将其转发到包含所需数据的数据节点。协调节点还处理搜索请求的结果合并和排序。

5. 如何配置节点的角色？
答：节点的角色是通过配置文件 elasticsearch.yml 进行配置的。可以通过设置 node.master 和 node.data 参数来指定节点的类型。设置 node.master=true 表示节点是主节点，设置 node.data=true 表示节点是数据节点。协调节点不需要专门配置，任何节点都可以充当协调节点。

6. 为什么要将节点角色分离？
答：将节点角色分离可以提高集群的性能和可伸缩性。主节点负责集群管理和协调工作，数据节点负责存储和处理数据，协调节点负责请求的路由和结果合并。这种分离可以使集群的各个部分专注于自己的任务，并能够更好地处理不同类型的负载。

这些问题可以帮助面试者了解 Elasticsearch 节点角色的概念、功能和配置。进一步的面试可能会涉及到节点角色之间的通信、选举算法、负载均衡和高可用性等方面的问题。



### 20、什么是 ElasticSearch 中的过滤器？

在 Elasticsearch 中，过滤器（Filter）是一种用于筛选和过滤文档的组件。它用于在搜索查询和聚合操作中，根据指定的条件对文档进行筛选，以返回符合条件的文档结果。

过滤器与查询（Query）的作用类似，但有一些关键的区别：

1. 缓存：过滤器的结果可以被缓存，以提高性能。当相同的过滤器被多次使用时，可以避免重复计算，从而加快查询速度。

2. 不计算相关性得分：过滤器不计算文档的相关性得分（relevance score），而是仅根据条件进行筛选。这使得过滤器在对结果进行排序和排除时更加高效。

3. 结果缓存：过滤器的结果可以被缓存起来，以便在后续的查询中重复使用。这对于一些频繁使用的过滤器非常有用，可以减少计算和提高查询性能。

常见的过滤器类型包括：

1. Term Filter：根据字段的精确值进行过滤。
2. Range Filter：根据字段的范围进行过滤。
3. Bool Filter：使用逻辑运算符（AND、OR、NOT）组合多个过滤器。
4. Exists Filter：检查字段是否存在。
5. Geo Distance Filter：根据地理位置进行过滤。
6. Script Filter：使用脚本进行自定义过滤逻辑。

过滤器可以与查询（Query）一起使用，以进一步缩小搜索结果的范围。它们可以通过组合和嵌套来构建更复杂的过滤条件。过滤器的使用可以提高查询性能，并允许更精确地控制搜索结果。



### 21、Es 启用属性，索引和存储的用途是什么？

在Elasticsearch中，有几个常见的属性、索引和存储选项，它们的用途如下：

1. 启用属性（Enabled）：启用属性是指在文档中是否启用某个字段。默认情况下，所有字段都是启用的，可以进行索引和搜索。但有时候，我们可能希望某些字段不被索引和搜索，可以将其禁用。

**启用属性的用途 **是控制字段是否参与全文搜索和查询操作。对于不需要进行搜索的字段，禁用它们可以减少索引大小和提高性能。

2. 索引属性（Index）：索引属性定义了字段是否需要被索引。默认情况下，所有字段都是被索引的，允许对其进行全文搜索和查询操作。但有时候，我们可能希望某些字段不被索引，只作为存储和检索的属性。

**索引属性的用途 **是控制字段是否参与全文搜索，但仍然可以存储和检索字段的原始值。这对于需要存储但不需要搜索的字段很有用，可以减少索引大小和提高性能。

3. 存储属性（Store）：存储属性定义了字段是否需要被存储。默认情况下，所有字段都是被存储的，可以通过 _source 字段检索到原始的文档内容。但有时候，我们可能希望某些字段不被存储，只作为索引的一部分。

**存储属性的用途 **是控制字段是否需要被存储，以减少索引的存储空间。对于不需要直接检索原始文档内容的字段，可以禁用存储属性，从而节省存储空间。

这些属性、索引和存储选项可以根据具体需求进行配置。根据字段的用途和搜索需求，我们可以灵活地启用或禁用这些选项，以达到最佳的性能和存储效率。





## Tomcat 篇

### 1、Tomcat 的缺省端口是多少，怎么修改？

默认端口为 8080，可以通过在 tomcat 安装包 conf 目录下，service.xml 中的 Connector 元素的 port 属性来修改端口



### 2、tomcat 有哪几种 Connector 运行模式（优化）？

这三种模式的不同之处如下：

BIO ：一个线程处理一个请求。缺点：并发量高时，线程数较多，浪费资源。Tomcat7版本或更低版本中，在Linux系统中默认使用这种方式。

NIO ：利用Java的异步IO处理，可以通过少量的线程处理大量的请求。tomcat8.0.x中默认使用的是NIO。Tomcat7必须修改Connector配置来启动：

```
<Connector port="8080" protocol="org.apache.coyote.http11.Http11NioProtocol" 
connectionTimeout="20000" redirectPort="8443"/>
```

APR ：即 Apache Portable Runtime，从操作系统层面解决io阻塞问题。Tomcat 7 或 Tomcat 8 在 Win 7 或以上的系统中启动默认使用这种方式。



### 3、Tomcat 有几种部署方式？

- 利用 Tomcat 的自动部署：把 web 应用拷贝到 webapps 目录（生产环境不建议放在该目录中）。Tomcat 在启动时会加载目录下的应用，并将编译后的结果放入 work 目录下。

- 使用 Manager App 控制台部署：在 tomcat 主页点击“Manager App” 进入应用管理控制台，可以指定一个web应用的路径或war文件。

- 修改 conf / server.xml 文件部署：在 server.xml 文件中，增加 Context 节点可以部署应用。

- 增加自定义的 Web 部署文件：在 conf/Catalina/localhost/ 路径下增加 xyz.xml 文件，内容是 Context 节点，可以部署应用。



### 4、tomcat 容器是如何创建 servlet 类实例？用到了什么原理？

1. 当容器启动时，会读取在 webapps 目录下所有的 web 应用中的 web.xml 文件，然后对 xml 文件进行解析，并读取 servlet 注册信息。然后，将每个应用中注册的servlet类都进行加载，并通过反射的方式实例化。（有时候也是在第一次请求时实例化）

2. 在 servlet 注册时加上1如果为正数，则在一开始就实例化，如果不写或为负数，则第一次请求

实例化。



### 5、Tomcat 线程模型是什么？有什么用

Tomcat 使用基于线程的模型来处理客户端的请求。它使用了一个称为"线程池"的机制，其中包含了一组预先创建的线程。当有请求到达时，Tomcat从线程池中获取一个空闲的线程来处理该请求。这种线程模型被称为"多线程模型"。

线程模型的作用是提高服务器的并发处理能力。通过使用线程池，Tomcat 可以同时处理多个请求，而不需要为每个请求创建一个新的线程。这样可以避免频繁地创建和销毁线程的开销，提高了服务器的性能和响应速度。

线程模型还可以控制并发请求的数量，通过配置线程池的大小和参数，可以限制同时处理的请求数量，以避免服务器过载。同时，线程模型还可以处理请求的排队和调度，以确保公平地分配资源，并尽可能地减少请求的等待时间。

总之，Tomcat的线程模型可以提高服务器的并发处理能力，提高性能和响应速度，并提供灵活的配置选项来控制并发请求的数量和调度。



### 6、Tomcat 组件有关

#### 1.请解释一下 Tomcat 是什么以及它的作用。

Tomcat是一个开源的Java Servlet容器，它实现了Java Servlet和JavaServer Pages（JSP）规范。它的主要作用是用于部署和运行Java Web应用程序。Tomcat可以处理客户端的HTTP请求，并将其发送给适当的Servlet进行处理，并将处理结果返回给客户端。

#### 2.Tomcat 的架构是怎样的？

Tomcat的架构是基于模块化的，它由多个组件组成。其中核心组件包括：

- Connector（连接器）：负责处理客户端的HTTP请求，并将请求传递给适当的处理器。
- Container（容器）：负责管理和执行Servlet和JSP的生命周期，以及处理相关的请求和响应。
- Mapper（映射器）：负责将请求映射到适当的Servlet或JSP。
- Loader（加载器）：负责加载Web应用程序的类文件。
- Valve（阀门）：用于在请求处理过程中执行特定的操作，例如身份验证、访问控制等。

此外，Tomcat还包括其他一些组件，如Session Manager（会话管理器）、Realm（身份验证和授权）等。

#### 3.请解释一下 Tomcat 的类加载机制。

Tomcat使用了一个双亲委派模型的类加载机制。当Tomcat启动时，它会创建一个Bootstrap类加载器，用于加载Tomcat自身的类。然后，它会创建一个Catalina类加载器，用于加载Catalina容器相关的类。最后，每个Web应用程序都会有一个独立的Web应用程序类加载器，用于加载该应用程序的类。

在类加载的过程中，首先会检查Bootstrap类加载器是否能够加载所需的类。如果无法加载，则会将请求传递给Catalina类加载器。如果Catalina类加载器也无法加载，则会将请求传递给Web应用程序类加载器。如果所有的类加载器都无法加载，则会抛出ClassNotFoundException异常。

#### 4.请解释一下 Tomcat 中的连接器（Connector）和容器（Container）的作用和区别。

连接器（Connector）负责处理客户端的HTTP请求，并将请求传递给适当的处理器（如Servlet）。它监听指定的端口，等待客户端的连接请求。当有请求到达时，连接器会将请求解析成HTTP协议的格式，并将其传递给容器进行处理。

容器（Container）负责管理和执行Servlet和JSP的生命周期，以及处理相关的请求和响应。它接收连接器传递过来的请求，并将其分发给适当的Servlet进行处理。容器还负责管理Servlet的实例化、初始化和销毁过程，以及处理相关的请求和响应。

区别在于，连接器负责处理HTTP请求的传输和解析，而容器负责处理请求的分发和处理。连接器是与底层的网络通信相关的组件，而容器是与Servlet和JSP的执行和管理相关的组件。

#### 5.Tomcat 中的 Realm 是什么？它的作用是什么？

Realm（内存中保存用户信息）、JDBC Realm（通过数据库验证用户信息）、LDAP Realm（通过LDAP服务器验证用户信息）等。

作用是用于对用户进行身份验证和授权。当用户访问受限资源时，Tomcat会使用Realm来验证用户的身份。如果用户通过验证，则可以授权用户访问资源；否则，用户将被拒绝访问。

通过配置Realm，可以实现不同的身份验证和授权策略。例如，可以使用内存Realm来存储用户信息并进行身份验证，或者使用JDBC Realm从数据库中验证用户信息。这样可以灵活地根据实际需求进行配置和管理用户身份验证和授权。



### 7、tomcat 生命周期有关

#### 1.请解释一下 Tomcat 的生命周期。

Tomcat 的生命周期指的是 Tomcat 服务器从启动到关闭的整个过程。它可以分为以下几个阶段：

- 初始化阶段：Tomcat 服务器启动时，会进行初始化操作，包括加载配置文件、创建并初始化组件等。
- 启动阶段：在初始化阶段完成后，Tomcat 会启动各个组件，如连接器、容器等，准备接受客户端的请求。
- 运行阶段：在启动阶段完成后，Tomcat 进入运行状态，可以接收和处理客户端的请求，并将响应返回给客户端。
- 停止阶段：当需要关闭Tomcat服务器时，会触发停止操作，Tomcat 会停止接收新的请求，并逐渐将已有的请求处理完毕，然后关闭各个组件，最终停止服务器。

#### 2.Tomcat的启动过程是怎样的？

Tomcat 的启动过程包括以下步骤：

- 加载配置文件：Tomcat 首先会加载配置文件，如 server.xml、web.xml 等，这些配置文件包含了 Tomcat 服务器的各种设置和参数。
- 创建并初始化组件：根据配置文件的内容，Tomcat 会创建并初始化各个组件，如连接器、容器等。每个组件都有自己的初始化过程，包括加载类、初始化参数、配置相关的属性等。
- 启动组件：组件初始化完成后，Tomcat 会按照配置文件中的顺序逐个启动各个组件，如启动连接器、容器等。每个组件的启动过程可能涉及到一些网络端口的监听和绑定等操作。
- 进入运行状态：当所有组件都成功启动后，Tomcat 服务器进入运行状态，可以接收和处理客户端的请求。

#### 3.Tomcat 的停止过程是怎样的？

Tomcat 的停止过程包括以下步骤：

- 停止接收新请求：当需要停止 Tomcat 服务器时，会发送一个停止信号，Tomcat 服务器会停止接收新的请求，并将其拒绝。
- 处理现有请求：Tomcat 会逐渐处理完已有的请求，确保这些请求能够正常完成。在处理请求的过程中，Tomcat 可能会等待一段时间，直到所有请求都处理完毕。
- 关闭组件：当所有请求都处理完毕后，Tomcat 会关闭各个组件，按照与启动相反的顺序逐个关闭。这些组件的关闭过程包括释放资源、关闭网络端口等操作。
- 停止完成：当所有组件都成功关闭后，Tomcat 服务器完全停止，停止过程结束。

#### 4.如何在 Tomcat 中实现自定义的初始化和销毁操作？

在 Tomcat 中，可以通过实现一些特定接口或使用一些特定的配置来实现自定义的初始化和销毁操作。

- 对于 Servlet 和 Filter 等 Web 组件，可以通过实现 `javax.servlet.ServletContextListener` 接口来监听 Web 应用程序的初始化和销毁事件，并在其中编写自定义的初始化和销毁逻辑。
- 对于整个 Tomcat 服务器，可以在 server.xml 配置文件中使用<Listener>元素来指定一个全局的监听器，该监听器可以监听Tomcat 的初始化和销毁事件，并执行相应的自定义操作。
- 还可以通过编写自定义的 Tomcat 扩展来实现自定义的初始化和销毁操作。可以创建一个类，继承自`org.apache.catalina.LifecycleBase`，并重写其 init() 和 destroy() 方法，在这些方法中编写自定义的初始化和销毁逻辑。然后将该类的实例添加到 Tomcat 的配置中，以使其生效。

无论是使用 ServletContextListener、<Listener>元素还是自定义的 Tomcat 扩展，都可以在初始化和销毁阶段执行自定义的操作，如加载配置文件、初始化数据库连接池、关闭资源等。这样可以实现更灵活的初始化和销毁过程，以满足特定需求。
