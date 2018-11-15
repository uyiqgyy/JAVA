# Spring DI & IoC
> _ym59779_ 
> _2018 Nov 16_
### 1.**SOLID**
#### [S] Single Responsibility Principle (单一功能原则)
![](https://images2015.cnblogs.com/blog/833855/201612/833855-20161225134507339-1405385614.png)
>让一个类只做一种类型责任，当这个类需要承担其他类型的责任的时候，就需要分解这个类。
#### [o] Open Close Principle （开闭原则）
![](https://images2015.cnblogs.com/blog/833855/201612/833855-20161225143359198-258837981.png)
> 对扩展开放，对修改关闭

``` java
public interface IBook{
    public String getName();
    public String getPrice();
    public String getAuthor();
}
```
``` java 
public class NovelBook implements IBook{
   private String name;
    private int price;  
    private String author;   
    public NovelBook(String name,int price,String author){ 
        this.name = name;     
        this.price = price;     
        this.author = author;
   }   
    public String getAutor(){     
        return this.author;
   }   
        
    public String getName(){
        return this.name;
   }  
 
   public int getPrice(){     
        return this.price;
   } 
}
```
``` java 
public class Client{   
    public static void main(Strings[] args){
     IBook novel = new NovelBook("笑傲江湖",100,"金庸");
     System.out.println(
            "书籍名字："+novel.getName()+
            "书籍作者："+novel.getAuthor()+
            "书籍价格："+novel.getPrice()
            );
   }
}
```

``` java
public class OffNovelBook implements NovelBook{
 
   public OffNovelBook(String name,int price,String author){
      super(name,price,author);
   }
      
    //覆写价格方法，当价格大于40，就打8析，其他价格就打9析
   public int getPrice(){
      if(this.price > 40){ 
          return this.price * 0.8;
     }else{
          return this.price * 0.9;
     }   
   } 
}
```
#### [I] Interface Segregation Principle（接口隔离原则）
![](https://images2015.cnblogs.com/blog/833855/201612/833855-20161225151308167-1584307072.png)
> 一个接口只服务于一个子模块或业务逻辑;
通过业务逻辑压缩接口中的public方法，接口要不断的精简，以达到接口不断完善;
已经被污染的接口，尽量去修改，若变更的风险较大，则采用适配器进行转化处理.
#### [L] Liskov Substitution Principle
![](https://images2015.cnblogs.com/blog/833855/201612/833855-20161225145745339-94188373.png)
> Subtypes must be substitutable for their base types

### [D] Dependency Inversion Principle（依赖反转原则）
![](https://images2015.cnblogs.com/blog/833855/201612/833855-20161225151256807-1927942307.png)
### 2.Relation between DI & IoC & DIP
![](http://www.tutorialsteacher.com/Content/images/ioc/principles-and-patterns.png)

* DIP 是一种思想，它认为上层代码不应该依赖下层的具体实现，而应该提供接口让下层实现。
* IoC 也是一种思想，它认为代码本职之外的其它工作都应该由某个第三方（框架）完成。
* DI 是一种技术，将依赖通过“注入”的方式提供给需要的类，是是 DIP 和 IoC 思想的具体表现。
####
### 3.What is IoC
IoC is a design principle which recommends inversion of different kinds of controls in object oriented design to achieve loose coupling between the application classes. Here, the control means any additional responsibilities a class has other than its main responsibility, such as control over the flow of an application, control over the dependent object creation and binding.

### 4.What is DIP
1. High-level modules should not depend on low-level modules. Both should depend on abstraction.
2. Abstractions should not depend on details. Details should depend on abstractions.

![](https://lotabout.me/2018/Dependency-Inversion-Principle/DIP-dao-normal.png)
![](https://lotabout.me/2018/Dependency-Inversion-Principle/DIP-dao-dip.png)
### 5.What is DI
* constructor injection ，依赖通过构造函数传入
* setter injection，依赖通过一个个 setter 传入
* interface injection，类显示实现一个 setter interface。
### 6.Workflow
![](http://www.tutorialsteacher.com/Content/images/ioc/ioc-steps.png)
### 7.Next Week - Spring implementation - IoC Container
