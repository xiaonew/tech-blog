## Node.js 医生办公室和快餐店 来理解事件驱动编程

    你正在试图理解 event-driven 编程么? 你是否被萦绕在大脑里的阻塞IO和非阻塞IO所困扰呢?又或者说你是否正在试图理解什么使Nodejs如此不同?为什么每个人都在讨论它?

    下面通过两个现实生活中的类比来更好的理解Nodejs的事件驱动

### 医生办公接待流程

**背景: **

    医生在接待病人的时候，首先需要填写一些表格，如一些保障信息隐私信息等。

**传统基于线程模式: **

    如果基于传统的线程工作模式，当接待员接待你,你需要站在柜台前填写你的表格直到你完成所有表格。如果你不得不填写3张表格，你将站在柜台前填写完3张表格，与此同时接待员在等你填写好所有表格，其实这样你正在阻塞接待员接待下一个客户。
    
    如果想增强服务能力，基于这种传统线程工作模式的系统，只能增加接待员。但是这样，将不得不支付更多的人力成本以及提供额外的窗口进行服务。

**事件驱动模式: **

   如果是基于Event-driven 模式工作，当你到达窗口，发现必须完成一些额外的表格，接待员给你一些表格、剪切板以及笔，并且告诉你当你填写好表格后再回来。这样你就不会阻塞接待人员为下一个人服务。
   当你完成你的表格，你回来重新排队，等待接待员的服务。这时你可能又有一个新的表格需要填写，那么将重复上面的步骤。这种工作模式在现在的医院中到处可见，并且是一个高扩展的工作模式，如果等待的队伍过长，可以增加接待员。这里的增加速率并不会像线程工作模式那么快。

### 快餐店流程

    和在快餐店或者地摊订餐流程也比较相似。

**传统基于线程工作模式: **

    你给你的订单给收银员，然后在那等待，知道你的订单被完成，收银员将不能为更多的消费者进行服务，如果需要为更多的消费者进行服务，将不得不增加更多的收银员。

**事件驱动工作模式: **

    但是实际上快餐店的工作模式并不是如此。实际上是，你把你的订单给收银员，然后你的订单会被送给其他人去完成，同时收银员进行收款，当你完成支付，你将站在一边，因为收银员将为下一个消费者服务。这里的关键是你不回阻塞收银员接待更多的消费者。在一些快餐店，将会给你一个号码，当你的订单完成时，会呼叫的名字或者编号叫你到前台去取，这在编程领域就是所谓的回调。

### Node.js 做了什么工作

    传统的web服务器，总是基于线程工作模式。你启动Apache或者其他web服务器，它开始接受请求连接，当它接受到一个连接，并且保持连接直到请求完成。如果这个连接请求花费数毫秒去读写硬盘或者数据库，那么在这段时间内，web服务器将被阻塞不能处理新的请求。为了扩展服务的处理能力，你将启动额外的服务进行服务。

    













