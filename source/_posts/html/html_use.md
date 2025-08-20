---
title: HTML基本用法
date: 2025-05-27 10:26:50
categories: "html"
tags:
  - "html"
---

# HTML 元素和常用属性

```html
<!-- 标题元素：h1 ~ h6，数字越小标题越大 -->
<h1>一级标题</h1>
<h2>二级标题</h2>

<!-- 段落元素：p -->
<p>这是一个段落。</p>

<!-- 链接元素：a，href为目标地址 -->
<a href="https://example.com" target="_blank">访问示例网站</a>

<!-- 图片元素：img，src为图片路径，alt为替代文字 -->
<img src="example.jpg" alt="示例图片" width="200" />

<!-- 无序列表：ul 和 li -->
<ul>
  <li>苹果</li>
  <li>香蕉</li>
</ul>

<!-- 有序列表：ol 和 li -->
<ol>
  <li>第一步</li>
  <li>第二步</li>
</ol>

<!-- 换行：br -->
<p>第一行<br />第二行</p>

<!-- 强调文本：strong 为加粗，em 为斜体 -->
<p><strong>加粗文本</strong> 和 <em>斜体文本</em></p>

<!-- 区块容器：div（块级） 和 span（行内） -->
<div style="background: #eee; padding: 10px;">这是一个区块容器</div>
<p>这是一个 <span style="color: red;">红色词语</span> 的例子。</p>

<!-- 输入框：input，type 可设为 text, password, checkbox 等 -->
<input type="text" placeholder="请输入用户名" />

<!-- 按钮：button -->
<button onclick="alert('你点击了按钮')">点我</button>

<!-- 表单：form -->
<form action="/submit" method="post">
  <label>用户名：<input type="text" name="username" /></label><br />
  <label>密码：<input type="password" name="password" /></label><br />
  <input type="submit" value="提交" />
</form>

<!-- 表格：table、thead、tbody、tr、th、td -->
<table border="1">
  <thead>
    <tr>
      <th>姓名</th>
      <th>年龄</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>小明</td>
      <td>18</td>
    </tr>
  </tbody>
</table>

<!-- 使用常见属性：id, class, style, title, disabled, checked -->
<div id="main" class="container" title="鼠标悬停提示" style="color: blue;">
  使用了一些常用属性的div
</div>

<input type="checkbox" checked /> 我已阅读协议<br />
<button disabled>禁用按钮</button>

<!-- 注释写法 -->
<!-- 这是一个HTML注释，网页中不会显示 -->
```

## 表格的基本用法和修饰属性

```html
<!-- 表格基础结构和修饰属性示例 -->
<table border="1" width="500" cellpadding="10" cellspacing="0">
  <!-- border：边框宽度；width：表格总宽度 -->
  <!-- cellpadding：单元格内边距；cellspacing：单元格之间的间距 -->

  <!-- 表头 -->
  <thead>
    <tr>
      <th>序号</th>
      <th>姓名</th>
      <th>年龄</th>
    </tr>
  </thead>

  <!-- 表体 -->
  <tbody>
    <tr>
      <td>1</td>
      <td>张三</td>
      <td>20</td>
    </tr>
    <tr>
      <td>2</td>
      <td>李四</td>
      <td>22</td>
    </tr>
  </tbody>

  <!-- 表尾 -->
  <tfoot>
    <tr>
      <td colspan="3">共 2 人</td>
    </tr>
  </tfoot>
</table>
```

## 表单 form 的各种用法及 input 中 type 的不同功能

```html
<!-- 表单常用结构及 input 类型举例 -->
<form action="/submit" method="post">
  <!-- action：提交地址；method：提交方式（get/post） -->

  <!-- 文本输入框 -->
  用户名：<input type="text" name="username" placeholder="请输入用户名" /><br />

  <!-- 密码输入框 -->
  密码：<input type="password" name="password" placeholder="请输入密码" /><br />

  <!-- 单选框（同组 name 只可选一项） -->
  性别：
  <input type="radio" name="gender" value="male" checked /> 男
  <input type="radio" name="gender" value="female" /> 女<br />

  <!-- 多选框（同组 name 可选多项） -->
  爱好：
  <input type="checkbox" name="hobby" value="reading" /> 阅读
  <input type="checkbox" name="hobby" value="sports" /> 运动
  <input type="checkbox" name="hobby" value="music" checked /> 音乐<br />

  <!-- 邮箱输入框 -->
  邮箱：<input type="email" name="email" /><br />

  <!-- 数字输入框 -->
  年龄：<input type="number" name="age" min="0" max="100" /><br />

  <!-- 日期选择 -->
  生日：<input type="date" name="birthdate" /><br />

  <!-- 文件上传 -->
  上传头像：<input type="file" name="avatar" /><br />

  <!-- 隐藏字段（用户不可见） -->
  <input type="hidden" name="token" value="abc123" /><br />

  <!-- 下拉框 -->
  城市：
  <select name="city">
    <option value="bj">北京</option>
    <option value="sh">上海</option>
    <option value="gz">广州</option></select
  ><br />

  <!-- 多行文本框 -->
  备注：<br />
  <textarea name="comments" rows="4" cols="40">请输入备注内容</textarea><br />

  <!-- 按钮 -->
  <input type="submit" value="提交" />
  <input type="reset" value="重置" />
  <button type="button" onclick="alert('自定义按钮')">点击我</button>
</form>
```
