title: 基于Hexo和GitPages搭建个人博客
date: 2017-08-28
categories: git
tags:
- github
- hexo
- blog

---

本文主要介绍了如何基于Hexo生成一个静态博客站点，并通过github发布。

<!-- more -->
# Hexo简介  
> A fast, simple & powerful blog framework.--Hexo官网

Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。它有下面几个特点：
- 超快速度 
Node.js 所带来的超快生成速度，让上百个页面在几秒内瞬间完成渲染。
- 支持Markdown 
Hexo 支持 GitHub Flavored Markdown 的所有功能，甚至可以整合 Octopress 的大多数插件。
- 一键部署 
只需一条指令即可部署到Github Pages，或其他网站
- 丰富的插件 
Hexo 拥有强大的插件系统，安装插件可以让 Hexo 支持 Jade, CoffeeScript。  

通过 Hexo 你可以轻松地使用 Markdown 编写文章，除了 Markdown 本身的语法之外，还可以使用 Hexo 提供的 标签插件 来快速的插入特定形式的内容。

# 安装
安装 Hexo 相当简单。然而在安装前，您必须检查电脑中是否已安装下列应用程序：
- Node.js
- Git

网上有很多的安装教程，这里不做赘述。
接下来只需要使用 npm 即可完成 Hexo 的安装。
```
$ npm install -g hexo-cli
```

安装 Hexo 完成后，我们首先需要为我们的项目创建一个指定文件夹，在指定文件夹中执行下列命令， Hexo 将会在指定文件夹中新建所需要的文件。
``` bash
$ hexo init
```

等待安装，安装完成后，<span id="inline-green">指定文件夹</span> 的目录如下：
``` 
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└──
```

继续执行命令
``` bash
$ hexo g
$ hexo s --debug
```
Hexo 将 source 文件夹中除 _posts 文件夹之外，开头命名为 _(下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件夹会被拷贝过去。
这个时候，我们在浏览器中访问 http://localhost:4000/ ，就可以看到基于 Hexo 的默认主题的原型。

# 主题扩展功能
首先，我们需要明白：
>在 Hexo 中有两份主要的配置文件，其名称都是 _config.yml 。其中，一份位于站点根目录下，主要包含 Hexo 本身的配置；另一份位于主题目录下，这份配置由主题作者提供，主要用于配置主题相关的选项。
  我们约定，将前者称为 **站点配置文件**，后者称为 **主题配置文件**
  可以通过hexo的官网选择喜欢的模板进行使用。

# 发布到github.io
在github中创建仓库，命名方式为**用户名.github.io**，一定要以这种方式命名，并且每个github账户只能有一个pages。在站点配置文件中增加发布仓库:
```md
deploy:
  type: git
  repo: https://github.com/DaisyGXL/DaisyGXL.github.io.git
  branch: master
```

运行
```bash
$ hexo g
$ hexo d
```
完成编译发布过程。 
