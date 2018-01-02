title: mocha+supertest单元测试
date: 2017-09-14
categories: node

tags:
- node
- 单元测试
- mocha
- supertest

---

最近一段时间刚刚接手了一个angular1.3前端+node服务器端多项目，作为一个刚刚入门node的新手，在熟悉了node的代码后便对已有的接口进行了单元测试用例的编写，此篇博客用于简单的记录。

<!-- more -->
## Mocha介绍
Mocha是非常流行JavaScript测试框架之一，在浏览器和Node环境都可以使用。
## 安装
```
npm i mocha -g
```
## 简单测试脚本
1.首先写一个简单的例子，add.js
```javasript
function add(a, b){ 
    return a+b; 
} 
module.exports = add;
```
2.新建测试脚本 add.test.js，一般命名规则测试脚本和原脚本同名，但是后缀名为.test.js
```javascript
let calcu = require('./add');
let should = require("should");

describe("add func test",() => {
    it('2 add 2 should equal 4',() => {
      calcu.add(2,2).should.equal(4)
    })
})
```
3.执行测试用例
```
mocha demo1/mocha demo1/calcu.test.js
```
describe 表示测试套件，是一序列相关程序的测试
it表示单元测试(unit test)，也就是测试的最小单位。
## 断言库
断言库可以理解为比较函数，也就是断言函数是否和预期一致，如果一致则表示测试通过，如果不一致表示测试失败，一个unit test里面可以包含多个断言语句。
本身mocha是不包含断言库的，所以必须引入第三方断言库，目前比较受欢迎的断言库  有 should.js、expect.js 、chai，而chai包含should、expect和assert三种风格，可扩展性比较强。
这里举例说明should断言方法：
```
// 全等，相当于=== 
.exactly 
(5).should.be.exactly(5) 

// 对象存在 
.ok 
true.should.be.ok; 
'yay'.should.be.ok; 
(1).should.be.ok; 
({}).should.be.ok; 
false.should.not.be.ok; 

// 真 
.true 
(5===5).should.be.true 
(err === null).should.be.true; 

// 相等,相当于 == 
.eql 
({ foo: 'bar' }).should.eql({ foo: 'bar' }); 
[1,2,3].should.eql([1,2,3]); 
// see next example it is correct, even if it is different types, but actual content the same 
[1, 2, 3].should.eql({ '0': 1, '1': 2, '2': 3 }); 

// 非数字 
.NaN 
(undefined + 0).should.be.NaN; 

// 判断类型 
.typeof 
user.should.be.type('object'); 
'test'.should.be.type('string'); 

// 构造函数的一个实例 
.instanceof user.should.be.an.instanceof(User); 
[].should.be.an.instanceOf(Array); 

// 存在 
.exist() 
should.not.exist(err) 

//深度包含 
.containDeep() 
[[1],[2],[3]].should.containDeep([[3]]); 
[[1],[2],[3, 4]].should.containDeep([[3]]); 
[{a: 'a'}, {b: 'b', c: 'c'}].should.containDeep([{a: 'a'}]); 
[{a: 'a'}, {b: 'b', c: 'c'}].should.containDeep([{b: 'b'}]); 

// 抛出异常 
.throw()和throwError() 
(function(){ throw new Error('fail'); }).should.throw(); 
(function(){ throw new Error('fail'); }).should.throw('fail'); 

// http响应的头部包含 
.header 
res.should.have.header('content-length'); 
res.should.have.header('Content-Length', '123'); 

// 包含或等价于 
.containEql 
({ b: 10 }).should.containEql({ b: 10 }); 
([1, 2, { a: 10 }]).should.containEql({ a: 10 });
```
## 用法
### 常规函数测试
如上面的add.test.js
### 异步函数测试
新建文件book.js
```javascript
let fs = require('fs');

exports.read = (cb) => {
        fs.readFile('./book.txt', 'utf-8', (err, result) => {
            if (err) return cb(err);
            console.log("result",result);
            cb(null, result);
        }) 
}
```
新建文件book.test.js
```javascript
let book = require('./book');
let expect = require("chai").expect;
let book = require('./book');
let expect = require("chai").expect;

describe("async", () => {
  it('read book async', function (done) {
    book.read((err, result) => {
      expect(err).equal(null);
      expect(result).to.be.a('string');
      done();
    })
  })
})
```
运行mocha book.test.js,我们会发现成功了，但是如果我们把book.js增加一个定时函数，改为如下例子：
```javascript
let fs = require('fs');
exports.read = (cb) => {
    setTimeout(function() {
        fs.readFile('./book.txt', 'utf-8', (err, result) => {
            if (err) return cb(err);
            console.log("result",result);
            cb(null, result);
        }) 
    }, 3000);
}
```
会发现报如下错误：
```
Timeout of 2000ms exceeded.
```
这是因为mocha默认每个测试用例最多执行2000毫秒，如果到时没有得到结果，就报错。所以我们在进行异步操作的时候，需要额外指定timeout时间。
```
mocha --timeout 5000 book.test.js
```
这样就保证测试用例成功。
### API测试
单单使用Mocha和should就几乎可以满足所有JavaScript函数的单元测试。但是对于Node应用而言，不仅仅是函数的集合，比如一个web应用的测试。这时候就需要配合一个http代理，完成Http请求和路由的测试。
Supertest是一个HTTP代理服务引擎，可以模拟一切HTTP请求行为。Supertest可以搭配任意的应用框架，从而进行应用的单元测试。
1.安装supertest
<code>npm i supertest --save-dev</code>
2.传入应用来实例化supertest
```javascript
let app = new koa();
let server = app.listen(0);
this.request = supertest(server);
```
3.调用API进行测试
```javascript
//get
describe('GET /users', function(){ 
    it('respond with json', function(done){ 
        request(app) 
        .get('/user') 
        .set('Accept', 'application/json') 
        .expect(200) 
        .end(function(err, res){ 
            should.not.exist(err); 
            res.text.should.containEql('success'); 
            done(); 
        }); 
    }); 
});
//post
describe('test login', function(){ 
    it('login sucessfully', function (done) { 
        request.post('/user') 
        .send({ username: 'username', password: '123456' }) 
        .end(function (err, res) { 
            should.not.exists(err); 
            done(); 
        }); 
    }); 
})
```
另外，可以通过.attach()方法测试文件上传。
## 常用命令行
```
node ./node_modules/.bin/istanbul cover ./node_modules/mocha/bin/_mocha
open coverage/lcov-report/index.html
```
## 生命钩子
mocha一共四个生命钩子 
before()：在该区块的所有测试用例之前执行 
after()：在该区块的所有测试用例之后执行 
beforeEach()：在每个单元测试前执行 
afterEach()：在每个单元测试后执行