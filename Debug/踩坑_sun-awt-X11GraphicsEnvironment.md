---
title: sun-awt-X11GraphicsEnvironment
date: 2022-12-28 12:37:33
tags:
categories: Debug
cover:
---

1. 事情起因：
项目中有一段代码是调用awt包下的createGE()方法生成图片的，在windows环境下调用正常，但是linux环境下运行的项目调用却报错，前端请求的时候控制台一直提示请求路径404,所以刚开始给我找问题很大的误导，之后我在去查看tomcat下的日志的时候，从下面的报错信息找出了问题所在。

2. 报错信息：
```bash 
Exception in thread "http-bio-8080-exec-5" java.lang.InternalError: Can't connect to X11 window server using 'localhost:10.0' as the value of the DISPLAY variable.
	at sun.awt.X11GraphicsEnvironment.initDisplay(Native Method)
	at sun.awt.X11GraphicsEnvironment.access$200(X11GraphicsEnvironment.java:65)
	at sun.awt.X11GraphicsEnvironment$1.run(X11GraphicsEnvironment.java:110)
	at java.security.AccessController.doPrivileged(Native Method)
	at sun.awt.X11GraphicsEnvironment.<clinit>(X11GraphicsEnvironment.java:74)
	at java.lang.Class.forName0(Native Method)
	at java.lang.Class.forName(Class.java:191)
	at java.awt.GraphicsEnvironment.createGE(GraphicsEnvironment.java:102)
	at java.awt.GraphicsEnvironment.getLocalGraphicsEnvironment(GraphicsEnvironment.java:81)
	at java.awt.image.BufferedImage.createGraphics(BufferedImage.java:1182)
	at java.awt.image.BufferedImage.getGraphics(BufferedImage.java:1172)
	at cn.saladday.travel.util.CaptchaUtils.createImage(CaptchaUtils.java:149)
	at cn.saladday.travel.util.CaptchaUtils.createImage(CaptchaUtils.java:137)
	at cn.saladday.travel.web.servlet.CheckCodeServlet.doGet(CheckCodeServlet.java:34)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:620)
	at javax.servlet.http.HttpServlet.service(HttpServlet.java:727)
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:303)
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:208)
	at org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:52)
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:241)
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:208)
	at cn.saladday.travel.web.filter.CharacterFilter.doFilter(CharacterFilter.java:28)
	at org.apache.catalina.core.ApplicationFilterChain.internalDoFilter(ApplicationFilterChain.java:241)
	at org.apache.catalina.core.ApplicationFilterChain.doFilter(ApplicationFilterChain.java:208)
	at org.apache.catalina.core.StandardWrapperValve.invoke(StandardWrapperValve.java:220)
	at org.apache.catalina.core.StandardContextValve.invoke(StandardContextValve.java:122)
	at org.apache.catalina.authenticator.AuthenticatorBase.invoke(AuthenticatorBase.java:503)
	at org.apache.catalina.core.StandardHostValve.invoke(StandardHostValve.java:170)
	at org.apache.catalina.valves.ErrorReportValve.invoke(ErrorReportValve.java:103)
	at org.apache.catalina.valves.AccessLogValve.invoke(AccessLogValve.java:950)
	at org.apache.catalina.core.StandardEngineValve.invoke(StandardEngineValve.java:116)
	at org.apache.catalina.connector.CoyoteAdapter.service(CoyoteAdapter.java:421)
	at org.apache.coyote.http11.AbstractHttp11Processor.process(AbstractHttp11Processor.java:1070)
	at org.apache.coyote.AbstractProtocol$AbstractConnectionHandler.process(AbstractProtocol.java:611)
	at org.apache.tomcat.util.net.JIoEndpoint$SocketProcessor.run(JIoEndpoint.java:316)
	at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)
	at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)
	at org.apache.tomcat.util.threads.TaskThread$WrappingRunnable.run(TaskThread.java:61)
	at java.lang.Thread.run(Thread.java:745)


```

3. 事情缘故：

图片采用awt图形处理并内存中生成，java程序会去寻找linux上的图形界面是否启动 ，如linux缺少组件则, 缺少了显示设备、键盘或鼠标会报这个错误;
所以需要开启无头模式, Headless模式是系统的一种配置模式 , 开始激活headless模式，告诉程序，现在你要工作在Headless mode下，就不要指望硬件帮忙了，你得自力更生，依靠系统的计算能力模拟出这些特性来继续。

4. 解决办法：

   在tomcat/bin/catalina.sh里加入下面这段代码，就可以正常运行了 ：

   ```bash
   JAVA_OPTS="$JAVA_OPTS -Djava.awt.headless=true"
   ```

   
