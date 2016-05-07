###参考

- [i5tingbook](http://i5ting.github.io/stuq-gulp/#10701)

- [gulp-book](https://github.com/xiaonew/gulp-book)

>前面两节学习到gulp的基本知识，也知道gulp可以进行压缩,监控，combo等等，这节进行实战。由于gulp有很多插件,所以我们不必重复造轮子,只用用好相关插件就可以

### gulp当做web服务器

某些时候我们会在本地安装Apache或者Nginx当做静态服务器,有了gulp之后就不用了。`gulp-connect`插件可以实现web服务器功能

**安装gulp-connect：**

``` javascript
    npm install --save-dev gulp-connect
```

**示例：**

``` javascript
    'use strict';
    const gulp = require('gulp');
    const connect  = require('gulp-connect');
    gulp.task('server',function(){
        connect.server();
    });
    gulp.task('default',['server']);
```

执行命令`gulp`,然后终端会打印日志如下:

```
    [22:07:20] Using gulpfile ~/WebstormProjects/es6-doc/gulpfile.js
    [22:07:20] Starting 'server'...
    [22:07:20] Finished 'server' after 34 ms
    [22:07:20] Starting 'default'...
    [22:07:20] Finished 'default' after 7.53 μs
    [22:07:20] Server started http://localhost:8080
```

可以看出已经启动了web服务器。可以通过`http://localhost:8080`访问。默认是访问`gulpfile.js`所在目录的`index.html`

**省时的浏览器同步测试工具browsersync**

[摘自官网](http://www.browsersync.cn/)
>rowsersync能让浏览器实时、快速响应您的文件更改（html、js、css、sass、less等）并自动刷新页面。更重要的是 Browsersync可以同时在PC、平板、手机等设备下进项调试。您可以想象一下：“假设您的桌子上有pc、ipad、iphone、android等设备，同时打开了您需要调试的页面，当您使用browsersync后，您的任何一次代码保存，以上的设备都会同时显示您的改动”。无论您是前端还是后端工程师，使用它将提高您30%的工作效率。

### gulp 压缩js css

**安装 gulp-uglify：**

```
    npm install --save-dev gulp-uglify
```


**示例：**

``` javascript
    gulp.watch('js/*.js',['uglify_js']);
    gulp.task('uglify_js',function(){
        gulp.src('js/*.js')
            .pipe(uglify())
            .pipe(gulp.dest('build/js'));
    });
    gulp.task('default',['uglify_js']);
```

上面这段代码可以实现,当js文档下面的js文件内容变化时，会被重新压缩并输出到目的目录


### gulp 编译less

**安装：**

```

```

**示例：**

``` javascript
    const gulp = require('gulp');
    const less = require('gulp-less');
    // 编译less
    // 在命令行输入 gulp less 启动此任务
    gulp.task('less', function () {
        // 1. 找到 less 文件
        gulp.src('less/**.less')
            // 2. 编译为css
            .pipe(less())
            // 3. 另存文件
            .pipe(gulp.dest('dist/css'))
    });

    // 在命令行使用 gulp auto 启动此任务
    gulp.task('auto', function () {
        // 监听文件修改，当文件被修改则执行 less 任务
        gulp.watch('less/**.less', ['less'])
    })

    // 使用 gulp.task('default') 定义默认任务
    // 在命令行使用 gulp 启动 less 任务和 auto 任务
    gulp.task('default', ['less', 'auto'])
```

上面代码可以实现less文件变化,编译输出到css文件夹下


>另外还可以用gulp来编译sass和压缩图片以及combo文件等等。可以自己去查看

[利用gulp构建项目](https://github.com/xiaonew/gulp-book/blob/master/chapter7.md)

