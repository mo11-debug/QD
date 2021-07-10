#### HTML5拓展

- 语义化标签

  - [ ] header:头部标签
  - [ ] nav：导航标签
  - [ ] article：内容标签
  - [ ] section：块级标签
  - [ ] aside：侧边栏标签
  - [ ] footer：尾部标签

- 多媒体音频标签：audio

  | 属性     | 值       | 描述                                           |
  | -------- | -------- | ---------------------------------------------- |
  | autoplay | autoplay | 如果出现该属性，则音频在就绪后马上播放         |
  | controls | controls | 如果出现该属性，则向用户显示控件，比如播放按钮 |
  | loop     | loop     | 如果出现该属性，则每当音频结束时重新开始播放   |
  | src      | url      | 要播放的音频的URL                              |

  ```js
  <audio controls>
      <!--因为chrome浏览器禁用autoplay属性-->
      <source src="myAudio.mp3" type="audio/mpeg">
      <source src="myAudio.ogg" type="audio/ogg">
      <p>Your browser doesn't support HTML5 audio. Here is a <a href="myAudio.mp4">link to the audio</a> instead.</p>
  </audio>
  ```

- 新增多媒体视频标签：video

  ```js
  <video src="./media/video.mp4" controls="conrols"></video>
  ```

  - [ ] 谷歌浏览器把音频和视频标签的自动播放都禁止了
  - [ ] 谷歌浏览器中视频添加**muted**属性就可以自己播放了
  - [ ] 多媒体标签在不同浏览器下情况不同，存在兼容性问题

- 新增input标签

  | 属性值            | 说明                 |
  | ----------------- | -------------------- |
  | type="email"      | 限制输入为Email类型  |
  | type="url"        | 限制输入为URL类型    |
  | type="date"       | 限制输入为日期类型   |
  | type="time"       | 限制输入为时间类型   |
  | type="month"      | 限制输入为月类型     |
  | type="week"       | 限制输入为周类型     |
  | **type="number"** | 限制输入为数字类型   |
  | **type="tel"**    | 手机号码             |
  | **type="serach"** | 搜索框               |
  | type="color"      | 生成一个颜色选择表单 |

- 新增表单属性

  | 属性         | 值                 | 说明                                                         |
  | ------------ | ------------------ | ------------------------------------------------------------ |
  | required     | required           | 表示内容不能为空，必填                                       |
  | placeholder  | 提示文本（占位符） | 表单提示信息，存在默认值将不显示                             |
  | autofocus    | autofocus          | 自动聚焦属性，页面加载完成自动聚焦到指定表单                 |
  | autocomplete | off/on             | 当用户在字段开始键入时，浏览器基于之前键入过的值，应该显示出字段中填写的选项。默认已经打开，如autocomplete=“on",关闭autocomplete=“off"，需要防止表单内同时加上name属性，同时成功提交 |
  | multiple     | multiple           | 可以多文件提交                                               |

#### CSS3新增

1. css3属性选择器



| 选择符        | 简介                                  |
| ------------- | ------------------------------------- |
| E[att]        | 选择具有att属性的E元素                |
| E[att="val"]  | 选择具有att属性且属性值等于val的E元素 |
| E[att^="val"] | 匹配具有att属性、且值以val开头的E元素 |
| E[att$="val"] | 匹配具有att属性、且值以val结尾的E元素 |
| E[att*="val"] | 匹配具有att属性、且值中含有val的E元素 |

   ```js
   button{
       cursor:pointer;
   }
   button[disabled]{
       cursor:default;
   }
       
       input[type=search]{
       color:skyblue;
   }
   span[class^=black]{
       color:lightgreen;
   }
   span[class$=black]{
       color:lightsalmon;
   }
   span[class*=black]{
       color:lightseagreen;
   }
   ```

2. 结构伪类选择器

   | 选择符           | 简介                          |
   | ---------------- | ----------------------------- |
   | E:first-child    | 匹配父元素中的第一个子元素E   |
   | E:last-child     | 匹配父元素中的最后一个子元素E |
   | E:nth-child(n)   | 匹配父元素中的第n个子元素E    |
   | E:first-of-type  | 指定类型E的第一个             |
   | E:last-of-type   | 指定类型E的最后一个           |
   | E:nth-of-type(n) | 指定类型E的第n个              |

   ```js
   ul li:first-child{
       background-color:lightseagreen;
   }
   ul li:last-child{
       background-color:lightcoral;
   }
   ul li:nth-child(3){
       background-color:aqua;
   }
   ```

   - nth-child(n)参数n详解
     - [ ] 本质就是选中第几个子元素
     - [ ] n可以是数字、关键字、公式
     - [ ] n如果是数字，就是选中第几个
     - [ ] 常见的关键字有even偶数、odd单数
     - [ ] 但是第0个元素或者超出了元素的个数会被忽略

   ```js
   <style>
     /* 偶数 */
     ul li:nth-child(even) {
       background-color: aquamarine;
     }
   
     /* 奇数 */
     ul li:nth-child(odd) {
       background-color: blueviolet;
     }
   
     /*n 是公式，从 0 开始计算 */
     ul li:nth-child(n) {
       background-color: lightcoral;
     }
   
     /* 偶数 */
     ul li:nth-child(2n) {
       background-color: lightskyblue;
     }
   
     /* 奇数 */
     ul li:nth-child(2n + 1) {
       background-color: lightsalmon;
     }
   
     /* 选择第 0 5 10 15, 应该怎么选 */
     ul li:nth-child(5n) {
       background-color: orangered;
     }
   
     /* n + 5 就是从第5个开始往后选择 */
     ul li:nth-child(n + 5) {
       background-color: peru;
     }
   
     /* -n + 5 前五个 */
     ul li:nth-child(-n + 5) {
       background-color: tan;
     }
   </style>
   ```

   **nth-child与nth-of-type区别：**

   前者选择父元素里面的第几个子元素，不管是第几个类型；后者选择指定类型的元素。

   ```js
   <style>
     div :nth-child(1) {
       background-color: lightblue;
     }
   
     div :nth-child(2) {
       background-color: lightpink;
     }
   
     div span:nth-of-type(2) {
       background-color: lightseagreen;
     }
   
     div span:nth-of-type(3) {
       background-color: #fff;
     }
   </style>
   ```

3. 伪元素选择器

   ::before:在元素内部前面插入内容;

   ::after:在元素内部后面插入内容

   - 注意事项
     - [ ] before和after必须有content属性
     - [ ] before和after创建的是一个元素，但是属于行内元素
     - [ ] 创建出来的元素在dom中查不到，所以称为伪元素
     - [ ] 伪元素和标签选择器一样，权重为1

   ```js
   <style>
       div {
         width: 100px;
         height: 100px;
         border: 1px solid lightcoral;
       }
   
       div::after,
       div::before {
         width: 20px;
         height: 50px;
         text-align: center;
         display: inline-block;
       }
       div::after {
         content: '德';
         background-color: lightskyblue;
       }
   
       div::before {
         content: '道';
         background-color: mediumaquamarine;
       }
     </style>
   ```

4. 2D转换之translate

   - 2D转换是改变标签在二维平面上的位置和形状：

     移动：translate，旋转：rorate，缩放：scale

   - translate语法：

     x就是x轴上水平移动，y就是y轴上水平移动.

     ```js
     transform:translate(x,y)
     transform:translateX(n)
     transform:translateY(n)
     ```

   - **重要知识点**

     - [ ] 2D的移动主要是指水平、垂直方向上的移动

     - [ ] translate最大的优点就是不影响其他元素的位置

     - [ ] translate中的100%单位，是相对于本身的宽度和高度来进行计算的

     - [ ] 行内标签没有效果

       ```js
       div{
           background-color:lightseagreen;
           width:200px;
           height:100px;
       
       //平移
       //水平垂直移动100px
       //transform:translate(100px,100px);
       //水平移动100px
       //transform:translate(100px,0);
       //垂直移动100px
       //transform:translate(0,100px);
       //水平移动100px
       //transform:translateX(100px);
       //垂直移动100px
       //transform:translateY(100px);
       }
       ```

5. 2D 转换之旋转 rotate 

   - rotate旋转：2D旋转指的是让元素在二维平面内顺时针或者逆时针旋转

   - rotate语法：

     ```js
     transform:rotate(度数)//单位：deg
     ```

   - 重点知识点：

     - [ ] rotate里面跟度数，单位是deg
     - [ ] 角度为正时，顺时针，角度为负时，逆时针
     - [ ] 默认旋转的中心点是元素的中心点

     ```js
     img:hover{
         transform:rotate(360deg)
     }
     ```

   - 使用步骤：给元素添加转换属性transform，属性值为rotate

     ```js
     div{
         transform:rotate(30deg);//顺时针转30度
     }
     ```

   - 案例：三角形

     ```js
     p::before{
         content:'';
         position:absolute;
         right:20px;
         top:10px;
         width:10px;
         height:10px;
         borfer-right:1px solid #000;
         border-bottom:1px solif #000;
         transform:rotate(45deg);
     }
     ```

6. 设置元素旋转中心点

   - 语法：transform-origin： x y ;

   - 重要知识点

     - [ ] 注意后面的参数x和y用空格隔开
     - [ ] x y默认转换的中心点是元素的中心点(50% 50%)
     - [ ] 还可以给x y 设置像素或者方位名词(top bottom left right center)

   - 旋转案例：

     ```js
     transform-origrin: x y;
     ```

7. 2D转换之scale

   - scale的作用：用来控制元素的放大与缩小

   - 语法：

     ```js
     transform:scale(x,y);
     ```

   - 知识点：

     - [ ] 注意，x与y之间使用逗号分隔
     - [ ] transform：scale(1,1):宽高都放大一倍，相当于没有放大
     - [ ] transform：scale(2,2):宽高都放大二倍
     - [ ] transform：scale(2):如果只写了一个参数，第二个参数就和第一个一致
     - [ ] transform：scale(0.5,0.5):缩小
     - [ ] scale最大的优势：可以设置转换中心点缩放，默认以中心点缩放，而且不影响其他盒子

   - 代码演示：

     ```js
     div:hover{
         transform:scale(0.5,0.5)//数字是倍数的意思，不用加单位
     }
     ```

8. 2D转换综合写法以及顺序问题

   - 知识点：

     - [ ] 同时使用多个转换，其格式为transform：translate()rotate()scale()
     - [ ] 顺序会影响到转换的效果(先旋转会改变坐标轴方向)
     - [ ] 但我们同时有位置或者其他属性的时候，要将位移放到最前面

   - 代码演示：

     ```js
     div:hover{
         transform:translate(200px,0) rotate(360deg) scale(1.2)
     }
     ```

   #### 动画（animation）

   1. 什么是动画

      - 动画是css3中最具颠覆性的特征之一，可通过设置多个节点来精确的控制一个或者一组动画，从而实现复杂的动画效果

   2. 动画的基本使用

      - 先定义动画
      - 在调用定义好的动画

   3. 定义动画：用keyframes定义动画（类似定义类选择器）

      ```js
      @keyframes 动画名称{
          0%{
              width:100px;
          }
          100%{
              width:200px;
          }
      }
      ```

   4. 使用动画

      ```js
      div{
          //调用动画
          animation-name:动画名称;
          //持续时间
          animation-duration:持续时间;
      }
      ```

   5. 动画序列

      - 0%是动画的开始，100%是动画的完成，这样的规则就是动画序列
      - 在@keyframes中规定某项css样式，就由创建当前样式逐渐改为新样式的动画效果
      - 动画是使元素从一个样式逐渐变化为另一个样式的效果，可以改变任意多的样式任意多的次数
      - 用百分比来规定变化发生的时间，或用from和to，等同于0%和100%

      ```js
      <style>
          <div>{
          width: 100px;
          height: 100px;
          background-color:aquamarine;
          animation-name:move;
          animation-duration:0.5s;
      }
      	@keyframes move{
              0%{
                  transform:translate(0px)
              }
              100%{
                  transform:translate(500px, 0)
              }
      }
      </style>
      ```

   6. 动画常见属性：

      | 属性                      | 描述                                                         |
      | ------------------------- | ------------------------------------------------------------ |
      | @keyframes                | 规定动画                                                     |
      | animation                 | 所有动画属性的简写属性，除了animation-play-state属性         |
      | animation-name            | 规定@keyframes动画的名称（必须的）                           |
      | animation-duration        | 规定动画完成一个周期所花费的秒或毫秒，默认是0（必须的）      |
      | animation-timing-function | 规定动画的速度曲线，默认是“ease"                             |
      | animation-delay           | 规定动画何时开始，默认是0                                    |
      | animation-iteration-count | 规定动画被播放的次数，默认是1，还有infinite                  |
      | animation-direction       | 规定动画是否在下一周期逆向播放，默认是”normal",alternate逆播放 |
      | animation-play-state      | 规定动画是否正在运行或暂停。默认是"running",还有“pasused"    |
      | animation-fill-mode       | 规定动画结束后状态，保持forwards回到backwards                |

      ```js
      div{
          width:100px;
          height:100px;
          background-color:aquamarine;
          //动画名称
          animation-name:move;
          //动画花费时间
          animation-duration:2s;
          //动画速度曲线
          animation-timing-function:ease-in-out;
          //动画等待多久执行
          animation-delay:2s;
          //规定动画播放次数 infinite：无限循环
          animation-iteration-count:infinite;
          //是否逆行播放
          animation-direction:alternate;
          //动画结束之后的状态
          animation-fill-mode:forwards;
      }
      div:hover{
          //规定动画是否暂停或播放
          animation-play-state:paused;
      }
      ```

   7. 动画简写方式

      ```js
      animation:name duration timing-function delay iteration-count direction fill-mode
      //animation：动画名称 持续时间 运动曲线  何时开始  播放次数  是否反方向  动画起始或者结束的状态;
      animation: myfirst 5s linear 2s infinite alternate;
      ```

      知识点：

      - 简写属性里面不包含 animation-play-state  
      - 暂停动画：animation-play-state:   puased;   经常和鼠标经过等其他配合使用 
      - 想要动画走回来 ，而不是直接跳回来：animation-direction   ：  alternate 
      - 盒子动画结束后，停在结束位置：  animation-fill-mode  ：   forwards ；

   8. 速度曲线细节

      animation-timing-function:规定动画的速度曲线，默认是”ease“

      | 值          | 描述                                           |
      | ----------- | ---------------------------------------------- |
      | linear      | 动画从头到尾的速度是相同的，匀速               |
      | ease        | 默认，动画从低速开始，然后加快，在速度前变慢。 |
      | ease-in     | 动画以低速开始                                 |
      | ease-out    | 动画以低速结束                                 |
      | ease-in-out | 动画以低速开始和结束                           |
      | steps()     | 指定了时间函数中的间隔数量（步长）             |

   ```js
   div{
       width:0 px;
       height:50px;
       line-height:50px;
       white-space:nowrap;
       overflow:hidden;
       background-color:aquamarine;
       animation:move 4s steps(24) forwards;
   }
   @keyframes move{
       0%{
           width:0 px;
           
       }
       100%{
           width:480px;
       }
   }
   ```

   ##### 奔跑的熊大

   ```js
   <!DOCTYPE HTML>
   <html lang="en">
       <head>
       	<meta charset="UTF-8">
            <meta name="viewport" content="width=device-width,initial-scale=1.0">
            <meta http-eqiv="X-UA-Compatible" content="ie=edge">
            <title>Document</title>
   		<style>
                body{
                    background-color:#ccc;
                }
               div{
                   position:absolute;
                   width:200px;
                   height:100px;
                   background:url(media/bear.png) no-repeat;
                   /*我们元素可以添加多个动画，用逗号分隔*/
                   animation:bear .4s steps(8) infinite, move 3s forwards;
               }
               @keyframes bear{
                   0%{
                       background-position: 0 0;
                   }
                   100%{
                       background-position:-1600px 0;
                   }
               }
               @keyframes move{
                   0%{
                       left:0;
                   }
                   100%{
                       left:50%;
                       //margin-left:-100px;
                       transform:translateX(-50%);
                   }
               }
          </style>
       </head>
   <body>
         <div></div>
   </body>
   </html>
   ```

   ##### 3D转换

   1. 特点：近大远小，物体和面遮挡不可见

   2. 三维坐标系：

      - x轴：水平向右--注意：x轴右边是正值，左边是负值
      - y轴：垂直向下--注意：y轴下面是正值，上面是负值
      - z轴：垂直屏幕--注意：往外边是正值，往里是负值

   3. 主要知识点

      - 3D位移：translate3d(x,y,z)
      - 3D旋转：rotate3d(x,y,z)
      - 透视：perspctive
      - 3D呈现：transfrom-style

   4. 3D移动translate3d

      - 3d移动就是在2D移动的基础上多加了一个可以移动的方向，就是z轴方向
      - transform：translateX(100px):仅仅是在x轴移动
      - transform：translateY(100px):仅仅是在y轴移动
      - transform：translateZ(100px):仅仅是在z轴移动
      - transform：translate3d(x,y,z):x,y,z移动的距离；注意，**xyz对应的值不能省略，不需要填写用0进行填充**

      ```js
      transform:translate3d(x,y,z)
      transform:translate3d(100px,100px,0)
      ```

   5. 透视perspective

      知识点：

      - 如果想要网页产生3D效果需要透视（理解成3D物体投影的2D平面上）

      - 模仿人类的视觉位置，可以安排一只眼睛取看

      - 透视也称为视距，就是人的眼睛到屏幕的距离

      - 透视的单位是像素

      - **透视需要写在被视察元素的父盒子上面**

        ```js
        body{
            perspective:100px;
        }
        ```

   6. translateZ:

      translateZ与perspective区别：perspective给父级进行设置，translateZ给子元素进行设置不同的大小

   7. 3D旋转rotateX

      语法：

      - transform:rotateX(45deg)：沿着x轴正方向旋转 45度 

      - transform:rotateY(45deg) ：沿着y轴正方向旋转 45deg

      - transform:rotateZ(45deg) ：沿着Z轴正方向旋转 45deg

      - transform:rotate3d(x,y,z,deg)： 沿着自定义轴旋转 deg为角度（了解即可）

        ```js
        div{
            perspective:300px;
        }
        img{
            display:block;
            margin: 100px auto;
            transition: all 1s;
        }
        img:hover{
            transform:rotateX(-45deg)
        }
        ```

      - 左手准则：左手的手拇指指向x轴的正方向，其余手指的弯曲方向就是该元素沿着x轴旋转的方向

   8. 3D旋转rotateY

      ```js
      div{
          perspective:500px;
      }
      img{
          display:block;
          margin: 100px auto;
          transition: all 1s;
      }
      img:hover{
          transform:rotateY(180deg)
      }
      ```

      - 左手准则：左手的拇指指向y轴的正方向，其余手指的弯曲方向就是该元素沿着y轴旋转的方向(正值)

   9. 3D旋转rotateZ

      ```js
      div{
          perspective:500px;
      }
      img{
          display:block;
          margin: 100px auto;
          transition: all 1s;
      }
      img:hover{
          transform:rotateZ(180deg)
      }
      ```

      - rotate3d

        transform：rotate3d(x,y,z,deg)--沿着自定义轴旋转deg为角度

      - x,y,z表示旋转轴的矢量，是标识你是否希望沿着该轴进行旋转，最后一个标识旋转的角度：

        transform:rotate3d(1,1,0,180deg)--沿着对角线旋转45deg

        transform:rotate3d(1,0,0,180deg)--沿着x轴旋转45deg

      ```js
      div{
          perspective:500px;
      }
      img{
          display:block;
          margin:100px auto;
          transition:all 1s;
      }
      img:hover{
          transform:rotate3d(1,1,0,180deg)
      }
      ```

   10. 3D呈现transform-style:

       - 控制子元素是否开启三维立体环境
       - transform-style：flat子元素不开启3d立体空间 默认的
       - transform-style：perserve-3d;子元素开启立体空间
       - 代码写给父级，但是影响的是子盒子
       - 这个属性很重要，后面必用

   

   

   

   

   

   

   