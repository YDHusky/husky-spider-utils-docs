         
# HuskySpider (0.2.3)

HuskySpider 是一个为同操作多次任务提供结构框架的类，实现了中断处理、多线程或单线程处理作业、文件下载以及进度条显示等功能。

## 🚀 快速开始

```python
from husky_spider_utils import HuskySpider
from husky_spider_utils.model.spider_task import SpiderTask


# 创建自定义配置类（可选）
class MyConfig(SpiderConfig):
    str_api_key = "your_api_key"
    label_api_key = "API密钥"


# 创建自定义任务类
class MyTask(SpiderTask):
    def __init__(self, spider):
        super().__init__(spider)
    
    def execute(self, task):
        # 实现具体任务逻辑
        self.spider.session.selenium_get(f"https://example.com/{task}")
        
    def import_task(self):
        self.task_list = ["task1", "task2", "task3"]
        
    def on_success(self, task):
        print(f"任务 {task} 已完成")


# 初始化爬虫
spider = HuskySpider(
    config_loder=MyConfig,
    config_folder_path="./config",
    download_folder_path="./downloads"
)

# 创建并执行任务
task = MyTask(spider)
task.import_task()
spider.execute_task(task, is_multi_thread=True, max_thread=4, is_tqdm_bar=True)
```

## 📋 主要功能

### 🔧 初始化与配置

```python
def __init__(self, 
             is_need_config=True, 
             config_loder=SpiderConfig, 
             config_folder_path="./", 
             config_filename="config.yml", 
             temp_folder_path="./temp", 
             download_folder_path="./download", 
             is_selenium_session=True, 
             **selenium_session_kwargs)
```

- **配置管理**：自动生成、读取和更新配置文件
- **会话管理**：创建和管理 Selenium 会话
- **文件夹初始化**：自动创建临时文件夹和下载文件夹

### ⚙️ 任务执行

```python
def execute_task(self, task, is_multi_thread=False, max_thread=8, is_tqdm_bar=False)
```

- **多线程支持**：可选择单线程或多线程执行任务
- **进度显示**：支持 tqdm 进度条显示执行进度
- **断点续传**：自动记录已完成任务，支持中断后继续执行

### 📥 文件下载

```python
def download_from_res(self, res: Response, filename)
```

- **便捷下载**：从 Response 对象直接保存文件到下载目录

## 🔄 工作流程

1. **初始化爬虫**：创建 HuskySpider 实例，设置配置和文件夹
2. **创建任务**：继承 SpiderTask 类并实现必要方法
3. **导入任务列表**：通过 import_task 方法设置任务列表
4. **执行任务**：调用 execute_task 方法处理所有任务
5. **处理结果**：任务完成后通过 on_success 回调处理结果

## 📝 核心方法详解

### 🔍 init_config

```python
def init_config(self, is_need_config=True, config_loder=SpiderConfig,
                config_folder_path="./", config_filename="config.yml")
```

- **功能**：初始化配置文件，支持配置版本管理
- **版本控制**：当配置版本不一致时，会备份旧配置并创建新配置

### 📊 execute_record

```python
def execute_record(self, record, task: SpiderTask)
```

- **功能**：执行单个任务并记录完成状态
- **去重处理**：自动跳过已完成的任务

### 🧵 execute_task

```python
def execute_task(self, task: SpiderTask, is_multi_thread=False, max_thread=8, is_tqdm_bar=False)
```

- **功能**：批量执行任务，支持多线程和进度显示
- **线程池**：使用自定义线程池管理多线程执行

## 🛠️ 高级用法

### 自定义配置类

```python
class CustomConfig(SpiderConfig):
    str_api_url = "https://api.example.com"
    label_api_url = "API地址"
    int_retry_count = 3
    label_retry_count = "重试次数"
```

### 多线程执行与进度显示

```python
spider.execute_task(
    task,
    is_multi_thread=True,  # 启用多线程
    max_thread=8,          # 设置最大线程数
    is_tqdm_bar=True       # 显示进度条
)
```

## 🔄 版本信息

当前版本：0.2.3
