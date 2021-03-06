# 说说你对Vue中数据代理的看法/理解,谈谈...(你对Vue的理解)

**Vue中有数据代理,实现方式:在创建Vue的实例对象的时候,传入配置对象并保存到内部的$options属性中,从里面获取data对象,保存到变量data中及_data属性中,通过Object.keys()遍历data中的所有的属性,内部通过Object.defineProperty()方法为vm对象添加data对象中的每个属性,从而实现数据代理**

   **在实例化Vue的时候,内部会通过Object.keys()方法获取data中的每个属性,然后遍历,通过Object.defineProperty()方法为vm添加data中的每个属性，从而实现数据代理**





# 谈谈你对模版解析的理解(插值语法)

在Vue实例的时候,内部会创建模版解析对象,用来进行模版解析的操作,首先把选择器（#App）和当前的vm实例对象传入到解析对象中,内部会先创建文档碎片对象(虚拟DOM),根据选择器获取对应的html容器(id为app的div),把该容器中的所有的节点全部的放在文档碎片对象中,内部遍历文档碎片对象中所有的节点,判断该节点是不是标签,是不是文本,如果不是标签,也不是文本,再判断该节点是不是有子节点,那么就进行递归操作,直到该节点是标签或者该节点是文本节点并且是和插值正则表达式匹配的

  如果是标签,暂且不说（事件指令哪里答）

  如果是文本节点,并且和插值的正则匹配,此时通过正则表达式提取组方式获取插值语法中的表达式msg,然后内部调用bind方法进行解析，并且获取updater对象中相关的方法,还要根据vm和表达式msg获取该属性msg的值,然后调用updater对象中相关的方法进行插值文本的语法的替换,这些操作都是在内存中的虚拟DOM中进行的,结束后,会重新的把文档碎片对象加入到id为app的div容器中,至此,插值语法的解析操作结束



思路：

创建vm对象---->内部---->创建Compile编译对象(解析对象)---->创建文档碎片对象(虚拟DOM)---->内存中---->进行插值语法的文本替换---->div容器--->结束

创建编译的时候内部创建虚拟DOM(文档碎片对象),div中所有的子节点全都在文档碎片对象中,解析是在内存中进行的





# 谈谈你对模版解析的理解(之事件指令)

Vue中有模版解析中有事件指令的解析

   在Vue实例的时候,内部会创建模版解析对象,用来进行模版解析的操作,首先把选择器和当前的vm实例对象传入到解析对象中,内部会先创建文档碎片对象(虚拟DOM),根据选择器获取对应的html容器(id为app的div),把该容器中的所有的节点全部的放在文档碎片对象中,内部遍历文档碎片对象中所有的节点,判断该节点是不是标签,是不是文本,如果不是标签,也不是文本,再判断该节点是不是有子节点,那么就进行递归操作,直到该节点是标签或者该节点是文本节点并且是和插值正则表达式匹配的

  如果是标签,要获取该标签上所有的属性,判断每个属性是不是指令，然后再判断指令是不是事件指令,如果是事件指令，通过vm获取methods对象中相关的回调函数,最终通过addEventListener()方法绑定事件,最终会把该标签上所有的指令相关的属性全都删掉,从而实现事件指令的解析



# 面试题: 谈谈你对模版解析的理解(之普通指令)

  Vue中有模版解析中有普通指令的解析

   在Vue实例的时候,内部会创建模版解析对象,用来进行模版解析的操作,首先把选择器和当前的vm实例对象传入到解析对象中,内部会先创建文档碎片对象(虚拟DOM),根据选择器获取对应的html容器(id为app的div),把该容器中的所有的节点全部的放在文档碎片对象中,内部遍历文档碎片对象中所有的节点,判断该节点是不是标签,是不是文本,如果不是标签,也不是文本,再判断该节点是不是有子节点,那么就进行递归操作,直到该节点是标签或者该节点是文本节点并且是和插值正则表达式匹配的



  如果是标签,要获取该标签上所有的属性,判断每个属性是不是指令，然后再判断指令是不是普通指令,如果是普通指令，通过vm获取该指令中使用的表达式的值,最终调用updater对象中相关的方法实现解析的操作



# 数据劫持

data中的msg叫属性,html容器中的msg叫表达式

   data对象中有多少个属性,就会创建对应个数的dep对象

   当数据代理结束,模版解析之前,进行数据劫持操作,把vm中的data保存在劫持对象中的data属性中,

   然后遍历vm中的data对象中所有的数据,通过Object.defineProperty方法把vm中data对象中的每个属性添加到劫持对象中的data属性中,在遍历这些属性之后,添加属性之前,要创建dep对象

   数据劫持的时候

   data中一个属性对应一个dep

   模版解析的时候

   html中有一个表达式就会创建一个watcher



   \1. 当实例化MVVM对象的时候,内部会进行数据代理的实现,紧接着开始进行数据劫持,把vm中的data保存在劫持对象中的data属性中,

   然后遍历vm中的data对象中所有的数据,通过Object.defineProperty方法把vm中data对象中的每个属性添加到劫持对象中的data属性中,在遍历这些属性之后,添加属性之前,要创建dep对象,此时一个属性对应着一个dep对象

   \2. 当数据劫持结束后,开始进行模版解析,模版解析内部先创建文档碎片对象,然后把id为app的div中所有的节点全部的放在文档碎片对象中,开始遍历内部所有的子节点,判断节点是标签还是文本节点并且复合插值正则表达式,无论是标签还是文本节点要进行表达式的解析(指令(事件指令/普通指令)/插值),都会调用bind方法,bind方法内部会创建监视对象,只要是html容器中有一个表达式,就会创建一个监视对象,如此,多个表达式就会创建多个监视(watcher)对象



   \3. 在监视对象内部,为了获取表达式的值(为了获取html容器中对应的表达式在vm对象中的data对象中的属性的值),此时开始建立dep对象和watcher对象的关系,dep和watcher关系,如下:

   (解析html模版的时候为了获取表达式的值的时候,此时建立dep和watcher的关系

   建立dep和watcher的关系)

   1对1的关系:一个dep对应一个watcher的关系,data中一个属性,html容器中一个表达式

   1对多的关系:一个dep对应多个watcher的关系,data中一个属性,html容器中多个表达式

   多对1的关系:多个dep对应一个watcher的关系,data中多个属性,html容器中一个表达式

   多对多的关系:多个dep对应多个watcher的关系,data中多个属性,html容器中多个表达式

   

​    3.1 如何建立关系呢? 当在监视对象内部为了获取表达式的值的时候,此时会进入到vm中的get方法中,会进入到劫持对象的get方法,然后会跳到watcher中,把dep添加到watcher中,watcher会添加到dep中,建立关系



   为什么要建立关系? 如果数据变化了,界面就要重新渲染





# 面试题:谈谈数据的双向绑定的理解

   Vue中有模版解析中有双向数据绑定的解析

   在Vue实例的时候,内部会创建模版解析对象,用来进行模版解析的操作,首先把选择器和当前的vm实例对象传入到解析对象中,内部会先创建文档碎片对象(虚拟DOM),根据选择器获取对应的html容器(id为app的div),把该容器中的所有的节点全部的放在文档碎片对象中,内部遍历文档碎片对象中所有的节点,判断该节点是不是标签,是不是文本,如果不是标签,也不是文本,再判断该节点是不是有子节点,那么就进行递归操作,直到该节点是标签或者该节点是文本节点并且是和插值正则表达式匹配的

  如果是标签,要获取该标签上所有的属性,判断每个属性是不是指令，如果这个属性是指令,判断该指令是不是事件指令或者是不是普通指令,v-model属于普通指令,通过updater对象中对应的方法,根据vm来获取msg表达式的值,把这个值给当前的input标签的value属性

  紧接着,通过addEventListener为当前的input标签绑定是input事件及对应的回调函数

  **dep如何通知watcher**

  当在页面中做相关的操作的时候,如果更改了页面(更改了数据-->data对象中的属性值),此时等于是在重新设置data对象中对应的某个属性值,那么就会进入到MVVM中的set方法,就会进入到Observer的set方法中,此时这个属性对应的dep会调用notify()方法通知当前的这个dep对应的所有的watcher对象,进行数据的更新,重新进行模版的解析,重新渲染界面,至此这就是所谓的dep和watcher真正的关系所要做的事情





# 谈谈你对Vue源码的理解

1) Vue中有数据代理,还有模版解析及双向数据绑定,数据劫持和监视

首先,在实例化Vue的时候,内部会先把data对象中所有的属性进行遍历,通过Object.defineProperty()方法把data中的每个属性一个一个的添加到vm对象上,从而实现数据代理

然后,把实例化Vue的时候内部的el属性中对应的选择器传给对应的模版解析对象,内部根据选择器获取html中对应的容器对象,创建文档碎片对象(可以理解为虚拟DOM),把容器对象中所有的子节点全部的放进文档碎片对象中,遍历里面所有的节点,判断节点是标签还是文本及符合插值的正则

如果是标签,获取该标签中所有的属性,判断属性是不是指令,如果是指令,再次判断是事件指令还是普通指令

 如果是事件指令,通过addEventListener为当前的节点标签绑定对应的事件

 如果是普通指令,还要获取该指令是不是双向数据绑定指令

  如果是v-model的这个指令

   则使用addEventListener为当前的标签绑定对应的input事件

   剩下的其他的普通指令,也会执行下面的内容

  最终都会调用updater对象中相关的方法进行解析

  如果是文本且符合插值正则,最终也会进入到updater对象中相关的方法

  以上都会进入到bind方法,内部开始创建监视对象(html容器中有一个表达式就创建一个watcher对象)

  在watcher对象内部为了获取表达式的值,开始建立dep和watcher对象的关系

 dep和watcher之间的关系如下:

   1对1的关系:一个dep对应一个watcher的关系,data中一个属性,html容器中一个表达式

   1对多的关系:一个dep对应多个watcher的关系,data中一个属性,html容器中多个表达式

   多对1的关系:多个dep对应一个watcher的关系,data中多个属性,html容器中一个表达式

   多对多的关系:多个dep对应多个watcher的关系,data中多个属性,html容器中多个表达式



  如果要修改数据,那么这个数据对应的属性对应的dep会通知watcher进行数据的更新,及界面的重新渲染

  以上就是数据代理及模版解析及数据劫持的理解