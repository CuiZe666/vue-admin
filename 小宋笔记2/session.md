# puppeteer

## 什么是puppeteer

- Puppeteer 是一个node库，他提供了一组用来操纵Chrome的API, 通俗来说就是一个 headless chrome浏览器
- 既然是浏览器，那么我们手工可以在浏览器上做的事情 Puppeteer 都能胜任

## 作用

- 生成网页截图或者 PDF
- 高级爬虫，可以爬取大量异步渲染内容的网页
- 模拟键盘输入、表单自动提交、登录网页等，实现 UI 自动化测试
- 捕获站点的时间线，以便追踪你的网站，帮助分析网站性能问题

## 官网用法

```javascript
const puppeteer = require('puppeteer');

(async () => {
  //先通过 puppeteer.launch() 创建一个浏览器实例 Browser 对象
  const browser = await puppeteer.launch();
  //然后通过 Browser 对象创建页面 Page 对象
  const page = await browser.newPage();
  //然后 page.goto() 跳转到指定的页面
  await page.goto('https://example.com');
  //调用 page.screenshot() 对页面进行截图
  await page.screenshot({path: 'example.png'});
  //关闭浏览器
  await browser.close();
})();
```



### 实战

```js
const puppeteer = require('puppeteer');

(async () => {
  // 1. 打开浏览器
  const browser = await puppeteer.launch({
    headless: false, // 有头模式
  });
  // 2. 新建一个标签页
  const page = await browser.newPage();
  // 3. 输入网址
  await page.goto('http://search.dangdang.com/?key=%BC%C6%CB%E3%BB%FA&act=input', {
    waitUntil: 'load' 
  });
  // 4. 抓取页面数据
  const books = await page.evaluate(() => {
    // 当前函数会在页面中调用
    const $lis = $('.bigimg>li');

    const data = [];

    for (let i = 0; i < $lis.length; i++) {
      const title = $lis.eq(i).find(".pic").attr("title");
      const img = $lis.eq(i).find(".pic>img").attr('src');
      const price = $lis.eq(i).find(".search_now_price").text();
      const detail = $lis.eq(i).find(".detail").text();
      const url = $lis.eq(i).find(".name>a").attr("href");
      
      data.push({
        title, // 详情页
        img, // 海报
        price, // 标题
        detail, // 评分
        url
      })
    }

    return data;
  });

  console.log(books);

  // 再去抓取其他数据
  for (let i = 0; i < books.length; i++) {
    const book = books[i];
    await page.goto(book.url, {
      waitUntil: 'load' 
    })
    const desc = await page.evaluate(() => {
      // 书籍介绍
      
    })
    book.desc = desc;
  }

  // console.log(movies);

  // 5. 关闭浏览器
  await browser.close();
})();
```





# cookie

## 什么是cookie

由于HTTP协议是无状态的，而服务器端的业务必须是要有状态的。Cookie诞生的最初目的是为了存储web中的状态信息，以方便服务器端使用。比如判断用户是否是第一次访问网站。

## cookie的流程

- 服务器向客户端发送cookie
- 浏览器将cookie保存
- 之后每次http请求浏览器都会将cookie发送给服务器端

## 服务器端处理cookie

服务器端像客户端发送Cookie是通过HTTP响应报文实现的，在Set-Cookie中设置需要向客户端发送的cookie

```js
//- 创建
//max-age=3600是浏览器cookie保存时间（如果不设置则是会话cookie）
res.setHeader("Set-Cookie", "user=jack;max-age=3600;httpOnly=true");
//- 获取
req.headers.cookie
//- 删除
res.setHeader("Set-Cookie", "user=;max-age=0")
```

## 客户端处理cookie

- 客户端获取 `document.cookie`

- 客户端设置 `document.cookie = 'name=rose;max-age=3600'`

## cookie的缺点

- cookie的数量(20-50不等)和长度(不能超过4kb)都有限制

- 潜在的安全风险：cookie可能被截取篡改，如果cookie被拦截，就可能会获取到所有的Session信息
- 用户配置为禁用，有的用户禁用了浏览器或者客户端设备接受cookie的能力，因此限制了这一功能
- 每次都会随着服务器端发送



## cookie练习

```js
const http = require("http");
const server = http.createServer((req, res) => {
  res.setHeader("Set-Cookie", "user=jack;max-age=3600;httpOnly=true");

  // 获取cookie
  // console.log(req.headers.cookie); // user=jack; name=rose
  // 将cookie解析成对象
  // ['user=jack', 'name=rose'] --> {user: 'jack', name: 'rose'}
  // const cookies = req.headers.cookie.split("; ").reduce((p, c) => {
  //   const [key, value] = c.split("=");
  //   p[key] = value;
  //   return p;
  // }, {});
  // console.log(cookies);

  // 删除cookie
  // res.setHeader("Set-Cookie", "user=;max-age=0");

  res.end("success");
});

server.listen(3000, "localhost", (err) => {
  if (err) {
    console.log("服务器启动失败", err);
    return;
  }
  console.log("服务器启动成功  http://localhost:9527");
});

```



# session

## 什么是session

- session是另一种记录客户状态的机制，cookie保存在客户端，session保存在服务器
- session运行在服务器，客户端第一次访问服务器时，就可以将客户的登录信息保存。当客户访问其他界面时，可以判断客户的登录状态，做出提示，相当于登录拦截。
- 当浏览器向服务器发送请求时，会在服务器建立一个session对象，相当于一个key，value的键值对，然后将key(cookie)f返回到客户端，当浏览器再次访问时，携带key(cookie)找到对应的session(value),客户的信息都存在session中。

## session的特点

- 本质上是存储在服务器对象
- 大小和数量理论上没有限制
- 传输流量小（只有一个cookie）
- 安全性较高

## session的使用

```js
const http = require("http");
// 创建session对象
const session = {};
/**
 * 生成随机id的方法
 */
function uniqueId() {
  // substring去掉.
  const randomNum = Number(Math.random().toString().substring(2));
  // .toString(32) 转换为32进制
  return (randomNum + Date.now()).toString(32);
}

/*
  如果访问 /login 代表要存储用户信息
  如果访问 /user 代表要获取用户信息
*/
const server = http.createServer((req, res) => {
  // console.log(req.url); // /?name=jack&age=18
  // name=jack&age=18
  const url = req.url;
  console.log(url)
  if (url === "/favicon.ico") return;

  if (url.startsWith("/login")) {
    // 获取查询字符串参数并解析成对象
    const qs = url.split("?")[1];
    // ['name=jack', 'age=18']
    const arr = qs.split("&");
    const query = arr.reduce((p, item) => {
      const [key, value] = item.split("=");
      p[key] = value;
      return p;
    }, {});

    // console.log(query); // { name: 'jack', age: '18' }
    // 创建session_id
    const sessionId = uniqueId();
    // 将用户数据存到session对象中
    session[sessionId] = query;
    // 返回一个携带session_id的cookie给用户
    res.setHeader(
      "Set-Cookie",
      `sessionId=${sessionId};max-age=3600;httpOnly=true`
    );

    res.end("success");

    return;
  }

  if (url === "/user") {
    // 获取请求头cookie并解析成对象
    const cookies = req.headers.cookie.split("; ").reduce((p, c) => {
      const [key, value] = c.split("=");
      p[key] = value;
      return p;
    }, {});
    // 找到sessionId
    const { sessionId } = cookies;
    // 找到用户数据
    const user = session[sessionId];

    if (user) {
      // 说明用户上次访问过/login
      res.setHeader("Content-Type", "application/json");
      res.end(JSON.stringify(user));
      return;
    }

    res.setHeader("Content-Type", "text/plain;charset=utf8");
    res.end("暂无数据~");
  }
});

server.listen(9527, "localhost", (err) => {
  if (err) {
    console.log("服务器启动失败", err);
    return;
  }
  console.log("服务器启动成功  http://localhost:9527");
});

```



# WebStorage

## 什么是WebStorage

- 在HTML5中，新加入了一个WebStorage特性，这个特性主要是用来作为本地存储来使用的，解决了cookie存储空间不足的问题
- WebStorage中一般浏览器支持的是5M大小，这个在不同的浏览器中localStorage会有所不同。
- WebStorage分为两种：localStorage 永久存储、sessionStorage 会话（临时）存储
- localStorage只支持string类型的存储
- 其他存储：indexDB，webSQL ，兼容性的库 localForage



## WebStorage方法

- xxxStorage.setItem(key, value) 存储数据的方法
- xxxStorage.getItem(key) 读取数据的方法
- xxxStorage.removeItem(key) 删除数据的方法
- xxxStorage.clear() 清空所有数据的方法



## WebStorage事件

- storage事件：用来跨页面通信
  - 当别的页面存储东西发生变化时触发的事件
  - 只能由localStorage触发



# WebSocket

## 什么是WebSocket

- 它是一种网络通信协议，是 `HTML5` 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议。
- 因为 HTTP 协议有一个缺陷：通信只能由客户端发起
- 我们都知道轮询的效率低，非常浪费资源（因为必须不停连接，或者 HTTP 连接始终打开）, 因此websocket应运而生。
- WebSocket用于在Web浏览器和服务器之间进行任意的双向数据传输的一种技术。WebSocket协议基于TCP协议实现，包含初始的握手过程，以及后续的多次数据帧双向传输过程。其目的是在WebSocket应用和WebSocket服务器进行频繁双向通信时，可以使服务器避免打开多个HTTP连接进行工作来节约资源，提高了工作效率和资源利用率。
- WebSocket目前支持两种统一资源标志符`ws`和`wss`，类似于HTTP和HTTPS
- 浏览器发出webSocket的连线请求，服务器发出响应，这个过程称为`握手`,握手的过程只需要一次，就可以实现持久连接。

## Socket.io

Socket.io是一个`webSocket`库，目标是构建不同浏览器和移动设备上使用的实时应用。它会自动根据浏览器从`webSocket` `ajax长轮询` `ifrane流`等各种方式选择最佳的方式。



## 聊天室的实现

```js
//服务端
const fs = require("fs");
const path = require("path");

// 创建http server
const server = require("http").createServer((req, res) => {
  if (req.url === "/socket.io.js") {
    const filePath = path.resolve(__dirname, "./public/socket.io.js");
    const rs = fs.createReadStream(filePath);
    res.setHeader("Content-Type", "application/javascript;charset=utf8");
    rs.pipe(res);
    return;
  }
  // 响应页面
  const filePath = path.resolve(__dirname, "./public/chat.html");
  const rs = fs.createReadStream(filePath);
  res.setHeader("Content-Type", "text/html;charset=utf8");
  rs.pipe(res);
});

// 创建WebSocket服务
const io = require("socket.io")(server);

// io可以代表所有客户端连接对象
io.on("connect", (socket) => {
  // socket代表当前连接上服务的客户端对象
  socket.on("client_to_server", function (data) {
    console.log(data);
    // 除它以外，通知其他客户端
    socket.broadcast.emit("server_to_client", data);
  });
});

// 启动服务
server.listen(3000, 'localhost');

```



```js
//客户端
const btn = document.querySelector(".chat-btn");
const content = document.querySelector("textarea");
const input = document.querySelector("#chat-name>input");
const chatContent = document.querySelector(".chat-content");

// 客户端连接上服务端
const socket = io("ws://192.168.10.2:3000");
// socket代表当前连接客户端对象

btn.onclick = function () {
    // 获取当前用户名
    const username = input.value;

    if (!username) {
        alert("请输入用户名！");
        return;
    }

    // 获取用户输入的消息
    const msg = content.value;

    if (!msg) {
        alert("请输入聊天内容！");
        return;
    }

    const time = Date.now();
    // 客户端向服务端发送消息
    socket.emit("client_to_server", {
        username,
        msg,
        time,
    });

    // 显示在你的聊天内容
    const div = document.createElement("div");

    div.innerHTML = `
        <div class="title right-title">${username}  
        ${new Date(time).toLocaleString()}
        </div>
        <div class="right-content">${msg}</div>`;

    chatContent.appendChild(div);

    // 聊天内容清空
    content.value = "";
};

socket.on("server_to_client", function (data) {
    const { username, msg, time } = data;

    // 显示别人的聊天内容
    const div = document.createElement("div");

    div.innerHTML = `
        <div class="title left-title">${username}  
        ${new Date(time).toLocaleString()}
        </div>
        <div class="left-content">${msg}</div>`;

    chatContent.appendChild(div);
});

```

