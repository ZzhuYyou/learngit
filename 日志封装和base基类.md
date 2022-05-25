#### 日志封装
```
class TestLog:
            # 初始化方法，每次实例化之前都要执行的方法
        def __init__(self):
```
```
            # 创建日志器
            self.log = logging.getLogger('好好学习')
```
```
            # 创建控制台处理器
        def consonle_head(self):
            self.console_headr = logging.StreamHandler()
            # 把定义好的格式放进来
            self.console_headr.setFormatter(self.set_fmt()[0])
            return self.console_headr
```
```
            #   创建文件处理器
        def file_head(self):
            self.file_headr = logging.FileHandler('./log.txt', encoding='utf-8')
            # 把定义好的文件格式传进来,因为返回值是元祖，所以用下标取值
            self.file_headr.setFormatter(self.set_fmt()[1])
            return self.file_headr
```
```
            # 定义打印格式
        def set_fmt(self):
            # 控制台的格式
            self.consonl_set = logging.Formatter(fmt='控制台--%(name)s--%(message)s--弟%(lineno)d行--%(filename)s')
            # 文件的格式
            self.file_set = logging.Formatter(fmt='文件--%(name)s--%(message)s--弟%(lineno)d行--%(filename)s')
            # 要返回值才能调用
            return self.consonl_set, self.file_set
```
```
        #     关键
        def get_log(self):
            self.log.addHandler(self.consonle_head())
            self.log.addHandler(self.file_head())
            return self.log
```
```
# 实例化对象
test = TestLog()
new = test.get_log()
new.warning('这是新方法打印的waning')
```
### base基类
先导包<br/>
import selenium<br/>
from selenium import webdriver<br/>
from selenium.webdriver.support.wait import WebDriverWait<br/>
```
class Open:
    def open_browser(self,type_):
        try:
            driver=getattr(webdriver,type_)()
        except Exception as a:
            print(a)
            driver=webdriver.Chrome()
        return driver
```
```
class Base:
    # 驱动浏览器
    def __init__(self,type_):
        self.driver = Open.open_browser(type_)
        self.driver.implicitly_wait(10)
```
```
    #输入网址
    def login(self,url):
        self.driver.get(url)
```
```
    # 定位元素
    def find(self,loc,timeout=10,loc):
        return WebDriverWait(self.driver,timeout=timeout).until(lambda x:x.find_elements(*loc))
```
```
    # 输入
    def input(self,loc,value):
        self.find(loc).send_keys(value)
```
```
    # 点击
    def clik(self,loc):
       return self.find(loc).click
```
```
    # 获取文本
    def text(self,loc):
        return self.find(loc).text
```