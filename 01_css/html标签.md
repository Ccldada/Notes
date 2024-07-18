# 文字标签

在body里

```xml
<h1><h6>标签 标题标签
<p></p> 段落标签
<em>页面展示区</em>强调 斜体
<i></i>斜体
<strong></strong>强调加强语气 加粗
  <b></b>加粗
<br>换行
字符实体  &asd；
空格   &nbsp；
&lt；<
&gt；>
<del></del>删除
```
# 图片标签

```xml
<img src="地址 相对路径 绝对路径" alt="图片显示不出时显示的文字" title="图片名字">
```
# 锚点标签

超链接

```xml
<a href="网址" target="打开方式 _blank新建空白页打开  _self当前页面打开">显示的文字</a>
<a href="tel:电话号码"></a>    打电话
<a href="mailto:邮箱地址"></a>    发邮件
<a href="文件地址"></a>    打开文件
<a href="#+id对应的"></a>    跳转
<a href="#"></a>    回到顶部
```
# 列表

无序列表

```xml
<ul type="disc圆点 circle圆圈  square方块">
<li></li>
</ul>
```
有序列表
```xml
<ol type="i I罗马数字" start="从某个数字开始" reversed="reversed倒序">
<li></li>
</ol>
```
自定义列表
><dl>
><dt></dt>
><dd></dd>前面有空格
></dl>
# 表格

```xml
<table border="外边框" cellspacing="单元格和单元格距离" cellpadding="内容和内边框距离">
<tbody>
<tr> <th>内容</th> </tr>表格头th 加粗效果
<tr> <td rowspan="">内容</td> </tr>其他用td
</tboby>
</table>
```
rowspan 上下合并
colspan 左右合并

# 表单

与服务器进行通讯的标签

```xml
<form action="提交的后台地址" method="请求方式  get  post" target="打开方式_blank  _self">
输入框  
<input type="text" name="usename">     name告诉后台这是什么
密码框
<input type="password" name="password" >
按钮
input
<input type="submit" value="显示的文字">  提交到action的地址里
<input type="reset">  重置
<input type="button">

button 
<botton type="button">普通按钮</botton>
<botton type="reset">重置</botton>
<botton type="submit">提交</botton>
</form>
```
单选
```xml
<input type="radio" name="保持一致 内容属性" value="内容的值">选择内容
<input type="radio" name="保持一致 内容属性" value="内容的值>选择内容
```
多选
```xml
<input type="checkbox" name="保持一致 内容属性" value="内容的值">选择内容
<input type="checkbox" name="保持一致 内容属性" value="内容的值">选择内容

<input type="radio" name="保持一致 内容属性" value="内容的值" id="****">
<label for="****id的值">选择内容</label>与input联系起来
```

下拉栏

```xml
<select name="" id="">
<option value="">选择内容</option>
<option value="">选择内容</option>
<option value="">选择内容</option>
</select>
```
表单 边框标题标签
```xml
<fieldset>
<legend>标题</legend>
内容    内容   内容
</fieldset>
```
评论框
```xml
<textarea name="" id="" cols="列" row="行"> </textarea>
```
内联框架
```xml
<iframe src="网址" frameborder="" >
```
# 容器标签

打包处理

```xml
<div></div>独占一行
<span></span>多个占一行
<nav></nav>独占一行
```
块元素：1.拥有完整盒模型 可以随意设置大小   2.独占一行
内联元素：1.没有完整盒模型  不能随意设置     2.多个独占一行

内联块元素：1.拥有完整盒模型  2.多个独占一行

属性设置 ：display="block块元素/inline内联元素/inline-block内联块元素"

