---
layout:     post
title:      "Beautiful Soup"
subtitle:   ""
date:       2019-11-20 15:44:27
author:     "Balbo"
header-img: "img/post-bg-2019.jpg"
tags:
    - reptile
---
## 解析器
Beautiful Soup 在解析时实际上依赖解析器，他除了支持 Python 标准库中的 HTML 解析器外，还支持一些第三方解析器。

| 解析器 | 使用方法 | 优势 | 劣势 |
| ---- | ---- | ---- | ---- |
| Python标准库 | BeautifulSoup(markup, 'html.parser') | Python 的内置标准库、执行速度适中、文档容错能力强 | Python 2.7.3 及 Python 3.2.2 之前的版本文档容错能力差 |
| lxml HTML 解析器 | BeautifulSoup(markup, 'lxml') | 速度快、文档容错能力强 | 需要安装 C 语言库 |
| lxml XML 解析器 | BeautifulSoup(markup, 'xml') | 速度快、唯一支持 XML 的解析器 | 需要安装 C 语言库 |
| html5lib | BeautifulSoup(markup, 'html5lib') | 最好的容错性、以浏览器的方式解析文档、生成 HTML5 格式的文档 | 速度慢、不依赖外部扩展 |

## 基本用法
```python
html = """

<html><head><title>The Dormouse's story</title></head>

<body>

<p class="title" name="dromouse"><b>The Dormouse's story</b></p>

<p class="story">Once upon a time there were three little sisters; and their names were

<a href="http://example.com/elsie" class="sister" id="link1"><!-- Elsie --></a>,

<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and

<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;

and they lived at the bottom of a well.</p>

<p class="story">...</p>

"""
```
```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')
# 把解析的字符串以标准的缩进格式输出
print(soup.prettify())
print(soup.title.string)
```
运行结果：
```html
<html>
 <head>
  <title>
   The Dormouse's story
  </title>
 </head>
 <body>
  <p class="title" name="dromouse">
   <b>
    The Dormouse's story
   </b>
  </p>
  <p class="story">
   Once upon a time there were three little sisters; and their names were
   <a class="sister" href="http://example.com/elsie" id="link1">
    <!-- Elsie -->
   </a>
   ,
   <a class="sister" href="http://example.com/lacie" id="link2">
    Lacie
   </a>
   and
   <a class="sister" href="http://example.com/tillie" id="link3">
    Tillie
   </a>
   ;
and they lived at the bottom of a well.
  </p>
  <p class="story">
   ...
  </p>
 </body>
</html>
The Dormouse's story
```
## 节点选择器
### 选择元素
```python
from bs4 import BeautifulSoup

soup = BeautifulSoup(html, 'lxml')
print(soup.title)
print(type(soup.title))
print(soup.title.string)
print(soup.head)
print(soup.p)
```
运行结果：
```
<title>The Dormouse's story</title>
<class 'bs4.element.Tag'>
The Dormouse's story
<head><title>The Dormouse's story</title></head>
<p class="title" name="dromouse"><b>The Dormouse's story</b></p>
```
### 提取信息

1. 获取名称

    可以利用 name 属性获取节点的名称
    ```python
    print(soup.title.name)
    ```
    运行结果：
    ```
    title
    ```

2. 获取属性

    每个节点可能有多个属性，比如 id 和 class 等，选择这个节点元素后，可以调用 attrs 获取所有属性：
    ```python
    print(soup.p.attrs)
    print(soup.p.attrs['name'])
    ```
    运行结果：
    ```
    {'class': ['title'], 'name': 'dromouse'}
    dromouse
    ```

3. 获取内容

    可以利用 string 属性获取节点元素包含的文本内容
    ```python
    print(soup.p.string)
    ```
    运行结果：
    ```
    The Dormouse's story
    ```

## 嵌套选择
```python
print(soup.head.title)
print(type(soup.head.title))
print(soup.head.title.string)
```
运行结果：
```
<title>The Dormouse's story</title>
<class 'bs4.element.Tag'>
The Dormouse's story
```

## 关联选择

1. 子节点和子孙节点

    选取节点元素之后，如果先要获取他的直接子节点，可以调用 contents 属性
    ```python
    from bs4 import BeautifulSoup

    html = """

    <html>
    <head>
    <title>The Dormouse's story</title>
    </head>
    <body>
    <p class="story">
        Once upon a time there were three little sisters; and their names were
        <a href="http://example.com/elsie" class="sister" id="link1">
    <span>Elsie</span>
    </a>
    <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> 
    and
    <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>
    and they lived at the bottom of a well.
    </p>
    <p class="story">...</p>

    """

    soup = BeautifulSoup(html, 'lxml')
    print(soup.p.contents)
    ```
    运行结果：
    ```
    ['\n    Once upon a time there were three little sisters; and their names were\n    ', <a class="sister" href="http://example.com/elsie" id="link1">
    <span>Elsie</span>
    </a>, '\n', <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>, ' \nand\n', <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>, '\nand they lived at the bottom of a well.\n']
    ```
    contents 得到的是直接子节点的列表

    同样，我们可以调用 children 属性得到相应的结果
    ```python 
    from bs4 import BeautifulSoup

    soup = BeautifulSoup(html, 'lxml')
    print(soup.p.children)
    for i, child in enumerate(soup.p.children):
        print(i, child)
    ```
    运行结果：
    ```
    <list_iterator object at 0x000000000B301D48>
    0 
        Once upon a time there were three little sisters; and their names were
        
    1 <a class="sister" href="http://example.com/elsie" id="link1">
    <span>Elsie</span>
    </a>
    2 
    3 <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>
    4  
    and
    5 <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
    6 
    and they lived at the bottom of a well.
    ```
    如果要得到所有的子孙节点，可以调用 descendants 属性：
    ```python
    from bs4 import BeautifulSoup

    soup = BeautifulSoup(html, 'lxml')
    print(soup.p.descendants)
    for i, child in enumerate(soup.p.descendants):
        print(i, child)
    ```
    运行结果：
    ```
    <generator object Tag.descendants at 0x000000000B2B9548>
    0 
        Once upon a time there were three little sisters; and their names were
        
    1 <a class="sister" href="http://example.com/elsie" id="link1">
    <span>Elsie</span>
    </a>
    2 
    3 <span>Elsie</span>
    4 Elsie
    5 
    6 
    7 <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>
    8 Lacie
    9  
    and
    10 <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
    11 Tillie
    12 
    and they lived at the bottom of a well.
    ```

2. 父节点和祖先节点

    如果要获取某个节点元素的父节点，可以调用 parent 属性：
    ```python
    from bs4 import BeautifulSoup

    soup = BeautifulSoup(html, 'lxml')
    print(soup.a.parent)
    ```
    运行结果：
    ```
    <p class="story">
        Once upon a time there were three little sisters; and their names were
        <a class="sister" href="http://example.com/elsie" id="link1">
    <span>Elsie</span>
    </a>
    <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> 
    and
    <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
    and they lived at the bottom of a well.
    </p>
    ```
    如果想获取所有的祖先节点，可以调用parents 属性：
    ```python
    from bs4 import BeautifulSoup

    soup = BeautifulSoup(html, 'lxml')
    print(type(soup.p.parents))
    print(list(enumerate(soup.a.parents)))
    ```
    运行结果：
    ```
    <class 'generator'>
    [(0, <p class="story">
        Once upon a time there were three little sisters; and their names were
        <a class="sister" href="http://example.com/elsie" id="link1">
    <span>Elsie</span>
    </a>
    <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> 
    and
    <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
    and they lived at the bottom of a well.
    </p>), (1, <body>
    <p class="story">
        Once upon a time there were three little sisters; and their names were
        <a class="sister" href="http://example.com/elsie" id="link1">
    <span>Elsie</span>
    </a>
    <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> 
    and
    <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
    and they lived at the bottom of a well.
    </p>
    <p class="story">...</p>
    </body>), (2, <html>
    <head>
    <title>The Dormouse's story</title>
    </head>
    <body>
    <p class="story">
        Once upon a time there were three little sisters; and their names were
        <a class="sister" href="http://example.com/elsie" id="link1">
    <span>Elsie</span>
    </a>
    <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> 
    and
    <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
    and they lived at the bottom of a well.
    </p>
    <p class="story">...</p>
    </body></html>), (3, <html>
    <head>
    <title>The Dormouse's story</title>
    </head>
    <body>
    <p class="story">
        Once upon a time there were three little sisters; and their names were
        <a class="sister" href="http://example.com/elsie" id="link1">
    <span>Elsie</span>
    </a>
    <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a> 
    and
    <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
    and they lived at the bottom of a well.
    </p>
    <p class="story">...</p>
    </body></html>)]
    ```

3. 兄弟节点

    ```python
    from bs4 import BeautifulSoup

    html = """

    <html>
    <body>
    <p class="story">
            Once upon a time there were three little sisters; and their names were
    <a href="http://example.com/elsie" class="sister" id="link1">
    <span>Elsie</span>
    </a>
            Hello
    <a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> 
            and
    <a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>
            and they lived at the bottom of a well.
    </p>
    """

    soup = BeautifulSoup(html, 'lxml')
    print('Next Sibling', soup.a.next_sibling)
    print('Prev Sibling', soup.a.previous_sibling)
    print('Next Siblings', list(enumerate(soup.a.next_siblings)))
    print('Prev Siblings', list(enumerate(soup.a.previous_siblings)))
    ```
    运行结果：
    ```
    Next Sibling 
            Hello
    Prev Sibling 
            Once upon a time there were three little sisters; and their names were
    Next Siblings [(0, '\n        Hello\n'), (1, <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>), (2, ' \n        and\n'), (3, <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>), (4, '\n        and they lived at the bottom of a well.\n')]
    Prev Siblings [(0, '\n        Once upon a time there were three little sisters; and their names were\n')]
    ```

4. 提取信息

    ```python
    from bs4 import BeautifulSoup

    html = """
    <html>
    <body>
    <p class="story">
            Once upon a time there were three little sisters; and their names were
    <a href="http://example.com/elsie" class="sister" id="link1">Bob</a><a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> 

    </p>
    """

    soup = BeautifulSoup(html, 'lxml')
    print('Next Sibling:')
    print(type(soup.a.next_sibling))
    print(soup.a.next_sibling)
    print(soup.a.next_sibling.string)
    print('Parent:')
    print(type(soup.a.parents))
    print(list(soup.a.parents)[0])
    print(list(soup.a.parents)[0].attrs['class'])
    ```
    运行结果：
    ```
    Next Sibling:
    <class 'bs4.element.Tag'>
    <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>
    Lacie
    Parent:
    <class 'generator'>
    <p class="story">
            Once upon a time there were three little sisters; and their names were
    <a class="sister" href="http://example.com/elsie" id="link1">Bob</a><a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>
    </p>
    ['story']
    ```

## 方法选择器

- find_all()

    1. name

        我们可以根据节点名来查询元素
        ```python
        from bs4 import BeautifulSoup

        html = """
        <div class="panel">
        <div class="panel-heading">
        <h4>Hello</h4>
        </div>
        <div class="panel-body">
        <ul class="list" id="list-1">
        <li class="element">Foo</li>
        <li class="element">Bar</li>
        <li class="element">Jay</li>
        </ul>
        <ul class="list list-small" id="list-2">
        <li class="element">Foo</li>
        <li class="element">Bar</li>
        </ul>
        </div>
        </div>
        """

        soup = BeautifulSoup(html, 'lxml')
        print(soup.find_all(name='ul'))
        print(type(soup.find_all(name='ul')[0]))
        ```
        运行结果：
        ```
        [<ul class="list" id="list-1">
        <li class="element">Foo</li>
        <li class="element">Bar</li>
        <li class="element">Jay</li>
        </ul>, <ul class="list list-small" id="list-2">
        <li class="element">Foo</li>
        <li class="element">Bar</li>
        </ul>]
        <class 'bs4.element.Tag'>
        ```
        因为都是 Tag 类型，所以依然可以进行嵌套查询。
        ```python
        for ul in soup.find_all(name='ul'):
        print(ul.find_all(name='li'))
        for li in ul.find_all(name='li'):
            print(li.string)
        ```
        运行结果：
        ```
        [<li class="element">Foo</li>, <li class="element">Bar</li>, <li class="element">Jay</li>]
        Foo
        Bar
        Jay
        [<li class="element">Foo</li>, <li class="element">Bar</li>]
        Foo
        Bar
        ```

    2. attrs

        除了工具节点名查询，我们也可以传入一些属性来查询，        对于一些常用的属性，比如 id 和 class 等，我们可以不用 attrs 来传递
        ```python
        from bs4 import BeautifulSoup

        html = """
        <div class="panel">
        <div class="panel-heading">
        <h4>Hello</h4>
        </div>
        <div class="panel-body">
        <ul class="list" id="list-1">
        <li class="element">Foo</li>
        <li class="element">Bar</li>
        <li class="element">Jay</li>
        </ul>
        <ul class="list list-small" id="list-2">
        <li class="element">Foo</li>
        <li class="element">Bar</li>
        </ul>
        </div>
        </div>
        """

        soup = BeautifulSoup(html, 'lxml')
        print(soup.find_all(attrs={'id': 'list-1'}))
        print(soup.find_all(attrs={'name': 'elements'}))
        print('-----')
        print(soup.find_all(id='list-1'))
        print(soup.find_all(class_='element'))
        ```
        运行结果：
        ```
        [<ul class="list" id="list-1">
        <li class="element">Foo</li>
        <li class="element">Bar</li>
        <li class="element">Jay</li>
        </ul>]
        []
        -----
        [<ul class="list" id="list-1">
        <li class="element">Foo</li>
        <li class="element">Bar</li>
        <li class="element">Jay</li>
        </ul>]
        [<li class="element">Foo</li>, <li class="element">Bar</li>, <li class="element">Jay</li>, <li class="element">Foo</li>, <li class="element">Bar</li>]
        ```

    3. text

        text 参数可用来匹配节点的文本，传入的形式可以是字符串，可以是正则表达式对象
        ```python
        import re

        from bs4 import BeautifulSoup

        html = """
        <div class="panel">
        <div class="panel-body">
        <a>Hello, this is a link</a>
        <a>Hello, this is a link, too</a>
        </div>
        </div>
        """

        soup = BeautifulSoup(html, 'lxml')
        print(soup.find_all(text=re.compile('link')))
        ```
        运行结果：
        ```
        ['Hello, this is a link', 'Hello, this is a link, too']
        ```

- find()
find() 返回的是单个元素
```python
from bs4 import BeautifulSoup

html = """
<div class="panel">
<div class="panel-heading">
<h4>Hello</h4>
</div>
<div class="panel-body">
<ul class="list" id="list-1">
<li class="element">Foo</li>
<li class="element">Bar</li>
<li class="element">Jay</li>
</ul>
<ul class="list list-small" id="list-2">
<li class="element">Foo</li>
<li class="element">Bar</li>
</ul>
</div>
</div>
"""

soup = BeautifulSoup(html, 'lxml')
print(soup.find(name='ul'))
print(type(soup.find(name='ul')))
print(soup.find(class_='list'))
```
运行结果：
```
<ul class="list" id="list-1">
<li class="element">Foo</li>
<li class="element">Bar</li>
<li class="element">Jay</li>
</ul>
<class 'bs4.element.Tag'>
<ul class="list" id="list-1">
<li class="element">Foo</li>
<li class="element">Bar</li>
<li class="element">Jay</li>
</ul>
```

    - find_parents() 和 find_parent()： 前者返回所有祖先节点，后者返回直接父节点
    - find_next_siblings() 和 find_next_sibling()： 前者返回后面所有的兄弟节点，后者返回后面第一个兄弟节点
    - find_previous_siblings() 和 find_previous_sibling()： 前者返回前面所有的兄弟节点，后者返回前面第一个兄弟节点
    - find_all_next() 和 find_next()： 前者返回节点后所有符合条件的节点，后者返回第一个符合条件的节点
    - find_all_previous() 和 fing_previous()： 前者返回节点前所有符合条件的节点，后者返回第一个符合条件的节点

## CSS 选择器
使用 CSS 选择器时，只需要调用 select() 方法，传入相应的 CSS 选择器即可
```python
from bs4 import BeautifulSoup

html = """
<div class="panel">
<div class="panel-heading">
<h4>Hello</h4>
</div>
<div class="panel-body">
<ul class="list" id="list-1">
<li class="element">Foo</li>
<li class="element">Bar</li>
<li class="element">Jay</li>
</ul>
<ul class="list list-small" id="list-2">
<li class="element">Foo</li>
<li class="element">Bar</li>
</ul>
</div>
</div>
"""

soup = BeautifulSoup(html, 'lxml')
print(soup.select('.panel .panel-heading'))
print(soup.select('ul li'))
print(soup.select('#list-2 .element'))
print(type(soup.select('ul')[0]))
```
运行结果：
```
[<div class="panel-heading">
<h4>Hello</h4>
</div>]
[<li class="element">Foo</li>, <li class="element">Bar</li>, <li class="element">Jay</li>, <li class="element">Foo</li>, <li class="element">Bar</li>]
[<li class="element">Foo</li>, <li class="element">Bar</li>]
<class 'bs4.element.Tag'>
```

- 嵌套选择

    ```python
    from bs4 import BeautifulSoup

    soup = BeautifulSoup(html, 'lxml')
    for ul in soup.select('ul'):
        print(ul.select('li'))
    ```
    运行结果：
    ```
    [<li class="element">Foo</li>, <li class="element">Bar</li>, <li class="element">Jay</li>]
    [<li class="element">Foo</li>, <li class="element">Bar</li>]
    ```

- 获取属性

    ```python
    from bs4 import BeautifulSoup

    soup = BeautifulSoup(html, 'lxml')
    for ul in soup.select('ul'):
    print(ul['id'])
    print(ul.attrs['id'])
    ```
    运行结果：
    ```
    list-1
    list-1
    list-2
    list-2
    ```

- 获取文本

    ```python
    from bs4 import BeautifulSoup

    soup = BeautifulSoup(html, 'lxml')
    for li in soup.select('li'):
    print('GET Text:', li.get_text())
    print('String:', li.string)
    ```
    运行结果：
    ```
    GET Text: Foo
    String: Foo
    GET Text: Bar
    String: Bar
    GET Text: Jay
    String: Jay
    GET Text: Foo
    String: Foo
    GET Text: Bar
    String: Bar
    ```
