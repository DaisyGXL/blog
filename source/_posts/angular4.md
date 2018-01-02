title: Angular4
date: 2017-11-02
categories: angular
summary_img: https://p1.meituan.net/dpnewvc/098e2f8a7219a97d7fbda8f16be45cf785101.png
tags:
- javascript
- angular

---

本文主要介绍了angular4的用法和新特性。

<!-- more -->
# Angular 与 AngularJS 有什么区别
- 不再有Controller和 Scope
- 更好的组件化及代码npm复用
- 更好的移动端支持
- 引入了 RxJS 与 Observable
- 引入了 Zone.js，提供更加智能的变化检测

## 特点
- 跨平台
- 高性能
- 多种IDE支持

# 环境搭建
## 基础要求
- Node.js
- Git

## 环境配置
- 安装 Angular CLI
```
npm install -g @angular/cli
```
- 检测 Angular CLI 是否安装成功
```
ng --version
```
- 创建新的项目
```
ng new PROJECT-NAME
```
这个CLI为我们创建了第一个Angular组件。 它就是名叫<code>app-root</code>的根组件。 你可以在<code>./src/app/app.component.ts</code>目录下找到它。

- 启动本地服务器
```
cd PROJECT-NAME
ng serve
```
使用--open（或-o）参数可以自动打开浏览器并访问http://localhost:4200/。

# 基础知识
## 显示数据
**插值表达式**
在 Angular 中，我们可以使用插值语法实现数据绑定。Angular 自动从组件中提取属性的值，并且把这些值插入浏览器中。当这些属性发生变化时，Angular 就会自动刷新显示。Angular 先对它求值，再把它转换成字符串。
```ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  template: `
    <h2>大家好，我是{{name}}</h2>
    <p>我来自<strong>{{address.province}}</strong>省,
      <strong>{{address.city}}</strong>市
    </p>
    <p>{{address|json}}</p>
  `,
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  name = 'Semlinker';
  address = {
    province: '福建',
    city: '厦门'
  }
}
```
值得一提的是，这里使用了 Angular 内置的 json 管道。
<code>@Component</code>装饰器中指定的 CSS 选择器<code>selector</code>，它指定了一个叫<code>app-root</code>的元素。 该元素是<code>index.html</code>的<code>body</code>里的占位符。当通过<code>main.ts</code>中的<code>AppComponent</code>类启动时，Angular 在<code>index.html</code>中查找一个<code>my-app</code>元素， 然后实例化一个<code>AppComponent</code>，并将其渲染到<code>my-app</code>标签中。
```html
<body>
  <app-root></app-root>
</body>
```
插值表达式可以把计算后的字符串插入到 HTML 元素标签内的文本或对标签的属性进行赋值。
```html
<h3>
  {{title}}
  <img src="{{heroImageUrl}}" style="height:30px">
</h3>
```

根据json管道补充一下管道操作符（|）：
在绑定之前，表达式的结果可能需要一些转换。例如，可能希望把数字显示成金额、强制文本变成大写，或者过滤列表以及进行排序。

## 模板语法
**模板表达式**
模板表达式产生一个值。 Angular 执行这个表达式，并把它赋值给绑定目标的属性，这个绑定目标可能是 HTML 元素、组件或指令。
JavaScript 中那些具有或可能引发副作用的表达式是被禁止的，包括：
- 赋值 (=, +=, -=, ...)
- new运算符
- 使用;或,的链式表达式
- 自增或自减操作符 (++和--)

表达式中的上下文变量是由模板变量、指令的上下文变量（如果有）和组件的成员叠加而成的。 如果我们要引用的变量名存在于一个以上的命名空间中，那么，**模板变量是最优先的，其次是指令的上下文变量，最后是组件的成员**。

**模板语句**
模板语句用来响应由绑定目标（如 HTML 元素、组件或指令）触发的事件。它出现在=号右侧的引号中，就像这样：<code>(event)="statement"</code>。
某些 JavaScript 语法是不允许的：
- new运算符
- 自增和自减运算符：++和--
- 操作并赋值，例如+=和-=
- 位操作符|和&
- 模板表达式运算符

语句上下文可以引用模板自身上下文中的属性。 在下面的例子中，就把模板的<code>$event</code>对象、模板输入变量 <code>(let hero)</code>和模板引用变量 <code>(#heroForm)</code>传给了组件中的一个事件处理器方法。
```html
<button (click)="onSave($event)">Save</button>
<button *ngFor="let hero of heroes" (click)="deleteHero(hero)">{{hero.name}}</button>
<form #heroForm (ngSubmit)="onSubmit(heroForm)"> ... </form>
```

**数据绑定和数据方向**
- 1.单向（数据源->视图）： 插值表达式 Property Attribute 类 样式
```
{{expression}}
[target]="expression"
bind-target="expression"
```
注1.方括号表示要计算的模板表达式，不加方括号，Angular会将表达式视为字符串，而不会计算这个字符串。
注2.当要渲染的数据类型是字符串时，倾向于使用插值表达式，否则必须使用属性绑定。
注3.当元素没有属性可绑的时候，就必须使用 attribute 绑定。
```html
<table border=1>
  <!--  expression calculates colspan=2 -->
  <tr><td [attr.colspan]="1 + 1">One-Two</td></tr>

  <!-- ERROR: There is no `colspan` property to set!
    <tr><td colspan="{{1 + 1}}">Three-Four</td></tr>
  -->
  <tr><td>Five</td><td>Six</td></tr>
</table>
```
注4.css类绑定
```html
<!-- 当badCurly有值的时候，整个class设置的内容全部被覆盖 -->
<div class="bad curly special"
     [class]="badCurly">Bad curly</div>

<!-- 模板表达式的值为真时，添加这个类，否则移除 -->
<div [class.special]="isSpecial">The class binding is special</div>

<div class="special"
     [class.special]="!isSpecial">This one is not so special</div>
```
注5.style样式绑定
样式绑定的语法与属性绑定类似。 但方括号中的部分不是元素的属性名，而由style前缀，一个点和 CSS 样式的属性名组成。 形如：<code>[style.style-property]</code>。
```html
<button [style.font-size.em]="isSpecial ? 3 : 1" >Big</button>
<button [style.font-size.%]="!isSpecial ? 150 : 50" >Small</button>
```

- 2.单向（视图->数据源）： 事件
```
(targrt)="statement"
on-target="statement"
```
事件对象的形态取决于目标事件。如果目标事件是原生 DOM 元素事件， $event就是 DOM事件对象，它有像target和target.value这样的属性。
```html
<input [value]="currentHero.name"
       (input)="currentHero.name=$event.target.value" >
```

- 3.双向
```
[(target)]="expression"
bindon-target="expression"
```
**在 Angular 的世界中，attribute 唯一的作用是用来初始化元素和指令的状态。 当进行数据绑定时，只是在与元素和指令的 property 和事件打交道，而 attribute 就完全靠边站了。**
```ts
import { Component, EventEmitter, Input, Output } from '@angular/core';
 
@Component({
  selector: 'app-sizer',
  template: `
  <div>
    <button (click)="dec()" title="smaller">-</button>
    <button (click)="inc()" title="bigger">+</button>
    <label [style.font-size.px]="size">FontSize: {{size}}px</label>
  </div>`
})
export class SizerComponent {
  @Input()  size: number | string;
  @Output() sizeChange = new EventEmitter<number>();
 
  dec() { this.resize(-1); }
  inc() { this.resize(+1); }
 
  resize(delta: number) {
    this.size = Math.min(40, Math.max(8, +this.size + delta));
    this.sizeChange.emit(this.size);
  }
}
```
```html
<app-sizer [(size)]="fontSizePx"></app-sizer>
<div [style.font-size.px]="fontSizePx">Resizable Text</div>
```

**内置指令**
1. 内置属性型指令
- NgClass - 添加或移除一组CSS类
可以把ngClass绑定到一个<code> key:value</code> 形式的控制对象。这个对象中的每个 key 都是一个 CSS 类名，如果它的 value 是true，这个类就会被加上，否则就会被移除。
```ts
currentClasses: {};
setCurrentClasses() {
  // CSS classes: added/removed per current state of component properties
  this.currentClasses =  {
    'saveable': this.canSave,
    'modified': !this.isUnchanged,
    'special':  this.isSpecial
  };
}
```
```html
<div [ngClass]="currentClasses">test</div>
```
- NgStyle - 添加或移除一组CSS样式
NgStyle需要绑定到一个<code> key:value</code>控制对象。 对象的每个 key 是样式名，它的 value 是能用于这个样式的任何值。
```ts
currentStyles: {};
setCurrentStyles() {
  // CSS styles: set per current state of component properties
  this.currentStyles = {
    'font-style':  this.canSave      ? 'italic' : 'normal',
    'font-weight': !this.isUnchanged ? 'bold'   : 'normal',
    'font-size':   this.isSpecial    ? '24px'   : '12px'
  };
}
```
```html
<div [ngStyle]="currentStyles">test</div>
```
- NgModel - 双向绑定到HTML表单元素
**在使用ngModel指令进行双向数据绑定之前，我们必须导入FormsModule并把它添加到Angular模块的imports列表中。**

2.内置结构型指令
- NgIf - 根据条件把一个元素添加到DOM中或从DOM移除
```html
<app-hero-detail *ngIf="isActive"></app-hero-detail>
```
- NgSwitch - 一组指令，用于切换一组视图
```html
<div [ngSwitch]="currentHero.emotion">
  <app-happy-hero    *ngSwitchCase="'happy'"    [hero]="currentHero"></app-happy-hero>
  <app-sad-hero      *ngSwitchCase="'sad'"      [hero]="currentHero"></app-sad-hero>
  <app-confused-hero *ngSwitchCase="'confused'" [hero]="currentHero"></app-confused-hero>
  <app-unknown-hero  *ngSwitchDefault           [hero]="currentHero"></app-unknown-hero>
</div>
```
- NgForOf - 对列表中的每个条目重复套用同一个模板
```html
<div *ngFor="let hero of heroes">{{hero.name}}</div>
<div *ngFor="let hero of heroes; let i=index">{{i + 1}} - {{hero.name}}</div>
```

**模板引用变量（#）**
模板引用变量通常用来引用模板中的某个DOM元素。
```html
<input #phone placeholder="phone number">
<button (click)="callPhone(phone.value)">Call</button>
```

**输入输出属性**
```html
<app-hero-detail [hero]="currentHero" (deleteRequest)="deleteHero($event)">
</app-hero-detail>
```
在HeroDetailComponent内部，这些属性被装饰器标记成了输入和输出属性。
```ts
@Input()  hero: Hero;
@Output() deleteRequest = new EventEmitter<Hero>();
```
从HeroDetailComponent角度来看，HeroDetailComponent.hero是个输入属性， 因为数据流从模板绑定表达式流入那个属性。
从HeroDetailComponent角度来看，HeroDetailComponent.deleteRequest是个输出属性， 因为事件从那个属性流出，流向模板绑定语句中的处理器。

为输出属性起别名：
```ts
@Output('myClick') clicks = new EventEmitter<string>(); 
```

**模板表达式操作符**
- 管道操作符（|）
- 安全导航操作符 ( ?. ) 和空属性路径
Angular 安全导航操作符 (?.) 是在属性路径中保护空值的更加流畅、便利的方式。 表达式会在它遇到第一个空值的时候跳出。 显示是空的，但应用正常工作，而没有发生错误。
```html
<!-- No hero, no problem! -->
The null hero's name is {{nullHero?.name}}
```
- 非空断言操作符（!）
在 Angular 编译器把你的模板转换成 TypeScript 代码时，这个操作符会防止 TypeScript 报告 "hero.name可能为null或undefined"的错误。
```html
<div *ngIf="hero">
  The hero's name is {{hero!.name}}
</div>
```


**内联 (inline)模板和模板文件**
可以使用<code>template</code>属性把它定义为内联的，或者把模板定义在一个独立的 HTML 文件中， 再通过<code>@Component</code>装饰器中的<code>templateUrl</code>属性， 在组件元数据中把它链接到组件。
```ts
@Component({
  selector: 'app-home',
  templateUrl: './home.component.html',
  styleUrls: ['./home.component.css']
})
```
```ts
@Component({
  selector: 'app-root',
  template: `
    <h2>大家好，我是{{name}}</h2>
    <p>我来自<strong>{{address.province}}</strong>省,
      <strong>{{address.city}}</strong>市
    </p>
    <p>{{address|json}}</p>
  `,
  styleUrls: ['./app.component.css']
})
```

# 组件交互
**1.通过输入型绑定把数据从父组件传到子组件 @Input()，需要import入core中的Input类**
**2.通过setter截听输入属性值的变化**
```ts
import { Component, Input } from '@angular/core';
 
@Component({
  selector: 'app-name-child',
  template: '<h3>"{{name}}"</h3>'
})
export class NameChildComponent {
  private _name = '';
 
  @Input()
  set name(name: string) {
    this._name = (name && name.trim()) || '<no name set>';
  }
 
  get name(): string { return this._name; }
}
```
**3.通过ngOnChanges()来截听输入属性值的变化**
使用OnChanges生命周期钩子接口的ngOnChanges()方法来监测输入属性值的变化并做出回应。

**未完待续**