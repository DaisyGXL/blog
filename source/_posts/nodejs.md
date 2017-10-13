title: Nodejs
date: 2017-09-25
categories: Node
tags:
- Node
- javascript

---

本文主要是一个nodejs的学习笔记。

<!-- more -->

# 前言
NodeJs 可以解析Js代码（没有浏览器安全级别的限制）
提供系统级别的API：
- 文件的读写
- 进程管理
- 网络通信

Node.js的版本：
- 偶数位为稳定版本（0.6.x 0.8.x)
- 奇数为非稳定版本 (0.5.x 0.7.x)

# 在Mac环境下搭建一个简单的web服务器
前提：
- 安装xcode <code>xcode-select --install</code>
- 检查是否已经安装 Python
- 安装 Homebrew
- 安装 Node
- 创建 Demo 目录

在以上步骤都准备好后，可以开始搭建我们最简单的Web服务器了，新建文件server.js，粘贴下面代码。
```javascript
var http = require('http');
http.createServer(function(req,res){
    res.writeHead(200,{'Content-Type':'text/plain'});
    res.end('Hello World\n');
}).listen(1337,'127.0.0.1');
console.log('Server running at http://127.0.0.1:1337/');
```
然后，在server.js目录下运行命令，即可启动Web服务器。
```
node serve.js
```
这样，就可以通过访问127.0.0.1:1337访问到我们的Hello World页面了。

# 模块与包管理工具
**js的天生缺陷——缺少模块化管理机制**
·表现
> JS中容易出现变量被覆盖，方法被替代的情况（既被污染）。特别是存在依赖关系时，容易出现错误。这是因为JS缺少模块管理机制，来隔离实现各种不同功能的JS判断，避免它们相互污染。

·解决
> 经常采用命名空间的方式，把变量和函数限制在某个特定的作用域内，人肉约定一套命名规范来限制代码，保证代码安全运行。jQuery中有许多变量和方法，但是无法直接访问，必须通过jQuery，$调用各个方法。

**Commonjs规范**
不同于jQuery，Commonjs是一套规范，约定了js如何组织，如何编写，包括包，二进制，套接字，单元测试等等。大部分标准在拟定和讨论之中，首先把执行不同任务的代码块和代码文件看为独立的模块，每一个模块都是一个单独的作用域，但不是孤立的，可能存在依赖关系。每个模块分为三个部分，定义、标识和引用。这套规范与现实产品如node.js相互影响，良性循环。

**NodeJs的模块管理机制**
基于commonjs实现了模块管理系统。node中每一个js文件都是一个独立的模块，在其内部不需要有命名空间，不需要担心变量的污染和方法定义时的隔离。同时模块之间可以组合形成更强大的模块或功能包。npm即是用来管理各种功能包的。

**模块的分类**
- 模块的分类: 核心模块、文件模块、第三方模块；
- 模块的引用：可以通过路径和模块名。模块名引用最终也会被映射为路径。包含了核心函数的核心模块会在node启动时被预先加载。
- 文件模块、第三方模块 都是非核心模块，文件模块就是本地模块。

**创建一个最简单的nodejs模块**
1.创建student.js
```javascript
function add(student) {
    console.log("Add student:" + student);
}

exports.add = add;
```
2.创建tescher.js
```javascript
function add(teacher) {
    console.log("Add teache:" + teacher);
}

exports.add = add;
```
3.创建klass.js，引入上面两个模块
```javascript
var student = require('./student');
var teacher= require('./teacher');

function add (teacherName, students) {
    teacher.add(teacherName);
    students.forEach(function(item, index) {
        student.add(item);
    });
}

exports.add = add;
```
4.创建index.js，调用klass模块
```javascript
var klass = require('./klass');

klass.add('Lily', ['hoho', 'xixi']);
```
 
最终，在命令行中执行<code>node index.js</code>，输出结果如下：
```
➜  school node index.js
Add teache:Lily
Add student:hoho
Add student:xixi
```

# Node的API
## URL网址解析
**url解析**
<code>url.parse(urlString, 是否把query转为对象, 是否识别未知协议的url);</code>
```
> url
{ Url: [Function: Url],
  parse: [Function: urlParse],
  resolve: [Function: urlResolve],
  resolveObject: [Function: urlResolveObject],
  format: [Function: urlFormat],
  URL: [Function: URL],
  URLSearchParams: [Function: URLSearchParams],
  domainToASCII: [Function: domainToASCII],
  domainToUnicode: [Function: domainToUnicode] }
```
```
> url.parse('http://www.imooc.com/video/6710?form=scott#loor1')
Url {
  protocol: 'http:',
  slashes: true,
  auth: null,
  host: 'www.imooc.com',
  port: null,
  hostname: 'www.imooc.com',
  hash: '#loor1',
  search: '?form=scott',
  query: 'form=scott',
  pathname: '/video/6710',
  path: '/video/6710?form=scott',
  href: 'http://www.imooc.com/video/6710?form=scott#loor1' }
```

**url格式化**
<code>url.format({})</code>
```
> url.format({
...   protocol: 'http:',
...   slashes: true,
...   auth: null,
...   host: 'www.imooc.com',
...   port: null,
...   hostname: 'www.imooc.com',
...   hash: '#loor1',
...   search: '?form=scott',
...   query: 'form=scott',
...   pathname: '/video/6710',
...   path: '/video/6710?form=scott',
...   href: 'http://www.imooc.com/video/6710?form=scott#loor1' })
'http://www.imooc.com/video/6710?form=scott#loor1'
```

**url拼接**
<code>url.resolve(str1, str2)</code>
```
> url.resolve('http://immoc.com/', '/course/list')
'http://immoc.com/course/list'
```

## querystring参数解析
**参数序列化**
<code>querystring.stringify({}, 参数之间的连接符-默认为&, key和value之间的符号-默认=)</code>
```
> querystring.stringify({name:'scott', course:['jade','node'], from: ''})
'name=scott&course=jade&course=node&from='

> querystring.stringify({name:'scott', course:['jade','node'], from: ''}, ',')
'name=scott,course=jade,course=node,from='

> querystring.stringify({name:'scott', course:['jade','node'], from: ''}, ',',':')
'name:scott,course:jade,course:node,from:'
```

**参数反序列化**
<code>querystring.parse('', 参数之间的连接符-默认为&, key和value之间的符号-默认=)</code>
```
> querystring.parse('name=scott&course=jade&course=node&from=')
{ name: 'scott', course: [ 'jade', 'node' ], from: '' }
```

**转义**
<code>querystring.escape(str)</code>
```
> querystring.escape('<哈哈>')
'%3C%E5%93%88%E5%93%88%3E'
```

 **反转义**
 <code>querystring.unescape</code>
 ```
 > querystring.unescape('%3C%E5%93%88%E5%93%88%3E')
'<哈哈>'
```

## Http
1.http客户端发起请求，创建端口
2.http服务器在端口监听客户端的请求
3.http服务器向客户端返回状态和内容

**浏览器发起请求到步骤**
1.Chrome搜索自身的DNS缓存
2.搜索操作系统的DNS缓存
3.读取本地的Host文件
4.浏览器发起一个DNS一个系统调用（向运营商发起，下面小节步骤）
5.浏览器获得域名对应的IP地址后，发起HTTP的三次握手
6.TCP/IP连接建立起来后，浏览器就可以向服务器发送HTTP请求了
7.服务器端接受到这个请求，根据路径参数，经过后端处理后，把处理后的一个结果的数据返回给浏览器
8.浏览器获取到目标网址的数据，例如返回一个HTML文件,HTML文档内的JS/CSS/图片静态资源同样也是一个个HTTP请求，也要包括上述步骤
9.浏览器根据获取到的资源对页面进行渲染，最终把网页呈献给用户

**运营商的DNS服务器**
1.宽带运营商服务器查看本身缓存
2.运营商服务器代替浏览器发起一个迭代的DNS解析请求，运营商服务器把结果返回给操作系统内核同时缓存起来，操作系统内核把结果返回给浏览器

**http头和正文信息**
HTTP头发送的是一些附加的信息：内容类型、服务器发送相应的日期、HTTP状态码。
正文就是用户提交的表单数据。

1、什么是回调函数？
回调是异步编程时的基础，将后续逻辑封装成起始函数的参数，逐层嵌套
2、什么事同步/异步？
同步：发送方发送数据后，等待接收方发回响应以后才发下一个数据包的通讯方式
异步：发送方发出数据后，不等接收方发回响应，接着发送下个数据包的通讯方式
3、什么事I/O?
文件系统里面 磁盘的写入（in）磁盘的读取（out）
4、什么是单线程/多线程？
一次只能执行一个程序叫做单线程
一次能执行多个程序叫做多线程
5、什么是阻塞/非阻塞？
阻塞：前一个程序未执行完就得一直等待
非阻塞：前一个程序未执行完时可以挂起，继续执行其他程序，等到使用时再执行
6、什么是事件？
一个触发动作（例如点击按钮）
7、什么是事件驱动？
一个触发动作引起的操作（例如点击按钮后弹出一个对话框）
8、什么是基于事件驱动的回调？
为了某个事件注册了回调函数，但是这个回调函数不是马上执行，只有当事件发生的时候，才会调用回掉函数，这种函数执行的方式叫做事件驱动~这种注册回掉就是基于事件驱动的回调，如果这些回调和异步I/O（数据写入、读取）操作相关，可以看作是基于回调的异步I/O。只不过这种回调在nodejs中是由事件来驱动的
9、什么是事件循环？
事件循环Eventloop，倘若有大量的异步操作，如一些I/O的耗时操作，甚至是一些定时器控制的延时操作，它们完成的时候都要调用相应的回调函数，而从完成一些密集的任务，而又不会阻塞整个程序执行的流程，此时需要一种机制来管理，这种机制叫做事件循环
总而言之，管理大量异步操作的机制叫做事件循环

**作用域和上下文**
作用域：与调用函数,访问变量的能力有关 作用域分为：局部和全局（在局部作用域里可以访问到全局作用域的变量，但在局部作用域外面就访问不到局部作用里面所设定的变量）

上下文：与this关键字有关 是调用当前可执行代码的引用
       this总是指向调用这个的方法的对象
js里的this 通常是当前函数的拥有者
this 是js的一个关键字 代表函数运行时自动生成的一个内部对象 只能在函数内部使用

1.作为对象的方法 
this在方法内部，this就指向调用这个方法的对象
```javascript
var pet =  {
    words: '...',
    speak: function() {
        console.log(this.words);
        console.log(pet===this);
    }
}

pet.speak();
```

2.函数的调用
this指向执行环境中的全局对象（浏览器->window  nodejs->global）
```javascript
function pets(words) {
    this.words = words;
    console.log(this.words);
    console.log(this);
    console.log(this===global);
}

pets('...');
```

3.构造函数
this所在的方法被实例对象所调用，那么this就指向这个实例对象
```javascript
function petss(words) {
    this.words = words;
    this.speak = function() {
        console.log(this.words);
        console.log(this);
    }
}

var cat = new petss('miao');
cat.speak();
```

更改上下文方法(更改this指向的内容,可方便地实现继承)：
- call(list);
```javascript
var pet = {
    words: '...',
    speak: function(say) {
        console.log(say + ' ' + this.words);
    }
}
var dog = {
    words: 'wang'
}
//通过使用call方法，将pet的上下文指向为dog内部
pet.speak.call(dog, 'Speak');
```
- apply(array);
根据call()、apply()改变上下文this指向的特性,也可以方便实现继承。
```javascript
function Pet(words) {
    this.words = words;
    this.speak = function() {
        console.log(this.words);
    }
}

function Dog(words) {
    Pet.call(this, words);
    //Pet.apply(this. arguments);
}

var dog = new Dog('wang');
dog.speak();
```

**事件Events**
a.EventEmitter支持多个事件监听，最大为10，也可以自定义最大数
```javascript
//添加监听
var EventEmitter = require('events').EventEmitter;
var instance = new EventEmitter();
instance.on('event',function(arguments){});
```
b.如果超过十个也能执行，不过有可能会造成内存泄漏
```javascript
//自定义最大数
//每个setMaxListeners针对的是一个特定事件：即event1,event2,... 默认最大都为10,本例为num
instance.setMaxListeners(num);
```
	c.事件监听之后，需要emit(发射,发出)才会执行
```javascript
instance.emit('event',arguments)
```
d.判断是否监听
```javascript
var a = instance.emit('event',arguments)	
console.log(a)   //打印出来的是布尔值true or false
```
e.移除监听事件
```javascript
//移除单个事件监听
instance.removeListener('event',funcName)	//移除事件需具名函数，匿名函数不行

//移除多个事件监听
instance.removeAllListerner()	//不传参表示移除所有事件监听
instance.removeAllListerner('event')	//移除特定event的所有事件监听
```
f.计算事件监听数量
```javascript
//第一种
instance.listeners('event').length

//第二种
EventEmitter.listenerCount(instance,'event')
```

**Http请求的性能测试**
1.如果没有安装Apache的话，首先要安装Apache。
2.mac直接在终端命令行中运行命令 <code>ab -n1000 -c10 http://localhost:2017/</code> 即测试本地服务的性能。
```javascript 
var http = require('http');

http.createServer(function(req, res) {
    res.writeHead(200, {'Content-Type': 'text/plain'});
    res.write('Hello nodejs');
    res.end();
}).listen(2017)
```
3.ab -n1000 -c10 url   即测试任意url的性能
-n1000 总请求数1000 默认值1
-c10 并发数10  默认值1
-t 测试的时间
-p post数据文件
    
##小实践
**http小爬虫**
```javascript
var http = require('http');
var url = 'http://www.imooc.com/learn/901';
var cheerio = require('cheerio');
function filterChapters(html){
	var $ = cheerio.load(html);
    var chapters = $('.learnChapter');
    var courseData = [];
	chapters.each(function(item){
		var chapter = $(this);
		var chapterTitle = chapter.find('strong').text();
		var videos = chapter.find('.video').children('li');
		var chapterData = {
			chapterTitle: chapterTitle,
			videos: []
		}
        videos.each(function(item){
			var video = $(this).find('.studyvedio');
			var videoTitle = video.text();
			var id = video.attr('href').split('video/')[1];
			chapterData.videos.push({title: videoTitle,id: id});
		});
		courseData.push(chapterData);
	});
	return courseData;
}
function printCourseInfo(courseData){
	courseData.forEach(function(item){
		var chapterTitle = item.chapterTitle;
		console.log(chapterTitle + '\n');
		item.videos.forEach(function(video){
			console.log('[' + video.id + ']' + video.title + '\n');
		});
	});
}
```
**评论灌水**
```javascript
var http =require('http');
var querystring =require('querystring');

var postData = querystring.stringify({
	'content':'小路人注水测试评论',
	'mid':8837
});
var options={
	hostname : 'www.imooc.com',
	port:80,
	path:'/course/docomment',
	method:'POST',
	headers:{消息头});
var req=http.request(options ,function (res){
	console.log('Status: '+res.statusCode);
	console.log('headers: '+JSON.stringify(res.headers));
	res.on('data',function(chunk){
		console.log(Buffer.isBuffer(chunk));
		console.log(typeof chunk);
	});
	res.on('end',function(){
		console.log('小路人注水评论完毕！');
	});
});
req.on('error',function(e){
	console.log('Error: '+e.message);
});
req.write(postData);
req.end();
```