### Exception
1. Should know Exception classifictiions
  * Throwalble
        |
  |----------|
 Error     Exception
               |
      |-----------------------|
  RuntimeException           Checked Exception  
  (Unchecked Exception)         like FileNotFoundException, should try catch
        |
  Predicted (NPE,IndexOutOfBoundsException) 
  - should check before call
  
  Caution Exception - should handle by upper framework ,like TimeOut
  
  Ignored Exception - handled by sytem framework no need to handle
  
2. Where - 
   * avoid try catch many paragraphs , only try-catch the place it may through Exceptions
   * Distinguish Exception, and respond accordingly
    
2. Who should respond ,otherwise throw upper
* better to use own exception 
* Remote call across application - Exceptoin should be encapsulated in Objec like Result with status code.
* inner call - Exception should be throw upper.

3. How 
 * should not catch but do nothing
 * should not just print log
 * skilled use try-catch-finally
   
 
### Log

1. Log specification
 * Naming - appName_logType_logName.log
 * Days of preservation - 15day ， then backup
 * Log Details
  Time + thread + log level + bacis info + exception , e call stack
2. Log Level
* DEBUG
* INFO
* WARN
* ERROR
* FATAL

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
