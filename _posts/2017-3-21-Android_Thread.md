---
layout:     post
title:      "Android 异步，线程"
subtitle:   "异步，线程 总结"
date:       2017-3-21
author:     "y"
header-mask: 0.3
header-img: "img/android.jpg"
catalog: true
tags:
    
---


## 异步

Android之所以有Handler和AsyncTask，都是为了不阻塞UI线程，而且UI的更新只能在主线程中完成，所以异步处理是不可避免的,随着RxJava的介入，让Android的异步操作更加方便而且代码结构逻辑更加清晰

#### AsyncTask

>AsyncTask,是android提供的轻量级的异步类,可以直接继承AsyncTask,在类中实现异步操作,并提供接口反馈当前异步执行的程度(可以通过接口实现UI进度更新),最后反馈执行的结果给UI主线程.

> 就是一个封装过的后台任务类，顾名思义就是异步任务

在Android 1.6之前的版本，AsyncTask是串行的

在1.6至2.3的版本，改成了并行的

在2.3之后的版本又做了修改，可以支持并行和串行，当想要串行执行时，直接执行execute()方法，如果需要并行执行，则要执行executeOnExecutor(Executor)......

优点:

	简单,快捷
	过程可控

缺点:

	在使用多个异步操作和并需要进行Ui变更时,就变得复杂起来.

AsyncTask 相比 Handler 轻量一些(代码轻量一些，实际上要比handler更耗资源)

AsyncTask 位置为android.os.AsyncTask，要使用AsyncTask工作我们要提供三个泛型参数，并重载几个方法

AsyncTask定义了三种泛型类型 `Params` `Progress`和`Result`。

	Params 启动任务执行的输入参数，比如HTTP请求的URL。
	Progress 后台任务执行的百分比。
	Result 后台执行任务返回的结果

异步加载数据最少要重写两个方法：

> doInBackground(Params…) 

后台执行，比较耗时的操作都可以放在这里,但是要注意的是这里不能直接操作UI，此方法在后台线程执行，完成任务的主要工作，通常需要较长的时间。

> onPostExecute(Result)  

可以用在`doInBackground` 得到的结果处理操作UI。 
此方法在主线程执行，任务执行的结果作为此方法的参数返回

有必要的话还得重写以下这三个方法：

> onProgressUpdate(Progress…)   

可以使用进度条增加用户体验度。 此方法在主线程执行，用于显示任务执行的进度

> onPreExecute()       

这里是最终用户调用Excute时的接口，当任务执行之前开始调用此方法，可以在这里显示进度对话框。

> onCancelled()             

用户调用取消时，要做的操作

使用时，必须要遵守几个规矩

	Task的实例必须在UI thread中创建；
	execute方法必须在UI thread中调用；

	不要手动调用
	onPreExecute(),
	onPostExecute(Result)，
	doInBackground(Params...), 
	onProgressUpdate(Progress...)
	这几个方法；

	该task只能被执行一次，否则多次调用时将会出现异常；

#### Handler

>在Handler异步实现时,涉及到 Handler, Looper, Message,Thread四个对象，实现异步的流程是主线程启动Thread（子线程）运行并生成Message-Looper获取Message并传递给HandlerHandler逐个获取Looper中的Message，并更新UI

>主要接受子线程发送的数据, 然后更新UI.

优点:

	结构清晰，功能定义明确
	对于多个后台任务时，简单，清晰

缺点:
	
	在单个后台异步处理时，显得代码过多，过于繁琐

当应用程序启动时，Android首先会开启一个主线程, 主线程为管理界面中的UI控件，进行事件分发；

更新UI只能在主线程中更新，这个时候，`Handler`就需要出来解决这个复杂的问题;

由于`Handler`运行在主线程中(UI线程中),它与子线程可以通过`Message`对象来传递数据,

这个时候，`Handler`就承担着接受子线程传过来的`Message`对象, 把这些消息放入主线程队列中，配合主线程进行更新UI

子线程用sedMessage()方法传递数据

>Handler中分发消息的一些方法

	post(Runnable)
	postAtTime(Runnable,long)
	postDelayed(Runnable long)
	sendEmptyMessage(int)
	sendMessage(Message)
	sendMessageAtTime(Message,long)
	sendMessageDelayed(Message,long)


#### 异步消息处理

>Handler 、 Looper 、Message 

`Looper` 创建一个`MessageQueue`，然后进入一个无限循环不断从`MessageQueue`中读取消息，消息的创建者就是一个或多个Handler

>looper

Looper主要是`prepare()`和`loop()`两个方法

主要作用：

	与当前线程绑定，保证一个线程只会有一个Looper实例，同时一个Looper实例也只有一个MessageQueue。
	
	loop()方法，不断从MessageQueue中去取消息，交给消息的target属性的dispatchMessage去处理
	


>handler

`Handler的`构造方法，会首先得到当前线程中保存的`Looper`实例，进而与`Looper`实例中的`MessageQueue`想关联。

`Handler`的`sendMessage`方法，会给`msg`的`target`赋值为`handler`自身，然后加入`MessageQueue`中。

在构造`Handler`实例时，会重写`handleMessage`方法，也就是`dispatchMessage(msg)`最终调用的方法。

#### RxJava，RxAndroid

>一个在 Java VM 上使用可观测的序列来组成异步的、基于事件的程序的库

这里不做叙述，因为我感觉 抛物线的[给 Android 开发者的 RxJava 详解](http://gank.io/post/560e15be2dca930e00da1083#toc_1)非常非常详细,值得深读

## 线程

#### 多线程

	1 继承Thread,重写里面的run方法
	2 实现runnable接口

android中几个实现方法：

	1	Activity.runOnUiThread(Runnable)
	2	View.post(Runnable);
		View.postDelay(Runnable , long)
	3	Handler
	4	AsyncTask

推荐后者，第一，java没有单继承的限制 第二，还可以隔离代码

#### 线程池
	
	提升性能,创建和消耗对象费时费CPU资源
	
	防止内存过度消耗,控制活动线程的数量，防止并发线程过多。
	
	重用线程池中的线程，避免因为线程的创建和销毁所带来的性能开销 

	能有效控制线程池的最大并发数，避免大量的线程之间因互相抢占系统资源而导致的阻塞现象  

	能够对线程简单的管理，并提供定时执行以及指定间隔循环执行等功能
	
	ExecutorService fixedThreadExecutor = Executors.newFixedThreadPool(5); 

	ExecutorService cachedThreadExecutor = Executors.newCachedThreadPool();  
	
	ExecutorService scheduledThreadExecutor = Executors.newScheduledThreadPool(5);
	
	ExecutorService singleThreadExecutor = Executors.newSingleThreadExecutor(); 

简单的例子： 下载时要求最多一次只能下载五个数据，这个时候接入线程池就可以很方便的控制逻辑的操作

## Java多线程问题

1. 进程和线程之间有什么不同？

	一个进程是一个独立(self contained)的运行环境，它可以被看作一个程序或者一个应用。而线程是在进程中执行的一个任务。Java运行环境是一个包含了不同的类和程序的单一进程。线程可以被称为轻量级进程。线程需要较少的资源来创建和驻留在进程中，并且可以共享进程中的资源。

2. 多线程编程的好处是什么？

	在多线程程序中，多个线程被并发的执行以提高程序的效率，CPU不会因为某个线程需要等待资源而进入空闲状态。多个线程共享堆内存(heap memory)，因此创建多个线程去执行一些任务会比创建多个进程更好。举个例子，Servlets比CGI更好，是因为Servlets支持多线程而CGI不支持。

3. 用户线程和守护线程有什么区别？


	当我们在Java程序中创建一个线程，它就被称为用户线程。一个守护线程是在后台执行并且不会阻止JVM终止的线程。当没有用户线程在运行的时候，JVM关闭程序并且退出。一个守护线程创建的子线程依然是守护线程。

4. 我们如何创建一个线程？


	有两种创建线程的方法：一是实现Runnable接口，然后将它传递给Thread的构造函数，创建一个Thread对象；二是直接继承Thread类。若想了解更多可以阅读这篇关于如何在Java中创建线程的文章。

5. 有哪些不同的线程生命周期？


	当我们在Java程序中新建一个线程时，它的状态是New。当我们调用线程的start()方法时，状态被改变为Runnable。线程调度器会为Runnable线程池中的线程分配CPU时间并且讲它们的状态改变为Running。其他的线程状态还有Waiting，Blocked 和Dead。读这篇文章可以了解更多关于线程生命周期的知识。

6. 可以直接调用Thread类的run()方法么？


	当然可以，但是如果我们调用了Thread的run()方法，它的行为就会和普通的方法一样，为了在新的线程中执行我们的代码，必须使用Thread.start()方法。

7. 如何让正在运行的线程暂停一段时间？


	我们可以使用Thread类的Sleep()方法让线程暂停一段时间。需要注意的是，这并不会让线程终止，一旦从休眠中唤醒线程，线程的状态将会被改变为Runnable，并且根据线程调度，它将得到执行。

8. 你对线程优先级的理解是什么？


	每一个线程都是有优先级的，一般来说，高优先级的线程在运行时会具有优先权，但这依赖于线程调度的实现，这个实现是和操作系统相关的(OS dependent)。我们可以定义线程的优先级，但是这并不能保证高优先级的线程会在低优先级的线程前执行。线程优先级是一个int变量(从1-10)，1代表最低优先级，10代表最高优先级。

9. 什么是线程调度器(Thread Scheduler)和时间分片(Time Slicing)？


	线程调度器是一个操作系统服务，它负责为Runnable状态的线程分配CPU时间。一旦我们创建一个线程并启动它，它的执行便依赖于线程调度器的实现。时间分片是指将可用的CPU时间分配给可用的Runnable线程的过程。分配CPU时间可以基于线程优先级或者线程等待的时间。线程调度并不受到Java虚拟机控制，所以由应用程序来控制它是更好的选择（也就是说不要让你的程序依赖于线程的优先级）。

10. 在多线程中，什么是上下文切换(context-switching)？


	上下文切换是存储和恢复CPU状态的过程，它使得线程执行能够从中断点恢复执行。上下文切换是多任务操作系统和多线程环境的基本特征。

11. 你如何确保main()方法所在的线程是Java程序最后结束的线程？

	
	我们可以使用Thread类的joint()方法来确保所有程序创建的线程在main()方法退出前结束。这里有一篇文章关于Thread类的joint()方法。

12. 线程之间是如何通信的？


	当线程间是可以共享资源时，线程间通信是协调它们的重要的手段。Object类中wait()\notify()\notifyAll()方法可以用于线程间通信关于资源的锁的状态。点击这里有更多关于线程wait, notify和notifyAll.

13. 为什么线程通信的方法wait(), notify()和notifyAll()被定义在Object类里？


	Java的每个对象中都有一个锁(monitor，也可以成为监视器) 并且wait()，notify()等方法用于等待对象的锁或者通知其他线程对象的监视器可用。在Java的线程中并没有可供任何对象使用的锁和同步器。这就是为什么这些方法是Object类的一部分，这样Java的每一个类都有用于线程间通信的基本方法

14. 为什么wait(), notify()和notifyAll()必须在同步方法或者同步块中被调用？


	当一个线程需要调用对象的wait()方法的时候，这个线程必须拥有该对象的锁，接着它就会释放这个对象锁并进入等待状态直到其他线程调用这个对象上的notify()方法。同样的，当一个线程需要调用对象的notify()方法时，它会释放这个对象的锁，以便其他在等待的线程就可以得到这个对象锁。由于所有的这些方法都需要线程持有对象的锁，这样就只能通过同步来实现，所以他们只能在同步方法或者同步块中被调用。

15. 为什么Thread类的sleep()和yield()方法是静态的？


	Thread类的sleep()和yield()方法将在当前正在执行的线程上运行。所以在其他处于等待状态的线程上调用这些方法是没有意义的。这就是为什么这些方法是静态的。它们可以在当前正在执行的线程中工作，并避免程序员错误的认为可以在其他非运行线程调用这些方法。

16. 如何确保线程安全？


	在Java中可以有很多方法来保证线程安全——同步，使用原子类(atomic concurrent classes)，实现并发锁，使用volatile关键字，使用不变类和线程安全类。在线程安全教程中，你可以学到更多。

17. volatile关键字在Java中有什么作用？

	
	当我们使用volatile关键字去修饰变量的时候，所以线程都会直接读取该变量并且不缓存它。这就确保了线程读取到的变量是同内存中是一致的。

18. 同步方法和同步块，哪个是更好的选择？


	同步块是更好的选择，因为它不会锁住整个对象（当然你也可以让它锁住整个对象）。同步方法会锁住整个对象，哪怕这个类中有多个不相关联的同步块，这通常会导致他们停止执行并需要等待获得这个对象上的锁。

19. 如何创建守护线程？


	使用Thread类的setDaemon(true)方法可以将线程设置为守护线程，需要注意的是，需要在调用start()方法前调用这个方法，否则会抛出IllegalThreadStateException异常。

20. 什么是ThreadLocal?


	ThreadLocal用于创建线程的本地变量，我们知道一个对象的所有线程会共享它的全局变量，所以这些变量不是线程安全的，我们可以使用同步技术。但是当我们不想使用同步的时候，我们可以选择ThreadLocal变量。

	每个线程都会拥有他们自己的Thread变量，它们可以使用get()\set()方法去获取他们的默认值或者在线程内部改变他们的值。ThreadLocal实例通常是希望它们同线程状态关联起来是private static属性。在ThreadLocal例子这篇文章中你可以看到一个关于ThreadLocal的小程序。

21. 什么是Thread Group？为什么建议使用它？


	ThreadGroup是一个类，它的目的是提供关于线程组的信息。

	ThreadGroup API比较薄弱，它并没有比Thread提供了更多的功能。它有两个主要的功能：一是获取线程组中处于活跃状态线程的列表；二是设置为线程设置未捕获异常处理器(ncaught exception handler)。但在Java 1.5中Thread类也添加了setUncaughtExceptionHandler(UncaughtExceptionHandler eh) 方法，所以ThreadGroup是已经过时的，不建议继续使用。

22. 什么是Java线程转储(Thread Dump)，如何得到它？


	线程转储是一个JVM活动线程的列表，它对于分析系统瓶颈和死锁非常有用。有很多方法可以获取线程转储——使用Profiler，Kill -3命令，jstack工具等等。我更喜欢jstack工具，因为它容易使用并且是JDK自带的。由于它是一个基于终端的工具，所以我们可以编写一些脚本去定时的产生线程转储以待分析。读这篇文档可以了解更多关于产生线程转储的知识。

23. 什么是死锁(Deadlock)？如何分析和避免死锁？


	死锁是指两个以上的线程永远阻塞的情况，这种情况产生至少需要两个以上的线程和两个以上的资源。

	分析死锁，我们需要查看Java应用程序的线程转储。我们需要找出那些状态为BLOCKED的线程和他们等待的资源。每个资源都有一个唯一的id，用这个id我们可以找出哪些线程已经拥有了它的对象锁。

	避免嵌套锁，只在需要的地方使用锁和避免无限期等待是避免死锁的通常办法，阅读这篇文章去学习如何分析死锁。

24. 什么是Java Timer类？如何创建一个有特定时间间隔的任务？


	java.util.Timer是一个工具类，可以用于安排一个线程在未来的某个特定时间执行。Timer类可以用安排一次性任务或者周期任务。

	java.util.TimerTask是一个实现了Runnable接口的抽象类，我们需要去继承这个类来创建我们自己的定时任务并使用Timer去安排它的执行。

25. 什么是线程池？如何创建一个Java线程池？


	一个线程池管理了一组工作线程，同时它还包括了一个用于放置等待执行的任务的队列。

	java.util.concurrent.Executors提供了一个 java.util.concurrent.Executor接口的实现用于创建线程池。线程池例子展现了如何创建和使用线程池，或者阅读ScheduledThreadPoolExecutor例子，了解如何创建一个周期任务。

## Java并发问题


1. 什么是原子操作？在Java Concurrency API中有哪些原子类(atomic classes)？


	原子操作是指一个不受其他操作影响的操作任务单元。原子操作是在多线程环境下避免数据不一致必须的手段。

	int++并不是一个原子操作，所以当一个线程读取它的值并加1时，另外一个线程有可能会读到之前的值，这就会引发错误。

	为了解决这个问题，必须保证增加操作是原子的，在JDK1.5之前我们可以使用同步技术来做到这一点。到JDK1.5，java.util.concurrent.atomic包提供了int和long类型的装类，它们可以自动的保证对于他们的操作是原子的并且不需要使用同步。可以阅读这篇文章来了解Java的atomic类。

2. Java Concurrency API中的Lock接口(Lock interface)是什么？对比同步它有什么优势？


	Lock接口比同步方法和同步块提供了更具扩展性的锁操作。他们允许更灵活的结构，可以具有完全不同的性质，并且可以支持多个相关类的条件对象。

	它的优势有：
	
	- 可以使锁更公平
	- 可以使线程在等待锁的时候响应中断
	- 可以让线程尝试获取锁，并在无法获取锁的时候立即返回或者等待一段时间
	- 可以在不同的范围，以不同的顺序获取和释放锁


3. 什么是Executors框架？


	Executor框架同java.util.concurrent.Executor 接口在Java 5中被引入。Executor框架是一个根据一组执行策略调用，调度，执行和控制的异步任务的框架。

	无限制的创建线程会引起应用程序内存溢出。所以创建一个线程池是个更好的的解决方案，因为可以限制线程的数量并且可以回收再利用这些线程。利用Executors框架可以非常方便的创建一个线程池，阅读这篇文章可以了解如何使用Executor框架创建一个线程池。

4. 什么是阻塞队列？如何使用阻塞队列来实现生产者-消费者模型？


	java.util.concurrent.BlockingQueue的特性是：当队列是空的时，从队列中获取或删除元素的操作将会被阻塞，或者当队列是满时，往队列里添加元素的操作会被阻塞。
	
	阻塞队列不接受空值，当你尝试向队列中添加空值的时候，它会抛出NullPointerException。
	
	阻塞队列的实现都是线程安全的，所有的查询方法都是原子的并且使用了内部锁或者其他形式的并发控制。

	BlockingQueue 接口是java collections框架的一部分，它主要用于实现生产者-消费者问题。


5. 什么是Callable和Future?


	Java 5在concurrency包中引入了java.util.concurrent.Callable 接口，它和Runnable接口很相似，但它可以返回一个对象或者抛出一个异常。

	Callable接口使用泛型去定义它的返回类型。Executors类提供了一些有用的方法去在线程池中执行Callable内的任务。由于Callable任务是并行的，我们必须等待它返回的结果。java.util.concurrent.Future对象为我们解决了这个问题。在线程池提交Callable任务后返回了一个Future对象，使用它我们可以知道Callable任务的状态和得到Callable返回的执行结果。Future提供了get()方法让我们可以等待Callable结束并获取它的执行结果。

6. 什么是FutureTask?


	FutureTask是Future的一个基础实现，我们可以将它同Executors使用处理异步任务。通常我们不需要使用FutureTask类，单当我们打算重写Future接口的一些方法并保持原来基础的实现是，它就变得非常有用。我们可以仅仅继承于它并重写我们需要的方法。阅读Java FutureTask例子，学习如何使用它。

7. 什么是并发容器的实现？


	Java集合类都是快速失败的，这就意味着当集合被改变且一个线程在使用迭代器遍历集合的时候，迭代器的next()方法将抛出ConcurrentModificationException异常。

	并发容器支持并发的遍历和并发的更新。
	
	主要的类有ConcurrentHashMap, CopyOnWriteArrayList 和CopyOnWriteArraySet，阅读这篇文章了解如何避免ConcurrentModificationException。

8. Executors类是什么？


	Executors为Executor，ExecutorService，ScheduledExecutorService，ThreadFactory和Callable类提供了一些工具方法。

	Executors可以用于方便的创建线程池。