# Python3-爬虫基本语法

**Python 爬虫基本语法**
```python
#爬取糗事百科热门段子，爬取地址URL: http://www.qiushibaike.com/hot/page/1
from bs4 import BeautifulSoup
import urllib.request
import csv
import re
import time
#防止错误：HTTP Error 403: Forbidden
headers = {'Accept': '*/*',
           'Accept-Language': 'en-US,en;q=0.8',
           'Cache-Control': 'max-age=0',
           'User-Agent': 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/48.0.2564.116 Safari/537.36',
           'Connection': 'keep-alive',
           'Referer': 'http://www.baidu.com/'
           }

link = 'http://www.qiushibaike.com/hot/page/'
#page = 1
for page in range(2, 14):
    time.sleep(1)
    print(page)
    url = link+str(page)
    req = urllib.request.Request(url, headers=headers)
    html_doc = urllib.request.urlopen(req)
    soup = BeautifulSoup(
    	html_doc,#HTML文档字符串
    	'html.parser'#HTML解析器
    	#from_encoding = 'utf-8'#bs会自动检测编码格式，然后转换为Unicode，但是如果指定可以加快速度。
    	)
    #print(soup.prettify())
    #articles = soup.find_all()
    articles = soup.find_all(class_="content")
    #注意这里class有下划线。
    #articles = soup.find_all('div',class_=re.compile("^article block untagged mb15.*$"))
    with open(r"comment.csv","a",encoding='utf-8',newline="") as csv_file:
        csv_writer = csv.writer(csv_file, delimiter=',')
        for article in articles:
            temp = article.select('span')[0]
            temp = re.sub('\n','',str(temp))#先去掉换行符，再去掉标签。
            csv_writer.writerow([re.sub(r'<.*?>','\n',str(temp))])

```







**基本库**
beautifulsoup 中文文档:https://beautifulsoup.readthedocs.io/zh_CN/v4.4.0/
参考：https://cuiqingcai.com/1319.html

```python
from bs4 import BeautifulSoup
import urllib.request
import re
```


**向服务器发送请求**

向服务器发送获取页面的请求-普通：
```python
html_doc = urllib.request.urlopen(URL)
```

疑问：返回的是什么？
参考：https://blog.csdn.net/bo_mask/article/details/76067790

向服务器发送获取页面的请求-加入headers验证：
req = urllib.request.Request(url, headers=headers)
html_doc = urllib.request.urlopen(req)

**编码**
参考beautiful Soup官方文档。
任何HTML或XML文档都有自己的编码方式,比如ASCII 或 UTF-8,但是使用Beautiful Soup解析后,文档都被转换成了Unicode:

对于返回的html文档，可以用BeautifulSoup对文档进行解码，方便后序操作。
soup = BeautifulSoup(html_doc, 'html.parser'，from_)
soup = BeautifulSoup(
    html_doc,#HTML文档字符串
    'html.parser'#HTML解析器
     #注意：这里的解析器【html.parser】python内置的解析器，还有多种更好的解析器。
    #from_encoding = 'utf-8'
     #bs会自动检测编码格式，然后转换为Unicode，但是如果指定可以加快速度,指定错了就唧唧，所以我不加了。
    )


**BeautifulSoup的方法**
#获取某个属性的整个标签的内容。
soup.find_all(name, attrs)
print(soup.prettify())#html标准化格式输出。
#找所有img标签，属性为data-original，内容我里面的正则表达式。重点，对于有下划线的属性处理，放在字典里。

soup.find_all(id='link2')#找id是link2的所有标签【id】查找
soup.find("form", class_="edit_checkout")#寻找form下类名为这个的标签。【标签、类名】查找
data_soup.find_all(attrs={"value": "data-foo"})【其他属性】查找

正则查找：比较难
soup.find_all('img', attrs={'data-original':re.compile("^//pic.qiantucdn.com")})
soup.find_all(re.compile("^b"))#找所有b开头的标签
articles = soup.find_all('div',class_=re.compile("^article block untagged mb15.*$"))
写了快两个小时喽。
注意：按照CSS类名搜索tag的功能非常实用,但标识CSS类名的关键字class在Python中是保留字,使用class做参数会导致语法错误.从Beautiful Soup的4.1.1版本开始,可以通过class_参数搜索有指定CSS类名的tag。

**CSS选择器**
标签名不加任何修饰，类名前加点，id名前加 #，在这里我们也可以利用类似的方法来筛选元素，用到的方法是 soup.select()，返回类型是 list。
这里注意：可以选择多次，返回的也是soup对象，但是不要从顶级标签开始选择。

soup.select('title') #通过标签名查找
soup.select('.sister')#通过类名查找
soup.select('#link1')#通过id名查找
soup.select('p #link1')#组合查找，标签为p的id为link1的标签。

soup.select("head > title")#子标签查找，head下的title标签。#箭头左右要打空格，不然字符串识别不出来。
house_url = house.select(".listCon > h3 > a")[0]['href']#获取属性
house_title = house.select(".listCon > h3 > a")[0].string#获取内容

查找时还可以加入属性元素，属性需要用中括号括起来，注意属性和标签属于同一节点，所以中间不能加空格，否则会无法匹配到。
soup.select('a[class="sister"]')#属性查找。
articles = soup.select('div[class^="article block untagged mb15.*$"]')
articles = soup.select('div[class^="article block untagged mb15"] > a > div > span')
#使用了正则，虽然很隐蔽！就是必须以字符串为开头的类名就可以。

**保存内容到本地csv文件**
#open(文件地址和名称，有文件就打开-覆盖写入-无就创建，在添加新纪录时默认会添加一个空行)
#注意绝对路径和相对路径的打开参考：https://blog.csdn.net/m0_37693335/article/details/81474995

import csv#注意添加编码格式，否则可能出现gbk编码错误等。
with open("rent.csv","w",encoding='utf-8', newline="") as csv_file:
    csv_writer = csv.writer(csv_file, delimiter=',')
    #打开文件的作用域，所有关于文件的读写操作都要在这里面写了，不要出去了。注意缩进。
    csv_writer.writerow(lines)



**正则表达式**
^： 匹配字符串的开头--这里是从头匹配，说明只可能有一个。
$:匹配字符串的末尾，这里是到文档末尾，说明也只能有一个。
.：匹配任意字符
？：匹配0或1个。
+：匹配1个或多个。

re.match(pattern, string, flags=0):
re.search(pattern, string, flags=0)
re.sub(pattern, repl, string, count=0, flags=0):替换字符串。
参考：https://www.cnblogs.com/nkwy2012/p/6548812.html

re.compile(pattern[,flags]):编译一个正则表达式，给其他函数用。
区别： re.match只匹配字符串的开始，如果字符串开始不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。实例：




### 出错记录
错误：HTTP Error 403: Forbidden
解决：添加headers，网站有基本的反爬措施，验证请求的硬件和版本信息。
参考：https://www.cnblogs.com/lixiaolun/p/4773433.html

错误：socket.gaierror: [Errno 11004] getaddrinfo failed
解决：链接不存在或者链接错误了。

爬取糗事百科时的错误。
错误：Remote end closed connection without response
解决：利用 urllib 发起的请求，UA 默认是 Python-urllib/3.5 而在 chrome 中访问 UA 则是 User-Agent:Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36，因为服务器根据 UA 来判断拒绝了 python 爬虫，其实就是改变headers里面的内容。