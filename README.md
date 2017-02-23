# Diary_February
## Diary 【2.14】
### Javascript中的return作用及javascript return关键字用法详解

return 语句从当前函数退出，并从那个函数返回一个值

语法：

return[()[expression][]]

可选项 expression 参数是要从函数返回的值。如果省略，则该函数不返回值

**用 return 语句来终止一个函数的执行，并返回 expression 的值**

如果 expression 被省略，或在函数内没有 return 语句被执行，则把值 undefined 赋给调用当前函数的表达式

如果函数名前有返回类型的定义，比如int,double等就必须有返回值，而如果是void型,则可以不写return,写了也无效

onClick='return add_onclick()'与 onClick='add_onclick()'的区别

而该值决定了当前操作是否继续

当返回的是**true**时，将**继续操作**

当返回是**false**时，将**中断操作**

而直接执行时（不用return）。将不会对window.event.returnvalue进行设置

所以会默认地继续执行操作

onclick事件时就相当于onclick="return true/false"

return 是javascript里函数返回值的关键字，一个函数内处理的结果可以使用return 返回，这样在调用函数的地方就可以用变量接收返回结果

## Diary 【2.15】

### Javascript中的闭包理解（上）

#### 一、变量的作用域

要理解闭包，首先必须理解Javascript特殊的变量作用域

变量的作用域无非就是两种：**全局变量和局部变量**

Javascript语言的特殊之处:

就在于函数内部可以直接读取全局变量,在函数外部自然无法读取函数内的局部变量

注意：函数内部声明变量的时候，一定要使用var命令。如果不用的话，你实际上声明了一个**全局变量**

#### 二、如何从外部读取局部变量？

这就是Javascript语言特有的“**链式作用域**”结构（**chain scope**）

子对象会一级一级地向上寻找所有父对象的变量。所以，父对象的所有变量，对子对象都是可见的，反之则不成立

#### 三、闭包的概念

闭包就是能够读取其他函数内部变量的函数

由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成“定义在一个函数内部的函数”

所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁

#### 四、闭包的用途

闭包可以用在许多地方。它的最大用处有**两**个，一个是**可以读取函数内部的变量**，另一个就是**让这些变量的值始终保持在内存中**不会在调用结束后，被垃圾回收机制（garbage collection）回收

首先在nAdd前面没有使用var关键字，因此 nAdd是一个全局变量，而不是局部变量

其次，nAdd的值是一个**匿名函数**（anonymous function），而这个匿名函数本身也是一个闭包，所以nAdd相当于是一个setter

**可以在函数外部对函数内部的局部变量进行操作**

#### 五、使用闭包的注意点

由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包

闭包会在父函数外部，改变父函数内部变量的值

#### 六、思考题

当内部函数在定义它的作用域的外部被引用时,就创建了该**内部函数的闭包**

如果**内部函数**引用了位于**外部函数的变量**,当外部函数调用完毕后,这些变量在内存不会被释放,因为闭包需要它们

参考网址：http://www.jb51.net/article/24101.htm

### Javascript中的闭包理解(下)

#### 一、什么是闭包？

闭包是一个拥有许多变量和绑定了这些变量的环境的表达式（通常是一个函数），因而这些变量也是该表达式的一部分。

**JavaScript中所有的function都是一个闭包**

不过一般来说，嵌套的function所产生的闭包更为强大，也是大部分时候我们所谓的“闭包”

`function a() { 
  var i = 0; 
  function b() { alert(++i); } 
  return b;
}
  var c = a();
  c();`
  
这段代码有两个特点：

1. 函数b嵌套在函数a内部；
2. 函数a返回函数b。

![picture](http://files.jb51.net/upload/201007/20100703001016918.jpg)

函数a外的变量c引用了函数a内的函数b,创建了一个闭包

当函数a的内部函数b被函数a外的一个变量引用的时候，就创建了一个闭包

#### 二、不被回收的原理

此时a返回函数b的引用给c，又函数b的作用域链包含了对函数a的活动对象的引用，也就是说b可以访问到a中定义的所有变量和函数

函数b被c引用，函数b又依赖函数a，因此函数a在返回后不会被GC回收。

执行时b的作用域链包含了3个对象：b的活动对象、a的活动对象和window对象，如下图所示：

![picture2](http://files.jb51.net/upload/201007/20100703001017585.jpg)

如图所示，当在函数b中访问一个变量的时候，搜索顺序是：

先搜索自身的活动对象，如果存在则返回，如果不存在将继续搜索函数a的活动对象，依次查找，直到找到为止

如果函数b存在prototype原型对象，则在查找完自身的活动对象后先查找自身的原型对象，再继续查找。这就是Javascript中的变量查找机制

如果整个作用域链上都无法找到，则返回undefined

#### 三、闭包的应用场景

1. **保护函数内的变量安全**。以最开始的例子为例，**函数a中i只有函数b才能访问**，而无法通过其他途径访问到，因此保护了i的**安全性**
2. **在内存中维持一个变量**。依然如前例，由于闭包，函数a中i的一直存在于内存中，因此每次执行c()，都会给i自加1
3. 通过保护变量的安全**实现JS私有属性和私有方法**（不能被外部访问） 私有属性和方法在Constructor外是无法被访问的

function Constructor(...) {  
  var that = this;  
  var membername = value; 
  function membername(...) {...}
}

## Diary 【2.16】

### JS之闭包及应用

概念理解：执行环境 作用域 作用域链 调用对象 函数作用域分为词法作用域和动态作用域

譬如全局环境下的函数A内嵌套了一个函数B，则该函数B的作用域链就是：

**函数B的作用域—>函数A的作用域—>全局window的作用域**

当函数B调用时，寻找**某标识符**，会按

**函数B的作用域—>函数A的作用域—>全局window的作用域**去寻找，实际上是按

**函数B的调用对象—>函数A的调用对象—>全局对象这个顺序**去寻找的

#### 闭包详解

在动态执行环境中，数据实时地发生变化，为了保持这些非持久型变量的值，我们用闭包这种载体来存储这些**动态数据**

**闭包的定义**：所谓“闭包”，指的是一个拥有许多变量和绑定了这些变量的环境的表达式（通常是一个函数），因而这些变量也是该表达式的一部分
  
闭包就是嵌套在函数里面的内部函数，并且该内部函数可以访问外部函数中声明的所有局部变量、参数和其他内部函数
 
**当该内部函数在外部函数外被调用，就生成了闭包** B是A的内部函数 B被C调用

实际上任何函数都是全局作用域的内部函数，都能访问全局变量，所以都是window的闭包
 
#### 小结：

在动态执行环境中，数据实时地发生变化，为了保持这些非持久型变量的值，我们用闭包这种载体来存储这些动态数据。这就是闭包的作用

也就说遇到需要存储动态变化的数据或将被回收的数据时，我们可以通过外面再包裹一层函数形成闭包来解决

当然，闭包会导致很多外部函数的调用对象不能释放，滥用闭包会使得内存泄露，所以在频繁生成闭包的情景下我们要估计下他带来的副作用。

参考网址：http://www.jb51.net/article/24156.htm

## Diary 【2.17】

### 正则表达式入门

\b是正则表达式规定的一个**特殊代码（元字符）**，代表着单词的开头或结尾，也就是单词的分界处

如果要精确地查找hi这个单词的话，我们应该使用 \bhi\b

.是另一个**元字符**，**匹配除了换行符以外的任意字符**

\*同样是元字符，不过它代表的不是字符，也不是位置，而是数量

它指定\*前边的内容可以**连续重复使用任意次**以使整个表达式得到匹配

.\*连在一起就意味着任意数量的不包含换行的字符

中国的电话号码 : 0\d\d-\d\d\d\d\d\d\d\d匹配这样的字符串：

以0开头，然后是两个数字，然后是一个连字号“-”(-不是元字符，只匹配它本身——连字符)，最后是8个数字 

我们也可以这样写这个表达式：0\d{2}-\d{8}。这里\d后面的{2}({8})的意思是前面\d必须连续重复匹配2次(8次)

参考网址：http://deerchao.net/tutorials/regex/regex.htm

## Diary 【2.20】

### 匿名函数

匿名函数是指那些无需定义函数名的函数

匿名函数与 Lambda 表达式（拉姆达表达式）是一回事

唯一的不同——语法形式不同，Lambda 表达式更进一步

本质上，它们的作用都是：产生方法——内联方法，也就是说，**省去函数定义，直接写函数体** 

baz = function() { return foo * bar; };

没有函数名，是个匿名函数，采用 Lambda 表达式

 "var baz = foo()" 是一个 bar 函数的引用；"var blat= foo()" 是另一个 bar 函数引用
 
 JavaScript 特色之一——**运行在定义**，而不是运行的调用
 
 例子 var add = a(); add(); add(); add(); // 会不断增加  var b = add(); //会重新计算 
 
 函数例子 function a(){var a = 10; b = function(){ a *= 2; return a; } return b;}

用闭包实现私有成员 

现在，需要创建一个只能在对象内部访问的变量

用闭包再适合不过，因为通过闭包你可以创建只允许特定函数访问的变量，而且这些变量在这些函数的各次调用间依然存在

为了创建私有属性，你需要在构造函数的作用域中定义相关变量

这些变量可以被定义于该作用域中的所有函数访问，包括那些特权方法

参考网址：http://www.jb51.net/article/28449.htm

## Diary 【2.21】

### javascript中this的四种用法

在javascript当中每一个function都是一个对象，所以在这个里var temp=this 指的是function当前的对象

this是Javascript语言的一个关键字。它代表函数运行时，自动生成的一个内部对象，只能在函数内部使用

**This:**在函数执行时，**this 总是指向调用该函数的对象。要判断 this 的指向，其实就是判断 this 所在的函数属于谁**

在《javaScript语言精粹》这本书中，把 this 出现的场景分为四类：

1. 有对象就指向调用对象
2. 没调用对象就指向全局对象
3. 用new构造就指向新对象
4. 通过 apply 或 call 或 bind 来改变 this 的所指

参考网址：http://www.jb51.net/article/65850.htm

## Diary 【2.22】

### z-index详解
发现 z-index 对节点没起作用. z-index 属性仅在节点的 **position 属性**为 **relative**, **absolute** 或者 **fixed** 时生效.

#### 默认值规则
 如果所有节点都定义了 position:relative. z-index 为 0 的节点与没有定义 z-index 在同一层级内没有高低之分;
 
 z-index 大于等于 1 的节点会遮盖 没有定义 z-index 的节点
 
 z-index 的值为负数的节点将被没有定义 z-index 的节点覆盖

#### 从父规则
如果 A, B 节点都定义了 position:relative, A 节点的 z-index 比 B 节点大, 那么 A 的子节点必定覆盖在 B 的子节点前面.

#### 顺序规则
如果所有节点都定义了 position:relative, A 节点的 z-index 和 B 节点一样大, 但因为顺序规则, B 节点覆盖在 A 节点前面

就算 A 的子节点 z-index 值比 B 的子节点大, B 的子节点还是会覆盖在 A 的子节点前面

#### Tips
很多人将 z-index 设得很大, 9999 什么的都出来了, 如果不考虑父节点的影响, 设得再大也没用, 那是无法逾越的层级

#### 总结
z-index越大的在上方，没有设置z-index的默认为：0

参考网址： http://www.cnblogs.com/ForEvErNoME/p/3373641.html

## Diary 【2.23】
### JS中setTimeout()的用法详解

