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


	import requests
	#引入requests库
	res = requests.get('URL')
	#requests.get是在调用requests库中的get()方法，它向服务器发送了一个请求，括号里的参数是你需要的数据所在的网址，然后服务器对请求作出了响应。
	#服务器返回的结果是个Response对象

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

#### response.text

- 这个属性可以把Response对象的内容以字符串的形式返回，适用于文字、网页源代码的下载。


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

#### res.encoding
- 它能定义Response对象的编码类型。


	在真实的情况中，我们该在什么时候用res.encoding呢？

	首先，目标数据本身是什么编码是未知的。用requests.get()发送请求后，我们会取得一个Response对象，其中，requests库会对数据的编码类型做出自己的判断。但是！这个判断有可能准确，也可能不准确。

	如果它判断准确的话，我们打印出来的response.text的内容就是正常的、没有乱码的，那就用不到res.encoding；如果判断不准确，就会出现一堆乱码，那我们就可以去查看目标数据的编码，然后再用res.encoding把编码定义成和目标数据一致的类型即可。

#### requests总结

![](crawlermote_files/12.jpg)


# 爬虫伦理

	Robots协议是互联网爬虫的一项公认的道德规范，它的全称是“网络爬虫排除标准”（Robots exclusion protocol），
	这个协议用来告诉爬虫，哪些页面是可以抓取的，哪些不可以。

- 查看网站的robots协议，在网站的域名后加上/robots.txt。
- [淘宝的robots协议](http://www.taobao.com/robots.txt)
- http://www.taobao.com/robots.txt


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


- Allow代表可以被访问，Disallow代表禁止被访问。
- 而且有趣的是，淘宝限制了百度对产品页面的爬虫，却允许谷歌访问。
















