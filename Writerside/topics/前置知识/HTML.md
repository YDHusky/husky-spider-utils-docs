        
# HTML 与 CSS 选择器入门指南

## 🌟 什么是 HTML？ {id="html_1"}

HTML（超文本标记语言）是构建网页的基础语言，它通过一系列标签来定义网页的结构和内容。

### 📝 HTML 基本结构 {id="html_2"}

```html
<html>
  <head>
    <title>网页标题</title>
  </head>
  <body>
    <h1>这是一个标题</h1>
    <p>这是一个段落。</p>
  </body>
</html>
```

### 🧩 常见 HTML 元素

| 标签 | 描述 |
|------|------|
| `<div>` | 定义文档中的分区或节 |
| `<p>` | 定义段落 |
| `<a>` | 定义超链接 |
| `<img>` | 定义图像 |
| `<ul>`, `<ol>`, `<li>` | 定义列表 |
| `<table>`, `<tr>`, `<td>` | 定义表格 |
| `<form>`, `<input>` | 定义表单 |

## 🎯 CSS 选择器基础

CSS 选择器是 CSS 规则的一部分，它用于"选择"你想要添加样式的 HTML 元素。

### 🔍 基本选择器类型

#### 1️⃣ 元素选择器
直接选择 HTML 标签

```css
p {
  color: blue;  /* 所有段落文字变蓝 */
}
```

#### 2️⃣ ID 选择器
使用 `#` 选择特定 ID 的元素（ID 在页面中应唯一）

```css
#header {
  background-color: black;  /* ID 为 header 的元素背景变黑 */
}
```

HTML 示例：
```html
<div id="header">网站头部</div>
```

#### 3️⃣ 类选择器
使用 `.` 选择特定类的元素（类可以重复使用）

```css
.highlight {
  background-color: yellow;  /* 所有 class 为 highlight 的元素背景变黄 */
}
```

HTML 示例：
```html
<p class="highlight">这段文字会被高亮</p>
<span class="highlight">这段也会</span>
```

#### 4️⃣ 属性选择器
选择具有特定属性的元素

```css
input[type="text"] {
  border: 1px solid gray;  /* 所有 type 为 text 的 input 元素添加边框 */
}
```

### 🔄 组合选择器

#### 后代选择器（空格）
选择某元素内的所有指定元素

```css
div p {
  margin: 10px;  /* 选择 div 内的所有 p 元素 */
}
```

#### 子元素选择器（>）
选择某元素的直接子元素

```css
ul > li {
  list-style: square;  /* 选择 ul 的直接子元素 li */
}
```

#### 相邻兄弟选择器（+）
选择紧接在某元素后的元素

```css
h2 + p {
  font-weight: bold;  /* 选择紧跟在 h2 后面的第一个 p 元素 */
}
```

## 🎓 实用技巧

1. **使用浏览器开发者工具**：右键点击网页元素 → 检查，可以查看元素的 HTML 结构
2. **从具体到一般**：编写选择器时，先尝试最具体的选择方式，再逐步扩大范围
3. **测试你的选择器**：在浏览器控制台中使用 `document.querySelectorAll('你的选择器')` 测试



通过掌握 HTML 结构和 CSS 选择器，你将能够更有效地使用 Husky Spider Utils 等爬虫工具提取网页数据！
