#<center>android线程间的通信   

**一.基本概念**   

*  Looper:每个线程都可以产生一个Looper，用来管理线程的Message，Looper对象会建立一个MessageQueue数据结构来存放message。
*  Handler：与Looper沟通的对象，可以push消息或者runnable对象到MessageQueue，也可以从MessageQueue中得到消息。    
<br>

查看handler的构造函数  
`Handler（）：`  
如不指定Looper，参数则默认利用当前线程 的Looper创建  
` Handler（Looper looper）：`  
使用指定的Looper对象创建Handler 
 
线程A的Handler对象引用可以传递给别的线程，让别的线程B或C等能送消息来给线程A。

线程A的Message Queue里的消息，只有线程A所属的对象可以处理。

**注意：Android里没有global的MessageQueue，不同进程(或APK之间)不能通过MessageQueue交换消息。**

**二.Handler通过Message通信的基本方式**  
Looper.myLooper()可以获取当前对象的Looper对象    
  使用`mHandler = new Handler（Looper.myLooper()）;`可以诞生用来处理卖你线程的Handler对象。  
使用Handler传递消息对象时将消息封装到一个Message对象中，Message对象中主要字段如下
   
*   `public int arg1/arg2`:  
  当需要传递的消息是整形时，arg1和arg2是一种低成本的可选方案，使用的是`setData()/getData()`访问或者修改字段。  
*   `public Object obj`:  
  可传送的任意object类型。  
*  `public int what`:  
  int类型用户自定义的消息类型码  

Message对象可以通过Message类的构造函数获得，但Google推荐使用Message.obtain()方法获得，该方法会从全局的对象池里返回一个可复用的Messgae实例  
`Message()`  
Handler发出消息时，既可以指定消息被接收后马上处理，也可以经过一定时间的间隔后处理。  
`sendMessageDelayed(Message msg,long delayMillis)`  
Handler消息被发送出去之后，将由`handleMessage(Message msg)`方法来处理。   
 
**注意：在Android里，新诞生一个线程，并不会自动建立其Message Loop可以通过调`Looper.prepare()`为该线程建立一个MessageQueue，再调用`Looper.loop()`进行消息循环。**  




