---
layout:     post
title:      "验证码的识别"
subtitle:   "验证码在很多地方都会用到"
date:       2019-11-25 16:38:20
author:     "Balbo"
header-img: "img/post-bg-2019.jpg"
catalog: true
tags:
    - reptile
    - python
---
## 图像验证码的识别
### 获取验证码

保存验证码图片以便实验，[验证](http://my.cnki.net/elibregister/CheckCode.aspx)

### 识别测试

使用 tesserocr 库识别验证码(由于 tesseract 与 window 不兼容， 暂时用 pytesseract 代替)
```python
import tesserocr
from PIL import Image

image = Image.open('code.jpg')
result = tesserocr.image_to_string(image)
print(result)
```

### 验证码处理

验证码内的多余线条可能会干扰图片的识别，对于这种情况，我们还需要做一些额外的处理,例如：

图片转化为灰度图像
```python
image = image.covert('L')
image.show()
```
进行二值化处理
```python
image = image.covert('1')
image.show()
```
指定二值化阈值
```python
image = image.covert('L')
threshold = 80
table = []
for i in range(256):
    if i < threshold:
        table.append(0)
    else:
        table.append(1)
        
image = image.point(table, '1')
image.show()
```

## 极验滑动验证码的识别
### 识别思路

对于应用了极验验证码的网站，如果我们中介模拟表单提交，加密参数的构造是个问题，需要分析其加密和校验逻辑，相对繁琐。随意我们采用直接模拟浏览器动作的方式来完成验证

首先，找到一个带有极验验证码的网站： http://account.geetest.com/login

次按钮为智能验证按钮。如果同一会话，一段时间内第二次点击会直接通过验证。如果智能识别不通过，则会弹出滑动窗口

所以，识别验证需要完成三步 

1. 模拟点击验证按钮
2. 识别滑动缺口的位置
3. 模拟拖动滑块

### 初始化

```python
EMAIL = 'test@test.com'
PASSWORD = '123456'


class CrackGeetest():

    def __init__(self):
        self.url = 'http://account.geetest.com/login'
        self.browser = webdriver.Chrome()
        self.wait = WebDriverWait(self.browser, 20)
        self.email = EMAIL
        self.password = PASSWORD
```

### 模拟点击

```python
    def get_geetest_button(self):
        """
        获取初始化验证按钮
        :return: 按钮对象
        """
        button = self.wait.until(EC.element_to_be_clickable((By.CLASS_NAME, 'geetest_radar_tip')))
        return button
```
获取一个 WebElement 对象，调用它的 click() 方法即可模拟点击
```python
        button = get_geetest_button()
        button.click()
```

### 识别缺口

```python
    def get_screenshot(self):
        """
        获取网页截图
        :return: 截图对象
        """
        screenshot = self.browser.get_screenshot_as_png()
        screenshot = Image.open(BytesIO(screenshot))
        return screenshot
    
    def get_position(self):
        """
        获取验证码位置
        :return: 验证码位置元组
        """
        img = self.wait.until(EC.presence_of_element_located((By.CLASS_NAME, 'geetest_cavas_img')))
        time.sleep(2)
        location = img.location
        size = img.size
        top, bottom, left, right = location['y'], location['y'] + size['height'], location['x'], location['x'] + size['width']
        return (top, bottom, left, right)
    
    def get_geetest_image(self, name='captcha.png'):
        """
        获取验证码图片
        :param name: 
        :return: 图片对象
        """
        top, bottom, left, right = self.get_position()
        print('验证码位置', top, bottom, left, right)
        screenshot = self.get_screenshot()
        captcha = screenshot.crop((left, top, right, bottom))
        return captcha
```
接下来，获取第二张图片，也就是带缺口的图片，要使图片出现缺口，只需要点击下方的滑动块即可
```python
    def get_slider(self):
        """
        获取滑块
        :return:滑块对象 
        """
        silder = self.wait.until(EC.element_to_be_clickable((By.CLASS_NAME, 'geetest_slider_button')))
        return silder
```
调用 click() 方法即可触发点击
```python
        slider = self.get_slider()
        slider.click()
```
接下来对比图片获取缺口。我们在这里遍历每个坐标点，获取两张图片对应像素点的 RGB 数据。如果 RGB 数据差距在一定范围内，代表两个像素相同，继续对比下一个像素，如果差距超过一定范围，则代表像素点不同，即缺口。
```python
    def is_pixel_equal(self, image1, image2, x, y):
        """
        判断两个像素是否相同
        :param image1: 图片1
        :param image2: 图片2
        :param x: 位置x
        :param y: 位置y
        :return: 像素是否相同
        """
        pixel1 = image1.load()[x, y]
        pixel2 = image2.load()[x, y]
        threshold = 60
        if abs(pixel1[0] - pixel2[0]) < threshold and abs(pixel1[1] - pixel2[1]) < threshold and \
                abs(pixel1[2] - pixel2[2]) < threshold:
            return True
        else:
            return False
        
    def get_gap(self, image1, image2):
        """
        还缺缺口偏移量
        :param image1:不带缺口图片 
        :param image2: 带缺口图片
        :return: 
        """
        left = 60
        for i in range(left, image1.size[0]):
            for j in range(image1.size[1]):
                if not self.is_pixel_equal(image1, image2, i, j):
                    left = i
                    return left
        return left
```

### 模拟拖动

模拟拖动过程并不复杂，但其中的坑比较多。如果是匀速运动，极验必然会识别出是程序操作，以为人无法做到完全匀速拖动。极验验证码利用机器学习模型，筛选此类数据为机器操作，验证码必失败。

所以，我们采用前段做匀加速运动，后段做匀减速运动，来模拟人拖动时的动作，利用物理学的加速度公式完成验证。
```python
# 当前速度 v，初速度 v0， 位移 x， 所需时间 t
x = v0 * t + 0.5 * a * t * t
v = v0 + a * t
```
构造轨迹移动算法，计算出先加速后减速的运动轨迹
```python
    def get_track(self, distance):
        """
        根据偏移量获取运动轨迹
        :param distance: 偏移量
        :return: 运动轨迹
        """
        # 运动轨迹
        track = []
        # 当前位移
        current = 0
        # 减速阈值
        mid = distance * 4 / 5
        # 计算间隔
        t = 0.2
        # 初速度
        v = 0

        while current < distance:
            if current < mid:
                # 加速度为+2
                a = 2
            else:
                # 加速度为-3
                a = -3
            # 初速度 v0
            v0 = v
            # 当前速度 v = v0 + at
            v = v0 + a * t
            # 移动距离 x = v0t + 1/2 * a * t * t
            move = v0 *t + 1 / 2 * a * t * t
            # 当前位移
            current += move
            # 加入轨迹
            track.append(round(move))
            
        return track
```
按照运动轨迹拖动滑块
```python
    def move_to_gap(self, slider, tracks):
        """
        拖动滑块到缺口处
        :param slider: 滑块
        :param tracks: 缺口
        :return: 
        """
        ActionChains(self.browser).click_and_hold(slider).perform()
        for x in tracks:
            ActionChains(self.browser).move_by_offset(xoffset=x, yoffset=0).perform()
        time.sleep(0.5)
        ActionChains(self.browser).release().perform()
```

### 完善表单，模拟登录

```python
BORDER = 6

    def __del__(self):
        self.browser.close()

    def open(self):
        """
        打开网页输入用户名密码
        :return: None
        """
        self.browser.get(self.url)
        email = self.wait.until(EC.presence_of_element_located((By.ID, 'email')))
        password = self.wait.until(EC.presence_of_element_located((By.ID, 'password')))
        email.send_keys(self.email)
        password.send_keys(self.password)

    def login(self):
        """
        登录
        :return: None
        """
        submit = self.wait.until(EC.element_to_be_clickable((By.CLASS_NAME, 'login-btn')))
        submit.click()
        time.sleep(10)
        print('登录成功')

    def crack(self):
        # 输入用户名密码
        self.open()
        # 点击验证按钮
        button = self.get_geetest_button()
        button.click()
        # 获取验证码图片
        image1 = self.get_geetest_image('captcha1.png')
        # 点按呼出缺口
        slider = self.get_slider()
        slider.click()
        # 获取带缺口的验证码图片
        image2 = self.get_geetest_image('captcha2.png')
        # 获取缺口位置
        gap = self.get_gap(image1, image2)
        print('缺口位置', gap)
        # 减去缺口位移
        gap -= BORDER
        # 获取移动轨迹
        track = self.get_track(gap)
        print('滑动轨迹', track)
        # 拖动滑块
        self.move_to_gap(slider, track)

        success = self.wait.until(
            EC.text_to_be_present_in_element((By.CLASS_NAME, 'geetest_success_radar_tip_content'), '验证成功'))
        print(success)

        # 失败后重试
        if not success:
            self.crack()
        else:
            self.login()


if __name__ == '__main__':
    crack = CrackGeetest()
    crack.crack()
```

## 点触验证码的识别
### 识别思路

互联网上有很多验证码服务平台，这里使用 [超级鹰](http://www.chaojiying.com) 其提供的服务种类非常广泛。

### [注册账号](https://www.chaojiying.com/user/reg)

### 获取 API

下载对应的 [API](https://www.chaojiying.com/api-14.html)
```python
import requests
from hashlib import md5
 
 
class Chaojiying(object):
 
    def __init__(self, username, password, soft_id):
        self.username = username
        self.password = md5(password.encode('utf-8')).hexdigest()
        self.soft_id = soft_id
        self.base_params = {
            'user': self.username,
            'pass2': self.password,
            'softid': self.soft_id,
        }
        self.headers = {
            'Connection': 'Keep-Alive',
            'User-Agent': 'Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.1; Trident/4.0)',
        }
 
    def post_pic(self, im, codetype):
        """
        im: 图片字节
        codetype: 题目类型 参考 http://www.chaojiying.com/price.html
        """
        params = {
            'codetype': codetype,
        }
        params.update(self.base_params)
        files = {'userfile': ('ccc.jpg', im)}
        r = requests.post('http://upload.chaojiying.net/Upload/Processing.php', data=params, files=files,
                          headers=self.headers)
        return r.json()
 
    def report_error(self, im_id):
        """
        im_id:报错题目的图片ID
        """
        params = {
            'id': im_id,
        }
        params.update(self.base_params)
        r = requests.post('http://upload.chaojiying.net/Upload/ReportError.php', data=params, headers=self.headers)
        return r.json()
```

### 初始化

```python
# http://admin.touclick.com/login.html的邮箱
EMAIL = '787549184@qq.com'
# http://admin.touclick.com/login.html的密码
PASSWORD = ''

CHAOJIYING_USERNAME = '787549184'
CHAOJIYING_PASSWORD = ''
CHAOJIYING_SOFT_ID = 898551
CHAOJIYING_KIND = 9102

class CrackTouClick():
    def __init__(self):
        self.url = 'http://admin.touclick.com/login.html'
        self.browser = webdriver.Chrome()
        self.wait = WebDriverWait(self.browser, 20)
        self.email = EMAIL
        self.password = PASSWORD
        self.chaojiying = Chaojiying(CHAOJIYING_USERNAME, CHAOJIYING_PASSWORD, CHAOJIYING_SOFT_ID)
```

### 获取验证码

完善相关表单，模拟点击呼出验证码
```python
    def open(self):
        """
        打开网页输入用户名密码
        :return: None
        """
        self.browser.get(self.url)
        email = self.wait.until(EC.presence_of_element_located((By.ID, 'email')))
        password = self.wait.until(EC.presence_of_element_located((By.ID, 'password')))
        email.send_keys(self.email)
        password.send_keys(self.password)

    def get_touclick_button(self):
        """
        获取初始验证按钮
        :return:
        """
        button = self.wait.until(EC.element_to_be_clickable((By.CLASS_NAME, 'touclick-hod-wrap')))
        return button
```
获取验证码图片位置和大小
```python
    def get_touclick_element(self):
        """
        获取验证图片对象
        :return: 图片对象
        """
        element = self.wait.until(EC.presence_of_element_located((By.CLASS_NAME, 'touclick-pub-content')))
        return element

    def get_position(self):
        """
        获取验证码位置
        :return: 验证码位置元组
        """
        element = self.get_touclick_element()
        time.sleep(2)
        location = element.location
        size = element.size
        top, bottom, left, right = location['y'], location['y'] + size['height'], location['x'], location['x'] + size[
            'width']
        return (top, bottom, left, right)

    def get_screenshot(self):
        """
        获取网页截图
        :return: 截图对象
        """
        screenshot = self.browser.get_screenshot_as_png()
        screenshot = Image.open(BytesIO(screenshot))
        return screenshot

    def get_touclick_image(self, name='captcha.png'):
        """
        获取验证码图片
        :return: 图片对象
        """
        top, bottom, left, right = self.get_position()
        print('验证码位置', top, bottom, left, right)
        screenshot = self.get_screenshot()
        captcha = screenshot.crop((left, top, right, bottom))
        captcha.save(name)
        return captcha
```

### 识别验证码
调用 Chaojiying 对象的 post_pic() 方法，即可把图片发生到超级鹰后台
```python
        # 获取验证码图片
        image = self.get_touclick_image()
        bytes_array = BytesIO()
        image.save(bytes_array, format='PNG')
        # 识别验证码
        result = self.chaojiying.post_pic(bytes_array.getvalue(), CHAOJIYING_KIND)
        print(result)
```
将 pic_str() 识别的文字坐标 解析，然后模拟点击
```python
    def get_points(self, captcha_result):
        """
        解析识别结果
        :param captcha_result: 识别结果
        :return: 转化后的结果
        """
        groups = captcha_result.get('pic_str').split('|')
        locations = [[int(number) for number in group.split(',')] for group in groups]
        return locations

    def touch_click_words(self, locations):
        """
        点击验证图片
        :param locations: 点击位置
        :return: None
        """
        for location in locations:
            print(location)
            ActionChains(self.browser).move_to_element_with_offset(self.get_touclick_element(), location[0],
                                                                   location[1]).click().perform()
            time.sleep(1)
```

### 完善表单，模拟登录

```python
    def touch_click_verify(self):
        """
        点击验证按钮
        :return: None
        """
        button = self.wait.until(EC.element_to_be_clickable((By.CLASS_NAME, 'touclick-pub-submit')))
        button.click()

    def login(self):
        """
        登录
        :return: None
        """
        submit = self.wait.until(EC.element_to_be_clickable((By.ID, '_submit')))
        submit.click()
        time.sleep(10)
        print('登录成功')

    def crack(self):
        """
        破解入口
        :return: None
        """
        self.open()
        # 点击验证按钮
        button = self.get_touclick_button()
        button.click()
        # 获取验证码图片
        image = self.get_touclick_image()
        bytes_array = BytesIO()
        image.save(bytes_array, format='PNG')
        # 识别验证码
        result = self.chaojiying.post_pic(bytes_array.getvalue(), CHAOJIYING_KIND)
        print(result)
        locations = self.get_points(result)
        self.touch_click_words(locations)
        self.touch_click_verify()
        # 判定是否成功
        success = self.wait.until(
            EC.text_to_be_present_in_element((By.CLASS_NAME, 'touclick-hod-note'), '验证成功'))
        print(success)

        # 失败后重试
        if not success:
            self.crack()
        else:
            self.login()


if __name__ == '__main__':
    crack = CrackTouClick()
    crack.crack()
```

## 微博宫格验证码的识别

```python
import os
import time
from io import BytesIO
from PIL import Image
from selenium import webdriver
from selenium.common.exceptions import TimeoutException
from selenium.webdriver import ActionChains
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
from os import listdir

USERNAME = 'xxxxxx'
PASSWORD = 'xxxxxxx'

TEMPLATES_FOLDER = 'templates/'


class CrackWeiboSlide():
    def __init__(self):
        self.url = 'https://passport.weibo.cn/signin/login?entry=mweibo&r=https://m.weibo.cn/'
        self.browser = webdriver.Chrome()
        self.wait = WebDriverWait(self.browser, 20)
        self.username = USERNAME
        self.password = PASSWORD
    
    def __del__(self):
        self.browser.close()
    
    def open(self):
        """
        打开网页输入用户名密码并点击
        :return: None
        """
        self.browser.get(self.url)
        username = self.wait.until(EC.presence_of_element_located((By.ID, 'loginName')))
        password = self.wait.until(EC.presence_of_element_located((By.ID, 'loginPassword')))
        submit = self.wait.until(EC.element_to_be_clickable((By.ID, 'loginAction')))
        username.send_keys(self.username)
        password.send_keys(self.password)
        submit.click()
    
    def get_position(self):
        """
        获取验证码位置
        :return: 验证码位置元组
        """
        try:
            img = self.wait.until(EC.presence_of_element_located((By.CLASS_NAME, 'patt-shadow')))
        except TimeoutException:
            print('未出现验证码')
            self.open()
        time.sleep(2)
        location = img.location
        size = img.size
        top, bottom, left, right = location['y'], location['y'] + size['height'], location['x'], location['x'] + size[
            'width']
        return (top, bottom, left, right)
    
    def get_screenshot(self):
        """
        获取网页截图
        :return: 截图对象
        """
        screenshot = self.browser.get_screenshot_as_png()
        screenshot = Image.open(BytesIO(screenshot))
        return screenshot
    
    def get_image(self, name='captcha.png'):
        """
        获取验证码图片
        :return: 图片对象
        """
        top, bottom, left, right = self.get_position()
        print('验证码位置', top, bottom, left, right)
        screenshot = self.get_screenshot()
        captcha = screenshot.crop((left, top, right, bottom))
        captcha.save(name)
        return captcha
    
    def is_pixel_equal(self,image1,image2,x,y):#判断刷新出来的验证码图片和已经存储好的验证码模板是否匹配
        pixel1=image1.load()[x,y]#返回的是RGB形式的元组
        pixel2=image2.load()[x,y]
        threshold=20#设定一个阈值
        if abs(pixel1[0] - pixel2[0]) < threshold and abs(pixel1[1] - pixel2[1]) < threshold and abs(
                pixel1[2] - pixel2[2]) < threshold:
            return True 
        else:
            return False
        
    def same_image(self,image,template):#第一个为待识别的验证码，第二个为存储的模板，只要是比对一下各个点的像素
            count=0#用于存储像素相同点的个数
            threshold=0.99#阈值，用于标定两张图表是否相同
            for i in range(image.width):
                for j in range(image.height):
                    if self.is_pixel_equal(image,template,i,j):#如果像素相同
                        count+=1
            result=float(count)/(image.width*image.height)#判断相同的像素点的占比
            if result>threshold:
                print ('成功匹配')
                return True 
            else：
                return False
        
    
     def detect_image(self,image):
         for template_name in listdir(TEMPLATES_FOLDER):
            print('正在匹配', template_name)
            template = Image.open(TEMPLATES_FOLDER + template_name)
            if self.same_image(image, template):
                # 返回顺序
                numbers = [int(number) for number in list(template_name.split('.')[0])]
                print('拖动顺序', numbers)
                return numbers
    
    def move(self,numbers):
        circles=self.browser.find_elements_by_css_selector('.patt-wrap .patt-circ')#获得的是四个按钮的列表
        dx=dy=0#初始一个原始的初始移动位置
        for index in range(4):
            circle=circles[numbers[index]-1]#numbers是1-4的一个列表
            # 如果是第一次循环
            if index == 0:
                # 点击第一个按点
                ActionChains(self.browser) \
                    .move_to_element_with_offset(circle, circle.size['width'] / 2, circle.size['height'] / 2) \
                    .click_and_hold().perform()
            else:
                # 小幅移动次数
                times = 30
                # 拖动
                for i in range(times):
                    ActionChains(self.browser).move_by_offset(dx / times, dy / times).perform()#当dx=dy=0的时候，index=0，不会执行这一句
                    time.sleep(1 / times)
            # 如果是最后一次循环
            if index == 3:
                # 松开鼠标
                ActionChains(self.browser).release().perform()
            else:
                # 计算下一次偏移
                dx = circles[numbers[index + 1] - 1].location['x'] - circle.location['x']
                dy = circles[numbers[index + 1] - 1].location['y'] - circle.location['y']
                
    def crack(self):
        """
        破解入口
        :return:
        """
        self.open()
        # 获取验证码图片
        image = self.get_image('captcha.png')
        numbers = self.detect_image(image)
        self.move(numbers)
        time.sleep(10)
        print('识别结束')
            
                    
            
        
    
     def main(self):
        count=0 
        while True:
            self.open()
            self.get_image(str(count)+'.png')
            count+=1
            
            
if __name__=='__main__':
    crack= CrackWeiboSlide()
    crack.crack()
```