#### jQuery介绍

1. JS库

   即一个封装好的特定的集合（方法和函数）。从封装一大堆函数的角度理解库，就是在这个库中，封装了很多预习定义好的函数在里面，比如动画animate、hide、show，比如获得元素等。

   常见的JS库：jQuery、Prototype、YUI、Dojo、Ext JS、移动端的zepto等，这些库都是对原生JS的封装，内部都是用JS实现的。

2. jQuery概念

   - jQery是一个快速、简洁的JS库，其设计的宗旨是“write Less,Do More",即倡导写更少的代码，做更多的事情。
   - j是JS，Query查询，意思就是查询js,把js中的DOM操作做了封装，我们可以快速的查询使用里面的功能。
   - jQuery封装； JS常用的功能代码，优化了DOM操作、事件处理、动画设计和Ajax交互。
   - 学习jQuery本质：就是学习调用这些函数（方法）。
   - jQuery出现的目的是加快前端人员的开发速度，我们可以非常方便的调用和使用它，从而提高开发效率。

3. jQuery优点

   - 轻量级。核心文件才几十kb.不会影响页面加载速度
   - 跨浏览器兼容，基本兼容了现在的主流浏览器
   - 链式编程、隐式迭代
   - 对事件、样式、动画支持，大大简化了DOM操作
   - 支持插件扩展开发，有着丰富的第三方的插件，例如:树形菜单、日期控件、轮播图等
   - 免费、开源

##### jQuery的基本使用

下载：官网：https://jquery.com/ 

体验：

- 引入jQuery文件
- 在文档最末尾插入script标签，书写体验代码
- $('div').hide()可以隐藏盒子

##### jQuery的入口函数

```js
//1.简单
$(function(){
    ...//此处是页面DOM加载完成的入口
});
//2.繁琐
    $(document).ready(function(){
        ...//此处是页面DOM加载完成的入口
    })
```

总结：

- 等着DOM结构渲染完毕即可执行内部代码，不必等到所有外部资源加载完成，jQuery帮我们完成了封装
- 相当于原生js中的DOMContentLoaded
- 不同于原生js中的load事件是等页面文档、外部的js文件、css文件、图片加载完毕才执行内部代码
- 更推荐第一种

##### jQuery中的顶级对象$

- $是jQuery的别称，在代码中可以使用jQuery代替
- $是jQuery的顶级对象，相当于原生js中的window，把元素利用￥包装成jQuery对象，就可以调用jQuery的方法

##### jQuery对象和DOM对象

使用jQuery方法和原生JS获取的元素是不一样的：

- 用原生JS获取来的对象就是DOM对象
- jQuery方法获取的元素就是jQuery对象
- jQuery对象本质是：利用$对DOM对象包装后产生的对象（伪数组形式存储）
- **注意：**只有jQuery对象才能使用jQuery方法，DOM对象则使用原生的JS方法

##### jQuery对象和DOM对象转换

DOM对象与jQuery对象之间是可以相互转换的，因为原生js比jQuery更大，原生的一些属性和方法jQuery没有给我们封装，要想使用这些属性和方法需要把jQuery对象转换为DOM对象才能使用

```js
//1.DOM对象转换成jQuery对象
var box = document.getElementById('box');//获取DOM对象
var jQueryObject = $(box);//把DOM对象转换为jQuery对象

//2.jQuery对象转换为DOM对象
//jQuery对象【索引值】
var domObject1 = $('div')[0]
//jQuery对象.get(索引值)
var domObject2 = $('div').get(0)
```

总结：实际开发比较常用的是把DOM对象转换为jQuery对象，这样能够调用功能强大的jQuery中的方法

##### jQuery选择器

原生JS获取元素方式很多，很杂，而且兼容性情况不一致，因此jQuery给我们做了封装，使获取元素统一标准

**基础选择器**

```js
$("选择器")//里面选择器直接写CSS选择器即可，但是要加引号
```

| 名称       | 用法            | 描述                     |
| ---------- | --------------- | ------------------------ |
| ID选择器   | $('div')        | 获取指定ID的元素         |
| 全选择器   | $('*')          | 匹配所有元素             |
| 类选择器   | $('.class')     | 获取同一类class的元素    |
| 标签选择器 | $('div')        | 获取同一类标签的所有元素 |
| 并集选择器 | $('div,p,li')   | 选取多个元素             |
| 交集选择器 | $('li.current') | 交集元素                 |

**层级选择器**

最常用的两个分别为：后代选择器和子代选择器

案例代码：

```js
<body>
    <div>我是div</div>
	<div class="nav">我是nav div</div>
	<p>我是p</p>
	<ul>
        <li>我是ul的</li>
        <li>我是ul的</li>
        <li>我是ul的</li>
    </ul>
    <script>
            $(function(){
            console.log($(".nav"));
            console.log($("ul li"));
        })
    </script>
</body>
```

**筛选选择器**：在所有的选项中选择满足条件的进行筛选选择。

| 语法       | 用法          | 描述                                 |
| ---------- | ------------- | ------------------------------------ |
| :first     | $('li:first') | 获取第一个li元素                     |
| :last      | $('li:last')  | 获取最后一个li元素                   |
| :eq(index) | $('li:eq(2)') | 获取到的li元素中，索引号为2的元素    |
| :odd       | $('li:odd')   | 获取到的li元素中，索引号为奇数的元素 |
| :even      | $('li:even')  | 获取到的li元素中，索引号为偶数的元素 |

案例代码：

```js
<body>
    <ul>
    	<li>多个里面筛选几个</li>
	    <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
    </ul>
    <ol>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
        <li>多个里面筛选几个</li>
    </ol>
    <script>
        $(function() {
            $("ul li:first").css("color", "red");
            $("ul li:eq(2)").css("color", "blue");
            $("ol li:odd").css("color", "skyblue");
            $("ol li:even").css("color", "pink");
        })
    </script>
</body>
```

另：jQuery中还有一些筛选方法，类似DOM中的通过一个节点找另外一个节点，父、子、兄以外有所加强

| 语法               | 用法                           | 说明                                                   |
| ------------------ | ------------------------------ | ------------------------------------------------------ |
| parent()           | $("li").parent();              | 查找父级                                               |
| children(selector) | $("li").children("li");        | 相当于$("ul>li")，最近一级（亲儿子）                   |
| find(selector)     | $("li").find("li");            | 相当于$("ul li")，后代选择器                           |
| siblings(selector) | $("li").siblings("li");        | 查找兄弟节点，不包括自己本身                           |
| nextAll([expr])    | $("li").nextAll();             | 查找当前元素之后所有的同辈元素                         |
| prevtAll([expr])   | $("li").prevAll();             | 查找当前元素之前所有的同辈元素                         |
| hasClass(class)    | $("li").hasClass("protected"); | 检查当前的元素是否含有某个特定的类，如果有，则返回true |
| eq(index)          | $("li").eq(2);                 | 相当于$("li").eq(2)，index从0开始                      |

##### 知识铺垫

- jQuery设置样式

  ```js
  $('div').css('属性','值')
  ```

- jQuery里面的排他思想

  ```js
  //想要多选一的效果，排他思想；当前元素设置样式，其余的兄弟元素清除样式
  $(this).css("color","red");
  $(this).siblings().css("color","");
  ```

- 隐式迭代

  ```js
  //遍历内部DOM元素（伪数组形式存储的过程就叫做隐式迭代
  //简单理解：给匹配到的所有元素进行循环遍历，执行相应的方法，而不用我们再进行循环，简化我们的操作，方便我们调用。
  &('div').hide();//页面中所有的div全部隐藏，不用循环操作
  ```

- 链式编程

  ```js
  //链式编程是为了节省代码量，看起来更优雅
  $(this).css('color','red').sibling().css('color','');
  ```

##### 案例：淘宝服饰精品

思路分析：

1. 核心原理：鼠标经过左侧盒子某个小li，就让内容区盒子相对应图片显示，其余的图片隐藏
2. 需要得到当前小li的索引号，就可以显示对应索引号的图片
3. jQuery得到当前元素索引号$(this).index()
4. 中间对应的图片，可以通过eq(index)方法区选择
5. 显示元素show()，隐藏元素hide()

##### jQuery样式操作

常见的有：css()和设置类样式方法

1. css方法操作：多用于样式少时操作

   ```js
   //1.参数只写属性名，则是返回属性值
   var strColor = $(this).css('color');
   //2.参数是属性名，属性值，逗号分隔，是设置一组样式，属性必须加引号，值如果是数字可以不用跟单位和引号
   $(this).css('color','red');
   //3.参数可以是对象形式，方便设置多组样式，属性名和属性值用冒号隔开，属性可以不用加引号
   $(this).css({"color":"white","font-size":"20px"});
   ```

2. 设置类样式方法:作用等同于以前的classList，可以操作类样式，注意操作类里面的**参数不要加点**。

   ```js
   //1.添加类
   $('div').addClass("current");
   //2.删除类
   $('div').removeClass("current");
   //3.切换类
   $('div').toggleClass("current");
   ```

   **注意：**

   - 设置类样式方法比较适合多样式时操作，可以弥补css()的不足
   - 原生JS中className会覆盖元素原先里面的类名，jQuery里面类操作只是对指定类进行操作，不影响原先的类名

**案例：tab栏切换**

思路分析:

1. 点击上部的li,当前li添加current类，其余兄弟移除类
2. 点击的同时，得到当前li的索引号
3. 让下部里面相应索引号的item显示，其余的item隐藏

##### jQuery效果

- 显示隐藏：show()/hide()/toogle()
- 滑入滑出：slideDown()/slideUp()/slideToggle();
- 淡入淡出：fadeln()/fadeOut()/fadeToggle()/fadeTo();
- 自定义动画：animate()

**注意**：动画或者效果一旦触发就会执行，如果多次触发，就造成多个动画或者效果排队执行。jQuery为我们提供另一个方法，可以停止动画排队：stop();

##### 显示隐藏：

常见三个方法：show()/hide()/toogle()

1. 语法规范

   ```js
   show([speed],[easing],[fn]);
   hide([speed],[easing],[fn]);
   toogle([speed],[easing],[fn]);
   ```

2. 参数

   - 参数都可以省略，无动画直接显示
   - speed：三种预定速度之一的字符串（"slow","normal",or"fast")或表示动画时长的毫秒数值（如：1000）。
   - easing:(Optional)用来指定切换效果，默认是“swing",可用参数”linear"
   - fn:回调函数，在动画完成时执行的函数，每个元素执行一次

```js
<body>
    <button>显示</button>
    <button>隐藏</button>
    <button>切换</button>
    <div></div>
    <script>
        $(function() {
            $("button").eq(0).click(function() {
                $("div").show(1000, function() {
                    alert(1);
                });
            })
            $("button").eq(1).click(function() {
                $("div").hide(1000, function() {
                    alert(1);
                });
            })
            $("button").eq(2).click(function() {
              $("div").toggle(1000);
            })
            // 一般情况下，我们都不加参数直接显示隐藏就可以了
        });
    </script>
</body>
```

##### **滑入滑出**：

常见三个方法：slideDown()/slideUp()/slideToggle():

1. 语法规范：

   ```js
   slideDown([speed,[easing],[fn]]);
   slideUp([speed,[easing],[fn]]);
   slideToggle([speed,[easing],[fn]]);
   ```

2. 参数：

   - 参数都可以省略，无动画直接显示
   - speed：三种预定速度之一的字符串（"slow","normal",or"fast")或表示动画时长的毫秒数值（如：1000）。
   - easing:(Optional)用来指定切换效果，默认是“swing",可用参数”linear"
   - fn:回调函数，在动画完成时执行的函数，每个元素执行一次

```js
<body>
    <button>下拉滑动</button>
    <button>上拉滑动</button>
    <button>切换滑动</button>
    <div></div>
    <script>
        $(function() {
            $("button").eq(0).click(function() {
                // 下滑动 slideDown()
                $("div").slideDown();
            })
            $("button").eq(1).click(function() {
                // 上滑动 slideUp()
                $("div").slideUp(500);
            })
            $("button").eq(2).click(function() {
                // 滑动切换 slideToggle()
                $("div").slideToggle(500);
            });
        });
    </script>
</body>
```

##### 自定义动画

1. 语法规范：

   ```js
   animate(params,[speed],[easing],[fn])
   ```

2. 参数：

   - params:想要更改的样式属性，以对象形式传递，必须写，属性名可以不用引号，如果是复合属性则需要采取驼峰命名法borderLeft,其余参数都可以省略

3. 代码演示：

   ```js
   <body>
       <button>动起来</button>
       <div></div>
       <script>
           $(function() {
               $("button").click(function() {
                   $("div").animate({
                       left: 500,
                       top: 300,
                       opacity: .4,
                       width: 500
                   }, 500);
               })
           })
       </script>
   </body>
   ```


##### 停止动画排队：

动画或者效果一旦触发就会执行，如果多次触发，就造成多个动画或者效果排队执行

- stop()方法用于停止动画或效果
- stop()写到动画或者效果的前面，相当于停止介绍上一次的动画
- 每次使用动画之前，先调用stop()，再调用动画

##### 事件切换：

jQuery中为我们添加了一个新事件hover:();功能类似css中的伪类:hover.

语法：

```js
hover([over,]out)//其中over和out为两个函数
```

- over:鼠标移到元素上要触发的函数（相当于mouseenter）
- out:鼠标移出元素要触发的函数（相当于mouseleaver）
- 如果只写一个函数，则鼠标经过和离家都会触发它

**hover事件和停止动画排列案例**

```js
<body>
    <ul class="nav">
        <li>
            <a href="#">微博</a>
            <ul><li><a href="">私信</a></li><li><a href="">评论</a></li><li><a href="">@我</a></li></ul>
        </li>
        <li>
            <a href="#">微博</a>
            <ul><li><a href="">私信</a></li><li><a href="">评论</a></li><li><a href="">@我</a></li></ul>
        </li>
    </ul>
    <script>
        $(function() {
            // 鼠标经过
            // $(".nav>li").mouseover(function() {
            //     // $(this) jQuery 当前元素  this不要加引号
            //     // show() 显示元素  hide() 隐藏元素
            //     $(this).children("ul").slideDown(200);
            // });
            // // 鼠标离开
            // $(".nav>li").mouseout(function() {
            //     $(this).children("ul").slideUp(200);
            // });
            // 1. 事件切换 hover 就是鼠标经过和离开的复合写法
            // $(".nav>li").hover(function() {
            //     $(this).children("ul").slideDown(200);
            // }, function() {
            //     $(this).children("ul").slideUp(200);
            // });
            // 2. 事件切换 hover  如果只写一个函数，那么鼠标经过和鼠标离开都会触发这个函数
            $(".nav>li").hover(function() {
                // stop 方法必须写到动画的前面
                $(this).children("ul").stop().slideToggle();
            });
        })
    </script>
</body>
```

**案例：王泽荣耀手风琴**

思路分析：

1. 鼠标经过某个小li有两步操作
2. 当前小li宽度变为224px，同时里面的小图片淡出，大图片淡入
3. 其余兄弟小li宽度变为69px,小图片淡入，大图片淡出

```js
$(function() 
{
// 鼠标经过某个小li 有两步操作：
    $(".king li").mouseenter(function() {
    // 1.当前小li 宽度变为 224px， 同时里面的小图片淡出，大图片淡入
    $(this).stop().animate({
        width: 224
        }).find(".small").stop().fadeOut().siblings(".big").stop().fadeIn();
        // 2.其余兄弟小li宽度变为69px， 小图片淡入， 大图片淡出
        $(this).siblings("li").stop().animate({
            width: 69
        }).find(".small").stop().fadeIn().siblings(".big").stop().fadeOut();
        })
});
```

#### jQuery属性操作

常用的有三种：prpo()/attr()/data();

##### 元素固有属性prpo()

```js
prpo("属性")//获取属性
prpo("属性","属性值")//设置属性值
```

注：prpo()除了普通属性操作，更适合操作表单属性：disabled/checked/selected等

##### 元素自定义属性值attr()

```js
attr("属性")//类似getArrtibute()
attr("属性","属性值")//类似setArrtibute()
```

注：attr()除了普通属性操作，更适合操作自定义属性

##### 数据缓存data()

```js
data("name","value")//向被选元素附加数据
data("name")//向被选元素获取数据
```

注：还可以读取h5自定义属性data-index,得到的是数字型

代码演示：

```js
<body>
    <a href="http://www.itcast.cn" title="都挺好">都挺好</a>
    <input type="checkbox" name="" id="" checked>
    <div index="1" data-index="2">我是div</div>
    <span>123</span>
    <script>
        $(function() {
            //1. element.prop("属性名") 获取元素固有的属性值
            console.log($("a").prop("href"));
            $("a").prop("title", "我们都挺好");
            $("input").change(function() {
                console.log($(this).prop("checked"));
            });
            // console.log($("div").prop("index"));
            // 2. 元素的自定义属性 我们通过 attr()
            console.log($("div").attr("index"));
            $("div").attr("index", 4);
            console.log($("div").attr("data-index"));
            // 3. 数据缓存 data() 这个里面的数据是存放在元素的内存里面
            $("span").data("uname", "andy");
            console.log($("span").data("uname"));
            // 这个方法获取data-index h5自定义属性 第一个 不用写data-  而且返回的是数字型
            console.log($("div").data("index"));
        })
    </script>
</body>
```

##### jQuery文本属性值

常见的有三种：html()/text()/val();分别对应JS中的innerHTML/innerText/value属性

**jQuery内容文本值**

```js
<body>
    <div>
        <span>我是内容</span>
    </div>
    <input type="text" value="请输入内容">
    <script>
        // 1. 获取设置元素内容 html()
        console.log($("div").html());
        // $("div").html("123");
        // 2. 获取设置元素文本内容 text()
        console.log($("div").text());
        $("div").text("123");
        // 3. 获取设置表单值 val()
        console.log($("input").val());
        $("input").val("123");
    </script>
</body>
```

##### jQuery元素操作

1. 遍历元素

   ```js
   $("div").each(function(index,domEle){xxx; })
   ```

   - each()方法遍历匹配的每一个元素，主要用DOM处理
   - 里面的回调函数有2个参数：index是每个元素的索引号，demEle是每个DOM元素对象，不是jQuery对象
   - 要使用jQuery方法，需要给这个dom元素转换为jQuery对象 $(domEle)

   ```js
   $.each(object,function(index,element){xxx; })
   ```

   - $.each()方法可用于遍历任何对象，主要用于数据处理，比如数值、对象
   - 里面的函数有2个参数：index是每个元素的索引号，element遍历内容

2. 创建、添加、删除

   创建：

   ```js
   $("<li></li>");
   ```

   添加：

   ```js
   element.append("内容")//内部添加，把内容放到匹配元素最后面
   element.prepend("内容")//内部添加，把内容放到匹配元素最前面
   element.affer("内容")//外部，后面
   element.before("内容")//外部，前面
   ```

   **内部添加元素生成之后是父子关系，外部是兄弟关系**

   删除：

   ```js
   element.remove()//删除元素本身
   element.empty()//删除匹配元素集合中所有子节点
   element.html()//清除匹配元素内容
   ```

##### jQuery尺寸、位置操作

1. 尺寸

   | 语法                             | 用法                                                |
   | -------------------------------- | --------------------------------------------------- |
   | width()/height()                 | 取得匹配元素宽度和高度值，只算width/height          |
   | innerWidth()/innerHeight()       | 取得匹配元素宽度和高度值包括padding                 |
   | outerWidth()/outerHeight()       | 取得匹配元素宽度和高度值包括padding、border         |
   | outerWidth(true)/outHeight(true) | 取得匹配元素宽度和高度值包括padding、border、margin |

   - 以上参数为空，则是获取相应值，返回的是数字型
   - 如果参数为数字，则是修改相应值
   - 参数可以不写单位

2. 位置

   - offset()设置或获取元素偏移
     - [ ] offset()方法设置或返回被选元素相对于**文档**的偏移坐标，和父级没有关系
     - [ ] 该方法有2个属性left，top.
     - [ ] 可设置元素的偏移：offset({top:10,left:30});
   - position()
     - [ ] position()方法设置或返回被选元素相对于**带有定位的父级**的偏移坐标，如果父级没有定位以文档为准
     - [ ] 该方法有2个属性left，top.
     - [ ] 只能获取
   - scrollTop()/scrollLeft()
     - [ ] scrollTop()方法设置或返回被选元素被卷去的头部
     - [ ] 不跟参数是获取，参数为不带单位的数值则是设置被卷去的头部

#### jQuery事件

##### jQuery事件注册

- 优点：操作简单，且不用担心事件覆盖等问题
- 缺点：普通的事件注册不能做事件委托，且无法实现事件解绑。

单个事件注册：

```js
element.事件(function(){})
$("div").click(function(){事件处理程序})
```

代码案例：

```js
<body>
    <div></div>
    <script>
        $(function() {
            // 1. 单个事件注册
            $("div").click(function() {
                $(this).css("background", "purple");
            });
            $("div").mouseenter(function() {
                $(this).css("background", "skyblue");
            });
        })
    </script>
</body>
```

##### jQuery事件处理

1. on()绑定事件

   可以绑定多个事件，多个处理事件处理程序：

   ```js
   $("div").on({
       mouseover:function(){},
       mouseout:function(){},
       click:function(){},
   });
   ```

   如果事件处理程序相同

   ```js
   $("div").on("mouseover mouseout", function(){
       $(this).toggleClass("current");
   });
   ```

   可以事件委派操作：把原来加给子元素身上的事件绑定在父元素上，就是把事件委派给父元素

   ```js
   $('ul').on('click','li',function(){
       alert('hello world');
   });
   ```

   动态创建的元素，click()没有办法绑定事件，on()可以给动态生成的元素绑定事件

   ```js
   $('div').on('click','p',function(){
       alert('给动态生成元素绑定事件')
   });
   ```

2. off()事件解绑

   当某个事件上面的逻辑，在特定需求下不需要的时候，可以把该事件上的逻辑移除，这个过程我们称为事件解绑。

   off()方法可以移除通过on()方法添加的事件处理程序

   ```js
   $("p").off//解绑p元素所有事件处理程序
   $("p").off("click")//解绑p元素上面的点击事件，需要的foo是侦听函数名
   $("p").off("click","li")//解绑事件委托
   ```

   如果有的事件只想触发一次，可以使用one()来绑定事件

3. trigger()/triggerHandler():事件触发

   在特定条件下，我们希望某些事件能够自动触发，比如轮播图自动播放功能跟点击右侧按钮一致。可以利用定时器自动触发右侧按钮点击事件，不必鼠标点击触发。

   trigger()

   ```js
   element.click()
   element.trigger("type")
   ```

   triggerHandler():不会自动触发元素的默认行为

   ```js
   element.triggerHandler("type")
   ```

##### jQuery事件对象

jQuery对DOM中的事件对象event进行了封装，兼容性更好，获取更方便。事件被触发，就会有事件对象的产生。

```js
element.on(events,[selection],function(event){})
```

阻止默认行为：event.preventDefault()或者return false

冒泡行为：event.stopPropagation()

##### jQuery拷贝对象

```js
$.extend([deep],target,object1,[objectN])
```

- deep:如果设为true深拷贝，默认为false浅拷贝
- target:要拷贝的目标对象
- object1:待拷贝到第一个对象的对象
- objectN:待拷贝到第N个对象的对象
- 浅拷贝目标对象引用的被拷贝的对象地址，修改目标对象会影响被拷贝对象
- 深拷贝，前面加true，完全克隆，修改目标对象不会影响被拷贝对象

```js
 <script>
        $(function() {
   			// 1.合并数据
            var targetObj = {};
            var obj = {
                id: 1,
                name: "andy"
            };
            // $.extend(target, obj);
            $.extend(targetObj, obj);
            console.log(targetObj);
   
   			// 2. 会覆盖 targetObj 里面原来的数据
            var targetObj = {
                id: 0
            };
            var obj = {
                id: 1,
                name: "andy"
            };
            // $.extend(target, obj);
            $.extend(targetObj, obj);
            console.log(targetObj); 
        })
    </script>
```

##### jQuery多库共存

- 把里面的$统一改为jQuery

- jQuery变量规定新的名称：$.onConflict()

- 代码

  ```js
  <script>
  	$(function() {
    		// 让jquery 释放对$ 控制权 让用自己决定
    		var suibian = jQuery.noConflict();
    		console.log(suibian("span"));
  	})
  </script>
  ```

##### jQuery插件

1. 瀑布流
2. 图片懒加载（图片使用延迟加载在可提高网页下载速度。它也能帮助减轻服务器负载）

  当我们页面滑动到可视区域，再显示图片。

  我们使用jquery 插件库  EasyLazyload。 注意，此时的js引入文件和js调用必须写到 DOM元素（图片）最后面

3. 全屏滚动（fullpage.js）

​      gitHub： <https://github.com/alvarotrigo/fullPage.js>

​      中文翻译网站： http://www.dowebok.com/demo/2014/77/

#### 综合案例：toDoList分析

**案例介绍：**

①文本框里面输入内容，按下回车，就可以生成待办事项。

②点击待办事项复选框，就可以把当前数据添加到已完成事项里面。

③点击已完成事项复选框，就可以把当前数据添加到待办事项里面。

④但是本页面内容刷新页面不会丢失。

**toDoList分析：**

①刷新页面不会丢失数据，因此需要用到本地存储 localStorage

②核心思路： 不管按下回车，还是点击复选框，都是把本地存储的数据加载到页面中，这样保证刷新关闭页面不会丢失数据

③存储的数据格式：var todolist =  [{ title : ‘xxx’, done: false}]

④注意点1： 本地存储 localStorage 里面只能存储字符串格式 ，因此需要把对象转换为字符串 JSON.stringify(data)。

⑤注意点2： 获取本地存储数据，需要把里面的字符串转换为对象格式JSON.parse() 我们才能使用里面的数据。

**toDoList 按下回车把新数据添加到本地存储里面** :

①切记： 页面中的数据，都要从本地存储里面获取，这样刷新页面不会丢失数据，所以先要把数据保存到本地存储里面。

②利用事件对象.keyCode判断用户按下回车键（13）。

③声明一个数组，保存数据。

④先要读取本地存储原来的数据（声明函数 getData()），放到这个数组里面。

⑤之后把最新从表单获取过来的数据，追加到数组里面。

⑥最后把数组存储给本地存储 (声明函数 savaDate())

**toDoList 本地存储数据渲染加载到页面** ：

①因为后面也会经常渲染加载操作，所以声明一个函数 load，方便后面调用

②先要读取本地存储数据。（数据不要忘记转换为对象格式）

③之后遍历这个数据（$.each()），有几条数据，就生成几个小li 添加到 ol 里面。

④每次渲染之前，先把原先里面 ol 的内容清空，然后渲染加载最新的数据。

**toDoList 删除操作 ：**

①点击里面的a链接，不是删除的li，而是删除本地存储对应的数据。

②核心原理：先获取本地存储数据，删除对应的数据，保存给本地存储，重新渲染列表li

③我们可以给链接自定义属性记录当前的索引号

④根据这个索引号删除相关的数据----数组的splice(i, 1)方法

⑤存储修改后的数据，然后存储给本地存储

⑥重新渲染加载数据列表

⑦因为a是动态创建的，我们使用on方法绑定事件

**toDoList  正在进行和已完成选项操作：**

①当我们点击了小的复选框，修改本地存储数据，再重新渲染数据列表。

②点击之后，获取本地存储数据。

③修改对应数据属性 done 为当前复选框的checked状态。

④之后保存数据到本地存储

⑤重新渲染加载数据列表

⑥load 加载函数里面，新增一个条件,如果当前数据的done为true 就是已经完成的，就把列表渲染加载到 ul 里面

⑦如果当前数据的done 为false， 则是待办事项，就把列表渲染加载到 ol 里面

**toDoList 统计正在进行个数和已经完成个数 ：**

①在我们load 函数里面操作

②声明2个变量 ：todoCount 待办个数  doneCount 已完成个数   

③当进行遍历本地存储数据的时候， 如果 数据done为 false， 则 todoCount++, 否则 doneCount++

④最后修改相应的元素 text() 