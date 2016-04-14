第一次接触Node.js时，就觉得他只不过是用javascript实现的服务端。但实际上他提供了许多浏览器端不具备的方法，比如EventEmitter类。我们在本文中来学习如何使用EventEmitter。

###EventEmitter是什么？
###
简单来说，使用EventEmitter，你可以监听一个事件，并且可以执行一个你绑定的回调函数。就像前端的javascript一样，你可以通过addEventListener来绑定用户的鼠标键盘交互事件，EventEmitter是基于发布订阅模式，因此我们可以通过订阅事件然后再发布。我们可以看到很多前端javascript库是支持订阅发布模式，但Node.js是内建的。
有一个重要的问题：你为什么要使用事件模式？因为在Node.js里，他可以替代各种深层嵌套的加调。Node.js很多方法是要异步运行的，这意味着当这个方法完成时，你就要传递一个可回调的函数。最后，你会发现你的代码看起来像一个巨大的漏斗。为了防止这种情况出现，你可以使用监听事件来优化这些事件，这可以更好地组织你的代码，而不是使用回调嵌套的方式。
使用事件方式还有一个好处，就是可以使你的代码得到很好的解耦。一个事件发出后，如果没有被监听，那么他也不会报错。
使用 EventEmitter
现在让我们一起开始使用EventEmitter，首先引用events模块：

``` javascript
    var events = require("events");
```

这个events对象有一个属性，就是EventEmitter类本身，现在我们做一个简单的例子来学习他：

``` javascript
    var EventEmitter = require("events").EventEmitter;

    var ee = new EventEmitter();
    ee.on("someEvent", function () {
        console.log("event has occured");
    });
    ee.emit("someEvent");
```

首先我们创建一个新的EventEmitter对象。这个对象有两种主要方法: on 和 emit; on这个方法有两个参数，第一个参数是我们要监听事件的名称，在上面的例子，要监听事件的名称就是"someEvent"。当然你可以定一个更好的名字。第二个参数是在事件发生时，要执行的函数。
现在要触发事件，你可以通过事件名称EventEmitter的实例emit方法。就是上面代码的最后一行。如果你运行代码，将得到打印到控制台的文本。
这是最基本的 EventEmitter 使用，你也可以触发事件时传递一个对象。

``` javascript
    ee.emit("new-user", userObj);
```

这只是一个数据参数，可以包含你想要的数据。可以在事件函数中可以通过参数来接收他:

``` javascript
    ee.on("new-user", function (data) {
        // use data here
    });
```

让我们理解清楚EventEmitter功能的一部分。其实一个事件不止被监听一次，还可一个事件被监听多次，并且当事件被触发时，所有监听者的事件都会被触发。默认情况下，Node.js允许一个事件同时被监听10次。如果再创建Node.js会发出警告。但是，我们可以通过使用setMaxListeners改变这个限制数。
例如，如果你运行下面的代码，你应该会看到警告信息。

``` javascript
    ee.on("someEvent", function () { console.log("event 1"); });
    ee.on("someEvent", function () { console.log("event 2"); });
    ee.on("someEvent", function () { console.log("event 3"); });
    ee.on("someEvent", function () { console.log("event 4"); });
    ee.on("someEvent", function () { console.log("event 5"); });
    ee.on("someEvent", function () { console.log("event 6"); });
    ee.on("someEvent", function () { console.log("event 7"); });
    ee.on("someEvent", function () { console.log("event 8"); });
    ee.on("someEvent", function () { console.log("event 9"); });
    ee.on("someEvent", function () { console.log("event 10"); });
    ee.on("someEvent", function () { console.log("event 11"); });

    ee.emit("someEvent");
```

为了可以设置更大的监听数量，可以运行下面的代码：

``` javascript
    ee.setMaxListeners(20);
```

当你运行他，你将不会再得到警告。
其他 EventEmitter 方法
还有其他非常有用的 EventEmitter方法 once, 像on方法，不同的是它仅适用于一次，被触发一次后，监听者会被删除。

``` javascript
    ee.once("firstConnection", function () {
        console.log("You'll never see this again");
    });

    ee.emit("firstConnection");
    ee.emit("firstConnection");
```

如果你运行上面的代码，你只能看到一次消息。第二次emit是不会触发任何事件，因为once监听者被使用后，就会被删除。
现在提到了去除监听者，其实我们也可以手动做到这点，有几个办法. 第一：我们可以通过removeListener方法去删除单个监听者，他需要两个参数：事件名称和监听器函数。到目前为止，我们一直在使用匿名函数作为我们的听众。如果我们希望以后可以删除监听器，那需要一个非匿名函数。我们可以参考一下removeListener方法。

``` javascript
    function onlyOnce () {
        console.log("You'll never see this again");
        ee.removeListener("firstConnection", onlyOnce);
    }

    ee.on("firstConnection", onlyOnce) 
    ee.emit("firstConnection");
    ee.emit("firstConnection");
```

如果你运行上面的代码，你会看到和 once 方法非常相似的效果。
如果你想删除所有的监听者，你可以使用removeAllListeners，只需要给他传递一个事件的名称。

``` javascript
    ee.removeAllListeners("firstConnection");
```

如果想删除所有监听事件，调用函数不用带任何参数。

``` javascript
    ee.removeAllListeners();
```

还有最后一个方法：listeners.这个方法接受一个事件称作为参数，并返回了监听事件的函数数组。看例子：

``` javascript
    function onlyOnce () {
        console.log(ee.listeners("firstConnection"));
        ee.removeListener("firstConnection", onlyOnce);
        console.log(ee.listeners("firstConnection"));
    }

    ee.on("firstConnection", onlyOnce) 
    ee.emit("firstConnection");
    ee.emit("firstConnection");
```

EventEmitter实例本身触发了两个事件，我们可以同样监听到：一个是我们新创建的监听者时触 发的事件newListener，另一个是我们删除监听者后触发的事件removeListener。看下面代码：

``` javascript
    ee.on("newListener", function (evtName, fn) {
        console.log("New Listener: " + evtName);
    });

    ee.on("removeListener", function (evtName) {
        console.log("Removed Listener: " + evtName);
    });

    function foo () {}

    ee.on("save-user", foo);
    ee.removeListener("save-user", foo);
```

运行上线代码，每当前建监听者和删除监听者，你都会看到我们的预期消息。
现在我们已经看到EventEmitter实例的所有方法，让我们来看看它是如何工作与其他模块一起使用。
EventEmitter内部模块
由于EventEmitter类只是普通的javascript，它非常有意义，它可以在其实模块中使用，在你的javascript模块，你可以创建EventEmitter实例，并用它们来处理内部事件。这很简单，更有趣的是，是创建继承自EventEmitter一个模块，所以我们可以使用公共API实现部分功能。
其实，有内置的Node模块做的正是这一点。例如，HTTP模块，这是用来创建web server的一个模块。下面的示例是展示EventEmitter类的on方法如何成为http.Server类的一部分。

``` javascript
    var http = require("http");
    var server = http.createServer();

    server.on("request", function (req, res) {
        res.end("this is the response");
    });

    server.listen(3000);
```

如果你运行这段代码，进程将等待一个请求，你可以去访问http://localhost:3000，你会得到响应。当服务器实例从浏览器获取请求时，它会发出一个“请求”事件，我们的监听器将接收并在可以充当一个事件。
那么，我们如何去创造一个继承于EventEmitter的类？这其实并不难。我们将创建一个简单的UserList类，它负责处理用户对象。所以，在userlist.js文件中：

``` javascript
    var util = require("util");
    var EventEmitter = require("events").EventEmitter;
```

我们需要的util模块，以帮助我们实现继承。接下来，我们需要一个数据库：而不是使用一个真正的数据库，他只是一个对象：

``` javascript
    var id = 1;
    var database = {
        users: [
            { id: id++, name: "Joe Smith",  occupation: "developer"    },
            { id: id++, name: "Jane Doe",   occupation: "data analyst" },
            { id: id++, name: "John Henry", occupation: "designer"     }
        ]
    };
```
现在，我们实可以创造我们的模块。如果你不熟悉Node.js模块，这简单介绍他们是如何工作的：这个文件里面的任何JavaScript是只可读的，默认情况下。如果我们想让它模块的公共API的一部分，我们把它module.exports的属性或module.exports，指定一个全新的对象或函数。让我们这样做：

``` javascript
    function UserList () {
        EventEmitter.call(this);
    }
```
这是构造函数，但它不是你平常的JavaScript构造函数。我们在EventEmitter的构造函数上使用call方法作用于UserList对象。继承构造函数是不够的，我们还需要继承他的原型。这时需要使用到util模块的继承功能了。

``` javascript
    util.inherits(UserList, EventEmitter);
```

这就可以把EventEmitter.prototype上的方法添加到 UserList.prototype 了，我们的UserList实例拥有了EventEmitter实例的所有方法，但我们想添加更多方法，可以添加一个save方法去增加新用户。

``` javascript
    UserList.prototype.save = function (obj) {
        obj.id = id++;
        database.users.push(obj);
        this.emit("saved-user", obj);  
    };
```

此方法可以保存一个对象到我们的“数据库”：它增加了一个id，并将其加入到用户数组。然后，它发出的“saved-user”事件，并且对象传递数据。如果这是一个真正的数据库，保存它很可能是一个异步的任务，这意味着与保存的记录，我们就必须接受一个回调的工作。
另一种情况是发出一个事件，如果我们想做保存的记录的工作，我们就可以监听其事件。我们接下来处理这些事情。

``` javascript
    UserList.prototype.all = function () {
        return database.users;
    };

    module.exports = UserList;
```

我增加了一个方法：一个简单的返回所有的用户，然后，我们指定的UserList到module.exports。 现在，让我们来尝试使用他，在另一个文件中，test.js添加以下内容:

``` javascript
    var UserList = require("./userlist");
    var users = new UserList();

    users.on("saved-user", function (user) {
        console.log("saved: " + user.name + " (" + user.id + ")");
    });

    users.save({ name: "Jane Doe", occupation: "manager" });
    users.save({ name: "John Jacob", occupation: "developer" });
```
我们引入刚设计好的模块，并创建它的一个实例后，我们监听了”saved-user”事件，然后，我们可以继续保存用户信息，当我们运行后，就可以看到，得到两个信息，打印出我们保存的记录名字和ID：

``` javascript
    saved: Jane Doe (4)
    saved: John Jacob (5)
```
