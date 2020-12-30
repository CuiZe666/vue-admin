# git准备

Git是一款开源免费的分布式的版本控制系统。是Linux之父为了方便管理Linux内核代码而开发的。

## 一、作用

版本控制系统在项目开发中作用重大，能记录文件的历史状态，主要功能有以下几点：

- 代码备份
- 版本回退
- 协作开发
- 权限控制

## 二、下载安装

下载地址 https://git-scm.com/

安装方式和QQ安装相同，一路下一步，中间可以设置软件的安装路径。

因权限问题，安装目录尽量保持默认设置在C盘。



## 三、Linux常用命令

Linux是一套开源免费的操作系统，与系统的交互通常用命令来实现，常用的命令有：

- `ls` 查看当前文件夹下的文件（list单词的缩写）， `ls -al` or `ls -a -l`查看隐藏文件并竖向排列
- `cd` 进入某一个文件夹（change directory）的缩写，`cd ..`回到上一级。`tab`键代码自动补全
- `clear` 清屏
- `mkdir xxx` 创建文件夹
- `touch test.html` 创建一个文件
- `rm test.html` 删除一个文件
- `rm -r dir` 删除文件夹
- `mv 原文件或文件夹 目标文件或文件夹` 移动文件
- `cat test.html` 查看文件内容
- `ctrl+c` 取消命令
- 上下方向键，可以查看命令历史



Vim是一款命令行下的文本编辑器，编辑方式跟图形化编辑器不同

- `vim test.html`编辑文件（文件不存在则创建）
- i可以进入编辑模式
- `ESC` + `:wq` 保存并推出
- `ESC`+`:q!` 不保存并推出

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gfdqt2es84j30iy0b2abb.jpg)





# git 初始配置

第一次使用Git的时候，会要求我们配置用户名和邮箱，用于表示开发者的信息。

## 一、用户名和邮箱地址的作用

用户名和邮箱地址是本地git客户端的一个变量，不随git库而改变。

每次commit都会用用户名和邮箱纪录。

github的contributions统计就是按邮箱来统计的。

## 二、用户名和邮箱地址设置

```git
//配置用户名
git config --global user.name "username"
//配置密码
git config --global user.email "email"
```

## 三、查看用户名和邮箱

```git
查看用户名和邮箱地址：
git config user.name
git config user.email
git config -l //查看git所有配置
```





## 四、git命令

```js
git init  //初始化命令,创建库
git add . //将文件由工作区添加到暂存区;（.是全部添加，后接文件则添加指定）
git commit -m '说明信息'  //将文件由暂存区添加到仓库区,生成版本号
git restore XXX //可以丢弃工作区的改动，回撤之前未改动状态  (空格加.  全部丢弃)
git restore --staged XXX  //可以取消暂存，已经添加到暂存区可以取消回撤到工作区
git status //查看状态
git log //查看日志 此时会进入日志状态,按q可退出;（回退之前的话只能看到之前的日志，看不到之后）
git log --oneline //查看版本号
git reflog //查看所有日志 (如果不小心关闭命名板,用git log只能看到当前的日志,用git reflog就可以看关闭前的日志)
//如果在回退以后又想再次回到之前的版本，git reflog 可以查看所有分支的所有操作记录（包括commit和reset的操作），包括已经被删除的commit记录，git log则不能察看已经删除了的commit记录

git diff //查看工作区与暂存区的差异（不显示删除或新增文件） 显示做了哪些修改
git diff --cached //查看暂存区与仓库的差异

git reset --hard //版本号 版本穿梭,即读取记录
登录账号
git config --global user.email '1264995180@qq.com'
git config --global user.name 'Daihui00'
退出账号
git config --global --unset user.email "1264995180@qq.com"
git config --global --unset user.name "Daihui00"

git push //将文件上传到远程仓库

```

**git commit -m '说明信息'** 

​	feat:新增加一个功能

​	fix:修复了一个bug

​	cors:对核心功能进行了操作

​	docs：对文档进行了操作

### 根据版本号进行回滚

- 版本回滚改动提交并不会影响之前已经提交的各个版本内容，即使是回退的目标版本
- 只会在你改动提交之后多了一个版本（根据需求回滚改动得到你想要的版本）
- 进行版本回退时，不需要使用完整的哈希字符串，前七位即可
- 版本切换之前，要提交当前的代码状态到仓库



`git status` 版本状态查看

红色：说明文件位于工作区

绿色：说明文件位于暂存区

没有体现，说明位于版本区









# 分支

- **创建库后默认有一个主分支叫master,主分支的代码一定能运行,一般用来发布正式的版本,上线用的,不能在主分支里写代码**
- **我们要编辑代码的时候要新建一个开发分支(dev),用来编码,测试,没问题后才会把它合并到主分支中;**
- **主分支和开发分支,不能在主分支里面写代码,而是在开发分支里面写代码,开发分支里面的代码没问题后,能运行后再添加到主分支**
- **主分支和开发分支就像平行宇宙,只要不合并,就互不干扰,开发分支就像草稿,完成后就添加到主分支里**
- **在某分支上创建其他分支时，创建的分支内容和某分支自己一样，创建的分支也会有之前的版本，支持版本回滚**
- **各个分支的操作只在各自的分支体现，不会影响到其他分支**



## git本地分支操作

在创建库后,编写代码时,在终端输入git branch 分支名称;创建分支,我们在分支里面写代码

代码写完后输入git checkout 分支名称;切换分支,切换到主分支,再输入git merge 分支名称;将开发分支合并到主分支;

**1.根据主分支master新建一个分支**

​      **git branch 分支名**

**2.进入到新分支**

​      **git checkout 分支名**

**3.在新分支中编码, add commit**

**4.切换到主分支**

​     **git checkout master**

**5.合并分支**

​     **git merge 分支名   （当前分支是合并者，merge后接的分支是被合并者，被合并的分支不会有影响）**

​     **注意: 一定要进到主分支master才能合并**

**6.删除分支**

​     **git branch -d 分支名**

​     **注意:要进入到主分支,才能删除次分支.**	

**补充:**

  **git branch    查看分支**

  **git checkout -b 分支名称  创建并切换分支;(创建一个分支,并进入创建的分支);**



## 冲突

当多个分支修改同一个文件后，合并分支的时候就会产生冲突。冲突的解决非常简单，将内容修改为最终想要的结果，然后继续执行 git add 与 git commit 就可以了。



# 基础使用

## 远程仓库两种情况

**先本地提交再push**

**push前先pull更新下看看，看别人先提交的跟你的有没有冲突，解决完冲突后在push提交上去**

**本地如果只有一个分支直接git push，多个则需要指定提交，pull也一样**

```
git push /git pull
git push origin dev / git pull origin dev
```





### 本地有仓库

1. 注册并激活账号

2. 创建 GitHub 中心仓库

3. 获取仓库的地址

4. 本地配置远程仓库的地址（关联目的推送过去）

   ```shell
   git remote add origin https://github.com/nowLetsgo/test.git
       //远端仓库管理 添加一个远程仓库的url的别名
       //add  添加
       //origin 远端仓库的别名（git remote -v 可以查看仓库所有的别名）
       //https://github.com/nowLetsgo/test.git    仓库地址
       //git remote可以添加删除重命名等操作（使用 git remote -h查看）
   ```

1. 本地提交（确认代码已经提交到本地仓库）

2. 将本地仓库某个分内容推送到远程仓库（前面写一次，后面不用-u）

   ```shell
   git push -u origin master
       //
       push 推送
       -u   关联, 加上以后,后续提交时可以直接使用 git push
       origin 远端仓库的别名
       master 本地仓库的分支
   ```





### 本地没有仓库

1. 注册并激活账号

2. 克隆仓库

   ```shell
   git clone https://github.com/nowLetsgo/test.git [name]
   //name 是对仓库名字的修改
   ```

3.拉取本地没有的分支，切换到分支上操作(有需要的话，一般刚开始都是新建一个自己的分支)

```js
git fetch origin dev:dev
git checkout dev
```

4.本地提交

```shell
git add .
git commit -m 'message'
```

5.推送分支(dev)到远程

```shell
git push origin dev
```



**注意**：

**1.本地创建的分支包含内容需要push到远程仓库，远程仓库才会有**

**2.本地如果只有一个分支直接git push，多个则需要指定提交 例如：git push origin dev**

**3.先本地提交再push到远程，push到远程前可以pull更新下看看有没有冲突，有就解决冲突再push**

**4.远程克隆仓库到本地，默认是主分支（master）的内容，没有分支内容，要自己拉取分支到本地再自己切换分支操作**

```
git fetch origin dev:dev
git checkout dev
```







# 配置忽略文件

## 一、仓库中没有提交该文件

项目中有些文件是不需要进入版本库中，比如编辑器的配置。Git 中需要创建一个文件.gitignore，在这个文件里编辑配置忽略文件

```gitignore
# 忽略所有的 .idea 文件夹
.idea
# 忽略所有以 .test 结尾的文件
*.test
# 忽略 node_modules 文件和文件夹
node_modules/
# 忽略 单个文件
001.css(直接名称加后缀名)
```

## 二、仓库中已经提交该文件

1. 对于已经加入到版本库的文件，可以在版本库中删除该文件（本地删除）

   ```sh
   git rm --cached .idea
   git rm --cached ./css/go.css
   ```

2. 然后在 .gitignore 中配置忽略

   ```sh
   .idea
   /css/go.css
   ```

3. add 和 commit 提交即可

4. 再推送上去即可





# 协作流程

## 入职第一天

- 得到 Git 远程仓库的地址和账号密码

- 将代码克隆到本地（地址换成自己的）

  ```shell
  git clone https://github.com/xiaohigh/test.git
  ```

- 切换分支

  ```
  git checkout -b xiaohigh
  ```

- 开发代码

- 本地提交

  ```shell
  git add -A
  git commit -m '注释内容'
  ```

- 合并分支

  ```shell
  git checkout master
  git merge xiaohigh
  ```

- 更新本地代码

  ```shell
  git pull
  //或者使用下面两行代码
  git fetch 
  git merge origin/master
  ```

- 提交代码

  ```shell
  git push 
  ```

## 第二天工作流程

1. 更新代码 (git pull)
2. 切换分支
3. 开发功能
4. 提交
5. 合并分支
6. 更新代码
7. 提交代码

#### 冲突解决

同分支冲突一样的处理，将代码调整成最终的样式，提交代码即可。





# ssh免密登录

1. 创建非对称加密对

   ```sh
   ssh-keygen //在任意位置输入命令即可
   ```

2. 文件默认存储在家目录（c:/用户/用户名/.ssh）的 .ssh 文件夹中。

   - id_rsa 私钥
   - id_rsa.pub 公钥，**（这个给别人别人给你设置权限）**

3. 将公钥（.pub）文件内容配置到账号的秘钥中

   - 首页 -> 右上角头像-> settings -> SSH and GPG keys -> new SSH Key

4. 克隆代码时，选择 ssh 模式进行克隆 （地址 在仓库首页 绿色 克隆的位置 选择 use ssh）

   ```shell
   git clone git@github.com/xiaohigh/team-repo-1.git 
   ```

5. 克隆代码时的提醒，这里需要输入 yes



# GitFlow

GitFlow 是团队开发的一种最佳实践，将代码划分为以下几个分支

![img](https://tva1.sinaimg.cn/large/007S8ZIlgy1gffiy2a356j30dy09yt90.jpg)

- Master 主分支。上面只保存正式发布的版本
- Hotfix 线上代码 Bug 修复分支。开发完后需要合并回Master和Develop分支，同时在Master上打一个tag
- Feather 功能分支。当开发某个功能时，创建一个单独的分支，开发完毕后再合并到 dev 分支
- Release 分支。待发布分支，Release分支基于Develop分支创建，在这个Release分支上测试，修改Bug
- Develop 开发分支。开发者都在这个分支上提交代码