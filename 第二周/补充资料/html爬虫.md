
## requests
requests 是用Python语言编写，基于 urllib，采用 Apache2 Licensed 开源协议的 HTTP 库。它比 urllib 更加方便。

## 安装
如果安装了Anaconda，requests就已经可用了。否则，需要在命令行下通过pip安装：

pip install requests


```python
#导入requests包
import requests

#访问豆瓣的首页
r = requests.get('https://www.douban.com/')

#查看状态码
print(r.status_code)

'''
常见的几种状态码：
200 ：请求成功
301 ：永久重定向
302 ：临时重定向
400 ：请求错误
403 ：没有权限访问
404 ：页面不存在
500 ：服务器内部错误
'''
#输出内容
# print(r.text)

#对于带参数的URL，传入一个dict作为params参数：
r = requests.get('https://www.douban.com/search', params={'q': 'python', 'cat': '1001'})

# 实际请求的URL
print(r.url)

#requests自动检测编码，可以使用encoding属性查看：
print(r.encoding)

#需要传入HTTP Header时，我们传入一个dict作为headers参数：
headers = {'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36'}
r = requests.get('https://www.douban.com/', headers = headers)
    
#要发送POST请求，只需要把get()方法变成post()，然后传入data参数作为POST请求的数据：
r = requests.post('https://accounts.douban.com/login', data={'form_email': 'abc@example.com', 'form_password': '123456'})

#更多详细的内容可以查看官方的文档：https://2.python-requests.org//zh_CN/latest/user/quickstart.html
```

    200
    https://www.douban.com/search?q=python&cat=1001
    utf-8


## Beautiful Soup
Beautiful Soup提供一些简单的、python式的函数用来处理导航、搜索、修改分析树等功能。它是一个工具箱，通过解析文档为用户提供需要抓取的数据，因为简单，所以不需要多少代码就可以写出一个完整的应用程序。

Beautiful Soup自动将输入文档转换为Unicode编码，输出文档转换为utf-8编码。你不需要考虑编码方式，除非文档没有指定一个编码方式，这时，Beautiful Soup就不能自动识别编码方式了。然后，你仅仅需要说明一下原始编码方式就可以了。

### 安装Beautiful Soup

可以直接pip install beautifulsoup4进行安装

注意：在PyPi中还有一个名字是 BeautifulSoup 的包,但那可能不是你想要的,那是 Beautiful Soup3 的发布版本,因为很多项目还在使用BS3, 所以 BeautifulSoup 包依然有效.但是如果你在编写新项目,那么你应该安装的 beautifulsoup4

### 安装解析器
Beautiful Soup支持Python标准库中的HTML解析器,还支持一些第三方的解析器,其中一个是 lxml 。

可以直接pip安装：

pip install lxml

推荐使用lxml作为解析器，因为效率更高。在Python2.7.3之前的版本和Python3中3.2.2之前的版本，必须安装lxml或html5lib，因为那些Python版本的标准库中内置的HTML解析方法不够稳定。


```python
from bs4 import BeautifulSoup
# 下面是一段html代码
html_doc = """
<html><head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.</p>
<p class="story">...</p>
"""

#使用BeautifulSoup解析这段代码,能够得到一个 BeautifulSoup 的对象,并能按照标准的缩进格式的结构输出:
# soup = BeautifulSoup(html_doc, 'html.parser')
#html.parser是python自带的解析器，lxml是第三方的解析器
soup = BeautifulSoup(html_doc, 'lxml')
print(soup.prettify())

```

    <html>
     <head>
      <title>
       The Dormouse's story
      </title>
     </head>
     <body>
      <p class="title">
       <b>
        The Dormouse's story
       </b>
      </p>
      <p class="story">
       Once upon a time there were three little sisters; and their names were
       <a class="sister" href="http://example.com/elsie" id="link1">
        Elsie
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



```python
#几个简单的浏览结构化数据的方法:
soup.title
# <title>The Dormouse's story</title>

soup.title.name
# u'title'

soup.title.string
# u'The Dormouse's story'

soup.title.parent.name
# u'head'

soup.p
# <p class="title"><b>The Dormouse's story</b></p>

soup.p['class']
# u'title'

soup.a
# <a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>

soup.find_all('a')
# [<a class="sister" href="http://example.com/elsie" id="link1">Elsie</a>,
#  <a class="sister" href="http://example.com/lacie" id="link2">Lacie</a>,
#  <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>]

soup.find(id="link3")
# <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>
```




    <a class="sister" href="http://example.com/tillie" id="link3">Tillie</a>




```python
#从文档中找到所有<a>标签的链接:
for link in soup.find_all('a'):
    print(link.get('href'))
    # http://example.com/elsie
    # http://example.com/lacie
    # http://example.com/tillie

```

    http://example.com/elsie
    http://example.com/lacie
    http://example.com/tillie



```python
#从文档中获取所有文字内容:
print(soup.get_text())
# The Dormouse's story
#
# The Dormouse's story
#
# Once upon a time there were three little sisters; and their names were
# Elsie,
# Lacie and
# Tillie;
# and they lived at the bottom of a well.
#
# ...
```

    The Dormouse's story
    
    The Dormouse's story
    Once upon a time there were three little sisters; and their names were
    Elsie,
    Lacie and
    Tillie;
    and they lived at the bottom of a well.
    ...


​    

更多详细的用法可以查看Beautiful Soup文档

https://www.crummy.com/software/BeautifulSoup/bs4/doc/index.zh.html#

### 练习：基于beautifulsoup的爬虫
整体流程大致如下：

1.使用requests访问糗事百科的网站，获取网站内容

2.使用beautifulsoup提取幽默笑话的数据


```python
import requests
from bs4 import BeautifulSoup

#headers的意思就是告诉网站，我们是一个正常的浏览器在给它发送信息，请它给我们正确的信息。一个简单的伪装。
headers = {'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36'}
r = requests.get('https://www.qiushibaike.com/text/', headers = headers)
#使用lxml解析网站内容
soup = BeautifulSoup(r.text, 'lxml')

#在网页源码中可以发下，所有的笑话都在span标签下，但是直接找所有的span标签，容易找到其他不需要的数据，所有这里就先找span的上一级标签
divs = soup.find_all(class_ = 'content')

#由于是找到所有的div class=conten的标签,所以需要用循环，从每一个标签里网下找span标签
for div in divs:
    joke = div.span.get_text()
    print(joke)

    
```

    二闺女昨天晚上和老婆说希望明天上辅导班的时候希望她妈妈开电动车送她，因为她不想走路去。说着说着她就跟老婆说:如果不答应的话，就要断绝母女关系。老婆被气乐了，断绝母女关系？那好啊，你现在就给我出去，我已经不是你妈妈了，没有义务给你做饭、洗衣服，你也不能待在我家。二闺女:我要先分家产呢。老婆:你都和我断绝母女关系了，我的家产为什么要给一个外人呢？我把所有好吃的、好衣服给姐姐。二闺女:妈妈，我明天走路去上课

    老婆早上换了身新裙子，儿子抱着她说：“妈妈你穿这条裙子真像个公主。” 老婆边笑边抱着亲了一通，回头瞪我：“孩子都比你嘴甜，学学！” 我挠挠头：“老婆你穿这条裙子真像个公主。” 老婆冷笑：“你还去过那种地方？”


​    

除了beautifusoup，还可以使用正则表达式,jsonpath,xpath,css选择器，json数据优先选择使用jsonpath，html页面个人比较喜欢使用xpath或者css选择器,如果数据很难通过这两种方式获取，这时再去考虑使用正则表达式。

## Xpath
XPath 是一门在 XML 文档中查找信息的语言。XPath 可用来在 XML 文档中对元素和属性进行遍历。XPath 是 W3C XSLT 标准的主要元素，并且 XQuery 和 XPointer 都构建于 XPath 表达之上。
### 安装
pip install lxml

### 基本使用


```python
#导入lxml库的etree模块
from lxml import etree

#一部分html文本，注意最后一个li标签没有闭合
wb_data = """
        <div>
            <ul>
                 <li class="item-0"><a href="link1.html">first item</a></li>
                 <li class="item-1"><a href="link2.html">second item</a></li>
                 <li class="item-inactive"><a href="link3.html">third item</a></li>
                 <li class="item-1"><a href="link4.html">fourth item</a></li>
                 <li class="item-0"><a href="link5.html">fifth item</a>
             </ul>
         </div>
        """
#构造一个Xpath解析的对象，在解析过程中自动补全代码
html = etree.HTML(wb_data)
print(html)

#查看补全的代码，在结果中可以看到添加了html、body
result = etree.tostring(html)
print(result.decode("utf-8"))
```

    <Element html at 0x2646c02e9c8>
    <html><body><div>
                <ul>
                     <li class="item-0"><a href="link1.html">first item</a></li>
                     <li class="item-1"><a href="link2.html">second item</a></li>
                     <li class="item-inactive"><a href="link3.html">third item</a></li>
                     <li class="item-1"><a href="link4.html">fourth item</a></li>
                     <li class="item-0"><a href="link5.html">fifth item</a>
                 </li></ul>
             </div>
            </body></html>



```python
#使用绝对路径匹配所有的li标签
result = html.xpath('/html/body/div/ul/li')
#也可以使用相对路径,用 // 表示
result = html.xpath('//li')
print(result)
#这里可以看到提取结果是一个列表形式，其中每个元素都是一个 Element对象。
```

    [<Element li at 0x2646c2bce48>, <Element li at 0x2646c2bce08>, <Element li at 0x2646c13fdc8>, <Element li at 0x2646c252888>, <Element li at 0x2646d4b9148>]



```python
#获取li下的a标签
result = html.xpath('//li/a')
print(result)

#注意匹配规则要一级一级的写，不能跳过，比如result = html.xpath('//ul/a')
```

    [<Element a at 0x2646bcf0188>, <Element a at 0x2646bc1ea08>, <Element a at 0x2646bc1e608>, <Element a at 0x2646d499a88>, <Element a at 0x2646d4993c8>]



```python
#当标签有属性时，也可以通过@属性去匹配
result = html.xpath('//li[@class="item-0"]')
print(result)
```

    [<Element li at 0x2646d8c1f88>, <Element li at 0x2646d4997c8>]



```python
#获取文本的内容,使用text()方法
result = html.xpath('//li/a/text()')
print(result)
```

    ['first item', 'second item', 'third item', 'fourth item', 'fifth item']



```python
#获取属性值，也用@
result = html.xpath('//li/a/@href')
print(result)
```

    ['link1.html', 'link2.html', 'link3.html', 'link4.html', 'link5.html']


### 练习：使用Xpath爬取百度新闻


```python
# 导入reques库和etree
import requests
from lxml import etree

#获取百度新闻的内容
r = requests.get('https://news.baidu.com/')

#构建解析的对象
html = etree.HTML(r.text)

#根据想要获取的内容，写匹配规则，这里只爬取了部分标题
news = html.xpath('//ul/li/a[@target="_blank"]/text()')
for new in news:
    print(new)
```

    七七事变82周年 抗战馆展出文物照片回味"文化抗战"
    27省份平均工资出炉，京沪津非私营年均超10万元
    不仅是防校园性侵！最高检态度坚决，要"没完没了"抓这件事
    南方强降雨来势汹汹 中国气象局启动四级应急响应
    港铁：为维护车站秩序 停售7日中午至晚间所有高铁车票
    山洪暴发致使进入莫高窟唯一道路中断 景区已暂停开放
    申遗成功的良渚古城遗址，因何而特别？
    茅台召开专家论证会：文化茅台不是空中楼阁要有具体载体
    四川宜宾市珙县发生2.8级地震 震源深度11千米
    山西：载30吨柴油的罐车发生碰撞 柴油持续喷泄
    岛内好感度调查显示蔡英文排第199，台网友：不意外
    三峡大坝变形？回应：工程运行安全可靠  
    伊朗正式宣布:浓缩铀丰度将超伊核协议规定的3.67%
    伊拉克大使：美对伊朗施压制裁 伊拉克将坚定站在伊朗身边
    伊朗解释击落无人机缘由：曾收到美国将要打击的警告
    杜特尔特：美国老在怂恿中菲对抗 当我们是蚯蚓吗
    阿富汗发生汽车炸弹袭击致12死43伤 塔利班称对此负责
    土耳其警方在汽车内发现核武器原料，价值7200万美元
    圆明园沉睡百年古莲复活盛开 引众游客驻足拍照
    法国总统府被盗 7件贵重艺术品不翼而飞
    美洲杯上被红牌罚下，梅西拒绝领奖：不想成为腐败的一份子
    日本停车场内发现中国女子遗体：背后有刀伤 疑遭弃尸
    老外酒驾追尾想掏钱摆平 交警怒怼“你要遵守中国法律!”
    上海城管检查收集运输环节：一垃圾站混合收运被责令整改
    河南长垣警方"清理库存"：悬赏100万通缉36名逃犯
    暖心！孕妇列车上突然临盆 紧急时刻幸好有他们在
    代买水果刀还要保密？外卖小哥接到订单，越想越不对劲
    河南拦路打老师案被告人家属：接通知 10日将宣判
    儿子遇车祸成“植物人” 公婆不忍耽误儿媳 代儿起诉离婚
    15岁男生在电梯里倒了瓶饮料 上百位邻居却要哭了 
    习近平：推进国家治理体系和治理能力现代化
    习近平会见孟加拉国总理哈西娜
    七七事变82周年
    良渚古城
    申遗成功
    27省工资出炉
    46个城市
    推进垃圾分类
    暴雨预警
    停机断网
    能充话费
    今日小暑
    飞天茅台批发价
    华为被禁加剧芯片过剩 三星电子利润腰斩
    全球首个AI设计药物进入人体试验阶段
    42款APP存在超范围收集用户信息等违规行为
    韩国威胁对日本限制高科技材料出口实施报复
    在苹果召回之前 一台MacBook Pro电池炸了
    滴滴宣布试行新规：司机开车送还物品将收费
    新浪娱乐微博账号短暂被销号？误操作
    为支持华为，马来西亚零售商联手干了这件事
    欧盟成员国拒绝基于WiFi的车联网技术标准
    沈南鹏：中国第一投资人如何炼成的？
    微信否认大规模封号 已打击上百万外挂账号
    苹果与韩国反垄断监管机构达成和解协议
    比特大陆完成员工期权合同签约 上市渐近
    360回应手机业务暂停：仍在寻找5G机会
    百度：L4乘用车前装产线已开始正式投产下线
    苹果首席设计师：一个职业技校学生的奋斗史


## 正则表达式
正则表达式，通常被用来检索、替换那些符合某个模式(规则)的文本。

正则表达式是对字符串操作的一种逻辑公式，就是用事先定义好的一些特定字符、及这些特定字符的组合，组成一个“规则字符串”，这个“规则字符串”用来表达对字符串的一种过滤逻辑。

给定一个正则表达式和另一个字符串，我们可以达到如下的目的：

给定的字符串是否符合正则表达式的过滤逻辑（“匹配”）；

通过正则表达式，从文本字符串中获取我们想要的特定部分（“过滤”）。

每个字符的意义如下：
![image.png](attachment:image.png)



```python
import re
#re.match函数
#re.match(pattern, string)
#pattern 匹配的正则表达式
#string 要匹配的字符串。
line = "Cats are smarter than dogs"
result = re.match( r'(.*) are (.*?) .*', line)
print(result.group())    #整个表达式的字符串
print(result.group(1))
print(result.group(2))
```

    Cats are smarter than dogs
    Cats
    smarter



```python
#re.search方法:扫描整个字符串并返回第一个成功的匹配。
line = "Cats are smarter than dogs"
result = re.search( r'(.*) are (.*?) .*', line)
print(result.group())    #整个表达式的字符串
print(result.group(1))
print(result.group(2))

#re.match与re.search的区别:re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。
```

    Cats are smarter than dogs
    Cats
    smarter



```python
#检索和替换:Python 的 re 模块提供了re.sub用于替换字符串中的匹配项。
#re.sub(pattern, repl, string)
#pattern : 正则中的模式字符串。
#repl : 替换的字符串，也可为一个函数。
#string : 要被查找替换的原始字符串。
phone = "2004-959-559"
num = re.sub(r'\D', "", phone)  #去除字符串中的-
print("电话号码是 : ", num)
```

    电话号码是 :  2004959559



```python
#re.compile：compile 函数用于编译正则表达式，生成一个正则表达式（ Pattern ）对象，供 match() 和 search() 这两个函数使用。
#re.compile(pattern[, flags])
#pattern : 一个字符串形式的正则表达式
#flags : 可选，表示匹配模式，比如忽略大小写，多行模式等，具体参数为：
#re.I 忽略大小写
#re.L 表示特殊字符集 \w, \W, \b, \B, \s, \S 依赖于当前环境
#re.M 多行模式
#re.S 即为 . 并且包括换行符在内的任意字符（. 不包括换行符）
#re.U 表示特殊字符集 \w, \W, \b, \B, \d, \D, \s, \S 依赖于 Unicode 字符属性数据库
#re.X 为了增加可读性，忽略空格和 # 后面的注释
pattern = re.compile(r'([a-z]+) ([a-z]+)', re.I)   # re.I 表示忽略大小写
m = pattern.match('Hello World Wide Web')
print(m.group(0)) 
```

    Hello World



```python
#findall：在字符串中找到正则表达式所匹配的所有子串，并返回一个列表，如果没有找到匹配的，则返回空列表。
#注意： match 和 search 是匹配一次 findall 匹配所有。
#findall(string[, pos[, endpos]])
#string : 待匹配的字符串。
#pos : 可选参数，指定字符串的起始位置，默认为 0。
#endpos : 可选参数，指定字符串的结束位置，默认为字符串的长度。
pattern = re.compile(r'\d+')   # 查找数字
result = pattern.findall('hello world 123 baidu 456')
print(result)
```

    ['123', '456']


### 练习：使用正则表达式爬取豆瓣图书 


```python
import requests
import re
url = 'https://book.douban.com/'
headers = {'User-Agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36'}
r = requests.get(url, headers=headers)
#用re获取数据时，是可以直接在字符串中用正则表达式提取，不用解析html
# 获取链接、标题、作者
patter = re.compile('<li class.*?cover.*?href="(.*?)".*?alt="(.*?)".*?<p class="author".*?>(.*?)</p>', re.S)
titles = re.findall(patter, r.text)
for each in titles:
    print('书籍链接:{},书籍标题：{},---书籍作者：{}'.format(each[0], each[1],each[2].strip()))
```

    书籍链接:https://book.douban.com/subject/30435804/?icn=index-latestbook-subject,书籍标题：多项选择,---书籍作者：作者：[英]阿瑟·克拉克
    书籍链接:https://book.douban.com/subject/33379779/?icn=index-topchart-subject,书籍标题：美国陷阱,---书籍作者：作者：[法] 弗雷德里克·皮耶鲁齐&nbsp;/&nbsp;[法] 马修·阿伦
    书籍链接:https://book.douban.com/subject/33445034/?icn=index-topchart-subject,书籍标题：喜鹊谋杀案,---书籍作者：作者：[英] 安东尼·霍洛维茨&nbsp;/&nbsp;Anthony Horowitz
    书籍链接:https://book.douban.com/subject/33385402/?icn=index-topchart-subject,书籍标题：事实,---书籍作者：作者：[瑞典] 汉斯·罗斯林&nbsp;/&nbsp;[瑞典] 欧拉·罗斯林&nbsp;/&nbsp;[瑞典]  安娜·罗斯林·罗朗德
    书籍链接:https://book.douban.com/subject/33426127/?icn=index-topchart-subject,书籍标题：渺小一生,---书籍作者：作者：[美]柳原汉雅
    书籍链接:https://book.douban.com/subject/30293663/?icn=index-topchart-subject,书籍标题：绝对笑喷之弃业医生日志,---书籍作者：作者：[英]亚当·凯
    书籍链接:https://book.douban.com/subject/30488936/?icn=index-topchart-subject,书籍标题：给所有人的黑塞童话,---书籍作者：作者：[德] 赫尔曼·黑塞
    书籍链接:https://book.douban.com/subject/30443973/?icn=index-topchart-subject,书籍标题：想象一朵未来的玫瑰,---书籍作者：作者：[葡]费尔南多·佩索阿
    书籍链接:https://book.douban.com/subject/30419377/?icn=index-topchart-subject,书籍标题：重生三部曲,---书籍作者：作者：[英]派特·巴克
    书籍链接:https://book.douban.com/subject/30324950/?icn=index-topchart-subject,书籍标题：像艺术家一样思考,---书籍作者：作者：[英] 威尔·贡培兹&nbsp;/&nbsp;Will Gompertz

