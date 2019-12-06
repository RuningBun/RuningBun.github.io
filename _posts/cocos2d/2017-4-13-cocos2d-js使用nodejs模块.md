---
layout: post
title: "cocos2d-js使用nodejs模块"
categories: cocos2d-js
tags: cocos2d-js nodejs
---


* content
{:toc}

# Node.js
自 Node.js 问世以来，JavaScript 在服务端的地位得到了许多人的认可。javascript 也不再是以往被认为的“前端的玩具语言”。在开发流程与规范上，node.js 提供了一套完整的方案，其中最基本的就是CommonJS 规范。它将 javascript 划分为模块，模块之间通过 require 进行调用——这很像其它语言的 include 或 import。


此外，node.js 还提供了 npm 包管理工具（node package manager）。这使得全球的 node 开发者能够非常方便的共享和使用优秀的模块。可以说 node.js 的成长离不开这些优秀的开源社区。
模块化开发的好处非常多，最重要的就是使得测试驱动变得更加友好，不用再去处理复杂的依赖关系。mocha 是 node.js 环境下的一款非常好用的测试/行为驱动框架，能够结合各种第三方测试框架（只要它能抛出异常），我个人比较喜欢 should.js
于是我尝试使用 mocha + should.js 进行了一段时间的 BDD 开发，感觉非常好。很快就能把模块写好，并且得到一份不错的测试文档。


Cocos2d-js
我想如果能将这种开发方式运用在游戏的 model 层开发，肯定可以提高很多效率。但是现在使用的 Cocos2d-js 似乎有点水土不服。
Cocos2d-js 是 Cocos2d-HTML5 v3.0 版的新名字，它从 Coco2d-x 分支中独立出来，并且对 Cocos2d-x jsb 提供反向支持。为了更适应 javascript 开发者，Cocos2d-js 对原有的代码进行了大面积的修改，去掉了大部分 cpp 的风格代码，使它更接近 javascript 的开发方式。然后通过 javascript binding 的方式对 native 提供支持（原本由 Cocos2d-x jsb 提供对 web 的支持，现在反过来了）
Cocos2d-js 主要面向浏览器开发，也可通过 javascript binding 运行在手机平台。虽然都是使用 javascript 进行开发，但实际上这两个平台还是有很多区别的。


在浏览器上，js 文件是异步加载的；
在 native 开发中 js 是资源文件，是以同步方式加载的；
Cocos2d-js 的变通方式是在网页中追加 
```<script>``` 标签来实现异步加载&同步执行。然后在 native 的 spidermonkey 引擎中增加 require 方法，以同步的方式执行外部文件。不过 Cocos2d-js 提供的 require 并没有像 node.js 那样灵活，从源码中可以看到，只是一个很死板的 executeScript:
/* scriptCore.cpp */

```
    // register some global functions
    JS_DefineFunction(cx, global, "require", ScriptingCore::executeScript, 1, JSPROP_READONLY | JSPROP_PERMANENT);
```
这个 require 没办法像 CommonJS 规定的那样返回模块对象。并且这个 require function 是只读的，无法在 spidermonkey 里面用自己的 require 覆盖掉。
Solution
浏览器和 Cocos2d-x jsb 都没办法直接使用 require 来加载 node.js 的模块，难道到没有别的方法了吗？这时候有一个叫 browserify 的工具出现了，它可以将 node.js 的模块打包成一个 bundle.js 文件，然后直接在浏览器中加载这个 bundle.js 文件，就可以 require 包中的各种模块了。


但是 Cocos2d-x jsb 将 require 限制为只读，只好动个小手术，把 require 更名为 ccrequire 或者其它的了。
```
    // register some global functions
    JS_DefineFunction(cx, global, "ccrequire", ScriptingCore::executeScript, 1, JSPROP_READONLY | JSPROP_PERMANENT);
```
此外，还要同时修改 jsb_boot.js / jsb.js / jsb_debugger.js 里面 require 为 ccrequire 即可。
这样，即不影响 Cocos2d-js 原有的 require 功能（其实只是简单的 excuteScript），又可以 require 由 browserify 打包的 node.js 模块了。而开发的过程中，则可以完全以 node.js 的方式对 model 层进行开发和测试。