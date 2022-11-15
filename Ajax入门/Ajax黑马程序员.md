==Ajax==黑马程序员

 Ajax的数据形式。通过调用后端接口获取数据

Ajax的三种请求形式：
$.get('url', data, function)
$.post('url', data, function)
$.ajax({type:,url:,data:},success:function(){})

获取特定ID或者class的jq写法

单击事件on('click')

获取值val

清除前后空格trim

获取自定义标签属性attr

添加内容append

CSS/HTML框架：bootstrap

组件：jQuery UI; jQuery-mouseWheel; 

==思考题==

> 输入框插入信息——如何防止恶意插入（XSS攻击）



组织表单的默认提交行为e.prentDefault

Jquery一次性获取表单的所有数据 .serialize()

关闭textarea的拉伸功能

jquery转原生js--> $('#??')[0]

清空文本框内容reset()



什么是模板引擎？为了减少字符串的拼接，代码结构更清晰

主流模板引擎：thymeleaf、freemaker、enjoy

学习art-template的基本语法
定义数据，调用template()方法，渲染HTML结构，定义模板
{{@value}}的用法 
if判断语句 {{if}}{{else if}}{{/if}}



定义简单的template模板引擎

学习XHR XMLHttpRequest

什么是XHR

readystate的五种状态

> readyState总共有5个状态值，分别为0~4，每个值代表了不同的含义
>
> 0：初始化，XMLHttpRequest对象还没有完成初始化
>
> 1：载入，XMLHttpRequest对象开始发送请求
>
> 2：载入完成，XMLHttpRequest对象的请求发送完成
>
> 3：解析，XMLHttpRequest对象开始读取服务器的响应
>
> 4：完成，XMLHttpRequest对象读取服务器响应结束

JSON转对象JOSN.parse反序列化
对象转JSON:JSON.stringify序列化

思考题

设置上传文件和展示图片
了解父节点和子节点的各类属性
掌握appendChild和removeChild的使用
熟练FileReader的使用 reader.radAsResult()
熟练监听器和event的使用。
input标签中的multiple和accept属性

```javascript
//监听文件上传的进度
xhr.upload.onprogress = function (e) {
    if (e.lengthComputable) {
        console.log(Math.ceil((e.loaded / e.total) * 100))
    }
}
```

bootstrap3的组件进度条
依据xhr的监听器来改变进度条的长度和innerHtml
通过upload.onload监听来改变（removeClass和addClass）最后的进度条状态（绿色）



jquery获取input元素和button元素绑定click事件通过new FormData对象提交files[0]
jquery监听Ajax的请求开始和停止ajaxstart()、ajaxstop()

JSONP解决跨域问题！！?callback=[function]&[...]&[...]

用ajax发起JSONP的请求

```javascript
$.ajax({
    url: 'http://www.liulongbin.top:3006/api/jsonp?name=zs&age=15',
    dataType: 'jsonp',
    success: function(res){
        console.log(res)
    }
})
```



AJAX的异步请求和同步请求   动力节点HTML/CSS/JavaScript/AJAX/jquery从入门到精通-

ajax优化之防抖！setTimeOut()  clearTimeOut()

搜索优化之缓存搜索

优化之设置节流阀，案例鼠标跟随效果（16ms节流阀）



Request Headers请求标头   第一行为请求行=请求方式空格+请求地址空格+协议版本

请求头部：
User-Agent：什么类型的浏览器
Content-Type：描述发送到服务器的数据格式
Accept：描述客户能收到什么类型的返回内容
Accept-Language：描述客户端期望收到那种人类语言的文本内容

![RequestHeaders](D:\nanshabu\big3\MyLearningNotesBook\img\RequestHeaders.jpg)

FormData请求体   只有Post才有请求体，Get没有请求体



响应消息：状态行+响应头部+空行+响应体

状态行=协议版本+状态码+状态描述

<img src="D:\nanshabu\big3\MyLearningNotesBook\img\常见的响应头部字段.jpg" alt="常见的响应头部字段" style="zoom: 50%;" />

#### 响应状态码

![image-20220925162934794](C:\Users\一个豆瓣\AppData\Roaming\Typora\typora-user-images\image-20220925162934794.png)

## Axios

<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

axios.()

请求方法的别名

```javascript
axios.request(config)
axios.get(url[, config])
axios.delete(url[, config])
axios.head(url[, config])
axios.options(url[, config])
axios.post(url[, data[, config]])
axios.put(url[, data[, config]])
axios.patch(url[, data[, config]])
```

### 并发

处理并发请求的助手函数

##### axios.all(iterable)

##### axios.spread(callback)

```javascript
function foo() {
    return axios.get("http://jsonplaceholder.typicode.com/posts")
}
function bar() {
    return axios.get("http://jsonplaceholder.typicode.com/posts/1")
}

axios.all([foo(), bar()]).then(axios.spread((foo, bar) => {
    console.log(foo.data, 11111111)
    console.log(bar.data, 22222)
})).catch(err=>{
    console.log(err,258)
})
```

### Git

初始化仓库

添加到暂存区

从暂存区提交到仓库

查看暂存区

撤销对文件的修改

取消暂存的文件  git reset HEAD 

跳过暂存区直接提交 git commit -a -m

移除文件（git仓库和工作区） git rm -f

移除文件（git仓库） git rm --cached 

## Ajax是什么

ajax的主要功能是发送**异步请求**，什么是异步请求呢？就是能在你页面不刷新不跳转的情况下提供你所需要的数据。

它的优点是

1. 无需刷新页面获取数据
2. 根据用户事件来更新页面部分内容