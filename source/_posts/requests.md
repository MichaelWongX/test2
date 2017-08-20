---
layout: python
title: requests
date: 2017-08-20 11:30:21
tags: python
---
# requests 摘要 #
requests 支持多种HTTP请求类型，如get,put,delete,head,options,post,
## requests construct url with paras ##

		payload = {'key1': 'value1', 'key2': 'value2'}
		r = requests.get("http://httpbin.org/get", params=payload}

		print(r.url)
		>>>http://httpbin.org/get?key1=value1&key2=value2

## requests content ##
Requests会根据内置的算法，猜测返回的内容编码方式，自动解码为unicode内容，但是有时会解码错位，response.encoding 查看其推测的文本编码，也可以response.encoding = 'utf8' 改变其编码方式，再次访问response.text返回重新解码后的文本

		r.encoding
		>>>'utf8'
		r.encoding = 'gb2312'
		r.content  # the response bytes
		r.text		#unicoded response
		r.json()	#parse the content using json parser

在某些情况下，可能要读取原始响应内容，可以通过在原始请求中设置**stream=True**

		r = requests.get('http://example.com', stream=True)
		r.raw
		r.raw.read(10)	# read 10 bytes of the resonpse content.

		# write the raw content into the file
		with open(filename, 'wb') as fd:
			for chunk in r.iter_content(chunk_size):
				fd.write(chunk)

## requests post ##


* 通常需要发送一些编码为表单形式的数据，在requests post 请求中可以通过**data=paras** 传递dict类型的参数，
* 在特殊请求中，可能需要传递string类型的参数，**data=json.dumps(string)**进行传递
* 也可以通过**json参数**，由系统自动转换为json类型传递
**直接通过data和json传递的同一个dict数据，传送到服务器时是有差异的**

## requests post file ##

		files= {'file': open('ex.txt', 'rb')}
		requests.post(url, files=files)

		#set the filename, filetype,file request
		files = {'file': ('ex.txt', open('ex.txt', 'rb'),'application/txt', {'Expires': '0'})}
		requests.post(url, files=files)

## request session ##
会话对象让你能够跨请求保持某些参数。它也会在同一个 Session 实例发出的所有请求之间保持 cookie， 期间使用 urllib3 的 connection pooling 功能。所以如果你向同一主机发送多个请求，底层的 TCP 连接将会被重用，从而带来显著的性能提升.
		s = requests.Session()
		s.get('http://httpbin.org/cookies/set/sessioncookie/123456789')
		r = s.get("http://httpbin.org/cookies")
		print(r.text)
		>>> '{"cookies": {"sessioncookie": "123456789"}}

		s = requests.Session()
		s.auth = ('user', 'pass')
		s.headers.update({'x-test': 'true'})
		# both 'x-test' and 'x-test2' are sent
		s.get('http://httpbin.org/headers', headers={'x-test2': 'true'})

## requests timeout ##
为防止服务器不能及时响应，大部分发至外部服务器的请求都应该带着 timeout 参数。如果没有 timeout，你的代码可能会挂起若干分钟甚至更长时间。
连接超时指的是在你的客户端实现到远端机器端口的连接时（对应的是`connect()`_），Request 会等待的秒数。
读取超时指的就是客户端等待服务器发送请求的时间。（特定地，它指的是客户端要等待服务器发送字节之间的时间。在 99.9% 的情况下这指的是服务器发送第一个字节之前的时间）

		r = requests.get('https://github.com', timeout=5) # timeout second  for both connection and read
		r = requests.get('https://github.com', timeout=(3.05, 27)) # timeout for connection and read seperately
		r = requests.get('https://github.com', timeout=None) # never timeout 


## 参考 ##
[requests 官方文档](http://docs.python-requests.org/en/master/)
