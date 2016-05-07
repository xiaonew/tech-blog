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

