# flexible.min.js
这段代码是从淘宝手机抓取的，做了一定修改

flexible.js是源码(和min不一样)

# sass

比方说设计稿 640 x 1136

    ```css
        $baseFontSize:        64px * 0.625

        // pixels to rems
        @function pxToRem($px)
            @return $px / $baseFontSize * 1rem

        // responsive font-size
        @mixin fontSize($fontSize)
            &
                font-size: #{$fontSize}px
            [data-dpr="3"] &
                font-size: #{$fontSize * 1.656}px
    ```
#lib.flexible

移动端自适应方案

## 使用方法

建议对于js做`内敛处理`，在**所有资源加载之前**执行这个js。

执行这个js后，会在`html`（也就是document.documentElement）上增加一个`data-dpr`属性，以及`font-size`样式。

之后页面中的元素，都可以用rem单位来设置。`html`上的`font-size`就是rem的基准像素。

## 把视觉稿中的px转换成rem

首先，假设视觉稿大小是`750`（iPhone6）。

当前方案会把这3类视觉稿分成`100份`来看待（为了以后兼容vh，vw单位）。每一份被称为一个单位a。同时，1rem单位认定为10a。拿750的视觉稿举例：

    1a = 7.5px
    1rem = 75px

因此，对于视觉稿上的元素的尺寸换算，只需要`原始px值`除以`rem基准px值`即可。例如240px * 120px的元素，最后转换为3.2rem * 1.6rem。

## 字体不使用rem的方法

字体的大小不推荐用rem作为单位。所以对于字体的设置，仍旧使用px作为单位，并配合用`data-dpr`属性来区分不同dpr下的的大小。

例如：

    div {
        width: 1rem; 
        height: 0.4rem;
        font-size: 12px; // 默认写上dpr为1的fontSize
    }
    
    [data-dpr="2"] div {
        font-size: 24px;
    }

    [data-dpr="3"] div {
        font-size: 36px;
    }

### 手动配置dpr

引入执行js之前，可以通过输出meta标签方式来手动设置dpr。语法如下：

    <meta name="flexible" content="initial-dpr=2,maximum-dpr=3" />

其中`initial-dpr`会把dpr强制设置为给定的值，`maximum-dpr`会比较系统的dpr和给定的dpr，取最小值。**注意：这两个参数只能选其一。**

### 手动设置rem基准值的方法

输出如下css样式即可：

    html {font-size: 60px!important;}

### 一些常用APIs

**[Number] lib.flexible.dpr**

当前页面的dpr值

**[Number] lib.flexible.rem** 

当前页面的rem基准值

**[Number|String] lib.flexible.rem2px([Number|String digital])**

把rem转换为px

**[Number|String] lib.flexible.px2rem([Number|String digital])** 

把px转换为rem

**lib.flexible.refreshRem()** 

刷新当前页面的rem基准值
