---
title: website-acceleration
date: 2019-06-26 11:22:49
tags: website 前端优化
description: 前端页面加速
---
### 原则
##### 1、PC优化手段在Mobile需要同样适用。
##### 2、在Mobile端我们尽量在三秒种渲染完成首屏指标，或者使用loading 提示用户等待。
##### 3、基于联通3G网络平均338KB/s(2.71Mb/s)，所以首屏加载的资源应该小，不应超过1014KB。
##### 4、Mobile因手机配置原因，除加载外渲染速度也是优化重点，同时也要合理处理代码减少渲染损耗。
##### 5、加载完成后用户交互使用时也需注意性能。

### 手段
##### 一、加载优化
###### 1）减少HTTP请求 （尽量合并CSS、JavaScript,合并小图片，使用雪碧图）
###### 2）使用缓存 （缓存一切可缓存的资源、使用外联式引用CSS、JavaScript）
###### 3）压缩HTML、CSS、JavaScript
###### 4）外部js 资源加载尽量放在页面下方，以防造成阻塞（如果必须的话可以给标签加上defer或者async属性）
###### 5）按需加载 （LazyLoad，滚动截流）
###### 6）压缩图片 （使用svg代替icon-font）
###### 7）减少cookie的使用 （静态资源网站就尽量不要使用cookie）

##### 二、HTML执行优化
###### 1）CSS写在头部，JavaScript写在尾部或异步
###### 2）避免图片和iFrame等的空Src，空Src会重新加载当前页面，影响速度和效率。
###### 3）尽量避免重设图片大小，重设图片大小是指在页面、CSS、JavaScript等中多次重置图片大小，多次重设图片大小会引发图片的多次重绘，影响性能。
###### 4）图片尽量避免使用DataURL，DataURL图片没有使用图片的压缩算法文件会变大，并且要解码后再渲染，加载慢耗时长。
###### 5）HTML使用Viewport。
###### 6）减少Dom节点。
###### 7）动画优化(尽量使用CSS3动画,适当使用Canvas动画5个元素以内使用css动画)。

##### 三、CSS优化
###### 1）尽量避免写在HTML标签中写Style属性。
###### 2）避免CSS表达式。
###### 3）移除空的CSS规则，空的CSS规则增加了CSS文件的大小，且影响CSS树的执行。
###### 4）正确使用Display的属性，Display属性会影响页面的渲染。
           （1）、display:inline后不应该再使用width、height、margin、padding以及float
           （2）、display:inline-block后不应该再使用float
           （3）、display:block后不应该再使用vertical-align
           （4）、display:table-*后不应该再使用margin或者float
###### 5）不滥用Float，Float在渲染时计算量比较大，尽量减少使用。
###### 6）不滥用Web字体，Web字体需要下载，解析，重绘当前页面，尽量减少使用。
###### 7）不声明过多的Font-size，过多的Font-size引发CSS树的效率。
###### 8）值为0时不需要任何单位，为了浏览器的兼容性和性能，值为0时不要带单位。          
###### 9）避免让选择符看起来像正则表达式。          
###### 10）标准化各种浏览器前缀         
           （1）、无前缀应放在最后。
           （2）、CSS动画只用（-webkit- 无前缀）两种即可。
           （3）、其它前缀为“-webkit- -moz- -ms-无前缀”四种（-o-Opera浏览器改用blink内核，所以淘汰）。
           
##### 四、JavaScript执行优化
###### 1）CSS写在头部，JavaScript写在尾部或异步
###### 2）避免图片和iFrame等的空Src，空Src会重新加载当前页面，影响速度和效率。
###### 3）尽量避免重设图片大小，重设图片大小是指在页面、CSS、JavaScript等中多次重置图片大小，多次重设图片大小会引发图片的多次重绘，影响性能。
###### 4）图片尽量避免使用DataURL，DataURL图片没有使用图片的     

### 总结
##### 1、 优化图片:图片要小，推荐使用webp格式的图片，是目前同等清晰度下最小的图片格式了，是pgep的2/3大概
##### 2、 启动缓存技术:把一些资源缓存下来，提高二次以及多次加载速度。
##### 3、 压缩文件:js,css,html 都进行压缩之后再加载，有效减少服务器的压力，提高加载速度。
##### 4、 前端代码尽量简介:页面结构要清晰，删掉冗余代码。
##### 5、 提高服务端性能:现在资源多放在cdn上，可以把项目部署在全球节点或者全国节点多的cdn上，有效提高访问速度，但是不同cdn成本不同，酌情考虑。


[参考链接](https://www.cnblogs.com/sese/p/5129192.html)
