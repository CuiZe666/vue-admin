# 数据库

数据库（DataBase）是按照数据结构来组织、存储和管理数据的仓库。

### 为什么要使用数据库

我们的程序都是在内存中运行的，一旦程序运行结束或者计算机断电，程序运行中的数据都会丢失。所以我们就需要将一些程序运行的数据持久化到硬盘之中，以确保数据的安全性。而数据库就是数据持久化的最佳选择。说白了，数据库就是存储数据的仓库。



# 数据库的分类

## 关系型的数据库

1.能使用SQL语句
2.存储数据的位置:硬盘,每次操作都是对硬盘进行I/O操作(效率较低)
3.存储数据类型:基本数据类型

代表：MySQL、Oracle、DB2、SQL Server...

## 非关系型的数据库

1.不能使用SQL语句
2.存储数据的位置:内存+硬盘,每次操作数据都是对内存进行操作,每隔一段时间将数据存储到硬盘中
3.存储数据类型:复杂数据类型(对象数据类型+基本数据类型)

代表有：MongoDB、Redis...



# MongoDB

#### 安装

#### 启动MongoDB服务

1. 添加环境变量，将MongoDB的bin目录添加到path下

[![0gEIG4.md.png](https://s1.ax1x.com/2020/10/11/0gEIG4.md.png)](https://imgchr.com/i/0gEIG4) [![0gEoRJ.md.png](https://s1.ax1x.com/2020/10/11/0gEoRJ.md.png)](https://imgchr.com/i/0gEoRJ) [![0gEHMR.png](https://s1.ax1x.com/2020/10/11/0gEHMR.png)](https://imgchr.com/i/0gEHMR)

1. 打开一个命令行窗口输入mongo启动数据库的客户端



## MongoDB的使用

#### 3.1.1数据库（database）

数据库是一个仓库，在仓库中可以存放集合。

#### 3.1.2集合（collection）

集合类似于JS中的数组，在集合中可以存放文档。

说白了，集合就是一组文档。

#### 3.1.3文档（document）

文档数据库中的最小单位，我们存储和操作的内容都是文档。

类似于JS中的对象，在MongoDB中每一条数据都是一个文档。

![0gZl1H.png](https://s1.ax1x.com/2020/10/11/0gZl1H.png)

### 3.2使用

#### 3.2.1 基本命令

- 显示所有的数据库

  show dbs

  show databases

- 切换到指定的数据库

  use 数据库名（没有这个数据库也可以）

  如果想要创建数据库,需要对内部进行插入数据，当我们向集合或数据库中第一次插入文档时，集合和数据库会自动创建。

- 显示当前所在的数据库

  db

- 给当前数据库添加一个集合，并插入数据

  db.students.insert({name:"lily",age:18})

  students是集合名。可以自己取

- 查找

  db.students.find()

- 删除当前数据库

  db.dropDatabase()

- 显示当前数据库中的所有集合

  show collections

- 删除当前集合

  db.collection.drop()



### 3.3数据库的CRUD（增删改查）

**1)     向集合中插入文档（create）**

db.collection.insert()    向集合中插入一个或多个文档

**1)     查询集合中的文档（read）**

db.collection.find(查询条件)      查询集合中所有符合条件的文档，总是会返回一个数组个文档对象

db.collection.findOne(查询条件)  查找满足条件的第一个

1. 操作符： >, >=, <, <=, != “$gt", "$gte", "$lt", "$lte", "$ne"与前面一一对应

2. 操作符："$or", "$in"，"$nin" 

   "$or"：查询多个字段的情况,符合其中一个即可

   "$in"：查询一个字段的多种情况,符合其中一个即可

   "$nin"：一个属性有多种条件,内部条件都不满足,就可以

3. 正则表达式

4. "$where"

   可以自定义一个函数,返回值为true就代表满足条件,this存放着当前的文档对象



**1)     修改集合中的文档（update）**

db.collection.update(查询条件,新的文档,配置对象)  修改或替换一个或多个文档

**2)     删除集合中的文档（delete）**

db.collection.remove(查询条件)  删除所有（或第一个）符合条件的文档

```js
db.students.find()

db.students.insert([{name:"老王",age:38},{name:"老绿",age:48}])

db.students.find({})

//查询name值为小王的文档对象
db.students.find({name:"小王"})

//查询name值为小王,age值为21的文档对象
db.students.find({name:"小王",age:21})

//查询age为21和23的文档对象
db.students.find({age:{$in:[21,23]}})
//模拟in
db.students.find({$or:[{age:21},{age:23}]})

//查询age不为21和23的文档对象
db.students.find({age:{$nin:[21,23]}})

//查询name为小王或者age为21的文档对象
db.students.find({$or:[{name:"小王"},{age:21}]})

//查询name为小红同时age为21的文档对象
db.students.find({$and:[{name:"小红"},{age:21}]})
db.students.find({name:"小红",age:21})

//查询age小于38的文档对象
db.students.find({age:{$lt:38}})

//查询age->小于等于->38的文档对象
db.students.find({age:{$lte:38}})


//查询age-1之后小于37的人
db.students.find({$where:function(){return this.age-1<37}})

//查询name中有"小"字的文档对象,^以什么开头,$以什么结尾
db.students.insert({name:"大小",age:33})
db.students.find({name:/小$/})

//查询name为小明的文档对象,并返回他的name值,不返回age
db.students.find({name:"小明"},{name:1,_id:0})

db.students.find()

//更新需要先查找,再修改
//将name为小明的文档对象,他的age更新为24
db.students.updateOne({name:"小明"},{$set:{age:24}})

db.students.insert({name:"老王八",age:32})

//将name为老王的文档对象,他们的age更新为31岁
db.students.updateMany({name:"老王"},{$set:{age:31}})

//将所有人的岁数都增加1岁
db.students.updateMany({},{$inc:{age:1}})

//将所有人的name属性名,更新为studentName
db.students.updateMany({},{$rename:{"name":"studentName"}})

db.students.update({name:/王八/},{$set:{age:42}},{upsert:true,multi:true})

db.students.remove({name:/王八/})

//软删除
db.students.updateMany({studentName:"老王"},{$set:{isdelete:true}})

db.students.updateMany({},{$set:{isdelete:false}})

db.students.find({isdelete:false})

```

$set->设置字段值
$inc->对某个字段值进行增加(可以为正数或者负数)
$rename->对某个字段名进行重命名

## Mongoose的使用

Mongoose是一个对象文档模型（ODM）库，它对Node原生的MongoDB模块进行了进一步的优化封装，并提供了更多的功能。

### 4.2优势

- 可以为文档创建一个模式结构（Schema）
- 可以对模型中的对象/文档进行验证
- 数据可以通过类型转换转换为对象模型
- 可以使用中间件来应用业务逻辑挂钩
- 比Node原生的MongoDB驱动更容易

### 4.3核心对象

#### 4.3.1 Schema

模式对象，通过Schema可以对集合进行约束

#### 4.3.2 Model

模型对象，相当于数据库中的集合，通过该对象可以对集合进行操作

#### 4.3.3 Document

文档对象，它和数据库中的文档相对应，通过它可以读取文档的信息，也可以对文档进行各种操作

### 4.4使用

#### 4.4.1连接数据库

- 下载安装Mongoose
  - npm i mongoose --save
- 引入Mongoose
  - var mongoose = require("mongoose");
- 连接MongoDB数据库
  - mongoose.connect("mongodb://ip地址:端口号/数据库名"

```js
// 引入mongoose
const mongoose = require("mongoose");
/*
    第一步:连接数据库
    第一个参数->mongodb数据库的地址
    第二个参数->配置对象
        useNewUrlParser->由于当前的url字符串解析器在未来版本即将被废除,所以需要使用新的解析器
        useUnifiedTopology->由于当前的监听器在未来版本即将被废除,所以需要使用新的监听器
*/
mongoose.connect(
  "mongodb://localhost:27017/dest",
  {
    useNewUrlParser: true,
    useUnifiedTopology: true,
    useCreateIndex:true
  },
  function (err) {
    if (err) {
      console.log(err);
    } else {
      console.log("数据库连接成功");
    }
  }
);
// 监听数据库连接情况
// mongoose.connection.on("open",function(err){
//     if(err){
//         console.log(err)
//     }else{
//         console.log("数据库连接成功");
//     }
// })

/*
    第二步:创建约束对象(对当前集合的字段进行约束)
*/

const destSchema = new mongoose.Schema({
  name: {
    type: String, //数据类型必须是字符串
    required: true, //当前数据必传
    unique: true, //当前数据必须是唯一的,不可重复
  },
  age: Number,
  sex: {
    type: String,
    default: "未知",
  },
  roles: [String], //当前字段数据类型必须是数组,内部数据类型必须是字符串,
  info: mongoose.Schema.Types.Mixed, //当前字段的属性值可以为任意值
});
/*
    第三步:创建模型对象(集合实例对象)
    const starsModel = mongoose.model(集合名称,约束对象)
    官方声明集合名称可以是单数也可以是复数,但是->经过实测写单数会出问题
*/

const destModel = mongoose.model("dests", destSchema);
/*
    第四步:创建文档对象(文档对象的实例)
*/

const destDocument = new destModel({
  name: "折原临也",
  age: 23,
  sex: "男",
  roles: ["池袋", "无头", "群像"],
  info: "池袋最帅骗子",
});
/*
    第五步:将文档对象存入集合中
*/
destDocument.save().then((msg) => {
  console.log(msg);
});

```



#### 使用Model来进行crud(增删改查)

**前三步骤保持不变**

**Model的方法--create()**

**创建一个或多个文档对象并添加到数据库中**

```js
destModel
  .create({
    name: "平和岛静雄",
    age: 25,
    sex: "男",
    roles: ["力大", "帅气", "弟控"],
    info: "收租的，临也的死对头",
    createTime: Date.now(),
  })
  .then((data) => {
    console.log(data);
  });

```

**Model的方法--find()**

**查找所有符合条件的文档，返回的是数组**

```js
destModel
  .find({
    age: 23,
  })
  .then((data) => {
    console.log(data);
  });

```

**Model的方法--update()**

**修改（替换）一个或多个**

```js
const p3 = destModel.updateOne({
    age: 19
}, {
    $set: {
        age: 1
    }
});
p3.then((value) => {
    console.log(value);
}).catch((err) => {
    console.log(err)
})


//starsModel.update({name:"彭于晏"},{
//     name:"彭于晏",
//     age:27.99,
//     sex:"猛男",
//     roles:["唐钰小宝"],
//     info:"舔狗不得好死"
// },{upsert:true})
// .then((msg)=>{
//     console.log(msg)
// })
```

**Model的方法--remove()**

**删除一个或多个文档**

```js
destModel.deleteOne({age:2})
```