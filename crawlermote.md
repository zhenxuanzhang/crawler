# python crawler



# 目录
- alt+w 查看大纲

---

# 初识爬虫

## 浏览器工作原理

![](crawlermote_files/1.jpg)

	首先，我们在浏览器输入网址（也可以叫URL）。然后，浏览器向服务器传达了我们想访问某个网页的需求，这个过程就叫做【请求】。
	紧接着，服务器把你想要的网站数据发送给浏览器，这个过程叫做【响应】。
	所以浏览器和服务器之间，先请求，后响应，有这么一层关系。

![](crawlermote_files/2.jpg)

	当服务器把数据响应给浏览器之后，浏览器并不会直接把数据丢给你。因为这些数据是用计算机的语言写的，浏览器还要把这些数据翻译成你能看得懂的样子，这是浏览器做的另一项工作【解析数据】。
	紧接着，我们就可以在拿到的数据中，挑选出对我们有用的数据，这是【提取数据】。
	最后，我们把这些有用的数据保存好，这是【存储数据】。
	以上，就是浏览器的工作原理，是人、浏览器、服务器三者之间的交流过程。


## 爬虫的工作原理

![](crawlermote_files/3.jpg)

	当你决定去某个网页后，
	首先，爬虫可以模拟浏览器去向服务器发出请求；
	其次，等服务器响应后，爬虫程序还可以代替浏览器帮我们解析数据；
	接着，爬虫可以根据我们设定的规则批量提取相关数据，而不需要我们去手动提取；
	最后，爬虫可以批量地把数据存储到本地。


![](crawlermote_files/4.jpg)

![](crawlermote_files/5.jpg)

	第0步：获取数据。爬虫程序会根据我们提供的网址，向服务器发起请求，然后返回数据。

	第1步：解析数据。爬虫程序会把服务器返回的数据解析成我们能读懂的格式。

	第2步：提取数据。爬虫程序再从中提取出我们需要的数据。

	第3步：储存数据。爬虫程序把这些有用的数据保存起来，便于你日后的使用和分析。

	这就是爬虫的工作原理啦，无论之后的学习内容怎样变化，其核心都是爬虫原理。


## 爬虫学习大纲

![](crawlermote_files/6.jpg)

## requests

- requests库可以帮我们下载网页源代码、文本、图片，甚至是音频。
- 其实，“下载”本质上是向服务器发送请求并得到响应。

### resquests.get

---
	import requests
	#引入requests库
	res = requests.get('URL')
	#requests.get是在调用requests库中的get()方法，它向服务器发送了一个请求，括号里的参数是你需要的数据所在的网址，然后服务器对请求作出了响应。
	#服务器返回的结果是个Response对象
---

### Response对象的常用属性

![](crawlermote_files/9.jpg)  
- res是一个对象，属于requests.models.Response类。

![](crawlermote_files/7.jpg)

#### response.status_code

![](crawlermote_files/10.jpg)  

![](crawlermote_files/8.jpg)

![](crawlermote_files/11.jpg)


#### response.content

- 它能把Response对象的内容以二进制数据的形式返回，适用于图片、音频、视频的下载

---
	import requests
	res = requests.get('https://res.pandateacher.com/2018-12-18-10-43-07.png')
	#发出请求，并把返回的结果放在变量res中
	pic=res.content
	#把Reponse对象的内容以二进制数据的形式返回
	photo = open('ppt.jpg','wb')
	#新建了一个文件ppt.jpg，这里的文件没加路径，它会被保存在程序运行的当前目录下。
	#图片内容需要以二进制wb读写。你在学习open()函数时接触过它。
	photo.write(pic) 
	#获取pic的二进制内容
	photo.close()
	#关闭文件
---

#### response.text

- 这个属性可以把Response对象的内容以字符串的形式返回，适用于文字、网页源代码的下载。

---
	import requests
	#引用requests库
	res = requests.get('https://localprod.pandateacher.com/python-manuscript/crawler-html/sanguo.md')
	#下载《三国演义》第一回，我们得到一个对象，它被命名为res
	
	res.encoding='utf-8'
	#定义Reponse对象的编码为utf-8。
	novel=res.text
	#把Response对象的内容以字符串的形式返回
	print(novel[:800])
	#打印小说的前800个字。
	
	k = open('《三国演义》.txt','a+')
	#创建一个名为《三国演义》的txt文档，指针放在文件末尾，追加内容
	k.write(novel)
	#写进文件中     
	k.close()
	#关闭文档
---

#### res.encoding
- 它能定义Response对象的编码类型。

---
	在真实的情况中，我们该在什么时候用res.encoding呢？

	首先，目标数据本身是什么编码是未知的。用requests.get()发送请求后，
	我们会取得一个Response对象，其中，requests库会对数据的编码类型做出自己的判断。
	但是！这个判断有可能准确，也可能不准确。

	如果它判断准确的话，我们打印出来的response.text的内容就是正常的、没有乱码的，
	那就用不到res.encoding；如果判断不准确，就会出现一堆乱码，
	那我们就可以去查看目标数据的编码，然后再用res.encoding把编码定义成和目标数据一致的类型即可。


#### requests总结

![](crawlermote_files/12.jpg)


## 爬虫伦理

	Robots协议是互联网爬虫的一项公认的道德规范，它的全称是“网络爬虫排除标准”（Robots exclusion protocol），
	这个协议用来告诉爬虫，哪些页面是可以抓取的，哪些不可以。

- 查看网站的robots协议，在网站的域名后加上/robots.txt。
- [淘宝的robots协议](http://www.taobao.com/robots.txt)
- http://www.taobao.com/robots.txt

---
	User-agent:  Baiduspider #百度爬虫
	Allow:  /article #允许访问 /article.htm
	Allow:  /oshtml #允许访问 /oshtml.htm
	Allow:  /ershou #允许访问 /ershou.htm
	Allow: /$ #允许访问根目录，即淘宝主页
	Disallow:  /product/ #禁止访问/product/
	Disallow:  / #禁止访问除 Allow 规定页面之外的其他所有页面
	​
	User-Agent:  Googlebot #谷歌爬虫
	Allow:  /article
	Allow:  /oshtml
	Allow:  /product #允许访问/product/
	Allow:  /spu
	Allow:  /dianpu
	Allow:  /oversea
	Allow:  /list
	Allow:  /ershou
	Allow: /$
	Disallow:  / #禁止访问除 Allow 规定页面之外的其他所有页面
	​
	…… # 文件太长，省略了对其它爬虫的规定，想看全文的话，点击上面的链接
	​
	User-Agent:  * #其他爬虫
	Disallow:  / #禁止访问所有页面
---

- Allow代表可以被访问，Disallow代表禁止被访问。
- 而且有趣的是，淘宝限制了百度对产品页面的爬虫，却允许谷歌访问。


# HTML基础

- HTML（Hyper Text Markup Language）是用来描述网页的一种语言，也叫超文本标记语言 

## 查看网页的HTML代码

- 在网页任意地方点击鼠标右键，然后点击“显示网页源代码”。
- Windows系统的电脑还可以使用快捷键ctrl+u来查看网页源代码。

这样查看的好处是，整个网页的源代码都完整地呈现在你面前。坏处是，在大部分情况下，它都会经过压缩，导致结构不够清晰，你不太容易懂每行代码的含义。而且，源代码和网页分开在两个页面展示。

- 另一种方法：
- 在网页的空白处点击右键，然后选择“检查”（快捷方式是ctrl+shift+i）。
- 开发者工具栏


## HTML的组成

### 标签和元素

- 夹在尖括号<>中间的字母，它们叫做【标签】。
- 标签通常是成对出现的：前面的是【开始标签】，比如<body>；后面的是【结束标签】，如</body>
- 也有标签是形单影只地出现，比如HTML代码的第四行<meta charset="utf-8">（定义网页编码格式为 utf-8）

![](crawlermote_files/13.jpg)


- 开始标签+结束标签+中间的所有内容，它们在一起就组成了【元素】

![](crawlermote_files/14.jpg)

- 常见元素  

![](crawlermote_files/15.jpg)

### 网页头和网页体

![](crawlermote_files/16.jpg)


	HTML文档的最外层标签一定是<html>，里面嵌套着<head>元素与<body>元素。
	<head>元素代表了【网页头】，<body>元素代表了【网页体】，
	这是最基本的网页结构。
	HTML文档和网页的内容一定是一一对应的。
	只是，【网页头】的内容不会被直接呈现在浏览器里的网页正文中，
	而【网页体】的内容是会直接显示在网页正文中的。

- 网页头


	<head>
		<meta charset="utf-8">  #定义了HTML文档的字符编码
		<title>我是网页的名字</title> #定义网页的标题，这个标题就是显示在浏览器的标签页中的内容
	</head>


![](crawlermote_files/17.jpg)


### HTML属性

- HTML标签可以通过设置【属性】来为HTML元素描述更多的信息。


	<h1 style="color:#20b2aa;">这个书苑不太冷</h1>

- style属性可以用来定义网页文本的样式，比如字体大小、颜色、间距、对齐方式等等。


	在HTML中，链接一般都由<a>标签定义，href属性用于规定指向页面的URL
	<a href="https://wordpress-edu-3autumn.localprod.forc.work/">我是一个链接，点我试试</a>
	
	
- class属性

---

	<style> 
	.book {
	/*以下是.book的具体样式规定*/
	float: left; /*控制元素浮动*/
	margin: 5px; /*外边距为5像素*/
	padding: 15px; /*内边距为15像素*/
	width: 350px; /*宽度为350像素*/
	height: 240px; /*高度为240像素*/
	border: 3px solid #20b2aa; /*边框为3像素*/
	} 
	</style>

	一个是网页头里的.book；一个是网页体里的<div class="book">。

	其实.对应class，所以.book代表class book。因此，网页头中的.book和网页体中的class="book"是有联系的。

	在网页头里面，定义了class属性，属性值为"book"，然后下面一长串代码是对这个class属性的描述；接着再在网页体中调用，所以看到了<div class="book">。

	在HTML中，class属性也可以被多次利用
	网页头的<style>元素中定义了.book的样式，因此，凡是class="book"的元素都会继承它的样式。
	
---
	
- id属性
- 给元素定义id和class的目的都是为了查找、定位元素，或者为元素设置样式
- 但id属性用于标识唯一的元素，而class用于标识一系列的元素。
- id就像是学生的学生证号码，每个人都是唯一的；而学生们可以属于同一个班级，班级就像class

![](crawlermote_files/18.jpg)

![](crawlermote_files/19.jpg)


# 爬虫初体验

## BeautifulSoup

- 使用BeautifulSoup解析和提取网页中的数据

### 解析数据

#### BeautifulSoup()
![](crawlermote_files/20.jpg)

第0个参数是要被解析的文本，注意了，它必须必须必须是字符串。
括号中的第1个参数用来标识解析器，我们要用的是一个Python内置库：html.parser。（它不是唯一的解析器，但是比较简单的）

---
	import requests #调用requests库
	res = requests.get('https://localprod.pandateacher.com/python-manuscript/crawler-html/spider-men5.0.html') 
	#获取网页源代码，得到的res是response对象
	print(res.status_code) #检查请求是否正确响应
	html = res.text #把res的内容以字符串的形式返回
	print(html)#打印html

---

	import requests
	from bs4 import BeautifulSoup
	#引入BS库
	res = requests.get('https://localprod.pandateacher.com/python-manuscript/crawler-html/spider-men5.0.html') 
	html = res.text
	soup = BeautifulSoup(html,'html.parser') #把网页解析为BeautifulSoup对象
	print(type(soup)) #查看soup的类型
	print(soup) # 打印soup

---


	虽然response.text和soup打印出的内容表面上看长得一模一样，却有着不同的内心，
	它们属于不同的类：<class 'str'> 与<class 'bs4.BeautifulSoup'>。
	前者是字符串，后者是已经被解析过的BeautifulSoup对象。
	之所以打印出来的是一样的文本，是因为BeautifulSoup对象在直接打印它的时候会调用该对象内的str方法，所以直接打印 bs 对象显示字符串是str的返回结果


### 提取数据

![](crawlermote_files/21.jpg)

#### find()
- find()与find_all()是BeautifulSoup对象的两个方法

它们可以匹配html的标签和属性，把BeautifulSoup对象里符合要求的数据都提取出来。
它俩的用法基本是一样的，区别在于，find()只提取首个满足要求的数据，而find_all()提取出的是所有满足要求的数据。

![](crawlermote_files/22.jpg)
---

	items = soup.find_all('div') #用find_all()把所有符合要求的数据提取出来，并放在变量items里
	print(type(items)) #打印items的数据类型
	print(items)       #打印items
	
	打印items的类型，显示的是<class 'bs4.element.ResultSet'>，
	是一个ResultSet类的对象。其实是Tag对象以列表结构储存了起来，可以把它当做列表来处理。
---

#### tag
- Tag对象
![](crawlermote_files/23.jpg)

---
	for item in items:
    print('想找的数据都包含在这里了：\n',item) # 打印item
    print(type(item))
	
	它们的数据类型是<class 'bs4.element.Tag'>，是Tag对象

- Tag对象可以使用find()与find_all()来继续检索
	
---	

	items = soup.find_all(class_='books') # 通过定位标签和属性提取我们想要的数据
	for item in items:
		kind = item.find('h2') # 在列表中的每个元素里，匹配标签<h2>提取出数据
		title = item.find(class_='title') #在列表中的每个元素里，匹配属性class_='title'提取出数据
		brief = item.find(class_='info') #在列表中的每个元素里，匹配属性class_='info'提取出数据
		print(kind,'\n',title,'\n',brief) # 打印提取出的数据
		print(type(kind),type(title),type(brief)) # 打印提取出的数据类型

---
- 完整代码

---
	import requests # 调用requests库
	from bs4 import BeautifulSoup # 调用BeautifulSoup库
	res =requests.get('https://localprod.pandateacher.com/python-manuscript/crawler-html/spider-men5.0.html')
	
	html=res.text
	
	soup = BeautifulSoup( html,'html.parser')
	
	items = soup.find_all(class_='books')   # 通过匹配属性class='books'提取出我们想要的元素
	for item in items:                      # 遍历列表items
		kind = item.find('h2')               # 在列表中的每个元素里，匹配标签<h2>提取出数据
		title = item.find(class_='title')     #  在列表中的每个元素里，匹配属性class_='title'提取出数据
		brief = item.find(class_='info')      # 在列表中的每个元素里，匹配属性class_='info'提取出数据
		print(kind.text,'\n',title.text,'\n',title['href'],'\n',brief.text) # 打印书籍的类型、名字、链接和简介的文字

---

## 对象的变化过程

![](crawlermote_files/24.jpg)

我们的操作对象是这样的：Response对象——字符串——BS对象。
到这里，又产生了两条分岔：一条是BS对象——Tag对象；
另一条是BS对象——列表——Tag对象。

![](crawlermote_files/25.jpg)



















