# WOS论文爬取实现

基于西南科技大学图书馆实现，Library类根据[OAAuth](SWUSTOA.md)改编

## 核心代码

```Python
import base64
import os.path
import re

from ddddocr import DdddOcr
from husky_spider_utils import SeleniumSession
from loguru import logger


class Library:
    url = "https://cas.swust.edu.cn/authserver/login?service="

    def __init__(self, username, password,
                 service="https:%2F%2Flib.swust.edu.cn%2Fcas%2Fcas%2Flogin%3Fservice%3Dhttps%253A%252F%252Flib.swust.edu.cn%252Findex%26orgcode%3Dswust%26oauth_provider%3DthirdProvider"):
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


def clean_filename(filename):
    """
    清理文件名中的非法字符
    """
    # 定义非法字符的正则表达式
    illegal_chars = r'[<>:"/\\|?*\x00-\x1F]'
    # 替换非法字符为空格
    cleaned_filename = re.sub(illegal_chars, ' ', filename)
    return cleaned_filename


class Wos(Library):
    wos_url = "https://lib.swust.edu.cn/redirect?appcode=databaseguide&url=04fb30f31631a211b1d3add746494da7bf9ce281cd4a397b2f7e4f2b2933dd391c3c9ece2d0bb8ac31344b993c4292edec90ba5e7caabda5a4ea76c45ac00a72b78e59aefca03bd5c7cca1645d854a695baddb44a5546145bac5d881b35913a32e0bbf6463ff2426e8ef3176da43d3c7150efad5257720642a6798cc7aef168fb3fb0d&contentid=swustSCI-kexueyinwensuoyinother1722930399913&id=1820728178532462594&vp=1-d14c-181f--2987-ec4e"
    search_url = "https://webofscience-clarivate-cn-s-661.erlist.cn/wos/alldb/basic-search"

    def __init__(self, username, password, output_folder="./output"):
        super().__init__(username, password)
        self.output_folder = output_folder
        if not os.path.exists(output_folder):
            os.mkdir(output_folder)
        self.init_cookies()

    def init_cookies(self):
        self.session.selenium_get(self.wos_url)
        self.session.try_click(".btn2", timeout=5)
        self.session.try_click("#onetrust-accept-btn-handler")

    def search(self, value, search_type="Topic", start=1):
        if self.session.get_current_url() != self:
            self.session.selenium_get("https://webofscience-clarivate-cn-s-661.erlist.cn/wos/alldb/basic-search")
        self.session.try_click("[data-ta='search-field-dropdown'] .mat-tooltip-trigger.dropdown.ng-star-inserted",
                               timeout=5)
        self.session.try_click(
            f"[data-ta='search-field-dropdown'] .mat-tooltip-trigger.wrap-mode.ng-star-inserted[title='{search_type}']",
            timeout=5)
        self.session.clean_key("#search-option")
        self.session.send_key("#search-option", value)
        self.session.try_click("button[data-ta='run-search']", timeout=5)
        try:
            if self.session.get_current_url() != self.search_url:
                max_value = int(self.session.find_element(".tab-results-count").text.replace(",", ""))

                logger.info(f"查询到{max_value}条记录")
                self.session.selenium_cookies_to_session()
                end = start + 999
                if end > max_value:
                    end = max_value
                while end <= max_value:
                    self.export(start, end, value)
                    start += 1000
                    end = start + 999
            else:
                search_error = self.session.find_element("#snSearchType .search-error").text
                logger.warning(f"搜索{value}发生{search_error}")
        except Exception:
            logger.warning(f"搜索{value}发生异常，跳过搜索!")

    def clean_session(self):
        self.session.driver.delete_all_cookies()
        self.session.selenium_get(self.wos_url)

    def export(self, start: int, end: int, filename: str):
        qid = self.session.get_current_url().split("/")[6]
        sid = self.session.execute_script("return window.sessionData.BasicProperties.SID")
        logger.info(f"qid: {qid}, start: {start}, end: {end}, sid: {sid}")

        headers = {
            "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:137.0) Gecko/20100101 Firefox/137.0",
            "Accept": "application/json, text/plain, */*",
            "Accept-Language": "zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2",
            "Accept-Encoding": "gzip, deflate, br, zstd",
            "X-1P-WOS-SID": sid,
            "Content-Type": "application/json",
            "Origin": "https://webofscience-clarivate-cn-s-661.erlist.cn",
            "Connection": "keep-alive",
            "Referer": f"{self.session.get_current_url()}(overlay:export/exc)",
            "Sec-Fetch-Dest": "empty",
            "Sec-Fetch-Mode": "cors",
            "Sec-Fetch-Site": "same-origin",
            "Priority": "u=0"
        }
        url = "https://webofscience-clarivate-cn-s-661.erlist.cn/api/wosnx/indic/export/saveToFile"
        data = {
            "parentQid": qid,
            "sortBy": "relevance",
            "displayTimesCited": "true",
            "displayCitedRefs": "true",
            "product": "UA",
            "colName": "ALLDB",
            "displayUsageInfo": "true",
            "fileOpt": "xls",
            "action": "saveToExcel",
            "markFrom": str(start),
            "markTo": str(end),
            "view": "summary",
            "isRefQuery": "false",
            "locale": "en_US",
            "fieldList": [
                "AUTHORS",
                "TITLE",
                "SOURCE",
                "CITTIMES",
                "ACCESSION_NUM",
                "ABSTRACT",
                "AUTHORSIDENTIFIERS",
                "ISSN_ISBN",
                "PMID",
                "CONFERENCE_INFO_SPONSORS",
                "USAGEIND"
            ]
        }
        self.session.headers = headers
        res = self.session.post(url, json=data, is_to_driver=False, is_refresh=False)
        out_path = os.path.join(self.output_folder, f'{clean_filename(filename)}_start_{start}_end_{end}.xls')
        with open(out_path, "wb") as f:
            if res.status_code == 200:
                f.write(res.content)
                logger.success(f"保存文件到{os.path.abspath(out_path)}成功!")
            else:
                logger.error(res.status_code)
                logger.error(res.text)

```