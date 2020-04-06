# <center>__Vue基础语法__</center>
<center><strong>面向数据，而不是DOM编程</strong></center>

---
## __1.挂载点，模板和实例之间的关系__

> <strong>挂载点</strong>：Vue实例中 <strong>el</strong> 属性所对应的节点

><strong>模板</strong>：挂载点内部的内容就是模板，也可以写在Vue实例的 <strong>template</strong> 属性里面

><strong>实例</strong>：js脚本中 <strong>new Vue</strong> 创建出来的为一个vue实例，其中包含挂载点，模板，数据，方法等一系列元素
 ```html
    <div id="root">
        <h1>{{content}}</h1>
    </div>

    <script>
        new Vue({
            el: "#root",
            // template: '<h1>Hello Vue</h1>',
            data: {
                content:"<h1>hello vue</h1>"
            }
        });
    </script>
```
---
## __2.数据和事件__
>__对一个DOM赋值可以使用一下三种方式__
* <strong>{{变量名}} </strong>插值表达式，网络不佳时，可能导致闪烁问题。使用v-cloak设置display none 消除闪烁
* <strong>v-text</strong> : 原样输出 变量里面的内容， 和插值表达式一样
* <strong>v-html</strong> : 会尝试将变量内容解析为html元素, 与使用template相似
``` html
    <div id="root">
        <!-- 引入数据的三种方式 -->
        <h1>{{number}}</h1>
        <h1 v-text="number"></h1>
        <h1 v-html="number"></h1>
    </div>

    <script>
        new Vue({
            el: "#root",
            data: {
                number: <h1>hello vue</h1>
            }
        })
    </script>
```
>__v-on 和 @ 都可以绑定事件，对应事件处理写在 method 中__
```html
    <div id="root">
        <div v-on:click="handleClick">{{content2}}</div>
        <div @click="handleClick2">{{content2}}</div>
    </div>
    <script>
        new Vue({
            el: "#root",
            data: {               
                content2:"hello"
            },
            methods: {
                handleClick: function(){
                    this.content2 = "world";
                },
                handleClick2: function(){
                    this.content2 = "hello";
                }
            }
        });
    </script>
```
---
## __3.属性绑定和双向数据绑定__
><strong>v-bind</strong> 可以实现属性和数据间的单向绑定，即数据可以单向传递给属性,可以简写为冒号加属性<strong>  :title</strong> 

><strong>v-model</strong> 可以实现value属性和数据间的双向绑定，任一方的值改变都可以传给另一方。__只能用在表单元素中__
```html
    <div id="root">
        <!-- 直接对应title，无法获取data中的DOMtitle值 -->
        <div title="DOMtitle">hello world</div>
        <!-- 使用 v-bind 标签可以将属性和data值绑定，DOMtitle 会对应到 data.DOMtitle  -->
        <div v-bind:title="DOMtitle">hello vue</div>
        <!-- 使用 v-bind 标签可以简写为冒号 : 代替  -->
        <div :title="DOMtitle">hello vue</div>
        
        <input v-model="content" />
        <div>{{content}}</div>
    </div>

    <script>
        new Vue({
            el: "#root",
            data: {
                DOMtitle: "this is hello vue",
                content: "this is content"
            }
        });
    </script>
```
---
## __4.计算属性与侦听器__
><strong>计算属性</strong>：一个属性的值是根据其他数据项计算得来的结果，写在<strong> computed </strong>中，使用函数来进行计算赋值

><strong>侦听器</strong>：监测某一个数据或者计算属性的变化，一旦发生变化，就可以触发侦听器中对应的业务逻辑，写在<strong> watch </strong>中
```html
    <div id="root">
        姓：<input v-model="firstName" />
        名：<input v-model="lastName" />
        <div>{{fullName}}</div>
        <div>{{count}}</div>
    </div>
    <script>
        new Vue({
            el: "#root",
            data: {
                firstName:"",
                lastName:"",
                // fullName:firstName+" "+lastName
                count: 0
            },
            //一个属性通过其他属性计算而来，如果firstName 和 lastName 都没改变，则会使用上一次计算的值
            computed: {
                fullName: function(){
                    return this.firstName + " " + this.lastName;
                }
            },
            watch: {
                // firstName: function(){
                //     this.count ++;
                // },
                // lastName: function(){
                //     this.count ++;
                // }
                fullName: function(){
                    this.count ++;
                }
            }
        });
    </script>
```
---
## __5.v-if, v-show 和 v-for 标签__
><strong>[v-if:变量名]</strong> :根据变量名对应的布尔值来确定当前元素是否显示，值为true时显示当前元素，为false时不显示，<font style="color:#d93737"><strong>其底层是通过创建和销毁DOM节点来实现的</strong></font>

><strong>[v-show:变量名]</strong> :与 <strong>v-if</strong> 使用方法相同，<font style="color:#d93737"><strong>其底层是通过添加和移除元素的样式"display:none"来实现元素的展示和隐藏</strong></font>，对于需要频繁改变现实状态的DOM，使用<strong> v-show </strong>效率高一点
```html
    <div id="root">
        <!-- 直接移除DOM节点俩实现隐藏 -->
        <div v-if="show">hello v-if</div>
        <!-- 通过给节点添加 display:none 样式来隐藏节点 -->
        <div v-show="show2">hello v-show</div>
        <button @click="handleVif">v-if</button>
        <button @click="handleVshow">v-if</button>
    </div>
    <script>
        new Vue({
            el: "#root",
            data: {
               show: true,
               show2:true
            },
            methods: {
                handleVif: function(){
                    this.show = !this.show;
                },
                handleVshow:function(){
                    this.show2 = !this.show2;
                }
            }
        });
    </script>
```
><strong> v-for </strong> :对于需要遍历的数据内容，可以使用<strong> v-for="<font style="color:#1c26f0">item</font>  of <font style="color:#aa3939">list</font>"</strong>的形式来遍历的数据，其中<strong>item</strong>为每个数据项，<strong>list</strong>是<strong>data</strong>中定义的数据集合,DOM中可以使用{{item}}来获取数据项的值
```html
    <div id="root">
       <ul>
           <li v-for="item of list">{{item}}</li>
            <!-- 添加key元素可以提高遍历效率，key必须唯一，item唯一时也可以当做key -->
           <li v-for="item of list" key="item">{{item}}</li>
           <!-- 如果暂时没有合适的值来作为key，可以item后附加index作为key，index是自动生成的唯一序列 -->
           <li v-for="(item, index) of list" key="index">{{item}}</li>
       </ul>
    </div>
    <script>
        new Vue({
            el: "#root",
            data: {
               list:[1,2,3]
            }
        });
    </script>
```
---
## __6.组件拆分，全局组件和局部组件及组件传值__
>__全局组件__: 在js脚本内，使用<strong><font color='#aa3939'>Vue.component(<font color=#004080>组件名</font>, {<font color=#004080>组件类型</font>：<font color=#004080>组件内容</font>})</font></strong> 来声明全局组件，并在页面中可直接以<font color=#aa3939><组件名></组件名></font>的方式直接使用全局组件
```html
    <div id="root">
      <ul>
          <!-- 直接将名称作为标签使用 -->
          <todo-item></todo-item>
      </ul>
    </div>
    <script>
        // 全局组件
        Vue.component('todo-item',{
            template: '<li>item</li>'
        });
        new Vue({
            el: "#root"
        });
    </script>
```
>__局部组件__:在js脚本中，使用变量<font color=#aa3939>__var a = {组件类型：组件内容}__</font>的形式来定义组件对象，定义好的组件需要在Vue对象中注册方可使用
```html
    <div id="root">
      <ul>
        <!-- 直接将名称作为标签使用 -->
        <todo-item></todo-item>
      </ul>
    </div>
    <script>
        // 局部组件
        var todoItem = {
            template:'<li>item</li>'
        }
        new Vue({
            el: "#root",
            components:{
                // 局部模板需要再这里注册才能使用
                'todo-item': todoItem
            }
        });
    </script>
```
>__组件传值__: 组件在定义时，可使用 <font color=#aa3939>__props：['参数名']__</font> 的形式指定参数，在使用组件时，可向其中传值，组件内部可直接以 <font color=#aa3939>__{{参数名}}__</font> 的形式使用参数
```html
    <todo-item 
        v-for="(item, index) of list" 
        :key="index"
        :content="item">
    </todo-item>
    
    <script>
        // 全局组件,定义 content 入参
        Vue.component('todo-item',{
            props: ['content'],
            template: '<li>{{content}}</li>'
        });
    </script>
```