# HTTP协议

## 1. 什么是HTTP协议

双方或者多方约定好的，大家都需要遵守的规则

HTTP协议是指客户端和服务器之间通信时双方所需要遵守的规则，HTTP协议中的数据叫报文

## 2. 协议格式

请求 & 响应

### 2.1 请求

#### 2.1.1 GET请求

1. 请求行

	- 请求方式	GET
	- 请求的资源路径[+?+请求参数]
	- 请求的协议版本号 HTTP/1.1

2. 请求头

	key: value 组成

```
//请求行：请求方式 请求的资源路径 协议版本号
GET /test/pages/test.html HTTP/1.1	

//请求头
Host: localhost:8080 //主机地址:端口号
Connection: keep-alive //连接方式 keep-alive closed
Cache-Control: max-age=0
sec-ch-ua: "Chromium";v="86", "\"Not\\A;Brand";v="99", "Google Chrome";v="86"
sec-ch-ua-mobile: ?0
DNT: 1 // do not track
Upgrade-Insecure-Requests: 1

//浏览器信息
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.111 Safari/537.36

//客户端可接收的数据类型
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document

//客户端可接受的数据编码格式
Accept-Encoding: gzip, deflate, br

//客户端可接受的语言类型
Accept-Language: zh-CN,zh-TW;q=0.9,zh;q=0.8

//Cookie信息
Cookie: JSESSIONID=BF485FFE7B25DF3475B804399E071DA1; _ga=GA1.1.1180231696.1571115630; username-localhost-8888="2|1:0|10:1603274690|23:username-localhost-8888|44:OGQyZTU1ZWJiYjA0NGY4YWI2MzdlNGU3ZWM4ODYwOGE=|fabaa429e2394adc6cc6915d80c11675055b8d8f819af4d7358438569bf5d76c"; _xsrf=2|e3704f48|0ffe6f5997f887206e08508117367c93|1603274690; username-localhost-8889="2|1:0|10:1603278138|23:username-localhost-8889|44:NmRlMTIzYmE5M2M0NDJlMjk0YjhiZmQwYWMzYzMxZGM=|aab47380fd809eb890b30d2677a35c7c137154797f0dd5faddfae94c14082309"; username-localhost-8890="2|1:0|10:1603278948|23:username-localhost-8890|44:MzYyMWEzNjMyMTg4NDhmZDgyNGVmMTIyYjhhMzRlMWQ=|caaf203396042b1dedef071237b9008fe52b1c6418ee69bb0d3bea919b7e4b25"; username-localhost-8891="2|1:0|10:1603281811|23:username-localhost-8891|44:MmEwZWE3ZjE4N2E5NGYyMTk4ODA5ZjkzNDZlMjEyYzc=|85a3f8689010c7a3bb136b631c17b7c74256754de158191eeef4dbf16d368320"
If-None-Match: W/"986-1603681910621"
If-Modified-Since: Mon, 26 Oct 2020 03:11:50 GMT
```





#### 2.1.2 POST请求

1. 请求行

	- 请求方式	POST
	- 请求的资源路径[+?+请求参数]
	- 请求的协议版本号 HTTP/1.1

2. 请求头

	key: value 组成 

	空行

3. 请求体（发送给服务器的数据）

```
//请求行：请求方式 资源路径 协议版本号
POST /test/context HTTP/1.1

//请求头
Host: localhost:8080
Connection: keep-alive
Content-Length: 13 //发送的数据长度
Cache-Control: max-age=0 //如何控制缓存
sec-ch-ua: "Chromium";v="86", "\"Not\\A;Brand";v="99", "Google Chrome";v="86"
sec-ch-ua-mobile: ?0
Origin: http://localhost:8080
Upgrade-Insecure-Requests: 1
DNT: 1
//表示发送的数据的类型
Content-Type: application/x-www-form-urlencoded //表示提交的数据格式是：name=value&name=value然后对其进行url编码
			// multipart/form-data 表示以多段的形式提交数据给服务器（以流的形式提交，用于上传）
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.111 Safari/537.36
//客户端可以接收的数据格式
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://localhost:8080/test/pages/test.html
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-CN,zh-TW;q=0.9,zh;q=0.8
Cookie: JSESSIONID=BF485FFE7B25DF3475B804399E071DA1; _ga=GA1.1.1180231696.1571115630; username-localhost-8888="2|1:0|10:1603274690|23:username-localhost-8888|44:OGQyZTU1ZWJiYjA0NGY4YWI2MzdlNGU3ZWM4ODYwOGE=|fabaa429e2394adc6cc6915d80c11675055b8d8f819af4d7358438569bf5d76c"; _xsrf=2|e3704f48|0ffe6f5997f887206e08508117367c93|1603274690; username-localhost-8889="2|1:0|10:1603278138|23:username-localhost-8889|44:NmRlMTIzYmE5M2M0NDJlMjk0YjhiZmQwYWMzYzMxZGM=|aab47380fd809eb890b30d2677a35c7c137154797f0dd5faddfae94c14082309"; username-localhost-8890="2|1:0|10:1603278948|23:username-localhost-8890|44:MzYyMWEzNjMyMTg4NDhmZDgyNGVmMTIyYjhhMzRlMWQ=|caaf203396042b1dedef071237b9008fe52b1c6418ee69bb0d3bea919b7e4b25"; username-localhost-8891="2|1:0|10:1603281811|23:username-localhost-8891|44:MmEwZWE3ZjE4N2E5NGYyMTk4ODA5ZjkzNDZlMjEyYzc=|85a3f8689010c7a3bb136b631c17b7c74256754de158191eeef4dbf16d368320"
//空行

//请求体
username=root
```

### 2.2响应

#### 2.1.1响应格式

- ​	响应行

	​		响应的协议和版本号

	​		响应状态码

	​		响应状态描述符

- ​	响应头

	​		key: value

- ​	响应体

	​		回传给客户端的数据

```html
HTTP/1.1 200 // 响应行：响应的协议 状态码 状态描述符（这里没有）
//响应头
Date: Mon, 26 Oct 2020 06:57:03 GMT // 请求响应的时间（格林时间）
Accept-Ranges: bytes
ETag: W/"1235-1603693851472"
Last-Modified: Mon, 26 Oct 2020 06:30:51 GMT 
Content-Type: text/html //响应的数据类型
Content-Length: 1235 //响应体长度
//空行

//响应体
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Test</title>

    <form action="../hello">
        <input type="submit" formmethod="get" value="hello get">
    </form>
    <br>
    <form action="../hello">
        <input type="submit" formmethod="post" value="hello post">
    </form>
    <br><br>

    <form action="../hello2">
        <input type="submit" formmethod="get" value="hello2 get">
    </form>
    <br>
    <form action="../hello2">
        <input type="submit" formmethod="post" value="hello2 post">
    </form>
    <br><br>

    <form action="../hello3">
        <input type="submit" formmethod="get" value="hello3 get">
    </form>
    <br>
    <form action="../hello3">
        <input type="submit" formmethod="post" value="hello3 post">
    </form>
    <br>
    <form action="../context">
        <input type="submit" formmethod="get" value="context get">
    </form>
    <br>
    <form action="../context">
        <input type="submit" formmethod="post" value="HTTP Post test">
        <input type="hidden" name="username" value="root">
        <input type="hidden" name="password" value="123456">
    </form>
</head>
<body>

</body>
</html>
```

#### 2.1.1 常见响应码

| 响应码 | 含义                   |
| ------ | ---------------------- |
| 200    | 请求成功               |
| 302    | 请求重定向             |
| 404    | 请求错误（请求地址错） |
| 500    | 服务器内部错误         |

## 3. MIME类型说明

MIME（Multipurpose Internet Mail Extensions）是HTTP协议中的数据类型。类型格式是 “大类型/小类型”，并与某一种文件的扩展名相对应。

| 文件               | MIME类型                               |
| ------------------ | -------------------------------------- |
| 超文本标记语言文本 | .html .html    text/html               |
| 普通文本           | .txt    text/plain                     |
| RTF 文本           | .rtf    application/rtf                |
| GIF 图形           | .gif    image/gif                      |
| JPEG 图形          | .jpeg .jpg     image/jpeg              |
| au 声音文件        | .au    audio/basic                     |
| MIDI 音乐文件      | .mid .midi    audio/midi, audio/x-midi |
| RealAudio 音乐文件 | .ra .ram    audio/x-pn-realaudio       |
| MPEG 文件          | .mpg .mpeg     video/mpeg              |
| AVI 文件           | .avi video    video/x-msvideo          |
| GZIP 文件          | .gz    application/x-gzip              |
| TAR 文件           | .tar   application/x-tar               |

