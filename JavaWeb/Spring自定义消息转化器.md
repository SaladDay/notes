---
title: Spring自定义消息转化器
date: 2023-01-13 12:57:22
tags:
categories: SpringMVC
cover:
---

- 首先让我们来了解一下spring的消息转换器

- 我们都使用过@RequestBody和@ResponseBody这两个注解，他们的作用就是前台向后台转递数据的时候，把请求报文中的数据通过springmvc的处理转化成我们需要的JAVA对象。或者是后台向前台发送数据的时候将java对象转化成前台所需要的数据类型(例如JSON\\XML)

  - 当用@RequestBody标注一个对象的时候，在请求过程中进行数据映射的时候，springmvc会根据Request报文中的header部分的content-Type类型，逐一匹配合适的HttpMessageConverter来读取数据。默认情况下，有以下七种HttpMessageConverter

    <img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20230113130217623.png" alt="image-20230113130217623" style="zoom: 50%;" />

  - 在响应的时候，springmvc会根据Request报文中header部分的accept属性，逐一去遍历找到能处理的HttpMessageConverter去处理

- 因此如果我们需要自定义映射，我们就需要去接管请求响应报文和对象映射的这个过程。

- 在实际过程中，用到的最多的HttpMessageConverter是MappingJackson2HttpMessageConverter，也就是我们所熟悉的Jackson。所以我们只需要去继承，再将我们重写的HttpMessConverter加入springmvc的消息转化器集合即可。

### 代码：

本人所需要接管的主要是后台向前台发送的：

1.Long

2.BigInteger

3.日期

```java

import com.fasterxml.jackson.databind.DeserializationFeature;
import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.module.SimpleModule;
import com.fasterxml.jackson.databind.ser.std.ToStringSerializer;
import com.fasterxml.jackson.datatype.jsr310.deser.LocalDateDeserializer;
import com.fasterxml.jackson.datatype.jsr310.deser.LocalDateTimeDeserializer;
import com.fasterxml.jackson.datatype.jsr310.deser.LocalTimeDeserializer;
import com.fasterxml.jackson.datatype.jsr310.ser.LocalDateSerializer;
import com.fasterxml.jackson.datatype.jsr310.ser.LocalDateTimeSerializer;
import com.fasterxml.jackson.datatype.jsr310.ser.LocalTimeSerializer;

import java.math.BigInteger;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.time.LocalTime;
import java.time.format.DateTimeFormatter;

import static com.fasterxml.jackson.databind.DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES;

public class JasksonObjectMapper extends ObjectMapper {
    public static final String DEFAULT_DATE_FORMAT = "yyyy-MM-dd";
    public static final String DEFAULT_DATE_TIME_FORMAT = "yyyy-MM-dd HH:mm:ss";
    public static final String DEFAULT_TIME_FORMAT = "HH:mm:ss";

    public JasksonObjectMapper() {
        //调用父类的构造函数
        super();
        //收到未知属性时不报异常
        this.configure(FAIL_ON_UNKNOWN_PROPERTIES, false);

        //反序列化时，属性不存在的兼容处理
        this.getDeserializationConfig().withoutFeatures(FAIL_ON_UNKNOWN_PROPERTIES);
        
        SimpleModule simpleModule = new SimpleModule()
                .addDeserializer(LocalDateTime.class, new LocalDateTimeDeserializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_TIME_FORMAT)))
                .addDeserializer(LocalDate.class, new LocalDateDeserializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_FORMAT)))
                .addDeserializer(LocalTime.class, new LocalTimeDeserializer(DateTimeFormatter.ofPattern(DEFAULT_TIME_FORMAT)))

                .addSerializer(BigInteger.class, ToStringSerializer.instance)
                .addSerializer(Long.class, ToStringSerializer.instance)
                .addSerializer(LocalDateTime.class, new LocalDateTimeSerializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_TIME_FORMAT)))
                .addSerializer(LocalDate.class, new LocalDateSerializer(DateTimeFormatter.ofPattern(DEFAULT_DATE_FORMAT)))
                .addSerializer(LocalTime.class, new LocalTimeSerializer(DateTimeFormatter.ofPattern(DEFAULT_TIME_FORMAT)));

        //注册功能模块 例如，可以添加自定义序列化器和反序列化器
        this.registerModule(simpleModule);
    }

}

```

同时在mvc容器中注册：

```java
 @Override
    protected void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
        MappingJackson2HttpMessageConverter hc = new MappingJackson2HttpMessageConverter();
        hc.setObjectMapper(new JasksonObjectMapper());
        converters.add(0,hc);
        super.extendMessageConverters(converters);
    }

```



- 此时我们就加入了我们自定义的HttpMessageConverter且优先工作

  <img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20230113131248369.png" alt="image-20230113131248369" style="zoom:50%;" />
