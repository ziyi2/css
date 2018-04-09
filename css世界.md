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


