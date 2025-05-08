# 西南科技大学一站式登录实现

本项目基于含验证码的浏览器登录实现对husky-spider-utils部分方法进一步展示  
本代码仅供学习参考，请勿滥用

## 核心封装

```Python
import base64
import os.path

from ddddocr import DdddOcr
from husky_spider_utils import SeleniumSession
from loguru import logger

class OAAuth:
    url = "https://cas.swust.edu.cn/authserver/login?service="

    def __init__(self, username, password,
                 service=""):
        self.service = service
        self.session = SeleniumSession(selenium_init_url=self.url + service)
        self.ocr = DdddOcr(show_ad=False)
        self.username = username
        self.password = password
        self.login()

    def login(self):
        # self.session.load_cookies("./temp/cookies.txt")
        if not self.session.get_current_url().startswith("https://cas.swust.edu.cn"):
            return
        self.session.send_key("#username", self.username)
        self.session.send_key("#password", self.password)
        captcha = self.get_captcha("#imgCode")
        self.session.send_key("input[name='captcha']", captcha)
        self.session.click("button[name='submit']")
        if self.session.get_current_url().startswith("https://cas.swust.edu.cn"):
            msg = "".join(self.session.get_element_selector(".textError").css(" ::text").getall()).strip()
            logger.error(msg)
            if msg == "验证码无效":
                self.login()
        else:
            logger.success("登录成功!")
            if not os.path.exists("./temp"):
                os.mkdir("./temp")
            # self.session.save_cookies("./temp/cookies.txt")

    def get_captcha(self, img_element):
        base64_image = self.session.get_img_base64_from_img_tag(img_element)
        captcha = self.ocr.classification(base64.b64decode(base64_image))
        logger.info(captcha)
        return captcha
```