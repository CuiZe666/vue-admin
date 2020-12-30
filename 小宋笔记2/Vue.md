

# Vue.js

## 简介

### 1.Vue (读音 /vjuː/，类似于 view)的简单认识

（1）Vue是一个渐进式的框架，什么是渐进式的呢？

- 渐进式意味着你可以将Vue作为你应用的一部分嵌入其中，带来更丰富的交互体验。
- 或者如果你希望将更多的业务逻辑使用Vue实现，那么Vue的核心库以及其生态系统。
- 比如Core+Vue-router+Vuex，也可以满足你各种各样的需求。

（2）Vue有很多特点和Web开发中常见的高级功能

- 解耦视图和数据
- 可复用的组件
- 前端路由技术
- 状态管理
- 虚拟DOM



- Vue.js 是目前最火的一个前端框架，React是最流行的一个前端框架（React除了开发网站，还可以开发手机App， Vue语法也是可以用于进行手机App开发的，需要借助于Weex）
- Vue.js 是前端的**主流框架之一**，和Angular.js、React.js 一起，并成为前端三大主流框架！
- Vue.js 是一套构建用户界面的框架，**只关注视图层**，它不仅易于上手，还便于与第三方库或既有项目整合。（Vue有配套的第三方类库，可以整合起来做大型项目的开发）
- 前端的主要工作？主要负责MVC中的V这一层；主要工作就是和界面打交道，来制作前端页面效果；

**提高开发效率的发展历程**

原生JS -> Jquery之类的类库 -> 前端模板引擎 -> Angular.js / Vue.js/React.js

1. jquery解决了兼容问题,但不能很好的操纵dom元素,比如表格,列表的生成需要不断地拼接

2. 所以后来出现了模板引擎很方便的生成dom元素,但该操作是整体性的,并不能单个操作,如果少量数据出错又要重新渲染,消耗性能

3. 所以出现了angular和vue框架,减少不必要的dom操作,提高渲染效率,

4. vue还提出了双向数据绑定的概念:通过框架提供的指令,我们只需要关心数据的业务逻辑,不再关心DOM是如何渲染;

## 框架和库的区别

**框架**:是一套完整的解决方案,对项目的侵入性很大,项目如果需要更换框架,则需要重新架构整个项目;

**库**(插件):提供某一个小功能,对项目的侵入性小,如果某些库无法完成某些功能,可以很容易切换到其他库实现需求;





## 基本使用

使用一个框架，我们第一步要做什么呢？安装下载它

安装Vue的方式有很多：

**方式一：直接CDN引入**
你可以选择引入开发环境版本还是生产环境版本

```js
<!-- 开发环境版本，包含了有帮助的命令行警告 --> 
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
1234
```

**方式二：下载和引入**

开发环境 https://vuejs.org/js/vue.js

生产环境 https://vuejs.org/js/vue.min.js

**方式三：NPM安装**

后续通过webpack和CLI的使用，我们使用该方式。



## MVVM

注：本文多数内容属于Vue2.6之前的内容，只有较为重要的地方才会补充2.6版本之后的内容，望周知。

### 1、Vue中的MVVM

#### （1）什么是MVVM呢？

通常我们学习一个概念，最好的方式是去看维基百科(对，千万别看成了百度百科)
https://zh.wikipedia.org/wiki/MVVM
维基百科的官方解释，我们这里不再赘述。

#### （2）Vue的MVVM

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200113231857687.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3d1eXhpbnU=,size_16,color_FFFFFF,t_70)

**View层：**

- 视图层
- 在我们前端开发中，通常就是DOM层。
- 主要的作用是给用户展示各种信息。

**Model层：**

- 数据层
- 数据可能是我们固定的死数据，更多的是来自我们服务器，从网络上请求下来的数据。
- 在我们计数器的案例中，就是后面抽取出来的obj，当然，里面的数据可能没有这么简单。

**VueModel层：(Vue的实例(监听DOM和数据绑定操作))**

- 视图模型层
- 视图模型层是View和Model沟通的桥梁。
- 一方面它实现了Data Binding，也就是数据绑定，将Model的改变实时的反应到View中
- 另一方面它实现了DOM Listener，也就是DOM监听，当DOM发生一些事件(点击、滚动、touch等)时，可以监听到，并在需要的情况下改变对应的Data。



**如果数据变化了,视图就会改变,改变视图,数据也会变化,就通过Vue来连接这种关系，也是一种方式,也是一种模式,MVVM模式**



## 三种引入方式

1.通过CDN在线引入vue的方式来实现Vue的相关操作

![image-20200915184852495](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200915184852495.png)

2.github搜索Vue,然后找到源码,找到里面的dist目录,下载里面的vue.js文件,通过本地引入

![image-20200915184942870](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200915184942870.png)

3.通过脚手架的方式进行Vue的开发操作,暂时不讲,Vue课程第二天再使用并进行讲解

## 名词解释

![image-20200909200304053](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200909200304053.png)



# vue指令

## v-bind 强制数据绑定

强制数据绑定:标签上的一些属性的值能够动态的进行操作,但是需要Vue中的指令来实现

强制数据绑定的指令,可以用来为某个属性动态的绑定数据

```js
<input type="text" v-bind:value="val" />
<p v-bind:text="tt">这是一个p标签</p>
<!--强制数据绑定的这个指令的简写方式:     :属性名字="表达式"-->
<p :text="tt">这是一个p标签</p>
插图片
 <img :src="carousel.imgUrl" alt />
```

为什么要有强制数据绑定? Vue搭建界面,离不开操作html标签,标签就有属性,属性中的值如果是动态的,那么操作起来会非常的方便,但是有很多的属性就是普通的标签的普通的属性,希望普通的属性中的数据也可以是动态的





## v-on 绑定事件监听

绑定事件监听,v-on:事件名字="回调函数"

绑定事件监听的简写方式:  @事件名字="回调函数"

```js
<!--绑定事件监听,v-on:事件名字="回调函数"-->
<button v-on:click="showMsg1">华哥说:</button>
<button v-on:click="showMsg2">静哥说:</button>
<button v-on:click="showMsg3">超哥说:</button>
<hr>
<!--绑定事件监听的简写方式:  @事件名字="回调函数"  -->
<button @click="showMsg4">一个小故事</button>
```

有时候后面不一定是回调函数，也有可能是表达式

![image-20200915190047501](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200915190047501.png)

## v-model 双向数据绑定

双向数据绑定指令,v-model="表达式",一般用在表单标签中,   相当于:value和input事件的配合

需求: 文本框中输入内容后,p标签中的内容会随时自动的变化



```js
<!--双向数据绑定指令,v-model="表达式",一般用在表单标签中-->
<input type="text" v-model="msg" />
<p>{{msg}}</p>
```

v-model 是v-bind和v-on 的结合



## v-if

v-if和v-else通常都是配合使用,

v-if可以单独使用

使用了v-if指令或者v-else指令的标签,如果为true,则该标签在DOM树存在,如果表达式的值是false,当前的这个标签在DOM树是不存在的

```js
<p v-if="isSeen">我喜欢你</p>
<p v-else>你喜欢我</p>
```

v-if指令可以配置v-else-if指令使用

```js
<button @click="score='C'">设置级别,显示对应的分数段</button>
<p v-if="score==='A'">90到100分之间</p>
<p v-else-if="score==='B'">80到90分之间</p>
<p v-else-if="score==='C'">70到80分之间</p>
<p v-else-if="score==='D'">60到70分之间</p>
<p v-else>不及格</p>
```



## v-show

v-show指令,可以设置标签的显示和隐藏,几乎和v-if类似,但是,区别在于使用v-show指令的标签,无论是显示还是隐藏,在DOM树中始终存在,v-if(v-else,v-else-if)指令的标签,有可能在DOM树中不存在

另外使用v-show指令的标签主要是通过style属性中的display属性来控制其显示或者隐藏

```js
<!--方式2:使用v-show指令实现-->
<button @click="isShow=!isShow">切换显示效果</button>
<p v-show="isShow">我想你啊</p>
```



## v-if和v-show

v-if和v-show都可以决定一个元素是否渲染，那么开发中我们如何选择呢？

- v-if当条件为false时，压根不会有对应的元素在DOM中。
- v-show当条件为false时，仅仅是将元素的display属性设置为none而已。

开发中如何选择呢？

- 当需要在显示与隐藏之间切片很频繁时，使用v-show
- 当只有一次切换时，通过使用v-if





## v-for:遍历数据的指令

![image-20200915194853750](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200915194853750.png)

```js
 v-for指令:
      语法1:
       v-for="(表达式1,表达式2) in 数组"
       表达式1---->数组中每一个项--->数组元素
       表达式2---->数组中的索引
      语法2:
       v-for="表达式 in 数组"
       表达式---->数组中每一个项----数组元素

      语法3:
       v-for="(表达式1,表达式2,表达式3) in 对象"
       表达式1--->值
       表达式2--->键
       表达式3--->索引
```

:key="表达式" 该表达式一般都是唯一的值,主要是用来标识该标签的唯一性

​    涉及到渲染虚拟DOM的时候效率的问题

​    虚拟DOM:---Vue的源码分析就清楚了

​    渲染:把虚拟DOM展示在界面中,变成了真实的DOM

​    :key=""值最好是使用唯一的标识值,如果仅仅是遍历数组展示数据信息,此时使用索引是可以的(一旦修改数组中的数据,或者排序操作,或者删除数据,那么此时不推荐使用索引,还是推荐使用唯一标识)



## v-text和v-html

```
<p v-text="content">{{content}}</p>
<a v-html="content">百度</a>
```

**参照innerHTML 和innerText**





### v-bind绑定class

绑定class有两种方式：

- 对象语法
- 数组语法

（1）绑定方式：对象语法

- 对象语法的含义是:class后面跟的是一个对象。

对象语法有下面这些用法：

一般意为当前的h2标签的class是否要应用`active`这个类样式

![image-20200915193618852](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200915193618852.png)

绑定方式：数组语法

- 数组语法的含义是:class后面跟的是一个数组。

数组语法有下面这些用法：

![image-20200915194118562](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200915194118562.png)



1. 在Vue中如何去修改或者操作DOM标签的样式
2.  操作class属性来设置或者修改标签样式

-  可以直接使用表达式的方式: 如: :class="表达式" --->:class="myClass"
-  使用对象的方式: 如: :class="{类样式名字:表达式}"--->:class="{cls:isFlag}"
-  isFlag是布尔类型,为false,表示该标签不应用该样式,为true,表示该标签应用该样式
-  使用数组的方式: 如: :class="[表达式1,表达式2,表达式3]"--->:class="[clsA,clsB,clsC]"
-  使用静态和动态结合的方式 如: class="cls1" :class="clsA"
-  使用静态绑定 :class="['cls1','cls2','cls3']"----知道有这么个写法



​          第一种和第二种和第四种用法比较多



### v-bind绑定style

![image-20200915194439830](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200915194439830.png)



Vue中style属性操作标签的样式的方式:

- 键值对的方式:----> :style="{color:fontColor,fontSize:fontSize}"
- 数组的方式:-----> :style="[fontColor1,fontSize2]"



## v-slot 插槽

什么是插槽？
插槽（Slot）是Vue提出来的一个概念，正如名字一样，插槽用于决定将所携带的内容，插入到指定的某个位置，从而使模板分块，具有模块化的特质和更大的**重用性**。
插槽显不显示、怎样显示是由父组件来控制的，而插槽在哪里显示就由子组件来进行控制 如下

这样两个插槽全都会使用父组件传的

![image-20201105172148867](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201105172148867.png)

**具名插槽**

![image-20201105175750030](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201105175750030.png)



从 vue@2.6.x 开始，Vue 为具名和范围插槽引入了一个全新的语法，即我们今天要讲的主角：v-slot 指令。目的就是想统一 slot 和 slot-scope 语法，使代码更加规范和清晰。既然有新的语法上位，很明显，slot 和 scope-slot 也将会在 vue@3.0.x 中彻底的跟我们说拜拜了。而从 vue@2.6.0 开始，官方推荐我们使用 v-slot 来替代后两者。

跟 v-on 和 v-bind 一样，v-slot 也有缩写，即把参数之前的所有内容 (v-slot:) 替换为字符 #。例如 v-slot:header 可以被重写为 #header：

v-slot的出现是为了代替原有的slot和slot-scope
简化了一些复杂的语法。
一句话概括就是v-slot ：后边是插槽名称，=后边是组件内部绑定作用域值的映射。

**作用域插槽**

父组件向子组件传递一个数组，子组件把遍历后的数组内容通过插槽slot（自定义属性方式）再传给了父组件

子组件遍历自己组件的数据再通过插槽给父组件

![image-20201105183130299](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201105183130299.png)





# vue自定义指令

 Vue提供了自定义指令的方法

​     1.注册全局指令

​     Vue.directive('指令名字',function(el,binding){})

​     2.注册局部指令

​     在Vue的配置中

```js
directives:{
	'指令名字'(el,binding){
		‘指令要干的事’
	}
}
```

​     \* el:element---指令属性所爱的标签对象

​     \* binding:包含指令相关数据的对象容器,里面有value值就是标签中的值

​     \* 区别:全局的指令作用范围更大,局部的指令只能在自己的包裹的标签中使用

**指令名字定义的时候不用V-，使用时要加V-**

**全局指令都能用，局部指令在哪个vue实例里面写的，只有这个实例对应的容器内能用**

















# computed 计算属性

计算属性无非就是某个属性的值发生了变化,相关联的属性的数据值也会自动的发生变化

### 计算属性的setter和getter

每个计算属性都包含一个getter和一个setter

- getter（return）读取数据。
- 在某些情况下，你也可以提供一个setter方法（设置数据）。
- 如果没有set的话默认是get

![image-20200915192009991](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200915192009991.png)



我们可能会考虑这样的一个问题：

- methods和computed看起来都可以实现我们的功能，
- 那么为什么还要多一个计算属性这个东西呢？
- 原因：计算属性会进行缓存，如果多次使用时，计算属性只会调用一次。

### computed区别于method的核心

在官方文档中，强调了computed区别于method最重要的两点

1. computed是**属性调用**，而methods是**函数调用**
2. computed带有**缓存功能**，而methods不是
3. computed定义的方法我们是以属性访问的形式调用的，`{{computedTest}}`
4. 但是methods定义的方法，我们必须要加上`()`来调用，如`{{methodTest()}}`，**否则，视图会出现test1的情况**，见下图![img](https://img-blog.csdnimg.cn/20191102162158428.png)
5. 我们可以将同一函数定义为一个方法而不是一个计算属性。两种方式的最终结果确实是**完全相同**的。然而，**不同的是计算属性是基于它们的响应式依赖进行缓存的。只在相关响应式依赖发生改变时它们才会重新求值。这就意味着只要 `text` 还没有发生改变，多次访问 `getText` 计算属性会立即返回之前的计算结果，而不必再次执行函数。而方法只要页面中的属性发生改变就会重新执行**
6. **对于任何复杂逻辑，你都应当使用计算属性**
7. **computed依赖于data中的数据，只有在它的相关依赖数据发生改变时才会重新求值**

**一般来说需要依赖别的属性来动态获得值的时候可以使用 computed，对于监听到值的变化需要做一些复杂业务逻辑的情况可以使用 watch。**

# Vue-watch 监视

watch: {}用来监听数据的改变，每当被监听的数据改变时，就会执行对应的方法。一般用来监听路由路径的改变。

**注意**：

watch中监视的变量发生改变时，执行的方法没有返回值

![image-20200915192719138](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200915192719138.png)

### deep

如果我们想对一下对象做深度观测的时候，需要设置这个属性为 true

```js
watch: {
 a: {
   deep: true,
   handler(newVal) {
     console.log(newVal)
   }
 }
}
```

**侦听开始回调立刻调用一次，不用等数据变化**

![image-20200928184817861](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200928184817861.png)









# vue事件参数对象的获取

```js
<div id="app">
  <h2>1.事件监听</h2>
  <button @click="show1('哈哈')">按钮1</button>
  <button @click="show2('嘎嘎',$event)">按钮2</button>
  <button @click="show3($event)">按钮3</button>
</div>
<script type="text/javascript">

  const vm = new Vue({
    el: '#app',
    methods: {
      show1(txt) {
        console.log(txt)
      },
      show2(txt, event) {
        console.log(txt, event)
      },
      show3(event) {
        console.log(event)
      }
    }
  })
```

@click="showMessage2($event)" 括号中直接可以传入事件参数对象,$event是固定写法,传入的事件参数的位置,一定是在后面

如果想要在Vue实例对象的某些相关方法中使用到事件参数对象(前提是在html模版中已经传了其他的参数了,或者没有传,但是还要想使用事件参数对象)就需要在html模版中的某个事件的回调函数中传入$event

此时在方法中进行接收该参数,可以直接使用该事件参数对象



# v-on指令高级使用-事件修饰符

- `.stop` - 调用 `event.stopPropagation()`。
- `.prevent` - 调用 `event.preventDefault()`。
- `.capture` - 添加事件侦听器时使用 capture 模式。
- `.self` - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
- `.{keyCode | keyAlias}` - 只当事件是从特定键触发时才触发回调。
- `.native` - 监听组件根元素的原生事件。
- `.once` - 只触发一次回调。
- `.left` - (2.2.0) 只当点击鼠标左键时触发。
- `.right` - (2.2.0) 只当点击鼠标右键时触发。
- `.middle` - (2.2.0) 只当点击鼠标中键时触发。
- `.passive` - (2.3.0) 以 `{ passive: true }` 模式添加侦听器

```vue
<!-- 方法处理器 -->
<button v-on:click="doThis"></button>

<!-- 动态事件 (2.6.0+) -->
<button v-on:[event]="doThis"></button>

<!-- 内联语句 -->
<button v-on:click="doThat('hello', $event)"></button>

<!-- 缩写 -->
<button @click="doThis"></button>

<!-- 动态事件缩写 (2.6.0+) -->
<button @[event]="doThis"></button>

<!-- 停止冒泡 -->
<button @click.stop="doThis"></button>

<!-- 阻止默认行为 -->
<button @click.prevent="doThis"></button>

<!-- 阻止默认行为，没有表达式 -->
<form @submit.prevent></form>

<!--  串联修饰符 -->
<button @click.stop.prevent="doThis"></button>

<!-- 键修饰符，键别名 -->
<input @keyup.="onEnter">

<!-- 键修饰符，键代码 -->
<input @keyup.13="onEnter">

<!-- 点击回调只会触发一次 -->
<button v-on:click.once="doThis"></button>

<!-- 对象语法 (2.4.0+) -->
<button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
```

### .stop 阻止冒泡

```vue
<div class="box" @click='div1'>
    <!-- 点击按钮是会触发事件冒泡 -->
    <!-- 冒泡机制,先触发按钮的点击事件,再触发div的点击事件 -->
    <!-- <input type="button" value="按钮" @click='btn'> -->
    <!-- .stop 阻止事件冒泡 -->
    <input type="button" value="按钮" @click.stop='btn'>
</div>
```

### .self  事件在该元素本身时触发回调

```vue
<!-- 点击按钮时会触发div的点击事件,所以div的点击事件是被动触发的 -->
<!-- .self 只有事件在该元素本身时才会触发回调 -->
<!-- 即只有点击div时才会触发div的点击事件,此时点击按钮时事件冒泡不触发 -->
<div class="box" @click.self='div1'>
    <input type="button" value="按钮" @click='btn'>
</div>
```

#### .stop和.self的区别

```vue
<!-- .self只会阻止自己身上冒泡行为的触发,并不会真正阻止冒泡行为 -->
<!-- 当点击按钮时依然会触发事件冒泡,但是会跳过div1,执行div2的点击事件 -->
<div class="box2" @click='div2'>
    <div class="box" @click.self='div1'>
        <input type="button" value="按钮" @click='btn'>
    </div>
</div>
```

.stop是阻止事件冒泡,

.self只会阻止自己身上冒泡行为的触发,并不会真正阻止冒泡行为

### .prevent 阻止事件默认行为

```vue
<a href="http://www.baidu.com" @click.prevent='linkClick'>{{msg3}}</a>
```

### .capture 事件捕获模式

```vue
<!-- .capture 添加事件侦听器时使用事件捕获模式 -->
<!-- 事件捕获 先触发div的点击事件,再触发按钮的点击事件 -->
<div class="box" @click.capture='div1'>
    <input type="button" value="按钮" @click='btn'>
</div>
```

### .once 只触发一次事件处理函数

1. v-once添加的元素,内部的胡子语法,只会解析一次,后续数据改变了不会触发更新
2. 某些元素只需要解析一次,后续不需要更新,可以使用这个指令提升性能

```vue
<!-- .once 只触发一次事件处理函数 -->
<!-- 即阻止默认行为只会触发一次,当点击第二次时,.prevent不起作用,页面会跳转 -->
<!-- 并且点击事件也只能触发一次,只会打印一次{{msg3}} -->
<a href="http://www.baidu.com" @click.prevent.once='linkClick'>{{msg3}}</a>
```



# vue的生命周期/钩子函数

可以理解vue生命周期就是指vue实例从创建到销毁的过程，在vue中分为8个阶段：**创建前/后，载入前/后，更新前/后，销毁前/后。**

## 一、创建（实例）

1、**beforeCreate（ 数据初始化之前**）：这个阶段实例已经初始化，只是数据观察与事件机制尚未形成，不能获取DOM节点（没有data，没有el）
使用场景：因为此时data和methods都拿不到，所以通常在实例以外使用
2、**created（ 数据初始化之后）**：**实例已经创建，仍然不能获取DOM节点（有data，没有el）**
使用场景：模板渲染成html前调用，此时可以获取data和methods，so 可以初始化某些属性值，然后再渲染成视图，异步操作可以放在这里

## 二、载入（数据）

1、**beforeMount（界面显示之前)**：是个过渡阶段，此时依然获取不到具体的DOM节点，但是vue挂载的根节点已经创建（有data，有el）
2、**mounted(界面渲染后)**：**数据和DOM都已经被渲染出来了**
使用场景：模板渲染成html后调用，通常是初始化页面完成后再对数据和DOM做一些操作，需要操作DOM的方法可以放在这里

## 三、更新

1、**beforeUpdate（界面更新前）**：检测到数据更新时，但在DOM更新前执行
2、**updated（界面更新后）**：更新结束后执行
使用场景：需要对数据更新做统一处理的；如果需要区分不同的数据更新操作可以使用$nextTick

## 四、销毁

1、**beforeDestroy（组件销毁前）**：当要销毁vue实例时，在销毁前执行
2、**destroyed（组件销毁后）**：销毁vue实例时执行

### 面试题

#### 1、什么是 vue 生命周期？有什么作用？

答：每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做 生命周期钩子 的函数，这给了用户在不同阶段添加自己的代码的机会。（ps：生命周期钩子就是生命周期函数）例如，如果要通过某些插件操作DOM节点，如想在页面渲染完后弹出广告窗， 那我们最早可在mounted 中进行。

#### 2、created和mounted的区别

- created一般是在html渲染前的操作，此时el还是undefined，data已经存在。这里不能对dom进行操作。
- mounted一般是在html渲染完成后的操作，此时el，data都已经加载完成，一般对dom的操作都写在mounted中，例如获取innerHTML，初始化echarts的时候。

#### 3、第一次页面加载会触发哪几个钩子？

答：beforeCreate， created， beforeMount， mounted

#### 4、简述每个周期具体适合哪些场景

答：

- beforeCreate：在new一个vue实例后，只有一些默认的生命周期钩子和默认事件，其他的东西都还没创建。在beforeCreate生命周期执行的时候，data和methods中的数据都还没有初始化。不能在这个阶段使用data中的数据和methods中的方法
- create：data 和 methods都已经被初始化好了，**如果要调用 methods 中的方法，或者操作 data 中的数据，最早可以在这个阶段中操作**
- beforeMount：执行到这个钩子的时候，在内存中已经编译好了模板了，但是还没有挂载到页面中，此时，页面还是旧的
- mounted：执行到这个钩子的时候，就表示Vue实例已经初始化完成了。此时组件脱离了创建阶段，进入到了运行阶段。 如果我们想要通过插件操作页面上的DOM节点，最早可以在和这个阶段中进行
- beforeUpdate： 当执行这个钩子时，页面中的显示的数据还是旧的，data中的数据是更新后的， 页面还没有和最新的数据保持同步
- updated：页面显示的数据和data中的数据已经保持同步了，都是最新的
- beforeDestory：Vue实例从运行阶段进入到了销毁阶段，这个时候上所有的 data 和 methods ， 指令， 过滤器 ……都是处于可用状态。还没有真正被销毁
- destroyed： 这个时候上所有的 data 和 methods ， 指令， 过滤器 ……都是处于不可用状态。组件已经被销毁了。

#### 5、vue获取数据在哪个周期函数

答：一般 created/beforeMount/mounted 皆可。
比如如果你要操作 DOM , 那肯定 mounted 时候才能操作。



# vue transition  过渡

过渡效果无非就是从隐藏到显示,从显示到隐藏,有一些淡入淡出的效果,通过css来实现

```js
从隐藏到显示,三个阶段
		.fade-enter  开始阶段
		.fade-enter-active 过渡阶段
		.fade-enter-to  结束阶段

		从显示到隐藏,三个阶段
		.fade-leave  开始阶段
		.fade-leave-active 过渡阶段
		.fade-leave-to 结束阶段
		//从隐藏到显示开始和从显示到隐藏的结束，样式一样可以合写
      .fade-enter,
      .fade-leave-to {
          opacity: 0;
      }

      .fade-enter-active,
      .fade-leave-active {
          transition: all 5s;
      }
```

自带类过渡动画`<transition></transition>`，如果是列表即用v-for则用`<transitiongroup></transitiongroup>`

两个大阶段：**v-enter-active**和**v-leave-active**，
进入阶段分为**v-enter**进入前，**v-enter-to**进入后。
离开阶段分为**v-leave**离开前，**v-leave-to**离开后。

如果是自定义过渡动画格式，在`<transition name ="my">`,style中fade-就变成了my-



# vue过滤器filter

```js
 <div id="app">
        <h2>显示格式化的日期时间</h2>
        <h2>{{time}}</h2>
        <h2>{{time|filter1}}</h2>
        <h2>{{time|filter2('YYYY-MM-DD')}}</h2>
        <h2>{{time|filter3}}</h2>
 </div>
<script src="https://cdn.bootcdn.net/ajax/libs/moment.js/2.24.0/moment.js"></script>
<script>
    //value 就是time作为参数传进去
    Vue.filter("filter1", function (value) {
        return moment(value).format('YYYY-MM-DD hh:mm:ss')
    })
    //可以自己书写形式传进去
    Vue.filter("filter2", function (value, filterString) {
        return moment(value).format(filterString)
    })
    //防止没传，自己设置个默认的形式
    Vue.filter("filter3", function (value, filterString = 'YY-MM-DD') {
        return moment(value).format(filterString)
    })
    const vm = new Vue({
        el: '#app',
        data: {
            time: Date.now()
        }
    })
</script>
```













# vue组件通信

**组件之间的数据传递**

## 事件总线实现任意组件通信

### 一、简介：

先说一下什么是事件总线，其实就是订阅发布者模式；

比如有一个bus对象，这个对象上有两个方法，一个是on（监听，也就是订阅），一个是emit（触发，也就是发布），我们通过on方法去监听某个事件，再用emit去触发这个事件，同时调用on中的回调函数，这样就完成了一次事件触发；

在vue被实例化之后，他就具备了充当事件总线对象的能力，在他上面挂了两个方法，是$emit和$on；

而vue文档说的很明白，$emit会触发当前实例上的事件，附加参数都会传给监听器回调；

### 二、实现全局事件总线对象

**bus可以实现兄弟传值，跨级**

**理解**

**前提：所有的组件全都直接或则简介继承自Vue.**

**bus是个vue的实例且在原型上,(带$几乎都是实例的方法),所以bus可以用实例的方法`$on`和`$emit`**

**又因为bus在原型上，组件继承自Vue，所以所有的组件都可以`.bus**`

```js
//在mian.js中
Vue.prototype.bus = new Vue()  //这样我们就实现了全局的事件总线对象，bus是个vue的实例，且在原型上
 
//组件A中，监听事件（带$都是实例的方法几乎）
this.bus.$on('updata', function(data) {//回调里可以调用方法，也可以把回调本身设置成方法
    console.log(data)  //data就是触发updata事件带过来的数据
})
 
//组件B中，触发事件
this.bus.$emit('updata', data)  //data就是触发updata事件要带走的数据



// 也可以写在生命周期函数里（main.js）
new Vue({
    beforeCreate() {
        // 事件总线的方式
        Vue.prototype.$bus = new Vue()
    },
    // 渲染App组件
    render: h => h(App),
    // 注册路由
    router,
    store // 注册vuex仓库
}).$mount('#app') // 相当于el:'#app'
```

**每个组件在销毁时连同事件也要销毁，不然它会在你看不到的地方继续执行而难以被发现**

## 父子组件通信prop

通过props向子组件传递数据

1. 在父组件的模板中将数据用单项数据绑定的形式，绑定在子组件身上

   ```html
   <Son :money = "money"/>
   ```

2. 在子组件的配置项中可以使用一个props配置项来接收这个数据，接收时，props的取值可以是一个数组

   ```javascript
   Vue.component('Son',{
     template: '#son',
     //方法一
     props: ['money']
     //方法二
     props: {
     	todo:Array,
     	show:Function,
     	todos:Object
   	}
     //方法三
     props: {
       addTodo: {
       type: Function, // 类型
       required: true, // 必须的
       },
     },
     
   })
   ```

3. 在子组件模板中，接收到的属性可以像全局变量一样直接使用
   `<p> 父亲给了我 {{ money }} 钱 </p>`





## $emit子---父通信（自定义事件）

通过事件向父组件发送消息

流程

在子组件中通过$emit()来触发事件

```js
// $emit()用来分发事件的
// 参数1:事件类型(事件名字)
// 参数2:事件的回调函数所需的参数
this.$emit('addTodo',todo)
```



在父组件中，通过v-on来监听子组件事件

```js
 <Header @addTodo="addTodo" />
 
methods: {
  // 添加数据的方法
  addTodo(todo) {
    this.todos.unshift(todo)
  },
 }
```





## pubsub.js(消息订阅与发布)

**也可以实现兄弟传值，跨级**

方法原理和$bus类似

使用1，npm install pubsub-js （下载）

​		2,组件里引入 import PubSub from 'pubsub-js'

```js
// 消息订阅（接收数据）
// 参数1:消息名字
// 参数2:回调函数,msg---消息名字,data---->该消息发布的时候传入的参数
// 返回值是该消息的标识
this.token = PubSub.subscribe('toggleTodo', (msg, data) => {  //类似于bus.$on
  // 回调里执行需要执行的方法
  this.toggleTodo(data)
})
```

```js
// 取消订阅
PubSub.unsubscribe(this.token)  //根据标识取消
PubSub.unsubscribe() //全部取消
PubSub.unsubscribe('toggleTodo')//根据消息名字取消
```

```js
//消息发布（传递数据）
PubSub.publish('toggleTodo', this.todo);//这个类似与bus.$emit 参数1事件名字，参数2需要的参数  异步发布
PubSub.publishSync('toggleTodo', this.todo)  //同步发布
```





## 总结

props: 父子组件通信

 \* 自定义事件:父子组件通信

 \* 事件总线($bus):任意组件通信

 \* 消息订阅(PubSub):任意组件通信,PubSub属于一个单独的插件(别人封装好的js库),不属于Vue

 \* 插槽:相当于占位(挖坑)----填坑

 \* Vuex(Vue中非常重点的)





# localStorage的存储,读取,删除

**localStorage存储**
我们通过以下方式将数据储存到localStorage中

```js
window.localStorage.setItem('key',value)
```

但有时value为一个对象Object,以上面的方式写入,会出现读取的返回值为
{object Object}的情况,但这并不是我们想要的,此时我们需要使用新的方式
传入Object

```js
window.localStorage.setItem('param',JSON.stringify(Object))
```

通过JSON.stringify(Object)方法将对象转化为一个json格式的字符串进行存储

**localStorage读取**
我们通过以下方式来读取localStorage中的值

```js
window.localStorage.getItem("key")
```

相对的在读取json格式字符串只有我们也无法直接使用,需要将它转换为josn对象之后才是我们想要的结果,所以我们需要调用 JSON.parse()方法来进行转化,
之后在继续使用

```js
JSON.parse(window.localStorage.getItem("key"))
JSON.parse(window.localStorage.getItem("key")||{})
```

**localStorage删除**
我们通过以下方法来删除对应key以及key中的内容

```js
window.localStorage.removeItem('key')

```

**localStorage清空所有的key**
清空localStorage中所有的key;
注意:请谨慎使用,它会清空所有的本地存储数据

```js
window.localStorage.clear()
```







# Vue-cli (脚手架2)安装

```js
cmd命令窗口中
 * node -v 版本
 * npm -v 版本
 * https://github.com/vuejs/vue-cli/tree/v2#vue-cli--
 * 安装脚手架的命令工具
 * npm install -g vue-cli
 * vue -V 版本
 * 
 * 以上 都是在cmd中执行的
 * 找到一个合适的目录中,打开命令窗口,进行下载操作
 * 下载脚手架对应的vue的项目(模版)
 * vue init webpack 项目名字
 * 
 * npm run dev  运行项目
 * 
 * npm run build 打包项目---将来在公司中开发完毕项目后,需要上线的时候,你的同事找你要的打包文件
 * 
 * serve dist 运行打包文件
```

# Vue-cli (脚手架2---脚手架4的过渡）

```js
/*
1. 全局卸载电脑中脚手架2工具
npm uninstall vue-cli -g

2. 全局安装脚手架4
npm install -g @vue/cli
或者
yarn global add @vue/cli

3. 查看当前vue的版本
vue -V

4. 通过脚手架4创建新的项目模版
vue create gshop_client

5. 此时电脑中不能再使用脚手架2的命令来下载项目啦,肿么办,安装桥接工具
npm install -g @vue/cli-init

6. 此时电脑中脚手架2/4的命令都可以下载项目了

7. npm run serve 运行项目
   npm run build 打包
   serve dist 运行打包


脚手架2和脚手架4 的区别
1) 目录及文件个数不同
2) 脚手架4的 index.html 在public目录中
3) 项目能够在浏览器中自动的打开, 在package.json中后面加--open
"serve": "vue-cli-service serve --open"
4) main.js中的创建Vue实例对象中的代码不同

// 脚手架4中的代码
new Vue({
  render: h => h(App),
}).$mount('#app')

// 脚手架2的项目中的代码
new Vue({
  el:'#app',
  components:{App},
  template:'<App/>'
})
```











# 插件vue-resource

　　1、体积小：vue-resource非常小巧，在压缩以后只有大约12KB，服务端启用gzip压缩后只有4.5KB大小，这远比jQuery的体积要小得多。

　　2、支持主流浏览器：和Vue.js一样，vue-resource除了不支持IE 9以下的浏览器，其他主流的浏览器都支持

　　3、支持Promise API和URI Templates：Promise是ES6的特性，Promise的中文含义为“先知”，Promise对象用于异步计算。 URI Templates表示URI模板，有些类似于ASP.NET MVC的路由模板

　　4、支持拦截器：拦截器是全局的，拦截器可以在请求发送前和发送请求后做一些处理。 拦截器在一些场景下会非常有用，比如请求发送前在headers中设置access_token，或者在请求失败时，提供共通的处理方式。

### 二、安装与引用

　　NPM：` npm install vue-resource --save-dev`

main.js中

```js
/*引入Vue框架*/
import Vue from 'vue'
// 引入App组件
import App from './App.vue'
/*引入资源请求插件*/
import VueResource from 'vue-resource'

/*使用VueResource插件*/
Vue.use(VueResource)
```

### 三、语法

　　　　**引入vue-resource后，可以基于全局的Vue对象使用http，也可以基于某个Vue实例使用http**

```js
// 基于全局Vue对象使用http
Vue.http.get('/someUrl', [options]).then(successCallback, errorCallback);
Vue.http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);

// 在一个Vue实例内使用$http
this.$http.get('/someUrl', [options]).then(successCallback, errorCallback);
this.$http.post('/someUrl', [body], [options]).then(successCallback, errorCallback);
```

　　在发送请求后，使用then方法来处理响应结果，then方法有两个参数，第一个参数是响应成功时的回调函数，第二个参数是响应失败时的回调函数。







# vue-router

\* vue-router 路由器:路由的管理工具

 \* 路由:指的是一种映射关系,地址和组件的关系

 \* 组件:具有特定功能效果的集合(html+css+js)

 \* 组件:普通的组件和路由组件--------

 \* 路由组件:普通组件通过注册和某个地址发生了关系,此时该组件就是路由组件

 \* 地址:路由地址---->路由链接地址

 \* 要想使用路由,必须要先注册路由,然后通过声明式路由或者编程式路由实现单页面应用

 \* 声明式路由:路由链接(地址)和路由视图(展示某个组件内容的)组成

 \* 编程式路由:通过js代码的方式来实现地址和组件的效果展示

 \* 

 \* 普通的组件一般放在components目录中

 \* 如果当前的组件是路由组件,一般推荐放在pages目录中(上班后看老大)

**<router-view> 是用来渲染通过路由映射过来的组件，当路径更改时， 中的内容也会发生更改，可以对应多个组件**



主要应用于单页面中，与router-link配合，渲染router-link 映射过来的组件。

**运作过程：就是几个跳转链接跳到对应的子页面（路由组件），程序运行的时候，会将子页面（路由组件）<template>标签里面的内容都注入到App.vue页面中的router-view标签中，从而实现无刷新的路由跳转**



 **路由视图可以传递数据**

![image-20200919082722799](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200919082722799.png)

![image-20200919082820362](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200919082820362.png)

## router和route

**router里有路由跳转的push和replace方法**

**route里则可以获取path(路由路径)，params和query传递的参数**

![image-20200919082505519](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200919082505519.png)



## 路由的使用

\* 路由的使用步骤:

 \* 1. 安装路由器插件

 \* npm install vue-router

 \* 2. 引入路由器对象并实例化,而且需要在实例化的Vue中进行路由器的注册

 \* 一般情况路由器的实例化和暴露会放在一个单独的目录中router目录,内部有一个index.js文件(名字可改)



**注册多个路由组件（新建单独文件一起注册然后暴露出去）**

![image-20200913200327467](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200913200327467.png)

**引入多个路由组件对象并注册(routes)**

![image-20200913200503854](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200913200503854.png)

**把index引入到main.js里，router注册路由器**（组件在main.js里注册了都会有$route这个对象(里面有params传递的参数)，还有$router里有push,replace等方法）

![image-20200913200237953](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200913200237953.png)



### **声明式路由跳转和编程式路由跳转**

**声明式路由跳转就是router-link**

**编程式就是绑定事件回调函数**

![image-20200915211401221](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200915211401221.png)



## 路由组件里面编程式路由跳转点击多次报错的解决

```js
编程式路由跳转点击多次会报错
1.传入成功的回调
this.$router.push('/search', () => {})
2.传入成功和失败的回调
this.$router.push(
  '/search',
  () => {},
  () => {}
)
3.传入失败的回调,成功的那个可以写null和undefined
this.$router.push('/search', undefined, () => {})
4.then和catch一起使用
this.$router
  .push('/search')
  .then(() => {})
  .catch(() => {})
5.只使用catch也可以解决问题
this.$router.push('/search').catch(()=>{})

```

///治标不治本，如果有多个跳转的话每个都写太麻烦，所以我们可以重写router的push和replace方法

在router文件夹下面的index.js里面，声明使用路由器插件前重写

![image-20200919002918320](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200919002918320.png)





## 路由的params和query的传参

**params是路由的一部分,必须要在路由后面添加参数名。query是拼接在url后面的参数，没有也没关系**

**params普通方式（单个）**

```js
// params的方式
// this.$router.push(`/search/${this.keyword}`)
//  <router-link :to="'/detail/'+item.productId">
```

**params对象方式（多个）**

```js
// params的对象方式传参(keyword与接收的地方要一致)
// this.$router.push({ name: 'search', params: { keyword: this.keyword } })
// <router-link :to="{name:'detail',params:{skuId:this.$route.query.skuId}}">查看商品详情</router-link>
//一般会判断
// 判断文本框中是否输入内容
if (this.keyword) {
  this.$router.push({
    name: 'search', //跳转的地址,注册组件那边写一个对应的name属性
    params: {
      keyword: this.keyword,
    },
  })
} else {
  this.$router.push({ name: 'search' })
}
```

![image-20200919003833660](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200919003833660.png)



**query普通方式（单个）**

```js
// query的方式(?后面keyword可以随便写，内容是最后显示在地址栏上的)
// this.$router.push(`/search?keyword=${this.keyword}`)
```

**query对象方式（多个）**

```js
//query的对象方式传参(keyword可任意写，内容是最后显示在地址栏上的)
// this.$router.push({ name: 'search', query: { keyword: this.keyword } })
// this.$router.push({ path: '/search', query: { keyword: this.keyword } })

//this.$router.push({ path: '/search', query: { keyword: this.keyword }，params:{} }) 也可以
```

**query和params以对象的形式做路由跳转并传参的时候,query的方式可以使用path或者name,但是params的方式不能用path,可以用name**

**还有一个meta对象也可以传**：

![image-20200919004530168](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200919004530168.png)

![image-20200928184448024](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200928184448024.png)

![image-20200919083035124](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200919083035124.png)

**props也可以路由传参**

![image-20200928184402078](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200928184402078.png)



需要组件里用props接收







## vue异步请求解决跨域

脚手架项目模板下新建一个vue.config.js文件

![image-20200919004916084](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200919004916084.png)

官方文档

![image-20200919005031339](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200919005031339.png)









### 如何区分是不是一个二级路由？

嵌套的二级子路由应该显示在一级路由视图里面（一级路由视图里有route-view显示二级路由视图），应该具有相同的部分，如果是全新的页面，则不是二级路由，并不是在一个路由里面点击跳转到另一个页面，那么这个页面就是二级路由，这是不对的

# VueX

Vuex:集中式的管理状态数据,是一种管理状态数据的模式,也是一种工具,也是一个对象

Vuex的状态管理模式:通过actions来改变数据的状态,从而使界面发生变化

Vuex的使用步骤

 1) npm install vuex 安装vuex

 2) 在src目录中新建一个目录: vuex目录/store.js文件(store目录/index.js)，在公司开发名字看老大

 3) 在store.js文件中引入vue,引入vuex,声明使用vuex的插件,实例化Vuex的对象,并暴露出去,main.js中引入store对象,并注册store仓库对象

 4) 在实例化Vuex的对象的时候,需要初始化内部的配置对象,里面有state,mutations,actions,getters,(modules暂时可不写)



### 五个核心概念

**state:包含了多个状态数据的对象(data里的数据)**

**mutations:包含了多个直接修改状态数据的方法的对象，（mutation接受 state 作为第一个参数）**

- ​	mutations对象中的每个方法都可以叫mutation，
- ​	都是同步的代码

**actions:包含了多个间接修改状态数据的方法的对象**

- ​	actions对象中的每个方法都可以叫action
- ​	同步或者异步的代码
- ​	Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 `context.commit` 提交一个mutation（commit之前可以修改返回的数据）
- 可以将commit解构赋值出来

![image-20200915212921852](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200915212921852.png)

**getters:包含了多个状态数据的计算属性的GET方法的对象（Getter 接受 state 作为其第一个参数）**



**modules：当项目庞大，状态非常多时，可以采用模块化管理模式。Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 `state、mutation、action、getter`、甚至是嵌套子模块——从上至下进行同样方式的分割**。

![image-20200920212558593](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200920212558593.png)

### 辅助函数

![image-20200920212329424](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200920212329424.png)

![image-20200920212400629](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200920212400629.png)

### 一般用法：

在src下建立store文件夹，文件夹里建一个index.js文件（引入vue vuex 声明使用vuex ,然后实例化Vuex并且暴露出去）

将五个核心概念对象分别写在五个JS文件中，通过export default {}，将对象暴露出来

index.js里面引入各个暴露对象的JS文件

![image-20200915231237892](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200915231237892.png)



**组件里面通过commit---->mutations里的方法,dispatch--->actions里的方法**

**dispatch第二个参数可以传参**

![image-20200915232020617](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200915232020617.png)



**流程：**

**一般组件里通过触发dispatch--->vuex里的actions，actions--->mutations里的方法，mutation改变了数据状态反映到state里，然后state再渲染到页面。**

**（有时候也可以直接commit找mutations）**

**组件里可以通过$store.state.xxx,$store.getters.xxx,拿到state对象和getters对象里的数据**

![image-20200914181452324](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200914181452324.png)







# 图片懒加载

![image-20200929184353131](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200929184353131.png)

![image-20200929184503054](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200929184503054.png)

v-lazy指令使用









# Vue组件通信（高级）

**Vue中组件通信的方式(普通的):**

  **\1. props: 父子组件之间通信,可以传递动态的属性或者是回调函数**

  **\2. 自定义事件:父子组件之间通信,事件**

  **\3. 事件总线:任意组件之间进行通信,本质:原型**

  **\4. PubSub消息订阅:不属于Vue,单独的一个插件(React中同样可以使用),任意组件通信**

  **\5. 插槽(普通插槽:没有名字,具名插槽:有名字,作用域插槽):父子组件通信**

  **\6. vuex:任意组件通信**

## Vue中组件通信的方式(高级的):

### 自定义事件

**自定义事件和原生事件的区分**

-    **原生事件和自定义事件,主要是针对组件而言的,组件中可以使用原生事件,也可以使用自定义事件**
-    **原生事件:系统自带的,事件绑定了回调函数后,事件一旦触发,对应的回调函数中的代码就会自动的执行**
-    **自定义事件:自己定义的事件**
-    **组件中原生事件:使用了系统自带的事件,并且使用了.native进行修饰**
-    **组件中自定义事件:自己定义的或者使用了系统自带的事件,但是没有使用.native进行修饰**
-    **组件中的原生事件,最终事件给了子级组件中的最外层的html标签了(使用了事件委托的方式实现的)**
-    **组件中的自定义事件:是子级组件内部手动的分发父级组件传递过来的自定义事件,才能够正常的使用**



   **组件中使用了自定义事件,但是又使用了.native进行了修饰,此时到底是原生事件还是自定义事件?**

   **自定义事件**

![image-20201009211139994](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201009211139994.png)

![image-20201009211249581](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201009211249581.png)

**组件中传递自定义事件的方式:可以实现父子组件,子父组件进行通信**



###  v-model指令实现组件通信(本质:value属性和input事件)

**当一个组件中使用了v-model指令,也就意味着,向这个子级组件传递了value属性和input事件,子级组件中可以接收并使用value属性的数据,同时也可以分发input自定义事件,最终可以实现父子,子父组件的通信**

**子级组件需要props接收value，并且分发input事件**

![image-20201009211651903](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201009211651903.png)

![image-20201009211758748](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201009211758748.png)



### .sync修饰符实现父子组件通信

.sync的本质:向子级组件内部动态传递数据和自定义的updata事件,子级内部分发父级组件传递的自定义的update事件,从而实现数据更新,其实是:父子组件通信

父向子传递动态属性数据，子需要直接修改该数据并传给父组件，此时用.sync

![image-20201009212042907](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201009212042907.png)

![image-20201009212234133](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201009212234133.png)

.sync可以用来实现 子组件修改父组件的数据

在父组件中通过  :属性名.sync = 数据

然后在子组件中通过 $emit(update:属性名,修改后的数据)来实现子组件修改  父组件的数据   



###  $attrs和$listeners实现组件通信

**$attrs: 父级组件向子级组件中传递的所有的属性都在$attrs中,(class和stlye和props接收的属性)都不会在$attrs中存在**

   **$listeners:父级组件向子级组件中传递的所有的事件,(.native修饰符的原生事件)除外,**

   **v-bind:可以绑定对象的形式**

   **v-on:可以绑定对象的形式**

![image-20201009212547299](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201009212547299.png)

![image-20201009212616970](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201009212616970.png)





### $children 和 $parent 实现组件通信

 $children:可以获取当前的父级组件中的直接子级组件(间接的父子关系的组件是不行的)

  $parent:可以获取当前组件的直接父级组件,要有才可以

**父组件定义事件**

![image-20201009221628615](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201009221628615.png)

**遍历所有子组件，调用子组件内的borrowMoney方法**

![image-20201009221817267](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201009221817267.png)

**子组件定义点击事件改变父组件数据**

![image-20201009222019350](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201009222019350.png)

**获取父级组件**

![image-20201009222131193](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201009222131193.png)





### 作用域插槽:可以实现父子,子父组件传递数据,组件通信

父组件向子组件传递一个数组，子组件把遍历后的数组内容通过插槽slot（自定义属性方式）再传给了父组件

父组件通过slot-scope="scope"与v-slot:default="scope"接收子组件传过来的数据对象

![image-20201009221226095](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201009221226095.png)

![image-20201009213222272](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201009213222272.png)

**slot-scope="scope"与v-slot:default="scope"是一样的**





# this.$nextTick

**this.$nextTick()将回调延迟到下次 DOM 更新循环之后执行。在修改数据之后立即使用它，然后等待 DOM 更新。它跟全局方法 Vue.nextTick 一样，不同的是回调的 this 自动绑定到调用它的实例上。**

由于Vue DOM更新是异步执行的，即修改数据时，视图不会立即更新，而是会监听数据变化，并缓存在同一事件循环中，等同一数据循环中的所有数据变化完成之后，再统一进行视图更新。为了确保得到更新后的DOM，所以设置了 `Vue.nextTick()`方法。

简单的理解是：**当数据更新了，在dom中渲染后，自动执行该函数，**

应用场景：在数据变化后要执行的某个操作，而这个操作需要使用随数据改变而改变的DOM结构的时候，这个操作都应该放进`Vue.nextTick()`的回调函数中。

通俗的理解是：更改数据后当你想立即使用js操作新的视图的时候需要使用它

1.在Vue生命周期的created()钩子函数进行DOM操作一定要放到Vue.nextTick()的回调函数中。

2.在数据变化后要执行的某个操作，而这个操作需要使用随数据改变而改变的DOM结构的时候，这个操作都应该放进Vue.nextTick()的回调函数中。

```
this.$nextTick(()=>{

})
```



# 路由的懒加载

```js
// 引入Home组件
// import Home from '@/pages/Home'
//路由的懒加载
const Home = () => import('@/pages/Home')
```

正则

```
const xxx= 正则
if(xxx.test(value)) value需要验证的
```





拿到vueX里面的数据遍历

![image-20201117014402789](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201117014402789.png)



控制单选多选，

![image-20201117014617697](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201117014617697.png)

![image-20201117014709707](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201117014709707.png)



methods

![image-20201117014939533](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201117014939533.png)

Getters

![image-20201117015114727](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20201117015114727.png)







# mockjs

mock数据
	模拟数据，拦截Ajax请求，然后模拟数据返回
	npm i mockjs
新建mock文件夹，里面index.js
1 先引入Mock
2.引入模板数据 （自己写的json数据）json文件存数据 {} Key为字符串，到时候放入data作为数据返回
Mock.mock(要拦截的Ajax地址,{code:200,message:'成功'，data:数据 })
有几个请求写几个Mock.mock


地址 ’/mock/xxx‘ ,将来mock拦截的是以mock开头的地址


3.main.js引入   import "./mock "

然后接口请求地址就是你写的地址后面的xxx，ajax封装的baseURL:'/mock'（根路径）