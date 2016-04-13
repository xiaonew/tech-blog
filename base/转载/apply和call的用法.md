## call apply bind 的使用

参考:http://web.jobbole.com/83642/

### 概念
    在javascript中,call和apply都是为了改变某个函数运行时的上下文(context)而存在的，换句话说,就是为了改变函数体内部this的指向。

[javascript 中this用法](Javascript的this用法.md)

Javascript的一大特点是，函数存在[定义时上下文]和[运行时上下文]以及[上下文是可以改变的]这样的概念。


### apply 和 call

- **示例1: apply**

``` javascript
function animal() {}

animal.prototype = {
    food: "鱼",
    eat: function () {
        console.log("吃"+this.food);
    }
}

var cat = new animal();
cat.eat();

var dog = {
    food:"骨头"
}
cat.eat.apply(dog);
```

可以看出apply改变了this的指向。


- **apply和call的用法以及区别**

    apply和call的作用相同,都是改变运行时的上下文。把传入的第一个参数当做方法调用的对象,如果不传入参数,则默认是global对象。

    用法：
``` javascript
    func.call(this,arg1,arg2)
    func.apply(this,[arg1,arg2])
```

从上面的用法也可以看出区别，就是后面参数的传入区别,call是固定的参数传入,apply是数组形式传入参数


### bind

- **概念**

    bind() 方法与 apply 和 call 很相似，也是可以改变函数体内 this 的指向。

**MDN的解释是：bind()**

方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 bind()方法的第一个参数作为 this，传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。

- **示例2**

``` javascript
var foo = {
     x : 4
}
var bar = function(){
    console.log(this.x);
}

bar(); // undefined
var func = bar.bind(foo);
func(); // 4
```

>多次 bind() 是无效的。更深层次的原因， bind() 的实现，相当于使用函数在内部包了一个 call / apply ，第二次 bind() 相当于再包住第一次 bind() ,故第二次以后的 bind 是无法生效的。


### apply call 和bind的区别

- **示例3**

``` javascript
var obj = {
    x: 81,
};

var foo = {
    getX: function() {
        return this.x;
    }
}

console.log(foo.getX.bind(obj)());  //81
console.log(foo.getX.call(obj));    //81
console.log(foo.getX.apply(obj)); //81
```

三个输出的都是81，但是注意看使用 bind() 方法的，他后面多了对括号。

**bind 是返回对应函数，便于稍后调用；apply 、call 则是立即调用**
