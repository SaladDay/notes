1.get请求：
前端可以使用URL拼接参数形式或者表单形式提交
后端可以使用@RequestParam,参数匹配接收，对象接收
get请求不可以使用@RequestBody接收

2.post请求
前端采用表单方式（application/x-www-form-urlencoded）提交时，后端可以使用@RequestParam方式，参数匹配方式，对象方式接收
前端采用application/json形式提交，后端使用@RequestBody方式接收
前端采用URL拼接参数形式提交，后端使用@RequestParam方式，参数匹配方式接收
