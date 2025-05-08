# Parsel Selector

parsel由scrapy团队开发，是将scrapy中的parsel独立抽取出来的，可以轻松解析html，xml内容，获取需要的数据。

## 安装(husky-spider-utils已经捆绑) {id="husky-spider-utils_1"}

```bash
pip install parsel
```

## 基本使用

### 从字符串读取

```python
from parsel import Selector

html = """
<html>
    <body>
        <h1>Hello, Parsel!</h1>
        <ul>
            <li><a href="https://example.com/page1">第一页</a></li>
            <li><a href="https://example.com/page2">第二页</a></li>
            <li><a href="https://example.com/page3">第三页</a></li>
        </ul>
    </body>
</html>
"""

selector = Selector(text=html)
```

### 从文件读取

```python
with open('example.html', 'r', encoding='utf-8') as f:
    html = f.read()
    
selector = Selector(text=html)
```

## 选择器类型

Parsel支持三种类型的选择器：

### CSS选择器

CSS选择器是最常用的选择方式，语法简洁明了：

```python
# 选择所有链接
links = selector.css('a')

# 获取链接文本
link_texts = selector.css('a::text').getall()
# 结果: ['第一页', '第二页', '第三页']

# 获取链接地址
link_urls = selector.css('a::attr(href)').getall()
# 结果: ['https://example.com/page1', 'https://example.com/page2', 'https://example.com/page3']

# 选择第一个链接
first_link = selector.css('a').get()

# 组合选择器
items = selector.css('ul > li:nth-child(2) > a::text').get()
# 结果: '第二页'
```

### XPath选择器

XPath提供更强大的选择能力：

```python
# 选择所有链接
links = selector.xpath('//a')

# 获取链接文本
link_texts = selector.xpath('//a/text()').getall()

# 获取链接地址
link_urls = selector.xpath('//a/@href').getall()

# 使用条件
second_link = selector.xpath('//li[2]/a/text()').get()
# 结果: '第二页'

# 包含特定文本的元素
contains_page = selector.xpath('//a[contains(text(), "第二")]').get()
```

### 正则表达式

可以结合CSS或XPath与正则表达式：

```python
# 使用正则提取文本
regex_result = selector.css('a::text').re(r'第(.*)页')
# 结果: ['一', '二', '三']

# 只获取第一个匹配
first_match = selector.css('a::text').re_first(r'第(.*)页')
# 结果: '一'
```

## 链式调用

Parsel支持链式调用，可以逐步缩小选择范围：

```python
# 先选择ul，再选择其中的链接
links = selector.css('ul').css('a::attr(href)').getall()

# 先选择所有li，然后遍历处理
for li in selector.css('li'):
    link_text = li.css('a::text').get()
    link_url = li.css('a::attr(href)').get()
    print(f"{link_text}: {link_url}")
```

## 常用方法

### 获取数据

```python
# 获取第一个匹配结果（返回字符串）
first = selector.css('h1::text').get()

# 获取所有匹配结果（返回列表）
all_items = selector.css('li::text').getall()

# 设置默认值（当没有匹配时）
safe_item = selector.css('p::text').get(default='没有找到')
```

### 数据清理

```python
# 自动去除空白字符
clean_text = selector.css('h1::text').get().strip()

# 使用map清理列表数据
clean_items = list(map(str.strip, selector.css('li::text').getall()))
```

## 在 Husky Spider Utils 中的应用

Husky Spider Utils 已经集成了 Parsel，可以直接在爬虫代码中使用：

```python
from husky_spider_utils import SeleniumSession

spider = SeleniumSession("https://example.com")
# 使用response.selector直接解析
title = spider.get_element_selector('title::text').get()
links = spider.get_element_selector('a::attr(href)').getall()

# 处理数据
for link in links:
    print(f"发现链接: {link}")
    # 继续爬取
    spider.get(link)
```

## 高级技巧

### 处理相对路径

```python
base_url = "https://example.com"
relative_urls = selector.css('a::attr(href)').getall()

absolute_urls = []
for url in relative_urls:
    if url.startswith('/'):
        absolute_urls.append(base_url + url)
    else:
        absolute_urls.append(url)
```

### 提取表格数据

```python
# 提取表格数据到列表
table_rows = []
for row in selector.css('table tr'):
    cells = row.css('td::text').getall()
    if cells:  # 跳过表头
        table_rows.append(cells)
```

### 处理分页内容

```python
# 获取下一页链接
next_page = selector.css('a.next-page::attr(href)').get()
if next_page:
    # 继续爬取下一页
    print(f"下一页: {next_page}")
```

## 性能提示

- 优先使用CSS选择器（通常比XPath更快）
- 避免过于复杂的选择器
- 使用更具体的选择路径减少搜索范围
- 对于大文档，考虑分段处理

## 常见问题

### 无法获取JavaScript生成的内容

Parsel只能解析静态HTML，对于JavaScript动态生成的内容，需要结合Selenium使用：

```python
from husky_spider_utils import SeleniumSession

session = SeleniumSession("https://example.com")

selector = session.get_element_selector("div.dynamic-content")

# 现在可以解析动态生成的内容
dynamic_content = selector.css('div.dynamic-content::text').get()
```

---

通过掌握Parsel选择器，你可以更高效地从网页中提取所需数据，结合Husky Spider Utils的其他功能，构建强大的爬虫应用。