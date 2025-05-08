


          
# 其他方法

husky-spider-utils 提供了多种实用工具方法，帮助你更高效地进行爬虫开发。

## 📄 页面源码获取

```python
page_source = session.get_page_source()
```

获取当前页面的HTML源代码。

### 返回值
**类型:** str  
当前页面的完整HTML源代码

## 🔍 元素选择器

```python
selector = session.get_element_selector("#main-content", by=By.CSS_SELECTOR, timeout=60)
```

获取页面元素的Parsel选择器对象，可用于进一步解析。

### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| value | str | - | 元素选择器 |
| by | By | By.CSS_SELECTOR | 选择器类型 |
| timeout | int | 60 | 等待超时时间(秒) |

### 返回值
**类型:** Selector  
Parsel选择器对象，可以使用CSS或XPath进行进一步解析

## 🍪 Cookie管理

### 保存Cookies

```python
session.save_cookies("cookies.json")
```

将当前浏览器的cookies保存到文件。

#### 参数

| 参数 | 类型 | 说明 |
|------|------|------|
| save_path | str | 保存路径 |

### 加载Cookies

```python
session.load_cookies("cookies.json")
```

从文件加载cookies到浏览器和session。

#### 参数

| 参数 | 类型 | 说明 |
|------|------|------|
| load_path | str | 加载路径 |

## 🔗 URL操作

### 获取当前URL

```python
current_url = session.get_current_url()
```

获取浏览器当前页面的URL。

### 返回值
**类型:** str  
当前页面的URL

## 🔄 Cookies同步

### Selenium到Session

```python
session.selenium_cookies_to_session()
```

将Selenium浏览器的cookies同步到Requests的session。

### Session到Selenium

```python
session.cookies_to_driver(is_refresh=True)
```

将Requests的session的cookies同步到Selenium浏览器。

#### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| is_refresh | bool | True | 是否刷新浏览器 |

### 获取Session的Cookies

```python
cookies_dict = session.get_session_cookies_to_dict()
```

获取Requests的session的cookies，转换为Selenium可用的格式。

### 返回值
**类型:** list  
包含cookie字典的列表，每个字典包含name和value字段

## 🖱️ 高级交互

### 获取ActionChains对象

```python
actions = session.get_action_chains()
actions.move_to_element(element).click().perform()
```

获取Selenium的ActionChains对象，用于执行复杂的交互操作。

### 返回值
**类型:** ActionChains  
ActionChains对象

## 📜 JavaScript执行

```python
result = session.execute_script("return document.title;")
```

在当前页面执行JavaScript代码。

### 参数

| 参数 | 类型 | 说明 |
|------|------|------|
| script | str | JavaScript代码 |
| *args | any | 传递给JavaScript的参数 |

### 返回值
**类型:** any  
JavaScript代码的返回值

## 🔍 元素查找

```python
element = session.find_element("#login-button", by=By.CSS_SELECTOR, timeout=60)
```

查找页面元素，返回WebElement对象。

### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| value | str | - | 元素选择器 |
| by | By | By.CSS_SELECTOR | 选择器类型 |
| timeout | int | 60 | 等待超时时间(秒) |

### 返回值
**类型:** WebElement  
Selenium的WebElement对象，如果未找到则返回None

## 🖼️ 图像处理

### 获取图片元素的Base64编码

```python
img_base64 = session.get_img_base64_from_img_tag("img.captcha", by=By.CSS_SELECTOR, timeout=60)
```

将页面上的图片元素转换为Base64编码字符串，常用于验证码处理。

### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| value | str | - | 图片元素选择器 |
| by | By | By.CSS_SELECTOR | 选择器类型 |
| timeout | int | 60 | 等待超时时间(秒) |

### 返回值
**类型:** str  
图片的Base64编码字符串（不包含前缀）

## 📋 示例：综合使用

```python
from husky_spider_utils import SeleniumSession
from selenium.webdriver.common.by import By

# 初始化会话
session = SeleniumSession("https://example.com")

# 尝试加载已保存的cookies
try:
    session.load_cookies("example_cookies.json")
except:
    # 如果没有cookies，执行登录
    session.get("https://example.com/login")
    session.try_send_key("#username", "user123")
    session.try_send_key("#password", "pass456")
    session.try_click("#login-button")
    
    # 保存cookies以便下次使用
    session.save_cookies("example_cookies.json")

# 获取当前URL
current_url = session.get_current_url()
print(f"当前页面: {current_url}")

# 获取验证码图片的Base64编码
captcha_base64 = session.get_img_base64_from_img_tag("#captcha-img")
print(f"验证码Base64: {captcha_base64[:20]}...")

# 使用Parsel选择器解析页面内容
selector = session.get_element_selector(".content")
items = selector.css(".item::text").getall()
print(f"找到 {len(items)} 个项目")

# 执行JavaScript获取隐藏内容
hidden_data = session.execute_script("return window.__INITIAL_DATA__;")
print(f"获取到隐藏数据: {hidden_data}")
```

---

通过使用husky-spider-utils提供的这些实用方法，你可以更高效地处理各种爬虫场景，包括页面解析、Cookie管理、元素交互和图像处理等。
