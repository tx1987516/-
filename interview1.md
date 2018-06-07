14。jvm组成、 算法，垃圾回收器、类加载机制

答

* jvm组成 java堆(eden,from,to,老年代)，java虚拟机栈、方法区、本地方法栈、程序计数器（字节码解释器工作时就是改变这个计数器的值）、直接内存、java8去掉方法区，多了一个元空间

* 垃圾回收算法，整体采用分代收集算法。有标记-清除算法（mark-sweep）、复制算法、标记-整理算法（mark-compact）

* 垃圾回收器 （serial连续收集器 ，ParNew并行收集器）/(Serial Old,CMS(concurrent mark sweep));Parallel Scavenge(吞吐量优先收集器)/(Parallel Old,Serial Old)  并发，并行多了一个并发标记的阶段

* 类加载机制 启动加载器（lib 或-Xbootclasspath参数），扩展加载器（lib/ext或java.ext.dirs系统变量），应用程序加载器（classpath），自定义加载器，双亲委派机制

15.redis 做分布式锁

答：

* 选用set(key,value,30秒，NX) 获取锁，执行结束后释放锁的，为保证原子性，选择lua脚本执行，脚本逻辑为判断是否为自己线程加的锁，并del(key) ,并为线程开起一个守护线程，在第29秒开始执行有效时间见续命20秒，每20秒执行一次，保证执行内容时长超30秒的锁也能又自己主动释放。

16.segment底层的实现

答：

* jdk7才用segment继承reentrantlock可重入锁机制 get方法两次使用unsafe的getObjectVolatile方法，put方法先定位segmengt再在segment的put方法中加锁
* jdk 8完全取消segment锁分段的思想，采用数组node+链表/红黑树的设计思路链表长度大于8时转成红黑树，小于6时转为链表 put方法是用的synchronized关键字，说明jdk8jvm自带锁已被优化。

17.++i;i++底层实现

18.动态代理具体细节

答：
* JDK代理  实现invocationHandler接口，并复写invoke方法，调用method.invoke(target，args)方法相当于调用被代理的对象方法，用Proxy.newProxyInstance创建代理对象（需要接口）

* cglib代理，实现MethodInterceptor,并复写interceptor方法，调用methodProxy.invokeSuper(o,args)，相当于调用被代理的对象，创建enhance，设置superClass为被代理对象，callback回调为实现methodInterceptor实现类，然后再create.(非final类即可)

* javassist动态代理 

* HandlerInterceptor 为SpringMvc拦截器接口

19 tomcat底层细节

20 线程基础重学

答 ：yield 谦让出去，重新竞争， join（本质是wait（0））等待完成，只有守护线程的情况下，jvm会马上停止，suspend（挂起）resume（继续执行）过时方法，不建议使用，不能保证执行先后顺序，interrupt通知中断，然后通过是否是被通知状态做出处理，sleep的catch后需要重新interrupt，因为抛出interruptexception后中断状态被重置了，start才会开启新线程，直接调用run只会执行方法，不会开启新线程，stop不推荐使用，比较暴力，它会释放所有的锁，wait.notify使用前需先获得锁

21.关于树 二叉查找树，自平衡二叉查找树,多路平衡查找树，B+树

