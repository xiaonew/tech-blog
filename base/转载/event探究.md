### Event Loop探究

在浏览器中,事件作为一个极为重要的机制,给予JavaScript响应用户操作与DOM变化的能力,在Node.js中事件模型则是高并发能力的基础.

在进程启动时,Node 便会创建一个类似于while(true)的循环,每次执行一次循环体的过程被称为tick.每个tick过程就是查看是否有时间待处理。如果有，就取出事件及相关的回调函数。如果存在关联的回调函数，就执行他们，然后进入下一个循环。如果不再有事件处理,就退出流程

```flow
st=>start: 开始
e=>end: 退出

cond1=>condition: 还有事件?
cond2=>condition: 是否有关联回调

op=>operation: 取出一个事件
op1=>operation: 执行回调

st->cond1
op->cond2
cond1(yes)->op
cond1(no)->e

op=>operation: 取出一个事件
op=>operation: 取出一个事件
op1->cond1
cond2(yes)->op1
cond2(no)->cond1

```

![](https://raw.githubusercontent.com/xiaonew/tech-blog/master/img/2_4.png)


### Event Loop

从字面上理解,就是事件循环.

-   **Wiki定义**

> Event Loop 是一种程序结构,用于等待和发送消息和事件.

    简单来说,就是再程序中设置两个线程,一个负责程序本身的运行,称之为"主线程",另一个负责主线程和其他进程的通信，被称为"Event Loop 线程".

-   **首先看下背景现状:**

    运行以后的程序叫做"进程",一般情况下,一个进程一次只能执行一个任务.但是现在的程序都需要在同一时刻运行很多任务如果有很多任务执行,可以采用
    下面的三种办法

    - **排队:** 因为一个进程一次只能执行一个任务,只要等前面的任务执行完了,再执行后面的任务

    - **新创建进程:** 进程非常耗费资源,如果有多少个任务就新建多少个进程,系统负载会非常高,而且不能有效的利用资源

    - **新建线程**: 因为进程太耗费资源,所以如今的程序往往允许一个进程包含多个进程,由线程去完成任务.
    但是由于JavaScript是一种单线程语言,所有任务都在一个线程上完成,所以只能采用第一种方法.


-   **JavaScript为什么是单线程的,难道不能实现为多线程么?**

    历史原因,Javascript从诞生起就是单线程.原因大概就是不想让浏览器变得太复杂.因为多线程需要共享资源/且有可能修改彼此的运行结果,对于一种网络脚本
    来说,太复杂,后来几约定俗成Javascript为一种单线程语言.(Worker API可以实现多线程,但是Javascript本身始终是单线程的.)

    **这里引用[冯东](http://www.zhihu.com/people/44faf17ff5f5a4ccc4cf9bda47de8da2)一句话**

>但凡这种[既是单线程又是异步]的语言有一个共同特点,它们都是event-driven.驱 动 event来自一个异构平台.这些语言的 top-leve不像c那样是main,
  而是一组event-handler.虽然所有event-handler都是在同一个线程内执行,但是它们被调用的时机是由那个驱动平台决定的.而且设计要求每个event-
  handler要尽快结束.未做完的工作可以通知那个异构的驱动平台来完成.所以那个驱动平台可以有多线程.


-   **直观看Event Loop作用**


    - 阻塞模式(blocking IO):
    
    ![](https://raw.githubusercontent.com/xiaonew/tech-blog/master/img/2_1.png)

    -  多线程:
    
    ![](https://raw.githubusercontent.com/xiaonew/tech-blog/master/img/2_2.png)

    - 非阻塞(NIO)
    
    ![](https://raw.githubusercontent.com/xiaonew/tech-blog/master/img/2_3.png)
    
    上图主线程的绿色部分,还是表示运行时间，而橙色部分表示空闲时间。每当遇到I/O的时候,主线程就让Event Loop 线程去通知相应的I/O程序,然后接着往后运行，所以不存在红色的等待时间。等到I/O程序完成操作,Event Loop线程再把结果返回主线程。主线程就调用事先设定的回调函数，完成整个任务。







### 参考

[初窥Javascript事件机制](https://segmentfault.com/a/1190000002914296)
[阮一峰eventloop](http://www.ruanyifeng.com/blog/2013/10/event_loop.html)
[understand event loop](https://nodesource.com/blog/understanding-the-nodejs-event-loop/)
[](http://code.danyork.com/2011/01/25/node-js-doctors-offices-and-fast-food-restaurants-understanding-event-driven-programming/)
[](http://stackoverflow.com/questions/10680601/nodejs-event-loop)
http://blog.mixu.net/2011/02/01/understanding-the-node-js-event-loop/