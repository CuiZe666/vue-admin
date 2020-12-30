## 1. less 简介

1. less是CSS的预编译器，可以扩展CSS语言（当然也兼容CSS），可以定义变量、混合、函数等等，让CSS代码更易维护和扩展
2. less与传统写法相比：

> - less后缀为" .less "
> - less中的注释有两种

```js
// 这种注释不会编译到CSS文件
/* 这种注释会编译到CSS文件*/
div {
  font-size: 14px;
}

```

3.less需要编译成css才能使用

## 2 .LESS地址

中文网站：http://lesscss.cn/

**英文官网：**[http://lesscss.org](http://lesscss.org/)

**less**源码： **[https://github.com/cloudhead/less.js](http://www.lesscss.net/)**



## 3.LESS的使用

#### 3.1 LESS的基础使用

- `<style type="text/less">` style标签的类型需要改成less
- 根据官网我们需要一个less编译的 less.js 文件，并在最下方引入，因为需要读取页面中所有less相关的文件，才可以进行编译



#### 3.2 LESS全局安装

- 安装nodeJS
- 打开命令行窗口，使用`node -v`和`npm -v`检查node和npm是否安装成功
- 使用`npm i -g less`进行全局安装less，并输入`less -V`检查版本
- 打开VSCode编辑器，安装`Easy LESS`插件
- 在文件夹中新建less文件，然后保存，即可在当前文件夹中编译出相应的css文件。



## 4 其他CSS预处理器语言

CSS 预处理器技术已经非常的成熟了，而且也涌现出了越来越多的 CSS 的预处理器框架。其中最常用的是 Sass、Less、Stylus。

> less --- 支持原生js,node， 让CSS有了基本的逻辑运算能力 , 基于js写的
>
> sass --- ruby环境，让CSS有了更完整的逻辑和判断能力,比如支持if这些 ,基于ruby
>
> stylus --- node -- VUE阶段开发项目中我们使用stylus， 这个让CSS变成跟一门编程语言类似了





## 5.变量

### 5.1 变量的定义和使用

- 定义：@name: value; （@black: #000;）
- 使用场合分3种：

> - 常规使用：@name
> - 作为选择器或属性名：@{name}
> - 作为URL："@{name}"

```js
/* 1.常规使用 */
@black: #000000;
div {
  color: @black
}

//编译结果
div {
  color: #000000;
}


/* 2.作用选择器和属性名 */
@selName: container;
@proName: width;

.@{selName} {  //作为选择器的话是 .和# 照样写
  @{proName}: 100px;
}

//编译结果
.container {
  width: 100px;
}


/* 3.作为URL */

//声明一个变量，保存图片
@url1:"http://www.atguigu.com/images/pic_new/logo.png";

@url2:url("http://www.atguigu.com/images/pic_new/logo.png");

background: url(@url1) 0 0 no-repeat;
background: @url2 0 0 no-repeat;

```

### 5.2 注意事项

1. **变量是延迟加载的，可以不预先声明**

```js
div {
  color: @black;
}

@black: #000000;
//编译结果
div {
  color: #000000;
}
```

2.**变量的作用域**

> - less会从当前作用域没找到，将往上查找（类似js）
> - 如果在某级作用域找到多个相同名称的变量，使用最后定义的那个（类似css）
> -  内部作用域定义的变量 只能在当前的样式及后代中使用

```js
@var: 0;
.class {
    @var: 1;
    .brass {
        @var: 2;
        three: @var; //使用变量的时候，变量延迟加载
        @var: 3;
    }
    one: @var;//类似js，无法访问.brass内部
} 

//编译结果
.class {
    one: 1;
}
.class .brass {
    three: 3;  //使用最后定义的 @var: 3
}

```



## 6. 嵌套规则

- 在使用标准CSS时，要为多层嵌套的元素定义样式，要么使用后代选择器从外到内的嵌套定义，要么给这个元素加上类名或 id 来定义。这样的写法虽然很好理解，但维护起来很不方便，因为无法清晰了解到样式之间的关系。
- 在Less中，嵌套规则使这个问题迎刃而解。嵌套规则允许在一个选择器中嵌套另一个选择器，这更容易设计出精简的代码，并且样式之间的关系一目了然。

### 6.1 less的嵌套规则类似HTML的结构，使得CSS代码清晰

```js
/*css 写法*/
div {
  font-size: 14px;
}

div p {
 margin: 0 auto;
}

div p a {
  color: red;
}

// less写法
div {
  font-size: 14px;
  p {
    margin: 0 auto;
    a {
      color: red;
    }
  }
}
```

### 6.2 父元素选择符 &

- &表示当前选择器的所有父选择器,常用在伪元素 伪类 css结构类等需求上
- 内层选择器前面的 & 符号就表示对父选择器的引用。在一个内层选择器的前面，如果没有 & 符号，则它被解析为父选择器的后代；如果有 & 符号，它就被解析为父元素

```js
//css写法
.bgcolor {
  background: #fff;
}
.bgcolor a {
  color: #888888;
}
.bgcolor a:hover {
  color: #ff6600;
}


//less写法
.bgcolor {
  background: #fff; 
  a {
    color: #888888;      
    &:hover {    //nth-child()一样
      color: #ff6600;
    }
  }
}

```

### 6.3 改变选择器的顺序

- 将&放到当前选择器之后，会将当前选择器移到最前面
- 只需记住 “& 代表当前选择器的所有父选择器”

```js
ul {
  li {
    .color &{
      background: #fff;
    }
  }
}

//编译结果

.color ul li {
  background: #fff;
}

```

### 

## 7. LESS运算

- 任何数值，颜色，变量都可以运算
- less会给你自动推算单位，所以不需要每一个都加单位，但保证至少有一个加了单位
- 运算符与数值之间要以空格分开，涉及优先级时以（）进行优先级计算

```less
@num1 : 30;
.box{
    width: 30px * @num1;
    height: 1000px / 3;
    .con{
        // 普通数值的加减法
        width: 100px - 30%;
        height: (100px + 10) * 20;
    }
    .inner{
        // 颜色的运算是先转换成10进制的rgba  然后再计算  再转换成16进制
        background-color: #fff-55;
    }
}
```

## 8. LESS的混合（Mixin）

在 LESS 中我们可以定义一些通用的属性集为一个class，然后在另一个class中去调用这些属性.

混合就是将一系列属性从一个规则引入到另一个规则中

#### 8.1普通混合

混合的定义：使用`.+混合名+()+{属性的合集}`

混合的使用：.+混合名+();

```js
//在 LESS 中我们可以定义一些通用的属性集为一个class，然后在另一个class中去调用这些属性.
//混合就是将一系列属性从一个规则引入到另一个规则中

//如果说作为mixin的类 不加小括号，则可以被编译出来，如果加上括号，则不能被编译出来，只能被调用
.mine(){
    border: 1px solid #000;
    border-radius: 10px;
    background-color: red;
}

.box{
    width: 100px;
    height: 100px;
    //调用Mixin的时候，可以加上小括号调用，也可以不加
    .mine();
}
.con{
    width: 100px;
    height: 100px;
    .mine; //不加小括号
}
```

#### 8.2带参数的混合

在声明混合的时候，可以在小括号中声明形参。形参由@+变量名定义。

调用的时候可以传入实参

```js
//定义的时候声明形参  和变量一个样子 
.center(@w,@h,@bg){
    width: @w;
    height: @h;
    background-color: @bg;
    position: absolute;
    left: 0;
    top: 0;
    bottom:0;
    right: 0;
    margin: auto;
}
.outer{
    //传参数
    .center(500px,500px,red);
    .inner{
       .center(300px,300px,green);
        .con{
            .center(100px,100px,pink);
        }
    }
}
```



#### 8.3 参数有默认值的混合

可以直接在混合中定义形参的时候，给形参设置默认值，比如（@color:red）。

当使用混合的时候，如果有实参传递，则使用实参的值，否则使用形参的默认值

```js
//定义的时候声明形参  和变量一个样子
//如果用户不传宽高的时候，默认宽高就是500px ，那么可以给形参定义默认值,只要传递参数就会覆盖默认值
.center(@w:500px,@h:500px,@bg){
    width: @w;
    height: @h;
    background-color: @bg;
    position: absolute;
    left: 0;
    top: 0;
    bottom:0;
    right: 0;
    margin: auto;
}
.border(@w,@s,@c){
    // border:@w @s @c;
    //在mixin中，可以使用@arguments代表传入的实参
    border:@arguments;
}
.outer{
    //传参数
    //命名参数：传递参数的时候，可以给对应的形参传递
    .center(@bg:yellow,@h:600px);  //如果传参的顺序不按照书写mixin的顺序  则传参的时候可以在前边限定参数
    .inner{
       .center(300px,300px,green);
        .con{
            .center(100px,100px,pink);
            .border(10px,dotted,yellow)
        }
    }
}
```



#### 8.4 命名参数

引用mixin时可以通过参数名称而不是参数的位置来为mixin提供参数值，任何参数都通过名称来引用，这样就不必按照特定的顺序来使用参数



#### 8.5 @arguments变量

@arguments表示所有可变参数
参数的先后顺序就是括号内的顺序 ，在赋值时，值的位置和个数也是一一对应的
只有一个值，把值赋给第一个，两个值，赋给第一个和第二个......
若想赋给第一个和第三个，必须把第二个参数的默认值写上

```js

.border(@x: solid, @c: red) {
  border: 21px @arguments;
}
.div1 {
  .border();
}
.div2 {
  .border(solid, black)
}
 
//编译输出
.div1 {
  border: 21px solid red;
}
.div2 {
  border: 21px solid black;

```



## 9.模式匹配 重载 守卫

### 模式匹配

自定义一个字符，使用时加上那个字符，就调用相应的规则

其中当参数是@_开头的，是调用此混合必选的。

```js
.mine(@_) {
    width: 300px;
    height: 100px;
}

.mine(color1) {
    background-color: red;
}

//这个混合中书写的不是变量，这个参数不是用来向混合内部传递的  而是用来识别匹配的
.mine(color2) {
    background-color: pink;
}

.outer {
    //根据混合匹配的参数，如果参数一致 则使用当前混合
    //混合中的参数如果是@_  则当前的混合是必选的
    .mine(color1);
}

//css编译
.outer {
  width: 300px;
  height: 100px;
  background-color: red;
}


```



### 重载

相同的混合，不同的行为，可以根据调用的时候传入的实参个数选择匹配的混合内容

```js
.mixin(@a, @b) {
    background-color: @a;
    width: 1000px;
}

.mixin(@a) {
    background-color: @a;
    width: 100px;
}

.box {
    border: 1px solid #000;
    height: 100px;
    .mixin(pink, red);
}

//css编译

.box {
  border: 1px solid #000;
  height: 100px;
  background-color: pink;
  width: 1000px;
}
```



### 守卫

根据判断条件选中Mixin的行为

类似于JavaScript的if/else，使用when语法

Guards 允许我们使用>,>=,<,<=,=,关键字true（只匹配关键字true，非true不会匹配），支持逻辑and ,not ()

同时我们可以使用“，”分割多个Guards，其表示只要其中任意一个满足就为true

```js
.mixin1(@q) {
    //无守卫 随时可以进入
    height: 100px;
}

.mixin1(@q) when(@q>10) {
    background-color: red;
}

.mixin1(@q) when(@q<=10),//满足一个条件就可以
(@q>100) {
    background-color: black;
}

.box {
    border: 1px solid #000;
    .mixin1(-1);
}


//css编译
.box {
  border: 1px solid #000;
  height: 100px;
  background-color: black;
}

```

## 10.引入文件

@import引入css影响性能

但是在less中使用@import引入less 是可以直接把less读取解析的 

#### 引入less文件

**less文件中可以导入其他的less文件，从而实现文件的合并,引入是可以不加后缀；注意空格；**

**导入Less文件： @import “lib.less”;  //等价于@import “lib”（可以不带后缀）**  

在前面引入就在前面编译



#### 引入css文件

**导入css文件： @import "lib.css";  并且引入时不能省略后缀名**



## 11.转义

```js
@url: 'http://www.baidu.com';
@w: 200;

.outer {
    //字符串插值  需要使用 @{a}的格式  直接书写在字符串中
    background: url('@{url}/01.png');

    width: ceil(20.22px);

    //~书写给字符串可以对字符串进行转义为css可用的内容
    height:~'@{w}px';
}

//css编译
.outer {
  background: url('http://www.baidu.com/01.png');
  width: 21px;
  height: 200px;
}

```







