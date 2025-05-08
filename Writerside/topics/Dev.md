# 开发实用

记录一些开发实用手段的心得

## 📝 笔记

### 网站推荐:

[爬虫工具库](https://spidertools.cn/) - 包含curl(f12网络中右键复制curl(bash格式))转python requests, json一键格式化和对比等多个功能

## 📓 使用notebook配合husky-spider-utils

Jupyter Notebook是爬虫开发的绝佳工具，可以与husky-spider-utils无缝集成，提高开发效率。

### 安装与配置

```python
# 安装必要的包
!pip install jupyter notebook
!pip install husky-spider-utils
```

### 基本使用

```python
from husky_spider_utils import SeleniumSession
import pandas as pd
import matplotlib.pyplot as plt

# 初始化会话
session = SeleniumSession("https://example.com")

# 交互式开发，可以立即查看结果
response = session.get("https://example.com/data")
data = response.json()

# 数据分析与可视化
df = pd.DataFrame(data)
df.head()
```

### 优势

1. **即时反馈** - 每个代码块可以立即执行并查看结果
2. **状态保持** - 浏览器会话在整个notebook生命周期内保持
3. **可视化** - 直接在notebook中查看数据、图表和HTML内容
4. **文档化** - 代码、注释和结果一体化，便于分享和复用

### 实用技巧

```python
# 在notebook中显示网页截图
from IPython.display import Image, display
import base64

def show_screenshot(session):
    screenshot = session.driver.get_screenshot_as_base64()
    display(Image(data=base64.b64decode(screenshot)))

# 使用示例
session.get("https://example.com")
show_screenshot(session)
```

## 🛠️ 合理使用devtools(F12开发者工具)

浏览器开发者工具是爬虫开发的强大助手，掌握它可以大大提高效率。

### 元素选择器(Elements)

1. **快速定位元素**
   - 使用元素选择工具(🔍)点击页面元素
   - 右键元素 → 检查
   - 复制选择器: 右键HTML元素 → Copy → Copy selector

2. **验证CSS选择器**
   - 在Console面板中使用`document.querySelector()`测试选择器
   ```javascript
   document.querySelector('.my-class') // 测试单个元素
   document.querySelectorAll('.item') // 测试多个元素
   ```

### 网络监控(Network)

1. **分析请求**
   - 筛选XHR请求找到API端点
   - 查看请求头、参数和响应
   - 右键请求 → Copy → Copy as cURL，然后使用`curl_cffi`在Python中重现

2. **模拟请求**
   ```python
   # 从开发者工具复制的请求头
   headers = {
       'User-Agent': '...',
       'Referer': '...',
       'X-Requested-With': 'XMLHttpRequest'
   }
   
   # 在husky-spider-utils中使用
   session = SeleniumSession()
   response = session.get("https://api.example.com/data", headers=headers)
   ```

### JavaScript调试(Console)

1. **提取隐藏数据**
   - 许多网站在全局变量中存储数据
   ```javascript
   // 在Console中执行
   window.__INITIAL_DATA__ // 查找预加载的数据
   ```

   ```python
   # 在Python中提取
   data = session.execute_script("return window.__INITIAL_DATA__;")
   ```


## 📊 调试与分析技巧

### 记录关键信息

```python
from husky_spider_utils import SeleniumSession
from loguru import logger

# 创建自定义爬虫
class DebugSpider(SeleniumSession):
    def __init__(self):
        super().__init__("https://example.com")
        
    def parse_page(self, url):
        self.get(url)
        # 保存页面截图用于调试
        self.driver.save_screenshot(f"debug_{url.split('/')[-1]}.png")
        # 保存页面源码
        with open(f"debug_{url.split('/')[-1]}.html", "w", encoding="utf-8") as f:
            f.write(self.get_page_source())
```

### 性能优化

1. **使用无头模式提高速度**
   ```python
   from selenium.webdriver.firefox.options import Options

   options = Options()
   options.headless = True
   
   session = SeleniumSession(option=options)
   ```


## 🔄 持续集成与自动化

### 定时任务

```python
# 使用schedule库创建定时任务
import schedule
import time
from my_spider import DataSpider

def job():
    spider = DataSpider()
    try:
        spider.run()
    finally:
        spider.quit()

# 每天早上8点运行
schedule.every().day.at("08:00").do(job)

while True:
    schedule.run_pending()
    time.sleep(60)
```

### 数据持久化

```python
import pandas as pd
from husky_spider_utils import SeleniumSession

session = SeleniumSession()
results = []

# 爬取数据
urls = ["https://example.com/page1", "https://example.com/page2"]
for url in urls:
    session.get(url)
    selector = session.get_element_selector(".item")
    items = selector.css(".item-title::text").getall()
    prices = selector.css(".item-price::text").getall()
    
    for title, price in zip(items, prices):
        results.append({"title": title.strip(), "price": price.strip()})

# 保存为CSV
df = pd.DataFrame(results)
df.to_csv("results.csv", index=False, encoding="utf-8")

# 或保存为数据库
import sqlite3
conn = sqlite3.connect("results.db")
df.to_sql("items", conn, if_exists="append", index=False)
```

---

通过结合Jupyter Notebook、浏览器开发者工具和husky-spider-utils的强大功能，你可以构建更高效、更可靠的爬虫应用。持续学习和实践这些技巧，将大大提升你的爬虫开发效率。
