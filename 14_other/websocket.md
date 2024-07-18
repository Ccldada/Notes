

# 对象版本

```less
// 信息提示
import { ElMessage } from 'element-plus'

function cutAfterHttp(url) {
  return url.split('https')[1] || url.split('http')[1];
}
// WebSocket地址
const url ="ws"+ cutAfterHttp(import.meta.env.VITE_APP_BASE_API)+'/websocket/2'

// WebSocket实例
let ws

// 重连定时器实例
let reconnectTimer

// WebSocket重连开关
let isReconnecting = false

// WebSocket对象
const websocket = {
  // WebSocket建立连接
  Init() {
    // 判断浏览器是否支持WebSocket
    if (!('WebSocket' in window)) {
      ElMessage.error('您的浏览器不支持 WebSocket')
      return
    }
    // 创建WebSocket实例
    ws = new WebSocket(url)
    // 监听WebSocket连接
    ws.onopen = () => {
      ElMessage.success('WebSocket连接成功')
    }

    // 监听WebSocket连接错误信息
    ws.onerror = (e) => {
      console.log('WebSocket重连开关', isReconnecting)
      console.log('WebSocket数据传输发生错误', e)
      ElMessage.error('WebSocket传输发生错误')
      // 打开重连
      reconnect()
    }

    // 监听WebSocket接收消息
    ws.onmessage = (e) => {
      console.log('WebSocket接收后端消息:' + e.data)
      // 心跳消息不做处理
      if (e.data === 'ok') {
        return
      }

      // 调用回调函数处理接收到的消息
      if (websocket.onMessageCallback) {
        websocket.onMessageCallback(e.data)
      }

      ElMessage.success(e.data)
    }
  },

  // WebSocket连接关闭方法
  Close() {
    // 关闭断开重连机制
    isReconnecting = true
    ws.close()
    ElMessage.error('WebSocket断开连接')
  },

  // WebSocket发送信息方法
  Send(data) {
    // 处理发送数据JSON字符串
    const msg = JSON.stringify(data)
    // 发送消息给后端
    ws.send(msg)
  },

  // 暴露WebSocket实例，其他地方调用就调用这个
  getWebSocket() {
    return ws
  },

  // 新增回调函数用于处理收到的消息
  onMessageCallback: null,

  // 设置消息处理回调函数
  setMessageCallback(callback) {
    this.onMessageCallback = callback
  }
}

// 监听窗口关闭事件，当窗口关闭时-每一个页面关闭都会触发-扩张需求业务
window.onbeforeunload = function () {
  // 在窗口关闭时关闭 WebSocket 连接
  websocket.Close()
  console.log('WebSocket窗口关闭事件触发')
}

// 浏览器刷新重新连接
// 刷新页面后需要重连-并且是在登录之后
if (performance.getEntriesByType('navigation')[0].type === 'reload') {
  console.log('WebSocket浏览器刷新了')

  // 延迟一定时间再执行 WebSocket 初始化，确保页面完全加载后再进行连接
  setTimeout(() => {
    console.log('WebSocket执行刷新后重连...')
    // 刷新后重连
    websocket.Init()
  }, 200) // 适当调整延迟时间
}

// 重连方法
function reconnect() {
  console.log('WebSocket重连开关', isReconnecting)
  // 判断是否主动关闭连接
  if (isReconnecting) {
    return
  }
  // 重连定时器-每次WebSocket错误方法onerror触发它都会触发
  reconnectTimer && clearTimeout(reconnectTimer)
  reconnectTimer = setTimeout(function () {
    // console.log('WebSocket执行断线重连...')
    websocket.Init()
    isReconnecting = false
  }, 4000)
}

// 暴露对象
export default websocket
```

# class版本

[https://juejin.cn/post/7359900973991067685](https://juejin.cn/post/7359900973991067685)

```less
class `WebSocketHeartbeat` {
  constructor(url) {
    this.url = url;
    this.pingTimeout = 10000; // 心跳消息的发送间隔
    this.pongTimeout = 1000; // pong消息允许的最大延时
    this.pingTimeoutId = null; // 用于发送心跳消息的延时器
    this.pongTimeoutId = null; // 用于检测服务器是否及时返回pong消息的延时器
    this.isReconnecting = false; // 用于记录当前是否处于重连的状态
    this.reconnectTimeout = 1000; // 尝试重连的时间间隔
    this.isForbiddenReconnect = false; // 用于标记是否要尝试重连
    this.onopen = () => {};
    this.onclose = () => {};
    this.onmessage = () => {};
    this.onerror = () => {};
    this.createWebSocket();
  }
  // 创建一个websocket实例
  createWebSocket() {
    this.ws = new WebSocket(this.url);
    this.init();
  }
  // 添加事件监听
  init() {
    this.ws.onopen = (e) => this.handleOpen(e);
    this.ws.onclose = (e) => this.handleClose(e);
    this.ws.onerror = (e) => this.handleError(e);
    this.ws.onmessage = (e) => this.handleMessage(e);
  }
  handleOpen(e) {
    console.log("websocket连接已建立", e);
    this.onopen(e);
    // 开始进行心跳检测
    this.checkHeartbeat();
  }
  handleMessage(e) {
    console.log("接收到的消息内容", e.data);
    this.onmessage(e);
    // 收到消息后，再次进行心跳检测
    this.checkHeartbeat();
  }
  handleClose(e) {
    console.log("websocket连接关闭了", e);
    this.onclose(e);
    // 尝试重连操作
    this.reconnect();
  }
  handleError(e) {
    console.log("websocket连接出错", e);
    this.onerror(e);
    this.reconnect();
  }
  send(data) {
    this.ws.send(data);
  }
  // 用于用户主动断开连接
  close() {
    this.ws.close();
    this.resetHeartbeat();
    this.isForbiddenReconnect = true;
  }
  // 原生的WebSocket并没有提供reconnect APi，
  // 因此我们此处reconnect实际上做的操作是重新创建一个WebSocket实例
  reconnect() {
    console.log("进行重连~");
    // 在重新创建连接的时候应该确保一次只创建一个，并且不要频繁创建
    if (this.isReconnecting || this.isForbiddenReconnect) return;
    this.isReconnecting = true;
    setTimeout(() => {
      this.createWebSocket();
      this.isReconnecting = false;
    }, this.reconnectTimeout);
  }
  checkHeartbeat() {
    this.resetHeartbeat();
    this.tryHeartbeat();
  }
  resetHeartbeat() {
    clearTimeout(this.pingTimeoutId);
    clearTimeout(this.pongTimeoutId);
  }
  tryHeartbeat() {
    this.pingTimeoutId = setTimeout(() => {
      this.send("heartbeat");
      // 如果超过一定时间（约定的pong返回时间）还没有接收到消息，此时需要进行重连操作
      this.pongTimeoutId = setTimeout(() => {
        console.log("需要进行重连操作");
        this.reconnect();
      }, this.pongTimeout);
    }, this.pingTimeout);
  }
}

export { WebSocketHeartbeat };

// 心跳消息（ping）的发送时机：
// 首先是发送间隔，这个应该是可以设置的
// 在建立websocket连接之后就开始尝试发送一次心跳消息
// 服务端会返回pong消息或者其它消息给客户端，此时再次尝试发送一次心跳消息

```
使用
```less
import { WebSocketHeartbeat } from './websocket-heartbeat.js'
const ws = new WebSocketHeartbeat('ws://localhost:3003')
ws.onmessage = ({ data }) => {
  console.log('用户处理消息', data);
}

const messageEl = document.querySelector('.message')
const sendBtnEl = document.querySelector('.send-btn')
const closeBtnEl = document.querySelector('.close-btn')
sendBtnEl.addEventListener('click', () => {
  ws.send(messageEl.value)
})
closeBtnEl.addEventListener('click', () => {
  ws.close()
})
```
.
