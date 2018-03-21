# css世界
## 流、元素和基本尺寸
### 块级元素

块级元素包括div(display:block)、li(display:list-item)以及table(display:table)等元素，因此可以看出块级元素和display:block的元素并不是同一个概念。块级元素的基本特征是一个水平流上只能单独显示一个元素，多个块级元素则换行显示。

#### 盒子

所有的块级元素都有一个主块级盒子，list-item除此之外还有一个附加盒子，之所以list-item会出现项目符号是因为生成了一个附加盒子，学名“标记盒子”，专门用来放圆点、数字这些项目符号。

需要注意的是每个元素都有两个盒子，**外在盒子**和**内在盒子**，外在盒子负责元素是可以一行显示还是只能换行显示，内在盒子负责宽高一级内容呈现等。内在盒子更专业的叫法是“容器盒子”。因此display:block的元素的盒子实际上由外在“块级盒子”和内在的“块级容器盒子”组成。display:inline的元素则内外均是“内联盒子”，而display:inline-block的元素外面的盒子是inline级的，内在的盒子是block级的。

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

- 4) 超出容器限制。以上3种情况尺寸都不会主动超过父级容器宽度，但是存在特殊情况，例如内容很长的连续的英文和数字，或者内联元素被设置了white-space:norwap(文本不会换行，文本会在在同一行上继续，直到遇到 <br> 标签为止)。

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















