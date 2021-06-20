# 基本

+ equals和==：
  + ==比较内存地址，equals如果没有重写也是比较的内存地址。
+ hashcode和equals：
  + Java中规定：
    + equals相等，返回的hashcode一定要相等
    + equals不相等，返回的hashcode不一定要相等
+ Java switch支持byte，short，int，char，enum，String。后两个是语法糖
+ Java long - int的转换是舍弃高位。
+ 存在继承关系时候，初始化顺序：实际上普通实例变量的初始化是在调用构造方法的时候初始化的。但是会优先于构造方法里写的其他方法。
  + 父类（静态变量、静态语句块）
  + 子类（静态变量、静态语句块）
  + 父类（实例变量、普通语句块）
  + 父类（构造函数）
  + 子类（实例变量、普通语句块）
  + 子类（构造函数）
+ Java深度克隆：
  + 重写clone()方法
  + 通过序列化和反序列化实现
+ SPI：Java SPI的具体约定为：当服务的提供者提供了服务接口的一种实现之后，在jar包的META-INF/services/目录里同时创建一个以服务接口命名的文件。该文件里就是实现该服务接口的具体实现类。而当外部程序装配这个模块的时候，就能通过该jar包META-INF/services/里的配置文件找到具体的实现类名，并装载实例化，完成模块的注入。基于这样一个约定就能很好的找到服务接口的实现类，而不需要再代码里制定。jdk提供服务实现查找的一个工具类：java.util.ServiceLoader。
+ 权限范围：
  + public：都能访问
  + protect：当前类和同一个包下的和子类能访问
  + default：当前类和同一个包下的类能访问
  + private：只有当前类能访问
+ 面向对象四大特性：
  + 继承
  + 多态
  + 封装
  + 重载


# 常用注解

+ `@Sharable`：表明类是多线程安全的
+ 

# Object

+ Object方法：

  + ```java
    public native int hashCode()
    
    public boolean equals(Object obj)
    
    protected native Object clone() throws CloneNotSupportedException
    
    public String toString()
    
    public final native Class<?> getClass()
    
    protected void finalize() throws Throwable {}	// jdk9取消
    
    public final native void notify()
    
    public final native void notifyAll()
    
    public final native void wait(long timeout) throws InterruptedException
    
    public final void wait(long timeout, int nanos) throws InterruptedException
    
    public final void wait() throws InterruptedException
    ```

  + 

# 动态代理

+ 输出代理文件：
  + 输出JDK代理文件：`System.getProperties().put("sun.misc.ProxyGenerator.saveGeneratedFiles", "true"); `
  + 输出CGLIB代理文件：`System.setProperty(DebuggingClassWriter.DEBUG_LOCATION_PROPERTY, "C:\\class"); `

## CGLIB动态代理

https://blog.csdn.net/yhl_jxy/article/details/80633194

+ CGLIB动态代理是基于ASM字节码生成实现的动态代理。将代理对象类的class文件加载进来，并修改其字节码生成子类来处理。
+ CGLIB会为需要增强的类创建一个子类。然后重写父类的方法。如下所示，生成的子类中会有一个判断，如果判断不为null，则调用interceptor中的方法（我们重写的`MethodInterceptor`的`intercept`方法）
+ CGLIB会为创建一个FastClass，调用的时候通过switch去查找指定的方法，然后调用。

```java
/**
* 父类方法
*/
public void show(){
    System.out.println("Show");
}

public final void show() {
    MethodInterceptor var10000 = this.CGLIB$CALLBACK_0;
    if (var10000 == null) {
        CGLIB$BIND_CALLBACKS(this);
        var10000 = this.CGLIB$CALLBACK_0;
    }

    if (var10000 != null) {
        // 调用我们重写的MethodInterceptor的intercept方法
        var10000.intercept(this, CGLIB$show$0$Method, CGLIB$emptyArgs, CGLIB$show$0$Proxy);
    } else {
        // 直接调用父类方法
        super.show();
    }
}
```

## JDK动态代理

+ JDK动态代理通过反射生成一个实现了代理接口并且继承proxy的实现类。在具体方法中调用proxy里的invokehandler来处理。
+ JDK是通过Java的发射去调用代理方法的，相比CGLIB的FastClass机制会慢。

```java
    public final void show() throws  {
        try {
            super.h.invoke(this, m3, (Object[])null);
        } catch (RuntimeException | Error var2) {
            throw var2;
        } catch (Throwable var3) {
            throw new UndeclaredThrowableException(var3);
        }
    }
```



# 异常

+ Java的异常的总接口是Throwable。
+ Throwable的两个子类为Error和Exception
+ Exception分为编译时异常（需要处理）和运行时异常（不需要处理）

![](图片/Java基础/Java异常.png)

## OutOfMemoryError

+ Java堆溢出：
  + Java堆内存的OOM是实际应用中最常见的内存溢出异常情况。出现Java堆内存溢出时，异常堆栈信息“java.lang.OutOfMemoryError”会跟着进一步提示“Java Heap space”。
  + 要解决这个区域的异常，一般是首先通过内存映像分析工具（如Eclipse Memory Analyzer）对dump出来的堆转储快照进行分析，重点是确认内存中的对象是否是必要的，也就是要先分清楚到底是出现了内存泄漏（Memory Leak）还是内存溢出（Memory Overflow）。
  + 内存泄漏：可进一步用通过工具查看泄漏对象到GC Roots的引用链。就能找到泄漏对象是通过怎样的路径与GC Roots相关联并导致垃圾收集器无法自动回收它们的。掌握了泄漏对象的类型信息，以及GC Roots引用链的信息，就可以比较准确地定位出泄漏代码的位置。
  + 内存溢出：检查虚拟机的对参数（-Xmx和-Xms），与机器物理内存对比看是否还可以调大，从代码上检查是否存在某些对象生命周期过长、持有状态时间过长的情况，尝试减少程序运行期的内存消耗。
+ 虚拟机栈和本地方法栈溢出：
  + 如果虚拟机在扩展栈时无法申请到足够的内存空间，则抛出OutOfMemoryError异常。
+ 运行时常量池溢出：
  + 运行时常量池溢出，在OutOfMemoryError后面跟随的提示信息是“PermGen Space”，说明运行时常量池属于方法区的一部分。
+ 方法区溢出：
+ 本地内存溢出

# 序列化

+ 注意：
  + 如果类要实现序列化的话，必须写`serialVersionUID`，否则编译器默认生成一个哈希字段，如果系统进行升级的话，可能会导致反序列化失败。
  + 在序列化中，对于枚举类只会出现枚举的名称，反序列化的时候会根据名称调用Enum.valueof方法获取枚举值，由于系统升级可能会导致反序列化失败。

# static与final所处位置

```java
class Fruit {
    static int x = 10;    // static int x 在方法区
    static BigWaterMelon bigWaterMelon_1 = new BigWaterMelon(x);    // static BigWaterMelon bigWaterMelon_1在方法区，而new BigWaterMelon(x)在堆上
 
    int y = 20;        // int y=20 在堆上
    BigWaterMelon bigWaterMelon_2 = new BigWaterMelon(y);    // BigWaterMelon bigWaterMelon_2 与 new BigWaterMelon(y) 都在堆上
 
    public static void main(String[] args) {    // String[] args 在vm栈上
        final Fruit fruit = new Fruit();        // Fruit fruit 在vm栈上，而 new Fruit() 在堆上
 
        int z = 30;    // int z = 30 在vm栈上
        BigWaterMelon bigWaterMelon_3 = new BigWaterMelon(z);    // BigWaterMelon bigWaterMelon_3 在vm栈上，而new BigWaterMelon(z)在堆上
 
        new Thread() {
            @Override
            public void run() {  
                int k = 100;    // int k=100 在栈帧上
                setWeight(k);
            }
 
            void setWeight(int waterMelonWeight) {    // int waterMelonWeight 在栈帧上
                fruit.bigWaterMelon_2.weight = waterMelonWeight;
            }
        }.start();
    }
}
 
class BigWaterMelon {
    public BigWaterMelon(int weight) {
        this.weight = weight;
    }
 
    public int weight;
}
```



# Java集合类

+ 快速失败（fail-fast）：当在迭代一个集合时，如果有另外一个线程在修改这个集合，就会跑出ConcurrentModificationException，java.util下都是快速失败。
  + 在使用迭代器对集合对象进行遍历的时候，如果A线程对集合进行遍历，正好B线程对集合进行修改（增加、删除、修改），则A线程会跑出ConcurrentModificationException异常。
  + 迭代器在遍历时直接访问集合中的内容，并且在遍历过程中使用一个modCount变量。集合在被遍历期间如果内容发生变化，就会改变modCount的值。每当迭代器使用hasNext（）或next（）遍历下一个元素之前，都会先检查modCount变量是否为expectmodCount值。如果是的话就返回遍历；否则抛出异常，终止遍历。
+ 安全失败（fail-safe）：在迭代时候会在集合二层做一个拷贝，所以在修改集合上层元素不会影响下层。在java.util.concurrent包下都是安全失败。
  + 采用安全失败机制的集合容器，在遍历时不是直接在集合上访问的，而是先复制原有集合内容，在拷贝的集合上进行遍历。

# try catch

+ 在jdk1.7以后，try可以带括号，如果流实现了`AutoCloseable`接口，那么可以在直接通过`try(流){}`的方式关闭流。

# Java优化

+ 不适用的对象应手动赋值为null。（参考Java虚拟机栈帧局部变量表复用）
+ this引用溢出