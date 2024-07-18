## 02路由

```typescript
//导入 express
const express = require("express");
//创建应用对象
const app = express();
//创建 get 路由
app.get("/home", (req, res) => {
  res.send("网站首页");
});
//首页路由
app.get("/", (req, res) => {
  res.send("我才是真正的首页");
});
//创建 post 路由
app.post("/login", (req, res) => {
  res.send("登录成功");
});
//匹配所有的请求方法
app.all("/search", (req, res) => {
  res.send("1 秒钟为您找到相关结果约 100,000,000 个");
});

app.listen(3000, () => {
  console.log("服务器已经启动...");
});

```
## 03 获取请求参数

```typescript
const express = require('express')

// 创建应用对象
const app = express()

// 创建路由
app.get('/req',(req,res)=>{
    res.send('get')
    // 原生操作
    console.log(req.method)
    console.log(req.url);
    console.log(req.httpVersion);
    console.log(req.header)
    // GET
    // /req
    // 1.1
    // [Function: header]

    // express 封装操作
    console.log(req.path)
    console.log(req.query)
    // http://localhost:3000/req?a=133&b=20
    // /req
    // { a: '133', b: '20' }

    // 获取客户端ip
    console.log(req.ip);
    // ::1

    // 获取请求头
    console.log(req.get('host'));
    // http://localhost:3000/req?a=133&b=20
    // localhost:3000

})

app.listen(3000,()=>{
    console.log('服务器已经启动...')
})
```
## 04 获取路由参数

```typescript
const express = require("express");

// 创建应用对象
const app = express();

// 创建路由
app.get("/:id.html", (req, res) => {
  // 获取 URL 路由参数
  console.log(req.params.id);
  
  res.setHeader("content-type", "text/html;charset=utf-8");
  res.end(req.params.id + "商品详情");
});

app.listen(3000, () => {
  console.log("服务器已经启动...");
});
```
## 05 根据路由参数返回对应数据

```typescript
const express = require("express");

// 导入 json 文件
const { singers } = require("./singer.json");

// 创建应用对象
const app = express();

// 创建路由
app.get("/singer/:id.html", (req, res) => {
  // 获取 URL 路由参数
  let { id } = req.params;
  // 在数组中获取对应 id 的数据
  let result = singers.find(item => {
    if (item.id === Number(id)) {
      return true;
    }
  });

  // 判断是否为空
  if (!result) {
    res.statusCode = 404
    res.end(`<h1>404 not found</h1>`)
    return
  }

  // 响应数据
  res.end(`
  <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h2>
        ${result.singer_name}
    </h2>
    <img src='${result.singer_pic}'/>
</body>
</html>`);
});

app.listen(3000, () => {
  console.log("服务器已经启动...");
});
```
## 06 响应设置

```typescript
const express = require("express");

// 创建应用对象
const app = express();

// 创建路由
app.get("/:id.html", (req, res) => {
  // 原生响应
  res.statusCode = 404; // 状态码
  res.statusMessage = "lo000000000 --"; // 状态信息
  res.setHeader("aaa", "bbb"); // 响应头
  res.write("hello !!!!!!!!!"); // 响应体 和 res.end() 一起使用
  res.send("response"); // 状态码
  res.end();

  // express 响应
  res.status(500);
  res.set("ass", "bb");
  res.send("你好 eee");
  // res.status(500).set("ass", "bb").send("你好 eee");

  // 其它响应
  // 重定向
  res.redirect("https://www.baidu.com/");
  // 下载响应  文件路径
  res.download(__dirname + "/package.json");
  // JSON 响应
  res.json({
    name:'尚硅谷',
    slogon:'1231231dasda'
  });
  // 响应文件内容
  res.sendFile(__dirname + '/1.html') // 绝对路径使用 path.resolve()
});

app.listen(3000, () => {
  console.log("服务器已经启动...");
});
```
## 07 中间件（函数）

```typescript
const express = require("express");
const fs = require("fs");
const path = require("path");

// 创建应用对象
const app = express();

// 声明中间件函数 请求，响应，
function redirectMiddleware(req, res, next) {
  // 获取 url 和 ip
  let { url, ip } = req;
  // 将信息保存在文件中 access.log 中
  fs.appendFileSync(path.resolve(__dirname, "./access.log"), `${url} ${ip}\r\n`);
  // 调用next
  next();
}
// 使用 全局中间件函数
app.use(redirectMiddleware);

// 创建路由
app.get("/home", (req, res) => {
  res.send("首页");
});

app.get("/admin", (req, res) => {
  res.send("后台");
});

app.get("*", (req, res) => {
  res.send("not found 404");
});

app.listen(3000, () => {
  console.log("服务器已经启动...");
});

```
## 08 路由中间件

```typescript
const express = require("express");
const fs = require("fs");
const path = require("path");
const { send } = require("process");

// 创建应用对象
const app = express();

// 声明中间件函数 请求，响应，
let redirectMiddleware = (req, res, next) => {
  // 判断 url 中是否 code 参数等于 521
  if (req.query.code === "521") {
    next();
  } else {
    res.send("暗号错误");
  }
};

// 创建路由
app.get("/home", (req, res) => {
  res.send("首页");
});

app.get("/admin", redirectMiddleware, (req, res) => {
  res.send("后台");
});

app.get("/setting", redirectMiddleware, (req, res) => {
  res.send("设置页面");
});

app.listen(3000, () => {
  console.log("服务器已经启动...");
});

```
## 09 静态资源

```typescript
const express = require("express");

// 创建应用对象
const app = express();

// 静态资源中间件设置
app.use(express.static(__dirname+'/public'))

// 创建路由
app.get("/home", (req, res) => {
  
});

app.listen(3000, () => {
  console.log("服务器已经启动...");
});

```
## 10 获取请求体数据

```typescript
const express = require("express");
/**
 * get  /login 显示表单网页
 * post /login 获取表单中的 用户名 和 密码
 */
// 创建应用对象
const app = express();

// 解析请求体中间件 npm i body-parser
const bodrParser = require("body-parser");
// 解析 json 格式 的请求体中间件
const jsonParser = bodrParser.json();
// 解析 querystring 格式 的请求体中间件
const urlencodedParser = bodrParser.urlencoded({ extended: false });

// 创建路由
app.get("/login", (req, res) => {
  res.sendFile(__dirname + "/11_form.html");
});

// urlencodedParser 中间件会向 req 添加 body 属性
app.post("/login",urlencodedParser, (req, res) => {
  // 响应 html 文件内容
  // 获取yingqi
  console.log(req.body) // [Object: null prototype] { username: '1907050101', password: '123123' }
  res.send("登录成功");
});

app.listen(3000, () => {
  console.log("服务器已经启动...");
});

```
## 11 防盗链

```typescript
const express = require("express");

// 创建应用对象
const app = express();

// 声明 防盗链 中间件设置
app.use((req, res, next) => {
  // 检测请求头中 referer 是否为127.0.0.1
  // 获取 referer
  let referer = req.get("referer");
  console.log(referer);
  // http://127.0.0.1:3000/
  // http://localhost:3000/

  if (referer) {
    // 实例化
    let url = new URL(referer);
    // 获取 hostname
    let hostname = url.hostname;
    // 判断
    if(hostname !== '127.0.0.1'){
        // 响应 404
        res.status(404).send('<h1>404 not found</h1>')
        return 
    }
  }
  next();
});

// 静态资源中间件设置
app.use(express.static(__dirname + "/public"));

app.listen(3000, () => {
  console.log("服务器已经启动...");
});

```
## 12 路由模块化

![图片](data:image/png;base64,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)


adminRouter.js

```typescript
// 1. 导入 express
const express = require('express')

// 2. 创建路由对象
const router =express.Router()

// 3. 创建路由规则
// 创建路由
router.get('/home',(req,res)=>{
    res.send('后台首页')
})

// 创建路由
router.get('/setting',(req,res)=>{
    res.send('设置搜索')
})

// 4. 暴露路由
module.exports = router
```
homerouter.js
```typescript
// 1. 导入 express
const express = require('express')

// 2. 创建路由对象
const router =express.Router()

// 3. 创建路由规则
// 创建路由
router.get('/home',(req,res)=>{
    res.send('前台首页')
})

// 创建路由
router.get('/search',(req,res)=>{
    res.send('内容搜索')
})

// 4. 暴露路由
module.exports = router
```
主文件
```typescript
const express = require("express");

// 引入 router
const homeRouter = require('./router/homerouter')
const adminRouter = require('./router/adminRouter')
// 创建应用对象
const app = express();

// 设置
app.use(homeRouter)
app.use(adminRouter)

app.listen(3000, () => {
  console.log("服务器已经启动...");
});

```
13 ejs
html

```typescript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    <h2>我爱你 <%= china%></h2>
    <p><%= weater%></p>
</body>
</html>
```
js
```typescript
// 1. 安装 ejs npm i ejs --save
// 2. 导入
const ejs = require('ejs')
const fs = require('fs')

// 字符串
let china = '中国'
let weater = '天气不错'

// 声明 变量 所在文件
let str  = fs.readFileSync('./14.html').toString()

// 使用 ejs 渲染
let result = ejs.render(str,{china:china,weater})

console.log(result)
```
## 13 express-generator

 npm install -g express-generator

express -e 文件名

npm start 运行

## 14 文件上传

npm i formidable















.

