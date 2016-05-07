>Gulp 是基于node实现的,一种自动化构建工具

### 为什么需要前端构建

下面这两篇文章讲解的非常好,谢谢下面这两位作者

[为什么需要前端构建](http://www.jianshu.com/p/f7e3a4a7598a)

[大公司怎么部署前端代码](https://www.zhihu.com/question/20790576/answer/32602154)

[前端工程和性能优化](https://github.com/fouber/blog/blob/master/201405/01.md#%E9%9D%99%E6%80%81%E8%B5%84%E6%BA%90%E7%AE%A1%E7%90%86%E4%B8%8E%E6%A8%A1%E5%9D%97%E5%8C%96%E6%A1%86%E6%9E%B6)


**总结来看是以下几点:**

- 解决JavaScript,Css浏览器缓存问题

- JavaScript和Css的依赖问题

- 文件的合并压缩

- 代码分析测试等等

> 其实就是把以前手动,或者用各种工具做的事情工程化,自动化了。目的当然是为了提供开发效率


### gulp的安装使用

更多内容请查看[gulp入门指南](https://github.com/xiaonew/gulp-book)

由于gulp是基于node开发的,所以gulp就是node的一个模块。也是通过npm去管理的。所以在安装gulp之前,首先要安装好node和npm.

这里我已经默认安装好了node和npm。

**安装:**

``` javascript
    npm install -g gulp
    //也可以用cnpm
    //cnpm install -g gulp
```


**验证:**

``` javascript
    gulp -v
    CLI version 3.9.1
```


**初体验:**

- 在项目的根路径创建gulpfile.js文件

- 引入gulp模块,创建task

- 然后运行 `gulp`

源码:

``` javascript
    'use strict';

    let gulp = require('gulp');

    gulp.task('default',function(){
       console.log('hello,world');
    });
```


输出:

``` javascript
    hello,world
```



