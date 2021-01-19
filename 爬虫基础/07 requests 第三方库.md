# requests 第三方库

> **官方定义**：Requests is the only Non-GMO HTTP library for Python, safe for human consumption.
>  **简单翻译一下就是**：Requests 是唯一适用于 Python 的 Non-GMO HTTP 库，可供开发人员安全使用。

上面是 requests 库的官方定义。简单来说 requests 库是 Python 的第三方 HTTP 库，以 urllib 为基础。因为使用简单，人性化，安全的特性，被广泛的用来爬虫的请求发送。



## 1. requests 安装

可以通过 PIP 进行安装：

> **Tips**：Python 环境安装之后 PIP 也会自动安装，直接打开 CMD 命令行使用即可。

```bash
pip install requests
```

也可以到 官网下载，然后通过以下命令进行安装。然后进入到下载的目录，输入以下命令：

```
python setup.py install
```



## 2. requests 请求

下面我们使用慕课网作为目标网站，并使用 requests 库进行请求：



### 2.1 get 无参数请求

我们直接使用 request 的 get 方法来请求慕课网，然后打印返回结果：

```python
import requests
r = requests.get('https://www.imooc.com/')
print(r.text)
```

请求结果如下，格式为HTML：

```hmtl
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>慕课网-程序员的梦工厂</title>
<meta http-equiv="X-UA-Compatible" content="IE=edge, chrome=1">
<meta name="renderer" content="webkit" />
<meta name="mobile-agent" content="format=wml"; url="https://m.imooc.com/">
<link rel="alternate" media="only screen and (max-width: 640px)" href="https://m.imooc.com/">
<meta name="mobile-agent" content="format=xhtml"; url="https://m.imooc.com/">
<meta name="mobile-agent" content="format=html5"; url="https://m.imooc.com/">
<meta property="qc:admins" content="77103107776157736375" />
......
```



### 2.2 get 有参数请求

下面我们给 get 请求加上参数，来看看返回结果：

```python
import requests

r = requests.get('https://www.imooc.com/common/adver-getadverlistbymarking?marking=global_newcomer')
print(r.text)

```

请求结果如下，格式为 Json：

```json
{"result":0,"data":{"global_newcomer":{"id":"2696","name":"\u65b0\u4eba\u6709\u793c","column_id":"366","description":"","pic":"\/\/img3.mukewang.com\/5df2084b096514ff25600136.png","links":"https:\/\/www.imooc.com\/act\/newcomer","type":"99","type_id":"0","create_time":"1576142925","uid":"10001","is_open":"0","seqid":"0","status":"0","start_time":"0","end_time":"0","skillid_list":""}},"msg":"\u6210\u529f"}
```



### 2.3 无参数的post请求

```python
import requests

r = requests.post('https://www.imooc.com/search/hotwords‘)
print(r.text)
```

请求结果如下，格式为Json：

```json
{"result":0,"data":["Vue","Python","Java","flutter","springboot","docker","React","\u5c0f\u7a0b\u5e8f"],"msg":"\u6210\u529f"}
```



### 2. 4 有参数的post请求

```python
import requests

r = requests.post('https://httpbin.org/post', data = {'key':'value'})
print(r.text)
```

请求结果如下，格式为Json：

```json
{
  "args": {}, 
  "data": "", 
  "files": {}, 
  "form": {
    "key": "value"
  }, 
  "headers": {
    "Accept": "*/*", 
    "Accept-Encoding": "gzip, deflate", 
    "Content-Length": "9", 
    "Content-Type": "application/x-www-form-urlencoded", 
    "Host": "httpbin.org", 
    "User-Agent": "python-requests/2.22.0", 
    "X-Amzn-Trace-Id": "Root=1-5e3bde61-bb0c787463f0852c81b7faa8"
  }, 
  "json": null, 
  "origin": "124.78.170.82", 
  "url": "https://httpbin.org/post"
}
```

除了 get，post 等基本请求，Request也支持其他的请求类型。以下罗列的是官方提供的其他请求方法：

```python
r = requests.put('https://httpbin.org/put', data = {'key':'value'})
r = requests.delete('https://httpbin.org/delete')
r = requests.head('https://httpbin.org/get')
r = requests.options('https://httpbin.org/get')
```



## 3. requests 的响应



### 3.1 获取二进制响应内容

```python
import requests
r = requests.post('https://www.imooc.com/')
print(r.content)
```

返回的二进制文本如下所示：

```python
b'\n\r\n<!DOCTYPE html>\r\n<html>\r\n<head>\r\n<meta charset="utf-8">\r\n<title>\xe6\x85\x95\xe8\xaf\xbe\xe7\xbd\x91-\xe7\xa8\x8b\xe5\xba\x8f\xe5\x91\x98\xe7\x9a\x84\xe6\xa2\xa6\xe5\xb7\xa5\xe5\x8e\x82</'
......
```

某些情况下，我们需要获取二进制的内容，比如图片或者一些视频的信息流。



### 3.2 获取响应状态码和响应编码

```python
import requests
r = requests.post('https://www.imooc.com/')
print(r.status_code)
print(r.encoding)
```

请求成功后将会得到以下的状态码。另外，request 库同时也提供了 `requests.codes.ok` 来表示请求成功。

通过响应码，我们可以知道我们请求的是否发送成功，是否被正确的解析，以及是否正确的返回。通过检验程序的编码，来防止编码不一致导致的乱码问题。



## 4. 自定义请求头

这里，我们把程序的请求头封装在了字典里，然后通过字典的形式传给 requests 进行请求，这样做，有助于我们代码的整洁性和可维护性。

```python
url = 'https://www.imooc.com/'
headers = {'user-agent': 'app/1.0'}
r = requests.get(url, headers=headers)
print(r.request.headers) # 响应状态码
```

上述代码，我们仍然可以请求成功，只是更改了 `user-agent` 字段而已，返回的结果如下所示：

```
{'user-agent': 'muke_app/1.0', 'Accept-Encoding': 'gzip, deflate', 'Accept': '*/*', 'Connection': 'keep-alive'}
```



## 5. 高级内容

上面的内容我们只是罗列了一些 requests 库经常使用的功能，但是已经足够我们进行爬虫开发了。当然 requests 库还有许多的高级功能比如设置请求时间，会话，代理，以及身份认证等。有兴趣的同学可以参考去 [Requests官网](https://requests.readthedocs.io/en/master/user/quickstart/#cookies) 学习。这里我们就不一一赘述了。



## 6. 小结

这一个小节，我们讲解了 Requests 第三方库，介绍了安装，以及请求和响应。Reqeusts 库的设计非常的简单方便，绝大多数的爬虫程序都是使用Requests库进行网页的爬取。