


          
# RESTful API 与 JSON 指南

## 🌐 什么是 RESTful API？ {id="restful-api_1"}

REST (Representational State Transfer) 是一种软件架构风格，用于设计网络应用程序。RESTful API 是遵循 REST 架构原则的应用程序接口。

### 📊 RESTful 设计原则

#### 1️⃣ 资源导向 (Resource-Oriented)
- API 围绕**资源**设计，资源是可命名的任何事物（如用户、文章、评论）
- 每个资源都有唯一的 URI（统一资源标识符）

#### 2️⃣ 使用 HTTP 方法表示操作
| HTTP 方法 | 操作 | 描述 |
|---------|------|------|
| GET | 读取 | 获取资源信息，不应修改数据 |
| POST | 创建 | 创建新资源 |
| PUT | 更新 | 完全替换现有资源 |
| PATCH | 部分更新 | 部分修改现有资源 |
| DELETE | 删除 | 删除资源 |

#### 3️⃣ 无状态通信
- 服务器不保存客户端状态
- 每个请求包含完整的信息
- 提高可扩展性和可靠性

#### 4️⃣ 统一接口
- 资源标识（URI）
- 通过表示操作资源
- 自描述消息
- 超媒体作为应用状态引擎（HATEOAS）

### 🔄 RESTful API 示例 {id="restful-api_2"}

**获取用户列表**:
```
GET /api/users
```

**获取特定用户**:
```
GET /api/users/123
```

**创建新用户**:
```
POST /api/users
Content-Type: application/json

{
  "name": "张三",
  "email": "zhangsan@example.com"
}
```

**更新用户信息**:
```
PUT /api/users/123
Content-Type: application/json

{
  "name": "张三",
  "email": "zhangsan_new@example.com"
}
```

**删除用户**:
```
DELETE /api/users/123
```

### 🔍 RESTful API 响应状态码 {id="restful-api_3"}

| 状态码 | 类别 | 描述 |
|-------|------|------|
| 2xx | 成功 | 请求成功处理 |
| 3xx | 重定向 | 需要进一步操作 |
| 4xx | 客户端错误 | 请求包含错误 |
| 5xx | 服务器错误 | 服务器处理请求出错 |

常见状态码：
- 200 OK：请求成功
- 201 Created：资源创建成功
- 400 Bad Request：请求格式错误
- 401 Unauthorized：未授权
- 404 Not Found：资源不存在
- 500 Internal Server Error：服务器内部错误

## 📝 什么是 JSON？ {id="json_1"}

JSON (JavaScript Object Notation) 是一种轻量级的数据交换格式，易于人阅读和编写，也易于机器解析和生成。

### 🧩 JSON 基本语法 {id="json_2"}

JSON 建立在两种结构上：
- **对象**：键值对的集合，由花括号包围
- **数组**：值的有序列表，由方括号包围

#### JSON 数据类型： {id="json_3"}
- 字符串：用双引号包围的文本
- 数字：整数或浮点数
- 布尔值：true 或 false
- null：表示空值
- 对象：键值对集合
- 数组：有序值列表

### 📋 JSON 示例 {id="json_4"}

**简单对象**:
```json
{
  "name": "张三",
  "age": 30,
  "isStudent": false,
  "email": "zhangsan@example.com"
}
```

**包含数组的对象**:
```json
{
  "name": "张三",
  "hobbies": ["阅读", "游泳", "编程"],
  "address": {
    "city": "北京",
    "zipcode": "100000"
  }
}
```

**对象数组**:
```json
[
  {
    "id": 1,
    "name": "张三",
    "role": "开发者"
  },
  {
    "id": 2,
    "name": "李四",
    "role": "设计师"
  }
]
```

### 🔄 JSON 与 RESTful API 的关系

JSON 是 RESTful API 中最常用的数据格式，用于：
- 请求体：客户端发送数据到服务器
- 响应体：服务器返回数据给客户端

**API 请求示例**:
```
POST /api/users
Content-Type: application/json

{
  "name": "张三",
  "email": "zhangsan@example.com",
  "role": "user"
}
```

**API 响应示例**:
```
HTTP/1.1 201 Created
Content-Type: application/json

{
  "id": 456,
  "name": "张三",
  "email": "zhangsan@example.com",
  "role": "user",
  "created_at": "2023-05-15T08:30:00Z"
}
```

## 🛠️ 在爬虫中的应用

在使用 Husky Spider Utils 等爬虫工具时，RESTful API 和 JSON 是获取数据的重要途径：

```python
import requests
import json

# 发送 GET 请求获取数据
response = requests.get('https://api.example.com/products')

# 解析 JSON 响应
products = response.json()

# 处理数据
for product in products:
    print(f"产品名称: {product['name']}, 价格: {product['price']}")

# 发送 POST 请求创建数据
new_product = {
    "name": "新产品",
    "price": 99.99,
    "description": "这是一个新产品"
}
response = requests.post(
    'https://api.example.com/products',
    json=new_product,
    headers={"Content-Type": "application/json"}
)
```

## 📚 最佳实践

### RESTful API 设计
- 使用名词而非动词表示资源
- 使用复数名词表示集合
- 使用嵌套资源表示关系
- 提供分页、排序和过滤功能
- 版本控制 API

### JSON 使用
- 保持结构一致性
- 使用有意义的键名
- 避免过深的嵌套
- 处理日期和时间格式
- 验证 JSON 格式

---

通过理解 RESTful API 和 JSON，你将能够更有效地使用 Husky Spider Utils 等工具与各种 Web 服务进行交互，获取和处理数据！
