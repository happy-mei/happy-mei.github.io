---
layout: post
title: 《深入浅出nodejs》学习总结之模块机制
date: 2020-02-01 14:09:10
tags: [NodeJs]
categories:
 - backend
---

# 1 模块实现

步骤：

<!-- more -->
1. 路径分析

   优先级：核心模块的缓存检查>文件模块的缓存检查>核心模块>文件模块

   文件模块中，相对路径>绝对路径>非路径形式的文件模块（第三方文件模块，即node_modules中的模块）
2. 文件定位

   文件扩展名分析：如果没有文件扩展名，按.js,.json,.node的次序补足

   > 优化点：如果是.node和.json文件，在传递给`require()`时带上扩展名，会加快一点速度

3. 编译执行

   新建一个模块对象，根据路径载入并编译

   - .js文件：通过fs模块同步读取文件后编译执行，编译过程中，Node对js文件内容进行头尾包装：

     ``` js
     (function (exports, reuire, module, __filename, __dirname) {
       // 文件内容
       // 引入依赖
       var math = require('math')
       // 输出
       exports.area = function() {
         // ...
     	}
     })
     ```

   - .node文件：这是用C/C++编写的扩展模块文件，不需要编译，直接通过`process.dlopen()`方法加载最后编译生成的文件然后执行。`dlopen()`方法在windows和 *nix 平台有不同的实现，通过libuv兼容层进行了封装，实际上在windows编译成.dll文件，在 *nix编译成.so文件，但为了看起来更自然一点所以扩展名统一为.node。

   - .json文件：通过fs模块同步读取文件后，用`JSON.parse()`解析返回结果

   - 其余扩展名文件被当作.js文件处理



# 2 模块分类

模块分为两类：

- 一类是Node提供的模块，即__核心模块__
- 另一类是用户编写的模块，即__文件模块__

## 2.1 核心模块

核心模块在Node源代码的编译过程中，编译进了二进制执行文件。在Node进程启动时，部分核心模块就被直接加载进内存中，比普通的文件模块从磁盘中一处一处查找要快很多。所以核心模块引入时，文件定位和编译执行这两步骤可以省略掉，并且在路径分析中优先判断，所以加载速度是最快的

核心模块分成两部分：

__1. 内建模块（C/C++编写的）__

在Node项目的[src](https://github.com/nodejs/node/tree/master/src)目录下，下面称为内建模块。性能上优于脚本语言。

- 编译：预先被编译进二进制文件。`node_extensions.h`文件将这些散列的内建模块统一放进了一个叫`node_module_list`的数组中。
- 加载：使用`get_builtin_module()`方法从`node_module_list`中取出，执行

__2. js核心模块（js编写的）__

在Node项目的[lib](https://github.com/nodejs/node/tree/master/lib)目录下。一类是作为C/C++内建模块的封装层和桥接层；一类是纯粹的功能模块，它不用跟底层打交道，但是又十分重要。

- 编译：Node采用V8附带的js2c.py，将所有js核心模块转换成C++的字符串数组（字符串即js核心模块文件内容），生成`node_natives.h`头文件。如上面所说编译.js文件的过程中经历了头尾包装，与2.2.2说的文件模块的区别在于：获取源代码的方式（核心模块从内存中加载）以及缓存执行结果的位置。

- 加载：通过`process.binding('natives')`取出上述的字符串数组放置在`nativeModule._source`。加载时直接从内存中加载，执行。 *[P25]*

  ``` js
  nativeModule._source = process.binding('natives')
  ```

## 2.2 核心模块的引入流程

在核心模块中，有些模块全部由C/C++编写；有些模块则由C/C++完成核心部分，其他部分由js实现包装或向外导出，以满足性能需求（脚本语言如js的开发速度优于静态语言如C/C++，但其性能弱于静态语言），如`buffer`、`crypto`、`fs`、`os`等。

即存在一种依赖层关系，文件模块可能依赖核心模块（Js），核心模块可能依赖内建模块（C/C++）。下面是引入`os`原生模块的流程。

![P25](https://user-gold-cdn.xitu.io/2020/2/1/16fff891e1381541?w=864&h=862&f=png&s=99119)

## 2.3 文件模块

在运行时动态加载，包括了上述完整的路径分析、文件定位、编译执行这些过程，速度比核心模块慢。

- 编译：如上面所说编译.js文件的过程中经历了头尾包装
- 加载：在运行时动态加载

由开发者编写，包括普通JS模块和C/C++扩展模块。主要调用方向为普通JS模块调用C/C++扩展模块。

__C/C++扩展模块__

C/C++扩展模块属于文件模块的一类。主要提升性能，如js只有double型的数据类型，而进行位运算需要把double型转为int型，所以js层面做位运算效率不高。这时就需要C/C++扩展模块。

- 编译：编译成.node文件

- 加载：通过`process.dlopen()`方法加载最后编译生成的文件然后执行。

## 2.4 模块调用栈
![P33](https://user-gold-cdn.xitu.io/2020/2/1/16fff89c26dfe60d?w=1058&h=562&f=png&s=59670)

文件模块包括：JS模块和C/C++扩展模块；核心模块包括：js核心模块和C/C++内建模块。

文件模块中JS模块可能调用C/C++扩展模块，文件模块同时也可能调用js核心模块，js核心模块可能依赖C/C++内建模块。文件模块同时也可能直接调用C/C++内建模块。

## 2.5 第三方文件模块（npm依赖包）

基本同普通文件模块，除了路径分析上的不同。

# 3 前后端共用模块

- CommonJS规范：`Node.JS`遵循`CommonJS`的规范。 `module.exports = xxx`导出， `require()`引入。同步加载模块。

- AMD：Requirejs对模块定义的规范化产出，异步模块定义，模块的加载不影响它后面语句的运行，依赖前置。`define`定义，`require`引入。

  ``` js
  require(['clock'],function(clock){
    clock.start();
  });
  ```

- CMD：Seajs对模块定义的规范化产出，同步模块定义，依赖就近。`define`定义，`require`引入。

  ``` js
  define(function(require, exports, module) {
     var clock = require('clock');
     clock.start();
  });
  ```

- es6规范：`export`导出，`import`引入

## 3.1 兼容多种模块规范

在目前的项目中，一般的做法是：

babel将es6代码转成es5（CommonJS规范）。webpack做浏览器兼容CommonJS。

参考Nodejs，webpack主要实现`exports`和`require`函数（`__webpack_require__`），并传入`module`、`exports`和`__webpack_require__`参数。详细可看[webpack模块化原理-commonjs](https://segmentfault.com/a/1190000010349749)



# 参考
 - 《深入浅出nodejs》
 - [webpack模块化原理-commonjs](https://segmentfault.com/a/1190000010349749)
