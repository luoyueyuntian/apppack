#### 为什么要模块化？
js文件定义的变量和方法都是定义在全局环境的，而一个项目可能有多人协同开发，每个人定义一个变量和方法时，都有可能和别人定义的变量和方法重名，而这些又都是运行在全局环境的，这样一个很大的问题就是后定义的会覆盖先前定义的，也就是命名冲突。如果不解决这个问题，将会成为项目规模扩大的瓶颈，毕竟每新增一个特性，都有可能对之前的功能造成影响，那项目就面临失控的风险。

除此之外，模块化还有如下好处：
+ 管理依赖
+ 提高代码的可读性
+ 代码解耦，提高代码的复用性

对于这个问题，最开始是通过命名空间的方式来解决的，比如有一个叫`fn`的方法，他们分别挂在模块a和模块b下面
<pre><code>// modleA
modulea.fn = function () { console.log('a') }

// moduleB
moduleb.fun = function () { console.log('b') }
</code></pre>
虽然都是`fn`，但因为有不同的命名空间，二者仍然是不冲突的。

随着前端的发展，模块化的探索终于有了一些成果。出现了诸如CMD、AMD、CommonJS等规范


#### CommonJS
CommonJS规范规定，每个文件就是一个模块，有自己的作用域。在一个文件里面定义的变量、函数、类，都是私有的，对其他文件不可见。如果想在多个文件分享变量，必须定义为`global`对象的属性。每个模块内部，`module`变量代表当前模块。这个变量是一个对象，它的`exports`属性（即`module.exports`）是对外的接口。加载某个模块，其实是加载该模块的`module.exports`属性。`require`方法用于加载模块。

CommonJS模块的特点如下:
+ 所有代码都运行在模块作用域，不会污染全局作用域。
+ 模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。
+ 模块加载的顺序，按照其在代码中出现的顺序。

Node 应用由模块组成，采用 CommonJS 模块规范。

CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。

#### AMD
全称异步模块加载机制，它采用异步方式加载模块。通过define方法去定义模块，require方法去加载模块。

requireJs是典型的实现AMD的模块加载器。

[requireJs官方文档](http://www.requirejs.cn/)
<pre><code>define(
    module_id /*可选的*/,
    [dependencies] /*可选的*/,
    definition function /*用来实例化模块或者对象的方法*/
);</code></pre>

##### requireJs机制
RequireJS使用head.appendChild()将每一个依赖加载为一个script标签。

RequireJS等待所有的依赖加载完毕，计算出模块定义函数正确调用顺序，然后依次调用它们。

在同步加载的服务端JavaScript环境中，可简单地重定义require.load()来使用RequireJS。build系统就是这么做的。该环境中的require.load实现可在build/jslib/requirePatch.js中找到。

##### data-main 入口点
require.js 在加载的时候会检察data-main 属性:

可以在data-main指向的脚本中设置模板加载 选项，然后加载第一个应用模块。

RequireJS的模块语法允许它尽快地加载多个模块，虽然加载的顺序不定，但依赖的顺序最终是正确的。同时因为无需创建全局变量，甚至可以做到在同一个页面上同时加载同一模块的不同版本。

#### CMD
CMD是sea.js在推广过程中对模块定义的规范化产出，主要用于浏览器端。在 CMD 规范中，一个模块就是一个文件，该规范明确了模块的基本书写格式和基本交互规则。
它主要特点是：对于依赖的模块是延迟执行，依赖可以就近书写，等到需要用这个依赖的时候再引入这个依赖。

[sea.js文档](https://www.zhangxinxu.com/sp/seajs/)
