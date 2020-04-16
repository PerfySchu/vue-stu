# <center>__Vue实例生命周期__</center>

`创建` -> `运行` -> `销毁`
> 在Vue实例运行的三个过程中，会伴随着各种各样的事件，这些事件统称为生命周期函数（钩子函数）

## __1.创建阶段__
* __new Vue()__ 创建一个Vue实例对象
* __Init Events & Lifecycle__
* 调用第一个钩子函数 __beforeCreate()__，此时对象中的 __data__ 和 __method__ 均未初始化
* __Init injections & reactivity__
* 调用第二个钩子函数 __created()__，此时 __data__ 和 __methods__ 均已初始化完毕
* 判断 __Has 'el' option ?__，对象中没有el，则停止初始化进程，直到执行 __vm.$mount(el)__
* 判断 __Has 'template' option ?__，如果对象中没有template, 则将el的outerHTML做为模板进行编译。如果有template则直接编译template
* 执行第三个钩子函数 __beforeMount()__，此时模板编译完成放在内存中，尚未挂载到页面中，且此时编译出来的DOM元素是原始的插值表达式，并未进行赋值运算
* __Create vm.$el and replace 'el' with it__ 将'el'替换为编译好的HTML结构
* 执行第四个钩子函数 __mounted()__，此时挂载已完成，页面渲染完成，创建阶段结束，进入运行阶段

## __2.运行阶段__