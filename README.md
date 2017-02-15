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
