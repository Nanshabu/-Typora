### 一、初识Node.js

==发现webApi没学好,一些东西比如canvas都还没学到包括XHR也需要重新去复习==

nodejs是基于v8引擎的JavaScript运行环境。相对的浏览器是作为js的前端运行环境，而nodejs就是js的后端运行环境。需要注意的是nodejs无法调用浏览器的DOM和BOM等内置Api。

Nodejs可以做什么：

![1667032662265](D:\!nanshabu\big3\MyLearningNotesBook\img\1667032662265.jpg)

![1667034109718](D:\!nanshabu\big3\MyLearningNotesBook\img\1667034109718.jpg)

#### 1）学习线路

js基础语法+nodejs内置API模块（fs、path、http）+第三方API模块（Express、Mysql等）

#### 2）安装nodejs

下载 安装 

终端查看版本`npm -version`

#### 3）开发环境搭建

使用vsCode自带的继承终端，在文件夹右键打开终端，输入node ./文件.js运行js文件

#### 4)模块-包

依赖关系容易乱，方法属性容易被覆盖重写，

##### CommonJS规范

##### npm的使用

pack.json负责记录版本，其中dependencies记录了所有的包以及版本，版本前面可以加^（保持第一位不变要最新的）或者~（保持前两位不变要最新）或者直接写*（最新版本）

##### node包的关系

node内置模块，自己写的模块，第三方模块

`npm list`查看当前安装模块

#### 全局安装nrm

npm install nrm -g

可以使用nrm控制镜像	例如nrm ls查看可用镜像

例如nrm use taobao	选择淘宝镜像

#### yarn

npm install yarn	安装yarn

yarn init	初始化yarn

yarn add md5	yarn安装md5

yarn upgrade md5@2	yarn更新md5到版本2

yarn remove md5	yarn卸载md5

yarn install	根据本地的packagejson和yarnlock文件生成模块

#### ES模块化写法

文件npm init之后，在packagejson文件中加一个"type":"module"，就完事

### 内置模块

#### http模块

创建http对象，通过http生成server，并传回request和response，在回调函数中，可以对response进行操作。对server对象进行同步监听，需要设置端口号。

```js
let http = require("http")

let moduleHTML = require("./module/renderHTML")
let moduleStatus = require("./module/renderStatus")
//创建服务器
http.createServer((req, res) => {
    //接受浏览器传的参数，返回渲染的内容
    // req  接受浏览器传来的参数
    // res  返回渲染的内容

    console.log(req.url);

    res.writeHead(moduleStatus.renderStatus(req.url), { "Content-Type": "text/html;charset=utf-8" })

    res.write(moduleHTML.renderHTML(req.url))

    res.end()

}).listen(3000, () => {   //监听3000端口号
    console.log("server start");
})
```

```js
//renderStatus模块
function renderStatus(url){
    var arr = ["/home","/shit","/api/shit","/api/shit1"]
    return arr.includes(url)?200:400
}

module.exports = {
    renderStatus,
}
```

```js
//renderHTML模块
function renderHTML(url) {
    switch (url) {
        case "/home":
            return `    <html>
                <b>home页面</b>
            </html>`
        case "/api/shit":
            return `[1,2,3]`

        case "/api/shit1":
            return `{name:"大傻"}`
        default:
            return `
    
                <html>
                    <b>404 NOT FOUND</b>
                </html>
                
                `
    }

}

exports.renderHTML = renderHTML

```



第二种主函数的写法：

创建http对象，通过http对象创建server，使用on方法对server进行监听请求事件，listen也需要设置端口号

```js
const http = require('http');

// 创建本地服务器来从其接收数据
const server = http.createServer();

// 监听请求事件
server.on('request', (request, res) => {
  res.writeHead(200, { 'Content-Type': 'application/json' });
  res.end(JSON.stringify({
    data: 'Hello World!'
  }));
});

server.listen(8000);
```

#### url模块

url.parse(request.url)：对url进行解析，返回一个Url对象，里面包含url解析结果。第二个参数填true可以对query进行json的转换

```js
const url = require('url')
const urlString = 'https://www.baidu.com:443/ad/index.html?id=8&name=mouse#tag=110'
const parsedStr = url.parse(urlString)
console.log(parsedStr)
```

format：对parse返回的对象生成url，相当于逆向的parse

```js
const url = require('url')
const urlObject = {
  protocol: 'https:',
  slashes: true,
  auth: null,
  host: 'www.baidu.com:443',
  port: '443',
  hostname: 'www.baidu.com',
  hash: '#tag=110',
  search: '?id=8&name=mouse',
  query: { id: '8', name: 'mouse' },
  pathname: '/ad/index.html',
  path: '/ad/index.html?id=8&name=mouse'
}
const parsedObj = url.format(urlObject)
console.log(parsedObj)
```

resolve：对url地址**字符串**进行操作。

```js
const url = require('url')
var a = url.resolve('/one/two/three', 'four')  ( 注意最后加/ ，不加/的区别 )
var b = url.resolve('http://example.com/', '/one')
var c = url.resolve('http://example.com/one', '/two')
console.log(a + "," + b + "," + c)
```

上面都是老版本的，现在都是直接new一个URL对象，然后通过format方法或者是getParams方法或者URL方法进行操作。

甚者，可以通过fileURLToPath对文件路径，urlToHttpOptions对路径生成OptionHttp对象（区别于URL对象）

###### 	03  querystring模块

**03.1 parse**

```js
const querystring = require('querystring')
var qs = 'x=3&y=4'
var parsed = querystring.parse(qs)
console.log(parsed)

```

**03.2 stringify**

```js
const querystring = require('querystring')
var qo = {
  x: 3,
  y: 4
}
var parsed = querystring.stringify(qo)
console.log(parsed)

```

**03.3 escape/unescape**

escape对字符串进行转义，可以防止sql注入

unescape则是反过来

#### http模块补充

一、跨域问题的解决CORS

在服务器请求过程中如果在不同地址下请求数据往往会出现跨域问题

前端解决办法可以通过jsonp的方法来解决

另外也可以通过后端对服务器访问的权限扩大来实现，在writedHead中添加`"access-control-allow-origin":"*"`将跨域问题扼杀在摇篮。

二、用node做中间件

node服务器请求不会跨域，用node服务器请求完在返回给前端页面。

http.get(路径，回调函数)

回调函数返回一个**数据流**，需要对数据进行接受处理，在高级点可以cb写一个回调函数返回给上层调用。

使用猫眼的get接口`https://i.maoyan.com/api/mmdb/movie/v3/list/hot.json?ct=汕头&ci=117&channelId=4`

使用小米有品的post接口`https://m.xiaomiyoupin.com/mtop/market/search/pla	ceHolder`

使用https.request(options, function)接受一个**数据流**

options需要包括

hostname

port 80 是http	port 443是https

path

method

headers包括

​	Content-type：application

#### event模块

event用于函数的解耦

主要是两步，一步是定义监听事件，二步是触发事件。

```js
const EventEmitter = require("events")

const event = new EventEmitter()

event.on("play",(data)=>{
    console.log(`事件触发了-play,${data}`);
})

event.emit("play",1111)
```

需要注意的是，event最好写在函数中，如果全局定义的话事件的定义不会覆盖，也就是说你定义一次play下次全局在定义一次会生成两个对play的监听，触发也是一次触发两次依次叠加。

#### fs文件模块

用于文件操作创建改名删除读写

__dirname 相当于文件的所处目录

##### mkdir(filename, function(err))	创建文件夹

##### rename(file1name, file2name, function(err))	更改文件夹名

##### rmdir(filename, function(err))	删除文件夹

##### writefile(filename, 编码, (err,data)=>{})	读取文件（以什么编码）

##### unlink(filename,function(err))	删除文件

注意，当文件夹中还有东西时使用rmdir会报noempty的错误，需要先unlink文件夹中的所有文件再rmdir

##### readdir(filename,function(err))	查看文件夹下的所有文件

stat(filename, (err,data)=>{})	查看该文件夹的状态，可以通过.isFile或者.isDirector来判断是文件还是目录

##### 删除有文件的文件夹

>  使用遍历将文件夹里面所有文件删除，unlink是异步操作，最后一步通过rmdir删除文件会在each之前执行，报非空的错误，需要通过同步控制来控制代码的执行顺序

##### fs搭配promise

```js
fs.readdir("./avatar").then(async (data)=>{

    // let arr = []
    // data.forEach(item=>{
    //     arr.push(fs.unlink(`./avatar/${item}`))
    // })

    // await Promise.all(arr)
    //升级一下，使用map方法
    await Promise.all(data.map(item=>{fs.unlink(`./avatar/${item}`)}))

    await fs.rmdir("./avatar")
})
```

#### Stream流模块

##### 1、fs.creatWriteStream

执行的写法

ws.write()	ws.end()

##### 2、fs.creatReadStream

监听的写法

rs.on("data",chunk=>{})	rs.on("end",()=>{})	rs.on("error",err=>{})







































































### 查漏补缺

1. JSONP动态创建script标签解决跨域请求问题



### BUG

node在请求发出后，http代码执行了两次

大概率是因为http重新对/favicon.ico多做了一次请求，而你忘了对他进行排除。多加一个if return解决问题



### 报错

#### 下载md5的时候：[..................] \ idealTree:0002: sill idealTree buildDeps

解决：配置镜像`npm config set registry https://registry.npm.taobao.org`



#### npm中 下载速度慢 和（无法将“nrm”项识别为 cmdlet、函数、脚本文件或可运行程序的名称。请检查名称的拼写，如果包括路径，请确保路径正确， 然后再试一次）。

1.首先我们看看自己的有没有安装cnpm(查看命令： npm list --depath=0 -g)
2.如果没有我们就安装cnpm(查看命令：npm i cnpm -g)
3.如果安装成功还是报错请看自己的安装路径 （查看命令：npm config get prefix）
4.我们再打开我的电脑(右键)->属性->高级系统->再找到高级这一列->打开环境配置->找到path
->添加自己的npm安装路径 就是(npm config get prefix)这个路径添加到path保存->都保存确定
->然后我们再打开 cmd 使用 nrm ls 就可以看到了
执行 命令 nrm ls 前面显示*号的就是你当前用的服务器下载地址

https://www.cnblogs.com/jpfss/p/11113004.html