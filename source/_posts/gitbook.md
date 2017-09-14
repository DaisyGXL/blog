title: git-pages
date: 2017-09-14
categories: git
summary_img: https://p0.meituan.net/dpnewvc/841fe2f06898fce64c7fb18c26151e83293936.png
tags:
- blog
- 电子书
- git

---

本文用来说明了如何从零开始基于gitpages搭建一个简易版的电子书。

<!-- more -->
### 创建本地gitbook
第一步，先安装脚手架工具
<pre><code>npm install gitbook-cli -g</code></pre>
然后，创建一个笔记文件夹
<pre><code>mkdir daisy-note</code></pre>
然后执行
<pre><code>cd daisy-note
gitbook init</code></pre>
这样，可以生成这样一个gitbook项目，其中包含下面两个文件   
<li>README.md 的内容会显示在书皮上</li>
<li>SUMMARY.md 是目录</li>

### 启动服务器，查看和编辑书籍
<pre><code>gitbook serve</code></pre>
这样，可以启动一个服务器，然后到 localhost:4000 端口，就可以看到这本书了。

可以修改 SUMMARY.md 来添加书籍目录
```md
# Summary

* [Introduction](README.md)
* [第一章：git](git/index.md)
  - [第一节：如何创建这样一篇电子书](git/gitbook.md)
```
创建 git 文件夹，然后里面就可以写笔记了。

### 托管我的 gitbook
首先到 github.com 上创建 daisy-note 仓库。  
为了部署方便，我们把我们的 daisy-note 的内容结构稍微调整一下，把原有的所有内容都放到 content 文件夹中，也就是有这样的目录结构
<pre><code>➜  daisy-note ls content
README.md  SUMMARY.md git
➜  daisy-note</code></pre>
然后，把当前项目变成一个 nodejs 的项目：
<pre><code>cd daisy-note
npm init</code></pre>
然后，package.json 中添加这些代码：
```json
"scripts": {
 "start": "gitbook serve ./content ./gh-pages",
 "build": "gitbook build ./content ./gh-pages",
 "deploy": "node ./scripts/deploy-gh-pages.js",
 "publish": "npm run build && npm run deploy",
 "port": "lsof -i :35729"
},
```
有了上面的 npm 脚本之后，我们如果我想在本地 4000 端口查看本书，我需要运行
<pre><code>npm start</code></pre>
在准备上传之前，先来创建一个 .gitignore 文件，里面填写
<pre><code>gh-pages
node_modules</code></pre>
然后，运行
<pre><code>git init
git add -A
git commit -a -m"hello my book"
git remote add origin git@github.com/DaisyGXL/daisy-note.git
git push -u origin master</code></pre>
上面这些完成后，gitbook 的原始代码就被安全的备份到 master 分支了。访问 https://github.com/DaisyGXL/daisy-note 可以看到这些内容。

### 部署书籍到 gh-pages  
这一步，可以手动做：
<li>第一步：运行 npm run build ，来把 md 文件翻译成 html 放到 gh-pages 文件夹</li>
<li>第二步：拷贝 gh-pages 中的所有文件，到本仓库的 gh-pages 分支，然后上传</li>
<li>第三步：以后每次修改完都需要拷贝到 gh-pages 分支</li>
手动做很麻烦，所以，我们采用一个 npm 包，来帮助我们完成上面的操作
<pre><code>cd daisy-note/
npm i --save gh-pages</code></pre>
然后创建 my-note/scripts/deploy-gh-pages.js  
里面的内容是：
```javascript
'use strict';
var ghpages = require('gh-pages');
main();
function main() {
    ghpages.publish('./gh-pages', console.error.bind(console));
}
```
这样，每次书稿有了修改，运行
<pre><code>npm run publish</code></pre>
就可以把书稿部署到 https://daisygxl.github.io/daisy-note

如果本地书稿正在运行中，也可以执行下面命令将书稿进行部署
<pre><code>npm run deploy</code></pre>

** ps: 本文依据happypeter老师的教程整理

最近访客
<div class="ds-recent-visitors" data-num-items="39" data-avatar-size="40" id="ds-recent-visitors"></div>