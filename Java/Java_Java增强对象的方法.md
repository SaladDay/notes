---
title: Java增强对象的方法
date: 2021-6-19 16:27:30
tags:
categories: Java
cover:
---

- 可以采用一些设计模式，用于将对象方法进行增强，例如改变参数，函数体，返回值;相当于来说就是重写了方法的对象。【需要拥有同样的接口】
  - 设计模式：一些通用的解决固定问题的方式

### 增强对象的方法的设计模式

1. 装饰模式

2. 代理模式

   - 概念：

     1. 真实对象
     2. 代理对象
     3. 代理模式

   - 实现方式

     1. 静态代理：有一个类文件描述代理模式

     2. 动态代理：自动加载代理类，在内存中形成。使用较多，有Proxy类来帮助实现；

        - 增强方式

          1. 增强参数列表
          2. 增强返回值类型
          3. 增强函数体内容（但只能增强其上下文内容）

        - 实现方式:

          ```java
          //例如我们需要写一个filter来增加HttpRequest对象的getParameter()方法
          //使其能够在参数的值为敏感词的时候自动替换成 ***
          
              @Override
              public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
                  
                  //这个是动态代理增强对象，其他方法都一致，getParameter被增强了
                  ServletRequest proxyReq =  (ServletRequest) Proxy.newProxyInstance(servletRequest.getClass().getClassLoader(),
                          servletRequest.getClass().getInterfaces(), new InvocationHandler() {
                              @Override
                              public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
          
                                  if("getParameter".equals(method.getName())){
                                      //此处调用的是主类的getParameterMap方法
                                      String value = (String) method.invoke(servletRequest,args);
                                      if(value!=null){//如果有此类输入参数
                                          for (String s : list) {
                                              if(value.contains(s)){
                                                  value = value.replaceAll(s, "***");
                                              }
                                          }
                                      }
                                      return value;
                                  }else{
                                      return method.invoke(servletRequest,args);
                                  }
          
                              }
                          });
                  //此时对servletRequest对象的getParameter增强已实现，
                  //应该用增强了getParameter的代理对象代替真实对象放行
                  filterChain.doFilter(proxyReq,servletResponse);
          
          
          
              }
          ```
          
          