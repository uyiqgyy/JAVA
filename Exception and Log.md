## 1. Exception
**1. The Exception classifictiions**
* **Throwable** is the supper class for all the errors and Exceptions in Java. 
* **Error** is the subclass of Throwable, it means uncontrollable error, system/application can't handle it and it need manually checking.  
  like : OutOfMemoryError,StackOverflowError ...
*  **Exception** is the subclass of Throwable too. It can be divided into two categories：  
   * Unchecked Exception:
    System/Application no need to try catch it.  
      like : NullpointException,ClassCastException,IndexOutOfBoundsException ...   

   * Checked Exception:
   Exceptions other than RuntimeException. Such exceptions are caught by the compiler at compile time and compile errors if they are not caught.  
   like : FileNotFoundException.
  
**2. Where should we try-catch the exceptions**
   * Better create own Exception
   * Avoid try catch many code paragraphs , only try-catch the place it may throw the Exceptions.
   ``` java
   //Wrong
   public static void main(String[] args) {
      try {
            function1();
            function2();
            function3();
            ...
            functionMayThrowExceptionA();
            ...
      }catch(ExceptionA ex) {
            //handle ExceptionA
      }
   }
   ```
   ``` java
   //Good
   public static void main(String[] args) {
      function1();
      function2();
      function3();
      ...
      try {
            functionMayThrowExceptionA();    
      }catch(ExceptionA ex) {
            //handle ExceptionA
      }
      ...
   }
   ```
   * Distinguish Exception, and take corresponding action
   ``` java
   //Wrong
   public static void main(String[] args) {
      try {
            function1MayThrowBImplementsExceptionA();
            function2MayThrowCImplementsB();
            function3MayThrowExceptionA();
      }catch(Exception ex){
            //Only catch the Exception and handle it.
      }
   }
   ```
   ``` java
   //Good
   public static void main(String[] args) {
      try {
            function1MayThrowBImplementsExceptionA();
            function2MayThrowCImplementsB();
            function3MayThrowExceptionA();
      } catch(ExceptionC exc) {
            //handle ExceptionC
      } catch(ExceptionB exb) {
            //handle ExceptionB
      } catch(ExceptinA exa) {
            //handle ExceptionA
      }
   }
 ```
    
**3. Who should handle the exception**
* Remote call across application - Exceptoin should be encapsulated in Objec like Result with status code. It can avoid NullPointException.
* Inner call - Exception should be throw upper if current function can't handle it.

**4. How to handle an exception**
 * We should not catch exceptions but do nothing or only just print log

   ``` java
   //Wrong
   public static void main(String[] args) {
      int result;
      try {
          result = function1MayThrowExceptionA();
      } catch(ExceptinA exa) {
           //Do nothing Or
           //log.error("ExceptionA Here!");
      }
   }
   ```

   ``` java
   //Good
   public static void main(String[] args) {
      int result;
      try {
           result = function1MayThrowExceptionA();
      } catch(ExceptinA exa) {
           log.error("ExceptionA happens when calling function1 " + exa.getErrorMessage(),exa);
           result = -1;
      }
   }
   ```
 * We should skilled use try-catch-finally
 ```java
 //Example 1
 public static int finallyNotWork() {
       int temp = 10000;
       try {
             throw new ExceptionA();
       } catch(ExceptionA exa) {
             return ++temp;
       } finally {
             temp = 9999
       }
 }
 ```
 ``` 
 10001
 ```
 ``` java 
 //Example 2
 public class TryCatchFinally {
     static int x = 1;
     static int y = 10;
     static int z = 100;

     public static void main(String[] args) {
           int value = finallyReturn();
           System.out.println("value=" + value);
           System.out.println("x=" + x);
           System.out.println("y=" + y);
           System.out.println("z=" + z);
     }

     public static int finallyReturn() {
           try{
                 //...
                 return ++z;
           } catch (Exception e) {
                 return ++y;
           } finally {
                 retrun ++z;
           }
     }
 }
 ```
 ```
 value=101
 x=2
 y=10
 z=101
 ```

   
 
### Log

1. Log specification
 * Naming - appName_logType_logName.log
 * Days of preservation - 15day ， then backup
 * Log Details
  Time + thread + log level + bacis info + exception , e call stack
2. Log Level   
Debug < Info < Warn < Error < Fatal
* DEBUG – The DEBUG Level designates fine-grained informational events that are most useful to debug an application. 
* INFO - The INFO level designates informational messages that highlight the progress of the application at coarse-grained level. 
* WARN – The WARN level designates potentially harmful situations. 
* ERROR – The ERROR level designates error events that might still allow the application to continue running.
* FATAL – The FATAL level designates very severe error events that will presumably lead the application to abort. 

3.tips
1.save resources
* if(logger.isDebugEnabled（））{
logger.debug("id="+id);//String appending
}
* logger.debug("id={}",id);
2.Forbiden to log DEBUG in PROD, only allow little info;
3. should log contex and exception stack.
logger.error("xxx" + e.getMesseage(),e);
4. Object override toString();

5.LOG Framework
* Log Implementation  
  log4j,logback,log-jdk,others
* Log Facade  
  slf4j,commons-logging
* Log Facade Adaptor
slf4j-jdk,slf4j-jcl,slf4j-log4j12
* Log Adaptor
jul-to-slf4j,log4j-over-slf4j,jcl-over-slf4j

![pic1](https://cnblogpic.oss-cn-qingdao.aliyuncs.com/blogpic/java_log/slf4j-concrete-bindings1.png)

![pic2](https://cnblogpic.oss-cn-qingdao.aliyuncs.com/blogpic/java_log/slf4j_brige.png)