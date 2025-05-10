# SpiderConfig (0.2.3)

SpiderConfig 是一个爬虫配置类，用于自动管理（生成和读取）配置文件。该配置可以配合 HuskySpider 使用，也可以单独使用。

## 快速开始

创建一个类继承 SpiderConfig 类，添加成员属性即可。成员属性需遵循特定的命名规范，详见下文。

```Python
from husky_spider_utils.config.spider_config import SpiderConfig


class ITestConfig(SpiderConfig):
    str_ai_model = "deepseek-chat"
    label_ai_model = "模型名称"
    str_ai_baseUrl = "https://api.deepseek.com/v1"
    label_ai_baseUrl = "模型url"
    str_ai_apikey = "必填!!!"
    label_ai_apikey = "模型apiKey"
    str_account_username = ""
    label_account_username = "ITest账户"
    str_account_password = ""
    label_account_password = "ITest密码"
    int_sleepTime = 10
    label_sleepTime = "提交等待时间"
```

## 配置属性命名规范

### 支持的数据类型

| 数据类型 | 前缀标识   | 示例值     | 说明            |
|------|--------|---------|---------------|
| 文本   | str    | "1.0.0" | 字符串类型         |
| 整数   | int    | 1       | 整数类型          |
| 小数   | float  | 1.0     | 浮点数类型         |
| 选项   | select | "edge"  | 需要声明options列表 |
| 列表   | list   | -       | 暂未实现          |
| 布尔值  | bool   | True    | 真/假           |

### 属性命名格式

属性命名遵循以下格式：`类型前缀_父属性_属性名`

示例：

- `str_account_username` → 生成配置结构: `account.username`
- `select_driverType` → 生成配置结构: `driverType`

> **重要提示**：属性在定义时必须初始化变量值，否则不会生成到配置文件中

### 属性描述标签

> 以下标签主要用于 HuskySpiderUtils 的图形化界面生成 (SpiderWindows)

| 标签类型 | 前缀标识    | 适用范围            | 示例值                           | 说明           |
|------|---------|-----------------|-------------------------------|--------------|
| 显示标签 | label   | 所有类型            | "用户名"                         | 在界面上显示的名称    |
| 提示文本 | hint    | str, int, float | "请输入有效的用户名"                   | 输入框提示信息      |
| 选项列表 | options | select          | ["chrome", "edge", "firefox"] | 类型为 str_list |

标签属性的命名格式：`标签前缀_父属性_属性名`

示例：

- 属性 `str_account_username` 对应的标签为 `label_account_username`

## 默认配置项

系统预设了以下默认配置项：

| 配置项           | 默认值     | 数据类型   | 说明                          |
|---------------|---------|--------|-----------------------------|
| configVersion | "1.0.0" | str    | 用于区分配置文件版本，不同版本会保存新配置并迁移旧配置 |
| driverType    | "edge"  | select | 与 HuskySpider 配合使用时指定默认浏览器  |

## 核心方法

### fun_to_dict()

将配置对象转换为字典类型。

### fun_save_to_yml(folder, file_name)

将配置保存到 YAML 文件。

**参数**：

- `folder`：配置文件夹路径
- `file_name`：配置文件名

### fun_load_from_yml(folder, file_name)

从 YAML 文件读取配置。

**参数**：

- `folder`：配置文件夹路径
- `file_name`：配置文件名




