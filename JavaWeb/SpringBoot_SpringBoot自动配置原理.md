---
title: SpringBoot自动配置原理
date: 2023-01-10 15:41:50
tags:
categories: SpringBoot
cover:
---
### 前置知识：
#### 1.@Import机制

@Import作用在配置类上，可以帮助我们将一些需要引入的Bean加入IOC容器中

其中可以有四种写法：

1. 导入Bean
2. 导入@Configuration配置类(在配置类上可以省略此注解)

3. 导入ImportSelector实现类。可以用于加载配置文件中的类/自定义注解所标识的类
4. 导入ImportBeanDefinitionRegister实现类。

#### 2.@Enable*

一般来说@Enable*都是在内部实现了@Import；

例如，我有一个类叫做User，若业务代码中需要使用则需要用@Import(User.class),若需要引入的Bean过多会导致代码混杂；此时我封装一个@EnableUser，在内部实现@Import(User.class),若我需要使用User则使用此Enable注解即可，并且注解之间也可以多次嵌套。

#### 3.@Conditional

需要接受一个条件类(extend condition),condition的实现类需要重写matches方法，若返回true则能加入此Bean入IOC。

Spring封装的很多的@ConditionOn*,意为在...环境下，此Bean加入IOC容器；

### 正文：

1. SpringBoot自动装配的核心注解在@SpringBootApplication，此注解是一个复合类注解，其中包括：

   <img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20230110160931809.png" alt="image-20230110160931809" style="zoom:67%;" />

   1. @SpringBootConfiguration是包含@Configuration,说明SpringBoot启动类是一个配置类，会被加载进SpringIOC中；

      <img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20230110161007732.png" alt="image-20230110161007732" style="zoom:67%;" />

   2. @EnableAutoConfiguration标识开启自动配置，注意这个是@Enable*注解，说明其内部是以@Import为原理的。

      <img src="https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20230110161046566.png" alt="image-20230110161046566" style="zoom:67%;" />

      除去元注解，其中还剩下两个注解

      1. @AutoConfigurationPackage，内部导入了AutoConfigurationPackages.Registrar.class，内部实现了ImportBeanDefinitionRegistrar，其能够扫描启动类包及其所有子包内的组件并加入IOC容器中。

         **因此我们很多情况下都将Dao,service等组件放在启动类包下**

      2. @Import导入了AutoConfigurationImportSelector.class，其内部实现了ImportSelector。其中有一个List\<String> getCandidateConfigurations(AnnotationMetadata metadata, AnnotationAttributes attributes)方法，其中通过SpringFactoriesLoader.loadFactoryNames()方法查找位于META-INF/spring.factories文件中的所有自动配置类，并加载这些类。 

         1. META-INF/spring.factories文件是以key-value键值对的形式存储文件里，其中有一个key=EnableAutoConfiguration，它对应的value值就是一个个以AutoConfiguration结尾来命名的 xxxAutoConfiguration 自动配置类。**注意在新版本中，此代码的位置转移到了spring-autoconfigure-metadata.properties文件中**

         2. 其中诸多的*AutoConfiguration其实就是Spring容器的JavaConfig形式，它的作用就是为Spring容器导入bean。

         3. 而很重要的一点就是并不是所有的*AutoConfiguration都会生效，其中几乎都配置了@Conditional注解，只有在满足一定的条件才会配置。![image-20230110162446329](https://saladday-figure-bed.oss-cn-chengdu.aliyuncs.com/img/image-20230110162446329.png)

            @ConditionOnxxx注解（新版本中为properties）用来指定自动配置类在哪些条件下会生效。我们要使用哪些类，就直接在spring boot项目中的pom.xml文件中导入相应的启动器即可，这样spring boot就会利用@ConditionOnxxx注解使我们需要的自动配置类生效，将该类的bean注入到spring容器中，这样就完成了整个自动配置的过程。


   ### 总结：

   - @SpringBootApplicaiotn:启动类注解
     - @SpringBootConfiguration：
       - @Configuration：说明启动类是一个配置类
     - @EnableAutoConfiguration
       - @AutoConfigurationPackage
         - @Import（AutoConfigurationPackages.Registrar.class）：导入Registrar，自动扫描启动类下的子包
       - @Import({AutoConfigurationImportSelector.class})：加载并扫描META-INFO下的配置类，加载*AutoConfiguration类，在满足condition的情况下加载类中的Bean进入IOC



​	



​	

​		

​	