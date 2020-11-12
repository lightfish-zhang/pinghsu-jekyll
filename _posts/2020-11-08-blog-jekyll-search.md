---
layout: post
title: 利用Simple-Jekyll-Search为网页增加搜索功能
date: 2020-11-08 21:35:10 +0800
category: Web
thumbnail: https://raw.githubusercontent.com/Ning-Qie/github_image/master/images/miguel-teirlinck-qeiyUaSX6fk-unsplash.jpg
icon: link
summary: 折腾了半天，把jekyll环境搞崩了，又折腾了半天只为了能够在新页面显示结果。
tag: [web,jekyll,javascript,github.io,html]
---

* content
{:toc}
# 序

在之前的网站上，初始就存在搜索功能，便一直想着为github page添加搜索功能，在网上搜索后比较发现了Simple-Jekyll-Search这款搜索插件。

<br>

# 添加Search功能



## 1.准备

1.资源网站：<a href="https://github.com/christian-fei/Simple-Jekyll-Search" target="_blank">Github：Simple-Jekyll-Search</a>

因为安装官方步骤配置失败，我就直接暴力搬运了。

😌😌😌



---

2.需要的文件或代码

通过内置的<a href="https://github.com/christian-fei/Simple-Jekyll-Search/tree/master/example" target="_blank">Example</a>知道需要用到的核心文件有：

> `search.json`   ;
>
> js文件夹下的`simple-jekyll-search.min.js`和`simple-jekyll-search.js`   ;
>
> _plugins文件夹下的`simple_search_filter.rb`和`simple_search_filter_cn.rb `  ;

需要的代码分别是：

```html
<div id="search-demo-container">
    <input type="search" id="search-input" placeholder="search...">
    <ul id="results-container"></ul>
</div>
```

和

```html
<script src="{{ site.baseurl }}/js/simple-jekyll-search.min.js"></script>

<script>
    window.simpleJekyllSearch = new SimpleJekyllSearch({
        searchInput: document.getElementById('search-input'),
        resultsContainer: document.getElementById('results-container'),
        json: '{{ site.baseurl }}/search.json',
        searchResultTemplate: '<li><a href="{url}?query={query}" title="{desc}">{title}</a></li>',
        noResultsText: 'No results found',
        limit: 10,
        fuzzy: false,
        exclude: ['Welcome']
    })
</script>
```

> `noResultsText: 'No results found'`可以自定义
>
> 其含义是：你想在查询不到结果时显示的内容
>
> 我就改成了`noResultsText: '没有找到内容,请换别的关键字进行检索'`

<br>

## 2.安装

### 搬运工作

1.首先将`search.json`复制到自己的根目录。

2.将两个js文件复制到自己的js文件夹内，例如我的：`\Ning-Qie.github.io\style\js`

3.将两个.rb文件也复制到自己的_plugins文件内，例如我的：`\Ning-Qie.github.io\_plugins`

4.复制两个代码段到你需要的地方，第一个是显示的模块，第二个是js功能实现。



### 适配工作

需要注意的几个地方：

1.代码段二的

```html
<script src="{{ site.baseurl }}/js/simple-jekyll-search.min.js"></script>
```

请自己修改为合适的地址，例：

```html
<script src="/style/js/simple-jekyll-search.min.js"></script>
```



---

2.代码段二的

```javascript
json: '{{ site.baseurl }}/search.json'
```

这里的地址需要自己设置合适，不然就链接不到文件，例：

```javascript
json: '/search.json'
```



---

这里提供看到的一种大佬写的样式（详情查看参考资料），需添加到style中：

```css
#search-input {
    width: 90%;
    height: 35px;
    color: #333;
    background-color: rgba(227,231,236,.2);
    line-height: 35px;
    padding:0px 16px;
    border: 1px solid #c0c0c0;
    font-size: 16px;
    font-weight: bold;
    border-radius: 17px;
    outline: none;
    box-sizing: border-box;
    box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px rgba(102,175,233,.6);
}
#search-input:focus {
    outline: none;
    border-color: rgb(102, 175, 233);
    background-color: #fff;
    box-shadow: inset 0 1px 1px rgba(0,0,0,.075), 0 0 8px #007fff;
}
```

这样配置完应该就可以正常使用了，不过样式的位置需要自己调整。

> 可以下载example的案例运行一下看看效果

<br>

## 3.魔改



### 新标签页显示

因为原本的搜索是直接在input框下展示结果，不是很美观，就想着在新标签页显示结果。

①通过url传递搜索关键词到新标签页，html代码：

```html
<header id="header" class="header bg-white">
    <div class="navbar-container">
        <!--<a href="www.webname.com" class="navbar-logo">
            网站名称
        </a>-->
        <div class="navbar-menu">
            <a href="www.baidu.com" target="_blank">百度</a>
        </div>
        <div class="navbar-search" onclick="">
            <span class="icon-search"></span>
            <form id="search" method="get" action="/search" role="search" target="_blank">
                <span class="search-box">
                    <input type="text" id="input" class="input" name="keyword" required="true" placeholder="Search..."
                        maxlength="30" autocomplete="off">
                </span>
            </form>
        </div>
    </div>
</header>
```

> 核心代码就是<form></form>的内容


css样式，可以自定义或直接搬用（也不是我自己写的样式，凑合用吧）：

```css
@charset "UTF-8";*{-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box;margin:0;padding:0;border:0;-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale}*,:after,:before{-webkit-box-sizing:border-box;-moz-box-sizing:border-box;box-sizing:border-box}html{overflow-x:hidden;-ms-text-size-adjust:100%;-webkit-text-size-adjust:100%;scroll-behavior: smooth;}::-moz-selection{color:#fff;background-color:#eb5055}::selection{color:#fff;background-color:#eb5055}
a:first-child h1,a:first-child h2,a:first-child h3,a:first-child h4,a:first-child h5,a:first-child h6{margin-top:0;padding-top:0}a{text-decoration:none;color:#5a5a5a;outline:0;}a:active,a:focus,a:hover{color:#eb5055;outline:0}    
button,input,select,textarea{font-family:-apple-system,SF UI Text,Arial,PingFang SC,Hiragino Sans GB,Microsoft YaHei,WenQuanYi Micro Hei,sans-serif;font-size:13px;line-height:1.6;resize:none}input:focus:invalid,input:required:invalid,textarea:focus:invalid,textarea:required:invalid{box-shadow:none}input::-webkit-input-placeholder,textarea::-webkit-input-placeholder{color:#5f5f5f}input:-moz-placeholder,textarea:-moz-placeholder{color:#5f5f5f}input::-moz-placeholder,textarea::-moz-placeholder{color:#5f5f5f}input:-ms-input-placeholder,textarea:-ms-input-placeholder{color:#5f5f5f}
.icon-search{position:relative;z-index:1;display:inline-block;width:13px;height:13px;margin:2px 0 0 3px;-webkit-transform:rotate(-45deg);-ms-transform:rotate(-45deg);transform:rotate(-45deg);color:#313131;border:solid 2px currentColor;border-radius:50%}.icon-search:before{position:absolute;top:11px;left:3px;width:2px;height:4px;content:'';background-color:currentColor}
.icon-menu{position:relative;display:inline-block;width:20px;height:12px;-webkit-transition:all .4s ease-in-out;transition:all .4s ease-in-out;-webkit-transition-timing-function:cubic-bezier(.61,.04,.17,1.32);transition-timing-function:cubic-bezier(.61,.04,.17,1.32)}.icon-menu .middle{position:absolute;top:50%;left:-.25em;display:inline-block;width:20px;height:2px;margin-top:-1px;-webkit-transition:all .4s ease-in-out;transition:all .4s ease-in-out;background:#313131}.icon-menu:after,.icon-menu:before{position:absolute;left:-.25em;width:20px;height:2px;content:'';-webkit-transition:all .4s ease-in-out;transition:all .4s ease-in-out;-webkit-transform-origin:50% 50% 0;-ms-transform-origin:50% 50% 0;transform-origin:50% 50% 0;background:#313131}.icon-menu:after{bottom:0}.icon-menu:before{top:0}.bg-ico-book{background-position:0 0!important}.bg-ico-game{background-position:0 -40px!important}.bg-ico-note{background-position:0 -80px!important}.bg-ico-chat{background-position:0 -120px!important}.bg-ico-code{background-position:0 -160px!important}.bg-ico-image{background-position:0 -200px!important}.bg-ico-web{background-position:0 -240px!important}.bg-ico-link{background-position:0 -280px!important}.bg-ico-design{background-position:0 -320px!important}.bg-ico-lock{background-position:0 -360px!important}
.header{line-height:68px;position:fixed;z-index:10;top:0;display:block;width:100%;height:70px;padding:0;text-align:right;-webkit-box-shadow:0 1px 5px rgba(0,0,0,.1);-moz-box-shadow:0 1px 5px rgba(0,0,0,.1);box-shadow:0 1px 5px rgba(0,0,0,.1)}.header.animated{-webkit-animation-duration:.5s;animation-duration:.5s;-webkit-animation-fill-mode:both;animation-fill-mode:both}.header.animated.slideUp{-webkit-animation-name:slideUp;animation-name:slideUp}.header.animated.slideDown{-webkit-animation-name:slideDown;animation-name:slideDown}
.navbar-container{position:relative;width:1040px;max-width:100%;height:70px;margin:0 auto}.navbar-logo{font-size:22px;line-height:22px;position:absolute;top:50%;left:0;display:block;width:auto;max-width:50%;height:32px;margin-top:-28px;margin-left:25px;text-decoration:none}.navbar-logo img{width:auto;height:56px;outline:0}.navbar-menu{z-index:10;display:inline-block;width:auto;padding-right:5px}.navbar-menu a{padding:0 15px;font-size:15px}.navbar-menu a.current{color:#eb5055}.navbar-mobile-menu{line-height:70px;z-index:1;display:none;width:28px;padding:0 45px 0 10px;cursor:pointer}.navbar-mobile-menu:active,.navbar-mobile-menu:hover{cursor:pointer}.navbar-mobile-menu:active:before,.navbar-mobile-menu:hover:before{-webkit-animation:pointer-ball .3s ease 1;animation:pointer-ball .3s ease 1;-webkit-animation-timing-function:cubic-bezier(.61,.04,.17,1.32);animation-timing-function:cubic-bezier(.61,.04,.17,1.32)}.navbar-mobile-menu:active .icon-menu,.navbar-mobile-menu:hover .icon-menu{-webkit-transform:rotateZ(360deg);-ms-transform:rotateZ(360deg);transform:rotateZ(360deg)}.navbar-mobile-menu:active .icon-menu.cross .middle,.navbar-mobile-menu:active .icon-menu.cross:after,.navbar-mobile-menu:active .icon-menu.cross:before,.navbar-mobile-menu:hover .icon-menu.cross .middle,.navbar-mobile-menu:hover .icon-menu.cross:after,.navbar-mobile-menu:hover .icon-menu.cross:before{background:#eb5055}.navbar-mobile-menu:active .icon-menu.cross .middle,.navbar-mobile-menu:hover .icon-menu.cross .middle{opacity:0}.navbar-mobile-menu:active .icon-menu.cross:after,.navbar-mobile-menu:hover .icon-menu.cross:after{bottom:5px;-webkit-transform:rotate(135deg);-ms-transform:rotate(135deg);transform:rotate(135deg)}.navbar-mobile-menu:active .icon-menu.cross:before,.navbar-mobile-menu:hover .icon-menu.cross:before{top:5px;-webkit-transform:rotate(45deg);-ms-transform:rotate(45deg);transform:rotate(45deg);-webkit-box-shadow:0 0 0 #fff;box-shadow:0 0 0 #fff}.navbar-mobile-menu li{position:relative;display:inline;margin:0;text-decoration:none}.navbar-mobile-menu ul{position:absolute;z-index:1;top:100%;overflow:hidden;clip:rect(1px,1px,1px,1px);margin:0 0 0 -95px;padding:0;-webkit-transition:-webkit-transform .3s;transition:-webkit-transform .3s;transition:transform .3s;-webkit-transform:translate(120px,0);-ms-transform:translate(120px,0);transform:translate(120px,0);text-indent:0}.navbar-mobile-menu:active>ul,.navbar-mobile-menu:focus>ul,.navbar-mobile-menu:hover>ul{overflow:inherit;clip:inherit;-webkit-transition:-webkit-transform .3s;transition:-webkit-transform .3s;transition:transform .3s;-webkit-transform:translateX(0) translateY(0) translateZ(0);transform:translateX(0) translateY(0) translateZ(0)}.navbar-mobile-menu ul li a{font-size:15px;line-height:2.2;display:block;width:140px;margin:0;padding:8px 25px;background-color:#eee}.navbar-search{line-height:70px;display:inline-block;width:20px;padding:0 40px 0 0;cursor:pointer}.navbar-search:active>form,.navbar-search:focus>form,.navbar-search:hover>form{overflow:inherit;clip:inherit;-webkit-transition:opacity .5s ease-in-out;transition:opacity .5s ease-in-out;opacity:1}.navbar-search:active>.icon-search,.navbar-search:focus>.icon-search,.navbar-search:hover>.icon-search{color:#eb5055}.navbar-search form{line-height:30px;position:absolute;top:50%;right:0;display:block;overflow:hidden;clip:rect(1px,1px,1px,1px);width:auto;max-width:60%;height:30px;margin-top:-15px;padding-right:10px;opacity:0}.navbar-search form .search-box{line-height:30px;position:relative;top:-1px;display:inline-block;width:400px;max-width:100%;height:30px;padding:0;border:none;border-radius:3px}.navbar-search form .search-box input{font-size:14px;line-height:30px;position:absolute;top:0;left:0;width:100%;height:30px;padding:0 40px 0 18px;color:#313131;border:1px solid #eb5055;border-radius:20px;outline:0;background-color:#fff;-webkit-appearance:none}
```



效果如下图：

![](https://raw.githubusercontent.com/Ning-Qie/github_image/master/images/20201109225419.png)

当往输入框输入关键词，按下回车后，创建新标签页，且新标签页url为

```
网站地址/search?keyword=关键词
```

> 记得创建search.html，不然链接不到，把步骤2 **安装** 两段代码放在其中
>
> jekyll的自定义链接为`/search`，例如我的
>
> `permalink: /search`

②获取关键词，并支持中文内容。

```javascript
//获取url中"?"符后的字符串并正则匹配
function GetQueryString(name) {
        var reg = new RegExp("(^|&)" + name + "=([^&]*)(&|$)", "i");
        var r = window.location.search.substr(1).match(reg); //获取url中"?"符后的字符串并正则匹配
        var context = "";
        if (r != null)
            context = r[2];
        reg = null;
        r = null;
        return context == null || context == "" || context == "undefined" ? "" : context;
    }
```

因为中文的url直接搜索不到，所以需要配合转换utf-8使用：

```javascript
//将URL中的UTF-8字符串转成中文字符串
function getCharFromUtf8(str) {
    var cstr = "";
    var nOffset = 0;
    if (str == "")
        return "";
    str = str.toLowerCase();
    nOffset = str.indexOf("%e");
    if (nOffset == -1)
        return str;
    while (nOffset != -1) {
        cstr += str.substr(0, nOffset);
        str = str.substr(nOffset, str.length - nOffset);
        if (str == "" || str.length < 9)
            return cstr;
        cstr += utf8ToChar(str.substr(0, 9));
        str = str.substr(9, str.length - 9);
        nOffset = str.indexOf("%e");
    }
    return cstr + str;
}
```

③然后赋值给input框，并激活搜索反馈。

通过查看`simple-jekyll-search.min.js`内容，可知，搜索功能是通过监听search-input搜索框反馈结果。

```javascript
m.searchInput.addEventListener("input",function (t) {...})
```

只改变value值无法激活`oninput`监听反馈，所以需要配合下列代码激活监听：

```javascript
var evt = document.createEvent('HTMLEvents')
evt.initEvent('input', true, true)
$("#tipinput").get(0).dispatchEvent(evt)
```

归纳后得：

```javascript
//传递到search-input搜索框
function saerch_change() {
    var input = document.getElementById("search-input");
    input.value = getCharFromUtf8(GetQueryString("keyword"));
    var evt = document.createEvent('HTMLEvents');
    evt.initEvent('input', true, true);
    input.dispatchEvent(evt);
    //console.log('saerch_change！')
}
```

再为search_cahnge设置一个定时器就好了：

```javascript
const id = setInterval(saerch_change, 500);
setTimeout(() => {
    clearInterval(id);
    //console.log('我结束定时器了！')
}, 4500);
```

为了让新标签页不显示搜索框，还需要样式：

```css
#search-input {
    display: none;
}
```

这就是效果了（自定义样式哦）：

![](https://raw.githubusercontent.com/Ning-Qie/github_image/master/images/20201109234847.png)

![](https://raw.githubusercontent.com/Ning-Qie/github_image/master/images/20201109234937.png)



### 无结果时显示搜索框，有结果不显示

①修改**新标签显示**步骤③的定时器更新频率

```javascript
const id = setInterval(saerch_change, 500);
setTimeout(() => {
    clearInterval(id);
    //console.log('我结束定时器了！')
}, 4500);
```

为：

```javascript
const id = setInterval(saerch_change, 10);
setTimeout(() => {
    clearInterval(id);
    //console.log('我结束定时器了！')
}, 1000);
```

②获取搜索结果个数

```javascript
    var s_r=0;
    function get_sr() {
        s_r = document.getElementById("results-container").getElementsByTagName("li").length;
        console.log(s_r);
    }
    const id_sr = setInterval(get_sr, 10);
    setTimeout(() => {
        clearInterval(id_sr);
        console.log('我结束定时器id_sr了！')
    }, 1000);
```

③判断是否有结果，并执行相应操作

```javascript
    function vvv() {
        if (s_r == 0) {
            console.log("no found!");
            document.getElementById("search-input").style.display = "block";
            //document.getElementById("search-tittle").innerHTML = "Search :";
        }
        else{
            document.getElementById("search-input").style.display = "none";
            clearInterval(id);
            console.log('2:我结束定时器id了！')
        }
    }
    const id_vvv = setInterval(vvv, 10);
    setTimeout(() => {
        clearInterval(id_vvv);
        console.log('我结束定时器id_vvv了！')
    }, 1000);
```

④当重新搜索时，改变搜索标题

```javascript
    setTimeout(() => {
        document.getElementById("search-input").addEventListener("input", function () {
        clearInterval(id);
        document.getElementById("search-tittle").innerHTML = "Search :";
    });
    }, 1000);
```

效果：

![](https://raw.githubusercontent.com/Ning-Qie/github_image/master/images/20201110004334.png)

![](https://raw.githubusercontent.com/Ning-Qie/github_image/master/images/20201109234847.png)



### 全文搜索

**①修改`search.json`文件**

因为liquid语句会自动执行，所以请自提内容：<a href="https://github.com/Ning-Qie/Ning-Qie.github.io/blob/master/search.json" target="_blank">search.json</a>

主要就是增加了post.content内容，然后把非法字符移除。

> `| replace: "a","b"`的功能是用b内容替换a内容，此项是为了替换掉非法字符



这样就可以扩展到全文搜索了，如果出现问题请打开生成的`_site`文件内找`search.json`查看有什么非法字符，添加到规则就行。

<br>

<br>

---

> 参考资料：
>
> 1.<a href='https://github.com/christian-fei/Simple-Jekyll-Search' target="_blank">Github：Simple-Jekyll-Search</a>
>
> 2.<a href='https://cloud.tencent.com/developer/article/1119290' target="_blank">Jekyll 静态博客实现搜索功能</a>
>
> 3.<a href='https://blog.csdn.net/weixin_45414738/article/details/107844539' target="_blank">原生js触发oninput事件</a>
>
> 4.<a href='https://blog.csdn.net/wuxuanecios/article/details/89146773' target="_blank">为Jekyll+GitHub Pages添加全文搜索功能</a>