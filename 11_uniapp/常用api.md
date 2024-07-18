# 缓存

### uni.setStorageSync(KEY,DATA)

将 data 存储在本地缓存中指定的 key 中，会覆盖掉原来该 key 对应的内容，这是一个同步接口。

|参数|类型|必填|说明|
|:----|:----|:----|:----|
|key|String|是|本地缓存中的指定的 key|
|data|Any|是|需要存储的内容，只支持原生类型、及能够通过 JSON.stringify 序列化的对象|

### uni.getStorageSync(KEY)

从本地缓存中同步获取指定 key 对应的内容。

|参数|类型|必填|说明|
|:----|:----|:----|:----|
|key|String|是|本地缓存中的指定的 key|

* 各个小程序端为其自带的storage api，数据存储生命周期跟小程序本身一致，即除用户主动删除或超过一定时间被自动清理，否则数据都一直可用。
* 微信小程序单个 key 允许存储的最大数据长度为 1MB，所有数据存储上限为 10MB。
# 交互

### uni.showToast(OBJECT)

显示消息提示框。

OBJECT参数说明

|参数|类型|必填|说明|平台差异说明|
|:----|:----|:----|:----|:----|
|title|String|是|提示的内容，长度与 icon 取值有关。|    |
|icon|String|否|图标，有效值详见下方说明，默认：success。|    |
|image|String|否|自定义图标的本地路径（app端暂不支持gif）|App、H5、微信小程序、百度小程序、抖音小程序（2.62.0+）|
|mask|Boolean|否|是否显示透明蒙层，防止触摸穿透，默认：false|App、微信小程序、抖音小程序（2.47.0+）|
|duration|Number|否|提示的延迟时间，单位毫秒，默认：1500|    |
|position|String|否|纯文本轻提示显示位置，填写有效值后只有 title 属性生效，且不支持通过 uni.hideToast 隐藏。有效值详见下方说明。|App|
|success|Function|否|接口调用成功的回调函数|    |
|fail|Function|否|接口调用失败的回调函数|    |
|complete|Function|否|接口调用结束的回调函数（调用成功、失败都会执行）|    |

icon 值说明

|值|说明|平台差异说明|
|:----|:----|:----|
|success|显示成功图标，此时 title 文本在小程序平台最多显示 7 个汉字长度，App仅支持单行显示。|支付宝小程序无长度无限制|
|error|显示错误图标，此时 title 文本在小程序平台最多显示 7 个汉字长度，App仅支持单行显示。|支付宝小程序、快手小程序、抖音小程序、百度小程序、京东小程序、QQ小程序不支持|
|fail|显示错误图标，此时 title 文本无长度显示。|支付宝小程序、抖音小程序|
|exception|显示异常图标。此时 title 文本无长度显示。|支付宝小程序|
|loading|显示加载图标，此时 title 文本在小程序平台最多显示 7 个汉字长度。|支付宝小程序不支持|
|none|不显示图标，此时 title 文本在小程序最多可显示两行。|    |

position 值说明（仅App生效）

|值|说明|
|:----|:----|
|top|居上显示|
|center|居中显示|
|bottom|居底显示|

### uni.hideToast()

隐藏消息提示框。

### uni.showLoading(OBJECT)

显示 loading 提示框, 需主动调用 [uni.hideLoading](https://uniapp.dcloud.net.cn/api/ui/prompt#hideloading) 才能关闭提示框。

OBJECT参数说明

|参数|类型|必填|说明|平台差异说明|
|:----|:----|:----|:----|:----|
|title|String|是|提示的文字内容，显示在loading的下方|    |
|mask|Boolean|否|是否显示透明蒙层，防止触摸穿透，默认：false|H5、App、微信小程序、百度小程序、抖音小程序（2.47.0+）|
|success|Function|否|接口调用成功的回调函数|    |
|fail|Function|否|接口调用失败的回调函数|    |
|complete|Function|否|接口调用结束的回调函数（调用成功、失败都会执行）|    |

### uni.hideLoading()

隐藏 loading 提示框。

# 登录

### uni.getUserProfile(OBJECT)

>微信小程序端基础库2.27.1及以上版本，[wx.getUserProfile 接口](https://developers.weixin.qq.com/miniprogram/dev/api/open-api/user-info/wx.getUserProfile.html)被收回，详见[《小程序用户头像昵称获取规则调整公告》](https://developers.weixin.qq.com/community/develop/doc/00022c683e8a80b29bed2142b56c01)。
获取用户信息。每次请求都会弹出授权窗口，用户同意后返回 userInfo。

* 该API仅支持微信小程序端（基础库2.10.4-2.27.0版本），微信小程序调整了相关接口（详见[《小程序登录、用户信息相关接口调整说明》](https://developers.weixin.qq.com/community/develop/doc/000cacfa20ce88df04cb468bc52801?highLine=getUserProfile%253Afail)）。每次触发 uni.getUserProfile 均会弹出授权窗口，用户授权后可成功获取用户信息。该API暂不支持在事件中使用异步操作，否则会触发错误：{errMsg: "getUserProfile:fail can only be invoked by user TAP gesture."}
OBJECT 参数说明

|参数名|类型|必填|说明|
|:----|:----|:----|:----|
|desc|String|是|声明获取用户个人信息后的用途，不超过30个字符|
|lang|String|否|指定返回用户信息的语言，默认为 en。更多值请参考下面的说明。|
|success|Function|否|接口调用成功的回调|
|fail|Function|否|接口调用失败的回调函数|
|complete|Function|否|接口调用结束的回调函数（调用成功、失败都会执行）|

注意： 可以使用 `if(uni.getUserProfile)` 判断uni.getUserProfile是否可用。

success 返回参数说明

| 参数            | 类型     | 说明                                                      |
| :------------ | :----- | :------------------------------------------------------ |
| userInfo      | OBJECT | 用户信息对象                                                  |
| rawData       | String | 不包括敏感信息的原始数据字符串，用于计算签名。                                 |
| signature     | String | 使用 sha1( rawData + sessionkey ) 得到字符串，用于校验用户信息。         |
| encryptedData | String | 包括敏感数据在内的完整用户信息的加密数据，详细见加密数据解密算法。                       |
| iv            | String | 加密算法的初始向量，详细见加密数据解密算法。                                  |
| cloudID       | String | 敏感数据对应的云 ID，开通云开发的小程序才会返回，可通过云调用直接获取开放数据，详细见云调用直接获取开放数据 |
| errMsg        | String | 描述信息                                                    |

userInfo 参数说明

|参数|类型|说明|
|:----|:----|:----|
|nickName|String|用户昵称|
|avatarUrl|String|用户头像|
|gender|Number|用户性别|
|country|String|用户所在国家|
|province|String|用户所在省份|
|city|String|用户所在城市|
|language|String|显示 country，province，city 所用的语言|

gender 的合法值

|值|说明|
|:----|:----|
|0|未知|
|1|男性|
|2|女性|

# 设备

### uni.getSystemInfo(OBJECT)

异步获取系统信息

OBJECT 参数说明：

|参数名|类型|必填|说明|
|:----|:----|:----|:----|
|success|Function|是|接口调用成功的回调|
|fail|Function|否|接口调用失败的回调函数|
|complete|Function|否|接口调用结束的回调函数（调用成功、失败都会执行）|

### uni.getSystemInfoSync()

获取系统信息的同步接口。`调用参数和返回值同上getSystemInfo`。

# 页面路由

### uni.navigateTo(OBJECT)

保留当前页面，跳转到应用内的某个页面，使用`uni.navigateBack`可以返回到原页面。

OBJECT参数说明

|参数|类型|必填|默认值|说明|平台差异说明|
|:----|:----|:----|:----|:----|:----|
|url|String|是|    |需要跳转的应用内非 tabBar 的页面的路径 , 路径后可以带参数。参数与路径之间使用?分隔，参数键与参数值用=相连，不同参数用&分隔；如 'path?key=value&key2=value2'，path为下一个页面的路径，下一个页面的onLoad函数可得到传递的参数|    |
|animationType|String|否|pop-in|窗口显示的动画效果，详见：[窗口动画](https://uniapp.dcloud.net.cn/api/router#animation)|App（uni-app x 不支持）|
|animationDuration|Number|否|300|窗口动画持续时间，单位为 ms|App（uni-app x 不支持）|
|events|Object|否|    |页面间通信接口，用于监听被打开页面发送到当前页面的数据。2.8.9+ 开始支持。|uni-app x 不支持|
|success|Function|否|    |接口调用成功的回调函数|    |
|fail|Function|否|    |接口调用失败的回调函数|    |
|complete|Function|否|    |接口调用结束的回调函数（调用成功、失败都会执行）|    |

* 页面跳转路径有层级限制，不能无限制跳转新页面
* 跳转到 tabBar 页面只能使用 switchTab 跳转
* 路由API的目标页面必须是在pages.json里注册的vue页面。
### uni.redirectTo(OBJECT)

关闭当前页面，跳转到应用内的某个页面。

OBJECT参数说明

|参数|类型|必填|说明|
|:----|:----|:----|:----|
|url|String|是|需要跳转的应用内非 tabBar 的页面的路径，路径后可以带参数。参数与路径之间使用?分隔，参数键与参数值用=相连，不同参数用&分隔；如 'path?key=value&key2=value2'|
|success|Function|否|接口调用成功的回调函数|
|fail|Function|否|接口调用失败的回调函数|
|complete|Function|否|接口调用结束的回调函数（调用成功、失败都会执行）|

* 跳转到 tabBar 页面只能使用 switchTab 跳转
### uni.navigateBack(OBJECT)

关闭当前页面，返回上一页面或多级页面。可通过 `getCurrentPages()` 获取当前的页面栈，决定需要返回几层。

OBJECT参数说明

|参数|类型|必填|默认值|说明|平台差异说明|
|:----|:----|:----|:----|:----|:----|
|delta|Number|否|1|返回的页面数，如果 delta 大于现有页面数，则返回到首页。|    |
|animationType|String|否|pop-out|窗口关闭的动画效果，详见：[窗口动画](https://uniapp.dcloud.net.cn/api/router#animation)|App（uni-app x 不支持）|
|animationDuration|Number|否|300|窗口关闭动画的持续时间，单位为 ms|App（uni-app x 不支持）|
|success|Function|否|    |接口调用成功的回调函数|    |
|fail|Function|否|    |接口调用失败的回调函数|    |
|complete|Function|否|    |接口调用结束的回调函数（调用成功、失败都会执行）|    |

# 视频图片

### uni.chooseMedia(OBJECT)

拍摄或从手机相册中选择图片或视频。

OBJECT 参数说明

|参数名|类型|默认值|必填|说明|
|:----|:----|:----|:----|:----|
|count|Number|9（注意：ios不可大于9）|否|最多可以选择的文件个数|
|mediaType|Array.<string>|['image', 'video']|否|文件类型|
|sourceType|Array.<string>|['album', 'camera']|否|图片和视频选择的来源|
|maxDuration|Number|10|否|拍摄视频最长拍摄时间，单位秒。时间范围为 3s 至 30s 之间|
|sizeType|Array.<string>|['original', 'compressed']|否|仅对 mediaType 为 image 时有效，是否压缩所选文件|
|camera|String|'back'|否|仅在 sourceType 为 camera 时生效，使用前置或后置摄像头|
|success|function|    |否|接口调用成功的回调函数|
|fail|function|    |否|接口调用失败的回调函数|
|complete|function|    |否|接口调用结束的回调函数（调用成功、失败都会执行）|

OBJECT.mediaType 合法值

|值|说明|
|:----|:----|
|image|只能拍摄图片或从相册选择图片|
|video|只能拍摄视频或从相册选择视频|
|mix|可同时选择图片和视频|

OBJECT.sourceType 合法值

|值|说明|
|:----|:----|
|album|从相册选择|
|camera|使用相机拍摄|

OBJECT.camera 合法值

|值|说明|
|:----|:----|
|back|使用后置摄像头|
|front|使用前置摄像头|

success 返回参数说明

|参数名|类型|说明|
|:----|:----|:----|
|tempFiles|Array.<string>|本地临时文件列表|
|type|String|文件类型，有效值有 image 、video、mix|

res.tempFiles 的结构

|参数名|类型|说明|
|:----|:----|:----|
|tempFilePath|String|本地临时文件路径 (本地路径)|
|size|Number|本地临时文件大小，单位 B|
|duration|Number|视频的时间长度|
|height|Number|视频的高度|
|width|Number|视频的宽度|
|thumbTempFilePath|String|视频缩略图临时文件路径|
|fileType|String|文件类型|

fileType 合法值

|值|说明|
|:----|:----|
|image|图片|
|video|视频|

# 上传

### uni.uploadFile(OBJECT)

将本地资源上传到开发者服务器，客户端发起一个 `POST` 请求，其中 `content-type` 为 `multipart/form-data`。 如页面通过 [uni.chooseImage](https://uniapp.dcloud.net.cn/api/media/image#chooseimage) 等接口获取到一个本地资源的临时文件路径后，可通过此接口将本地资源上传到指定服务器。

OBJECT 参数说明

|参数名|类型|必填|说明|平台差异说明|
|:----|:----|:----|:----|:----|
|url|String|是|开发者服务器 url|    |
|files|Array|是（files和filePath选其一）|需要上传的文件列表。使用 files 时，filePath 和 name 不生效。|App、H5（ 2.6.15+）|
|fileType|String|见平台差异说明|文件类型，image/video/audio|仅支付宝小程序，且必填。|
|file|File|否|要上传的文件对象。|仅H5（2.6.15+）支持|
|filePath|String|是（files和filePath选其一）|要上传文件资源的路径。|    |
|name|String|是|文件对应的 key , 开发者在服务器端通过这个 key 可以获取到文件二进制内容|    |
|header|Object|否|HTTP 请求 Header, header 中不能设置 Referer。|    |
|timeout|Number|否|超时时间，单位 ms|H5(HBuilderX 2.9.9+)、APP(HBuilderX 2.9.9+)、微信小程序、支付宝小程序、抖音小程序、快手小程序|
|formData|Object|否|HTTP 请求中其他额外的 form data|    |
|success|Function|否|接口调用成功的回调函数|    |
|fail|Function|否|接口调用失败的回调函数|    |
|complete|Function|否|接口调用结束的回调函数（调用成功、失败都会执行）|    |

# 界面

## uni.createSelectorQuery()

返回一个 `SelectorQuery` 对象实例。可以在这个实例上使用 `select` 等方法选择节点，并使用 `boundingClientRect` 等方法选择需要查询的信息。

Tips:

* 使用 `uni.createSelectorQuery()` 需要在生命周期 `mounted` 后进行调用。
* 默认需要使用到 `selectorQuery.in` 方法。


