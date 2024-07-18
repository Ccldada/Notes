1. 目录介绍
![[Pasted image 20240717171726.png]]
- .vitepress/cache  缓存文件
	- dist  打包后文件
	- theme  主题
	- config.mts  配置
		- title  标题
		- base  (路径，解决线上样式文件引入失败设置，为git仓库名字)
		- description  描述
		- srcDir  存放文档的路径（index.md 的父目录），VitePress 会基于这个目录来编译和生成静态网站；如果配置为 ., 则是对应的项目根目录，scDir 下 必须要有一个 index.md，配置主页布局
		- themeConfig
			- nav  导航栏设置（右上角）
				- text  标题
				- link  链接路径，这个路径应该是相对于 `srcDir` 的。比如 `/docs/test` 指向的是 `scDir/docs/test.md`
			 - sidebar  侧边栏
			 - socialLinks  github链接
- api-examples.md  介绍自定义模板
- index.md  默认首页文件
	Hero部分位于主页顶部。以下是配置Hero的方法。
```
---
layout: home

hero:
  name: VitePress        # `text` 上方的字符，带有品牌颜色
  text: Vite & Vue powered static site generator.   #hero 部分的主要文字，被定义为 `h1` 标签
  tagline: Lorem ipsum...    # `text` 下方的标语
  image:
    src: /logo.png           # text 和 tagline 区域旁的图片
    alt: VitePress           # 图片的alt文字
  actions:
    - theme: brand           # 按钮的颜色主题，默认为 `brand`
      text: Get Started      # 按钮的标签
      link: /guide/what-is-vitepress # 按钮的目标链接
      target: _blank        #可选的内容 链接的 target 属性，如果使用_blank,可以在浏览器重新打开一个标签
      rel:                  #可选的内容
    - theme: alt
      text: View on GitHub
      link: https://github.com/vuejs/vitepress
---
```
	
	Features的配置
	
```

---
layout: home

hero:
  name: VitePress        # `text` 上方的字符，带有品牌颜色
  text: Vite & Vue powered static site generator.   #hero 部分的主要文字，被定义为 `h1` 标签
  tagline: Lorem ipsum...    # `text` 下方的标语
  image:
    src: /logo.png           # text 和 tagline 区域旁的图片
    alt: VitePress           # 图片的alt文字
  actions:
    - theme: brand           # 按钮的颜色主题，默认为 `brand`
      text: Get Started      # 按钮的标签
      link: /guide/what-is-vitepress # 按钮的目标链接
      target: _blank        #可选的内容 链接的 target 属性，如果使用_blank,可以在浏览器重新打开一个标签
      rel:                  #可选的内容
    - theme: alt
      text: View on GitHub
      link: https://github.com/vuejs/vitepress
---
```
建议在这里使用表情符号（Emoji），它们是Unicode提供的一种跨平台、跨语言的字符编码方式，确保字符能在任何设备或操作系统上以相同的方式显示。表情符号是Unicode 6.0版本中引入的，从那时起，各种表情符号就开始在全球范围内流行起来。

40种常用的Emoji，直接复制就可以使用。

| 图标  | 描述      | 图标  | 描述      | 图标  | 描述      |
| --- | ------- | --- | ------- | --- | ------- |
| 📌  | 圆形图钉    | 📍  | 圆形目标    | 📎  | 回形针     |
| 🔗  | 链接符号    | 📏  | 直尺      | 📐  | 三角尺     |
| ✂️  | 剪刀      | 🖌️ | 画笔      | 🖋️ | 钢笔      |
| 🗂️ | 索引卡     | 🗒️ | 螺旋笔记本   | 📓  | 彩色笔记本   |
| 📔  | 封皮笔记本   | 📕  | 闭合的书籍   | 📖  | 打开的书    |
| 📚  | 书本堆     | 🔍  | 放大镜（放大） | 🔎  | 放大镜（缩小） |
| 💡  | 电灯泡     | 🔑  | 钥匙      | 🗝️ | 钥匙孔     |
| 🔐  | 锁       | 🌐  | 地球      | 🌙  | 新月      |
| 🌟  | 闪亮的星星   | 🌠  | 流星      | 🌈  | 彩虹      |
| 📱  | 手机      | 💻  | 笔记本电脑   | 🎓  | 学位帽     |
| 🎨  | 调色板     | 🎧  | 耳机      | 🎤  | 麦克风     |
| 🎼  | 音乐谱号    | 🎵  | 音乐笔记    | 🎶  | 多个音乐笔记  |
| 📞  | 电话      | 🚀  | 火箭      | 🛸  | 飞碟      |
| 💼  | 公文包     | 📦  | 箱子      | 📱  | 手机2     |
| 🔋  | 电池      | 🔌  | 电源插头    | 💻  | 笔记本电脑2  |
| 🖥️ | 桌面电脑    | 📱  | 手机3     | 📱  | 手机4     |
| 🔧  | 扳手      | 🛠️ | 锤子      | 🔩  | 螺母      |
| 🔨  | 锤子2     | 🪓  | 剪刀2     | 🧱  | 砖块      |
| 🏗️ | 建筑工地    | 🏢  | 办公大楼    | 🏗️ | 建筑工地2   |
| 🚪  | 门       | 🪟  | 窗户      | 🛏️ | 床       |
| 🛋️ | 沙发      | 🖇️ | 回形针2    | 🔑  | 钥匙2     |
| 🗅  | 圆环      | 🔗  | 链接符号2   | 📌  | 图钉2     |
| 📏  | 直尺2     | 📐  | 三角尺2    | 🖇️ | 回形针3    |
| 🎓  | 学位帽2    | 🎨  | 调色板2    | 🎧  | 耳机2     |
| 🎤  | 麦克风2    | 🎼  | 音乐谱号2   | 🎵  | 音乐笔记2   |
| 🎶  | 多个音乐笔记2 | 📞  | 电话2     | 🚀  | 火箭2     |
| 🛸  | 飞碟2     | 🏞  | 国家公园    | 🏝️ | 无人岛     |
| 🌄  | 日出      | 🌅  | 日落      | 🌃  | 夜晚的城市   |
| 🌆  | 城市天际线   | 🌇  | 夕阳下的天际线 | 🌉  | 夜晚的桥    |
| ⛰️  | 山       | 🏕️ | 露营      | 🏞️ | 国家公园2   |
| 🌈  | 彩虹2     | 🎡  | 摩天轮     | 🎢  | 过山车     |
| 🏗️ | 建筑起重机   | 🏰  | 城堡      | 💒  | 婚礼教堂    |
| 🗽  | 自由女神像   | 🗼  | 埃菲尔铁塔   | 🏰  | 城堡2     |

可以使用SVG甚至是PNG格式的图标，推荐使用阿里巴巴的矢量图标库网站。

> [https://www.iconfont.cn/](https://www.iconfont.cn/)


- markdown-examples.md  对文件进行扩展