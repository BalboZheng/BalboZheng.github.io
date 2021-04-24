---
layout:     post
title:      "正则表达式"
subtitle:   "正则表达式"
date:       2019-11-17 14:06:25
author:     "Balbo"
header-img: "img/post-bg-2019.jpg"
tags:
    - reptile
    - java
    - python
---
| 模式 | 描述 |
| ---- | ---- |
| \w | 匹配字母、数字及下划线 |
| \W | 匹配字母、数字及下划线的字符 |
| \s | 匹配任意空白字符，等价于\[\t\n\r\f] |
| \S | 匹配任意非空字符 |
| \d | 匹配任意数字，等价于\[0~9] |
| \D | 匹配任意非数字的字符 |
| \A | 匹配字符串开头 |
| \Z | 匹配字符串结尾，如果存在换行，只匹配到换行前的结束字符串 |
| \z | 匹配字符串结尾，如果存在换行，同时还会匹配换行符 |
| \G | 匹配最后匹配完成的位置 |
| \n | 匹配一个换行符 |
| \t | 匹配一个制表符 |
| ᶺ | 匹配一行字符串的开头 |
| $ | 匹配一行字符串的结尾 |
| . | 匹配任意字符，处理换行符，当 re.DOTALL 标记被指定时，则可以匹配包括换行符的任意字符 |
| \[...] | 用来表示一组字符，单独列出，比如\[amk]匹配 a、m或k |
| \[ᶺ...] | 不在\[]中的字符，比如\[ᶺabc]匹配除了a、b、c之外的字符 |
| * | 匹配0个或多个表达式 |
| + | 匹配1个或多个表达式 |
| ? | 匹配0个或1个前面的正则表达式定义的片段，非贪婪方式 |
| \{n} | 精确匹配n个前面的表达式 |
| \{n, m} | 匹配n到m次由前面正则表达式定义的片段，贪婪方式 |
| a\|b | 匹配a或b |
| () | 匹配括号内的表达式，也表示一个组 |

## re.match 
尝试从字符串的起始位置匹配一个模式，如果不是起始位置匹配成功的话，match()就返回none。

> re.match(pattern, string, flags=0)

下面届通过实例，形象的介绍正则表达式：
### 精准匹配
```python
import re

content = 'Hello 123 4567 World_This is a Regex Demo'
print(len(content))
# \s:匹配任意空格字符 \d：数字   \d{4}:匹配4个数字 \w{10}:匹配10个字母数字下划线
# .匹配任意字符  *匹配0个多个前面的表达式 .*就是匹配任意非换行符的字符 $:以它之前的表达式结尾
result = re.match('^Hello\s\d\d\d\s\d{4}\s\w{10}.*Demo$', content)
print(result)
# 匹配到的内容
print(result.group())
# 匹配到的字符串区间范围
print(result.span())
```
### 泛匹配：
```python
import re

content = 'Hello 123 4567 World_This is a Regex Demo'
result = re.match('^Hello.*Demo$', content)
print(result)
print(result.group())
print(result.span())
```
### 匹配某一个目标字符串：
```python
import re

content = 'Hello 1234567 World_This is a Regex Demo'
result = re.match('^Hello\s(\d+)\sWorld.*Demo$', content)
print(result)
# 输出的是第一个()中正则表达式匹配到的字符串，即目标字符
print(result.group(1))
print(result.span())
```
### 贪婪匹配

当正则表达式中有“.”时，“.”会尽可能的匹配更多的字符，示例如下：
```python
import re

content = 'Hello 1234567 World_This is a Regex Demo'
# '+'匹配的是1个或多个字符，根据贪婪匹配规则，123456‘六个数字会被.*’匹配
result = re.match('^He.*(\d+).*Demo$', content)
print(result)
# 输出的是“7”，而不是“1234567”
print(result.group(1))
```
所以，贪婪模式可能导致最后的结果缺少一些字符
### 非贪婪匹配

使得‘.’尽可能匹配少的字符，即在‘.’后加上一个‘？’即可，‘？’指定为非贪婪匹配
```python
import re

content = 'Hello 1234567 World_This is a Regex Demo'
result = re.match('^He.*?(\d+).*Demo$', content)
print(result)
print(result.group(1))
```
### 匹配模式
```python
import re

content = '''Hello 1234567 World_This#这里有个换行符，而'.*'一般是不能匹配换行符的
is a Regex Demo
'''
# 指定匹配模式后‘.*’可以匹配任何字符
result = re.match('^He.*?(\d+).*?Demo$', content, re.S)
print(result.group(1))
```
### 转义
```python
import re

content = 'price is $5.00'
# 关键词要转义
result = re.match('price is \$5\.00', content)
print(result)
```
### 总结

上面就是常用正则表达式的使用，总结：

1. 尽量使用泛匹配
2. 使用括号得到匹配目标
3. 尽量使用非贪婪模式
4. 有换行符就用re.S
5. re.match必须从第一个字符开始匹配，如果第一个字符匹配不上，就返回None

> html中的换行符很多
> 
> re.serch比re.match更好用

## re.search

re.search是扫描整个字符串，返回第一个成功的匹配。

re.match示例
```python
import re

content = 'Extra stings Hello 1234567 World_This is a Regex Demo Extra stings'
result = re.match('Hello.*?(\d+).*?Demo', content)
# 由于match没有匹配上content中的第一个字符串，所以结果是None
print(result)
```
re.search示例
```python
import re

content = 'Extra stings Hello 1234567 World_This is a Regex Demo Extra stings'
result = re.search('Hello.*?(\d+).*?Demo', content)
print(result)
print(result.group(1))
```
所以：尽量使用**re.search**
## re.search实战演练

匹配出下面html中的：***齐秦 往事随风***
```python
import re
html = '''<div id="songs-list">
    <h2 class="title">经典老歌</h2>
    <p class="introduction">
        经典老歌列表
    </p>
    <ul id="list" class="list-group">
        <li data-view="2">一路上有你</li>
        <li data-view="7">
            <a href="/2.mp3" singer="任贤齐">沧海一声笑</a>
        </li>
        <li data-view="4" class="active">
            <a href="/3.mp3" singer="齐秦">往事随风</a>
        </li>
        <li data-view="6"><a href="/4.mp3" singer="beyond">光辉岁月</a></li>
        <li data-view="5"><a href="/5.mp3" singer="陈慧琳">记事本</a></li>
        <li data-view="5">
            <a href="/6.mp3" singer="邓丽君"><i class="fa fa-user"></i>但愿人长久</a>
        </li>
    </ul>
</div>'''
result = re.search('<li.*?active.*?singer="(.*?)">(.*?)</a>', html, re.S)
if result:
    print(result.group(1), result.group(2))
```
注意：re.search是返回的是第一个匹配到的字符串，比如下面的案例：
```python
import re

html = '''<div id="songs-list">
    <h2 class="title">经典老歌</h2>
    <p class="introduction">
        经典老歌列表
    </p>
    <ul id="list" class="list-group">
        <li data-view="2">一路上有你</li>
        <li data-view="7">
            <a href="/2.mp3" singer="任贤齐">沧海一声笑</a>
        </li>
        <li data-view="4" class="active">
            <a href="/3.mp3" singer="齐秦">往事随风</a>
        </li>
        <li data-view="6"><a href="/4.mp3" singer="beyond">光辉岁月</a></li>
        <li data-view="5"><a href="/5.mp3" singer="陈慧琳">记事本</a></li>
        <li data-view="5">
            <a href="/6.mp3" singer="邓丽君">但愿人长久</a>
        </li>
    </ul>
</div>'''
result = re.search('<li.*?singer="(.*?)">(.*?)</a>', html, re.S)
if result:
    print(result.group(1), result.group(2))
```
上面打印的是：***任贤齐 沧海一声笑***

为了体现出re.S的作用，我们把re.S去掉试试
```python
import re

html = '''<div id="songs-list">
    <h2 class="title">经典老歌</h2>
    <p class="introduction">
        经典老歌列表
    </p>
    <ul id="list" class="list-group">
        <li data-view="2">一路上有你</li>
        <li data-view="7">
            <a href="/2.mp3" singer="任贤齐">沧海一声笑</a>
        </li>
        <li data-view="4" class="active">
            <a href="/3.mp3" singer="齐秦">往事随风</a>
        </li>
        <li data-view="6"><a href="/4.mp3" singer="beyond">光辉岁月</a></li>
        <li data-view="5"><a href="/5.mp3" singer="陈慧琳">记事本</a></li>
        <li data-view="5">
            <a href="/6.mp3" singer="邓丽君">但愿人长久</a>
        </li>
    </ul>
</div>'''
result = re.search('<li.*?singer="(.*?)">(.*?)</a>', html)
if result:
    print(result.group(1), result.group(2))
```
上面打印的是：***beyond 光辉岁月***
## re.findall

搜索字符串，以列表形式返回全部能匹配的子串。

下面是具体的实例：
```python
import re

html = '''<div id="songs-list">
    <h2 class="title">经典老歌</h2>
    <p class="introduction">
        经典老歌列表
    </p>
    <ul id="list" class="list-group">
        <li data-view="2">一路上有你</li>
        <li data-view="7">
            <a href="/2.mp3" singer="任贤齐">沧海一声笑</a>
        </li>
        <li data-view="4" class="active">
            <a href="/3.mp3" singer="齐秦">往事随风</a>
        </li>
        <li data-view="6"><a href="/4.mp3" singer="beyond">光辉岁月</a></li>
        <li data-view="5"><a href="/5.mp3" singer="陈慧琳">记事本</a></li>
        <li data-view="5">
            <a href="/6.mp3" singer="邓丽君">但愿人长久</a>
        </li>
    </ul>
</div>'''
＃ 使用非贪婪模式
results = re.findall('<li.*?href="(.*?)".*?singer="(.*?)">(.*?)</a>', html, re.S)
print(results)
print(type(results))
for result in results:
    # list类型
    print(result)
    print(result[0], result[1], result[2])
```
运行结果：
```
    [('/2.mp3', '任贤齐', '沧海一声笑'), ('/3.mp3', '齐秦', '往事随风'), ('/4.mp3', 'beyond', '光辉岁月'), ('/5.mp3', '陈慧琳', '记事本'), ('/6.mp3', '邓丽君', '但愿人长久')]
    　
    <class 'list'>
    　　
    ('/2.mp3', '任贤齐', '沧海一声笑')
    /2.mp3 任贤齐 沧海一声笑　
    　
    ('/3.mp3', '齐秦', '往事随风')
    /3.mp3 齐秦 往事随风
    　
    ('/4.mp3', 'beyond', '光辉岁月')　
    /4.mp3 beyond 光辉岁月
    　
    ('/5.mp3', '陈慧琳', '记事本')
    /5.mp3 陈慧琳 记事本
    　
    ('/6.mp3', '邓丽君', '但愿人长久')
    /6.mp3 邓丽君 但愿人长久
```
但是上面的“一路有你”那首歌没有检测出来，那么如何匹配出所有的歌名呢？
```python
# 前面代码不变
results = re.findall('<li.*?>\s*?(<a.*?>)?(\w+)(</a>)?\s*?</li>', html, re.S)
print(results)
for result in results:
    print(result[1])
```

> 注意：
    后边多一个？表示懒惰模式。
    必须跟在*或者+后边用

### re.sub

替换字符串中每一个匹配的子串后返回替换后的字符串。
```python
import re

content = 'Extra stings Hello 1234567 World_This is a Regex Demo Extra stings'
# 正则表达式匹配到的字符串使用'replace'代替，返回替换后的字符串
content = re.sub('\d+', 'replace', content)
print(content)
# 输出：Extra stings Hello replace World_This is a Regex Demo Extra stings

```
如果替换时需要使用到匹配到的原字符串：
```python
import re

content = 'Extra stings Hello 1234567 World_This is a Regex Demo Extra stings'
content = re.sub('(\d+)', r'\1 8910', content)#\1:表示的就是匹配到的字符串
print(content)
# 输出：Extra stings Hello 1234567 8910 World_This is a Regex Demo Extra stings
```
这个替换操作很有用，尤其是在提取之前，可以屏蔽部分小的差异，例如
```python
import re

html = '''<div id="songs-list">
    <h2 class="title">经典老歌</h2>
    <p class="introduction">
        经典老歌列表
    </p>
    <ul id="list" class="list-group">
        <li data-view="2">一路上有你</li>
        <li data-view="7">
            <a href="/2.mp3" singer="任贤齐">沧海一声笑</a>
        </li>
        <li data-view="4" class="active">
            <a href="/3.mp3" singer="齐秦">往事随风</a>
        </li>
        <li data-view="6"><a href="/4.mp3" singer="beyond">光辉岁月</a></li>
        <li data-view="5"><a href="/5.mp3" singer="陈慧琳">记事本</a></li>
        <li data-view="5">
            <a href="/6.mp3" singer="邓丽君">但愿人长久</a>
        </li>
    </ul>
</div>'''
# '|'表示OR，匹配‘|’之前的或之后的表达式
html = re.sub('<a.*?>|</a>', '', html)
print(html)
results = re.findall('<li.*?>(.*?)</li>', html, re.S)
print(results)
for result in results:
    print(result.strip())
```
匹配的结果

执行后的结果：
```
    <div id="songs-list">
    <h2 class="title">经典老歌</h2>
    <p class="introduction">
    经典老歌列表
    </p>
    <ul id="list" class="list-group">
    <li data-view="2">一路上有你</li>
    <li data-view="7">
    沧海一声笑
    </li>
    <li data-view="4" class="active">
    往事随风
    </li>
    <li data-view="6">光辉岁月</li>
    <li data-view="5">记事本</li>
    <li data-view="5">
    但愿人长久
    </li>
    </ul>
    </div>
```
在替换后的基础上，匹配所有歌曲就显得很简单了，只需要在替换后的基础上添加下面代码就可以了：
```python
results = re.findall('<li.*?>(.*?)</li>', html, re.S)
print(results)
for result in results:
    # 去掉多余的空格
    print(result.strip()
```
## re.compile

将正则字符串编译成正则表达式对象(方便复用)
```python
import re

content = '''Hello 1234567 World_This
is a Regex Demo'''
# 定义正则
pattern = re.compile('Hello.*Demo', re.S)
# 使用正则，方便重复使用
result = re.match(pattern, content)
print(result)
```
