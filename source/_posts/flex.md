title: Flex实现五种常用布局
date: 2018-01-09
categories: Node
tags:
- css
- flex
- 布局
- Layout

---

[原文链接](https://github.com/meikidd/flex-layout) 

<!-- more -->

## 经典上-中-下布局
当页面内容高度小于可视区域高度时，footer 吸附在底部；当页面内容高度大于可视区域高度时，footer 被撑开排在 content 下方。
<img src="https://p0.meituan.net/dpnewvc/096980425eb89f986991f866639fea4543883.png" width="800px" height="350px">
```html
<body>
  <header>HEADER</header>
  <article>CONTENT</article>
  <footer>FOOTER</footer>
</body>
```
```css
body {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}
article {
  flex: auto;
}
```

## 在上-中-下布局的基础上，加了左侧定宽 sidebar
<img src="https://p1.meituan.net/dpnewvc/d1755ca8e911e19045f232bede34216946140.png" width="800px" height="350px">
```html
<body>
  <header>HEADER</header>
  <div class="content">
    <aside>ASIDE</aside>
    <article>CONTENT</article>
  </div>
  <footer>FOOTER</footer>
</body>
```
```css
body {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
  }
  .content {
    flex: auto;
    display: flex;
  }
  .content article {
    flex: auto;
  }
  .content aside {
    flex: none;
    width: 200px;
  } 
```

## 左边是定宽 sidebar，右边是上-中-下布局。
<img src="https://p0.meituan.net/dpnewvc/e04fc60927afab215d7d2d38570dbaa645908.png" width="800px" height="350px">
```html
<body>
  <aside>ASIDE</aside>
  <div class="content">
    <header>HEADER</header>
    <article>CONTENT</article>
    <footer>FOOTER</footer>
  </div>
</body>
```
```css
body {
    min-height: 100vh;
    display: flex;
  }
  aside {
    flex: none;
    width: 200px;
  }
  .content {
    flex: auto;
    display: flex;
    flex-direction: column;
  }
  .content article {
    flex: auto;
  }
```

## 还是上-中-下布局，区别是 header 固定在顶部，不会随着页面滚动。
<img src="https://p0.meituan.net/dpnewvc/fa4df59463d759605a1cf0ebeb04927e40963.png" width="800px" height="350px">
```html
<body>
  <header>HEADER</header>
  <article>CONTENT</article>
  <footer>FOOTER</footer>
</body>
```
```css
body {
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    padding-top: 60px;
  }
  header {
    height: 60px;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    padding: 0;
  }
  article {
    flex: auto;
    height: 1000px;
  }
```

## 左侧 sidebar 固定在左侧且与视窗同高，当内容超出视窗高度时，在 sidebar 内部出现滚动条。左右两侧滚动条互相独立。
<img src="https://p1.meituan.net/dpnewvc/c0bceb861d99c29b4eba816b755eabcc71777.png" width="800px" height="350px">
```html
<body>
  <aside>
    ASIDE
    <p>item</p>
    <p>item</p>
    <!-- many items -->
    <p>item</p>
  </aside>
  <div class="content">
    <header>HEADER</header>
    <article>CONTENT</article>
    <footer>FOOTER</footer>
  </div>
</body>
```
```css
body {
  height: 100vh;
  display: flex;
}
aside {
  flex: none;
  width: 200px;
  overflow-y: auto;
  display: block;
}
.content {
  flex: auto;
  display: flex;
  flex-direction: column;
  overflow-y: auto;
}
.content article {
  flex: auto;
}
```