### 自动化登录案例：
1.导入unittest包，selenium包<br/>
2.新建一个类继承unittest里面的TestCase<br/>
3.在类里面test方法，一个方法就是一个用例 <br/>


 def test_01(self):
 用self.drvier=webdriver.Chome()//打开谷歌浏览器


打开网址<br/>

self.driver.get('http://124.221.246.225:8080/logout')


找到登录框用name找，并且用send_keys输入账号<br/>


self.driver.find_element_by_name('loginName').send_keys('13308127108')


找到密码框并且输入密码<br/>


 self.driver.find_element_by_name('password').send_keys('123456')


找到登录按钮（用xpath方法，因为没有名字）并且点击click()<br/>


self.driver.find_element_by_xpath('//*[@id="loginForm"]/div[2]/input').click()


退出浏览器(在这之前用time.slee()加个等待时间)<br/>


self.driver.quit()


然后用main方法进行打开<br/>


if __name__ == '__main__':
    unittest.main()


就是unittest里面的main方法<br/>
二。在每次用例执行之前可以用setup方法添加重复的操作（打开浏览器和输入网址）<br/>


def setUp(self) -> None:
    self.driver=webdriver.Chrome()
    self.driver.get('http://124.221.246.225:8080/logout')
用例执行结束后可以用（关闭浏览器和延迟关闭时间）<br/>
def tearDown(self) -> None:
    time.sleep(4)
    self.driver.quit()


需要测试的数据可以用变量来代替，不要写死，添加数据驱动from ddt import data，unpack，然后在类上面加解释器@ddt,在方法上面data里面添加列表数据,然后在用unpack解包就可以<br/>
生成测试报告<br/>
先导入unittese包和HtmlTestRuner包<br/>
再找到用例文件（括号里面使路径和文件名）<br/>


discover = unittest.defaultTestLoader.discover('.', 'cs_01.py')


执行测试用例，自动创建测试报告（选择wb二进制的方式）with下面要缩进<br/>


with open('../report/report.html','wb') as f:
    HTMLTestRunner.HTMLTestRunner(f,2,'login hmtl').run(discover)



zmail发送邮件<br/>
第一步就是先导入zmail包，然后定义个变量把自己的邮箱账号和授权码放进去，然后用zmail.sever给进行使用<br/>


mymail={'username':'Zhuyouzai7@163.com','password':'EFYNWTSHNGFGMJKD'}
server=zmail.server(mymail['username'],mymail['password'])


第二步再写邮件的内容(标题subject，正文content_text，附件Attachments）,用字典类型<br/>


content_mail={
    'subject':'来自兄弟的第一封邮件',
    'content_text':'python来',   'Attachments':'../report/report.html'
}


第三步就是写要发给谁，对方的邮箱账号，再用sendmail发送<br/>



revics=['Zhuyouzai7@163.com','1751523922@qq.com']
server.send_mail(revics,content_mail)



然后把这个发送邮件给封装成函数，再去用例执行的后面进行一个调用就可以了，调用之前加个时间响应一下<br/>
 单接口测试	先导入request包<br/>
先写请求的路径赋值给变量url<br/>


url='http://124.221.246.225:8085/api/v1/user/login'


要请求的参数赋值给data<br/>


 data={"loginName": "13308127108","passwordMd5": "e10adc3949ba59abbe56e057f20f883e"}


发送请求request.请求方式(路径，（json格式）=参数)，打印在控制台上<br/>


res=requests.post(url, json=data)
print(res.json())


写一个预期的结果可以根据响应状态关键字<br/>


mesagA = 'SUCCESS'


再把实际的结果给提取出来<br/>


mesagB = res.json()['message']


再用if else语句作对比<br/>


 if mesagA==mesagB:
    print('用例成功')
else:
    print('用例失败')


用Excel进行python接口自动化<br/>
首先导入openpyxl包<br/>
然后用openpyxl.workbook找到Excel表的位置和名字<br/>


 workbook=openpyxl.load_workbook('casetest.xlsx')


然后找到表里面的数据是在哪一sheet页<br/>


sheet=workbook['Sheet1']


然后找到表里面的值可以用单元格的行和列的编号来进行匹配<br/>


 list1=[]
for i in range(2,6):
    dict_case=dict(id=sheet.cell(row=i,column=1).value,
    url=sheet.cell(row=i,column=2).value,
    data=sheet.cell(row=i,column=3).value)
    list1.append(dict_case)
return list1


行号可以用for循环+range范围，变量来代替，然后整体的数据可以放在字典里面dict（） ，封装成函数在下一个页面可以再去调用。归类一下。然后再另外的页面可以去直接调用，但是需要ddt装饰器，解包<br/>


 import unittest
from ddt import ddt,data,unpack,file_data
from Study.request_01 import post_request
from Study.test_02 import excel_red


@ddt()
class TaseCase(unittest.TestCase):
    @data(*excel_red())
    @unpack
    def test_1(self,id,url,data):
        # print(id,url,data)
        post_request(url, data)

if __name__ == '__main__':
    unittest.main()


### pytest相关 
+ pytest <br/>
    - 作用：管理用例，生成测试报告，用例重报，用例并发<br/>
    - pytest框架<br/>
    - 测试文件以test_开头或者以_text结尾<br/>
    - 测试类以Test开头，并且不能带有int方法<br/>
    - 测试函数以test_开头<br/>
    - 断言使用基本的assert即可<br/>
    - 基本使用：首先导入pytest包<br/>
    - 然后编写类格式要规范Test大写 <br/>
    - 然后定义方法，每一方法都是一个用例（可以用assert断言）<br/>



class TestCase:
    def test_01(self):
        print('这是第一个错误用例')
        assert 1==2


运行的方法也是可以在控制台pytest+文件名或者main方法运行，pytest.main()，里面可以写上[-sv+文件名]展示详细的结果内容<br/>


 if __name__ == '__main__':
    pytest.main(['-sv','test_st1.py'])


然后可以用setup和teardown方法去定义前置和后置的操作。还可以用setup_class和teardown_class去定义，区别就是setup每一个方法都要去执行打开关闭，setup_class就是整体执行前要做的事情，所有用例执行之后，再去执行后置的。 <br/>


 class TestCase:
    def setup_class(self):
        print('打开浏览器')
    def teardown_class(self):
        print('关闭浏览器')
然后在pytest里面可以用装饰器来指定用例执行的顺序<br/>


 @pytest.mark.run(order=2)
def test_01(self):
    print('这是第一个错误用例')
    assert 1==2
    没找到元素解决：1	.表达式有问题，确定没有就去2.加强制等待，影视等待，显示等待3.看是否有iframe 窗口切换 ，特殊元素 <br/>
 mes='登陆失败!'
sj=WebDriverWait(self.driver,10).until(lambda x:x.find_element_by_xpath('/html/body/div[3]/div')).text
assert mes == sj


加显示等待定位界面中的登录失败<br/>
面试：<br/>
元素定位失败的原因：是否添加等待<br/>
句柄是否切换（一个页面跳转到另外一个界面）<br/>
如果元素在iframe中要用switch to 来进行切换<br/>
元素是否被遮挡<br/>
元素值是否正确   id，name，xpath，坐标，css<br/>
元素定位否是第一个（检查表达式定位的是多个中的哪个，是不是自己需要的）<br/>


数据驱动:<br/>
excel驱动：驱动较为死板，管理起来比较麻烦<br/>
ymal文件驱动：数据直观，可以灵活优化，但实现对于技术要求较高<br/>
其它：txt，csv，xml <br/>
+ 如何实现高质量的自动化脚本：<br/>
    - 独立化测试场景，每一个场景都需要独立起来 ，避免将代码都封装在一个函数里面，提升代码维护性，避免冗余<br/>
    - 应用合理的设计模式（关键字驱动，pom），结合业务需求，团队技术能力，自动化执行目的，确保设计出来的框架以最少的工作投入，覆盖最多的测试需求<br/>
    - 使用分层结构设计，确保实际运行中测试代码，测试数据，逻辑彻底分离<br/>
    - 对于业务流程，核心是从业务的正向流程去分析与设计测试用例 ，不要把所有操作企图自动化实现<br/>
    - 函数的封装上，需要考虑更加灵活的方法来实现<br/>
元素定位的方式：id，name，classname，link，css，xpath，byxpath（属性，text，层级定位，逻辑运算符）<br/>
用代码批量生成手机号数据并且放在表里（最后千万不能忘记实例化对象）<br/>
1.用Faker+for循环来生成批量的号码<br/>


 faker = Faker(locale="zh_CN")
for i in range(50):
    # # 姓名
    # print(faker.name())
    # # 电话
    # print(faker.phone_number())
    # 把生成的数据保存在表里面
    self.sheet.append([faker.name(),faker.phone_number()])
    # 保存文件的路径
    self.book.save(Red_path().red_path())


然后用Excel表格的方式进去追加写入（需要导入openpyxl包）先定义个类，再实例化对象，定义表和sheet页<br/>
实例化对象找到表，找到sheet页，active默认第一页<br/>


 def __init__(self):

    self.book=Workbook()
    self.sheet=self.book.active


定义标题和内容，把上面的for循环给放进来，进行追加写入<br/>
 定义个方法写入首行的标题，保存进去<br/>


 def c_ecexl(self):

    title_data = ['姓名','手机号']
    self.sheet.append(title_data)



    # 用faker生成数据
    faker = Faker(locale="zh_CN")
    for i in range(50):
        # # 姓名
        # print(faker.name())
        # # 电话
        # print(faker.phone_number())
        # 把生成的数据保存在表里面
        self.sheet.append([faker.name(),faker.phone_number()])


再写要保存的路径（这个就需要去封装一个路径的函数）<br/>
保存文件的路径<br/>


self.book.save(Red_path().red_path())<br/>


### 封装查看路径的函数filelib里面的path（用//和包名进行拼接）先找到主路径，然后拼接就可以<br/>
封装查看文件的路径<br/>
from pathlib import Path
 找到前面公共的路径<br/>


class Red_path():
     Base=Path(__file__).resolve().parent.parent
     print(Base)


定义一个查找的方法，用包名进行拼接，后面是要查看的文件名，然后再实例化对象<br/>


 def red_path(self):
       run_path = Red_path.Base/"Study"/"phone.xlsx"
       return run_path
red=Red_path()
print(red.red_path())


### 表单切换：switch_to
self.driver.switch_to.frame('定位的name或者id都可以')<br/>
如果要从a页面切换到b页面需要先返回到默认主界面目录<br/>
> self.driver.switch_to.default_content()
再从主界面切换到b界面<br/>

句柄切换<br/>
先获取所有窗口的的句柄，用变量接收一下<br/>


handles = self.driver.window_handles


再获取当前窗口的句柄<br/>


> current = self.driver.current_window_handle


然后用for循环遍历，因为获取的所有句柄是列表类型，再用if去判断，执行用switch_to切换<br/>


for i in handles:
    if i != current:
    切换 
    self.driver.switch_to.window(i)


