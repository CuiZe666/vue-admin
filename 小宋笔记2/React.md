# React

快捷模板rcc

## 1、本地引入react文件

![image-20201016202339361](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201016202339361.png)



## 2、react 创建虚拟 DOM 对象的两种方式

```js
// js
const vDom1 = React.createElement('h1(标签)',  {id:'myTitle'(属性，可多写)},  title(文本内容))
// 2. 将虚拟Dom对象渲染到页面指定容器中(root2为body里的一个div容器)
ReactDOM.render(vDom1, document.getElementById("root2"));
```

```js
// jsx
// 1. 创建虚拟DOM对象
const vDom = <h1>hello react</h1>;
// 2. 将虚拟DOM对象添加到页面指定容器中生效
ReactDOM.render(vDom, document.getElementById("root1"));
```

**案列：react遍历数据展示**

```js
// 3.react遍历数据展示
const data = ['标签一', '标签二', '标签三']
const result = data.map((item, index) => {
    return <li key={index}>{item}</li>
})

const vDom4 = <ul>
    {result}
</ul>
ReactDOM.render(vDom4, document.getElementById("root3"))
```



## 3、谈谈 jsx 语法

1. ​	react定义的一种类似于XML的JS扩展语法: XML+JS
2. ​	作用: 用来创建react虚拟DOM(元素)对象
3. ​	标签名与标签属性任意: HTML标签或其它标签
4. ​	babel.js的作用
   1. 浏览器不能直接解析JSX代码, 	需要babel转译为纯JS的代码才能运行
   2. 只要用了JSX，都要加上type="text/babel", 	声明需要babel来处理
5. ​	基本语法规则
   1. 遇到 	<开头的代码, 	以标签的语法解析: 	html同名标签转换为html同名元素, 	如果不是，会当做组件解析
   2. 遇到以 	{ 	开头的代码，以JS语法解析: 	标签中的js代码必须用{ 	}包含

![image-20201016202530825](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201016202530825.png)



## 3、创建组件的两种方式

```js
// 定义组件的两种方式
// 1. 工厂函数组件（定义简单组件）
function MyComponent1() {
  console.log("MyComponent1", this); // this指向window，严格模式（babel）下为undefined
  // 返回值就是组件要渲染的内容
  return (
    <div>
      <h1>工厂函数组件（定义简单的组件）</h1>
      <p>hello</p>
    </div>
  );
}
// 将组件渲染到页面指定容	器中
ReactDOM.render(<MyComponent1 />, document.getElementById("root1"));
```

```js
// 2. ES6类组件（定义复杂组件）
class MyComponent2 extends React.Component {
  render() {
    console.log("MyComponent2", this);
    // 返回值，组件要渲染的内容
    return <h1>ES6类组件（定义复杂组件）</h1>;
  }
}
ReactDOM.render(<MyComponent2 />, document.getElementById("root2"));
```

**在什么情况下你会优先选择使用 Class Component 而不是 Functional Component？**
在组件需要包含内部状态或者使用到生命周期函数的时候使用 Class Component ，否则使用函数式组件。

## 4、创建组件三个注意事项

1. 组件名首字母必须大写

​      jsx语法：如果标签名是小写，会当做html元素解析，解析不了就会报错

​      如果标签名是大写，会当做组件解析

​     2. 组件要渲染虚拟DOM对象，必须有一个根标签

​     3. **使用标签必须是闭合标签**

​        看标签内部是否有内容，如果有就是双标签，没有就是单标签

## 5、组件的三大属性

### 组件的 state 用法

1)     state是组件对象最重要的属性, 值是对象(可以包含多个数据)

2)     组件被称为"状态机", 通过更新组件的state来更新对应的页面显示(重新渲染组件)

那个组件需要state数据展示，state就写在哪个组件里，判断需不需要state？界面更新渲染了就需要state

```js
// 简写 初始化
  state = { isLikeMe: true }
// 读取值
 const {isLikeMe} = this.state
// 修改值
this.setState({ isLikeMe: !isLikeMe })  // this指向组件实例对象  setState应该是原型上的方法

//this.setState修改状态数据值，值发现了改变就会调用render更新渲染界面   
//state(状态) 更新可能是异步的，异步多次更新会合并为一次，合并为最后一次（相同的状态数据改变会覆盖，不同的会增加）
// 不建议直接修改原数据，建议产生一份全新数据

this.setState既可以表现为同步也可以表现为异步
在定时器中，原生Dom事件中表现是同步的，同步更新，多次更新都会生效
在生命周期函数中，react合成事件是异步的，异步更新，多次更新会合并为一次


在async函数中，如果加了await, setState更新就是同步的，没有就是异步的
一般情况下async和await一起使用，简单理解就是async函数中更新也是同步的
await this.setState() 这样就是同步了
为了避免同步异步问题：
在一个函数中，setState建议只调用一次（不管同步异步都不会影响功能）  


```

![image-20201016204229080](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201016204229080.png)



***修正：上述图中的state属性和handleClick方法是属于实例对象直接属性(私有的)，因为是箭头函数，如果是普通函数则是原型上的方法***

### 组件的 props 用法

**理解：**

**1)     每个组件实例对象都会有props(properties的简写)属性（this.props）**

**2)     组件标签的所有属性都保存在props中**

**作用：**

**1)     通过标签属性从组件外向组件内传递变化的数据**

**2)     注意: 组件内部不要修改props数据**

![image-20201018220300263](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201018220300263.png)

![image-20201018220309024](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201018220309024.png)

**内部读取某个传递过来的属性值**

​    ***this.props.propertyName***

 **对props中的属性值进行类型限制和必要性限制/属性默认值**

![image-20201017105309135](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201017105309135.png)

**constructor里的super**

![image-20201018231622854](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201018231622854.png)

### 组件的 ref 用法

1)     组件内的标签都可以定义ref属性来标识自己

a.     <input type="text" ref={this.createRef}/> (推荐使用) 

  **给实例对象添加createRef属性的写法，通过createRef里的current访问到元素**

b.     <input type="text" ref={input => this.funcRef = input}/> 

**界面渲染就把元素传到回调里，然后回调里再把元素挂在到实例对象的funcRef 属性上，后面通过该属性访问元素**

c.     <input type="text" ref="stringRef" /> (即将废弃)

d.     回调函数在组件初始化渲染完或卸载时自动调用

2)     在组件中可以通过this.createRef.current / this.funcRef / this.stringRef来得到对应的真实DOM元素

3)     作用: 通过ref获取组件内容特定标签对象, 进行读取其相关数据

![image-20201018230808289](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201018230808289.png)



![image-20201018230815749](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201018230815749.png)

### this.setState方法

**this指向组件实例对象  setState应该是原型上的方法**

**this.setState修改状态数据值，值发生了改变就会调用render更新渲染界面** 

**this.setState是同步还是异步呢？**

**this.setState既可以表现为同步也可以表现为异步**
	**在定时器中，原生Dom事件中表现是同步的，同步更新，多次更新都会生效**

```js
// 如何设置原生的DOM事件
btnRef = React.createRef();
<button ref={this.btnRef}> 按钮 </button>
this.btnRef.current.onclick = () => {
const { num } = this.state;
this.setState({
  num: num + 2,
});
```

​	**在生命周期函数中，react合成事件是异步的，异步更新，多次更新会合并为一次，合并为最后一次（相同属性的状态数据改变会覆盖，不同属性的状态数据改变则会合并过去生效）**

​		**在async函数中，如果加了await, setState更新就是同步的，没有就是异步的;一般情况下async和await一起使用，简单理解就是async函数中更新也是同步的**


```js
await this.setState() 这样就是同步了
```

**为了避免同步异步问题：**
		**在一个函数中，setState建议只调用一次（不管同步异步都不会影响功能）**  



**不建议修改原数据，建议产生一份新数据**

```
 this.setState({
      todos: [{ id: Date.now(), content, isChecked: false }, ...todos],
 });
 
this.setState({
  todos: todos.map((todo) => {
    return {
      ...todo,
      isChecked: check,
    };
  }),
});

this.setState({
  todos: todos.filter((todo) => todo.id !== id),
});


```







## 6.react应用(基于react脚手架)

 **创建项目并且启动**

**1 CMD全局安装react项目的脚手架库: create-react-app**

**2 VScode终端下载react项目的脚手架，然后启动**

```js
npm install -g create-react-app
create-react-app hello-react(项目名字自己起的)
cd hello-react
npm start运行

```



## 事件

### React合成事件和原生事件区别

React合成事件一套机制：React并不是将click事件直接绑定在dom上面，而是采用**事件冒泡**的形式冒泡到document上面，然后React将事件封装给正式的函数处理运行和处理。

**1)     通过event.target得到发生事件的DOM元素对象**

**给标签元素绑定事件，如果事件的回调函数需要传入参数的话，那么事件回调函数内部需要return返回一个函数**

![image-20201018214743962](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201018214743962.png)

![image-20201018215137059](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201018215137059.png)

### 收集表单数据

![image-20201018215554278](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201018215554278.png)

![image-20201018215603849](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201018215603849.png)



**收集表单数据（类似v-model）onChange事件和value属性结合**

![image-20201019200718852](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201019200718852.png)

![image-20201019200820604](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201019200820604.png)

![image-20201019200936440](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201019200936440.png)

## 组件的生命周期函数：

**在初始化时会触发5个钩子函数(初始化会触发render,更新也会触发render)**

1、getDefaultProps() 忽略

> 设置默认的props，也可以用dufaultProps设置组件的默认属性。

2、getInitialState() 忽略

> 在使用es6的class语法时是没有这个钩子函数的，可以直接在constructor中定义this.state。此时可以访问this.props。

3、componentWillMount()被取代

> 组件初始化时只调用，以后组件更新不调用，整个生命周期只调用一次，此时可以修改state。

4、 render()

> react最重要的步骤，创建虚拟dom，进行diff算法，更新dom树都在此进行。此时就不能更改state了。

**5、componentDidMount() 重要 发送Ajax请求、绑定事件、设置定时器等一次性任务**

> 组件渲染之后调用，可以通过this.getDOMNode()获取和操作dom节点，只调用一次。

**在更新时也会触发5个钩子函数：**

6、componentWillReceivePorps(nextProps)被取代

> 组件初始化时不调用，组件接受新的props时调用。

7、**shouldComponentUpdate(nextProps, nextState)**

> react性能优化非常重要的一环。组件接受新的state或者props时调用，我们可以设置在此对比前后两个props和state是否相同，如果相同则返回false阻止更新，因为相同的属性状态一定会生成相同的dom树，这样就不需要创造新的dom树和旧的dom树进行diff算法对比，节省大量性能，尤其是在dom结构复杂的时候。不过调用this.forceUpdate会跳过此步骤。
>
> 决定组件要不要更新？看数据（state，props）是否发生变化， 只要state，props有一个发生变化，就要更新

8、componentWillUpdate(nextProps, nextState)

> 组件初始化时不调用，只有在组件将要更新时才调用，此时可以修改state

9、render()

> 不多说

10、componentDidUpdate()

> 组件初始化时不调用，组件更新完成后调用，此时可以获取dom节点。

还有一个卸载钩子函数

**11、componentWillUnmount() 重要，做一些收尾工作，如清理定时器**

> 组件将要卸载时调用，一些事件监听和定时器需要在此时清除。

以上可以看出来react总共有10个周期函数（render重复一次），这个10个函数可以满足我们所有对组件操作的需求，利用的好可以提高开发效率和组件性能。

**12、static getDerivedStateFromProps()重要，**

> 这个生命周期函数用来取代 UNSAFE_componentWillMount  UNSAFE_componentWillReceiveProps，所以 UNSAFE_componentWillMount 不会生效

### unmountComponentAtNode（需要引入react-dom）

unmountComponentAtNode用于执行卸载组件的操作，执行在componentWillUnmount之前。

```js
goDie = () => {
  // 卸载组件的操作
  ReactDOM.unmountComponentAtNode(document.getElementById("root"));
};
j
<button onClick={this.goDie}>不活了</button>
```

## 受控组件

**受控（Control）组件：通过state和onChange事件来收集表单数据的组件**

**Controlled Component(受控组件) 与 Uncontrolled Component (非受控组件)之间的区别是什么？**
受控组件（Controlled Component）代指那些交由 React 控制并且所有的表单数据统一存放的组件；非受控组件（Uncontrolled Component）则是由DOM存放表单数据，并非存放在 React 组件中。



## React 脚手架中ajax请求跨域解决

在package.json文件中添加一条如下代码

```js
单个地址  
"proxy": "http://localhost:9527" //目标服务器的地址
将你当前的地址代理到http://localhost:9527，由于当前的地址经常会变，不能写死，所以baseURL为'/'

然后你页面中的请求fetch('/api/userdata/')就会转发到proxy中的地址

多个地址
 "proxy": {
    "/data": {
      "target": "http://localhost",
      "changeOrigin": true
    },
    "/rest": {
      "target": "http://localhost/rest",
      "changeOrigin": true
    }
  }

create-react-app 的版本低于 2.0 的时候可以在 package.json 增加 proxy 配置， proxy可以是object类型，
create-react-app 的版本高于 2.0 版本的时候在 package.json 只能配置 string 类型.

作者：知足常乐晨
链接：https://www.jianshu.com/p/4a7f26adbedf
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

## React组件通信

1 prop 父——————》子

2 pubsub  任意

3 自定义事件    子——————》父

4 redux

5 context



# React Router(路由)

### 前端路由的原理

就是禁止a标签默认行为，再去内部通过history进行跳转

1)     history库

a.     网址: https://github.com/ReactTraining/history

b.     管理浏览器会话历史(history)的工具库

c.     包装的是原生BOM中window.history和window.location.hash

2)     history API

a.     History.createBrowserHistory(): 得到封装window.history的管理对象

b.     History.createHashHistory(): 得到封装window.location.hash的管理对象

c.     history.push(): 添加一个新的历史记录

d.     history.replace(): 用一个新的历史记录替换当前的记录

e.     history.goBack(): 回退到上一个历史记录

f.     history.goForword(): 前进到下一个历史记录

g.     history.listen(function(location){}): 监视历史记录的变化



## 路由的相关API

npm install react-router-dom下载

React里面将路由功能封装成了组件如下

1)     <BrowserRouter>

2)     <HashRouter>

3)     <Route> 类似于router-view(视图显示)，有几个组件就写几个

```js
<Route path="/home/news" component={News}></Route>
// 是不是这个地址，然后加载对应的组件显示
// exact是Route下的一个属性，react路由会匹配到所有能匹配到的路由组件，exact能够使得路由的匹配更严格一些。
//'/'根路径能匹配所有的路径，所以要加exact属性严格匹配
```

4)     <Redirect>

```js
<Redirect to="/home/news" />
// 重定向 能匹配所有路径,一般写在Route后面，写在前面的话后面的Route就不会显示了，前面的Route地址都匹配不到的话，就匹配Redirect地址
```

5)     <Link> 

```js
<Link to="/home/news">News</Link>
// 就是router-link，编程式路由跳转，不带样式，点击浏览器路径会变
// 也有exact属性
```

6)     <NavLink>

```js
<NavLink to="/home/news">News</NavLink>
// 就是router-link，编程式路由跳转，带选中样式
// 也有exact属性
```

7)     <Switch>

```js
<Switch>
  <Route path="/home/news" component={News}></Route>
  <Route path="/home/message" component={Message}></Route>
  <Redirect to="/home/news" />
</Switch>

//  Switch 切换路由组件显示：只让其中一个路由组件生效,两个地址一样的话，默认第一个生效
```

**react-router-dom库向外暴露n个组件**

  **BrowserRouter history模式**

  **HashRouter  hash模式(地址带#)**

   **要求：所有路由组件都必须是他们的子组件（必须是嵌套关系）**

所以在最外层套任意一个

```js
import React, { Component } from "react";
import {
  BrowserRouter as Router,
  HashRouter,
  Link,
  NavLink,
  Route,
  Switch,
  Redirect,
} from "react-router-dom";

export default class App extends Component {
  render() {
    return (
      <HashRouter>
        
      </HashRouter>
    );
  }
}
```

## 路由跳转的两种方式

1. **路由链接导航(声明式)**

​    **Link NavLink**

```js
 <Link to={`/home/message/${mes.id}`}>{mes.content}</Link>
 <Route path="/home/message/:id" component={MessageDetail} />
 // :id 路径定义占位符
```



2. **编程式导航（编程式路由跳转）**

​    **history.push/replace/goBack/goForward**

```js
<button onClick={this.push(path)}>push</button>
<button onClick={this.replace(path)}>replace</button>
```

**什么时候用哪种？**

   **如果只要去做跳转行为，就用路由链接导航**

   **如果除了跳转行为，还需要干一些其他事（发送请求、保存数据等），就用编程式导航**



## 路由传参

**1、params传递参数** 

​       **\- 路径定义占位符**

```js
 Link方式：
<Link to={`/home/message/${id}`}>{mes.content}</Link>
 <Route path="/home/message/:id" component={MessageDetail} />
 // :id 路径定义占位符
Js方式：
 this.props.history.push(`/home/message/${id}`)
```

​       **\- 使用   MessageDetail  组件通过 this.props.match.params 获取传递的参数ID**

​      



 **2、query传递参数(用的少)**

 注：不需要配置路由表。路由表中的内容照常：<Route path='/sort' component={Sort}></Route>

 **1.Link处**    

​    **HTML方式**

​      **<Link to={{ path : ' /sort ' , query : { name : 'sunny' }}}>**　　　　　　　　

​    **JS方式**

​      **this.props.history.push({ path : '/sort' ,query : { name: ' sunny'} })**

 **2.sort页面**   

​       **this.props.location.query.name**

​       **输入地址 、 更新地址 http://localhost:3000/home/message/1?name=jack**

```js
<button onClick={this.push(`/home/message/${id}?name=jack`)}>push</button>
//params和query可以并存
```

​       **\- 使用 组件通过 this.props.location.search获取传递的参数，但是不是一个对象，未处理的需要自己处理好**

​       

3. **state传递参数**

​       **\- 传递参数** （参数加密，不在地址栏显示）

```js
<button onClick={this.push(path)}>push</button>
push = (path) => {
  return () => {
    this.props.history.push(path, { name: "huahua", age: 48 });
  };
};
// push方法第一个参数为path(to)地址，第二个参数为需要传递的参数
```

​       **\- 使用 组件通过 this.props.location.state 获取传递的参数**

​     **通过state**

​       **同query差不多，只是属性不一样，而且state传的参数是加密的，query传的参数是公开的，在地址栏**

​     **1.Link 处**    

​      **HTML方式：**

​        **<Link to={{ pathname : ' /user' , state : { day: 'Friday' }}}>** 　

​     **JS方式：**

​      **this.props.history.push({ pathname:'/user',state:{ day : 'Friday' } })**

​     **2. user页面**     

​      **this.props.location.state.day**



**凡是通过Route加载的组件，就成为路由组件，组件内部能接受到三个属性**

​       **history 路由跳转**

​       **location 获取query和state参数，以及当前路由路径**

​       **match 获取params参数**

## 路由配置文件写法

**src下新建一个router文件夹，内置index.js**

**引入组件配置并且暴露出去**

```js
import Home from "../pages/Home";
import Pins from "../pages/Pins";
import Topics from "../pages/Topics";
import Books from "../pages/Books";
import Events from "../pages/Events";

const routes = [
  // 按书写顺序遍历
  {
    path: "/",
    component: Home,
    exact: true,
    title: "首页",
  },
  ......
]
export default routes;
```

**要用的地方先import引入配置文件，然后遍历**

```js
// NavLink遍历
{routes.map((route) => {
  return (
    <li key={route.path}>
      <NavLink to={route.path} exact={route.exact}>
        {route.title}
      </NavLink>
    </li>
  );
})}

// Route遍历
<Switch>
  {routes.map((route) => {
    return (
      <Route
        path={route.path}
        exact={route.exact}
        component={route.component}
        key={route.path}
      />
    );
  })}
</Switch>
```



# Redux

## redux是什么?

**使用下载 npm install  redux**

1)     redux是一个独立专门用于做状态管理的JS库(不是react插件库)

2)     它可以用在react, angular, vue等项目中, 但基本与react配合使用

3)     作用: 集中式管理react应用中多个组件共享的状态

## 什么情况下需要使用redux

1)     总体原则: 大型项目状态管理复杂才用

2)         某个组件的状态，需要共享

3)         某个状态需要在任何地方都可以拿到

4)         一个组件需要改变全局状态

5)         一个组件需要改变另一个组件的状态

## redux的三个核心概念

### action

**状态数据的操作类型，改变状态数据的方法**

要定义多少个action函数，要看对数据有多少种操作

作用：用来创建action对象的工厂函数模块

![image-20201023203053837](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201023203053837.png)

### reducer(纯函数，一开始会自动触发一次)

**action里改变状态数据方法的真正的代码执行处**

- **作用，根据之前的状态数据和action对象来计算生成最新的数据**
- **Store 收到Action之后（*dispatch触发更新，触发reducers*），必须给出一个新的State，这样才能使View发生变化。这种State的计算过程叫做Reducer。**
  **Reducer 是一个函数，它接受Action和当前的state作为参数，返回一个新的state。**
  整个应用的初始状态，可以作为state的默认值
- **previousState默认初始值要考虑到状态数据的类型(是用对象存储数据还是数组存储数据)，把他当前存储状态数据的state**
- **reducers函数名称就是要管理的数据名称**

![image-20201023202719090](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201023202719090.png)

###  store

- redux中的store是通过createStore方法创建的，该方法接收两个参数**reducer函数**和**初始化的数据(currentState)**，从而形成一颗状态树

- createStore方法调用时传入的reducer方法会在store的dispatch被调用的时候，被调用，接收store中的state和action，根据业务逻辑（即reducer方法）返回新的state
- **store含有四个方法，`subscribe`、`dispatch`、`getState`和`replaceReducer`**
  1. `subscribe`:接收一个回调(listener)，当dispatch触发时，执行reducer函数去修改当前数据(currentState)，并执行subscribe传入的回调函数(listener)**（监听数据变化）**
  2. `dispatch`:分发 action。这是触发 state 变化的惟一途径**（触发更新）**
  3. `getState`:**读取所有数据**
  4. `replaceReducer`:替换 store 当前用来计算 state 的 reducer,不常用

![image-20201023202307922](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201023202307922.png)



## Redux工作流程图

![](D:\笔记案例\熊健 React教学\redux工作流程图.png)

组件调用action函数生成action对象，调用store.dispatch(action) 触发更新，触发reducers

```js
// 1. 调用action函数生成action对象
const action = increment(this.state.num);
// 2. 调用store.dispatch(action) 触发更新，触发reducers
store.dispatch(action);
```

触发调用reducers，并且传入两个参数：**当前的state** 和 **收到的Action**. Reducer 会返回新的state。

读取store对象管理的所有数据的方法

```
const count = store.getState();
```

数据变了组件没更新

```js
componentDidMount() {
  // redux数据更新后会触发这个函数
  store.subscribe(() => {
    // 重新的渲染组件
    this.setState({});
  });
}
```

## react-redux

### 是什么

1)     一个react插件库

2)     专门用来简化react应用中使用redux

###  使用react-redux

**npm install --save react-redux**

### 相关API

**1)     Provider**

**让所有组件都可以得到state数据**

![image-20201023205013401](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201023205013401.png)

**2)     connect()**    组件之外书写 **可以在当前的组件得到状态数据以及改变状态数据的方法**

**connect 高阶组件（HOC）：本质上是一个函数，执行函数传入一个组件作为参数，返回值是一个新组件**

**作用：主要是用来复用代码**

```js
const NewComponent = connect(mapStateToProps, mapDispatchToProps)(App);
// NewComponent为新组件（容器组件），APP为其子组件（Ui组件），父组件更新子组件也会更新渲染
// 将mapStateToProps和mapDispatchToProps方法的返回值对象作为属性以props的方式传给APP组件
export default NewComponent
```

**3)     mapStateToprops()**

**将外部的数据（即state对象）转换为UI组件(容器组件的子组件，哪个组件用就是哪个组件)的标签属性**

```js
// 将redux中的state数据取出，以props传递给子组件
// state就是内部通过store.getState()调用后的返回值，说白了就是所有状态数据
 const mapStateToProps = (state) => {
   return {
     count: state, // return 状态数据对象作为属性以props的方式传给了App组件
   };
 };
App组件使用   const count = this.props.count;
```

**4)     mapDispatchToProps()**

**将分发action的函数（increment，decrement）转换为UI组件(容器组件的子组件，哪个组件用就是哪个组件)的标签属性**

**简洁语法可以直接指定为actions对象或包含多个action方法的对象**

```js
// 将触发更新redux的方法封装好，以props传递给子组件 dispatch即为store.dispatch
 const mapDispatchToProps = (dispatch) => {
   return {
     // return 返回值作为属性以props的方式传给了App组件
     increment(num) {   
       // 1. 调用action函数生成action对象
       const action = increment(num);
       // 2. 调用store.dispatch(action)
       dispatch(action);
     },
     decrement(num) {
       // 1. 调用action函数生成action对象
       const action = decrement(num);
       // 2. 调用store.dispatch(action)
       dispatch(action);
     },
   };
 };
APP组件内使用  this.props.increment(this.state.num);
```



### 简洁写法(组件都可用)

**参数1为状态数据 ，参数2为改变状态数据的方法，都作为属性以props的方式传给了当前ui组件**

**不需要的参数可以为null**

```js
import { connect } from "react-redux";
import { increment, decrement } from "./redux/actions";
// 参数1为状态数据 ，参数2为改变状态数据的方法
export default connect((state) => ({ count: state }), {
  // 这两个是引入的action里的函数
  // 内部也进行了mapDispatchToProps()的方式处理过
  // 作用有生成action对象并且触发reducer函数
  increment,
  decrement,
})(App);

// 将count和increment，decrement都作为属性以props的方式传给了App组件
```



###  redux异步编程（异步action）

1)   redux默认是不能进行异步处理的, 

2)   应用中又需要在redux中执行异步任务(ajax, 定时器)

#### 下载redux插件(异步中间件)

**npm install redux-thunk**

**在store文件中：**

```js
import {createStore, applyMiddleware} from 'redux'
import thunk from 'redux-thunk'
// 根据counter函数创建store对象
const store = createStore(
  counter,
  applyMiddleware(thunk) // 应用上异步中间件
)

```



### 使用上redux调试工具

安装chrome浏览器插件 redux-devtools

下载工具依赖包

npm install redux-devtools-extension -D

在store文件中：

```js
import { composeWithDevTools } from 'redux-devtools-extension'

const store = createStore(
  counter,
  composeWithDevTools(applyMiddleware(thunk)) 
)

```

### Store文件的一般固定写法

```js
/* 
  用来集中存储所有数据
    读取数据、更新数据的方法都在store中
*/
import { createStore, applyMiddleware } from "redux";
// 引入异步中间件,redux默认是不能进行异步处理的	应用中又需要在redux中执行异步任务(ajax, 定时器)时）
import thunk from "redux-thunk";
// 引入调试工具
import { composeWithDevTools } from "redux-devtools-extension";
// 引入reducers
import reducers from "./reducers";
// 应用上异步中间件
let middleware = applyMiddleware(thunk);
// 判断开发环境下 调试工具才有用
if (process.env.NODE_ENV === "development") {
  middleware = composeWithDevTools(middleware);
}

// 创建store对象
const store = createStore(reducers, middleware);
// 暴露出去
export default store;

```

### combineReducers

**之前我们书写的reducers中只有一个函数，如果redux需要保管多个状态数据的话，就需要定义多个函数**

```js
import { combineReducers } from "redux";
// comments状态
function comments(prevState = initComments, action) {
  switch (action.type) {
    case ADD_COMMENT:
      return [action.data, ...prevState];
    case DEL_COMMENT:
      return prevState.filter((comment) => comment.id !== action.data);
    default:
      return prevState;
  }
}
// users状态
function users(prevState = {}, action) {
  switch (action.type) {
    default:
      return prevState;
  }
}

// combineReducers整合多个reducer函数为一个,
// 并且redux保存的值就是一个{ comments, users }
export default combineReducers({
  comments,
  users
})
// 组件使用状态数据,state是所有的数据，这里需要从state数据对象中`.`出来
export default connect((state) => ({ count: state.count }), {
})(App);
```









# redux 开发流程

1. 首先下载包
   yarn add redux react-redux redux-thunk redux-devtools-extension

2. 定义 redux 四个模块

- store
- actions
- reducers
- contants

首先可以确定写死的是 store 模块，其他模块要根据需求去完成

3. 使用 react-redux
   在 index.js 中使用 Provider 组件
   <Provider store={store}>
    <App />
   </Provider>

以上就是 redux 的准备工作

4. 分析需求

- 根据实际需求分析有哪些数据要存储在 redux 中（看数据有没有多个组件使用，至少两个组件使用才有意义）
- 分析对数据有多少种操作行为

5. 定义 actions

- 根据对数据的操作行为来定义 action 函数
  - 如果要发送请求就定义异步 action，否则就是同步 action
  - 异步 action 往往都需要一个同步 action 来生成 action 对象

6. 定义 reducers

- 根据 actions 操作类型，定义 case 语句，来对数据进行计算，生成 newState
- reducers 需要一个初始化值，作为第一个参数 prevState 的默认值

7. 组件使用

- 通过 connect 高阶组件给组件传递 redux 的数据和更新数据的方法
- 组件通过 ths.props.xxx 使用



# React hooks

## 理解

Hooks是React16.8的新增特性。它可以让你在不编写class类组件的情况下使用state以及其他的React特性

## 作用

Hook也叫钩子本质是函数，能让你使用React组件的状态和生命周期函数

使用工厂函数的方式定义组件，让代码的复用性更强，不用再定义复杂的HOC（高阶组件），彻底消除this

## 注意

只能在顶层调用钩子，不要在循环，控制流和嵌套的函数中调用钩子

只能从React的函数式声明的组件中调用钩子。不要再常规的JS函数中调用钩子



## 具体使用（useState和useEffect）

**React.useState(0) 用来给工厂函数组件使用state**

  **const [state, setState] = React.useState(defaultValue)**

   **state 就是状态数据**

   **setState 就是更新状态数据的方法**

   **defaultValue 就是状态数据的初始化值**



**React.useState(0) 用来给工厂函数组件使用state**

```js
import React, { useState, useEffect } from "react";
const [count, setCount] = useState(0);// setCount只能修改count，
// 如果有多个状态数据
const [count, setCount] = useState(0);
const [num, setNum] = useState(0);
// 一个更新状态数据的方法对应一个状态数据

```

 **React.useEffect() 用来给工厂函数组件使用生命周期函数**

```js
useEffect(() => {
    /*
      相当于componentDidMount和componentDidUpdate
    */
    console.log("useEffect");

    return () => {
      // componentDidUpdate，比上面函数先触发
      // componentWillUnmount
      console.log("cb");
    };
    // 第二个参数是数组，数组中放置当前函数的依赖的值，如果依赖的值发生了变化，就会重新触发多次
    // 如果没有变化，就不会触发多次
  }, []);
  
```

- **在useEffect函数中如果不传递第二个参数相当于componentDidMount和componentDidUpdate生命周期函数**
- **在useEffect函数中return的函数里则相当于componentDidUpdate和componentWillUnmount生命周期函数**
- **第二个参数是数组，数组中放置当前函数的依赖的值，如果依赖的值发生了变化，就会重新触发多次**

- ***这样类似于componentDidMount生命周期函数***
  `useEffect(() => {}, [])`

