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
- 3) 收缩到最小。在规范中被描述为min-content。

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

##### 外部尺寸与流体特性

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

##### 内部尺寸与流体特性

假如元素里面没有内容，width:auto时宽度是0，那就是应用“内部尺寸”。

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



