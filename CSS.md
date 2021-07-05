#### 引入CSS样式表

- 行内式，缺点：没有实现样式和结构相分离
- 内部式，缺点：没有彻底分离结构和样式
- 外链式，将所有的样式放在一个或多个以.css为扩展名的外部样式表文件中，通过link标签将外部样式表文件链接到HTML文档中。

```js
<div style="color:red, font-size:12px;">行内样式</div>

<!--内嵌式-->
<head>
    <style type="text/css">
        选择器{
            属性1：属性值1；
            属性2：属性值2；
        }
	</style>
</head>

<link rel="stylesheet" href="index.css">
    h3{
        color:pink;
    }
```

#### CSS基础选择器

1. 标签选择器

   ```js
   标签名{属性1：属性值1；属性2：属性值2；属性3：属性值3；}
   ```

2. 类选择器

   ```js
   .类名{
       属性1：属性值1；
       属性2：属性值2；
       属性3：属性值3；
   }
   <p class="类名"></p>
   ```

3. id选择器

   ```js
   #id名{属性1：属性值1；属性2：属性值2；属性3：属性值3；}
   <p id="id名"></p>
   ```

4. 通配符选择器

   ```js
   *{属性1：属性值1；属性2：属性值2；属性3：属性值3；}
   
   *{
       margin:0;
       padding:0;
   }/*清除所有HTML标记的默认边距*/
   ```

#### CSS复合选择器

1. - 后代选择器

   ```js
   父级 子级{属性1：属性值1；属性2：属性值2；属性3：属性值3；}
   .class h3{color:red;font-size:16px;}
   ```

2. 子元素选择器:只能选择作为某元素**子元素（亲儿子）**的元素。

   ```js
   .class>h3{color:red;font-size:14px;}
   ```

3. 交集选择器

   ```js
   h3.class{color:red;font-size:25px;}
   两个选择器之间不能有空格
   ```

4. 并集选择器:用于集体声明，用逗号隔开，理解为和的意思

   ```js
   .one,p,#test{
   color:red;
   }/*表示三个选择器都会执行颜色为红色*/
   ```

5. 链接伪类选择器:采用的是交集选择器

   - a:link 未访问的链接
   - a:visited 已访问的链接
   - a:hover 鼠标移动到链接上
   - a:active 选定的链接

   ```js
   a{/*所有的链接*/
       font-weight:700;
       font-size:16px;
       color:blue;
       text-decoration:none;/*清除下划线*/
   }
   a:hover{
       color:red;/*鼠标经过链接时会由原来的蓝色变成红色*/
   }
   ```

   #### CSS字体样式

   ##### font字体

   1. font-size：设置字号大小

      ```js
      p{font-size:20px;}
      /*em:文本字体尺寸
      px:像素，最常用*/
      ```

   2. font-family

      ```js
      p{font-family:"微软雅黑";}
      ```

   3. font-weight

      normal:不加粗

      bold：定义粗体

      100~900：400等同于normal，700等同于bold

   4. font-style:定义字体风格，设置斜体、倾斜或正常字体

      normal：默认值，浏览器显示标准字体样式

      italic：斜体

   5. font:综合设置字体样式

      ```js
      选择器{font:font-style font-weight font-size/line-height font-family;}
      其中不需要设置的属性可以省略，但是必须保留font-size和font-family属性，否则font属性将不起作用
      ```

   #### CSS外观属性

   1. color:

      | 表示           | 属性值                        |
      | -------------- | ----------------------------- |
      | 预定义的颜色值 | red,green,blue,pink           |
      | 十六进制       | #FF0000,#FF6600,#29D794       |
      | RGB代码        | rgb(255,0,0)或rgb(100%,0%,0%) |

   2. text-align:设置文本内容的水平对齐方式，相当于html中的align对齐属性

      **是让盒子里面的文本内容水平居中，不是盒子居中对齐**

      | 属性   | 解释           |
      | ------ | -------------- |
      | left   | 左对齐（默认） |
      | right  | 右对齐         |
      | center | 居中对齐       |

   3. line-height：设置行间距

      **文字的行高等于盒子的高度**：让单行文本在盒子中垂直居中对齐

      ```js
       /*line-height 要设置在font属性下面，否则无效，例如：*/
      height: 80px;
      text-align: center;
      font: normal bold 30px "宋体";
      line-height: 80px;
      
      可以使用display:flex;布局方式让文字水平垂直居中
      display: flex;
      align-items: center;     /* 侧轴对齐方式*/
      justify-content: center; /* 主轴对齐方式 */
      ```

   4. text-indent:用于设置首行文本的缩进

      ```js
      p {
            /*行间距*/
            line-height: 25px;
            /*首行缩进2个字  em  1个em 就是1个字的大小*/
            text-indent: 2em;  
       }
      ```

   5. text-decoration:文本的修饰

      | 值           | 描述                             |
      | ------------ | -------------------------------- |
      | none         | 默认（常用来取消下划线）         |
      | underline    | 下划线                           |
      | overline     | 定义文本上的一条线（不用）       |
      | line-through | 定义穿过文本下的一条线（不常用） |

   #### 标签显示模式（display)

   - 块转行内：display:inline;
   - 行内转块：display：block；
   - 块、行内元素转换为行内块：display:inline-block;

   1. 块级元素：```js
   <h1>~<h6>、<p>、<div>、<ul>、<ol>、<li>
   ```等，其中<div>是最典型的块元素

      特点：独占一行，宽度默认是容器的100%，高度，宽度，外边距内边距都可以控制

      注意：只有文字能组成段落，因此p标签里面不能放块级元素，**特别是p不能放div.**同理，h1~h6,dt,都是文字类块级标签，里面不能放其他块级元素。

   2. 行内元素（内联元素）：<a>、<strong>、<b>、<em>、<i>、<del>、<s>、<ins>、<u>、<span>等，其中<span>是最典型的行内元素

      特点：相邻行内元素在一行，一行可以显示多个，高度，宽度，外边距内边距直接设置都无效，默认高度是它本身内容的宽度。**行内元素只能容纳文本或其他行内元素。**

      注意：链接里面不能再放链接；特殊情况a里面可以放块级元素，但是给a转换一下块级模式最安全。

   3. 行内块元素（inline-block）：<img>、<input>、<td>.

      特点：一行可以显示多个，高度行高边距都可以控制，宽度默认是本身宽度。

#### CSS背景

1. 背景颜色

   ```js
   background-color:颜色值；默认为transparent（透明的）
   ```

2. 背景图片

   ```js
   background-image:none|url(url);
   background-image:url(images/1.jpg);
   ```

3. 背景平铺

   ```js
   background-repeat:repeat|no-repeat|repeat-x|repeat-y
   ```

4. 背景位置

   ```js
   background-position:length||length
   background-position:pisition||position
   ```

5. 背景半透明

   ```js
   background:rgba(0,0,0,0.3);
   background:rgba(0,0,0,.3);
   
   opacity:.2;
   ```

#### CSS三大特性

1. css层叠性：多种样式的叠加

   样式冲突：**就近原则**：哪个样式离结构近执行哪个样式

   样式不冲突：不层叠

2. css继承性：子标签会继承父标签的某些样式

   **text-,font-,line-**这些元素开头的可以继承，以及color属性

3. css优先级

   | 标签选择器               | 计算权重公式 |
   | ------------------------ | ------------ |
   | 继承或者 *               | 0,0,0,0      |
   | 每个元素（标签选择器）   | 0,0,0,1      |
   | 每个类，伪类             | 0,0,1,0      |
   | 每个ID                   | 0,1,0,0      |
   | 每个行内样式 style=""    | 1,0,0,0      |
   | 每个!important  最重要的 | ∞ 无穷大     |

   - 数位之间没有进制，级别之间不可超越，从左到右一级比一级大
   - 权重可以叠加
   - **继承的权重是0**

#### 盒子模型：css盒子模型、浮动、定位

##### 网页布局本质

- 首先利用css设置好盒子的大小，然后摆放盒子的位置
- 最后把网页元素比如文字图片等等，放入盒子里面

1. ##### 盒子模型：由元素内容，边框（border）、内边距（padding)、和外边距（margin）组成。

   ##### 盒子实际大小=内容的宽度和高度+内边距+边框

2. ##### 盒子边框（border）

   ##### 边框的样式：

   - none:没有边框
   - solid：边框为单实线
   - dashed:边框为虚线
   - dotted:边框为点线

   ```js
   边框综合设置
   border : border-width || border-style || border-color 
   
   border: 1px solid red;  没有顺序要求  
   ```

   border-top-style:上边框样式

   ##### 表格细线边框：cellspacing="0",单元格与单元格之间距离设置为0

   ```js
   <style>
       <table>{
       width:500px;
       height:300px;
       border:1px solid red;
   }
   td{
       border:1px solid red;
       text-align:center;
   }
   table,td{
       border-collapse:collapse;/*合并相邻边框*/
   }
   </style>
   ```

##### 内边距（padding）：边框与内容之间的 距离

| 属性           | 作用     |
| -------------- | -------- |
| padding-left   | 左内边距 |
| padding-right  | 右内边距 |
| padding-top    | 上内边距 |
| padding-bottom | 下内边距 |

- 当我们给盒子指定padding值之后，内容和边框之间有了距离，添加了内边距，盒子会变大。
- **解决措施**：通过给设置了宽高的盒子，减去相应的内边距的值，维持盒子原有的大小。
- **padding不影响盒子大小情况：**如果没有给一个盒子指定宽度，此时，如果给这个盒子指定padding，则不会撑开盒子。

##### 外边距（margin）：盒子与盒子之间的距离

| 属性          | 作用     |
| ------------- | -------- |
| margin-left   | 左外边距 |
| margin-right  | 右外边距 |
| margin-top    | 上外边距 |
| margin-bottom | 下外边距 |

**块级盒子水平居中：**

- 盒子必须指定宽度
- 左右边距设置为auto

```js
.header{
    width:960px;margin:0 auto;
}
```

##### 文字居中和盒子居中区别：

- 盒子水平居中是text-align:center;而且还可以让行内元素和行内块居中对齐
- 块级盒子水平居中：左右margin改为auto

##### 插入图片和背景图片区别：

- 插入图片：移动位置只能靠盒模型padding margin
- 背景图片：一般用小图标背景或者超大背景图片、背景图片，移动位置只能通过background-position

##### 清除元素的默认内外边距

行内元素为了照顾兼容性，尽量只设置左右内外边距，不要设置上下内外边距。

```js
* {
   padding:0;         /* 清除内边距 */
   margin:0;          /* 清除外边距 */
}
```

#### 外边框合并

1. 相邻块元素垂直外边距的合并

   - 当上下相邻的两个块元素相遇时，如果上面的元素有下边距margin-bottom
   - 下面的元素有上外边距margin-top，则她们之间的垂直间距不是margin-bottom与margin-top之和
   - **取两个值中的较大者**

   <u>解决方案：尽量只给一个盒子添加margin值</u>

2. 嵌套块元素垂直外边距的合并

   - 对于两个嵌套关系的块元素，如果父元素没有上内边距及边框
   - 父元素的上外边距会与子元素的上外边距发生合并
   - 合并后的外边距为两者中的较大者

   <u>解决方案</u>：

   - [ ] 为父元素定义上边框
   - [ ] 为父元素定义上内边距
   - [ ] 为父元素添加**overflow：hidden**（常用）

3. 盒子模型布局稳定性：优先使用宽度，其次使用内边距，再次外边距

   ```js
   width>padding>margin
   ```

   原因：

   - margin会有外边距合并
   - padding会影响盒子大小
   - width没有问题，我们经常使用宽度剩余法，高度剩余法来做

##### 圆角边框：

```js
border-radius:length;

border-top-left-radius 定义左上角的弧度
border-top-right-radius 定义右上角的弧度
border-bottom-right-radius 定义右下角的弧度
border-bottom-left-radius 定义左上下角的弧度

border-radius：50%； 也可以表示为百分比的形式

border-radius：左上角 右上角 右下角 左下角；
```

盒子阴影（box-shadow):

```js
box-shadow:offset-x offset-y [blur [spread]] [color] [inset]
blur:阴影模糊距离，不能取负数 spread：阴影大小 inset:添加内阴影，默认为外阴影

div {
   width: 200px;
   height: 200px;
   border: 10px solid red;
   /* box-shadow: 5px 5px 3px 4px rgba(0, 0, 0, .4);  */
   /* box-shadow:水平位置 垂直位置 模糊距离 阴影尺寸（影子大小） 阴影颜色  内/外阴影； */
   box-shadow: 0 15px 30px  rgba(0, 0, 0, .4);   
}
```

### 浮动

#### css布局三种机制：普通流（标准流）、浮动、定位

**普通流**：

- 块级元素会独占一行，从上到下顺序排列：
  - [ ] 常用元素：div、hr、p、h1~h6、ul、ol、dl、form、table
- 行内元素会按照顺序，从左到右顺序排列，碰到父元素边缘则自动换行；
  - [ ] 常用元素：span、a、i、em等

**浮动**：

- 让盒子从普通流中浮起来，主要作用让多个块级盒子一行显示

**定位**：

- 将盒子定在浏览器的某一位置

#### 什么是浮动？

- 脱离标准流控制，不占位置，简称脱标
- 移动到指定位置

##### 作用：

- 让多个盒子水平排列成一行，使得浮动称之为布局的重要手段
- 可以实现盒子的左右对齐
- 浮动最早是用来控制图片，实现文字环绕图片效果
- float属性会改变元素的display属性，任何元素都可以浮动。浮动元素会生成一个块级框，而不论它本身是何种元素。生成的块级框和我们前面的行内块极其相似。

```js
选择器{float:属性值;}
none:不浮动
left：左浮
right：右浮
```

**注意：**如果一个盒子里有多个子盒子，其中一个盒子浮动了，其他兄弟盒子也应该浮动。浮动元素会改变display属性，类似转换为了行内块，但是元素之间没有空白缝隙

#### 清除浮动：

###### **清除浮动本质**：清除浮动主要为了解决父级元素因为子级浮动引起内部高度为0 的问题。清除浮动之后， 父级就会根据浮动的子盒子自动检测高度。父级有了高度，就不会影响下面的标准流了 

```js
选择器 { clear: 属性值; }

1.额外标签法(通过在浮动元素末尾添加一个空的标签)
<div style=”clear:both”></div>

2.级添加overflow属性方法
overflow为 hidden| auto| scroll  都可以实现。
常用：overflow：hidden；

3.使用after伪元素清除浮动
.clearfix:after {
        content: "";
        display: block;
        height: 0;
        clear: both;
        visibility: hidden;
}
  
/* IE6、7 专有 */
.clearfix {
        *zoom: 1;
 }

4.使用双伪元素清除浮动
.clearfix:before,
.clearfix:after {
        content: "";
        display: table;
}

.clearfix:after {
        clear: both;
}

.clearfix {
       *zoom: 1;
}
```

##### 什么时候用清除浮动？

- 父级没高度
- 子盒子浮动了
- 影响下面布局了

##### CSS属性书写顺序

1. 布局定位属性：display / position / float / clear / visibility / overflow（建议 display 第一个写，毕竟关系到模式） 
2. 自身属性：width / height / margin / padding / border / background
3. 文本属性：color / font / text-decoration / text-align / vertical-align / white- space / break-word
4. 其他属性（CSS3）：content / cursor / border-radius / box-shadow / text-shadow / background:linear-gradient …

### 定位（position）:

###### 把盒子定在某一个位置，自由的漂浮在其他盒子的上面。

- 定位=定位模式+边偏移。

- 定位模式：

  ```js
  选择器 { position: 属性值; }
  ```

1. 静态定位（static）

   - 静态定位是元素的默认定位方式，无定位的意思。它相当于border里面的none，不要定位的时候用
   - 静态定位按照标准流特性摆放位置。它没有边偏移。
   - 静态定位在布局时基本不用

2. 相对定位（relative）

   - [ ] 相对定位是元素相对于它原来在标准流中的位置来说的
   - [ ] 相对于自己原来在标准流中位置移动
   - [ ] 原来在标准流的区域继续占有，后面的盒子仍然以标准流的方式对待它。不脱标。

3. 绝对定位（absolute)

   绝对定位是元素以带有定位的父级元素来移动位置

   - 完全脱标--完全不占位置
   - 父元素没有定位，则以浏览器为准定位

   **定位口诀--子绝父相**

4. 固定定位（fixed）:绝对定位的一种特殊形式

   - 完全脱标
   - 只认浏览器的可视窗口--**浏览器可视窗口+边偏移**属性来设置元素的位置
     - [ ] 跟父元素没有任何关系，单独使用
     - [ ] 不随滚动条滚动

##### 定位扩展

###### 绝对定位的盒子居中

- left：50%；让盒子的左侧移动到父级元素的水平中心位置
- margin-left：-100px;让盒子向左移动自身宽度的一半。
- 同理垂直居中。

###### 堆叠顺序（z-index)

- 加了定位的盒子默认**后来居上**，后面的盒子会压住前面的盒子
- 应用z-index层叠等级属性可以调整盒子的堆叠顺序

z-index特性：

- [ ] 属性值：正整数、负整数或0，默认值是0，**数值越大，盒子越靠上**
- [ ] 如果属性值相同，则按照书写顺序，后来居上
- [ ] 数字后面不能加单位
- [ ] z-index只能用于相对定位、绝对定位和固定定位的元素，其他标准流、浮动和静态定位无效。

###### 定位改变display属性

| 定位模式         | 是否脱标占有位置     | 移动位置基准           | 模式转换（行内块） | 使用情况                 |
| ---------------- | -------------------- | ---------------------- | ------------------ | ------------------------ |
| 静态static       | 不脱标，正常模式     | 正常模式               | 不能               | 几乎不用                 |
| 相对定位relative | 不脱标，占有位置     | 相对自身位置移动       | 不能               | 基本单独使用             |
| 绝对定位absolute | 完全脱标，不占有位置 | 相对于定位父级移动位置 | 能                 | 要和定位父级元素搭配使用 |
| 固定定位fixed    | 完全脱标，不占有位置 | 相对于浏览器移动位置   | 能                 | 单独使用，不需要父级     |

**注意：**

1. `边偏移` 需要和 `定位模式` 联合使用，`单独使用无效`；
2. `top` 和 `bottom` 不要同时使用；
3. `left` 和 `right` 不要同时使用。

#### CSS高级技巧

元素的显示与隐藏

1. **display显示（重点）**

   - display：none隐藏对象，隐藏之后不再保留位置
   - display：block除了转换为块级元素之外，同时还有显示元素的意思

2. **visibility可见性**

   ```js
   visibility：visible ;  对象可视
   
   visibility：hidden;    对象隐藏
   ```

3. **overflow溢出**

   清除浮动，隐藏超出内容，不允许内容超过父盒子

   | 属性       | 区别                   | 用途                                                         |
   | ---------- | ---------------------- | ------------------------------------------------------------ |
   | display    | 隐藏对象，不保留位置   | 配合后面js做特效，比如下拉菜单，原先没有，鼠标经过，显示下拉菜单， 应用极为广泛 |
   | visibility | 隐藏对象，保留位置     | 使用较少                                                     |
   | overflow   | 只是隐藏超出大小的部分 | 1. 可以清除浮动 2. 保证盒子里面的内容不会超出该盒子范围      |

##### CSS用户界面样式

1. 鼠标样式

   ```js
   <ul>
     <li style="cursor:default">我是小白</li>
     <li style="cursor:pointer">我是小手</li>
     <li style="cursor:move">我是移动</li>
     <li style="cursor:text">我是文本</li>
     <li style="cursor:not-allowed">我是文本</li>
   </ul>
   ```

2. 轮廓线 outline

   ```js
   outline : outline-color ||outline-style || outline-width 
   ```

3. 防止拖拽文本域resize

   ```js
   <textarea  style="resize: none;"></textarea>
   ```

##### vertical-align 垂直对齐

- 让块级元素居中对齐：margin：0 auto；
- 让文字居中对齐：taxt-align:center;

```js
设置或检索对象内容的垂直对其方式。
vertical-align : baseline |top |middle |bottom 
vertical-align 不影响块级元素中的内容对齐，它只针对于「行内元素」或者「行内块元素」
```

##### 图片、表单和文字对齐

| 模式                                 | 单词                     |
| ------------------------------------ | ------------------------ |
| 基线对齐（默认是文字和图片基线对齐） | vertical-align:baseline; |
| 垂直居中                             | vertical-align:middle;   |
| 顶部对齐                             | vertical-align:top;      |

##### 去除图片底侧空白缝隙：父盒子由图片撑开，图片下面会多出缝隙。

解决方法：

- 给img vertical-align:middle|top|bottom等。让图片不要和基线对齐
- 给img添加display：block；转换为块级元素

##### 溢出的文字省略号显示

```js
强制一行显示内容
white-space:normal ；默认处理方式
white-space:nowrap ； 强制在同一行内显示所有文本，直到文本结束或者遭遇br标签对象才换行。

文字溢出text-overflow
text-overflow : clip ；不显示省略标记（...），而是简单的裁切 
text-overflow：ellipsis ； 当对象内文本溢出时显示省略标记（...）

/*1. 先强制一行内显示文本*/
white-space: nowrap;
/*2. 超出的部分隐藏*/
overflow: hidden;
/*3. 文字用省略号替代超出的部分*/
text-overflow: ellipsis;
```

##### CSS精灵技术（sprite）

- **为什么需要精灵技术**：为了有效地减少服务器接受和发送请求的次数，提高页面的加载速度。 
- 精灵技术主要针对**背景图片**
- 核心技术就是利用`CSS精灵`（主要是背景位置）和 `盒子padding`撑开宽度, 以便能适应不同字数的导航栏。 

```js
<li>
  <a href="#">
    <span>导航栏内容</span>
  </a>
</li>

* {
    padding:0;
    margin:0;

   }
    body{
      background: url(images/wx.jpg) repeat-x;
    }
    .father {
      padding-top:20px;
    }
    li {
      padding-left: 16px;
      height: 33px;
      float: left;
      line-height: 33px;
      margin:0  10px;
      background: url(./images/to.png) no-repeat left ;
    }
	a {
      padding-right: 16px;
      height: 33px;
      display: inline-block;
      color:#fff;
      background: url(./images/to.png) no-repeat right ;
      text-decoration: none;
    }
    li:hover,
    li:hover a {
      background-image:url(./images/ao.png);
    }
```

1. a设置背景左侧，padding撑开合适宽度

2. span设置背景右侧，padding撑开合适宽度，剩下由文字继续撑开宽度

3. a包含span就是因为整个导航都是可以点击的

   ##### css三角形

   ```js
   div {
   width: 0; 
   height: 0;
   line-height:0；
   font-size: 0;
   border-top: 10px solid red;
   border-right: 10px solid green;
   border-bottom: 10px solid blue;
   border-left: 10px solid #000; 
   }
   
   为了照顾兼容性 低版本的浏览器，加上 font-size: 0;  line-height: 0;
   ```

   

   

