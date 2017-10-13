title: Bootstrap
date: 2017-09-18
categories: boostrap
tags:
- bootstrap

---

本文主要介绍了Boostrap的全局样式和常用组件。

<!-- more -->
# Bootstrap的特性
- 响应式设计
- 栅格布局
- 完整的类库
- JQuery插件
- 不同的使用场景

# 从Hello world开始
程序员学习一门语言，第一个例子一定是著名的**Hello world**，那么，我们先来准备一个**Hello world**吧！
首先，到[Bootstrap中文网](http://v3.bootcss.com/getting-started/#download)下载用于生产环境的Bootstrap。 
Bootstrap 提供了两种形式的压缩包，在下载下来的压缩包内可以看到以下目录和文件，这些文件按照类别放到了不同的目录内，并且提供了压缩与未压缩两种版本。
>Bootstrap 插件全部依赖 jQuery
请注意，Bootstrap 的所有 JavaScript 插件都依赖 jQuery，因此 **jQuery 必须在 Bootstrap 之前**引入，就像在基本模版中所展示的一样。在 bower.json 文件中 列出了 Bootstrap 所支持的 jQuery 版本。

下载压缩包之后，将其解压缩到任意目录即可看到以下（压缩版的）目录结构：
```
bootstrap/
├── css/
│   ├── bootstrap.css
│   ├── bootstrap.css.map
│   ├── bootstrap.min.css
│   ├── bootstrap.min.css.map
│   ├── bootstrap-theme.css
│   ├── bootstrap-theme.css.map
│   ├── bootstrap-theme.min.css
│   └── bootstrap-theme.min.css.map
├── js/
│   ├── bootstrap.js
│   └── bootstrap.min.js
└── fonts/
    ├── glyphicons-halflings-regular.eot
    ├── glyphicons-halflings-regular.svg
    ├── glyphicons-halflings-regular.ttf
    ├── glyphicons-halflings-regular.woff
    └── glyphicons-halflings-regular.woff2
```
可以直接引用bootstrap的预编译文件，但是要注意的是，**Bootstrap是要依赖JQuery的**，因此，需要在引用Bootstrap的js之前引用JQuery。
这里使用的是Bootstrap3，需要使用的JQuery版本需要高于1.3。

这样，就完成了我们的第一个例子--Hello world
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Bootstrap</title>
    <link rel="stylesheet" href="./css/bootstrap-theme.min.css">
    <script src="./js/jquery-3.2.1.min.js"></script>
    <script src="./js/bootstrap.min.js"></script>
</head>
<body>
    <button class="btn btn-info">hello world</button>
</body>
</html>
```
这里要强调的是，Bootstrap 使用到的某些 HTML 元素和 CSS 属性需要将页面设置为 HTML5 文档类型。在你项目中的每个页面都要参照下面的格式进行设置。
```html
<!DOCTYPE html>
<html lang="zh-CN">
  ...
</html>
```
另外，**Bootdtrap是移动设备优先的**，针对移动设备的样式融合进了框架的每个角落，而不是增加一个额外的文件。
为了确保适当的绘制和触屏缩放，需要在 <code>head</code>标签 之中添加 viewport 元数据标签。
```
<meta name="viewport" content="width=device-width, initial-scale=1">
```

# 全局样式
传统前端开发过程中的问题：
- 重复，复杂，无意义的命名
- 结构冗余，胡乱嵌套
- 页面错乱

Bootstrap全局样式的特点：
- 代码整洁
- 风格统一
- 美观易用

## 栅格系统
通过一系列行（row）和列（column）的组合来创建页面布局。
**工作原理**
- **“行（row）”必须包含在 .container （固定宽度）或 .container-fluid （100% 宽度）中**，以便为其赋予合适的排列（aligment）和内补（padding）。
- 通过“行（row）”在水平方向创建一组“列（column）”。
- 你的内容应当放置于“列（column）”内，并且，**只有“列（column）”可以作为行（row）”的直接子元素**。
- 类似 <code> .row </code>和 <code>.col-xs-4</code> 这种预定义的类，可以用来快速创建栅格布局。
- 通过为“列（column）”设置 padding 属性，从而创建列与列之间的间隔（gutter）。通过为 <code>.row</code> 元素设置负值 margin 从而抵消掉为 <code>.container</code> 元素设置的 padding，也就间接为“行（row）”所包含的“列（column）”抵消掉了padding。
- 负值的 margin就是下面的示例为什么是向外突出的原因。在栅格列中的内容排成一行。
- 栅格系统中的列是通过指定1到12的值来表示其跨越的范围。例如，三个等宽的列可以使用三个 <code>.col-xs-4</code> 来创建。
- 如果一“行（row）”中包含了的“列（column）”大于 12，多余的“列（column）”所在的元素将被作为一个整体另起一行排列。
- 栅格类适用于与屏幕宽度大于或等于分界点大小的设备 ， 并且针对小屏幕设备覆盖栅格类。 因此，在元素上应用任何 <code>.col-md-* </code>栅格类适用于与屏幕宽度大于或等于分界点大小的设备 ， 并且针对小屏幕设备覆盖栅格类。 因此，在元素上应用任何<code> .col-lg-* </code>不存在， 也影响大屏幕设备。

```css
/* 超小屏幕（手机，小于 768px） */
/* 没有任何媒体查询相关的代码，因为这在 Bootstrap 中是默认的（还记得 Bootstrap 是移动设备优先的吗？） */
/* 小屏幕（平板，大于等于 768px） */
@media (min-width: @screen-sm-min) { ... }
/* 中等屏幕（桌面显示器，大于等于 992px） */
@media (min-width: @screen-md-min) { ... }
/* 大屏幕（大桌面显示器，大于等于 1200px） */
@media (min-width: @screen-lg-min) { ... }
```

**水平布局**
使用单一的一组 <code>.col-md-*</code> 栅格类，就可以创建一个基本的栅格系统，在手机和平板设备上一开始是堆叠在一起的（超小屏幕到小屏幕这一范围），在桌面（中等）屏幕设备上变为水平排列。所有“列（column）必须放在 ” <code>.row</code>内。
将最外面的布局元素 <code>.container </code>修改为 <code>.container-fluid</code>，就可以将固定宽度的栅格布局转换为 100% 宽度的布局。
```html
<div class="row">
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
  <div class="col-md-1">.col-md-1</div>
</div>
<div class="row">
  <div class="col-md-8">.col-md-8</div>
  <div class="col-md-4">.col-md-4</div>
</div>
<div class="row">
  <div class="col-md-4">.col-md-4</div>
  <div class="col-md-4">.col-md-4</div>
  <div class="col-md-4">.col-md-4</div>
</div>
<div class="row">
  <div class="col-md-6">.col-md-6</div>
  <div class="col-md-6">.col-md-6</div>
</div>
```
<img src="https://p1.meituan.net/dpnewvc/bbabd1e87128cc9b56c4fd4ce22e1e6e92121.jpg" width="1000px" height="330px">
如果希望在小屏幕设备上列也不要堆叠在一起，可以使用针对超小屏幕定义大类<code>.col-xs-*</code>和为中等屏幕定义的类<code>.col-md-*</code>，针对平板设备则是用<code>.col-sm-* </code>这个类。

如果在一个 .row 内包含的列（column）大于12个，包含多余列（column）的元素将作为一个整体单元被另起一行排列。

**列偏移**
使用 <code>.col-md-offset-*</code> 类可以将列向右侧偏移。这些类实际是通过使用 <code>* </code>选择器为当前元素增加了左侧的边距（margin）。例如，<code>.col-md-offset-4</code> 类将 <code>.col-md-4</code> 元素向右侧偏移了4个列（column）的宽度。
<img src="https://p1.meituan.net/dpnewvc/f034ae99767bcdd566e2987d9e4ff5fa50992.jpg" width="1000px" height="330px">

**嵌套列**
为了使用内置的栅格系统将内容再次嵌套，可以通过添加一个新的 <code>.row </code>元素和一系列 <code>.col-sm-* </code>元素到已经存在的 <code>.col-sm-*</code> 元素内。被嵌套的行（row）所包含的列（column）的个数不能超过12（其实，没有要求你必须占满12列）。
```html
<div class="row">
  <div class="col-sm-9">
    Level 1: .col-sm-9
    <div class="row">
      <div class="col-xs-8 col-sm-6">
        Level 2: .col-xs-8 .col-sm-6
      </div>
      <div class="col-xs-4 col-sm-6">
        Level 2: .col-xs-4 .col-sm-6
      </div>
    </div>
  </div>
</div>
```
<img src="https://p0.meituan.net/dpnewvc/fe9ca665624ded927a66a99dca54685c29071.jpg" width="1000px" height="330px">

**列排序**
通过使用 .col-md-push-* 和 .col-md-pull-* 类就可以很容易的改变列（column）的顺序。---应用不多

## 排版
**基本全局样式**
Bootstrap 排版、链接样式设置了基本的全局样式。分别是：
- 为 <code>body</code> 元素设置 <code>background-color: #fff</code>;
- 使用 <code>@font-family-base</code>、<code>@font-size-base</code> 和 <code>@line-height-base</code> 变量作为排版的基本参数;
- 为所有链接设置了基本颜色 <code>@link-color</code> ，并且当链接处于 <code>:hover </code>状态时才添加下划线。

**标题**
标题（h1~h6/.h1~.h6） h1:36px, h2:30px, h3:24px, h4:18px, h5:14px, h6: 12px
副标题（small）

在<code>body</code>标签中输入下面的代码：
```html
<h1>标题<small>小标题</small></h1>
<h2>标题</h2>
<h3>标题</h3>
<h4>标题</h4>
<h5>标题</h5>
<h6>标题</h6>
<span class="h1">标题</span>
<span class="h2">标题</span>
<span class="h3">标题</span>
<span class="h4">标题</span>
<span class="h5">标题</span>
<span class="h6">标题</span>
```
可以看到在span中也可以使用标题的字体。
<img src="https://p0.meituan.net/dpnewvc/2871abd0a5c74ff645f1ab29b50fdf3232431.png" width="1000px" height="300px">

**文本**
Bootstrap将全局的**字号设置为14px，行高为1.428**。并给段落（<code>P标签</code>）定义了初始的样式，**默认14px，行高20px，底部间距10px**。而普通网页中的段落字体大小默认为16px。
可以通过<code>mark</code>标签实现对文本的黄底显示，通过<code>del</code>标签实现删除线,通过<code>ins</code>或者<code>u</code>实现下划线效果。通过<code>small</code>文字变小，<code>strong</code>文字加粗，<code>em</code>实现斜体。。。这些都是html5中的标签，只不过Bootstrap对样式进行了封装。
另外，可以通过添加 <code>.lead</code> 类可以让段落突出显示。
```html
<p>测试一下段落<mark>打个马</mark><del>删除</del><ins>插入文本下划线效果</ins></p>
<p>测试一下段落</p>
```
效果图：
<img src="https://p0.meituan.net/dpnewvc/90f0f2ea4a06401b44b50bc8815910f914951.png" width="1000px" height="300px">

**对齐**
靠左对齐 <code>.text-left</code>
居中对齐 <code>.text-center</code>
靠右对齐 <code>.text-right</code>

**大小写**
大写转小写 <code>.text-lowercase</code>  
小写转大写 <code>.text-uppercase</code>  
首字母大写 <code>.text-capitalize</code>

**缩略语**
当鼠标悬停在缩写和缩写词上时就会显示完整内容，Bootstrap 实现了对 HTML 的 <abbr> 元素的增强样式。缩略语元素带有 title 属性，外观表现为带有较浅的虚线框，鼠标移至上面时会变成带有“问号”的指针。如想看完整的内容可把鼠标悬停在缩略语上（对使用辅助技术的用户也可见）, 但需要包含 title 属性。
为缩略语添加 .initialism 类，可以让 font-size 变得稍微小些。
```html
<abbr title="HyperText Markup Language" class="initialism">HTML</abbr>
```

**引用**
```html
<blockquote>
  <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer posuere erat a ante.</p>
  <footer>Someone famous in <cite title="Source Title">Source Title</cite></footer>
</blockquote>
```
<img src="https://p1.meituan.net/dpnewvc/c45688bfdb0a44ed1a7ce9601119dacc39299.jpg" width="1000px" height="300px">
通过赋予 .blockquote-reverse 类可以让引用呈现内容右对齐的效果。

**列表**
列表分为无序列表(ul)和有序liebiao(ol)两种，默认具有<code>list-style</code>样式，即子元素<code>li</code>为逐行展示,可以通过设置 <code>display: inline-block;</code> 并添加少量的内补（padding），将所有元素放置于同一行。
另外还有一种带有描述的短语列表，可以通过<code>dl</code>实现，其中<code>dt</code>为短语，<code>dd</code>为描述内容。
<img src="https://p0.meituan.net/dpnewvc/1882c9abdf6ebb97ac31ee2d8e6a5aef64282.jpg" width="1000px" height="300px">
<code>.dl-horizontal <code>可以让<code>dl</code> 内的短语及其描述排在一行。开始是像 <code>dl</code> 的默认样式堆叠在一起，随着导航条逐渐展开而排列在一行。
<img src="https://p1.meituan.net/dpnewvc/7d03f61fd206927687a0736d9b16ebf2105944.jpg" width="1000px" height="300px">

## 代码
**内联代码**
通过 <code>&lt;code&gt;</code> 标签包裹内联样式的代码片段。
**用户输入**
通过 <code>&lt;kbd&gt;</code>  标签标记用户通过键盘输入的内容。
**代码块**
多行代码可以使用 <code>&lt;pre&gt;</code> 标签。
还可以使用 .pre-scrollable 类，其作用是设置 max-height 为 350px ，并在垂直方向展示滚动条。
**变量**
通过 <code>&lt;var&gt;</code> 标签标记变量。
**程序输出**
通过 <code>&lt;samp&gt;</code> 标签来标记程序输出的内容。

## 表格
表格 <code>table</code>
带边框的表格 <code>.table-bordered</code>
条纹状表格 <code>.table-striped</code>
悬停变色 <code>.table-hover</code>
紧凑风格 <code>.table-condensed</code>

- .active	鼠标悬停在行或单元格上时所设置的颜色
- .success	标识成功或积极的动作
- .info	    标识普通的提示信息或动作
- .warning	标识警告或需要用户注意
- .danger	标识危险或潜在的带来负面影响的动作

**响应式**
将任何 .table 元素包裹在 .table-responsive 元素内，即可创建响应式表格，其会在小屏幕设备上（小于768px）水平滚动。当屏幕大于 768px 宽度时，水平滚动条消失。

## 表单
Bootstrap给HTML大部分表单都设置了默认样式，我们可以给表单添加相应类名，以实现表单的水平排列和个性化定制。
- 所有设置了 .form-control 类的 <code>&lt;input&gt;</code>、<code>&lt;textarea&gt;</code> 和 <code>&lt;select&gt;</code> 元素都将被默认设置宽度属性为 width: 100%;。 将 label 元素和前面提到的控件包裹在 .form-group 中可以获得最好的排列。
- 每个<code>&lt;form&gt;</code>标签为一套表单，每个.form-group类为一个表单组，包含一个标签合一个表单元素。
```html
<form>
  <div class="form-g
roup">
    <label for="exampleInputEmail1">Email address</label>
    <input type="email" class="form-control" id="exampleInputEmail1" placeholder="Email">
  </div>
  <div class="form-group">
    <label for="exampleInputPassword1">Password</label>
    <input type="password" class="form-control" id="exampleInputPassword1" placeholder="Password">
  </div>
  <div class="form-group">
    <label for="exampleInputFile">File input</label>
    <input type="file" id="exampleInputFile">
    <p class="help-block">Example block-level help text here.</p>
  </div>
  <div class="checkbox">
    <label>
      <input type="checkbox"> Check me out
    </label>
  </div>
  <button type="submit" class="btn btn-default">Submit</button>
</form>
```
<img src="https://p0.meituan.net/dpnewvc/1fb77f141ca9206130b7791f57b1351e41332.png" width="1000px" height="300px">
- 为 <code>&lt;form&gt;</code> 元素添加 .form-inline 类可使其内容左对齐并且表现为 inline-block 级别的控件。只适用于视口（viewport）至少在 768px 宽度时（视口宽度再小的话就会使表单折叠）。
```html
<form class="form-inline">
  <div class="form-group">
    <label for="exampleInputName2">Name</label>
    <input type="text" class="form-control" id="exampleInputName2" placeholder="Jane Doe">
  </div>
  <div class="form-group">
    <label for="exampleInputEmail2">Email</label>
    <input type="email" class="form-control" id="exampleInputEmail2" placeholder="jane.doe@example.com">
  </div>
  <button type="submit" class="btn btn-default">Send invitation</button>
</form>
```
- 如果你没有为每个输入控件设置 label 标签，屏幕阅读器将无法正确识别。对于这些内联表单，你可以通过为 label 设置 .sr-only 类将其隐藏。还有一些辅助技术提供label标签的替代方案，比如 aria-label、aria-labelledby 或 title 属性。如果这些都不存在，屏幕阅读器可能会采取使用 placeholder 属性，如果存在的话，使用占位符来替代其他的标记，但要注意，这种方法是不妥当的。
```html
<form class="form-inline">
  <div class="form-group">
    <label class="sr-only" for="exampleInputEmail3">Email address</label>
    <input type="email" class="form-control" id="exampleInputEmail3" placeholder="Email">
  </div>
  <div class="form-group">
    <label class="sr-only" for="exampleInputPassword3">Password</label>
    <input type="password" class="form-control" id="exampleInputPassword3" placeholder="Password">
  </div>
  <div class="checkbox">
    <label>
      <input type="checkbox"> Remember me
    </label>
  </div>
  <button type="submit" class="btn btn-default">Sign in</button>
</form>
```
- 通过为表单添加 .form-horizontal 类，并联合使用 Bootstrap 预置的栅格类，可以将 label 标签和控件组水平并排布局。这样做将改变 .form-group 的行为，使其表现为栅格系统中的行（row），因此就无需再额外添加 .row 了。
```html
<form class="form-horizontal">
  <div class="form-group">
    <label for="inputEmail3" class="col-sm-2 control-label">Email</label>
    <div class="col-sm-10">
      <input type="email" class="form-control" id="inputEmail3" placeholder="Email">
    </div>
  </div>
  <div class="form-group">
    <label for="inputPassword3" class="col-sm-2 control-label">Password</label>
    <div class="col-sm-10">
      <input type="password" class="form-control" id="inputPassword3" placeholder="Password">
    </div>
  </div>
  <div class="form-group">
    <div class="col-sm-offset-2 col-sm-10">
      <div class="checkbox">
        <label>
          <input type="checkbox"> Remember me
        </label>
      </div>
    </div>
  </div>
  <div class="form-group">
    <div class="col-sm-offset-2 col-sm-10">
      <button type="submit" class="btn btn-default">Sign in</button>
    </div>
  </div>
</form>
```

**支持的控件**
1.输入框--input
包括大部分表单控件、文本输入域控件，还支持所有 HTML5 类型的输入控件： text、password、datetime、datetime-local、date、month、time、week、number、email、url、search、tel 和 color。
>只有正确设置了 type 属性的输入控件才能被赋予正确的样式。

2.文本域--textarea
支持多行文本的表单控件。可根据需要改变 rows 属性。

3.单选和多选
单选框和多选框的默认外观是纵向堆叠在一起的。
通过将 <code>.checkbox-inline</code> 或 <code>.radio-inline</code> 类应用到一系列的多选框（checkbox）或单选框（radio）控件上，可以使这些控件排列在一行。
```html
<label class="checkbox-inline">
  <input type="checkbox" id="inlineCheckbox1" value="option1"> 1
</label>
<label class="checkbox-inline">
  <input type="checkbox" id="inlineCheckbox2" value="option2"> 2
</label>
<label class="checkbox-inline">
  <input type="checkbox" id="inlineCheckbox3" value="option3"> 3
</label>

<label class="radio-inline">
  <input type="radio" name="inlineRadioOptions" id="inlineRadio1" value="option1"> 1
</label>
<label class="radio-inline">
  <input type="radio" name="inlineRadioOptions" id="inlineRadio2" value="option2"> 2
</label>
<label class="radio-inline">
  <input type="radio" name="inlineRadioOptions" id="inlineRadio3" value="option3"> 3
</label>
```
<img src="https://p1.meituan.net/dpnewvc/7657853e404ba1b871582cdce1054c208624.jpg" width="1000px" height="300px">
如果需要 <label> 内没有文字，输入框（input）正是你所期望的。 目前只适用于非内联的 checkbox 和 radio。 请记住，仍然需要为使用辅助技术的用户提供某种形式的 label（例如，使用 aria-label）。<code>aria-label="..."</code>

4.下拉列表
对于标记了 multiple 属性的 <code>&lt;select&gt;</code> 控件来说，默认显示多选项。

**静态控件**
如果需要在表单中将一行纯文本和 <code>&lt;label&gt;</code>  元素放置于同一行，为 <code>&lt;p&gt;</code>  元素添加 <code>.form-control-static</code> 类即可。
```html
<form class="form-horizontal">
  <div class="form-group">
    <label class="col-sm-2 control-label">Email</label>
    <div class="col-sm-10">
      <p class="form-control-static">email@example.com</p>
    </div>
  </div>
</form>
```
表单控件后面友好的帮助信息可以在<code>&lt;span&gt;</code>标签中增加<code>.help-block</code>类名。

**校验状态**
Bootstrap 对表单控件的校验状态，如 error、warning 和 success 状态，都定义了样式。使用时，添加 <code>.has-warning</code>、<code>.has-error</code> 或 <code>.has-success</code> 类到这些控件的父元素即可。任何包含在此元素之内的 <code>.control-label</code>、<code>.form-control </code>和 <code>.help-block</code> 元素都将接受这些校验状态的样式。

**在表单输入区域内添加额外的图标**
你还可以针对校验状态为输入框添加额外的图标。只需设置相应的 <code>.has-feedback</code> 类并添加正确的图标即可。
```html
<div class="form-group has-success has-feedback">
    <label class="control-label" for="inputSuccess2">Input with success</label>
    <input type="text" class="form-control" id="inputSuccess2" aria-describedby="inputSuccess2Status">
    <span class="glyphicon glyphicon-ok form-control-feedback" aria-hidden="true"></span>
    <span id="inputSuccess2Status" class="sr-only">(success)</span>
    </div>
<div class="form-group has-warning has-feedback">
    <label class="control-label" for="inputWarning2">Input with warning</label>
    <input type="text" class="form-control" id="inputWarning2" aria-describedby="inputWarning2Status">
    <span class="glyphicon glyphicon-warning-sign form-control-feedback" aria-hidden="true"></span>
    <span id="inputWarning2Status" class="sr-only">(warning)</span>
</div>
<div class="form-group has-error has-feedback">
    <label class="control-label" for="inputError2">Input with error</label>
    <input type="text" class="form-control" id="inputError2" aria-describedby="inputError2Status">
    <span class="glyphicon glyphicon-remove form-control-feedback" aria-hidden="true"></span>
    <span id="inputError2Status" class="sr-only">(error)</span>
</div>
<div class="form-group has-success has-feedback">
    <label class="control-label" for="inputGroupSuccess1">Input group with success</label>
    <div class="input-group">
        <span class="input-group-addon">@</span>
        <input type="text" class="form-control" id="inputGroupSuccess1" aria-describedby="inputGroupSuccess1Status">
    </div>
    <span class="glyphicon glyphicon-ok form-control-feedback" aria-hidden="true"></span>
    <span id="inputGroupSuccess1Status" class="sr-only">(success)</span>
</div>
```
<img src="https://p0.meituan.net/dpnewvc/0ca9c1035e4766088e3f4dc8b272fcdd80194.jpg" width="1000px" height="300px">

**控件尺寸**
通过 <code>.input-lg</code> 类似的类可以为控件设置高度，通过 <code>.col-lg-*</code> 类似的类可以为控件设置宽度。
大号 <code>.input-lg</code>
小号 <code>.input-sm</code>
通过添加 <code>.form-group-lg</code> 或 <code>.form-group-sm </code>类，为 <code>.form-horizontal</code> 包裹的 label 元素和表单控件快速设置尺寸
用栅格系统中的列（column）包裹输入框或其任何父元素，都可很容易的为其设置宽度。<code>.col-xs-*</code>

**按钮**
```html
<button type="submit" class="btn btn-default">Submit</button>
<button type="submit" class="btn btn-success">Submit</button>
<button type="submit" class="btn btn-warning">Submit</button>
<button type="submit" class="btn btn-primary">Submit</button>
<button type="submit" class="btn btn-info">Submit</button>
<button type="submit" class="btn btn-danger">Submit</button>
<button type="submit" class="btn btn-link">Submit</button>
```
<img src="https://p0.meituan.net/dpnewvc/3ba18629d1d8ddff5af049ea23af943b14120.png" width="1000px" height="300px">
需要让按钮具有不同尺寸吗？使用 .btn-lg、.btn-sm 或 .btn-xs 就可以获得不同尺寸的按钮。
通过给按钮添加 .btn-block 类可以将其拉伸至父元素100%的宽度，而且按钮也变为了块级（block）元素。

## 响应式
**图片**
在 Bootstrap 版本 3 中，通过为图片添加 <code>.img-responsive</code> 类可以让图片支持响应式布局。如果需要让使用了 <code>.img-responsive</code> 类的图片水平居中，请使用 <code>.center-block</code> 类。
**修改图片形状**
```html
<img src="..." alt="..." class="img-rounded">
<img src="..." alt="..." class="img-circle">
<img src="..." alt="..." class="img-thumbnail">
```
**可用的类**
<img src="https://p0.meituan.net/dpnewvc/463f17aaedefdc98beceaa66cd814cde143534.jpg" width="1000px" height="300px">
```
.visible-*-block
.visible-*-inline
.visible-*-inline-block
```

## 辅助类
**文本颜色**
```html
<p class="text-muted">...</p>
<p class="text-primary">...</p>
<p class="text-success">...</p>
<p class="text-info">...</p>
<p class="text-warning">...</p>
<p class="text-danger">...</p>
```
**背景色**
```html
<p class="bg-primary">...</p>
<p class="bg-success">...</p>
<p class="bg-info">...</p>
<p class="bg-warning">...</p>
<p class="bg-danger">...</p>
```
**关闭模态框和警告框**
```html
<button type="button" class="close" aria-label="Close"><span aria-hidden="true">&times;</span></button>
```
**三角下拉图标**
```html
<span class="caret"></span>
```
**快速浮动**
```html
<div class="pull-left">...</div>
<div class="pull-right">...</div>
```
**让内容块居中**
```html
<div class="center-block">...</div>
```
**清除浮动**
```html
<div class="clearfix">...</div>
```
**显示或隐藏内容**
```html
<div class="show">...</div>
<div class="hidden">...</div>
```

# 组件
## 下拉菜单
将下拉菜单触发器和下拉菜单都包裹在 <code>.dropdown</code> 里，或者另一个声明了 <code>position: relative;</code> 的元素。然后加入组成菜单的 HTML 代码。
通过为下拉菜单的父元素设置 <code>.dropup</code> 类，可以让菜单向上弹出（默认是向下弹出的）。
```html
<div class="dropdown">
  <button class="btn btn-default dropdown-toggle" type="button" id="dropdownMenu1" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    Dropdown
    <span class="caret"></span>
  </button>
  <ul class="dropdown-menu" aria-labelledby="dropdownMenu1">
    <li><a href="#">Action</a></li>
    <li><a href="#">Another action</a></li>
    <li><a href="#">Something else here</a></li>
    <li role="separator" class="divider"></li><!--分割线 -->
    <li><a href="#">Separated link</a></li>
  </ul>
</div>
```
<img src="https://p1.meituan.net/dpnewvc/e6573589543dc67bc355a4ce0b1def3b24537.jpg" width="1000px" height="300px">
为<code> .dropdown-menu</code> 添加 <code>.dropdown-menu-right</code> 类可以让菜单右对齐。
```html
<ul class="dropdown-menu dropdown-menu-right" aria-labelledby="dLabel">
  ...
</ul>
```
为下拉菜单中的 <code>li</code> 元素添加 .disabled 类，可以禁用相应的菜单项。

## 按钮组
通过按钮组容器<code>.btn-group</code>把一组按钮放在同一行里。
让一组按钮垂直堆叠排列显示而不是水平排列<code>.btn-group-vertical</code>
让一组按钮拉长为相同的尺寸，填满父元素的宽度<code>.btn-group-justified</code>
把一组 <code>.btn-group</code> 组合进一个 <code>.btn-toolbar</code> 中就可以做成更复杂的组件。
```html
<div class="btn-toolbar" role="toolbar" aria-label="...">
  <div class="btn-group" role="group" aria-label="...">
    <button type="button" class="btn btn-default">Left</button>
    <button type="button" class="btn btn-default">Middle</button>
    <button type="button" class="btn btn-default">Right</button>
  </div>
  <div class="btn-group" role="group" aria-label="...">...</div>
  <div class="btn-group" role="group" aria-label="...">...</div>
</div>
```
<img src="https://p0.meituan.net/dpnewvc/fef193c1536ac3b0a913ec3416de1bb210347.jpg" width="1000px" height="300px">
按钮组修改尺寸：
```html
<div class="btn-group btn-group-lg" role="group" aria-label="...">...</div>
<div class="btn-group" role="group" aria-label="...">...</div>
<div class="btn-group btn-group-sm" role="group" aria-label="...">...</div>
<div class="btn-group btn-group-xs" role="group" aria-label="...">...</div>
```

想要把下拉菜单混合到一系列按钮中，只须把 .btn-group 放入另一个 .btn-group 中。
```html
<div class="btn-group" role="group" aria-label="...">
  <button type="button" class="btn btn-default">1</button>
  <button type="button" class="btn btn-default">2</button>

  <div class="btn-group" role="group">
    <button type="button" class="btn btn-default dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
      Dropdown
      <span class="caret"></span>
    </button>
    <ul class="dropdown-menu">
      <li><a href="#">Dropdown link</a></li>
      <li><a href="#">Dropdown link</a></li>
    </ul>
  </div>
</div>
```

## 按钮式下拉菜单
**单按钮下拉菜单**
```html
<!-- Single button -->
<div class="btn-group">
  <button type="button" class="btn btn-success dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    Action <span class="caret"></span>
  </button>
  <ul class="dropdown-menu">
    <li><a href="#">Action</a></li>
    <li><a href="#">Another action</a></li>
    <li><a href="#">Something else here</a></li>
    <li role="separator" class="divider"></li>
    <li><a href="#">Separated link</a></li>
  </ul>
</div>
```
<img src="https://p0.meituan.net/dpnewvc/32b89b485f7506f507d179a440476e6f27455.jpg" width="1000px" height="300px">
**分裂式按钮下拉菜单**
```html
<!-- Split button -->
<div class="btn-group">
  <button type="button" class="btn btn-danger">Action</button>
  <button type="button" class="btn btn-danger dropdown-toggle" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
    <span class="caret"></span>
    <span class="sr-only">Toggle Dropdown</span>
  </button>
  <ul class="dropdown-menu">
    <li><a href="#">Action</a></li>
    <li><a href="#">Another action</a></li>
    <li><a href="#">Something else here</a></li>
    <li role="separator" class="divider"></li>
    <li><a href="#">Separated link</a></li>
  </ul>
</div>
```
<img src="https://p0.meituan.net/dpnewvc/e36d156025e0fa26674472a078aae91428882.jpg" width="1000px" height="300px">
给父元素添加 .dropup 类就能使触发的下拉菜单朝上方打开。
**尺寸**
```css
.btn-lg
.btn-sm
.btn-xs
```

## 输入框组
通过在文本输入框 <code>input</code> 前面、后面或是两边加上文字或按钮，可以实现对表单控件的扩展。为 <code>.input-group</code> 赋予 <code>.input-group-addon</code> 或 <code>.input-group-btn</code> 类，可以给 <code>.form-control</code> 的前面或后面添加额外的元素。
可以将多选框，单选框，按钮，按钮式下拉菜单或分裂式按钮下拉菜单作为额外元素添加到输入框组中，使用<code>.input-group-btn</code>类包装。
```html
<div class="input-group">
  <span class="input-group-addon">$</span>
  <input type="text" class="form-control" aria-label="Amount (to the nearest dollar)">
  <span class="input-group-addon">.00</span>
</div>

<label for="basic-url">Your vanity URL</label>
<div class="input-group">
  <span class="input-group-addon" id="basic-addon3">https://example.com/users/</span>
  <input type="text" class="form-control" id="basic-url" aria-describedby="basic-addon3">
</div>
```
**尺寸**
```css
.input-group-lg
.input-group-sm
.input-group-xs
```

## 导航
Bootstrap 中的导航组件都依赖同一个 <code>.nav </code> 类，状态类也是共用的。
Tab标签页式导航： <code>.nav-tabs</code>
```html
<ul class="nav nav-tabs">
  <li role="presentation" class="active"><a href="#">Home</a></li>
  <li role="presentation"><a href="#">Profile</a></li>
  <li role="presentation"><a href="#">Messages</a></li>
</ul>
```
胶囊式标签页导航： <code>.nav-pills</code>，垂直堆叠只需添加 <code>.nav-stacked</code> 类
```html
<ul class="nav nav-pills">
  <li role="presentation" class="active"><a href="#">Home</a></li>
  <li role="presentation"><a href="#">Profile</a></li>
  <li role="presentation"><a href="#">Messages</a></li>
</ul>
```
在大于 768px 的屏幕上，通过 <code>.nav-justified</code> 类可以很容易的让标签页或胶囊式标签呈现出同等宽度。在小屏幕上，导航链接呈现堆叠样式。
对任何导航组件（标签页、胶囊式标签页），都可以添加 <code>.disabled</code> 类，从而实现链接为灰色且没有鼠标悬停效果。

同样的，导航效果也可以添加下拉菜单，可以参照按钮下拉。

## 导航条
导航条是在您的应用或网站中作为导航页头的响应式基础组件。它们在移动设备上可以折叠（并且可开可关），且在视口（viewport）宽度增加时逐渐变为水平展开模式。
```html
<nav class="navbar navbar-default">
  <div class="container-fluid">
    <!--品牌图标-->
    <div class="navbar-header">
      <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-example-navbar-collapse-1" aria-expanded="false">
        <span class="sr-only">Toggle navigation</span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
        <span class="icon-bar"></span>
      </button>
      <a class="navbar-brand" href="#">Brand</a>
    </div>

    <!-- Collect the nav links, forms, and other content for toggling -->
    <div class="collapse navbar-collapse" id="bs-example-navbar-collapse-1">
      <ul class="nav navbar-nav">
        <li class="active"><a href="#">Link <span class="sr-only">(current)</span></a></li>
        <li><a href="#">Link</a></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Dropdown <span class="caret"></span></a>
          <ul class="dropdown-menu">
            <li><a href="#">Action</a></li>
            <li><a href="#">Another action</a></li>
            <li><a href="#">Something else here</a></li>
            <li role="separator" class="divider"></li>
            <li><a href="#">Separated link</a></li>
            <li role="separator" class="divider"></li>
            <li><a href="#">One more separated link</a></li>
          </ul>
        </li>
      </ul>
      <!--表单-->
      <form class="navbar-form navbar-left">
        <div class="form-group">
          <input type="text" class="form-control" placeholder="Search">
        </div>
        <button type="submit" class="btn btn-default">Submit</button>
      </form>
      <ul class="nav navbar-nav navbar-right">
        <li><a href="#">Link</a></li>
        <li class="dropdown">
          <a href="#" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">Dropdown <span class="caret"></span></a>
          <ul class="dropdown-menu">
            <li><a href="#">Action</a></li>
            <li><a href="#">Another action</a></li>
            <li><a href="#">Something else here</a></li>
            <li role="separator" class="divider"></li>
            <li><a href="#">Separated link</a></li>
          </ul>
        </li>
      </ul>
    </div><!-- /.navbar-collapse -->
  </div><!-- /.container-fluid -->
</nav>
```
<img src="https://p1.meituan.net/dpnewvc/aa2737fc5d798653898f0468db9421b464104.jpg" width="1000px" height="300px">

**品牌图标**
将导航条内放置品牌标志的地方替换为 <code>img</code> 元素即可展示自己的品牌图标。由于 .navbar-brand 已经被设置了内补（padding）和高度（height），你需要根据自己的情况添加一些 CSS 代码从而覆盖默认设置。

**表单**
将表单放置于 <code>.navbar-form</code> 之内可以呈现很好的垂直对齐，并在较窄的视口（viewport）中呈现折叠状态。 使用对齐选项可以规定其在导航条上出现的位置。

**按钮**
对于不包含在 <code>form</code> 中的 <code>button</code> 元素，加上 <code>.navbar-btn</code> 后，可以让它在导航条里垂直居中。

**文本**
把文本包裹在 <code>.navbar-text</code> 中时，为了有正确的行距和颜色，通常使用 <code>p</code> 标签。
```html
<p class="navbar-text">Signed in as Mark Otto</p>
```

**非导航链接**
或许你希望在标准的导航组件之外添加标准链接，那么，使用 <code>.navbar-link</code> 类可以让链接有正确的默认颜色和反色设置。
```html
<p class="navbar-text navbar-right">Signed in as <a href="#" class="navbar-link">Mark Otto</a></p>
```

**左右浮动**
通过添加 <code>.navbar-left</code> 和 <code>.navbar-right</code> 工具类让导航链接、表单、按钮或文本对齐。两个类都会通过 CSS 设置特定方向的浮动样式。

**固定在顶部**
添加 <code>.navbar-fixed-top</code> 类可以让导航条固定在顶部，还可包含一个 <code>.container </code>或 <code>.container-fluid</code> 容器，从而让导航条居中，并在两侧添加内补（padding）。

**固定在底部**
添加 <code>.navbar-fixed-bottom</code> 类可以让导航条固定在底部，并且还可以包含一个 <code>.container</code> 或 <code>.container-fluid</code> 容器，从而让导航条居中，并在两侧添加内补（padding）。

**静止在顶部**
通过添加 <code>.navbar-static-top</code> 类即可创建一个与页面等宽度的导航条，它会随着页面向下滚动而消失。还可以包含一个 <code>.container</code> 或 <code>.container-fluid</code> 容器，用于将导航条居中对齐并在两侧添加内补（padding）。

**改变导航条颜色**
通过添加 <code>.navbar-inverse</code> 类可以改变导航条的外观。

## 路径导航(面包屑)
在一个带有层次的导航结构中标明当前页面的位置。各路径间的分隔符已经自动通过 CSS 的 <code>:before</code> 和 <code>content</code> 属性添加了。
```html
<ol class="breadcrumb">
  <li><a href="#">Home</a></li>
  <li><a href="#">Library</a></li>
  <li class="active">Data</li>
</ol>
```

## 分页
```html
<nav aria-label="Page navigation">
  <ul class="pagination">
    <li>
      <a href="#" aria-label="Previous">
        <span aria-hidden="true">&laquo;</span>
      </a>
    </li>
    <li><a href="#">1</a></li>
    <li><a href="#">2</a></li>
    <li><a href="#">3</a></li>
    <li><a href="#">4</a></li>
    <li><a href="#">5</a></li>
    <li>
      <a href="#" aria-label="Next">
        <span aria-hidden="true">&raquo;</span>
      </a>
    </li>
  </ul>
</nav>
```
<img src="https://p1.meituan.net/dpnewvc/dd5cbe5d4fe11fcb8486cfda34dfa0e17628.jpg" width="1000px" height="300px">
可以给不能点击的链接添加 <code>.disabled</code> 类、给当前页添加 <code>.active</code> 类。
<code>.pagination-lg</code> 或 <code>.pagination-sm</code> 类提供了额外可供选择的尺寸，添加到 <code>ul</code> 标签。

**上一页和下一页的简单翻页**
```html
<nav aria-label="...">
  <ul class="pager">
    <li><a href="#">Previous</a></li>
    <li><a href="#">Next</a></li>
  </ul>
</nav>
```

**两端对齐**
```html
<nav aria-label="...">
  <ul class="pager">
    <li class="previous"><a href="#"><span aria-hidden="true">&larr;</span> Older</a></li>
    <li class="next"><a href="#">Newer <span aria-hidden="true">&rarr;</span></a></li>
  </ul>
</nav>
```

## 标签
```html
<span class="label label-default">Default</span>
<span class="label label-primary">Primary</span>
<span class="label label-success">Success</span>
<span class="label label-info">Info</span>
<span class="label label-warning">Warning</span>
<span class="label label-danger">Danger</span>
```
<img src="https://p1.meituan.net/dpnewvc/83a138b7b8884b0b26f575a08207373a17133.jpg" width="1000px" height="300px">

## 数量标记
给链接、导航等元素嵌套 <code>&lt;span class="badge"&gt;</code> 元素，可以很醒目的展示新的或未读的信息条目。
```html
<a href="#">Inbox <span class="badge">42</span></a>
<button class="btn btn-primary" type="button">
  Messages <span class="badge">4</span>
</button>
```

## 巨幕
```html
<div class="jumbotron">
  <h1>Hello, world!</h1>
  <p>bibibibibibibibibi</p>
  <p><a class="btn btn-primary btn-lg" href="#" role="button">Learn more</a></p>
</div>
```

## 缩略图
```html
<div class="row">
  <div class="col-sm-6 col-md-4">
    <div class="thumbnail">
      <img src="..." alt="...">
      <div class="caption">
        <h3>Thumbnail label</h3>
        <p>...</p>
        <p><a href="#" class="btn btn-primary" role="button">Button</a> <a href="#" class="btn btn-default" role="button">Button</a></p>
      </div>
    </div>
  </div>
</div>
```

## 警告框
```html
<div class="alert alert-success" role="alert">...</div>
<div class="alert alert-info" role="alert">...</div>
<div class="alert alert-warning" role="alert">...</div>
<div class="alert alert-danger" role="alert">...</div>
```
**为警告框添加一个可选的 .alert-dismissible 类和一个关闭按钮。**
```html
<div class="alert alert-warning alert-dismissible" role="alert">
  <button type="button" class="close" data-dismiss="alert" aria-label="Close"><span aria-hidden="true">&times;</span></button>
  <strong>Warning!</strong> Better check yourself, you're not looking too good.
</div>
```
**警告中的链接**
```html
<a href="#" class="alert-link">...</a>
```

## 进度条
```html
<div class="progress">
  <div class="progress-bar" role="progressbar" aria-valuenow="60" aria-valuemin="0" aria-valuemax="100" style="width: 60%;">
    60%
  </div>
</div>
```
在展示很低的百分比时，如果需要让文本提示能够清晰可见，可以为进度条设置 min-width 属性。
<img src="https://p1.meituan.net/dpnewvc/475af00b5fc7ff702374fc05dc48c6d019512.jpg" width="1000px" height="300px">
要展示不同效果都进度条
```
<div class="progress-bar progress-bar-success"
<div class="progress-bar progress-bar-info"
<div class="progress-bar progress-bar-warning"
<div class="progress-bar progress-bar-danger"
```
条纹效果类<code>.progress-bar-striped</code>
为 <code>.progress-bar-striped</code> 添加 <code>.active</code> 类，使其呈现出由右向左运动的动画效果.
把多个进度条放入同一个 .progress 中，使它们呈现堆叠的效果。

## 媒体对象
```html
<div class="media">
  <div class="media-left media-middle">
    <a href="#">
      <img class="media-object" src="..." alt="...">
    </a>
  </div>
  <div class="media-body">
    <h4 class="media-heading">Middle aligned media</h4>
    Cras sit amet nibh libero, in gravida nulla. Nulla vel metus scelerisque ante sollicitudin commodo. Cras purus odio, vestibulum in vulputate at, tempus viverra turpis. Fusce condimentum nunc ac nisi vulputate fringilla. Donec lacinia congue felis in faucibus.
  </div>
</div>
<div class="media">
  <div class="media-left media-middle">
    <a href="#">
      <img class="media-object" src="..." alt="...">
    </a>
  </div>
  <div class="media-body">
    <h4 class="media-heading">Middle aligned media</h4>
    Cras sit amet nibh libero, in gravida nulla. Nulla vel metus scelerisque ante sollicitudin commodo. Cras purus odio, vestibulum in vulputate at, tempus viverra turpis. Fusce condimentum nunc ac nisi vulputate fringilla. Donec lacinia congue felis in faucibus.
    <div class="media">
          <div class="media-left media-middle">
            <a href="#">
              <img class="media-object" src="..." alt="...">
            </a>
          </div>
          <div class="media-body">
            <h4 class="media-heading">Middle aligned media</h4>
            Cras sit amet nibh libero, in gravida nulla. Nulla vel metus scelerisque ante sollicitudin commodo. Cras purus odio, vestibulum in vulputate at, tempus viverra turpis. Fusce condimentum nunc ac nisi vulputate fringilla. Donec lacinia congue felis in faucibus.
          </div>
        </div>
  </div>
</div>
```
<img src="https://p1.meituan.net/dpnewvc/eeec5d411b188d5feadf15f3ada50e7d282860.jpg" width="1000px" height="300px">

**对齐**
```
media-top
media-middle
media-bottom
```

## 列表组 
```html
<ul class="list-group">
  <li class="list-group-item list-group-item-success">Dapibus ac facilisis in</li>
  <li class="list-group-item list-group-item-info">Cras sit amet nibh libero</li>
  <li class="list-group-item list-group-item-warning">Porta ac consectetur ac</li>
  <li class="list-group-item list-group-item-danger">Vestibulum at eros</li>
</ul>
<div class="list-group">
  <a href="#" class="list-group-item list-group-item-success">Dapibus ac facilisis in</a>
  <a href="#" class="list-group-item list-group-item-info">Cras sit amet nibh libero</a>
  <a href="#" class="list-group-item list-group-item-warning">Porta ac consectetur ac</a>
  <a href="#" class="list-group-item list-group-item-danger">Vestibulum at eros</a>
</div>
```

## 面板
```html
<div class="panel panel-default">
  <div class="panel-heading">
    <h3 class="panel-title">Panel title</h3>
  </div>
  <div class="panel-body">
    Panel content
  </div>
  <div class="panel-footer">Panel footer</div>
</div>
<!--情景效果-->
<div class="panel panel-primary">...</div>
<div class="panel panel-success">...</div>
<div class="panel panel-info">...</div>
<div class="panel panel-warning">...</div>
<div class="panel panel-danger">...</div>
```

## Well效果
把 Well 用在元素上，就能有嵌入（inset）的简单效果。
```html
<div class="well">...</div>
```