


          
# RESTful

基于requests的session封装的基本方法，提供了与Web服务交互的简便方式。

## 🔍 Get

```python
from husky_spider_utils import SeleniumSession

session = SeleniumSession()

response = session.get("https://cn.bing.com")
```

### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| url | str | - | 网址、链接 |
| headers | dict | None | 附加headers |
| is_to_driver | bool | True | 是否将返回cookies载入selenium |
| is_refresh | bool | False | 是否刷新浏览器 |
| **kwargs | - | - | 其他requests.get()支持的原生参数 |

#### url

**类型:** str  
网址、链接，请求的目标地址

#### headers

**类型:** dict  
附加headers，通过update更新全局headers(不会影响全局链接)
> 💡 若希望更新全局headers可以通过`session.header.update({})`的方式

#### is_to_driver

**类型:** bool  
**默认值:** True  
是否将返回cookies载入selenium，启用后会自动同步cookies到浏览器

#### is_refresh

**类型:** bool    
**默认值:** False    
是否刷新浏览器，刷新浏览器才能在相关页面载入cookies

#### **kwargs

其他requests.get()支持的原生参数，如timeout、verify等

## 📤 POST

```python
from husky_spider_utils import SeleniumSession

session = SeleniumSession()

response = session.post("https://cn.bing.com", data={"key": "value"})
```

### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| url | str | - | 网址、链接 |
| data | dict/str | None | 表单数据 |
| json | dict | None | JSON数据 |
| headers | dict | None | 附加headers |
| is_to_driver | bool | True | 是否将返回cookies载入selenium |
| is_refresh | bool | True | 是否刷新浏览器 |
| **kwargs | - | - | 其他requests.post()支持的原生参数 |

#### data

**类型:** dict/str  
**默认值:** None  
POST请求的表单数据，用于application/x-www-form-urlencoded内容类型

#### json

**类型:** dict  
**默认值:** None  
POST请求的JSON数据，用于application/json内容类型

## 🔄 Request

通用请求方法，支持所有HTTP方法。

```python
from husky_spider_utils import SeleniumSession

session = SeleniumSession()

# 使用PUT方法
response = session.request("PUT", "https://api.example.com/resource", json={"status": "updated"})
```

### 参数

| 参数 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| url | str | - | 网址、链接 |
| method | str | - | HTTP方法 |
| headers | dict | None | 附加headers |
| is_to_driver | bool | True | 是否将返回cookies载入selenium |
| is_refresh | bool | True | 是否刷新浏览器 |
| **kwargs | - | - | 其他requests.request()支持的原生参数 |

#### method

**类型:** str  
HTTP请求方法，如GET、POST、PUT、DELETE、PATCH等


## 📋 示例：完整RESTful交互

```python
from husky_spider_utils import SeleniumSession

# 初始化会话
session = SeleniumSession("https://api.example.com")

# 登录获取token
login_data = {
    "username": "user123",
    "password": "pass456"
}
login_response = session.post("https://api.example.com/login", json=login_data)
token = login_response.json().get("token")

# 使用token获取资源
headers = {"Authorization": f"Bearer {token}"}
user_data = session.get("https://api.example.com/user/profile", headers=headers).json()

# 删除资源
delete_response = session.request(
    "DELETE",
    "https://api.example.com/user/posts/123",
    headers=headers
)

print(f"删除状态: {delete_response.status_code}")
```

---

通过使用husky-spider-utils的RESTful封装，你可以轻松地与各种Web API进行交互，同时保持selenium和requests之间的cookie同步，简化爬虫开发流程。
