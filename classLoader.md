
## ClassLoader
> ym59779  
> 1st Mar 2019

### 1.Java类装载过程

![](https://images2015.cnblogs.com/blog/809371/201608/809371-20160831162844230-2019325734.jpg)

* 装载：通过累的全限定名获取二进制字节流，将二进制字节流转换成方法区中的运行时数据结构，在内存中生成Java.lang.class对象； 

* 链接：执行下面的校验、准备和解析步骤，其中解析步骤是可以选择的； 
 * 校验：检查导入类或接口的二进制数据的正确性；（文件格式验证，元数据验证，字节码验证，符号引用验证） 

 * 准备：给类的静态变量分配并初始化存储空间； 

 * 解析：将常量池中的符号引用转成直接引用；主要包括4种解析过程：类或接口的解析、字段解析、类方法解析、接口方法解析。
 >常量池中主要存放2大类常量：字面量（Literal）和符号引用（Symbolic References）。
字面量主要指文本字符串、被声明为final的常量值。  
符号引用主要包括以下3类：  
> 1.类和接口的全限定名
> 2.字段的名称和描述符
> 3.方法的名称和描述符

* 初始化：激活类的静态变量的初始化Java代码和静态Java代码块，并初始化程序员设置的变量值。

> 必须立即对类进行初始化4种情况：  
> 1.遇到new、getstatic、putstatic、invokestatic这4条字节码指令时，如果类还没有初始化过则先对其进行初始化。与这几条指令对应的场景通常有：采用new关键字实例化一个类的对象、读取或设置类的静态字段（被final关键字修饰并且在编译期已经生成常量的除外）、调用类的静态方法；   
2.对类进行反射调用时，如果该类还没有初始化，则先触发其初始化；  
3.当对类进行初始化时，如果发现其父类还没有初始化过，则先触发其父类的初始化；  
4.当虚拟机启动时，会先对入口类（包含main()方法的类）进行初始化；


> 不会进行初始化：  
> 1.通过子类调用父类的静态变量、静态方法不会触发子类初始化。  
> 2.对类里的常量字段进行引用不会触发类的初始化.   
> 3.通过数组定义来引用类不会触发类的初始化. 

**Q**: How about Interface ?  
**Q**：类加载顺序？  
**Q**：正确初始化？

### 2.双亲委派模型
![](https://upload-images.jianshu.io/upload_images/5955727-6178fd65e0c340e5.jpg)

1. 组合模式.
2. instance of & ==  
一是class信息是否“相等”，这里的“相等”指的是描述类的class信息是一致的，包括包名一致、类名一致、类里的信息一致等；  
另一个就是加载该class的ClassLoader是否是同一个   
**Q**：如何实现使用多个相同单例模式类 ？
3. 类回收
	1. 该类所有的实例都已经被回收，也就是说Java堆中不存在该类的任何实例；

	2. 加载该类的ClassLoader已经被回收；

	3. 该类对应的java.lang.Class对象没有在任何地方被引用，无法在任何地方通过反射访问该类的方法

### 3.Tips
1. 类加载死锁
 
 
case 1

``` java
public class Test { 

    public static class A {

        static {
            System.out.println("class A init.");
            B b = new B();
        }   
        
        public static void test() {
            System.out.println("method test called in class A");
        }
    }
    
    public static class B {
        
        static {
            System.out.println("class B init.");
            A a = new A();
        }
        
        public static void test() {
            System.out.println("method test called in class B");
        }
    }
    
    public static void main(String[] args) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                A.test();
            }       
        }).start();
            
        new Thread(new Runnable() {
            @Override
            public void run() {
                B.test();
            }       
        }).start();
    }   
}
```

```
class A init.
class B init.
```

**Q** : how about below ?

``` java
public static void main(String[] args) {
    A.test();
    B.test();
}
```

case 2

``` java
public class Test { 

    public static class Parent {
        static {
            System.out.println("Parent init.");
        }

        public static final Parent EMPTY = new Child();
        
        public static void test() {
            System.out.println("test called in class Parent.");
        }
        
    }
    
    public static class Child extends Parent {      
        static {
            System.out.println("Child init.");
        }
    }
    
    public static void main(String[] args) {
        Thread t1 = new Thread(new Runnable() {
            @Override
            public void run() {
                Child c = new Child();
            }       
        });
        Thread t2 = new Thread(new Runnable() {
            @Override
            public void run() {
                Parent.test();
            }       
        });
        t1.start();
        t2.start();
    }   
}
```
```
Parent init.
```

2.forName() Vs loadClass() Vs findClass()

* 关于forName()方法  
这个方法总是返回要加载的类的Class类的实例  
1、forName(String className)单参数时, initialize=true  
    a.总是使用当前类装载器(也就是装载执行forName()请求的类  的类装载器)  
    b.总是初始化这个被装载的类(当然也包括：装载、连接、初始化)  
2、forName(String className, boolean initialize, ClassLoader loader)  
    a.loader指定装载参数类所用的类装载器，如果null则用bootstrp装载器。  
    b.initialize=true时，肯定连接，而且初始化了；  
    c.false时，绝对不会初始化，但是可能被连接了，但是这里有个例外，如果在调用这个forName()  前，已经被初始化了(当然，这里也暗含着：className是被同一个loader所装载的，即被参数中的loader所装载的，而且这个类被初始化了)，那么返回的类型也肯定是被初始化的  

* 关于用户自定义的类装载器的loadClass()方法  
1、loadClass(String name)单参数时, resolve=false  
    a.如果这个类已经被这个类装载器所装载，那么，返回这个已经被装载的类型的Class的实例，否则，就用这个自定义的类装载器来装载这个class，这时不知道是否被连接。绝对不会被初始化  
    b.这时唯一可以保证的是，这个类被装载了。但是不知道这个类是不是被连接和初始化了  
2、loadClass(String name, boolean resolve)  
    a.resolve=true时，则保证已经装载，而且已经连接了。resolve=false时，则仅仅是去装载这个类，不关心是否连接了，所以此时可能被连接了，也可能没有被连接    
        
* findClass() 一般自定义只需要重载此方法即可，会由loadClass()调用。
