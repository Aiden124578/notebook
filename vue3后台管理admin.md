## vue3后台管理admin

原创 王大合 [王大合](javascript:void(0);) *1月15日*

收录于话题

\#vue3.0

3个

```

```



# vue3后台管理admin

## 1创建项目以及vue-router4

### 1.1-使用vite或者vuecli4创建项目

```
# 或者使用yarn
yarn add -g create-vite-app
yarn create vite-app <project-name>
#yarn初始化packeage依赖,并且运行
yarn 
yarn dev
```

安装依赖

我需要使用

| 依赖库包   |
| ---------- |
| vue-cli4   |
| vuex       |
| vue-router |
| less       |

##### 引入typescript

```
# 安装 typescript

yarn add typescript -D
```

初始化`tsconfig.json`

```
# 然后在控制台执行下面命令
npx tsc --init
```

使用vuecli4创建vue3项目

```
vue create 'project'
```



### 1.2-vue-router 4



##### 安装vue-router

```
yarn add vue-router@next
```



##### 了解下vue-router 4

Vue Router 4引入了 `createRouter` API，可以创建一个可与Vue 3一起安装的路由器实例。

```
import { createRouter } from "vue-router";

export default createRouter({
routes: [...],
});
```

##### History选项

在之前的Vue Router版本中，你可以设置 `mode` 选项设置导航的模式。

`hash` 模式利用URL hash来模拟完整的URL，这样当URL发生变化时，页面不会被重新加载。`history` 利用HTML5 History API来实现URL导航，而不需要重新加载页面。

src/router/index.js

```
// Vue Router 3
const router = new VueRouter({
 mode: "history",
 routes: [...]
});
```

在Vue Router 4中，这些模式已被抽象到模块中，可以将其导入并分配给新的 `history` 选项。这种额外的灵活性使你可以根据需要自定义路由历史记录的实现。

src/router/index.js

```
// Vue Router 4
import { createRouter, createWebHistory } from "vue-router";

export default createRouter({
 history: createWebHistory(),
 routes: [],
});
```

Vue.js 3.0下vue-router 4.0 hash模式

```
import { createRouter, createWebHashHistory } from 'vue-router';
const router = createRouter({
history: createWebHashHistory(),
routes
});
```

Vue.js 3.0下vue-router 4.0 history模式

```
import { createRouter, createWebHistory} from 'vue-router';
const router = createRouter({
history: createWebHistory(process.env.BASE_URL),
routes
});
```



##### 配置vue-router

在项目`src`目录下面新建`router`目录，然后添加`index.ts`文件，在文件中添加以下内容

```
import {createRouter, createWebHashHistory} from 'vue-router'

// 在 Vue-router新版本中，需要使用createRouter来创建路由
export default createRouter({
// 指定路由的模式,此处使用的是hash模式
history: createWebHashHistory(),
// 路由地址
routes: []
})
```

在main.ts中配置添加即可

```
import { createApp } from 'vue'
import App from './App.vue'
import './index.css'
import router from './router/index'


const app = createApp(App)

app.use(router)
app.mount('#app')
```



## 2 ant design vue

### 2.1-ant design vue

vue2 element ui

element3 & element plus

安装ant2.0 ui组件

```
yarn add ant-design-vue@next -s
```

完整引用ant在main.ts中引入组件

```
import { createApp } from 'vue'
import App from './App.vue'
import './index.css'
import router from './router/index'
import Antd from 'ant-design-vue';
import 'ant-design-vue/dist/antd.css';

const app = createApp(App)


app.use(Antd)
app.use(router)
app.mount('#app')
```

### 2.2-babel-plugin-import 按需引用

![图片](D:/%E7%AC%94%E8%AE%B0/images/640)



如果你在开发环境的控制台看到下面的提示，那么你可能使用了 `import { Button } from 'ant-design-vue';` 的写法引入了 antd 下所有的模块，这会影响应用的网络性能

安装

```
yarn add babel-plugin-import -D
```

然后，将 .babel.config.js 修改为：

```
module.exports = {
 presets: [
   '@vue/cli-plugin-babel/preset'
],
 "plugins": [
  [
       "import",
      {libraryName: "ant-design-vue", libraryDirectory: "es", style:"css"}
  ]
]
}
```

然后在main.ts按照需求引入组件

```
import { createApp } from 'vue'
import App from './App.vue'
import router from './router/index'
import {Button} from 'ant-design-vue';
import 'ant-design-vue/dist/antd.css';

const app = createApp(App)
app.config.productionTip = false;

app.use(Button)
app.use(router)
app.mount('#app')
```

### 2.3-将ant组件封装

如果引入的组件比较多,会影响main.ts的优雅

我对A必组件进行封装

在src目录下新建 ant-design-vue/index.js的目录

```
import { Form, Input, Button } from 'ant-design-vue';
import 'ant-design-vue/dist/antd.css';

export function setupAntd(app) {
  app.use(Button)
  app.use(Form);
  app.use(Input);
}
```

ant-design-vue/index.js封装

```
import { Form, Input, Button } from 'ant-design-vue';
import 'ant-design-vue/dist/antd.css';

const components= [
  Form,
  Input,
  Button
]
export function setupAntd(app) {
  components.forEach(component => {
      app.use(component)
    })
}
```

然后在main.ts当中引入setupAntd即可

```
import { createApp } from 'vue'
import App from './App.vue'
import router from './router/index'
import {setupAntd} from './ant-design-vue'


const app = createApp(App)


setupAntd(app)
app.use(router)
app.mount('#app')
```



## 3-首页layout布局

### 3.1-layout布局介绍

![图片](D:/%E7%AC%94%E8%AE%B0/images/640)

### 3.2 -Ant 来实现layout骨架

1.index.vue为首页,安装下less 和less loader

```
yarn add less less-loader -D
```

2.配置ant 组件 layout menu

```
import { Form, Input, Button, Layout, Menu } from 'ant-design-vue';
import 'ant-design-vue/dist/antd.css';

const components= [
  Form,
  Input,
  Button,
  Layout,
  Menu
]
export function setupAntd(app) {
  components.forEach(component => {
      app.use(component)
    })
}
```

3.先行适配desktop端

```
<template>
<a-layout class="app-wapper">
  <a-layout-sider v-model:collapsed="collapsed" class="app-sider">
    <div class="logo" />
    <a-menu theme="dark" mode="inline" v-model:selectedKeys="selectedKeys">
      <a-menu-item key="1">
        <user-outlined />
        <span>nav 1</span>
      </a-menu-item>
      <a-menu-item key="2">
        <video-camera-outlined />
        <span>nav 2</span>
      </a-menu-item>
      <a-menu-item key="3">
        <upload-outlined />
        <span>nav 3</span>
      </a-menu-item>
    </a-menu>
  </a-layout-sider>
  <a-layout >
    <a-layout-header class="app-header">
      <menu-unfold-outlined
        v-if="collapsed"
        class="trigger"
        @click="() => (collapsed = !collapsed)"
      />
      <menu-fold-outlined v-else class="trigger" @click="() => (collapsed = !collapsed)" />
    </a-layout-header>
    <a-layout-content
      :style="{ margin: '24px 16px', padding: '24px', background: '#fff', minHeight: '480px' }"
    >
      Content
    </a-layout-content>
  </a-layout>
</a-layout>
</template>
<script>
import {
UserOutlined,
VideoCameraOutlined,
UploadOutlined,
MenuUnfoldOutlined,
MenuFoldOutlined,
} from '@ant-design/icons-vue';
import {reactive,toRefs} from 'vue'
export default {
components: {
  UserOutlined,
  VideoCameraOutlined,
  UploadOutlined,
  MenuUnfoldOutlined,
  MenuFoldOutlined,
},
setup() {
  const state = reactive({
    selectedKeys: ['1'],
    collapsed: false,
  })

  return {
    ...toRefs(state)
  }
}
};
</script>
<style lang="less">
.app-wapper {
.app-sider {
   
  left: 0;
  height: 100vh;
  overflow: auto;
}
.app-header{
  padding: 0;
  background: #fff;
    .trigger {
    font-size: 18px;
    line-height: 64px;
    padding: 0 24px;
    cursor: pointer;
    transition: color 0.3s;
    }
}

}



</style>
```



桌面端就此搞定





\###

### 3.3-引用iconfont图标

引入iconfont

```
import { createFromIconfontCN } from '@ant-design/icons-vue';

const IconFont = createFromIconfontCN({
scriptUrl: '//at.alicdn.com/t/font_1617479_lw8fprdjcs.js',
});


<icon-font type="icon-user" />  //使用方式
```



## 4 登录模块

后台管理系统，需要实现导航菜单从后台拉取的效果；根据登录用户的权限不同分别拉出来的导航菜单也不一样，另外可操作的界面也存在区别

如果要完成权限 登录 查询表单

必须要有后端接口或者自己mock数据

首先安装mockjs和axios

```
yarn add mockjs -S
yarn add axios -S
```









登录接口

### 4.1-用户登录的简单实现思路

1、第一次登录的时候，前端调后端的登陆接口，发送用户名和密码

2、后端收到请求，验证用户名和密码，验证成功，就给前端返回一个token

3、前端拿到token，将token存储到localStorage和vuex中，并跳转路由页面

4、前端每次跳转路由，就判断 localStroage 中有无 token ，没有就跳转到登录页面，有则跳转到对应路由页面

5、每次调后端接口，都要在请求头中加token

6、后端判断请求头中有无token，有token，就拿到token并验证token，验证成功就返回数据，验证失败（例如：token过期）就返回401，请求头中没有token也返回401



创建Mock文件夹

创建mockjs

```
import Mock from 'mockjs'

Mock.mock(
  '/api/login',
  'post',
  (req) => {
      const { password, username } = JSON.parse(req.body)
      if (username === 'admin' && password === '123456') {
          return {
            success: true
          }
        } else {
          return {
            success: false
          }
        }
  }
)
```

引入axios

创建request.js

```
import axios from 'axios'

const instance = axios.create({
  baseURL: '/api/',
  
   
})


export default instance;
```

创建index.js

```
import instance from './request'

export const Login = (res) =>{
  return instance({
      url:'/login',
      method: 'POST',
      data:res

  }).then(response =>{
      console.log(response.data)
      if (response.data.success) { 

      } else {

      }
  })
} 

export default {Login}
```



创建 views文件夹



同时将共有界面error部分创建好

403报错

```
<template>
<div class="page-container">
  <div>
    <h1>403</h1>
    <h1>当前帐号没有操作角色,请联系管理员</h1>
    <router-link to="/" class="ant-btn ant-btn-primary">回到首页</router-link>
  </div>
  <img src="~@/assets/images/error/403.png" alt="">
</div>
</template>

<script >
import {defineComponent} from 'vue'

export default defineComponent({
name: "404",
})
</script>

<style lang="less" scoped>
.page-container {
width: 100vw;
height: 100vh;
display: flex;
justify-content: center;
align-items: center;
background-color: white;
}
</style>
```

404报错

```
<template>
<div class="page-container">
  <div>
    <h1>404</h1>
    <h1>当前页面不存在...</h1>
    <router-link to="/" class="ant-btn ant-btn-primary">回到首页</router-link>
  </div>
  <img src="~@/assets/images/error/404.png" alt="">
</div>
</template>

<script >
import {defineComponent} from 'vue'

export default defineComponent({
name: "404",
})
</script>

<style lang="less" scoped>
.page-container {
width: 100vw;
height: 100vh;
display: flex;
justify-content: center;
align-items: center;
background-color: white;
}
</style>
```

路由改造为

```
import {createRouter, createWebHashHistory} from 'vue-router'

export const constantRoutes = [
{
  path: '/login',
  component: () => import('@/views/login/Login.vue'),
  hidden: true,
},
{
  path: '/403',
  name: '403',
  component: () => import('@/views/error/403.vue'),
  hidden: true,
},
{
  path: '/404',
  name: '404',
  component: () => import('@/views/error/404.vue'),
  hidden: true,
},
]



const router = createRouter({
history: createWebHashHistory(),
routes: constantRoutes,
})

export default router
```



### 4.2 -登录页面form提交

```
<template>
<div class="login-container">
  <a-row>
    <a-col :xs="0" :md="0" :sm="12" :lg="14" :xl="16"></a-col>
    <a-col :xs="24" :sm="24" :md="12" :lg="10" :xl="6">
      <div class="login-container-form">
        <header>
          <img src="@/assets/images/logo.png" />
          <h1>vue3-admin</h1>
        </header>
        <a-form
          :model="form"
          @submit="handleSubmit"
          @submit.prevent
           
        >
          <a-form-item>
            <a-input
              v-model:value="form.username"
              size="large"
              placeholder="Username"
            >
              <template v-slot:prefix><user-outlined type="user" /></template>
            </a-input>
          </a-form-item>
          <a-form-item>
            <a-input
              v-model:value="form.password"
              size="large"
              type="password"
              placeholder="Password"
            >
              <template v-slot:prefix><lock-outlined type="user" /></template>
            </a-input>
          </a-form-item>
          <a-form-item>
            <a-button type="primary" size="large" @click="handleSubmit" block> Login in </a-button>
          </a-form-item>
        </a-form>
      </div>
    </a-col>
  </a-row>
</div>
</template>
<script>
import { UserOutlined, LockOutlined } from "@ant-design/icons-vue";
import {message} from 'ant-design-vue'
import { defineComponent, reactive, toRefs } from "vue";
import {Login} from '../../../mock/index'
export default defineComponent({














name: "Login",
components: {
  UserOutlined,
  LockOutlined,
},

setup() {
  const state = reactive({
    form: {
      username: "",
      password: "",
    },
  });
  const handleSubmit = async () => {
      const {username, password} = state.form
      if(username.trim() == '' || password.trim() == '') return message.warning('用户名或密码不能为空！')
      const res = await Login(state.form)
      console.log(res)
  };

  /* 返回 */
  return {
    ...toRefs(state),
    handleSubmit,
  };
},
});
</script>
<style lang="less" scoped>
.login-container {
height: 100vh;
background: url("~@/assets/images/login/login_bg.png");
background-size: cover;
&-form {
  width: calc(100% - 40px);
  height: 380px;
  padding: 4vh;
  margin-top: calc((100vh - 380px) / 2);
  margin-right: 20px;
  margin-left: 20px;
  background: url("~@/assets/images/login/login_form.png");
  background-size: 100% 100%;
  border-radius: 10px;
  box-shadow: 0 2px 8px 0 rgba(7, 17, 27, 0.06);
  background-color: #fff;
   
  header {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 20px;
    img {
      display: inline-block;
      width: 40px;
    }

    h1 {
      margin-bottom: 0;
      font-size: 24px;
      color: #222;
      text-align: center;
    }
  }

  form {
    display: flex;
    align-items: center;
    flex-direction: column;
    width: 100%;
    margin-top:40px;
  }
  
}
}
</style>
```



### 4.3- localstorage获取token值

了解下localstorage

对浏览器来说，使用 Web Storage 存储键值对比存储 Cookie 方式更直观，而且容量更大，它包含两种：localStorage 和 sessionStorage

1. sessionStorage（临时存储） ：为每一个数据源维持一个存储区域，在浏览器打开期间存在，包括页面重新加载
2. localStorage（长期存储） ：与 sessionStorage 一样，但是浏览器关闭后，数据依然会一直存在

所以上次使用cookie的时候就遇到了一个坑,设置后马上访问session会获取不到,蛋疼,还需要刷新一下,原因是:

当我们首次访问设置Cookie的页面时，服务器会把设置的Cookie值通过响应头发送过来，告诉浏览器将cookie存储的本地相应文件夹中（注意:第一次访问时本地还没有存储Cookie,所以此时获取不到值）;

当第二次访问(或在进行cookie设置后,过期前所有的访问)时，请求头信息你中都会把Cookie值携带。(百度到的,暂时还没理解透彻,先搬过来).

#### 使用方法

注意:sessionStorage 和 localStorage 的用法基本一致，引用类型的值要转换成JSON,所以这里就只列举localStorage



##### 1 保存

```
//对象

const info = { name: 'hou', age: 24, id: '001' };

//字符串

const str="haha";

 

localStorage.setItem('hou', JSON.stringify(info));

localStorage.setItem('zheng', str);
```

　　



##### 2 获取

```
var data1 = JSON.parse(localStorage.getItem('hou'));

var data2 = localStorage.getItem('zheng');
```

　　



##### 3 删除

```
//删除某个   localStorage.removeItem(``'hou'``);

//删除所有   localStorage.clear();
```



**mock/mock.js**

```
import Mock from 'mockjs'

Mock.mock(
  '/api/login',
  'post',
  (req) => {
      const { password, username } = JSON.parse(req.body)
      if (username === 'admin' && password === '123456') {
          return {
            code : 200,
            token:'admin-token'
          }
        } else {
          return {
            success: false
          }
        }
  }
)
```

**新建api,并且创建request.js和user.js**

**request.js**

```
import axios from 'axios'

const instance = axios.create({
  baseURL: '/api/',
  
   
})


export default instance;
```

**user.js**

```
import instance from './request'

export const Login = (res) =>{
  return instance({
      url:'/login',
      method: 'POST',
      data:res

  })
} 

export default {Login}
```



**login.vue**

```
<!-- -->
<template>
<div class="login-container">
  <a-row>
    <a-col :xs="0" :md="0" :sm="12" :lg="14" :xl="16"></a-col>
    <a-col :xs="24" :sm="24" :md="12" :lg="10" :xl="6">
      <div class="login-container-form">
        <header>
          <img src="@/assets/images/logo.png" />
          <h1>vue3-admin</h1>
        </header>
        <a-form :model="form" @submit="handleSubmit" @submit.prevent>
          <a-form-item>
            <a-input
              v-model:value="form.username"
              size="large"
              placeholder="Username"
            >
              <template v-slot:prefix><user-outlined type="user" /></template>
            </a-input>
          </a-form-item>
          <a-form-item>
            <a-input
              v-model:value="form.password"
              size="large"
              type="password"
              placeholder="Password"
            >
              <template v-slot:prefix><lock-outlined type="user" /></template>
            </a-input>
          </a-form-item>
          <a-form-item>
            <a-button type="primary" size="large" @click="handleSubmit" block>
              Login in
            </a-button>
          </a-form-item>
        </a-form>
      </div>
    </a-col>
  </a-row>
</div>
</template>

<script>
import { UserOutlined, LockOutlined } from "@ant-design/icons-vue";
import {defineComponent,reactive,toRefs} from 'vue'
import {message} from 'ant-design-vue'
import {Login} from '../../api/user'
export default defineComponent ({
  name:'Login',
  components:{
    UserOutlined,
      LockOutlined  
  },

  setup() {
      const state = reactive({
          form:{
              username:"",
              password:""
          }
      })

  const handleSubmit = async() =>{
      const {username,password} = state.form
      if(username.trim() == '' || password.trim() == '') return message.warning('用户名和密码不能为空')
      const res = await Login(state.form)
      console.log(res)
      if(res.data.code == 200) {
        localStorage.setItem('token', JSON.stringify(res.data.token));
      }
       
  }

      /* 返回 */
      return {
          ...toRefs(state),
          handleSubmit 
      }
  }

});
</script>
<style lang='less' scoped>
.login-container {
height: 100vh;
background: url("~@/assets/images/login/login_bg.png");
background-size: cover;
&-form {
  width: calc(100% - 40px);
  height: 380px;
  padding: 4vh;
  margin-top: calc((100vh - 380px) / 2);
  margin-right: 20px;
  margin-left: 20px;
  background: url("~@/assets/images/login/login_form.png");
  background-size: 100% 100%;
  border-radius: 10px;
  box-shadow: 0 2px 8px 0 rgba(7, 17, 27, 0.06);
  background-color: #fff;

  header {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 20px;
    img {
      display: inline-block;
      width: 40px;
    }

    h1 {
      margin-bottom: 0;
      font-size: 24px;
      color: #222;
      text-align: center;
    }
  }

  form {
    display: flex;
    align-items: center;
    flex-direction: column;
    width: 100%;
    margin-top: 40px;
  }
}
}
</style>
```

　　



### 4.4 vuex安装引入

\#

**安装和引入vuex**

首先,在vuex中设置状态值 判定是否登录

```
yarn add vuex@next -S
```

创建store文件夹

新建index.js

```
import { createStore } from 'vuex'

export default createStore({

})
```

在main.js当中引入即可

```
import { createApp } from 'vue'
import App from './App.vue'
import router from './router/index'
import {setupAntd} from './ant-design-vue'
import '../mock/mock'
import store from './store'

const app = createApp(App)

setupAntd(app)
app.use(store)
app.use(router)
app.mount('#app')
```

### 4.5 actions 的LoginResult

store/index.js

```
import { createStore } from 'vuex'
import {Login} from '@/api/user'

export default createStore({
  state:{
      token:''
  },
   
  mutations:{
      SET_TOKEN: (state, token) => {
          state.token = token
      },
  },
   
  actions: {
      LoginResult({commit}, userInfo) {
          return new Promise((resolve,reject) =>{
              Login(userInfo).then(response => {
                  const {token, code} = response.data
                  if(code == 200) {
                      localStorage.setItem('token',token)
                      commit('SET_TOKEN',token)
                  }
                  resolve(response.data)
              }).catch(error =>{
                  reject(error)
              })
          })
      }
  }
})
```

login.vue

```
<!-- -->
<template>
<div class="login-container">
  <a-row>
    <a-col :xs="0" :md="0" :sm="12" :lg="14" :xl="16"></a-col>
    <a-col :xs="24" :sm="24" :md="12" :lg="10" :xl="6">
      <div class="login-container-form">
        <header>
          <img src="@/assets/images/logo.png" />
          <h1>vue3-admin</h1>
        </header>
        <a-form :model="form" @submit="handleSubmit" @submit.prevent>
          <a-form-item>
            <a-input
              v-model:value="form.username"
              size="large"
              placeholder="Username"
            >
              <template v-slot:prefix><user-outlined type="user" /></template>
            </a-input>
          </a-form-item>
          <a-form-item>
            <a-input
              v-model:value="form.password"
              size="large"
              type="password"
              placeholder="Password"
            >
              <template v-slot:prefix><lock-outlined type="user" /></template>
            </a-input>
          </a-form-item>
          <a-form-item>
            <a-button type="primary" size="large" @click="handleSubmit" block>
              Login in
            </a-button>
          </a-form-item>
        </a-form>
      </div>
    </a-col>
  </a-row>
</div>
</template>

<script>
import { UserOutlined, LockOutlined } from "@ant-design/icons-vue";
import {defineComponent,reactive,toRefs} from 'vue'
import {message} from 'ant-design-vue'
import {useStore} from 'vuex'
export default defineComponent ({
  name:'Login',
  components:{
    UserOutlined,
      LockOutlined  
  },

  setup() {
      const state = reactive({
          form:{
              username:"",
              password:""
          }
      })

  const store = useStore()

  const handleSubmit = async() =>{
      const {username,password} = state.form
      if(username.trim() == '' || password.trim() == '') return message.warning('用户名和密码不能为空')
      const res = await store.dispatch('LoginResult', state.form)
      console.log(res)
      console.log(store.state.token)
       
  }

      /* 返回 */
      return {
          ...toRefs(state),
          handleSubmit 
      }
  }

});
</script>
<style lang='less' scoped>
.login-container {
height: 100vh;
background: url("~@/assets/images/login/login_bg.png");
background-size: cover;
&-form {
  width: calc(100% - 40px);
  height: 380px;
  padding: 4vh;
  margin-top: calc((100vh - 380px) / 2);
  margin-right: 20px;
  margin-left: 20px;
  background: url("~@/assets/images/login/login_form.png");
  background-size: 100% 100%;
  border-radius: 10px;
  box-shadow: 0 2px 8px 0 rgba(7, 17, 27, 0.06);
  background-color: #fff;

  header {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 20px;
    img {
      display: inline-block;
      width: 40px;
    }

    h1 {
      margin-bottom: 0;
      font-size: 24px;
      color: #222;
      text-align: center;
    }
  }

  form {
    display: flex;
    align-items: center;
    flex-direction: column;
    width: 100%;
    margin-top: 40px;
  }
}
}
</style>
```

首页跳转登录页面



### 4.6 beforeEach首页路由守卫

outer.beforeEach((to,from,next)=>{})

to,是将要跳转的路由，

from，是离开的路由

next是个方法，通往下一个钩子函数,判断to.path 或者 from.path ,如果符合条件，则允许跳转

在router文件夹下的index.js

```
export const constantRoutes = [
{
  path: '/',
  component: Layout,
  redirect: '/index',
  meta: {
    title: '首页',
    affix: true,
  },
  children: [
    {
      path: 'index',
      name: 'Index',
      component: () => import('@/views/index'),
      meta: {
        title: '首页',
        affix: true,
      },
    },
  ],
},

{
  path: '/login',
  name: 'login',
  component: () => import('@/views/login/Login.vue'),
  hidden: true,
},
{
  path: '/403',
  name: '403',
  component: () => import('@/views/error/403.vue'),
  hidden: true,
},
{
  path: '/404',
  name: '404',
  component: () => import('@/views/error/404.vue'),
  hidden: true,
},
]
router.beforeEach((to, from, next) => {
if (to.path === '/login') {
    next();
  } else {
    let token = localStorage.getItem('token');
  
    if (!token) {
      next('/login');
    } else {
      next();
    }
  }
 
})
```

### 4.7 header用户信息模块

header/index.vue

```
<template>
<div class="app-avatar">
  <a-dropdown>
    <span class="ant-dropdown-link">
      <a-avatar :src="avatar" />
      {{ username }}
      <DownOutlined />
    </span>
    <template v-slot:overlay>
      <a-menu>
        <a-menu-item @click="Logout">退出登录</a-menu-item>
      </a-menu>
    </template>
  </a-dropdown>
</div>
</template>

<script>
import { DownOutlined } from "@ant-design/icons-vue";
import { useStore } from "vuex";
import {useRouter,useRoute} from 'vue-router'
import { reactive, toRefs} from "vue";

export default {
name: "AppAvatar",
components: { DownOutlined },
setup() {

  const store = useStore();
  const router = useRouter()
  const route = useRoute()

  const state = reactive({
    avatar: store.getters.avatar,
    username: store.getters.username,
  });

 

  return {
    ...toRefs(state),
    
  };
},
};
</script>
<style lang="less">
.app-avatar {
.ant-dropdown-link {
  display: block;
  min-height: 64px;
  cursor: pointer;
}
}
</style>
```

logo/index.vue

```
<template>
<div class="logo">
  <img src="~@/assets/logo.png" alt="">
  <h2 v-show="!collapsed" class="title">vue3-admin</h2>
</div>
</template>

<script>
export default {
name: "logo",
props: {
  collapsed: {
    type: Boolean
  }
}
}
</script>

<style lang="less" scoped>
.logo {
display: flex;
align-items: center;
padding-left: 24px;
height: 64px;
line-height: 64px;
overflow: hidden;
white-space: nowrap;

img {
  height: 32px;
  margin-right: 8px;
}

.title {
  color: white;
  margin-bottom: 0;
}
}
</style>
```

layout/index.vue

```
<!-- -->
<template>
<a-layout class="app-wapper">
  <!-- 左sider -->
  <a-layout-sider 
  v-model:collapsed="collapsed" :trigger="null" collapsible
  class="app-sider" >
    <!-- logo -->
    <app-logo :collapsed="collapsed" />
      <a-menu theme="dark" mode="inline" v-model:selectedKeys="selectedKeys">
      <a-menu-item key="1">
        <icon-font type ="icon-home-fill" />
        <span>nav 1</span>
      </a-menu-item>
      <a-menu-item key="2">
        <icon-font type ="icon-dianying" />
        <span>nav 2</span>
      </a-menu-item>
      <a-menu-item key="3">
        <icon-font type = "icon-shengyin" />
        <span>nav 3</span>
      </a-menu-item>
    </a-menu>

  </a-layout-sider>
  <!-- 右main -->
  <a-layout>
    <!-- header -->
    <a-layout-header class="app-header">
      <a-row>
        <a-col>
          <menu-unfold-outlined
            v-if="collapsed"
            class="trigger"
            @click="() => (collapsed = !collapsed)"
          />
          <menu-fold-outlined
            v-else
            class="trigger"
            @click="() => (collapsed = !collapsed)"
          />
        </a-col>
        <a-col>
          <!-- avatar -->
          <app-header />
        </a-col>
      </a-row>
       
    </a-layout-header>
    <!-- content -->
    <a-layout-content :style="{ margin: '24px 16px', padding: '24px', background: '#fff', minHeight: '280px' }"> 
      Content
    </a-layout-content>
  </a-layout>
</a-layout>
</template>

<script>
import {

MenuUnfoldOutlined,
MenuFoldOutlined,
} from '@ant-design/icons-vue';
import IconFont from '../iconfont'
import AppLogo from './logo/index.vue'
import AppHeader from './header/index.vue'
import {reactive,toRefs} from 'vue'
export default {
components: {
  IconFont,
  MenuUnfoldOutlined,
  MenuFoldOutlined,
  AppLogo,
  AppHeader,

},

setup() {
  const state = reactive({
    collapsed:false,
    selectedKeys:['1']
  })
   

  return {
    ...toRefs(state)
  }
}
};
</script>

<style lang="less">
.app-wapper {
  .app-sider {
   
  left: 0;
  height: 100vh;
  overflow: auto;
}
 
.app-header{
  padding: 0;
  background: #fff;
    .trigger {
    font-size: 18px;
    line-height: 64px;
    padding: 0 24px;
    cursor: pointer;
    transition: color 0.3s;
    }
    .ant-row {
      display: flex;
      justify-content: space-between;
      padding:0 16px
    }
}

}  
 
</style>
```



### 4.8 登录后获取用户getInfo

需要在mockjs->api->store三端都配置好

mockjs

```
import Mock from 'mockjs'

/* login */
Mock.mock(
  '/api/login',
  'post',
  (req) => {
      const { password, username } = JSON.parse(req.body)
      if (username === 'admin' && password === '123456') {
          return {
            code : 200,
            token:'admin-token'
          }
        } else if(username === 'editor' && password === '123456') {
          return {
            code : 200,
            token:'editor-token'
          }
        } else if(username === 'test' && password === '123456') {
          return {
            code : 200,
            token:'test-token'
          }
        }
  }
)

/* getInfo */

Mock.mock(
'/api/getInfo',
'post',
(req) => {
  const {token} = req.body
  let roles = ['admin']
  let username = 'admin'
  if(token == 'admin-token') {
    roles = ['admin']
    username = 'admin'
    
  } 
  if (token == 'editor-token') {
    roles = ['editor']
    username = 'editor'
     
  } 
  if (token == 'test-token') {
      roles = ['test']
      username = 'test'
     
  }

  return {
    code : 200,
    msg: 'success',
    data: {
      roles,
      username,
      avatar:'https://i.gtimg.cn/club/item/face/img/2/15922_100.gif'
    }
  }
  
}

)
```

api/user.js

```
import instance from './request'


export const Login = (res) =>{
  return instance({
      url:'/login',
      method: 'POST',
      data:res

  })
} 

export const UserInfo = (res) =>{
  return instance({
      url:'/getInfo',
      method: 'POST',
      data:res
  })
}

export default {Login,UserInfo}
```

最后是store

```
import { createStore } from 'vuex'
import {Login,UserInfo} from '@/api/user'

export default createStore({
  state:{
      token:localStorage.getItem('token'),
      avatar:localStorage.getItem('avatar'),
      username:localStorage.getItem('username'),
      roles:[]
  },

  mutations:{
      SET_TOKEN:(state,token) =>{
          state.token = token
      },
      SET_AVATAR:(state,avatar) =>{
          state.avatar = avatar
      },
      SET_USERNAME:(state,username) =>{
          state.username = username
      },
      SET_ROLES: (state, roles) => {
          state.roles = roles
      },
  },

  actions:{
      /* login */
      LoginResult({commit}, userInfo) {
          return new Promise((resolve,reject) =>{
              Login(userInfo).then(response =>{
                  const {code ,token} = response.data
                  if(code == 200) {
                      localStorage.setItem('token',token)
                      commit('SET_TOKEN',token)
                  }
                  resolve(response.data)
              }).catch(error =>{
                  reject(error)
              })
          })
      },
      /* getINfo */
      GetInfo ({commit},token) {
          return new Promise((resolve,reject) => {
               
              UserInfo(token).then(response =>{
                  const {code ,data} = response.data
                   
                  if(code == 200) {
                      commit('SET_AVATAR',data.avatar)
                      commit('SET_USERNAME',data.username)
                      commit('SET_ROLES',data.roles)
                      resolve(response.data)
                  }
               
                   
               
              }).catch(error =>{
                  reject(error)
              })
          })
      },

      /* logout */
      LogoutResult({commit}) {
          commit('SET_TOKEN','')
          commit('SET_AVATAR','')
          commit('SET_USERNAME','')
          localStorage.removeItem('token')
          localStorage.removeItem('username')
          localStorage.removeItem('avatar')
      }
  },

  getters:{
  token: state => state.token,
  avatar: state => state.avatar,
  username: state => state.username,
  roles: state => state.roles,
   
  }
})
```

搞定后在login.vue中添加store中的getInfo

```
<!-- -->
<template>
<div class="login-container">
  <a-row>
    <a-col :xs="0" :md="0" :sm="12" :lg="14" :xl="16"></a-col>
    <a-col :xs="24" :sm="24" :md="12" :lg="10" :xl="6">
      <div class="login-container-form">
        <header>
          <img src="@/assets/images/logo.png" />
          <h1>vue3-admin</h1>
        </header>
        <a-form :model="form" @submit="handleSubmit" @submit.prevent>
          <a-form-item>
            <a-input
              v-model:value="form.username"
              size="large"
              placeholder="Username"
            >
              <template v-slot:prefix><user-outlined type="user" /></template>
            </a-input>
          </a-form-item>
          <a-form-item>
            <a-input
              v-model:value="form.password"
              size="large"
              type="password"
              placeholder="Password"
            >
              <template v-slot:prefix><lock-outlined type="user" /></template>
            </a-input>
          </a-form-item>
          <a-form-item>
            <a-button type="primary" size="large" @click="handleSubmit" block>
              Login in
            </a-button>
          </a-form-item>
        </a-form>
      </div>
    </a-col>
  </a-row>
</div>
</template>

<script>
import { UserOutlined, LockOutlined } from "@ant-design/icons-vue";
import {defineComponent,reactive,toRefs} from 'vue'
import {useRoute, useRouter} from "vue-router";
import {useStore} from 'vuex'
import {message} from 'ant-design-vue'
export default defineComponent ({
  name:'Login',
  components:{
    UserOutlined,
      LockOutlined  
  },

  setup() {
      const state = reactive({
          form:{
              username:"",
              password:""
          }
      })

  const store = useStore()
  const router = useRouter()
  const route = useRoute()

  const handleSubmit = async() =>{
      const {username,password} = state.form
      if(username.trim() == '' || password.trim() == '') return message.warning('用户名和密码不能为空')
      const res = await store.dispatch('LoginResult', state.form)
      
       
    if(res.code == 200) {
        const toPath = decodeURIComponent((route.query?.redirect || '/')) //获取登录成功后要跳转的路由。
        message.success('登录成功！')
        let tokenResult = localStorage.getItem('token')
        const result = await store.dispatch('GetInfo',tokenResult )
      const {username,avatar} = result.data
      if(result.code == 200) {
        localStorage.setItem('username',username)
        localStorage.setItem('avatar',avatar)
      }
       
        router.replace(toPath).then(() => {
        if (route.name == 'login') {
          router.replace('/')

        }
      })
    } else {
      message.info('登录失败')
    }
       
  }

      /* 返回 */
      return {
          ...toRefs(state),
          handleSubmit 
      }
  }

});
</script>
<style lang='less' scoped>
.login-container {
height: 100vh;
background: url("~@/assets/images/login/login_bg.png");
background-size: cover;
&-form {
  width: calc(100% - 40px);
  height: 380px;
  padding: 4vh;
  margin-top: calc((100vh - 380px) / 2);
  margin-right: 20px;
  margin-left: 20px;
  background: url("~@/assets/images/login/login_form.png");
  background-size: 100% 100%;
  border-radius: 10px;
  box-shadow: 0 2px 8px 0 rgba(7, 17, 27, 0.06);
  background-color: #fff;

  header {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 20px;
    img {
      display: inline-block;
      width: 40px;
    }

    h1 {
      margin-bottom: 0;
      font-size: 24px;
      color: #222;
      text-align: center;
    }
  }

  form {
    display: flex;
    align-items: center;
    flex-direction: column;
    width: 100%;
    margin-top: 40px;
  }
}
}
</style>
```

### 4.9 用户登出logout

这里我们使用ant ui的modal和message,包括vue-router



```
<template>
<div class="app-avatar">
  <a-dropdown>
    <span class="ant-dropdown-link">
      <a-avatar :src="avatar" />
      {{ username }}
      <DownOutlined />
    </span>
    <template v-slot:overlay>
      <a-menu>
        <a-menu-item @click="Logout">退出登录</a-menu-item>
      </a-menu>
    </template>
  </a-dropdown>
</div>
</template>

<script>
import { DownOutlined } from "@ant-design/icons-vue";
import { useStore } from "vuex";
import {useRouter,useRoute} from 'vue-router'
import { reactive, toRefs} from "vue";
import {message, Modal} from 'ant-design-vue'
export default {
name: "AppAvatar",
components: { DownOutlined },
setup() {

  const store = useStore();
  const router = useRouter()
  const route = useRoute()

  const state = reactive({
    avatar: store.getters.avatar,
    username: store.getters.username,
  });

  const Logout = () => {
    Modal.confirm({
      title: '您确定要退出登录吗？',
       
      onOk(){
        store.dispatch('LogoutResult').then(()=>{
          message.success('成功退出登录')
            router.replace({
            name: 'login',
            query: {
              redirect: route.fullPath
            }
          })
        })
         
      }
    })
  }

  return {
    ...toRefs(state),
    Logout
  };
},
};
</script>
<style lang="less">
.app-avatar {
.ant-dropdown-link {
  display: block;
  min-height: 64px;
  cursor: pointer;
}
}
</style>
```



## 5 路由权限篇

### 5.1 layout的logo组件



首先layout/logo/logo.vue

```
<template>
<div class="logo">
  <img src="~@/assets/logo.png" alt="">
  <h2 v-show="!collapsed" class="title">Vue3-admin</h2>
</div>
</template>

<script>
export default {
name: "index",
props: {
  collapsed: {
    type: Boolean
  }
}
}
</script>

<style lang="less" scoped>
.logo {
display: flex;
align-items: center;
padding-left: 24px;
height: 64px;
line-height: 64px;
overflow: hidden;
white-space: nowrap;

img {
  height: 32px;
  margin-right: 8px;
}

.title {
  color: white;
  margin-bottom: 0;
}
}
</style>
```

在layout的index.vue引入logo.vue

```
import AppLogo from "./logo/logo.vue";
```



### 5.2layout的menu组件

引入iconfont

```
//第一步
import { createFromIconfontCN } from "@ant-design/icons-vue";
//第二步
const IconFont = createFromIconfontCN({

scriptUrl: "//at.alicdn.com/t/font_1617479_lw8fprdjcs.js",

});
```

传入collapsed

```
props: {
  collapsed: {
    type: Boolean
  }
}
```

然后创建menu组件,给组件传参数

```
    v-model:open-keys="openKeys"
    v-model:selected-keys="selectedKeys"
    mode="inline"
    theme="dark"
    :inline-collapsed="collapsed"
```

vue 项目的权限控制都是通过 `addRoutes`来实现的。简单说就是：用户登录之后会返回一个权限凭证`Token`，用户在根据这个`Token`去问服务端询问自己的权限，辟如服务端返回权限是`['editor']`，前端再根据这个权限动态生成他能访问的路由，再通过`addRoutes`进行动态的路由挂载。

创建vue实例的时候将vue-router挂载，但这个时候vue-router挂载一些登录或者不用权限的公用的页面。

当用户登录后，获取用role，将role和路由表每个页面的需要的权限作比较，生成最终用户可访问的路由表。

调用router.addRoutes(store.getters.addRouters)添加用户可访问的路由。

使用vuex管理路由表，根据vuex中可访问的路由渲染侧边栏组件。

router.js

在登录请求中，获得用户的权限数据，前端根据权限数据，只显示当前用户能访问的权限内菜单

界面权限

如果用户没有登录，手动在地址栏输入管理界面的地址，需要跳转到登录页面；如果用户已经登录，手动在地址栏输入非权限内的地址，需要跳转到404页面

```
export const asyncRoutes = [
{
  path: '/',
  component: Layout,
  redirect: '/index',
  meta: {
    title: '首页',
    affix: true,
  },
  children: [
    {
      path: 'index',
      name: 'Index',
      component: () => import('@/views/index'),
      meta: {
        title: '首页',
        affix: true,
         
      },
    },
  ],
},
{
  path: '/demo',
  component: Layout,
  redirect: '/demo/table',
  meta: {
    title: '组件',
    icon: 'apps-line',
    roles: ['admin', 'editor']
  },
  children: [
    {
      path: 'table',
      name: 'Table',
      component: () => import('@/views/demo/table'),
      meta: {
        title: '表格',
        icon: 'table-2',
      },
    },
    {
      path: 'icon',
      name: 'Icon',
      component: () => import('@/views/demo/icon'),
      meta: {
        title: '图标',
        icon: 'remixicon-line',
      },
    },
  ],
},
{
  path: '/test',
  component: Layout,
  redirect: '/test/test',
  meta: {
    title: '动态路由测试',
    icon: 'test-tube-line',
  },
  children: [
    {
      path: 'test1',
      name: 'Test1',
      component: () => import('@/views/test1'),
      meta: {
        title: '路由测试1',
        icon: 'test-tube-line',
      },
    },
    {
      path: 'test2',
      name: 'Test2',
      component: () => import('@/views/test2'),
      meta: {
        title: '路由测试2',
        icon: 'test-tube-line',
      },
    },
    {
      path: 'test3',
      name: 'Test3',
      component: () => import('@/views/test3'),
      meta: {
        title: '路由测试3',
        icon: 'test-tube-line',
      },
    },
  ],
},
{
  path: '/error',
  name: 'Error',
  component: Layout,
  redirect: '/error/403',
  meta: {
    title: '错误页',
    icon: 'error-warning-line',
  },
  children: [
    {
      path: '403',
      name: 'Error403',
      component: () => import('@/views/error/403'),
      meta: {
        title: '403',
        icon: 'error-warning-line',
      },
    },
    {
      path: '404',
      name: 'Error404',
      component: () => import('@/views/error/404'),
      meta: {
        title: '404',
        icon: 'error-warning-line',
      },
    },
  ],
},

{
  path:'/system',
  name:'system',
  component: Layout,
  redirect: '/system/account',
  meta:{
    title:'system',
    icon:'system',
    roles: ['admin']
  },
  children:[
    {
      path:'account',
      name:'account',
      component: ()=> import('@/views/system/account'),
      meta:{
        title:'account',
        icon:'account'
      }

    },
    {
      path:'group',
      name:'group',
      component: ()=> import('@/views/system/group'),
      meta:{
        title:'group',
        icon:'group'
      }
    }
  ]

}
]
```