---
title: HTML元素和常用属性
date: 2025-05-27 10:53:38
tags:
  - "html"
categories: "html"
---

# html 标签和元素

## 🧱 一、结构标签（布局相关）

| 标签            | 含义与作用                | 常用用法示例                                                          |
| --------------- | ------------------------- | --------------------------------------------------------------------- |
| &lt;html&gt;    | HTML 文档根元素           | &lt;html lang="zh-CN"&gt;...&lt;/html&gt;                             |
| &lt;head&gt;    | 页面头部信息（非可视）    | 包含 &lt;title&gt;, &lt;meta&gt;, &lt;link&gt; 等                     |
| &lt;body&gt;    | 页面主体内容（可视）      | 所有显示的内容都写在里面                                              |
| &lt;header&gt;  | 页眉，常用于 logo、导航等 | &lt;header&gt;&lt;h1&gt;我的网站&lt;/h1&gt;&lt;/header&gt;            |
| &lt;footer&gt;  | 页脚，版权等信息          | &lt;footer&gt;© 2025 我的站点&lt;/footer&gt;                          |
| &lt;main&gt;    | 页面主要内容区域          | 页面主体内容结构化的标记                                              |
| &lt;section&gt; | 区块，语义上分段          | &lt;section&gt;&lt;h2&gt;新闻&lt;/h2&gt;...&lt;/section&gt;           |
| &lt;article&gt; | 独立的文章内容            | &lt;article&gt;&lt;h2&gt;博客&lt;/h2&gt;内容...&lt;/article&gt;       |
| &lt;nav&gt;     | 导航栏                    | &lt;nav&gt;&lt;ul&gt;&lt;li&gt;首页&lt;/li&gt;&lt;/ul&gt;&lt;/nav&gt; |
| &lt;aside&gt;   | 边栏内容（如推荐、广告）  | &lt;aside&gt;相关文章&lt;/aside&gt;                                   |
| &lt;div&gt;     | 通用容器，无语义          | &lt;div class="box"&gt;...&lt;/div&gt;                                |

## 📄 二、文本内容标签

| 标签                  | 作用                     | 示例                                      |
| --------------------- | ------------------------ | ----------------------------------------- |
| &lt;h1&gt;~&lt;h6&gt; | 标题（h1 最大，h6 最小） | &lt;h1&gt;主标题&lt;/h1&gt;               |
| &lt;p&gt;             | 段落                     | &lt;p&gt;这是一个段落。&lt;/p&gt;         |
| &lt;br&gt;            | 换行（单标签）           | 第一行&lt;br&gt;第二行                    |
| &lt;hr&gt;            | 水平分割线               | &lt;hr&gt;                                |
| &lt;span&gt;          | 内联容器（无语义）       | &lt;span class="red"&gt;红字&lt;/span&gt; |
| &lt;strong&gt;        | 加粗（强调）             | &lt;strong&gt;重要&lt;/strong&gt;         |
| &lt;em&gt;            | 斜体（强调）             | &lt;em&gt;强调&lt;/em&gt;                 |
| &lt;b&gt;             | 加粗（不强调）           | &lt;b&gt;加粗&lt;/b&gt;                   |
| &lt;i&gt;             | 斜体（不强调）           | &lt;i&gt;斜体&lt;/i&gt;                   |
| &lt;mark&gt;          | 高亮                     | &lt;mark&gt;高亮文字&lt;/mark&gt;         |
| &lt;small&gt;         | 小号文字                 | &lt;small&gt;小字&lt;/small&gt;           |
| &lt;del&gt;           | 删除线                   | &lt;del&gt;删除内容&lt;/del&gt;           |
| &lt;ins&gt;           | 插入线（下划线）         | &lt;ins&gt;新增内容&lt;/ins&gt;           |
| &lt;pre&gt;           | 保留格式的文本           | &lt;pre&gt; 保留空格和换行&lt;/pre&gt;    |
| &lt;code&gt;          | 行内代码显示             | &lt;code&gt;console.log()&lt;/code&gt;    |

## 📎 三、链接与图片

| 标签        | 说明   | 示例                                                   |
| ----------- | ------ | ------------------------------------------------------ |
| &lt;a&gt;   | 超链接 | &lt;a href="https://example.com"&gt;点击跳转&lt;/a&gt; |
| &lt;img&gt; | 图片   | &lt;img src="logo.png" alt="Logo"&gt;                  |

## 📋 四、列表

| 标签       | 说明     | 示例                                                                        |
| ---------- | -------- | --------------------------------------------------------------------------- |
| &lt;ul&gt; | 无序列表 | &lt;ul&gt;&lt;li&gt;苹果&lt;/li&gt;&lt;li&gt;香蕉&lt;/li&gt;&lt;/ul&gt;     |
| &lt;ol&gt; | 有序列表 | &lt;ol&gt;&lt;li&gt;第一&lt;/li&gt;&lt;li&gt;第二&lt;/li&gt;&lt;/ol&gt;     |
| &lt;li&gt; | 列表项   | 同上                                                                        |
| &lt;dl&gt; | 定义列表 | &lt;dl&gt;&lt;dt&gt;术语&lt;/dt&gt;&lt;dd&gt;定义内容&lt;/dd&gt;&lt;/dl&gt; |

## 📦 五、表格

| 标签          | 说明           | 示例                                                                      |
| ------------- | -------------- | ------------------------------------------------------------------------- |
| &lt;table&gt; | 表格容器       | &lt;table&gt;&lt;tr&gt;&lt;td&gt;数据&lt;/td&gt;&lt;/tr&gt;&lt;/table&gt; |
| &lt;tr&gt;    | 表格行         | 表格的一行                                                                |
| &lt;td&gt;    | 表格数据单元格 | 表格的普通格子                                                            |
| &lt;th&gt;    | 表头单元格     | 加粗居中的格子                                                            |
| &lt;thead&gt; | 表头区域       |                                                                           |
| &lt;tbody&gt; | 表体区域       |                                                                           |
| &lt;tfoot&gt; | 表脚区域       |                                                                           |
| colspan       | 跨列           | &lt;td colspan="2"&gt;合并&lt;/td&gt;                                     |
| rowspan       | 跨行           | &lt;td rowspan="2"&gt;合并&lt;/td&gt;                                     |

## 🎛️ 六、表单标签

| 标签             | 用途         | 示例                                                           |
| ---------------- | ------------ | -------------------------------------------------------------- |
| &lt;form&gt;     | 表单容器     | &lt;form action="/submit"&gt;&lt;/form&gt;                     |
| &lt;input&gt;    | 输入框       | &lt;input type="text" name="user"&gt;                          |
| &lt;textarea&gt; | 多行文本框   | &lt;textarea name="msg"&gt;&lt;/textarea&gt;                   |
| &lt;button&gt;   | 按钮         | &lt;button type="submit"&gt;提交&lt;/button&gt;                |
| &lt;select&gt;   | 下拉菜单     | &lt;select&gt;&lt;option&gt;选项&lt;/option&gt;&lt;/select&gt; |
| &lt;option&gt;   | 下拉项       | 同上                                                           |
| &lt;label&gt;    | 输入说明文本 | &lt;label for="name"&gt;姓名&lt;/label&gt;                     |

## 🧪 七、媒体标签

| 标签           | 用途     | 示例                                                    |
| -------------- | -------- | ------------------------------------------------------- |
| &lt;audio&gt;  | 音频播放 | &lt;audio controls src="song.mp3"&gt;&lt;/audio&gt;     |
| &lt;video&gt;  | 视频播放 | &lt;video controls src="video.mp4"&gt;&lt;/video&gt;    |
| &lt;source&gt; | 媒体资源 | 用于 &lt;audio&gt; 或 &lt;video&gt; 中多种格式支持      |
| &lt;iframe&gt; | 嵌套网页 | &lt;iframe src="https://example.com"&gt;&lt;/iframe&gt; |

## 📜 八、元信息和脚本

| 标签           | 用途                   | 示例                                           |
| -------------- | ---------------------- | ---------------------------------------------- |
| &lt;title&gt;  | 页面标题（浏览器标签） | &lt;title&gt;我的网站&lt;/title&gt;            |
| &lt;meta&gt;   | 元信息（编码、描述等） | &lt;meta charset="UTF-8"&gt;                   |
| &lt;link&gt;   | 外部资源链接（如 CSS） | &lt;link rel="stylesheet" href="style.css"&gt; |
| &lt;style&gt;  | 页面内嵌 CSS 样式      | &lt;style&gt;p { color: red; }&lt;/style&gt;   |
| &lt;script&gt; | 引入或编写 JS 脚本     | &lt;script src="main.js"&gt;&lt;/script&gt;    |
