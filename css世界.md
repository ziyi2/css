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


