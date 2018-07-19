# 4 盒尺寸四大家族

## 4.1 深入理解content

### 4.1.1 contnet 与替换元素

#### 什么是替换元素

    通过修改某个属性值呈现的内容就可以被替换的元素就称为“替换元素”。典型的替换元素 `<img>`,`<object>`,`video`,`<iframe>`,`<textarea>`,`<input>`
    * 内容外观不受页面上的css的影响
    * 有自己的尺寸
    * 在很多css属性上有自己的一套表现规则


#### 替换元素的默认的display值

    所有的替换元素都是内联水平元素。替换元素的display值对于其尺寸计算规则无影响。


* 替换元素的尺寸计算规则  
   替换元素的尺寸：固有尺寸、HTML尺寸、CSS尺寸
    * 如果没有CSS尺寸和HTML尺寸则使用固有尺寸作为最终的宽高
    * 如果没有CSS尺寸则使用HTML尺寸
    * 如果存在CSS尺寸，则最终尺寸由CSS属性决定
    * 如果“固有尺寸”含有固有的宽高比例，同时仅设置了宽度或者仅设置了高度，则元素依然按照固有的宽高比例显示。
    * 如果上面的条件都不符合，则最终宽度表现为300像素，高度为150像素。宽高比2:1。
    * 内联替换元素和块级替换元素使用上面同一套尺寸计算规则。


* 替换元素和非替换元素的距离  
    * 替换元素如img去掉src属性则是普通非替换元素。
        ```html
        <style>
            img {
                display: block;
                border: 1px solid #eee;
            }
        </style>
        <!-- alt为任意值 -->
        <img alt="1"> 
        ```
        此时img的宽度100%自适应父级容器  
        
    * 非替换元素和替换元素只隔了一个CSScontent属性
     ```html
      h1 {
            content: url(../th.jpg);
            width: 50px;
            height:50px;
        }
     <!-- 普通元素content -->
      <h1>图片</h1>
     ```
      此时h1就是一个50*50的图片


# 4.2 温和的padding属性
## 4.2.2 padding的百分比值
padding百分比值无论是水平方向还是垂直方向均是相对于宽度来计算。 实际使用实例可以做自适应的等比矩形。

```html
<style>
    .img-wrapper {
        position: relative;
        padding: 50%;
    }
    .img-wrapper > img {
        position: absolute;
        width: 100%;
        height: 100%;
        left:0;
        top:0;
    }
</style>
 <div class="img-wrapper">
    <img src="../th.jpg" alt="1" >
</div>
```
内联元素的padding
 * padding相对宽度计算
 * 默认的高度的宽度细节有差异
 * padding 会断行

# 4.3 激进的margin属性  
## 4.3.1 margin与元素尺寸以及相关布局 
1. margin 与元素的内部尺寸

    marign改变元素的可视尺寸与padding相反。对于paddding，元素设定了width属性或者保持**包裹性**的时候，会改变元素可视尺寸。而margin则无视之，只有当元素是“充分利用可用空间”状态的时候，margin才可以改变元素的可视尺寸。

    ```html
    <style>
        .father {
            padding:0 10px ;
            
        }
        .son {
            margin: 0 -10px;
            line-height: 20px;
            text-align: center;
            background-color:#e1e1e1;
        }
    </style>
    
    <body>
        <!-- marigin 改变子级宽度 -->
        <div class="father">
            <div class="son">barfoo</div>
        </div>
    </body>
    ```

 ## 4.3.4 深入理解CSS中的margin:auto
 触发`margin:auto`计算有个前提条件就是width或者height为auto时，元素是具有对应方向的自动填充特性。(比如块状元素的width默认100%继承父级，绝对定位元素的格式化宽度和高度)

 `marigin:auto`的填充规则  
   1. 如果一侧定值,一侧auto,则auto为剩余空间。
   1. 如果两侧均是auto,则平分剩余空间。 

   针对第一条可以实现**块状元素**的左右对齐(而不是只有通过`float:left/right`)

## 例子（利用content生成伪元素以及margin:auto实现常见布局）

![](http://okn6xxp76.bkt.clouddn.com/cssworld/cssworld1.png)
```html
 <style>
    .home-icon-item:after {
        position: absolute;
        display: inline-block;
        content: '';
        width: 50px;
        height: 50px;
        left: 0;
        top: 0;
        right: 0;
        bottom: 0;
        margin: auto;
        border-radius: 50%;
        z-index: -1;
    }

 </style>    

 <div class="home-icon-wrapper">
    <div class="home-icon-item">
        <i class="iconfont">1</i>
        <span class="text">培训</span>
    </div>
    <div class="home-icon-item">
        <i class="iconfont">2</i>
        <span class="text">会议</span>
    </div>
    <div class="home-icon-item">
        <i class="iconfont">3</i>
        <span class="text">学员服务</span>
    </div>
    <div class="home-icon-item">
        <i class="iconfont">4</i>
        <span class="text">资料下载</span>
    </div>
</div>
```


# 4.4功勋卓越的border属性
## 4.4.3 border-color和color

border-color 有一个很重要也很实用的特性，就是“border-color默认颜色就是color色值”。根据这个特性可以做个简单的边框hover变色实例。

``` html
<style>
    .border {
        position: relative; 
        width: 80px;
        height:80px;
        border:2px dashed;
        color: #e3e3e3;
    }
    .border:hover {
        color:red;
    }
    .border::after {
        position: absolute;
        display: block;
        content: '';
        width:40px;
        height: 40px;
        border-top:6px solid;
        top:40px;
        left:40px;
        margin-top: -3px;
        margin-left: -20px;
    }
    .border::before {
        position: absolute;
        display: block;
        content: '';
        width:40px;
        height: 40px;
        top:40px;
        left:40px;
        margin-top: -20px;
        margin-left: -3px;
        border-left:6px solid;
    }
</style>
<body>
    <!-- border属性缺省 -->
    <div class="border">

    </div>
</body>
```

## 例子（利用border和margin实现等分布局）
```html
<style>
.bg {
    padding: 15px  30px;
    background-color: #e1e1e1;
}

.ul-wrapper {
    padding-left: 0;
    margin-right: -15px;
}
.ul-wrapper::after {
    display: block;
    content: '';
    clear: both;
}
.li-item {
    float: left;
    width:25% ;
    box-sizing: border-box;
    border-right: 15px solid transparent;
    list-style-type:none;
}
.color {
    height: 150px;
    background-color: #08c;
}
</style>
 <!-- margin 等分布局 -->
    <div class="bg">
        <p >管理中心</p>
        <ul class="ul-wrapper">
            <li class="li-item">
               <div class="color"></div>
            </li>
            <li class="li-item">
                <div class="color"></div>
            </li>
            <li class="li-item">
                <div class="color"></div>
            </li>
            <li class="li-item">
                <div class="color"></div>
            </li>
        </ul>
    </div>
```
## 两种等高布局
 *  border
 *  margin + padding

 ```html
 <style>
.border-box {
    margin: 10px 0;
    border-left:150px solid #c7c7c7;
    background-color: #f1f1f1;
    
}

/* 此处不能用overflow:hidden清除浮动否则左浮动的导航列表元素就会被隐藏掉 */
.border-box::after {
    display: block;
    content: '';
    clear: both;
}
.border-box > nav {
    width: 150px;
    margin-left: -150px;
    float: left;
    text-align: center;
    
}
 </style>
 <!-- border等高布局-->
<div class="border-box">
    <nav>
        <h3>导航1</h3>
        <h3>导航2</h3>
    </nav>
    <section>
    <h5>模块1</h5>
    <h5>模块2</h5>
    <h5>模块3</h5>
    </section>
</div>

 ```

 ```html
 <style>
.margin-box {
    overflow: hidden;
}
.column-left ,.column-right {
    width: 50%;
    float: left;
    padding-bottom: 9999px;
    margin-bottom: -9999px;

}
.column-left {
    background-color: #c7c7c7;
}
.column-right {
    background-color: #f1f1f1;
}
</style>
 <!-- margin + padding 等高布局 -->
<div class="margin-box">
    <div class="column-left">
        <h3>导航1</h3>
        <h3>导航2</h3>
    </div>
    <div class="column-right">
        <h4>模块1</h3>
    </div>
</div>
 ```