# WebServer学习版

本项目的初衷是写一个能在局域网上运作的HTTP服务器，这样我就可以在电脑上放电影躺床上用手机观看了。  
本项目的原型是github上非常经典的WebServer项目:   
https://github.com/qinguoyi/TinyWebServer  
https://github.com/markparticle/WebServer  
主要采用下面这个更精简的版本  
Reactor+线程池以及自定义的Buffer  


我抄了不少内容，或者说整个框架基本都源于此，但也做了一些删改，至少改了几个能更符合我直觉的函数名。  
因为这东西只运行在自己家里，所以完全没有数据库、日志、登录等繁琐业务逻辑。  
因为没有日志，所以输出全靠cout  

## 主要的改进

为了支持我看电影的需求，我在原WebServer基础上增加了对Range的支持。  
播放媒体如电影/音频时，由于进度条的存在浏览器发出的请求报文会插入Range: bytes 114-514  
表示请求这个文件的某个部分。响应报文需要返回代码206，并给出Content-Range: bytes 114-514/1919810  
并只需要发送这一部分文件即可。  

在提供了这一支持后，页面所播放的视频已经可以随意自由地拖动进度条，且每次拖动都不需要等待一整个文件重新发送了。  

## 性能参数

```
webbench -c 10500 -t 5 http://127.0.0.1:12345/index.html
Webbench - Simple Web Benchmark 1.5
Copyright (c) Radim Kolar 1997-2004, GPL Open Source Software.

Benchmarking: GET http://127.0.0.1:12345/index.html
10500 clients, running 5 sec.

Speed=70824 pages/min, 3882335 bytes/sec.
Requests: 5902 susceed, 0 failed.
```

由于实现比较简陋，性能直接差了一个数量级。

## 注意事项

本项目为学习使用，非常粗糙，内部包含在阅读原项目时留下的大量混乱注释与猜想。
试了试小写下划线命名法感觉不如驼峰命名法一根。

- 性能不好。和原项目比几乎差了一个数量级。根据issue反馈疑是环境问题。
- 不具备响应不完整请求的能力。这一点可以观看原项目的issue。
- keepalive处理模糊。

对于这些问题，我给后来者的建议是去找更成熟的开源库来看。

## 未来更新

由于只有自己在用，就不考虑性能上的优化了。

- 由于没有日志调试起来很不方便，如果日后有时间就加一个。
- 有考虑改成文件服务器的想法，有待日后增加这方面的支持。


