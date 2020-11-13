---
layout: post
title: 通过浏览器开发者工具获取Bilibili视频av号
date: 2020-06-23 16:44:51 +0800
updatetime: 2020/11/12 下午6:11:23
category: Something
thumbnail: https://ning-blog-1304206373.cos.ap-nanjing.myqcloud.com/image/thumbnail/311dfa441ee3658a07b6ccd927662644.jpg
icon: web
summary: 获取B站视频av号
tag: [Bilibili,B站]
---


视频：[bilibili十周年纪念影片《干杯》](https://www.bilibili.com/video/BV1h441137Uc?t=312)

![image.png](https://raw.githubusercontent.com/Ning-Qie/Ning-Qie.github.io/master/ning_file/image/image-3b22e87161e34736a031175f95db3159.png)

按下<font face="黑体" color=#B0C4DE size=5> F12 </font>打开开发者工具

![image.png](https://raw.githubusercontent.com/Ning-Qie/Ning-Qie.github.io/master/ning_file/image/image-7f2ced6c5e7040c184a68970d0f51e6d.png)

输入以下命令即可得到该视频的av号
```language
window.aid
```

![image.png](https://raw.githubusercontent.com/Ning-Qie/Ning-Qie.github.io/master/ning_file/image/image-9cf5b88ef5e440cd8ca8bd7fa365e0bd.png)


<div style="position: relative; padding: 30% 45%;">
    <iframe style="position: absolute; width: 100%; height: 100%; left: 0; top: 0;"
        src="//player.bilibili.com/player.html?aid=56315372&as_wide=1&danmaku=1" scrolling="no" border="0"
        frameborder="no" framespacing="0" allowfullscreen="true"></iframe>
</div>
