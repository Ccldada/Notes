#  css
## 1. 覆盖
```css
/* 1.没有访问过 */
.aa:link{}
/* 2.访问过 */
.aa:visited{}
/* visited接受的样式有限 */
/* 3.鼠标覆盖在上面 */
.aa:hover{}
/* 4.鼠标按下不放开 */
.aa:active{}
```
## 2. position    定位
static  默认值，没有定位，正常的文档流中，会忽略 top, bottom, left, right 或者 z-index 声明，块级元素从上往下纵向排布，⾏级元素从左向右排列

absolute	相对于static定位以外的一个父元素进行定位。元素的位置通过left、top、right、bottom属性进行规定

relative  相对于其原来的位置进行定位。元素的位置通过left、top、right、bottom属性进行规定

fixed  指定元素相对于屏幕视⼝（viewport）的位置来指定元素位置。元素的位置在屏幕滚动时不会改变，⽐如回到顶部的按钮⼀般都是⽤此定位⽅式

## 3. transition    过渡
元素从这个属性(color)的某个值(red)过渡到这个属性(color)的另外一个值(green)，

这是一个状态的转变，需要一种条件来触发这种转变，比如我们平时用到的:hoever、:focus、:checked、媒体查询或者JavaScript。

```javascript
transition: property duration timing-function delay;
```
property 规定设置过渡效果的 CSS 属性的名称
duration 规定完成过渡效果需要多少秒或毫秒

timing-function 规定速度效果的速度曲线

delay  定义过渡效果何时开始

                     持续时间  匀速   延迟

简写 transition：all 1s linear 2s

## 4. transform 旋转
旋转、缩放、移动或者倾斜 应用2D，3D转换，

translate位移  translate（x，y），translateX（），translateY（），translate3D（x,y,z）

rotate 旋转  rotate3D（x,y,z,角度）deg旋转角度单位，负数逆时针

scale 缩放 缩放基数为1， 大于1放大， 小于1缩小

skew [skjuː]倾斜 skey（x,y) skeyX() skeyY() 第一个水平扭转方向，第二个垂直扭转方向

## 5. animation：动画
使用@keyframes，来定义关键帧，使用百分比来划分动画持续的时长，

简写 animation: name duration timing-function delay iteration-count direction play-state fill-mode;

name ： 用来调用@keyframes定义好的动画，与@keyframes定义的动画名称一致

duration ： 指定元素播放动画所持续的时间

timing-function ： 规定速度效果的速度曲线

delay ： 整个animation执行之前等待的时间

iteration-count ： 定义动画的播放次数，可选具体次数或者无限(infinite)

direction fill-mode ：设置动画播放方向：normal(按时间轴顺序),reverse(时间轴反方向运行),alternate(轮流，即来回往复进行),alternate-reverse(动画先反运行再正方向运行，并持续交替运行)

play-state ：控制元素动画的播放状态，通过此来控制动画的暂停和继续，两个值：running(继续)，paused(暂停)

fill-mode ： 控制动画结束后，元素的样式，有四个值：none(回到动画没开始时的状态)，forwards(动画结束后动画停留在结束状态)，backwords(动画回到第一帧的状态)，both(根据animation-direction轮流应用forwards和backwards规则)，注意与iteration-count不要冲突(动画执行无限次)

## 6. 漂浮
float：left；

清除漂浮：clear：left

## 7. 伪元素
一种在页面中可以看到 但是不在html文档内的内容

```javascript
p::before{
    content: "123";
    display: block;
    /* 可见性 隐藏 */
    visibility: hidden;
}
p::after{
    content: "456";
}   
```
## 8. 伪类选择器
```javascript
.item:first-child{}
.list .item:nth-child(3){color: red; }
.list .item:last-child{}
```
## 9. 3d
```javascript
.wrap{
       /* 视距 */
       perspective: 400px;
       /* 视点 */
       perspective-origin: 50% 50%;
}
.stage{
       /* 保留子元素的3d效果 */
       transform-style: preserve-3d;
}
```






















.

