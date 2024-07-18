# 1. ajax-json-server
AJAX 全称为 Asynchronous JavaScript And XML，就是异步的 JS 和 XML。 

通过 AJAX 可以在浏览器中向服务器发送异步请求，最大的优势：无刷新获取数据。 

AJAX 不是新的编程语言，而是一种将现有的标准组合在一起使用的新方式。 

# 2. XML
XML 可扩展标记语言。 

XML 被设计用来传输和存储数据。 

XML 和 HTML 类似，不同的是 HTML 中都是预定义标签，而 XML 中没有预定义标签， 

全都是自定义标签，用来表示一些数据。

**现在已经被****JSON****取代了**

```java
{"name":"孙悟空","age":18,"gender":"男"}
```
# 3. AJAX的特点
1 AJAX的优点

1) 可以无需刷新页面而与服务器端进行通信。 

2) 允许你根据用户事件来更新部分页面内容。 

 2 AJAX的缺点

1) 没有浏览历史，不能回退 

2) 存在跨域问题(同源) 

3) SEO (搜索引擎优化)不友好 

# 4. HTTP协议
（hypertext transport protocol）协议[超文本传输协议]，协议详细规定了浏览器和万维网服务器之间互相通信的规则。

协议就是一种约定，规则

* 请求（报文）  浏览器给服务器发送的内容称为请求  request
格式参数

```
/

行       GET POST 请求类型   /s?ie=utf-8  url  

头       Host: atguigu.com   

Cookie: name=guihu

Content-type: application/x-www-form-urlencoded

User-Agent: chrome 83

格式 ： 名字：（空格）＋值

空行

体        username = admin&password=admin

(post 请求体可以不为空，get为空)

/

* 响应  （报文） 服务器给浏览器返回的结果称为响应  response
/

行         HTTP/1.1   200(响应状态码)    OK 

头         Content-type: text/html;charser=utf-8

Content-length： 2048

Content-encoding： gzip

对响应体的描述

空行

体          <html>

<head></head>

<body></body>

</html>

/
```

# 5. chrome查看
```
GET

Headers

请求行和请求头内容：Resuest Headers

对请求的url做解析：Query String Parameters

响应头和响应行：Response Headers

Response  响应体

POST

Headers

请求行和请求头内容：Resuest Headers

请求体：From Data
```

# express框架

```javascript
// npm init --yes
// npm i express

// 1. 引入express
const express = require ("express");

// 2. 创建路由规则
const app = express ();

// 3. 创建路由规则
// require 是对请求报文的封装
// response 是对响应报文的封装
app.get('/',(require,response)=>{
    // 设置响应头   设置允许跨域
    response.setHeader('Access-Control-Allow-Origin','*');
    // 设置响应体
    response.send('HELLO AJAX');
})

// 4. 监听端口启动服务
app.listen(3001,()=>{
    console.log("服务器已经启动，3001端口监听中。。。");
})

// http://localhost:3001/
```
# 6.原生Ajax

1. get请求
### html

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, 
initial-scale=1.0">
    <title>Document</title>

    <style>
        #result{
            width: 200px;
            height: 300px;
            border: solid 1px black;
        }
    </style>

</head>
<body>
    <button>点击发送</button>
    <div id= "result"></div>
</body>

    <script>
        // 获取button元素
        const btn= document.getElementsByTagName('button')[0];
        // 绑定事件
        btn.onclick = function(){
            //测试
            // console.log('text');

            // ajax
            // 1. 创建对象
            const xhr = new XMLHttpRequest();

            // 2. 初始化  设置请求的方法和 url
            xhr.open('GET','http://localhost:3001/');

            // 3. 发送
            xhr.send();

            // 4. 事件绑定 处理服务器返回的结果
            // readystate xhr对象中的属性，表示状态 
            // 0（未初始化） 1（open方法调用完毕） 
            // 2 （send方法调用完毕）3 （服务端返回部分结果）
            // 4（服务端返回所以结果）  
            xhr.onreadystatechange = function(){
                // 判断 服务端返回结果
                if(xhr.readyState === 4){
                    // 判断响应状态码 200 404 
                    if(xhr.status >= 200 && xhr.status <300){
                        // 处理结果  行 头 空行 体
                        // 1. 响应行
                        console.log(xhr.status);    //  状态码
                        console.log(xhr.statusText);    //  状态字符串
                        //所有响应头
                        console.log(xhr.getAllResponseHeaders());   
                        console.log(xhr.response);      //响应体
                        // 设置result的文本
                        result.innerHTML = xhr.response;
                    }else{

                    }

                }
            }

        }
    </script>
</html>
```
### server.js

```javascript
//1. 引入express
const express = require('express');

//2. 创建应用对象
const app = express();

//3. 创建路由规则
// request 是对请求报文的封装
// response 是对响应报文的封装
app.get('/server', (request, response) => {
    //设置响应头  设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    //设置响应体
    response.send('HELLO AJAX - 2');
});
//4. 监听端口启动服务
app.listen(8000, () => {
    console.log("服务已经启动, 8000 端口监听中....");
});
```
2. post请求
post 请求体在send（）里面传入

### html

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AJAX POST 请求</title>
    <style>
        #result{
            width:200px;
            height:100px;
            border:solid 1px #903;
        }
    </style>
</head>
<body>
    <div id="result"></div>
    <script>
        //获取元素对象
        const result = document.getElementById("result");
        //绑定事件
        result.addEventListener("mouseover", function(){
            //1. 创建对象
            const xhr = new XMLHttpRequest();
            //2. 初始化 设置类型与 URL
            xhr.open('POST', 'http://127.0.0.1:8000/server');
            //设置请求头
            xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
            xhr.setRequestHeader('name','atguigu');
            //3. 发送
            xhr.send('a=100&b=200&c=300');
            // xhr.send('a:100&b:200&c:300');
            // xhr.send('1233211234567');
            
            //4. 事件绑定
            xhr.onreadystatechange = function(){
                //判断
                if(xhr.readyState === 4){
                    if(xhr.status >= 200 && xhr.status < 300){
                        //处理服务端返回的结果
                        result.innerHTML = xhr.response;
                    }
                }
            }
        });
    </script>
</body>
</html>
```
xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded');
Content-Type：设置请求体类型

application/x-www-form-urlencoded：参数查询字符串固定写法

### server.js

```javascript
//1. 引入express
const express = require('express');

//2. 创建应用对象
const app = express();

//3. 创建路由规则
// request 是对请求报文的封装
// response 是对响应报文的封装
// app.all 可以接受各种请求
app.post('/server', (request, response) => {
    //设置响应头  设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    //设置响应体
    response.send('HELLO AJAX POST');
});
//4. 监听端口启动服务
app.listen(8000, () => {
    console.log("服务已经启动, 8000 端口监听中....");
});
```
3. JSON
### HTML

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>JSON响应</title>
    <style>
        #result{
            width:200px;
            height:100px;
            border:solid 1px #89b;
        }
    </style>
</head>
<body>
    <div id="result"></div>
    <script>
        const result = document.getElementById('result');
        //绑定键盘按下事件
        window.onkeydown = function(){
            //发送请求
            const xhr = new XMLHttpRequest();
            //设置响应体数据的类型
            xhr.responseType = 'json';
            //初始化
            xhr.open('GET','http://127.0.0.1:8000/json-server');
            //发送
            xhr.send();
            //事件绑定
            xhr.onreadystatechange = function(){
                if(xhr.readyState === 4){
                    if(xhr.status >= 200 && xhr.status < 300){
                        //
                        // console.log(xhr.response);
                        // result.innerHTML = xhr.response;
                        // 1. 手动对数据转化
                        // let data = JSON.parse(xhr.response);
                        // console.log(data);
                        // result.innerHTML = data.name;
                        // 2. 自动转换
                        console.log(xhr.response);
                        result.innerHTML = xhr.response.name;
                    }
                }
            }
        }
    </script>
</body>
</html>
```
### server.js

```javascript
//JSON 响应
app.all('/json-server', (request, response) => {
    //设置响应头  设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    //响应头
    response.setHeader('Access-Control-Allow-Headers', '*');
    //响应一个数据
    const data = {
        name: 'atguigu'
    };
    //对对象进行字符串转换
    let str = JSON.stringify(data);
    //设置响应体
    response.send(str);
});
```
npm install-g nodemon 服务器自动重启插件
4. **解决****IE****缓存问题**
问题：在一些浏览器中(IE),由于缓存机制的存在，ajax 只会发送的第一次请求，剩 

余多次请求不会在发送给浏览器而是直接加载缓存中的数据。 

ie浏览器会对ajax请求结果做一个缓存，等下一次再次发送请求时，走的是本地缓存而不是服务器返回的最新数据

解决方式：浏览器的缓存是根据 url 地址来记录的，所以我们只需要修改 url 地址 

即可避免缓存问题 

xhr.open("get","/testAJAX?t="+Date.now())

### html

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>IE缓存问题</title>
    <style>
        #result{
            width:200px;
            height:100px;
            border:solid 1px #258;
        }
    </style>
</head>
<body>
    <button>点击发送请求</button>
    <div id="result"></div>
    <script>
        const btn = document.getElementsByTagName('button')[0];
        const result = document.querySelector('#result');

        btn.addEventListener('click', function(){
            const xhr = new XMLHttpRequest();
            xhr.open("GET",'http://127.0.0.1:8000/ie?t='+Date.now());
            xhr.send();
            xhr.onreadystatechange = function(){
                if(xhr.readyState === 4){
                    if(xhr.status >= 200 && xhr.status< 300){
                        result.innerHTML = xhr.response;
                    }
                }
            }
        })
    </script>
</body>
</html>
```
### server.js

```javascript
//针对 IE 缓存
app.get('/ie', (request, response) => {
    //设置响应头  设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    //设置响应体
    response.send('HELLO IE - 5');
});
```
5. 请求超时与网络异常
### html

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>请求超时与网络异常</title>
    <style>
        #result{
            width:200px;
            height:100px;
            border:solid 1px #90b;
        }
    </style>
</head>
<body>
    <button>点击发送请求</button>
    <div id="result"></div>
    <script>
        const btn = document.getElementsByTagName('button')[0];
        const result = document.querySelector('#result');

        btn.addEventListener('click', function(){
            const xhr = new XMLHttpRequest();
            //超时设置 2s 设置
            xhr.timeout = 2000;
            //超时回调
            xhr.ontimeout = function(){
                alert("网络异常, 请稍后重试!!");
            }
            //网络异常回调
            xhr.onerror = function(){
                alert("你的网络似乎出了一些问题!");
            }

            xhr.open("GET",'http://127.0.0.1:8000/delay');
            xhr.send();
            xhr.onreadystatechange = function(){
                if(xhr.readyState === 4){
                    if(xhr.status >= 200 && xhr.status< 300){
                        result.innerHTML = xhr.response;
                    }
                }
            }
        })
    </script>
</body>
</html>
```
### server.js

```javascript
//延时响应
app.all('/delay', (request, response) => {
    //设置响应头  设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    response.setHeader('Access-Control-Allow-Headers', '*');
    setTimeout(() => {
        //设置响应体
        response.send('延时响应');
    }, 1000)
});
```
6. Ajax取消请求
```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>取消请求</title>
</head>
<body>
    <button>点击发送</button>
    <button>点击取消</button>
    <script>
        //获取元素对象
        const btns = document.querySelectorAll('button');
        let x = null;

        btns[0].onclick = function(){
            x = new XMLHttpRequest();
            x.open("GET",'http://127.0.0.1:8000/delay');
            x.send();
        }

        // abort（） 取消请求
        btns[1].onclick = function(){
            x.abort();
        }
    </script>
</body>
</html>
```
7. 重复请求
```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>重复请求问题</title>
</head>
<body>
    <button>点击发送</button>
    <script>
        //获取元素对象
        const btns = document.querySelectorAll('button');
        let x = null;
        //标识变量
        let isSending = false; // 是否正在发送AJAX请求

        btns[0].onclick = function(){
            //判断标识变量
            if(isSending) x.abort();// 如果正在发送, 则取消该请求, 创建一个新的请求
            
            x = new XMLHttpRequest();
            //修改 标识变量的值
            isSending = true;
            x.open("GET",'http://127.0.0.1:8000/delay');
            x.send();
            x.onreadystatechange = function(){
                if(x.readyState === 4){
                    //修改标识变量
                    isSending = false;
                }
            }
        }

        // abort
        btns[1].onclick = function(){
            x.abort();
        }
    </script>
</body>
</html>
```
# 7. jq
## **1. get****请求**

$.get(url, [data], [callback], [type]) 

url:请求的 URL 地址。 

data:请求携带的参数。 

callback:载入成功时回调函数。 

type:设置返回内容格式，xml, html, script, json, text, _default。 

## **2 post****请求**

$.post(url, [data], [callback], [type]) 

url:请求的 URL 地址。 

data:请求携带的参数。 

callback:载入成功时回调函数。 

type:设置返回内容格式，xml, html, script, json, text, _default。 

## html

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>jQuery 发送 AJAX 请求</title>
    <link crossorigin="anonymous" href="https://cdn.bootcss.com/twitter-bootstrap/3.3.7/css/bootstrap.min.css" rel="stylesheet">
    <script crossorigin="anonymous" src="https://cdn.bootcdn.net/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
</head>
<body>
    <div class="container">
        <h2 class="page-header">jQuery发送AJAX请求 </h2>
        <button class="btn btn-primary">GET</button>
        <button class="btn btn-danger">POST</button>
        <button class="btn btn-info">通用型方法ajax</button>
    </div>
    <script>
        $('button').eq(0).click(function(){
            $.get('http://127.0.0.1:8000/jquery-server', {a:100, b:200}, function(data){
                console.log(data);
            },'json');
        });

        $('button').eq(1).click(function(){
            $.post('http://127.0.0.1:8000/jquery-server', {a:100, b:200}, function(data){
                console.log(data);
            });
        });

        $('button').eq(2).click(function(){
            $.ajax({
                //url
                url: 'http://127.0.0.1:8000/jquery-server',
                //参数
                data: {a:100, b:200},
                //请求类型
                type: 'GET',
                //响应体结果
                dataType: 'json',
                //成功的回调
                success: function(data){
                    console.log(data);
                },
                //超时时间
                timeout: 2000,
                //失败的回调
                error: function(){
                    console.log('出错啦!!');
                },
                //头信息
                headers: {
                    c:300,
                    d:400
                }
            });
        });

    </script>
</body>
</html>
```
## server.js

```xml
//jQuery 服务
app.all('/jquery-server', (request, response) => {
    //设置响应头  设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    response.setHeader('Access-Control-Allow-Headers', '*');
    // response.send('Hello jQuery AJAX');
    const data = {name:'尚硅谷'};
    response.send(JSON.stringify(data));
});
```
# 8. axios
ajax工具库

特点：

* 浏览器端发起XMLHttpRequests请求
* node端发起http请求
* 支持Promise API
* 监听请求和返回
* 对请求和返回进行转化
* 取消请求
* 自动转换json数据
* 客户端支持抵御XSRF攻击
### html

```xml
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>axios 发送 AJAX请求</title>
    <script crossorigin="anonymous" src="https://cdn.bootcdn.net/ajax/libs/axios/0.19.2/axios.js"></script>
</head>

<body>
    <button>GET</button>
    <button>POST</button>
    <button>AJAX</button>

    <script>
        // https://github.com/axios/axios
        const btns = document.querySelectorAll('button');

        //配置 baseURL 
        //如果没有配置请求axios.get('http://127.0.0.1:8000/axios-server',{})
        axios.defaults.baseURL = 'http://127.0.0.1:8000';

        btns[0].onclick = function () {
            //GET 请求
            axios.get('/axios-server', {
                //url 参数
                params: {
                    id: 100,
                    vip: 7
                },
                //请求头信息
                headers: {
                    name: 'atguigu',
                    age: 20
                }
            }).then(value => {
                console.log(value);
            });
        }

        btns[1].onclick = function () {
            // 第一个参数url 第二个data请求体 后面其他
            axios.post('/axios-server', {
                username: 'admin',
                password: 'admin'
            }, {
                //url 
                params: {
                    id: 200,
                    vip: 9
                },
                //请求头参数
                headers: {
                    height: 180,
                    weight: 180,
                }
            });
        }
    
        btns[2].onclick = function(){
            axios({
                //请求方法
                method : 'POST',
                //url
                url: '/axios-server',
                //url参数
                params: {
                    vip:10,
                    level:30
                },
                //头信息
                headers: {
                    a:100,
                    b:200
                },
                //请求体参数
                data: {
                    username: 'admin',
                    password: 'admin'
                }
            }).then(response=>{
                //响应状态码
                console.log(response.status);
                //响应状态字符串
                console.log(response.statusText);
                //响应头信息
                console.log(response.headers);
                //响应体
                console.log(response.data);
            })
        }
    </script>
</body>

</html>
```
### server.js

```javascript
//axios 服务
app.all('/axios-server', (request, response) => {
    //设置响应头  设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    response.setHeader('Access-Control-Allow-Headers', '*');
    // response.send('Hello jQuery AJAX');
    const data = {name:'尚硅谷'};
    response.send(JSON.stringify(data));
});
```

# 9. fetch
**Fetch** fetch号称是AJAX的替代品，是在ES6出现的，使用了ES6中的promise对象。Fetch是基于promise设计的。Fetch的代码结构比起ajax简单多。**fetch不是ajax的进一步封装，而是原生js，没有使用XMLHttpRequest对象**。

 

fetch的优点：

 

* 语法简洁，更加语义化
* 基于标准 Promise 实现，支持 async/await
* 更加底层，提供的API丰富（request, response）
* 脱离了XHR，是ES规范里新的实现方式
 

fetch的缺点：

 

* fetch只对网络请求报错，对400，500都当做成功的请求，服务器返回 400，500 错误码时并不会 reject，只有网络错误这些导致请求不能完成时，fetch 才会被 reject。
* fetch默认不会带cookie，需要添加配置项： fetch(url, {credentials: 'include'})
* fetch不支持abort，不支持超时控制，使用setTimeout及Promise.reject的实现的超时控制并不能阻止请求过程继续在后台运行，造成了流量的浪费
* fetch没有办法原生监测请求的进度，而XHR可以
## html

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>fetch 发送 AJAX请求</title>
</head>
<body>
    <button>AJAX请求</button>
    <script>
        //文档地址
        //https://developer.mozilla.org/zh-CN/docs/Web/API/WindowOrWorkerGlobalScope/fetch
        
        const btn = document.querySelector('button');

        btn.onclick = function(){
            fetch('http://127.0.0.1:8000/fetch-server?vip=10', {
                //请求方法
                method: 'POST',
                //请求头
                headers: {
                    name:'atguigu'
                },
                //请求体
                body: 'username=admin&password=admin'
            }).then(response => {
                // return response.text();
                return response.json();
            }).then(response=>{
                console.log(response);
            });
        }
    </script>
</body>
</html>
```
## server

```javascript
//fetch 服务
app.all('/fetch-server', (request, response) => {
    //设置响应头  设置允许跨域
    response.setHeader('Access-Control-Allow-Origin', '*');
    response.setHeader('Access-Control-Allow-Headers', '*');
    // response.send('Hello jQuery AJAX');
    const data = {name:'尚硅谷'};
    response.send(JSON.stringify(data));
});
```
# 10. 跨域
# 11. 同源策略
同源策略(Same-Origin Policy)最早由 Netscape 公司提出，是浏览器的一种安全策略。 

同源： 协议、域名、端口号 必须完全相同。 

违背同源策略就是跨域。 

**同源策略：protocol（协议）、domain（域名）、port（端口）三者必须一致。**

**同源政策主要限制了三个方面：**

 

* 当前域下的 js 脚本不能够访问其他域下的 cookie、localStorage 和 indexDB。
* 当前域下的 js 脚本不能够操作访问操作其他域下的 DOM。
* 当前域下 ajax 无法发送跨域请求。
 同源政策的目的主要是为了保证用户的信息安全，它只是对 js 脚本的一种限制，并不是对浏览器的限制，对于一般的 img、或者script 脚本请求都不会有跨域的限制，这是因为这些操作都不会通过响应结果来进行可能出现安全问题的操作。

html 同源例子

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>首页</title>
</head>
<body>
    <h1>尚硅谷</h1>
    <button>点击获取用户数据</button>
    <script>
        const btn = document.querySelector('button');

        btn.onclick = function(){
            const x = new XMLHttpRequest();
            //这里因为是满足同源策略的, 所以 url 可以简写
            x.open("GET",'/data');
            //发送
            x.send();
            //
            x.onreadystatechange = function(){
                if(x.readyState === 4){
                    if(x.status >= 200 && x.status < 300){
                        console.log(x.response);
                    }
                }
            }
        }
    </script>
</body>
</html>
```
server.js
```javascript
const express = require('express');

const app = express();

app.get('/home', (request, response)=>{
    //响应一个页面
    response.sendFile(__dirname + '/index.html');
});

app.get('/data', (request, response)=>{
    response.send('用户数据');
});

app.listen(9000, ()=>{
    console.log("服务已经启动...");
});
```
2. 如何解决跨域
### **2.1 JSONP**

(1) JSONP 是什么 

JSONP(JSON with Padding)，是一个非官方的跨域解决方案，纯粹凭借程序员的聪明 

才智开发出来，只支持 get 请求。 

(2) JSONP 怎么工作的？ 

在网页有一些标签天生具有跨域能力，比如：img link iframe script。 

JSONP 就是利用 script 标签的跨域能力来发送请求的。

**jsonp**的原理就是利用`<script>`标签没有跨域限制，通过`<script>`标签src属性，发送带有callback参数的GET请求，服务端将接口返回数据拼凑到callback函数中，返回给浏览器，浏览器解析执行，从而前端拿到callback函数返回的数据。

返回的形式是函数的调用，并且把参数放里面

**JSONP的缺点：**

* 具有局限性， 仅支持get方法
* 不安全，可能会遭受XSS攻击
1. jsonp原理
html

```javascript
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>原理演示</title>
    <style>
        #result {
            width: 300px;
            height: 100px;
            border: solid 1px #78a;
        }
    </style>
</head>

<body>
    <div id="result"></div>
    <script>
        //处理数据
        function handle(data) {
            //获取 result 元素
            const result = document.getElementById('result');
            result.innerHTML = data.name;
        }
    </script>
    <!-- <script src="http://127.0.0.1:5500/%E8%AF%BE%E5%A0%82/%E4%BB%A3%E7%A0%81/7-%E8%B7%A8%E5%9F%9F/2-JSONP/js/app.js"></script> -->
    <script src="http://127.0.0.1:8000/jsonp-server"></script>
</body>

</html>
```
js
```javascript
const data = {
    name: '尚硅谷atguigu'
};

handle(data);
```
server.js
```javascript
//jsonp服务
app.all('/jsonp-server',(request, response) => {
    // response.send('console.log("hello jsonp")');
    const data = {
        name: '尚硅谷atguigu'
    };
    //将数据转化为字符串
    let str = JSON.stringify(data);
    //返回结果
    response.end(`handle(${str})`);
});
```
2.  实践
#### 原生jsonp实践

 判断用户登录

html

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>案例</title>
</head>
<body>
    用户名: <input type="text" id="username">
    <p></p>
    <script>
        //获取 input 元素
        const input = document.querySelector('input');
        const p = document.querySelector('p');
        
        //声明 handle 函数
        function handle(data){
            input.style.border = "solid 1px #f00";
            //修改 p 标签的提示文本
            p.innerHTML = data.msg;
        }

        //绑定事件
        input.onblur = function(){
            //获取用户的输入值
            let username = this.value;
            //向服务器端发送请求 检测用户名是否存在
            //1. 创建 script 标签
            const script = document.createElement('script');
            //2. 设置标签的 src 属性
            script.src = 'http://127.0.0.1:8000/check-username';
            //3. 将 script 插入到文档中
            document.body.appendChild(script);
        }
    </script>
</body>
</html>
```
server.js
```javascript
app.all('/check-username',(request, response) => {
    // response.send('console.log("hello jsonp")');
    const data = {
        exist: 1,
        msg: '用户名已经存在'
    };
    //将数据转化为字符串
    let str = JSON.stringify(data);
    //返回结果
    response.end(`handle(${str})`);
});
```
#### jq-jsonp实践

html

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>jQuery-jsonp</title>
    <style>
        #result{
            width:300px;
            height:100px;
            border:solid 1px #089;
        }
    </style>
    <script crossorigin="anonymous" src='https://cdn.bootcss.com/jquery/3.5.0/jquery.min.js'></script>
</head>
<body>
    <button>点击发送 jsonp 请求</button>
    <div id="result">

    </div>
    <script>
        $('button').eq(0).click(function(){
            $.getJSON('http://127.0.0.1:8000/jquery-jsonp-server?callback=?', function(data){
                $('#result').html(`
                    名称: ${data.name}<br>
                    校区: ${data.city}
                `)
            });
        });
    </script>
</body>
</html>
```
serverjs
```javascript
app.all('/jquery-jsonp-server',(request, response) => {
    // response.send('console.log("hello jsonp")');
    const data = {
        name:'尚硅谷',
        city: ['北京','上海','深圳']
    };
    //将数据转化为字符串
    let str = JSON.stringify(data);
    //接收 callback 参数
    let cb = request.query.callback;

    //返回结果
    response.end(`${cb}(${str})`);
});
```
### 2.2 cors

[https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)

1) CORS 是什么？ 

CORS（Cross-Origin Resource Sharing），跨域资源共享。CORS 是官方的跨域解决方 

案，它的特点是不需要在客户端做任何特殊的操作，完全在服务器中进行处理，支持 

get 和 post 请求。跨域资源共享标准新增了一组 HTTP 首部字段，允许服务器声明哪些 

源站通过浏览器有权限访问哪些资源 

2) CORS 怎么工作的？ 

CORS 是通过设置一个响应头来告诉浏览器，该请求允许跨域，浏览器收到该响应 

以后就会对响应放行。 

html

```xml
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CORS</title>
    <style>
        #result{
            width:200px;
            height:100px;
            border:solid 1px #90b;
        }
    </style>
</head>
<body>
    <button>发送请求</button>
    <div id="result"></div>
    <script>
        const btn = document.querySelector('button');

        btn.onclick = function(){
            //1. 创建对象
            const x = new XMLHttpRequest();
            //2. 初始化设置
            x.open("GET", "http://127.0.0.1:8000/cors-server");
            //3. 发送
            x.send();
            //4. 绑定事件
            x.onreadystatechange = function(){
                if(x.readyState === 4){
                    if(x.status >= 200 && x.status < 300){
                        //输出响应体
                        console.log(x.response);
                    }
                }
            }
        }
    </script>
</body>
</html>
```
server.js
```javascript
app.all('/cors-server', (request, response)=>{
    //设置响应头
    response.setHeader("Access-Control-Allow-Origin", "*");
    response.setHeader("Access-Control-Allow-Headers", '*');
    response.setHeader("Access-Control-Allow-Method", '*');
    // response.setHeader("Access-Control-Allow-Origin", "http://127.0.0.1:5500");
    response.send('hello CORS');
});
```
下面是MDN对于CORS的定义：
 

> 跨域资源共享(CORS) 是一种机制，它使用额外的 HTTP 头来告诉浏览器  让运行在一个 origin (domain)上的Web应用被准许访问来自不同源服务器上的指定的资源。当一个资源从与该资源本身所在的服务器不同的域、协议或端口请求一个资源时，资源会发起一个跨域HTTP 请求。 
 

CORS需要浏览器和服务器同时支持，整个CORS过程都是浏览器完成的，无需用户参与。因此实现**CORS的关键就是服务器，只要服务器实现了CORS请求**，就可以跨源通信了。

 

浏览器将CORS分为**简单请求**和**非简单请求**：

 

简单请求不会触发CORS预检请求。若该请求满足以下两个条件，就可以看作是简单请求：

 

**1）请求方法是以下三种方法之一：**

 

* HEAD
* GET
* POST
 

**2）HTTP的头信息不超出以下几种字段：**

 

* Accept
* Accept-Language
* Content-Language
* Last-Event-ID
* Content-Type：只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain
 若不满足以上条件，就属于非简单请求了。

**（1）简单请求过程：**

 

对于简单请求，浏览器会直接发出CORS请求，它会在请求的头信息中增加一个Orign字段，该字段用来说明本次请求来自哪个源（协议+端口+域名），服务器会根据这个值来决定是否同意这次请求。如果Orign指定的域名在许可范围之内，服务器返回的响应就会多出以下信息头：

 

```plain
Access-Control-Allow-Origin: http://api.bob.com  // 和Orign一直
Access-Control-Allow-Credentials: true   // 表示是否允许发送Cookie
Access-Control-Expose-Headers: FooBar   // 指定返回其他字段的值
Content-Type: text/html; charset=utf-8   // 表示文档类型
复制代码
```
 
如果Orign指定的域名不在许可范围之内，服务器会返回一个正常的HTTP回应，浏览器发现没有上面的Access-Control-Allow-Origin头部信息，就知道出错了。这个错误无法通过状态码识别，因为返回的状态码可能是200。

 

**在简单请求中，在服务器内，至少需要设置字段：**`Access-Control-Allow-Origin`

 

**（2）非简单请求过程**

 

非简单请求是对服务器有特殊要求的请求，比如请求方法为DELETE或者PUT等。非简单请求的CORS请求会在正式通信之前进行一次HTTP查询请求，**称为预检请求**。

 

浏览器会询问服务器，当前所在的网页是否在服务器允许访问的范围内，以及可以使用哪些HTTP请求方式和头信息字段，只有得到肯定的回复，才会进行正式的HTTP请求，否则就会报错。

 

预检请求使用的**请求方法是OPTIONS**，表示这个请求是来询问的。他的头信息中的关键字段是Orign，表示请求来自哪个源。除此之外，头信息中还包括两个字段：

 

* **Access-Control-Request-Method**：该字段是必须的，用来列出浏览器的CORS请求会用到哪些HTTP方法。
* **Access-Control-Request-Headers**： 该字段是一个逗号分隔的字符串，指定浏览器CORS请求会额外发送的头信息字段。
 

服务器在收到浏览器的预检请求之后，会根据头信息的三个字段来进行判断，如果返回的头信息在中有Access-Control-Allow-Origin这个字段就是允许跨域请求，如果没有，就是不同意这个预检请求，就会报错。

 

服务器回应的CORS的字段如下：

 

```plain
Access-Control-Allow-Origin: http://api.bob.com  // 允许跨域的源地址
Access-Control-Allow-Methods: GET, POST, PUT // 服务器支持的所有跨域请求的方法
Access-Control-Allow-Headers: X-Custom-Header  // 服务器支持的所有头信息字段
Access-Control-Allow-Credentials: true   // 表示是否允许发送Cookie
Access-Control-Max-Age: 1728000  // 用来指定本次预检请求的有效期，单位为秒
复制代码
```
 
只要服务器通过了预检请求，在以后每次的CORS请求都会自带一个Origin头信息字段。服务器的回应，也都会有一个Access-Control-Allow-Origin头信息字段。

 

**在非简单请求中，至少需要设置以下字段：**

 

```plain
'Access-Control-Allow-Origin'  
'Access-Control-Allow-Methods'
'Access-Control-Allow-Headers'
复制代码
```
 
减少OPTIONS请求次数：

 

OPTIONS请求次数过多就会损耗页面加载的性能，降低用户体验度。所以尽量要减少OPTIONS请求次数，可以后端在请求的返回头部添加：Access-Control-Max-Age：number。它表示预检请求的返回结果可以被缓存多久，单位是秒。该字段只对完全一样的URL的缓存设置生效，所以设置了缓存时间，在这个时间范围内，再次发送请求就不需要进行预检请求了。

 

CORS中Cookie相关问题：

 

在CORS请求中，如果想要传递Cookie，就要满足以下三个条件：

 

* 在请求中设置 `withCredentials`
 

默认情况下在跨域请求，浏览器是不带 cookie 的。但是我们可以通过设置 withCredentials 来进行传递 cookie.

 

```plain
// 原生 xml 的设置方式
var xhr = new XMLHttpRequest();
xhr.withCredentials = true;
// axios 设置方式
axios.defaults.withCredentials = true;
复制代码
```
 
* Access-Control-Allow-Credentials 设置为 true
* Access-Control-Allow-Origin 设置为非 `*`
