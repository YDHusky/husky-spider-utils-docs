


          

# 自动化操作

浏览器的一些自动化操作封装，提供了丰富的交互方法，让爬虫开发更加便捷。

## 🌐 浏览器请求

```python
from husky_spider_utils import SeleniumSession

session = SeleniumSession()
session.selenium_get("https://cn.bing.com")
```

### 参数

| 参数 | 类型 | 说明 |
|------|------|------|
| url | str | 网址、链接 |

## 🔤 输入操作

### ⌨️ 发送文本

```python
session.send_key("#search_box", "husky spider utils", by=By.CSS_SELECTOR, timeout=60)
```

#### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| value | str | - | 元素选择器 |
| send_value | str | - | 要发送的文本 |
| by | By | By.CSS_SELECTOR | 选择器类型 |
| timeout | int | 60 | 等待超时时间(秒) |

### 🧹 清空输入框

```python
session.clean_key("#search_box", by=By.CSS_SELECTOR, timeout=60)
```

#### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| value | str | - | 元素选择器 |
| by | By | By.CSS_SELECTOR | 选择器类型 |
| timeout | int | 60 | 等待超时时间(秒) |

## 🖱️ 点击操作

### 点击元素

```python
session.click("#submit_button", by=By.CSS_SELECTOR, timeout=60)
```

#### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| value | str | - | 元素选择器 |
| by | By | By.CSS_SELECTOR | 选择器类型 |
| timeout | int | 60 | 等待超时时间(秒) |

### 🔄 尝试点击（自动重试）

```python
session.try_click("#submit_button", by=By.CSS_SELECTOR, timeout=60, max_attempt=5, sleep_time=0.5)
```

#### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| value | str | - | 元素选择器 |
| by | By | By.CSS_SELECTOR | 选择器类型 |
| timeout | int | 60 | 等待超时时间(秒) |
| max_attempt | int | 0 | 最大尝试次数，0表示无限尝试 |
| sleep_time | float | 0.5 | 尝试间隔时间(秒) |

## 🔄 增强输入操作

### 尝试发送文本（自动重试）

```python
session.try_send_key("#search_box", "husky spider utils", by=By.CSS_SELECTOR, timeout=60, max_attempt=5, sleep_time=0.5)
```

#### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| value | str | - | 元素选择器 |
| send_value | str | - | 要发送的文本 |
| by | By | By.CSS_SELECTOR | 选择器类型 |
| timeout | int | 60 | 等待超时时间(秒) |
| max_attempt | int | 0 | 最大尝试次数，0表示无限尝试 |
| sleep_time | float | 0.5 | 尝试间隔时间(秒) |

### 尝试清空输入框（自动重试）

```python
session.try_clean_key("#search_box", by=By.CSS_SELECTOR, timeout=60, max_attempt=5, sleep_time=0.5)
```

#### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| value | str | - | 元素选择器 |
| by | By | By.CSS_SELECTOR | 选择器类型 |
| timeout | int | 60 | 等待超时时间(秒) |
| max_attempt | int | 0 | 最大尝试次数，0表示无限尝试 |
| sleep_time | float | 0.5 | 尝试间隔时间(秒) |

## ⏱️ 等待操作

### 随机等待

```python
session.sleep_random_time(min_time=5, max_time=10)
```

#### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| min_time | int | 5 | 最小等待时间(秒) |
| max_time | int | 10 | 最大等待时间(秒) |

## 🖱️ 鼠标操作

### 悬停操作

```python
session.hover("#menu_item", by=By.CSS_SELECTOR, timeout=60)
```

#### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| value | str | - | 元素选择器 |
| by | By | By.CSS_SELECTOR | 选择器类型 |
| timeout | int | 60 | 等待超时时间(秒) |

## 📝 用户交互

### 获取用户输入

```python
user_input = session.on_input("请输入验证码：")
```

#### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| des | str | "" | 提示文本 |

## 📜 滚动操作

### 滚动到指定高度

```python
session.scroll(height=500)
```

#### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| height | int | 200 | 滚动高度(像素) |

### 滚动到指定元素

```python
element = session.driver.find_element(By.ID, "target")
session.scroll_to_el(element)
```

#### 参数

| 参数 | 类型 | 说明 |
|------|------|------|
| element | WebElement | 目标元素 |

### 通过选择器滚动到元素

```python
session.scroll_to_el_by_value("#target", by=By.CSS_SELECTOR)
```

#### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| value | str | - | 元素选择器 |
| by | By | By.CSS_SELECTOR | 选择器类型 |

### 滚动到页面顶部

```python
session.scroll_to_top()
```

### 平滑滚动到底部

```python
session.scroll_to_bottom_fade(step=100, max_height=None)
```

#### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| step | int | 100 | 每次滚动的距离(像素) |
| max_height | int | None | 最大滚动高度，None表示滚动到页面底部 |

### 直接滚动到底部

```python
session.scroll_to_bottom()
```

## 🚪 退出浏览器

```python
session.quit()
```

## 📋 示例：完整自动化操作

```python
from husky_spider_utils import SeleniumSession
from selenium.webdriver.common.by import By

# 初始化会话
session = SeleniumSession("https://example.com")

# 导航到登录页
session.selenium_get("https://example.com/login")

# 输入用户名和密码
session.try_send_key("#username", "user123", max_attempt=3)
session.try_send_key("#password", "pass456", max_attempt=3)

# 点击登录按钮
session.try_click("#login-button", max_attempt=3)

# 等待登录完成
session.sleep_random_time()

# 滚动到特定元素
session.scroll_to_el_by_value(".content-section")

# 悬停在菜单上
session.hover("#menu")

# 点击子菜单项
session.try_click("#submenu-item", max_attempt=3)

# 平滑滚动浏览页面
session.scroll_to_bottom_fade(step=50)

# 需要用户输入验证码
captcha = session.on_input("请输入图片验证码：")
session.try_send_key("#captcha-input", captcha)

# 提交表单
session.try_click("#submit-button")

# 完成后退出
session.quit()
```

---

通过使用husky-spider-utils的自动化操作封装，你可以轻松实现复杂的浏览器交互，大大简化爬虫开发流程。
