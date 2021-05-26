# Less

## less 介绍

#### 1. 什么是Less

less,sass,stylus 都属于css预处理器，基本思想是用一种专门的编程语言，为CSS增加一些编程的特性，如：变量、语句、函数、继承等概念。将CSS 作为目标生成文件，开发者只需要使用这种语言进行CSS 编码工作。

使用原因：

css中 书写很多重复的选择器，导致了我们在工作中无端增加了许多工作量

使用CSS预处理器，提供 CSS 缺失的样式层复用机制、减少冗余代码，提高样式代码的可维护性。大大提高了我们的开发效率。

#### 2. less 官方网站

​    官方网站  https://less.bootcss.org/

​    中文网  https://less.bootcss.com/   

#### 3. less 示例

#### 4. less 中的注释

​    单行不会编译到CSS文件中去  //

​    多行会编译到CSS 文件中去  /* */

## 使用less

**注意**：less 文件只有被编译后才能被浏览器识别，浏览器只认识css 文件

#### Less 编译工具

1. 考拉  http://koala-app.com
2. koala是一个国产免费前端预处理器语言图形编译工具，支持Less、Sass、Compass、CoffeeScript，帮助web开发者更高效地使用它们进行开发。跨平台运行，完美兼容windows、linux、mac。

安装注意：

**必须要新建一个文件夹，且该文件夹中没有其他内容，然后将该文件夹设置为安装地址。**

直接安装在一个有其他文件的目录中 会导致 在安装过程中，把该目录下其他的文件都删除了且回收站都找不到

2. 编辑器自带编译less插件

   （1）hbuliderX 

      **首先** “菜单栏” 找到 “工具”，选择“插件安装”，找到“less编译” "sass/scss编译" 点击安装，重启hbuilderX

      **其次** 安装成功后，“菜单栏” 找到 “工具”，选择“插件配置”，选择“compile-less”,选择package.json

      **再** 搜索属性名： onDidSaveExecution，默认false, 在此修改为 true

   ​        搜索属性名：key ，默认“”，设置为"ctrl+s", 即保存文件时自动编译

     （2）Vscode

   ​     首先 选择左侧菜单“扩展”，搜索”easyless“ 插件，安装完成， 重启Vscode

   ​     其次  选择顶部菜单”文件“，找到”首选项“ ，点击”设置“  ，搜索less.complie 选择  setting.json 添加配置

   ```
"less.compile": {
           "compress": false, // 不压缩
           "sourceMap": false, // 不生成源码文件
           "out": "./css",       // 这里是代表编译后生成的css文件所放的位置 基于当前less文件位置   
           ./css 代表 生成以css 为命名的css 文件
           ./css/ 代表 生成css文件的目录 为css文件夹，生成的css文件名称默认为原less文件的名称
   }     
   ```

#### 客户端调式方式

#####   1. 引用.less 文件

​       link 引入需要将rel 属性设置为 rel="stylesheet/less" 

​       本页面使用less语法时 指定类型 text/less

#####   2. 引用less.js 

​      必须放置在less 样式文件之后

## 变量

##### 1. 普通的变量

变量的定义方式@ 

示例：

 less 语句 :

```
@red:#ff000;
.box{
  color:@red;
}
```

 编译结果：

```
 .box{color:#ff000;}
```

注意：由于变量只能定义一次，实际上他们就是”常量“

##### 2. 作为选择器和属性名

使用规则：@{变量名}

示例：

 less 语句：

```
 @myselector:width  
 .@{myselector}{
     @{myselector}:960px
 }
```

 编译结果：

```
 .width{
     width:900px
 }
```

##### 3. 作为URL

使用规则：”“ 将变量值包裹起来，@{变量名} 使用

示例：

 less 语句：

```
 @bgUrl:"http://www.baidu.com"
 .box{
   background-image:url("@{bgUrl}/logo.png");
 }
```

编译结果：

```
.box{
   background-image:url("http://www.baidu.com/logo.png");
 }
```

##### 4. 延迟加载

说明：变量是延迟加载的，在使用前不一定要预先声明

大意：在前面使用了这个变量，但没有定义，但可以在后面给他定义出来，不影响输出。

示例：

less 语句

```
.box{
   color:@red
}
@red:red
```

编译结果

```
.box{
   color:red
}
```

##### 5. 定义多个相同名称的变量时的规则

 定义一个变量多次时，按最后定义的变量走，less 会从当前作用域中向上搜索（ 类似 css 属性值 覆盖）

```
@font:12px;
ul{
   @font:16px;
   font-size: @font;
       li{
           @font:20px;
           a{						
               font-size: @font;
               @font:24px;
           }
           @font:12px;
           font-size: @font;
        }
    @font:12px
}
// 编译结果：ul=12px li=12px a=24px
```

## 混合

说明：**将需要重复使用的代码封装到一个类中**,在需要使用的地方调用封装好的类即可，在预处理的时less会自动将用到的封装好的类中的代码拷贝过来，本质就是ctrl+c 到  **ctrl + v** 的过程

##### 1. 普通混合

说明：将一系列属性从一个规则集引入到另一个规则集

示例：

less 语句：

```
.bg{
   color:red;
   background:blue;
}
.box1{
   width:100px;
   .bg
}
.box2{
   width:100px;
   .bg
}
```

编译结果：

```
.box1{
   width:100px;
   color:red;
   background:blue;
}
.box2{
   width:100px;
   color:red;
   background:blue;
}
```

##### 2. 不带输出的混合

用法：创建一个混合集，但不让它编译到最终的CSS样式中，在不想输出的混合集的名字后面加一个括号()

说明：如果混合名称的后面没有(), 那么在预处理的时候,会保留混合的代码（意思是在输出的css中，会含有.my-other-mixin这部分代码），如果混合名称的后面加上(),那么在预处理的时候,不会保留混合的代码，

示例：

less 语句：

```
.my-mixin{
   font-size:16px;
}
.my-other-mixin(){
   color:red;
}
.box{
   .my-mixin;
   .my-other-mixin;
}
```

编译结果：

```
.my-mixin{
   font-size:16px;
}
.box{
   font-size:16px;
   color:red;
}
```

##### 3. 带选择器的混合 比如 伪类

less 语句：

```
.hover-mixin{ // 不带输出的混合
  &:hover{
    color:pink;
  }
}

a{
  .hover-mixin
}
```

编译结果：

```
a:hover{
  color:pink;
}
```

##### 4. 带参数的混合

定义混合

```
.border(@border-color){
    border:1px solid @border-color;
}
```

使用混合( 需要传参数进去)

```
div{
   .border(red) 
}
```

编译结果：

```
div{
  border: 1px solid red;
}
```

##### 5. 带参数且有默认值

定义混合

```
.border(@border-color:pink){
    border:1px solid @border-color;
}
```

使用混合( 不传参)

```
div{
   .border() 
}
```

编译结果：

```
div{
   border:1px solid pink;
}
```

传参时如有实参即按所传参数走

##### 6. 带多个参数的混合

一个组合可带多个参数，参数之间用逗号或者分号分割

示例：

less 语句：

```
.mixin(@color: red; @fontSize: 14px) {
  color: @color;
  font-size: @fontSize;
}
.box1{
  .mixin(@color: #ccc, @fontSize: 18px);
}
.box2{
  .mixin(@fontSize: 22px,#eee;); // 注意带;了
}
```

编译结果：

```
.one {
  color: #ccc;
  font-size: 18px;
}
.two {
  color: red;
  font-size: 22px, #eee;
}
```

总结：

形参括号里全是以 逗号（，）分割，那么会依次传给各个参数值

形参括号里是以 逗号（，）和分号（；）分割，那么会将分号前看作一个整体，传给一个参数值

##### 7. 命名参数

形参通过特定的参数名称而不是参数位置来提供参数值

```
.box2{
  .mixin(@fontSize: 22px,@color:#eee); 
}
```

##### 8. @arguments 变量

说明：代表所有的可变参数

注意点：所传参数必须是一一对应，若不想传参，写默认值

less 语句

```
.boxShadow(@x,@y,@blur,@color){
    -moz-box-shadow: @arguments;
    -webkit-box-shadow: @arguments;
    box-shadow: @arguments;
}
.box4{
    .boxShadow(2px,2px,3px,#FF36)
}
```

编译结果

```
.box4 {
    -moz-box-shadow: 2px 2px 3px #FF36;
    -webkit-box-shadow: 2px 2px 3px #FF36;
    box-shadow: 2px 2px 3px #FF36;
}
```



##### 9. 匹配模式

传值的时候定义一个字符，在使用的时候使用那个字符，就调出那条规则

```
.border(all,@color:red){
    border:1px solid @color;
}
.border(b-l,@color:red){
    border-left:1px solid @color;
}
.border(b-r,@color:red){
    border-right:1px solid @color;
}
.border(b-t,@color:red){
    border-top:1px solid @color;
}
.border(b-b,@color:red){
    border-bottom:1px solid @color;
}

.box4{
    .border(b-r,blue)
}	
```

编译结果

```
.box4{
    border-right: 1px solid #0000ff;
}	
```

##### 10. 得到混合变量中的返回值

less 语句

```
.compute(@a,@b){
    @sum:(@a + @b); // @cha:(@a - @b)
}
.box4{
   .compute(200px,100px);
   width:@sum;
  // height:@cha
}	
```

编译结果

```
.box4{
    width: 300px;
    height: 100px;
}
```



## 嵌套规则

##### 1. 什么是嵌套规则

  模仿了HTML结构，让css代码更加简洁明了

##### 2. 示例

传统css写法

```
.header{
    background:blue;
}
.header .logo{
    width:100px;
}
.header nav{
    width:500px;
}
```

less 写法

```
.header{
    background:blue;
    .logo{
        width:100px;
    }
    nav{
        width:500px;
    }
}
```

##### 3.父元素选择器 &

传统css写法

```
li{
   background:pink;
}
li a{
   color:#fff;
}
li a:hover{
   color:#000
}
```

less 写法

```
li {
   background:pink;
   a{
      color:#fff;
      &:hover{
        color:#000 
      }
   }
}
```

##### 4. 改变选择器的顺序

说明：把&放到当前选择器后，就会将当前选择器插入到所有的父选择器之前

注意：& 符号 前后有无空格的区别

less 语句

```
ul {
   li {
      .box & {
         background:red
      }
   }
}
```

编译结果

```
.box ul li{
   background:red  // 指定当前子选择器所属的父选择器
}
```

##### 5. 组合使用生成所有可能的选择器列表

less 语句

```
ul,li,a{
    border:1px solid red;
    & &{ // & 代表所有父类 & & 代表 两两组合  & & & 三三组合
       border-top:0;
    }
}
```

编译结果

```
ul ul,
ul li,
ul a,
li li,
li a{
   border-top:0;
}
```



## 运算

##### 1. 运算说明

   任何数值，颜色，变量都可以进行运算

##### 2. 数值型运算

  less 会自动推算数值的单位，不必每一个值都加单位

  运算符与值之间必须以空格分开，涉及优先级时以()进行优先级运算

示例

```
.box{
   width:500px + 100
}
```

优先级运算

```
.box{
    width: (150px - 50) * 6  
}
```

结果

```
.box{
    width:600px
}
```

##### 3. 颜色值运算

less 在运算时，先将颜色转化为rgb模式，然后转换为16进制的颜色值并且返回

示例：

less运算

```
.box{
    color:#000000 + 21
}
```

编译结果

```
.box{
    color:#151515
}
```

注意：rgb 取值范围 0-255 ，所以所加的值不能超过这个区间，超过后默认使用最大值255计算

不能直接以颜色值进行运算



## 函数

less 中提供了许多用于准换颜色，处理字符串和进行算术运算的函数，使用起来非常简单

##### 1. 常见的rgb()函数

将rgb 转换成16进制

less 示例

```
.bg{
   background:rgb(0,0,0)
}
```

编译结果

```
.bg{
   background:#000000
}
```

##### 2. 提取蓝色值 blue()函数

less 示例

```
.bg{
   background:blue(#ffffff)  // 255,255,255
}
```

编译结果

```
.bg{
   background:255
}
```



## 命名空间

##### 1. 什么是命名空间

将一些需要的混合组合在一起，可通过嵌套多层id或者class来实现

less 语句

```
.bg-color{
    background: #FFFF00;
    .color1{
        color:yellow;
        &:hover{
			font-size: 20px;
		}
        .color2{
			color:green;
			font-size: 30px;
        }
    }
    .color3{
        color:orchid;
        font-size: 30px;
    }
}
```

编译结果

```
.box4{
    .bg-color  //  background: #FFFF00;
    .bg-color>.color1  // color:yellow; hover:font-size: 20px;
    .bg-color>.color1>.color2  //  color:green; font-size: 30px;
    .bg-color>.color3  // color:orchid; font-size: 30px;
}
```

##### 2. 省略写法

大于号 可有空格 替代

```
.box4{
    .bg-color .color1
}
```



## 作用域

##### 1. 什么是作用域

less 中的作用域和编程语言中的作用域概念相似，首先会在局部查找变量和混合，如果没有找到，编辑器就会在父作用域中查找，以此类推

less 语句

```
@border:1px solid red;
.box4{
    background: #000000;
    color: #fff;
    a {
        border:@border;
    }
    @border:1px solid yellow;
}
```

编译结果

```
.box4 a{
   border:1px solid yellow;   // a 自己的作用域里没有这个变量 去父级找
}
```

less 语句

```
@border:1px solid red;
.box4{
    background: #000000;
    color: #fff;
    a {
        @border:1px solid pink;
        border:@border;
    }
    @border:1px solid yellow;
    border:@border
}
```

编译结果

```
.box4 a{
   border:1px solid pink;   // a 自己的作用域里有
}
```



## 引入 importing

##### 1. 什么是引入

可以引入一个或者多个less 文件，这个文件中的所有变量都可以在当前的less 项目中使用

比如：有一个style.less 文件，其中一个变量为@bg:red

现在我们想引入这个文件，然后使用其中的变量 @bg

当前项目文件 index.less 

  (1) import  "main" // 引入main.less 文件  

  (2) 使用main.less 文件的变量 

```
.box{
   background:@bg
}
```

注意：引入.css 文件会被原样输出到编译文件中

示例：b.less 引入a.css

a.css

```
.aBorder{
	border:20px dashed #007AFF;
}
```

b.less

```
@import "test";
@import "a.css";
```

编译b.less 结果

```
@import "a.css";
```

##### 2. 引用文件 带参数

##### （1）reference  使用less文件 但不输出

 a.less 文件

```
@bg:red;
.test{
   color:pink
}
```

b.less

```
@import "a";
.box{
  border-color:@bg
}
```

b.css (b.less 编译结果)

```
.test{
   color:pink
}
.box{
  border-color:red
}
```

b.less

```
@import (reference) "a"; // 注意reference 前后空格
.box{
  border-color:@bg
}
```

b.css (b.less 编译结果)

```
.box{
  border-color:red
}
```

##### （2）inline 引用less 文件 但不作任何处理，包括不适用其中的变量混合等

a.less

```
@bg:red;
```

b.less

```
@import (inline) "test.less";
.box{
   width:100px;
}
```

b.css (b.less 编译结果)

```
@bg:red;
.box{
   width:100px;
}
```

##### 3. 将less 作为文件对象，无论其扩展名是什么,都以less 文件来操作

```
@import (less) "a.css";
```

可在a.css 中定义变量 ，在当前less 文件中使用

##### 4. 将css作为文件对象，无论其扩展名是什么，都以css 文件来操作,且原样输出

less 文件

```
@import (css) "a.less";
```

编译结果：

```
@import "a.less";
```

##### 5. multiple  允许引入多次 文件名相同的文件

a.less

```
.color{
  color:red
}
```

less 文件

```
@import (multiple) "a.less";
@import (multiple) "a.less";
@import (multiple) "a.less";
```

编译文件

```
.color{
  color:red
}
.color{
  color:red
}
.color{
  color:red
}
```



## 关键字

说明：在调用的合集后面加！important ,所有混合集都继承它

```
.med(@color,@font,@height){
    color:@color;
    font-size: @font;
    height:@height
}

.box5{
   .med(red,30px,300px) !important
}
```

编译结果

```
.box5 {
    color: #ff0000 !important;
    font-size: 30px !important;
    height: 300px !important;
}
```

## 条件表达式

##### 1. 带条件的混合

比较运算符   <、>、<=、>=、=

注意：Less使用关键字 when 而不是 if/else来实现条件判断

```
.mixin (@a) when (@a = 20px){
    color:red;
}
.mixin (@a) when (@a < 20px){
    color:blue;
}
.mixin (@a) {
    font-size: @a;
}
.box{
   .mixin (20px)
}
```

编译结果

```
.box6 {
  font-size: 20px;
  color: red;
}
```

还可以使用关键字true，它表示布尔真

```
.class (@a) when (@a) { ... }
.class (@a) when (@a = true) { ... }
```

在Less中，只有 true 表示布尔真，关键字true以外的任何值，都被视为布尔假

```
.myclass {
  .truth(40);  // 不会匹配以上任何定义
}
```

Less中，条件可以是多个表达式，多个表达式之间，使用逗号‘,’分隔。如果其中任意一个表达式的结果为 true，则匹配成功，这就相当于“或”的关系。如：

```
.mixin (@a) when (@a < -10), (@a > 10) {
    width: 100px;
}
```

上述表达式： [-10,10] 之间的值将匹配失败，其余均匹配成功。

```
.class1 {
   .mixin(-20);
}
.class2 {
   .mixin(0);
}
.class3 {
   .mixin(20);
}
```

编译结果

```
.class1 {
  width: 100px;
}
.class3 {
  width: 100px;
}
```

使用 and 和 not 来进行逻辑运算

less 语句

```
.mixin (@a) when (@a > 50%) and (@a > 5px){
    font-size: 14px;
}
.mixin (@a) when not (@a < 50%) and not (@a < 5px){
    font-size: 20px;
}
.mixin (@a) {
    color: @a;
}
```

```
.class1 {
    .mixin(#FF0000)
 }
.class2 {
    .mixin(#555)
}
```

编译结果

```
.class1 {
  color: #ff0000;
}
.class2 {
  color: #555555;
}
```

##### 2.类型检查函数

如果想基于值的类型、或特定的单位进行匹配，就可以使用 is* 函数。如：

```
.mixin (@a, @b: 0) when (isnumber(@a)) { ... }
```

```
.mixin (@a, @b: black) when (iscolor(@b) and (@a > 0)) { ... }`
```

常见的类型检查函数：

- Iscolor：是否为颜色值。
- Isnumber：是否为数值。
- Isstring：是否为字符串。
- Iskeyword：是否为关键字。
- Isurl：是否为URL字符串。

```
.mixin (@a) when (Ispixel(@a)) { ... }
```

```
.mixin (@a) when (ispercentage(@a)) { ... }
```

##### 3. 常见的单位检查函数：

- Ispixel：是否为像素单位。
- ispercentage：是否为百分比。
- isem：是否为em单位。
- isunit：是否为单位。

## 循环

循环在编程语言中应该是很常见的，一般编程语言中都有循环，例如在 JavaScript 中有 `for` 循环、`while` 循环等，但是在 Less 中没有这两种语法，而是通过自身调用来实现循环。

在 Less 中，混合可以调用它自身，当一个混合递归调用自身，再结合 表达式和模式匹配这两个特性，就可以写出循环结构。

例1:循环输出四个 `padding` 属性：

```
.loop(@i) when (@i > 0) {
   .loop((@i - 1));    // 递归调用自身
   padding: (10px + 5 * @i); 
 }
 
.call{
   .loop(4); // 调用循环
}
```

编译结果

```
.call {
  padding: 15px;
  padding: 20px;
  padding: 25px;
  padding: 30px;
}
```

分析：

- 首先看第一行，是一个带有条件判断的混合，这个前面学过。`.loop` 带有一个参数 `@i`，当满足条件 `@i > 0` 时可以执行花括号中的代码。
- 然后 `.loop((@i - 1))` 表示调用混合自身，并且将参数值减去一，这一步只要满足条件判断就会执行，一直执行到 `@i > 0`
- 最后就是在 `.call` 中调用 `.loop`，给参数 `@i` 赋值，因为要求是循环输出四次 `padding` 属性，所以可以给 `@i` 参数赋值为 4。



例2：使用递归循环生成栅格系统的CSS。

```less
.xkd(@n, @i: 1) when (@i =< @n) {
  .grid@{i} {
    width: (@i * 100% / @n);
  }
  .xkd(@n, (@i + 1));
}

.xkd(5);
```

编译结果：

```
.grid1 {
  width: 20%;
}
.grid2 {
  width: 40%;
}
.grid3 {
  width: 60%;
}
.grid4 {
  width: 80%;
}
.grid5 {
  width: 100%;
}
```

例3：在实际项目中，我们经常会将字体、边距等样式作为公共样式放到一个公共的文件中，然后定义一些列的样式，例如字体大小定义 12px、14px、16px 等等，这样一个一个写会比较麻烦，如果使用循环，几句代码就能实现：

```
.font(@i) when(@i <= 28){
   .f@{i} {
      font-size: @i + 0px ; //为了生成12px
   }
   .font((@i + 2));
}
   
.font(12);
```

编译结果：

```less
.f12 {
  font-size: 12px;
}
.f14 {
  font-size: 14px;
}
.f16 {
  font-size: 16px;
}
.f18 {
  font-size: 18px;
}
.f20 {
  font-size: 20px;
}
.f22 {
  font-size: 22px;
}
.f24 {
  font-size: 24px;
}
.f26 {
  font-size: 26px;
}
.f28 {
  font-size: 28px;
}
```

例4：改变颜色

循环需要知道两个函数的用法，

length：用来获取定义的变量的长度，

extract：用来获取定义的变量的第`n`个元素（类似于`js`的通过下标获取数据）

```
// 定义颜色数组
@colorArr: #37a2da, #32c5e9, #67e0e3, #9fe6b8;
 
// 定义循环最大值，此处使用颜色数组的长度
@len: length(@colorArr); // 4

/**
 * 定义循环方法
 * @index--传入的循环起始值
 * @len--循环的最大值  也可使用常量  eg:(@index<4)
 */ 
.Loop(@index) when(@index<@len){
 
    // 执行内容 
    // 类名参数要加大括号@{index}   
    // 根据index获取对应的某个值 extract(数组名, 对应的序号)
    li:nth-child(@{index}){  // 作为选择器
        color:extract(@colorArr,@index);
    }
 
    //递归调用 达到循环目的
    .Loop(@index+1);
 
}
 
// 调用循环
.Loop(0);

```

编译结果

```
li:nth-child(1){
    color: #37a2da;
}
li:nth-child(2){
    color: #32c5e9;
}
li:nth-child(3){
    color: #67e0e3;
}
li:nth-child(4){
    color: #9fe6b8;
}
```

## 合并属性

##### 1. ”+“逗号分割所合并的属性

```
.mixin() {
  box-shadow+: inset 0 0 10px #555 ;
}
.myclass {
  .mixin();
  box-shadow+: 0 0 20px black;
}
```

编译结果

```
.myclass {
  box-shadow: inset 0 0 10px #555 , 0 0 20px black;
}
```

##### 2. "+_" 空格分隔所合并的属性

```
.a(){
  background+:#f60;
  background+_:url("/sss.jod") ;
  background+:no-repeat;
  background+_:center;
}
.myclass {
  .a()
}
```

编译结果

```
.myclass {
  background: #f60 url("/sss.jod"), no-repeat center;
}
```

## 函数库

#####  长度相关函数

1. length() 函数

   ```
   length（1px solid red）// 输出3
   ```

2. extract() 函数

   返回数组变量中指定索引的值

   ```
   @list:"red","blue","pink","grey";
   n:length(@list)
   
   n:4
   ```


   其他函数参考官方文档

## 总结

常用的语法

1. 定义变量 

   比如：图片路径

2. 混合

   （1） 不带输出的混合   

   ```
   .img-100(){
      display:block;
      width:100%;
   }
   .box{
      img{
        .img-100
      }
   }
   ```

3. 带选择器的伪类混合

   ```
   .hover-a(@color,@bg:transparent){ // 不带输出的混合
     &:hover,&.active{
       color:@color;
       background:@bg
     }
   }
   
   li{
      a{
        .hover-a(red)
      }
   }
   ```

4. 嵌套规则

   ```
   .box{
      .content{
          .left{
             width:50%;
          }
          .right{
             width:50%;
          }
      }
   }
   ```

5. 命名空间

​    为了更好地封装，我们会将会一些变量或是混合模块进行打包操作，为了后续进行复用

```
.bundle{ // 打包    
   // 关于伪类的
   .hover(@color,@bg){
      color:@color;
      background:@bg;
   }
   // 关于背景图片的
   .back(@path,@img,@repeat,@position:left top,@size:auto){
      background: url("@{path}@{img}") @{repeat} @{position}/@{size}
   }
   // 定位的
   .postion-r(@way,@left,@right,@top,@bottom){
       position:@way;
       left:@left;
       right:@right;
       top:@top;
       bottom:@bottom;
   }
}
```

##### 6. 导入

```
可以将所有的变量和类单独放在一个less文件中，便于修改和管理
```

# Sass和Scss

## sass和scss 介绍

ruby社区的工程师觉得CSS很难用于是发明了sass，sass可以理解为简洁版CSS，但是传统前端表示看不懂，于是设计者在sass新增功能的基础上改的比较像CSS，从而方便传统前端使用，最终形成了SCSS

##### 1.sass有两套语法

1. 最新的语法叫scss,它是css 语法的扩展
2. 更旧的语法叫sass，

##### 2. sass 官网

  英文网站   https://sass-lang.com/

   中文网     https://www.sass.hk/

## 使用scss

**注意**：scss 文件只有被编译后才能被浏览器识别，浏览器只认识css 文件

scss 基于服务端  node ,需要下载 node.js  http://nodejs.cn/download/

##### 1. scss 编译工具

1. 考拉  http://koala-app.com
2. <u>**安装时必须要新建一个文件夹，且该文件夹中没有其他内容，然后将该文件夹设置为安装地址**</u>，直接安装将会删除该目录下所有文件。

##### 2. 编辑器自带编译

1. HbuliderX

   同less 安装方式相同

   （1）菜单栏 — 工具 — 插件安装 — scss/sass 编译 

   （2）菜单栏 — 工具 — 插件配置 —  compile-node-sass  打开package.json

   ```
   "commands": [
       {
       "id": "SASS_COMPILE",
       "name": "编译scss/sass",
       "command": [
           "${programPath}",
           "${file}",
           "../css/${fileBasename}.css"  // 编译后的css文件位置 
       ],
       "extensions": "scss,sass",
       "key": "ctrl+s",  // 设置编译快捷键  保存时编译
       "showInParentMenu": false,
       "onDidSaveExecution": true  // 
       }
   ]
   ```

   安装出错解决方案：

   出现这种情况一般都是node-sass版本和node版本不对应

   1. cmd  打开命令行工具  检查node 版本    

   2. node -v   显示node版本
   3. 根据node 版本 去找  windows 对应下的  node-sass 版本

   ​     去GitHub找到node-sass的对应版本：https://github.com/sass/node-sass/releases

   ​    根据编辑器报错信息，下载对应版本，放到对应报错文件夹下，且修改文件名  binding.node

   4. 文件夹位置  编辑器已给出提示

      具体操作：桌面— 编辑器— 右击— 打开文件位置— plugins — compile-node-sass— node_modules—

      node-sass-china —vendor —win32-x64-79 — binding.node

      ![image-20210520154129649](C:\Users\11050\AppData\Roaming\Typora\typora-user-images\image-20210520154129649.png)

      ![image-20210520154355957](C:\Users\11050\AppData\Roaming\Typora\typora-user-images\image-20210520154355957.png)

      ![image-20210520154554814](C:\Users\11050\AppData\Roaming\Typora\typora-user-images\image-20210520154554814.png)

      ![image-20210520153542263](C:\Users\11050\AppData\Roaming\Typora\typora-user-images\image-20210520153542263.png)

      ![image-20210520154756850](C:\Users\11050\AppData\Roaming\Typora\typora-user-images\image-20210520154756850.png)

2. vscode

     选择左侧菜单“扩展”，搜索”easy Sass “ 插件，安装完成， 重启Vscode

    顶部菜单-->文件-->首选项--> 设置--> 搜索sass--> 选择easy sass --> 在setting.json中编辑

   ```
   "easysass.formats": [ 
           //expanded：没有缩进的、扩展的css代码。
           //compact：简洁格式的 css 代码。
           //compressed：压缩后的 css 代码
   
           {
               "format": "expanded",
               "extension": ".css" //设置编译输出的文件名
           },
           {
               "format": "compressed",
               "extension": ".min.css" //设置编译输出的文件名
           }
       ],
       "easysass.targetDir": "./css/" //提供 css 输出路径的设置（可以是绝对路径或者相对路径）
   ```
   

![img](https://img2020.cnblogs.com/blog/309980/202006/309980-20200611103854870-618680889.png)

## scss 语法

##### 1. 声明变量

语法：以$开头

```
$avtive-color:red;
div{
   border: 1px solid $active-color
}
```

属性连写

```
.userCard-name {
    color: black;
    font: {
      size: 28px;
      weight: bold;
      family: 'Courier New', Courier, monospace;
    }
}
```

编译结果

```css
.userCard-name {
    color: black;
    font-size: 28px;
    font-weight: bold;
    font-family: 'Courier New', Courier, monospace;
}
```

##### 2. 选择器嵌套，可配合>使用

```
 .box {
        height: 100vh;
        background: grey;
        .xxx {
            height: 50vh;
            background: yellow;

            > .x {
                height: 25vh;
                background: blue;
            }
        }
  }
```

##### 3. 选择器复用

```
.xxx{
  backgrond:red;
  &:hover{
    backgrond:blue;
  }
}  
```

##### 4. 函数function

示例：rem 替换 vw时自动计算占比

```
$designWidth: 640;
@function px($px){
   return $px/$designWidth * 10 + rem;
}
.box{
   width:px(320)
}
```

##### 5. 引入外部CSS

```
@import 'markdown-css'
```

#####  6.mixin直接复用

```
@mixin debug{
  border:1px solid red;
}

.xxx{
  @include debug;
}
```

可以配合变量使用 	

```
@mixin debug($color:red) {
    border: 1px solid $color;
}

.text {
    @include debug(blue);
    height: 100%;
    margin: 0.5em 0.5em;
}
```

##### 7.placeholder，重复使用相同CSS时直接将选择器并列

```
 %font{
        color:red;
    }
    .text1 {
        @extend %font;
        height: 100%;
        margin: 0.5em 0.5em;
    }
    .text2 {
        @extend %font;
        height: 100%;
        margin: 0.5em 0.5em;
    }
```

##### 8.配合变量循环生成CSS

```
 $class: col-;
 @for $n from 1 through 24 {
     .#{$class}#{$n} {
     width: ($n/24)*100%;
     }
 }

@for $n from 1 through 24 {
    .offset-#{$n} {
    margin-left: ($n/24)*100%;
    }
}
```

