# MVVM模式

## **回顾MVC**

**MVC**  

Model-View-Controller（模型-视图-控制器） 模式

**Model**（模型） ：代表存取数据的对象或 JAVA POJO
**View**（视图）：视图代表模型包含的数据的可视化
**Controller**（控制器）：控制器作用于模型和视图上。它控制数据流向模型对象，并在数据变化时更新视图

![image-20200613175135135](F:\typora笔记\pic\image-20200613175135135.png)





## MVVM介绍

**MVVM**（Model-View-ViewModel）是一种软件架构设计模式，源自于 MVC

核心是 **ViewModel** 层，负责转换 Model 中的数据对象来让数据变得更容易管理和使用。

其作用如下：

- 该层向上与视图层进行双向数据绑定
- 向下与 Model 层通过接口请求进行数据交互

![image-20200613175818773](F:\typora笔记\pic\image-20200613175818773.png)



## **MVVM 模式的优势**

- **低耦合**： 视图（View）可以独立于 Model 变化和修改，一个 ViewModel 可以绑定到不同的 View 上，当 View 变化的时候 Model 可以不变，当 Model 变化的时候 View 也可以不变
- **可复用**： 你可以把一些视图逻辑放在一个 ViewModel 里面，让很多 View 重用这段视图逻辑
- **独立开发**： 开发人员可以专注于业务逻辑和数据的开发（ViewModel），设计人员可以专注于页面设计
- **可测试**： 界面素来是比较难于测试的，而现在测试可以针对 ViewModel 来写



## MVVM 的组成部分

![image-20200613180134316](F:\typora笔记\pic\image-20200613180134316.png)

View 层展现的不是 `Model` 层的数据，而是 `ViewModel` 的数据，由 `ViewModel` 负责与 `Model` 层交互，**完全解耦了 View 层和 Model 层，这个解耦是至关重要的，它是前后端分离方案实施的重要一环**





# Vue简介

Vue 是一套用于构建用户界面的渐进式框架，Vue 的核心库只关注视图层，易于上手，且便于与第三方库（如：vue-router，vue-resource，vuex）或既有项目整合。

**MVVM 模式的实现者**
Model：模型层，在这里表示 JavaScript 对象
View：视图层，在这里表示 DOM（HTML 操作的元素）
ViewModel：

- 连接视图和数据的中间件，**Vue.js** 就是 MVVM 中的 **ViewModel** 层的实现者

- ViewModel 能够观察到数据的变化，并对视图对应的内容进行更新
- ViewModel 能够监听到视图的变化，并能够通知数据发生改变
- Vue.js 就是一个 MVVM 的实现者，他的核心就是实现了 DOM 监听 与 数据绑定




# 第一个Vue程序

vue官网：https://cn.vuejs.org/

vue中文文档：https://vuejs.bootcss.com/api/

1.IDEA 可以安装 Vue 的插件

2.Vue.js 安装

- 方式一：下载到本地

  https://vuejs.org/js/vue.min.js

- 方式二：使用在线CDN（两种）

```html
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.min.js"></script>
<script src="https://unpkg.com/vue/dist/vue.js"></script>
```



**代码编写**

Vue.js 的核心是实现了 MVVM 模式，她扮演的角色就是 **ViewModel** 层

下面示例展示其数据绑定功能，操作流程如下

创建一个 HTML 文件

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>vue01</title>
    
    <!--引入vue.js-->
    <script src="https://unpkg.com/vue/dist/vue.js"></script>
</head>
<body>
    <!--将数据绑定到页面元素-->
    <div id="app">
        {{msg}}		<!--使用双括号绑定数据-->
    </div>
  
    <script type="text/javascript">
        var vm=new Vue({
            el:'#app',		//绑定元素的 ID
            data:{
                msg:'hello Vue!'
            }		//数据对象中有一个名为message的属性，并设置了初始值 Hello Vue!
        });
    </script>
    
</body>
</html>
```

**测试**

![image-20200614170513652](pic/image-20200614170513652.png)

进入开发者工具

![image-20200614170633221](pic/image-20200614170633221.png)

- 这里可以在控制台直接输入 vm.msg  来修改值，这就是借助了 Vue 的 数据绑定功能实现的

- ViewModel来实现数据的监听与绑定，以做到数据与视图的快速响应

# 基础语法

## **v-bind**

**示例**

“将这个元素节点的 title 特性和 Vue 实例的 message 属性保持一致”

```html
<body>
    <div id="app">
        <span v-bind:title="msg">鼠标悬停几秒钟查看此处动态绑定的提示信息！</span>
    </div>
    <script type="text/javascript">
        var vm=new Vue({
            el:'#app',
            data:{
                msg:'hello Vue!'
            }
        });
    </script>
</body>
</html>
```

```

```

浏览器控制台输入 vm.message = ‘新消息’，就会再一次看到这个绑定了 title 特性的 HTML 已经进行了更新



## v-if,v-else-if,v-else

```html
<body>
<div id="app">
    <h2 v-if="msg==='A'">a</h2>
    <h2 v-else-if="msg==='B'">b</h2>
    <h2 v-else>other</h2>
</div>
<script type="text/javascript">
    var vm=new Vue({
        el:'#app',
        data:{
            msg:'A'
        }
    });
</script>
</body>
```

![image-20200614205409900](pic/image-20200614205409900.png)

## v-for

```html
	<div id="app">
        <li v-for="item in items">		<!--items是数组，item是数组元素迭代的别名-->
            {{item.msg}}		
        </li>
    </div>
    <script type="text/javascript">
        var vm=new Vue({
            el:'#app',
            data:{
                items:[
                    {msg:"java"},
                    {msg:"python"},
                    {msg:"javaEE"}
                ]
            }
        });
    </script>
```

![image-20200614180035015](pic/image-20200614180035015.png)

# v-on 监听事件

事件有Vue的事件、和前端页面本身的一些事件

这里的`click`是vue的事件，可以绑定到Vue中的`methods`中的方法事件！

```html
<body>
<div id="app">
  <!--使用v-on绑定事件，事件类型同jquery-->
  <button v-on:click="sayHello">单击我</button>
</div>
<script type="text/javascript">
    var vm=new Vue({
        el:'#app',
        data:{
            msg:'hello world！'
        },
        methods:{   //方法必须定义在Vue的methods对象中，v-on：事件
            sayHello:function (e) {
                alert(this.msg);
            }
        }
    });
</script>
</body>
```

![image-20200614211215785](pic/image-20200614211215785.png)

### 

# Vue双向绑定  v-model


数据双向绑定，当数据发生变化的时候，视图也就发生变化，当视图发生变化的时候，数据也会跟着同步变化。

在表单中使用双向数据绑定
你可以用 v-model 指令在表单 <input>、<textarea> 及 <select> 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。 v-model 负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。

注意：v-model 会忽略所有表单元素的 value、checked、selected 特性的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 data 选项中声明初始值!

**单行文本<input>**

```html
<body>
<div id="app">
  input：<input type="text" v-model="msg">{{msg}}
</div>
<script type="text/javascript">
    //viewModel 实现与Model双向绑定，动态更新视图
    var vm=new Vue({
        el:'#app',
        data:{
            msg:'hello world！'
        },

    });
</script>
```

![image-20200614220955857](pic/image-20200614220955857.png)

![image-20200614220802825](pic/image-20200614220802825.png)

**多行文本<input>**

```html
<div id="app">
    input：<textarea v-model="msg"></textarea>
    <br/>
    多行文本内容：{{msg}}
</div>
<script type="text/javascript">
    var vm=new Vue({
        el:'#app',
        data:{
            msg:'hello world！'
        },
    });
</script>
```

![image-20200614222026521](pic/image-20200614222026521.png)

**复选框**

```html
<div id="app">
    <input id="c1" type="checkbox" v-model="checked1">
    <input id="c2" type="checkbox" v-model="checked2">
    <br/>
    {{checked1}}
    {{checked2}}
</div>
<script type="text/javascript">
    var vm=new Vue({
        el:'#app',
        data:{
            checked1:false,
            checked2:false
        },
    });
</script>
```

![image-20200614223657944](pic/image-20200614223657944.png)

**下拉框**

**注意**：如果 `v-model` 表达式的初始值未能匹配任何选项， 元素将被渲染为“未选中”状态。

在 iOS 中，这会使用户无法选择第一个选项。因为这样的情况下，iOS 不会触发 change 事件。因此，更推荐像下面这样提供一个值为空的禁用选项。

```html
<div id="app">
    <select v-model="selected">
        <option disabled value="">请选择</option>
        <option>A</option>
        <option>B</option>
        <option>C</option>
    </select>
    <span>选中的值：{{selected}}</span>
</div>
<script type="text/javascript">
    var vm=new Vue({
        el:'#app',
        data:{
            selected:''
        },
    });
</script>
```

![image-20200614224515729](pic/image-20200614224515729.png)

# **Vue组件**

组件是可复用的 `Vue` 实例，一组可以重复使用的模板，如页头、侧边栏、内容区等组件。

![image-20200614234429053](pic/image-20200614234429053.png)

## 第一个 Vue 组件

注意：在实际开发中，我们并不会用以下方式开发组件，而是采用 vue-cli 创建 .vue 模板文件的方式开发，以下方法只是为了理解什么是组件。

组件命名规则：官方推荐的组件名是 每个单词首字母大写（PascalCase） 或者 全小写用 - 连接（kebab-case） 。 在DOM（这里即vue.js）中使用的时候， 改为全小写， 单词之间用 - 连接。

**经过测试还是全小写加 - 连接比较合适，推荐使用**

- Vue.component()：注册组件
- my-component：自定义组件的名字
- template：组件的模板

```html
<div id="app">
    <ul>
        <my-component></my-component>
        <my-component></my-component>
        <my-component></my-component>
    </ul>

</div>
<script type="text/javascript">
    //先注册组件
    Vue.component('my-component',{
        template:'<li>hello li</li>'
    });

    //再实例化Vue
    var vm=new Vue({
        el:'#app',
        data:{
            msg:''
        }
    });
</script>
```

**使用 props 属性传递参数**

像上面那样用组件没有任何意义，所以我们是需要传递参数到组件的，此时就需要使用 `props` 属性了！

```html
<body>
<div id="app">
    <ul>
        <my-component v-for="item in items" v-bind:chen="item"></my-component>

    </ul>

</div>
<script type="text/javascript">
    //先注册组件
    Vue.component('myComponent',{
        props:['chen'],
        template:'<li>{{chen}}</li>'
    });
    //再实例化Vue
    var vm=new Vue({
        el:'#app',
        data:{
            items:['java','linux','docker']
        }
    });
</script>

</body>
```

![image-20200615091436634](pic/image-20200615091436634.png)

![image-20200615093245215](pic/image-20200615093245215.png)



## vue.js 组件命名和组件prop属性命名

（1）组件标签命名是“驼峰命名法”命名。因为HTML不支持大小写。浏览器会将大写转为小写。
    所以如果组件标签使用如“vueHtml”的命名，则在vue里相应的是“vuehtml”

![image-20200615093111128](pic/image-20200615093111128.png)

（2）组件标签命名使用短横线分隔(如“vue-html”)，在vue里不变的使用（“vue-html”）

![image-20200615093125942](pic/image-20200615093125942.png)

（3）vue的组件的props属性支持驼峰命名,不支持连接线命名。所以如果组件标签中props属性命名为“outer-message”,则在vue里使用“outerMessage”与之对应

![image-20200615093140779](pic/image-20200615093140779.png)

（4）如果组件标签中props属性命名使用“驼峰命名法”，如“outerMessage”，在vue里使用“outermessage”

![image-20200615093208584](pic/image-20200615093208584.png)

# **解决闪烁问题**

当网络较慢，网页还在加载 Vue.js ，而导致 Vue 来不及渲染，这时页面就会显示出 Vue 源代码。

‘我们可以使用 **v-cloak** 指令来解决这一问题

css

```css
[v-cloak]{
    display: none;
}
```

html

```html
<div id="app" v-cloak>
    {{context}}
</div>
<script>
    var app = new Vue({
        el: '#app',
        data: {
            context:'互联网头部玩家钟爱的健身项目'
        }
    });
</script>
```



# Vue的生命周期

Vue 实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模板、挂载 DOM、渲染→更新→渲染、卸载等一系列过程，我们称这是 Vue 的生命周期。通俗说就是 Vue 实例从创建到销毁的过程，就是生命周期。

在 Vue 的整个生命周期中，它提供了一系列的事件，可以让我们在事件触发时注册 JS 方法，可以让我们用自己注册的 JS 方法控制整个大局，在这些事件响应方法中的 this 直接指向的是 Vue 的实例。


![image-20200615105553480](pic/image-20200615105553480.png)

![image-20200615110331190](pic/image-20200615110331190.png)

![image-20200615105756566](pic/image-20200615105756566.png)

**下面示例mounted的使用**

# Axios异步通信

## 什么是Axios

Axios 是一个开源的可以用在浏览器端和 NodeJS 的**异步通信框架**，她的主要作用就是实现 **AJAX** 异步通信，其功能特点如下：

从浏览器中创建 XMLHttpRequests
从 node.js 创建 http 请求
支持 Promise API [JS中链式编程]
拦截请求和响应
转换请求数据和响应数据
取消请求
自动转换 JSON 数据
客户端支持防御 XSRF（跨站请求伪造）

中文文档：http://www.axios-js.com/

在线CDN

```html
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
或
<script src="https://cdn.staticfile.org/axios/0.18.0/axios.min.js"></script>
```

## 为什么要使用 Axios

由于 Vue.js 是一个 视图层框架 并且作者（尤雨溪）严格遵守 SoC （关注度分离原则），所以 Vue.js 并不包含 AJAX 的通信功能，为了解决通信问题，作者单独开发了一个名为 vue-resource的插件，不过在进入 2.0 版本以后停止了对该插件的维护并推荐了 Axios 框架。

少用jQuery.ajax，因为它操作Dom太频繁！

## 第一个 Axios 应用程序

1.模拟一段 JSON 数据	myData.json

![image-20200615103417272](pic/image-20200615103417272.png)

```json
{
  "name": "chen",
  "url": "http://182.92.224.175/typecho/",
  "page": 1,
  "address": {
    "street": "南天门",
    "city": "佛山",
    "country": "中国"
  },
  "links": [
    {
      "name": "百度一下",
      "url": "https://www.baidu.com"
    },
    {
      "name": "Axios中文文档",
      "url": "http://www.axios-js.com/zh-cn/docs/vue-axios.html"
    }
  ]
}
```

2.测试

```html
<body>
<div id="app">
    <div>{{info.name}}</div>
    <div>{{info.address.city}}</div>
    <div>链接:<a v-bind:href="info.url">{{info.url}}</a></div>
</div>
    
<script src="js/vue.min.js"></script>
<script src="js/axios.min.js"></script>
<script type="text/javascript">
    //实例化Vue
    var vm=new Vue({
        el:'#app',
        //注意：这里是data()方法，才可以接受并return mounted()响应回来的json数据，不同于之前的data属性！！
        data(){
            return {    //return返回响应回来的json数据
                info:{	//请求的返回参数名必须和json字符串一样
                    name:null,
                    address:{
                        street:null,
                        city:null,
                        country:null,
                    },
                    url:null
                }
            }
        },
        //钩子函数
        mounted(){
            axios.get('myData.json').then(
                response=>(this.info=response.data));	//链式编程
        },
    });
</script>
</body>
```

![image-20200615103528791](pic/image-20200615103528791.png)

# 计算属性

计算属性的重点突出在 属性 两个字上（属性是名词），首先它是个 属性 其次这个属性有 计算的能力（计算是动词），这里的 计算 就是个函数，简单点说，它就是一个能够将计算结果缓存起来的属性（将行为转化成了静态的属性），类似**缓存**！

- methods：定义方法，调用方法使用 currentTime1()，需要带括号
- computed：定义计算属性，调用属性使用 currentTime2，不需要带括号；this.message 是为了能够让 currentTime2 观察到数据变化而变化
- this.message 是为了能够让 currentTime2 观察到数据变化而变化

```html
<body>
<div id="app" >
  <p>currentTime1: {{currentTime1()}}</p>	
  <p>currentTime2: {{currentTime2}}</p><!--调用计算属性使用currentTime2，不需要带括号-->
</div>
<script src="js/vue.min.js"></script>
<script type="text/javascript">
    //实例化Vue
    var vm=new Vue({
        el:'#app',
        data:{
        },
        methods:{
            currentTime1:function(){
                return Date.now();//返回一个时间戳
            }
        },
        computed:{//计算属性：method，computed方法名不要重名，重名之后，优先调用methods的方法
            currentTime2:function(){
                this.message;
                return Date.now();
            }
        }

    });
</script>

</body>
```

![image-20200615112447841](pic/image-20200615112447841.png)

![image-20200615112251778](pic/image-20200615112251778.png)

![image-20200615112351856](pic/image-20200615112351856.png)

![image-20200615113345987](pic/image-20200615113345987.png)

**结论：**

调用方法时，每次都需要进行计算，既然有计算过程则必定产生系统开销，那如果这个结果是不经常变化的呢？此时就可以考虑将这个结果缓存起来，采用计算属性可以很方便的做到这一点

**计算属性的主要特性就是为了将不经常变化的计算结果进行缓存，以节约我们的系统开销**

# 插槽<slot>

在 `Vue.js` 中我们使用 **<slot>** 元素作为承载分发内容的出口，作者称其为插槽，可以应用在组合组件的场景中。

**示例**

准备制作一个待办事项组件（todo），该组件由待办标题（todo-title）和待办内容（todo-items）组成，但这三个组件又是相互独立的，该如何操作呢？

**第一步: 定义一个待办事项的组件**

```html
<div id="app" >
    <todo></todo>
</div>

<script src="js/vue.min.js"></script>
<script type="text/javascript">
    Vue.component('todo',{
    template:'<div>\
                    <div>待办事项</div>\
                    <ul>\
                        <li>学习Vue</li>\
                    </ul>\
               </div>'
    });
</script>
```

**第二步: 留出一个 插槽 让待办事项的标题和值实现动态绑定**

1.修改上面代码

```html
Vue.component('todo', {
    template: '<div>\
                    <slot name="todo-title"></slot>\
                    <ul>\
                        <slot name="todo-items"></slot>\
                    </ul>\
               </div>'
});
```

2.定义一个名为 todo-title 的待办标题组件 和 todo-items 的待办内容组件

```js
Vue.component('todo-title',{
    props:['title'],
    template: '<div>{{title}}</div>'
});
Vue.component('todo-items',{
    props:['item'],
    template: '<li>{{item}}</li>'
});
```

3.实例化 Vue 并初始化数据

```js
var vm=new Vue({
        el:'#app',
        data:{
            title:'课程目录',
            todoItems:['vue','js','python']
        }
    });
```

4.将这些值,通过插槽插入

```html
	<todo>
        <!-- : 是 v-bind: 的简写,绑定vm初始化数据中的title，以便组件todo-title调用-->
        <todo-title slot="todo-title" :title="title"></todo-title>
        <!-- 这里绑定for循环遍历出来的item以便组件todo-items使用！！！-->
        <todo-items slot="todo-items" v-for="item in todoItems" :item="item">	
        </todo-items>
    </todo>
```

![image-20200615121720358](pic/image-20200615121720358.png)



# 自定义事件

以上代码，数据项在 Vue 的实例中，但删除操作要在组件中完成，组件如何才能删除 Vue 实例中的数据呢？此时就涉及到参数传递与事件分发了，Vue 为我们提供了自定义事件的功能很好的帮助我们解决了这个问题，使用 **this.$emit(‘自定义事件名’, 参数)**，操作过程如下:

1.在上面示例中,增加 methods 对象并定义一个名为 removeItems的方法

```js
var vm=new Vue({
        el:'#app',
        data:{
            title:'课程目录',
            todoItems:['vue','js','python']
        },
        methods:{
            // 该方法可以被模板中自定义事件触发
            removeItems:function (index) {
                console.log("删除"+this.todoItems[index]+"成功");
                // 使用splice()方法从数组中删除项目，index为删除项目的位置，1 表示删除的数量
                this.todoItems.splice(index,1);
            }
        }
    });
```

2.修改 todo-items 待办内容组件的代码,增加一个删除按钮,并且绑定事件!

```js
Vue.component('todo-items',{
        props:['item','index'],
        template: '<li>{{index+1}}.{{item}}<button @click="remove_component">删除</button></li>',
        methods: {
            remove_component: function (index) {
                //这里的remove是自定义事件的名称，需要在html中使用v-on:remove的方式指派
                this.$emit('remove', index);
            }
        }
    });
```

3.修改 todo-items 待办内容组件的 HTML 代码,增加一个自定义事件,比如叫 remove,可以和组件的方法绑定,然后绑定到vue的方法中

```html
<!--增加了 v-on:remove="removeItems(index)" 自定义事件，该事件会调用 Vue 实例中定义的名为 removeItems 的方法-->
<todo-items slot="todo-items" v-for="(item,index)
         in todoItems" :item="item" :index="index" :key="index" v-on:remove="removeItems(index)">
</todo-items>
```

![image-20200615220303732](pic/image-20200615220303732.png)![image-20200615220311034](pic/image-20200615220311034.png)

**逻辑理解**

![image-20200615220417493](pic/image-20200615220417493.png)

# Vue 入门小结

核心 : 数据驱动 , 组件化

**常用属性**

```
v-if
v-else-if
v-else
v-for
v-on 绑定事件 , 简写@
v-bind 给组件绑定参数,简写 :
v-model 数据双向绑定
```

**组件化**

- 组合组件 **slot 插槽**
- 组件内部绑定事件需要使用到 **this.$emit("事件名",参数)**
- **计算属性**的特色,缓存计算数据
- 遵循**SoC关注度分离原则**,Vue是纯粹的视图框架,并不包含Ajax之类的通信功能为了解决通信问题,我们需要使用**Axios** 框架做异步通信

说明

Vue的开发都是要基于NodeJS, 实际开发采用 vue-cli脚手架开发,vue-router 路由,vuex做状态管理; Vue UI,界面我们一般使用 ElementUI(饿了么出品),或者ICE(阿里巴巴出品!)来快速搭建前端项目~

# vue-cli 脚手架

## vue-cli简介及环境安装

vue-cli 是官方提供的一个脚手架,用于快速生成一个 vue 的项目模板。

预先定义好的目录结构及基础代码，就像在创建 Maven 项目时可以选择创建一个骨架项目，加速开发。

**主要的功能**

统一的目录结构
本地调试
热部署
单元测试
集成打包上线



**需要的环境**

Node.js : http://nodejs.cn/download/

安装就一直下一步就好,安装在自己的环境目录下

![image-20200615221650449](pic/image-20200615221650449.png)

Git : https://git-scm.com/downloads
镜像:https://npm.taobao.org/mirrors/git-for-windows/

**确认nodejs是否安装成功**

cmd 下输入 node -v , npm-v 查看是否能够正确打印出版本号即可!

![image-20200615222826541](pic/image-20200615222826541.png)
这个npm,就是一个软件包管理工具,和linux下的 apt 软件安装差不多。



**【cmd管理员下操作】**



安装 Node.js 淘宝镜像**加速**器（cnpm）

```cmd
# -g 全局安装
npm install cnpm -g
```

![image-20200615224458521](pic/image-20200615224458521.png)

**安装 vue-cli**

```cmd
cnpm install vue-cli -g
# 测试是否安装成功
# 查看可以基于哪些模板创建vue应用程序，通常我们会选择webpack模板
vue list
```

![image-20200615223634507](pic/image-20200615223634507.png)

## 第一个 vue-cli 应用程序



### vue-liu项目部署及运行

1. 创建一个Vue项目,建立一个空的文件夹
2. 创建一个基于 webpack 模板的 vue 应用程序

```cmd
# 这里的 vue01 是项目名称，可以根据自己的需求起名
vue init webpack vue01
```

![image-20200615225006458](pic/image-20200615225006458.png)	

一路都选择no即可

说明:

- Project name：项目名称，默认 回车 即可
- Project description：项目描述，默认 回车 即可
- Author：项目作者，默认 回车 即可
- Install vue-router：是否安装 vue-router，选择 n 不安装（后期需要再手动添加）
- Use ESLint to lint your code：是否使用 ESLint 做代码检查，选择 n 不安装（后期需要再手动添加）
- Set up unit tests：单元测试相关，选择 n 不安装（后期需要再手动添加）
- Setup e2e tests with Nightwatch：单元测试相关，选择 n 不安装（后期需要再手动添加）
- Should we run npm install for you after the project has been created：创建完成后直接初始化，选择 n，我们手动执行;运行结果!

![image-20200615225122527](pic/image-20200615225122527.png)

**初始化并运行**

修改端口号（可选）

打开项目路径 \vue-study\myvue\config\index.js

![image-20200616104026987](pic/image-20200616104026987.png)

官方提示的安装步骤是这样的，但按这种方式安装一直报错，不推荐使用

```cmd
cd vue01
npm install
npm run dev
```

后面用淘宝镜像加速下载，一步到位，推荐使用这种方式，省得折腾！！！cnpm真香！！！

```cmd
cd vue01
cnpm install
cnpm run dev
```

![image-20200616104114524](pic/image-20200616104114524.png)

执行完成后,目录多了很多依赖

![image-20200616103625928](pic/image-20200616103625928.png)

安装并运行成功后在浏览器访问测试成功！

![image-20200616104134273](pic/image-20200616104134273.png)



### Vue-cli目录结构

我们用IDEA,open打开刚才的项目

![image-20200616104734256](pic/image-20200616104734256.png)



build 和 config：WebPack 配置文件
node_modules：用于存放 npm install 安装的依赖文件
src： **项目源码目录**
static：静态资源文件
.babelrc：Babel 配置文件，主要作用是将 ES6 转换为 ES5
.editorconfig：编辑器配置
eslintignore：需要忽略的语法检查配置文件
.gitignore：git 忽略的配置文件
.postcssrc.js：css 相关配置文件，其中内部的 module.exports 是 NodeJS 模块化语法
index.html：首页，仅作为模板页，实际开发时不使用
package.json：项目的配置文件
name：项目名称
version：项目版本
description：项目描述
author：项目作者
scripts：封装常用命令
dependencies：生产环境依赖
devDependencies：开发环境依赖

**src 目录**

`src` 目录是项目的源码目录，所有代码都会写在这里

![image-20200616110000350](pic/image-20200616110000350.png)

**main.js**

项目的入口文件，我们知道所有的程序都会有一个入口

```js

import Vue from 'vue'	//ES6 写法，会被转换成 require("vue");（require是NodeJS 提供的模块加载器）
import App from './App'	//意思同上，但是指定了查找路径，./ 为当前目录

Vue.config.productionTip = false	//关闭浏览器控制台关于环境的相关提示

new Vue({				//实例化Vue
  el: '#app',			//查找index.html中id为app的元素
  components: { App },	//引入组件，使用的是import App from './App'定义的App组件
  template: '<App/>'	//模板
})
```

**App.vue**

```vue
<template>			<!--template：HTML代码模板-->
  <div id="app">	
    <img src="./assets/logo.png">
    <HelloWorld/>
  </div>
</template>

<script>
//引入HelloWorld组件，用于替换template中的<HelloWorld/>
import HelloWorld from './components/HelloWorld'	

//export导出NodeJS对象，作用是外界可以通过import关键字导入
export default {
  name: 'App',	//定义组件的名称
  components: {
    HelloWorld		//定义子组件
  }
}
</script>

<!--css样式-->
<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```



# Webpack

WebPack 是一款模块加载器兼打包工具，它能把各种资源，如 JS、JSX、ES6、SASS、LESS、图片等都作为模块来处理和使用。

- 主要就是将 ES6 格式的文件打包成 ES5 格式的，适应浏览器，因有些浏览器不支持 ES6 格式

**安装**（cmd管理员下）

```cmd
npm install webpack -g
npm install webpack-cli -g
```

**测试安装成功**

```
webpack -v
webpack-cli -v
```

![image-20200616111823876](pic/image-20200616111823876.png)


 **webpack.config.js 配置文件介绍**

- entry：入口文件，指定 WebPack 用哪个文件作为项目的入口

- output：输出，指定 WebPack 把处理完成的文件放置到指定路径

- module：模块，用于处理各种类型的文件

- plugins：插件，如：热更新、代码重用等

- resolve：设置路径指向

- watch：监听，用于设置文件改动后直接打包

  ```js
  module.exports = {
      entry: "",
      output: {
          path: "",
          filename: ""
      },
      module: {
          loaders: [
              {test: /\.js$/, loader: ""}
          ]
      },
      plugins: {},
      resolve: {},
      watch: true
  }
  ```

  

## 使用webpack

1. 创建一个空文件夹，使用idea打开

2. 创建一个名为 modules 的目录，用于放置 JS 模块等资源文件

3. 在modules下创建模块文件，如 hello.js，用于编写 JS 模块相关代码

   ```js
   //暴露一个方法:sayHi
   exports.sayHi = function () {
     document.write("<div>Hello WebPack</div>");
   };
   ```

4. 在modules下创建一个名为 main.js 的入口文件，用于打包时设置 entry 属性

   ```js
   //require 导入一个模块,就可以调用这个模块中的方法了
   var hello = require("./hello");
   hello.sayHi();
   ```

   **hello.js就像java中的类，main.js实例化它生成一个hello对象，hello对象就可以调用hello.js中的方法了！！！**

5. 在项目目录下创建 webpack.config.js 配置文件，使用 webpack 命令打包

   ```js
   module.exports = {
       entry: "./modules/main.js",
       output: {
           filename: "./js/bundle.js"
       }
   };
   ```

   ![image-20200616115205721](pic/image-20200616115205721.png)

   ![image-20200616115242443](pic/image-20200616115242443.png)

6. 在项目目录下创建 HTML 页面，如 index.html，导入 WebPack 打包后的 JS 文件（bundle.js）

   ![image-20200616115842925](pic/image-20200616115842925.png)

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Title</title>
   </head>
   <body>
       <script src="dist/js/bundle.js"></script>	<!--只需这一步即可-->
   </body>
   </html>
   ```

7. 在IDEA控制台中直接执行webpack;如果失败的话,就使用管理员权限运行即可!

8. 运行 HTML 看效果

   ![image-20200616115930495](pic/image-20200616115930495.png)

**补充**

```cmd
# 参数 --watch 用于监听变化（命令行输入，实现热部署，js文件一变自动重新打包）
# 但不推荐使用，耗费资源
webpack --watch
```

# vue-router路由

Vue Router 是 Vue.js 官方的路由管理器。

它和 Vue.js 的核心深度集成，让构建**单页面应用**变得易如反掌。包含的功能有：

- 嵌套的路由/视图表
- 模块化的、基于组件的路由配置
- 路由参数、查询、通配符
- 基于 Vue.js 过渡系统的视图过渡效果
- 细粒度的导航控制
- 带有自动激活的 CSS class 的链接
- HTML5 历史模式或 hash 模式，在 IE9 中自动降级
- 自定义的滚动条行为
  

**安装**

**基于第一个vue-cli进行测试学习;先查看node_modules中是否存在 vue-router**

vue-router 是一个插件包，需要用 npm/cnpm 来进行安装的。打开命令行工具，进入你的项目目录，输入下面命令

```cmd
cnpm install vue-router --save-dev
```

![image-20200616140635715](pic/image-20200616140635715.png)

如果在一个模块化工程中使用它，必须要通过 Vue.use() 显式地安装路由功能

```js
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter);
```



**测试**

2、`components`目录下存放自己编写的组件
3、定义`Content.vue` 的组件`Main.vue` 

Content.vue

```vue
<template>
  <div>
  <h1>内容页</h1>
  </div>
</template>

<script>
    export default {
        name: "Content"
    }
</script>

<style scoped>
</style>
```

Main.vue

```vue
<template>
      <h1>首页</h1>
</template>

<script>
    export default {
        name: "Main"
    }
</script>

<style scoped>

</style>

```

4、 **安装路由,在src目录下,新建一个文件夹 : router,专门存放路由**

```js
import Vue from 'vue'
// 导入路由插件
import Router from "vue-router"
// 导入自定义的组件
import Content from "../components/Content";
import Main from "../components/Main";

//安装路由，切记！否则报错！
Vue.use(Router);

//配置导出路由
export default new Router({
  routes:[
    {
      //路由路径
      path:'/content',
      //自定义路由名称（可选）
      name:'Content', 
      //跳转到Content组件
      component:Content
    },
    {
      path:'/main',
      name:'main',
      component:Main
    }
  ]
});

```

5、在`main.js` 中配置路由

```js
import Vue from 'vue'
import App from './App'

// 导入上面创建的路由配置目录
import router from './router'

//关闭生产模式下给出的提示
Vue.config.productionTip = false;

new Vue({
  el: '#app',
  //配置路由！！！
  router,
  components: { App },
  template: '<App/>'
})
```

6、在`App.vue`中使用路由

```vue
<template>
  <div id="app">
    <h1>vue-router学习</h1>
<!-- router-link：默认会被渲染成一个 <a> 标签，to 属性为指定链接-->
    <router-link to="/main">首页1</router-link>
    <router-link to="/content">内容页</router-link>
<!--router-view：用于渲染路由匹配到的组件视图-->
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
```

启动测试： **npm run dev**（若没有呈现效果查看浏览器console的错误信息）

![image-20200616153448466](pic/image-20200616153448466.png)



# Vue 结合 elementUI 实战

elementUI中文文档：https://element.eleme.cn/#/zh-CN/component/installation

## **创建工程**

注意： 命令行都要使用管理员模式运行

1、创建一个名为 hello-vue 的工程

```cmd
vue init webpack hello-vue
```

2、安装依赖，我们需要安装 vue-router、element-ui、sass-loader 和 node-sass 四个插件

```cmd
# 进入工程目录
cd hello-vue
# 安装 vue-router
cnpm install vue-router --save-dev
# 安装 element-ui
cnpm i element-ui -S
# 安装依赖
cnpm install
# 安装 SASS 资源加载器（因前端现在不是纯css写，需要安装SASS去编译）
cnpm install sass-loader node-sass --save-dev
# 启动测试
cnpm run dev
```

3、npm命令解释：(用 cnpm 更好 )

- npm install moduleName：安装模块到项目目录下
- npm install -g moduleName：-g 的意思是将模块安装到全局，具体安装到磁盘哪个位置，要看 npm config prefix 的位置
- npm install -save moduleName：–save 的意思是将模块安装到项目目录下，并在 package 文件的 dependencies 节点写入依赖，-S 为该命令的缩写
- npm install -save-dev moduleName：–save-dev 的意思是将模块安装到项目目录下，并在 package 文件的 devDependencies 节点写入依赖，-D 为该命令的缩写
  

## 创建登录页面

idea打开项目，把没有用的初始化东西删掉！

在源码目录中创建如下结构：

- assets：用于存放资源文件
- components：用于存放 Vue 功能组件
- views：用于存放 Vue 视图组件
- router：用于存放 vue-router 配置

![image-20200616163033998](pic/image-20200616163033998.png)

**创建首页视图，在 views 目录下创建一个名为 Main.vue 的视图组件**

```vue
<template>
   <h1>首页</h1>
</template>

<script>
    export default {
        name: "Main"
    }
</script>

<style scoped>

</style>
```

**创建登录页视图在 views 目录下创建一个名为 Login.vue 的视图组件**

**其中 el-\* 的元素为 ElementUI 组件（从elementUI官网复制好看的登录页面过来）**

```vue
<template>
  <div>
    <el-form ref="loginForm" :model="form" :rules="rules" label-width="80px" class="login-box">
      <h3 class="login-title">欢迎登录</h3>
      <el-form-item label="账号" prop="username">
        <el-input type="text" placeholder="请输入账号" v-model="form.username"/>
      </el-form-item>
      <el-form-item label="密码" prop="password">
        <el-input type="password" placeholder="请输入密码" v-model="form.password"/>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" v-on:click="onSubmit('loginForm')">登录</el-button>
      </el-form-item>
    </el-form>

    <el-dialog
      title="温馨提示"
      :visible.sync="dialogVisible"
      width="30%"
      :before-close="handleClose">
      <span>请输入账号和密码</span>
      <span slot="footer" class="dialog-footer">
        <el-button type="primary" @click="dialogVisible = false">确 定</el-button>
      </span>
    </el-dialog>
  </div>
</template>

<script>
  export default {
    name: "Login",
    data() {
      return {
        form: {
          username: '',
          password: ''
        },

        // 表单验证，需要在 el-form-item 元素中增加 prop 属性
        rules: {
          username: [
            {required: true, message: '账号不可为空', trigger: 'blur'}
          ],
          password: [
            {required: true, message: '密码不可为空', trigger: 'blur'}
          ]
        },

        // 对话框显示和隐藏
        dialogVisible: false
      }
    },
    methods: {
      onSubmit(formName) {
        // 为表单绑定验证功能
        this.$refs[formName].validate((valid) => {
          if (valid) {
            // 使用 vue-router 路由到指定页面，该方式称之为编程式导航
            this.$router.push("/main");
          } else {
            this.dialogVisible = true;
            return false;
          }
        });
      }
    }
  }
</script>

<style lang="scss" scoped>
  .login-box {
    border: 1px solid #DCDFE6;
    width: 350px;
    margin: 180px auto;
    padding: 35px 35px 15px 35px;
    border-radius: 5px;
    -webkit-border-radius: 5px;
    -moz-border-radius: 5px;
    box-shadow: 0 0 25px #909399;
  }

  .login-title {
    text-align: center;
    margin: 0 auto 40px auto;
    color: #303133;
  }
</style>
```

**创建路由,在 router 目录下创建一个名为 index.js 的 vue-router 路由配置文件**

```js
import Vue from 'vue'
import VueRouter from "vue-router";

import Main from "../view/Main";
import Login from "../view/Login";

Vue.use(VueRouter);

export default new VueRouter({
  routes:[
    {
        // 首页
      path:'/main',
      component:Main
    },
    {
        // 登录页
      path:'/login',
      component:Login
    },
  ]
})
```

**配置路由**（修改 **main.js** 入口代码）

```js
import Vue from 'vue'
import App from './App'
import router from './router'

//导入ElementUI
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';

// 安装 ElementUI
Vue.use(ElementUI);

new Vue({
  el: '#app',
  //启用路由
  router,
  // 启用 ElementUI，
  render: h => h(App)	//给App.vue渲染ElementUI
})
```

修改 App.vue 组件代码

```vue
<template>
  <div id="app">
      <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App',
}
</script>
```

启动测试： **npm run dev**

如果出现错误: 可能是因为sass-loader的版本过高导致的编译错误，当前最高版本是8.x，需要退回到7.3.1 

![image-20200616162203588](pic/image-20200616162203588.png)

去package.json文件里面的 "sass-loader" 的版本更换成7.3.1

![image-20200616164058375](pic/image-20200616164058375.png)

然后重新 **cnpm install **后运行项目即可！

![image-20200617100837272](pic/image-20200617100837272.png)

# **路由嵌套**

嵌套路由又称子路由，在实际应用中，通常由多层嵌套的组件组合而成。同样地，URL 中各段动态路径也按某种结构对应嵌套的各层组件，例如：

```
/user/foo/profile                     /user/foo/posts
+------------------+                  +-----------------+
| User             |                  | User            |
| +--------------+ |                  | +-------------+ |
| | Profile      | |  +------------>  | | Posts       | |
| |              | |                  | |             | |
| +--------------+ |                  | +-------------+ |
+------------------+                  +-----------------+
```

1、用户信息组件，在 views/user 目录下创建一个名为 Profile.vue 的视图组件；

```vue
<template>
    <div>
      个人信息
    </div>
</template>

<script>
    export default {
        name: "Profile"
    }
</script>

<style scoped>
</style>
```

2、用户列表组件在 views/user 目录下创建一个名为 List.vue 的视图组件；

```vue
<template>
  <div>
    用户列表
  </div>
</template>

<script>
    export default {
        name: "List"
    }
</script>

<style scoped>
</style>
```

3、配置嵌套路由（修改 router 目录下的 index.js 路由配置文件）

说明：主要在路由配置中增加了 children 数组配置，用于在该组件下设置嵌套路由

```js
import Vue from 'vue'
import VueRouter from "vue-router";

import Main from "../view/Main";
import Login from "../view/Login";

// 用于嵌套的路由组件
import Profile from "../view/user/Profile";
import List from "../view/user/List";

Vue.use(VueRouter);

export default new VueRouter({
  routes:[
    {
        //首页
      path:'/main',
      component:Main,
      // 配置嵌套路由！！！！
      children:[
        {path:'/user/profile',component:Profile},
        {path:'/user/list',component:List},
      ]
    },
    {
        //登录页
      path:'/login',
      component:Login
    },
  ]
})
```

4、修改首页视图 Main.vue ，此处使用了 ElementUI 布局容器组件

```vue
<template>
  <div>
    <el-container>
      <el-aside width="200px">
        <el-menu :default-openeds="['1']">
          <el-submenu index="1">
            <template slot="title"><i class="el-icon-caret-right"></i>用户管理</template>
            <el-menu-item-group>
              <el-menu-item index="1-1">
                  <!--这里嵌套，跳转-->
                <router-link to="/user/profile">个人信息</router-link>
              </el-menu-item>
              <el-menu-item index="1-2">
                  <!--这里嵌套，跳转-->
                <router-link to="/user/list">用户列表</router-link>
              </el-menu-item>
            </el-menu-item-group>
          </el-submenu>
          <el-submenu index="2">
            <template slot="title"><i class="el-icon-caret-right"></i>内容管理</template>
            <el-menu-item-group>
              <el-menu-item index="2-1">分类管理</el-menu-item>
              <el-menu-item index="2-2">内容列表</el-menu-item>
            </el-menu-item-group>
          </el-submenu>
        </el-menu>
      </el-aside>

      <el-container>
        <el-header style="text-align: right; font-size: 12px">
          <el-dropdown>
            <i class="el-icon-setting" style="margin-right: 15px"></i>
            <el-dropdown-menu slot="dropdown">
              <el-dropdown-item>个人信息</el-dropdown-item>
              <el-dropdown-item>退出登录</el-dropdown-item>
            </el-dropdown-menu>
          </el-dropdown>
        </el-header>

        <el-main>
          <router-view />
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
  export default {
    name: "Main"
  }
</script>

<style scoped lang="scss">
  .el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
  }

  .el-aside {
    color: #333;
  }
</style>
```

![image-20200616172139610](pic/image-20200616172139610.png)

# 参数传递

例如，我们有一个 User 组件，对于所有 ID 各不相同的用户，都要使用这个组件来渲染，将用户的信息展示出来。此时我们就需要传递参数了。

## **方式一**（使用$route.params接收参数）

1、修改index.js路由配置（在 path 属性中增加:id 这样的占位符获取传过来的值）

```js
{
    path: '/user/profile/:id',
 	name:'Profile',
 	component:Profile
}
```

2、传递参数（Main.vue）

此时我们将 to 改为了 :to，是为了将这一属性当成对象使用，而且使用 ：双向绑定了传的值才能被接收

```vue
<router-link :to="{name:'Profile',params:{id:1}}">个人信息</router-link>
```

【注意】 router-link 中的 name 属性名称 一定要和 路由中的 name 属性名称 匹配，因为这样 Vue 才能找到对应的路由路径。

3、接收参数, 在目标组件 Profile.vue

```vue
<template>
    <div>
<!--route是一个跳转的路由对象，每一个路由都会有一个route对象，是一个局部的对象，可以获取对应的name,path,params,query等-->
      个人信息:{{ $route.params.id }}	
    </div>
</template>
```

![image-20200616175614297](pic/image-20200616175614297.png)



## **方式二	使用 props 的方式**（个人觉得这种更清晰，好用！）

1、修改路由配置 , 主要增加了 props: true 属性

```js
{path:'/user/profile/:id',
  name:'Profile',
  component:Profile,
  props:true		//增加props: true 属性
},
```

2、传递参数和之前一样，不变

```vue
<router-link :to="{name:'Profile',params:{id:1}}">个人信息</router-link>
```

3、接收参数：目标组件增加 props 属性

```vue
<template>
    <div>
      个人信息:{{id}}
    </div>
</template>

<script>
    export default {
      props:['id'],
        name: "Profile"
    }
</script>

<style scoped>
</style>

```



## **实战（登录后首页获取用户名信息）**

1.Login.vue的onSubmit方法中修改跳转路径，携带username参数

```js
onSubmit(formName) {
        this.$refs[formName].validate((valid) => {
          if (valid) {
            this.$router.push("/main/"+this.form.username);		//携带参数this.form.username
          } else {
            this.dialogVisible = true;
            return false;
          }
        });
      }	
```

2.路由index.js中接收参数

```js
export default new VueRouter({
  routes:[
    {
      path:'/main/:name',	//获取传递的参数
      component:Main,
      props:true,		   //增加props: true 属性
      ....
```

3.Main.vue获取name参数并显示到视图

```vue
<script>
  export default {
    props:['name'],		//获取传郭过来的属性值 name
    name: "Main"
  }
</script>
```

```vue
<el-header style="text-align: right; font-size: 12px">
  <el-dropdown>
    <i class="el-icon-setting" style="margin-right: 15px"></i>
    <el-dropdown-menu slot="dropdown">
      <el-dropdown-item>个人信息</el-dropdown-item>
      <el-dropdown-item>退出登录</el-dropdown-item>
    </el-dropdown-menu>
  </el-dropdown>
  <span>{{name}}</span>		<!--将name值显示到页面-->
</el-header>
```

![image-20200616235800950](pic/image-20200616235800950.png)

![image-20200616235820253](pic/image-20200616235820253.png)

# **组件重定向**

重定向的意思大家都明白，Vue 中的重定向是作用在路径不同但组件相同的情况下

**示例**

路由index.js中配置

```js
{
      path: '/main',
      name: 'Main',
      component: Main
    },
    {
      path: '/goHome',
      redirect: '/main'		//访问 /goHome 重定向到 /main
    }
```

Main.vue 设置对应路径即可

```vue
<el-menu-item index="1-3">
          <router-link to="/goHome">回到首页</router-link>
</el-menu-item>
```

# 路由模式

路由模式有两种

- hash：路径带 # 符号，如 http://localhost/#/login
- history：路径不带 # 符号，如 http://localhost/login

修改路由配置，代码如下

```js
export default new VueRouter({
  mode:'history',		//修改路由模式mode为history
  routes:[
      ......
    ]
});   
```

![image-20200617000032509](pic/image-20200617000032509.png)



# **处理 404**

view下创建一个名为 `NotFound.vue` 的视图组件

```vue
<template>
    <div>
      <h1>404，你的页面走丢了！！！</h1>
    </div>
</template>

<script>
    export default {
        name: "NotFound"
    }
</script>

<style scoped>
</style>
```

修改 index.js 路由配置 

```js
import NotFound from '../views/NotFound'		//导入组件

Vue.use(VueRouter);

export default new VueRouter({
  routes:[
    ......
      //配置404路由
    ,{
      path: '*',  // *：匹配不到的走这里
      component: NotFound
    }
  ]
})
```

![image-20200617000640451](pic/image-20200617000640451.png)

# 路由钩子与异步请求

`beforeRouteEnter`：在进入路由前执行
`beforeRouteLeave`：在离开路由前执行

## **示例1 路由钩子类似java过滤器**

**(to, from, next)** 就像 java 过滤器中的 **(request,response,chain)**

```js
export default {
    props: ['id'],
    name: "UserProfile",
    beforeRouteEnter: (to, from, next) => {
      console.log("准备进入个人信息页");
      next();
    },
    beforeRouteLeave: (to, from, next) => {
      console.log("准备离开个人信息页");
      next();
    }
  }
```

**参数说明**

- to：路由将要跳转的路径信息
- from：路径跳转前的路径信息
- next：路由的控制参数
- next() 跳入下一个页面
- next(’/path’) 改变路由的跳转方向，使其跳到另一个路由
- next(false) 返回原来的页面
- next((vm)=>{}) 仅在 beforeRouteEnter 中可用，vm 是组件实例（**后面示例使用**）

![image-20200617001834782](pic/image-20200617001834782.png)

## **在钩子函数中使用异步请求**

1、安装 Axios （若安装失败重新安装即可）

```cmd
cnpm install --save vue-axios
```

2、`main.js`引用 Axios（可查看Axios官网）

```js
import axios from 'axios'
import VueAxios from 'vue-axios'

Vue.use(VueAxios, axios)
```

3、模拟数据 ： 只有我们的 static 目录下的文件是可以被浏览器直接访问到的，所以我们就把静态文件放入该目录下

​	静态数据存放的位置 ：**static/mock/data.json**

```json
{
  "name": "chen",
  "url": "http://182.92.224.175/typecho/",
  "page": 1,
  "address": {
    "street": "南天门",
    "city": "佛山",
    "country": "中国"
  },
  "links": [
    {
      "name": "百度一下",
      "url": "https://www.baidu.com"
    },
    {
      "name": "Axios中文文档",
      "url": "http://www.axios-js.com/zh-cn/docs/vue-axios.html"
    }
  ]
}
```

测试是否能访问到

![image-20200617003900658](pic/image-20200617003900658.png)

**4、在 beforeRouteEnter 中进行异步请求**

```vue
<script>  
export default {
      props:['id'],
      name: "Profile",
      beforeRouteEnter:(to,from,next)=>{
        console.log("准备进入个人信息页");
        // 注意，一定要在 next 中请求，因为该方法调用时 Vue 实例还没有创建，此时无法获取到 this 对象，在这里使用官方提供的回调函数拿到当前实例
        next(vm => {
          vm.getData();
        });
      },
      beforeRouteLeave:(to,from,next)=>{
        console.log("准备离开个人信息页");
        next();
      },
    //编写获取json数据的方法供钩子函数调用
      methods:{
        getData:function () {
          this.axios({
            method:'get',
            url:'http://localhost:8080/static/mock/data.json',
          }).then(function (repos) {
            console.log(repos);
          }).catch(function (error) {
            console.log(error);
          })
        }
      }
    }
</script>

<style scoped>

</style>

```

点击个人信息，查看控制台输出

![image-20200617004155633](pic/image-20200617004155633.png)