# SpiderWindows (0.2.3)

本类为基于SpiderConfig和HuskySpider的图形化配置

## 一键构建图形化

```Python
class TestWindows(SpiderWindows):
    spider:HuskySpider # 可以替换为继承自HuskySpider的实现，方便补全和检验
    def __init__(self, **kwargs):
        super().__init__(spider=HuskySpider(), **kwargs)


app = TestWindows()

import flet as ft
ft.run(app.main_window)
    
```

## 添加自定义页面

页面布局与实现请自行查看flet文档

```Python
# app.add_page(page, **kwargs)

self.add_page(ConfigPage(), 
icon=ft.Icons.SETTINGS_OUTLINED, 
selected_icon=ft.Icons.SETTINGS, 
label="配置")
```

## 启动按钮连接

由于爬虫实现逻辑各不相同，run方法并没有自动启动爬虫的功能，需要自行实现点击按钮后的实现，继承时重写run方法即可


