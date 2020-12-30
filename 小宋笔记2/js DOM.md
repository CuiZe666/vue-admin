# DOM基础

## 什么是 DOM

- DOM( Document Object Model,文档对象模型)是w3C制订的一套技术规范,用来描述 Javascript脚本如何与HTML或XML文档进行交互的Web标准
- 加载 HTML 页面时, Web浏览器生成一个树型结构,用来表示页面内部结构。DOM 将这种树型结构理解为由节点组成的DOM树
- DOM规定了一系列标准接口,允许开发人员通过标准方式访问文档结构、操作网页内容、控制样式和行为等
- 在DOM中，接口可以理解为就是函数（函数 方法 API 接口 本质上都是一个函数）



## document对象

- **Document 对象**是是window对象的一个属性，因此可以将document对象作为一个全局对象来访问。当浏览器载入 html文档, 它就会成为 **Document 对象**。
- 打开控制台输入 document，然后我们就看到了一个document对象，既然是对象，输入 typeof document, 控制打印了 "object"

```js
console.log(document);
console.log(typeof document);
//查看document对象的方法和属性
for(var i in document){
    console.log(i)
}
```



## 节点

### 什么是节点

- 在网页中所有对象和内容都被称为节点。
- 如文档、元素、文本、属性、注释等。
- 节点(Node)是DOM最基本的单元,并派生出不同类型的节点,它们共同构成了文档的树形结构模型。

### 节点种类(常用)

- document 文档节点
- documentFragment 文档片段节点
- Element 元素节点
- attr 属性节点
- text 文本节点
- comment 注释节点

### 节点关系

DOM把文档视为一棵树形结构,也称为节点树。节点之间的关系包括:上下父子关系，相邻兄弟关系

相邻兄弟关系

- 在节点树中,最顶端节点为根节点。
- 除了根节点之外,每个节点都有一个父节点
- 节点可以包含任何数量的子节点
- 叶子是没有子节点的节点
- 同级节点是拥有相同父节点的节点

### 节点类型名称和值

- childNodes:获取某个元素中的所有的子节点，使用中括号语法或者item()方法可以获取对应的节点
- nodeType：获取节点的类型
- nodeName：节点名称
- nodeValue：节点的值

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>节点</title>
</head>
<body>
<div id="box">
    <p>我是p标签</p>
    <span>hello</span>
    <i>i标签</i>
    <div>我是div</div>
    <!--<h3>h333</h3>-->
    <em>em标签</em>
</div>
<script>
    var oBox = document.getElementById("box");
    var nodes = oBox.childNodes;
    console.log(nodes);//节点的集合 NodeList类型
    console.log(nodes[3]);//直接可以获取到节点
    console.log(nodes.item(5));//直接可以获取到节点

    //document的节点类型 名称 和值
    console.log(document.nodeType);//9
    console.log(document.nodeName);//#document
    console.log(document.nodeValue);//null


    //元素节点的类型 名称和 值
    console.log(nodes[1].nodeType);//1
    console.log(nodes[1].nodeName);//节点名称就是标签名 大写
    console.log(nodes[1].nodeValue);//null

    //文本节点的类型 名称和 值（文本节点没有子节点）
    console.log(nodes[0].nodeType);//3
    console.log(nodes[0].nodeName);//#text
    console.log(nodes[0].nodeValue);//当前的文本内容

    //注释节点的类型 名称和 值
    console.log(nodes[9].nodeType);//8
    console.log(nodes[9].nodeName);//#comment
    console.log(nodes[9].nodeValue);//当前被注释的内容

    var myAttr = document.createAttribute("my");
    myAttr.value = "yes";
    console.log(myAttr);
    console.log(myAttr.nodeType);//2
    console.log(myAttr.nodeName);//my  属性节点是标准的键值对形式   属性名就是节点名称
    console.log(myAttr.nodeValue);//yes  属性值就是节点的值

</script>
</body>
</html>
```



## 访问节点

### 获取元素基础方法

- **通过标签名获取(**完美兼容)
  - 可以使用内置对象document上的`getElementsByTagName`方法来获取页面上的某一种标签
  - 获取的一定是一个选择集（伪数组），不是数组，但是可以用下标的方式操作选择集里面的标签元素。
  - 这个集合拥有length属性
  - 通过标签名获取的对象 要么在使用的时候添加下标，要么在获取的时候添加下标,因为使用的时候，只能拿一个元素去使用

```js
  var oLis = document.getElemerntsByTagName("li");
  console.log(oLis);
  oLis[0].style.backgroundColor = "green";
  oLis[1].style.backgroundColor = "green";
  oLis[2].style.backgroundColor = "green";
  oLis[3].style.backgroundColor = "green";

  //或者

  var oLis1 = document.getElementsByTagName("li")[0];
  oLis1.style.backgroundColor = "pink";
```

- **通过id获取标签(**完美兼容)
  - 可以使用内置对象document上的`getElementByID`方法来获取页面上的某一种标签
  - id是唯一的 所以通过id获取的标签也是唯一的
  - id如果没有获取，那么仍然可以拿着id名来使用，因他是唯一的。但是不建议这样书写

```js
  var oBox = document.getElementById("box");
  console.log(oBox);//得到整个元素
  oBox.style.backgroundColor = "red";
```

- **通过类名来获取元素**（不兼容IE678）
  - 可以使用内置对象document上的`getElementsByClassName`方法来获取页面上的某一种标签
  - 使用和`getElementsByTagName`使用方法一致

```js
  // IE8不认识此方法：对象不支持“getElementsByClassName”属性或方法
  var oLi1 = document.getElementsByClassName("li1");
  oLi1[0].style.backgroundColor = "green";
```

- **通过Name属性获取元素**（几乎不使用）
  - 可以使用内置对象document上的`getElementsByName`方法来获取页面上的某一种标签
  - 使用和`getElementsByTagName`使用方法一致
  - 兼容性：针对ie：通过Name获取 只能获取到表单元素 不支持其他元素通过name属性获取

```js
  var oLi = document.getElementsByName("oli");
  oLi[0].style.height = "300px";
```

### selectors API

- selectors API就是由W3C发布的一个事实标准，为浏览器实现元素的css选择器
- 两个核心方法 `querySelector`和`querySelectorAll（IE8+）`
- `querySelector`和`querySelectorAll`方法参数必须是符合css选择器语法规则的字符串。其中`querySelector`返回的是一个元素，`querySelectorAll`返回的是一个集合,一个NodeList对象（可以使用数组的forEach方法）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>新的选择器</title>
</head>
<body>
<ul class="outer">
    <li>11</li>
    <li>22</li>
    <li>33</li>
</ul>
<script>
    var oLis = document.querySelectorAll(".outer>li");
    console.log(oLis);//[li,li,li]
    console.log(oLis[0]);

    var oLi = document.querySelector(".outer>li");
    console.log(oLi);

    var oLi2 = document.querySelector(".outer>li:nth-child(2)");
    console.log(oLi2);
</script>
</body>
</html>
```

### selectors API和传统方法的比较

- getElementsByXXXXX获取的是一个动态的集合，（但是后生成的元素不能绑上之前的事件，事件委托可以）
- queryselectorAll获取的是静态的集合
- 动态：选出来的元素会随着文档的改变而改变，静态：只要取出来，就和页面有没有任何关系

```js
var oUl = document.getElementsByTagName("ul")[0];
var oLis = oUl.getElementsByTagName("li");
console.log(oLis.length);

//直接陷入死循环  因为getElementsByTagName获取的元素是动态的，所以我们遍历的长度是获取元素的长度
// 每次插入一个 长度就会加1  所有永远运行不完
for (var i = 0; i < oLis.length; i++) {
    oUl.appendChild(document.createElement("li"))
}
console.log(oLis.length);


// querySelectorAll获取的元素就是静态的
var oLis = document.querySelectorAll("ul li");
console.log(oLis.length);

for (var i = 0; i < oLis.length; i++) {
    document.querySelector("ul").appendChild(document.createElement("li"))
}
console.log(oLis.length);
```



### 节点关系中访问节点方法

- parentNode 获取元素的父元素节点 (全兼容)
- children：获取元素的所有子元素节点(IE678 可以获取到注释节点 所有在IE678这样获取的时候注意不要写注释)
- 获取元素的上一个兄弟节点
  - previousSibling 获取元素的上一个兄弟节点（在ie678中获取的是上一个兄弟元素节点 在非ie678中获取的是上一个节点（可能是文本节点））
  - previousElementSibling 在ie678中不支持 在非ie678中获取的是上一个元素节点（兼容性有顺序要求 previousSibling不能写前，因为previousSibling所有浏览器识别 但是意义不一样）
- 获取下一个兄弟节点
  - nextSibling 获取下一个兄弟节点
  - nextElementSibling 获取下一个兄弟节点
- 获取第一个子元素节点
  - firstChild:获取元素的第一个子元素节点
  - firstElementChild:获取元素的第一个子元素节点
- 获取最后一个子元素节点
  - lastChild:获取元素的最后一个子元素节点
  - lastElementChild:获取元素的最后一个子元素节点

```js
<ul id="box">
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
</ul>
<script>
    var oBox = document.getElementById("box");
    //children:获取所有的子元素节点(IE678可以获取到注释节点，如果要兼容IE678 则避免书写注释)
    var oLis = oBox.children;
    console.log(oLis);
    //parentNode:获取当前元素的父节点（全兼容）
      console.log(oLis[0].parentNode);//ul
      console.log(oLis[0].parentNode.parentNode);//body
      console.log(oLis[0].parentNode.parentNode.parentNode);//html
      console.log(oLis[0].parentNode.parentNode.parentNode.parentNode);//#document
      console.log(oLis[0].parentNode.parentNode.parentNode.parentNode.parentNode);//null
      //previousSibling:在ie678中获取的上一个兄弟元素节点，在其他浏览器中 获取的是上一个兄弟节点（可能是文本节点等等。。。）
      //previousElementSibling:在浏览器中获取上一个兄弟元素节点 ie678不识别
      console.log(oLis[1].nextSibling);    //ie的        #text
      console.log(oLis[1].nextElementSibling); //其他的  <li>1</li>

    //获取上一个兄弟节点
      console.log(oLis[3].previousSibling);
      console.log(oLis[3].previousElementSibling);
      // 兼容性写法
      if (oLis[3].previousElementSibling){
          console.log(oLis[3].previousElementSibling);
      }else{
          console.log(oLis[3].previousSibling);
      }

      // 获取下一个兄弟
      console.log(oLis[3].nextElementSibling);

      //获取第一个子节点
      console.log(oBox.firstElementChild);

      //获取最后一个子节点
      console.log(oBox.lastElementChild);



```

```js
//获取上一个兄弟元素的兼容性封装
function getPrevSibling(ele){
		if(ele.previousElementSibling){
				return ele.previousElementSibling;
		}else{
				return ele.previousSibling;
		}
}
console.log(getPrevSibling(oLis[2]));  //<li>2</li>

//获取下一个兄弟元素的兼容性封装
function getnextSibling(ele){
		if(ele.nextElementSibling){
				return ele.nextElementSibling;
		}else{
				return ele.nextSibling;
		}
}
console.log(getnextSibling(oLis[2]));  //<li>4</li>
```





### [其他获取节点方法]

- 获取body元素

  `document.body`==`document.getElementsByTagName('body')[0]`

- 获取head元素

  `document.head`==`document.getElementsByTagName('head')[0]`

- 获取html元素

  `document.documentElement`==`document.getElementsByTagName('html')[0]`





# JS事件

## DOM0级绑定事件方法

- `元素 . on+事件名称 = 函数`
- 相当于给一个元素的属性赋值,只能赋一个值,后面如果再赋值就会覆盖前面的值,所以说DOM0级事件只可以绑定一次,如果绑定多次,后面的会把前面的给覆盖了,因为是一个赋值的过程,一个属性只能赋一个值

## 失去和获取焦点事件

- `focus` 获取焦点事件

- `blur`失去焦点事件

- 练习：模拟placeholder

  ![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggpmwh2bbag314g0dctev.gif)

  ```html
  <input type="text" id="ipt" value="请输入电话号码">
      <script>                                          
          var oIpt=document.getElementById("ipt");    //获取元素
          oIpt.onfocus=function(){                    //绑定事件
              if(oIpt.value==="请输入电话号码"){
                   oIpt.value="";
              }
          }
          oIpt.onblur=function(){
              if(oIpt.value===""){
                   oIpt.value="请输入电话号码";
              }
          }
      </script>
  ```





## 点击事件

- click:鼠标点击事件
- contextmenu:鼠标右键事件
- ondblclick:鼠标双击事件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>点击事件</title>
    <style>
        #box{
            width: 300px;
            height: 200px;
            background: pink;
        }
    </style>
</head>
<body>
    <div id="box">我是box</div>
    <script>
        var oBox = document.getElementById("box");
        // 当在一个元素上按下鼠标  并又抬起的时候 才会触发click事件
        //右键也可能触发，但是右键的点击有专门的的事件 oncontextmenu
        oBox.onclick = function () {
            // alert(1);
        }

        // 右键事件  按下 并抬起的时候 弹出
        oBox.oncontextmenu=function(){
            alert(2);

            //取消默认右键的菜单弹出  取消默认事件 写在最后
            return false;
        }

        //双击事件
        oBox.ondblclick = function () {
            alert("用户双击");
        }
    </script>
</body>
</html>
```



练习：点击循环变色

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggpn0r8zp4g31bg0dgjx8.gif)

```js
开关法：
1.先获取元素
2.元素绑定事件
3.定义一个变量flag开关保存当前状态
4.事件发生后判断状态
5.改变完成后，改变开关的状态
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        #box{
            width: 300px;
            height: 200px;
            background: red;
        }
    </style>
</head>
<body>                                
    <div id="box">我是div</div>
    <script>
        var oBox=document.getElementById("box")
        var flag=true;
        oBox.onclick=function(){
            if(flag){
                oBox.style.background="pink";
            }else{
                oBox.style.background="red";
            }
            flag=! flag;
        }
    </script>
</body>
</html>
```

```js
累加求余法
1.获取元素
2.绑定点击事件
3.定义一个变量保存当前状态
4.每次点击变量加1，然后和2取余看是否为0判断奇偶来改变
<body>                                
    <div id="box">我是div</div>
    <script>
        var oBox=document.getElementById("box");
        var num=0;
        oBox.onclick=function(){
            num++;
            if(num % 2===0){
                oBox.style.background="red";
            }else{
                oBox.style.background="pink";
            } 
        }
    </script>
</body>

```





## 键盘事件

一般绑定给input或document

- onkeydown:键盘按下

- onkeyup:键盘抬起

- 练习：检测剩余字数

  ![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggpn84kfrdg315a0dwkjl.gif)

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>检测剩余字数</title>
      <style>
          textarea{
              border: 1px solid #ccc;
              outline: none;
          }
      </style>
  </head>
  <body>
      <textarea id="text" cols="30" rows="10" ></textarea>
      <p>还剩余 <span id="reduce">100</span> 个字</p>
      <script>
          /*
           * 用户在表单中输入内容，剩余字数减少，
           * 当剩余字数减少到负数的时候 把提示字数变成红色
           *
           * 1、获取标签
           * 2、书写键盘抬起事件 当抬起的时候 去计算剩余字数
           * 3、获取表单输入的长度
           * 4、计算剩余的字数是多少
           * 5、把计算好的剩余字数给到 reduce标签中
           * 6、检测剩余字数 小于0的时候，让标签变红  并且让textarea的边框变红
           * 7、当用户把字数重新减小，然后样式变成正常
           */
          // 1、获取标签
          var oReduce = document.getElementById("reduce");
          var oText = document.getElementById("text");
  
          // 2、书写键盘抬起事件 当抬起的时候 去计算剩余字数
          oText.onkeyup = function () {
              // 3、获取表单输入的长度
              var oTextLen = oText.value.length;
              // console.log(oTextLen);
              // 4、计算剩余的字数是多少
              var reduceTextNum = 100 - oTextLen;
              // 5、把计算好的剩余字数给到 reduce标签中
              oReduce.innerHTML = reduceTextNum;
              // 6、检测剩余字数 小于0的时候，让标签变红  并且让textarea的边框变红
              if (reduceTextNum < 0){
                  oReduce.style.color = "red";
                  oText.style.border = "1px solid red";
              }else{
                  // 7、当用户把字数重新减小，然后样式变成正常
                  oReduce.style.color = "#000";
                  oText.style.border = "1px solid #ccc";
              }
          }
      </script>
  </body>
  </html>
  ```







## 表单事件

- input:当表单内容改变时触发

- change:当表单内容改变并且失去焦点时候触发

- 练习：

  模拟数据双向绑定

  ![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggpoixp4h0g30qw0ccqag.gif)

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>input和change事件</title>
  </head>
  <body>
      <input type="text" id="ipt">
      <br>
      <span id="text"></span>
      <script>
          /*
          * 模拟 数据双向绑定效果
          * 当用户在input中输入内容的时候  span中的内容跟着改变
          *
          * 1、获取标签
          * 2、绑定事件  当用户输入的时候触发
          *   oninput事件-->表单内容只要发生改变就会实时触发
          *   onchange事件 -->失去焦点 并且表单内容发生改变 才会触发
          * 3、获取输入的内容 并赋值给text
          */
  
          // 1、获取标签
          var oIpt = document.getElementById("ipt");
          var oText = document.getElementById("text");
  
          // 2、绑定事件  当用户输入的时候触发
          oIpt.oninput = function () {
              // 3、获取输入的内容 并赋值给text
              oText.innerHTML = oIpt.value;
          }
  
      </script>
  </body>
  </html>
  ```





## 鼠标事件

- mousedown:鼠标按下
- mouseup:鼠标抬起
- mousemove:鼠标移动
- mouseover:鼠标移入(可以触发事件的冒泡)
- mouseout:鼠标移出(可以触发事件的冒泡)
- mouseenter:鼠标移入(不会触发事件的冒泡)
- mouseleave:鼠标移出(不会触发事件的冒泡)

```js
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>按下抬起事件</title>
    <style>
        #box{
            width: 200px;
            height: 200px;
            background: red;
        }
    </style>
</head>
<body>
    <div id="box">

    </div>
    <script>
        /*
        * 鼠标在元素上 按下 元素变成黄色  抬起 变成红色
        * onmousedown  鼠标按下事件
        * onmouseup  鼠标抬起事件
         */

        var oBox = document.getElementById("box");
        oBox.onmousedown = function () {
            oBox.style.backgroundColor = "yellow";
        }
        oBox.onmouseup = function () {
            oBox.style.backgroundColor = "red";
        }
    </script>
</body>
</html>
```





## 滚轮事件

- onscroll 事件在元素滚动条在滚动时触发。

- 练习

  ![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggppcpnx7bg30xx0dwh7f.gif)

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>onscroll</title>
      <style>
          #outer{
              width: 400px;
              height: 300px;
              border:1px solid #000;
              overflow: scroll;
          }
          #con{
              width: 800px;
              height: 1000px;
          }
      </style>
      <script>
          window.onload = function () {  //script写在head标签中也行
              // onscroll是滚动条滚动即执行
              var oOuter = document.getElementById("outer");
              var oCon = document.getElementById("con");
              oOuter.onscroll = function () {
                  console.log(Date.now());//只要在滚动  那么每次间隔最小可能是十几毫秒 甚至2毫秒  所以onscroll事件执行特别的频繁
                  console.log("我滚了 再见");
              }
          }
      </script>
  </head>
  <body>
  <div id="outer">
      <div id="con">
          今天天气真好 <br> * 100
      </div>
  </div>
  </body>
  </html>
  ```



## 常见window事件

### onload事件

onload 事件会在页面或图像加载完成后立即发生（一个页面中只能出现一次window.onload 因为DOM0级事件 会覆盖）

- window.onload事件：当整个文档内容（DOM节点+所需要的资源（音频、视频、图片、程序等等））全部加载完毕，才会执行

```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>window.onload</title>
      <script>
          // js是可以放在这个位置的，
          // 但是在这个位置的js 直接获取元素是会获取不到的  因为代码还没有运行到DOM节点
         /* var oBox = document.getElementById("box");
          oBox.style.backgroundColor = "red";*/

         // 解决方法：添加window.onload
          window.onload = function () {
              var oBox = document.getElementById("box");
              oBox.style.backgroundColor = "red";
          }

      </script>

  </head>
  <body>
  <div id="box">我是box</div>
  <img src="../images/01.jpg" alt="">
  <img src="../images/02.jpg" alt="">
  <img src="../images/03.jpg" alt="">
  <img src="../images/04.jpg" alt="">
  </body>
  </html>
```

```js
<body>
    <img id="img" src="01.jpg" alt="">
    <script>
        //offsetWidth:获取元素的宽度
        var oImg = document.getElementById("img");
        console.log(oImg.offsetWidth);//0 此时图片还没有下载完成

        /*
        * 可以给img标签绑定onload事件，当图片加载完成后执行
        * */
        oImg.onload = function () {
            console.log(oImg.offsetWidth);//1920
        }
    </script>
</body>
</html>
```



### 进度条练习

 1.获取元素

​    \*  2.遍历生成所有的img标签

​    \*  3.给img标签添加src属性

​    \*  4.给img标签添加onload事件

​    \*  5.当事件发生 让计数器加1

​    \*  6.根据计数器的值 计算当前图片加载的百分比，然后操作DOM

```js
<head>
    <meta charset="UTF-8">
    <title></title>
    <style>
        .box{
            width: 400px;
            height: 40px;
            border:3px solid #000;
        }
        .per{
            width: 0;
            height: 40px;
            background-color: red;
        }
    </style>
</head>
<body>
   <div class="box">
       <div class="per"></div>
   </div>
   <p>已经加载了<span class="num">0</span>%</p>
   <script>
       var oPer=document.querySelector(".per");
       var oNum=document.querySelector(".num"); //1获取元素
       var count=0;                          //定义一个图片加载计数器
       for(var i=1;i<31;i++){
        var newImg=new Image();  						//生成img标签
        newImg.src="images/"+(i<10 ? "0"+i : i)+".jpg"; 
        console.log(newImg);
        newImg.onload=function(){                //给img标签添加onload事件
            count++;                            //每加载完一个加1
            var long=parseInt(count/30 *100);   //计算已经加载完成的图片所占用所有图片的百分比
            oPer.style.width=long+"%"          //控制per的宽度
            oNum.innerHTML=long;               //改变num的值
        }
       }
   </script>
</body>
</html>
```



### window.onscroll

- window.onscroll：当系统滚动条滚动的时候触发



### resize事件

- 窗口重置，resize事件是在浏览器窗口被重置时触发。

- 练习

  ![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggpposxnpvg315w0dw185.gif)

  

```js
<script>
    //获取视口大小封装函数
    var getScreen = function () {
        var w = document.documentElement.clientWidth||document.body.clientWidth;
        var h = document.documentElement.clientHeight||document.body.clientHeight;;
        return {
            width:w,
            height:h
        }
    }
    function fn(){
        if(getScreen().width >= 1000){
            document.body.style.background = "red";
        }else if(getScreen().width >= 700){
            document.body.style.background = "pink";
        }else if(getScreen().width >= 400){
            document.body.style.background = "blue";
        }
    }
    fn();
    window.onresize = fn;  //方法一

    // window.onload = window.onresize = fn; //方法二
</script>
```



## TAB选项卡切换

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggqtfovkxng30np0dwq61.gif)

**方法一**

1.for循环遍历h2给所有的h2绑定点击事件

2.当事件发生的时候，遍历所有的h2

3.判断哪一个h2标签和当前点击的对象相等，如果相等则当前的 下标i就是点击的对象的下标

4.给当前的i的h2和li特殊类型，其他的去掉类名

```js
 <style>
        *{
            margin: 0;
            padding: 0;
            list-style: none;
        }
        #tab{
            width: 600px;
            margin: 50px auto;
            border: 1px solid #000;
        }
        #tab .head{
            overflow:hidden;
        }
        #tab .head h2{
            width: 198px;
            float: left;
            height: 40px;
            line-height: 40px;
            text-align: center;
            font-size: 20px;
            border: 1px solid #000;
        }
        #tab .head h2.active{
            background-color: red;
            color: #fff;
        }

        #tab .con{
            height: 300px;
        }
        #tab .con li{
            height: 300px;
            font-size: 60px;
            text-align: center;
            line-height: 300px;
            display: none;
        }
        #tab .con li.show{
            display: block;
        }
    </style>
</head>
<body>
    <div class="outer">
        <div id="tab">
            <div class="head">
                <h2 class="active">标题1</h2>
                <h2>标题2</h2>
                <h2>标题3</h2>
            </div>
            <ul class="con">
                <li class="show">内容1</li>
                <li>内容2</li>
                <li>内容3</li>
            </ul>
        </div>
    </div>

    <script>
        var oH2s=document.querySelectorAll(".head h2");  //获取元素是个集合
        var oLis=document.querySelectorAll(".con li");
				for(var i=0;i<oH2s.length;i++){                 //遍历所有H2
            oH2s[i].onclick=function(){                 //给所有H2绑定点击事件
                for(var i=0;i<oH2s.length;i++){         //事件发生遍历所有h2
                    if(oH2s[i]==this){                  //如果等于当前的话，则加类名，不等于则去掉其他类名
                        oH2s[i].className="active";
                        oLis[i].className="show";
                    }else{
                        oH2s[i].className="";
                        oLis[i].className="";
                    }
                }
            }
        }
    </script>





//for是同步代码，事件函数是异步代码
        //等事件函数执行的时候，同步代码已经已经执行完毕，此时for循环的i已经执行到完成阶段了，已经3了
        /*for (var i = 0; i < oH2s.length; i++) {
            oH2s[i].onclick = function(){
                this.className = "active";
                oLis[i].className = "show"
            }
        }*/
```



方法二：

1.给所有的h2绑定点击事件

2.我们自己给每一个h2元素对象 扩展一个自定义对象属性 num 保存的是当前h2在父级元素中的下标

3.每次点击 先全部清空

4.ran然后给当前的元素添加类名字



```js
<style>
        *{
            margin: 0;
            padding: 0;
            list-style: none;
        }
        #tab{
            width: 600px;
            margin: 50px auto;
            border: 1px solid #000;
        }
        #tab .head{
            overflow:hidden;
        }
        #tab .head h2{
            width: 198px;
            float: left;
            height: 40px;
            line-height: 40px;
            text-align: center;
            font-size: 20px;
            border: 1px solid #000;
        }
        #tab .head h2.active{
            background-color: red;
            color: #fff;
        }

        #tab .con{
            height: 300px;
        }
        #tab .con li{
            height: 300px;
            font-size: 60px;
            text-align: center;
            line-height: 300px;
            display: none;
        }
        #tab .con li.show{
            display: block;
        }
    </style>
</head>
<body>
    <div class="outer">
        <div id="tab">
            <div class="head">
                <h2 class="active">标题1</h2>
                <h2>标题2</h2>
                <h2>标题3</h2>
            </div>
            <ul class="con">
                <li class="show">内容1</li>
                <li>内容2</li>
                <li>内容3</li>
            </ul>
        </div>
    </div>

    <script>
        var oH2s=document.querySelectorAll(".head h2");
        var oLis=document.querySelectorAll(".con li");
				for(var i=0;i<oH2s.length;i++){
            oH2s[i].num=i;                           //自定义H2对象属性num 保存当前h2的下标
            oH2s[i].onclick=function(){              
                for(var i=0;i<oH2s.length;i++){      //事件发生全部清空先
                    oH2s[i].className ="";    
                    oLis[i].className ="";
                }
                this.className="active";						//ran然后给当前的元素添加类名字
                oLis[this.num].className="show"
            }
        }
    </script>
```





**动画版**

```js
//for循环遍历h2给所有的h2绑定点击事件
for (var i = 0; i < oH2s.length; i++) {
    oH2s[i].onclick = function () {
        //当事件发生的时候，遍历所有的h2
        for (var i = 0; i < oH2s.length; i++) {
            //判断哪一个h2标签和当前点击的对象相等，如果相等则当前的 下标i就是点击的对象的下标
            //给当前的i的h2和li特殊类型，其他的去掉类名
            if(oH2s[i] === this){
                oH2s[i].className = "active";

                //起始位置，直接获取当前滚动条位置即可
                var startScroll = oCon.scrollTop;
                //结束位置 和当前点击的元素的下标相关
                var endScroll = i * oLis[0].offsetHeight;
                //起始步数
                var startStep = 0;
                //结束步数
                var endStep = 22;
                //每一步所走的距离
                var everyStep = (endScroll - startScroll)/(endStep - startStep);

                var timer = setInterval(function () {
                    startStep ++;
                    if(startStep >= endStep){
                        clearInterval(timer);
                    }

                    //scroll自身可以参与计算，但是最终计算结果会进行省略小数，所有可能不精确，所以请使用变量去计算 然后再赋值
                    /*console.log(oCon.scrollTop,everyStep)
                    oCon.scrollTop += everyStep;
                    console.log(oCon.scrollTop);*/
                    startScroll += everyStep;
                    oCon.scrollTop = startScroll;
                },1000/60)
            }else{
                oH2s[i].className = ""
            }
        }
    }
}
```



# BOM对象

## 什么是BOM

- 浏览器对象模型（Browser Object Model）。主要用在客户端浏览器的管理。
- 一直没有被标准化，不过各个主流浏览器都支持BOM，浏览器提供商会按照各自的想法随意去扩展它
- 它使 javascript 有能力与浏览器进行“对话”。



## window对象

- BOM 的核心对象是 `window`，它表示浏览器的一个实例。是客户端浏览器对象模型的基类。
- 在浏览器中，`window` 对象有双重角色，它既是通过 JavaScript 访问浏览器窗口的一个接口，又是 ECMAScript 规定的 `Global` 对象。
- 这意味着在网页中定义的任何一个对象、变量和函数，都以 `window` 作为其 `Global` 对象，因此有权访问 `isNaN()`、`isFinite()`、`parseInt()`、`parseFloat()` 等方法



### 访问客户端对象

使用 window对象可以访问客户端其他对象,这种关系构成浏览器对象模型。

- navigator:包含客户端有关浏览器的信息
- screen：包含客户端屏幕的信息
- history：包含浏览器窗口访问过的URL信息
- location：包含当前网页文档的URL信息
- document：包含整个HTML文档，可以用来访问文档内容及其所有页面元素







### 打开和关闭窗口

- 使用 `window.open()` 方法既可以导航到一个特定的 URL，也可以打开一个新的浏览器窗口。
- 这个方法可以接收3个参数：要加载的URL、窗口目标、一个特性字符串控制窗口外观
  - url：需要载入的地址
  - 新窗口的名称：_self _blank
  - 新窗口的属性：窗口的大小 位置。例如：`width=300,height=300,left=200,top=100`
- `窗口名.close()`可以关闭窗口，如果关闭自身 那就使用 window.close()。`close()` 方法仅适用于通过 `window.open()` 打开的弹出窗口

```js
var newWin = null;
oBtn1.onclick = function () {
    // 当书写新窗口打开 并且书写窗口大小位置的时候，会开一个新的浏览器窗口
    // window.open("http://www.baidu.com","_blank","width=300,height=300,left=200,top=100");

    // 当不写第三个参数的时候，_blank是打开一个新的标签页
    // window.open 返回一个值代表的是这个窗口，可以供我们对窗口操作
    newWin = window.open("http://www.baidu.com","_blank");
}

oBtn2.onclick = function () {
    // 关闭新窗口
    newWin.close();

    // 关闭自身
    // window.close();
}
```





### 超时调用和间歇调用（定时器）

JavaScript 是单线程语言，但它允许通过设置超时值和间歇时间值来调度代码在特定的时刻执行。前者是在指定的时间过后执行代码，而后者则是每隔指定的时间就执行一次代码。

#### 超时调用

- 超时调用需要使用 `window` 对象的 `setTimeout()` 方法，能够在指定的时间段后执行特定代码

- ```
  var TimerID = setTimeout(code,delay,....);
  ```

  - 参数code表示要延迟执行的字符串型代码，将在Window环境中执行,如果包含多个语句,应该使用分号进行分隔
  - delay表示延迟时间,以毫秒为单位
  - setTimeout方法的第一个参数虽然是字符串，但是也可以是一个函数，一般建议把函数作为参数传递给setTimeout方法，等待延迟调用
  - 如果setTimeout方法的第1个参数是一个函数,则 setTimeout方法第二个参数之后可以接收任意多个参数,这些参数将作为该函数的参数使用

- 该方法返回值是一个 TimerID,这个ID编号指向延迟执行的代码控制句柄。如果把这个句柄传给 clearTimeout方法,则会取消代码的延迟执行

```js
 		//写法1
    setTimeout('var a = 1;alert(a);',2000);

    //写法2
    setTimeout(function(){
        var a = 2;
        alert(a);
    },2000)

    // 写法3
    function fn(){
        var a = 2;
        alert(a);
    }
    setTimeout(fn,2000);

    //传参方法1
    function fn(a){
        alert(a);
    }
    // setTimeout('alert(1)',2000)
    setTimeout('fn("传参")',2000)

    //传参方法2
    function f(name,age) {
        alert(name+age)
    }
    setTimeout(f,2000,"老李",20);
```



#### 取消超时调用

利用`cleartimeout`方法在特定的条件下清除延迟处理代码。方法的参数是setTimeout方法的句柄

```js
 oBtn1.onclick = function () {
            //setTimeout返回一个句柄 是一个id  可以通过这个id（唯一的） 来控制当前的定时器
            timer1 = setTimeout(function(){
                alert("执行1");
            },2000)
            timer2 = setTimeout(function(){
                alert("执行2");
            },4000)
            console.log(timer1);
            console.log(timer2);
        }
        oBtn2.onclick = function () {
            //向clearTimeout方法传入要取消的超时调用计时器的句柄 就可以取消
            clearTimeout(timer1)
        }
```

#### 超时调用练习

<!DOCTYPE html>

```js
var oBox = document.getElementById("box");
    var oCon = document.getElementById("con");
    var timer = null;
    oBox.onmouseenter = function(){
        /*
        * 每次鼠标移入的时候，可能还有一个移出的计时器在执行，那么就有可能等计时器执行结束后，触发消失效果
        * 所以每次移入的时候，要清除计时器
        * */
        clearTimeout(timer);
        oCon.style.display = "block";
    }
    oBox.onmouseleave = function () {
        timer = setTimeout(function(){
            oCon.style.display = "none";
        },1000)
    }
```





#### 间歇调用

- setInterval()方法能够周期性执行指定的代码，如果不加以处理，那么该方法将会被持续执行，直到浏览器窗口关闭或者跳转到其他页面为止。
- `var TimerID = setInterval(code,interval);`
- 该方法的用法与`setTimeout()`方法基本相同,其中参数code表示要周期执行的代码字符串,参数interval表示周期执行的时间间隔,以毫秒为单位
- 该方法返回值是一个TimerID,这个ID编号指向对当前周期函数的执行引用,利用该值对计时器进行访问,如果把这个值传递给clearTimeout()方法,则会强制取消周期性执行的代码。

```js
 <button id="btn">按钮1</button>
    <script>
        var oBtn = document.getElementById("btn");
        var a = 0;
        var timer = setInterval(function () {
            a ++;
            alert(a);
        },2000)

        oBtn.onclick = function(){
            clearInterval(timer)
        }
    </script>
```

#### 取消间歇调用

利用`clearInterval`方法在特定的条件下清除延迟处理代码。方法的参数是setInterval方法的句柄。

- 练习1

  书写一个倒计时5秒到达战场

  ![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggpceq20xvg30ly0dwn3a.gif)

```js
 var oNum = document.getElementById("num");
    var oBox = document.getElementById("box");
    var num = 5;
    var timer = setInterval(function(){
        num --;
        if (num <= 0){
            clearInterval(timer);
            oBox.innerHTML = "全局出击";
        }
        oNum.innerHTML = num;
    },1000)
```





练习2

动画：点击按钮，让元素向移动

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggpchbmti0g31dw0dswhg.gif)

```js
  <script>
        var oBtn = document.getElementById("btn");
        var oCon = document.getElementById("con");
        var num = 0;
        var timer = null;

        //css3方法
        // oBtn.onclick = function () {
        //     oCon.style.left = "1000px";
        // }

        //js方法
        oBtn.onclick = function () {
            clearInterval(timer);  //先清楚下计时器，每次点击都会生成一个计时器（不会覆盖）
            timer = setInterval(function(){
                num += 3;
                //判断临界值
                if(num >= 1000){
                    //一般情况下，书写临界值使用大于等于，并在判断语句中固定好最终的值
                    num = 1000;
                    clearInterval(timer)
                }
                //一般在判断临界值之后，在最后赋值操作
                oCon.style.left = num + "px";
            },16)
        }
    </script>
```

上面不先清计时器的话，每次点击生成一个新计时器，num每点击一次在原基础上累加，只会跑的越来越快





#### 计时器函数的this指向

- 计时器函数是在window中运行的 计时器中的this基本上都是指向window的



#### 计时器面试题

- 面试题1

- ```js
  //计时器函数和事件一样 都是异步代码 。让同步代码（for）执行完成以后 才会去执行异步代码,当去执行异步代码的时候 for已经执行完毕 i此时是5了。
  for (var i = 0; i < 5; i++) {
     setTimeout(function () {
         console.log(i);  // 5，5，5，5，5
     },0)
  }
  ```

- 面试题2

  对上边的代码进行修改，让弹出 0,1,2,3,4

  ```js
  for (var i = 0; i < 5; i++) {
      (function fn(i) {
          //只要在这个函数中 出现i  那么就不会去使用for循环的i
          // 形参i 其实就是这个作用域的变量
          setTimeout(function () {
              console.log(i);
          },0)
      })(i);
  }
  ```

计时器不会覆盖，计时器是异步代码





## navigator对象

### navigator概念

- navigator对象存储了与浏览器相关的基本信息,如名称、版本和系统等。
- 通过 window.navigator可以引用该对象,并利用它的属性来读取客户端基本信息。
- 常见属性：
  - onLine:表示浏览器是否连接到因特网
  - platform:浏览器所在的系统平台
  - userAgent:浏览器的用户代理字符串

### 浏览器检测方法

检测浏览器类型的方法有多种,常用的方法包括两种:**特征检测法**和**字符串检测法**,这两种方法都存在各自的优点与缺点,用户可以根据需要进行选择.

- 特征检测法
  - 特征检测法就是根据浏览器是否支持特定功能来决定相应操作的方式。这是一种非精确判断法。但却是最安全的检测方法
  - 因为准确检测浏览器的类型和型号是一件很困难的事情,而且很容易存在误差,如果不关心浏览器的身份,仅仅在意浏览器的执行能力,那么使用特征检测法就完全可以满足需要
  - 当使用一个对象、方法或属性时,先判断它是否存在,如果存在,则说明浏览器支持该对象、方法属性,那么就可以放心使用

```js
  if (document.documentElement){
      var w = document.documentElement.clientWidth
  } else{
      var w = document.body.clientWidth
  }
```

- 字符串检测法
  - 客户端浏览器每次发送HTTP请求时,都会附带有一个user-agent(用户代理)字符串,对于Web开发人员来说,可以使用用户代理字符串检测浏览器类型
  - userAgent字符串包含了web浏览器的大量信息，如浏览器的名称和版本。

```
​```js
var ua = navigator.userAgent.toLowerCase();
var info = {
    ie:/msie/.test(ua) && !/opera/.test(ua),
    op:/opera/.test(ua),
    sa:/version.*safari/.test(ua),
    ch:/chrome/.test(ua),
    ff:/gecko/.test(ua) && !/webkit/.test(ua)
}
info.ie && console.log("ie");
info.op && console.log("op");
info.sa&& console.log("sa");
info.ch && console.log("ch");
info.ff && console.log("ff");
​```
```

### 操作系统检测方法

- navigator.userAgent返回值一般都会包含操作系统的基本信息,不过这些信息比较散乱,没有统一的规则。

- 用户可以检测一些更为通用的信息,如检测是否为 Windows系统,或者为 Macintosh系统,而不去分辨操作系统的版本号

- 例如,如果仅检测通用信息,那么所有 Windows版本的操作系统都会包含"Win”字符串,所有Macintosh版本的操作系统都包含有"Mac”字符串,所有Umix版本的操作系统都包含有"X11”,而 Linux操作系统会同时包含"X11”和" Linux

  ```js
  var isWin = (navigator.userAgent.indexOf("Win") != - 1);
      // 如果是Windows系统，则返回true
  var isMac = (navigator.userAgent.indexOf("Mac") != - 1); 
      // 如果是Macintosh系统，则返回true 
  var isUnix = (navigator.userAgent.indexOf("X11") != - 1); 
      // 如果是UNIX系统，则返回true
  var isLinux = (navigator.userAgent.indexOf("Linux") != - 1); 
      // 如果是Linux系统， xc则返回true
  ```

## location对象

- location对象存储了与当前文档位置(URL)相关的信息,简单地说就是网页地址字符串,使用 window对象的loaction属性可以访问

- location对象定义了8个属性,其中7个属性可以获取当前URL的各部分信息,另一个属性(href)包含了完整的URL信息

  - href : 声明或获取当前文档完整的URL  **跳转** 

  - protocol:协议部分包括后缀的冒号。例如http:

  - host:主机和端口名称 [www.baidu.com:8080](http://www.baidu.com:8080/)

  - hostname:主机名称

  - port：端口号

  - pathname 路径部分

  - search：url的查询部分，包括前导问号

  - hash：锚部分包括前导 #

    ```js
    // location对象属性(哈希值在地址栏中不会影响页面的跳转 浏览器在请求服务器时是不管他的)
    console.log(location.hash);
    
    // host:主机名：端口号
    console.log(location.host);//localhost:63342
    
    //hostname:主机名
    console.log(location.hostname);//localhost
    
    // prot：端口号
    console.log(location.port);//63342
    
    // pathname:路径名  在服务器中 当前页面的路径名称
    console.log(location.pathname);
    
    // href：url地址 完整路径
    console.log(location.href);
    
    // search 查询字符串  路径问号后跟的数据
    console.log(location.search);
    ```

- location还定义两三方法：reload()和replace() assign()

  - assign:和href相当 **跳转**

  - reload:重新加载文档 **刷新**

  - replace：可以装载一个新文档而无须为它创建一个新的历史记录。替换当前文档的历史记录 **替换地址回不去**了

    ```js
    // location的方法
    var oBtn1 = document.getElementById("btn1");
    var oBtn2 = document.getElementById("btn2");
    var oBtn3 = document.getElementById("btn3");
    oBtn1.onclick = function () {
       location.assign("http://www.baidu.com");
    }
    oBtn2.onclick = function () {
       location.replace("http://www.baidu.com");
    }
    oBtn3.onclick = function () {
       // location.reload(true);//硬刷新
       location.reload(false);
    }
    ```

- 练习

  倒计时跳转

  ![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1ggpd6iri81g311z0dw000.gif)

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>倒计时跳转</title>
  </head>
  <body>
  <button id="btn">点击我注册成功</button>
  <h2><span id="time">3</span>秒后进行跳转</h2>
  <script>
      var oBtn = document.getElementById("btn");
      var oTime = document.getElementById("time");
      var timer = null;
      oBtn.onclick = function () {
          var reduceTime = 3;
          timer = setInterval(function () {
              reduceTime --;
              oTime.innerHTML = reduceTime;
              if(reduceTime <=0 ){
                  clearInterval(timer)
                  // location.href 也可以设置页面跳转
                  // location.href = "http://www.baidu.com";
                  // location.assign("http://www.baidu.com");
                  location.replace("http://www.baidu.com");
                  // window.open("http://www.baidu.com");
              }
  
          },1000)
      }
  </script>
  </body>
  </html>
  ```

## history对象

- history对象存储了客户端浏览器的浏览历史，通过window对象的history属性可以访问该对象
- **在历史记录中后退：window.history.back(),相当于返回**
- **在历史记录中前进：window.history.forward(),相当于前进**
- 移动到指定的历史记录点，使用go方法从当前会话的历史记录中加载页面。上一页就是-1，上上一页就是-2
- length属性可以了解历史记录栈中一共有多少页 window.history.length

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>01</title>
</head>
<body>
<h1>01</h1>
<a href="02.html">02</a>
<a href="03.html">03</a>
<button id="forward">前进</button>
<button id="back">后退</button>
<button id="go">走你</button>
<script>
    var oForward = document.getElementById("forward");
    var oBack= document.getElementById("back");
    var oGo = document.getElementById("go");
    oForward.onclick = function () {
        history.forward();
    }
    oBack.onclick = function () {
        history.back();
    }
    oGo.onclick = function () {
        history.go(-2)
    }
</script>
</body>
</html>
```







# 脚本化CSS

## 设计大小

### offsetWidth和offsetHeight（border-box的宽高）

- 使用offsetWidth和offsetHeight属性可以获取元素的尺寸(content+padding+border)
- offsetWidth表示元素在页面中所占用的总宽度，offsetHeight表示元素在页面中所占用的总高度。
- offsetWidth和offsetHeight是获取元素宽高最好的方法
- 当元素隐藏显示时，即设置样式属性display的值为none时，offsetWidth和offsetHeight返回的值是0

```html
 <style>
        #box{
            width: 100px;
            height: 100px;
            background-color: red;

            padding: 10px;
            border: 1px solid #000;
            margin: 5px;

            /*display: none;*/
        }
    </style>
</head>
<body>
    <button id="btn">点我啊</button>
    <div id="box"></div>
    <script>
        var oBtn = document.getElementById("btn");
        var oBox = document.getElementById("box");
        oBtn.onclick = function(){
            /*offsetWidth 获取元素的宽度 border-box*/
            console.log(oBox.offsetWidth);//122
        }
    </script>
```

### clientWidth和clientHeight（padding-box的宽高）可视部分视口（窗口）

- 获取元素可视部分的宽度，即css的content和padding属性的和，不包含任何能滚动的区域和边框。

```html
<style>
        #box{
            width: 100px;
            height: 100px;
            background-color: red;

            padding: 10px;
            border: 1px solid #000;
            margin: 5px;

            /*display: none;*/
        }
    </style>
</head>
<body>
    <button id="btn">点我啊</button>
    <div id="box"></div>
    <script>
        var oBtn = document.getElementById("btn");
        var oBox = document.getElementById("box");
        oBtn.onclick = function(){
            /*clientWidth 获取元素的宽度 padding-box*/
            console.log(oBox.clientWidth);//120
        }
    </script>
```

### scrollWidth和scrollHeight（元素可以滚动的内容的宽度）

- 元素包含内容的完全宽度和padding

```html
 <style>
        #box{
            width: 100px;
            height: 100px;

            padding: 10px;
            border: 1px solid #000;
            margin: 5px;

            overflow: auto;

        }
        .con{
            width: 1000px;
            height: 100px;
            background-color: red;
        }
    </style>
</head>
<body>
    <button id="btn">点我啊</button>
    <div id="box">
        <div class="con"></div>
    </div>
    <script>
        var oBtn = document.getElementById("btn");
        var oBox = document.getElementById("box");
        oBtn.onclick = function(){
            /*scrollWidth 获取元素的宽度 自身的宽度加溢出一侧的padding*/
            console.log(oBox.scrollWidth);//1010
        }
    </script>
```



## 设计位置

### 使用offsetLeft和offsetTop（距离包含块的距离）

- offsetLeft和offsetTop属性返回当前元素的偏移位置。
- DOM标准模式以最近的定位元素（包含块）为参考进行偏移的位置

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>设计位置-offsetLeft和offsetTop</title>
    <style>
        #outer{
            width: 500px;
            height: 500px;
            margin: 50px;
            overflow: hidden;
            background-color: red;
            padding: 10px;
            border: 1px solid #000;
            position: relative;
        }
        #inner{
            width: 300px;
            height: 300px;
            background-color: yellow;
            margin: 20px;
            padding: 10px;
            border: 1px solid #ccc;
            /*position: relative;*/

        }
        #con{
            width: 100px;
            height: 100px;
            margin: 10px;
            /*position: absolute;*/
            /*left: 40px;*/
            /*top: 40px;*/
            background-color: #0ee69c;
        }
    </style>
</head>
<body>
<div id="outer">
    <div id="inner">
        <div id="con"></div>
    </div>
</div>
<script>
    var oCon = document.getElementById("con");
    console.log(oCon.offsetLeft);
</script>
</body>
</html>
```

### offsetParent（获取包含块）

- offsetParent属性表示最近的上级定位元素（包含块）

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>offsetParent</title>
    <style>
        #outer{
            width: 500px;
            height: 500px;
            margin: 50px;
            overflow: hidden;
            background-color: red;
            padding: 10px;
            border: 1px solid #000;
            position: relative;
        }
        #inner{
            width: 300px;
            height: 300px;
            background-color: yellow;
            margin: 20px;
            padding: 10px;
            border: 1px solid #ccc;
            position: relative;

        }
        #con{
            width: 100px;
            height: 100px;
            margin: 10px;
            /*position: absolute;*/
            /*left: 40px;*/
            /*top: 40px;*/
            background-color: #0ee69c;
        }
    </style>
</head>
<body>
<div id="outer">
    <div id="inner">
        <div id="con"></div>
    </div>
</div>
<script>
    var oCon = document.getElementById("con");
    console.log(oCon.offsetParent);
</script>
</body>
</html>
```

**获取con距离屏幕边缘的距离**

```js
var l = 0;//定义一个值用来储存left
while(oCon){ //第二次循环开始ocon变成了包含块，一直到null循环停下
    l += oCon.offsetLeft; // oCon到包含块的距离与包含块到自己包含块的距离累加
    oCon = oCon.offsetParent; //这时的 oCon是ocon的包含块了
}
console.log(l)
```



### scrollLeft和scrollTop（滚动条滚过去的距离）

- 使用scrollLeft和scrollTop可以读写 **移出可视区域外面的宽度和高度**  可获取可设置
- scrollLeft：读写元素左侧已经滚过的距离
- scrollTop：读写元素顶部已滚动的距离

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>scrollLeft和scrollTop</title>
    <style>
        #outer{
            width: 200px;
            height: 200px;
            border: 1px solid #000;
            overflow: auto;
        }
        #con{
            width: 2000px;
            height: 2000px;
            background-color: pink;
        }
    </style>
</head>
<body>
<button id="btn">点一哈</button>
<div id="outer">
    <div id="con"></div>
</div>
<script>
    var oOuter = document.getElementById("outer");
    var oBtn = document.getElementById("btn");
    oBtn.onclick = function () {
        // oOuter.scrollLeft = 200;
        console.log(oOuter.scrollLeft);
    }

</script>
</body>
</html>
```

### clientLeft和clientTop（获取边框的宽度）

- 边框的大小

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>设计位置-clientLeft和clientTop</title>
    <style>
        #outer{
            width: 500px;
            height: 500px;
            margin: 50px;
            overflow: hidden;
            background-color: red;
            padding: 10px;
            border: 1px solid #000;
            position: relative;
        }
        #inner{
            width: 300px;
            height: 300px;
            background-color: yellow;
            margin: 20px;
            padding: 10px;
            border: 1px solid #ccc;
            /*position: relative;*/

        }
        #con{
            width: 100px;
            height: 100px;
            margin: 10px;
            /*position: absolute;*/
            /*left: 40px;*/
            /*top: 40px;*/
            background-color: #0ee69c;
        }
    </style>
</head>
<body>
<div id="outer">
    <div id="inner">
        <div id="con"></div>
    </div>
</div>
<script>
    var oCon = document.getElementById("con");
    console.log(oCon.clientLeft);
    console.log(oCon.parentNode.clientLeft);
    console.log(oCon.parentNode.parentNode.clientLeft);
</script>
</body>
</html>
```



## 系统滚动条位置

### 获取系统滚动条位置

**self.pageYOffse获取系统滚动条位置的新方法**

```js

//兼容性封装
<script>
document.onclick = function(){  //可以不要点击事件
    function getDocScroll() {
        var h = document.documentElement;
        var b = document.body;
        var y = self.pageYOffset||
                h.scrollTop||
                b.scrollTop;
        var x = self.pageXOffset||
                h.scrollLeft||
                b.scrollLeft;

        return {
            x:x,    //返回对象
            y:y
        }
    }
    console.log(getDocScroll().y)  //
}        
</script>
```

### 设置系统滚动条位置

- 使用window对象的scrollTo(x,y)方法可以定位滚动条的位置
- 其中参数x可以定位页面内容在x轴方向上的偏移量。
-  window.scrollTo设置系统滚动条位置的新方法

```js

//设置滚动条位置兼容性封装
<script>
   document.onclick = function(){   //可以不要点击事件
        function setDocScroll(x,y) {
            if(arguments.length === 1 && typeof x === "number"){
                window.scrollTo(x,0);
                document.documentElement.scrollLeft = x;
                document.body.scrollLeft = x;
            }
            if(arguments.length === 2 && typeof x === "number" && typeof y === "number"){
                window.scrollTo(x,y);
                document.documentElement.scrollLeft = x;
                document.documentElement.scrollTop = y;
                document.body.scrollLeft = x;
                document.body.scrollTop = y;
            }
        }
        setDocScroll(0,300)
    }
</script>
```



### 获取窗口的大小

- 获取 `<html>`标签的clientWidth和clientHeight属性，就可以知道浏览器窗口的可视宽度和高度。
- 而获取html标签的脚本表示为`document.documentElement`
- 所以获取窗口宽度是 `document.documentElement.clientWidth`
- 在怪异模式下，body是最顶层的可视元素，所以在怪异模式下获取窗口的宽度是 `document.body.clientWidth`
- 兼容性代码就是：`document.documentElement.clientWidth||document.body.clientWidth;`

```js
//获取窗口的大小兼容性封装
<script>
    document.onclick = function(){
        function getWinSize(){
            var h = document.documentElement;
            var b =document.body;
            return {
                y:h.clientHeight || b.clientHeight,
                x:h.clientWidth || b.clientWidth
            }
        }
   			console.log(getWinSize().X)
        console.log(getWinSize().y)
    }
</script>
```



### 获取整个文档全文的宽高

```js
//获取整个文档大小兼容性封装
<script>
    document.onclick = function(){
        function getDocSize(){
            var h = document.documentElement;
            var b =document.body;
            return {
                y:h.offsetHeight || b.offsetHeight,
                x:h.offsetWidth || b.offsetWidth
            }
        }
  			console.log(getDocSize().x)
        console.log(getDocSize().y)
    }
</script>
```



## getBoundingClientRect

- obj.getBoundingClientRect用于获取某个元素相对于视窗的位置集合。集合中有top, right, bottom, left等属性
- rectObject.top：元素上边到视窗上边的距离;
- rectObject.right：元素右边到视窗左边的距离;
- rectObject.bottom：元素下边到视窗上边的距离;
- rectObject.left：元素左边到视窗左边的距离;
- ie9以上支持width/height属性。

```
oBox.getBoundingClientRect().top;        // 元素上边距离页面上边的距离

oBox.getBoundingClientRect().right;      // 元素右边距离页面左边的距离

oBox.getBoundingClientRect().bottom;      // 元素下边距离页面上边的距离

oBox.getBoundingClientRect().left;        // 元素左边距离页面左边的距离

```



## 封装my.js

```js
/*
* 作者：李沛华
* 时间：2020-7-18
* 内容：个人封装函数库
*
*   ;是为了防止其他库末尾不加分号 引起错误
*   + - ~ ！ 等一元运算符 代表把匿名函数括起来
*   在匿名函数中定义一个对象，把所有的方法放到这个对象上，方便识别和管理
* */

;+function(w){
    w.my = {};


    /*
    *   getDocScroll方法：获取系统滚动条的位置
    *   @param {} 无参数
    *   @return {Object} x：横向滚动条  y：竖向滚动条
    * */
    w.my.getDocScroll = function(){
        var h = document.documentElement;
        var b = document.body;
        var y = self.pageYOffset||
                h.scrollTop||
                b.scrollTop;
        var x = self.pageXOffset||
                h.scrollLeft||
                b.scrollLeft;
        return {
            x:x,
            y:y
        }
    }


    /*
   *   setDocScroll方法：设置系统滚动条的位置
   *   @param {x:[number],y:[number]}  x：横向滚动条  y：竖向滚动条
   *   @return {} 无
   * */
    w.my.setDocScroll = function(x,y){
        if(arguments.length === 1 && typeof x === "number"){
            window.scrollTo(x,0);
            document.documentElement.scrollLeft = x;
            document.body.scrollLeft = x;
        }
        if(arguments.length === 2 && typeof x === "number" && typeof y === "number"){
            window.scrollTo(x,y);
            document.documentElement.scrollLeft = x;
            document.documentElement.scrollTop = y;
            document.body.scrollLeft = x;
            document.body.scrollTop = y;
        }
    }

    
     /*
   *   getWinSize方法：获取视口的大小
   *   @param 无
   *   @return {Object} x：宽  y：高
   * */
    w.my.getWinSize = function(){
        var h = document.documentElement;
        var b =document.body;
        return {
            y:h.clientHeight || b.clientHeight,
            x:h.clientWidth || b.clientWidth
        }
    }
        

      /*
   *   getDocSize方法：获取文档的大小
   *   @param 无
   *   @return {Object} x：文档宽  y：文档高
   * */

    w.my.getDocSize = function(){
        var h = document.documentElement;
        var b =document.body;
        return {
            y:h.offsetHeight || b.offsetHeight,
            x:h.offsetWidth || b.offsetWidth
        }
    }
}(window)
```



### 回到顶部效果通用版

```js
<script src="./my.js"></script>
<script>
    var oBack = document.querySelector(".back");
    var oBox = document.getElementById("box");
    var timer = null;

    //window的scroll事件触发
    window.onscroll = function(){
        if (my.getDocScroll().y >= 200){ //获取当前滚动条位置作比较
            oBack.style.display = "block";
        }else{
            oBack.style.display = "none";
        }
    }


    oBack.onclick = function(){
        clearInterval(timer);
        //起始位置
        var startScroll = my.getDocScroll().y;
        //结束位置
        var endScroll = 0;
        //起始步数
        var startStep = 0;
        //总步数
        var endStep = 40;
        //每一步所走的距离
        var everyStep = (endScroll - startScroll) / (endStep - startStep); //为负数

        //执行动画
        timer = setInterval(function(){
            //让起始步数累加，通过起始步数和结束步数的对比 判断临界值
            startStep ++; //纯临界值判断作用
            if(startStep >= endStep){
                clearInterval(timer);
            }
            startScroll += everyStep; //当前位置加负数每一步距离
            //每次计时器执行结束，就赋值操作
            my.setDocScroll(0,startScroll);
        },1000/60)
    }
</script>
```





# DOM进阶

## 节点操作

### 创建节点document.createElement（元素节点）

- 使用document对象的createElement方法能够根据参数指定的标签名创建一个新的元素。并返回对新元素的引用
- 使用creatElement方法创建的新元素不会被自动添加到文档里，需要使用appendChild等方法

```js
var newLi=document.createElement("li");//插入li节点
```



### 创建文本节点newLi.innerHTML

- 使用document对象的createTextNode()方法可创建文本节点

- 参数是一个字符串

- 创建的文本节点需要使用appendChild等方法才能插入到元素节点中

- 当然也可以使用innerHTML方法给元素节点添加内容

  ```js
  //1.
  var newText=document.createTextNode("新的li");
  newLi.appendChild(newText);
  
  //2.
  newLi.innerHTML="新的li";
  ```

  

### 插入节点

#### appendChild()  父级调用

- appendChild()方法可以向当前节点的字节点列表的末尾添加新的节点

- 如果文档树中已经存在参数节点，则将从文档树中删除，然后重新插入新的位置

  ```js
  oBox.appendChild(newLi); //插到父级ul oBox中去了
  ```

  

#### insertBefore()

- 使用insertBefore(newChild,oldChild)方法可以在已有的子节点前插入一个新的子节点

- newChild表示新插入的节点，oldChild用于指定插入节点的后边的相邻位置。

- 插入成功以后，该方法返回新插入的节点

- insertBefore可以把指定元素及其所包含的所有子节点都一起插入到指定位置中。同时会先删除移动的元素，再重新插入

  ```js
  oBox.insertBefore(oLis[3],oLis[1]) // 旧的元素也可以插 在下标3前面插了
  oBox.insertBefore(newLi,oLis[1])  //插入新元素
  ```

  

### 复制节点cloneNode()

- cloneNode()方法可以创建一个节点的副本
- 参数 true (深复制)，复制整个节点和里面的内容zs； false (浅复制)，只复制节点不要里面的内容
- 复制后的新节点，也不会被自动插入到文档，需要用到之前的方法去插入
- 由于复制的节点会包含原节点的所有特性，如果原节点中包含id属性，就会出现id属性值重叠的情况。为了避免潜在的冲突，应修改其中某个节点的id属性值

```js
var newBox = oBox.cloneNode(true); //辅助oBox
newBox.id = newBox.id + i;
```



### 删除节点removeChild() 

**被删除元素的父级调用**

- removeChild方法可以从子节点列表中删除某个节点

- 如果删除成功，则返回被删除的节点，如果失败则返回null

- 当remove 删除节点的时候，该节点所包含的所有子节点将同时被删除

  ```js
  var oLisDeles = document.querySelectorAll("#box li a")
  for (var i = 0; i < oLisDeles.length; i++) {
      oLisDeles[i].onclick = function(){
          /*
          * 被删除元素的父级 调用removeChild方法  removeChild方法的参数是被删除的元素
          * */
          this.parentNode.parentNode.removeChild(this.parentNode);//a-li-ul.removeChild(li)
      }
  }
  ```

  



### 替换节点replaceChild()

- replaceChild(new,old)方法可以将某个子节点替换为另一个
- 替换节点替换的是所有子节点以及包含的所有内容
- 其中参数new为指定的新节点，old代表被替换的节点
- 如果替换成功则返回被替换的节点，否则返回null

```js
 oBox.replaceChild(newLi,oLis[1])
 oBox.replaceChild(oLis[3],oLis[1])
```





## 元素内容操作

##### innerHTML 和 innerText 

```js
<div id="box">
    <p>新节点</p>
    <a href="##">删除</a>
</div>

<script>
        var oBox=document.getElementById("box");
        console.log(oBox.innerHTML);   //获取字符串会带标签
        oBox.innerHTML="<li>我是新的内容</li>"   // 设置可以解析标签并且会替换掉原来的内容(BOX内的)
      	console.log(oBox.innerHTML);
      	
        console.log(oBox.innerText); //获取内容不带标签
        oBox.innerText="<li>我是新的内容</li>"//设置解析不了标签 ，也会替换原有内容(BOX内的)
        console.log(oBox.innerText);
</script>
```

##### outerHTML和 outerText

```js
console.log(oBox.outerHTML);       //获取包含BOX本身的标签与内容
oBox.outerHTML = "<li>我是新的内容</li>" // 设置可以解析标签但是包含BOX直接被替换了
console.log(oBox.outerText);      //获取内容不带标签
oBox.outerText = "<li>我是新的内容</li>"   //设置解析不了标签 ，但是也会替换掉整个BOX
```



##### textContent

```js
console.log(oBox.textContent); //获取内容不带标签
oBox.textContent = "<li>我是新的内容</li>" //设置解析不了标签 ，也会替换原有内容
console.log(oBox.textContent);
```





##### textContent和innerText的区别

- textContent 会获取style= “display:none” 中的文本，而innerText不会
- innerText不会理会html格式，直接输出不换行的文本 ，textContent会根据标签里面的元素独立一行
- innerText 对IE的兼容性较好 ，textContent虽然作为标准方法但是只支持IE8+以上的浏览器
- 兼容性处理



```js
//兼容性处理
function setOrGetContent(node,content) {
if(arguments.length === 1){
    //代表当前是读取操作
    if(node.textContent){
        //只要能拿到这个dom对象的textContent属性值，代表当前用户是高级浏览器
        return node.textContent;
    }else {
        //代表拿不到  那就是低级浏览器
        return node.innerText;
    }
}else if(arguments.length === 2){
    //代表写入操作
    if(node.textContent){
        //代表高级
        node.textContent = content;
    }else {
        //代表低级
        node.innerText = content;
    }
}
```





## 属性节点

### 创建属性节点document.createAttribute("study");

```
oBtn.onclick = function(){
    //创建一个属性节点 没有值
    var myAttr = document.createAttribute("study");
    console.log(myAttr)
    //通过value属性给属性节点赋值
    myAttr.value = "js";
    console.log(myAttr)
    //setAttributeNode 是给某一个元素设置一个属性节点
    oBox.setAttributeNode(myAttr);
}
```



### 读取属性值  getAttribute

### 设置属性值  setAttribute("select",true);

### 删除属性  removeAttribute("study");

```js
<script>
    var oBtn = document.getElementById("btn");
    var oBox = document.getElementById("box");
    oBox.study = "css";
    oBtn.onclick = function(){
        //元素自有的属性可以直接通过点语法来获取  class例外 需要使用className
        console.log(oBox.id);
        //非自有属性，必须通过getAttribute属性来获取
        //如果不是自有属性，通过点语法获取的是 js对象上的属性  而不是js对象对应的元素上的属性
        console.log(oBox.study)
        console.log(oBox.getAttribute("study"));

        //如果直接通过点语法设置属性，设置的仅仅是js对象的属性，而不是元素上的属性
        oBox.select = true;
        oBox.setAttribute("select",true);

        //removeAttribute只能移出元素上的属性节点
        oBox.removeAttribute("study");
    }
</script>
```





## 自定义属性

HTML5允许用户为元素自定义属性，但是要求添加前缀data

```js
oBtn.onclick = function(){
    //直接使用元素节点的dataset属性上  去访问自定属性
    console.log(oBox.dataset);
    console.log(oBox.dataset.study);

    //直接给元素节点的dataset属性添加属性 就可以添加属性节点
    oBox.dataset.select = true;

    //删除 只需要删除dataset上的属性即可
    delete oBox.dataset.study;
}
```





## 文档片段节点（DocumentFragment）

- DocumentFragment是一个虚拟的节点类型，仅仅存在于内存中，没有添加到文档树中，所以看不到渲染效果
- 使用文档碎片的好处，就是避免浏览器渲染和占用资源
- 当文档片段设计完善后，再使用js一次性添加到文档树中显示出来，提高效率，减少页面重绘的次数。解决大量添加节点时候的性能问题
- 使用document.createDocumentFragment()方法创建，使用appendChild等方法插入

```js
// createDocumentFragment创建一个DocumentFragment,它是一个轻量级的文档格式，作用是临时存储节点，等待插入到文档中
// createDocumentFragment是解决大量添加节点时候的性能问题
var fragment = document.createDocumentFragment();
for (var i = 0; i < 100; i++) {
    var newLi = document.createElement("li");
    // textContent给元素插入文本
    newLi.textContent = "hello 6666";
    fragment.appendChild(newLi); //先全部插到fragment中（仅仅存在于内存中，没有添加到文档树中）
}
oBox.appendChild(fragment);//再把fragment插到oBox中
```





# DOM高级

## 事件模型

**基本事件模型:也称为DOM0事件模型（同一元素只能绑定同一事件一次，后绑定的会覆盖前面的）**

**DOM事件模型:**

**ie事件模型**



## 事件流

事件流就是多个节点对象对同一种事件进行响应的先后顺序,主要包括以下3种类型

- 冒泡型 ：指定目标由内到外触发
- 捕获型 ：由外到内触发执行到最特定的目标，也就是事件从上向下进行相应。
- 混合型： w3C的DOM事件模型支持捕获型和冒泡型两种事件流,其中捕获型事件流先发生,然后才发生冒泡型事件流。两种事件流会触及DOM中的所有层级对象,从 document对象开始,最后返回 document对象结束。因此,可以把事件传播的整个过程分为3个阶段
  - **捕获阶段**:事件从 document对象沿着文档树向下传播到目标节点,如果目标节点的任何一个上级节点注册了相同的事件,那么事件在传播的过程中就会首先在最接近顶部的上级节点执行,依次向下传播
  - **目标阶段**:注册在目标节点上的事件被执行
  - **冒泡阶段**:事件从目标节点向上触发,如果上级节点注册了相同的事件,将会逐级响应,依次向上传播





## 绑定事件

### 静态绑定：

把JS脚本作为属性值，直接赋值给事件属性**(直接在元素行内绑定)**

```js
<div onclick="fn()">div</div>
<script>
    function fn() {
        alert(1);
    }
</script>
```



### 动态绑定：（DOM0级）

使用DOM对象的事件属性进行赋值

​    \* DOM0的特点

​    \*  1.绑定事件简单 把事件函数赋值给DOM对象的事件属性上

​    \*  2.DOM0绑定事件事件流是冒泡

​    \*  3.DOM0对同一个元素绑定同一个事件多次，会进行覆盖

​    \*  4.注销DOM0事件 只需要给事件属性赋值为null

```js

oOuter.onclick = function(){
    alert("outer");
}
oInner.onclick = function(){
    alert("inner");
}
oCon.onclick = function(){
    alert("con1");
}
oCon.onclick = function(){
    alert("con2");
}

/*取消事件*/
var oBtn = document.getElementById("btn");
btn.onclick = function () {
    oInner.onclick = null;
}

```



### 事件处理函数：

事件处理函数是一类特殊的函数，与函数直接量结构相同，主要任务是实现事件处理，为异步调用，由事件触发进行相应。

#### 1.DOM2注册事件(事件监听) addEventListener()

```js
/*
* addEventListener()是事件处理函数
*   - type：事件名称
*   - fn: 事件函数
*   - boolean：控制冒泡或捕获  false:冒泡 true就是捕获
*
*   特点：
*       1.可以绑定同一个元素同一个事件多次
*       2.可以控制冒泡或捕获
*       3.DOM2级事件（DOMContentLoaded事件） 只能通过DOM2绑定事件方法绑定
*
* */

oCon.addEventListener("click", function () {
    alert("con")
}, true);

oOuter.addEventListener("click", function () {
    alert("outer1")
}, true);
oOuter.addEventListener("click", function () {
    alert("outer2")
}, false);
oInner.addEventListener("click", function () {
    alert("inner")
}, false);

//执行顺序由外到内先执行捕获，然后由内到外再执行冒泡
```

#### 注册事件注销 removeEventListener（）

```js
//事件函数，方便绑定和移除事件使用
function fn(){
    alert("con")
}
oCon.addEventListener("click",fn,false);

/*
* removeEventListener 注销事件
*   - type 要注销的事件名
*   - fn  要注销事件的函数
* */
oBtn.onclick = function(){
    oCon.removeEventListener("click",fn)
}

```





#### 2.DOM2注册事件IE attachEvent()

```JS
/*
* attachEvent() 低版本IE事件处理函数
*   - type  事件名称 加on
*   - fn 事件函数
*
*   注意：
*       对一个对象绑定用一个事件多次，执行顺序是逆向的（下--上）
*				只支持冒泡
*
* */
oCon.attachEvent("onclick",function(){
    alert("con1")
})
oCon.attachEvent("onclick",function(){
    alert("con2")
})
oInner.attachEvent("onclick",function(){
    alert("inner")
})
oOuter.attachEvent("onclick",function(){
    alert("outer")
})
```



#### DOM2注册事件IE注销  detachEvent

```js
//事件函数，方便绑定和移除事件使用
function fn(){
    alert("con")
}
oCon.attachEvent("onclick",fn);

/*
* detachEvent 注销事件
*   - type 要注销的事件名
*   - fn  要注销事件的函数
* */
oBtn.onclick = function(){
    oCon.detachEvent("onclick",fn)
}
```





#### 绑定事件 注销事件兼容

```js
/*
* 只有当需要DOM2绑定时 才会需要兼容
*   ele：事件绑定的对象
*   type:事件名称
*   fn:事件函数
*   boo：冒泡或捕获的布尔值
* */

function addEvent(ele,type,fn,boo) {
    if (ele.addEventListener){
        ele.addEventListener(type,fn,boo || false);
    }else if(ele.attachEvent){
        ele.attachEvent("on"+type,fn);
    }else{
        ele["on"+type] = fn;
    }
}

 addEvent(oCon,"click",fn,false);


//注销事件兼容
function delet(ele, type, fn) {
    if (ele.removeEventListener) {
        ele.removeEventListener(type, fn);
    } else if (ele.detachEvent) {
        ele.detachEvent(type, fn);
    } else {
        ele["on" + type] = null;
    }
}
delet(oCon, "click", fn, false);
```



## DOMContentLoaded事件

- window.onload当所有的节点和资源加载完成才执行
- 加载需要等待资源加载 并且还只能对window绑定一次
- 解决方法1： 仍然是等资源加载完成才执行
- 解决方法2 使用DOMContentLoaded事件 ：当所有节点加载完毕就可以执行
- DOMContentLoaded是DOM2级的事件 需要使用DOM2的监听方式来绑定事件

```js
window.onload = function(){
    alert("DOM和资源加载完毕")
}
/*
* DOMContentLoaded事件
*   - DOM2级事件 需要使用事件监听来绑定
*   - 当DOM节点加载完成即可执行，不需要等待资源(图片加载)
* */
window.addEventListener("DOMContentLoaded",function(){
    alert("DOM节点加载完毕")
},false)

window.addEventListener("load",function(){

})
```



## event事件对象

- 只要事件发生就会有一个事件对象,每个事件都有自己的对象
- 事件对象保存的是当前事件所有的详情信息
- 兼容性：高版本浏览器和低版本浏览器（ie 678）
- 高版本 事件函数的第一个参数 就是evnet事件对象
- 低版本 在低版本中，window对象上有evnet对象，上边保存的是当前事件的时间对象
- 兼容：

```js
oBox.onclick = function(e){
    var e = e || window.event;
    console.log(e);
}
```



## event事件的鼠标定位

- clientX: 元素（鼠标）距离浏览器视口的距离 ，只和视口有关系，滚动后的不计算
- offsetX:  和当前的目标元素的距离，如果里面还有元素的话鼠标可能相对里面元素计算
- pageX:  距离文档的边缘的距离 会计算滚动条滚过文档的距离
- screenX:  获取距离屏幕边缘的距离（显示器）

```js
oBox.onclick = function (e) {
    var e = e || window.event;
    console.log(e);

    // clientX和clientY：元素（鼠标）距离浏览器视口的距离 ，视口变了也会变不算滚动条 
    // console.log(e.clientX)
    // console.log(e.clientY)
}
```





## 阻止传播  e.stopPropagation()

- ​     e.stopPropagation() 现代浏览器
- ​     e.cancelBubble = true; //ie低版本
- ​    e.stopPropagation?e.stopPropagation():e.cancelBubble = true;
- ​    阻止传播 只会阻止当前事件类型的传播，不会阻止其他类型的传播



```js
//只会阻止自身当前事件类型的传播
//只阻止自身的传播，不影响其他人的传播


oCon.addEventListener("click", function (e) {
    //阻止传播
    e.stopPropagation();
    alert("con")
}, false);

oInner.addEventListener("click", function () {
    alert("inner")
}, false);
oOuter.addEventListener("click", function () {
    alert("outer1")
}, false);
```





## 阻止默认事件   e.preventDefault()

-    有以下操作 系统会给定默认的效果，我们把它称作为默认事件，有时不需要，则要阻止事件
- ​    比如：点击提交，表单会提交  比如cv 比如拉取内容 右键菜单
- ​    e.preventDefault()
- ​    e.returnValue = false;
- ​    当然也有一个粗暴的方法  return false;但是需要注意书写位置，要放在最后边，因为return是直接退出函数
-    但是e.preventDefault的方法 可以放在任意位置 阻止默认事件

```js
//阻止鼠标右键
document.oncontextmenu = function (e) {
    var e = e || window.event;
    e.preventDefault ? e.preventDefault() : e.returnValue = false;
    return false;
}
```







### 键盘事件属性

- 键盘事件定义了很多属性

- keyCode：对应键盘中对应键位的键值

- shiftKey、ctrlKey、altKey是否按下shift strl alt按键 返回布尔值

  

```js
//ctrl c+v 失效
document.onkeydown = function (ev) {
    console.log(ev);
    if (ev.keyCode === 67 && ev.ctrlKey) {
        alert("cv再见");
        ev.preventDefault();
    }
}
```





## 事件委托：

- 事件委托（delegate） 也称为事件托管或者事件代理，就是把目标节点的事件绑定到祖先的节点上
- 这种简单而优雅的事件注册方式是给予事件传播过程中，逐层冒泡总能被祖先元素捕捉到
- 优点：优化代码提升性能，把html和js分离，防止动态添加或删除节点中注册事件的丢失现象

把原本内部目标节点的事件绑定到祖先的节点上，通过冒泡被祖先捕捉到，然后再使用event对象的target获取事件的精准目标

```js
<button id="btn">
添加一个新元素
</button>
<ul id="box">
<li>11</li>
<li>22</li>
<li>33</li>
<li>44</li>
<li>55</li>
</ul>
<script>
  var oBtn = document.getElementById("btn");
  var oBox = document.getElementById("box");
  var oLis = oBox.getElementsByTagName("li");
	oBtn.onclick = function () {
    var newLi = document.createElement("li");
    newLi.innerHTML = "newLi";
    oBox.appendChild(newLi);
}

  oBox.onclick = function (ev) {
      var ev = ev || window.event;
      // ev.target当前时间精确发生的元素
      console.log(ev.target);

      // 在这里this仍然是指向oBox
      // this.style.backgroundColor = "red";

      // 给精确的元素绑定点击事件，但是没有判断   所有元素都能触发
      // ev.target.style.backgroundColor = "red";

      if (ev.target.nodeName.toLowerCase() == "li" ){
          ev.target.style.backgroundColor = "red";
      }
  }
```

 //根据冒泡原理，点击li就是点击了ul 所以如果给ul绑定点击事件，则点击li的时候就会触发ul的点击事件

​    //事件委托，1.减少绑定事件，提高效率  2.可以给未来的元素绑定事件

​      //event事件对象的target属性，就是你的目标元素（鼠标点击精确的元素）



## 滚轮事件

 	谷歌/ie

​      \*  滚轮事件：onmousewheel

​      \*    需要获取event事件对象的 wheeldelta 属性

​      \*    如果wheeldelta > 0 滚轮上滚

​      \*    如果wheeldelta < 0 滚轮下滚

​      \* 火狐：

​      \*  滚轮事件：DOMMouseScroll

​      \*    需要event事件对象的 detail属性

​      \*    如果detail属性 > 0 滚轮下滚

​      \*    如果detail属性 < 0 滚轮上滚

```js
function mouseScroll(e) {
    var e = e || window.event;
    if (e.detail){
        if(e.detail > 0){
            x ++;
        }else{
            x --;
        }
    } else{
        if (e.wheelDelta > 0){
            x --;
        } else{
            x ++;
        }
    }
    oBox.style.width = x + "px";
}
//火狐调用
oBox.addEventListener("DOMMouseScroll",mouseScroll);
// 谷歌调用
oBox.onmousewheel = mouseScroll;

```





## 表单提交

```js
<div class="outer">
  <form action="###">
      <input type="text" name="username" autocomplete="off">
      <button>提交</button>
  </form>
</div>
<script>
  var oForm = document.querySelector("form");
  var oBtn = document.querySelector("form button");
  var oUserName = document.querySelector("input[name=username]");
  document.onkeydown = function(e){
      if(e.keyCode === 13){  //回车键码
          //判断input不为空且全部都是数字 就直接提交
          if(oUserName.value && !isNaN(Number(oUserName.value))){
              // JS提交form表单 使用form元素submit方法
              oForm.submit();
          }else{
              alert("格式不对或者不能为空")
          }
          return false;
      }
  }
```

