# JS中各种规范学习

--------------------------------------------------------------------------------

## 1. CommonJS是一种规范，NodeJS是这种规范的一种实现。
> 根据CommonJS规范，一个单独的文件就是一个模块。加载模块使用require方法，该方法读取一个文件并执行，最后返回文件内部的exports对象。

- 定义一个模块文件： module.js

```javascript
//可以拥有自己的私有变量
var PI = 3.1415926;

function tools() {
    this.calc = function () {
      //do somthing
    }
}
//exports对象上的方法和变量是公有的
exports.tools = new tools();

//或者更直接的方式导出方法
exports.action = function () {
  //do somthing
}
```

- 使用模块文件

```javascript
//require方法默认读取js文件，所以可以省略js后缀
var tools = require('./module').tools;
//引入模块化就可以使用导出的方法了
tools.calc();

//直接使用方法
var module = require('./module');
module.action();
```

--------------------------------------------------------------------------------

## 2. AMD(Asynchromous Module Definition)
> AMD 是 RequireJS 在推广过程中对模块定义的规范化产出<br>AMD异步加载模块。它的模块支持对象 函数 构造器 字符串 JSON等各种类型的模块。<br>适用AMD规范适用define方法定义模块。

- 通过数组引入依赖 ，回调函数通过形参传入依赖

```javascript
define(['module1', 'module2'], function (module1, module2) {
    function foo () {
        //do someing
        module1.some();
        module2.some();
    }
    return {foo: foo}
});
```

- AMD规范允许输出模块兼容CommonJS规范

```javascript
define(function (require, exports, module) {
    var someModule = require("./someModule");
    someModule.some();
    exports.action = function () {
        //do someing
    }
});
```

--------------------------------------------------------------------------------

## 3. CMD
> CMD是SeaJS 在推广过程中对模块定义的规范化产出; 对于依赖的模块AMD是提前执行，CMD是延迟执行。不过RequireJS从2.0开始，也改成可以延迟执行（根据写法不同，处理方式不通过） CMD推崇依赖就近，AMD推崇依赖前置。

```javascript
//AMD
define(['./a','./b'], function (a, b) {
    //依赖一开始就写好
    a.test();
    b.test();
});

//CMD
define(function (requie, exports, module) {
    //依赖可以就近书写
    var a = require('./a');
    a.test();
    //软依赖
    if (status) {

        var b = requie('./b');
        b.test();
    }
});
//AMD也支持CMD写法
```

--------------------------------------------------------------------------------

## 4.UMD
> umd是AMD和CommonJS的糅合

--------------------------------------------------------------------------------

# JS对比决定采取方式

服务器端JS      | 浏览器端JS
----------- | ---------------------
相同的代码需要多次执行 | 代码需要从一个服务器端分发到多个客户端执行
CPU和内存资源是瓶颈 | 带宽是瓶颈
加载时从磁盘中加载   | 加载时需要通过网络加载

所以服务器端多采用CommonJs规范，浏览器端一般采用AMD规范。
