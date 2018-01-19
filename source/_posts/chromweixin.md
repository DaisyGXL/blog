title: Chrome在电脑上模拟微信浏览器
date: 2018-01-16
categories: chrome
tags:
- chrome
- 调试

---

最近做了微信商城的开发，涉及到在微信内部的页面跳转问题，用此方法调试。

<!-- more -->

1、先了解安卓微信和Ios微信的UA（User agent：用户代理）

安卓微信UA： mozilla/5.0 (linux; u; android 4.1.2; zh-cn; mi-one plus build/jzo54k) applewebkit/534.30 (khtml, like gecko) version/4.0 mobile safari/534.30 micromessenger/5.0.1.352  

2、打开Chrome，F12打开开发人员工具，点击菜单按钮-----More Tools -----Network condition打开Network condition窗口

3、 User agent选项，选择Custom（自定义），然后在下面的文本框中输入Android或者Ios的UA就可以了

4、测试一下成果如何
```
<script type="text/javascript">
window.onload = function() {
    isWeixinBrowser();
}
//判断是否微信浏览器
function isWeixinBrowser() {  
    var ua = navigator.userAgent.toLowerCase();  
    var result = (/micromessenger/.test(ua)) ? true : false;
    if (result) {
        console.log('你正在访问微信浏览器');
    }
    else {
        console.log('你访问的不是微信浏览器');
    }
    return result;
}  
</script>
```