# jQuery

jQuery 是一个 JavaScript 库，它通过封装原生的 JavaScript 函数得到一整套定义好的方法



## jQuery的功能和优势

- 像 CSS 那样访问和操作 DOM
- 修改 CSS 控制页面外观
- 简化 JavaScript 代码操作
- 事件处理更加容易
- 各种动画效果使用方便
- 让 Ajax 技术更加完美
- 基于 jQuery 大量插件
- 自行扩展功能插件



## jQuery的使用

### jQuery 下载

- http://jquery.com/download/
- http://www.jq22.com/jquery-info122

### jQuery的使用

### 入口函数，当DOM节点加载完成以后执行

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gdx77xbr0hj315s0gwq3o.jpg)

```js
$(document).ready(function(){
    
})
简写
$(function(){
    
})
```

## js的入口函数

```js
window.onload = function(){ //节点和资源加载完执行
    
}

//通过jQuery的方式创建入口函数,但还是js的入口函数
$(window).load(function(){
    
})


window.addEventListener("DOMContentLoaded",function(){   //节点加载完就执行
    alert("DOM节点加载完毕")      
},false)
```

定义:有一定功能的代码集合体

作用:独立作用域

目的:先加载页面再执行

### js的入口函数和jQuery的入口函数的区别

1.jQuery的入口函数可以写多个,而js的入口函数只能写一个,否则后面的会覆盖前面的

2.执行时间:jQuery的入口函数先执行,js的入口函数后执行

​	jQuery的入口函数要等待页面上的dom树加载完执行

​	js的入口函数要等待页面上所有的资源(dom树,外部图片,外部链接的css/js/图标)都加载完才执行



## $

在没有引入jQuery文件,就会报一个错:$ is not definds

引入jQuery文件后$其实是一个方法

既然$是一个方法,那根据传递的参数不同,效果也不一样

a:如果参数是一个匿名函数,那他就是个入口函数  $(function(){});

b:如果参数是一个字符串,那他就是选择器或者创建元素

如果是选择器,有时候会有参数2,参数2代表在参数2的范围里找到到标签;

创建的元素会存在内存中,要手动添加到页面

```js
$("#objid"); 如果这样写获取到的是document页面中的id为objid的元素
$("#objid", parent.document); 这样写 获取到的是parent.document 里面的 id为objid的元素

$('<a href="https://www.baidu.com"></a>');
```

c:如果是一个dom对象,那他就是把dom对象转换为jQuery对象;

```js
在jQuery文件中有这么一句话
(function(){
    window.jQuery = window.$ = jQuery;
}());
即jQuery和$都成了window的属性,返回值是jQuery;
所以入口函数也可以写成
jQuery(function(){
    
})
```



## jQuery对象模型

### jQuery执行时机

|          | **window.onload**                                        | **$(document).ready()**                               |
| -------- | -------------------------------------------------------- | ----------------------------------------------------- |
| 执行时机 | 必须等待网页全部加载完毕(包括 图片等),然后再执行包裹代码 | 只需要等待网页中的DOM结构 加载完毕,就能执行包裹的代码 |
| 执行次数 | 只能执行一次,如果第二次,那么 第一次的执行会被覆盖        | 可以执行多次,第N次都不会被上 一次覆盖                 |
| 简写方案 | 无                                                       | $(function () { });                                   |

### 代码风格

**jQuery的每一个方法都会返回一个jQuery对象，所以才能进行链式调用，**

**返回的对象要么是自己方法获取的，要么是离他最近的方法获取的**

- jQuery 程序中,不管是页面元素的选择、内置的功能函数,都是美元符号“$”来起始的。

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gdx7atdyr4j30cm04rgln.jpg)

- jQuery类库定义了一个全局函数：`jQuery()`。该函数使用频繁，因此在类库中还给它定义了一个快捷别名：`$`。这是jQuery在全局命名空间中定义的唯一两个变量。

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gdx7ce144qj30eo04tq31.jpg)

- css() 这个功能函数返回的对象其实也就是 jQuery 对象， jQuery 的代码模式是采用的连缀方式,可以不停的连续调用功能函数

  ![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gdx7cxq9lcj30fy00qmx0.jpg)

### jQuery对象和JS对象

#### 什么是JS对象

DOM对象，即是我们用传统的方法(javascript)获得的对象。

#### 什么是jQuery对象

jQuery对象即是用jQuery类库的选择器获得的对象，jQuery对象就是通过jQuery包装DOM对象后产生的对象，它是jQuery独有的。如果一个对象是jQuery对象，那么就可以使用jQuery里的方法

#### jQuery对象和DOM对象的互相转换

**jQuery选择器获取的是一个集合 是一个对象（类数组）对象 （jQuery的对象），也有length属性**

dom对象转换成jQuery对象

$(dom对象)

```js
var oOuter = document.querySelector(".outer");
$(oOuter).css("color", "red");
```

jQuery对象转换成dom对象

1.jQuery对象[下标];

2.jQuery对象.get(下标);

```js
var oOuter = $(".outer")
oOuter[0].style.backgroundColor = "yellow";
oOuter.get(0).style.background = "red";
//因为jQuery其实是一个伪数组,里面存放的是一个个的dom对象,那么就可以通过下标将这些dom取出,取出后就变成了dom对象
```





### 类数组操作

- children() ：返回原始包装集元素的所有不同子元素所组成的新包装集,如果指定了参数，那么该参数也是筛选表达式
- parent() ：返回原始包装集所有元素的唯一直接父元素的新包装集；如果指定了参数，那么该参数也是筛选表达式
- siblings() ：返回原始包装集元素中的所有兄弟元素所组成的新包装集；如果指定了参数，那么该参数也是筛选表达式
- find(String) ：返回原始包装集里与传入的选择器表达式相匹配的所有元素的新包装集，并且原始包装集中的元素的后代也会被传入新的包装集
- prev()取得一个包含匹配的元素集合中每一个元素紧邻的前一个同辈元素的元素集合，可以筛选
- prevAll()前面所有的兄弟元素。可以筛选
- next()取得一个包含匹配的元素集合中每一个元素紧邻的后一个同辈元素的元素集合，可以筛选
- nextAll后面所有的兄弟元素，可以筛选

```js
console.log($(".con").children());//获取选择器元素的所有元素的 子元素节点集合
console.log($(".con").children(".pp"));//可以在children方法中书写选择器 对获取的集合进行筛选

console.log($(".pp").parent());//获取父元素的集合
console.log($(".pp").parent("#con1"));//可以筛选

console.log($(".pp1").siblings()); //获取兄弟元素集合
console.log($(".pp1").siblings(".pp"));//可以筛选

console.log($(".outer").find("span"));//find 寻找后代元素 find中可以直接书写选择器

console.log($("span").prev(""));//取得一个包含匹配的元素集合中每一个元素紧邻的前一个同辈元素的元素集合
console.log($("span").prev(".pp"));//可以筛选

console.log($("p").prevAll());//前面所有兄弟元素
console.log($("p").prevAll("span"));
 
```



#### end()(返回上一次的破坏性操作之前)

```js
$("body").css("background", "green")
.find(".con").css("font-size", "50px")
.children(".pp").css("background", "red")
.end().css("font-size", "25px").end().css("background", "yellow");//先回.con,第二个回body
```



### .index（）和.eq()

查找元素的索引值，类似下标

```js
<ul>
  <li id="foo">foo</li>
  <li id="bar">bar</li>
  <li id="baz">baz</li>
</ul>

$('li').index(document.getElementById('bar')); //1，传递一个DOM对象，返回这个对象在原先集合中的索引位置
$('li').index($('#bar')); //1，传递一个jQuery对象
$('li').index($('li:gt(0)')); //1，传递一组jQuery对象，返回这个对象中第一个元素在原先集合中的索引位置
$('#bar').index('li'); //1，传递一个选择器，返回#bar在所有li中的索引位置
$('#bar').index(); //1，不传递参数，返回这个元素在同辈中的索引位置。  
```

```js
//eq()方法的元素获取与排队原理如下：
//先获取所有满足条件的元素进行整体的大排队，然后在这个大排队的基础上确定某个位置上的元素。
$(".box p").eq(6).css("background","green"); //所有box 后代p 的第七个


```



# 选择器

## 基本选择器

基本选择器:和css中的选择器一样
id选择器    $('#id');
类选择器    $('.calss')
标签选择器  $('标签名')
并集选择器  $('.box1,.box2')
交集选择器  $('li.current')

## 层次选择器

子代选择器  $('ul>li')
后代选择器  $('div li')   

兄弟元素选择器

| $("prev+next")         | 选取紧接在 prev 元素后的 next 元素     | 集合元素     | $(“.one+p”)选取 class 为 one 的下一个 p兄弟元素       |
| ---------------------- | -------------------------------------- | ------------ | ----------------------------------------------------- |
| **$("prev~siblings")** | **选取prev元素之后的所有siblings元素** | **集合元素** | **$(“#two~p”)选取 id 为 two 的元素后面所有兄弟元素p** |

## 过滤选择器     

选择指定下标的元素    li:eq(索引);

```js
$("ul>li:eq(1)~li")
```

选择偶数    li:odd;    

选择奇数   li :even;

```js
$('li:odd').css('backgroundColor','pink');
$('li:even').css('backgroundColor','skyblue');
```

### 过滤选择器

```js
console.log($("ul li:first")); //所有ul后代li 里的第一个
console.log($("ul li:first-child"));//每个ul下面的第一个li
console.log($("ul li:last")); //所有ul后代li 里的最后一个
console.log($("ul li:last-child"));//每个ul下面的最后一个li

```



# 元素设置

### 获取样式:css(样式名);

- 获取样式时行内样式和内嵌样式都能获取,

- 如果使用老浏览器获取样式,一定要指定清楚是哪个样式;

- 但是如果获取多个dom对象,那么只会返回第一个dom对象的样式值;

- 获取的数值是字符串类型;


### 设置样式:css(样式名,样式值);

- 用css()方式设置样式是设置在元素的行内

- 无论是获取还是设置样式,

- 样式名:在单样式中,都要打引号,在对象中可以不用打引号,允许使用连接符,但在对象中使用连接符的话就要打引号;

- 样式值:数值如果有单位则要打引号,没单位不用打引号;


```js
<script>
        // css 设置获取样式
        $(function () {
            //获取样式 css(样式名)
            // 1.获取id为div1的元素的样式
            $('#getBtn').click(function () {
                console.log($('#div1').css('width'));
                console.log($('#div1').css('height'));
                console.log($('#div1').css('backgroundColor'));
                console.log($('#div1').css('background-color'));
                console.log($('#div1').css('border'));
                //行内样式和内嵌样式都能获取,可以使用连接符-

                //如果使用老浏览器获取样式,一定要指定清楚是哪个样式
                console.log($('div1').css('border-top-width'));//'2px';
                console.log($('div1').css('borderTopWidth'));//'2px';

                //注意:如果jQuery对象包含多个dom对象,那只会返回第一个dom对象对应的样式值
                console.log($('div').css('width'));//'200px';
            })

            //2.设置样式
            //用css方式设置样式,是设置在元素的行内
            $('#setBtn').click(function(){
                // 2.1给div1设置样式
                //a.设置单样式,css(样式名,样式值);
                $('div1').css('width',300);
                $('div1').css('width','300px');
                $('#div1').css('background-color','green');
                $('#div1').css('borderTopWidth','10px');
                //b.设置多样式css(对象);对象就可以传多个样式名样式值
                $('div1').css({
                    width: '300px',
                    'height': 300,
                    'background-Color': 'green',
                    borderTopWidth:10
                })
                //样式名,在单样式中,都要打引号,在对象中可以不用打引号,但在对象中使用连接符的话就要打引号
                //样式值,数值如果有单位则要打引号,没单位不用打引号
            })
        })
    </script>
```



### 类名设置(addclass)

```javascript
    /*1.添加一个类*/
    $('li').addClass('now');
    /*2.删除一个类*/
    $('li').removeClass('now');
    /*3.切换一个类  有就删除没有就添加*/
    $('li').toggleClass('now');
    /*4.匹配一个类  判断是否包含某个类  如果包含返回true否知返回false*/
    $('li').hasClass('now');
```



### 属性设置（attr）

```javascript
    /*1.获取属性*/
    $('li').attr('name');
    /*2.设置属性*/
    $('li').attr('name','tom');
    /*3.设置多个属性*/
    $('li').attr({
        'name':'tom',
        'age':'18'
    });
    /*4.删除属性*/
    $('li').removeAttr('name');
```

### prop方法

自定义属性获取不到

在jQuery1.6之后，对于checked、selected、disabled这类boolean类型的属性来说，不能用attr方法，只能用prop方法。

```javascript
    /*对于布尔类型的属性，不要attr方法，应该用prop方法 prop用法跟attr方法一样。*/
    $("#checkbox").prop("checked");//获取状态
    $("#checkbox").prop("checked", true);
    $("#checkbox").prop("checked", false);
    $("#checkbox").removeProp("checked");
```



## 节点操作

### 创建节点

```javascript
    /*创建节点*/
    var $a = $('<a href="http://www.baidu.com" target="_blank">百度1</a>');

```

### 克隆节点(clone)

克隆的节点只存在内存中,如果要在页面显示要追加
参数是布尔值,默认false,不管是什么参数都会克隆内容
参数是true,深克隆,会克隆事件
参数是false,浅克隆,不会克隆事件

```javascript
$('#clone').click(function () {
  var $divClone = $('#div1').clone(true);
  console.log($divClone);
  //修改克隆的元素的id.
  $divClone.attr('id', 'div2');
  //把克隆的元素追加到body中.
  $('body').append($divClone);
})
```



### 替换节点（replaceAll）

- 运用传统的JS方法，使用`replaceChild`来替换节点
- jQuery 中使用 replaceAll 的方法替换节点,节点被替换后,所包含的事件行为就全部消失了

```js
 $("<li study='jQuery'>新元素2222</li>").replaceAll($("ul li:eq(1)"));//前面替换后面
```





### 添加&移动节点

#### append();

父元素.append(子元素);

作为最后一个子元素添加

```js
 $("#box").append($("<li study='jQuery'>新元素2222</li>"))
```

#### appendTo();

子元素.appendTo(父元素);

作为最后一个子元素添加

```js
 $("<li study='jQuery'>新元素2222</li>").appendTo($("#box"));
```

#### prepend();

父元素.prepend(子元素);

作为第一个子元素添加

```js
 $("#box").prepend($("<li study='jQuery'>新元素2222</li>"))
```

#### prependTo();

子元素.prependTo(父元素);

作为第一个子元素添加

```js
$("<li study='jQuery'>新元素2222</li>").prependTo($("#box"));
```

#### before();

兄弟元素A.before(兄弟元素B);

把元素B插入到元素A的前面;

```js
$("ul li:eq(1)").before($("<li study='jQuery'>新元素2222</li>"))
$("<li study='jQuery'>新元素2222</li>").insertBefore($("ul li:eq(1)"))
```

#### after();

兄弟元素A.after(兄弟元素B);

把元素B插入到元素A的后面;

```js
$("ul li:eq(1)").after($("<li study='jQuery'>新元素2222</li>"))
$("<li study='jQuery'>新元素2222</li>").insertAfter($("ul li:eq(1)"))//相当于appendTo
```



### 删除节点&清空节点(empty  remove)

```javascript
    /*1.清空box里面的元素*/
    /* 清理门户 *///不仅可以清空内容,还能删除内容元素上的事件.
    $('#box').empty();
    /*2.删除某个元素*/
    /* 自杀 */
    $('#box').remove();
```



## jQuery特殊属性操作

### val方法

> val方法用于设置和获取表单元素的值，例如input、textarea的值

```javascript
    //设置值
    $("#name").val('张三');
    //获取值
    $("#name").val();

```

### html方法与text方法

> html方法相当于innerHTML text方法相当于innerText

```javascript
    //设置内容可以解析标签
    $('div').html('<span>这是一段内容</span>');
    //获取内容会获取到标签
    $('div').html()
    
    //设置内容解析不了标签
    $('div').text('<span>这是一段内容</span>');
    //获取内容不会获取标签
    $('div').text()

```

区别：html方法会识别html标签，text方法会那内容直接当成字符串，并不会识别html标签。

### width方法与height方法

> 设置或者获取高度，不包括内边距、边框和外边距

```javascript
    //带参数表示设置高度
    $('img').height(200);
    //不带参数获取高度
    $('img').height();


```

**获取网页的可视区宽高**

```javascript
    //获取可视区宽度
    $(window).width();
    //获取可视区高度
    $(window).height();
```

### innerWidth(); innerHeight();

方法返回元素的宽度/高度（包括内边距）。**padding-box的宽高**

```js
console.log($('div').innerWidth()); //240
console.log($('div').innerHeight()); //240
$('div').innerWidth(300);
$('div').innerHeight(300);
```

### outerWidth(); outerHeight();

/方法返回元素的宽度/高度（包括内边距和边框）。**border-box的宽高**

```js
console.log($('div').outerWidth()); //260
console.log($('div').outerHeight()); //260
```

### outerWidth(true); outerHeight(true);

方法返回元素的宽度/高度（包括内边距、边框和外边距）。**border-box+外边距的宽高**

```js
console.log($('div').outerWidth(true)); //320
console.log($('div').outerHeight(true)); //320
```



### scrollTop与scrollLeft

> 设置或者获取滚动条的位置

```javascript
  //获取
console.log($('div').scrollLeft());
console.log($('div').scrollTop());

//设置
$('div').scrollLeft(200);
$('div').scrollTop(200);

//获取页面滚出去的距离
$(window).scrollLeft();
$(window).scrollTop();

//设置页面滚出去的距离
$(window).scrollLeft(200);
$(window).scrollTop(200);

//动画回到顶部
$('body,html').animate({
    scrollTop: 0
},2000);

//滚动事件
$(window).scroll(function(){
    console.log(1);
})
```

### offset方法与position方法

> offset方法获取元素距离document的位置(文档对象窗口)，position方法获取的是元素距离有定位的父元素的位置。

```javascript
    //获取元素距离document的位置,返回值为对象：{left:100, top:100}
    $(selector).offset();
    //设置值
    $('#son').offset({
      left: 200,
      top: 200
    })
    //获取相对于其最近的有定位的父元素的位置。
    $(selector).position();
```





## jQuery动画

### jQuery 的显示和隐藏

**.show()为显示， .hide()为隐藏**

- 在 .show() 和 .hide() 方法可以传递一个参数,这个参数以毫秒(1000 毫秒等于 1 秒钟)来控 制速度。并且里面富含了匀速变大变小,以及透明度变换

- 除了直接使用毫秒来控制速度外, jQuery 还提供了三种预设速度参数字符串: slow 、normal 和 fast ,分别对应 600 毫秒、400 毫秒和 200 毫秒，不管是传递毫秒数还是传递预设字符串,如果不小心传递错误或者传递空字符串。 那么它将采用默认值:400 毫秒

- 如果需要控制动画的形式，则可以有第二个参数easing。默认有两个效果："linear"和"swing"。

  其中swing（随着动画的开始变得更加快一些，然后再慢下来。swing是一个常用设置，因此，如果没有指定任何缓动，jQuery会使用swing方法）

- 使用第三个参数可以书写回调函数

- jQuery 提供给我们一个显示隐藏独立的方法 .toggle()，**（上面只能单独显示或隐藏，这个显示隐藏都可以）**

  ```js
   /*注意：动画的本质是改变容器的大小和透明度*/
      /*注意：如果不传参数是看不到动画*/
      /*注意：可传入特殊的字符  fast normal slow*/
      /*注意：可传入数字 单位毫秒*/
      /*1.展示动画*/
      $('li').show();
      /*2.隐藏动画*/
      $('li').hide();
      /*3.切换展示和隐藏*/
      $('li').toggle();
  ```
  
  ```js
  $("#btn").click(function(){
     $(".con").toggle('normal',linear,function(){
        alert(1);//动画的回调函数，在动画执行结束之后运行
     });
  })
  ```



### jQuery的滑动和卷动（本质高度变化）

- jQuery 提供了一组改变元素高度的方法 **.slideUp() 、 .slideDown() 和 .slideToggle()** 。顾名思义,向上收缩(卷动)和向下展开(滑动)。

- .**slideDown()向下展开(滑动)**

- **.slideUp() 向上收缩(卷动）**

- 参数参考show和hide方法

  ```js
   /*注意：动画的本质是改变容器的高度*/
      /*1.滑入动画*/
      $('li').slideDown();
      /*2.滑出动画*/
      $('li').slideUp();
      /*3.切换滑入滑出*/
      $('li').slideToggle();
  ```

  

### jQuery的淡入和淡出（本质不透明度变化）

- jQuery 提供了一组改变元素透明度的方法 **.fadeIn() 、 .fadeOut() 和 .fadeToggle()** 。
-  **.fadeIn() 淡入**
-  **.fadeOut()淡出**
- 参数参考show和hide方法

```js
 /*注意：动画的本质是改变容器的透明度*/
    /*1.淡入动画*/
    $('li').fadeIn();
    /*2.淡出动画*/
    $('li').fadeOut();
    /*3.切换淡入淡出*/
    $('li').fadeToggle();
    $('li').fadeTo('speed','opacity');
```



## jQuery自定义动画

#### 自定义动画的基础使用

- 在jQuery中，可以使用`animate()`方法来自定义动画，满足更多复杂多变的要求
- `animate()`的语法结构为：`animate(params,speed,callback)`
  - `params`：一个包含样式属性及值的映射速度
  - `speed`：三种预定速度之一的字符串("slow","normal", or "fast")或表示动画时长的毫秒数值(如：1000)
  - `easing`:要使用的擦除效果的名称(需要插件支持).默认jQuery提供"linear" 和 "swing".
- `callback`：在动画完成时执行的函数

```js
  /*
    * 自定义动画
    * 参数1：需要做动画的属性
    * 参数2：需要执行动画的总时长
    * 参数3：执行动画的时候的速度
    * 参数4：执行动画完成之后的回调函数
    * */
    $('#box1').animate({left:800},5000);
    $('#box2').animate({left:800},5000,'linear');
    $('#box3').animate({left:800},5000,'swing',function () {
        console.log('动画执行完成');
    });
```

```js
$("#btn4").click(function(){
    $(".con").animate({
        width:"400px",
        opacity:".5",
        fontSize:"40px"
    },2000,"linear",function(){
        alert("动画执行完成")
    })
})
```



- 累加、累减动画：可以在设置属性的时候，给属性的值为+= 或者 -= 实现累加累减动画

  例如：`left:"+=500px"`

  ```js
  $("#btn").click(function () {
      $(".con").animate({
          left: "+=50px"
      }, 500)
  })
  ```

  

#### 动画队列

第一个动画还没执行完的情况下立即触发下一个动画,则下一个动画会等第一个动画执行完后再执行,有种排队的现象;

- 如果想要按照顺序执行动画，只需要把代码按照顺序就可以，
- 因为`animate()`方法都是对同一个jQuery对象进行操作，所以也可以改为链式写法
- 动画效果的执行具有先后顺序，称为"动画队列"
- 但是除了动画以外，其他的方法不会放到队列中，而是直接执行

```js
//只有动画放在动画队列中，其他方法会立即执行
$("#btn").click(function(){
    $(".con").animate({
        left:"1000px"
    },2000)
    .animate({
        top:"500px"
    },2000)
    .animate({
        left:"0px"
    },2000)
    .animate({
        top:"0px"
    },2000)
    .css("opacity",".5") //立即执行
})
```



#### stop()

stop(clearQueue, jumpToEnd);  停止动画

参数1: 是否清除队列

参数2: 是否跳转到当前动画的最终效果

```js
$('#stop').click(function(){
    $('div').stop(true,true) //后面的动画不执行,瞬间完成动画效果一
    $('div').stop(true,false) //后面的动画不执行，中途停止动画一
    $('div').stop(false,false) //立即执行后面的动画，中途停止动画一
    $('div').stop(false,true) //立即执行后面的动画，瞬间完成动画效果一
    $('div').stop() //没有参数默认都为false;
})
```



#### 判断元素是否处于动画状态

- 当用户在某个元素上执行`animate()`动画时，就会出现动画积累
- 使用`is(":animated")`方法判断是否处于动画状态
- 解决办法是判断元素是否正在处于动画状态，如果元素不处于动画状态，才会为元素添加新动画

```js
$(".outer").click(function () {

    if ($(".outer").is(":animated")) {
        return;
    }

    $(this).stop(true, false).animate({
        height: "400px"
    }, 2000)
})
```



#### 延迟动画

- 在动画执行过程，如果想对动画进行延迟操作，那么可以使用方法
- `delay()`方法允许我们将队列中的函数延迟执行。它既可以推迟动画队列中函数的执行，也可以用于自定义队列

```js
$(".outer").mouseenter(function(){
    $(this).delay(2000).animate({
        width: "600px"
    },2000)
})
```



## jQuery事件机制

> JavaScript中已经学习过了事件，但是jQuery对JavaScript事件进行了封装，增加并扩展了事件处理机制。jQuery不仅提供了更加优雅的事件处理语法，而且极大的增强了事件的处理能力。

### jQuery事件发展历程(了解)

简单事件绑定>>bind事件绑定>>delegate事件绑定>>on事件绑定(推荐)

不能同时注册多个事件,指的是一个事件名不能做两件事

不能动态事件,指的是后面生成的元素不会有事件

注册委托事件,指的是将该元素要执行的事件委托给它的父元素,所以找的时候找父元素

> #### 简单事件注册

```javascript
    $('div').click(function(){
    console.log('我被点击了');
    })
    $('div').mouseenter(function(){
    console.log('我被移入了');
    })
```

缺点：不能同时注册多个事件，不能动态事件

> #### bind方式注册事件

```javascript
    //第一个参数：事件类型
    //第二个参数：事件处理程序
    $("p").bind("click mouseenter", function(){
        //事件响应方法
    });
	
	//多个事件
    $('div').bind({
        'click':function(){
            console.log('我被点击了');
        },
        'mouseenter':function(){
            console.log('我被移入了');
        }
    })


```

缺点：不支持动态事件绑定

> #### delegate注册委托事件
>
> 同时注册多个事件
>
> 支持动态注册 

```javascript
    // 第一个参数：selector，要绑定事件的元素
    // 第二个参数：事件类型
    // 第三个参数：事件处理函数
    $(".parentBox").delegate("p", "click", function(){
        //为 .parentBox下面的所有的p标签绑定事件
    });


    $('body').delegate('div,p',{
      'click':function(){
          console.log('我被点击了');

      },
      'mouseenter':function(){
          console.log('我被移入了');

      }
  })
```

缺点：只能注册委托事件，因此注册时间需要记得方法太多了

> on注册事件

### on注册事件(重点)

> jQuery1.7之后，jQuery用on统一了所有事件的处理方法。
>
> 最现代的方式，兼容zepto(移动端类似jQuery的一个库)，强烈建议使用。
>
> **多个事件绑定同一个函数**、**多个事件绑定不同函数**

**on注册简单事件（支持同时注册多个事件）**

```javascript
    // 表示给$(selector)绑定事件，并且由自己触发，不支持动态绑定。
    $(selector).on( "click", function() {});
		
    $('div').on({
        'click': function(){
            console.log('单击了');

        },
        'mouseenter': function(){
            console.log('移入了');

        }
    })

```

**on注册委托事件（支持动态注册事件）**

```javascript
    // 表示给$(selector)绑定代理事件，当必须是它的内部元素span才能触发这个事件，支持动态绑定
    $(selector).on( "click",'span', function() {});

		
		$('body').on('click', 'div,p', function (e) {
       console.log('单击了');
       //虽然是body调用的方法,但此时的this指的是div和p,jQuey为了方便程序员的操作,把this的指向给修改了
       console.log(this);
       //此时指向的也是div和p
       console.log(e.target);
 	})
```

**on注册事件的语法：**

```javascript
    // 第一个参数：events，绑定事件的名称可以是由空格分隔的多个事件（标准事件或者自定义事件）
    // 第二个参数：selector, 执行事件的后代元素（可选），如果没有后代元素，那么事件将有自己执行。
    // 第三个参数：data，传递给处理函数的数据，事件触发的时候通过event.data来使用（不常使用）
    // 第四个参数：handler，事件处理函数
    $(selector).on(events,[selector],[data],handler);

```

### 事件解绑（off）

> unbind方式（不用）

```javascript
    $(selector).unbind(); //解绑所有的事件
    $(selector).unbind("click"); //解绑指定的事件

```

> undelegate方式（不用）

```javascript
    $( selector ).undelegate(); //解绑所有的delegate事件
    $( selector).undelegate( 'click' ); //解绑所有的click事件

```

> off方式（推荐）

```javascript
    // 解绑匹配元素的所有事件
    $(selector).off();
    // 解绑匹配元素的所有click事件
    $(selector).off("click");

		//委托事件的解绑,解绑对象是父亲
    $('body').on('click','div',function(){
        alert('ad');
    })
    $('#btn2').click(function(){
      $('body').off('click');
    })
```

### 触发事件（trigger()）

触发器:trigger();

用代码的方式触发事件

触发被选元素的指定事件类型

选定的元素的事件触发一般是有条件,比如点击,而用触发器就可以直接触发,比如页面一进入就调用该元素的点击事件

```javascript
    $(selector).click(); //触发 click事件
    $(selector).trigger("click");

```



### one绑定事件

不管是 .bind() 还是 .on() ,绑定事件后都不是自动移除事件,需要通过 .unbind()和 .off() 来手工移除。jQuery 提供了 **.one() 方法,绑定元素执行完毕后自动移除事件,方法仅触发一次的事件。**

```js
$("body").one("click", ".box", "123", function () {
    alert(1);
})

$("body").on("click", ".box", "123", function () {
    alert(1);
}, 1) //也执行一次
```



### jQuery事件对象

jQuery事件对象其实就是js事件对象的一个封装，处理了兼容性。

- jQuery 提供了一个事件对象的方法:event.stopPropagation();这个方法设置到需要触发的事件上时,所有上层的冒泡行为都将被取消
- jQuery 提供了一个事件对象的方法:event. preventDefault()；这个方法用来阻止默认行为

```js
$('.box').contextmenu(function(e){
    e.preventDefault();
})
```



### 显示迭代each()

- **jQuery的隐式迭代会对所有的DOM对象设置相同的值，但是如果我们需要给每一个对象设置不同的值的时候，就需要自己进行迭代了。**
- each(index,ele)

- index:遍历出来的索引

- index()方法也是获取索引

- ele:遍历出来的当前元素们,是dom对象


```js
$lis.each(function(index,ele){
                $(ele).css('opacity',(index+1)/10)
            })
```

##### $.each(object, *[callback]*)

**object**:需要遍历的对象或数组。

**callback**:每个成员/元素执行的回调函数。

```
$.each( [0,1,2], function(i, n){
  alert( "Item #" + i + ": " + n );
});
```





## jQuery 扩展

### 什么是jQuery扩展

- **jQuery自定义了jQuery.extend()和jQuery.fn.extend()方法**

- **jQuery.extend()为扩展jQuery类本身.为类添加新的方法**

- 类似对象的静态属性，只有对象（jQuery）自己能用

  ```js
  // 为jQuery类添加类方法，可以理解为添加静态方法。
  jQuery.extend({
      fly:function(){
          alert("我要飞~")
      },
      eat:function(){
          alert("你吃不吃")
      }
  })
  
  $.fly();//jQuery类才能用
  // $("body").fly();//报错不能用
  ```

- **jQuery.fn.extend(object);给jQuery对象添加方法。jQuery.fn = jQuery.prototype。**

- 类似于jQuery的原型对象上的方法，实例化对象都可以用，jQuery自己不能用

  ```js
  //就是为jQuery类添加“成员函数”。jQuery类的实例才可以调用这个“成员函数”。
  $.fn.extend({
      alertWhileClick: function() {
          $(this).click(function() {
              alert($(this).val());
          });
      }
  });
  
  $.alertWhileClick()//报错不能用
  //$("#input1")是jQuery的实例，调用这个扩展方法
  $("#input1").alertWhileClick();
  ```





## 链式编程的特例

什么时候可以用链式编程

调用一个jQuery方法,返回的还是一个jQuery对象,那就可以继续点出jQuery的方法.

```js
1.2 下面这句话有错没有?
有,width()是获取一个数值, 数值不能继续点出jquery方法类.
$('div').width(100).width().height(100).css('backgroundColor','red');
```

有些时候需要注意:一个jQuery方法返回的确是一个jQuery对象,但不是我们想要操作的那个jQuery对象.





## 多库共存

查看引入的jQuery版本

```js
 console.log(jQuery.fn.jquery);
 console.log(jQuery.prototype.jquery);
 console.log($.fn.jquery);
 console.log($.prototype.jquery);
```

如果我们引入多个jQuery文件,那么我们使用的$符号是最后一个的
如果想使用前面的jQuery文件中的内容,jQuery为我们提供了一种机制,多库共存;

```js
var _$ = $.noConflict();//释放$的控制权,释放的是最后一个引入的jQuery文件的
//_$是用来替代原来$功能的,除了替代,还可以用jQuery来使用被释放了$的文件里的内容
```

```
//1.4 引入三个也是一样的.
var _$300 = $.noConflict();//先释放3.0的$,给了1124
var _$1124 = $.noConflict();//再释放1124的
console.log(_$300.fn.jquery);
console.log(_$1124.fn.jquery);
console.log($);
```





## jQuery书写插件

- 插件名推荐使用 jquery.[插件名].js ,以免和其他 js 或者其他库相冲突;
- 局部对象附加 jquery.fn 对象上,全局函数附加在 jquery 上;
- 插件内部 this 指向是当前的局部对象;
- 可以通过 this.each 来遍历所有元素;
- 所有的方法或插件,必须用分号结尾,避免出现问题;
- 插件应该返回的是 jQuery 对象,以保证可链式连缀;
- 避免插件内部使用 $ ,如果要使用,请传递 jQuery 进去。