# 更新日志

当前文档基于0.2.3编写

### 0.2.3
- spiderWindows中添加对page的变量调用
### 0.2.2

- 将windows由继承式改为变量式

### 0.2.1

- 修复huskyspider实例化时的driverType[从配置文件获取]

### 0.2.0

- 使用flet实现通过配置类自动生成图形化界面

`test.py`

```python
from husky_spider_utils.config.spider_config import SpiderConfig

from husky_spider_utils.windows.spider_windows import SpiderWindows


class TestConfig(SpiderConfig):
    str_configVersion = "1.1.1"
    str_account_username = ""
    str_account_password = ""


app = SpiderWindows(config_loder=TestConfig, is_selenium_session=False)
import flet as ft

ft.app(app.main_window)
```

打包windows

```bash
flet build windows --module-name test.py
```

更多查看flet

### 0.1.6

- 新增HuskySpider类
- 封装配置文件加载
- 多任务处理器(支持多线程或单线程处理或展示)