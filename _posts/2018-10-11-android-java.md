---
layout:     post
title:      "Android面试总结--java部分"
subtitle:   "Android面试总结--java部分"
date:       2018-10-11
tags:
    - android
    - java
---


##  引用

[https://github.com/7449/AndroidDevelop/blob/develop/markdown-read/app/src/main/assets](https://github.com/7449/AndroidDevelop/blob/develop/markdown-read/app/src/main/assets)

## 基础

#### 解释一下 OOP 的概念

> Object-oriented programming

#### 什么是对象

* 万物皆对象，对象是程序的基本单元
* 类的定义包含数据的形式（成员变量）以及对数据的操作（成员方法），对象是类的实例
* 每个对象都有自己的空间，可以容纳其他对象（成员变量）

#### 面向对象三大特性
  
* 面向对象编程三大特性：封装、继承、多态
* 类可以封装自己的属性和方法（设置为私有）可参考[链接](http://www.cnblogs.com/chenssy/p/3351835.html)
* 封装隐藏了类的内部实现机制，可以在不影响使用的情况下改变类的内部结构，同时也保护了数据
* 继承可以重用父类代码，达到代码复用和可扩展的效果
* 多态就是对象的一个方法在执行时可以有多种状态，即父类引用可以持有子类对象。
* 非静态成员方法的调用：编译时看父类，运行时看子类。即父类类型的引用可以调用父类中定义的所有属性和方法，对于只存在于子类中的方法和属性无法调用
* 多态优点：不用创建一堆子类对象的引用；多态缺点：不能使用子类特有的成员属性和子类特有的成员方法

#### [什么是多态？什么是继承？](http://www.cnblogs.com/chenssy/p/3354884.html)
	
多态：

* 多态就是父类引用可以持有子类对象。非静态成员方法的调用：编译时看父类，运行时看子类
* 多态优点：不用创建一堆子类对象的引用
* 多态缺点：不能使用子类特有的成员属性和子类特有的成员方法

继承：
	
* 继承是使用已存在的类的定义作为基础建立新类的技术，新类的定义可以增加新的数据或新的功能，也可以用父类的功能，但不能选择性地继承父类
* 通过使用继承我们能够非常方便地复用以前的代码，大大提高开发的效率

#### [抽象类和接口的区别?](https://arjun-sna.github.io/java/2017/02/02/abstractvsinterface/)

区别：

* 关键字不同：接口使用 `implements` 来实现；抽象类使用 `extends` 来继承
* 抽象类可以包含具体方法和抽象方法；接口只可以包含抽象方法（抽象方法指没有实现的方法）
* 抽象类的方法可以使用 `public``protected``private` 等修饰符；接口的方法默认使用 `public abstract` 修饰，成员变量默认使用 `public static final` 修饰
* 一个接口可以继承多个父接口；一个抽象类只能继承一个父类
* 接口可以理解为抽象方法的集合；抽象类可以理解为一个没有包含足够的信息去描述具体对象的类，终归还是类

相同点：

* 接口和抽象类都不可以实例化

#### [什么是匿名内部类](http://wiki.xuchongyang.com/Java/%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86%E7%82%B9.html)

* 匿名内部类即没有名字的内部类，只可被使用一次
* 使用匿名内部类前提条件：必须继承自一个父类或实现一个接口
* 语法格式为 `new SuperType(construction parameters){}`
* 语法格式为 `new 父类构造器(参数列表)|实现接口(){}`
* 直接将抽象类的方法或者接口方法在大括号中实现，可以省略一个类的书写
* 匿名内部类中不能定义构造方法，但可以使用初始化语块代替构造方法

#### 接口

 1. 接口中的字段全部默认为 `public static`类型。
 2. 接口中的方法全部默认为 `public`类型。
 3. 接口中可以申明内部类，而默认为`public static`，正因为是`static`，只是命名空间属于接口，代码逻辑不属于接口。所以不违法接口定义。
 4. 接口本身可以申明为`public`或者缺省。
 5. 抽象类继承自某接口。如果在抽象类中实现了父类（接口）中的方法，在其子类可以不用实现，否则在子类必须实现。
 6. 多继承

#### 接口实例化

接口无法直接创建对象，接口是一个特殊的抽象类，它里面所有的方法都是抽象的。
当一个类实现了这个接口，那么这个类是可以创建对象的。
这个类实现了这个接口，相当于创建了这个接口的子类，接口就是这个类的父类，
而父类引用是可以指向子类的，我们创建的不是接口对象，而是实现了该接口的子类的对象

    List<String> list=new ArrayList<String>(); 

`List`是一个接口，`ArrayList`是`List`接口的实现类，这就是所谓的面向接口编程

#### 访问修饰符

* 访问修饰符是指能够控制类、成员变量、方法的使用权限的关键字
* 可以使用它们来保护对类、变量、方法和构造方法的访问
* `public`：共有的，对所有类可见
* `protected`：受保护的，对同一包内的类和所有子类可见
* `private`：私有的，在同一类内可见
* 默认：在同一包内可见。默认不使用任何修饰符。

#### 类型转换

* 基本数据类型(转换分为自动转换和强制转换)

	* 自动转换是从位数低的类型向位数高的类型转换
	* 强制类型转是从位数高的类型向位数低的类型转换，需要转型的数据前加上“()”，然后在括号内加入需要转化的数据类型，转换会导致精度丢失

* 引用数据类型

	* 子类可以转换成父类
	* 父类转换为子类不一定可行：当父类（引用）引用的为子类对象时，此时父类可以正常转换为子类；而当父类（引用）引用的确实为父类对象时，
	此时无法转换为子类，会抛出 `ClassCastException` 异常
	
#### [泛型](http://www.infoq.com/cn/articles/cf-java-generics)

* 泛型即宽泛的数据类型
* 允许在定义类和接口的时候使用类型参数（`type parameter`）
* 声明的类型参数在使用时用具体的类型来替换
* 使用泛型的时候加上的类型参数，会被编译器在编译的时候去掉，这个过程就称为类型擦除
	
#### 静态绑定和动态绑定的区别

> 绑定指的是一个方法的调用与方法所在的类(方法主体)关联起来。

1. 定义
* 静态绑定：编译过程中编译器已经准确的知道这个方法是属于哪个类的了
* 动态绑定：需要在运行时根据对象具体的类型来绑定，来选择调用哪个类的方法
2. 区分
* `private` 方法、`static` 方法、`final` 方法、构造器方法都是静态绑定的
* 其他方法全部为动态绑定

#### [`static` 关键字是什么意思？](http://www.cnblogs.com/chenssy/p/3386721.html)

> 静态方法不可以被重写，静态方法隶属于某个类，只和类相关

* `static` 表示“全局”或者“静态”的意思，用来修饰成员变量、成员方法或者内部类，也可以修饰代码块

* `static` 所蕴含“静态”的概念表示着它是不可恢复的，即在那个地方，你修改了，他是不会变回原样的，你清理了，他就不会回来了
* 被 `static` 修饰的成员变量和成员方法是独立于该类的，它不依赖于某个特定的实例变量，也就是说它被该类的所有实例共享
* 所有实例的引用都指向同一个地方，任何一个实例对其的修改都会导致其他实例的变化
* 静态变量是随着类加载时被完成初始化的；静态方法也是类加载时就存在了，所以不可为 `abstract`，必须实现；静态代码块也会随着类的加载一块执行

#### 序列化是什么?如何实现它?

[参考链接1](https://my.oschina.net/andot/blog/784235)、[参考链接2](https://tech.meituan.com/serialization_vs_deserialization.html)、[参考链接3](https://docs.microsoft.com/zh-cn/dotnet/standard/serialization/)

* 序列化指把应用层的对象或数据结构转换成一段连续的二进制串
* 反序列化指把二进制串转换成应用层的对象或数据结构

* 序列化按可读性可分为两类：二进制序列化、文本序列化（如 XML、JSON）以及半文本序列化

    * 文本序列化：可读性、可编辑性好
    * 二进制序列化：不具有可读性、但解析速度更有优势

* 按跨语言能力可分为：特定语言专有序列化和跨语言序列化

    * 大部分语言内置的序列化都属于特定语言专有序列化，如 Java 的序列化
    * 文本序列化通常为跨语言序列化，如 JSON、XML等

* `Java` 序列化是一种将对象转换为字节流的过程，目的是为了将该对象存储到内存中，等后面再次构建该对象时可以获取到该对象先前的状态及数据信息。
* 然而，在 `Android` 中，我们不应该使用 `Serializable` 接口。因为` Serializable` 接口使用了反射机制，这个过程相对缓慢，而且往往会产生出很多临时对象，
这样可能会触发垃圾回收器频繁地进行垃圾回收。相比而言，`Parcelable` 接口比 `Serializable` 接口效率更高，性能方面要高出 10x 多倍。

#### [方法重载和重写的区别](http://droidyue.com/blog/2014/12/28/static-biding-and-dynamic-binding-in-java/index.html)

* 重载是在同一个类中完成的，重写父类需要有子类。
* 方法重载时返回值类型、参数列表可以不同；方法重写时返回值类型、参数列表必须相同
* 重载的方法使用静态绑定完成，发生在编译时；重写的方法使用动态绑定完成，发生在运行时。性能：重载比重写更有效率
* `private` 方法、`static` 方法、`final` 方法都是可以重载，但不可以重写

#### 对象是否会以引用传递或者值传递

> [参考链接](http://xuchongyang.com/2017/10/11/Java-%E5%BC%95%E7%94%A8%E5%AD%A6%E4%B9%A0/)

对象以按值传递的形式进行调用，所有方法得到的都是参数值的拷贝
	
* 对于基本数据类型的参数，方法得到的是其值的拷贝
* 对于引用数据类型的参数，方法得到的也是其值（所指向对象的内存地址）的拷贝
* 方法均不可以修改参数变量的值，对于引用类型参数来说，就是不能让引用类型参数指向新的对象
* 但是方法可以修改引用类型参数所指向对象的内容，因为方法得到的是该对象地址的拷贝，可以根据地址获取到该对象

#### 本地变量、实例变量以及类变量之间的区别？

* 类体由两部分组成，变量定义和方法定义
* 本地变量即局部变量，定义在方法内部或者方法的形参中
* 实例变量为为非静态变量，每一个对象的实例变量都不同
* 类变量为静态变量，属于类所有

#### [垃圾回收机制，如何更有效的管理内存，减少OOM的概率](http://www.cnblogs.com/vamei/archive/2013/04/28/3048353.html)

1. 内存结构：
    
    * 内存分为栈（`satck`）和堆（`heap`）两部分
    * `JVM` 中栈记录了方法调用，每个线程拥有一个栈（栈的每一帧中保存有该方法调用的参数、局部变量和返回地址）
    * 栈中被调用方法运行结束时，相应的帧也会删除，参数和局部变量占用的空间也会释放
    * 堆是 `JVM` 中可以自由分配给对象的区域，堆区由所有线程共享

2. 垃圾回收

    * `JVM` 自动清空堆中无用对象占用的空间就是垃圾回收
    * 当一个对象没有引用指向它时，为不可达对象，此时会被回收

3. 对象是否回收的依据
    
    * 引用计数算法
    * 可达性分析算法

4. 回收基础

    * `Mark and sweep` 机制：每个对象都有用于表示该对象是否可达的标记信息。垃圾回收启动时，`Java` 程序暂停运行，`JVM` 从根出发，找到所有可达对象并标记。然后扫描整个堆找到不可达对象，清空它们占用的内存
    * `Copy and sweep` 机制：堆被分为两个区域，对象存活于两个堆中的一个。垃圾回收启动时，`Java` 程序暂停运行，`JVM` 从根出发找到所有可到达对象，
    把所有可到达对象复制到空白区域中并紧密排列（并修改由于对象地址变化引起的引用变化）。最后直接清空对象原先所存活的区域，使其成为新的空白区域。
    * 对象比较长寿适用于 `mark and sweep` 机制；对象比较活跃则适用于 `copy and sweep` 机制，避免出现空隙
    * 世代指对象经历过的垃圾回收的次数，堆分为三个世代：永久世代、成熟世代、年轻世代
    * 永久世代的对象不会被垃圾回收
    * 年轻世代进一步分为三个区域：`eden` 区、`from` 区、`to` 区
    * 新生对象指从上次垃圾回收后创建的对象，存在于 `eden` 区。`from` 区和 `to` 区相当于 `copy and sweep` 中的两个区

5. 分代回收

    * 新建对象无法放入 `eden` 区时，会触发`minor collection`，`JVM` 会采用 `copy and sweep` 机制将 `eden` 区和 `from` 区的可到达对象复制到 `to` 区，
    进行一次垃圾回收清空 `eden` 区和 `from` 区，`to` 区则存放着紧密排列的对象。接着 `from` 区成为了新的 `to`区，原来的` to` 区成为了新的 `from` 区
    * 当 `minor collection` 时发现 `to` 区也放不下时，会将部分对象放入成熟世代
    * 即使 `to` 区没有满，`JVM` 也会移动世代足够久远的对象到成熟世代
    * 如果成熟世代放满对象无法放入新的对象，会触发` minor collection`，`JVM` 采用` mark and sweep `机制对成熟世代进行垃圾回收

#### 垃圾收集器是什么?它是如何工作的

* 垃圾收集是一种自动的内存管理机制
* 所有的对象实例都在 `JVM` 管理的堆区域分配内存
* 只要对象被引用，`JVM`就会认为它还存活于进程中
* 一旦对象不再被引用，垃圾收集器将删除它并重新声明为未使用的内存

#### [静态块何时运行](http://www.jianshu.com/p/8a3d0699a923)

* 静态块在 `JVM` 加载类时执行，也就是在类进行初始化时执行，仅执行一次
* 一个类中可以有多个静态代码块
* 类调用时，先执行静态代码块，然后才执行主函数
* 静态代码块其实就是给类初始化的，而构造代码块是给对象初始化的
* 静态代码块中的变量是局部变量，与普通函数中的局部变量性质没有区别

#### 什么是自动装箱和拆箱？

* 自动装箱就是 `Java` 自动将原始类型值转换成对应的对象，比如将 `int` 的变量转换成 `Integer` 对象
* 反之将 `Integer` 对象自动转换成 `int` 类型值，这个过程叫做拆箱
* 自动装箱时编译器调用 `valueOf` 将原始类型值转换成对象
* 自动拆箱时，编译器通过调用类似 `intValue()` `doubleValue()` 这类的方法将对象转换成原始类型值
* 自动装箱、拆箱主要发生在赋值时和方法调用时
	
###### `Integer` 和 `int` 之间的区别[自动装箱、自动拆箱](http://droidyue.com/blog/2015/04/07/autoboxing-and-autounboxing-in-java/index.html)

* `int` 是基本数据类型，`Integer` 是包装类
* `Java`中，会对 `-128` 到 `127` 的 `Integer` 对象进行缓存，当创建新的 `Integer` 对象时，如果符合这个这个范围，并且已有存在的相同值的对象，则返回这个对象，否则创建新的` Integer` 对象
	
#### [多进程和多线程的区别](http://markxu.coding.me/wiki/%E5%A4%9A%E7%BA%BF%E7%A8%8B/Java%E5%9F%BA%E7%A1%80%EF%BC%9A%E5%A4%9A%E7%BA%BF%E7%A8%8B%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86.html)

* 本质区别在于：每个进程拥有自己的一整套变量，而线程则共享数据。

* 多进程：


    进程是程序的一次执行。计算机在同一刻运行多个程序，每个程序称为一个进程（计算机将 `CPU` 的时间片分配给每一个进程）

* 多线程：


    线程是 `CPU` 的基本调度单位。一个程序同时执行多个任务，每个任务称为一个线程
    
#### 什么是内存泄露，如何处理？

* 存在下面的这种对象，这些对象不会被` GC` 回收，却占用着内存，即为内存泄漏（简单说：存在已申请的无用内存无法被回收）

	* 该对象是可达的，即还在被引用着
	* 该对象是无用的，即程序以后不会再使用该对象

* 长生命周期的对象持有短生命周期的引用，就很可能会出现内存泄露
* 内存泄漏可能的情况：

	* 静态集合类引起的
	* 非静态内部类
	* 单例模式
	* HashSet 集合元素的属性被修改
	* 各种连接问题
    
#### 类的实例化和类的初始化之间的区别是什么?

[参考链接1](https://www.caoqq.net/java-class-initialization.html)、
[参考链接2](http://blog.csdn.net/czhpxl007/article/details/50558319)、
[参考链接3](http://blog.csdn.net/justloveyou_/article/details/72466416)

* 执行顺序不同：
	
	* （类）初始化执行顺序：静态成员变量／静态代码块
	* （对象）实例化执行顺序：成员变量／构造代码块  -> 构造方法
	* 以上，静态成员变量／静态代码块的执行顺序为代码中的定义顺序，成员变量／构造代码块的执行顺序也为代码中的定义顺序
	
* 执行时机不同
	
	类的初始化时机：
	
	* 类的一个实例被创建时
	* 类的一个静态方法被调用时
	* 类声明的一个静态变量被赋值时
	* 类声明的一个静态变量被使用且这个变量不是常量时
	* 一个类初始化时，如果父类没初始化则需要先初始化父类
	
	类的实例化时机
	
	* 当创建一个类的新的实例时（可以通过 new 关键字、反射机制、clone 方法、反序列化方式等）
	
## 代码相关

####  `==` 和 `equals()` 

* `==` 用来判断内存地址是否相同
* `equals()`用来判断字符串的内容是否相同

#### `hashCode()` 和 `equals()` 何时使用？

* 相等（相同）的对象必须具有相等的哈希码（或者散列码）
* 如果两个对象的 `hashCode` 相同，它们并不一定相同

* 往 `Set` 集合中添加元素时，要保证元素不重复，先调用该元素的 `hasCode` 方法，定位到它应该放置的物理位置。 
如果这个位置上没有元素，就可以直接存储在这个位置上，不用再进行任何比较；如果这个位置上已经有元素了，就调用它的 `equals` 方法与新元素进行比较，相同的话就不存了，不相同就散列其它的地址。
* 集合查找时，`hashcode` 能大大降低对象比较次数，提高查找效率

#### `HashSet` 和 `TreeSet`

* `TreeSet` 会自动按自然排序法给元素排序，`HashSet` 不会
* 如果不需要使用排序功能,应该使用 `HashSet`,因为其性能优于` TreeSet`
* 如果 `TreeSet` 传入的是自定义的对象,必须让该对象实现 `Comparable` 接口

#### `String`、`StringBuffer` 和`StringBuilder` 的区别在哪里?

* `String` 是不可变对象，因此每次对 `String` 对象进行更改时，都会生成一个新的对象，然后将指针指向新的对象。
* 使用 `StringBuffer` 类时，每次都是对 `StringBuffer` 对象本身进行操作，并不是生成新的对象并改变引用。线程安全！
* `StringBuilder` 和 `StringBuffer` 非常类似，但是是非线程安全的，在单线程中性能比 `StringBuffer` 高。非线程安全！

#### [为什么说 `String` 不可变的？](http://blog.csdn.net/zhangjg_blog/article/details/18319521)

* 什么是不可变对象

	* 如果一个对象，在它创建完成之后，不能再改变它的状态，那么这个对象就是不可变的。
	* 不能改变状态的意思是，不能改变对象内的成员变量，包括基本数据类型的值不能改变，引用类型的变量不能指向其他的对象，引用类型指向的对象的状态也不能改变。

* 在 `Java` 中 `String` 类其实就是对字符数组的封装
* `String` 类中有一个 `char` 数组的 `value` 引用变量，`value` 指向真正的数组对象
* `JDK 1.6` 及以前还有 `offset` 和 `count` 两个变量
* 这三个变量都是 `private final` 的，即一旦这三个值被初始化，就不能再改变了
* 但是可以通过反射改变 `value` 变量引用的对象，进而改变 `String` 对象

#### `sleep` 和 `wait` 的区别

二者都可以暂停当前线程，释放 `CPU` 控制权，区别在于作用于谁和是否释放锁？

* `wait` 方法作用于` Object`，`sleep` 方法作用于 `Thread`
* `Object.wait` 方法在释放 `CPU` 的同时，释放了对象锁的控制，使得其他线程可以使用同步控制块或方法；`Thread.sleep` 方法没有释放锁

#### `final`

> 将方法声明为`final`有两个原因

1. 就是说明你已经知道这个方法提供的功能已经满足你要求，不需要进行扩展，并且也不允许任何从此类继承的类来覆写这个方法，但是继承仍然可以继承这个方法，也就是说可以直接使用。
2. 就是允许编译器将所有对此方法的调用转化为`inline`调用的机制，它会使你在调用`final`方法时，直接将方法主体插入到调用处，而不是进行例行的方法调用，例如保存断点，压栈等，
这样可能会使你的程序效率有所提高，然而当你的方法主体非常庞大时，或你在多处调用此方法，那么你的调用主体代码便会迅速膨胀，可能反而会影响效率，所以你要慎用`final`进行方法定义。

######  `final`, `finally` 和 `finalize`?

* `final`：修饰基本数据类型时，代表常量（不可修改）；修饰对象引用时，引用不可重新赋值，而对象依然可以修改；修饰方法时，代表该方法不可被重写；修饰类时，代表该类不可被继承
* `finally`：异常处理时 `try／catch／finally` 语句中的 `finally` 语句块无论有没有异常都会执行 `finally` 语句块，当然也有特殊情况不执行 `finally`
* `finalize`：垃圾回收器要回收对象的时候，首先会调用这个类的 `finalize` 方法，可以在该方法中释放调用本地方法时分配的内存

#### `HashTable` `HashMap`

1. 继承不同
 
        public class HashTable extends Dictionary implements Map
        public class HashMap extends AbstractMap implements Map

2. `HashTable`中的方法是同步的，而`HashMap`中的方法在缺省情况下是非同步的。在多线程并发的环境下，可以直接使用`HashTable`，但是要使用`HashMap`的话就要自己增加同步处理了。
3. `HashTable`中，`key`和`value`都不允许出现`null`值。在`HashMap`中，`null`可以作为键，这样的键只有一个；可以有一个或多个键所对应的值为`null`。当get()方法返回`null`值时，即可以表示`HashMap`中没有该键，也可以表示该键所对应的值为`null`。
因此，在`HashMap`中不能由`get()`方法来判断`HashMap`中是否存在某个键， 而应该用`containskey()`方法来判断。 两个遍历方式的内部实现上不同。
4. `HashTable`、`HashMap`都使用了 `Iterator`。而由于历史原因，`HashTable`还使用了`Enumeration`的方式 。 哈希值的使用不同，`HashTable`直接使用对象的`hashCode`。
而`HashMap`重新计算`hash`值。
5. `HashTable`和`HashMap`它们两个内部实现方式的数组的初始大小和扩容的方式。`HashTable`中`hash`数组默认大小是11，增加的方式是old*2+1。`HashMap`中`hash`数组的默认大小是16，而且一定是2的指数。

#### `Iterator` `Enumeration`

1. 函数接口不同 `Enumeration`只有2个函数接口。通过`Enumeration`，我们只能读取集合的数据，而不能对数据进行修改。`Iterator`只有3个函数接口。
`Iterator`除了能读取集合的数据之外，也能数据进行删除操作。
2. `Iterator`支持`fail-fast`机制，而`Enumeration`不支持。 `Enumeration` 是`JDK1.0`添加的接口。使用到它的函数包括`Vector`、`HashTable`等类，
这些类都是`JDK 1.0`中加入的，`Enumeration`存在的目的就是为它们提供遍历接口。`Enumeration`本身并没有支持同步，而在`Vector`、`HashTable`实现`Enumeration`时，添加了同步。
而`Iterator`是`JDK1.2`才添加的接口，它也是为了`HashMap`、`ArrayList`等集合提供遍历接口。
`Iterator`是支持`fail-fast`机制的：当多个线程对同一个集合的内容进行操作时，就可能会产生`fail-fast`事件。

#### [`Array`  `ArrayList`](http://blog.qianlicao.cn/translate/2016/03/09/array-vs-arraylist/)

>不同点：

* 灵活性：`ArrayList` 优于 `Array`，`ArrayList` 是动态的；`Array` 是静态的，创建了数组就无法更改它的大小
* 元素类型：`Array` 既可以保存基本类型，也可以保存对象；`ArrayList` 不可以保存基本类型，可以保存封装类
* 实现上：`Array` 是本地的程序设计组件或者数据结构，`ArrayList` 是一个 `Java` 集合类的类
* 性能上：`Array` 会优于 `ArrayList`
* 类型安全方面：`ArrayList` 是类型安全的，支持编译时检查；`Array` 不是类型安全的，支持运行时检查
* 泛型：`ArrayList` 可以显示的支持泛型，`Array` 不可以
* 维度：`Array` 可以是多维度的，`ArrayList` 并不支持指定维度

> 相同点：

* 数据结构：都是基于 `index` 的数据结构
* 空值：都可以存储空值（`null`），但只有 `Object` 的数组可以这样，基本类型会存储他们的默认值
* 重复：都允许元素重复

#### `ArrayLis` `LinkedList`

`ArrayList`初试大小为10，大小不够会调用`grow`扩容：`length = length + (length >> 1)`

`LinkedList`中的`Node`的 `first`和`last`分别指向头尾

`ArrayList`和`LinkedList`在性能上各 有优缺点，都有各自所适用的地方，总的说来可以描述如下：

1. 对`ArrayList`和`LinkedList`而言，在列表末尾增加一个元素所花的开销都是固定的。对`ArrayList`而言，
主要是在内部数组中增加一项，指向所添加的元素，偶尔可能会导致对数组重新进行分配；而对`LinkedList`而言，这个开销是 统一的，分配一个内部`Entry`对象。
2. 在`ArrayList`的中间插入或删除一个元素意味着这个列表中剩余的元素都会被移动；而在`LinkedList`的中间插入或删除一个元素的开销是固定的。
3. `LinkedList`不 支持高效的随机元素访问。
4. `ArrayList`的空间浪费主要体现在在`list`列表的结尾预留一定的容量空间，而`LinkedList`的空间花费则体现在它的每一个元素都需要消耗相当的空间。可以这样说：当操作是在一列 数据的后面添加数据而不是在前面或中间,并且需要随机地访问其中的元素时,使用`ArrayList`会提供比较好的性能；当你的操作是在一列数据的前面或中 间添加或删除数据,并且按照顺序访问其中的元素时,就应该使用`LinkedList`了。

#### `synchronized`

> [参考链接](http://www.cnblogs.com/GnagWang/archive/2011/02/27/1966606.html#!comments)

* 当它用来修饰一个方法或者一个代码块的时候，能够保证在同一时刻最多只有一个线程执行该段代码。
* 当两个并发线程访问同一个对象 `object` 中的这个 `synchronized(this)` 同步代码块时，一个时间内只能有一个线程得到执行。另一个线程必须等待当前线程执行完这个代码块以后才能执行该代码块。
* 然而，当一个线程访问 `object` 的一个 `synchronized(this)` 同步代码块时，另一个线程仍然可以访问该 `object` 中的非 `synchronized(this)` 同步代码块。
* 尤其关键的是，当一个线程访问 `object` 的一个 `synchronized(this)` 同步代码块时，其他线程对` object` 中所有其它 `synchronized(this)` 同步代码块的访问将被阻塞。
* 上一个例子同样适用其它同步代码块。也就是说，当一个线程访问 `object` 的一个 `synchronized(this)` 同步代码块时，它就获得了这个 `object` 的对象锁。
结果，其它线程对该 `object` 对象所有同步代码部分的访问都被暂时阻塞。

#### [`ThreadPoolExecutor`](https://blog.mindorks.com/threadpoolexecutor-in-android-8e9d22330ee3)

* 线程池的目的是实现线程复用，减少频繁创建和销毁线程，提高系统性能
* `ThreadPoolExecutor` 是线程池的核心类，用于创建线程池
* `ThreadPoolExecutor` 的构造方法包括如下参数：

	* `corePoolSize` 核心线程池大小
	* `maximumPoolSize` 线程池最大容量
	* `keepAliveTime` 线程池空闲时，线程存活时间
	* `TimeUnit` 时间单位
	* `ThreadFactory` 线程工厂
	* `BlockingQueue` 任务队列
	* `RejectedExecutionHandler` 线程拒绝策略

我们通常通过` Executors `类的如下几个静态方法来创建不同类型的线程池：

* `newCachedThreadPool`：可缓存线程池，无限大
* `newFixedThreadPool`：定长线程池
* `newScheduledThreadPool`：定长线程池，支持定时及周期性任务执行
* `newSingleThreadExecutor`：单个线程池

## GC

####  GC怎么识别垃圾、其他语言是怎么管理内存的，和java有什么区别？

1.` GC`识别垃圾的算法：引用计数算法和可达性分析算法。

* 引用计数器：通过一个引用计数器去标志对象的被引用数。
* 可达性分析算法：利用一系列称为`GCRoots`的对象为起始点，向下搜索。当一个对象到`GCRoots`没有任何引用链（路径）时候，该对象不可用。

2. `C++`中没有垃圾收集技术，在内存管理领域，拥有每一个对象的所有权，又担负着每一个对象从生命开始到终结的维护责任。`Java`有动态内存分配和垃圾收集技术，借助`JVM`完成无用对象释放，
而且不容易出现内存泄露和内存溢出问题。
3. `C++`经过编译时直接编译成机器码，而`Java`是编译成字节码，由`JVM`解释执行。

#### 什么可以作为`GCRoots`

* 虚拟机栈中引用的对象（栈帧中的本地变量表）
* 方法区中类静态属性引用的对象
* 方法区中常量引用的对象
* 本地方法栈中`JNI`引用的对象

#### 新生代老年代的收集算法

* 标记-清除算法：最基础收集算法，标记出需要回收对象并在标记完成后统一回收。存在效率问题和空间碎片问题。
* 复制算法：将内存划分为两块，每次只用一块，当一块内存满了，将存活对象复制到另外一块上面，不用考虑内存碎片。比如虚拟机的一块Eden空间和两块Survior空间配合使用。
* 标记-整理算法：标记过程同前面一样，但是不是直接清理，而是让所有存活对象向一端移动，然后清理边界以外的内存

#### 新生代为什么用复制算法

1. 新生代的`MinorGC`触发比较频繁，所有必须选择效率较高的收集算法，这里相比标记-整理两步走来说复制算法快得多，
且经过优化的复制算法利用`Eden/S0/S1`的组合运用和老年代的分配担保也可以保证一定的空间利用率。
2. 老年代一般用标记-整理算法，一方面因为`FullGC`不频繁，另一方面因为复制算法需要额外空间分配担保以应对内存对象100%存活的极端情况，适合标记-整理算法。

#### 老年代担保是什么？

1. 针对新生代使用的复制算法，如果`Survior`空间不够存放新生代收集下来的存货对象的时候，就需要依赖老年代的空间了。
2. 前提是老年代本身拥有容纳这些存活对象的空间，这里取每一次晋升到老年队对象容量的平均大小作为经验值和老年代剩余空间比较，决定是否进行`FullGC`让老年代腾出空间。

#### 在不考虑循环引用的前提下：引用计数法和可达性分析的性能比较？

1. `JVM`没有使用引用计数法，而是使用了可达性分析来进行`GC`。`C++11`的智能指针就是基于引用计数的，它提供了`weak_ptr`来解决循环引用的问题。（类似于`Java`中的`WeakReference`，作用类似）
2. 不考虑循环引用情况下引用计数法实现比较简单也快速，能够立即释放内存，而可达性分析必须进行一遍可达性判定才行。
3. 先用引用计数处理绝大多数对象回收，少数循环引用的对象先放在那儿，等积累较多，占用内存比较大的时候，再用可达性算法再来清除一次，这样既能快速回收垃圾，
又解决了循环引用的问题。---`python`就是这样

#### GC一般的触发条件？发生在何时

1. `Java`的`GC`分为`FullGC`和`MinorGC`。
2. `MinorGC`的触发条件：当`young gen`中的`eden`区分配满的时候触发。注意`young GC`中有部分存活对象会晋升到`old gen`，所以`young GC`后`old gen`的占用量通常会有所升高。
3. `FullGC`的触发条件包括：当准备要触发一次`young GC`时，如果发现统计数据说之前`young GC`的平均晋升大小比目前`old gen`剩余的空间大，
则不会触发`young GC`而是转为触发`full GC`（因为`HotSpot VM`的`GC`里，除了`CMS`的`concurrent collection`之外，其它能收集`old gen`的`GC`都会同时收集整个`GC`堆，
包括`young gen`，所以不需要事先触发一次单独的`young GC`）；或者，如果有`perm gen`的话，要在`perm gen`分配空间但已经没有足够空间时，
也要触发一次`full GC`；或者`System.gc()`、`heap dump`带`GC`，默认也是触发`full GC`。

#### GC针对的对象？

* 利用可达性分析算法从`root`搜索不到，而且经过第一次标记、清理后，仍然没有复活的对象。 

#### 四种引用概念？

1. 强引用：类似`Object obj = new Object（）`;这种，垃圾收集器不会回收这种对象。
2. 软引用：描述一些有用但非必需对象，将要发生OOM异常时候会对这些对象进行二次回收。
3. 弱引用：只能生存到下一次`GC`之前
4. 虚引用：一个对象是否有虚引用，完全不会对生存时间造成影响，存在目的就是对象被`GC`时候可以收到系统通知。

* 强引用：是使用最普遍的引用。如果一个对象具有强引用，那垃圾回收器绝不会回收它。
* 软引用：如果一个对象只具有软引用，且内存空间足够，垃圾回收器就不会回收它；如果内存空间不足了，就会回收这些对象的内存。
* 弱引用：只具有弱引用的对象拥有比软引用更短暂的生命周期，在垃圾回收器线程扫描它所管辖的内存区域的过程中，一旦发现了只具有弱引用的对象，不管当前内存空间足够与否，都会回收它的内存。不过，由于垃圾回收器是一个优先级很低的线程，因此不一定会很快发现那些只具有弱引用的对象。
* 如果一个对象仅持有虚引用，那么它就和没有任何引用一样，在任何时候都可能被垃圾回收器回收。一般用于跟踪对象被垃圾回收器回收过程。

#### 内存分配规则和策略

1. 对象优先在`Eden`分配：对象大多数情况下在`Eden`区分配，若`Eden`区域不足，会执行一次`MinorGC`
2. 大对象直接进入老年代：需要大量连续空间的对象直接进入老年代，阈值由`PretenureSizeThreshold`参数控制，避免大量的内存复制。
3. 长期存活对象进入老年代：利用年龄计数器进行年龄计算，多次`MinorGC`后仍然能够被`Survior`容纳的话就晋升到老年代，通过`MaxTenuringThreshold`参数控制。
4. 动态对象年龄判断：并非年龄计数器必须达到阈值才能晋升，如果在`S`空间中相同年龄所有对象大小和大于`S`空间一半，那么年龄大于等于此的对象就可以晋升。
5. 空间分配担保：前面已经详细说过

#### 被可达性分析或者引用计数法标记为无用对象后就一定会被GC回收吗？

1. 不是的，一个对象的死亡至少要经历两次标记过程
2. 如果对象被标记为可回收对象，那他将会被第一次标记并且进行一次筛选，筛选的条件是此对象是否有必要执行`finalize()`方法，当对象没有覆盖`finalize()`方法，
或者`finalize()`方法已经被虚拟机调用过，虚拟机将这两种情况都视为“没有必要执行方法”。（这也是为什么大家不鼓励重写`finalize`方法的原因之一）
3. 对象判断为有必要执行`finalize（）`方法，那么这个对象会放到一个叫做`F-Queue`队列中，并在稍后一个由虚拟机自动建立的、低优先级的`Finalizer`线程去执行它，
这里的执行只是负责触发这个方法，并不一定等这个方法执行结束。`inalize()`方法是对象逃脱死亡命运的最后一次机会，稍后`GC`将对`F-Queue`中的对象进行第二次小规模的标记，
如果对象要在`finalize()`中成功拯救自己——只要重新与引用链上的任何一个对象建立关联即可（这里描述的是可达性分析算法，因为虚拟机几乎都不采取引用计数法）。
比如把自己（this关键字）赋值给某个类变量或者对象的成员变量，那么在第二次标记时他将被移除出“即将回收”的队列；若对象这时候还没有逃脱，那么基本上真的就要被回收了。

#### JVM中的垃圾收集器有哪些？

1. 截止`JDK1.7`，`JVM`包括了`Serial`和`SerialOld`、`ParllelScavenge`和`ParellelOld`、`ParNew`和`CMS`以及`G1`垃圾收集器
2. `Serial`和`SerialOld`是分别针对新生代和老年代的单线程收集器，他的特点就是简单高效，对于限定单个`CPU`环境下没有线程交互开销，用于运行于`Client`模式下的虚拟机
3. `ParNew`收集器是`Serial`收集器的多线程版本，目前只有它能够和`CMS`配合工作。
4. `ParallelScavenge`和`ParallelOld`收集器配合使用，他的特点是关注吞吐量而非尽可能缩短垃圾收集时候的停顿时间。停顿时间影响用户体验，而吞吐量影响工作效率，适用于后台运算任务。
5. `CMS`收集器是以最短回收停顿时间为目标的收集器，适合互联网站或者B/S系统服务端，重视服务响应速度。`CMS`基于标记-清楚算法，运作分为四个步骤：初始标记，并发标记，重新标记，并发清除。
`CMS`有三个明细缺点：
* 并发阶段占用线程导致工作线程减少，程序变慢。
* 无法处理浮动垃圾（并发清理时产生的垃圾）
* 标记-清除算法带来的空间碎片
6. `G1`收集器：

## 设计模式

#### [单例](http://wuchong.me/blog/2014/08/28/how-to-correctly-write-singleton-pattern/)

* 单例模式是一种对象创建型模式，指的是一个类只能被初始化一次，即只有一个实例。
* 这个类只能有一个实例
* 它必须自行创建这个实例
* 它必须自行向整个系统提供这个实例

 单例模式几种写法对比：
 
1. 懒汉式：非线程安全
 
2. 懒汉式（加锁）：线程安全了，但效率低，任何时候只能有一个线程调用方法
 
3. 恶汉式：非懒加载，单例对象依赖参数或配置文件时就无法使用了
 
4. 双重校验锁：推荐
 
5. 匿名内部类：推荐！静态内部类是私有的，除 `getInstance` 没有其他方法可访问它，所以是懒加载；读取实例时不会进行同步，没有性能缺陷
 
6. 枚举：`Android` 中不推荐