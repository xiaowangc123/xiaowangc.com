---
title: HTTP协议(部分)
tags:
  - HTTP
cover: img/fengmian/nginx.png
categories:
  - nginx
abbrlink: 5c044483
date: 2024-04-16 17:42:23
---
# HTTP

1989年，就职于欧洲核子研究中心(CERN)的蒂姆·伯纳斯-李发表了一篇论文，提出了互联网上构建超链接文档系统的构想。这篇论文中确立了几项关键技术：

1. URL：统一资源标识符，作为互联网上资源的唯一身份
2. HTML：超文本标记语言，描述超文本文档
3. HTTP：超文本传输协议，用来传输超文本
4. 一个用于提供访问文档的服务器，即http前身

这几个部分完成于1990年底，且第一批服务器已经在1991年初在CERN以外的地方运行了。HTTP在应用的早期阶段非常简单，后来被称为HTTP/0.9，有时候也叫做单行协议。

## 什么是HTTP

HTTP是超文本传输协议：

- 协议：由两个或多个参与者组成并指定一种行为约定和规范。(好比几个不同国家的歪果仁都讲各自语言，大家都无法进行交流了，我们几个制定一份协议，大家下次交流都是使用普通话)
- 传输：HTTP是一个双向协议(两个最基本的参与者，请求方和应答方)，HTTP是一个在计算机世界里专门用来在两点之间传输数据的约定和规范
- 超文本：文本(Text)是完整的、有意义的数据可以被浏览器和服务器这样的上层应用处理。在互联网早期文本就是简单的字符文字，经过发展到现在，所谓超文本就是超越了普通文本的文本，它是文字、图片、音频和视频等的混合体。

HTTP是一个在计算机世界里专门在两点之间传输文字、图片、音频和视频等超文本数据的约定和规范。

## HTTP/0.9

初期的互联网非常简陋，计算机处理能力低，存储容量小，网速慢，还是一片信息荒漠。网络上的绝大数资源都是纯文本，很多通信协议也都是使用纯文本，所以HTTP的设计也不可避免地受到了时代的限制。

最初版本的HTTP协议并没有版本号结构也比较简单，后来它的版本号被定位在0.9以区分后来的版本。最初设想的系统里系统的文档都是只读的，所以只允许用`GET`动作从服务器上获取HTML文档，并且在响应请求之后立马关闭连接，功能十分有限。

## HTTP/1

1993年，NCSA(美国国家超级计算应用中心)开发出了Mosaic，是第一个可以图文混排的的浏览器，随后又在1995年开发出了服务器软件Apache，简化了HTTP服务器的搭建工作。在同一时期，计算机多媒体技术也有了新的发展：1992年发明了JPEG图像格式，1995年发明了MP3音乐格式。这些新软件技术一经推出就吸引了广大网民的热情，更多的人开始使用互联网，研究HTTP并提出改进意见，甚至实验性地往协议里添加各种特性，从用户需求的角度促进了HTTP的发展。

HTTP1.0版本在1996年正式发布。它在许多方面增强了HTTP/0.9版，形成了已经和我们现在的HTTP差别不大了，使其用户更广：

- 协议版本信息现在会随着每个请求发送
- 状态码会在响应开始时发送，使浏览器能了解请求执行成功或失败，并相应调整行为
- 引入HTTP标头的概念，无论是对于请求还是响应，允许传输元数据，使协议变得非常灵活，更具扩展性
- 增加了HEAD、POST等新方法
- 传输的数据不再仅限于文本(凭借Content-Type标头)

## HTTP/1.1

HTTP/1.0多种不同的实现方式在实际中运用显得有些混乱，自HTTP/1.0文档发布的下一年，就开始修订HTTP的第一个标准化版本。比较重要的是它是一个正式的标准，而不是一份可有可无的参考文档。这意味着今后的互联网上所有的浏览器、网关、服务器、代理等，只要用到HTTP协议，就必须严格遵守这个标准。

HTTP/1.1主要的变化有：

1. 增加了PUT、DELETE等新的方法
2. 增加了缓存管理和控制
3. 明确了连接管理，允许持久连接
4. 允许响应数据分块，利于传输大文件
5. 强制要求Host头

一个典型的HTTP请求/响应看上去如下:

```txt
# 请求
GET / HTTP/1.1
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9,en;q=0.8
Cache-Control: no-cache
Connection: keep-alive
Host: www.xiaowangc.com
Pragma: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 16_6 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) Version/16.6 Mobile/15E148 Safari/604.1
```

```txt
# 响应 
HTTP/1.1 200 OK
Server: nginx/1.25.4
Date: Thu, 11 Apr 2024 05:39:41 GMT
Content-Type: text/html
Content-Length: 615
Last-Modified: Wed, 14 Feb 2024 16:03:00 GMT
Connection: keep-alive
ETag: "65cce434-267"
Accept-Ranges: bytes
```

## HTTP/2

HTTP/1.1发布之后，整个互联网世界呈现出了爆发式的增长，这期间也出现了一些对HTTP不满的意见，主要就是连接慢，无法跟上迅猛发展的互联网。

为此Google首先开发了自己的浏览器Chrome，然后推出了新的SPDY协议，并在Chrome里应用于自己的服务器。互联网标准化组织以SPDY为基础开始制定新版本的HTTP协议，在2015年发布了HTTP/2。

HTTP/2的定制充分考虑现今互联网的现状：带宽、移动、不安全，在高度兼容HTTP/1.1的同时在性能改善方面做了很大努力，主要特点有：

1. 使用二进制协议，不再是文本协议
2. 多路复用协议。并行的请求能在同一个连接中处理，移除了HTTP/1.1中顺序和阻塞的约束(管道)
3. 使用专用算法压缩头部信息，减少数据传输量
4. 允许服务器主动向客户端推送数据
5. 增加了安全性(加密通信)

## HTTP/3

HTTP/3基于UDP实现的一个可靠的传输协议QUIC。它在HTTP/3中代替了TCP作为传输层

QUIC在为HTTP连接提供更快的设置和更低的延迟：

- 在TCP中，初始TCP握手之后可以选择进行TLS握手，该握手必须在数据传输之前完成。由于TLS现在几乎无处不在，QUIC将TLS握手集成到初始QUIC握手中，从而减少了设置期间必须交换的信息数量。
- HTTP/2是一种多路复用协议，允许同时进行多个HTTP事务。但是事务通过单个TCP连接进行多路复用，这意味着TCP层的数据包丢失和后续重传可能会阻止所有事务。QUIC通过在UDP上运行并为每个流单独实施数据包检测和重传来避免这种情况，这意味着数据包丢失只会阻塞数据包丢失的特定流。



# HTTP连接管理

连接管理是HTTP的关键话题：打卡和保持连接在很大程度上影响着网站和Web应用程序的性能。在HTTP/1.X中有很多种模型：

- 短链接
- 长连接
- HTTP流

HTTP的传输协议主要依赖于TCP来提供从客户端到服务端之间的连接。在早期，HTTP使用一个简单的模型来处理这样的连接。这些连接的生命周期是短暂的：**每发起一个请求都会创建一个新的连接**，并在收到应答时立即关闭。

这个简单的模型对性能有先天的限制：打开一个TCP连接都是相当耗费资源的操作。客户端和服务端之间需要交换多个消息。当请求发起时，网络延迟和带宽都会对性能造成影响。现代浏览器往往要发起很多次请求才能拿到所需的完整信息，证明了这个早期模型的效率低下。

有两个新的模型在HTTP/1.1诞生了。首先是长连接模型，它会保持连接去完成多次连续的请求，减少了不断打开连接的时间。然后是HTTP流模型，它还要更先进一些，多个连续的请求甚至都不用等待立即返回就可以被发送，这样就减少了耗费在网络延迟上的时间。

## 短链接

HTTP最早期的模型和HTTP/1.0的默认模型，是短链接。每个HTTP请求都由它自己独立的连接完成；这意味着每一个HTTP请求之前都会有一次TCP握手，而且是连续不断的。

TCP连接建立本身就是需要耗费大量时间的，所以TCP可以保持更多的热连接来适应负载。短连接破坏了TCP具备的能力，并且新的冷连接降低了其性能。

这是HTTP/1.0的默认模型。而在HTTP/1.1中，只有当`Connection`被设置为`close`时才会用到这个模型。

## 长连接

短链接有两个比较大的问题：创建新的连接耗费的时间尤为明显，另外TCP连接的性能只有在该连接被使用一段时间后(热连接)才能得到改善。为了缓解这个问题，长连接的概念便设计出来了，甚至在HTTP/1.1之前。或者，这被称为一个Keepalive连接。

一个长连接会保持一段时间，重复用于发送一系列请求，节省了新建TCP连接握手的时间，还可以利用TCP的性能增强能力。当然这个连接也不会一直保留着：在连接空闲一段时间后会被关闭(服务器可以使用keepAlive协议头来指定一个最小的连接保持时间)。

长连接也是有缺点的；就算在空闲状态，它还是会消耗服务器资源，而且在高负载时，还有可能遭受攻击。在这种情况下，可以使用非长连接，即尽快关闭那些空闲的连接，对性能也有所提升。

HTTP/1.0里默认不使用长连接。把`Connection`设置成`close`以外的其他参数都可以让其保持长连接，通常会设置为`retry-after`。

在HTTP/1.1里，默认就是长连接不再需要标头(但是一般情况下会加上，万一某一场景需要使用HTTP/1.0呢)。

## HTTP流

> 不推荐使用包括域名分片，建议升级到HTTP/2

默认情况下，HTTP请求是按顺序发出的。下一个请求只有在当前请求收到响应过后才会被发出。由于会受到网络延迟和带宽的限制，在下一个请求被发送到服务器之前，可能需要等待很长时间。

流是在同一条长连接上发出的连续请求，而不用等待应答返回。这样可以避免连接延迟。理论上讲，性能还会因为两个HTTP请求有可能打包到一个TCP消息包中而得到提升。就算HTTP请求不断的继续，尺寸会增加，但设置TCP的最大分段大小选型，仍然足够包含一系列简单的请求。

并非所有的类型的HTTP请求都能用到流：只有幂等方式比如`GET`、`HEAD`、`PUT`和`DELETE`能够被安全的重试。如有故障发生时，流水线的内容要能被轻易的重试。

# HTTP安全

## CSP

内容安全策略(CSP)是一个额外的安全层，用于检测大量特定类型的工具，包括跨站脚本(XSS)和数据注入攻击等。无论是提取、网站内容污染还是数据盗版恶意软件分发，这些都是主要攻击手段

CSP被设计成完全光纤兼容。不支持CSP的浏览器也能实现与CSP服务器正常工作，反之亦然：不支持CSP的浏览器只是忽略，如常运行，为默认网页内容使用标准的同源策略。如果网站不提供CSP标头，浏览器也使用标准的同源策略

例：

```shell
Content-Security-Policy: default-src 'self'
```

## HSTS

HTTP **Strict-Transport-Security**（简称HSTS）响应标头用于通知浏览器应该只通过HTTPS访问该站点，并且以后使用HTTP访问该站点的所有尝试都应自动重定向到HTTPS。

> 这比简单地配置HTTP到HTTPS(301)重定向要安全，因为网关的HTTP连接仍然容易受到中间人攻击。

例：

```shell
Strict-Transport-Security: max-age=<expire-time>
Strict-Transport-Security: max-age=<expire-time>; includeSubDomains
Strict-Transport-Security: max-age=<expire-time>; includeSubDomains; preload
```

## X-Content-Type-Options

**X-Content-Type-Options** HTTP消息头相当于一个提示标志，被服务器用来提示客户端一定要遵循在Content-Type首部中对MIME类型的设定，而不能对其进行修改。这就禁用了客户端的MIME类型嗅探行为。

```shell
X-Content-Type-Options: nosniff
```

## X-Frame-Options

**X-Frame-Options** HTTP响应头是用来给浏览器指示允许一个页面是否在frame、iframe、embed或者object中展现标记。站点可以通过确保网站没有被嵌入到别人的网站里面，从而避免点击劫持攻击。

```shell
X-Frame-Options: DENY
X-Frame-Options: SAMEORIGIN
```

## X-XSS-Protection

HTTP **X-XSS-Protection**响应头是Intelnet Explorer，Chrome和Safari的一个特性，当检测到跨站脚本攻击时，浏览器将停止加载页面。若网站设置了良好的Content-Security-Policy来禁用内联JS，现代浏览器不太需要这些保护，但其仍然可以为尚不支持CSP的旧版浏览器用户提供保护

# HTTP访问控制

跨源资源共享(CORS)是一种基于HTTP头的机制，该机制通过允许服务器标示除了它自己以外的其他源(域、协议或端口)，使得浏览器允许这些源访问加载自己的资源。跨源资源共享还通过一种机制来检查服务器是否会允许要发送的真实请求，该机制通过浏览器发起一个到服务器托管的跨源资源的预检请求。



