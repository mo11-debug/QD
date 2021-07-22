### Web API

API概念：API是一些预先定义的函数，目的是提供应用程序与开发人员基于某软件或硬件得以访问一组例程的能力，而又无需访问源码，或理解内部工作机制的细节。

- 任何开发语言都有自己的API
- API的特征输入和输出(I/O):var max=Math.max(1,2,3);
- API的使用方法(console.log('adf'))

Web API的概念：浏览器提供的一套操作浏览器功能和页面元素的API(BOM和DOM)

MDN-Web API:(https://developer.mozilla.org/zh-CN/docs/Web/API) 

ECMAScript-JS的核心：定义了JS语法的规范，描述了语言的基本语法和数据类型，定义了一种语言的标准与具体实现无关

##### BOM-浏览器对象模型

一套操作浏览器功能的API，通过BOM可以操作浏览器窗口，比如：弹出框、控制浏览器跳转、获取分辨率等

##### DOM-文档对象模型

一套操作页面元素的API，DOM可以把HTML看做文档树，通过DOM提供的API可以对树上的节点进行操作

#### DOM

概念：文档对象模型，是W3C推荐的可扩展标记语言的标准编程接口。它是一种与平台和语言无关的应用程序接口(API)，它可以动态地访问程序和脚本，更新其内容、结构和文档的风格。文档可以进一步被处理，处理的结果可以加入到当前的页面。DOM是一种基于树的API文档，它要求在处理过程中整个文档都表示在存储器中。

- 文档：一个网页可以称为文档
- 节点：网页中的所有内容
- 元素：网页中的标签
- 属性：标签的属性

##### DOM经常进行的操作

- 获取元素
- 对元素进行操作（设置其属性或调用其方法）
- 动态创建元素
- 事件（什么时机做相应的操作）

#### 获取页面元素

##### 为什么要获取页面元素？

例如：我们想要操作页面上的某部分（显示、隐藏，动画），需要先获取到该部分对应的元素，才进行后续操作

**根据id获取元素**

```js
var div = document.getElementById('main');
console.log(div);
//获取到的数据类型HTMLDivElement.对象都是有类型的
//使用console.dir()可以打印我们获取的元素对象，更好的查看对象里面的属性和方法
```

注意：由于id名具有唯一性，部分浏览器支持使用id名访问元素，但不是标准方式，不推荐使用

**根据标签名获取元素**

```js
var divs = document.getElementsByTagName('div');
for(var i = 0; i < divs.length; i ++){
    var div = divs[i];
    console.log(div);
}//因为得到的是一个对象的集合，所以我们想要操作里面的元素就要遍历
//得到元素对象是动态的
```

**根据name获取元素**

```js
var inputs=document.getElementsByName('hobby');
for(var i = 0 ; i < inputs.length; i++){
    var input = inputs[i];
    console.log(input);
}
```

**根据类名获取元素**

```js
var mains = documents.getElementByClassName('main');
for (var i = 0; i < mains.length; i++){
    var main = mains[i];
    console.log(main);
}
```

**根据选择器获取元素**

```js
var text = document.querySelector('#text');
console.log(text);//根据指定选择器返回第一个元素对象

var boxes = documents.querySelectorAll('.box');
for (var i = 0; i < boxes.length; i++){
    var box = boxes[i];
    console.log(box);
}
```

**获取特殊元素（body,html)**

```js
document.body;
document.documentElement;
```

需要：

- 掌握getElementById()、getElementsByTagName()
- 了解getElementsByName()、getElementsByClassName()、querySelector()、querySelectorAll()

#### 事件：触发-响应机制

##### 事件三要素：

- 事件源：触发（被）事件的元素
- 事件名称：click点击事件
- 事件处理程序：事件触发后要执行的代码（函数形式）

##### 事件的基本使用

```js
var box = document.getElementById('box');
box.onclick = function(){
    console.log('代码会在box被点击之后执行');
};
```

案例：

点击按钮弹出提示框：

分析：

- 获取事件源（按钮）
- 注册事件（绑定事件），使用noclick
- 编写事件处理程序，写一个函数弹出alert警示框

实现代码：

```js
var btn = document.getElementById('btn');
btn.onclick = function(){
    alert('你好吗');
};
```

常见的鼠标事件：

| 鼠标事件    | 触发条件         |
| ----------- | ---------------- |
| onclick     | 鼠标点击左键触发 |
| onmouseover | 鼠标经过触发     |
| onmouseout  | 鼠标离开触发     |
| onfocus     | 获得鼠标焦点触发 |
| onblur      | 失去鼠标焦点触发 |
| onmousemove | 鼠标移动触发     |
| onmouseup   | 鼠标弹起触发     |
| onmousedown | 鼠标按下触发     |

#### 操作元素

JS的DOM操作可以改变网页内容、结构和样式，我们可以利用DOM操作元素来改变元素里面的内容、属性等。

##### 改变元素内容

```je
element.innetText
//从开始到终止位置的内容，但它去除html标签，同时空格和换行也会去掉
element.innerHTML
//起始位置到终止位置的全部内容，包括html标签，同时保留空格和换行
```

##### 常见元素的属性操作

href、title、id、src、className

```js
var link = document.getElementById('link');
console.log(link.href);
console.log(link.title);

var pic = document.getElementById('pic');
console.log(pic.sic);
```

案例：分时显示不同图片，显示不同问候语

- innerHTML和innerText

  ```js
  var box = document.getElementById('box');
  box.innetHTML='我是文本<p>我会生成标签</p>';
  console.log(box.innerHTML);
  box.innetText='我是文本<p>我不会生成标签</p>';
  console.log(box.innerText);
  ```

- HTML转义符

```js
"		&quot;
'		&apos;
&		&amp;
<		&lt;   // less than  小于
>		&gt;   // greater than  大于
空格	   &nbsp;
©		&copy;
```

#### 表单元素属性

- value用于大部分表单元素的内容获取（option）除外
- type可以获取input标签的类型（输入框或复选框等）
- disabled禁用属性
- checked复选框选中属性
- selected下拉菜单选中属性

#### 自定义属性操作

- getAttribute()获取标签行内属性
- setAttribute()设置标签行内属性
- removeAttribute()移除行内属性
- 与element属性的区别：上述三个方法用于获取任意的行内属性

##### 样式操作

使用style方式设置的样式显示在标签行内

```js
var box = document.getElementById('box');
box.style.width = '100px';
box.style.height = '100px';
box.style.backgroundColor = 'red';
```

注意：通过样式属性设置宽高、位置的属性类型是字符串，需要加上px.

##### 类名操作

修改标签的className属性相当于直接修改标签的类名

```js
var box = document.getElementById('box');
box.className = 'show';
```

#### 创建元素的三种方式

document.write()

```js
document.write('新设置的内<p>标签也可以生成</p>');
```

innerHTML

```js
var box = document.getElementById('box');
box.innerHTML = '新内容<p>新标签</p>';
```

document.createElement()

```js
var div = document.createElement('div');
document.body.appendChild(div);
```

##### 性能问题

- innerHTML方法由于会对字符串进行解析，需要避免在循环内多次使用
- 可以借助字符串或数组的方式进行替换，再设置给innerHTML
- 优化后与document.createElement性能相近

#### 节点操作

```js
var body = document.body;
var div = documen.createElement('div');
body.appendChild(div);

var firstEle = body.children[0];
body.inserBefore(div,firstEle);

body.removeChild(firstEle);

var text = document.createElement('p');
body.replaceChild(text,div);
```

#### 节点属性

- node Type节点的类型

  1. 元素节点
  2. 属性节点
  3. 文本节点

- nodeName节点的名称（标签名称）

- nodeValue节点值

  元素节点的nodeValue始终是null

```js
function Node(option){
    this.id = option.id || '';
    this.nodeName = option.nodeName || '';
    this.nodeValue = option.nodeValue || '';
    this.nodeType = 1;
    this.children = option.children || [];
}

var doc = new Node({
    nodeName : 'html';
})
var head = new Node({
    nodeName : 'head';
})
var body = new Node({
    nodeName : 'body';
})
doc.children.push(head);
doc.children.push(body);

var div = new node({
    nodeName:'div';
    nodeValue:'haha';
})
var p = new node({
    nodeName:'p';
    nodeValue:'段落';
})
body.children.push(div);
body.children.push(p);

function getChildren(ele){
    for(var i = 0; i < ele.children.length; i++){
        var child = ele.children[i];
        console.log(child.nodeName);
        getChildren(child);
    }
}
getChildren(doc);
```

节点层级

```js
var box = document.getElementById('box');
console.log(box.parentNode);
console.log(box.childNodes);
console.log(box.children);
console.log(box.nextSibling);
console.log(box.previousSibling);
console.log(box.firstChild);
console.log(box.lastChild);
```

**注意**

- childNodes和children的区别，childNodes获取的是子节点，，children获取的是子元素
- nextSibling和previousSibling获取的是节点，获取元素对应的属性是nextElementSibling和previousElementSibling获取的是元素
- nextElementSibling和previousElementSibling有兼容性问题，IE9以后才支持

**总结**

```js
节点操作，方法
appendChild()
insertBefore()
removeChild()
replaceChild()
节点层次，属性
parentNode
childNodes
children
nextSibling/previousSibling
firstChild/lastChild
```

#### 事件详解

##### 注册/移除事件的三种方式

```js
var box = document.getElementById('box');
box.onclick = function(){
    console.log('点击后执行');
};
box.onclick = null;

box.addEventListener('click', eventCode, false);
box.removeEventListtener('click', eventCode, false);

box.attachEvent('onclick', eventCode);
box.detachEvent('onclick', eventCode);

function eventCode(){
    console.log('点击后执行');
}
```

##### 兼容代码

```js
function addEventListener(element, type, fn){
    if(element.addEventListener){
        element.addEventListener(type, fn, false);
    } else if(element.attachEvent){
        element.attachEvent('on' + type,fn);
    } else{
        element['on' + type] = fn;
    }
}

function removeEventListene(element,type,fn){
    if(element.removeEventListener){
        element.removeEventListener(type,fn,false);
    } else if(element.detachEvent){
        element.deatchEvent('on' + type,fn);
    } else {
        element['on'+type] = null;
    }
}
```

##### 事件的三个阶段

1. 捕获阶段

2. 当前目标阶段

3. 冒泡阶段

   事件对象.eventPhase属性可以查看事件触发时所处的阶段

##### 事件对象的属性和方法

- event.type获取事件类型
- clientX/clientY所有浏览器都支持，窗口位置
- pageX/pageY IE8以前不支持，页面位置
- event.targe || event.strcElement用于获取触发事件的元素
- event.preventDefault()取消默认行为

案例

- 跟着鼠标飞的天使
- 鼠标点哪图片飞到哪里
- 获取鼠标在div内的坐标

##### 阻止事件传播的方式

- 标准方式event,stopPropagation();
- IE低版本event.cancelBubble = true;标准中已废弃

##### 常用的鼠标和键盘事件

- onmouseup鼠标按键放开时触发
- onmousedown鼠标按键按下触发
- onmousemove鼠标移动触发
- onkeyup键盘按键按下触发
- onlkeydown键盘按键抬起触发

#### 元素的类型

#### BOM

概念：BOM是指浏览器对象模型，浏览器对象提供了独立于内容的、可以与浏览器窗口进行互动的对象结构。BOM由多个对象组成，其中代表浏览器窗口的window对象是BOM的顶层对象，其他对象都是该对象的子对象。

我们在浏览器中的一些操作都可以使用BOM的方式进行编程处理，比如：刷新浏览器、后退、前进、在浏览器中输入URL等

##### BOM的顶级对象window

window是浏览器的顶级对象，当调用window下的属性和方法时，可以省略

window注意：window下一个特殊的属性window.name

##### 对话框

- alert()
- prompt()
- confirm()

##### 页面加载事件

- onload

  ```js
  window.onload = function(){
      //当前页面加载完成执行
      //当页面完全加载所有内容（包括图像、脚本文件、CSS文件等）执行
  }
  ```

- onunload

  ```js
  window.onunload = function(){
      //当用户退出页面时执行
  }
  ```

##### 定时器

setTimeout()和clearTimeout()

在指定的毫秒数到达之后执行指定的函数，只执行一次

```js
//创建一个定时器，1000毫秒后执行，返回定时器的标示
var timerId = setTimeout(function(){
    console.log('hello world');
},1000);

//取消定时器的执行
clearTimeout(timerId);
```

setInterval()和clearInterval()

定时调用的函数，可以按照给定的时间（单位毫秒）周期调用函数

```js
//创建一个定时器，每隔一秒调用一次
var timerId = setInterval(function(){
    var date = new Date();
    console.log(date.toLocaleTimeString());
},1000);
```

案例：定时器，简单动画

##### location对象

location对象是window对象下的一个属性，使用的时候可以省略window对象

location可以获取或者设置浏览器地址栏的URL

##### location有哪些成员？

- 使用chrome的控制台查看

- 查MDN

  [Location - Web API 接口参考 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/API/Location) 

- 成员

  - [ ] assign()/reload()/replace()
  - [ ] hash/host/hostname/search/href...

##### URL

统一资源定位符

- URL的组成

  ```js
  scheme://host:port?query#fragment
  http://www.itheima.com/a/b/index.html?name=zs&age=18#bottom
  scheme:通信协议
  	常用的http.ftp.matio等
  host:主机
  	服务器（计算机）域名系统（DNS）主机名或IP地址。
  port：端口号
  	整数，可选，省略时使用方案的默认端口，如http的默认端口为80
  path：路径
  	由零或多个'/'符号隔开的字符串，一般用来表示主机上的一个目录或文件地址
  query:查询
  	可选，用于给动态网页传递参数，可有多个参数，用'&'符号隔开，每个参数的名和值用'='符号隔开，例如：name=zs
  fragment:信息片段
  	字符串，锚点
  ```

##### 作用

解析URL中的query,并返回对象的形式

```js
function getQuery(queryStr){
    var query = {};
    if(queryStr.indexOf('?') > -1){
        var index = queryStr.indexOf('?');
        queryStr = queryStr.substr(index + 1);
        var array = queryStr.split('&');
        for(var i = 0; i < array.length; i++){
            var tmpArr = arrayp[i].split('=');
            if(tmpArr.length === 2){
                query[tmpArr[0]] = tmpArr[1];
            }
        }
    }
    return query;
}
console.log(getQuery(location.search));
console.log(getQuery(location.href));
```

##### history对象

- back()
- forward()
- go()

##### navigator对象

userAgent()

#### 特效

##### 偏移量

- offsetParent()用于获取定位的父级元素
- offsetParent和parentNode的区别

```js
var box = document.getElementById('box');
console.log(box.offsetParent);
console.log(box.offsetLeft);
console.log(box.offsetTop);
console.log(box.offsetWidth);
console.log(box.offsetHeight);
```

##### 客户区大小

```js
var box = document.getElementById('box');
console.log(box.clientLeft);
console.log(box.clientTop);
console.log(box.clientWidth);
console.log(box.clientHeight);
```

##### 滚动偏移

```js
var box = document.getElementById('box');
console.log(box.scrollLeft);
console.log(box.scrollTop);
console.log(box.scrollWidth);
console.log(box.scrollHeight);
```

案例

- 拖拽案例
- 弹出登录窗口
- 放大镜案例
- 模拟滚动条
- 匀速动画函数
- 变速动画函数
- 无缝轮播图
- 回到顶部











