# 精通Css
## 基础知识
-  除非确定元素是页面上唯一的存在才使用id为元素命名，否则使用通用的class命名。
-  类不应该被滥用，应该视情况而定，除非特殊情况，应该使用**层叠**标识某些元素。如果发现添加了许多类，那么可能意味着HTML文档结构有问题。
- div元素并非没有语义，实际代表着**部分**(division)，可以将文档分割为几个有意义的区域，为了将不必要的标记减到最少，应该只有在没有现有元素能够实现区域分割的情况下使用div元素，例如以下代码是非必要的。

``` html
<div class="nav">
 <ul>
    <li>Home</li>
    <li>Contact</li>
  </ul>
</div>
```

-  DOCTYPE声明用于浏览器解析DTD(文档类型定义）、HTML文档有效性验证以及浏览器模式的设定(标准模式和混杂模式)等。

## 为样式找到应用目标

- :link和:visited称为链接伪类，:hover、:active和:focus称为动态伪类，理论上可以应用于任何元素。可以试试:visited:hover的效果。
- 后代选择器选择一个元素的所有后代，而子选择器值选择元素的直接后代。
- 层叠和特殊性可以参考《css权威指南》。
- 特殊性中需要说明的是如果css规则没有起到作用，那很有可能是出现了特殊性冲突，可以在选择器中添加一个它的父元素id(如果页面中的所有样式都是以class进行设计，那么要增加特殊性其实只要添加id就能做到，当然也并不是滥用id，比id更高的就是内联样式和!important了，但是这里最好也别滥用!important)。  
- 使用@import导入样式和link的链接样式的区别：兼容性问题(链接样式无兼容性问题)，导入样式的浏览器下载时间比链接样式慢等。当然如果要在css样式表中引入外部的样式表，就需要使用@import了。

### 设计css代码的结构

 一般性样式
 - 主体样式（body标记的应该由站点上所有元素继承的样式）
 - reset样式
 - 链接
 - 标题
 - 其他元素
 
辅助样式
 - 表单
 - 通知和错误
 - 一致的条目
 
页面结构
 - 标题、页脚和导航
 - 布局
 - 其他页面结构元素
 
页面组件 
 - 各个页面
 
覆盖

> 注：其实可见这样的代码结构从上至下处理的样式越来越特殊，这样有助于维护样式表，而防止非预期的特殊性覆盖问题。

建立统一风格的注释则有助于快速寻找特性样式以及规划css代码

``` css
/* @group general styles
---------------------------------------------*/

/* @group helper styles
---------------------------------------------*/

/* @group page structure
---------------------------------------------*/

/* @group page components
---------------------------------------------*/

/* @group overrides
---------------------------------------------*/
```

除了@group这种结构注释，也可以增加自我提示注释，例如

``` css
/* @colordef #434343; dark gray */
/* @todo 以后需要进行修改、修复或复查 */
/* @bugfix 特定浏览器遇到的问题等 */
/* @workaround 并不完善的权宜之计 */
```

> 注：这些关键字称为gotcha，都是[CSSDoc项目](http://cssdoc.net) 的一部分，目的是为样式表开发一套标准的注释语法。  

## 可视化格式模型

### 盒模型

盒模型：页面上的每个元素都可以看做是一个矩形框，这个框由元素的内容、内边距、边框和外边距组成。
- 内边距：出现在内容区域的周围，如果在元素上增加背景，那么背景会应用于元素的内容和内边距。通过增加内边距可以在内容周围创建隔离带，内容不会和背景混在一起。
- 边框：内边距的区域外形成的一条线
- 外边距：主要用于**控制元素之间的间隔**。

> 注：CSS2.1还包括outline(轮廓)属性，与border属性不同的是轮廓绘制在元素框之上，不影响元素的大小或定位，轮廓有助于修复bug?

```
// 注意这样覆盖浏览器样式是有副作用的，例如对option元素有不利影响，因此可以使用全局reset显示设置样式
* {
	margin: 0;
	padding: 0;
}
```

#### IE和盒模型

IE早期版本包括IE6在**混杂模式**使用的是非标准盒模型，在混杂模式中width不仅仅是元素内容的宽度，而是内容、内边距和边框的宽度总和。

> 注：除了使用CSS3的box-sizing属性目前最好的方法是不要给元素添加具有指定宽度的内边距，而是尝试将内边距或外边距添加到元素的父元素或子元素。

#### 外边距叠加

外边距叠加是指当两个或多个**垂直外边距**相遇时，只会形成一个外边距。这个外边距的高度等于这几个外边距中高度最大者。

- 当一个元素包含在另一个元素中时(**假设子元素没有内边距或边框将外边距分隔开**)，父子元素之间的顶或底外边距也会发生叠加。
- 外边距甚至可以与本身发生叠加，假设一个空元素**没有边框和内边距**，这种情况下顶外边距和底外边距碰在一起就会发生叠加，取其中的较大者。
- 只有**普通文档流**中的块框的**垂直外边距**才会发生外边距叠加 ，**行内框、浮动框或绝对定位框**之间的外边距不会叠加。

### 定位

CSS中有3种基本的定位机制：普通流、浮动和绝对定位。

行内框在一行中水平排列，可以使用水平内边距、边框和外边距调整它们之间的水平间距，但是垂直内边距、边框和外边距不影响行内框的高度。在行内框上设置显式的高度和宽度是没有影响的。修改行内框尺寸的唯一方法是修改行高、水平边框、内边距或外边距。

#### 相对定位

元素仍然占据原来的空间。相对定位实际上被看做普通流定位的一部分，因为元素的位置是相对于它在普通流中的位置。


#### 绝对定位

绝对定位使元素的位置与文档流无关，因此不占据空间，普通文档流中其他元素的布局就像绝对定位的元素不存在时一样。绝对定位的元素的位置相对于距离它最近的那个已定位的祖先元素进行定位，如果没有已定位的祖先元素，那么相对于初始包含块定位，初始包含块是HTML元素或者画布。

固定定位是绝对定位的一种，相对于绝对定位的差异在于固定定位的包含块是视口。

#### 浮动

浮动框和绝对定位一样脱离了普通文档流。创建浮动元素可以使文本围绕该浮动元素，如果想要阻止行框围绕浮动框的外边，需要对浮动元素应用clear属性，**在清理元素时，浏览器在元素的顶上添加足够的外边距，使元素的顶边缘垂直下降到浮动框的下面**。尽管浮动元素脱离了文档流，不影响周围的元素，但是对元素进行清理实际上为前面的浮动元素留出了垂直空间。

```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<style> 
.float-container {
	background-color: gray;
	border: 1px solid black;
}
	
.float-left {
	margin: 16px;
	float: left;
	background-color: blue;
	
}
	
.float-right {
	margin: 16px;
	float: left;
	background-color: red;
}
	
</style>
</head>
<body>
	<div class="float-container">
		<div class="float-left">float left</div>
		<div class="float-right">float right</div>
	</div>
</body>
</html>
```

> 注：此时class为float-container的容器不包含浮动元素，因为浮动元素脱离了正常文档流。所以两个浮动元素不占据空间。

如果要使容器在视觉上包含浮动元素，那么需要在这个元素的某个地方应用clear属性。此时class为float-clear的元素在正常的文档流中，但是使用了清除属性后，**在该元素的顶上添加足够的外边距，使元素的顶边缘垂直下降到浮动框的下面**，因此容器就可以在视觉上包含浮动元素了。

```
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8"> 
<style> 
.float-container {
	background-color: gray;
	border: 1px solid black;
}
	
.float-left {
	margin: 16px;
	float: left;
	background-color: blue;
	
}
	
.float-right {
	margin: 16px;
	float: left;
	background-color: red;
}

.float-clear {
	clear: both;
}	
	
</style>
</head>
<body>
	<div class="float-container">
		<div class="float-left">float left</div>
		<div class="float-right">float right</div>
		<div class="float-clear"></div>
	</div>
</body>
</html>
```

> 注：关于更多清除浮动的方法可以查看[清除和去除浮动的方法详解](https://ziyi2.github.io/2017/08/02/%E6%B8%85%E9%99%A4%E5%92%8C%E5%8E%BB%E9%99%A4%E6%B5%AE%E5%8A%A8%E7%9A%84%E6%96%B9%E6%B3%95%E8%AF%A6%E8%A7%A3.html#more)。

### 布局

CSS布局技术的根本就是3个基本概念：定位、浮动和外边距操纵。

#### 计划布局

把页面划分为大的结构性区域，比如容器、页眉、内容区域和页脚。最后在内容区域中寻找不同的布局结构，例如把某些信息分为两列、三列或四列。

#### 设置基本结构

根据计划布局，标记如下

``` htmlbars
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <div class="wrapper">
    <div class="header">
      <!-- page header content -->
    </div>
    <div class="content">
      <!-- page content-->
    </div>
    <div class="footer">
      <!-- page footer -->
    </div>
  </div>
</body>
</html>
```

> 注意这种写法并不是HTML5语义化的写法。

##### 使用外边距让设计居中


``` css
 .wrapper {
   width: 920px;
   margin: 0 auto;
 }
```

混杂模式的IE5.x和IE6不支持margin:auto声明，需要做hack处理，这种处理是无害的，对代码没有严重影响

``` css
body {
  text-align: center;
}
.wrapper {
  width: 920px;
  margin: 0 auto;
  text-align: left;
}
```

### 基于浮动的布局

 
浮动元素不占据文档流的任何空间，不再对包围它的块框(普通文档流)产生任何影响，因此需要对布局中各个点上的浮动元素进行清理，当然并不是连续的浮动和清理元素，而是浮动几乎所有的东西，然后让整个文档的某个结构(比如页脚)上进行一次或两次清理，也可以使用溢出方法(overflow)清理某些元素的内容。

#### 两列浮动布局

两列布局主要包含主内容和次要内容，其中次要内容包括站点导航等，将位于左侧。为了提高易用性和可访问性，将主内容放在次要内容的前面。为了防止右侧浮动元素被退到下面(IE中3像素文本偏移BUG和双外边距浮动BUG)，需要避免浮动布局在包含它的元素中太挤，可以不使用**水平外边距或内边距**建立隔离带，而是让一个元素向左浮动，另一个元素向右浮动，从而创建视觉上的隔离。如果一个元素的尺寸意外的增加了几个px，也不会立刻占满水平空间并下降，而只是扩展到了视觉隔离带中。


``` htmlbars
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    body {
      text-align: center;
    }
    .wrapper {
      width: 920px;
      margin: 0 auto;
      text-align: left;
      border: 1px solid #ccc;
    }
    .content {
      overflow: hidden; /* 清浮动(这种清浮动的方式要考虑使用场景) */
    }
    .content .primary {
      width: 650px;
      height: 200px;
      float: right; 
      display: inline; /* 防止IE双外边距浮动bug */
      background: #ccc;
    }
    .content .secondary {
      width: 230px;
      height: 200px;
      float: left;
      display: inline; /* 防止IE双外边距浮动bug */
      background: #ccc;
    }
  </style>
</head>
<body>
  <div class="wrapper">
    <div class="header">
      <!-- page header content -->
    </div>
    <!-- 两列浮动布局 -->
    <div class="content">
      <div class="primary">
        <!-- page main content -->
      </div>
      <div class="secondary">
        <!-- page nav or secondary content -->
      </div>
    </div>
    <div class="footer">
      <!-- page footer -->
    </div>
  </div>
</body>
</html>
```

> 左右浮动元素之间产生了40px的隔离带，这个隔离带可以防止内容扩展导致浮动元素下降的问题。

#### 三列浮动布局

三列浮动布局其实和两列浮动布局类似，可以在两列布局的主内容中进行再分列。

``` htmlbars
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    body {
      text-align: center;
    }
    .wrapper {
      width: 920px;
      margin: 0 auto;
      text-align: left;
      border: 1px solid #ccc;
    }
    .content {
      overflow: hidden; /* 清浮动 */
    }
    .content .primary {
      width: 650px;
      height: 200px;
      float: right; /* 防止IE双外边距浮动bug */
      display: inline;
    }
    .content .secondary {
      width: 250px;
      height: 200px;
      float: left;
      display: inline; /* 防止IE双外边距浮动bug */
      background: #ccc;
    }

    .content .primary .primary {
      width: 400px;
      height: 100%;
      float: left;
      display: inline;
      background: #ccc;
    }

    .content .primary .secondary {
      width: 230px;
      height: 100%;
      float: right;
      display: inline;
      background: #ccc;
    }
  </style>
</head>
<body>
  <div class="wrapper">
    <div class="header">
      <!-- page header content -->
    </div>
    <!-- 三列浮动布局 -->
    <div class="content">
      <div class="primary">
        <div class="primary">
          <!-- page primary primary content -->
        </div>
        <div class="secondary">
          <!-- page primary secondary content -->
        </div>
      </div>
      <div class="secondary">
        <!-- page nav or secondary content -->
      </div>
    </div>
    <div class="footer">
      <!-- page footer -->
    </div>
  </div>
</body>
</html>
```


### 固定宽度、流式和弹性布局

之前的浮动布局都是固定宽度的布局，固定宽度布局的缺点是无法充分利用可用空间(在高分辨率的屏幕上设计会缩小并出现在屏幕中间，在低分辨率屏幕上可能导致水平滚动)。固定宽度的布局往往适用于浏览器默认文本字号，但是只要将文本字号增加几级，可能就会导致文本挤满块框并且行长太短，阅读体验差。

为了解决固定宽度带来的问题，可以采用流式布局或弹性布局替代固定宽度的布局。

#### 流式布局

流式布局的元素尺寸采用百分数而不是像素设置的，但是如果窗口宽度较小，行变得非常窄，也会变得很难阅读。因此又要添加以像素或em为单位的min-width，从而防止布局太窄。

```htmlbars
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    body {
      text-align: center;
    }
    .wrapper {
      width: 76.8%; /* 设置成比例宽度 */
      margin: 0 auto;
      text-align: left;
      border: 1px solid #ccc;

      max-width: 125em; /* 防止容器过大 */
      min-width: 62em; /* 防止容器过小 */
    }
    .content {
      overflow: hidden; /* 清浮动 */
    }
    .content .primary {
      width: 72.82%; /* 设置成比例宽度 */
      height: 200px;
      float: right; /* 防止IE双外边距浮动bug */
      display: inline;
    }
    .content .secondary {
      width: 25%; /* 设置成比例宽度 */
      height: 200px;
      float: left;
      display: inline; /* 防止IE双外边距浮动bug */
      background: #ccc;
    }

    .content .primary .primary {
      width: 59.7%;
      height: 100%;
      float: left;
      display: inline;
      background: #ccc;
    }

    .content .primary .secondary {
      width: 34.33%;
      height: 100%;
      float: right;
      display: inline;
      background: #ccc;
    }
  </style>
</head>
<body>
  <div class="wrapper">
    <div class="header">
      <!-- page header content -->
    </div>
    <!-- 两列浮动布局 -->
    <div class="content">
      <div class="primary">
        <div class="primary">
          <!-- page primary primary content -->
        </div>
        <div class="secondary">
          <!-- page primary secondary content -->
        </div>
      </div>
      <div class="secondary">
        <!-- page nav or secondary content -->
      </div>
    </div>
    <div class="footer">
      <!-- page footer -->
    </div>
  </div>
</body>
</html>
```

#### 弹性布局

虽然流式布局可以充分利用空间，但是在大分辨率显示器上，行会过长让用户不舒服。而在窄窗口中或者增加文本字号时，行会变得非常短，内容很零碎，对于这个问题弹性布局是一种很好的解决方案。

弹性布局相对于字号（而不是浏览器宽度）来设置元素的宽度，以em为为单位设置宽度，可以确保在字号增加时整个布局随之扩大，这可以将行长保持在可阅读的范围。与此同时，在文本字号增加时整个布局会加大，所以弹性布局会变得比浏览器窗口宽，导致水平滚动条出现，为了防止这种问题，需要在容器div上添加100%的max-width。

创建弹性布局的技巧是要设置基字号，让1em大致相当于10px，大多数浏览器上默认的字号时16px，10px是16 * 62.5%。


``` htmlbars
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    body {
      font-size: 62.5%; /* 1em ~ 10px */
      text-align: center;
    }
    .wrapper {
      width: 92em; /* 已em单位设置容器宽度，内部宽度仍然使用百分数，内部宽度仍然相对于字号，可以方便修改布局的总尺寸，不必修改每个元素的宽度 */
      margin: 0 auto;
      text-align: left;
      border: 1px solid #ccc;

      max-width: 95%; /* 防止容器过大 */
    }
    .content {
      overflow: hidden; /* 清浮动 */
    }
    .content .primary {
      width: 72.82%; /* 设置成比例宽度 */
      height: 200px;
      float: right; /* 防止IE双外边距浮动bug */
      display: inline;
    }
    .content .secondary {
      width: 25%; /* 设置成比例宽度 */
      height: 200px;
      float: left;
      display: inline; /* 防止IE双外边距浮动bug */
      background: #ccc;
    }

    .content .primary .primary {
      width: 59.7%;
      height: 100%;
      float: left;
      display: inline;
      background: #ccc;
    }

    .content .primary .secondary {
      width: 34.33%;
      height: 100%;
      float: right;
      display: inline;
      background: #ccc;
    }
  </style>
</head>
<body>
  <div class="wrapper">
    <div class="header">
      <!-- page header content -->
    </div>
    <!-- 两列浮动布局 -->
    <div class="content">
      <div class="primary">
        <div class="primary">
          page primary primary content
        </div>
        <div class="secondary">
          page primary secondary content
        </div>
      </div>
      <div class="secondary">
        page nav or secondary content
      </div>
    </div>
    <div class="footer">
      <!-- page footer -->
    </div>
  </div>
</body>
</html>


```

#### 流式和弹性图像

对于常规内容图像，如果希望垂直和水平伸缩以避免被剪切，可以将图像元素添加到没有设置任何尺寸的页面上，设置图像的百分数宽度，添加与图像宽度相同的max-with以避免像素失真。

``` css

img {
  width: 25%;
  max-width: 200px; /* 图片实际大小，防止失真 */
  float: left;
  display: inline;
}

p {
  width: 68%;
  float: right;
  display: inline;
}
```



### 高度相等列

``` css
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    body {
      font-size: 62.5%; /* 1em ~ 10px */
      text-align: center;
    }
    .wrapper {
      font-size: 14px;
      width: 92em; /* 已em单位设置容器宽度，内部宽度仍然使用百分数，内部宽度仍然相对于字号，可以方便修改布局的总尺寸，不必修改每个元素的宽度 */
      margin: 0 auto;
      text-align: left;
      border: 1px solid #ccc;
      max-width: 95%; /* 防止容器过大 */
      overflow: hidden; /* 清浮动 */
    }
    .box {
      width: 25%;
      margin: 3%;
      float: left;
      display: inline;
      padding: 1em;
      background: #ccc; 
      padding-bottom: 100px;  /* 设置足够大的内边距，使元素溢出容器元素, 容器的overflow不仅可以清浮动，还可以将列进行裁切，实现三列登高 */
      margin-bottom: -100px; /* 利用父外边距消除内边距带来的高度 */
    }
  </style>
</head>
<body>
  <div class="wrapper">
    <div class="box">
      <h1>Ply</h1>
      <p>
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word.
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word.
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word.
      </p>
    </div>
    <div class="box">
      <h1>Zx</h1>
      <p>
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word. 
        This is Ply, hello word.
        This is Ply, hello word. 
      </p>
    </div>
    <div class="box">
      <h1>Zxk</h1>
      <p>
          This is Ply, hello word. 
          This is Ply, hello word. 
          This is Ply, hello word. 
          This is Ply, hello word. 
          This is Ply, hello word. 
          This is Ply, hello word.
          This is Ply, hello word. 
          This is Ply, hello word. 
          This is Ply, hello word. 
          This is Ply, hello word. 
          This is Ply, hello word. 
          This is Ply, hello word. 
          This is Ply, hello word.
          This is Ply, hello word.
        </p>
    </div> 
  </div>
</body>
</html>
```