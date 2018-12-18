---
title: VUE踩坑总结
date: 2018-12-04 15:27:48
tags: 
- vue
categories:
- 技术类
- vue
description: VUE踩坑总结
---
#### 在 vue-cli 环境开发中，多人合作一个项目，在pull代码重新执行的时候会遇到报错
    npm ERR! code ETARGET
    npm ERR! notarget No matching version found for har-validator@5.1.2
    npm ERR! notarget In most cases you or one of your dependencies are requesting
    npm ERR! notarget a package version that doesn't exist.
    npm ERR! notarget
    npm ERR! notarget It was specified as a dependency of 'project'
    npm ERR! notarget

    npm ERR! A complete log of this run can be found in:
    npm ERR!     /Users/user/.npm/_logs/2018-12-04T07_14_54_875Z-debug.log
    
>  此时我们需要把项目里的 `package-lock.json` 删除掉，然后重新执行 `npm install ` 即可

#### 用vue-cli 开发的时候，我们通常会引入一些包，有些包会很大，比如所你使用的ui库，像element-ui,mint-ui等，如果直接使用webpack打包的话，打包出来的wendor.js会非常大，对服务器压力等方面会造成一些影响，此时就可以使用webopack提供的`externals`来指明那些不用webpack打包,但是我们要在cdn上把这些包引入
>在webpack.base.conf.js文件中

    module.exports = {
      context: path.resolve(__dirname, '../'),
      entry: {
        app: './src/main.js'
      },
      externals: {
        'vue': 'Vue',
        'vue-router': 'VueRouter', 
        'mint-ui': 'mint-ui'            # mint-ui为例
        'element-ui': 'ELEMENT'         # element-ui为例
      },
      ****
    }
>如果这样配置的话，在`main.js` 里面就要去掉 `Vue.use(xxx)` 这种用法，不然会报 `module undefeated` 这类错误

> 在index.html里面

    <html>
      <head>
        <meta charset="utf-8">
        <meta name="viewport">
        <script src="https://cdn.jsdelivr.net/npm/vue@2.5.17/dist/vue.min.js"></script>
        <script src="https://cdn.jsdelivr.net/npm/vue-router@3.0.2/dist/vue-router.min.js"></script>
        <script src="https://cdn.jsdelivr.net/npm/mint-ui@2.2.13/lib/index.js"></script>
        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/mint-ui@2.2.13/lib/style.min.css">
        ******
      </head>
    </html>
> 注意 Vue.js引入一定要在最上面，不然会报 `prototype undefined` 这类错误