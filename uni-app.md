# uni-app

## 介绍

是一个使用Vue.js开发所有信息应用的框架，开发者编写一套代码，可发布到IOS、Android、H5以及各种小程序等多个平台



## 页面

和小程序一样的生命周期函数



## 组件

components下的组件

- 和vue一样的8个生命周期函数
- 一样通过import导入
- components注册使用



## 兄弟组件通信

为了解决页面vue与子窗体nvue通信或者组件之间的通性

- 通过uni.$on设置函数名和回调函数，uni.$on(函数名,回调函数)
- 通过uni.$emit(函数名，传递参数)去改变



## 小程序打包

- 我们需要配置manifest.json这个文件，配置好appid



## h5打包

- 路由使用hash模式

- 我们需要配置manifest.json这个文件，在其中的h5配置中加入publicPath配置，配置如下：

```json
"h5" : {
  "publicPath": "./",
    "router" : {
        "mode" : "hash",
        "base" : ""
    },
},
```



## 原生窗体组件

- app和h5端：为了解决因为原生组件样式权限太高，自己写的样式显示不出来，z-index也显示不出来
- 小程序直接通过z-index就可以了

- 创建nvue文件
- 配置

```json
{  
    "pages": [{  
        "path": "pages/index/index", //首页  
        "style": {  
            "app-plus": {  
                "subNVues":[{  
                    "id": "concat", // 唯一标识  
                    "path": "pages/index/subnvue/concat", // 页面路径  
                    /*"type": "popup",  弹出层，这里不需要*/  
                    "style": {  
                        "position": "absolute",  
                        "dock": "right",  
                        "width": "100upx",  
                        "height": "150upx",  
                        "background": "transparent"  
                    }  
                }]  
            }  
        }  
    }]  
}

```

- 使用

```javascript
const subNVue = uni.getSubNVueById('concat');
subNVue.show('concat',200,()=>{
    console.log('subNVue 原生子窗体显示成功');
})
subNVue.hide('concat',200);
```



## 隐藏tab栏

uni.hideTabBar()



## 隐藏导航栏

"navigationStyle":"custom"