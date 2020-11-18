# CSS预处理器学习Less&Sass

## 1、安装环境搭建

### less

```
    首先设置npm源：npm config set registry https://registry.npm.taobao.org
    查看源：npm get registry
    使用nodejs局部安装： npm install less -D
```

看下面一段less简单嵌套的源码：

``` less
    body{
        margin: 0;
        padding: o;
    }

    .contain {
        background: #fff;
        .nav{
            font-size: 18px;
            .content {
                background-color: bisque;
                &:hover{
                    color: yellow;
                }
            }
        }
    }
```

保存为一个hello.less 然后调用  ** .\node_modules\.bin\lessc.cmd .\hellow.less > hellow.css **
查看hellow.css的内容：

```css
    body {
    margin: 0;
    padding: o;
    }
    .contain {
    background: #fff;
    }
    .contain .nav {
    font-size: 18px;
    }
    .contain .nav .content {
    background-color: bisque;
    }
    .contain .nav .content:hover {
    color: yellow;
    }

```
下面来看一段sass的源码，和less基本相同：
```scss
	body{
    margin: 0;
    padding: o;
}

.contain {
    background: #fff;
    .nav{
        font-size: 18px;
        .content {
            background-color: bisque;
            &:hover{
                color: yellow;
            }
        }
    }
}
```
然后使用命令：
** .\node_modules\.bin\node-sass.cmd  --output-style   expanded .\hellow.scss hellow-sass.css **
来进行编译（--output-style   expanded 的作用是输出文件为展开的CSS样式文件）

## less中变量的使用
在less中我们使用 @变量名 来命名一个变量，如下示例：
```less
@baccolor:red;
@my_font:14px;

body{
    margin: 0;
    padding: o;
}

.contain {
    background:lighten(@baccolor, 40%);
    .nav{
        font-size: @my_font;
        .content {
            background-color: @baccolor;
            font-size: 14px + 2px;
            &:hover{
                color: yellow;
            }
        }
    }
}
```

然后运行编译命令后输出以为css文件内容：

```css
    body {
    margin: 0;
    padding: o;
    }
    .contain {
    background: #ffcccc;
    }
    .contain .nav {
    font-size: 14px;
    }
    .contain .nav .content {
    background-color: red;
    font-size: 16px;
    }
    .contain .nav .content:hover {
    color: yellow;
    }

```
可以看到背景色和字体大小都通过变量的计算发生了变化

## sass的变量用法使用 $加变量名

```scss
    $baccolor:red;
    $my_font:14px;

    body{
        margin: 0;
        padding: o;
    }

    .contain {
        background:lighten($baccolor, 40%);
        .nav{
            font-size: $my_font;
            .content {
                background-color: $baccolor;
                font-size: 14px + 2px;
                &:hover{
                    color: yellow;
                }
            }
        }
    }
```

编译后的输出为

``` css
body {
  margin: 0;
  padding: o;
}

.contain {
  background: #ffcccc;
}

.contain .nav {
  font-size: 14px;
}

.contain .nav .content {
  background-color: red;
  font-size: 16px;
}

.contain .nav .content:hover {
  color: yellow;
}

```
和less的输出基本上相似

## mixin的使用
mixin可以使CSS代码进行复用，如下在less中的使用：

```less
@fontSize: 12px;
@bgColor: red;

// 使用一个类选择器来定义个一个mixin函数，有以下两种方式：

// 不加括号类型的
.my_mixin_block1{
    width: 300px;
    height: 200px;
}

// 不加括号的mixin编译后会存在，加括号的会消失
.my_mixin_block2(@input_px){
    width: 300px + @input_px;
    height: 200px + @input_px;
}

body{
    padding:0;
    margin:0;
}

.wrapper{
    background:lighten(@bgColor, 40%);
    .my_mixin_block1();
    .nav{
        .my_mixin_block1();
        color:red;
    }
    .content{
        .my_mixin_block2(50px);
        &:hover{
            background:red;
        }
    }
}
```

以下是编译后的css文件内容：
```css
.my_mixin_block1 {
  width: 300px;
  height: 200px;
}
body {
  padding: 0;
  margin: 0;
}
.wrapper {
  background: #ffcccc;
  width: 300px;
  height: 200px;
}
.wrapper .nav {
  width: 300px;
  height: 200px;
  color: red;
}
.wrapper .content {
  width: 350px;
  height: 250px;
}
.wrapper .content:hover {
  background: red;
}

```

## sass mixin的用法
sass中使用如下语法来定义一个mixin,创建方式与使用方式相对less都有所区别
```scss
$fontSize: 12px;
$bgColor: red;


@mixin block1($input_size){
    font-size: $input_size;
    border: 1px solid #ccc;
    border-radius: 4px;
}

@mixin block2{
    border: 1px solid #ccc;
    border-radius: 4px;
    width: 300px;
    height: 400px;
}

body{
    padding:0;
    margin:0;
}

.wrapper{
    background:lighten($bgColor, 40%);

    .nav{
        @include block1($fontSize + 4px);
    }
    .content{
        @include block2();
        &:hover{
            background:red;
        }
    }
}
```

## extend的使用
使用extend方法可以使CSS选择自动分组，可以减少代码重复
如下是less中使用extend的示例：
```less

@fontSize: 12px;
@bgColor: red;

.block1{
    font-size: @fontSize;
    border: 1px solid #ccc;
    border-radius: 4px;
}

.block2{
    width: 300px ;
    height: 400px ;
}

body{
    padding:0;
    margin:0;
}

.wrapper{
    background:lighten(@bgColor, 40%);

    .nav:extend(.block1){
        color: #333;
    };
    .main:extend(.block2){
        color:red
    };
    .content{
        &:extend(.block1);
        &:hover{
            background:red;
        }
    }
}

```

输出CSS文件的内容为：

```css
.block1,
.wrapper .nav,
.wrapper .content {
  font-size: 12px;
  border: 1px solid #ccc;
  border-radius: 4px;
}
.block2,
.wrapper .main {
  width: 300px ;
  height: 400px ;
}
body {
  padding: 0;
  margin: 0;
}
.wrapper {
  background: #ffcccc;
}
.wrapper .nav {
  color: #333;
}
.wrapper .main {
  color: red;
}
.wrapper .content:hover {
  background: red;
}
```

## sass中extend的使用：
```scss

$fontSize: 12px;
$bgColor: red;

.block{
    font-size: $fontSize;
    border: 1px solid #ccc;
    border-radius: 4px;
}

body{
    padding:0;
    margin:0;
}

.wrapper{
    background:lighten($bgColor, 40%);

    .nav{
        @extend .block;
        color: #333;
    }
    .content{
        @extend .block;
        &:hover{
            background:red;
        }
    }
}

```
## 循环语句在less中的实现
	由于less中没有循环语句，因此我们需要借用递归来实现循环，如下实例：

```less
.gen-col(@n) when (@n > 0){
    .gen-col(@n - 1);
    .col-@{n}{
        width: 1000px/12*@n;
    }
}

.gen-col(12);

```

输出结果为：

```css
.col-1 {
  width: 83.33333333px;
}
.col-2 {
  width: 166.66666667px;
}
.col-3 {
  width: 250px;
}
.col-4 {
  width: 333.33333333px;
}
.col-5 {
  width: 416.66666667px;
}
.col-6 {
  width: 500px;
}
.col-7 {
  width: 583.33333333px;
}
.col-8 {
  width: 666.66666667px;
}
.col-9 {
  width: 750px;
}
.col-10 {
  width: 833.33333333px;
}
.col-11 {
  width: 916.66666667px;
}
.col-12 {
  width: 1000px;
}
```

## sass中loop的实现
sass中有可用的循环语句，因此可以有如下实现
```scss

// @mixin gen-col($n){
//     @if $n > 0 {
//         @include gen-col($n - 1);
//         .col-#{$n}{
//             width: 1000px/12*$n;
//         }
//     }
// }

// @include gen-col(12);


@for $i from 1 through 12 {
    .col-#{$i} {
        width: 1000px/12*$i;
    }
}

```

## import的使用
less和sass中可以使用css的模块化，与原生css模块化不同的是，css的导入会产生额外的http请求，相当于先加载再导入
而less和sass是先导入所有模块为一个文件，然后加载

有如下三个模块

model1.less
```less
@themeColor: blue;
@fontSize: 14px;
```

model2.less
```less
.module1{
    .box{
        font-size:@fontSize + 2px;
        color:@themeColor;
    }
    .tips{
        font-size:@fontSize;
        color:lighten(@themeColor, 40%);
    }
}

```

model3.less
```less
.module2{
    .box{
        font-size:@fontSize + 4px;
        color:@themeColor;
    }
    .tips{
        font-size:@fontSize + 2px;
        color:lighten(@themeColor, 20%);
    }
}

```
接下来我们使用如下文件来导入所有模块并编译：

import_main.less
``` less
@import "./model1";
@import "./model2";
@import "./model3";
```

编译后的css文件：
```css
.module1 .box {
  font-size: 16px;
  color: blue;
}
.module1 .tips {
  font-size: 14px;
  color: #ccccff;
}
.module2 .box {
  font-size: 18px;
  color: blue;
}
.module2 .tips {
  font-size: 16px;
  color: #6666ff;
}

```

同样的sass中使用如下3个模块


model1.scss
```scss
$themeColor: blue;
$fontSize: 14px;
```

model2.scss
```scss
.module1{
    .box{
        font-size:$fontSize + 2px;
        color:$themeColor;
    }
    .tips{
        font-size:$fontSize;
        color:lighten($themeColor, 40%);
    }
}

```

model3.scss
```scss
.module2{
    .box{
        font-size:$fontSize + 4px;
        color:$themeColor;
    }
    .tips{
        font-size:$fontSize + 2px;
        color:lighten($themeColor, 20%);
    }
}

```
接下来我们使用如下文件来导入所有模块并编译：

import_main.csss
``` scss
@import "./model1";
@import "./model2";
@import "./model3";
```

## 使用less外部框架est的示例：
```less
@import "est/all";

@support-ie-version: 7;
@use-autoprefixer: false;

.global-reset();

.box{
    .inline-block();
    .opacity(60);
    height: 100px;
    background: green;
    margin:10px;
}
.left{
    float:left;
    .clearfix();
}


.row{
    .make-row();
    .col{
        .make-column(1/4);
        background:red;
        height: 100px;
    }
}
.my-triangle{
    margin:100px;
    // width:100px;
    // height:200px;
    // border: 1px solid red;
}
.my-triangle::after{
    content: ' ';
    .triangle(top left, 100px, red, side);
}

```















