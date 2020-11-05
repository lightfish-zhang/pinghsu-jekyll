---

layout: post
title: 本地安装jekyll环境实现静态网页运行
date: 2020-11-05 22:03:00 +0800
category: Code
thumbnail: /style/image/stephanie-harlacher-jMGnXrHYDv0-unsplash.jpg
icon: code
summary: 为了更方便的测试github.io样式，安装jekyll从而实现本地静态网页的显示。

---

* content
{:toc}

# 序

通过github.io搭建了一个静态网页，但由于github page更新与转译太过繁琐，于是便在自己的电脑上配置jekyll。



# 1.安装Ruby（With DevKit）

下载地址：<a href="https://rubyinstaller.org/downloads/" target="_blank">Ruby官网下载</a>

![image.png](https://raw.githubusercontent.com/Ning-Qie/Ning-Qie.github.io/master/ning_file/image/Snipaste_2020-11-05_22-15-36.jpg)

按照默认选项一路next下去，直到安装完成就可以。（最后finish页面要求你配置的东西可以勾掉）

> 注意：安装路径最好选用默认，Ruby安装路径不可以带空格，例如：Program File

安装完成后，可以在命令行输入ruby -v检测

```
ruby -v
```

出现版本号即安装成功：`ruby 2.7.2p137 (2020-10-01 revision 5445e04352) [x64-mingw32]`



# 2.安装jekyll



**2.1 检测gem**

要使用 gem 安装，首先检测gem是否被安装上：

```
gem -v
```

出现版本号即为正常：`3.1.4`

---



**2.2 为gem换源**

```
gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/

gem sources -l
```



---

**2.3 安装jekyll** 

```
gem install jekyll
```

> 注意：需要等待一些时间，不用着急



---

**2.4  安装jekyll-paginate**

```
gem install jekyll-paginate
```

验证jekyll安装情况：

```
jekyll -v
```



---

**2.5 安装 Bundler**

```
gem install bundler 
```



# 3.本地调试github.io页面

在命令行cd到github.io所在位置，例：

```
cd C:\Users\14254\Documents\Code\github_io\Ning-Qie.github.io
```

然后输入jekyll serve，如果不成功可以尝试bundle exec jekyll serve

```
jekyll serve

bundle exec jekyll serve
```

>  启动成功后，按CTRL + C 终止



然后在浏览器地址栏输入localhost:4000就可以查看自己的网页了。

```
http://localhost:4000/
```



---

> 参考资料：
>
> 1.<a href='https://blog.csdn.net/mouday/article/details/79300135'>windows安装jekyll步骤及问题</a>
>
> 2.<a href='https://www.cnblogs.com/pergrand/p/12875597.html'>Windows 系统上安装 Jekyll（简单详细教程）</a>
>
> 3.<a href='http://www.caotama.com/79399.html'>jekyll更换主题导致jekyll serve无法启动？</a>
>
> 4.<a href='https://docs.github.com/cn/free-pro-team@latest/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll'>GitHub Docs：使用 Jekyll 在本地测试 GitHub Pages 站点</a>

