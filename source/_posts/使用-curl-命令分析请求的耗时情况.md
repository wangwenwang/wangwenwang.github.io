---
title: 使用 curl 命令分析请求的耗时情况
date: 2017-04-12 15:47:32
tags: Curl
---

# 使用-curl-命令分析请求的耗时情况

---

自己搭了个blog，却像蜗牛一样，怎样一步一步找到问题的原因。在网络上搜索了一下，发现了一个非常好用的方法，curl 命令就能帮你分析请求的各个部分耗时。

先往文本文件 curl-format.txt 写入下面的内容：

```
$ cat curl-format.txt

```


```
time_namelookup:  %{time_namelookup}\n
time_connect:  %{time_connect}\n
time_appconnect:  %{time_appconnect}\n
time_redirect:  %{time_redirect}\n
time_pretransfer:  %{time_pretransfer}\n
time_starttransfer:  %{time_starttransfer}\n
----------\n
time_total:  %{time_total}\n

```


•    time_namelookup：DNS 域名解析的时候，就是把 https://zhihu.com 转换成 ip 地址的过程
•	time_connect：TCP 连接建立的时间，就是三次握手的时间
•	time_appconnect：SSL/SSH 等上层协议建立连接的时间，比如 connect/handshake 的时间
•	time_redirect：从开始到最后一个请求事务的时间
•	time_pretransfer：从请求开始到响应开始传输的时间
•	time_starttransfer：从请求开始到第一个字节将要传输的时间
•	time_total：这次请求花费的全部时间


我们先看看一个简单的请求，没有重定向，也没有 SSL 协议的时间：

```
$ curl -w "@curl-format.txt" -o /dev/null -s -L "http://163xw.com"

```

•	-w：从文件中读取要打印信息的格式
•	-o /dev/null：把响应的内容丢弃，因为我们这里并不关心它，只关心请求的耗时情况
•	-s：不要打印进度条

从这个输出，我们可以算出各个步骤的时间：
•	DNS 查询：12ms
•	TCP 连接时间：pretransfter(227) - namelookup(12) = 215ms
•	服务器处理时间：starttransfter(443) - pretransfer(227) = 216ms
•	内容传输时间：total(867) - starttransfer(443) = 424ms


来个比较复杂的，访问某度首页，带有中间有重定向和 SSL 协议：

```
$ curl -w "@curl-format.txt" -o /dev/null -s -L "https://baidu.com"
end

```


```
time_namelookup:  0.012
time_connect:  0.018
time_appconnect:  0.328
time_redirect:  0.356
time_pretransfer:  0.018
time_starttransfer:  0.027
----------
time_total:  0.384

```

可以看到 time_appconnect 和 time_redirect 都不是 0 了，其中 SSL 协议处理时间为 328-18=310ms。而且 pretransfer 和 starttransfer 的时间都缩短了，这是重定向之后请求的时间。



#### 参考资料：
• [Cizixs Writes Here](http://cizixs.com/2017/04/11/use-curl-to-analyze-request?utm_source=tuicool&utm_medium=referral) 

