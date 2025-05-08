# 以观书法

以观书法的汉字图片爬取

## 核心代码

```Python
import os
import time

from husky_spider_utils import SeleniumSession
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By
from selenium.webdriver.common.actions.wheel_input import ScrollOrigin
from parsel import Selector


class YGSF:
    url = "https://web.ygsf.com/"
    max_image = 360
    min_sleep_time = 2
    max_sleep_time = 3

    def __init__(self, max_attempts=20, output_folder="./output", browser="edge"):
        self.output_folder = output_folder
        self.session = SeleniumSession(
            selenium_init_url=self.url, driver_type=browser)
        self.max_attempts = max_attempts
        self.init()

    def init(self):
        if not os.path.exists(self.output_folder):
            os.makedirs(self.output_folder)

    def search(self, text):
        self.session.selenium_get(self.url)
        self.session.try_click(".u-search")
        time.sleep(1)
        self.session.try_send_key("input[type='search']", text)
        time.sleep(1)
        self.session.try_click(".van-search__action .u-nav-icon")
        ui_type = self.ui_type()
        for index in range(len(ui_type)):
            if index != 0:
                self.session.try_click(f'//*[@id="app"]/div/div[1]/div/div[1]/ul/li[{index + 1}]', by=By.XPATH)
            ui = ui_type[index]
            # self.session.sleep_random_time()
            time.sleep(3)
            self.get_images_urls(ui_type=ui, text=text)

    def get_images_urls(self, ui_type="", text=""):
        images = []
        cnt = 0
        current_attempt = 0
        while True:
            drag = self.session.driver.find_element(by=By.CSS_SELECTOR, value=".m-grid__box")
            ActionChains(self.session.driver).scroll_from_origin(ScrollOrigin(drag, 200, 300), 0, 1000).perform()
            document = Selector(text=self.session.driver.page_source)
            images.extend(document.css(".m-image::attr(data-src)").getall())
            if cnt == len(set(images)):
                current_attempt += 1
                if current_attempt >= self.max_attempts:
                    break
            cnt = len(set(images))
            if cnt >= self.max_image:
                break
                # login_tip = self.session.driver.find_element(by=By.CSS_SELECTOR, value=".van-dialog__header")
                #
                # if login_tip.is_displayed():
                #     self.session.try_click(".van-dialog__cancel")
                #     break
            self.session.sleep_random_time(self.min_sleep_time, self.max_sleep_time)
        images = set(images)
        self.save(images, f"{text}-{ui_type}")

    def ui_type(self):
        document = Selector(text=self.session.driver.page_source)
        u_types = document.css(".m-category .u-type::text").getall()
        return u_types

    def save(self, images, name):
        path = os.path.join(self.output_folder, name + ".txt")
        if not os.path.exists(path):
            with open(path, "w", encoding="utf-8") as f:
                f.write("")
        for image in images:
            with open(path, "a") as f:
                f.write(image + "\n")


def get_input(input_path):
    with open(input_path, "r", encoding="utf-8") as f:
        return f.read().split("\n")


def main():
    input_path = "input.txt"
    ygsf = YGSF(browser="edge")  # edge firefox chrome
    input_data = get_input(input_path)
    for text in input_data:
        ygsf.search(text)


if __name__ == '__main__':
    main()

```