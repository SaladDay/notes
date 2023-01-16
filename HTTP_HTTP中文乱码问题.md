---
title: response对象获取的io流中文乱码问题
date: 2020-12-13 15:16:36
tags:
categories: HTTP
cover:
---

- Response对象获取的字符输出流默认使用的是ISO编码，因此我们必须在获取流对象之前设置编码格式为UTF-8：response.setCharacterEncoding("utf-8")
- 但是这样只不过是保证了通过res获取的io流以utf-8编码，解码还是不知道怎么解码，在不同的浏览器上可能解码的方式便不同，因此我们要用响应头的方式告知浏览器该怎么解码，response.setHeader("content-type","text/html;charset=utf-8")
- 同时还有简化的版本：response.setContentType("text/html;charset=utf-8")，同时也需要在获取流之前设置。
