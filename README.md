## 说明

本代码使用 Node.js 演示了以下两种 WEB 代理的工作原理：

第一种是 [RFC 7230 - HTTP/1.1: Message Syntax and Routing](http://tools.ietf.org/html/rfc7230)（即修订后的 RFC 2616，HTTP/1.1 协议的第一部分）描述的普通代理。这种代理扮演的是「中间人」角色，对于连接到它的客户端来说，它是服务端；对于要连接的服务端来说，它是客户端。它就负责在两端之间来回传送 HTTP 报文。

第二种是 [Tunneling TCP based protocols through Web proxy servers](https://tools.ietf.org/html/draft-luotonen-web-proxy-tunneling-01)（通过 Web 代理服务器用隧道方式传输基于 TCP 的协议）描述的隧道代理。它通过 HTTP 协议正文部分（Body）完成通讯，以 HTTP 的方式实现任意基于 TCP 的应用层协议代理。这种代理使用 HTTP 的 CONNECT 方法建立连接，但 CONNECT 最开始并不是 RFC 2616 - HTTP/1.1 的一部分，直到 2014 年发布的 HTTP/1.1 修订版中，才增加了对 CONNECT 及隧道代理的描述，详见 [RFC 7231 - HTTP/1.1: Semantics and Content](https://tools.ietf.org/html/rfc7231#section-4.3.6)。

## 测试说明

首先在服务器上 git clone 代码，并在代码根目录生成证书公私钥：

```
openssl genrsa -out private.pem 2048
openssl req -new -x509 -key private.pem -out public.crt -days 99999
```

第二行命令运行后，需要填写一些证书信息。需要注意的是 **Common Name 一定要填写后续提供 HTTPS 服务的域名或 IP**，其它信息随意填写。然后把 `public.crt` 下载到本地，导入到系统受信任根证书列表中。

最后运行 proxy.js 即可：

```bash
node proxy.js

#在后台运行
nohup node proxy.js &
``` 

给 Chrome 配置代理，填写以下三项：

* Protocol：填 HTTPS；
* Server：填服务器 IP 或域名（必须等于生成证书时填写的 Common Name）；
* Port：填服务器端口（默认是 8888，可以直接在 proxy.js 中修改）；

推荐使用 [Proxy SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif) 扩展管理 Chrome 代理。

## 更多介绍

* [HTTP 代理原理及实现（一）](https://imququ.com/post/web-proxy.html)
* [HTTP 代理原理及实现（二）](https://imququ.com/post/web-proxy-2.html)

