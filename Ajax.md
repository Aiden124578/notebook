# PHP



## 设置编码格式

```php
header('content-type:text/html;charset=utf-8');
```



## echo

echo '输出的内容'；



## $定义变量

变量不以数字开头，分号不能省略

```php
$name = '吴京';
echo '$name';
```



## 拼接字符串

拼接字符串用.拼接

![image-20200717115205252](.\images\image-20200717115205252.png)



数组

```php
$foodArr = array('芒果','西兰花','鸡蛋');
echo $foodArr[2];
count($foodArr); //count()计算数组的长度
```



## 数组对象

```php
$person = array('name'=>'吴京','film'=>'战狼','wife'=>'谢楠');
echo $person['wife'];
print_r($person); //完整输出数组
echo '<br>'
//遍历
    foreach($person as key => $value){
        echo $key.'----'.$value.'<br>';
    }
```

![image-20200717142028405](.\images\image-20200717142028405.png)



## include

引入其他的php页面

```php
include 'fruit.php';
```



## $_Get()

超全局变量，$_Get('key');用于获取提交的数据

![image-20200717172718527](.\images\image-20200717172718527.png)

## get和post的区别

$_Get和$_Post

$_FILES;接受提交的文件

![image-20200717173837553](.\images\image-20200717173837553.png)

![image-20200717174253259](.\images\image-20200717174253259.png)





## 请求，响应

HTTP协议

![image-20200731091805943](images/image-20200731091805943.png)

![image-20200731091449585](images/image-20200731091449585.png)

具体步骤

![image-20200731093137671](images/image-20200731093137671.png)

![image-20200731093414251](images/image-20200731093414251.png)

![image-20200731094023318](images/image-20200731094023318.png)

get请求

![image-20200731094156201](images/image-20200731094156201.png)

![image-20200731094209580](images/image-20200731094209580.png)

回调函数

![image-20200731094655974](images/image-20200731094655974.png)

![image-20200731095037886](images/image-20200731095037886.png)

延迟数据

![image-20200731094711905](images/image-20200731094711905.png)

完整步骤

![](images/image-20200731095517501.png)

![image-20200731095420670](images/image-20200731095420670.png)

![image-20200731100327358](images/image-20200731100327358.png)

用户是否存在

![](images/image-20200731102004799.png)

![image-20200731101831781](images/image-20200731101831781.png)

添加提示信息

![image-20200731102658611](images/image-20200731102658611.png)

Post请求：要加上请求头，不能省略

![](images/image-20200731103321627.png)

![image-20200731103807346](images/image-20200731103807346.png)



## 获取JSON并解析

本质：Json的载体是字符串

![image-20200812092744352](images/image-20200812092744352.png)

返回对象：对象.属性

返回数组：循环

![image-20200812090851106](images/image-20200812090851106.png)

步骤

![image-20200812091111882](images/image-20200812091111882.png)

- 浏览器端

```javascript
document.querySelector('input').onclick=function(){
	//创建异步对象
	var xhr = new XMLHttpRequest();
	//设置请求行
	xhr.open("post",backJSON.php);
	//设置请求头（get请求可以省略）
	//注册状态改变事件（回调函数）
	xhr.onreadystatechange=function(){
	  //判断状态&请求是否成功并使用数据
	  if(xhr.readyState==4&&xhr.status==200){
	  console.log(xhr.responseText);
	  }
	}
	//发送请求
	xhr.send(null);
}
```

- 服务器端

```php
<?php
    //json也要设置一段内容
    //告诉浏览器返回的是json格式的数据，编码是utf-8
    header('content-type:application/json;charset=utf-8');
//读取JSON数据
    $jsonString = file_get_contents('data/stars.json');
//返回读取的内容
echo $jsonString;
?>
```

- 回调函数

```javascript
document.querySelector('input').onclick=function(){
	//创建异步对象
	var xhr = new XMLHttpRequest();
	//设置请求行
	xhr.open("post",backJSON.php);
	//设置请求头（get请求可以省略）
	//注册状态改变事件（回调函数）
	xhr.onreadystatechange=function(){
	  //判断状态&请求是否成功并使用数据
	  if(xhr.readyState==4&&xhr.status==200){
          //转化为对应的对象（数组）
	  var arr =  JSON.parse(xhr.responseText);
          console.log(arr);
          //遍历打印
          for(let i=0;i<arr.length;i++){
              let currentObj = arr[i];
              console.log('姓名：'+currentObj.name+'技能：'+currentObj.skill);
          }
	  }
	}
	//发送请求
	xhr.send(null);
}
```

- 回调函数渲染数据，html来渲染

![image-20200812093029672](images/image-20200812093029672.png)

![image-20200812093233517](images/image-20200812093233517.png)





## 获取网络请求

步骤：network->xhr->response

![image-20200812093459852](images/image-20200812093459852.png)

![image-20200812093722149](images/image-20200812093722149.png)

![image-20200812093751986](images/image-20200812093751986.png)



## ajax简化版

### post

- 第一个参数: page 访问的页面
- 第二个参数: {name:value} 提交的数据
- 第三个参数: function(){} 响应函数

```javascript
$.post(
    page,
    {"name":value},
    function(result){
        $("#checkResult").html(result);
    }
);
```

### get

- 第一个参数: page 访问的页面
- 第二个参数: {name:value} 提交的数据
- 第三个参数: function(){} 响应函数
- 只有第一个参数是必须的，其他参数都是可选

```javascript
$.get(
    page,
    {"name":value},
    function(result){
      $("#checkResult").html(result);
    }
);
```



## fetch

data.text()返回的是promise对象，所以后面用.then方法

![image-20201205231726304](images/image-20201205231726304.png)

参数

![image-20201205232153049](images/image-20201205232153049.png)

delete和get传参一样

![image-20201205232822465](images/image-20201205232822465.png)

![image-20201205232859940](images/image-20201205232859940.png)

post请求

![image-20201205233114934](images/image-20201205233114934.png)