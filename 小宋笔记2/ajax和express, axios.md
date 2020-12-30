# 033cxAjax阶段笔记

Ajax就是让浏览器跟服务器交互的一套API，通过它咱们就可以跟服务器进行交互

# 服务器

## 什么是服务器

服务器就是提供了某种服务的性能很强的公共计算机。

服务器常见的服务有:

1. 数据库服务
2. web服务
3. 文件服务
4. 多媒体服务
5. 邮件服务 
6. 等

为了能够提供这些服务咱们只需要安装对应的软件即可

## web服务器

web服务器就是提供web服务的计算机

通俗来说：就是让你可以访问在其内部的资源(比如网页)。

### web服务器访问流程

咱们在访问web服务器时其实经历了:

1. **浏览器(客户端) 通过 请求 向服务器发送数据**
2. **服务端通过 响应 向浏览器(客户端)响应数据**

这么一个 **请求 - 响应** 的流程

### 请求响应流程

1. 用户在浏览器地址栏输入我们需要访问的网站网址（`URL`）
2. 浏览器向服务器发送请求(HTTP 请求)，告知服务器要获取的内容(`请求`)
3. 服务端处理浏览器发送的请求，并把处理的结果返回(HTTP 响应)给浏览器(`响应`)
4. 浏览器将服务端返回的结果呈现到界面上

凝练一点就是:**浏览器发送请求并接收服务器的响应**

## 向服务器发送请求的几种方式

1.a标签的跳转

2.浏览器直接输入地址

3.通过window.location.hred

这三种方式本质都是一样的，通过修改浏览器的URL向服务器发送请求，但这三种方法更新信息的时候都会跳转页面

4.ajax

**更新信息时不会跳转页面**



## ajax优点和缺点

优点：

1. 最大的一点是页面无刷新即可获取数据，用户的体验非常好。
2. 使用异步方式与服务器通信，具有更加迅速的响应能力。
3. 可以把以前一些服务器负担的工作转嫁到客户端，利用客户端闲置的能力来处理，减轻服务器和带宽的负担，节约空间和宽带租用成本。并且减轻服务器的负担，ajax的原则是“按需取数据”，可以最大程度的减少冗余请求，和响应对服务器造成的负担。
4. 基于标准化的并被广泛支持的技术，不需要下载插件或者小程序。

缺点：

1. ajax不支持浏览器back按钮。
2. 安全问题 AJAX暴露了与服务器交互的细节。
3. 对搜索引擎的支持比较弱。
4. 破坏了程序的异常机制。
5. 不容易调试。





## Ajax基本使用

### 基本使用-get请求

ajax_get使用步骤就4步

1.创建xhr对象

2.规定请求方式和地址（接口文档有，别乱写，请求方式地址和服务器的一致则返回内容）

3.发送请求

4.注册onload事件,拿到服务器给你的结果

```js
//1.创建xhr对象
var xhr = new XMLHttpRequest();
//2.规定请求方式和地址
xhr.open('get','服务器地址');  //open有三个参数，第一个参数是请求方式，第二个参数是请求地址，第三个参数是同步还是异步
//3.发送请求
xhr.send();//参数为空或者null
//4.拿到结果
xhr.onload = function(){
    console.log(xhr.responseText);
}
//下面这个老版本比较复杂
xhr.onreadystatechange = function () {
    if (xhr.readyState === 4 && xhr.status >= 200 && xhr.status <= 300) {
        console.log(xhr.responseText)//返回的数据，具体看你要怎么用
    }
}

```

步骤说明

1.xhr对象即`XMLHttpRequest`，这是ajax里的核心对象，xhr取自每个单词首字母

2.初始化请求，我们用`open`方法，参数1是请求方法，参数2代表你要请求的服务器路径

3.发送请求

4.发请求就是为了拿到服务器给我们的结果



#### get传送数据

请求地址?参数名=传递的数据（在请求地址后面拼接）

```js
var xhr = new XMLHttpRequest();
xhr.open('get',' http://127.0.0.1:8081/sum?a=1&b=2');
xhr.send();
xhr.onload = function(){
    console.log(xhr.response);
   }

//get请求在IE浏览器中，如果请求地址相同 可能会读取缓存
//在不影响其他的情况下，修改请求地址，就可以每次不会读取缓存
//在请求地址中添加一个时间戳
xhr.open("GET", "http://localhost:3001/login?user=" + oUser.value + "&pass=" + oPass.value + "&_t=" + Date.now(), true);
//服务器端可以使用 if (req.url.startsWith('/login'))来判断路径是否正确
```

### 基本使用-post请求

ajax_post使用步骤就5步

1.创建xhr对象

2.规定请求方式和地址

3.设置请求头

4.发送请求(此处传送数据)send中书写

5.注册onload事件,拿到服务器给你的结果

```js
//1. 创建xhr对象
var xhr = new XMLHttpRequest();
//2. 初始化请求
xhr.open('post',服务器地址);
//3. 设置请求头
xhr.setRequestHeader('Content-type','application/x-www-form-urlencoded');	//xhr.setRequestHeader("content-type", "application/json")
//4. 发送请求
xhr.send('提交给服务器数据');//发送数据
//5. 监听响应的回调函数
xhr.onload = function(){
    console.log(xhr.responseText);//返回的响应数据，看是什么类型的是否可以解构赋值
}
```

数据格式

```js
//发送的数据是键值对的形式的时候(请求头是默认值)
xhr.setRequestHeader("Content-Type","x-www-form-urlencoded");
xhr.send("user=lily"); 

//发送的格式是一个json字符串的时候
xhr.setRequestHeader("Content-Type", "application/json")
xhr.send('{"user":"lucy"}');//直接传
xhr.send(JSON.stringify(userData));//转json字符串传



// 没有数据 下面三种都可以
xhr.send()
xhr.send(null)
xhr.send(undefined)
```

get侧重获取 不安全 url有长度限制

post侧重提交 比较安全 url没有长度限制

### 数据传递

对于不同的请求方式数据传递的方式不同:

1.get请求直接拼接在url的后面:

```js
url?key=value&key2=value2
```

2.post请求通过send方法传递

```js
xhr.send('key1=value2&key2=value2')
```



### ajax同步和异步

ajax中根据async的值不同分为同步(async = false)和异步(async = true)

默认为true



## Ajax 解决浏览器缓存问题？

- 1、在ajax发送请求前加上 anyAjaxObj.setRequestHeader("If-Modified-Since","0")。
- 2、在ajax发送请求前加上 anyAjaxObj.setRequestHeader("Cache-Control","no-cache")。
- 3、在URL后面加上一个随机数： "fresh=" + Math.random();。
- 4、在URL后面加上时间戳："nowtime=" + new Date().getTime();。
- 5、如果是使用jQuery，直接这样就可以了 $.ajaxSetup({cache:false})。这样页面的所有ajax都会执行这条语句就是不需要保存缓存记录。



## jQuery中的AJAX请求

### $.ajax()使用方法

```js
$(() => {
    const userMes = {
        user: "lucy",
        age: 18
    }
    $("#form").submit(() => {
         $.ajax({
            //请求地址
            url:"http://192.168.16.38:3001/login",
            //请求方式
            method:"POST",
            //请求数据
            // data:"user=lily&age=15",
            headers:{
                "Content-Type":"application/json",
                "Content-Type":"x-www-form-urlencoded"
            },
            data:JSON.stringify(userMes),
            //成功回调
            success(data){
                console.log(data);
            },
            //失败回调
            error(err){
                console.log(err);
            }
        }) 
    })
})
```



### $.get()使用方法

```js
$(()=>{
    $("#form").submit(()=>{
       $.get("http://192.168.16.38:3001/login","user=lily&age=15",(data)=>{
              console.log(data)
       }) 
    })
})
```



### $.post()使用方法

```js
$(()=>{
    const userMes = {
        user: "lucy",
        age: 18
    }
    $("#form").submit(()=>{
        $.post("http://192.168.16.70:3001/login", JSON.stringify(userMes), (data) => {
          console.log(data)
        })
    })
})
```



# onreadystatechange()方法

onload()事件是新浏览器支持的事件,老浏览器不支持

刚开始ajax是使用onreadystatechange()方法

使用onreadystatechange()方法会根据状态值的变化而触发事件;

## readyState状态值

xhr.readyState;

| readyState | 状态描述         | 说明                                                      |
| ---------- | ---------------- | --------------------------------------------------------- |
| 0          | UNSENT           | 代理（XHR）被创建，但尚未调用 `open()` 方法。             |
| 1          | OPENED           | `open()` 方法已经被调用，建立了连接。                     |
| 2          | HEADERS_RECEIVED | `send()` 方法已经被调用，并且已经可以获取状态行和响应头。 |
| 3          | LOADING          | 响应体下载中， `responseText` 属性可能已经包含部分数据。  |
| 4          | DONE             | **响应体下载完成，可以直接使用 `responseText`。**         |

一般判断写法：

```js
xhr.onreadystatechange = function () {
    if (xhr.readyState === 4 && xhr.status >= 200 && xhr.status <= 300) {
        console.log(xhr.responseText)
    }
}
```



# express基本使用

1. 初始化 `npm init -y`
2. 安装第三方 模块 `npm i express `
3. 导入  并使用

#### 注意点

1. express依赖于很多的第三方模块
2. npm i express时会自动的下载 这些模块
3. 不需要人为的记忆，是自动的，知道有这个功能即可



### express实现接口（搭建服务器）

```js
// 导包
const express = require('express')
// 创建服务器
const app = express()//express 是一个函数
// express.static 可以指定一个文件夹作为静态资源。可以指定多个（再写一行）
app.use(express.static(__dirname+"/weibo"));// 函数对象，实现对静态资源的加载。
// 以get请求方式本服务器的话
app.get('/', (req, res) => {  // req 请求对象,res响应对象
    console.log('执行了');
    //express框架提供的方法,响应结果给浏览器
    res.send('Hello World!')
})

// 指定端口号为80。
app.listen(80,()=>{
    // 当服务创建完成之后执行该回调
    console.log("success");
})
```

```js
// 当遇到访问方法与地址相同时，遇到第一个，默认不会向下执行。只能响应一次
app.get("/my.html",function (req,res,next) {
    // res.end("get1->my.html");
    next();// 是一个函数，当执行该函数后，会继续向下查找是否有满足条件的路由。
})
app.get("/my.html",function (req,res) {
    console.log("get2");
    res.end("get2->my.html")
})
```

注意：

​	接口的逻辑根据需求来决定 ，不同的需求 ，逻辑是不同 ，编码的位置都在服务器端

​	epxress中，写在回调函数中

理解：

​	浏览器调用接口 理解为 调用服务器中的 一个方法

​	服务器方法执行完毕之后，返回一个结果（return）

​	浏览器接收结果，并且根据需求处理逻辑

### express注册路由

app.get('/路由地址'，回调函数)

1. 什么是路由,对应关系
   1. **后台地址** 和 **后台逻辑**的对应关系
   2. 后台逻辑 和 回调函数的对应关系



### get数据获取

get请求的数据，用对象 的方式保存到`query`或`params`中

###### 1、qeury  

```js
1、传     
     http: //127.0.0.1/sum/1/2 
2、接收  
   app.get("/sum/:a/:b",(req,res)=>{
       console.log(req.query);//{ a: '1', b: '2' }值是字符串类型的
     	const {a,b} = req.query //可以解构赋值出来
       res.end("/sum");
   })
```

###### 2、params  

```js
1、传  
http: //127.0.0.1:8081/sum?a=1&b=2
2、接收  
app.get("/sum",(req,res)=>{
    console.log("/sum/:a/:b",req.params) //sum/:a/:b { a: '1', b: '2' }//也是字符串，同样可以解构赋值出来
    res.end(req.params.a+req.params.b);
})
 
 ######老师对应博客地址 https://blog.csdn.net/u012149969/article/details/108107555  
```

### post数据-文本获取

1. 找到模块`body-parser`
2. 下载
3. 导入

重点

1. 使用body-parser解析post的数据
2. 整合了body-parser之后 rq属性会增加一个body属性 通过body属性可以获取提交的数据

```js
// 导包
const express = require('express')
// 导入 第三方模块 中间件
const bodyParser = require('body-parser')
// 创建服务器
const app = express()

// 添加接收post数据的功能
// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))
//或则,只接受JSON数据
app.use(bodyParser.json());
app.post('/',(req,res)=>{
    console.log(req.body)
})
```

1. extended: false：表示使用系统模块querystring来处理，也是官方推荐的
2. extended: true：表示使用第三方模块qs来处理
3. 从功能性来讲，qs比querystring要更强大，所以这里可以根据项目的实际需求来考虑

**application/x-www-form-urlencoded： 窗体数据被编码为名称/值对。这是标准的编码格式。**

**content-type:application/json的数据格式：json字符串**



### post数据-文件

1. 用到了哪个模块`multer`
2. 使用步骤
   1. 安装
   2. 导入
   3. 根据实例 c+v
   4. 上传
3. req.file 图片信息
4. req.body 其他信息

```js
var express = require('express')
var multer = require('multer')
var upload = multer({ dest: 'uploads/' })

var app = express()

// avatar 文件的key
app.post('/profile', upload.single('avatar'), function(req, res, next) {
  // req.file is the `avatar` file
  // req.file 图片信息
  console.log(req.file)
  // req.body will hold the text fields, if there were any
  // 文本信息
  console.log(req.body)
  res.send('welcome')
})

// 开启服务器
app.listen(3000, () => {
  console.log('success')
})

```



## body-parser 和 multer 区别

都用在post中,都有req.body属性

1. 纯文本用 body-parser
2. 文件 用  multer



## AJAX async await

```js
 function ajax(url){
        return new Promise((resolve,reject)=>{
            const xhr = new XMLHttpRequest();
            xhr.open("get",url);
            xhr.send();
            xhr.onload = function () {
                resolve(xhr.responseText);
            }
        })
    }
    (async function(){
        const res = await ajax("1、async 函数.html");
        console.log(res);
    })()
```













# 跨域

![](C:\Users\宋文现\Desktop\wenjian\images\1559699195946.png)

看到这种错误  就是跨域

## 跨域-基本概念

浏览器在Ajax调接口时，请求了一个` 不同源`的 地址 就回出现跨域问题**(针对Ajax请求，简单访问可以)**

## 跨域-同源

同源策略(Same-Origin Policy)最早由 Netscape 公司提出，是浏览器的一种安全策略。

一个完整的请求地址 包含了3个部分

http://127.0.0.1:4399/login 

1. 协议:http://
2. 地址:127.0.0.1
3. 端口:4399

什么是同源？

两个网址中:协议，地址，端口这三个 都一样

什么是不同源?

两个网址中:协议，地址，端口三个中任何一个不相同，称之为不同源

```js
// 浏览器地址
file:///C:/Users/hulinghao/Desktop/%E6%B7%B1%E5%9C%B3%E5%89%8D%E7%AB%AF%E5%B0%B1%E4%B8%9A35%E6%9C%9F/Node.js%20-%20day05/04-%E6%BA%90%E4%BB%A3%E7%A0%81/03.demo/login.html
// 浏览器地址2
http://127.0.0.1:5500
// 接口地址
http://127.0.0.1:4399/login 
```

**浏览器请求地址和Ajax请求的接口地址比较，两个网址中:协议，地址，端口三个中任何一个不相同，称之为不同源，即跨域**

## 跨域-浏览器同源安全限制 

1. 为了保证用户的安全，浏览器做了限制
2. 当ajax请求不同源接口是，默认不允许的，
3. 因为 页面请求不同源接口时，意味着 页面 和 接口所处的服务器是不一样的，请求的服务器中是否有恶意代码，无法确定，出于安全，直接不允许

## 跨域-方案

工作中不可避免会碰到一些不同源的调用情况，有很多的解决方案可以实现这个功能

### 跨域-方案1-CORS

需要后端配合，通过设置白名单（浏览器地址），允许你的浏览器地址下的ajax 代码可以请求这个不同源的接口。

##### 注意

1. 服务器设置一个允许的 头 (响应头)浏览器即可无视 同源限制 ，直接访问了

2. ##### 原理

   1. 服务器设置一个允许的`header`
   2. 浏览器接收到这个响应之后，检查是否有这个`header`
      1. 有：允许访问
      2. 没有：阻止 访问 ，直接报错
   3. 工作中，一般是后端来设置
      1. 碰到出现跨域问题，找后端

```js
//cors设置，只需要设置响应头Access-Control-Allow-Origin：值为你设置的白名单  白名单可以设置为* 代表所有
// res.setHeader("Access-Control-Allow-Origin","http://127.0.0.1:5500")
// res.setHeader("Access-Control-Allow-Origin","*")
//限定请求方式
// res.setHeader("Access-Control-Allow-Methods",'GET,POST,PUT,DELETE,OPTIONS')
//设置类型
//res.setHeader("Access-Control-Allow-Headers", "content-type");



//OPTIONS请求会出现在第一次跨域请求中（预检请求）
//先携带非常少量的数据去探探路看能不能跨域，如果可以 则在次进行我们需要的请求
res.setHeader("Access-Control-Allow-Methods",'GET,POST,PUT,DELETE,OPTIONS')
res.setHeader("Access-Control-Max-Age",0)



//设置白名单
//得到用户的请求地址
const userIP = req.headers.origin;

//设置多个白名单的处理
const whiteOrder = [
    "http://127.0.0.1:5500",
    "http://127.0.0.1:5501",
    "http://127.0.0.1:5502",
    "http://127.0.0.1:5503",
];
const ip = whiteOrder.find((item) => item === userIP)
console.log(ip);
res.setHeader("Access-Control-Allow-Origin",ip)
```

上面这种多个路由就不好用，所以主用下面：

```js
//all 所有的请求（ get post）。 * 所有的地址。
app.all("*", (req, res, next) => {
    // 允许所有的站点跨域访问
    // res.header("Access-Control-Allow-Origin","*");

    // 指定某一个站点允许跨域
    // res.header("Access-Control-Allow-Origin","http://192.168.16.24:8081");

    // 设置多个站点跨域（设置白名单）
    // console.log(req.headers.origin);// 获得请求信息当中的域名
    console.log(req.headers.origin)
    const arr = [
        "http://localhost:3000",
        "http://192.168.0.111:3000",
        "http://127.0.0.1:3000",
        "http://192.168.16.70:3000",
        "http://127.0.0.1:5502",
    ]
    //判断请求的路径是否在白名单内
    if (arr.includes(req.headers.origin)) {
        res.setHeader("Access-Control-Allow-Origin", req.headers.origin);
        //客户端带头请求也要设置
        res.setHeader("Access-Control-Allow-Headers", "content-type");
    }
    next();
})
```



### 跨域-方案2-JSONP（支持GET）

JSONP不是Ajax请求，因为他完全没有使用XMLHttpRequest对象

- JSONP是什么
  - JSONP(JSON with Padding)，是一个非官方的跨域解决方案，纯粹凭借程序员的聪明才智开发出来，只支持get请求。

- JSONP怎么工作的？
  - 在网页有一些标签天生具有跨域能力，比如：img link iframe script。（例如页面映入网上图片，jQueryCDN引入）
  - JSONP就是利用script标签的跨域能力来发送请求的。

- JSONP的使用

  - 动态的创建一个script标签
    - var script = document.createElement("script");
  - 设置script的src，设置回调函数
    - script.src = "http://localhost:3000/testAJAX?callback=abc";
  - 将script添加到body中
  - 服务器 接收script标签的src发过来的请求，返回了一个函数的调用
  - 浏览器接收到这个函数的调用，本地解析为js 并执行（只要页面中定义了这个函数就会被调用）

防止重名，可以通过变量书写函数名

**通过callback参数 告诉服务器函数名**

1. script的src属性中，额外的写一个参数 callback=xxx   xxx是函数名
2. 服务器接收 xxx 生成一个函数的调用，并返回 
   1. 接收callback的值
   2. 拼接为xxx({"name":"jack"})
3. 浏览器接收到返回的值，解析为js 并执行

   1. xxx({"name":"jack"})
4. callback后面的值可以改吗？

   1. 想怎么变就怎么写
5. 返回的值，服务器可以更改吗？

   1. 可以修改，可以根据逻辑返回

6. 现在很少用到，少到你毕业之后可能压根就不会去用

7. 挺喜欢问的

   1. 浏览器中：

      1. 定义函数 有一个形参
      2. 定义一个script标签 src指定一个不同源的地址 callback=函数名

   2. 服务器中:

      1. 接收callback的值
      2. 拼接为一个函数的调用 并返回 

8. 函数浏览器定义的 内部逻辑可以自己写

9. 参数 服务器返回的 具体是什么值 看服务器的代码

```js
 // 后端接口（地址）：返回的内容是行JS代码。通过该代码调用前端的函数，并传值
    const btn = document.querySelector("input[type=button]");
    function run(sum){
        console.log(sum);
    }
    btn.onclick = function () {
        const script = document.createElement("script");
        script.src = "http://127.0.0.1/my?age=8&cb=run";
        document.body.appendChild(script);
        document.body.removeChild(script);//用完了移除
    }
//后端
app.get("/my",(req,res)=>{
  	const num=100
    const age = (req.query.age || 0)/1;// 如果没有age属性，默认值0，转为Number
    const sum = age+num+"";
    const cb = req.query.cb;
    // 将数字转为字符串，拼接字符串
    res.end(cb+"("+sum+");");//返回js代码 run(sum)
})
```

**jquery**

```js
// 4.在全局声明一个回调函数，让script标签的src携带这个函数名去请求
function callBack(data) {
    console.log(data);
    console.log("22")
}
$(() => {
    $("#btn").click(() => {
        //1.创建一个script标签
        const newScript = document.createElement("script");
        //2.给script一个src 让js去跨域  
        //src的请求都是get请求，所以把回调函数 和需要发送给服务端的值 都拼接在src上
        newScript.src = "http://127.0.0.1:3001?user=lily&pass=12345&cb=callBack";
        //3.只有把script添加到页面中 才可以发送请求
        document.body.appendChild(newScript);
    })
})
//后端
console.log(req.url);
const userData = req.url.split("?")[1].split("&").reduce((p, c) => {
    const [key, value] = c.split("=");
    p[key] = value;
    return p;
}, {})
 // 解构赋值对象内容
const {
    user,
    pass,
    cb
} = userData

//只要想要向客户端响应  那么需要响应一个 res.end("cb(data)") (js类型)
if (user === "lily" && pass === "12345") {
    //成功登录响应
    const data = "登录成功";
    res.setHeader("Content-Type", "application/javascript");
    res.end(`${cb}('${data}')`)
    return;
}
```

### 跨域-方案3-proxy服务代理

代理：代理服务器。自身无法直接调用数据，可以通过创建一个服务，通过该服务调用你的数据。

我调用代理服务器，代理服务器调用你最终的服务

最终调用的服务将数据返给代理服务，代理服务获得数据返回给你。

意思就是：

​	1.自己建一个服务器，让自己的服务器去访问要跨域访问的服务器获得数据（服务器之间是没有跨域概念的）

​	2.我访问自己的服务器，让服务器把获得的数据再返给我，形成一个间接获得数据的方式

**需要借助第三方request模块 ，npm i  request  -D下载**

```js
//自己的服务器
const express = require("express")
const request = require("request");

const app = express()
app.use(express.static(path.resolve(__dirname, "./04")))

app.get("/login", (req, res) => {
   //  request是异步的。第一个参数是请求的地址，第二个参数是一个回调函数，用于接收值
    // err:错误。response:返回响应相关的信息  body:获得响应的数据。
  //第一个参数为要访问的地址
    request("https://m.lagou.com/listmore.json?pageNo=3&pageSize=15", (err, response, body) => {
        if (!err && response.statusCode === 200) {
            res.json(JSON.parse(body))
        } else {
            res.json({
                ok: -1,
                msg: "请求失败"
            })
        }
    })

})

app.listen(3005, () => {
    console.log("sussce")
})

//前端
const oBtn = document.getElementById("btn");
oBtn.onclick = function () {
    const xhr = new XMLHttpRequest();
    xhr.open("GET", "http://localhost:3005/login", true);
    xhr.send(null);
    xhr.onload = function () {
        // const res = JSON.parse(xhr.responseText).content.data.page.result;
        // console.log(res);
        console.log(xhr.responseText);//接受json格式数据
    }
}
```



# 模块化

## node 模块化

### 1、如何导出 mo.js

```$xslt
module.exports.a = 2;
module.exports.b = 3;
```

### 2、如何引入

```
const mo = require("./mo.js");
console.log(mo.a,mo.b);// 2,3
```

* 注意：1、导入时.js可以省略。如果名字省略，默认index.js.
      指定默认文件：新建一个package.json

    ```
      {
          "main":"my.js"
      }
    ```

## es6 模块化

1.如何导出

``` 
export let a = 1;
export let b = 2;
```

2、如何引入

``` 
<script type="module"> //必须要这个
    import {a,b} from "./module/mo.js"; //相对地址 需要用{}来接收
    console.log(a,b)
</script>
```

*******别名:解决命名冲突**************
1、导出

``` 
export let userName = "xixi";
```

2、引入:引入mo.js,并使用其中的userName,userName起了别名name

``` 
import {userName as name} from "mo.js"
```

**************可以通过该形式进行批量导出。************************
1、导出

``` 
const a = 1;
const b = 2;
export {a, b}
```

2、引入

``` 
import {a,b} from "./module/mo3.js";
console.log(a,b)
```

****************导出别名********************
1、导出

``` 
const a = 3;
const b = 4;
export {
    a as c,
    b
}
```

2、引入

```
import {c,b} from "./module/mo4.js";
    console.log(c,b)
```

***************默认导出************************
1、导出

export default 导出的是什么就是什么，直接接收直接用

```
// export default 只允许出现一次
   export default {a:1,b:2,fun(){}}
```

2、引入

```
    import mo from "./module.mo5.js"
```

****************混合导出***********************
1、导出

```
export let a = 1;
export default {
    a:3
}
```

2、引入

```
   import mo,{a} from "./module/mo6.js";
    console.log(mo.a,a);
```

**************** * as  ***********************
1、导出

```
export let a = 1;
export let b  = 2;
export let c = 3;
export let d = 1;
export let e = 1;
export let f = 1;
export let g = 1;
export let h = 1;
```

2、引入

```
 import * as mo from "./module/mo7.js";
 console.log(mo.a,mo.b,mo.c);
```

*******************************************

* 在使用es6模块化，以及ajax时不要直接打开页面，不要直接打开页面。要通过服务器方式打开

### es6导出引入

    1、导出：  
        export default {}// 导出对象  
        export default function(){}// 导出函数  
        export let userName = xxx;
        let a = 1;
        let b = 2;
        export {a,b as test};  
    2、引入  : 需要使用站点服务：http https
        <script type="module">
            import {userName as name} from "./mo.js";
            import my,{userName,a,b} from "./mo.js"
        </script>   

**边导入边导出**

```
export { default as trademark } from './product/trademark'
```





##### 

# fetch

```js
// fetch ---> ajax请求的一种
    // get  fetch 返回的是一个promise实例

    // patch -->post
    fetch("http://api.songwenxian.com:8090/scoreList/9", {
        method: "PATCH",//局部更新
        headers: {   //// xhr.setRequestHeaders
            "content-type": "application/json"
        },
        body: JSON.stringify({  // // body 相当于之前的xhr.send(); 
            userName: "伟人啊男"
        })

    }).then(response => response.json())//json()返回的结果是一个Promise.可以获得响应体的数据
        .then(res => {
            console.log(res);
        })
```



# axios的理解和使用

##  axios是什么?

1. 前端最流行的ajax请求库

2. react/vue官方都推荐使用axios发ajax请求

3. 文档: https://github.com/axios/axios

##  axios特点

1. 基于xhr + promise的异步ajax请求库

2. 浏览器端/node端都可以使用

3. 支持请求／响应拦截器

4. 支持请求取消

5. 请求/响应数据转换

6. 批量发送多个请求

其余：

```js
1. 函数的返回值为promise, 成功的结果为response, 异常的结果为error

2. 能处理多种类型的请求: GET/POST/PUT/DELETE

3. 函数的参数为一个配置对象

{

url: '',  // 请求地址

method: '',  // 请求方式GET/POST/PUT/DELETE

params: {}, // GET/DELETE请求的query参数

data: {}, // POST或PUT请求的请求体参数 

}

4. 响应json数据自动解析为json
```



## 使用

1、下载 https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js

2、引入 <script src="axios.min.js"/>

3、使用：curd操作

**第一个参数为地址，第二个参数是对象（里面是config配置信息，书写传参，请求信息），也可以把地址写在第二个参数对象config里**

```js
const btns = document.querySelectorAll("input");
btns[0].addEventListener("click", function () {
    const result = axios.get("http://api.songwenxian.com:8090/scoreList");
    result.then(value => {
        console.log(value);
    })
})
```

axios跟$一样，把axios.js文件引入进来就可以用了，返回值是一个promise

返回里有个config，为请求配置信息

##  不同类型的请求及其作用

1. GET: 从服务器端读取数据

2. POST: 向服务器端添加新数据

3. PUT: 更新服务器端数据 （完整更新数据）

4. PATCH: 更新服务器数据 （局部更新数据）

5. DELETE: 删除服务器端数据

## 拦截器(有两个参数，第二个为失败的回调)

### 请求拦截

```js
axios.interceptors.request.use(config=>{
    // console.log(config);
    // config.method = "post";
    config.url = "http://api.shangguigu.com"+config.url;
    return config;
},err=>{
  
});

然后后面的请求可以省掉前面的http://api.shangguigu.com
axios.get("/scoreList")
```

 **请求拦截,可以在此处更改请求配置信息**



### 响应拦截

```js
axios.interceptors.response.use(res=>{
    // res
    // console.log(2222,res);
    // 通过return 返回数据。返回的内容，即是本次请求得到的内容。
    return res.data;
},err=>{
  
})
```

**响应拦截:对返回数据进行二次处理（过滤数据。）**



### 拦截器函数/ajax请求/请求的回调函数的调用顺序

1. 说明: 调用axios()并不是立即发送ajax请求, 而是需要经历一个较长的流程

2. 流程: 请求拦截器2 => 请求拦截器1 => 发ajax请求 => 响应拦截器1 => 响应拦截器2 => 请求的回调

3. 注意: 此流程是通过promise串连起来的, 请求拦截器传递的是config, 响应拦截器传递的是response

```js
axios.interceptors.request.use(config=>{
        console.log("请求拦截1")
        // return config;
        throw "报错啦"
    },err=>{
        return Promise.reject(err);//官方写法，返回一个失败的promise
    })
    
    axios.interceptors.request.use(config=>{
        console.log("请求拦截2")
        return config;
    },err=>{
        console.log("请求拦截2异常",err)
        return Promise.reject(err);
    })
    
    axios.interceptors.response.use(res=>{
        console.log("响应拦截1")
        return res;
    },err=>{
        console.log("响应拦截1异常",err)
        return Promise.reject(err);
    })
    
    axios.interceptors.response.use(res=>{
        console.log("响应拦截2")
        return res;
    },err=>{
        console.log("响应拦截2异常",err)
        return Promise.reject(err);
    })



axios.get("http://127.0.0.1/one")
        .then(value => {
            console.log(value);
        }).catch(err => {
            console.log("请求失败", err)//返回失败的promise，进catch
        })

//请2 -》请1 -》响1异常 报错啦 -》响2异常 报错啦 -》"请求失败", 报错啦
```









## get请求

```js
axios.interceptors.request.use(config=>{
    config.url = "http://api.shangguigu.com"+config.url;
    return config;
});
// 响应拦截:对数据进行二次处理（过滤数据。）
axios.interceptors.response.use(res=>{
    // 通过return 返回数据。返回的内容，即是本次请求得到的内容。
    return res.data;
})
const btns = document.querySelectorAll("input");

btns[0].addEventListener("click",async function(){
    // 1、基本写法
    const result = axios.get("/scoreList")
    result.then(value=>{
         console.log(value.data);
     })

    // 通过解构赋值接收数据
     result.then(({data})=>{
         console.log(data);
    	})

    // 拦截器。请求拦截  响应的拦截。
     result.then(data=>{
        console.log(111,data);
     })

    // 2、连缀写法
     axios.get("/scoreList")
     .then(data=>{
         console.log(data);
     })

    // 3、async await
     const data = await axios.get("/scoreList");
     console.log(data);

    // 4、通过axios的配置 axios(config),直接书写配置
    const data = await axios({
        url:"/scoreList",// 请求地址
        // method:"post"// 请求方式，默认是get
        params:{// 传递的参数
            sex:"男"
        }
    })
    console.log(data);

});
```

### get传参(params:{})

params:{}里的内容会拼接在请求地址的后面，跟原生Ajax GET请求一样传参在地址后面拼接

传值 get第一个参数是地址，第二个参数是对象。也就是config配置对象，里面可以传参

```js
const data = await axios.get("/scoreList",{
    // 会将params转换为urlencoded
    params:{
        sex:"女",
        id:4
    },
  	header:{}
})
写法2
const data = await axios({
    url: "/scoreList",// 请求地址
    method:"post"// 请求方式，默认是get
    params: {// 传递的参数
        sex: "男"
    }
})
```



## post请求

**传参 data:{}**

```js
axios.interceptors.request.use(config => {
    config.url = "http://api.songwenxian.com:8090" + config.url;
    return config;
});
// 响应拦截:对数据进行二次处理（过滤数据。）
axios.interceptors.response.use(res => {
    return res.data;
})
const btns = document.querySelectorAll("input");

//写法1
btns[1].addEventListener("click", async function () {
    const data = await axios.post("/scoreList", {//直接传参
        userName: "口罩",
        price: 2
    })
    console.log(data);
})

//写法2
const data = await axios.post("/scoreList","userName=手机&age=12")
console.log(data);


//写法3
const data = await axios({
    url:"/scoreList",
    method:"post",
    data:{
        userName:"桃",
        price:2
    },
    headers:{
        "content-type":"application/json"//可以不写，会根据你的传参内容生成对应的
    }
})
console.log(data);
```



## delete请求

**delete 相当于 get**

```js

btns[2].addEventListener("click", async function () {
    // 删除ID为1的数据
     const result = await axios.delete("/scoreList/1");
     console.log(result);

// 删除ID为3的数据
     const result = await axios({
         method:"delete",
         url:"/scoreList/3"
     })
     console.log(result);

})    
```

## put请求

**传参 data:{}，全部更新**

```js
btns[3].addEventListener("click", async function () {
    const result = await axios.put("/scoreList/4",{
        userName:"赵四"
    })
    console.log(result);

    axios({
        url:"/scoreList/5",
        method:"put",
        data:{
            userName:"王五"
        }
    })
})
```



## patch请求

**传参 data:{}，局部更新**

```js

btns[4].addEventListener("click", async function () {
  //写法1
     const result = await axios.patch("/scoreList/8",{
         userName:"张学友"
     })
     console.log(result);
//写法2
    const result = await axios({
        method: "patch",
        url: "/scoreList/8",
        data: {
            userName: "刘德华"
        }
    })
    console.log(result);
})

```

## 默认全局配置

自定义设置一些全局配置，下面就不用每个都写了

```js
axios.defaults.baseURL = "http://127.0.0.1";// 基础路径
axios.defaults.timeout = 1000;// 指定超时时间
axios.defaults.headers = {
    "authorization":"nihaoya"
}
axios.defaults.method = 'post'
```



## axios.create(config)创建实例



1. 根据指定配置创建一个新的axios, 也就就每个新axios都有自己的配置

2. 新axios只是没有取消请求和批量发请求的方法, 其它所有语法都是一致的

3. 为什么要设计这个语法?

(1)   需求: 项目中有部分接口需要的配置与另一部分接口需要的配置不太一样, 如何处理

(2)   解决: 创建2个新axios, 每个都有自己特有的配置, 分别应用到不同要求的接口请求中

```js
//  通过 create 创建一个axios的实例 。
const server = axios.create({
    baseURL:"http://127.0.0.1",
})
const server8090 = axios.create({
    baseURL:"http://127.0.0.1:8090"
})
server.get("/one")
.then(value=>console.log(value))

server8090.get("/two")
.then(value=>console.log(value.data));
```



## 取消请求

1. 基本流程

-   配置cancelToken对象
-   缓存用于取消请求的cancel函数
-   在后面特定时机调用cancel函数取消请求
-   在错误回调中判断如果error是cancel, 做相应处理

2. 实现功能

-   点击按钮, 取消某个正在请求中的请求
-   在请求一个接口前, 取消前面一个未完成的请求



```js
<body>
<input type="button" value="发送请求">
<input type="button" value="取消请求">
</body>
<script>
    const btns = document.querySelectorAll("input");
    let cancelFn = null;// 取消函数
    btns[0].addEventListener("click",function () {
        if(cancelFn) cancelFn();  //在请求一个接口前, 取消前面一个未完成的请求
        axios({
            method:"get",
            url:"http://127.0.0.1/one",
            // canelToken
            cancelToken:new axios.CancelToken(c=>{
                cancelFn = c;// cancelFn
            })
        }).then(({data})=>{
            cancelFn = null;
            console.log(data)
        }).catch(err=>{
            console.log(err);
        })
    })
    btns[1].addEventListener("click",function () {
        cancelFn("我取消了axios");
    })
</script>
```

## 简易版axios封装

1,首先是个函数

2.传入一个参数对象config，为你调用时的config配置信息

3.返回一个promise

4.内部用的还是XMLHttpRequest()

```js
function axios(config) {
    // 将请求方式全部转为大写
    const method = (config.method || "get").toUpperCase();//要么是你配置的config.method，要么就是默认的get
    // 返回Promise
    return new Promise((resolve, reject) => {
        // 声明xhr
        const xhr = new XMLHttpRequest();
        // 定义一个onreadystatechange监听事件
        xhr.onreadystatechange = function () {
            // 数据全部加载完成
            if (xhr.readyState === 4) {
                // 判断状态码是否正确
                if (xhr.status >= 200 && xhr.status < 300) {
                    // 得到响应体的内容
                    const data = JSON.parse(xhr.responseText);
                    // 得到响应头
                    const headers = xhr.getAllResponseHeaders();
                    // request 即是 xhr
                    const request = xhr;
                    // 状态码
                    const status = xhr.status;
                    // 状态码的说明
                    const statusText = xhr.statusText
                    resolve({
                        config,
                        data,
                        headers,
                        request,
                        status,
                        statusText
                    });
                } else {
                    reject("请求失败" + xhr.status + xhr.statusText);
                }
            }
        }
        // http://127.0.0.1/two?a=1&b=2
        // 判断是否拥有params,且类型为object
        if (typeof config.params === "object") {
            // 将object 转为 urlencoded {a:1,b:2} a=1&b=2
            // ["a","b"]
            const arr = Object.keys(config.params);
            // ["a=1","b=2"]
            const arr2 = arr.map(v => v + "=" + config.params[v]);
            // a=1&b=2
            const url = arr2.join("&");
            // config.url = config.url + "?" + url;
            config.url += "?" + url;
        }
        xhr.open(method, config.url);
        // post put patch
        if (method === "POST" || method === "PUT" || method === "PATCH") {
            if (typeof config.data === "object"){ //这里是请求时传的参数
              xhr.setRequestHeader("content-type", "application/json");
            }
            else if (typeof config.data === "string"){
              xhr.setRequestHeader("content-type", "application/x-www-form-urlencoded");
            }
            xhr.send(JSON.stringify(config.data));
        } else {
            xhr.send();
        }

    })
}
```

## axios.request(config): 等同于axios(config)







# （老师笔记）AJAX

## 1原生AJAX

### 1.1 AJAX 简介

- AJAX 全称为Asynchronous Javascript And XML，就是异步的 JS 和 XML。

- 通过AJAX可以在浏览器中向服务器发送异步请求。

- AJAX 不是新的编程语言，而是一种使用现有标准的新方法。

### 1.2 XML简介

- XML 可扩展标记语言。
- XML 被设计用来传输和存储数据。
- XML和HTML类似，不同的是HTML中都是预定义标签，而XML中没有预定义标签，全都是自定义标签，用来表示一些数据。
- XML：保存和传输数据    HTML展示数据

### 1.3 AJAX的工作原理
- Ajax的工作原理相当于在用户和服务器之间加了一个中间层(Ajax引擎)，使用户操作与服务器响应异步化。

### 1.4 AJAX的特点

#### 1.4.1 AJAX的优点

- 可以无需刷新页面而与服务器端进行通信。

- 允许你根据用户事件来更新部分页面内容

#### 1.4.2 AJAX的缺点

- 没有浏览历史，不能回退

- 存在跨域问题

- SEO不友好

### 1.5 AJAX的使用

#### 1.5.1 核心对象

XMLHttpRequest，AJAX的所有操作都是通过该对象进行的。

#### 1.5.2 使用步骤

- 创建XMLHttpRequest对象`var xhr = new XMLHttpRequest();`

- 设置请求信息`xhr.open(method, url);`
  - 可以设置请求头，一般不设置
  - `xhr.setRequestHeader('Content-Type', 'application/x-www-form-urlencoded');`

- 发送请求

  -  `xhr.send(body)`  //get请求不传body参数，只有post请求使用

- 接收响应

  - `xhr.responseXML` 接收xml格式的响应数据

  - `xhr.responseText` 接收文本格式的响应数据

- 事件触发 `onreadystatechange`

  ```js
  xhr.onreadystatechange = function (){
  	if(xhr.readyState == 4 && xhr.status == 200){
  		var text = xhr.responseText;
  		console.log(text);
  	}
  }
  ```

  

#### 1.5.3 解决IE缓存问题

- 问题：在一些浏览器中(IE),由于缓存机制的存在，ajax只会发送的第一次请求，剩余多次请求不会在发送给浏览器而是直接加载缓存中的数据。

- 解决方式：浏览器的缓存是根据url地址来记录的，所以我们只需要修改url地址即可避免缓存问`xhr.open("get","/testAJAX?t="+Date.now());`

#### 1.5.4 AJAX请求状态

- xhr.readyState 可以用来查看请求当前的状态

- 0: 对应常量UNSENT，表示XMLHttpRequest实例已经生成，但是open()方法还没有被调用。

- 1: 对应常量OPENED，表示send()方法还没有被调用，仍然可以使用setRequestHeader()，设定HTTP请求的头信息。

- 2: 对应常量HEADERS_RECEIVED，表示send()方法已经执行，并且头信息和状态码已经收到。

- 3: 对应常量LOADING，表示正在接收服务器传来的body部分的数据，如果responseType属性是text或者空字符串，responseText就会包含已经收到的部分信息。

- 4: 对应常量DONE，表示服务器数据已经完全接收，或者本次接收已经失败了



```js
//服务器
const fs = require("fs");
const http = require("http");
http.createServer((req,res)=>{
    console.log(req.url);
    if(req.url==="/01.html"){//判断路径
        const rs = fs.createReadStream("./01.html");
        res.setHeader("Content-Type","text/html;charset=utf-8;")
        rs.pipe(res);
        return;
    }
    if(req.url==="/02.html"){
        const rs = fs.createReadStream("./02.html");
        res.setHeader("Content-Type","text/html;charset=utf-8;")
        rs.pipe(res);
        return;
    }
    if(req.url==="/jquery.html"){
        const rs = fs.createReadStream("./jquery.html");
        res.setHeader("Content-Type","text/html;charset=utf-8;")
        rs.pipe(res);
        return;
    }

    if(req.url==="/axios.html"){
        const rs = fs.createReadStream("./axios.html");
        res.setHeader("Content-Type","text/html;charset=utf-8;")
        rs.pipe(res);
        return;
    }

    console.log(req.method)//GET //POST
    if(req.method === 'GET'){//get方式通过查询字符串拿到内容
        console.log(req.url.split("?")[1]);
        res.end("success111~"); 
    }

    if(req.method === 'POST'){//get方式通过查询字符串拿到内容
        req.on("data",(chunk)=>{
            console.log(chunk.toString());
        })
        res.end("post success")

    }
   
})
.listen(3000,"127.0.0.1",(err)=>{
})
```



```html
<!-- 基础ajax请求--get -->
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <button id="btn">按钮</button>
    <script>
        const oBtn = document.getElementById("btn");
        oBtn.onclick = function () {
            const xhr = new XMLHttpRequest();
            xhr.open("get", "http://127.0.0.1:5501?user=lily&sex=nv&t="+Date.now(), true)
            xhr.setRequestHeader("Content-Type", "x-www-form-urlencoded");
            xhr.send();
            xhr.onreadystatechange = function () {
                console.log(xhr.readyState);
                /*
                    0:xhr刚刚创建
                    1:设置了请求信息，但是还没有发送请求
                    2:已经发送了请求，并得到部分响应结果（响应首行，响应头）
                    3:接受到部分响应体数据（如果数据较小，就全部接受了）
                    4:全部接受完成
                */
                
                if (xhr.readyState == 4 && xhr.status >= 200 && xhr.status <= 300) {
                    //成功响应
                    console.log(xhr.responseText);
                    console.log("hhhhh")
                }
            }
            console.log("全部代码执行了")
        }
    </script>
</body>

</html>
```





```html
<!-- 基础ajax请求--post -->
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <button id="btn">按钮</button>
    <script>
        const oBtn = document.getElementById("btn");
        oBtn.onclick = function () {
            const xhr = new XMLHttpRequest();
            xhr.open("post", "http://127.0.0.1:5501")

            // xhr.setRequestHeader("Content-Type", "x-www-form-urlencoded");
            // xhr.send("user=lily&pass=12345");

            xhr.setRequestHeader("Content-Type", "application/json");
            xhr.send('{"user":"lily","pass":"12345"}');
            xhr.onreadystatechange = function () {
                console.log(xhr.readyState);
                if (xhr.readyState == 4 && xhr.status >= 200 && xhr.status <= 300) {
                    //成功响应
                    console.log(xhr.responseText);
                    console.log("hhhhh")
                }
            }
            console.log("全部代码执行了")
        }
    </script>
</body>

</html>
```



```html
<!-- jquery请求 -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <button id="btn">按钮</button>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.js"></script>
    <script>
        $("#btn").click(()=>{
            /* $.ajax({
                url:"http:127.0.0.1:5001",
                method:"post",
                data:JSON.stringify({
                    name:"lily",
                    age:"12"
                }),
                headers:{
                    "Content-type":"application/json"
                },
                success(data){
                    console.log(data);
                },
                error(err){
                    console.log(err);
                }
            }) */

            /* $.get("http:127.0.0.1:5001",{name:"lucy",age:18},(data)=>{
                console.log(data)
            }) */
            $.post("http:127.0.0.1:5001",{name:"lucy",age:18},(data)=>{
                console.log(data)
            })
        })
    </script>
</body>
</html>
```



```html
<!-- axios请求 -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <button id="btn">按钮</button>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.js"></script>
    <script src="https://cdn.bootcdn.net/ajax/libs/axios/0.20.0/axios.js"></script>
    <script>
        $("#btn").click(()=>{
            /* const promise = axios({
                url:"http:127.0.0.1:5001",
                method:"get",
                params:{
                    name:"lucy",
                    age:12
                }
            }) */

            /* const promise = axios({
                url:"http:127.0.0.1:5001",
                method:"post",
                data:{
                    name:"lucy",
                    age:12
                }
            }) */

            /* const promise = axios.get("http:127.0.0.1:5001",{
                params:{name:"lucy",age:14}
            }) */

            const promise = axios.post("http:127.0.0.1:5001",{name:"lucy",age:14}
            ) 

            promise.then((result)=>{
                console.log(result)
            }).catch((err)=>{
                console.log(err);
            })
        })
    </script>
</body>
</html>
```





# 2 跨域

## 2.1 同源策略

同源策略(Same-Origin Policy)最早由 Netscape 公司提出，是浏览器的一种安全策略。

同源： 协议、域名、端口号 必须完全相同。

违背同源策略就是跨域。

## 2.2 如何解决跨域

### 2.2.1 JSONP

- JSONP是什么
  - JSONP(JSON with Padding)，是一个非官方的跨域解决方案，纯粹凭借程序员的聪明才智开发出来，只支持get请求。

-  JSONP怎么工作的？
  - 在网页有一些标签天生具有跨域能力，比如：img link iframe script。
  - JSONP就是利用script标签的跨域能力来发送请求的。

- JSONP的使用

  - 动态的创建一个script标签
    - var script = document.createElement("script");
  - 设置script的src，设置回调函数
    - script.src = "http://localhost:3000/testAJAX?callback=abc";

  - 将script添加到body中
  - 服务器中路由的处理

```html
<!-- jsonp html -->
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <button id="btn"></button>
    <script src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.js"></script>
    <script>
        function callback(data) {
            console.log(data);
        }
        $("#btn").click(() => {
            const newScript = document.createElement("script");

            newScript.src = "http://127.0.0.1:3000?callback=callback";

            document.body.appendChild(newScript);
        })
    </script>
</body>

</html>
```



```js
//jsonp
if(req.method === 'GET'){//get方式通过查询字符串拿到内容

        const {callback} = req.url.split("?")[1].split("&").reduce((p,c)=>{
            const [key,value] = c.split("=");
            p[key]=value;
            return p;
        },{});
        console.log(callback);

        const person = {
            name:"lily",
            age:"19"
        }

        res.setHeader("Content-Type","application/javascript;charset=utf-8")
        res.end(`${callback}(${JSON.stringify(person)})`)
    }

```





### 2.3.2 CORS

- CORS是什么？
  - CORS（Cross-Origin Resource Sharing），跨域资源共享。CORS是官方的跨域解决方案，它的特点是不需要在客户端做任何特殊的操作，完全在服务器中进行处理，支持get和post请求。

- CORS怎么工作的？
  - CORS是通过设置一个响应头来告诉浏览器，该请求允许跨域，浏览器收到该响应以后就会对响应放行。

- CORS的使用
  - 主要是服务器端的设置



```html
<!-- html cors-->
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>

<body>
    <button id="btn">按钮</button>
    <script>
        const oBtn = document.getElementById("btn");
        oBtn.onclick = function () {
            const xhr = new XMLHttpRequest();
            xhr.open("get", "http://127.0.0.1:3000/cors",true)
            xhr.setRequestHeader("aaa","111")
            xhr.send();
            
            xhr.onreadystatechange = function () {
                console.log(xhr.readyState);
                if (xhr.readyState == 4 && xhr.status >= 200 && xhr.status <= 300) {
                    //成功响应
                    console.log(xhr.responseText);
                    console.log("hhhhh")
                }
            }
            console.log("全部代码执行了")
        }
    </script>
</body>

</html>
```



```js
//cors
const fs = require("fs");
const http = require("http");
http.createServer((req,res)=>{
    console.log(req.url);
    if(req.url==="/cors"){
        const safeUrl = [
            'http://127.0.0.1:5501',
            'http://127.0.0.1:5502',
            'http://127.0.0.1:5503',
            'http://127.0.0.1:5504',
        ]
        console.log(req.headers.origin)
        const origin = req.headers.origin;
        const resultUrl = safeUrl.find((url)=>{
            return url === origin
        })
        if(resultUrl){
            res.setHeader("Access-Control-Allow-Origin",resultUrl)
            res.setHeader("Access-Control-Allow-Headers","aaa")
            // OPTIONS:预检请求当前请求是否可以跨域
            res.setHeader("Access-Control-Allow-Methods","GET,POST,PUT,DELETE,OPTIONS")
            res.setHeader("Access-Control-Max-Age",0)
        }
        const person = {
            name:"lily",
            age:"19"
        } 
        res.setHeader("Access-Control-Allow-Origin","http://127.0.0.1:5501")
        res.setHeader("Content-Type","application/json;charset=utf-8")
        
        res.end(JSON.stringify(person)) 
    }
   
})
.listen(3000,"127.0.0.1",(err)=>{
})
```



 