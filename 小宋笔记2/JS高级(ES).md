# js高级

### 数据类型

- 基本(值)类型
  - Number: 任意数值
  - String: 任意文本
  - Boolean: true/false
  - undefined: undefined
  - null: null
- 对象(引用)类型
  - Object: 任意对象 主要用来包含无序复杂的数据
  - Array: 特别的对象类型(下标/内部数据有序)
  - Function: 特别的对象类型(可执行)，Function对象可以调用执行 ‘函数对象’

### 判断

- typeof:
  - 可以区别: 数值, 字符串, 布尔值, undefined, function
  - 不能区别: null与对象, 一般对象与数组
- instanceof
  - 专门用来判断对象数据的类型: Object, Array与Function
  - A instance B:判断 A 是否是 B的实例
- ===
- 因为null和undefined类型只有一个值，所以可以使用全等来判断null和undefined类型





### Undefined 和 Null

- 什么是Undefined类型
  - Undefined类型只有一个值，即undefined。比如在使用var声明变量但未对其加以初始化时，这个变量的值就是undefined。
  - 我们不会对一个值设置undefined，一般都是出现错误的时候，才会被我们打印出来
  - 常见的Undefined环境：
    - 变量被声明了，但没有赋值时，就等于undefined
    - 调用函数时，应该提供的参数没有提供，该参数等于undefined
    - 对象没有赋值的属性，该属性的值为undefined
    - 函数没有返回值时，默认返回undefined
- 什么是Null类型
  - null 类型是第二个只有一个值的数据类型，这个特殊的值是 null
  - 从逻辑角度来看，null 值表示一个空对象指针，而这也正是使用 typeof 操作符检测null时会返回"object"的原因
  - 常见Null的环境：
    - 作为函数的参数，表示该函数的参数不是对象
    - 作为对象原型链的终点
    - 如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为null而不是其他值
    - 让变量指向的对象成为垃圾对象



### 变量类型与数据类型

- js的变量本身是没有类型的, 变量的类型实际上是变量内存中数据的类型
- 变量类型:
  - 基本类型: 保存基本类型数据的变量
  - 引用类型: 保存对象地址值的变量
- 数据对象
  - 基本类型
  - 对象类型

```js
// 3. 严格区别变量类型与数据类型?
var a4 = 4
var a5 = [1, 2]

/*
    4: 基本类型的数据
    [1,2]: 对象类型的数据
    a4: 内存中保存的是基本类型的数据  ==> 基本类型的变量
    a5: 内存中保存的是对象内存的地址值  ==> 引用类型的变量
    变量a5是引用类型的变量，对象类型的数据保存在堆内存然后再返回一个地址到栈区对应的引用类型变量上，变量通过地址访问堆内存对象
*/
```



### 关于引用变量赋值问题

- 2个引用变量指向同一个对象, 通过一个引用变量修改对象内部数据, 另一个引用变量也看得见

```js
  var o1 = {m: 1}
  var o2 = o1;
  // 通过一个引用变量修改对象内部数据
  o2.m = 2;
  console.log(o1.m, o1===o2) // 2  true
```

- 2个引用变量指向同一个对象,让一个引用变量指向另一个对象, 另一个引用变量还是指向原来的对象

```js
  var o3 = {m: 3}
  var o4 = o3  //o4的地址还是指向原来
  o3 = {m: 4}
  console.log(o4.m, o4===o3) // 3  false
```

- 练习

- 引用类型的的赋值是地址

  ```js
  //test1
  var o5 = {m: 5};
  var o6 = o5;
  function fn(obj) {
      obj = {m: 6};//这里传参也是赋值，只不过是把地址给了obj，obj后面的地址指向又改了而已，o5，o6的地址指向还是原来
  }
  fn(o5)
  console.log(o5.m, o6.m, o6===o5)//5 5 true
  
  //test2
  var o5 = {m: 5};
  var o6 = o5;
  function fn(obj) {
      obj.m = 6 
  }
  fn(o5)
  console.log(o5.m, o6.m, o6===o5)// 6 6 true
  ```



### 关于数据传递问题

- 函数传参是值的传递， 没有引用传递, 传递的都是变量的值, 只是这个值可能是基本数据, 也可能是地址(引用)数据

```js 
function fn(a) {
    console.log(a)
}

var b = 3
fn(b) // 传递给a的是b变量的值, 值为基本类型的数据--3

b = {}
fn(b) // 传递给a的是b变量的值, 值为一个地址值---对象的地址值(而不是b的地址值)
```



### 值类型与引用类型的差别

- 基本类型在内存中占据固定大小的空间，因此被保存在栈内存中
- 从一个变量向另一个变量复制基本类型的值，复制的是值的副本
- 引用类型的值是对象，保存在堆内存
- 包含引用类型值的变量实际上包含的并不是对象本身，而是一个指向该对象的指针
- 从一个变量向另一个变量复制引用类型的值的时候，复制是引用指针，因此两个变量最终都指向同一个对象





## 对象

### 如何访问对象内部数据

- `.属性名`: 编码简单, 但有时不能用
- `['属性名']`: 编码麻烦, 但通用
- 属性名都是字符串类型，只不过使用`.`操作时候不用双引号。`[]`操作时需要加引号。
- 属性名不合法是需要用`['属性名']`来操作

### 面试题

```js
var a = {}
var obj1 = {n: 2}
var obj2 = {m: 3}
a[obj1] = 4
a[obj2] = 5
console.log(a)//[object Object]:5
console.log(a[obj1]) // 输出多少? 答案是5


//解释：
		//a[obj1]=4  把对象obj1当成属性传入对象a  a[{n: 2}]=4
    //a[obj2]=5  把对象obj2当成属性传入对象a  a[{m: 3}]=5
		//由于对象的属性名是字符串类型，而上面是对象，所以隐式转换为字符串得到属性名[object Object]
		//由于两个都是[object Object]，所以后面输出的时候等同于说输出a的[object Object]属性，所以为5

```



```JS
<script>
    var a = {x : 1};
    var b = a;
    a.x = a = {n : 1}; //a.x先运行，代表了对象的x属性的值的空间  将来是给a对象的n的空间赋值的
		console.log(a)      //n:1
		console.log(b)      // x:{n:1}
    console.log(a.x);   //undefined
    console.log(b.x);   //n:1
//a.x = a = {n : 1}连续赋值等同于a.x={n : 1}; a = {n : 1} 
</script>
```

运行如图：

![image-20200726145401709](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200726145401709.png)





## 函数

### 回调函数

- 什么函数才是回调函数?
  - 你定义的
  - 你没有直接调用
  - 但最终它执行了(在特定条件或时刻)
- 常见的回调函数?
  - DOM事件函数
  - 定时器函数
  - ajax回调函数(后面学)
  - 生命周期回调函数(后面学)

### IIFE

- 理解

  - 全称: Immediately-Invoked Function Expression 立即调用函数表达式
  - 别名: 匿名函数自调用

- 作用

  - 隐藏内部实现

  - 不污染外部命名空间

    只执行一次

    方便用来构建局部作用域!

```js
(function (i) {
    var a = 4
    function fn() {
        console.log('fn ', i+a)
    }
    fn()
})(3)
```







### 函数中的this

### this是什么?

- 一个关键字, 一个内置的引用变量
- 在函数中都可以直接使用this
- this代表调用函数的当前对象
- 在定义函数时, this还没有确定, 只有在执行时才动态确定(绑定)的

### this的指向方式

1. **默认绑定** ：常用的函数调用类型：独立函数调用

   可以把这个规则看作是无法应用其他规则的时候 默认的规则，基本指向的是window

```js
   function foo() {
       console.log(this);
   }
   foo(); //window

   var obj = {
       do: function () {
           foo(); //foo是直接使用不带任何修饰的函数引用进行调用
       },
   };
   obj.do(); //只是方法调用了do函数
```

2.**隐式绑定**

当函数引用有上下文对象的时候（obj），隐式绑定规则会把函数中的this绑定到这个上下文对象上

```js
   function foo() {
       console.log(this.a);//2
   }
   var obj = {
       a: 2,
       foo: foo,
   };
   obj.foo(); // //当foo调用的时候，它的落脚点确实是指向的obj对象，当函数引用有上下文对象的时候（obj），隐式绑定规则会把函数中的this绑定到这个上下文对象上
```

**3.隐式绑定可能会出现隐式丢失的问题 ：被隐式绑定的函数，会丢失了绑定对象**

```js
   function foo() {
       console.log(this.a);
   }
   var obj = {
       a: 2,
       foo: foo,
   };
   var fn1 = obj.foo;
   var a = "hello";
   fn1(); //hello 虽然fn1是obj.foo的一个引用，但是实际上它的引用是foo函数本身，因此fn1其实是一个不带任何修饰的函数调用，属于默认绑定

   //
   function foo() {
       console.log(this.a);//window.a 全局没有对象没有a属性，所以为undefined
   }
   function doFoo(fn) {
       fn(); //相当于foo(),还是window，
   }
   var obj = {
       a: 2,
       foo: foo,
   };
   doFoo(obj.foo);
```

4.**显式绑定**

```js
   function foo() {
       console.log(this.a);
   }
   var obj = {
       a: 2,
   };
   foo.call(obj); //2 //通过call 调用foo时，把foo的this强制的绑定给了obj上
```

5.**new绑定**

构造函数只是一些使用new

操作符被调用的函数,使用new调用函数的时候，会构造一新的对象，这个时候 就把新的对象绑定给了函数的this上

```js
function Foo(a) {
    this.a = a; //指向实例化的对象
}
var bar = new Foo(2);
console.log(bar.a); //2
```



### 怎么判断this指向

1. 函数是否在new中调用，如果是的话，this绑定的是新创建的对象
2. 函数是否通过call、apply（显示绑定）调用，如果是，则this绑定的是执行的对象
3. 函数是否在某个上下文对象中调用(隐式绑定)，如果有，则this绑定在这个上下文对象上
4. 如果以上都不是 则默认绑定 执行window

```js

var x = 1;
var obj = {
    f:function(){
        console.log(this.x)//指向obj   值为2
    },
    x:2
}
obj.f();//方法调用
var f1 = obj.f;
f1();//window.X 值为1
```

### 经典面试题

```js
function Foo() {
    abc = function () { alert(4); };
    return this;
}
Foo.prototype.abc = function () { alert(1); };
Foo.abc = function () { alert(2); };//相当于给FOO函数添加了一个属性abc(函数)
function abc() { alert(0); };
var abc = function () { alert(6); };

Foo.abc();//2
abc();//6   先执行上面的abc 0 ,后面abc 6覆盖了0
Foo().abc();//4
abc();//4
new Foo.abc();//2
new Foo().abc()//1

//先变量提升
//解析：1.Foo.abc()执行函数Foo里的属性abc，所以为2
			 2.先执行弹出0的abc函数，后面又有一个弹出6的abc函数覆盖了，所以为6
       3.执行Foo()，返回this即window,Foo函数里的abc未用var声明，执行覆盖原来的ABC变成全局的新abc，window.abc(),所					以值为4。
			 4.abc()，此时的abc已经是上一步的全局abc,所以也为4
			 5.new 调用 Foo.abc(),把Foo.abc当作整体(类似object),Foo对象中的abc，所以为2
       6.new Foo()实例对象中没有abc这个属性（方法），然后往他的构造函数的原型对象Foo.prototype上找，所以为1
       
```



## call apply 与 bind

在JavaScript中，`call`、`apply`和`bind`是`Function`对象自带的三个方法。

这三个函数的存在意义是**改变函数执行时的上下文**，再具体一点就是**改变函数运行时的this指向**。



### call

call()方法调用一个函数,其具有一个指定的this值和分别地提供的参数(参数列表).

注意:该方法的作用和'apply()'方法类似,只有一个区别,就是call()方法接受的是若干个参数的列表,而apply()方法接受的是一个包含多个参数的数组

```js
语法
fun.call(thisArg[,arg1[,arg2[,...]]])
```

参数

thisArg:

​	在fun函数运行时指定的this值

​	如果指定了null或者undefined则内部this指向window

arrg1,arrg2,...

​	指定的参数列表



**thisArg: 在fun函数运行时指定的`this`值。需要注意的是下面几种情况**

- 不传，或者传`null`，`undefined`， 函数中的`this`指向window对象
- 传递另一个函数的函数名，函数中的`this`指向这个函数的引用，并不一定是该函数执行时真正的`this`值
- 值为原始值(数字，字符串，布尔值)的`this`会指向该原始值的自动包装对象，如 `String`、`Number`、`Boolean`
- 传递一个对象，函数中的this指向这个对象

案例：

```js
function a(){
    //输出函数a中的this对象
    console.log(this); 
}
//定义函数b
function b(){} 

var obj = {name:'我是obj'}; //定义对象obj
a.call(); //window
a.call(null); //window
a.call(undefined);//window
a.call(1); //Number
a.call(''); //String
a.call(true); //Boolean
a.call(b);// function b(){}
a.call(obj); //Object
```

### apply

apply()方法调用一个函数,其具有一个指定的this值,以及作为一个数组(或类似数组的对象)提供的参数

注意:该方法的作用和'call()方法'类似,只有一个区别,就是call()方法接受的是若干个参数的列表,而apply()方法接受的是一个包含多个参数的数组

```js
语法
fun.apply(thisArg,[argsArray]);
```

参数

thisArg

argsArray

apply()与call()非常相似,不同之处在于提供参数的方式

apply()使用参数数组而不是一组参数列表

```js
例如
fun.apply(this,['eat','bananas'])

var arr = [1,2,3,4,5,6,10];
console.log(Math.max.apply(null,arr)); //10 最大值
console.log(Math.min.apply(null,arr)); //1最小值
```

### bind

- 先改变this的指向，创建一个新this指向的函数，然后返回对函数的引用`var res=fn.bind()`
- 不会立即执行，需要在后面再加一个`()`执行，
- 传参和call类似

```
语法
fn.bind(thisArg[,arg1[,arg2[,...]]])
```



### 小结

1.call和apply特性一样

- ​	都是用来调用函数,而且是立即调用

- ​	但是可以在调用函数的同时,通过第一个参数指定函数内部this指向

- ​	call调用的时候,参数必须以参数列表的形式进行传递,也就是以逗号分隔的方式依次传递即可

- ​	apply调用的时候,参数必须是一个数组,然后再执行的时候,会将数组内部的元素一个一个拿出来,与形参一一对应进行传递

- ​	如果第一个参数指定了null或者undefined则内部this指向window


2.bind

​	可以用来指定内部this的指向,然后生成了一个改变了this指向的新的函数

​	它和call,apply最大的区别是bind不会调用

​	bind支持传递参数,它的传参方式比较麻烦,一个有两个位置可以传参

​		1.在bind的同时,以参数列表的形式进行传递

​		2.在调用的时候,以参数列表的形式进行传递

​		那到底以谁 bind 的时候传递的参数为准呢还是以调用的时候传递的参数为准

​		两者合并：bind 的时候传递的参数和调用的时候传递的参数会合并到一起，传递到函数内部



### call方法的执行顺序

call方法是个函数

```js
function call(){
	this()
}

假设fn.call(fn2)，执行如下
1，先把调用call方法的对象的this指向 改为call方法的第一个参数，也就是把fn的this指向改为了fn2。
2.调用call方法自己的this(call方法的this指向调用自己的对象)，this()=fn()（也就是把调用call方法的对象fn给调用了）
```



### 面试题

```js
function fn1() {
    console.log(1);
}
function fn2() {
    console.log(2);
}
fn1.call(fn2); //1
fn1.call.call(fn2);//2
//解析：1.第一个call并没有`()`调用，所以这里跟fn1没什么关系
		 //2.把fn1.call当成一个函数，实则就是call函数，只不过call恰巧需要函数来调用而已
		 //3.第一个call函数调用了第二个call方法，执行顺序第二个call先把第一个call的this指向改为了fn2，再调用了call方法自己的this，自己的this指向第一个call，，第一个call this又指向fn2
     //4.由于第一个call的this指向前面已经改成fn2，所以执行时this()=fn2(),调用了fn2函数，所以答案为2
```





## 原型链

### 函数的prototype属性

* 每个函数都有一个prototype属性, 它默认指向一个Object空对象(即称为: 原型对象)

  

  ```js
  console.log(Date.prototype, typeof Date.prototype)
  function fn() {}
  console.log(fn.prototype, typeof fn.prototype)
  ```

  

* 原型对象中有一个属性constructor, 它指向函数对象

  

  ```js
  console.log(Date.prototype.constructor===Date)
  console.log(fn.prototype.constructor===fn)
  ```

  


- 给原型对象添加属性(一般都是方法)，函数的所有实例对象自动拥有原型中的属性(方法)

  

  ```js
  function F() {}
  F.prototype.age = 12 //添加属性
  F.prototype.setAge = function (age) { // 添加方法
      this.age = age
  }
  // 创建函数的实例对象
  var f = new F()
  console.log(f.age)
  f.setAge(23)
  console.log(f.age)
  ```



### 显式原型与隐式原型

- 每个函数function都有一个prototype，即显式原型

- 每个实例对象都有一个__proto__，可称为隐式原型

- 对象的隐式原型的值为其对应构造函数的显式原型的值

- 内存结构如图：

- 总结
  * 函数的prototype属性: 在定义函数时自动添加的, 默认值是一个空Object对象，创建函数的实例对象时使用。
  * 对象的__proto__属性: 创建对象时自动添加的, 默认值为构造函数的prototype属性值，当查找对象中的某个属性，但对象本身没有
  * 程序员能直接操作显式原型, 但不能直接操作隐式原型(ES6之前)



### 原型链

#### 基础原型链

- 所有的实例对象都有__proto__属性, 它指向的就是原型对象
- 这样通过__proto__属性就形成了一个链的结构---->原型链
- 当查找对象内部的属性/方法时, js引擎自动沿着这个原型链查找
- 当给对象属性赋值时不会使用原型链, 而只是在当前对象中进行操作



```js
function Fn() {
    this.test1 = function () {
        console.log('test1()')
    }
}
Fn.prototype.test2 = function () {
    console.log('test2()')
}
var fn = new Fn()

fn.test1()
fn.test2()
console.log(fn.toString())
fn.test3()
```



#### 构造函数/原型/实例对象的关系(图解)

```js
var o1 = new Object();
var o2 = {};
```





#### 构造函数/原型/实例对象的关系2(图解)

- 所有函数对象的__proto__属性值都相等
- 所有函数都是Function的实例, 包含它自己



  

#### 原型链属性问题

1. 读取对象的属性值时: 会自动到原型链中查找
2. 设置对象的属性值时: 不会查找原型链, 如果当前对象中没有此属性, 直接添加此属性并设置其值
3. 方法一般定义在原型中, 属性一般通过构造函数定义在对象本身上



```js
function Person(name, age) {
    this.name = name;
    this.age = age;
}
Person.prototype.setName = function (name) {
    this.name = name;
}
Person.prototype.sex = '男';

var p1 = new Person('Tom', 12)
p1.setName('Jack')
console.log(p1.name, p1.age, p1.sex)
p1.sex = '女'
console.log(p1.name, p1.age, p1.sex)

var p2 = new Person('Bob', 23)
console.log(p2.name, p2.age, p2.sex)
```



#### 探索instanceof

1. instanceof是如何判断的?

   - 表达式: A instanceof B

   - 如果B函数的显式原型对象在A对象的原型链上, 返回true, 否则返回false

2. Function是通过new自己产生的实例



```js
function Foo() {  }
var f1 = new Foo();
console.log(f1 instanceof Foo);
console.log(f1 instanceof Object);

//案例2
console.log(Object instanceof Function)
console.log(Object instanceof Object)
console.log(Function instanceof Object)
console.log(Function instanceof Function)
console.log(Object instanceof  Foo);

console.log(Function.prototype.__proto__===Object.prototype)
console.log(Function.prototype, {})
```

#### 最终原型链图

![image-20200728144952491](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200728144952491.png)







## 执行上下文和执行上下文栈

### 变量提升与函数提升

- **问题: 变量提升和函数提升是如何产生的?**

- **变量提升和函数提升是通过变量对象来实现的**
- **创建变量对象，将变量放入对象中。**

### 执行上下文

- JS引擎并不是一行行的解析和执行代码，而是一段段的去分析和执行，当执行一段代码时，先开始预处理，比如声明提升和函数提升
- 在执行某段js代码的时候，会进行一个准备工作，这个准备工作用专业的说法 叫“执行上下文”，其实执行上下文也是在内存中开辟的一个空间
- js可执行的代码分为3种类型， 全局代码  、  函数代码   、eval代码（忽略）
- 每执行一段代码，都会创建相对应的执行上下文，在脚本中可能存在多个执行上下文
- 因为有太多的执行上下文， JS创建了一个执行上下文栈（stack） 用来管理执行上下文
- 当js开始解析程序的时候，最先遇到的全局代码，此时向执行上下文栈中 压入一个全局执行上下文，全局的一定是在整体运行结束以后才被清空
- 当执行一个函数的时候  会创建一个函数的执行上下文，并压入到执行上下文栈中，只要函数执行完成，会将函数从栈里弹出

### 变量对象

- **每个执行上下文 都有三个重要属性：1.变量对象（VO） 2.作用域链  3.this**
- 变量对象是 ECMAScript规范术语。在一个执行上下文中,变量对象才被激活,只有激活的变量对象,其各种属性才能被访问
- 变量对象是与执行上下文相关的数据作用域，储存了在上下文中定义的变量和函数声明
- 变量对象中添加所有的形参和实参

#### 全局上下文的变量对象

- `window`是预定义对象，作为JS全局函数和全局属性的占位符（全局的变量和函数就是window对象属性和方法）
- 全局执行上下文的变量对象其实就是全局对象window

#### 函数上下文的变量对象

- 进入执行上下文  不会立马执行代码，只进行分析。此时首先第一步，变量对象包括了函数所有的形参和实参
- 检查所有声明的函数，由名称和对应值 组成一个变量对象的属性 被创建。如果变量对象已经有相同名字的属性，则完全替换
- 检查所有的声明的变量，创建键值对儿 
- 变成变量对象的属性，如果变量名和已经声明的形参或函数相同，则变量声明不会干扰已经存在的这类属性

```js
function foo(a) {
    var b = 2;
    function c() {}
    var d = function () {};
    b = 3;
}
foo(1, 2);

//模拟以上代码的变量对象：
var AO = {
    //arguments是保存所有的的实参
    arguments:{
        0:1,
        1:2,
        length:2
    },
    a:1,
    c:function c(){},
    b:undefined,
    d:undefined
}

```



#### 作用域与执行上下文

- 区别1
  - 全局作用域之外，每个函数都会创建自己的作用域，**作用域在函数定义时就已经确定了。而不是在函数调用时**
  - 全局执行上下文环境是在全局作用域确定之后, js代码马上执行之前创建
  - 函数执行上下文环境是在调用函数时, 函数体代码执行之前创建
- 区别2
  - 作用域是静态的, 只要函数定义好了就一直存在, 且不会再变化
  - 上下文环境是动态的, 调用函数时创建, 函数调用结束时上下文环境就会被释放
- 联系
  - 上下文环境(对象)是从属于所在的作用域
  - 全局上下文环境==>全局作用域
  - 函数上下文环境==>对应的函数使用域



#### 作用域链

- 当代码在一个环境中执行时,会创建变量对象的一个作用域链( scope chain),作用域链的用途,**是保证对执行环境有权访问的所有变量和函数的有序访问**。
- 作用域链的前端,始终都是当前执行的代码所在环境的变量对象。
- 全局执行环境的变量对象始终都是作用域链中的最后一个对象。
- 标识符解析是沿着作用域链一级一级地搜索标识符的过程。搜索过程始终从作用域链的前端开始，然后逐级地向后回溯,直至找到标识符为止(如果找不到标识符,通常会导致错误发生)

```js
var fn = function () {
    console.log(fn) //打印fn函数体
}
fn()

var obj = {
    fn2: function () {
        console.log(fn2) //报错，要通过对象才能访问到fn2
    }
}
obj.fn2() 
```





# 闭包

### 理解闭包

- 如何产生闭包?

  - 当一个嵌套的内部(子)函数引用了嵌套的外部(父)函数的变量(函数)时, 就产生了闭包

    

    ```js
    // 1. 将函数作为另一个函数的返回值
    function fn1() {
        var a = 2
        function fn2() {
            a++
            console.log(a)//这里需要外面的A所以A没销毁
        }
        return fn2
    }
    var f = fn1()
    f() // 3
    f() // 4
    
    //var f = fn1(),将fn1()的返回值也就是fn2的地址赋给了f,
    //函数地址在就不销毁，所以里面的闭包也在，闭包在的话，闭包里面的变量a也保存在
    ```

    

- 闭包到底是什么?

  - 使用chrome调试查看
  - 理解一: 闭包是嵌套的内部函数(绝大部分人)
  - 理解二: 包含被引用变量(函数)的对象(极少数人)
  - 注意: 闭包存在于嵌套的内部函数中

- 产生闭包的条件?

  - 函数嵌套
  - 内部函数引用了外部函数的数据(变量/函数)
  - 执行外部函数

- 常见闭包

  - 将函数作为另一个函数的返回值  但是需要有变量来接收

  - 将函数作为实参传递给另一个函数调用

    

    ```js
    // 1. 将函数作为另一个函数的返回值
    function fn1() {
        var a = 2
        function fn2() {
            a++
            console.log(a)
        }
        return fn2
    }
    //只要f的地址还是   之前的fn2所代表的函数   则闭包一直都在
    var f = fn1()
    f() // 3
    f() // 4
    
    var f2 = fn1();//创建一个新的闭包
    f2();//3
    
    
    
    // 2. 将函数作为实参传递给另一个函数调用
    function showMsgDelay(msg, time) {
        setTimeout(function () {
            console.log(msg)
        }, time)
    }
    showMsgDelay('hello', 1000)
    

    
    for (var i = 0; i < 3; i++) {
        (function (i) {
            setTimeout(function () {
                console.log(i);
            }, 0)
        })(i)
    }
    ```
    
    

- 闭包的作用

  - 使用函数内部的变量在函数执行完后, 仍然存活在内存中(延长了局部变量的生命周期)

  - 让函数外部可以操作(读写)到函数内部的数据(变量/函数)

    

    ```js
    function fun1() {
        var a = 3;
    
        function fun2() {
            a++;            //引用外部函数的变量--->产生闭包
            console.log(a);
        }
    
        return fun2;
    }
    var f = fun1();  //由于f引用着内部的函数-->内部函数以及闭包都没有成为垃圾对象
    
    f();   //4   间接操作了函数内部的局部变量
    f();   //5
    ```

    

- 闭包的生命周期

  - 产生: 在嵌套内部函数定义执行完(创建函数对象)时就产生了(不是在调用)

  - 死亡: 在嵌套的内部函数成为垃圾对象时

    

    ```js
    function fun1() {
        //此处闭包已经产生
        var a = 3;
    
        function fun2() {
            a++;
            console.log(a);
        }
    
        return fun2;
    }
    var f = fun1();
    
    f();
    f();
    f = null //此时闭包对象死亡
    ```

    

### 闭包的应用

定义JS模块

  * 具有特定功能的js文件
  * 将所有的数据和功能都封装在一个函数内部(私有的)
  * 只向外暴露一个包含n个方法的对象或函数
  * 模块的使用者, 只需要通过模块暴露的对象调用方法来实现对应的功能

### 闭包的缺点及解决

- 缺点
  - 函数执行完后, 函数内的局部变量没有释放, 占用内存时间会变长
  - 容易造成内存泄露
- 解决
  - 能不用闭包就不用
  - 及时释放

### 闭包总结（上面理解，结论看这里）

#### 什么是闭包：

​	**1.闭包的产生是函数嵌套函数、内部函数引用外部函数变量、外部函数调用**

​	**2.闭包可以理解为内部的函数**

​	**3.深入理解的话，闭包就是 ：包含 被内部引用变量 的对象（浏览器调试中对象名为 closure）**

#### 闭包作用（优点）：

​	**1.外部间接的访问到了内部的变量**

​	**2.当外部函数执行完成后，局部变量暂时不会消失，延长了局部变量的生命周期**



#### 闭包的缺点：

​	**内存泄漏**



#### 解决方法：

​	**1.不用闭包** 

​	**2.及时让闭包所在的函数变成垃圾对象（f=null）**



#### 闭包的死亡周期：

​	**1.产生：当内部函数被定义的时候就产生了**

​	**2.死亡：当内部函数没有被引用的时候，函数就会成为垃圾对象，并且闭包随之消失**



#### 面试题



```js
//代码片段一
var name = "The Window";
var object = {
    name: "My Object",
    getNameFunc: function () {
        return function () {
            return this.name;
        };
    }
};
console.log(object.getNameFunc()());  //The Window

//代码片段二
var name2 = "The Window";
var object2 = {
    name2: "My Object",
    getNameFunc: function () {
        var that = this;
        return function () {
            return that.name2;
        };
    }
};
console.log(object2.getNameFunc()()); //My Object



function fun(n, o) {
  console.log(o)
  return {
      fun: function (m) {
          return fun(m, n)
      }
  }
}
var a = fun(0)//undefined
a.fun(1);//0
a.fun(2);//0
a.fun(3);//0

var b = fun(0).fun(1).fun(2).fun(3)//undefined 0 1 2

var c = fun(0).fun(1) //undefined 0
c.fun(2) //1
c.fun(3) //1

```





## 线程机制和事件机制

### 进程与线程

- 进程：程序的一次执行, 它占有一片独有的内存空间
- 线程： 进程内的一个独立单元，CPU的基本调度单位, 是程序执行的一个完整流程

进程与线程

- 一个进程中一般至少有一个运行的线程: 主线程
- 一个进程中也可以同时运行多个线程, 我们会说程序是多线程运行的
- 一个进程内的数据可以供其中的多个线程直接共享
- 多个进程之间的数据是不能直接共享的

### 比较单线程和多线程

多线程：

​	优点  能有效提升CPU的利用率

​	缺点  创建多线程开销  线程间切换开销  死锁与状态同步问题

单线程：

​	优点  顺序编程简单易懂

​	缺点  效率低



**js是单线程运行的，但是使用H5中的 Web Workers可以多线程运行，浏览器都是多线程**



## 浏览器引发的思考

- 定时器真是定时执行的吗?
  - 定时器并不能保证真正定时执行
  - 一般会延迟一丁点(可以接受), 也有可能延迟很长时间(不能接受)
- 定时器回调函数是在分线程执行的吗?
  - 在主线程执行的, js是单线程的
- 定时器是如何实现的?
  - 事件循环模型(后面讲)

```js
document.getElementById('btn').onclick = function () {
    var start = Date.now()
    console.log('启动定时器')
    setTimeout(function () {
        console.log('定时器执行了: ', Date.now()-start)
    }, 100)

    //定时器启动之后做一个长时间的工作
    for (var i = 0; i < 1000000000; i++) {

    }
    console.log('完成长时间工作', Date.now()-start)
}
```

## 事件循环模型

- 所有代码分类

  - 初始化执行代码(同步代码): 包含绑定dom事件监听, 设置定时器, 发送ajax请求的代码
  - 回调执行代码(异步代码): 处理回调逻辑

- js引擎执行代码的基本流程

  - 初始化代码===>回调代码

- 模型的2个重要组成部分

  - 事件管理模块
  - 回调队列

- 模型的运转流程

  - 执行初始化代码, 将事件回调函数交给对应模块管理
  - 当事件发生时, 管理模块会将回调函数及其数据添加到回调列队中
  - 只有当初始化代码执行完后(可能要一定时间), 才会遍历读取回调队列中的回调函数执行

  ![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gh5zsm0f2cj310p0u076v.jpg)

## H5多线程

- H5规范提供了js分线程的实现, 取名为: Web Workers
- 相关API
  - Worker: 构造函数, 加载分线程执行的js文件
  - Worker.prototype.onmessage: 用于接收另一个线程的回调函数
  - Worker.prototype.postMessage: 向另一个线程发送消息
- 不足
  - worker内代码不能操作DOM(更新UI)
  - 不能跨域加载JS
  - 不是每个浏览器都支持这个新特性

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<button id="btn1">主线程运算计算</button>
<button id="btn2">worker计算</button>
<button>我是否假死</button>
<script>
    btn1.onclick=function () {
        var num=0;
        for (var i=0;i<999999999;i++){
            num+=i;
            num+=Math.sqrt(num);
        }
        alert(num);
    }

    btn2.onclick=function () {
        // 1.创建一个worker
        var wk=new Worker("worker1.js");
        //2.把需要计算的数据传递到worker
        wk.postMessage(9999999);

        // 主线程也要书写一个事件，当子线程传输回来数据的时候，会直接出发message事件
        wk.onmessage=function (e) {
            alert(e.data);
            wk.terminate();
        }

    }
</script>
</body>
</html>
//在worker中用this调用message事件
//在event事件对象中，它的data就是postmessage传输过来的值
this.onmessage=function (e) {
    var num=0;
    for (var i=0;i<e.data;i++){
        num+=i;
        num+=Math.sqrt(num);
    }
    // 在worker中，可以直接使用postmessage把得到数据传送回主线程
    postMessage(num);
    close();
}
```





## 对象的继承模式

继承是子类对象能够使用父类对象的数据和行为。

### 借用构造函数继承

套路:

1. 定义父类型构造函数
2. 定义子类型构造函数
3. 在子类型构造函数中调用父类型构造

```js
/*借用构造函数继承*/
function Animal(age) {
    this.age = age;
    this.eat = function () {
        console.log("eat~")
    }
}

//让Cat类都拥有Animal类的属性和方法
function Cat(color) {
    this.color = color;
    this.say = function () {
        console.log("miao~");
    }
    Animal.call(this, 5);  //让Animal中的this指向本this（也就是cat1）
}
var cat1 = new Cat("yello");
console.log(cat1.age);
console.log(cat1.eat);
```

​	



### 原型链继承

把父类实例化对象赋值给 子类的原型对象 就可以实现原型对象上方法和属性的继承

关键

1. 子类型的原型为父类型的一个实例对象,这样就可以通过实例对象的proto找到其父类型（构造函数）原型对象上的方法

```js
function Animal(age) {
    this.age = age;
}

Animal.prototype.eat = function () {
    console.log("eat~")
}

function Cat(color) {
    this.color = color;
}

Cat.prototype = new Animal(); //重新设置Cat的原型对象，让Cat的原型对象为拥有Animal构造函数原型对象的对象、

Cat.prototype.constructor = Cat;  //修正原型链
Cat.prototype.say = function () {  //自己原型对象的属性书写在重置原型对象后，不然会被覆盖掉
    console.log("miao~");
}
var cat1 = new Cat("yellow");
console.log(cat1.age);// age未传参，所以为undefined
console.log(cat1.say);
console.log(cat1.eat);        
```



### 混合继承

原型链+借用构造函数的组合继承





```js
function Animal(age) {
    this.age = age;
}
Animal.prototype.eat = function () {
    console.log("eat~")
}


function Cat(color) {
    this.color = color;
    Animal.call(this, 3); //只能继承私有的属性和方法
}
//共有的属性和方法在prototype上，需要用prototype来实现继承
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;


Cat.prototype.say = function () {
    console.log("miao")
}
var cat1 = new Cat('yellow');
console.log(cat1)

```





**大概的图**

![image-20200728192441533](C:\Users\宋文现\AppData\Roaming\Typora\typora-user-images\image-20200728192441533.png)













## ES5(补充)

## 严格模式

### 理解

- 除了正常运行模式(混杂模式)，ES5添加了第二种运行模式："严格模式"（strict mode）。
- 顾名思义，这种模式使得Javascript在更严格的语法条件下运行

### 目的/作用

- 严格模式修复了一些导致 JavaScript引擎难以执行优化的缺陷：有时候，相同的代码，严格模式可以比非严格模式下运行得更快
- 消除代码运行的一些不安全之处，为代码的安全运行保驾护航
- 为未来新版本的JavaScript做好铺垫

### 使用

- 在全局或函数的第一条语句定义为: 'use strict',将"use strict"放在函数体的第一行，则整个函数以"严格模式运行"。
- 如果浏览器不支持, 只解析为一条简单的语句, 没有任何副作用

### 语法和行为改变

- **变量必须声明才能使用**（在正常模式中，如果一个变量没有声明就赋值，默认是全局变量。严格模式禁止这种写法）

- 禁止自定义的函数中的this指向window（**严格模式下全局作用域中定义的函数中的this为undefined**）

- 创建eval作用域

- 禁止在函数内部遍历调用栈( caller:调用当前函数的函数的引用，即外层函数的引用； )

- 参考地址：https://segmentfault.com/a/1190000015798019

  ```js
  function f1(){
      "use strict";
      f1.arguments; //报错
  }
  f1();
  ```







## JSON对象

### 什么是JSON对象]

- JSON全称是JavaScript Object Notation,是一种轻量级的数据交换格式。JSON 与XML具有相同的特性，是一种数据存储格式，但是JSON相比XML 更易于人编写和阅读，更易于生成和解析。

- **JSON对象是ES中内置的对象，不用定义，直接就能用。**

  **大写的JSON表示的是js中内置的对象，小写的json表示的是一种数据格式。**

  * **json是一种数据格式；我们以后大多数时候都是通过json数据格式进行前后端数据交互。**

  * **json本质上就是字符串，简称json串。**

    **后台往前台传递数据时，要传json格式的数据。**

- **json中对象要求给属性加上双引号。**



### JSON对象的方法

- **JSON.stringify(obj/arr)**
  - js对象(数组) --> 转换为json对象(数组)(字符串类型)

```js
 //字符串类型的json
        var data1 = '{"name":"xiaowang","score":[100,20,100]}';
        var data2 = '[1,2,3,4,5]';
   
   //JSON.parse 把json字符串转换为js对象
        console.log(JSON.parse(data1))
        console.log(JSON.parse(data2))

```



- **JSON.parse(json)**

  - json对象(数组)(字符串类型) --> 转换为js对象(数组)

  ```js
   				//js对象数组
   				var obj1 = {
              "name": "xiaowang",
              "score": [100, 20, 100]
          };
          var obj2 = [1, 2, 3, 4, 5];
          
          //JSON.stringify把js对象转换为JSON字符串
          console.log(JSON.stringify(obj1))
          console.log(JSON.stringify(obj2))
  ```



## Object扩展

### 使用 Object create方法

- Object.create是 ECMAScript5新增的一个静态方法,用来定义一个实例对象。**(声明对象的另一种方式)**

- 该方法可以指定对象的原型和对象特性，使用现有的对象来提供新创建的对象的proto指向。

  。具体用法如下

  ```
  object.create (prototype, descriptors)
  ```

  - **参数1 prototype**:必须参数,指定原型对象,可以为**null，已有对象，object.prototype**
  - **Object.create创建对象的时候 可以设置原型对象（可以继承）**

  ```js
  var myObj = {
      name: "xiaowang",
      age: 18
  }
  //3.Object.create把第一个参数的已有对象作为新对象的原型对象
  var obj2 = Object.create(myObj);
  console.log(obj2)  //新对象的proto指向myObj
  console.log(obj2.name) //可以使用，因为原型对象上有这个属性
  ```

  

  **想要通过create方法创建一个普通的对象（字面量创建的对象）**

  ```js 
   var obj3 = Object.create(Object.prototype);
   console.log(obj3)
  ```

  

  **当第一个参数是null的时候，就是创建一个干净的对象**

  ```js
   var obj4 = Object.create(null);
   console.log(obj4);
  ```

  

  - **参数2 descriptors可选参数**,包含一个或多个属性描述符的 JavaScript对象。属性描述符包含数据特性和访问器特性,其中数据特性说明如下
    - value:指定属性值
    - writable:默认为 false,设置属性值是否可写  **不写默认false**
    - enumerable:默认为 false,设置属性是否可枚举( for/in)  **不写默认false**
    - configurable:默认为false,设置是否可修改属性特性和删除属性  **不写默认false**

**第二个参数是设置新创建对象的属性 格式是一个对象**

**对象属性的描述也必须是一个对象**

```js
 //第二个参数是设置新创建对象的属性 格式是一个对象
var obj5 = Object.create(null, {
    name: { //对象属性的描述必须是一个对象
        value: "lily", //value属性就是对象属性的值
        writable: true, //设置对象值是否可写，默认是false ，value不能改写
        enumerable: true, //设置对象值是否可以枚举 默认是false， name(属性)不能被遍历，仅限当前属性
        configurable: true //设置对象的属性是否可以被删除 及 对象属性的描述是否可以被修改
    }
})
```



### Object.defineProperty（）添加修改属性

使用 Object.defineProperty函数可以为**对象添加属性,或者修改现有属性**。如果指定的属性名在对象中不存在,则执行添加操作:如果在对象中存在同名属性,则执行修改操作

```js
 Object.defineProperty(obj, "name", {
            value: "laowang", //如果这个属性是正常创建的，则可以正常使用，比如可以被枚举
            enumerable: false,//会让这个属性打印颜色变虚
            writable: false,
            configurable: false
        })
```



### Object.defineProperties() 添加修改属性

- 可以一次修改多个属性
- Object.defineProperties(object,description)
  - object:对其添加或修改属性的对象,可以是本地对象或DOM对象
  - description:包含一个或多个描述符对象,每个描述符对象描述一个数据属性或访问器属性

```js
var obj = {
    name: "xiaowang",
    age: "18",
    sex: "boy"
}
Object.defineProperties(obj, {
    name: {
        value: "laowang"
    },
    sex: {
        value: "19"
    }
})
```

**上述两个方法也有 enumerable  writable  configurable等参数**

**与Object.create设置对象属性的区别是 enumerable  writable  configurable等参数不写的话默认为true，可遍历可改写**





## 存取器属性 getter和setter的实现

- getter负责查询值（取），它不带任何参数，setter则负责设置键值（存），值是以参数的形式传递，在他的函数体中，一切的return都是无效的。

```js
var obj1 = {
    firstName: "yan",
    lastName: "haijing",
    get fullName() { //getter 一个对象可以有对个存储器属性的  定义fullName属性的读取的方法
        return this.firstName + " " + this.lastName;
    },
    set setName(a) { //setter一定要有参数     定义fullName属性的修改/设置/写的方法
        var nameArr = a.split(" ");
        this.firstName = nameArr[0];
        this.lastName = nameArr[1];
    }
}
console.log(obj1);
console.log(obj1.fullName); //是个属性 不用添加括号调用
obj1.setName = "li peihua"; //是个属性 直接赋值即可（将值以以参数传给a）
console.log(obj1.fullName)
```





# ES6

# 关键字扩展

## 1 let和块级作用域

### 1.1 函数作用域

- **在ES5中,JS的作用域分为全局作用域和局部作用域。通常是用函数区分的，函数内部属于局部作用域。**
- **用var声明的变量 都是根据函数确定作用域的**
- **ES5 只有全局作用域和函数作用域，没有块级作用域，这带来很多不合理的场景如下：**

- 内层变量可能会覆盖外层变量。

  ```js
  var b = 1;
  fn1();
  function fn1() {
      console.log(b);//undefined
      if (true) {
          var b = 2;
      }
  }
  ```

- 用来计数的循环变量泄露为全局变量。

  ```js
  //变量i是var命令声明的，在全局范围内都有效，所以全局只有一个变量i。每一次循环，变量i的值都会发生改变
  for (var i = 0; i < 5; i++) {
      setTimeout(function () {
          console.log(i);//5 5 5 5 5
      })
  }
  ```

### 1.2 块级作用域

- 在ES6中新增了块级作用域的概念，使用{}扩起来的区域叫做块级作用域
- let关键字声明变量，实际上为 JavaScript 新增了块级作用域。
- **块作用域由 { } 包括，if语句和for语句里面的{ }也属于块作用域。**
- **在块内使用let声明的变量，只会在当前的块内有效。**
- **使用let和const声明变量 都是以代码块确定作用域**
- 作用域是通过声明变量的方式确定的
-  对于函数来说，规范不统一 所以尽量不要再块作用域中声明函数，如果非要声明，则使用函数表达式的方式
-  代码块：{}

```js
//全局声明一个变量b
let b = 1;
fn1();
function fn1() {
    //当前的作用于没有b，沿着作用于寻找到全局的b
    console.log(b);
    if (true) {
        //只在当前的作用于中生效
        let b = 2;
    }
}
```



### 1.3 let关键字

ES6 新增了`let`命令，用来声明变量。它的用法类似于`var`，但是所声明的变量，只在`let`命令所在的代码块内有效，也就是增加了块级作用域。

- **使用块级作用域（let定义的变量属于块级作用域） 防止全局变量污染**

  ```js
  {
      let b = 20;
  }
  console.log(b);//Uncaught ReferenceError: b is not defined
  ```

- **块级作用域可以任意嵌套**

  ```js
  //块级作用域和函数作用域类似，可以有作用域链的关系
  //外层作用域无法读取内层作用域的变量 
  {
      {
          let a = 1;
          {
             console.log(a); //1
          }
      }
      console.log(a);//Uncaught ReferenceError: a is not defined
  }
  ```

- **for循环的计数器，就很合适使用let命令**

  ```js
  //计数器i只在for循环体内有效，在循环体外引用就会报错
  for (let i = 0; i < 4; i++) {     //这里声明的i是局部变量
      console.log(i); //0 1 2 3
  }
  console.log(i); //报错
  ```

- 变量i是let声明的，当前的i只在本轮循环有效，所以每一次循环的i其实都是一个新的变量

  > 你可能会问，如果每一轮循环的变量i都是重新声明的，那它怎么知道上一轮循环的值，从而计算出本轮循环的值？这是因为 JavaScript 引擎内部会记住上一轮循环的值，初始化本轮的变量i时，就在上一轮循环的基础上进行计算

  ```js
  for (let i = 0; i < 5; i++) {
      setTimeout(function () {
          console.log(i);//0 1 2 3 4
      })
  }
  //上述for是父作用域，代码块{}是子作用域，定时器函数是函数作用域，此处形成闭包
  //每次循环代码块作用域都保存了当时的变量i，当定时器函数里要用i时，自身没有，向上找到了代码块作用域里的i使用
  ```

- for循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域

  ```js
  for (let i = 0; i < 4; i++) {
      let i = "a";
      console.log(i);//a
  }
  console.log(i);//报错
  ```

### 1.4 let关键字特点

1. **没有声明提升**

   ```js
   //ES6 明确规定，如果区块中存在let和const命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错）
   // 这样的设计是为了让大家养成良好的编程习惯，变量一定要在声明之后使用，否则就报错
   let c = 1;
   {
       console.log(c);//Uncaught ReferenceError(引用错误): Cannot access 'c' before initialization
       let c = 2;
   }
   ```

2. **不允许重复声明（不能再同一个作用域中声明一个变量多次）**

   ```js
   // let不允许在相同作用域内，重复声明同一个变量
   // 报错
   function func() {
       let a = 10;
       var a = 1;
   }
   
   // 报错
   function func() {
       let a = 10;
       let a = 1;
   }
   ```

3. **块级作用域的出现，实际上使得获得广泛应用的匿名立即执行函数表达式（匿名 IIFE）不再必要了**

   ```js
   // IIFE 写法
   (function () {
       var tmp = "...;"
   }());
   
   // 块级作用域写法
   {
       let tmp = "...;"
   }
   ```



## 2 const关键字

**一般先用const声明，觉得要变的就用let,const居多**

常量：不会变化的数据，有些时候有的数据是不允许修改的，所以需要定义常量。

- `const`声明一个只读的常量。定义常量可以写大写。

  ```js
  const PI = "3.1415926";
  console.log(PI)
  ```

- **`const`声明的常量不得改变值**

  ```js
  const PI = "3.1415926";
  PI = 22;//Assignment to constant variable.(报错：给常量赋值)
  ```

- **`const`声明的常量如果是对象，可以修改对象的内容**

  ```js
  // const只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了
  //但是对象的引用不能被修改，比如用一个对象替代这个对象，那么引用就被修改了
  const PATH = {};
  PATH.name = "lily";
  console.log(PATH);//{name:"lily"}
  PATH = {}//Assignment to constant variable
  ```

- **`const`一旦声明变量，就必须立即初始化，不能留到以后赋值**

  ```js
  const time;//SyntaxError: Missing initializer in const declaration
  ```

- **`const`只在声明所在的块级作用域内有效**

  ```js
  {
      const PI = 3.14;
  }
  console.log(PI)//ReferenceError(引用错误)PI is not defined
  ```

- **`const`命令声明的常量也是不提升**

  ```js
  console.log(c);//Cannot access 'c' before initialization
  const c = "hello"
  ```

- **`const`声明的常量，也与`let`一样不可重复声明**

  ```js
  const PI = "3.14";
  const PI = "3.14";//SyntaxError: Identifier(标识符) 'PI' has already been declared(声明)
  ```

- **let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性。也就是说，从 ES6 开始，全局变量将逐步与顶层对象的属性脱钩。**

  ```js
  var c = 3;
  let a = 1;
  const b = 2;
  console.log(window.c) //3
  console.log(window.a) //undefined
  console.log(window.b) //undefined
  
  ```



# 变量解构赋值

## 1. 什么是变量的解构赋值

**ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。**

**解构赋值 本质就是赋值，把结构解散重构 然后赋值 其实是一种模式的匹配，关键要掌握一一对应关系。**

**解构赋值作用就是方便赋值。**



## 2 数组的解构赋值

- **数组解构赋值可以从数组中提取值，按照对应位置，对变量赋值**

  ```js
  let arr = [1, 2]
  let [a, b] = arr;
  console.log(a, b)
  ```

- **解构失败返回undefined**

  ```js
  let [c, d] = [1];
  console.log(d);//undefined
  ```

- **不完全解构也可以成功**

  ```js
  let [a] = [10, 20];
  console.log(a);
  ```

- **可以为解构赋值可以设置默认值(有就覆盖没有就用默认)**

  ```js
  let [e = 2, f = 20] = [10];
  console.log(e, f)//10 20
  ```

- **可以使用rest参数（当值比变量多的时候）**

  ```js
  let [g, h, ...rest] = [10, 11, 23, 22, 12, 3, 4, 5, 3];
  console.log(g, h, rest)//10，11，[23, 22, 12, 3, 4, 5, 3]
  ```

- **多维数组只要解构关系一一对应也可以进行赋值**

  ```js
  let [a, [b, c, d], e] = [1, [2, 3, 4], 5]
  console.log(a, b, c, d, e)
  ```

**练习**

```js
 //练习：交换两个变量的值
        {
            let a = 1;
            let b = 2;
            [b, a] = [a, b]
            console.log(a, b)
        }
```

**练习2**

```js
 let [foo = hoo, hoo = 2] = [];
 console.log(foo, hoo); //报错 给foo设置默认值得时候 hoo还没有被初始化 

//这里使用默认值foo=hoo,hoo初始化在后，没有提升，所以报错
//let [hoo = 2,foo = hoo] = [] ,把hoo = 2放前面就可以
```

**练习3**

```js
//通过解构赋值获取数组的第一个值和最后一个值 
const arr = [1, 2, 3, 4];
const {0: a,[arr.length - 1]: d } = arr;//当对象的属性加上中括号，则可以是变量
console.log(a, d)

```

## 3 对象的解构赋值

- 解构不仅可以用于数组，还可以用于对象.

- 对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值

- 对应关系是根据key不需要考虑顺序

  ```js
  //对象的解构赋值(对应关系是根据key)
  let { foo, hoo } = { foo: "hello", hoo: "word" };//key和value一样可以简写
  console.log(foo, hoo);
  
  //上边{foo,hoo}这样写法的来历
  //其实如果对象的key和value是一样的，可以简写
  let a = 1, b = 2;
  console.log({ a: a, b: b })
  console.log({ a, b })
  
  //找到foo对应的关系，然后把1赋值给c
  let { foo: c, hoo: d } = { foo: 1, hoo: 2 };
  console.log(foo, hoo);//报错
  console.log(c, d);//1,2
  ```

- 对象解构赋值可以设置默认值

  ```js
  let { x, y = 10 } = { x: 1 };
  console.log(x, y)
  ```

- 对象和数组解构可以嵌套

  ```js
  let obj = { a: 1, b: [2, 3, 4] }
  let { a, b: [b1, b2, b3] } = obj;
  console.log(a, b1, b2, b3)
  ```

- 练习

  ```js
  //练习1：如果想要获取数组的第一个和最后一个值怎么办(使用解构赋值)
  let arr = [1, 2, 3];
  // 由于数组本质是特殊的对象，因此可以对数组进行对象属性的解构
  let { 0: first, [arr.length - 1]: last } = arr;
  
  //练习2:获取log方法
  let { log } = console;
  log(1);
  ```



## 4 解构赋值的应用

- 函数的多个返回值获取

  ```js
  //1.接收函数的多个返回值
  function fn(){
      return [1,2,3,4];
  }
  let [a,b,c,d] = fn();
  console.log(a,b,c,d);
  
  function fn() {
      return { foo: "hello", hoo: "word" };
  }
  let { foo } = fn();
  console.log(foo);
  ```

- 函数传参数

  ```js
  //2.函数传参数
  function fn1([x, y, z]) {
      console.log(x, y, z);
  }
  fn1([1, 2, 3]);
  
  function fn2({ x, y, z }) {
      console.log(x, y, z);
  }
  fn2({ y: 2, x: 1, z: 3 });
  ```

- json数据的提取

  ```js
  //3.json数据的提取
  let json = {
      name: "lily",
      sex: "nv",
      age: 18
  }
  let { name, age, sex } = json;
  console.log(name, age, sex)
  ```





# 字符串的扩展

## 1. 模版字符串

- 传统的 JavaScript 语言，输出模板通常要拼接字符串

```js
  // 原始方法做模版
  let data = {
      message: {
          title: "今天天气真的很好",
          todo: "打台球",
          time: "时间2020.3.25"
      }
  }
  let oOuter = document.getElementById("outer");
  let str = '<div class="box"><h2> ' + data.message.title + '</h2><p>' + data.message.todo + '</p><p>' + data.message.time + '</p></div> ';
  oOuter.innerHTML = str; 
```

- 模板字符串（template string）是增强版的字符串，用反引号（`）标识。可以嵌套变量，可以换行，可以包含单引号和双引号。
- 它可以当作普通字符串使用，也可以用来定义多行字符串。模板字符串中嵌入变量，需要将变量名写在`${}`之中。
- 大括号内部可以放入任意的 JavaScript 表达式，可以进行运算，以及引用对象属性。

```js
  let data = {
      message: {
          title: "今天天气真的很好",
          todo: "打台球",
          time: "时间2020.3.25",
          isLike: false
      }
  }
  let oOuter = document.getElementById("outer");
  let str = `
      <div class="box">
      <h2>${data.message.title}</h2>
      <p>${data.message.todo}</p>
      <p>${data.message.time}</p>
      <p>${data.message.isLike ? "我喜欢你" : "我不喜欢你"}</p >
      </div>
  `;
  // oOuter.appendChild(str);//Failed to execute 'appendChild' on 'Node': parameter 1 is not of type 'Node'.
  oOuter.innerHTML = str;//需要用innerHTML设置到元素中
```

- 模板字符串之中还能调用函数。

```js
  function fn(){
      return "hello";
  }
  let str = `${fn()} world`;
console.log(str);//hello world
```





# 数组扩展

## 1 扩展运算符

### 1.1什么是扩展运算符（...rest）

扩展运算符（spread）是三个点（`...`）。它好比 rest 参数的逆运算，将一个数组转为用逗号分隔的参数序列，目前也可以用来展开数组。

```js
//扩展运算符 是把数组在其他数组中展开
        {
            let arr = [1, 2, 3];
            let arr2 = [4, 5, ...arr];
            console.log(arr2); //4,5,1,2,3
        }

```

### 1.2 其他应用

- 复制数组
- 合并数组
- 解构赋值
- 字符串转换为数组

```js
//复制数组
let arr = [1, 2, 3, [4, 5]];
let newArr = [...arr];
console.log(arr, newArr);


//合并数组
{
    let arr1 = [1, 2, 3]
    let arr2 = [4, 5, 6]
    let arr3 = [7, 8, 9]
    let arr4 = [...arr1, ...arr2, ...arr3];
    console.log(arr4);
}

//3.解构复制
//生成一个数组 (此时扩展运算符必须在最后一个)
let [a, b, ...newarr] = [1, 2, 3, 4, 5, 5, 6, 8, 9];
console.log(newarr);


//可以展开字符串 把展开字符串放在数组中，可以直接转为数组
{
    let str = "abcd";
    let arr = [...str];
    console.log(arr); //["a","b","c","d"]
}
```



## 2 数组的新方法

### 2.1 Array对象的新方法(Array对象的方法 只能是Array来调用)

#### Array.from

**Array.from 把伪数组转换成数组(可以使用数组的方法)**

**伪数组，就是像数组一样有 `length` 属性，也有 `0、1、2、3` 等属性的对象，看起来就像数组一样，但不是数组，比如:**

**常见的伪数组有：**

- **函数内部的 `arguments`**
- **DOM 对象列表（比如通过 `document.getElementsByTags` 得到的列表）**
- **jQuery 对象（比如 `$("div")` ）**

**伪数组是一个 Object，而真实的数组是一个 Array。**

**伪数组存在的意义，是可以让普通的对象也能正常使用数组的很多方法**

```js
{
    const arg = {
        0: 1,
        1: 2,
        2: 3,
        3: 4,
        length: 4
    }
    Array.from(arg).forEach(function (item, index) {
        console.log(item);//1 2 3 4

    })
  //当伪数组直接被forin遍历的时候，可能会拿到我们不需要的属性
 	 for (let i in arg) {
        console.log(i); //可能会拿到length属性
    }

    for (let i in Array.from(arg)) {
        console.log(i); //0 1 2 3
    }
}

```

#### Array.of

将一组数字转换成数组 弥补Array的不足

```js
 //Array.of() 创建一个新数组，用来弥补new Array的不足
{
    let arr = new Array(4); //[,,,,]普通创建长度为4的数组
    let arr2 = Array.of(4); //[4] 方法创建一个只有一个项为4的数组
    // console.log([, , , , ].length)
    console.log(arr, arr2);
}
```



### 2.2 Array原型上的新方法

#### copyWithin

数组实例的`copyWithin()`方法，在当前数组内部，将指定位置的成员复制到其他位置（会覆盖原有成员），然后返回当前数组。也就是说，使用这个方法，**会修改当前数组**

它接受三个参数。

- target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
- start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示从末尾开始计算。
- end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示从末尾开始计算。

```js
  //copyWithin方法(替换的开始位置，复制的起始位置，复制的结束位置【不包含】)
{
    let arr = [1, 2, 3, 4, 5, 6, 7, 8, 9];
    arr.copyWithin(1, 5, 8);
    console.log(arr); //[1,6,7,8,5,6,7,8,9]
}
```



#### fill(使用固定值填充数组)

使用固定值填充数组

`fill`方法用于空数组的初始化非常方便。数组中已有的元素，会被全部抹去。

`fill`方法还可以接受第二个和第三个参数，用于指定填充的起始位置和结束位置。

```js
//fill()填充数组 改变原数组
//（填充的内容，填充起始位置，填充结束位置【不包含】）
{
    let arr = [1, 2, 3, 4, 5, 6];
    // arr.fill(0);
    arr.fill("a", 2, 4);
    console.log(arr);//1 2 a a 5 6
}
```



#### entries()，keys() 和 values()（`for...of`）

ES6 提供三个新的方法——`entries()`，`keys()`和`values()`——用于遍历数组。它们都返回一个遍历器对象,可以用`for...of`循环进行遍历，唯一的区别是`keys()`是对键名的遍历、`values()`是对键值的遍历，`entries()`是对键值对的遍历。

```js
 //entries keys values 是分别用来获取数组的 键值对的迭代器 键名的迭代器  键值得迭代器
 //方便我们使用for of 对数组的 键值对 键名 键值 进行遍历操作
{
    const arr = ['a', 'b', 'c', 'd'];
    for (let i of arr.entries()) {
        console.log(i); //[0,"a"] [1,"b"] [2,"c"] [3,"d"] 键值对
    }
}

{
    const arr = ['a', 'b', 'c', 'd'];
    console.log(arr.keys())  ////Array Iterator遍历器对象
    for (let i of arr.keys()) {   //keys键，也就是下标
        console.log(i); //0 1 2 3     i就是每次遍历器调用next方法得到的值
    }
}

{
    const arr = ['a', 'b', 'c', 'd'];
    console.log(arr.values())
    for (let i of arr.values()) {
        console.log(i); // a b c d
    }
}
```



#### find() 和 findIndex()

数组实例的`find`方法，用于找出第一个符合条件的数组成员。它的参数是一个回调函数，所有数组成员依次执行该回调函数，直到找出第一个返回值为`true`的成员，然后返回该成员。如果没有符合条件的成员，则返回`undefined`

```js

//find会遍历数组，当某一个值在回调函数中第一次返回true，则find就返回这个值
{
    let arr = [1, 3, 6, 5, 4, 2, 7];
    //找出第一个偶数
    let re = arr.find(function (item, index) {
        return item % 2 === 0;
    })
    console.log(re);//6
}
//findIndex方法
//找到第一个符合条件的值得下标
{
    let arr = [1, 3, 6, 5, 4, 2, 7];
    //找出第一个偶数
    let re = arr.findIndex(function (item, index) {
        return item % 2 === 0;
    })
    console.log(re);//6的下标为2，返回2
}
```

#### includes()

`includes`方法返回一个布尔值，表示某个数组是否包含给定的值，与字符串的`includes`方法类似

```js
//includes检查数组中有没有存在某一个值
{
    let arr = [1, 2, 3, 4, 5, [], 7];
    console.log(arr.includes(3)); //true
    console.log(arr.includes([])); //false 数组是引用类型地址不同不是同一个
}
```

#### flat()

- 数组的成员有时还是数组，`Array.prototype.flat()`用于将嵌套的数组“拉平”，变成一维的数组。该方法返回一个新数组，对原数据没有影响。
- `flat()`默认只会“拉平”一层，如果想要“拉平”多层的嵌套数组，可以将`flat()`方法的参数写成一个整数，表示想要拉平的层数，默认为1
- 如果不管有多少层嵌套，都要转成一维数组，可以用`Infinity`关键字作为参数

```js
{
    //二维数组
    let arr = [1, 2, [3, 4]];
    console.log(arr.flat())
}
{
    //多维数组 想拉平 可以传入参数
    let arr = [1, 2, [3, [5, 6, 7]]];
    console.log(arr.flat(2))
}
{
    //如果不知道自己的数组有多少层  那么可以传递infinity 代表无穷大
    let arr = [1, 2, [3, [5, 6, 7]]];
    console.log(arr.flat(Infinity))
}
```







# 函数的扩展

## 1 函数参数默认值

ES6 之前，不能直接为函数的参数指定默认值，只能采用变通的方法。

```js
function fn1(age, sex) {
    // sex ? sex : sex = "男"; 
    console.log(age, sex || "男")
}
fn1(11)
```

### 1.2 ES6 默认参数

ES6 允许为函数的参数设置默认值，即直接写在参数定义的后面。

```js
function fn2(a, b = "world") {
    console.log(a, b);
}
fn2("hello", "");// hello 空的也算
fn2("hello");//hello world
```



## 2 rest参数(...rest)

- ES6 引入 rest 参数（形式为`...变量名`），用于获取函数的多余参数，这样就不需要使用`arguments`对象了。rest 参数搭配的变量是一个数组，该变量将多余的参数放入数组中。
- 和普通参数混合使用的时候，需要放在参数的最后
- 函数的`length`属性，不包括 rest 参数

```js
function fn4(...rest) {
    // rest参数本身就是一个数组
    let re = rest.reduce(function (p, n) {
        return p + n;
    })
    console.log(re);
}
fn4(1, 2, 3, 4, 5)
```



## 3 箭头函数

### 3.1 什么是箭头函数

ES6 允许使用“箭头”（`=>`）定义函数

### 3.2 箭头函数的写法

箭头函数分为以下几种情况

- 只有一个参数 并函数体是一句话的时候（一个参数可以省略`()`）
- 没有参数或者多个参数的时候，参数的括号不能省略
- 当函数体不是一句话的时候

```js
 //箭头函数 当函数有参数，并且函数体只有一句返回值的时候，可以省略return
        /* function fn1(n) {
            return n + 1;
        } */
        const fn1 = n => n + 1;


//当函数没有参数或多个参数 函数体只有一句返回值的时候
        /* function fn2() {
            return "hello world" + Date.now();
        } */
        const fn2 = () => "hello world" + Date.now();
        console.log(fn2());

 
//当函数参数随意，函数体不仅仅是一句返回值的时候
        /* function fn3() {
            let str = "hello world"
            return str + Date.now();
        } */
        const fn3 = () => {
            let str = "hello world"
            return str + Date.now();
        }
        console.log(fn3());



//练习 把以下函数改成箭头函数
        function fn4(...rest) {
            // rest参数本身就是一个数组
            let re = rest.reduce(function (p, n) {
                return p + n;
            })
            console.log(re);
        }
        fn4(1, 2, 3, 4, 5) 

        const fn4 = (...rest) => {
            let re = rest.reduce((p, n) => p + n);
            console.log(re);
        }
        fn4(1, 2, 3, 4, 5)


        setTimeout(() => {
            console.log(1);
        }, 1000)
```

```js
//加括号的函数体返回对象字面量表达式：
params => ({foo: bar})

//同样支持参数列表解构
let f = ([a, b] = [1, 2], {x: c} = {x: a + b}) => a + b + c;
f();  // 6
```



### 3.3 箭头函数的注意事项（this）

- **关于this 箭头函数没有自己的this，箭头函数内部的this并不是调用时候指向的对象,而是定义时指向的对象**

- **箭头函数的this指向在定义函数的时候就已经确定了**

- **箭头函数没有自己的this 要看定义时所在函数（环境）的this指向**

  ```js
  document.onclick = function () {
         console.log(this)//普通函数的this指向调用对象document
  }
  
  document.onclick = () => {
       //定义这个箭头函数的时候，是在window环境下定义的，所以指向window
       console.log(this);
  }
  
  document.onclick = function () {
        console.log(this); //document
        let f = () => { //箭头函数无自己的this，所以看所在函数事件函数内的this指向
            console.log(this); //document
        }
        f();
  }
  
  document.onclick = () => {
        console.log(this);  //window
        let f = () => {
            console.log(this); //window
        }
        f();
  }
  ```

**this指向练习：**

```js
function fn11() {
    console.log(this); //document 普通函数的this指向了调用者
    const f11 = () => {
        console.log(this); //document 箭头函数没有自己的this 要看定义时所在函数（环境）的this指向
    }
    f11()
}
document.onclick = fn11;

//
const obj2 = {
    name,
    age,
    sex,
    //不推荐对象中方法使用箭头函数简写，因为可能会this指向出问题
    do: () => {
        // do这个箭头函数在window中， 所以do的this指向window （do本身是个箭头函数）
        console.log(this);
        const f3 = () => {
            //f3的this要看当前箭头函数所在的环境，所在环境（函数）this指向的是window
            console.log(this); //window
        }
    },
}
obj2.do();

//
const obj = {
    name: "lily",
    //这种简写就是语法糖，是function的简写
    do() {
        console.log(this) //obj
        // do是普通函数，this指向调用者-- > obj
        const fn1 = () => {
            console.log(this) //看当前箭头函数所在的函数this指向 obj
        }
        fn1();
    }
}
obj.do();


//
document.onclick = () => {
    console.log(this) //window
    const obj = {
        name: "xiaoming",
        do() {
            console.log(this); //obj
            const fn1 = () => {
                console.log(this) //obj
            }
            fn1();
        }
    }
    obj.do();
}
```



- **箭头函数不能用于构造函数，也就是不能使用new关键字调用**

  ```js
  //箭头函数不能用于构造函数 因为不能被当做一个构造器
  const Person = (age) => {
      this.age = age;
  }
  new Person(18);  //Person is not a constructor
  
  //由于this必须是对象实例，而箭头函数是没有实例的，此处的this指向别处，不能产生person实例，自相矛盾。
  ```

- **箭头函数没有arguments对象(也没有caller，callee)**

  ```js
  let f3 = () => {
        console.log(arguments);//arguments is not defined
  }
  f3(1, 2, 3, 4, 5)
  ```

- **箭头函数不能使用yield命令，意味着不能当作gennerator函数（后边会讲）**

- **箭头函数通过call和apply调用，不会改变this指向，只会传入参数**

  ```js
   let obj2 = {
              a: 10,
              b: function (n) {
                  let f = (n) => n + this.a;
                  return f(n);
              },
              c: function (n) {
                  function f(n) {
                      return n + this.a
                  }
                  let m = {
                      a: 20
                  };
                  return f.call(m, n);
              }
          };
          console.log(obj2.b(1));  // 11
          console.log(obj2.c(1)); // 11
  ```



# 对象的扩展

## 1 对象的简写

ES6 允许在大括号里面，直接写入变量和函数，作为对象的属性和方法。这样的书写更加简洁。

```js
// 1.属性的简写
let [name, age, sex] = ["xiaowang", 20, "女"];
let p1 = {
    name: name,
    age: age,
    sex: sex
}
console.log(p1);
//在es6中，对象的key和value如果相同，则可以简写
let p2 = {
    name,
    age,
    sex
}
console.log(p2);
//2.方法的简写
let p3 = {
    name,
    age,
    sex,
    do: function () {
        console.log("eat")
    }
}
p3.do();

let p4 = {
    name,
    age,
    sex,
    do() {
        console.log("eat")
    }
}
p4.do();
```

## 2 属性名表达式

JavaScript 定义对象的属性，有两种方法：点运算符和中括号运算符

但是，如果使用字面量方式定义对象（使用大括号），在 ES5 中只能使用标识符，而不能使用变量定义属性。

也就是说在ES5中 key/value key是固定不变的，在ES6中，支持属性表达式，支持key发生变化

```js
//在ES5中 key/value key是固定不变的
let p5 = {
    name: "laowang",
    sex: "女"
}

let friend = "grilFriend";
let obj1 = [];
let obj2 = [];
//对象的属性名在ES5之前只能是字符串，不可能当做变量解析的
//ES6中 提供了新的写法属性表达式的写法  就是把属性名用中括号括起来 中括号中就可以书写变量
const obj = {
    name: "xiaoli",
    ["sex"]: "nan",
    [friend]: "lily",
    [obj1]: "foo",
    [obj2]: "hoo"
}
console.log(obj);
console.log(obj.grilFriend)//"lily"
console.log(obj[friend])//"lily"

console.log(obj[obj1]) //hoo
console.log(obj[obj2]) //hoo  [obj1]  [obj2]解析为字符串一样，hoo覆盖了foo
```

## 3 对象的扩展运算符（...rest)

ES2018 将这个运算符引入了对象。

```js
 const obj = {
            a: 1,
            b: 2,
            c: 3,
            d: 4,
            e: 5
        }
        const {
            a,
            b,
            ...o
        } = obj;
        console.log(a, b, o) //1 2 { c: 3,d: 4,e: 5}
```

## 4 对象新增的方法

### 4.1 Object.is()

判断对象是否相等 相当于===,修复NaN不等自己的问题

```js
console.log(Object.is(1, 1))//true
console.log(Object.is({}, {}))//false
console.log(Object.is(NaN, NaN))//true
console.log(Object.is(Object, Object)); //true  Array也一样
```

### 4.2 合并方法Object.assign

```js
// Object.assign();合并对象
        const obj3 = {
            a: 1
        };
        const obj4 = {
            b: 2
        };
        const obj5 = {
            c: 3,
            a: 6 //如果有重复属性，则覆盖
        };
        //把Object.assign方法种的其他参数代表的对象 全部合并到第一个参数对象上，并且返回了对第一个参数对象的引用
        let re = Object.assign(obj3, obj4, obj5);
        console.log(re);  //{ a:6 , b:2 , c:3 }
        console.log(obj3); //{ a:6 , b:2 , c:3 }
        console.log(re === obj3);//true
```



# 新增数据类型

## 1 Symbol（基本类型）

### 1.1 什么是Symbol

ES5 的对象属性名都是字符串，这容易造成属性名的冲突。比如，你使用了一个他人提供的对象，但又想为这个对象添加新的方法，新方法的名字就有可能与现有方法产生冲突。如果有一种机制，保证每个属性的名字都是独一无二的就好了，这样就从根本上防止属性名的冲突。这就是 ES6 引入`Symbol`的原因。

ES6 引入了一种新的原始数据类型`Symbol`，表示独一无二的值。它是 JavaScript 语言的第七种数据类型

### 1.2 Symbol的使用

- Symbol 值通过`Symbol`函数生成。
- 这就是说，对象的属性名现在可以有两种类型，一种是原来就有的字符串，另一种就是新增的 Symbol 类型。凡是属性名属于 Symbol 类型，就都是独一无二的，可以保证不会与其他属性名产生冲突

```js
let s1 = Symbol(); //s1
let s2 = Symbol();
console.log(s1 === s2); //false 唯一值不相等
console.log(s1); //Symbol()
console.log(typeof s1); //'symbol' 字符串类型的symbol

```

//对象中有很多的属性，我们可能看不到对象所有的属性名

我们拿到数据之后需要给obj去添加一个属性,如果说你设置的属性和原有对象属性名重复，则会覆盖操作

为了解决这个问题，我们想要属性名是一个唯一的永远不会和其他相等的值 那就是symbol

```js
let s1 = Symbol(); 
let s2 = Symbol();
const obj = {
            title: "今天天气真好",
            content: "你和林有有去逛街",
            price: "3.4",
            [s1]: 1,
            [s2]: 2
        }
console.log(obj);//Symbol(): 1    Symbol(): 2
console.log(obj[s1]);  // 1 通过变量设置进去 则通过变量获取出来
console.log(obj[s2]);  //2
console.log(obj[Symbol()]); //这里的Symbol又是一个唯一的值，不可能获取到的



//给obj设置属性  属性名为了不重复设置为Symbol
obj[Symbol("num3")] = 3; //如果这样设置之后 你将永远取不到这个值
obj[Symbol("num3")] = 4; //如果这样设置之后 你将永远取不到这个值
console.log(obj[Symbol("num3")]) //undefined
```



### 1.4 Symbol的注意事项

- Symbol中传入的字符串没有任何意义，只是用来描述Symbol的

  ```js
  //symbol方法的参数，仅仅是为了方便识别
  {
      let obj1 = {
          name: "lily",//原有对象拥有name属性
          age: 18,
      }
      let name = Symbol("name");
      let age = Symbol("age");
      obj1[name] = "xiaowang";
      obj1[age] = 21;
      console.log(obj1)
  }
  ```

- Symbol不能使用New调用

```js
  new Symbol();//Symbol is not a constructor
```

- 类型转换的时候，不能转数字

```js
// console.log(Number(Symbol("a")));//不能转数字
  console.log(String(Symbol("a"))); //'Symbol(a)'
  console.log(Boolean(Symbol("a"))); //true

```

- 如果把Symbol当作一个对象的属性和方法的时候，一定要用一个变量来储存，否则定义的属性和方法没有办法使用

  ```js
  {
      let obj = {
          name: "lily",//原有对象拥有name属性
          age: 18,
      }
      obj[Symbol()] = "再见";//如果直接设置，则再也获取不到这个属性了
    //因该是  let sy=Symbol()   obj[sy] = "再见"
      console.log(obj);
  
      console.log(obj[Symbol()])//undefined
  }
  ```

- for in 不能遍历出来，可以使用Object.getOwnPropertySymbols方法来拿

  ```js
  let s1 = Symbol();
  let s2 = Symbol();
  const obj = {
      title: "今天天气真好",
      content: "你和林有有去逛街",
      price: "3.4",
      [s1]: 1,
      [s2]: 2
  }
  
  for (let i in obj) {
      console.log(i) //title  content price
  }
  
  //通过getOwnPropertySymbols方法可以拿到所有Symbol属性。返回的数组进行遍历可以得到所有的Symbol属性的值
  const re = Object.getOwnPropertySymbols(obj); //[Symbol(), Symbol()]
  re.forEach((item, index) => {
      console.log(item)
      console.log(obj[item])
  })
  ```





## 2 BigInt(基本类型值)

- JavaScript 所有数字都保存成 64 位浮点数，这给数值的表示带来了两大限制。一是数值的精度只能到 53 个二进制位（相当于 16 个十进制位），大于这个范围的整数，JavaScript 是无法精确表示的，这使得 JavaScript 不适合进行科学和金融方面的精确计算。二是大于或等于2的1024次方的数值，JavaScript 无法表示，会返回`Infinity`。
- 引入了一种新的数据类型 BigInt（大整数），来解决这个问题。BigInt 只用来表示整数，没有位数的限制，任何位数的整数都可以精确表示。

```JS
//当数值大于2的53次方的时候，计算就可能不精确
  let num1 = 2 ** 53;
  let num2 = 2 ** 53 + 1;
  console.log(num1, num2, num1 == num2);

  //当超过了2的1024次方的时候 就使用Infinity来表示
  console.log(2 ** 1024);

  //当遇到大数量的运算的时候，把书写书写为BigInt类型（就是在数字后添加一个字母n即可）
  let num3 = 87998162919183758109175091137409171n;
  let num4 = 92n;
  console.log(num3 ** num4) //返回的值也是BigInt类型

  //number和bigint在数值相等的时候 是相等判断为true 但是全等判断是false
  let num5 = 2n;
  let num6 = 2;
  console.log(typeof num5); //bigint
  console.log(num5 == num6); //true
  console.log(num5 === num6); //false
```







# 新增数据结构

## 1 Set

### 1.1 什么是Set

- ES6 提供了新的数据结构 Set。它**类似于数组**，但是成员的值都是唯一的，没有重复的值。
- `Set`本身是一个构造函数，用来生成 Set 数据结构。
- `Set`函数可以接受一个数组（或者具有 iterable 接口的其他数据结构）作为参数，用来初始化
- set构造函数中可以传入的是 拥有iterator迭代器属性的值（数组、伪数组、字符串）
-  set数据结构本身也是拥有iterator迭代器属性的 所以可以被forof遍历 被三点运算符展开

```js
const set1 = new Set([1, 2, 3, 4, 2, 1, 4, 6, 3]);
console.log(set1); //[1,2,3,4,6] set
```





### 1.2 Set的其他使用(数组字符串去重)

```js
const arr = [1, 2, 3, 4, 2, 3, 4, 3, 5, 6, 7];
const newArr = [...new Set(arr)];
console.log(newArr);

//去除字符串里面的重复字符
const set2 = [...new Set('ababbc')].join('')
console.log(set2); //abc
console.log(typeof set2);//string
```



### 1.3 Set的属性及方法

- size 返回Set的长度
- add 添加某个值，返回 Set 结构本身。
- delete 删除某个值，返回一个布尔值，表示删除是否成功。
- has 返回一个布尔值，表示该值是否为`Set`的成员
- clear 清除所有成员，没有返回值。
- `keys()`：返回键名的遍历器,由于 Set 结构没有键名，只有键值（或者说键名和键值是同一个值），所以`keys`方法和`values`方法的行为完全一致。
- `values()`：返回键值的遍历器
- `entries()`：返回键值对的遍历器
- `forEach()`：使用回调函数遍历每个成员

```js

const set4 = new Set([1, 2, 3, 4, 5]);
console.log(set4.size); //5

console.log(set4.add(6)); //添加修改原set4 并且返回引用

console.log(set4.delete(1)); //打印true 原set结构的值被修改
console.log(set4); //[2,3,4,5]

console.log(set4.has(1)); //false 被上面删除了

console.log(set4.keys()); //set结构的keys和values打印出来是一样的（打印数组一样的形式）
console.log(set4.values());
console.log(set4.entries());


set4.forEach((item, index) => {
    console.log(item, index) //key和value是一样的 1 2 3 4 5
})
```





## 2 Map

### 2.1 什么是Map

- JavaScript 的对象（Object），本质上是键值对的集合（Hash 结构），但是传统上只能用字符串当作键。这给它的使用带来了很大的限制。
- 为了解决这个问题，ES6 提供了 Map 数据结构。**它类似于对象，也是键值对的集合**，但是“键”的范围不限于字符串，各种类型的值（包括对象）都可以当作键。也就是说，Object 结构提供了“字符串—值”的对应，Map 结构提供了“值—值”的对应，是一种更完善的 Hash 结构实现。如果你需要“键值对”的数据结构，Map 比 Object 更合适。

```js
let friend = 'boyFriend';
let score = [];
//在对象中，对象的属性虽然可以使用 属性表达式来书写变量，但是最终解析的也是字符串
const obj = {
    name: "xiaowang",
    [friend]: "xiaoli",
    [score]: 100 //最终解析的是[Object object]：100
}


  //为了解决对象中 key必须是字符串的问题，创建了一个新的数据结构 Map
        const o1 = {};
        const map1 = new Map([
            ["name", "lily"],
            [{}, 1],//这样设置的话的 以后将取不到这个值
            [[], 2],
            [o1,3]
        ])
        map1.set("age",19);//设置属性 需要通过set方法
        console.log(map1)
        console.log(map1.get(o1))//获取值 要通过get方法来获取


 //forEach
map1.forEach((item,index) => {
    console.log(item,index);
})
```

### 2.2 Map的属性和方法

- `size`属性返回 Map 结构的成员总数。
- `set`方法设置键名`key`对应的键值为`value`，然后返回整个 Map 结构。如果`key`已经有值，则键值会被更新，否则就新生成该键。`set`方法返回的是当前的`Map`对象，因此可以采用链式写法。
- `get`方法读取`key`对应的键值，如果找不到`key`，返回`undefined`。
- `has`方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。
- `delete`方法删除某个键，返回`true`。如果删除失败，返回`false`。
- `clear`方法清除所有成员，没有返回值。
- `keys()`：返回键名的遍历器。
- `values()`：返回键值的遍历器。
- `entries()`：返回所有成员的遍历器。
- `forEach()`：遍历 Map 的所有成员。







# Class

### ES6的类和继承（语法糖）

```js
class Animal {
    //类中都有一个constructor的方法，方法中书写的是实例化对象的私有属性
    //当这个类被实例化的时候，会自动调用该方法
    constructor(age, sex) {
        this.age = age;
        this.sex = sex;
    }
    //直接在类中书写的方法 是原型对象上的方法
    say() {
        console.log("jiao")
    }
    //直接在外边定义的属性是实例对象私有的（基本不这样书写）
    index = 1;

    //定义静态的属性或方法  只属于构造函数本身的
    static con = "hello world";
}

//也可以给类的原型对象扩展，一般不这么写了，因为在里面写也是在原型对象上
        Animal.prototype.eat = function () {
            console.log("eat")
        };
        
let a1 = new Animal(18, "nv");
let a2 = new Animal(18, "nv");
console.log(a1);
console.log(Animal.con);
console.log(a1.say === a2.say);

```





- **定义一个子类继承父类**
- **定义一个Person类 继承Animal类**
  - ES6中继承的子类中，如果使用构造函数constructor()那么就必须使用super()方法初始化，这样下面才可以调用this关键字。super()只能用在子类的构造函数之中，用在其他地方就会报错,这是因为子类自己的this对象，必须先通过父类的构造函数完成塑造，得到与父类同样的实例属性和方法，然后再对其进行加工，加上子类自己的实例属性和方法。
  - 如果不调用super方法，子类就得不到this对象。

```js
class Person extends Animal {
    constructor(name, age, sex) {
        //super是拿到了父类的this，然后我们在对这个this进行加工
        super(age, sex); //这里如果不写的话，对应的属性还有，值为undefined
        this.name = name;
    }
    do() {
        console.log("敲代码")
    }
}
const p1 = new Person("laoli", "21", "nan");
console.log(p1)
p1.say();
p1.eat();
p1.do();
```



# iterator

## 1 什么是iterator

- JavaScript 原有的表示“集合”的数据结构，主要是数组（`Array`）和对象（`Object`），ES6 又添加了`Map`和`Set`。这样就有了四种数据集合，用户还可以组合使用它们，定义自己的数据结构，比如数组的成员是`Map`，`Map`的成员是对象。这样就需要一种统一的接口机制，来处理所有不同的数据结构。
- **遍历器也叫迭代器（Iterator）就是这样一种机制**。**它是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署 Iterator 接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）。**
- Iterator 的作用有三个：一是为各种数据结构，提供一个统一的、简便的访问接口；二是使得数据结构的成员能够按某种次序排列；三是 ES6 创造了一种新的遍历命令`for...of`循环，Iterator 接口主要供`for...of`消费。

```js
//手写iterator接口
const arr = [1, 3, 5, 7, 9]
function Iterator() {
    let num = 0;
    return {
        next() {
            if (num < arr.length) {
                return {
                    done: false, //是否遍历完
                  //第一次执行的之后，先得到arr[0] 然后又加1
                    value: arr[num++]
                }
            } else {
                return {
                    done: true,
                    value: undefined
                }
            }
        }
    }
}
let it = Iterator(arr) //调用Iterator返回一个（迭代器）对象，对象中有next方法
console.log(it);
console.log(it.next());
console.log(it.next());
console.log(it.next());
console.log(it.next());
console.log(it.next());
console.log(it.next());

```

**所谓迭代器，其实就是一个具有 next() 方法的对象，每次调用 next() 都会返回一个结果对象，该结果对象有两个属性，value 表示当前的值，done 表示遍历是否结束。**



## 2 iterator的使用

- 一种数据结构只要部署了Iterator接口，我们就称这种数据结构是可以迭代的

- 在ES6中，只要一种数据结构具有了Symbol.iterator属性，那么就认为是可以迭代的

- **Symbol.iterator属性是一个方法（手写接口函数），调用这个方法可以得到一个迭代器对象，所以可以自己给对象部署**

  ```js
   const arr = [1, 2, 3, 4, 5, 6];
   const it = arr[Symbol.iterator](); //调用这个属性（方法），返回一个拥有next()方法的迭代器对象
   console.log(it.next());
  ```

  

- **在ES6中，很多数据结构都部署了iterator接口(Array,set,Map,string)  ，但是对象和数值是没有接口的，但是可以部署**

- 应用场景：

  - 解构赋值的时候默认调用iterator接口
  - 扩展运算符使用的时候页默认调用iterator接口
  - for of 使用的是iterator接口
  - 对象是没有部署Iterator接口

```js
const arr = [1, 2, 3, 4, 5, 6]; //数组
console.log(arr)

const str = "1244321";//字符串

const set1 = new Set([1, 2, 3, 2, 1])//set

const map1 = new Map([  //map
    [true, "1"],
    [123, "2"]
])

const num = 123; //num，没有iterator
const obj = { //obj没有iterator
    name: "lily",
    sex: "nv"
}

const it = arr[Symbol.iterator]();
console.log(it);
console.log(it.next());
console.log(it.next());
console.log(it.next());
console.log(it.next());
console.log(it.next());
console.log(it.next());
console.log(it.next());
```



## 3 for...of

- ES6 借鉴 C++、Java、C# 和 Python 语言，引入了`for...of`循环，作为遍历所有数据结构的统一的方法。
- 一个数据结构只要部署了`Symbol.iterator`属性，就被视为具有 iterator 接口，就可以用`for...of`循环遍历它的成员。也就是说，`for...of`循环内部调用的是数据结构的`Symbol.iterator`方法。
- `for...of`循环可以使用的范围包括数组、Set 和 Map 结构、某些类似数组的对象（比如`arguments`对象、DOM NodeList 对象）、后文的 Generator 对象，以及字符串。

```js
//for of 格式
const arr = [1, 2, 3, 4, 5, 6];
for (let item of arr) {
    console.log(item);
}

```

**for...of循环得到的值是 values ,for...in循环(得到的值是键名key)**



# Generator

## 1 什么是Generator

- Generator 函数是 ES6 提供的一种异步编程解决方案，内部封装了很多的状态，被称作状态机 生成器
- **执行Generator会返回一个迭代器对象（iterator），使用iterator来遍历出Generator内部的状态**
- 形式上，Generator 函数是一个普通函数，但是有两个特征。一是，`function`关键字与函数名之间有一个星号；二是，函数体内部使用`yield`表达式，定义不同的内部状态（`yield`在英语里的意思就是“产出”）

```js
//定义一个Generator函数
function* gen() {
    console.log("hh");
    yield;
    console.log("hh");
    yield "2";  //
    console.log("hh");
    yield "3";
    console.log("hh");
    yield "4";
    console.log("hh");
}
let it = gen(); // 执行gen会返回一个迭代器对象（iterator）  函数不会立马执行 而是在调用了next方法的时候才执行
console.log(it);
console.log(it.next()); //{value: undefined, done: false}  执行遇到yield执行完yield后停止，要再次调用next
console.log(it.next()); //{value: "2", done: false}
```



## 2 Generator的注意事项

- **generator代码内部不会马上执行，需要调用next方法的时候才会执行**

- **yield语句返回结果通常为undefined**

- **调用了generator后返回的是迭代器对象，在generator函数中，遇到yield执行完yield后就会停止，直到运行next才会执行下一个**

- **可以使用for of执行gen**

  ```js
  function* gen() {
      console.log("hh");
      yield;
      console.log("hh");
      yield "2";   //这个yield后面跟的是yield语句返回结果 ，也就是value的值
  }
  let it = gen();
  console.log(it.next()); //{value: undefined, done: false}  执行遇到yield停止，要再次调用next
  console.log(it.next());  //{value: "2", done: false}
  
  
  for (let item of gen()) {
      console.log(item); //item就是yield的返回值
  }
  ```





## 3 Generator的练习

```js
//请求a数据，再请求b数据，请求c数据（使用计时器模拟请求）
function* gen() {
    console.log("开始请求");
    yield setTimeout(() => {
        console.log("a数据请求成功");
        it.next(); //定时器异步代码，所以可以获取到it
    }, 1000);

    yield setTimeout(() => {
        console.log("b数据请求成功");
        it.next();
    }, 1000);


    yield setTimeout(() => {
        console.log("c数据请求成功")
    }, 1000);
}
const it = gen();
it.next();
```

## 4 async和await

async 用于申明一个 function 是异步的，而 await 用于等待一个异步方法执行完成

### async

- async函数(源自ES2017 - ES8),真正意义上去解决异步回调的问题，同步流程表达异步操作,是 Generator的语法糖

- 语法：

  async function foo(){

   await 异步操作;

   await 异步操作；

  }

- **async 函数会返回一个 Promise 对象，默认返回的promise对象是resolve状态，当函数没有返回值的时候，resolve方法的参数是undefined，否则resolve的参数是当前函数的返回值（return）适用于then/canth返回的新promise对象**

- **如果asyn函数内部报错（抛出异常），则返回失败的promise**

- **async 函数return的是promise对象的话，那么内部return的promise对象的状态即是你async函数返回的promise对象的状态，**

```js
async function fn() {
    console.log("fn函数执行了");
    return 333;  //resolve的参数
}
 const p = fn();
 console.log(p);  //[[PromiseStatus]]: "fulfilled"
								 //[[PromiseValue]]: undefined
```

### await

**async取代Generator函数的星号*，await取代Generator的yield**

**不需要像Generator去调用next方法，遇到await等待，当前的异步操作完成就自动往下执行**

**`await` 是个运算符，用于组成表达式，await 表达式的运算结果取决于它等的东西**

- 如果它等到的不是一个 Promise 对象，那 await 表达式的运算结果就是它等到的东西
- 如果它等到的是一个 Promise 对象，await 就忙起来了，它会阻塞后面的代码，等着 Promise 对象 resolve，然后得到 resolve 的参数值，作为 await 表达式的运算结果。（可以传递数据），没有参数undefined
- 如果promise执行结果为reject，则await无返回值,退出当前的async函数（可以用try...catch来捕获异常）





- **await会阻塞函数的运行，需要等后边的异步代码执行完毕才会向下执行**
  - **await后必须跟promise对象，才能进行阻塞函数代码的运行，才会等待，否则是不会等待**
- **await等待是promise对象，则根据状态继续执行**
  - **1.pending状态，那么此时就一直在阻塞 async的返回值也一直是pending （不写resolved或者rejected。 抛出错误也算pending状态）**
  - **2.resolved状态，那么此时继续向下执行函数的代码，直到函数执行完毕，如果有任意await返回失败 则async函数就返回失败状态**（async函数返回的 Promise 对象是成功状态，没有return返回值的时候，resolve方法的参数是undefined，有就是return）
  - **3.rejected状态，那么此时直接退出async函数，并且async函数返回rejected状态**（直接退出函数，async函数返回的Promise 对象是rejected状态，reject方法的参数就是失败时reject的参数）

```js
async function gen() {
    await new Promise((resolve, reject) => {
        console.log("正在加载a。。。。");
        setTimeout(() => {
            console.log("a加载成功");
            resolve("成功喽")
            // reject("失败楼")
        }, 1000)
    })
    console.log(111);
    await new Promise((resolve, reject) => {
        console.log("正在加载b。。。。");
        setTimeout(() => {
            console.log("b加载成功");
            // resolve("成功喽")
            reject("失败楼")
        }, 1000)
    })
}
const p = gen();
console.log(p);
p.then((success) => {
    console.log("成功了")
})
```



```js
// 请求a数据，请求成功在请求b数据，请求成功在请求c数据
async function gen() {
    const data1 = await new Promise((resolve, reject) => {
        console.log("正在请求a数据。。。。")
        setTimeout(() => {
            console.log("a请求成功");
            const data = {
                name: "xiaoli"
            }
            resolve(data);//状态是成功，await表达式的结果为resolve方法的参数data 即data1 = data
        }, 1000)
    })

    const data2 = await new Promise((resolve, reject) => {
        console.log("正在请求b数据。。。。")
        setTimeout(() => {
            console.log("b请求成功");
            const data = {
                age: 18
            }
            Object.assign(data, data1)
            resolve(data);
        }, 2000)
    })
    const data3 = await new Promise((resolve, reject) => {
        console.log("正在请求c数据。。。。")
        setTimeout(() => {
            console.log("c请求成功");
            const data = {
                sex: "nv"
            }
            Object.assign(data, data2)
            resolve(data);
        }, 1000)
    })
    console.log(data3)
}
const promise = gen();
```









# Promise基础

## Promise功能

- 回调函数嵌套回调函数被称作**回调地狱**，代码层层嵌套，环环相扣，很明显，逻辑稍微复杂一些，这样的程序就会变得难以维护。
- 对于这种情况，程序员们想了很多解决方案（比如将代码模块化），但流程控制上，还是大量嵌套。
- ES2015的标准里，Promise的标准化，一定程度上解决了JavaScript的流程操作问题。Promise对象时一个异步编程的解决方案，**可以将异步操作以同步的流程表达出来, 避免了层层嵌套的回调函数(俗称'回调地狱')**

```js
setTimeout(() => {
    console.log("a数据请求回来了~");
    setTimeout(() => {
        console.log("b数据请求回来了~");
        setTimeout(() => {
            console.log("c数据请求回来了~");
        }, 3000);
    }, 2000);
}, 1000);
```

## Promise入门

- `Promise`是一个构造函数，自己身上有`all`、`reject`、`resolve`这几个眼熟的方法，原型上有`then`、`catch `、`finally`等同样很眼熟的方法
- `Promise`的构造函数接收一个参数，是函数，并且传入两个参数：**`resolve`，`reject`，分别表示异步操作执行成功后的回调函数和异步操作执行失败后的回调函数。(成功在前，失败在后顺序不能乱)**
- `Promise`对象有三种状态：代表异步执行的状态,对象的状态只能改变一次
  - **pending 初始化状态（异步代码还在执行中）没有resolve   reject的话默认pending，异步里抛错也是pending**
  - **resolved / fulfilled 成功状态（异步代码执行成功了），调用resolve函数，可以将promise对象的状态由pending变成resolved**
  - **rejected 失败状态（异步代码执行失败了），调用reject函数，可以将promise对象的状态由pending变成rejected**

```js
/* 
Promise返回值有三种状态
    pending: promisestatus:pending
             promisevalue:undefined

    resolved: promisestatus:resolved
              promisevalue:    resolve方法的参数

    rejected: promisestatus:rejected
              promisevalue:     reject方法的参数

当reject和resolve同时调用的时候，以书写顺序为基准 只能执行1个

promise是异步编程的解决方案，用来解决回调地狱问题的，后续可以通过promise的返回值来确定下一步的操作

*/
const p = new Promise((resolve, reject) => {
setTimeout(() => {
    console.log("a数据请求成功")
    //当成功以后 直接调用resolve方法
    reject("error")
    // resolve("ok~");
}, 1000)
})
console.log(p);
```



**Promise本身不是异步的是同步的，而是对异步编程的解决方案**

`new Promise()`函数是同步执行的

```js
//请说出以下代码的执行顺序
// 111---222--555--666--333--444
console.log(111)
const p = new Promise((resolve, reject) => {
    console.log(222)
    setTimeout(() => {
        console.log(333)
        reject("error")
        console.log(444)
    }, 1000)
    console.log(5555)
})
console.log(666)
```

## Promise的then和catch及finally方法     (原型上的方法)

**promise返回值的状态由reject和resolve来决定，如果都没有 则是pending状态，pending状态都不执行**

**then和catch方法都是异步状态，因为后面要处理操作等返回值**

- `Promise.prototype.then` 捕获promise成功的状态，执行成功的回调

- `Promise.prototype.catch` 捕获promise失败的状态，执行失败的回调

- **回调函数的参数就是resolve或reject的参数（通常用来传递数据）**

  ```js
  const p = new Promise((resolve, reject) => {
      setTimeout(() => {
          console.log("数据请求成功")
          // reject("error!!!!")
          // resolve("success!!!")
      }, 1000)
  })
  console.log(p);
  p.then(
      (success) => {  // //函数的参数就是resolve或reject的参数
          console.log(success + "鼓掌")
      }
  )
  p.catch(
      (error) => {
          console.log(error + "一巴掌");
      }
  )
  
  //then / catch 方法返回值是一个新的promise对象，所以可以链式调用
  
  new Promise((resolve, reject) => {
      setTimeout(() => {
          console.log("数据请求成功")
          // reject("error!!!!")
          resolve("success!!!")
      }, 1000)
  }).then(
      (success) => {
          //书写请求成功后继续重新请求的内容
          console.log(success + "鼓掌")
      }
  ).catch(
      (error) => {
          //书写请求有问题的解决方案
          console.log(error + "一巴掌");
      }
  )
  ```

## then / catch 方法特点

**then / catch 方法返回值是一个新的promise对象**

- **新promise对象默认是成功状态**

  - **then和catch 默认都是resolve状态(没有return值 和 return的不是一个promise对象的时候)**

- **当then方法或catch方法中的函数 返回值是一个promise对象的时候，那么then和catch的返回值就是这个promise对象的状态 即（ then / catch 方法返回值的promise就是这个promise对象）**

- **如果没有返回 promise对象，就会新建一个默认成功状态promise**（没有返回值默认undefined，有返回值则传递下去的参数是return返回值）

- **内部如果报错了，返回一个失败状态的promise(当这个错误的状态被catch接受的时候，那么就不会抛出错误)**

  **返回值是一个promise对象的时候且在返回的promise对象里异步函数里抛错的话则返回的状态是pending（无法捕获到异步里的状态），then / catch里报错则返回失败的promise状态**
  
  **新建的promise里抛错则返回失败的promise**
  
  ​	

**案例**

```js
/* 
    需求：setTimeout模拟发送请求
    请求a数据，请求成功在请求b数据，请求成功在请求c数据
    把每次请求过来的数据合并起来
 */

new Promise((resolve, reject) => {
    console.log(".....正在请求a数据")
    setTimeout(() => {
        console.log("a数据请求成功");
        //假设data就是请求回来的数据
        const data = {
            name: "lily"
        }
        resolve(data)
    }, 1000)
}).then((success) => {
    return new Promise((resolve, reject) => {
        console.log("正在请求b数据。。。。。。");
        setTimeout(() => {
            console.log("b数据请求成功");
            //假设data就是请求回来的数据
            const data = {
                sex: "nv"
            }
            //合并a和b数据
            const result = Object.assign(success, data)
            resolve(result)
        }, 2000)
    })
}).then((success) => {
    return new Promise((resolve, reject) => {
        console.log("正在请求c数据。。。。。。");
        setTimeout(() => {
            console.log("c数据请求成功");
            //假设data就是请求回来的数据
            const data = {
                age: 18
            }
            //合并a和b数据
            const result = Object.assign(success, data)
            resolve(result)
        }, 2000)
    })
}).then((success) => {
    console.log(success, "成功")

}).catch((error) => {
    console.log(error, "没有成功")
})
```



**`Promise.prototype.finally` 只要promise对象的状态不是pending 那么都会进入finally执行，会和then / catch 方法一起执行，可以写多个**

```js
new Promise((resolve, reject) => {
    setTimeout(() => {
        console.log("数据请求成功")
        reject("error!!!!")
        // resolve("success!!!")
    }, 1000)
}).then(
    (success) => {
        //书写请求成功后继续重新请求的内容
        console.log(success + "鼓掌")
    }
).catch(
    (error) => {
        //书写请求有问题的解决方案
        console.log(error + "一巴掌");
    }
).finally(() => {
    console.log("不管你失败还是成功 我都是你的港湾1")
})
```





## Promise.all()方法   构造函数自身的方法

- **Promise.all([promise1, ...]) 传入n个promise对象，只有n个promise对象的状态都成功，才成功，只要有一个失败，就失败，也是返回一个promise，传递的值为数组，数组里的值为n个promise对象resolve的参数**
- **Promise.allSettled([promise1, ...])传入n个promise对象，等n个promise对象状态全部发生变化，得到所有结果值**

```js
const promise1 = new Promise((resolve, reject) => {
    //请求a数据
    console.log("正在请求a数据。。。。。");
    setTimeout(() => {
        console.log("a数据请求成功");
        resolve('a');
    }, 1000)
})
const promise2 = new Promise((resolve, reject) => {
    //请求a数据
    console.log("正在请求b数据。。。。。");
    setTimeout(() => {
        console.log("b数据请求成功");
        resolve('b');
    }, 3000)
})
const promise3 = new Promise((resolve, reject) => {
    //请求a数据
    console.log("正在请求c数据。。。。。");
    setTimeout(() => {
        console.log("c数据请求成功");
        // resolve('c');
        reject('c失败了');
    }, 2000)
})

//Promise.all如果全部请求成功 则返回成功的promise  否则则返回失败的promise
const p = Promise.all([promise1, promise2, promise3]);
p.then((success) => {
    //then的函数参数success是一个数组，保存的是三个promise对象resolve的参数
    console.log(success, "全部成功")
}).catch((error) => {
    //error就是某一个失败promise的reject的参数
    console.log(error, "有失败的promise")
})
```

- **Promise.resolve() 返回一个成功状态promise对象，如果参数还是promise的话（new Promise（）），那么返回的状态具体看参数promise的状态**

- **Promise.reject() 返回一个失败状态promise对象，参数无论是不是promise，或是成功promise又或是失败的promise，都将返回失败的promise**

-  **Promise.race方法: (promises) => {}**

  **promises: 包含n个promise的数组**

  **说明: 返回一个新的promise, 第一个完成的promise的结果状态就是最终的结果状态**





## try ...catch

 try...catch需要成对出现，否则报语法错误（SyntaxError)

如果是异步，try...catch是捕获不到的。所以可以将其写在异步函数内

```js
/ * try catch：自己处理异常
  * try {
  *可能出现异常的代码
  *} catch（异常类名A e）{
  *如果出现了异常类A类型的异常，那么执行该代码
  *} ...（catch可以有多个）
  * finally {
  *最终肯定必须要执行的代码（例如释放资源的代码）
  *}
```

 *代码执行的顺序：
 \* 1.try内的代码从出现异常的那一行开始，中断执行
 \* 2.执行对应的catch块内的代码
 \* 3.继续执行try catch结构之后的代码



## Error错误

```js
/*
* Erorr:错误
* 1.ReferenceError: 引用的变量不存在
2.TypeError: 数据类型不正确的错误
3.RangeError: 数据值不在其所允许的范围内
4.SyntaxError: 语法错误*/

// 1、ReferenceError 引用的变量不存在
// console.log(my);

// 2.TypeError  数据类型不正确的错误
// const obj = null;
// TypeError: Cannot read property 'userName' of null
// console.log(obj.userName)

// const a = 12;
// TypeError: a is not a function
// a();

// 3、RangeError: 数据值不在其所允许的范围内

// RangeError: Invalid array length
// const arr = new Array(-1);

// RangeError: Maximum call stack size exceeded
// function fn() {
//     fn();
// }
// fn();

// let a = 1;
// function fn() {
//     a++;
//     if(a<10000)
//         fn();
// }
// fn();

// 4、SyntaxError: 语法错误
// const a = """";// SyntaxError: Unexpected string
// const 6I = "";// SyntaxError: Invalid or unexpected token
```



### 抛出异常

 抛出异常。抛出的值可以是任意类型。在catch当中进行接收.

 抛出异常之后，后续代码不会执行。

 throw 1;

 throw {a:3,d:5};

 throw new Error("我抛出了一个异常");







