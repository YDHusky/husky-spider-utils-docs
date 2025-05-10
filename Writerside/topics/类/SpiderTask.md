         
# SpiderTask (0.2.3)

SpiderTask 是一个用于声明任务及其运行方式的类，可与 HuskySpider 结合进行多线程子任务执行。

## 🚀 快速开始

```Python
from husky_spider_utils.model.spider_task import SpiderTask


class TestTask(SpiderTask):
    spider: HuskySpider

    def __init__(self, spider):
        super().__init__(spider)

    def execute(self, task):
        self.spider.session.selenium_get(f"https://cn.bing.com/?q={task}")
        self.spider.session.sleep_random_time()

    def import_task(self):
        self.task_list = [
            "1", "2", "3"
        ]
        
    def on_success(self, task):
        print(f"{task}执行完成")
```

## 📋 核心方法

### 🔧 初始化

```python
def __init__(self, spider):
    super().__init__(spider)
```

- **参数**：`spider` - HuskySpider 的实例化对象
- **说明**：创建 SpiderTask 实例时，需要传入一个 HuskySpider 实例

### ⚙️ execute

```python
def execute(self, task):
    # 实现具体任务逻辑
```

- **参数**：`task` - 单个任务对象
- **说明**：定义每个任务的具体执行逻辑，这是必须重写的核心方法

### 📥 import_task

```python
def import_task(self):
    self.task_list = ["任务1", "任务2", "任务3"]
```

- **说明**：用于导入或定义任务列表，将任务添加到 `self.task_list` 中
- **提示**：可以从文件、数据库或其他来源导入任务

### ✅ on_success

```python
def on_success(self, task):
    print(f"{task}执行完成")
```

- **参数**：`task` - 已完成的任务对象
- **说明**：当单个任务成功完成后执行的回调方法
- **用途**：可用于记录日志、保存结果或触发后续操作

## 🔄 工作流程

1. **创建任务类**：继承 SpiderTask 并实现必要方法
2. **初始化**：传入 HuskySpider 实例
3. **导入任务**：通过 import_task 方法设置任务列表
4. **执行**：系统会自动调用 execute 方法处理每个任务
5. **回调**：任务完成后触发 on_success 方法

## 📝 最佳实践

- 在 execute 方法中处理单个任务的所有逻辑
- 使用 on_success 方法保存结果或更新状态
- 合理设计任务粒度，避免单个任务执行时间过长
- 利用 HuskySpider 提供的会话管理和异常处理功能

## 🔄 版本信息

当前版本：0.2.3
