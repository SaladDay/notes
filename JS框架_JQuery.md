---
title: JQuery
date: 2021-06-20 12:33:18
tags:
categories: JS
cover:
---
### 1. JS和JQ对象的相互转换

    1. jQuery对象获取的是一个object，js获取的是一个集合
    2. 两者互相转换
     - JQ-->JS	:	jQ对象[索引] 或者 jQ对象.get(索引)
     - JS-->JQ	:	$(JS对象)  


### 2.DOM操作
    1. 内容操作
        1. html(): 获取/设置元素的标签体内容   
        2. text(): 获取/设置元素的标签体纯文本内容   
        3. val()： 获取/设置元素的value属性值
    2. 属性操作
    	1.通用属性操作
            1. attr(): 获取/设置元素的属性
            2. removeAttr():删除属性
            3. prop():获取/设置元素的属性
            4. removeProp():删除属性
    
            * attr和prop区别？
            * prop:property
            * attr:attribute
            1. 如果操作的是元素的固有属性，则使用prop
            2. 如果操作的是元素自定义的属性，则使用attr
        2.对class属性操作(因为class属性比较常用，多用于改变css样式)
        	1.addClass()
        	2.removeClass()
        	3.toggleClass():判断元素对象上是否有此属性，有则删除，无则添加；
    3. 增删改查：
    	1.append():
    	2.prepend()
    	3.appendTo()
    	4.prependTo()
    	
    	5.after()
    	6.before()
    	7.insertAfter()
    	8.insertBefore()
    	
    	9.remove()
    	10.empty():清空元素的所有后代元素

### 3.动画

	1. 三种方式显示和隐藏元素
		1. 默认显示和隐藏方式
			1. show([speed,[easing],[fn]])
			2. hide([speed,[easing],[fn]])
			3. toggle([speed],[easing],[fn])
		
		2. 滑动显示和隐藏方式
			1. slideDown([speed],[easing],[fn])
			2. slideUp([speed,[easing],[fn]])
			3. slideToggle([speed],[easing],[fn])
	
		3. 淡入淡出显示和隐藏方式
			1. fadeIn([speed],[easing],[fn])
			2. fadeOut([speed],[easing],[fn])
			3. fadeToggle([speed,[easing],[fn]])
	2. 参数的意义
			1. speed：动画的速度。三个预定义的值("slow","normal", "fast")或表示动画时长的毫秒数值(如：1000)
			2. easing：用来指定切换效果，默认是"swing"，可用参数"linear"
				* swing：动画执行时效果是 先慢，中间快，最后又慢
				* linear：动画执行时速度是匀速的
			3. fn：在动画完成时执行的函数，每个元素执行一次。

### 4.遍历

	1.JS的遍历
		- for(;;)
	2.JQ的遍历
		1.jq对象.each(callback)
		2.$.each(object,callback)
	3.callback
		- 可以传入两个参数，index,element--index代表此对象在集合中的索引--element代表当前对象(JS对象)
		- 如果在其中return true代表continue;如果在其中return false代表break;

### 5.插件

	1.效果：增强JQuery的功能
	2.实现方法：
		$.fn.extend(object):Object字典类型 name:function
		- 对JQ对象进行增强
		
		$.extend(object)
		- 对jQuery/$对象进行增强