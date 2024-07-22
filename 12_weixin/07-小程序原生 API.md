## 微信原生 API



### 1. API 基础



小程序开发框架提供丰富的微信原生 `API`，可以方便的调起微信提供的能力，如获取用户信息，本地存储，支付功能等，几乎所有小程序的 `API` 都挂载在 `wx` 对象底下，例如：`wx.chooseMedia()`、`wx.request()`, `wx` 对象实际上就是小程序的宿主环境微信所提供的全局对象



通常，在小程序 API 有以下几种类型：

1. 事件监听 API：约定以 `on` 开头 API 用来监听某个事件是否触发，例如：`wx.onThemeChange()`
2. 同步 API：约定以 `Sync` 结尾的 API 都是同步 API，例如：`wx.setStorageSync()`
3. 异步 API：大多数 API 都是异步 API，例如：`wx.setStorage()`



![](http://8.131.91.46:6677/mina/base/小程序 API 类型.png)



异步 API 支持 callback & Promise 两种调用方式：

1. 当接口参数 Object 对象中不包含 success/fail/complete 时将默认返回 Promise

2. 部分接口如 request, uploadFile 本身就有返回值，因此不支持 Promise 风格的调用方式，它们的 promisify 需要开发者自行封装。





[小程序 API 介绍](https://developers.weixin.qq.com/miniprogram/dev/framework/app-service/api.html)

[微信小程序 API 文档](https://developers.weixin.qq.com/miniprogram/dev/api/)







### 2. 网络请求



**知识点：**



在微信小程序中，如果需要发起 HTTPS 网络请求需要使用：`wx.request()`，语法如下：



```js
wx.request({
  // 接口地址，仅为示例，并非真实的接口地址
  url: 'example.php',
  // 请求的参数
  data: { x: '' },
  // 请求方式
  method: 'GET|POST|PUT|DELETE',
  success (res) {
    console.log(res.data)
  },
  fail(err) {
    console.log(err)
  }
})
```



<font color="red">**注意：wx.request() 请求的域名需要在小程序管理平台进行配置，如果小程序正式版使用wx.request请求未配置的域名，在控制台会有相应的报错。**</font>



![](http://8.131.91.46:6677/mina/base/038-网络请求错误.png)



这时候就需要在小程序管理后台进行设置请求的域名，打开微信公众后台：点击左侧 开发 → 开发管理 → 开发设置 → 服务器域名。**域名只支持 `https` 而且要求已备案**


![](http://8.131.91.46:6677/mina/base/39-开发设置.png)

![](http://8.131.91.46:6677/mina/base/40-配置服务器域名.png)



但一般我们在开发阶段时，处于开发阶段的服务器接口可能还没部署到对应的域名下，经常会通过另一个域名来进行开发调试，考虑到这一点，为了方便开发者进行开发调试，开发者工具、小程序的开发版和小程序的体验版在某些情况下允许 `wx.request` 请求任意域名 (只适用于开发环境，只能在小程序开发者工具中生效)，在开发工具中设置步骤如下：



将 **不校验合法域名、web-view (业务域名)、TLS版本以及HTTPS证书** 勾选上：

​![](http://8.131.91.46:6677/mina/base/41-不校验合法域名.png)
![](http://8.131.91.46:6677/mina/base/41-不校验合法域名-1.png)



> 📌 **注意事项：**
>
> ​	这两种方式只适用于开发者工具、小程序的开发版和小程序的体验版
>
> ​	项目上线前必须在小程序管理平台进行合法域名的配置



**落地代码：**



```js
Page({
  // 页面的初始数据
  data: {},

  // 获取数据
  getPostInfo() {
    wx.request({
      url: 'https://jsonplaceholder.typicode.com/posts/1',
      method: 'GET',
      success(res) {
        console.log(res)
      },
      fail(err) {
        console.log(err)
      }
    })
  },
    
  // coding...
}
```









### 3. 界面交互



小程序还提供了一些用于界面交互的 API，如消息提示框、模态对话框、 loading 提示框等等



#### 3.1 loading 提示框



**知识点：**



小程序提供了一些用于界面交互的 API，例如： loading 提示框、消息提示框、模态对话框等 API。

![](http://8.131.91.46:6677/mina/base/loading 提示框.gif)



loading 提示框常配合网络请求来使用，用于增加用户体验，对应的 API 有两个：

1. `wx.showLoading` 显示加载提示框
2. `wx.hideLoading` 隐藏加载提示框



语法如下：

```js
wx.showLoading({
  title: '提示内容', // 提示的内容
  mask: true, // 是否显示透明蒙层，防止触摸穿透
  success() {}, // 接口调用成功的回调函数
  fail() {} // 接口调用失败的回调函数
})
```





官方文档：

[wx.showLoading 官方文档](https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.showLoading.html)

[wx.hideLoading 官方文档](https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.hideLoading.html)





**落地代码：**

```js
Page({

  data: {
    list: []
  },

  // 获取数据
  getData () {

+     // 显示 loading 提示框
+     wx.showLoading({
+       // 用来显示提示的内容
+       // 提示的内容不会自动换行，如果提示的内容比较多，因为在同一行展示
+       // 多出来的内容就会被隐藏
+       title: '数据加载中...',
+       // 是否展示透明蒙层，防止触摸穿透
+       mask: true
+     })

    // 如果需要发起网络请求，需要使用 wx.request API
    wx.request({
      // 接口地址
      url: 'https://gmall-prod.atguigu.cn/mall-api/index/findBanner',
      // 请求方式
      method: 'GET',
      // 请求参数
      data: {},
      // 请求头
      header: {},
      // API 调用成功以后，执行的回调
      success: (res) => {
        // console.log(res)
        if (res.data.code === 200) {
          this.setData({
            list: res.data.data
          })
        }
      },
      // API 调用失败以后，执行的回调
      fail: (err) => {
        console.log(err)
      },
      // API 不管调用成功还是失败以后，执行的回调
      complete: (res) => {
        // console.log(res)

+         // 关掉 loading 提示框
+         // hideLoading 和 showLoading 必须结合、配对使用才可以
+         wx.hideLoading()
      }
    })

  }

})

```





#### 3.2 模态对话框以及消息提示框



**知识点：**



`wx.showToast()`：消息提示框用来根据用户的某些操作来告知操作的结果，如退出成功给用户提示，提示删除成功等，语法如下：



```js
wx.showToast({
  title: '标题', // 提示的内容
  duration: 2000, // 提示的延迟时间
  mask: true, // 是否显示透明蒙层，防止触摸穿透
  icon: 'success', // 	图标
  success() {}, // 接口调用成功的回调函数
  fail() {} // 接口调用失败的回调函数
})
```



`wx.showModal()` 模态对话框也是在项目中频繁使用的一个小程序 `API`，通常用于向用户询问是否执行一些操作，例如：点击退出登录，显示模态对话框，询问用户是否真的需要退出等等

```js
wx.showModal({
  title: '提示', // 提示的标题
  content: '您确定执行该操作吗？', // 提示的内容
  confirmColor: '#f3514f',
  // 接口调用结束的回调函数（调用成功、失败都会执行）
  success({ confirm }) {
    confirm && consle.log('点击了确定')
  }
})
```





官方文档：

[wx.showToast 官方文档](https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.showToast.html)

[wx.showModal 官方文档](https://developers.weixin.qq.com/miniprogram/dev/api/ui/interaction/wx.showModal.html)



![](http://8.131.91.46:6677/mina/base/模态对话框以及消息提示框.gif)





**落地代码：**



```js
Page({
  // coding...
  
  // 删除商品
  async delHandler () {

    // showModal 显示模态对话框
    const { confirm } = await wx.showModal({
      title: '提示',
      content: '是否删除该商品 ?'
    })

    if (confirm) {
      // showToast 消息提示框
      wx.showToast({
        title: '删除成功',
        icon: 'none',
        duration: 2000
      })
    } else {
      wx.showToast({
        title: '取消删除',
        icon: 'error',
        duration: 2000
      })
    }

  }
   
  // coding...

})
```









### 4. 本地存储



小程序中也能够像网页一样支持本地数据缓存，本地数据缓存是小程序存储在当前设备上硬盘上的数据，本地数据缓存有非常多的用途，我们可以利用本地数据缓存来存储用户在小程序上产生的操作，在用户关闭小程序重新打开时可以恢复之前的状态。我们还可以利用本地缓存一些服务端非实时的数据提高小程序获取数据的速度，在特定的场景下可以提高页面的渲染速度，减少用户的等待时间。其包含以下 8个主要的 API



| 同步 API               | 异步 API              | 作用                                |
| ---------------------- | --------------------- | ----------------------------------- |
| `wx.setStorageSync`    | `wx.setStorage`       | 将数据存储在本地缓存中指定的 key 中 |
| `wx.getStorageSync`    | `wx.getStorage`       | 从本地缓存中同步获取指定 key 的内容 |
| `wx.removeStorageSync` | `wx.removeStorage`    | 从本地缓存中移除指定 key            |
| `wx.clearStorageSync`  | `wx.clearStorageSync` | 清理本地数据缓存                    |



![](http://8.131.91.46:6677/mina/base/本地存储.png)



异步方式的 `API`，在调用的时候都需要传入对象类型的参数

同步方式执行的 API 在使用时简洁比较好，缺点是同步会阻塞程序执行，执行效率上相较异步版本要差一些。



> 📌 **注意事项**：
>
> 1. 对象类型的数据，可以直接进行存储，无需使用 `JSON.stringify` 转换
> 2. 对象类型的数据存的时候没有使用转换，因此获取的时候也不需要使用 `JSON.parse` 转换



**落地代码：**

```html
<button type="primary" bindtap="handler" size="mini" plain bindtap="setData">
  存储数据
</button>

<button type="primary" bindtap="handler" size="mini" plain bindtap="getData">
  获取数据
</button>

<button type="warn" bindtap="handler" size="mini" plain bindtap="delData">
  删除数据
</button>

<button type="warn" bindtap="handler" size="mini" plain bindtap="clearData">
  移除数据
</button>
```



```js
Page({

  // 将数据存储到本地
  setStorage () {

    // 第一个参数：本地存储中指定的 key
    // 第二个参数：需要存储的数据
    // wx.setStorageSync('num', 1)

    // 在小程序中
    // 如果存储的是对象类型数据，不需要使用 JSON.stringify 和 JSON.parse 进行转换
    // 直接进行存储和获取即可
    // wx.setStorageSync('obj', { name: 'tom', age: 10 })

    // ------------------- 异步 API -------------------
 
    wx.setStorage({
      key: 'num',
      data: 1
    })

    wx.setStorage({
      key: 'obj',
      data: { name: 'jerry', age: 18 }
    })

  },

  // 获取本地存储的数据
  async getStorage () {

    // 从本地存储的数据中获取指定 key 的数据、内容
    // const num = wx.getStorageSync('num')
    // const obj = wx.getStorageSync('obj')

    // console.log(num)
    // console.log(obj)

    // ------------------- 异步 API -------------------

    const { data } = await wx.getStorage({
      key: 'obj'
    })

    console.log(data)

  },

  // 删除本地存储的数据
  removeStorage () {

    // 从本地移除指定 key 的数据、内容
    // wx.removeStorageSync('num')

    // ------------------- 异步 API -------------------

    wx.removeStorage({
      key: 'num'
    })

  },

  // 清空本地存储的全部数据
  clearStorage () {
    // wx.clearStorageSync()
      
    // ------------------- 异步 API -------------------

    wx.clearStorage()
  },

})

```





### 5. 路由与通信



**知识点：**



在小程序中实现页面的跳转，有两种方式：

1. 声明式导航：`navigator` 组件
2. 编程式导航：使用小程序提供的 `API`
   - `wx.navigateTo()`：保留当前页面，跳转到应用内的某个页面，但是不能跳到 tabbar 页面
   - `wx.redirectTo()`：关闭当前页面，跳转到应用内的某个页面。但是不允许跳转到 tabbar 页面
   - `wx.switchTab()`：跳转到 tabBar 页面，路径后不能带参数
   - `wx.navigateBack()`：关闭当前页面，返回上一页面或多级页面
3. **路径后可以带参数，参数需要在跳转到的页面的 `onLoad` 钩子函数中通过形参进行接收**
   - 参数与路径之间使用 `?` 分隔
   - 参数键与参数值用 `=` 相连
   - 不同参数用 `&` 分隔
   - 例如 `path?key=value&key2=value2`



![](http://8.131.91.46:6677/mina/base/路由与通信.png)



**落地代码：**

```js
Page({


  navigateTo() {

    // 保留当前页面，跳转到应用中其他页面，不能跳转到 tabBar 页面
    wx.navigateTo({
      url: '/pages/list/list?id=1&name=tom'
      // url: '/pages/cate/cate'
    })

  },

  redirectTo() {

    // 关闭(销毁)当前页面，跳转到应用中其他页面，不能跳转到 tabBar 页面
    wx.redirectTo({
      url: '/pages/list/list?id=1&name=tom'
      // url: '/pages/cate/cate'
    })

  },

  switchTab() {

    // 跳转到 tabBar 页面，不能跳转到 非 tabBar 页面，路径后面不能传递参数
    wx.switchTab({
      // url: '/pages/list/list'
      url: '/pages/cate/cate?id=1&name=tom'
    })

  },

  reLaunch() {

    // 关闭所有的页面，然后跳转到应用中某一个页面
    wx.reLaunch({
      url: '/pages/list/list?id=1&name=tom'
      // url: '/pages/cate/cate?id=1&name=tom'
    })

  }

})
```



```js
// list.js
Page({

  navigateBack() {

    // 关闭当前页面，返回上一页或者返回多级页面
    // 默认返回上一页
    wx.navigateBack({
      delta: 1
    })

  },

  onLoad(options) {
    console.log(options)
  }

})

```





















### 6. 事件监听-上拉加载更多



上拉加载是小程序中常见的一种加载方式，当用户滑动页面到底部时，会自动加载更多的内容，以便用户继续浏览



小程序中实现上拉加载的方式：

1.在 app.json 或者 page.json 中配置距离页面底部距离： onReachBottomDistance；默认 50px

2.在 页面.js 中定义 onReachBottom 事件监听用户上拉加载


![](http://8.131.91.46:6677/mina/base/上拉加载更多案例.gif)




**落地代码：**



```html
<view wx:for="{{ numList }}" wx:key="*this">{{ item }}</view>
```



```scss
/* pages/market/market.wxss */

view {
  height: 400rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

view:nth-child(odd) {
  background-color: lightskyblue;
}

view:nth-child(even) {
  background-color: lightsalmon;
}
```



```js
Page({

  data: {
    numList: [1, 2, 3]
  },

  // 监听用户上拉加载
  onReachBottom() {
    // console.log('监听用户上拉加载')
    // 产品需求：
    // 当用户上拉，需要数字进行累加

    // 当用户上拉加载时，需要对数字进行累加，每次累加 3 个数字
    // 目前是 [1, 2, 3]，[1, 2, 3, 4, 5, 6]
    // 怎么进行追加 ？
    // 获取目前数组中最后一项 n，n + 1, n + 2, n + 3

    wx.showLoading({
      title: '数据加载中...'
    })

    setTimeout(() => {
      // 获取数组的最后一项
      const lastNum = this.data.numList[this.data.numList.length - 1]
      // 定义需要追加的元素
      const newArr = [lastNum + 1, lastNum + 2, lastNum + 3]

      this.setData({
        numList: [...this.data.numList, ...newArr]
      })

      wx.hideLoading()
    }, 1500)

  }

})

```









### 7. 事件监听-下拉刷新



下拉刷新是小程序中常见的一种刷新方式，当用户下拉页面时，页面会自动刷新，以便用户获取最新的内容。



小程序中实现上拉加载更多的方式：

1.在 app.json 或者 page.json 中开启允许下拉，同时可以配置 窗口、loading 样式等 

2.在 页面.js 中定义 onPullDownRefresh 事件监听用户下拉刷新



​![](http://8.131.91.46:6677/mina/base/下拉刷新.gif)
![](http://8.131.91.46:6677/mina/base/下拉刷新案例.gif)  





**落地代码：**



```html
<view wx:for="{{ numList }}" wx:key="*this">{{ item }}</view>
```



```scss
/* pages/market/market.wxss */

view {
  height: 400rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

view:nth-child(odd) {
  background-color: lightskyblue;
}

view:nth-child(even) {
  background-color: lightsalmon;
}
```



```js
Page({

  data: {
    numList: [1, 2, 3]
  },

  // 监听用户上拉加载
  onReachBottom() {
    
     // coding...

  },

  // 监听用户下拉刷新
  onPullDownRefresh () {
    // console.log('监听用户下拉刷新')

    // 产品需求：
    // 当用户上拉加载更多以后，如果用户进行了下拉刷新
    // 需要将数据进行重置
    this.setData({
      numList: [1, 2, 3]
    })

    // 在下拉刷新以后，loading 效果有可能不会回弹回去
    if (this.data.numList.length === 3) {
      wx.stopPullDownRefresh()
    }
  }

})

```













### 8. 增强 scroll-view



#### 8.1 scroll-view 上拉加载



**知识点**：



bindscrolltolower：滚动到底部/右边时触发

lower-threshold：距底部/右边多远时，触发 scrolltolower 事件

enable-back-to-top：**让滚动条返回顶部**，iOS 点击顶部状态栏、安卓双击标题栏时，只支持竖向



![](http://8.131.91.46:6677/mina/base/scroll-上拉加载更多.gif)


**落地代码：**



```html
<scroll-view
  class="scroll-y"
  scroll-y
  lower-threshold="100"
  bindscrolltolower="getMore"
  enable-back-to-top
>

  <view wx:for="{{ arr }}" wx:key="*this">{{ item }}</view>

</scroll-view>
```



```js
// index.js
Page({

  data: {
    arr: [1, 2, 3]
  },

  // 上拉加载更多
  getMore() {

    wx.showLoading({
      title: '数据正在加载中...'
    })

    setTimeout(() => {

      // 记录当前数组的最后一个元素
      let lastNum = this.data.arr[this.data.arr.length - 1]
      // 最后一个元素加 1
      lastNum++
      // 每次向数组中新增三项
      const newArr = [lastNum, lastNum + 1, lastNum + 2]

      this.setData({
        arr: [...this.data.arr, ...newArr]
      })

     // 数据返回，隐藏 Loading
     wx.hideLoading()

     wx.showToast({
       title: '数字请求完毕，上滑继续浏览',
       icon: 'none'
     })

    }, 1000)

  }
})
```







#### 8.2 scroll-view 下拉刷新



**知识点：**



refresher-enabled：开启自定义下拉刷新

refresher-default-style自定义下拉刷新默认样式支持设置 `black | white | none`， none 表示不使用默认样式

refresher-background：自定义下拉刷新区域背景颜色

bindrefresherrefresh：自定义下拉刷新状态回调

refresher-triggered：设置当前下拉刷新状态，(true 下拉刷新被触发，false 表示下拉刷新未被触发，**用来关闭下拉效果**)

![](http://8.131.91.46:6677/mina/base/scroll-下拉刷新.gif)



**落地代码：**



```html
<scroll-view
  class="scroll-y"
  scroll-y
  lower-threshold="100"
  bindscrolltolower="getMore"
  enable-back-to-top
             
  refresher-enabled
  refresher-default-style="black"
  refresher-background="#f7f7f8"
  refresher-triggered
  bindrefresherrefresh="onrefresh"
  refresher-triggered="{{ triggered }}"
>

  <view wx:for="{{ arr }}" wx:key="*this">{{ item }}</view>

</scroll-view>
```



```js
// index.js
Page({

  data: {
    triggered: false, // 控制 scroll-view 下拉刷新效果
    arr: [1, 2, 3]
  },

  // scroll-view 下拉刷新回调函数
  onrefresh() {

    wx.showLoading({
      title: '数据正在加载中...'
    })

    // 定时器模拟网络请求，1 秒后数据返回
    setTimeout(() => {

      // 重置数据
      this.setData({
        arr: [1, 2, 3]
      })

      // 数据返回，隐藏 Loading
      wx.hideLoading()

      wx.showToast({
        title: '下拉刷新完成，数据已重置...',
        icon: 'none'
      })

      // 数据返回，关闭 scroll-view 下拉刷新效果
      this.setData({
        triggered: false
      })

    }, 1000)
  }
})
```







#### 8.3 增强 scroll-view 完整代码



```html
<scroll-view
  scroll-y
  class="scroll-y"

  lower-threshold="100"
  bindscrolltolower="getMore"
  enable-back-to-top

  refresher-enabled
  refresher-default-style="black"
  refresher-background="#f7f7f8"
  bindrefresherrefresh="refreshHandler"
  refresher-triggered="{{isTriggered}}"
>
  
  <view wx:for="{{ numList }}" wx:key="*this">{{ item }}</view>

</scroll-view>
```



```scss
/* pages/index/index.wxss */

.scroll-y {
  height: 100vh;
  background-color: #efefef;
}

view {
  height: 500rpx;
  display: flex;
  align-items: center;
  justify-content: center;
}

view:nth-child(odd) {
  background-color: skyblue;
} 

view:nth-child(even) {
  background-color: lightsalmon;
}

```



```js

Page({

  data: {
    numList: [1, 2, 3],
    isTriggered: false
  },

  // 下拉刷新
  refreshHandler () {

    wx.showToast({
      title: '下拉刷新...'
    })
    
    setTimeout(() => {
      this.setData({
        numList: [1, 2, 3],
        isTriggered: false
      })
    }, 2000)

  },


  // scroll-view 上拉加载更多事件的事件处理函数
  getMore () {

    
    wx.showLoading({
      title: '数据加载中...'
    })

    setTimeout(() => {
      // 获取数组的最后一项
      const lastNum = this.data.numList[this.data.numList.length - 1]
      // 定义需要追加的元素
      const newArr = [lastNum + 1, lastNum + 2, lastNum + 3]

      this.setData({
        numList: [...this.data.numList, ...newArr]
      })

      wx.hideLoading()
    }, 1500)


  }

})

```































