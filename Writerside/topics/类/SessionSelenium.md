


          
# SessionSelenium

SessionSelenium是husky-spider-utils的核心组件，它整合了Selenium和Requests的功能，让爬虫开发更加便捷高效。

## 🚀 快速开始

```python
from husky_spider_utils import SeleniumSession

session = SeleniumSession("https://cn.bing.com")
```

## ⚙️ 初始化参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| selenium_init_url | str | https://cn.bing.com | Selenium初始化链接 |
| driver_type | str | firefox | 浏览器类型 |
| headers | dict | *见下文* | Requests的headers |
| download_path | str | "" | 浏览器下载路径 |
| driver_path | str | "./" | 驱动程序路径 |
| os_type | str | "windows" | 操作系统类型 |
| option | object | None | Selenium选项 |

### 🌐 selenium_init_url

**类型:** str  
**默认值:** https://cn.bing.com (提一嘴，建议大家可以使用bing作为常用搜索引擎)  
selenium的初始化链接，用于启动浏览器时访问的第一个页面。

### 🔍 driver_type

**类型:** str  
**默认值:** firefox  
浏览器类型，支持以下三种类型：
- 🦊 `firefox`
- 🌐 `edge`
- 🌀 `chrome`

> 💡 **提示**：只需确保电脑上安装了相应浏览器，无需手动下载驱动程序，husky-spider-utils会自动下载并配置驱动。

### 📋 headers

**类型:** dict  
**默认值:** None  
这里是requests的session的默认headers，虽然传值为None，但实际默认值如下，传入的值以**update**的方式修改默认值：

```python
headers = {
    "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/131.0.0.0 Safari/537.36",
    "Accept": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7",
    "Accept-Language": "zh-CN,zh;q=0.9",
}
```

### 📂 download_path

**类型:** str  
**默认值:** ""  
浏览器下载文件路径设置，为空则使用浏览器默认下载路径。

### 🔧 driver_path

**类型:** str  
**默认值:** "./"  
driver路径，默认为工作目录根目录下，若未检测到会自动下载。

### 💻 os_type

**类型:** str  
**默认值:** "windows"  
这里的作用主要为driver的下载，支持填写：
- `windows`
- `linux`

### ⚙️ option

selenium options对象，用于自定义浏览器行为。注意：`download_path`参数的优先级大于传入的option中的下载路径设置。

## 📝 建议开发方式

husky-spider-utils提供了灵活的开发方式，以下是两种推荐的使用模式：

### 🧬 继承式

通过继承`SeleniumSession`类，可以方便地扩展功能：

```python
from husky_spider_utils import SeleniumSession

class Spider(SeleniumSession):
    def __init__(self, **kwargs):
        super(Spider, self).__init__(selenium_init_url="https://cn.bing.com", **kwargs)
        print(self.get("cn.bing.com"))
        
    def custom_method(self):
        # 添加自定义方法
        pass
```

### 🔄 成员变量式

将`SeleniumSession`作为类的成员变量使用：

```python
from husky_spider_utils import SeleniumSession

class Spider:
    base_url = "https://cn.bing.com"

    def __init__(self, **kwargs):
        self.session = SeleniumSession(self.base_url)
        print(self.session.get(self.base_url))
        
    def custom_method(self):
        # 使用self.session调用SeleniumSession的方法
        pass
```
---
通过使用SessionSelenium，你可以轻松结合Selenium的浏览器自动化能力和Requests的高效网络请求功能，大大简化爬虫开发流程。

