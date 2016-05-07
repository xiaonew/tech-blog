> 学习gulp的用法,首先得学习下gulp的几个常用的api


### task

**源码:**

`Gulp`继承`Orchestrator`,`task`方法就是原型方法`add`,`add`原型方法来自`Orchestrator`

``` javascript
    function Gulp() {
        Orchestrator.call(this);
    }
    util.inherits(Gulp, Orchestrator);
    Gulp.prototype.task = Gulp.prototype.add;
```


`Orchestrator`的add方法,前端的代码都不用管,最后面是重要的代码,有一个`tasks`对象,把`name`的task添加了进去

``` javascript
    Orchestrator.prototype.add = function (name, dep, fn) {
        ...
        this.tasks[name] = {
            fn: fn,
            dep: dep,
            name: name
        };
        return this;
    };
```

**api:**

定义

    gulp.task(name[,deps,fn])

注册一个task,name是task的名字,deps是可选项,就是这个task依赖的tasks,fn是task要执行的函数.

示例

``` javascript
    gulp.task('js3', ,['js1', 'js2'], function(){
        console.log('js3');
    });
```

上述task的名字`js3`,依赖`js1`和`js2`task,必须js2和js3先运行,才能运行js3,但是js1和js2是并行运行的.


**task运行过程**

大家知道执行`gulp`命令就可以执行,上面gulpfile中的task,执行的过程如下

1. 运行`gulp`命令,其实就是运行gulp模块的/bin/gulp.js

2. 然后执行gulp的task方法

从下面源码可知,原先运行的run方法,现在都是运行task方法,**并且gulp后面是可接参数的,后面跟执行的task名称,如果没参数,则默认执行名称为`default`的task,**所以参考[gulp学习(一)-介绍安装的示例](gulp学习(一)-介绍安装.md),默认执行的default task.

``` javascript
    Gulp.prototype.run = function() {
      // `run()` is deprecated as of 3.5 and will be removed in 4.0
      // Use task dependencies instead

      // Impose our opinion of "default" tasks onto orchestrator
      var tasks = arguments.length ? arguments : ['default'];

      this.start.apply(this, tasks);
    };
```

一般gulpfile多个task这样写:

``` javascript
    'use strict';

    let gulp = require('gulp');

    gulp.task('task1',function(){
       console.log('hello,world1');
    });

    gulp.task('task2',function(){
       console.log('hello,world2');
    });

    gulp.task('default',['task1','task2'],function(){
       console.log('all task finished');
    });
```


### src pipe dest


**api:**

source 定义

    gulp.src(globs[,options])

与globs匹配的文件,可以是string或者一个数组.用来读取文件的内容,输出一个stream可以被`pipe`插入到别的插件中去。可选项除`base`外基本不用.

pipe 定义

    就是一个管道,用来接受内容的流

dest 定义

    gulp.dest(path[,options])

**就是最终文件输出的路径.`options`一般不用.`src`的`base`选项,会影响`dest`的输出路径**

示例1

``` javascript
    gulp.task('task1',function(){
      gulp.src('bin/gulp/gulp_src.txt')
      .pipe(gulp.dest('build'));
    });
```

![](https://raw.githubusercontent.com/xiaonew/tech-blog/master/img/1657_1.png)

dest参数是输出目的路径，文件名是原先的文件名.不过src的选项base会影响输出路径.

``` javascript
    gulp.task('task1',function(){
      gulp.src('bin/gulp/gulp_src.txt',{'base':'bin'})
      .pipe(gulp.dest('build'));
    });
```

原先的输出路径是build/gulp_src.txt,现在时build/gulp/gulp_src.txt。base是输出内容的保留目录。


### watch

**api:**

watch定义

    gulp.watch(glob[, opts, cb])

一个 glob 字符串，或者一个包含多个 glob 字符串的数组，用来指定具体监控哪些文件的变动。
cb回调函数参数`event`,event.type共有added/changed/deleted三种类型。event.path是内容变动的文件路径

``` javascript
  gulp.watch('bin/gulp/*.txt', function(event) {
      console.log('File ' + event.path + ' was ' + event.type + ', running tasks...');
  });
```

运行`gulp`之后,修改了gulp_src.txt文档的内容,输出`File /Users/lh/WebstormProjects/es6-doc/bin/gulp/gulp_src.txt was changed, running tasks...`

















