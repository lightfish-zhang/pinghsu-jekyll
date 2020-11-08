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

先写到这，现在的样式就是修改后的，等有空了再补充。

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