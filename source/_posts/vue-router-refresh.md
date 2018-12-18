---
title: VUE路由改变页面却不刷新的问题
date: 2018-12-03 17:05:02
tags: 
- vue
- vue-router
categories:
- 技术类
- vue
description: VUE路由改变页面却不刷新
---

#### 在vue-cli 环境开发中，碰到了个问题，就是在详情页面里面，点击推荐游戏或者书籍，跳转到当前页面，会发现路由改变，但是页面还会之前上一个游戏或者书籍的内容。
##### 1.路由介绍
      vue-router是Vue.js官方的路由插件，它和vue.js是深度集成的，适合用于构建单页面应用。
      vue的单页面应用是基于路由和组件的，路由用于设定访问路径，并将路径和组件映射起来。
      可以发现，vue-router的切换不同于传统的页面的切换。
      相比于传统页面使用一些超链接来实现页面切换和跳转，路由之间的切换，其实就是组件之间的切换，不是真正的页面切换。
      这也会导致一个问题，就是引用相同组件的时候，会导致该组件无法更新，也就是我们口中的页面无法更新的问题了。
##### 2.路由刷新
###### vue-router在处理不同组件之间的跳转的时候，就会刷新页面，然后重新加载。
###### 在处理相同路由的时候，我们可以一下做法
>  1. router-view有一个key属性来识别vue-router是不是同一个，知道这个就好办了。
    我们可以给router-view加一个变化的值，比如说当前时间加一个随机数，或者就是当前时间的一个字符串，
    当路由刷新的时候，这个key会被监听到做了改变，就会刷新页面
    
        <router-view :key="changeKey" />
        
        computed:{
                changeKey(){
                    return new Date().toString();
                }
            }

>  2. 当路由变化的时候，我们首先把 router-view 组件给销毁掉，其实就是使用 v-if ，然后再次创建router-view，也可以达到刷新路由的目的。
        
          <router-view  v-if="routerShow" />
                  
          data(){
                return{
                  routerShow:true
                }
          },
          watch: {
            '$route': function (to, from) {  #监听路由参数变化
              this.routerRefresh()
            }
          },
          methods:{
            routerRefresh(){
              this.routerShow = false;
              this.$nextTick(()=>{  #Vue 实现响应式并不是数据发生变化之后 DOM 立即变化，而是按一定的策略进行 DOM 的更新。
                                    $nextTick 是在下次 DOM 更新循环结束之后执行延迟回调，在修改数据之后使用 $nextTick，则可以在回调中获取更新后的 DOM.
                this.routerShow = true;
              });
            }
          }
                            