# Socket.IO

Socket.IO是一个库，可用于在浏览器和服务器之间进行实时，双向和基于事件的同学。它包括：

- 使用Node.js服务器
- 为浏览器（可从Node.js的也运行）一个JavaScript客户端库

## 安装

### 服务器

```shell
npm install --save socket.io
```

### Javascript客户端

```shell
npm install --save socket.io-client
```

## 与Node http服务器一起使用

### 服务器（app.js）

```javascript
var app = require('http').createServer(handler)
//解决vue3跨域问题
var io = require('socket.io')(app,{
  cors: {
    origin: "*",
    methods: ["GET", "POST"]
  }
})
var fs = require('fs')

app.listen(80)

function handler(req,res){
  fs.readFile(__dirname + '/index.html',function(err,data){
    if(err){
      res.writeHead(500)
      return res.end('Error loading index.html')
    }
    res.writeHead(200)
    res.end(data)
  })
}

io.on('connection',function(socket){
  socket.emit('news',{hello:'world'})
  scoket.on('my other event',function(data){
    console.log(data)
  })
})
```

### 客户端（index.html）

```javascript
<script src="js/socket.js"></script>
<script>
  var socket = io('http://localhost')
  socket.on('news',function(data){
    console.log(data)
    socket.emit('my other event',{my:'data'})
  })
</script>
```



# webSocket

## 客户端

![image-20210717100530241](E:\笔记\images\image-20210717100530241.png)

![image-20210717100655367](E:\笔记\images\image-20210717100655367.png)

## 服务端
