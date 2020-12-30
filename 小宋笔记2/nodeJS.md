

## Node.js是什么

- Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。node.js不是一种独立语言，是一个可以让js在服务器端运行的平台。(不用借助HTML在浏览器运行了)
- 简单来讲，就是一个使用JS写服务器的代码环境。
- NodeJS的特性其实就是JS的特性
  - 异步非阻塞的I/O
  - 事件驱动



## 学习目的

1. 了解服务端开发，知道后台是干嘛的
2. 工作中碰到了服务端（后台）的问题知道是他的问题，找他解决
3. 面试中加分
4. 写接口，web开发，数据库的增删改查，后端开发的一些必备知识



### 同步和异步

- 判断标准：调用者是否主动等待被调用者的返回结果
- 同步
  - 理论说明：任务A的执行过程中调用了任务B。任务A对任务B发起调用后，主动等待调用结果。
- 异步
  - 理论说明：任务A的执行过程中调用了任务B。任务A对任务B发起调用后，继续执行后续工作。任务B完成后通过状态、通知来通知调用者。

同步异步取决于任务B是否主动通知A

### 阻塞和非阻塞

- 判断标准：调用方在等待被调用方的返回结果时，是否可以做其他事（是否被挂起）

- 阻塞

  - 理论说明：任务A对任务B发起调用后，任务B需要执行一段时间才可返回结果，任务A选择等待任务B的返回结果（暂时挂起）。

  - 一直等待调用结果，不去干别的事

    

- 非阻塞

  - 理论说明：任务A对任务B发起调用后，与此同时，任务A在任务B执行的过程中去完成别的工作，等待任务B结果返回后再继续（不挂起，而是继续执行自己的任务）。

  - 等待时间去干别的事

    

## Node.js有什么特点

###  优点

- 异步的非阻塞的I/O（I/O线程池）io读写
- 事件循环机制
- 单线程
- 跨平台

### 缺点

- 回调函数嵌套太多、太深（俗称回调地狱）(promise可以解决)
- 单线程，处理不好CPU 密集型任务，最好处理数据密集型



## Node.js的应用场景

主要应用场景：中间层BFF(backends for frontends)、RESTful API

I/O 密集型的领域：如 服务端渲染，前端项目构建。

低延迟的网络应用：如 即时聊天。





## Global

- Browser（全局对象是window）中js的组成：
  - DOM --> document 用来操作文档
  - BOM --> window 用来操作浏览器（控制浏览记录、读取浏览器的信息...）
  - ES --> 语法规范
- Nodejs（全局对象是Global）中js的组成：
  - DOM(完全没有)
  - BOM(干掉了绝大部分，只保留少部分)
    - console.log
    - setInterval
    - setTimeout



- ES(基本实现)支持

- **setImmediate**
  - 该方法用来把一些需要长时间运行的操作放在一个回调函数里,在浏览器完成后面的其他语句后,就立刻执行这个回调函数
  - clearImmediate 方法可以用来取消通过setImmediate设置的将要执行的语句
  - 要在setImmediate执行之前清掉setImmediate 否则 setImmediate运行后就没有清的必要了

```js
  //setTimeout、setInterval、setImmediate的执行方式
  //4-3-1-2
  //先执行同步代码4 然后执行立即执行函数3 然后setTimeout和setInterval按照顺序执行
  setTimeout(() => {
     console.log("1");
  }, 1000);

  setInterval(() => {
    console.log("2");
  }, 1000);

  setImmediate(() => {
    console.log("3");
  });

  console.log("4");
```

- process.nextTick()

  立即执行函数

  如果你希望异步任务尽可能快地执行，那就使用 process.nextTick



- **setTimeout和setInterval 是和window中的计时器一样**
- **setImmediate 相当于延迟0秒的计时器， 立即执行函数， 但是不一定是最先执行的**
- **process.nextTick() 立即执行函数 当开始执行异步代码的时候就直接执行了**



## NodeJS的事件轮询机制

- JavaScript是单线程的， 那么nodejs是如何做到非阻塞呢，在nodejs内部使用了第三方库libuv，nodejs会把IO，文件读取等异步操作交由他处理，而nodejs主线程可以继续去处理其他的事情。

- libuv会开启不同的线程去处理这些延时操作，处理完后，会把异步操作的回调函数放到nodejs的轮询队列中，nodejs会在适当的时候处理轮询队列中的回调函数，从而实现非阻塞。

   所以，实际上nodejs在处理这些阻塞操作时，并不是单线程的。



![image-20200817181343565](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200817181343565.png)



**1.timers：主要用来处理setTimeout和setInterval的回调函数**

**2.pending callback：主要是处理系统的一些回调**

**3.idle prepare：处理nodejs内部的操作**

**4.poll ：处理一些I/o操作**

​    **\- 1.当poll中有回调队列成员，则回调队列执行完毕后或到达最大回调数，进入到下个阶段**

​    **\- 2.当poll是空的时候，那么此时需要两个条件才能执行到下个阶段**

​      **\- 1.timers队列中的计时器超时了**

​      **\- 2.下个阶段check阶段 设置了setImmediate的时候，就直接进入到下个阶段**

**5.check：专门处理setImmediate的回调函数**

**6.close阶段：专门执行关闭的回调函数**



## 宏任务、微任务

- 异步代码有优先级关系。有的优先级高先执行，有的优先级低后执行。
- 微任务优先级高: process.nextTick 、 Promise.then/catch/fanally 、queueMicrotask
  - 优先级最高的是process.nextTick
  - 其他微任务，按代码顺序依次执行
- 宏任务优先级低：setTimeout、setImmediate，顺序看nodejs的事件轮询机制   
- js引擎执行异步代码。会优先执行微任务，再执行宏任务
- 执行微任务时，添加的微任务放入下一个微任务队列

**优先级高的即使在后面书写，代码也是先执行**

**setTimeout、setImmediate虽然setTimeout优先级高，但是如果延时久的话，那么会先执行setImmediate**

 **微任务优先级高于宏任务，并且事件轮询每次到一个新的阶段之前，都会检查微任务队列**

 **微任务中如果新添加了微任务，则把新添加的微任务放到下一个微任务队列执行**

```js
/* //2 5 1 4 3 6
process.nextTick(() => { //微任务
    console.log(111);
});
const promise = new Promise(resolve => {
    console.log(222); //同步
    resolve();
});

setTimeout(() => { //宏任务
    console.log(333);
}, 0);

promise.then(() => { //微任务
    console.log(444);
});

setImmediate(() => { //宏任务
    console.log(666);
});
console.log(555); //同步 */
```

```js
//1 4 8 5 2 6 3 9 
process.nextTick(() => {
    console.log(111); //微任务
});

setTimeout(() => {
    console.log(222); //timers宏任务
}, 0);
setImmediate(() => {
    console.log(333); //check宏任务
});

const promise = Promise.resolve();

promise
    .then(() => {
        console.log(444); //微任务
        process.nextTick(() => {
            console.log(555); //下一个微任务（微任务中添加的微任务）
        });
        setTimeout(() => {
            console.log(666); //timers宏任务
        }, 0);
    })
    .catch(() => {
        console.log(777);
    })
    .then(() => {
        console.log(888); //微任务
        setImmediate(() => {
            console.log(999); //check阶段 宏任务
        });
    })
    .catch(() => {
        console.log(101010);
    })
```



### 微任务queueMicrotask

虽然process.nextTick是微任务，但是优先级太高，一般不书写，当我们想要定义一个微任务的时候，可以使用queueMicrotask

语义化的微任务定义方式：

```js
queueMicrotask(() => {
    console.log(11);
})
```





# 模块化

### 模块化是什么

- 模块化指的就是将一个大的功能拆分为一个一个小的模块，通过不同的模块的组合来实现一个大功能。
- **Node中使用的是CommonJS规范来实现的模块化，前端使用的模块化规范是AMD、CMD和ES6四种**



### CommonJS

- CommonJS 是一套规范，它包含：模块、二进制、Buffer、字符集编码、I/O流、进程环境、文件系统、套接字、单元测试、Web服务器网关接口、包管理等。
- Node借鉴了commonjs的规范实现了一套模块系统，我们也叫做commonJS模块化系统。
- CommonJS对模块的定义主要分为模块引用、模块定义和模块标识3个部分



### 模块定义(module.exports)

- 在CommonJS模块规范中，一个JS文件就是一个模块，并通过`module.exports`和`exports`两种方式来导出模块中的变量或函数
  - 默认情况下模块内部变量和函数对于外部来说都是不可见的，可以通过两种方式向外部暴露变量和函数



- **只有module.exports的指向的才是向外暴露的，module.exports默认是个空对象**
- **exports本身是不具备暴露的功能，exports是指向的module.exports默认的对象地址的一个新对象，所以才具备暴露功能**
- **所以不能修改exports的指向，千万不要直接使用exports = xxx来暴露，这样就不会指向module.exports 就无法暴露**
- **如果修改了module.exports的对象指向，则exports不再具备暴露功能 例如：module.exports=xxx，module.exports={add:add}**

```js
function add(...rest) {
    return rest.reduce((p, c) => p + c, 0)
}

//暴露一个
module.exports.add = add; // 向module.exports空对象里加了add这个方法 ，接受的就是{add:add}
module.exports = {        //直接将module.exports空对象更改成暴露的，接受的也是{add:add}
    add: add
}; 
module.exports=add  //如果直接暴露，module.exports = add,暴露的对象就是add函数 [function:add]

exports.add = add  //也是加了一个add这个方法,接受的还是{add:add}
exports=add//错误

//暴露一组
module.exports = {       //得到{add:add,mins:mins}
    add: add ,
  	mins:mins
}; 
//或者
module.exports = {    
    add, //对象简写
  	mins
}; 
```



### 模块引用require()

#### 如何引用

- Node模块类型分为两种：`核心模块`和`文件模块`，并通过`require`方法来引入模块。前者是Node中内置的模块，而后者一般是用户自己定义的模块。
- CommonJS 模块输出的是一个值的拷贝，ES6 模块输出的是值的引用。
- 通过require()函数来引入外部的模块
- 模块化都有一个入口文件
  1. 模块解析从入口文件开始解析
  2. 负责引入其他模块

**require()中的文件，如果没有后缀名，会以.js .json .node次序补充扩展名，依次尝试。有时可以省略后缀**

**require()函数返回的是暴露的对象**

```js
//如果说暴露是通过module.exports.add = add; //接受的就是{add:add}
//那如果想要使用add 则通过解构赋值 或 add.add()

//如果说暴露是通过module.exports = { add: add };
const {add} = require('./01.add.js');
console.log(add(1, 2, 3, 4, 5)) 


//如果暴露方法是module.exports = add;
const add = require("./01.add.js");
console.log(add(1, 2, 3, 4, 5)) 


//如果使用的是exports.mins = mins; 返回的是一个exports对象，mins只是对象的一个方法
const {
    mins
} = require("./02.mins.js");
console.log(mins(5,3))
```



### 模块标识

- 模块标识就是传递给`require()`方法的参数，采用小驼峰命名，或者以`.`、`..`开头的相对路径或绝对路径。它可以不加文件后缀.js。对于下载的模块或系统模块模块的标识就是文件的名字。
- 模块定义的意义
  - 模块内所有的变量或方法都运行在模块作用域内，不会污染全局作用域
  - 模块可以多次加载，但每次加载只会运行一次，并将运行结果缓存，以待下次使用，如果想要模块再次运行，则需要清除缓存
  - 模块加载顺序和代码运行顺序一致

**模块标识：（require函数中的引入参数写法）**

  1.自定义模块  

​    可以不书写后缀名：默认按照 js  json  node 顺序给添加后缀

​    一定要加上./ 或者 ../之类的路径 否则可能解析错误

  2.nodejs自有模块

​    直接写模块名称

  3.第三方模块i

​    首先先通过`npm i `下载相应的模块

​    直接require相应的包名就可以了

```js
//自定义模块引入
const add = require("./01.add");


//内建模块（node自有模块）
// os 查看cpu占有率
const os = require("os");
const mem = os.freemem() / os.totalmem() * 100;
console.log(`目前cpu还剩余${mem.toFixed(2)}%`)


// 第三方模块 常见 jquery  bootstrap
const $ = require("jquery");
console.log($);
```





## module对象

### module对象的值

```js
 Module {
        id: '.',
        //当前文件的目录（文件夹的路径地址） 绝对路径
        path: 'C:\\Users\\lipeihua\\Desktop\\class0610\\day02\\05.模块化',
        //暴露的对象
        exports: { add: [Function: add] },
        //当前的模块是否是引入其他模块
        parent: null,
        //文件的路径名 绝对路径
        filename: 'C:\\Users\\lipeihua\\Desktop\\class0610\\day02\\05.模块化\\05.module对象.js',
        loaded: false,
        children: [],
        paths: [
        'C:\\Users\\lipeihua\\Desktop\\class0610\\day02\\05.模块化\\node_modules',
        'C:\\Users\\lipeihua\\Desktop\\class0610\\day02\\node_modules',
        'C:\\Users\\lipeihua\\Desktop\\class0610\\node_modules',
        'C:\\Users\\lipeihua\\Desktop\\node_modules',
        'C:\\Users\\lipeihua\\node_modules',
        'C:\\Users\\node_modules',
        'C:\\node_modules'
        ]
    } 
```



### 4.2 module.exports

- module.exports 对象是由模块系统创建的，表示当前文件对外输出的接口。
- `module.exports`就是模块暴露的本质

### 4.3 exports

- 为了方便，Node为每个模块提供一个exports变量，指向module.exports，例如： `module.exports.fun = …`，相当于`exports.fun = ...`
- 但注意，不能将一个值赋值给`exports`，而是使用`exports.XXX`来暴露。否则这样它将不再绑定到`module.exports`。如果exports导出的变量类型是引用类型如函数，则会断开与`module.exports`的地址指向，导致变量导出失败。因为最终还是要靠`module.exports`来导出变量的

```js
  module.exports.hello = true; // 从对模块的引用中导出
  exports.hello = true;//从对模块的引用中导出
  exports = { hello: false };  // 不导出，只在模块内有效
```

### 4.4 module.exports和exports的使用

- 如果模块内部只有一个功能需要暴露，通常使用module.exports = XXX
- 如果模块内部有多个功能需要暴露：这两种都可以书写



## 5 node中的函数

- 在nodeJS中，每一个JS模块外层都包裹了一层函数。
- 在JS中，通过`arguments.callee.toString()`可以看到一个函数：
- `function (exports, require, module, __filename, __dirname) {}`
- 这个函数是所有模块都有的，node编译时往其中注入5个参数：
  - exports 暴露模块
  - require 引入模块
  - module exports属性暴露模块
  - __filename 文件的绝对路径
  - __dirname 文件夹的绝对路径

```js
//打印arguments所在的函数 并转成字符串显示
console.log(arguments.callee.toString())

//每一个node模块外层都有一个函数，函数有5个参数，当node启动的时候，会自动调用这个函数
function (exports, require, module, __filename, __dirname) {
    console.log(arguments.callee.toString())
}
exports:exports对象，指向的是module.exports
require：引入模块的方法
module：module对象
__filename：文件的绝对路径
__dirname：文件夹的绝对路径


console.log(__filename); //直接就可以使用
```





# 包和包管理器

## 1 package包

- Node.js的包基本遵循CommonJS规范，包将一组相关的模块组合在一起，形成一组完整的工具，主要文件就是package.json。
- package.json 文件其实就是对项目或者模块包的描述，里面包含许多元信息。比如项目名称，项目版本，项目执行入口文件，项目贡献者等等。npm install 命令会根据这个文件下载所有依赖模块。
- package.json 文件创建有两种方式，手动创建或者自动创建。
  - 手动创建 直接在项目根目录新建一个 package.json 文件，然后输入相关的内容。
  - 自动创建 也是在项目根目录下执行 npm init，然后根据提示一步步输入相应的内容完成后即可自动创建。

### 1.1 package.json

package.json也叫包描述文件，用于表达非代码相关的信息，它是一个JSON格式的文件。

包描述文件包含以下字段：

> **name** - 包名不能有中文、特殊字符等 作用；下载包时传入的名称
>
> **version** - 包的版本号。
>
> **description** - 包的描述。
>
> **homepage** - 包的官网URL。
>
> **author** - 包的作者
>
> **contributors** - 包的其他贡献者
>
> description：项目描述，是一个字符串
>
> keywords：项目关键字，是一个字符串数组
>
> private：是否私有，设置为 true 时，npm 拒绝发布
>
> license：软件授权条款，让用户知道他们的使用权利和限制
>
> bugs：bug 提交地址
>
> repository：项目仓库地址
>
> homepage：项目包的官网 URL
>
> dependencies：生产环境下，项目运行所需依赖，运行包时需要使用的依赖
>
> > 显示的版本号：
> >
> > 1.12.4 --> 必须是 1.12.4 版本
> >
> > ^1.12.4 --> 必须是 1.12.x 版本，x取最新的
> >
> > ~1.12.4 --> 必须是 1.x.x 版本，x取最新的
>
> devDependencies：开发环境下，项目所需依赖，构建包时需要使用依赖(辅助工具之类的)
>
> scripts：执行 npm 脚本命令简写，比如 “start”: “react-scripts start”, 执行 npm start 就是运行 “react-scripts start”
>
> bin：内部命令对应的可执行文件的路径。
>
> eslintConfig：EsLint 检查文件配置，自动读取验证
>
> engines：项目运行的平台
>
> browserslist：供浏览器使用的版本列表
>
> files：被项目包含的文件名数组

## 2 NPM

全称：Node Package Manager , Node的包管理器

### 2.1 NPM能干什么

- 通过NPM可以对Node的包进行搜索、下载、安装、删除、上传
- NPM的常用指令：
  - npm -v ：查看npm的版本
  - npm init：初始化项目的package.json文件
  - npm init -y 初始化一个默认配置package.json，
  - `npm  i 包名`
    - 安装指定的包（下载之前先要初始化一个package.json）
    - 包会下载到node_modules文件夹中
    - package-lock.json 下载包的缓存文件
    - 下载包会自动添加到package.json中的开发依赖z
- `npm i xxx --save`或`npm i xxx -S` 或`npm i xxx` 安装指定包并添加到项目的生产依赖中（生产环境）
- `npm  i xxx --save-dev` 或 `npm  i xxx -D`:下载包并添加到package.json中的开发依赖里,安装指定包并添加到项目的开发依赖中（开发环境）
- `npm i` :安装项目中的所有依赖
- `npm i xxx@1.12.1`:下载指定版本的包,下载1.12.x版本的包，x代表最新版本
- `npm install / i 包名 -g`:
  - 全局安装（全局安装都是安装一些工具）
  - 作用：将来作为cmd/终端指令使用，不是通过模块化语法引入使用
  - 比如：npm i webpack -g --> 将来就可以在 cmd/终端 使用webpack指令
- `npm remove / r 包名` :删除指定的包



## npm使用流程

新建文件夹  初始化   安装   

### 初始化 npm init

初始化目的:创建package.json文件

1. 新建一个文件夹（不要有中文）
2. **npm init -y** 建议 
   1. 自动生成package.json 所有的都是默认值
   2. 初学阶段用 这个命令即可
3. **npm init** （自行输入）
   1. 手动的设置每一个的值
4. package.json关键字
   1. name:项目名
   2. version:版本号
   3. description:项目描述
   4. author:作者

### 安装 npm i 模块名

1. 找到模块 npm
2. 根据文档   安装 `npm i 模块名`
   1. 首次会创建 `package-lock.json` 
   2. 首次会创建创建`node_modules`文件夹
   3. **`package.json`中增加一个模块的名字(看你安装的是生产环境的包还是开发环境的包)**
      1. 保存模块的名字及版本号（自动的）
   4. `package-lick.json`增加一个模块的信息
      1. 保存了模块的信息，及下载地址（自动的）
   5. `node_modules`中增加一个模块对应的文件夹
      1. 下载了第三方模块保存到这个文件夹下面（自动）
3. 配置文件保存模块的目的
   1. 共享项目时，不需要拷贝`node_modules`文件很多，npm直接可以下载
   2. 运行没有`node_modules`的项目 首先
      1. 终端进入项目
      2. **npm i**
         1. 读取`package.json`及`package-lock.json`中的模块信息，自动全部下载

### npm i

读取`package.json`及`package-lock.json`中的模块信息，自动全部下载

### 适用情景

npm i 模块名:下载指定的模块

​	下载某个时，使用

​	开发阶段

npm i ：读取配置文件，并全部下载

​	运行别人node项目时

​	自己的项目写完了，共享出去了，

记忆：有配置文件，没有`node_modules`，基本上就是用`npm i `

### 文件作用

1. package.json ：项目基本配置
2. package-lock.json:模块的下载地址，方便第二次下载提速（不是必须的）
   1. 早期 ，没有这个文件的，node 4.x+之后 出现的
3. node_modules/: 第三方模块的保存位置 



## cnpm

### cnpm是什么

它是淘宝对国外npm服务器的一个完整镜像版本，也就是**淘宝** **NPM** **镜像**

### cnpm的安装

- 直接安装:`npm install -g cnpm --registry=https://registry.npm.taobao.org`
- 修改npm仓库地址：`npm config set registry https://registry.npm.taobao.org/`

### cnpm的使用

cnpm和npm的使用基本没有区别，只需要将npm替换成cnpm

## Yarn

###  yarn是什么

yarn是Facebook开源的新的包管理器，可以用来代替npm。

### yarn的特点

有缓存。

没有自己的仓库地址，使用的是npm仓库地址。

### yarn的安装

npm install yarn -g

### 常用命令

- yarn --version
- yarn init //生成package.json ！！！注意生成的包名不能有中文，大写
- yarn add global package (全局安装)
- yarn add package (局部安装)
- yarn add package --dev (相当于npm中的--save-dev)
- yarn remove package
- yarn 下载所有包

### Cyarn

- yarn引用npm的仓库，因为‘墙’的存在，可能会导致下载不了或速度很慢的情况，所以需要引入cyarn（淘宝镜像）
- 直接安装cyarn：`npm install cyarn -g --registry https://registry.npm.taobao.org` 配置后，只需将yarn改为cyarn使用即可
- 修改npm仓库地址：`yarn config set registry https://registry.npm.taobao.org/`



# Node.js核心模块

## Buffer 缓冲器

###  Buffer是什么

Buffer是一个和数组类似的对象，不同是Buffer是专门用来保存二进制数据的(数据储存为二进制数据，性能是最好的)。

**Buffer 类在全局作用域中，在Global上，可以直接使用，因此无需使用 require('buffer').Buffer**

###  Buffer特点

1) 大小固定：在创建时就确定了，且无法调整

2) 性能较好：直接对计算机的内存进行操作

3) 每个元素大小为1字节



### Buffer的创建

- **Buffer.alloc(size，file)：** 返回一个指定大小的 Buffer 实例，如果没有设置 file，则默认填满 0

  ```js
  const buff1 = Buffer.alloc(10);
  //在内存中找到一篇区域，清空内容并创建一个空的buffer
  console.log(buff1); //<Buffer 00 00 00 00 00 00 00 00 00 00>
  
  //第二个参数是fill 填充buffer数据 但是长度确定 内容要填充完整  
  const buff3 = Buffer.alloc(10, "todayis");
  console.log(buff3) //<Buffer 74 6f 64 61 79 74 6f 64 61 79>
  ```

  

- **Buffer.allocUnsafe(size)：** 返回一个指定大小的 Buffer 实例，但是它不会被初始化，所以它可能包含敏感的数据

  ```js
  //当计算机内存不要的时候，是等待空闲才会把当前内存的数据清除
  //allocUnsafe 就是直接找了一个空闲的区域，但是这个区域中可能还有一些垃圾数据
  //但是这种方法创建的buffer最快速
  
  const buff2 = Buffer.allocUnsafe(10);
  console.log(buff2); //<Buffer 98 38 71 03 6a 01 00 00 00 00>
  ```

  

- **Buffer.from(string)：**  根据传入内容的长度 创建一个Buffer 

```js
const buff4 = Buffer.from('atguigu尚硅谷');
console.log(buff4);
```



### 使用

- Buffer可以通过forEach遍历出来
- Buffer可以通过toString方法展示出buffer的数据



```js
const buff4 = Buffer.from('atguigu尚硅谷');

//Buffer可以通过forEach遍历出来
buff4.forEach((item, index) => {
    console.log(item); //转换成10进制打印出来了
})

//Buffer可以通过toString方法展示出buffer的数据
console.log(buff4.toString())//atguigu尚硅谷
```



## process

### process是什么

`process` 对象是一个全局变量，它提供有关当前 Node.js 进程的信息并对其进行控制。 作为一个全局变量，它始终可供 Node.js 应用程序使用，**无需使用 `require()`。 它也可以使用 `require()` 显式地访问：**

```js
const process = require('process');
```

### process的常见的属性和方法

- argv 属性返回一个数组，其中包含当启动 Node.js 进程时传入的命令行参数

```js
//获取启动的时候 命令参数
console.log(process.argv);
[
  'C:\\Program Files\\nodejs\\node.exe',   //node命令安装路径
  'C:\\Users\\宋文现\\Desktop\\text\\index.js' //启动文件的绝对路径
]
```



- argv0 属性保存当 Node.js 启动时传入的 `argv[0]` 的原始值的只读副本,**也就是获取nodejs程序目录**

```js
console.log(process.argv0)
得到：C:\Program Files\nodejs\node.exe
```



- env 属性返回或设置包含用户环境的对象，环境变量：Path --> 遍历每个路径，找到程序运行
- 我们可以通过process.env.NODE_ENV 来设置当前的环境是开发环境还是生产环境，方便判断

```
console.log(process.env.NODE_ENV) 
```



- cwd() 方法返回 Node.js 进程的当前工作目录。绝对路径

```js
//process.cwd()Node.js 进程的当前工作目录 (启动时候所在的绝对路径，而不是启动文件所在的绝对路径)
console.log(process.cwd());  //C:\Users\宋文现\Desktop\text

```

- exit([code]) 退出进程

```js
let num = 0;
setInterval(() => {
    num++;
    if (num >= 5) {
        process.exit(); //直接退出当前进程
    }
    console.log(num)
})

console.log("haha")
```



## path路径

### path是什么

`path` 模块提供用于处理文件路径和目录路径的实用工具。**需要require引入**

**path.resolve([...paths]) 方法将路径或路径片段的序列解析为绝对路径,只是构建一个路径，文件不一定存在**

```js
//path.resolve()把路径解析为一个绝对路径
//参数可以是一个相对路径（相对启动命令的路径） 所以得到的路径可能是错误的

//如果在当前目录（day03）启动 则 得到的路径是C:\Users\lipeihua\Desktop\class0610\day03\08.path.js
const path1 = path.resolve('./08.path.js')
console.log(path1);


//如果在上级目录（class0610）启动 则 得到的路径是C:\Users\lipeihua\Desktop\class0610\08.path.js
const path2 = path.resolve('./08.path.js')
console.log(path2);


//resolve可以传入多个路径，会拼接路径
const path3 = path.resolve('./today', "./a", '../b', "text.js")
console.log(path3);  //C:\Users\宋文现\Desktop\text\today\b\text.js


//无论在哪里启动，都要得到当前文件夹中的03.procees.js的路径
//先获取当前的文件的绝对路径（无论启动命令在哪里，绝对路径不会发生变化）__dirname
//然后再去根据当前的绝对路径，去找到你要使用的文件路径

const path = require("path");
const path4 = path.resolve(__dirname, "03.process.js");
console.log(path4);// C:\Users\宋文现\Desktop\text\03.process.js
```





## fs文件系统

### fs是什么

全称为file system，所谓的文件系统，就是对计算机中的文件进行增删改查等操作。它是一个服务器的基础，在Node中通过fs模块来操作文件系统。

###  fs的使用

- fs模块是Node的核心模块，不需要下载，直接引入即可使用

  ```js
  const fs = require('fs');
  ```

- fs中的大部分方法都为我们提供了两个版本：

  - 同步方法：带sync的方法
    - 同步方法会阻塞程序的执行
    - 同步方法通过返回值返回结果
  - 异步方法：不带sync的方法
    - 异步方法不会阻塞程序的执行
    - 异步方法都是通过回调函数来返回结果的

### 文件的写入

#### 同步写入（一般不用）（openSync，writeSync ，closeSync）

```js
/* 
    写入：
        - openSync 打开文件
            fs.openSync(path[, flags, mode])
            flag:
                'r': 打开文件用于读取。 如果文件不存在，则会发生异常 (默认)
                'a': 打开文件用于追加。 如果文件不存在，则创建该文件。
                'w': 打开文件用于写入。 如果文件不存在则创建文件，如果文件存在则截断文件。
            mode:默认值一般是0o666 一般不会修改（尤其windows系统）
        - writeSync 写入文件
            fs.writeSync(fd, buffer[, offset[, length[, position]]])
            - fd 打开的文件的标识
            - buffer 写入的内容  可以是buffer 也可以是字符串
            - offset 书写的位置 前面空几个字符
        - closeSync 关闭文件
            - 方法关闭打开的文件

*/

const fs = require("fs");
const path = require("path");

//为了确保打开路径正确，所以先使用path.resolve方法处理 你需要的路径
const filePath = path.resolve(__dirname, './txt', "01.txt");
console.log(filePath) //获取正确的路径

//打开文件
const fd = fs.openSync(filePath, "w", 0o666);
console.log(fd) //代表当前文件的识别码

//写入文件
const result = fs.writeSync(fd, "今天天真好111", 3);
console.log(result); //18  字节数

//关闭文件
const resultClose = fs.closeSync(fd);
console.log(resultClose); //关闭文件没有返回值

```



#### 异步写入（**open  write  close**）

**open  write  close**  打开里嵌套写入，写入里嵌套关闭

```js
const fs = require("fs");
const path = require("path");

//处理路径
const filePath = path.resolve(__dirname, "./int/01.txt");;

//打开文件 回调函数有两个参数，err,data
const fd = fs.open(filePath, "w", (err, fd) => {
    //err在前，因为nodejs是错误优先处理机制,如果有错误则返回错误对象，否则返回null
    if (err) {
        console.log("文件打开失败");
        return;
    }
    //文件打开成功
    console.log(fd); //当前打开文件的句柄（识别码）
    console.log("文件打开成功")


    //写入文件
    fs.write(fd, "锄禾日当午~", (err, data) => {
        //错误处理
        if (err) {
            console.log("文件写入失败");
        }
        console.log(data); //写入的字节数量
        console.log("文件写入成功");

        //关闭文件 无论写入成功或者而失败 都要关闭
        fs.close(fd, (err) => {
            if (err) {
                console.log("文件关闭失败")
            }
            console.log("文件关闭");
        })
    })
})
console.log(fd); //undefined 异步得到的结果在回调函数中
```

#### 解决异步写入的回调地狱

```js
//引入包
const fs = require("fs");
const path = require("path");
const {
    resolve
} = require("path");

//处理路径
const filePath = path.resolve(__dirname, "./txt/01.txt");;

//把封装的函数直接写入async函数中，模块化更明显
(async function () {
    // 异常处理 try...catch
    try {
        //await只能等待一个promise对象
        //await的返回值就是promise对象成功状态的resolve的参数
        const fd = await new Promise((resolve, reject) => {
            fs.open(filePath, "w", (err, fd) => {
                //err在前，因为nodejs是错误优先处理机制,如果有错误则返回错误对象，否则返回null
                if (err) {
                    console.log("文件打开失败");
                    reject("文件打开失败" + err);
                    return;
                }
                //文件打开成功
                console.log(fd); //当前打开文件的句柄
                console.log("文件打开成功")
                resolve(fd);
            })
        });
        await new Promise((resolve, reject) => {
            fs.write(fd, "锄禾日当午", (err, data) => {
                //错误处理
                if (err) {
                    console.log("文件写入失败");
                    reject("文件写入失败" + err);
                    return;
                }
                console.log(data); //写入的字节数量
                console.log("文件写入成功");
                resolve();
            })
        });
        await new Promise((resolve, reject) => {
            fs.close(fd, (err) => {
                //错误处理
                if (err) {
                    console.log("文件关闭失败");
                    reject("文件关闭失败" + err);
                }
                console.log("文件关闭成功");
                resolve();
            })
        })
    } catch (err) {
        console.log(err);
        console.log("文件出现错误")
    }
})();
```



#### 简单写入(fs.writeFile())

```
同步方法：fs.writeFileSync(file, data[, options])
异步方法：fs.writeFile(file, data[, options], callback)
```

> 参数：
>
> - file 要写入的文件的路径
> - data 要写入的内容，可以是一个String也可以是一个Buffer
> - options 配置对象，需要一个对象作为参数，默认如下： {encoding:"utf8",flag:"w",mode:0666}
> - callback 回调函数

```js
//异步
/* 
    简单写入：
        异步方法：fs.writeFile(file, data[, options], callback)
            file：要写入的文件地址
            data：写入的内容
            options：文件的配置  权限、字符编码  {encoding:"utf8",flag:"w",mode:0666}
            callback：回调函数

*/

const fs = require("fs");
const path = require("path");

const filePath = path.resolve(__dirname, "./txt/01.txt");

//简单写入
fs.writeFile(filePath, "今天天气真好~", {
    encoding: "utf-8",
    flag: "a",
}, (err, data) => {
    if (err) {
        console.log("文件写入失败" + err);
        return;
    }
    console.log("文件写入成功")
})
```



#### 流式写入(fs.createWriteStream())

**可写流**

- fs模块处理文件的缺点：将文件的数据全读到内存中，在把数据写到文件内，会大量占用内存
- 流（stream）是 Node.js 中处理流式数据的抽象接口，是一组有序的，有起点和终点的字节数据传输手段。可以实现将数据从一个地方流动到另一个地方，**其边读取边写入的特点有别于fs模块的文件处理，并且可以做到控制读取文件和写入文件的速度，从而减少内存的占用**
- 流是基于事件的，所有的流对象都用 on(once)绑定事件，并触发 (once只绑定一次）
- 流式文件写入适用于一些比较大的文件，可以分多次向文件中写入内容，有效避免内存溢出的问题

```
fs.createWriteStream(path[, options])
```

```js
/* 
    fs.createWriteStream(path[, options])
        创建可写流
            - path 被写入的文件
            - options  配置
*/

const fs = require("fs");
const path = require("path")

const filePath = path.resolve(__dirname, "./txt/01.txt");

//创建可写流  
const ws = fs.createWriteStream(filePath);
// console.log(ws); //返回的是一个WriteStream对象 里边包含对当前流文件的操作方法和属性

//在nodejs中使用on或者once来绑定事件
//流是支持事件的，可以通过事件来知道流的状态
//WriteStream 有open事件，当流打开的时候触发，有一个close方法,当流关闭的时候触发
ws.once("open", () => {
    console.log("开始写入文件.....")
})

ws.once("close", () => {
    console.log("写入文件结束，文件关闭")
})


//write方法 向可写流中书写内容  可以书写1次或多次
ws.write("啊，雨下的真大");
ws.write("我的衣服没有晾");
ws.write("今天晚上有台风啊");
ws.write("漂亮");


ws.close(); //关闭可写流 但是关闭的是末尾（可能造成数据丢失）
ws.end(); //关闭可写流，关闭的是开始，数据是完整的
```



### 文件的读取

#### 简单读取文件(fs.readFile())

```
同步：fs.readFileSync(path[, options])
异步：fs.readFile(path[, options], callback)
```

> 参数：
>
> - path 读取文件的路径
>
> - options 配置对象 默认如下： {encoding:"utf8",flag:"w",mode:0666}
>
> - callback 回调函数，通过回调函数返回读取到的数据
>
>   err 错误对象
>
>   data 返回的数据（Buffer）

```js
/* 
    简单读取：
        异步方法：fs.readFile(file, [, options], callback)
            file：要写入的文件地址
            options：文件的配置  权限、字符编码  {encoding:"utf8",flag:"w",mode:0666}
            callback：回调函数
*/

const fs = require("fs");
const path = require("path")

const filePath = path.resolve(__dirname, "./txt/01.txt");

fs.readFile(filePath, (err, data) => {
    if (err) {
        console.log("文件读取失败" + err);
        return;
    }
    console.log(data); //读到的是一个buffer
    console.log(data.toString()); //啊，雨下的真大我的衣服没有晾今天晚上有台风啊漂亮
})
```



#### 流式读取文件(fs.createReadStream())

**可读流**

适合较大的文件

创建可读流 fs.createReadStream()

```js
/* 
    fs.createReadStream(path[, options])
        创建可读流
            - path 被读取文件
            - options  配置
*/

const fs = require("fs");
const path = require("path")

const filePath = path.resolve(__dirname, "./txt/01.mp4");

//创建可写流  
const rs = fs.createReadStream(filePath);
// console.log(rs); //返回的是一个ReadStream对象 里边包含对当前流文件的操作方法和属性

//事件驱动
let num = 0;
//每次读取事件
rs.on("data", (chunk) => {
    //流式读取的话，是每次读取64kb
    //data事件就是消费每一次的读取 
    // chunk就是每次读取的数据
    console.log(chunk);
    num++; //计数器，检测读取次数
    console.log(num)
})

//可读流关闭事件
rs.on("end", () => {
    console.log("可读流关闭")
})
```

#### promisify

- promisify是util工具包的一个对象，所以需要require引入
- promisify是把一个普通函数，封装成一个返回promise对象的函数
-  异常处理 try...catch ，有错误会返回catch处理。不会影响asyn函数

```js
//promisify解决回调地狱
const fs = require("fs")
const path = require("path")
const filePath = path.resolve(__dirname, "./int/008.txt")
const {
    promisify
} = require("util");

const openFile = promisify(fs.open);
const writeFile = promisify(fs.write);
const closeFile = promisify(fs.close);

(async function () {
    // 异常处理 try...catch
    try {
        const fd = await openFile(filePath, "w");
        await writeFile(fd, "锄禾日当午，汗滴禾下土");
        await closeFile(fd);
    } catch (err) {
        console.log(err);
        console.log("文件出现错误")
    }
})();
```



```js
//简单读取
const fs = require("fs");
const path = require("path")

const filePath = path.resolve(__dirname, "./txt/01.txt");

fs.readFile(filePath, (err, data) => {
    if (err) {
        console.log("文件读取失败" + err);
        return;
    }
    console.log(data); //读到的是一个buffer
    console.log(data.toString()); //啊，雨下的真大我的衣服没有晾今天晚上有台风啊漂亮
})

//promisify封装
const fs = require("fs");
const path = require("path")
const {
    promisify
} = require("util");


const filePath = path.resolve(__dirname, "./txt/01.txt");

//把fs.readFile方法处理为一个返回promise对象的方法
//当使用readFile来读取文件的时候，如果读取成功则会返回一个成功状态的promise，否则返回一个失败的状态
const readFile = promisify(fs.readFile);
readFile(filePath)
    .then((data) => {
        console.log(data);
    })
```

```js
const fs = require("fs");
const path = require("path")
const {
    promisify
} = require("util");


const filePath = path.resolve(__dirname, "./txt/01.txt");

//把fs.readFile方法处理为一个返回promise对象的方法
//当使用readFile来读取文件的时候，如果读取成功则会返回一个成功状态的promise，否则返回一个失败的状态
const readFile = promisify(fs.readFile);
//data就是读取的文件数据
(async function () {
    const data = await readFile(filePath);
    console.log(data);
})();
```



### 流式读写

```js
//流式读写.js

const fs = require("fs");
const path = require("path");


const filePath1 = path.resolve(__dirname, "./txt/01.mp4");
const filePath2 = path.resolve(__dirname, "./txt/02.mp4");

//创建可读流
const rs = fs.createReadStream(filePath1);
//创建可写流
const ws = fs.createWriteStream(filePath2);

 //读写
rs.on("data", (chunk) => {
    //chunk就是每次读取的64kb的数据
    ws.write(chunk);
})
rs.on("close", () => {
    console.log("读取完成")
}) 

// pipe()方法是可读流的一个方法，可以持续的消费可读流的内容，并直接写入可写流
//读写完成自动关闭
rs.pipe(ws);
```



##  events 事件触发器

### 什么是events

- 所有能触发事件的对象都是 `EventEmitter` 类的实例。这些对象有一个 `eventEmitter.on()` 函数，用于将一个或多个函数绑定到命名事件上。还有一个.emit()函数用于触发监听器。

```js
  const EventEmitter = require('events');
  class MyEmitter extends EventEmitter {}
  const myEmitter = new MyEmitter();
  myEmitter.on('event', () => {
    console.log('触发事件');
  });
  myEmitter.emit('event');
```

- 当绑定多个函数时，本质是同步执行的，当里面含有异步函数时，就会切换到异步模式。
- once()函数可注册最多只监听一次的函数。

### 常见方法

- on()添加一个监听器(随便定义)
- addListener().和上面一样，别名而已。
- once(),某个事件只执行一次。
- emit() 事件触发器。

```js
//引入一个events包
const EventEmitter = require('events');

//创建一个MyEmitter类 继承了引入的EventEmitter类,不继承也可以，继承的话可以区分多个事件
class MyEmitter extends EventEmitter {}

//实例化MyEmitter的类
const myEmitter = new MyEmitter();


//实例化对象可以使用on来定义一个事件
myEmitter.on('event', () => { //自己想定义什么都可以
    console.log('触发事件');
});

// 还是这个实例化对象，可以使用emit来调用你自己定义的事件
myEmitter.emit('event');



myEmitter.once("haha", () => {
    console.log("笑个鬼哦~")
})

myEmitter.emit("haha");//笑个鬼哦~
```



##  crypto 加密

###  什么是crypto

`crypto` 模块提供了加密功能，包括对 OpenSSL 的哈希、HMAC、加密、解密、签名、以及验证功能的一整套封装。

使用 `require('crypto')` 来访问该模块。

### 加密算法

消息摘要加密算法 （md5 sha1 sha256 sha512）

- 生成的密文长度固定
- 同样的明文加密后一定得到同样的密文
- 不可逆 （不能通过密文逆向破解明文）

```js
/* 
    crypto 
        消息摘要加密算法
        MD5(128) sha1(160) sha256(256) sha512(512)
        - 生成的密文长度固定
        - 明文加密后得到的是固定的密文
        - 不可逆
*/

//创建明文
const crypto = require("crypto");
const str = "lipeihua0922";

//一般来说的这样直接把明文转换成密文容易被破解，所以我们一般会加点料
const myStr = str + "sha"

//createHmac方法 第一个参数是加密方式，第二个参数是明文
//createHmac方法返回的对象 调用digest('hex') 则返回密文的哈希值
const secret1 = crypto.createHmac("MD5", myStr).digest('hex');
const secret2 = crypto.createHmac("MD5", secret1).update('I love huahua').digest('hex');
const result = crypto.createHmac("MD5", secret2).update('haha').digest('hex');
console.log(result);
```





# Node.js静态服务器

## http

###  什么是http

- 传统的HTPP服务器会由Aphche、Nginx、IIS之类的软件来担任，但是nodejs并不需要，nodejs提供了http模块，自身就可以用来构建服务器，而且http模块是由C++实现的，性能可靠
- 要使用 HTTP 服务器和客户端，必须 `require('http')`。
- 既能搭建服务器，也能客户端
  - 服务器：接受请求、处理请求、返回响应
  - 客户端：发送请求，接受响应

### http概念

- ​    http:超文本传输协议，规定的是网络通信中，两台计算机之间通信的规则。所有的www文件都要执行这个标准
- ​    报文：网络通信的内容我们称之为报文，请求发送的内容是请求报文，响应的内容是响应报文
- ​    http协议其实是规定的请求和响应的报文格式



### 如何使用http

- 通过指令运行服务器

  ```
  node xxx
  ```

- **通过 [http://localhost:3000](http://localhost:3000/) 访问服务器**

- **一旦访问服务器，服务器就会通过 http.createServer(callback(req,res)) 中 callback 处理请求**

- **res.end("hello server~"); 来返回响应**

- **通过server.listen(port, host,callBack)来监听端口号和主机**

-  **request 请求对象：客户端发送给服务器的数据    **

- **response 响应对象：服务器发送给客户端的数据**

## 第2章 搭建服务器端和客户端

### 2.1 搭建服务器端

```js
 //无论是搭建http服务器，还是客户端，都要引入http模块
const http = require("http");

//创建一个http服务
//serve就是你创建的服务
const serve = http.createServer((request, response) => {

    //设置相应头 Content-Type 就是相应的文件格式和字符编码
    response.setHeader("Content-Type", "text/html;charset=utf-8")

    //返回响应内容
    response.end("<h1>今天天气真好</h1>");
})

//启动服务
const port = 3838;   //端口号范围内随意
const host = "localhost"; //创建一个本机的服务器 也可以写作 127.0.0.1，也可以自己的IP地址，同一网段可访问

//启动服务 以及 确定域名和端口号
serve.listen(port, host, (err) => {
    if (err) {
        console.log("服务器打开失败" + err);
        return;
    }
    console.log(`服务器启动成功 请访问 http://${host}:${port}`)
});
```

### 2.2 搭建客户端

```js
const http = require("http");

const url = "http://localhost:3838";
//书写请求
const request = http.request(url, res => {
    //res就是得到相应的内容
    // console.log(res);

    //res.statusCode是相应状态码
    console.log(res.statusCode)

    let result = "";
    //响应的data事件 用来消费读取的内容，每次传输的是64kb  需要我们在事件中把响应的内容拼接
    res.on("data", chunk => {
        //得到所有的响应内容
        result += chunk;
    })
    console.log(result)

})

//发送请求
request.end();
```



# HTTP课程

### HTTP协议是什么

- http:超文本传输协议，规定的是网络通信中，两台计算机之间通信的规则。所有的www文件都要执行这个标准
- ​    报文：网络通信的内容我们称之为报文，请求发送的内容是请求报文，响应的内容是响应报文
- ​    http协议其实是规定的请求和响应的报文格式

### 什么是HTTP报文

- 它是HTTP应用程序之间发送的数据块。这些数据块以一些文本形式的元信息开头，这些信息描述了报文的内容及含义，后面跟着可选的数据部分。这些报文都是在客户端、服务器和代理之间流动。

- **HTTP报文的流动方向：**一次HTTP请求，HTTP报文会从“客户端”流到“代理”再流到“服务器”，在服务器工作完成之后，报文又会从“服务器”流到“代理”再流到“客户端”

- 报文的语法：

  所有的HTTP报文都可以分为两类，

  请求报文和响应报文

  。请求和响应报文的基本报文结构大致是相同的，只有起始行的语法有所不同。

  - **请求报文：**它会向Web服务器请求一个动作
  - **响应报文：**它会将请求的结果返回给客户端。



### 报文包

**由报文首行，报文头部 ，报文空行，报文体四部分组成**

**又分为请求报文包和响应报文包**

#### 请求报文包

##### GET请求报文包

请求报文首行:

```js
 GET http://localhost:3838/?username=lipeihua&password=123456 HTTP/1.1
    - GET：请求方式，get请求是把请求内容拼接在请求地址上的
    - http://localhost:3838/：请求地址
    - ?username=lipeihua&password=123456：请求参数 （查询字符串）
    - HTTP/1.1：http协议的版本（兼容性最好的）   最新的是http/2版本
```

请求报文头部:

```js
- 请求的主机地址  域名+端口号
    Host: localhost:3838 
    - 连接方式：保持长连接（会在一定时间内 保持客户端和服务端的连接，当短时间内多次请求的话，可以节省时间）
    Connection: keep-alive
    - 允许使用https访问
    Upgrade-Insecure-Requests: 1
    - 用户代理（一般是浏览器的信息）
    User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.97 Safari/537.36
    - 客户端可以接收的类型
    Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,\*\/\*;q=0.8,application/signed-exchange;v=b3;q=0.9
    Sec-Fetch-Site: none
    Sec-Fetch-Mode: navigate
    Sec-Fetch-User: ?1
    Sec-Fetch-Dest: document
    - 客户端可以接受的压缩格式  gzip  deflate br
    Accept-Encoding: gzip, deflate, br
    - 可以支持的语言
    Accept-Language: zh-CN,zh;q=0.9
```

请求报文空行:

```
一个回车或者空格
```

请求报文体:

```js
 get请求的报文体 是直接跟在了请求地址上的(等于没有)
```



##### post请求报文包

请求报文首行:

```js
  POST http://localhost:3838/ HTTP/1.1
```

请求报文头部:

请求报文空行:

请求报文体:

```js
GET请求的报文体在首行中，但是POST是在报文体中
username=lipeihua&password=123456
```



#### 响应报文包

##### 	GET响应报文包

​	响应报文首行：

```js
HTTP/1.1 200 OK
    - HTTP/1.1 ：协议版本
    - 200OK ：响应状态码
```

​	响应报文头部：

```js
 - 响应数据的类型
    Content-Type: text/html;charset=utf-8
    - 响应的事件
    Date: Wed, 19 Aug 2020 03:13:21 GMT
    - 保持长连接
    Connection: keep-alive
    - 响应的内容的字节长度
    Content-Length: 27
```

​	响应报文空行：

​	响应报文主体：

```
<h1>今天天气真好</h1>
```



##### 	post响应报文包和get一样



###  请求方式

**请求方式 GET（查询） POST（新增） PUT（修改） DELETE（删除） OPTIONS（预检）**

- 请求地址 请求服务器地址 协议名://域名（ip地址）:端口号/路径
- 请求参数
  - GET参数：请求首行
    - 也叫做查询字符串（querystring）参数
    - ?username=admin&password=123456
    - ?key=value&key=value&key=value
  - POST参数：请求体
    - 也叫做请求体（body）参数
    - username=aaaa&password=bbbbb

```js
 				1.GET：（查）
            - 用于请求指定的页面信息，并返回实体
            - 数据会随着url请求地址发送（查询字符串）

        2.POST：（增）
            - 向指定的资源提交数据进行处理（登录注册）
            - 数据会在请求报文的报文体中发送

        3.PUT：（改）
            - 更改服务器数据

        4.DELETE（删）
            - 删除指定的数据

```



###  Content-Type(响应头部内)

`Content-Type`实体头部用于指示资源的MIME类型。

在响应中，Content-Type标头告诉客户端实际返回的内容的内容类型。

MIME类型也叫媒体类型，是一种标准，用来表示文档、文件或字节流的性质和格式。

重要的MIME类型：

```js
 - text/plain   文本类型
        - text/html html
        - text/css  css
        - application/javascript  js
        - application/json json
        - image/jpg
        - image/jpeg
        - image/png
        - image/gif
        - image/webp webp图片
        - audio/mp3 音频
        - video/mp4 视频
        - application/x-www-form-urlencoded form表单

response.setHeader("Content-Type", "application/javascript;charset=utf-8");
```



### Status(响应状态码)

HTTP 响应状态代码（status）指示特定 HTTP 请求是否已成功完成。响应分为五类：

列举一些主要记住的：

```js
				1XX： 正在处理请求中
            100：一切正常，正在请求
            101：正在切换协议
        2XX：请求成功
            200：请求成功
            204：PUT和DELETE一般返回204，表示页面主体不发生修改
        3XX: 重定向
            301：永久重定向
            302：临时重定向
            304：读取缓存（协商缓存）
        4XX：客户端错误
            400: 代表响应报文有问题，需要修改
            403：代表服务器拒绝访问
            404：服务器端无法找到请求的资源，或者服务端不想给给响应
        5XX：服务端错误
            500：代表服务器端执行客户端请求时出错
            503：代表服务器超负荷或者停机维护中，无法处理响应
```



## 相关面试题

###  从url输入地址到最终网页渲染，中间发生了什么？

**1.DNS 查询/解析**

​    输入的是域名，需要DNS解析

​    DNS解析是把域名解析为IP地址（4个缓存 1个递归查询）

​    \- 浏览器的DNS缓存

​    \- 计算机DNS缓存

​    \- 路由器DNS缓存

​    \- 运营商缓存

​    \- a.com  b.a.com   c.b.a.com



  **2.tcp三次握手**

​    目的：确认让客户端和服务端互相知道，对方有接受和发送的能力

​    为什么要第三次握手：因为第三次握手以后，服务端才能知道客户端有接受信息的能力

​    \- 客户端发送给服务端，告诉服务端我要准备发请求了

​    \- 服务端发送给客户端，我同意你可以发请求

​    \- 客户端发送给服务端，好的收到你的确认信息



​    Connection: keep-alive：长连接 代表的就是tcp的连接



  **3.发送请求**

​    客户端发送请求报文给服务端



  **4.返回响应**

​    服务端处理完客户端的请求，并向客户端返回响应报文包

 

  **5.渲染页面**

​    \- 根据html生成DOM tree

​    \- 根据css生成CSSOM tree

​    \- 结合CSSOM和DOM 生成 render tree(渲染树)

​    \- 分层（根据层叠上下文分层）

​    \- 生成图层绘制指令

​    \- 栅格化 把图层分为图块

​    \- 合成与显示 最终绘制过程

  

  **6.TCP四次挥手**

​    \- 客户端发送给服务器端：请求报文发送完毕

​    \- 服务器端发送给客户端：请求报文已经接受完毕，可以等待断开

​    \- 服务器端发送给客户端：响应报文已经发送完毕

​    \- 客户端发送给服务器端：响应报文已经接受完毕 可以断开连接



### 为什么建立连接是三次握手，而关闭连接却是四次挥手呢？

TCP 是双向的，所以需要在两个方向分别关闭，每个方向的关闭又需要请求和确认，所以一共就4次。









## 缓存



![image-20200824202755366](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200824202755366.png)







## session

![image-20200824203042065](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200824203042065.png)