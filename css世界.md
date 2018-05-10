# css世界
## 流、元素和基本尺寸
### 块级元素

块级元素包括div(display:block)、li(display:list-item)以及table(display:table)等元素，因此可以看出块级元素和display:block的元素并不是同一个概念。块级元素的基本特征是一个水平流上只能单独显示一个元素，多个块级元素则换行显示。

#### 盒子

所有的块级元素都有一个主块级盒子，list-item除此之外还有一个附加盒子，之所以list-item会出现项目符号是因为生成了一个附加盒子，学名“标记盒子”，专门用来放圆点、数字这些项目符号。

需要注意的是每个元素都有两个盒子，**外在盒子**和**内在盒子**，外在盒子负责元素是可以一行显示还是只能换行显示，内在盒子负责宽高以及内容呈现等。内在盒子更专业的叫法是“容器盒子”。因此display:block的元素的盒子实际上由外在“块级盒子”和内在的“块级容器盒子”组成。display:inline的元素则内外均是“内联盒子”，而display:inline-block的元素外面的盒子是inline级的，内在的盒子是block级的。

> 实际上根据inline-block的命名，block可以命名为block-block，而table可以命名为block-table。

那么可以清楚的知道inline-table的元素外面是内联盒子，而里面是table盒子

``` html
<style>
  .inline-table {
    display: inline-table;
    border: 1px solid black;
    width: 200px;
  }

  .inline-table>p {
    display: table-cell;
  }
</style>
<body>
  <div class="inline-table">
    <p>第一列</p>
    <p>第二列</p> 
  </div> 
  <div class="inline-table">
    <p>第一列</p>
    <p>第二列</p> 
  </div>
</body>
```

### width/height的作用

#### width:auto

width的默认值为auto，但是却包含了4种不同的宽度表现

- 1) 充分可利用空间。例如div、p这些元素的宽度默认是100%于父容器的，这种行为叫做fill-available。
- 2) 收缩和包裹。浮动、绝对定位、inline-block元素或table元素，宽度收缩到合适，CSS3中叫做fit-content。
- 3) 收缩到最小。在规范中被描述为min-content。例如table-layout为auto的表格。

``` html
<style>
  table {
    margin: 0 auto;
    width: 280px;
    text-align: left;
  }

  td { /* width: auto; */
    border: 1px solid black;

  }
</style>
<body>
  <table>
    <td>一列一列一列一列一列</td>
    <td>非一列非一列非一列非一列非一列非一列非一列非一列非一列非一列非一列非一列非一列非一列非一列非一列</td>
    <td>非一列非一列非一列非一列非一列非一列非一列非一列非一列非一列非一列非一列非一列非一列非一列非一列</td>
  </table>
</body>
```

> 当每一列空间都不够的时候，文字能断就断，中文是随便断的，英文单词不能断。于是第一列每个字都被断开了。

- 4) 超出容器限制。以上3种情况尺寸都不会主动超过父级容器宽度，但是存在特殊情况，例如内容很长的连续的英文和数字，或者内联元素被设置了white-space:norwap(文本不会换行，文本会在在同一行上继续，直到遇到<br>标签为止)。

``` html
<style>
  div {
    border: 1px solid black;
  }
  .father {
    width: 150px;
    white-space: nowrap;
  }
  .child {
    display: inline-block;
  }
</style>
<body>
  <div class="father">
    <span class="child">人之初，性本善。人之初，性本善。人之初，性本善。</span>
  </div>
  <div>
    <span >人之初，性本善。人之初，性本善。人之初，性本善。</span>
  </div>
</body>
```
> 注意span元素的width为auto。

##### 外部尺寸与流体特性(block容器)

- 1) 在一个容器中倒入水，水会均匀的铺满整个容器，这就是流动特性。将一个div元素放置到页面上，也会像水流一样铺满容器。这就是block容器的流动性。因此下面的写法没有任何必要

``` html
<style>
a {
  display: block;
  width: 100%; /* 没有必要 */
}
</style>
```

甚至要注意，width:auto和width:100%是有区别的，width:100%会使元素的流动性丢失。

``` html
<style>
  .nav, .nav-100 {
    background-color: coral;
  }
  a {
    display: block;
    margin: 0 10px;
    padding: 10px;
    border-bottom: 1px solid brown;
    border-top: 1px solid gold;
    color: white;
  }
  a:first-child {
    border-top: 0;
  }
  a + a + a {
    border-bottom: 0;
  }
  .nav-100>a {
    width: 100%; /* width继承了nav的宽度，因为有margin和padding，所以会超出容器大小 */
  }
</style>
<body>
  <h1>流动性(nav的width:auto)</h1>
  <div class="nav">
    <a href="">导航1</a>
    <a href="">导航2</a>
    <a href="">导航3</a>
  </div>
  <h1>非流动性(nav的width:100%)</h1>
  <div class="nav-100">
    <a href="">导航1</a>
    <a href="">导航2</a>
    <a href="">导航3</a>
  </div>
</body>
```
2) 格式化宽度。暂不说明。

##### 内部尺寸与流体特性(inline-block元素，浮动元素和绝对定位元素)

假如元素里面没有内容，width:auto时宽度是0，那就是应用“内部尺寸”。和外部尺寸表现的特性不同，外部尺寸的影响是和容器元素有关，而内部尺寸则是和元素本身的内容有关。

- 1) 包裹性。按钮是“包裹性”最好的例子。按钮文字越多宽度越宽(内部尺寸特性)，如果文字足够多，会在容器的宽度处自动换行(自适应特性)。


``` html
<style>
  .box {
    width: 240px;
  }
</style>
<body>
  <div class="box">
    <button>按钮</button>
    <button>文字多一点的按钮</button>
    <button>button按钮button按钮button按钮button按钮button按钮button按钮button按钮button按钮button按钮</button>
    <input type="button" value="input按钮input按钮input按钮input按钮input按钮input按钮input按钮input按钮input按钮input按钮input按钮" />
  </div>
</body>
```
> button元素默认display:inline-block，input按钮white-space默认是pre(类似于pre标签的行文，空白和换行等会被保留)。因此按钮会自适应换行，而input不会。


对于一个元素如果其display属性值是inline-block，那么其里面的内容再多，只要是正常文本，宽度也不会超过容器。按钮就是典型的inline-block元素。包裹性对实际开发的作用

``` html
<style>
  div {
    padding: 10px;
    border: 1px solid gold;
    margin: 10px;
    text-align: center;
    width: 300px;
  }

  p {
    display: inline-block; /* 产生包裹性 */
    text-align: left;
  }
</style>
<body>
  <div>
    <p>居中显示的元素</p>
  </div>
  <div>
    <p>内容过多时居左显示内容过多时居左显示内容过多时居左显示</p>
  </div>
</body>
```

> 第一个p元素产生了居中的效果是因为整个p元素具有包裹性并且整个p元素居中，第二个p元素是因为inline-block元素内容过多宽度也不会超过容器，因此p元素虽然居中，但是p元素填充了整个div元素，而p元素本身是居左的，导致视觉上div元素的内容居左。

> 除了inline-block元素，浮动元素和绝对定位元素都具有包裹性。

- 2) 首选最小宽度。

如果width设置为0，inline-block元素的宽度并不一定是0，因为图片和文字的权重远大于布局，此时表现的宽度就是首选最小宽度了。

 - 东亚文字(例如中文)的宽度为每个汉字的最小宽度。
 - 西方文字的最小宽度由特定的连续的英文字符单元决定，会终止于空格、短横线、问号以及其他非英文字符等(如果想要英文字符和中文字符一样，每个字符都用最小宽度单元，可以使用word-break: break-all)。

``` html
<style>
  .ao,.tu {
    display: inline-block;
    width: 0; /* width设置为0，仍然有宽度 */
    font-size: 14px;
    line-height: 18px;
    margin-left: 60px;
  }
  .ao:before,.tu:before {
    outline: 2px solid gold;
  }
  .ao:before {
    content: "love你love"
  }
  .tu {
    direction: rtl;
  }
  .tu:before {
    content: "我love你"
  }
</style>
<div>
  <span class="ao"></span>
  <span class="tu"></span>
</div>
```

- 3) 最大宽度。

最大宽度就是元素可以有的最大宽度，等同于“包裹性”元素设置了white-space: norwap声明后的宽度，如果内部没有块级元素或者块级元素没有设定宽度值，“最大宽度”实际上是最大连续内联盒子的宽度。连续内联盒子是指display为inline、inline-block、inline-table等的元素(内联盒子)连在一起且中间没有任何的换行标签br或其他块级元素。

``` html
<body>
  <div>
    我是匿名内联盒子<span>我在inline标签内</span>
    <button>我是一个内联盒子</button><img src="" alt="我是图片">
    到这里连续内联盒子结束
    <br>
    我是匿名内联盒子<p>不好意思，这里是块级元素，连续内联盒子断开</p>
  </div>
</body>
```

#### width值作用的细节

之前所讲的都是width:auto的作用。现在来看看width设置具体的值会有哪些表现。

盒子可以分为块状盒子、内联盒子以及外在盒子、内在盒子。其中内在盒子又可以被分为4个盒子，分别是content box(content-box)、padding box(padding-box)、border box(border-box)以及margin box(没有名称且目前也没有任何使用场景, margin的背景永远是透明的，不可能作为background-clip或background-origin的属性值(现有属性有padding-box、border-box和content-box)出现)。

并且width的值作用在了content box(content box是环绕着width和height给定的矩形)上。

- 显示设置了width的值会使流动性丢失。
- 元素实际的大小可能超出了width的大小(加上padding和border值)，因此容器出现页面布局错位的问题，可以采用“宽度分离原则”。

#### 宽度分离原则

css的width属性不与padding/border(有时候也包括margin)属性共存。

``` html
<style>
  .father {
    width: 180px; /* width独占一层标签，father元素用于定宽 */
  }
  .son { /* width:auto，由于流动性使得son填充满整个father，大小就是180px */
    background: goldenrod;
    border: 1px solid green;
    padding: 20px;
  }
</style>
```

#### 改变width/height作用的box-sizing(盒尺寸)

宽度分离原则会多使用一层标签，因此可以采用width作用细节的box-sizing属性。之前说width默认是作用在content-box上的，但是box-sizing属性可以改变width的作用盒子，理论上box-sizing也可以作用在其他三个盒子上，但是实际上浏览器厂商并没有全部实现

``` css
.box {
  box-sizing: border-box; /* 全部支持 */
  box-sizing: content-box; /* 默认值 */
  box-sizing: padding-box; /* firefox曾经支持 */
  box-sizing: margin-box; /* 从未支持 */
}
```

``` html
<style>
  .border-box {
    box-sizing: border-box;
    width: 100px;
    background: pink;
    border: 5px solid greenyellow;
    padding: 10px;
  }

  .content-box {
    /* box-sizing: content-box; */
    margin-top: 10px;
    width: 100px;
    background: pink;
    border: 5px solid greenyellow;
    padding: 10px;
  }
</style>
<body>
  <div class="border-box">100px</div>
  <div class="content-box">130px</div>
</body>
```

> box-sizing为什么不支持margin-box，因为规范中说“margin的背景永远都是透明的”，如果box-sizing支持了margin-box，那么background-oirigin就会生效，背景图片就可以填充到margin区域，这不符合规范的要求。

##### 如何评价 *{ box-sizing: border-box; }

- 易产生没必要得消耗。例如对普通内联元素(非图片等替换元素)，box-sizing无论是什么值，对其渲染表现都没有影响，因此*对这些元素而言就是没有必要的消耗。同时对于input元素而言其border-sizing就是border-box，因此也是一种没有必要的消耗。

##### box-sizing的真正作用

替换元素得特性之一就是尺寸由内部元素决定，且无论其display属性值是inline还是block。而对于非替换元素，如果display属性值为block，则会具有流动性，宽度由外部尺寸决定，但是替换元素的宽度则不受display水平影响。

``` html
<style>
  .box {
    width: 100px;
  }
  textarea {
    display: block; /* 尺寸并没有流动性特性 */
  }
</style>
<body>
  <div class="box">
    <textarea name="" id="" cols="30" rows="10"></textarea>
  </div>
</body>
```

如果要使textarea的尺寸自适应box盒子的尺寸，就需要给textarea一个width:100%，那么如果想要textarea拥有padding和border，那么势必要使textarea的width、padding和border共存，如果此时不用box-sizing，而是使用宽度分离原则，那么势必要再套一层盒子，这层盒子用于控制padding和border，然后textarea控制width:100%。

``` html
<style>
  .box {
    width: 280px;
    margin: 0 auto;
  }
  .textarea {
    padding: 10px;
    border: 1px solid #d0d0d5;
    border-radius: 4px;
    background-color: #fff;
  }
  textarea {
    width: 100%;
    padding: 0;
    border: 0;
    outline: 0;
    background: none;
    resize: none;
  }
</style>
<div class="box">
  <div class="textarea">
    <textarea name="" id="" cols="30" rows="10"></textarea>
  </div>
</div>
```

此时如果使用box-sizing，就可以轻松解决问题

``` html
<style>
  .box {
    width: 280px;
    margin: 0 auto;
  }
  textarea {
    width: 100%;
    box-sizing: border-box;
    padding: 10px;
    border: 1px solid #d0d0d5;
    border-radius: 4px;
    outline: 0;
    background: none;
    resize: none;
  }
</style>
<div class="box">
  <textarea name="" id="" cols="30" rows="10"></textarea>
</div>
```


所以box-sizing的最大初衷是解决替换元素的宽度自适应问题，因此以下css重置比较合适

``` css
input, textarea, img, vedio, object {
  box-sizing: border-box;
}
```


#### height:auto

height:auto要比width:auto简单一些，因为css的默认流式水平方向的，宽度是稀缺的，高度是无限的。

#### height:100%

如果父元素没有具体的高度值(height:auto)，height:100%会无效。css规范指出：如果包含块的高度没有显示指定(高度由内容决定)，并且该元素不是绝对定位，则计算值为auto。因此auto*100/100=NaN。需要注意的是width没有在css规范中明确定义。但是基本上所有浏览器都表现出如下行为：



``` html
<style>
  .box {
    display: inline-block;
    white-space: nowrap;
    background-color: #d0d0d5;
    padding: 10px;
  }

  .text {
    display: inline-block;
    width: 100%;
    background-color: aquamarine;
    color: white;
  }
</style>
<div class="box">
  <span>hello world</span>
  <span class="text">hello width 100%</span>
</div>
```
> 此时width的渲染流程：先渲染父元素，此时子元素的width:100%并没有渲染，宽度是两个span元素文字内容叠加的宽度，等渲染到最后一个span时，父元素的宽度width根据两个span元素的宽度已经固定，因此此时最后一个span的宽度就是父元素已经固定好的宽度(content-box的宽度)。因此可以发现height:100%和width:100%的浏览器渲染行为是不一样的。其实这个宽度的行为是css规范中的一种未定义行为，浏览器可以根据自己的理解自由发挥，好在浏览器都统一了这一行为(按照包含块(width:100%)的真实的计算值作为百分比计算的基数)。


##### 如何让元素支持height:100%的效果

- 设定显示的高度值

``` css
div {
  height: 600px; /* 显示设置高度值 */
}

html, body {
  height: 100%; /* 设置可以生效的百分比高度值 */
}
```

- 使用绝对定位

``` css
div {
  height: 100%; /* 此时height:100%就会有计算值，即使祖先元素的height计算为auto */
  position: absolute;
}
```
> 绝对定位元素的百分比计算是相对于padding box的(会把padding大小值计算在内)，而非绝对定位元素是相对于content box计算的。

``` html
<style>
  .box {
    height: 160px;
    width: 200px;
    padding: 30px;
    box-sizing: border-box; /* 注意是border-box */
    background-color: pink;
    margin: 10px;
    position: relative;
    border: 1px solid blanchedalmond;
  }

  .normal {
    height: 100%;
    background-color: aquamarine;
  }

  .absolute {
    height: 100%;
    position: absolute;
    background-color: aquamarine;
  }
</style>
<!-- width:auto流动性 -->
<div class="box">
  <div class="normal">高度100px</div>
</div>
<!--  width:auto浮动元素和绝对定位元素具有包裹性 -->
<div class="box">
  <div class="absolute">高度158px</div>
</div>
```

> 绝对定位元素height:100%的计算方式 160px - 2px, 因此不包裹border的高度。


使用绝对定位实现左右各一半且占满整个容器高度

``` html
<style>
  html,body {
    height: 100%;
    margin: 0;
    padding: 0;
    position: relative;
  }
  .left, .right {
    height: 100%; /* 占满容器的高度 */
    width: 50%;
    position: absolute;
    top: 0;
    opacity: .5;
  }
  .left {
    background-color: pink;
    left: 0;
  }
  .right {
    background-color: blanchedalmond;
    right: 0;
  }
</style>
<body>
  <div class="left"></div>
  <div class="right"></div>
</body>
```


### min-width/max-width和min-height/max-height的作用

#### min-width/max-width

min-width/max-width主要用于自适应布局或流体布局。现代桌面显示器分辨率越来越大，960px网页设计已经显得小家碧玉，而设置成1400px的大尺寸网页宽度也不合适，为了兼顾笔记本用户和大屏显示器用户，一种特定区间内得自适应布局方案是使宽度在1200~1400像素范围内。

``` css
html,body {
  max-width: 1400px;
  min-width: 1200px;
  margin: 0 auto;
}
```
例如防止图片在移动端展示过大影响体验

``` css
img {
  max-width: 100%;
  height: auto !important; /* 必需，强制height:auto可以保证宽度不超出的同时图片保持原来的比例，问题是加载时图片占据高度会从0变成计算高度，图片会有明显得瀑布式下落 */
}
```


#### min-/max-初始值

min-width/min-height的初始值是auto(MDN和W3C文档显示的是0，浏览器表现都是auto)，max-width/max-height的初始值是none，注意width和height的初始值是auto。证明min-height的值默认是auto不是0：

``` html
<style>
  .box {
    transition: min-height .3s;
    background-color: pink;
  }
  .box:hover {
    min-height: 300px;
  }
</style>
<div class="box">
  无动画效果
</div>
```

``` html
<style>
  .box {
    min-height: 0;
    transition: min-height .3s;
    background-color: pink;
  }
  .box:hover {
    min-height: 300px;
  }
</style>
<div class="box">
  有动画效果
</div>
```

max系列设置成none而不是auto，是因为auto容器限制子元素的高度和宽度，例如子元素800px，父元素400px，如果父元素得max-height是auto，那么使用和width一样的渲染规则，子元的max-height的值就是400px，因为max-height会覆盖height，因此子元素的800px就失效了。

#### 超越!important，超越最大


``` html
<style>
  .box {
    max-width: 200px;
    background-color: pink;
  }
</style>
<div class="box" style="width: 400px !important;">
  width为200px
</div>
```

>从这里可以看出max-width的权重超越了带有!important声明的width。


``` html
<style>
  .box {
    min-width: 300px;
    max-width: 200px;
    background-color: pink;
  }
</style>
<div class="box" style="width: 400px !important;">
  width为300px
</div>
```

>当min-width和max-width起冲突的时候，min-width的权重超越了max-width。

#### 任意高度元素的展开收起的动画技术

如果高度不固定的情况下，height:0到height:auto是无法计算过度和动画效果的。此时可以利用max-height形成动画效果，只要max-height的值永远大于内容值就行


``` html
<style>
  .box {
    background-color: pink;
    max-height: 100px;
    overflow: hidden;
    transition: max-height .25s;
  }

  .box:hover {
  max-height: 666px; /* 足够高 */
  }
</style>
<div class="box" style="width: 400px !important;">
  产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果
  产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果
  产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果
  产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果
  产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果产生动画效果
</div>
```

> 提示：max-height尽量不要设置太大，max-height被overflow裁剪的部分也会计算在动画时间内，因此被裁剪部分执行的动画并没有实际视觉效果，容器给人延迟感。


### 内联元素

#### 哪些是内联元素

- 定义。内联元素是指“外在盒子”是inline级内联盒子的元素。所以display值为inline、inline-block、inline-table的元素都是内联元素，button元素默认display:inline-block，img元素默认display:inline，因此这些元素是内联元素。
- 表现。内联元素的典型特征是可以和文字在一行显示。因此文字、图片、按钮和输入框、下拉框等原生表单控件都是内联元素。


#### 内联盒模型

``` html
<p>匿名内联盒子<span>内联盒子</span></p>
```

> p元素的内部是【行框盒子】，【行框盒子】由一个个的【内联盒子】组成。而【包含盒子】(containing block)则是由一行一行的【行框盒子】组成。

##### 幽灵空白节点

此示例中，第一个div有高度，而第二个div的高度是0，是因为第一个div的span元素有一个宽度为0的空白字符(幽灵空白节点)。

``` html
<style>
  div {
    background-color: pink;
  }

  span {
    display: inline-block;
  }
</style>
<div><span></span></div>
<div></div>
```

>该空白节点实际上也是一个盒子，是一个存货在于每个“行框盒子”前面，同时具有该元素字体和行高属性的0宽度的内联盒。



## 盒尺寸四大家族

盒尺寸的4个盒子content box、padding box、border box和margin box分别对应content、padding、border和margin属性。

### 深入理解content
 
#### content与替换元素

根据是否具有可替换内容，可以把元素分为替换元素和非替换元素。通过修改某个属性值呈现的内容就可以被替换的元素称为“替换元素”。例如img、object、video、iframe、select以及表单元素input、textarea都是典型的替换元素。

1. 替换元素的特性：
- 内容的外观不受页面上的css影响
- 有自己的尺寸
- 在很多css属性上有自己的一套表现规则，例如vertical-align的默认值是baseline，定义为字符x下边缘，但是替换元素的内容往往不可能含有字符x，替换元素的基线就被定义为元素的下边缘。

2. 替换元素默认的display值

所有替换元素都是内联水平元素。但是替换元素默认的display值根据不同的元素有一定的差异

| 元素      |     chrome |   ie   |
| :-------- | --------:| :------: |
| img    |   inline |  inline  |
| iframe    |   inline |  inline  |
| vedio    |   inline |  inline  |
| select    |   inline-block |  inline-block  |
| input    |   inline |  inline  |
| range/file(input)    |   inline |  inline  |
| hidden(input)    |   none |  none  |
| button    |   inline-block |  inline-block  |
| textarea    |   inline-block |  inline-block  |

> 替换元素的display是inline、block和inline-block中的任意一个，其尺寸计算规则都是一样的。


3. 尺寸计算规则

- 固有尺寸。图片和视频等作为一个独立文件有自己的宽度和高度。
- HTML尺寸。通过HTML原生属性改变元素尺寸，例如img的width和height属性，input的size属性，textarea的cols和rows属性等。
- css尺寸。通过设置css的width和height或者max-width/min-width和max-height/min-height设置尺寸，对应盒尺寸的content box。

> 三个尺寸的优先级分别是 css尺寸 > HTML尺寸 > 固有尺寸。

- 默认是图片的尺寸在起作用。

``` html
<body>
  <p>固有尺寸：540px X 258px</p>
  <img src="https://www.baidu.com/img/bd_logo1.png" alt="">

</body>
```

- 没有CSS尺寸的前提下，HTML尺寸起作用。

``` html
<body>
  <p>HTML尺寸：540px X 258px</p>
  <img src="https://www.baidu.com/img/bd_logo1.png" width="128" height="96" alt="">
</body>
```

- 三者尺寸都有的前提下，CSS尺寸起作用。需要注意如果CSS尺寸只设置了宽度或高度，则依然按照图片固有尺寸的宽高比例显示。

``` html
<p>固有尺寸：540px X 258px</p>
<img src="https://www.baidu.com/img/bd_logo1.png" alt="">
<p>HTML尺寸：128px X 96px</p>
<img src="https://www.baidu.com/img/bd_logo1.png" width="128" height="96" alt="">
<p>CSS尺寸：200px X 100px</p>
<img class="cssContent" src="https://www.baidu.com/img/bd_logo1.png" width="128" height="96" alt="">
```
- 如果上述条件都不符合，则最终宽度表现为300px，高度变现为150px。例如<vedio></vedio>（chrome下是0x0）、<canvas></canvas>。

- 内联替换元素和块级替换元素使用上面同一套尺寸计算规则。


``` html
<style>
.cssContent {
  width: 200px;
  height: 100px;
}

.cssBlock {
  display: block;
}
</style>

<body>
  <p>固有尺寸：540px X 258px</p>
  <img src="https://www.baidu.com/img/bd_logo1.png" alt="">
  <p>HTML尺寸：128px X 96px</p>
  <img src="https://www.baidu.com/img/bd_logo1.png" width="128" height="96" alt="">
  <p>CSS尺寸：200px X 100px</p>
  <img class="cssContent" src="https://www.baidu.com/img/bd_logo1.png" width="128" height="96" alt="">
  <vedio></vedio> 
  <canvas></canvas>
  <p>宽度并没有默认的100%</p>
  <img class="cssBlock" src="https://www.baidu.com/img/bd_logo1.png" alt="">
</body>
```

> 尽管img变成了块级元素，但是width并没有变成100%。

- 图片有例外情况

``` html
<body>
  <img>
  <img src="">
</body>
```

> chrome下是0px X 0px，而ie下是28px X 30px。firefox下是0px X 19px。 按照规范尺寸则应该是300px X 150px。需要注意src=""是会发起请求的，而src属性缺省则不会发起请求。


``` html
<style>
  img {
    width: 200px;
    height: 100px;
  }
</style>
<body>
  <img>
</body>
```

> ie和chrome下css样式生效，而在firefox下还是0px X 19px。解决方法是将img显示为内联块级元素inline-block。

- 无法改变替换元素内容的固有尺寸。

``` html
<style>
  div:before {
    width: 200px;
    height: 100px;
    content: url("https://www.baidu.com/img/bd_logo1.png")
  }
</style>
<body>
  <div></div>
</body>
```

> 图片的尺寸是固有的，无法被改变。

既然图片是固有尺寸，为什么修改img的width和height能修改图片的大小，可以查看
http://www.zhangxinxu.com/wordpress/2015/03/css3-object-position-object-fit/。

4. 替换元素和非替换元素

- 替换元素和非替换元素之间只隔了一个src属性。如果把img元素的src属性去掉，就成了和span类似的普通内联元素，也就是非替换元素。


``` html
<style>
  img {
    outline: 1px solid;
    display: block;
  }
</style>
<body>
  <img>
  <img alt="img">
  <img alt="chrome 100%">
</body>
```
> firefox下的宽度是100%自适应的。chrome带特定的alt条件能够宽度自适应。


- 替换元素和非替换元素之间只隔了一个CSS content属性。

``` html

<style>
  img {
    content: url("https://www.baidu.com/img/bd_logo1.png");
    width: 100px;
    height: 200px;
  }
</style>
<body>
  <img>
</body>
```
> 在chrome下此时和“<img src="https://www.baidu.com/img/bd_logo1.png">”一样。

``` html
<style>
img:hover {
  content: url(https://img02.sogoucdn.com/app/a/100520122/a363a0b1_loulan.gif);
}
</style>
<body>
  <img src="https://www.baidu.com/img/bd_logo1.png">
</body>
```

> 在chrome下此时利用content属性把图片的内容替换掉。

5. content与替换元素关系剖析

content属性生成的对象称为“匿名替换元素”，因此content属性生成的内容都是替换元素。

- 使用content生成的文本无法被选中。和设置了user-select:none声明一样。
- 不能左右:empty伪类。
- content动态生成值无法获取。

#### content内容生成技术

"content内容生成技术"也称为":before/:after"伪元素技术。

1. content辅助元素生成

``` html
<style>
div:before {
  content: ''
}
</style>
<body>
  <div></div>
</body>
``` 

> 利用空字符串和其他CSS代码来生成辅助元素，或实现图形效果或实现特定布局。与使用HTML标签相比这样做的好处是HTML代码会显得更加干净和精简。

``` html
<style>
  div:after {
    content: '';
    display: table; /* 也可以是block */
    clear: both;
  }
</style>
<body>
  <div></div>
</body>
```
> 清除浮动。


2. content字符内容生成
3. content图片生成
4. 了解content开启闭合符号生成
5. content attr属性值内容生成

``` html
<style>
  img:before {
    content: attr(data-title); /* 生成alt内容 */ 
  }

  div:after {
    content: attr(data-title); /* 生成data-title内容 */ 
  }
</style>
<body>
  <img src="https://www.baidu.com/img/bd_logo1.png" data-title="百度图片" />
  <div data-title="data-title content">div content</div>
</body>
```

6. 深入理解content计数器

- counter-reset: 计数器重置
- counter-increment: 计数器递增
- counter()/counters():  方法，用于显示计数


``` html
<style>
  .counter {
    counter-reset: ziyi2 2;
    counter-increment: ziyi2;
    font-size: 32px;
    font-family: Arial Black;
    color: pink;
  }

  .counter:before {
    content: counter(ziyi2);
  }
</style>
<body>
  <p>ziyi2: </p>
  <p class="counter"></p>
</body>
```

> 这里显示3。


``` html
<style>
  .counter {
    counter-reset: ziyi2 2 ziyi3 3;
    counter-increment: ziyi2 ziyi3;
    font-size: 32px;
    font-family: Arial Black;
    color: pink;
  }

  .counter:before {
    content: counter(ziyi2);
  }
  .counter:after {
    content: counter(ziyi3);
  }
</style>
<body>
  <p>ziyi2: </p>
  <p class="counter"></p>
</body>
```

> 这里显示34。

``` html
<style>
  .counter {
    counter-reset: ziyi2 2 ziyi3 3;
    counter-increment: ziyi2 ziyi3;
    font-size: 32px;
    font-family: Arial Black;
    color: pink;
  }

  .counter:before {
    counter-increment: ziyi2 ziyi3;
    content: counter(ziyi2);
  }
  .counter:after {
    counter-increment: ziyi2 ziyi3;
    content: counter(ziyi3);
  }
</style>
<body>
  <p>ziyi2: </p>
  <p class="counter"></p>
  <p class="counter"></p>
</body>

```

> 这里ziyi2递增了2次(after那一次递增是后面递增的)，ziyi3递增了3次，所以显示46。


``` html
<style>
  .counter {
    counter-reset: ziyi2 2 ziyi3 3;
    counter-increment: ziyi2 ziyi3;
    font-size: 32px;
    font-family: Arial Black;
    color: pink;
  }

  .counter:before {
    counter-increment: ziyi2 ziyi3;
    content: counter(ziyi2);
  }
  .counter:after {
    counter-increment: ziyi2 ziyi3;
    content: counter(ziyi3);
  }

  .counter1:before {
    font-size: 32px;
    font-family: Arial Black;
    color: pink;
    content: counter(ziyi2);
  }
</style>
<body>
  <p>ziyi2: </p>
  <p class="counter"></p>
  <p class="counter1"></p>
</body>
```

> 此时counter显示的是46，而counter因为counter的after又递增了一次，所以显示5。


``` html
<style>
  .counter {
    counter-reset: ziyi2 2 ziyi3 3;
    counter-increment: ziyi2 ziyi3;
    font-size: 32px;
    font-family: Arial Black;
    color: pink;
  }

  .counter:before {
    counter-increment: counter 3;
    content: counter(ziyi2, lower-roman); /* counter(name, style) */
  }
  .counter:after {
    display: block;
    counter-increment: ziyi2 ziyi3;
    content: counter(ziyi3, '~'); /* counter(name, style) */
  }
</style>
<body>
  <p>ziyi2: </p>
  <p class="counter"></p>
</body>
```

> 显示罗马数字。



``` html
<style>
  .reset {
    padding-left: 20px;
    counter-reset: ziyi2;
  }

  .counter:before {
    content: counters(ziyi2, '-') '. '; /* 这是一种content内容生成混合特性，可以让字符.跟在计数值后 */
    counter-increment: ziyi2;
  }
</style>
<body>
  <div class="reset">
    <div class="counter">
       1
      <div class="reset">
        <div class="counter">
           1-1
        </div>
        <div class="counter">
           1-2
        </div>
      </div>
    </div>
    <div class="counter">
         2
        <div class="reset">
          <div class="counter">
             2-1
          </div>
          <div class="counter">
             2-2
          </div>
        </div>
      </div>
  </div>
</body>
```

> counters的用法是counters(name, string)。这样就实现了书目录的排布。


### 温和的padding属性

#### padding与元素尺寸

css默认的box-sizing是content-box，因此padding会增加元素的尺寸。

``` html
<style>
  .box {
    padding: 20px 60px;
    width: 80px;
    box-sizing: border-box;
    background-color: pink;
  }
</style>
<body>
  <div class="box"></div>
</body>
```

> 此时width会无效，因为padding的两个60px正好占据了width的大小。


``` html
<style>
  .box {
    padding: 20px 60px;
    width: 80px;
    box-sizing: border-box;
    background-color: pink;
  }

  a.link {
    padding: 20px;
    background-color: aqua;
    text-decoration: none;
  }
</style>
<body>
  <div class="box"></div>
  <div><a class="link" href="">链接</a></div>
  <div class="box"></div>
</body>
```

>内联元素的padding不仅会影响水平方向，也会影响垂直方向。内联元素没有可视宽度和可视高度的说法(clientHeight和clientWidth永远是0)，垂直方向的行为也完全受line-height和vertical-align的影响，这个例子在视觉上并没有改变上一行和下一行内容的间距(上下元素原本的布局没有任何影响)，只是a标签的尺寸明显受到padding影响，导致在垂直方向上发生了层叠。a的padding覆盖了box的区域。


CSS中有很多其他场景或属性会出现这种不影响其他元素布局而出现层叠效果的现象。例如relative元素的定位、盒阴影box-shadow以及outline等。这些层叠现象实际上可分为两类： 
- 纯视觉层叠，不影响外部尺寸(盒阴影box-shadow以及outline)
- 会影响外部尺寸(inline元素的padding，例如以上例子影响了a元素的尺寸)

给元素的父元素增加overflow:auto，层叠区域超出父容器会出现滚动条，这会影响尺寸、影响布局，没有出现滚动条这是纯视觉层叠。


``` html

<style>
  .box {
    padding: 20px 60px;
    width: 80px;
    box-sizing: border-box;
    background-color: pink;
    width: 200px;
  }

  div {
    overflow: auto;
    width: 200px;
  }

  a.link {
    padding: 20px;
    background-color: aqua;
    text-decoration: none;
  }
</style>
<body>
  <div class="box"></div>
  <div><a class="link" href="">链接</a></div>
  <div class="box"></div>
</body>
```

>此时第2个div出现了滚动条。所以说垂直方向padding对内联元素没有作用的说法是错误的。


padding元素也有一些其他作用，例如实现高度可控的分割线。


``` html
<style>
  a {
    text-decoration: none;
  }
  a + a:before {
    content: '';
    font-size: 0;
    padding: 10px 3px 1px; /* 注意border在左padding的左边 */
    margin-left: 6px; /* 为了实现border居中，margin值是两倍的水平padding值 */
    border-left: 1px solid grey;
  }
</style>
<body>
  <a href="">登录</a><a href="">注册</a>
</body>
```

> 此时控制上下padding就可以控制分割线的大小。控制左右padding就可以控制两个a标签之间的分割线距离(注意同时要调成margin)。

#### padding的百分比值

padding的百分比值无论是水平还是垂直方向都是相对于宽度（父元素）计算的。因为垂直高度height大多数情况下计算值可能是0，因为相对于高度计算很多情况下会失效。

``` html
<style>
  .box {
    padding: 10% 50%;
    position: relative;
  }

  .box > img {
    position: absolute;
    width: 100%;
    height: 100%;
    left: 0;
    top: 0;
  }
</style>
<body>
  <div class="box">
    <img src="https://www.baidu.com/img/bd_logo1.png" alt="">
  </div>
</body>
```

> box的width为0，因此box的宽高比例始终为5:1，因此图片的比例始终是5:1大小，而且图片可以天然等比例缩小放大。

如果padding百分比应用于内联元素

- 同样相对于宽度计算
- 默认的高度和宽度细节有差异
- padding会断行


``` html
<style>
  div {
    width: 200px;
  }
  span {
    padding: 50%;
    background-color: gray;
  }
</style>
<body>
  <div>
    <span>内有若干文字</span>
  </div>
</body>
```

> 由于字体的换行导致padding跟着换行。


``` html
<style>
  div {
    width: 200px;
  }
  span {
    padding: 50%;
    background-color: gray;
  }
</style>
<body>
  <div>
    <span></span>
  </div>
</body>
```

>此时高度和宽度不一样，因为幽灵空白节点的出现。导致高度和宽度大，因为幽灵空白节点占据行高。


``` html
<style>
  div {
    width: 200px;
  }
  span {
    padding: 50%;
    font-size: 0;
    background-color: gray;
  }
</style>
<body>
  <div>
    <span></span>
  </div>
</body>
```

> 此时幽灵空白节点的高度变成了0，span的宽高一样。

#### 标签元素内置的padding

- input/textarea输入框内置padding
- button内置padding
- 部分浏览器下拉内置padding
- radio/checkbox单复选框无内置padding
- button按钮的padding最难控制

``` html
<style>
  button {
    padding: 0;
  }
  /* Firefox需要额外的控制才能清除button的内置padding */
  button::-moz-focus-inner {
    padding: 0;
  }
</style>
<body>
  <button>按钮</button>
</body>
```

以上设置对于IE7浏览器仍然无效，IE7下会随着文字变多padding逐渐变大，因此需要设置button { overflow: visible; }才能去除padding。

#### padding与图形绘制

``` html
<style>
  .icon-menu {
    display: inline-block;
    width: 140px;
    height: 10px;
    padding: 35px 0;
    border-top: 10px solid #666;
    border-bottom: 10px solid #666;
    background-color: #666;
    background-clip: content-box;  /* 背景绘制在内容方框内（剪切成内容方框） */
  }

  .icon-dot {
    display: inline-block;
    width: 100px;
    height: 100px;
    border-radius: 50%;
    padding: 10px;
    border: 10px solid #666;
    background-color: #666;
    background-clip: content-box;
  }
</style>
<body>
  <i class="icon-menu"></i>
  <i class="icon-dot"></i>
</body>
```


## 激进的margin属性

1. 元素尺寸的相关概念

- 元素尺寸： 包括padding和border，元素border-box的尺寸。DOM API中写作offsetWidth和offsetHeight，也称为“元素偏移尺寸”。
- 元素内部尺寸：包括padding但是不包括border，元素padding-box的尺寸。DOM API写作clientWidth和client Height，也称为“元素可视尺寸”。
- 元素外部尺寸：包括padding、border和margin，元素margin-box的尺寸。没有对应的DOM API。


2. margin与元素内部尺寸

margin可以改变元素的内部尺寸，但和padding施互补态势。


``` html
<style>
  div {
    width: 300px;
    height: 300px;
    background-color: pink;
    margin: 0 -20px;
  }
</style>
<body>
   <div></div>
</body>
```
> 此时div内部尺寸不变。只要宽度设定，margin就无法改变元素的尺寸。此时元素div保持了“包裹性”，因此margin设值对于元素尺寸没有影响。

``` html

 <style>
    div.father {
      width: 300px;
      height: 300px;
      border: 1px solid pink;
    }

    div.son {
      height: 100%;
      margin: 0 -20px;
      border: 1px solid pink;
    }
  </style>
<body>
   <div class="father">
     <div class="son"></div>
   </div>
</body>
```

> 此时son的大小就变成了340p宽度了。此时虽然father具有“包裹性”，但是son是“流体特性”，是“充分利用可利用空间”的状态，因此此时margin可以改变元素的内部尺寸。


但是以上例子并不会改变元素的高度，只会在水平方向改变元素的尺寸。因为CSS的世界默认的流方向是水平方向，因此对于普通流元素，margin能改变元素水平方向的尺寸，而垂直方向不行(其实是因为垂直方向没有流体特性，不符合“充分可利用空间，注意height:100%和具有流体特性是不同的”)。但是对于具有拉伸特性的绝对定位元素，则水平或垂直方向都可以。


``` html
<style>
  div.father {
    width: 300px;
    height: 300px;
    margin: 0 auto;
    position: relative
  }

  div.son {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    margin: -20px -20px;
    background-color: pink;
  }
</style>
<body>
   <div class="father">
     <div class="son"></div>
   </div>
</body>
```
> 此时son的大小为340px X 340px。

利用margin可以实现布局，例如实现左列定宽，右列自适应布局。


``` html
<style>
  .container:after {
    display: table;
    content: '';
    clear: both;
  }
  .container .left {
    float: left;
    width: 200px;
    height: 200px;
    background-color: pink;
  }
  .container .right { /* 流体特性和margin改变内部尺寸 */
    margin-left: 220px; 
    height: 200px;
    background-color: pink;
  }
</style>
<body>
  <div class="container">
    <div class="left"></div>
    <div class="right"></div>
  </div>
</body>
```

3. margin与元素的外部尺寸

使用margin外部尺寸实现等高布局

``` html
<style>
  .container {
    overflow: hidden
  }

  .left, .right {
    width: 50%;
    float: left;
    margin-bottom: -9999px;
    padding-bottom: 9999px;
    background-color: pink;
  }

  .right {
    background-color: aqua;
  }
</style>
<body>
  <div class="container">
    <div class="left">
      <p>1</p>
      <p>1</p>
      <p>1</p>
      <p>1</p>
    </div>
    <div class="right">
      <p>1</p>
      <p>1</p>
      <p>1</p>
      <p>1</p>
      <p>1</p>
      <p>1</p>
    </div>
  </div>
</body>
```


>无论是左侧内容多还是右侧内容多，两栏的背景高度是一样的。使用margin-bottom: -9999px会使后面的所有元素往上移动9999px，会破坏布局，但是通过padding-bottom:9999px可以修复这个问题，增加了元素内部尺寸的同时又使后面的元素往下移动了9999px，从而对布局层不会产生影响，同时我们知道background默认是作用在content-box上的，因此padding区域能产生背景色，但是9999px的背景色太大了，因此给父元素一个overflow:hidden把多出来的色块背景隐藏掉，于是在视觉上呈现了等高布局效果。


- margin等高布局限制：如果子元素定位到容器之外，父级的overflow:hidden是一个限制。同时如果触发锚点定位或者使用DOM.scrollIntoview()方法的时候可能会出现奇怪的定位问题。
- border边框模拟：兼容性足够好，没有锚点定位的隐患，最多只能3栏，且border不支持百分比宽度，只能实现至少一侧定宽布局。
- table-cell布局：优点是天然登高，不足是IE8以上浏览器才支持，如果不需要支持IE67则推荐使用这个等高布局方式。
- 伪table-cell布局：不需要考虑table的语义，没有语义且可以像table那样布局，html的层次结构相比直接用table元素也要简单一些，我们这里只用到了3层，直接用table元素的话可能还有tbody这一层，缺点是分栏之间的间隔不能用margin和padding来做，如果用margin，这个属性在display: table-cell的元素上根本不会生效；如果用padding，那像demo里面各栏的背景色就都会连到一块，做不出间隔的效果，如果在layout__col里面再嵌套一层，在这一层设置背景色的话，又会增加html的层次，也不是很好。


4. 实现等高布局的几种方法

- 伪table-cell布局

``` html
<style>
  .table {
    display: table;
    width: 100%;
  }
  .table-row {
    display: table-row;
  }
  .table-col {
    text-align: center;
    display: table-cell;
  }

  .left,.right {
    background-color: #daf1ef;
  }

  .center {
    background-color: #4DBCB0;
  }

</style>
<body>
  <div class="table">
    <div class="table-row">
      <div class="table-col left">
        <p>1</p>
        <p>1</p>
        <p>1</p>
        <p>1</p>
        <p>1</p>
      </div>
      <div class="table-col center">
        <p>1</p>
        <p>1</p>
        <p>1</p>
        <p>1</p>
      </div>
      <div class="table-col right">
        <p>1</p>
        <p>1</p>
        <p>1</p>
      </div>
    </div>
  </div>
</body>
```

> 使用table-cell布局的好处是天然等高布局，以上实现了宽度自适用的三列等宽等高布局。


``` html
<style>
  .table {
    display: table;
    width: 100%;
  }
  .table-row {
    display: table-row;
  }
  .table-col {
    text-align: center;
    display: table-cell;
  }

  .left,.right {
    background-color: #daf1ef;
    width: 200px; /* 两边固定宽度，中间自适应宽度 */
  }

  .center {
    background-color: #4DBCB0;
  }
</style>
<body>
  <div class="table">
    <div class="table-row">
      <div class="table-col left">
        <p>1</p>
        <p>1</p>
        <p>1</p>
        <p>1</p>
        <p>1</p>
      </div>
      <div class="table-col center">
        <p>1</p>
        <p>1</p>
        <p>1</p>
        <p>1</p>
      </div>
      <div class="table-col right">
        <p>1</p>
        <p>1</p>
        <p>1</p>
      </div>
    </div>
  </div>
</body>
```

> 此时实现了两端定宽，中间自适应的布局。

``` html
<style>
  .table {
    display: table;
    width: 100%;
  }
  .table-row {
    display: table-row;
  }
  .table-col {
    text-align: center;
    display: table-cell;
  }

  .left,.right {
    background-color: #daf1ef;
    width: 200px; /* 两边固定宽度，中间自适应宽度 */
  }

  .center {
    background-color: #4DBCB0;
  }
</style>
<body>
  <div class="table">
    <div class="table-col left">
      <p>1</p>
      <p>1</p>
      <p>1</p>
      <p>1</p>
      <p>1</p>
    </div>
    <div class="table-col center">
      <p>1</p>
      <p>1</p>
      <p>1</p>
      <p>1</p>
    </div>
    <div class="table-col right">
      <p>1</p>
      <p>1</p>
      <p>1</p>
    </div>
  </div>
</body>
```

> 如果去掉table-row，根据table生成匿名块的特性不会影响布局。而如果把table也去掉，则table-cell不指定宽度就具有包裹性，而不会自适应宽度。

#### margin合并

1. 什么margin合并

块级元素的上外边距和下外边距有时会合并为单个外边距，这样的现象称为margin合并。

- 块级元素：不包括浮动或绝对定位元素。
- 只发生在垂直方向，当然如果改变writing-mode值(改变文档流方向)，则也可以改变合并方向，准确的说应该是只发生在和当前文档流方向的相垂直的方向上。

2. margin合并的3种场景
1) 相邻兄弟元素的margin合并

``` html
<style>
  p {
    margin: 10px;
  }
</style>
<body>
  <p>实际上下margin是10px而不是20px</p>
  <p>实际上下margin是10px而不是20px</p>
</body>
```
2) 父元素和第一个/最后一个子元素

``` html
<style>
  p {
    margin: 80px 0;
  }
  div {
    margin: 80px 0;
    background-color: pink;
  }
</style>
<body>
  <div>
    <p>margin:80px</p>
  </div>
</body>
```

``` html
<style>
  p {
    margin: 80px 0;
  }

  div {
    background-color: pink;
  }
</style>
<body>
  <div>
    <p>margin:80px</p>
  </div>
</body>
```

``` html
<style>
  div {
    margin: 80px 0;
    background-color: pink;
  }
</style>
<body>
  <div>
    <p>margin:80px</p>
  </div>
</body>
```

> 以上三种的margin其实都是一样的，查看div的背景色可以发现，p元素的margin值作用到div之外，就好像div本身产生的margin一样，这就是因为产生了margin合并。



如何阻止父子元素margin合并:

对于margin-top满足以下任意条件即可
- 父元素设置为块状格式化上下文元素（例如设置overflow:hidden）
- 父元素设置border-top值
- 父元素设置padding-top值
- 父元素和第一个子元素之间添加内联元素进行分隔
对于margin-bottom满足以下任意条件即可
- 父元素设置为块状格式化上下文元素
- 父元素设置border-bottom值
- 父元素设置padding-bottom值
- 父元素和最后一个子元素之间添加内联元素进行分隔
- 父元素设置height、min-height或max-height值

3) 空块级元素的margin合并

``` html
<style>
  p {
    margin: 80px 0;
  }

  div {
    overflow: hidden;
    background-color: pink;
  }
</style>
<body>
  <div>
    <p></p>
  </div>
</body>
```

> 此时div的高度是80px而不是160px。因为此时p元素的margin-top和margin-bottom产生了合并，所以只剩下80px了。


``` html
<style>
  p {
    margin: 80px 0;
  }

  div {
    background-color: pink;
  }
</style>
<body>
  <div>
    <p>第一行</p>
    <div></div>
    <p>第二行</p>
  </div>
</body>
```

> 两个p之间的距离仍然是80px，因为div容器内部的三个元素产生了三次margin合并，第一次是p元素和相邻的div空元素margin-bottom合并，然后是第二个p元素和div空元素的margin-top合并，由于div是空元素，因此margin-bottom和margin-top又产生了一次合并。当然p元素和父元素div也发生了margin合并。

阻止空元素的margin合并：
- 设置垂直方向的border
- 设置垂直方向的padding
- 里面添加内联元素（空格无效）
- 设置height或min-height

3. margin合并规则
1） 正正取大值。

``` html
<style>
  p:first-child {
    margin: 80px 0;
  }

  p:last-child {
    margin: 120px 0;
  }

  div {
    background-color: pink;
  }
</style>
<body>
  <div>
    <p>第一行</p>
    <p>第二行</p>
  </div>
</body>
```

> 此时两个p元素之间的距离是120px。

2）正负值相加。
3) 负负最负值。

#### 深入理解css中的margin:auto

margin:auto是为了填充闲置的width而设置的。

- 如果一侧定宽、一侧auto，则auto为剩余空间大小
- 如果两侧均是auto，则平分剩余空间。

``` html

<style>
  .father {
    width: 300px;
    height: 300px;
    border: 1px solid pink;
  }
  .son {
    width: 200px;
    height: 300px;
    margin-right: 80px;
    margin-left: auto; /* */
    background-color: pink;
  }
</style>
<body>
  <div class="father">
    <div class="son"></div>
  </div>
</body>
```

> 此时margin-left自动计算为20px大小。

``` html
 <style>
  .father {
    width: 300px;
    height: 300px;
    border: 1px solid pink;
  }
  .son {
    width: 200px;
    height: 300px;
    margin-left: auto;
    background-color: pink;
  }
</style>
<body>
  <div class="father">
    <div class="son"></div>
  </div>
</body>
```

> CSS中margin的初始值大小是0，因此以上实例正好实现了右对齐的效果，根据一侧定宽，一侧auto这auto为剩余值的计算方法，margin-left的实际大小为300-200-0,因此正好实现了右对齐的效果。有的时候为了实现元素右对齐，则可以采用margin-left:auto的方法，而非float:right。

- 实现居中效果： margin-left: auto; margin-right: auto;
- 实现右对齐效果： margin-left: auto;
- 实现左对齐效果：margin-right: auto;

正好和text-align控制内联元素的左中右对齐类似。

需要注意的margin:auto并不能实现元素的垂直居中对齐，因为在垂直方向height:auto时元素不具有自动填充特性，因此无法垂直居中。

当然通过改变writing-mode改变文档流的方向可以实现垂直居中

``` html
<style>
  .father {
    width: 300px;
    height: 300px;
    border: 1px solid pink;
    writing-mode: vertical-lr; /* 垂直方向自左而右的书写方式。*/
  }
  .son {
    width: 200px;
    height: 200px;
    margin: auto;
    background-color: pink;
  }
</style>
<body>
  <div class="father">
    <div class="son">111</div>
  </div>
</body>
```

> 此时垂直居中了,但是水平就不能居中了。

如何实现水平和垂直居中呢，可以使用绝对定位元素的margin:auto居中。

``` html
<style>
  .father {
    width: 300px;
    height: 300px;
    border: 1px solid pink;
    position: absolute;
  }
  .son {
    background-color: pink;
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
  }
</style>
<body>
  <div class="father">
    <div class="son"></div>
  </div>
</body>
```

> 此时son元素的尺寸表现为“格式化宽度和格式化高度”，和div的“正常流宽度”一样，同属于外部尺寸，也就是尺寸自动填充父级元素的可用尺寸。

``` html
 <style>
  .father {
    width: 300px;
    height: 300px;
    border: 1px solid pink;
    position: absolute;
  }
  .son {
    width: 200px;
    height: 200px;
    margin: auto;
    background-color: pink;
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
  }
</style>
<body>
  <div class="father">
    <div class="son"></div>
  </div>
</body>
```

> 此时给son一个高度和宽度，原本应该被填充的空间被空余出来了，这多余的空间就是margin:auto计算的空间，此时设置margin:auto就能实现垂直和水平居中了。


由于绝对定位元素的格式化高度即使父元素height:auto也支持，因此应用场景非常广泛，唯一不足的就是此居中计算IE8及以上版本浏览器才支持。如果无需考虑支持IE7，此种方法是最直接有效的方法，必top:50%然后margin负一半元素高度的方法好用很多。

需要注意如果son比father大，那margin:auto会被当做0处理，其实就等于没有设置margin值。对于替换元素，可以通过设置display:block;margin:auto;同样生效。

``` html
<style>
  .father {
    width: 300px;
    height: 300px;
    border: 1px solid pink;
    position: absolute;
  }
  img {
    display: block;
    width: 200px;
    height: 200px;
    margin: auto;
    background-color: pink;
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
  }
</style>
<body>
  <div class="father">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

以上使用margin:auto的方式唯一的不足是必须要设置son的宽高。除此之外可以使用其他方式设置垂直居中

``` html
<style>
  .father {
    width: 400px;
    height: 400px;
    border: 1px solid #ccc;
    display: flex;
    justify-content: center; /* 在主轴（横轴）方向上的中心对齐 */
    align-items: center; /* 侧轴（纵轴）方向上中心对齐 */
  }
  .son {
    width: 80%;
    height: 80%;
    background: #ccc;
  }
</style>
<body>
  <div class="father">
    <div class="son"></div>
  </div>
</body>
```

> 使用flex布局需要注意此方法只兼容IE10。



``` html
<style>
  .father {
    width: 400px;
    height: 400px;
    border: 1px solid #ccc;
    display: table-cell;
    text-align: center;
    vertical-align: middle;
  }
  .son {
    width: 80%;
    height: 80%;
    display: inline-block;
    background: #ccc;
  }
</style>
<body>
  <div class="father">
    <div class="son"></div>
  </div>
</body>
```

> 使用table-cell垂直居中布局可以兼容IE8以上。需要注意table-cell不是真正的垂直居中。

``` html
<style>
  .father {
    width: 400px;
    height: 400px;
    border: 1px solid #ccc;
    position: relative;
  }
  .son {
    width: 80%;
    height: 80%;
    background: #ccc;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
  }
</style>
<body>
  <div class="father">
    <div class="son"></div>
  </div>
</body>
```
> 此方法兼容ie9以上。




#### margin无效情形解析

- display计算值为inline的非替换元素的垂直margin无效，对于内联替换元素垂直margin有效，并且没有margin合并的问题。


``` html
<style>
  .father {
    width: 300px;
    height: 300px;
    background-color: pink;
  }
  span {
    margin-bottom: 20px;
  }
  img {
    width: 200px;
    height: 200px;
    background-color: pink;
    margin-top: 30px;
  }
</style>
<body>
  <div class="father">
    <span>span inline</span>
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <div>div block</div>
  </div>
</body>
```

> span作为内联非替换元素设置margin无效。img作为内联替换元素设置垂直margin有效，而且不会和father进行margin合并。

- 表格中tr和td元素或设置display:table-cell或display:table-row的元素的margin无效。


``` html
<style>
  .father {
    width: 300px;
    height: 300px;
    background-color: pink;
  }
  .son {
    width: 100px;
    height: 100px;
    display: table-cell;
    margin: 20px;
    background-color: aqua;
  }
</style>
<body>
  <div class="father">
    <div class="son"></div>
    <div class="son"></div>
  </div>
</body>
```

> 此时son元素设置的margin无效。


- margin合并时更改margin的值可能无效。
- 绝对定位元素非定位方位的margin值“无效”。

``` html

<style>
  .father {
    width: 300px;
    height: 300px;
    background-color: pink;
    position: relative;
  }
  .son {
    position: absolute;
    top: 100px;
    left: 100px;
    width: 100px;
    height: 100px;
    margin-right: 30px;
    margin-bottom: 200px;
    background-color: aqua;
  }
</style>
<body>
  <div class="father">
    <div class="son"></div>
    <div class="son"></div>
  </div>
</body>

```

> 此时margin-right和margin-bottom设置无效。主要是因为绝对定位元素的渲染是独立的，普通元素和兄弟元素心连心，你动我也动，但是绝对定位元素由于独立渲染无法和兄弟元素插科打诨，因此margin无法影响兄弟元素定位，所以看上去无效。

- 定高容器的子元素的margin-bottom或者定宽定死的子元素的margin-right的定位失效。一个普通元素在默认流下，其定位方向是左侧以及上方，此时只有margin-left和margin-top可以影响元素的定位。


- 鞭长莫及导致margin失效。

``` html
<style>
  .father>img {
    float: left;
    width: 300px;
  }

  .father>div {
    overflow: hidden;
    margin-left: 200px;
  }
</style>
<body>
  <div class="father">
    <div>111</div>
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

- 内联特性导致margin失效。

``` html
<style>
  .father {
    border: 1px solid pink;
  }
  .father>img {
    height: 100px;
    margin-top: -10000px;
  }
</style>
<body>
  <div class="father">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

> 此时和设置margin-top:-5000px一样，内联元素在设置了一定高度的负值之后不再往上偏移。

### 功勋卓越的border属性

#### border-style属性

border-style默认值是none，因此单独进行以下设置是没有边框出现的


``` html
<style>
    div  {
      height: 200px;
      width: 200px;
    }
    .box1 {
      border: 1px;
    }
    .box2 {
      border: pink;
    }
  </style>
<body>
  <div class="box1"></div>
  <div class="box2"></div>
</body>
```

``` html
<style>
  div  {
    height: 200px;
    width: 200px;
  }
  .box1 {
    border: 1px;
  }
  .box2 {
    border: pink;
  }
  .box3 {
    border: solid; /* 默认出现了3px的边框 */
  }
</style>
<body>
  <div class="box1"></div>
  <div class="box2"></div>
  <div class="box3"></div>
</body>
```

> 默认3px是和border-style:double有关，border-style:3px下才能实现双线条分离的视觉效果。


实现一个性能最佳的无底部边框

``` html
<style>
  div  {
    height: 200px;
    width: 200px;
  }
  .box {
    border: solid;
    border-bottom: 0 none;
  }
</style>
<body>
  <div class="box"></div>
</body>
```

border-style:double的表现规则：双线宽度永远相等，中间间隔+1。因此可以利用这个特性实现“三道杠”大队长效果。

``` html
<style>
  .box {
    width: 100px;
    height: 20px;
    border-top: 60px double;
    border-bottom: 20px solid;
  }
</style>
<body>
  <div class="box"></div>
</body>
```

#### border与透明边框技巧

1. 增加点击区域大小

``` html
<style>
  .box {
    width: 10px;
    height: 10px;
    border: 50px solid transparent;
  }
</style>
<body>
  <div class="box">111</div>
</body>
```

2. 三角形图形绘制

``` html
<style>
  .box {
    width: 0;
    border: 10px solid;
    border-color: pink transparent transparent;
  }
</style>
<body>
  <div class="box"></div>
</body>
```

#### border与图形构建

``` html
<style>
  .box {
    width: 0;
    border-width: 20px 10px;
    border-style: solid;
    border-color: pink transparent transparent;
  }
</style>
<body>
  <div class="box"></div>
</body>
```

> 窄三角形


``` html
<style>
  .box {
    width: 0;
    border-width: 20px 10px;
    border-style: solid;
    border-color: pink pink transparent transparent;
  }
</style>
<body>
  <div class="box"></div>
</body>
```

> 对话框尖角

#### border等高布局技术


``` html
<style>
  .box {
    border-left: 150px solid #333;
    background-color: pink;
    /* overflow: hidden; 不能使用, 溢出隐藏是基于padding-box的，这里的border-box会被隐藏掉 */
  }

  .box:after {
    content: '';
    display: table;
    clear: both;
  }

  .box > nav {
    width: 150px;
    margin-left: -150px;
    float: left;
  }

  .box > nav > h3 {
    margin: 0;
  }

  .box > section {
    overflow: hidden;
  }

  .nav {
    line-height: 40px;
    color: #fff;
  }

  .module {
    line-height: 40px;
  }
</style>
<body>
  <div class="box">
    <nav>
      <h3 class="nav">导航</h3>
      <h3 class="nav">导航2</h3>
      <h3 class="nav">导航3</h3>
    </nav>
    <section>
      <div class="module">模块1</div>
    </section>
  </div>
</body>
```

> 最多满足2~3列，理论上采用border-style:double最多可以实现7栏布局。不会出现锚点定位带来的问题。

## 内联元素与流

### 字母x

#### 字母x与基线

在内联模型中，涉及垂直方向的排版或者对齐都离不开最基本的基线(baseline)。line-height行高的定义就是两基线的间距，vertical-align的默认值就是基线。

基线的定义：字母x的下边缘(线)就是基线。

#### 字母x与x-height

x-height就是小写字母x的高度(术语描述就是基线和等分线mean line[也称作中线，midline])之间的距离。

其中vertical-align:middle指的是基线往上1/2 x-height高度。可以近似理解为字母x交叉点那个位置。vertical-align:middle并不是绝对的垂直居中对齐。只是一种近似的效果，因为不同的字体在行内盒子中的位置是不一样的，比如"微软雅黑"就是一个字符下沉比较明显的字体，所在字符的位置比其他字体要偏下一点。所以内联元素的垂直居中是相对于文字而言，而非相对外部的块级容器而言。

#### 字母x与ex

ex是一个相对单位，指的是小写字母x的高度，就是指x-height。ex不适合用来限定元素的尺寸，而是可以作用于不受字体和字号影响的内联元素的垂直居中对齐效果。

#### 内联元素与行高line-height

##### 行高line-height

默认div里写上文字，div的高度由行高line-height决定而不是字体大小决定。


``` html
<style>
  .line-height-0 {
    line-height: 0;
    font-size: 16px;
    background: #eee;
    margin-bottom: 40px;
  }

  .font-size-0 {
    line-height: 16px;
    font-size: 0;
    background: #eee;
  }
</style>
<body>
  <div class="line-height-0">111</div>
  <div class="font-size-0">222</div>
</body>
```

>  line-height-0的div高度为0，字111存在，而font-size-0的div高度为16px，字222却未显示。显然div高度由行高决定，而非文字。

对于非替换元素的纯内联元素，其可视高度完全由line-height决定(padding、border属性对可视高度完全没有任何影响)。

内联元素的高度由固定高度和不固定高度组成，不固定高度部分就是“行距”。line-height之所以起作用，就是改变“行距”(上下半行距)来实现的。“行距”分散在当前文字的上方和下方(传统印刷的行距是上下两行文字之间预留的间隙)。即使是第一行文字其上方也是会预留间隙的，只是这个间隙是当前"行距"的一半，因此也被称为"半行距"。

行距 = 行高 - em-box (行距 = line-height - font-size)
半行距 = 行距 / 2

em-box指的是一个盒子，其高度正好是1em，1em等同于当前一个font-size大小。

内容区域和em-box是不一样的，内容区域通常而言要比em-box高一些，em-box只受font-size影响，而内容区域则不仅受font-size影响，还受到font-family影响。只有当字体是宋体时，内容区域和em-box是等同的。因此使用宋体可以准确看出“半行距”。

``` html
<style>
  .simsun {
    font-family: simsun;
    font-size: 24px;
    line-height: 36px;
    background-color: pink;
  }
</style>
<body>
  <div class="simsun">simsun</div>
</body>
```

> 此时内容区域(鼠标选中高亮的区域)就是em-box的区域。而半行距正好是选中区域外上下多余的额外背景区域。

``` html
<style>
  .line-height-2 {
    line-height: 2;
  }
  .line-height-1 {
    line-height: 1;
  }
  .line-height-half {
    line-height: 0.5;
  }

  div {
    margin-bottom: 10px;
  }
  p {
    margin: 0;
    border: 1px solid pink;
  }
</style>
<body>
  <div class="line-height-2">
    <p>文字</p>
    <p>文字</p>
  </div>
  <div class="line-height-1">
    <p>文字</p>
    <p>文字</p>
  </div>
  <div class="line-height-half">
    <p>文字</p>
    <p>文字</p>
  </div>
</body>
```

##### 替换元素和块级元素与行高line-height

line-height不能影响替换元素的高度。

``` html
<style>
  .box {
    line-height: 256px;
  }
</style>
<body>
  <div class="box">
    <img height="128" src="https://www.baidu.com/img/bd_logo1.png" alt="">
  </div>
</body>
```

> 此时图片的高度仍然是128px，并不会使256px。但是div的高度确实是256px。

需要注意的是div高度变高，是因为line-height把"幽灵空白节点"的高度变高了，图片为内联元素，会构成一个"行框盒子"，在HTML5文档模式下，每一个"行框盒子"的前面都有一个宽度为0的"幽灵空白节点"，其内联特性表现和普通字符一模一样，所以div的高度会等于line-height设置的属性值256px。

如果是内联替换元素和内联非替换元素排布在一起时，line-height的作用会是什么样？由于同属于内联元素，因此会共同形成一个“行框盒子”，line-height在这个混合元素的“行框盒子”中决定这个行盒的最小高度。对于纯文本元素，line-height直接决定了最终的高度，但是如果有替换元素，则line-height只能决定这个行盒的最小高度，因为替换元素的高度不受line-height影响，而是vertical-align属性也会起作用。

也有这种情况，明明文字设置了line-height为20px，但是文字后面如果有小图标，最后行框盒子的高度却是21px或者22px，这种情况是vertical-align属性在起作用。

对于块级元素line-height本身没有任何作用，通过改变line-height，块级元素的高度跟着变化实际上是通过改变块级元素里面的内联级别元素占据的高度实现。

#### line-height可以让内联元素“垂直居中”

要让单行文字垂直居中，只需要设置line-height即可，而不需要设置line-height = height。行高控制文字垂直居中，不仅适用于单行，多行也可以。需要注意的是垂直居中只是近似的居中，因为文字字形的垂直中线位置普遍要比“行框盒子”的垂直中线位置低，例如微软雅黑字体：


``` html
<style>
  p {
    font-size: 100px;
    line-height: 200px;
    background-color: black;
    font-family: 'micriosoft yahei';
    color: white;
  }
</style>
<body>
  <div class="box">
    <p>微软雅黑</p>
  </div>
</body>
```

> 此时字体明显要偏下一点，并不是真正的垂直居中。只是平时使用的font-size较小，因此很难观察到这种情况。


多行文本或者替换元素的垂直居中和单行文本不一样，不仅需要设置line-height属性，还需要设置vertical-align属性


``` html
<style>
  div {
    line-height: 200px;
    width: 200px;
    background-color: black;
    font-family: 'micriosoft yahei';
    color: white;
  }
  p {
    display: inline-block;
    line-height: 20px;
    margin: 0 20px;
    vertical-align: middle;
  }
</style>
<body>
  <div class="box">
    <p>微软雅黑微软雅黑微软雅黑微软雅黑微软雅黑微软雅黑</p>
  </div>
</body>
```

> 使用display:inline-block既能重置外部的line-height大小，又能保持内联元素特性，从而可以设置vertical-align属性，产生非常关键的"行框盒子"。需要注意的是"行框盒子"会附带一个“幽灵空白节点”(多次强调，内联元素会构成一个"行框盒子"，在HTML5文档模式下，每一个"行框盒子"的前面都有一个宽度为0的"幽灵空白节点"，其内联特性表现和普通字符一模一样，所以div的高度会等于line-height设置的属性值200px。如果把p元素去掉或者不设置p元素的display:inline-block，那么div的高度就一不定是line-height的高度了)，所以在p元素之前的撑起了一个高度为200px宽度为0的内联元素(幽灵空白节点)。又因为内联元素默认是基线对齐的，通过设置vertical-align:middle来调整多行文本的垂直位置，从而实现“垂直居中”效果。而设置了display:inline-block后，p元素内部创建了一个独立的“行框盒子”，这样p设置的line-height就可以生效了，这是多行文本垂直居中的原因。

如果是图片等替换元素

``` html
<style>
  div {
    line-height: 200px;
    width: 200px;
    background-color: black;
    font-family: 'micriosoft yahei';
    color: white;
  }
  img {
    display: inline-block;
    vertical-align: middle;
    height: 100px;
    margin: 0 50px;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

> 实现了垂直居中。需要注意如果要水平居中则还需要对margin做处理。

#### line-height属性值

line-height属性值设置为normal会根据字体类型而变化，因此通常而言需要对line-height值进行重置。line-height可以设置数字、百分比值以及长度值。


``` html

<style>
  div {
    font-size: 14px;
    margin-bottom: 50px;
  }
  .line-height-1 {
    line-height: 1.5;
  }
  .line-height-2 {
    line-height: 150%;
  }

  .line-height-3 {
    line-height: 1.5em;
  }
  h3,p {
    margin: 0;
  }
  h3 {
    font-size: 32px;
  }
  p {
    font-size: 20px;
  }
</style>
<body>
  <div class="line-height-1">
    <h3>line-height:48px</h3>
    <p>line-height:32px</p>
  </div>
  <div class="line-height-2">
    <h3>line-height:21px</h3>
    <p>line-height:21px</p>
  </div>
  <div class="line-height-3">
    <h3>line-height:21px</h3>
    <p>line-height:21px</p>
  </div>
</body>
```

> 明显可以发现后两者都是在body的font-size的基础上进行计算，因为body的font-size:14px; 因此各自的line-height都是21px，也就是百分比和em继承的是计算值。而line-height:1.5的继承不同，p元素和h3元素继承的是属性值1.5而不是计算值，因此各自的line-height都根据各自的font-size进行计算。

通常而言想要类似于。line-height:1.5的效果，可以使用

``` html
* {
  line-height: 200%;
}
```

> 此时所有的元素都会根据自身的font-size计算line-height。需要注意和body设置line-height不同的是一些表单类的替换元素，具有继承特性的CSS属性例如line-height都有自己的一套，因此body设置的line-height不能被这些替换元素继承，毕竟继承属于最弱的权重，而通配符*的权重大于继承，因此会重置替换元素默认的line-height。

如果考虑到*通配符的性能，可以折中使用 

``` html
<style>
  body {
    line-height: 1.5;
  }

  input, button {
    line-height: inherit;
  }
</style>
```

> 需要注意如果是一个重图文内容展示的网页，如博客、论坛、公众号之类的一般使用数值作为单位，如果是偏重布局结构精致的网站，使用长度值或者数值都可以。通常而言大型网站都使用数值作为全局的line-height值。

#### 内联元素line-height的“大值特性”


``` html
<style>
  .line-height-1 {
    line-height: 96px;
  }

  .line-height-1 span {
    line-height: 20px;
  }

  .line-height-2 {
    line-height: 20px;
  }

  .line-height-2 span {
    line-height: 96px;
  }
</style>
<body>
  <div class="line-height-1">
    <span>111</span>
  </div>
  <div class="line-height-2">
    <span>222</span>
  </div>
</body>
```

> 此时div的高度都是96px。无论内联元素line-height如何设置，最终父级元素的高度都是由数值大的那个line-height决定。

例如

``` html
<style>
  .box {
    line-height: 20px;
  }
  span:first-child {
    line-height: 100px;
  }
  span:last-child {
    line-height: 200px;
  }
</style>
<body>
  <div class="box">
    <span>222</span>
    <span>333</span>
  </div>
</body>
```


此时div的高度是200px。span元素是内联元素，自身有一个“内联盒子”，只要有“内联盒子”，就一定有“行框盒子”，每一行内联元素外面包裹的一层盒子，在每个“行框盒子”前有一个宽度为0的具有该元素的字体和行高属性的看不见的“幽灵空白节点”。当设置box的line-height是200px时，此时拥有行框盒子的box的幽灵空白节点撑开了box的高度，可以使box是200px高，而如果是span设置了line-height为200px，那么由最高的那个“内联盒子”决定了box的高度。


### vertical-align

凡是line-height起作用的地方，vertical-align也一定起作用，只是有的时候视觉上感觉不到。


``` html
<style>
  .box {
    line-height: 32px;
  }

  .box > span {
    font-size: 24px;
  }
</style>
<body>
  <div class="box">
    <span>222</span>
  </div>
</body>
```
> 此时div的高度并不是32px，而是35px。此时是因为vertical-align在起作用。


#### vertical-align属性值

- 线类：baseline、top、middle、bottom
- 文本类：如text-top、text-bottom
- 上标下标类：如sub、super
- 数值百分比类：如20px、2em、20%等(相对于基线往上或往下偏移)

``` html
<style>
  img {
    width: 100px;
  }

  .baseline span {
    vertical-align: baseline;
  }

  .number span {
    vertical-align: 20px;
  }

  .em span {
    vertical-align: -1em;
  }

  .percent span {
    vertical-align: 100%;
  }

</style>
<body>
  <div class="baseline">
    <span>字母x</span>
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
  <div class="number">
    <span>字母x</span>
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
  <div class="em">
    <span>字母x</span>
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
  <div class="percent">
    <span>字母x</span>
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```
> 负值相对于基线往下偏移，正值往上偏移，事实上vertical-align:base-line等同于vertical-align:0。

vertical-align的属性值middle、text-top等在IE6、IE7下可能渲染规则和其他浏览器不同，但是baseline和相对于baseline的数值百分比类兼容一直很好。需要注意的是vertical-align的百分比值是根据line-height计算的。但是现在的网页基本上line-height都是固定的，因此使用数值比百分比值更方便，而且需要重新计算。

#### vertical-align作用的前提

vertical-align属性只能应用于内联元素以及display值为table-cell的元素，因此vertical-align只能应用于display值为inline、inline-block、inline-table或table-cell的元素[span、strong、em、img、button、input等元素天然支持vertical-align属性，块级元素则不支持]。


需要注意浮动和绝对定位会让元素块状化，因此以下组合是不合理的

``` html
<style>
.box {
  float: left;
  vertical-align: middle;
}
.box {
  position: absolute;
  vertical-align: middle;
}
</style>
```


以下示例中，图片并没有垂直居中，凡是line-height起作用的地方，vertical-align也一定起作用，只是有的时候视觉上感觉不到，以下就是因为box内的幽灵空白节点高度太小，实际上vertical-align在努力渲染。


``` html
<style>
  .box {
    height: 128px;
    background-color: pink;
  }
  .box > img {
    height: 96px;
    vertical-align: middle;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

此时通过设置box的line-height就可以让图片垂直居中了

``` html
<style>
  .box {
    height: 128px;
    line-height: 128px;
    background-color: pink;
  }
  .box > img {
    height: 96px;
    vertical-align: middle;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

需要注意的是display:table-cell可以无视行高是因为对于该表现的元素而言，vertical-align起作用的是table-cell元素自身。


``` html
<style>
  .box {
    height: 128px;
    width: 128px;
    display: table-cell;
    background-color: pink;
  }
  .box > img {
    height: 96px;
    vertical-align: middle;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

> 此时img元素并没有垂直居中。除非向前面一样设置line-height属性。

``` html
<style>
  .box {
    height: 128px;
    width: 128px;
    vertical-align: middle;
    display: table-cell;
    background-color: pink;
  }
  .box > img {
    height: 96px;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```
> 此时天然垂直居中。因此table-cell元素设置了middle则垂直对齐的就是子元素，其作用的是元素自身而不是子元素，如果子元素是块级元素也一样可以有垂直对齐的表现。

#### vertical-align和line-height的关系

vertical-align的百分比值是相对于line-height计算的，只要出现内联元素，一定会同时出现vertical-align和line-height。之前那个设置了line-height但是高度不是line-height的例子：

``` html
<style>
  .box {
    line-height: 32px;
  }
  .box > span {
    font-size: 24px;
  }
</style>
<body>
  <div class="box">
    <span>此时box的高度是35px, span的高度是31px。</span>
  </div>
</body>
```
> font-size大小设置在span元素上而不是box元素上，导致了box元素的字体大小和span元素有较大出入。而span这个内联元素前面必定有一个“幽灵空白节点”。


在span元素前面添加一个字母x，从而可以从视觉上观察出"幽灵空白节点"的作用。

``` html
<style>
  .box {
    line-height: 32px;
  }
  .box > span {
    font-size: 24px;
  }
</style>
<body>
  <div class="box">x<span>文本x</span></div>
</body>
```
此时匿名内联元素x和span内联元素"文本x"的字体大小不一样，由于受到了line-height:32px的影响，这两个盒子的高度都是32px，对字符而言，font-size越大，基线位置相对越往下，因此两个内联盒子的基线相对而言后者低于前者，尽管各自的基线位置相对而言不一样，但是在一个行框盒子内，文字默认都是基线对齐(vertical-align:baseline;)，因此span内联元素低于匿名内联元素的基线需要往上抬，和匿名内联元素保持一致，导致了span内联元素整体往上移，那么此时span内联元素的高度尽管仍然是32px，但是和匿名内联元素一结合，高度肯定大于了32px(两个内联盒子高度都是32px，但是位置一个高一个低，不保持平行，那么总的行框盒子的高度肯定大于32px了)。因此box元素的高度并不是想象中的32px，而是比32px更大。


为了解决box元素变大，可以有两种解决方案，一种是设置匿名内联元素的字体大小和span内联元素一致，这样的话两个内联盒子的基线默认对齐，内联盒子不会有上下位移需要保持基线对齐的操作

``` html
<style>
  .box {
    line-height: 32px;
    font-size: 24px;
  }
  .box > span {
    font-size: 24px;
  }
</style>
<body>
  <div class="box">x<span>文本x</span></div>
</body>
```
> 此时box的高度就是line-height的高度32px了。

或者改变垂直对齐方式，如顶部对齐，而不是默认的基线对齐

``` html
<style>
  .box {
    line-height: 32px;
  }
  .box > span {
    font-size: 24px;
    vertical-align: top;
  }
</style>
<body>
  <div class="box">x<span>文本x</span></div>
</body>
```

> 此时box的高度也仍然是32px。

大小字号文字的高度偏差问题，在图片中也容易出现，任意一个块级元素若有图片，则块级元素高度基本上都要比图片的高度高。

``` html
<style>
  .box {
    width: 280px;
    outline: 1px solid pink;
    text-align: center;
  }
  .box > img {
    height: 96px;
  }
</style>
<body>
  <div class="box">
    x<img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

> box高度101px，图片高度为96px。


间隙产生的三大元凶是“幽灵空白节点”、line-height和vertical-align属性。line-height是20px，而font-size是14px，因此字母x(用于模仿幽灵空白节点)往下至少有3px的半行间距，图片作为替换元素其基线是自身的下边缘(也就是x字母的下边缘)，因此图片的下边缘和字母x的下边缘对齐，而字母x因为line-height的缘故，字母x下边缘还有3px的半行间距，导致图片底部在视觉上底部留有空白间隙。

解决方法

- 图片块状化

``` html
<style>
  .box {
    width: 280px;
    outline: 1px solid pink;
    text-align: center;
    font-size: 14px;
    line-height: 20px;
  }
  .box > img {
    height: 96px;
    display: block;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

- 容器line-height足够小，只要保证字母x的半行间距小到字母x的下边缘位置或以上，例如line-height:0。
- 容器font-size足够小。

``` html
<style>
  .box {
    width: 280px;
    outline: 1px solid pink;
    text-align: center;
    font-size: 0;
  }
  .box > img {
    height: 96px;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

- 设置vertical-align为其他属性值，例如top、middle或者bottom等。

``` html
<style>
  .box {
    width: 280px;
    outline: 1px solid pink;
    text-align: center;
  }
  .box > img {
    height: 96px;
    vertical-align: bottom;
  }
</style>
<body>
  <div class="box">
    x<img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

之前提到的"内联特性导致margin无效"

``` html
<style>
  .box {
    width: 280px;
    outline: 1px solid pink;
    text-align: center;
  }
  .box > img {
    height: 96px;
    margin-top: -800px;
  }
</style>
<body>
  <div class="box">
    x<img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

> 按照理解margin-top值远远超过图片的高度，图片应该跑到容器外面，但是事实上图片仍然有部分在.box元素中，就算margin-top设置成-9999px，图片也不会继续往上移。在css中，非主动触发位移的内联元素是不可能跑到计算容器外面的，导致图片的位置被字母x或“幽灵空白节点”的vertical-align:baseline给限死。


``` html
<style>
  .box {
    text-align: justify;
    background-color: pink;
  }
  .fix {
    display: inline-block;
    width: 96px;
    outline: 1px solid brown;
  }

  img {
    height: 100px;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <i class="fix"></i>
    <i class="fix"></i>
    <i class="fix"></i>
  </div>
</body>
```

> 此时i标签的上面和下面都产生了间隙。

``` html
<style>
  .box {
    text-align: justify;
    background-color: pink;
    line-height: 0; /* 解决图片和图片之间的间隙 */
  }
  .fix {
    display: inline-block;
    width: 96px;
    outline: 1px solid brown;
  }

  img {
    height: 100px;
  }
</style>
```

> 此时i标签和img之间仍然有间隙。此时inline-block和baseline在起作用。

#### vertical-align线性类属性值

1. inline-block和baseline

vertical-align:baseline的作用：文本之类的内联元素就是字符x的下边缘，而替换元素则是替换元素的下边缘。但是对于inline-block元素则不一样：

inline-block元素里面没有内联元素，或者overflow不是visible，则该元素的基线就是其margin的底边缘，否则基线就是元素里最后一行内联元素的基线。


``` html
<style>
  span {
    display: inline-block;
    width: 150px;
    height: 150px;
    border: 1px solid #eee;
    background-color: pink;
  }
</style>
<body>
  <span></span>
  <span>x</span>
</body>
```
> 第一个span的基线是容器的margin下边缘，第二个span的基线是字母x的下边缘，于是左边span的下边缘和右边span中的x的下边缘对齐。

``` html
<style>
  span {
    display: inline-block;
    width: 150px;
    height: 150px;
    border: 1px solid #eee;
    background-color: pink;
    line-height: 0;
  }
</style>
<body>
  <span></span>
  <span>x</span>
</body>
```


此时如果设置line-height为0，字符占据的高度也为0，此时高度的起始位置就变成了字符内容区域的垂直中心位置，于是文字就有一半落在了框的外面，由于文字字符上移了，自然基线位置也往上移动了，于是两个框的垂直落差就更大了。


再来看看之前的问题

``` html
<style>
  .box {
    text-align: justify;
    background-color: pink;
    line-height: 0;
  }
  .fix {
    display: inline-block;
    width: 96px;
    outline: 1px solid brown;
  }

  img {
    height: 100px;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <i class="fix">x</i>
    <i class="fix"></i>
    <i class="fix"></i>
  </div>
</body>
```

此时给第一个i标签加上字母x之后(模拟不可见的幽灵空白节点)，导致字母x的基线在line-height:0的情况下在上移，导致inline-block元素在有内联元素的情况下基线是内联元素的基线(否则是本身的下边缘)，最终inline-block的元素基线上移，为了使内联元素基线和中线重合在一起，可以改变"幽灵空白节点"的基线位置。

``` html
<style>
  .box {
    text-align: justify;
    background-color: pink;
    line-height: 0;
    font-size: 0;
  }
  .fix {
    display: inline-block;
    width: 96px;
    outline: 1px solid brown;
  }

  img {
    height: 100px;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <i class="fix">x</i>
    <i class="fix"></i>
    <i class="fix"></i>
  </div>
</body>
```

> 由于字体足够小，基线和中线会重合在一起。且没有上下半行距产生空白间隙。使用font-size:0可以使各类对齐效果更彻底。当然同样可以采用vertical-bottom/top(前提仍然是line-height:0)解决产生空隙的问题。

2. 了解vertical-align:top/bottom


vertical-top:
- 内联元素：元素底部和当前行框盒子的顶部对齐
- table-cell元素： 元素底padding边缘和表格行的顶部对齐

3. vertical-align:middle与近似垂直居中

- 内联元素：元素的垂直中心点和行框盒子基线往上1/2 x-height处对齐(通过设置font-size:0可以实现真正的垂直居中，此时根据line-height的半行距上下等分规则，这个点就正好是整个容器的垂直中心点)
- table-cell元素：单元格填充盒子相对于外面的表格行居中对齐

#### 深入理解vertical-align文本类属性值

- vertical-align:text-top 盒子的顶部和父级内容区的顶部对齐
- vertical-align:text-bottom 盒子的底部和父级内容区域的底部对齐


``` html
<style>
  .box {
    background-color: pink;
    font-size: 24px;
  }

  span:first-child {
    font-size: 16px;
    vertical-align: bottom;
  }

  span:nth-child(2) {
    font-size: 24px;
    vertical-align: bottom;
  }

  span:last-child {
    font-size: 36px;
    vertical-align: bottom;
  }
</style>
<body>
  <div class="box">
    <span>16px</span>
    <span>24px</span>
    <span>36px</span>
  </div>
</body>
```

#### 了解vertical-align上标和下标类属性值

- vertical-align:sub 提高盒子的基线到父级合适的上标基线位置
- vertical-align:super 降低盒子的基线到父级合适的下标基线位置

#### 无处不在的vertical-align

对于内联元素，如果有不好理解的现象肯定是“幽灵空白节点”以及无处不在的vertical-align属性在启作用。

而top/bottom和baseline/middle则完全不同，前者对齐看边缘看行框盒子，而后者对齐则是和字符x相关。

#### 基于vertical-align属性的水平垂直居中弹框

``` html
<style>
  .box {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background-color: rgba(0, 0, 0, .5);
    text-align: center;
    font-size: 0;
    white-space: nowrap;
    overflow: auto;
  }

  .box:after {
    content: '';
    display: inline-block;
    height: 100%;
    vertical-align: middle;
  }

  .dialog {
    display: inline-block;
    vertical-align: middle;
    text-align: left;
    font-size: 14px;
    white-space: normal;
    background-color: white;
    padding: 100px;
    border-radius: 10px;
  }
</style>
<body>
  <div class="box">
    <div class="dialog">
      内容占位内容占位内容占位内容占位内容占位内容占位内容占位内容占位内容占位内容占位内容占位
    </div>
  </div>
</body>
```

- 节省了很多无谓的定位js代码，不需要浏览器resize事件之类的处理，当弹框内容动态变化时也无须重新定位

- 性能更改、渲染速度更快(浏览器内置的CSS的即时渲染比js处理更好)
- 可以非常灵活的设置垂直居中的比例，比例.box { height: 90% }
- 设置overflow:auto可以实现弹框高度超过一屏时可以出现滚动条
- 这里有两个vertical-align:middle(如果是一个vertical-align，则和line-height有关)，因为弹窗的高度不确定，虽然不能使用某个具体的行高值创建足够的内联元素，于是借助伪元素创建了一个和外部容器一样高的宽度为0的inline-block元素，类似于幽灵空白节点。
- 设置font-size:0，所以x的中心位置就是box的上边缘，此时高度100%宽度为0的伪元素和这个中心位置对齐，由于设置了vertical-align:middle则使x中心点位置和容器的垂直中心线对齐。
- 弹框元素.dialog也设置了vertical-align:middle，根据定义，弹框的垂直中心位置和x中心点位置对齐，于是dialog元素就和容器的垂直中心位置对齐了，从而实现垂直居中的效果。
- 水平居中使用text-align:center。

## 流的破坏与保护

### float

#### float本质

浮动的本质是为了实现文字环绕效果。
浮动没有内联元素的间隙问题和margin合并问题。

float特性：
- 包裹性
- 块状化并格式化上下文
- 破坏文档流
- 无margin合并问题

1. 包裹

``` html
<style>
  .box {
    width: 200px;
  }
  .float {
    float: left;
  }

  .float > img {
    width: 100px;
  }
</style>
<body>
  <div class="box">
    <div class="float">
      <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    </div>
  </div>
</body>
```
> 此时float的宽度就是img的宽度。

2. 自适应性

``` html
<style>
  .box {
    width: 200px;
  }
  .float {
    float: left;
  }

  .float > img {
    width: 100px;
  }
</style>
<body>
  <div class="box">
    <div class="float">
      <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
      我是大傻瓜我是大傻瓜我是大傻瓜我是大傻瓜我是大傻瓜我是大傻瓜我是大傻瓜我是大傻瓜我是大傻瓜我是大傻瓜
    </div>
  </div>
</body>
```

> 此时float的宽度是200px，适应父元素的宽度。

西方文字的首选最小宽度由特定的连续的英文字符单元决定，会终止于空格、短横线、问号以及其他非英文字符等(如果想要英文字符和中文字符一样，每个字符都用最小宽度单元，可以使用word-break: break-all)。

浮动元素想要最大宽度自适应父元素的宽度，前提是浮动元素的“首选最小宽度”比父元素的宽度要小的前提下。因此如果上面例子把文字全部替换成连续的英文字符串或者数字串则宽度还是可能超过父元素的宽度。

3. 块状化

元素的一旦float的属性值不为none，则display计算值一定是block或者table.


``` html
<style>
  .box {
    width: 200px;
  }
  .float {
    float: left;
  }

  .float > img {
    width: 100px;
  }
</style>
<body>
  <div class="box">
    <span class="float">
      <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
      11111111111111111111111111111111111111111111111111111111111111
    </span>
  </div>
</body>
```

> 此时span元素的display为block，且span元素超过了父元素的大小。需要注意inline-table元素浮动后display:table，其他元素都转化为display:block。

因此以下css组合是不合理的


``` html
<style>
  .float {
    float: left; // 有了float之后不需要display:block
    display: block; 
    vertical-align: center; // 只对内联元素有效
    text-align: right; // text-align对浮动元素无效
  }
</style>
```

4. 格式化上下文(BFC)


#### float的作用机制

- 父级元素高度塌陷(非BUG), 查看[清除和去除浮动的方法详解](https://ziyi2.github.io/2017/08/02/%E6%B8%85%E9%99%A4%E5%92%8C%E5%8E%BB%E9%99%A4%E6%B5%AE%E5%8A%A8%E7%9A%84%E6%96%B9%E6%B3%95%E8%AF%A6%E8%A7%A3.html#more)
- 行框盒子的区域限制

1. 父级元素高度塌陷

``` html
<style>
  .box {
    width: 200px;
  }
  .float {
    float: left;
    display: block;
    text-align: right;
  }

  .float > img {
    width: 100px;
  }
</style>
<body>
  <div class="box">
    <span>猫猫猫猫猫猫猫猫猫猫猫猫猫</span>
    <span class="float">
      <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    </span>
    <span>猫猫猫猫猫猫猫猫猫猫猫猫猫</span>
  </div>
</body>
```

> 父元素box此时高度塌陷，详情可查看[清除和去除浮动的方法详解](https://ziyi2.github.io/2017/08/02/%E6%B8%85%E9%99%A4%E5%92%8C%E5%8E%BB%E9%99%A4%E6%B5%AE%E5%8A%A8%E7%9A%84%E6%96%B9%E6%B3%95%E8%AF%A6%E8%A7%A3.html#more), 但是文字围绕在图片周围，这就是浮动的作用。


2. 行框盒子的区域限制

``` html
<style>
  .box {
    width: 200px;
  }
  .float {
    float: left;
    display: block;
    text-align: right;
  }

  .float > img {
    width: 100px;
    opacity: 0;
  }

  p:first-line {
    background-color: pink;
    color: brown;
  }
</style>
<body>
  <div class="box">
    <span>猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫</span>
    <span class="float">
      <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    </span>
    <p>猫猫猫猫猫猫猫猫猫猫猫猫猫</p>
  </div>
</body>
```

需要注意p元素和img元素是完全重叠的，例如给环绕的p元素设置一个背景色，同时给img透明

``` html
<style>
  .box {
    width: 200px;
  }
  .float {
    float: left;
    display: block;
    text-align: right;
  }

  .float > img {
    width: 100px;
    opacity: 0;
  }

  p {
    background-color: pink;
    color: brown;
  }
</style>
<body>
  <div class="box">
    <span>猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫猫</span>
    <span class="float">
      <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    </span>
    <p>猫猫猫猫猫猫猫猫猫猫猫猫猫</p>
  </div>
</body>
```

> p元素的背景色在图片区域也存在，说明块状盒子和图片是完全重叠的(块状盒子由行框盒子组成，行框盒子由每行内联元素所在的那些内联盒盒子组成)。

需要注意的是p元素整体虽然和图片重叠，但是p元素的每个行框盒子则是被浮动元素限制，没有任何重叠

``` html
<style>
  .box {
    width: 200px;
  }
  .float {
    float: left;
    display: block;
    text-align: right;
  }

  .float > img {
    width: 100px;
    opacity: 0;
  }

  p:first-line {
    background-color: pink;
    color: brown;
  }
</style>
<body>
  <div class="box">
    <span class="float">
      <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    </span>
    <p>猫猫猫猫猫猫猫猫猫猫猫猫猫</p>
  </div>
</body>
```

> 此时p元素的第一行的背景色并没有和图片区域重合，就算给p元素一个margin-left负值，p元素所在的这些行框盒子仍然不会改变位置，而是会被左边的浮动元素限制住。

#### float更深入的作用机制

``` html
<style>
  .box {
    width: 200px;
  }
  a {
    float: right;
  }
</style>
<body>
  <div class="box">
    <h3>房地产销售房地产销售房地产销售 <a href="">更多</a></h3>
  </div>
</body>
```
> 此时更多显示在下一行的右边，而不是首行的右边。


为什么不显示在首行而是显示在最后一行的右边，此时需要了解两个和float相关的术语

- 浮动锚点(float元素所在的流中的一个点，本身并不浮动，像是没有margin、border和padding的空内联元素)
- 浮动参考(浮动元素对齐参考的实体)


float元素的“浮动参考”是“行框盒子”，float元素在当前“行框盒子”(是行框盒子而不是外面的包含块盒子)内定位，所以以上的a标签定位参考的是它所在的行框盒子(h3因为内容区域限制换行后的第二行行框盒子)，如果是相对包含块盒子，那么a标签此时应该在box的最右边，也就是第一行的最右边。

h3因为内容区域限制而导致换行，从而产生了两个行框盒子，而a标签紧跟在h3后面，因此a标签参考定位的行框盒子是第二个行框盒子，那么如果第二个行框盒子正好占满了h3的内容(或者说浮动元素所在的行框盒子已经容纳不下浮动元素)，而第三行却有没有h3的内容，此时a标签如何展示


``` html
<style>
  .box {
    width: 200px;
  }
  a {
    float: right;
  }
</style>
<body>
  <div class="box">
    <h3>房地产销售房地产销售房地产销售地产销售销 <a href="">更多</a></h3>
  </div>
</body>
```

> 此时a标签在下一行的最右侧显示。


之前所说浮动元素是参考当前所在的行框盒子定位，但是这里h3的内容高度只是它所在的文字区域的高度，而a标签则跑到了这个文字区域的下一行，这正是“浮动锚点”在起作用，“浮动锚点”本身是浮动元素所在流中的一个空内联元素，其作用就是产生一个行框盒子(有内联元素必有行框盒子)，于是浮动元素就能参考这个行框盒子进行定位，只是这个行框盒子没有任何内容。


#### float与流体布局

实现一侧定宽，一侧宽度自适应的两栏自适应布局


``` html
<style>
  .box {
    overflow: hidden; /* 清浮动 */
  }
  .box-left {
    float: left;
    width: 200px;
    background-color: peru;
    height: 200px;
  }
  .box-right {
    margin-left: 210px;
    background-color: pink;
    height: 200px;
  }

</style>
<body>
  <div class="box">
    <div class="box-left"></div>
    <div class="box-right"></div>
  </div>
</body>
```
> box-right元素没有设置浮动，也没有设置宽度，因此具有很好的流动性，设置margin-left、border-left或padding-left都可以自动改变content-box的尺寸，从而实现宽度自适应的布局效果。需要注意的是这里不应该设置box-right元素的width从而破坏元素的流动性。


上面的例子适用于一侧定宽一侧不固定，如果宽度不固定的流式布局，则可以使用百分比设置box-left的宽度以及box-right的margin值。

如果是多栏流式布局，也同样适用，例如三栏流式布局


``` html
<style>
  .box {
    overflow: hidden; /* 清浮动 */
  }
  .box-left, .box-right {
    width: 10%;
    background-color: peru;
    height: 200px;
  }
  .box-left {
    float: left;
  }
  .box-right {
    float: right;
  }
  .box-center {
    margin: 0 10%;
    background-color: pink;
    height: 200px;
  }

</style>
<body>
  <div class="box">
    <div class="box-left"></div>
    <div class="box-right"></div>
    <div class="box-center"></div>
  </div>
</body>
```

> 此方法需要注意正常流元素需要放在两个浮动元素之前，否则浮动元素会被挤到下一列，因为box-center是正常的块级元素。


### clear属性

#### clear属性介绍

clear:元素盒子的边不能和前面的浮动元素相邻。需要注意凡是left和right起作用的地方，都可以使用both。clear属性只能清除前面的浮动元素，不能清除后面的浮动元素


``` html
<style>
  .box {
    border: 1px solid pink;
  }

  .box:before { // 换成after可以
    display: table;
    content: '';
    clear: both;
  }

  .box-left {
    float: left;
  }
</style>
<body>
  <div class="box">
    <div class="box-left">1111</div>
  </div>
</body>

```
> 此时清除浮动是没有任何效果的，使用after伪元素可以。


#### clear属性的限制

clear属性只有块级元素才有效，一般after伪元素默认都是内联水平，所以使用伪元素清除浮动时需要设置display值（table或者block或者list-item都可以）。


clear:both的本质是让自己不和float元素在一行显示，因此会有一些副作用特性

- 如果clear:both元素前面的元素就是float元素，则margin-top负值没有任何效果
- clear:both后面的元素仍旧可能发生文字环绕现象


``` html
<style>
  .box:after {
    display: table;
    content: '';
    clear: both;
  }

  .box img {
    width: 100px;
    float: left;
  }

  .box + p {
    margin: 0;
    padding: 0;
    margin-top: -1px;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊
  </div>
  <p>啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊</p>
</body>
```      
> 此时尽管box清除了浮动，但是p元素仍然会围绕图片。


###  BFC结界

BFC全称block formatting context，称为“块级格式化上下文”。相对应的还有IFC(inline formatting context)，称为“内联格式化上下文”。

BFC特性就像一个结界特性，会形成一个封闭空间，内部子元素无论是什么表现形式都不会影响到BFC外部的元素，如果元素具有BFC特性，那么BFC元素之间是不可能发生margin重叠的，因为margin重叠是会影响外面的元素的。BFC元素也可以用来清楚浮动的影响，因为BFC元素不会影响外部的元素，如果不清楚浮动则会导致父元素高度塌陷从而影响后面元素的布局和定位，这显然和BFC元素的子元素不会影响外部元素不符。

什么情况下会触发BFC？

-  html根元素
- float不为none的元素
- overflow的值为auto、scroll或hidden的元素
- display为table-cell、table-caption和inline-block的元素
- position的值不为realative和static的元素

理论上来说只要元素符合上面任意一个条件，就无须使用clear:both属性去清除浮动的影响。

#### BFC与流体布局

``` html
<style>
  .box {
    width: 200px;
  }
  .box:after {
    display: table;
    content: '';
    clear: both;
  }
  .box img {
    width: 100px;
    float: left;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <p>啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊</p>
  </div>
</body>
```

> 此时文字会环绕图片，而如果超出图片的部分则会显示在图片的下面。

此时如果使P元素具有BFC特性

``` html
<style>
  .box {
    width: 200px;
  }
  .box:after {
    display: table;
    content: '';
    clear: both;
  }
  .box img {
    width: 100px;
    float: left;
  }

  .box p {
    overflow: hidden;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <p>啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊</p>
  </div>
</body>
```
> 此时BFC元素不会受外部元素的影响，也不会影响外部元素，因此这里的p元素不和浮动元素产生交集，会顺着浮动边缘形成自己的封闭上下文。

如果此时希望img和p之间有一个10px大小的间隙

- img { margin-right: 10px }
- img { border-right: 10px }
- img { padding-right: 10px }
- p { border-left: 10px solid transparent }
- p { padding-left: 10px }

> 注意如果想使用p元素的margin-left，那就需要加上img的宽度大小，这样就变成动态不可控了。

``` html
<style>
  .box {
    width: 200px;
  }
  .box:after {
    display: table;
    content: '';
    clear: both;
  }
  .box img {
    width: 100px;
    float: left;
    margin-right: 10px;
  }

  .box p {
    overflow: hidden;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <p>啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊</p>
  </div>
</body>
```

和基于纯流体特性实现的两栏或多栏自适应布局相比，基于BFC特性的自适应布局有如下有点

- 1. 自适应内容封闭而更健壮，容错性更强，比如在BFC元素的子元素中设置clear属性不会和左侧img元素相互干扰。
- 2. 自适应元素自动填满浮动以外的区域，无须关心浮动元素宽度。

两栏BFC特性的自适应布局

``` html
<style>
  .bfc {
    overflow: hidden;
  }
  .right {
    float: right;
  }
  .left {
    width: 100px;
    float: left;
  }
</style>
<body>
  <div class="bfc">
    <img class="left" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <p class="right">啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊</p>
  </div>
</body>
```

BFC自适应布局的缺陷

- float:left 浮动元素本身BFC化，浮动元素本身具有破坏性和包裹性，失去了元素本身的流体自适应性，因此无法实现自动填满容器的自适应布局。

- position:absolute 脱离文档流。
- overflow: hidden 不像浮动和绝对定位对于布局的副作用很大，块状元素的流体特性保存完好，附上BFC的独立区域特性，从IE7就开始支持，兼容性也不错。唯一的问题是容器盒子外的元素可能会被隐藏掉，一定程度上限制了使用规模，不过溢出隐藏的交互场景比例不高，可以作用常用BFC布局属性使用。
- display:inline-block 会让元素包裹性收缩，完全没有block的流体特性，但是在IE6和IE7下block水平元素设置display:inline-block元素还保持block的水平特性
- display: table-cell IE8及以上版本才支持，也具有收缩性，但是单元格如果宽度值设置的很大，实际宽度也不会超过表格容器的宽度，因此可以给这样表现的元素设置超级大的宽度，例如9999px，让元素像block元素一样保持流体特性的同时保持BFC特性(连续英文字符换行吃力，需要设置word-break)
- display: table-row 无法自适应于剩余容器控件
- display: table-caption 一无是处


因此总结出来能够用作布局的

- overflow: auto/hidden 适用于IE7及以上版本
- display: inline-block 适用于IE6和IE7
- display: table-cell 适用于IE8及以上版本

最终可以采用以下两种方案

``` html
<style>
  .bfc {
    overflow: hidden;
  }
</style>
```

``` html
<style>
  .bfc {
    display: table-cell;
    width: 9999px;
    /* 兼容IE7处理 */
    *display: inline-block;
    *width: auto;
  }
</style>
```

关于display: table-cell元素内连续英文字符无法换行的问题，可以解决

``` html
<style>
.word-break {
  display: table;
  width: 100%;
  table-layout: fixed; /* 此时匿名table-cell元素宽度100% */
  word-break: break-all;
}
</style> 
```
> 注意这里设置的display:table其实是table默认会生成一个匿名的table-cell，是这个匿名的table-cell元素形成了BFC。

### 最佳结界overflow

想要彻底清除浮动的影响，最适合的属性不是clear而是overflow，一般使用overflow:hidden。利用BFC的“结界”特性彻底解决浮动对外部或兄弟元素的影响。overflow也不会影响原先的流体特性或宽度表现，需要注意的是overflow的本职工作是对块容器元素的内容溢出进行裁剪。

#### overflow的裁剪界线borde box。

``` html
<style>
  .bfc {
    width: 100px;
    height: 100px;
    border: 10px solid pink;
    padding: 10px;
    overflow: hidden;
  }

  .right {
    float: left;
  }

  .left {
    width: 200px;
    float: left;
  }
</style>
<body>
  <div class="bfc">
    <img class="left" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

> 裁剪的内容是border-box的内边缘，而非padding-box的内边缘，因此padding-right和padding-bottom都被裁剪掉了。

``` html
<style>
  .bfc {
    width: 100px;
    height: 100px;
    border: 10px solid pink;
    padding: 10px;
    overflow: auto;
  }

  .right {
    float: left;
  }

  .left {
    width: 200px;
    float: left;
  }
</style>
<body>
  <div class="bfc">
    <img class="left" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

> 在chrome下，滚动到最低端会有padding-bottom留白(算在滚动尺寸中)，在IE和Firefox会忽略padding-bottom。为了避免出现兼容性问题，在实际开发中应该避免在滚动容器设置padding-bottom值，除了样式表现不统一，还会导致scrollHeight值不一样。

#### overflow-x和overflow-y

除非overflow-x和overflow-y的属性值都是visible，否则visible会当成auto来解析。

``` html
<style>
html {
  overflow-x: hidden;
  overflow-y: auto; /* 多余 */
}
</style>
```

#### overflow与滚动条

HTML中有两个标签默认是可以产生滚动条的，一个是根元素html，另一个是textarea。

- 在PC端，默认滚动条均来自html，而不是body标签。PC端的窗口滚动高度可以使用document.documentElement.scrollTop获取，在移动端使用document.body.scrollTop获取。

- PC端的滚动条会占用容器的可用宽度或高度。移动端不会，因为屏幕有限，滚动条一般都是悬浮模式，不会占据可用宽度，滚动栏所占据的宽度是精准的17px。

#### 依赖overflow的样式表现

单行文字溢出的...效果实现必须使用如下3个声明

``` html
<style>
  .ell {
    text-overflow: ellipsis;
    white-space: nowrap;
    overflow: hidden;
  }
</style>
```

#### overflow与锚点定位

1. 锚点定位行为的触发条件

- URL地址中的锚链与锚点元素对应并有交互行为
- 可focus的锚点元素处于focus状态

focus锚点定位指的是类链接、按钮、输入框等可以被focus的元素在被focus时发生的页面重定位现象。使用Tab快速定位可focus的元素时，元素如果正好在屏幕之外，浏览器会自动重定位，将这个屏幕之外的元素定位到屏幕之中。如果一个可读写的input元素在屏幕之外，执行document.querySelector('input').focus()输入框就会自动定位到屏幕之中。

> focus锚点定位不依赖于javascript，是浏览器内置的无障碍访问行为。

"URL地址锚链定位"是让元素定位在浏览器窗体的上边缘，而"focus锚点定位"是让元素在浏览器窗体范围内显示即可，不一定在上边缘。

2. 锚点定位作用的本质

锚点定位的本质是改变容器滚动(注意和浏览器滚动是有区别的)的高度或宽度来实现。

``` html
<style>
  .box {
    height: 120px;
    border: 1px solid pink;
    overflow: auto;
  }
  .box-content {
    height: 200px;
    background: palegoldenrod;
  }
  .left {
    width: 200px;
    float: left;
  }
</style>
<body>
  <div class="box">
    <div class="box-content"></div>
    <h4 id="link">锚链位置</h4>
  </div>
  <p><a href="#link">前往</a></p>
</body>
```
> 锚点定位可以发生在普通的容器元素上。


``` html
<style>
  .box {
    height: 120px;
    border: 1px solid pink;
    overflow: auto;
  }
  .content {
    height: 900px;
    background: orchid;
  }
  .box-content {
    height: 200px;
    background: palegoldenrod;
  }
  .left {
    width: 200px;
    float: left;
  }
</style>
<body>
  <div class="box">
    <div class="box-content"></div>
    <h4 id="link">锚链位置</h4>
  </div>
  <div class="content"></div>
  <p><a href="#link">前往</a></p>
</body>
```

> 锚点定位可以由内而外，普通元素和窗体同时可滚动的时候，会由内而外触发所有可能滚动窗体的锚点定位行为。此时box元素先触发锚点定位行为，滚动到底部，然后触发窗体的锚点定位，h4和浏览器窗口的上边缘对齐。

#### BFC与流体布局

``` html
<style>
  .box {
    width: 200px;
  }
  .box:after {
    display: table;
    content: '';
    clear: both;
  }
  .box img {
    width: 100px;
    float: left;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <p>啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊</p>
  </div>
</body>
```

> 此时文字会环绕图片，而如果超出图片的部分则会显示在图片的下面。

此时如果使P元素具有BFC特性

``` html
<style>
  .box {
    width: 200px;
  }
  .box:after {
    display: table;
    content: '';
    clear: both;
  }
  .box img {
    width: 100px;
    float: left;
  }

  .box p {
    overflow: hidden;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <p>啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊</p>
  </div>
</body>
```
> 此时BFC元素不会受外部元素的影响，也不会影响外部元素，因此这里的p元素不和浮动元素产生交集，会顺着浮动边缘形成自己的封闭上下文。

如果此时希望img和p之间有一个10px大小的间隙

- img { margin-right: 10px }
- img { border-right: 10px }
- img { padding-right: 10px }
- p { border-left: 10px solid transparent }
- p { padding-left: 10px }

> 注意如果想使用p元素的margin-left，那就需要加上img的宽度大小，这样就变成动态不可控了。

``` html
<style>
  .box {
    width: 200px;
  }
  .box:after {
    display: table;
    content: '';
    clear: both;
  }
  .box img {
    width: 100px;
    float: left;
    margin-right: 10px;
  }

  .box p {
    overflow: hidden;
  }
</style>
<body>
  <div class="box">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <p>啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊</p>
  </div>
</body>
```

和基于纯流体特性实现的两栏或多栏自适应布局相比，基于BFC特性的自适应布局有如下有点

- 1. 自适应内容封闭而更健壮，容错性更强，比如在BFC元素的子元素中设置clear属性不会和左侧img元素相互干扰。
- 2. 自适应元素自动填满浮动以外的区域，无须关心浮动元素宽度。

两栏BFC特性的自适应布局

``` html
<style>
  .bfc {
    overflow: hidden;
  }
  .right {
    float: right;
  }
  .left {
    width: 100px;
    float: left;
  }
</style>
<body>
  <div class="bfc">
    <img class="left" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    <p class="right">啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊啊</p>
  </div>
</body>
```

BFC自适应布局的缺陷

- float:left 浮动元素本身BFC化，浮动元素本身具有破坏性和包裹性，失去了元素本身的流体自适应性，因此无法实现自动填满容器的自适应布局。

- position:absolute 脱离文档流。
- overflow: hidden 不像浮动和绝对定位对于布局的副作用很大，块状元素的流体特性保存完好，附上BFC的独立区域特性，从IE7就开始支持，兼容性也不错。唯一的问题是容器盒子外的元素可能会被隐藏掉，一定程度上限制了使用规模，不过溢出隐藏的交互场景比例不高，可以作用常用BFC布局属性使用。
- display:inline-block 会让元素包裹性收缩，完全没有block的流体特性，但是在IE6和IE7下block水平元素设置display:inline-block元素还保持block的水平特性
- display: table-cell IE8及以上版本才支持，也具有收缩性，但是单元格如果宽度值设置的很大，实际宽度也不会超过表格容器的宽度，因此可以给这样表现的元素设置超级大的宽度，例如9999px，让元素像block元素一样保持流体特性的同时保持BFC特性(连续英文字符换行吃力，需要设置word-break)
- display: table-row 无法自适应于剩余容器控件
- display: table-caption 一无是处


因此总结出来能够用作布局的

- overflow: auto/hidden 适用于IE7及以上版本
- display: inline-block 适用于IE6和IE7
- display: table-cell 适用于IE8及以上版本

最终可以采用以下两种方案

``` html
<style>
  .bfc {
    overflow: hidden;
  }
</style>
```

``` html
<style>
  .bfc {
    display: table-cell;
    width: 9999px;
    /* 兼容IE7处理 */
    *display: inline-block;
    *width: auto;
  }
</style>
```

关于display: table-cell元素内连续英文字符无法换行的问题，可以解决

``` html
<style>
.word-break {
  display: table;
  width: 100%;
  table-layout: fixed; /* 此时匿名table-cell元素宽度100% */
  word-break: break-all;
}
</style> 
```
> 注意这里设置的display:table其实是table默认会生成一个匿名的table-cell，是这个匿名的table-cell元素形成了BFC。

### 最佳结界overflow

想要彻底清除浮动的影响，最适合的属性不是clear而是overflow，一般使用overflow:hidden。利用BFC的“结界”特性彻底解决浮动对外部或兄弟元素的影响。overflow也不会影响原先的流体特性或宽度表现，需要注意的是overflow的本职工作是对块容器元素的内容溢出进行裁剪。

#### overflow的裁剪界线borde box。

``` html
<style>
  .bfc {
    width: 100px;
    height: 100px;
    border: 10px solid pink;
    padding: 10px;
    overflow: hidden;
  }

  .right {
    float: left;
  }

  .left {
    width: 200px;
    float: left;
  }
</style>
<body>
  <div class="bfc">
    <img class="left" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

> 裁剪的内容是border-box的内边缘，而非padding-box的内边缘，因此padding-right和padding-bottom都被裁剪掉了。

``` html
<style>
  .bfc {
    width: 100px;
    height: 100px;
    border: 10px solid pink;
    padding: 10px;
    overflow: auto;
  }

  .right {
    float: left;
  }

  .left {
    width: 200px;
    float: left;
  }
</style>
<body>
  <div class="bfc">
    <img class="left" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

> 在chrome下，滚动到最低端会有padding-bottom留白(算在滚动尺寸中)，在IE和Firefox会忽略padding-bottom。为了避免出现兼容性问题，在实际开发中应该避免在滚动容器设置padding-bottom值，除了样式表现不统一，还会导致scrollHeight值不一样。

#### overflow-x和overflow-y

除非overflow-x和overflow-y的属性值都是visible，否则visible会当成auto来解析。

``` html
<style>
html {
  overflow-x: hidden;
  overflow-y: auto; /* 多余 */
}
</style>
```

#### overflow与滚动条

HTML中有两个标签默认是可以产生滚动条的，一个是根元素html，另一个是textarea。

- 在PC端，默认滚动条均来自html，而不是body标签。PC端的窗口滚动高度可以使用document.documentElement.scrollTop获取，在移动端使用document.body.scrollTop获取。

- PC端的滚动条会占用容器的可用宽度或高度。移动端不会，因为屏幕有限，滚动条一般都是悬浮模式，不会占据可用宽度，滚动栏所占据的宽度是精准的17px。

#### 依赖overflow的样式表现

单行文字溢出的...效果实现必须使用如下3个声明

``` html
<style>
  .ell {
    text-overflow: ellipsis;
    white-space: nowrap;
    overflow: hidden;
  }
</style>
```

#### overflow与锚点定位

1. 锚点定位行为的触发条件

- URL地址中的锚链与锚点元素对应并有交互行为
- 可focus的锚点元素处于focus状态

focus锚点定位指的是类链接、按钮、输入框等可以被focus的元素在被focus时发生的页面重定位现象。使用Tab快速定位可focus的元素时，元素如果正好在屏幕之外，浏览器会自动重定位，将这个屏幕之外的元素定位到屏幕之中。如果一个可读写的input元素在屏幕之外，执行document.querySelector('input').focus()输入框就会自动定位到屏幕之中。

> focus锚点定位不依赖于javascript，是浏览器内置的无障碍访问行为。

"URL地址锚链定位"是让元素定位在浏览器窗体的上边缘，而"focus锚点定位"是让元素在浏览器窗体范围内显示即可，不一定在上边缘。

2. 锚点定位作用的本质

锚点定位的本质是改变容器滚动(注意和浏览器滚动是有区别的)的高度或宽度来实现。

``` html
<style>
  .box {
    height: 120px;
    border: 1px solid pink;
    overflow: auto;
  }
  .box-content {
    height: 200px;
    background: palegoldenrod;
  }
  .left {
    width: 200px;
    float: left;
  }
</style>
<body>
  <div class="box">
    <div class="box-content"></div>
    <h4 id="link">锚链位置</h4>
  </div>
  <p><a href="#link">前往</a></p>
</body>
```
> 锚点定位可以发生在普通的容器元素上。


``` html
<style>
  .box {
    height: 120px;
    border: 1px solid pink;
    overflow: auto;
  }
  .content {
    height: 900px;
    background: orchid;
  }
  .box-content {
    height: 200px;
    background: palegoldenrod;
  }
  .left {
    width: 200px;
    float: left;
  }
</style>
<body>
  <div class="box">
    <div class="box-content"></div>
    <h4 id="link">锚链位置</h4>
  </div>
  <div class="content"></div>
  <p><a href="#link">前往</a></p>
</body>
```

> 锚点定位可以由内而外，普通元素和窗体同时可滚动的时候，会由内而外触发所有可能滚动窗体的锚点定位行为。此时box元素先触发锚点定位行为，滚动到底部，然后触发窗体的锚点定位，h4和浏览器窗口的上边缘对齐。

需要注意的是设置了overflow:hidden的元素也是可以滚动的，hidden与auto和scroll的差别仅在于有没有滚动条，在设置了overflow:hidden内容高度溢出之后滚动依然存在，仅仅是滚动条不存在(鼠标滚动时不会触发滚动效果)。

需要注意使用锚点定位时，滚动依然会发生

``` html
<style>
  .box {
    height: 120px;
    border: 1px solid pink;
    overflow: hidden;
  }
  .content {
    height: 900px;
    background: orchid;
  }
  .box-content {
    height: 200px;
    background: palegoldenrod;
  }
  .left {
    width: 200px;
    float: left;
  }
</style>
<body>
  <div class="box">
    <div class="box-content"></div>
    <h4 id="link">锚链位置</h4>
  </div>
  <div class="content"></div>
  <p><a href="#link">前往</a></p>
</body>
```

例如使用overflow属性实现无JavaScript交互的选项卡切换效果

``` html
<style>
  .box {
    height: 120px;
    border: 1px solid pink;
    overflow: hidden;
  }
  
  .list {
    height: 100%;
    background: pink;
  }
</style>
<body>
  <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
  <div class="box">
    <div class="list" id="one">1</div>
    <div class="list" id="two">2</div>
    <div class="list" id="three">3</div>
  </div>
  <div class="link">
    <a href="#one">1</a>
    <a href="#two">2</a>
    <a href="#three">3</a>
  </div>
  <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
</body>
```

> 这个设置有很多不足之处，第一个是box元素的容器必须定高，第二个是如果页面html是可以滚动的，那么点击选项卡后页面也会发生跳动，之前已经说过锚点定位是可以由内而外的。



需要注意的是“focus锚点定位”则没有这个问题，只要定位元素在浏览器窗体内，则不会触发窗体的滚动。

``` html
<style>
  .box {
    height: 120px;
    border: 1px solid pink;
    overflow: hidden;
  }
  
  .list {
    height: 100%;
    background: pink;
    position: relative;
  }

  .list > input {
    position: absolute;
    top: 0;
    height: 100%;
    width: 1px;
    border: 0;
    padding: 0;
    margin: 0;
    clip: rect(0 0 0 0);
  }
</style>
<body>
  <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
  <div class="box">
    <div class="list"><input id="one">1</div>
    <div class="list"><input id="two">2</div>
    <div class="list"><input id="three">3</div>
  </div>
  <div class="link">
    <label for="one">1</label>
    <label for="two">2</label>
    <label for="three">3</label>
  </div>
  <br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
</body>
```

> 在每个列表中塞入一个看不见的input输入框，通过label的for与input的id关联，点击label会触发输入框的focus行为，触发锚点定位，实现选项卡切换。还有有一个优点是可以使用Tab键切换。需要注意如果列表部分区域在浏览器外面(例如滚动条往下滚使得列表部分显示在浏览器顶端外部)时依然会产生跳动的问题，此时需要javascript做一定的处理。


需要注意之前使用margin-bottom负值加上padding-bottom正直以及父元素的overflow:hidden配合实现等高布局，如果使用dom.scrollIntoView()或者触发窗体视区范围内的内部元素的锚点定位行为，布局就会飞掉，此时容器的scrollHeight(视区高度+可滚动高度)要远远大于clientHeight(视区高度),而锚点定位的本质就是改变滚动高度，因此容器的滚动高度不是0，发生了类似之前选项卡的效果，产生布局问题。

### position:absolute

position:absolute和float:left/right是兄弟关系，都兼具“块状化”、“破坏性”、“包裹性”等特性。absoulte比float能力更霸道，因此absolute和float同时存在时，float属性是无任何效果的。


``` html
<style>
.box {
  position: absolute;
  float: left; // 无效
}
</style>
```


1. 块状化
position一旦置为absolute或fixed,其display值就是block或table。

``` html
<style>
  span {
    position: absolute;
  }
</style>
<body>
  <span>1111</span>
</body>
```

> 此时span元素显示为display:block。


2. 破坏性

和float类似，absolute破坏正常的流来实现自己的特性表现，但本身还是受到普通的流体元素布局、位置甚至是一些内联相关的CSS属性影响（例如text-align）。

3. 格式化

和float类似，能形成“块状格式化上下文”BFC。

4. 包裹性

display:inline-block具有“包裹性”，如果为了使absolute元素具有包裹性，那么以下设置没必要

``` html
<style>
  span {
    position: absolute;
    display: inline-block;
  }
</style>
```

5. 自适应性

和float或其他“包裹性”声明的自适应性相比，absolute的自适应性最大宽度往往不是由父元素决定，是由“包含块”的差异决定。

#### absolute的包含块

普通元素的百分比宽度是相对于父元素的content-box宽度计算的，绝对定位的宽度是相对于第一个position不为static的祖先元素计算的。

- 根元素html被称为“初始包含块”，其尺寸等同于浏览器可视窗口的大小。
- position是relative或static的元素，“包含块”是最近的块容器祖先盒的content-box边界形成。
- position是fixed的元素，则“包含块”是“初始包含块”。
- position是absolute的元素，则“包含块”由最近的position不为static的祖先元素建立。

如果该祖先元素是inline元素： 
(1). 假设给内联元素的前后各生成一个宽度为0的内联盒子，则这两个内联盒子的padding box外面的包围盒就是内联元素的“包含块”。
(2). 如果该内联元素被跨行分隔了，那么“包含块”未定义。
否则该祖先元素不是inline元素，“包含块”由该祖先元素的padding box边界形成。
如果没有符合的祖先元素，则“包含块”是“初始包含块”。

absolute绝对定位元素的“包含块”有3个明显差异：
1. 内联元素也可以作为“包含块”所在的元素
2. “包含块”所在的元素不是父块级元素，而是最近的position不为static的祖先元素或根元素
3. 边界是padding box而不是content box。


 ``` html
<style>
  span {
    position: absolute;
  }
  big {
    font-size: 30px;
  }
</style>
<body>
  <span>我是很大的<big>111</big>哦</span>
</body>
 ```

> 此时span元素的包含块范围与big元素毫无关系，内联元素的包含块是由生成的前后内联盒子决定的，与里面的内联盒子细节没有任何关系(span元素前后的内联盒子相关，而与span元素内部的内联盒子不相关)。


``` html
<style>
  .box {
    position: absolute;
  }

  .container {
    width: 200px;
    border: 1px solid pink;
    position: relative;
  }
</style>
<body>
  <div class="container">
    <div class="box">
      1111
    </div>
  </div>
</body>
```
>此时box的宽度就是文字的宽度，表现为包裹性。

``` html
<style>
  .box {
    position: absolute;
  }

  .container {
    width: 200px;
    border: 1px solid pink;
    position: relative;
  }
</style>
<body>
  <div class="container">
    <div class="box">
      111111111111111111111111111111111111111111111111111111111111111111111111
    </div>
    <div class="box">
      我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字
    </div>
  </div>
</body>
```

> 此时第一个box不换行，第二个box宽度是container的宽度，是因为被container包含块限制了宽度。因此决定定位的默认最大宽度是“包含块”的宽度，表现为自适应性。需要注意container高度塌陷是因为absolute元素脱离了正常流。此时如果给box元素设置一个max-width:100%是多余的，因为absolute元素默认的最大宽度就是包含块的宽度。需要注意的是有的时候纯表现为包裹性会有换行效果，如果要不换行可以设置white-space:nowrap。

最后需要注意的是计算和定位是相对于祖先定位元素的padding box，这和overflow隐藏的是padding box边界类似，是使用场景决定的。


``` html
<style>
  .container {
    width: 200px;
    height: 200px;
    padding: 50px;
    background-color: pink;
    position: relative;
  }
  .box {
    position: absolute;
    top: 0;
    right: 0;
  }
</style>
<body>
  <div class="container">
    我是内容
    <div class="box">
      小图标
    </div>
  </div>
</body>
```
> 此时box在container的padding区域内。而不是定位在container的content box区域的右上角。

如果想要定位在内容区域的右上角，此时不应该使用padding，因为使用padding需要对top和left设置padding值

``` html
<style>
  .container {
    width: 200px;
    height: 200px;
    padding: 50px;
    background-color: pink;
    position: relative;
  }
  .box {
    position: absolute;
    top: 50px;
    right: 50px;
  }
</style>
<body>
  <div class="container">
    我是内容
    <div class="box">
      小图标
    </div>
  </div>
</body>
```

> 此时如果padding值变化了，那么top和right值也需要跟着变化，从而使得维护性下降。

此时可以使用border属性模拟padding从而达到同样的效果

``` html
<style>
  .container {
    width: 200px;
    height: 200px;
    border: 50px solid transparent;
    background-color: pink;
    position: relative;
  }
  .box {
    position: absolute;
    top: 0;
    right: 0;
  }
</style>
<body>
  <div class="container">
    我是内容
    <div class="box">
      小图标
    </div>
  </div>
</body>
```

### position:absolute

position:absolute和float:left/right是兄弟关系，都兼具“块状化”、“破坏性”、“包裹性”等特性。absoulte比float能力更霸道，因此absolute和float同时存在时，float属性是无任何效果的。


``` html
<style>
.box {
  position: absolute;
  float: left; // 无效
}
</style>
```


1. 块状化
position一旦置为absolute或fixed,其display值就是block或table。

``` html
<style>
  span {
    position: absolute;
  }
</style>
<body>
  <span>1111</span>
</body>
```

> 此时span元素显示为display:block。


2. 破坏性

和float类似，absolute破坏正常的流来实现自己的特性表现，但本身还是受到普通的流体元素布局、位置甚至是一些内联相关的CSS属性影响（例如text-align）。

3. 格式化

和float类似，能形成“块状格式化上下文”BFC。

4. 包裹性

display:inline-block具有“包裹性”，如果为了使absolute元素具有包裹性，那么以下设置没必要

``` html
<style>
  span {
    position: absolute;
    display: inline-block;
  }
</style>
```

5. 自适应性

和float或其他“包裹性”声明的自适应性相比，absolute的自适应性最大宽度往往不是由父元素决定，是由“包含块”的差异决定。

#### absolute的包含块

普通元素的百分比宽度是相对于父元素的content-box宽度计算的，绝对定位的宽度是相对于第一个position不为static的祖先元素计算的。

- 根元素html被称为“初始包含块”，其尺寸等同于浏览器可视窗口的大小。
- position是relative或static的元素，“包含块”是最近的块容器祖先盒的content-box边界形成。
- position是fixed的元素，则“包含块”是“初始包含块”。
- position是absolute的元素，则“包含块”由最近的position不为static的祖先元素建立。

如果该祖先元素是inline元素： 
(1). 假设给内联元素的前后各生成一个宽度为0的内联盒子，则这两个内联盒子的padding box外面的包围盒就是内联元素的“包含块”。
(2). 如果该内联元素被跨行分隔了，那么“包含块”未定义。
否则该祖先元素不是inline元素，“包含块”由该祖先元素的padding box边界形成。
如果没有符合的祖先元素，则“包含块”是“初始包含块”。

absolute绝对定位元素的“包含块”有3个明显差异：
1. 内联元素也可以作为“包含块”所在的元素
2. “包含块”所在的元素不是父块级元素，而是最近的position不为static的祖先元素或根元素
3. 边界是padding box而不是content box。


 ``` html
<style>
  span {
    position: absolute;
  }
  big {
    font-size: 30px;
  }
</style>
<body>
  <span>我是很大的<big>111</big>哦</span>
</body>
 ```

> 此时span元素的包含块范围与big元素毫无关系，内联元素的包含块是由生成的前后内联盒子决定的，与里面的内联盒子细节没有任何关系(span元素前后的内联盒子相关，而与span元素内部的内联盒子不相关)。


``` html
<style>
  .box {
    position: absolute;
  }

  .container {
    width: 200px;
    border: 1px solid pink;
    position: relative;
  }
</style>
<body>
  <div class="container">
    <div class="box">
      1111
    </div>
  </div>
</body>
```
>此时box的宽度就是文字的宽度，表现为包裹性。

``` html
<style>
  .box {
    position: absolute;
  }

  .container {
    width: 200px;
    border: 1px solid pink;
    position: relative;
  }
</style>
<body>
  <div class="container">
    <div class="box">
      111111111111111111111111111111111111111111111111111111111111111111111111
    </div>
    <div class="box">
      我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字我是文字
    </div>
  </div>
</body>
```

> 此时第一个box不换行，第二个box宽度是container的宽度，是因为被container包含块限制了宽度。因此决定定位的默认最大宽度是“包含块”的宽度，表现为自适应性。需要注意container高度塌陷是因为absolute元素脱离了正常流。此时如果给box元素设置一个max-width:100%是多余的，因为absolute元素默认的最大宽度就是包含块的宽度。需要注意的是有的时候纯表现为包裹性会有换行效果，如果要不换行可以设置white-space:nowrap。

最后需要注意的是计算和定位是相对于祖先定位元素的padding box，这和overflow隐藏的是padding box边界类似，是使用场景决定的。


``` html
<style>
  .container {
    width: 200px;
    height: 200px;
    padding: 50px;
    background-color: pink;
    position: relative;
  }
  .box {
    position: absolute;
    top: 0;
    right: 0;
  }
</style>
<body>
  <div class="container">
    我是内容
    <div class="box">
      小图标
    </div>
  </div>
</body>
```
> 此时box在container的padding区域内。而不是定位在container的content box区域的右上角。

如果想要定位在内容区域的右上角，此时不应该使用padding，因为使用padding需要对top和left设置padding值

``` html
<style>
  .container {
    width: 200px;
    height: 200px;
    padding: 50px;
    background-color: pink;
    position: relative;
  }
  .box {
    position: absolute;
    top: 50px;
    right: 50px;
  }
</style>
<body>
  <div class="container">
    我是内容
    <div class="box">
      小图标
    </div>
  </div>
</body>
```

> 此时如果padding值变化了，那么top和right值也需要跟着变化，从而使得维护性下降。

此时可以使用border属性模拟padding从而达到同样的效果

``` html
<style>
  .container {
    width: 200px;
    height: 200px;
    border: 50px solid transparent;
    background-color: pink;
    position: relative;
  }
  .box {
    position: absolute;
    top: 0;
    right: 0;
  }
</style>
<body>
  <div class="container">
    我是内容
    <div class="box">
      小图标
    </div>
  </div>
</body>
```


#### 具有相对特性的无依赖absolute绝对定位

一个绝对定位元素没有任何left/top/right/bottom属性设置，并且其祖先元素全部都是非定位元素，其位置还是再当前位置。

absolute是非常独立的css属性值，其样式和行为表现不依赖其他任何CSS属性就可以完成。

``` html
<style>
  .container {
    width: 200px;
    height: 200px;
    border: 50px solid transparent;
    background-color: pink;
  }
  .box {
    position: absolute;
  }
</style>
<body>
  <div class="container">
    <div class="box">
      小图标
    </div>
    我是内容
  </div>
</body>
```
> 此时小图标和我是内容重叠在了一起。事实上小图标的位置仍然在原来的位置，不需要父元素设置relative，并且box元素也没有设置left/top/right/bottom属性值的绝对定位称为“无依赖绝对定位”。“无依赖绝对定位”本质上就是“相对定位”，仅仅是不占据CSS流的尺寸空间而已。



``` html
<style>
  .nav {
    display: table;
    table-layout: fixed;
    width: 100%;
    max-width: 600px;
    margin: 1em auto;
    background-color: pink;
    text-align: center;
  }
  .nav-list {
    display: table-cell;
    font-weight: 400;
  }
  .nav-a {
    display: block;
    line-height: 20px;
    padding: 20px;
    color: white;
    text-decoration: none;
  }
  .nav .nav-list a i {
    position: absolute;
    margin: -6px 0 0 6px;
  }

</style>
<body>
  <div class="nav">
    <h4 class="nav-list">
      <a class="nav-a">普通导航<i class="fa fa-check-square-o"></i></a>
    </h4>
    <h4 class="nav-list">
      <a class="nav-a">热门导航<i class="fa fa-bell-alt"></i></a>
    </h4>
    <h4 class="nav-list">
      <a class="nav-a">新导航<i class="fa fa-step-backward"></i></a>
    </h4>
  </div>
</body>
```
> 此时的i元素就是在原来位置不变的基础上进行了margin偏移这种定位的方法兼容性良好，如果i标签需要下架处理，只需要删除对应的HTML和CSS即可，日后维护也很方便。更关键的是，如果导航中的文字发生变化，图标依然定位良好，“无依赖绝对定位”的图标是自动跟随文字后面显示的。此时如果需要给父元素设置positon:relative然后使用right/top定位，文字长度一旦发生变化，则CSS代码很有可能需要重新调整。


需要注意的是普通的水平对齐图标也可以使用“无依赖绝对定位”


``` html
<style> 
  .alert {
    line-height: 20px;
    padding-left: 20px;
  }

  .alert i {
    position: absolute;
    margin-left: -20px;
    width: 20px;
    height: 20px;
  }
</style>
<body>
  <div class="alert">
    <i class="fa fa-warning"></i> 邮件格式请正确填写！
  </div>  
</body>
```
> 使用“无依赖绝对定位”然后采用简单的margin偏移实现，兼容性良好，与inline-block对齐相比的好处在于，inline-block对齐最终的行框高度并不是20px，而是21px或者更大，因为中文下沉，图标居中，要想视觉上水平，图标vertical-align对齐要比实际低一点，会导致整个行框的高度不是预期的20px，而是比20px更大。使用“无依赖绝对定位”实现，则完全不需要担心这个问题，因为绝对定位不会改变正常流尺寸空间，就算图标变成了30px大小，行框高度依然是纯文本所在的20px高度。



#### absolute与text-align

只有原本是内联水平的元素绝对定位后可以受到text-align属性影响。


``` html
<style> 
 p {
   text-align: center;
 }
 img {
   position: absolute;
   width: 100px;
 }
</style>
<body>
  <div class="alert">
    <p>
      <img class="left" src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    </p>
  </div>  
</body>
```
> 由于absolute元素的display值是块状的，因此text-align是不会起作用，之所以这里受到了text-align的影响，是因为受到了“幽灵空白节点”和“无依赖绝对定位”共同作用。由于img是内联水平，p元素中存在“幽灵空白节点”，于是受到了text-align:center影响而水平居中显示。而img设置了position:absolute后，表现为“无依赖绝对定位”，因此在空白节点之后定位显示。


### absolute与overflow

如果overflow不是定位元素，同时绝对定位元素和overflow容器之间也没有定位元素， 则overflow无法对absolute元素进行裁剪。

``` html
<style> 
 .overflow {
   height: 100px;
   overflow: hidden;
 }

 img {
   height: 200px;
   position: absolute;
 }
</style>
<body>
  <div class="overflow">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>  
</body>
```

> div元素无法对img进行裁剪。个人理解是div是正常文档流，而img脱离了文档流，所以不能被裁剪。

``` html
<style> 
 .relative {
   position: relative;
 } 
 .overflow {
   height: 100px;
   overflow: hidden;
 }

 img {
   height: 200px;
   position: absolute;
 }
</style>
<body>
  <div class="relative">
    <div class="overflow">
      <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    </div>
  </div>  
</body>
```
> div元素无法对img元素进行裁剪。个人理解是div是正常文档流，而img脱离了文档流，所以不能被裁剪。


``` html
<style> 
 .overflow {
   height: 100px;
   overflow: hidden;
   position: relative;
 }

 img {
   height: 200px;
   position: absolute;
 }
</style>
<body>
  <div class="overflow">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

> 此时可以被裁剪。

overflow元素和绝对定位元素之间有定位元素，也会被裁剪(这里感觉relative仍然是正常文档流，受到了overflow的影响，又限制了absolulte元素, 如果把.relative元素换成absolute元素，仍然不会被裁剪，因为脱离了正常文档流)。


``` html
<style> 
 .overflow {
   height: 100px;
   overflow: hidden;
 }

 .relative {
   position: relative;
 }

 img {
   height: 200px;
   position: absolute;
 }
</style>
<body>
  <div class="overflow">
    <div class="relative">
      <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
    </div>
  </div>
</body>
```
> 此时也可以被裁剪。

如果overflow的属性值不是hidden而是auto或者scroll，即使是绝对定位元素高度比overflow元素高度大，也不会出现滚动条，这里的个人理解还是因为脱离了文档流。


``` html
<style> 
 .overflow {
   height: 100px;
   overflow: auto;
 }

 img {
   height: 200px;
   position: absolute;
 }
</style>
<body>
  <div class="overflow">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```
> 此时仍然是因为img脱离了正常文档流，所以overflow无法限制。


### absolute与clip

裁剪属性clip要起作用，那么元素必须是绝对定位或者固定定位元素。

#### clip属性

1. fixed固定定位的裁剪

对于普通元素或者绝对定位元素，想要对其进行裁剪，可以使用overflow属性，但是对于position:fixed元素，overflow往往就力不能及。

2. 最佳可访问性隐藏

例如优化SEO以及无障碍识别

``` html
<style> 
 .logo h1 {
   position: absolute;
   clip: rect(0 0 0 0);
 }
</style>
<body>
  <a href="/" class="logo">
    <h1>ziyi2</h1>
  </a>
</body>
```
> 一种方法是使用display:none或者hidden，屏幕阅读器会忽略这里的文字，这显然不是最好的方法。text-indent缩进，但是如果缩进到屏幕阅读器之外，也是不会读取的。color:transparent是移动端的上策，桌面端有IE8浏览器兼容性问题。clip裁剪是隐藏的上策，既满足视觉隐藏，屏幕阅读器设备也能支持。


``` html
<style> 
 .clip {
   position: absolute;
   clip: rect(0 0 0 0);
 }
</style>
<body>
  <form>
    <input type="submit" id="ziyi2" class="clip">
    <label for="ziyi2">提交</label>
  </form>
</body>
```

> 为了解决input在IE7下的样式不统一问题，可以使用label代替input的行为。label没有内置UI，兼容性良好。

- display:none或者visibility:hidden隐藏两个问题，一个是按钮无法被focus，另外一个是IE8下提交行为丢失。
- 透明度0在移动端推荐，但是在桌面端成本较高。
- 还有一种具有适用性的“可访问隐藏”，屏幕外隐藏

``` html
<style> 
 .abs-out {
   position: absolute;
   left: -999px;
   top: -999px;
 }
</style>
```
> 此种方法会有一个问题，就是当一个控件元素被focus的时候，浏览器会自动改变滚动高度，让控件元素在屏幕内显示，如果label提交按钮在第二屏，点击按钮的时候浏览器会自动跳到第一屏置顶，因为按钮隐藏在了屏幕外，而clip就地裁剪，不会又页面跳动的问题。

#### 深入理解clip

``` html
<style> 
 .overflow {
   width: 117px;
   height: 50px;
   position: relative;
   overflow: auto;
 }

 .overflow > img {
   width: 100px;
   height: 100px;
   position: absolute;
 }
</style>
<body>
  <div class="overflow">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

> 此时产生滚动条。

``` html
<style> 
 .overflow {
   width: 117px;
   height: 50px;
   position: relative;
   overflow: auto;
 }

 .overflow > img {
   width: 100px;
   height: 100px;
   position: absolute;
 }
</style>
<body>
  <div class="overflow">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```
> 此时滚动条并没有消失，但是图片已经视觉不可见。在chrome下，clip仅仅决定了哪一部分不可见，对于原来占据的空间并无影响。但是在ie和firefox下是没有滚动条的，这是一种未定义行为。但是下面这些特性是所有浏览器基本上都一致的：使用clip进行裁剪的元素其clientWidth和clientHeight包括样式计算的宽高都还是原来的大小。

clip仅仅决定了哪部分可见，非可见部分无法响应点击事件。


### absolute的流体特性

#### left/top/right/bottom属性

当absolute遇到left/top/right/bottom属性的时候，absolute元素才真正变成绝对定位元素。如果设置了left/top或者right/bottom属性，则其相对特性丢失，如果仅仅设置left/right或者top/bottom，则垂直方向或者水平方向仍然保持相对特性。

#### absolute的流体特性

“对立方向同时发生定位的时候”（其中left和right、top和bottom属于对立方向），绝对定位元素具有流体特性。

``` html
<style> 
 .absolute {
   position: absolute;
   left: 0;
   background-color: pink;
 }
</style>
<body>
  <div class="absolute">
  </div>
</body>
```

> 此时只设置了left属性值，因此绝对定位元素表现为包裹性，宽度是0。


``` html
<style> 
 .absolute {
   position: absolute;
   left: 0;
   right: 0;
   background-color: pink;
 }
</style>
<body>
  <div class="absolute">
  </div>
</body>
```

> 此时表现为水平流体特性，宽度表现为“格式化宽度”，宽度大小自适应于.absolute包含块的padding-box。如果padding-box宽度发生变化，则.absolute的宽度也会跟着变化。


``` html
<style> 
 .absolute {
   position: absolute;
   left: 0;
   right: 0;
   top: 0;
   bottom: 0;  
   background-color: pink;
 }
</style>
<body>
  <div class="absolute">
  </div>
</body>
```

> 此时水平和垂直方向都表现为流体特性。需要注意此时不应该给.absolute元素设置宽度100%，这样会丢失流体特性。垂直方向保持了流动性之后，子元素的高度百分比值可以生效，所以高度自适应、高度等比例布局可以从容实现。

``` html
<style> 
 .absolute {
   position: absolute;
   left: 0;
   right: 0;
   top: 0;
   bottom: 0;  
   width: 100%;
   padding: 100px;
   background-color: pink;
 }
</style>
<body>
  <div class="absolute">
  </div>
</body>
```

> 此时会出现水平滚动条，失去了流体特性。

#### sbsoulte的margin:auto居中

当absolute绝对定位元素处于流体状态的时候，盒模型相关属性的解析和普通流体元素一模一样。

此时使用margin:auto不仅可以让元素水平居中，还可以让元素垂直居中（如果绝对定位元素在水平和垂直方向都保持了流体特性）。

``` html
<style> 
 .absolute {
   position: absolute;
   left: 0;
   right: 0;
   top: 0;
   bottom: 0;  
   width: 100px;
   height: 100px;
   margin: auto;
   background-color: pink;
 }
</style>
<body>
  <div class="absolute">
  </div>
</body>
```
### position:relative

#### relative对absolute的限制

``` html
<style> 
 .absolute {
   position: absolute;
   left: 0;
   right: 0;
   top: 0;
   bottom: 0;  
   margin: auto;
   background-color: pink;
 }

 .relative {
   width: 20px;
   height: 20px;
   position: relative;
 }
</style>
<body>
  <div class="relative">
    <div class="absolute">
    </div>
  </div>
</body>
```

> 此时absolute元素的包含块是relative元素。

#### relative与定位

relative定位是相对于自身的原始位置进行定位，并且定位后原始位置的空间保留。相对定位元素同时应用对立方向定位时，由于默认的文档流是自上而下、从左往右，因此top/bottom同时使用时，bottom被忽视，left/right同时使用时，right被忽视。


#### relative的最小化影响原则

- 尽量不使用realtive，如果想定位某些元素，观察是否可使用“无依赖绝对定位”
- 如果场景受限，一定要使用relative，则该relative务必最小化

相对于普通元素，定位元素的层叠顺序更高。


### position:fixed

fixed固定定位元素的“包含块”是根元素，近似可看成html元素。因此relative和absolute元素对fixed元素没有任何的限制作用。

``` html
<style> 
 .fixed {
   position: fixed;
   top: 0;
   right: 0;
   width: 50px;
   height: 50px;
   background-color: pink;
 }

 .relative {
   width: 20px;
   height: 20px;
   position: relative;
 }
</style>
<body>
  <div class="relative">
    <div class="fixed">
    </div>
  </div>
</body>
```

> 此时fixed元素并不会被relative元素限制。

#### position:fixed与背景锁定

蒙层弹窗是网页中常见的交互，黑色半透明全屏幕覆盖的蒙层基本上是使用position:fixed定位实现。但是无法覆盖右侧的滚动栏。鼠标滚动的时候后面的背景仍然可以被滚动。可以滚动的元素是根元素，正好是fixed元素的包含块。如果希望背景被锁定，可以使用absolute模拟fixed定位，让页面滚动条由内部的普通元素产生。

``` html
<style> 
 html,body {
   height: 100%;
   overflow: hidden;
 }
 .page {
   height: 100%;
   overflow: auto;
 }   
 .fixed {
   position: absolute;
   top: 0;
   right: 0;
   left: 0;
   bottom: 0;
   background-color: darkgray;
 }
</style>
<body>
  <div class="fixed">固定定位元素</div>
  <div class="page">
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
  </div>
</body>
```
> 根元素直接overflow:hidden；滚动条就会消失，滚动条消失会导致页面可用宽度变化，会产生晃动问题，此时只要使用同等宽度透明的border填充消失的滚动条即可。
### position:relative

#### relative对absolute的限制

``` html
<style> 
 .absolute {
   position: absolute;
   left: 0;
   right: 0;
   top: 0;
   bottom: 0;  
   margin: auto;
   background-color: pink;
 }

 .relative {
   width: 20px;
   height: 20px;
   position: relative;
 }
</style>
<body>
  <div class="relative">
    <div class="absolute">
    </div>
  </div>
</body>
```

> 此时absolute元素的包含块是relative元素。

#### relative与定位

relative定位是相对于自身的原始位置进行定位，并且定位后原始位置的空间保留。相对定位元素同时应用对立方向定位时，由于默认的文档流是自上而下、从左往右，因此top/bottom同时使用时，bottom被忽视，left/right同时使用时，right被忽视。


#### relative的最小化影响原则

- 尽量不使用realtive，如果想定位某些元素，观察是否可使用“无依赖绝对定位”
- 如果场景受限，一定要使用relative，则该relative务必最小化

相对于普通元素，定位元素的层叠顺序更高。


### position:fixed

fixed固定定位元素的“包含块”是根元素，近似可看成html元素。因此relative和absolute元素对fixed元素没有任何的限制作用。

``` html
<style> 
 .fixed {
   position: fixed;
   top: 0;
   right: 0;
   width: 50px;
   height: 50px;
   background-color: pink;
 }

 .relative {
   width: 20px;
   height: 20px;
   position: relative;
 }
</style>
<body>
  <div class="relative">
    <div class="fixed">
    </div>
  </div>
</body>
```

> 此时fixed元素并不会被relative元素限制。

#### position:fixed与背景锁定

蒙层弹窗是网页中常见的交互，黑色半透明全屏幕覆盖的蒙层基本上是使用position:fixed定位实现。但是无法覆盖右侧的滚动栏。鼠标滚动的时候后面的背景仍然可以被滚动。可以滚动的元素是根元素，正好是fixed元素的包含块。如果希望背景被锁定，可以使用absolute模拟fixed定位，让页面滚动条由内部的普通元素产生。

``` html
<style> 
 html,body {
   height: 100%;
   overflow: hidden;
 }
 .page {
   height: 100%;
   overflow: auto;
 }   
 .fixed {
   position: absolute;
   top: 0;
   right: 0;
   left: 0;
   bottom: 0;
   background-color: darkgray;
 }
</style>
<body>
  <div class="fixed">固定定位元素</div>
  <div class="page">
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
    <br><br><br><br><br><br>
  </div>
</body>
```
> 根元素直接overflow:hidden；滚动条就会消失，滚动条消失会导致页面可用宽度变化，会产生晃动问题，此时只要使用同等宽度透明的border填充消失的滚动条即可。


## CSS层叠规则

### z-index

z-index属性只有和定位元素(position不为static的元素)在一起的时候才起作用，可以是正数也可以是负数。需要注意在css3中flex盒子的子元素也可以设置z-index属性。

### 理解CSS的层叠上下文和层叠水平

#### 什么是层叠上下文

层叠上下文类似于“块状格式化上下文”，理解为一种“层叠结界”，在“层叠结界”中可能存在其他的“层叠结界”，也可能自身也处于其他的“层叠结界”中。

#### 什么是层叠水平

层叠水平决定了在同一个层叠上下文中元素在z轴上的显示顺序。需要注意层叠水平不仅仅指z-index，z-index只能影响定位元素和flex盒子的子元素，而层叠水平在所有的元素中都存在。

### 理解元素的层叠顺序

需要注意层叠上下文和层叠水平是概念，而层叠顺序是规则。

在CSS2.1的世界里，层叠顺序的规则使

- 1. 层叠上下文的backgroun/border
- 2. 负z-index
- 3. block块状水平盒子
- 4. float浮动盒子
- 5. inline水平盒子
- 6. z-index:auto或者z-index:0
- 7. 正z-index

> 每一个层叠顺序仅适用于当前层叠上下文元素的小世界。内联元素的层叠顺序比浮动元素高是因为浮动元素用于布局，而内联元素用于显示内容，内容当然优先显示。background/border为装饰属性，浮动元素和块状元素一般用作布局，而内联元素都是内容，因为内容的层叠顺序最高（网页中最重要的当然是内容）。

### 层叠准则

- 谁大谁上：在同一个层叠上下文中，层叠水平值大的覆盖小的。
- 后来居上：当元素的层叠水平一致、层叠顺序相同的时候，在DOM流中处于后面的元素会覆盖前面的元素。

### 层叠上下文

#### 层叠上下文的特性

- 层叠上下文的层叠水平比普通元素高
- 层叠上下文可以嵌套，内部层叠上下文及其所有子元素均受制于外部层叠上下文
- 每个层叠上下文和兄弟元素独立，进行层叠变化或渲染的时候，只需要考虑后台元素
- 每个层叠上下文是自成体系的，当元素发生层叠的时候，整个元素被认为是父层叠上下文的层叠顺序中

#### 层叠上下文的创建

和块状格式化上下文一样，层叠上下文也由一些特定的CSS属性创建

- 页面根元素天生具有层叠上下文，称为根层叠上下文
- z-index值为数值的定位元素的传统层叠上下文
- 其他CSS3属性

1. 根层叠上下文

html元素，因此页面中所有元素一定处于至少一个层叠结界中

2. 定位元素与传统层叠上下文


``` html
<style> 
  .first, .last {
    z-index: auto;
    position: relative;
  }

  img {
    width: 100px;
  }

  .first img {
    position: absolute;
    z-index: 2;
  }

  .last img {
    position: absolute;
    top: 10px;
    left: 10px;
    z-index: 1;
  }
 
</style>
<body>
  <div class="first">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
  <div class="last">
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

> 此时z-index:2的图片位于上面，符合预期效果。如果此时将div元素的z-index:auto改为z-index:0会发现后者覆盖了前者，正好相反。需要注意的是z-index:auto并没有创建定位元素的传统层叠上下文，因此div元素是一个普通定位元素，于是两个img元素的层叠比较不受父级影响，按照层叠准则谁大谁上，在同一个根层叠上下文中，谁大谁上。而z-index:0是设置了准确的数值，因此两个div元素各自创建了一个传统层叠上下文，两个上下文自成体系，根据层叠上下文特性的最后一条，两个img元素受制于各自父元素div的层叠顺序，两个div元素的层叠水平一样（z-index都为0），因此根据后来居上的原则，第二个div元素会覆盖第一个div元素，就算第一个div元素里的img的z-index为无穷大，此时第二个div元素的img仍然会覆盖第一个div的img元素。此时img元素的z-index没起作用。


3. CSS3的层叠上下文

- 1）flex布局元素，同时z-index不为auto
- 2）opacity值不为1
- 3）transform值不是none
- 4）mix-blend-mode值不为normal
- 5）filter值不为none
- 6）isolation值不是isolate
- 7）will-change属性值为2）~6）的任意一个（will-change:opacity等）
- 8）-webkit-overflow-scrolling设为touch

#### 层叠上下文与层叠顺序

一旦普通元素具有了层叠上下文，层叠顺序会变高。

- 如果层叠上下文元素不依赖z-index数值，其层叠顺序是z-index:auto，可看成z-index:0级别
- 如果层叠上下文依赖z-index数值，则层叠顺序由z-index值决定

因此可以对层叠顺序图进行修改

- 1. 层叠上下文的backgroun/border
- 2. 负z-index
- 3. block块状水平盒子
- 4. float浮动盒子
- 5. inline水平盒子
- 6. z-index:auto或者z-index:0，这里新增不依赖z-index的层叠上下文
- 7. 正z-index

一旦元素变成定位元素，那么z-index会自动生效，此时默认就是auto，因为定位元素会覆盖inline/block/float元素。需要注意不依赖z-index层叠上下文的元素和z-index:auto的定位元素是在同一个层叠顺序，因此遵循后来居上的原则。

``` html
<style> 
  img {
    width: 100px;
  }
  img:first-child {
    position: relative;
  }
  img:last-child {
    transform: scale(2);
  }
</style>
<body>
  <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
</body>
```

> 此时遵循后来居上的原则。两个img都创建了各自的层叠上下文，而且处于同一个层叠顺序，因此遵循后来居上的原则。


### z-index负值的深入理解


``` html
<style> 
  html {
    background-color: pink;
  }

  div {
    /* background-color: cyan; */
  }
  
  img {
    width: 100px;
  }
  img {
    position: relative;
    z-index: -1;
  }
</style>
<body>
  <div>
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>

```
> 此时因为html是根元素，根元素创建了根层叠上下文，因此根元素的修饰属性background/border默认顺序最高，z-index负值的顺序其次，因此图片在html背景色之上。此时如果给div一个背景色，那么由于div是块状水平元素，顺序在z-index负值之后，因此div的背景色会覆盖图片。


如果此时给div元素创建一个层叠上下文，那么图片又会显示在div元素的背景之上


``` html
<style> 
  html {
    background-color: pink;
  }

  div {
    background-color: cyan;
    transform: scale(1);
  }
  
  img {
    width: 100px;
  }
  img {
    position: relative;
    z-index: -1;
  }
</style>
<body>
  <div>
    <img src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1522831997482&di=b790721e923403adfaf7da42b65ed5be&imgtype=0&src=http%3A%2F%2Fimg.25pp.com%2Fuploadfile%2Fapp%2Ficon%2F20160830%2F1472514571151657.jpg" alt="">
  </div>
</body>
```

> 因此可以发现z-index负值渲染过程就是寻找它所在的层叠上下文元素的过程。

z-index为负值的作用

- 可访问性隐藏：只需要层叠上下文内的某一个父元素加个背景色就可以。与clip隐藏的优势在于无须绝对定位，并对原来的布局和元素行为没有任何影响。劣势当然是需要父元素加背景配合。

- IE8下的多背景模拟
- 定位元素的后面


## z-index不犯二准则

对于非浮层元素，避免设置z-index值，z-index值没有任何道理需要超过2。

需要注意对于javascript驱动的浮层组件，需要“层级计数器”来管理
- 总会遇到意想不到的高层级元素
- 组件的覆盖规则具有动态性


## 文本处理能力

### font-size

#### ex、em和rem

ex: 字符x的高度，font-size越大，ex越大。

em的大小

```html
<style> 
  html {
    font-size: 12px;
  }

  div {
    font-size: 2em; // 24px = 12px * 2
    margin-left: 0.5em; // 24px*0.5
    margin-top: 0.5em; // 24px*0.5
  }

  p {
    font-size: 1.5em; // 36px = 24px * 1.5
    padding: 0.5em; // 18px = 36px * 0.5
  }
</style>
<body>
  <div>
    div
    <p>p</p>
  </div>
</body>

```

> 此时可以看出1em的计算值等于当前元素所在font-size计算值。这里先计算元素的font-size大小，然后再计算给其他使用em单位的属性值大小。使用相对单位的好处是样式表现更具有弹性，从而实现弹性布局。

一旦div元素的font-size调整，继承div元素font-size的计算值就会调整，因此维护成本较高，由于这种限制，rem(root em)就出现了，rem是根元素em大小，em相对于当前元素，而rem相对于根元素，当前元素容易变化，而根元素是固定的

``` html
<style> 
  html {
    font-size: 12px;
  }
  div {
    font-size: 2rem; // 24px = 12px * 2
    margin-left: 0.5rem;  // 6px = 12px * 0.5
    margin-top: 0.5rem; // 6px = 12px * 0.5
  }
  p {
    font-size: 1.5rem; // 18px = 12px * 1.5
    padding: 0.5rem; // 6px = 12px * 0.5
  }
</style>
<body>
  <div>
    div
    <p>p</p>
  </div>
</body>
```
> 此时的1rem就是html的font-size大小。rem是CSS3单位，只有IE9以上才支持。

#### font-size:0与文本的隐藏

font-size:0可以隐藏文本。



