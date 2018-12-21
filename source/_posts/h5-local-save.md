---
title: H5本地存储
date: 2018-12-21 11:30:56
tags: 
- SessionStorage
- LocalStorage
- IndexedDb
categories:
- 技术类
- JavaScript
description: H5三大主流本地存储
---
##### SessionStorage

>设置 SessionStorage 

    window.sessionStorage.setItem(name,value) #name和value 都必须是字符串
    如果想存储json对象，需要JSON.stringify(json)
>获取 SessionStorage 

    let result =  window.sessionStorage.getItem(name) #name
    如果获取的是个json格式的字符串，可以使用 JSON.parse(result)
>移除 SessionStorage 

    window.sessionStorage.removeItem(name) #name 
>清空 SessionStorage 

    window.sessionStorage.clear() #name   
    
##### LocalStorage

>设置 LocalStorage 

    window.localStorage.setItem(name,value) #name和value 都必须是字符串
    如果想存储json对象，需要JSON.stringify(json)
>获取 LocalStorage 

    let result = window.localStorage.getItem(name) #name
    如果获取的是个json格式的字符串，可以使用 JSON.parse(result)

>移除 LocalStorage 

    window.localStorage.removeItem(name) #name 
>清空 LocalStorage 

    window.localStorage.clear() #name  
    
##### IndexedDb
> IndexedDB能够在客户端存储大量的结构化数据，并且使用索引高效检索的API。(说白了就是一个浏览器的数据库)

> 事务

      在对新数据库做任何事情之前，需要开始一个事务。事务中需要指定该事务跨越哪些object store。
      事务具有三种模式
      只读：read，不能修改数据库数据，可以并发执行
      读写：readwrite，可以进行读写操作
      版本变更：verionchange

> 打开一个数据库

    window.indexedDB.open(name,version) 
    # 如果没有这个数据库则会创建一个名为name的数据库
    
    function openDB (name,version) {
                var version=version || 1;
                var request=window.indexedDB.open(name,version);
                request.onerror=function(e){
                    console.log(e.currentTarget.error.message);
                };
                request.onsuccess=function(e){
                    console.log(e.target.result);
                };
                request.onupgradeneeded=function(e){
                    console.log('DB version changed to '+version);
                };
            }
    openDB('test',2);
    

> 删除数据库

    window.indexedDB.deleteDatabase(name)

> 关闭数据库

    var db = null 
    
    function openDB (name,version) {
                    var version=version || 1;
                    var request=window.indexedDB.open(name,version);
                    request.onerror=function(e){
                        console.log(e.currentTarget.error.message);
                    };
                    request.onsuccess=function(e){
                        db = e.target.result;
                    };
                    request.onupgradeneeded=function(e){
                        console.log('DB version changed to '+version);
                    };
                }
    openDB('test',2);
     
    db.close()
> 往数据库添加数据
    
    调用数据库实例的createObjectStore方法可以创建object store，方法有两个参数：store_name和键类型。
    daveDate = [
    {name:"a",age:1},
    {name:"b",age:2},
    {name:"c",age:3}
    ]
    
    function openDB(name,tableName, versions) {
      let version = versions || 1;
      let request = window.indexedDB.open(name, version);
      request.onerror = function (e) {
        console.log(e.currentTarget.error.message);
      };
      request.onsuccess = function (e) {
        let db = e.target.result;
      };
      request.onupgradeneeded = function (e) {
        let db = e.target.result;
        if (!db.objectStoreNames.contains(tableName)) {
          let store = db.createObjectStore(tableName, {keyPath: 'id'});
          for (let i = 0; i < saveDate.length; i++) {
            store.add(that.saveDate[i]);
          }
        }
      };
    }
    
> 查找数据
       
    可以调用object store的get方法通过键获取数据，以使用keyPath做键为例
    
    function getDataByKey(db,storeName,value){
        var transaction=db.transaction(storeName,'readwrite'); 
        var store=transaction.objectStore(storeName); 
        var request=store.get(value); 
        request.onsuccess=function(e){ 
            var student=e.target.result; 
            console.log(student.name); 
        };
    }
> 更新数据
    
    可以调用object store的put方法更新数据，会自动替换键值相同的记录，达到更新目的，没有相同的则添加，以使用keyPath做键为例
    
    function updateDataByKey(db,storeName,value){
                var transaction=db.transaction(storeName,'readwrite'); 
                var store=transaction.objectStore(storeName); 
                var request=store.get(value); 
                request.onsuccess=function(e){ 
                    var student=e.target.result; 
                    student.age=35;
                    store.put(student); 
                };
    }

> 删除数据及object store
        
    调用object store的delete方法根据键值删除记录
        
    function deleteDataByKey(db,storeName,value){
        var transaction=db.transaction(storeName,'readwrite'); 
        var store=transaction.objectStore(storeName); 
        store.delete(value); 
    }
    
    调用object store的clear方法可以清空object store
    
    function clearObjectStore(db,storeName){
        var transaction=db.transaction(storeName,'readwrite'); 
        var store=transaction.objectStore(storeName); 
        store.clear();
    }
    
    调用数据库实例的deleteObjectStore方法可以删除一个object store，这个就得在onupgradeneeded里面调用了
    
    if(db.objectStoreNames.contains('students')){ 
        db.deleteObjectStore('students'); 
    }

[参考地址](https://www.cnblogs.com/dolphinX/p/3415761.html?_blank)