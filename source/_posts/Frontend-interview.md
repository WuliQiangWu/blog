---
title: Frontend-interview
date: 2019-04-22 11:44:04
tags:
- 前端面试
categories:
- 技术类
- JavaScript
description: 前端面试汇总(仅供个人面试之前复习参考)
---
### es6、7、8新特性
#### es6
> #### 1、let,const.
##### 1.块级作用域：见到这种变量首先想到的就是es6新添了一种作用域，块级作用域。而生效过程即使在有let和const存在就会有会计作用域，顾名思义就是在大括号里有作用域，即for，if等语句中也存在作用域
##### 2.不存在变量提升：传统的会有变量提升，先`var v1;console.log(v1);v1=2;`let不存在变量提升就会报错
##### 3.统一块级作用域不允许重复声明 
##### 4.const声明时必须赋值 ，否则报错，且const a=引用类型数据时改变不会报错。
##### 5.暂时性死区：一个块级作用域内部如果定义了局部变量，就不会使用外部变量。

> #### 2、结构赋值，其实很简单，就是一一对应对象或数组中的值进行赋值

    let [a, [[b], c]] = [1, [[2], 3]];
    console.log(a, b, c); // 1, 2, 3
     
    let [ , , c] = [1, 2, 3];
    console.log(c);       // 3
     
    let [a, , c] = [1, 2, 3];
    console.log(a,c);     // 1, 3
     
    let [a, ...b] = [1, 2, 3];
    console.log(a,b);     // 1, [2,3]
     
    let [a, b, ..c.] = [1];
    console.log(a, b, c); // 1, undefined, []
    
> #### 3、字符串函数的拓展
##### 1.includes():返回布尔值，表示是否找到参数字符串。
##### 2.startsWith():返回布尔值，表示参数字符串是否在原字符串的头部。
##### 3.endsWith():返回布尔值，表示参数字符串是否在原字符串的尾部。
##### 4.repeat方法返回一个新字符串，表示将原字符串重复n次。
> #### 4、模板字符串
    
    let str = 'template str'
    let a = `${str}`
    console.log(a)  // template str
> #### 5、箭头函数

    let fn = ()=> x
    fn(a)
    
    function fn(){
        return x
    }
    fn(a)
##### 箭头函数相比较于function定义的函数简洁了许多，而且它的this指向的是上下文的this，没有自己的this,也没有自己的原型链，它内部的this是不会修改的。
##### 箭头函数没有arguments对象，可以使用rest 代替，它的参数搭配是个数组,将该变量的参数都放入数组中。 具体用法`...变量名`
    
    function push(array, ...items) {
    		items.forEach(function(item) {
    			array.push(item);
    			console.log(item);
    		});
    	}
     
    	var a = [];
    	push(a, 1, 2, 3)
> #### 6、Array.from方法
##### 用于将类似数组的对象或可遍历的对象转为真正的数组
> #### 7、函数参数可以添加默认值 
    
    function log(a, b = 'World') {
        console.log(a, b);
    }
     
    log('Hello') 		    // Hello World
    log('Hello', 'js') 		// Hello js
    log('Hello', '') 		// Hello
    log('Hello', undefined) // Hello World
>#### 8、ES6 提供了新的数据结构 Set。它类似于数组，但是成员的值都是唯一的
>#### 9、map结构
>#### 10、generators生成器函数，现在已经逐渐被async函数取代
##### 1. 与普通函数相比，生成器函数需要在 关键字function 和 函数名之间，加一个星号，星号前后是否写空格无所谓，带有星号的函数，就是生成器函数，即 Generator函数。
##### 2. yield类似return，中文为产出的意思，表示程序执行到 yield时，程序暂停，产出一个结果，这个结果为对象 {value: 值， done：[false| true]}，value指的就是yield后面跟的值，done指程序是否已经完成。
##### 3. 生成器函数本身只返回一个对象，这个对象具有next方法，执行next方法时，函数体内容
    
        Generator 函数，它最大的特点，就是可以交出函数的执行权，即暂停执行。
         
        	function * fn(num1, num2){
        		var a = num1 + num2;
        		yield a;
        		var b = a - num2;
        		yield b;
        		var c = b * num2;
        		yield c;
        		var d = c / num2;
        		yield d;
        	} 
        	var f = fn(1, 2);
        	console.log( f.next() );		//	{value:3, done:false}
        	console.log( f.next() );		//	{value:1, done:false}
        	console.log( f.next() );		//	{value:2, done:false}
        	console.log( f.next() );		//	{value:1, done:false}
        	console.log( f.next() );		//	{value:undefined, done:true}

#### es7
>#### 1、Array.prototype.includes()方法
##### 类似str.includes()方法，判断数组内是否含有某个元素返回boolean值
>#### 2、指数操作符(**)
    
    let a = 2 ** 2 ; // 9
    // 等效于
    Math.pow(2, 2);  // 9
    
    let a = 2 ** 3 ; // 9
        // 等效于
    Math.pow(2, 3);  // 9

#### es8
>#### async await
##### 其实它就是Generator函数的语法糖。
##### async函数使用起来，只要把Generator函数的**（*）**号换成async，yield换成await即可
##### await 只能等待 promise 函数

      捕获异常
      async func1(){
        try{
           let data = awati func2()     
              
          }catch(e){
            console.error(e)
          }
      }
      
      
##### 

>#### 2、Object原型上加了一些方法，Object.keys()，Object.values()，Object.entries()

    
    var a = { f1: 'hi', f2: 'leo'};
    Object.keys(a); // ['f1', 'f2']
    
    let a = { f1: 'hi', f2: 'leo'};//如果不是对象返回空数组
    Object.values(a); // ['hi', 'leo']
    
    let a = { f1: 'hi', f2: 'leo'};
    Object.entries(a); // [['f1','hi'], ['f2', 'leo']]

>#### 3、字符串填充 padStart()和padEnd()
##### 用来为字符串填充特定字符串，并且都有两个参数：字符串目标长度和填充字段，第二个参数可选，默认空格。
    
    'es8'.padStart(2);          // 'es8'
    'es8'.padStart(5);          // '  es8'
    'es8'.padStart(6, 'woof');  // 'wooes8'
    'es8'.padStart(14, 'wow');  // 'wowwowwowwoes8'
    'es8'.padStart(7, '0');     // '0000es8'
     
    'es8'.padEnd(2);            // 'es8'
    'es8'.padEnd(5);            // 'es8  '
    'es8'.padEnd(6, 'woof');    // 'es8woo'
    'es8'.padEnd(14, 'wow');    // 'es8wowwowwowwo'
    'es8'.padEnd(7, '6');       // 'es86666'
    从上面结果来看，填充函数只有在字符长度小于目标长度时才有效，若字符长度
    已经等于或小于目标长度时，填充字符不会起作用，而且目标长度如果小于字符
    串本身长度时，字符串也不会做截断处理，只会原样输出。
    
>### import、require 的区别 和 module.export 、export、 export default、exports的区别和联系
    
##### import 和require的区别
    
    import 是 es6 提供的一个导入模块的方式
    require 是 CommonJS 提供的导入方式
    
    module.exports 和 exports 是 CommonJS 提供的导出方法
    其实exports变量是指向module.exports，加载模块实际是加载该模块的module.exports.
    不可以给exports 赋值，否则会改变 exports 指向
    
    
    //example.js
    var x = 5;
    var addX = function (value) {
      return value + x;
    };
    module.exports.x = x;
    module.exports.addX = addX;
    
    require.js //
    var example = require('./example.js');
    console.log(example.x); // 5
    console.log(example.addX(1));
    
    
    export 和export default 是es6 提供的导出方法
    
    export其实和export default就是写法上面有点差别，一个是导出一个个单独接口，一个是默认导出一个整体接口。
    使用import命令的时候，用户需要知道所要加载的变量名或函数名，否则无法加载。
    这里就有一个简单写法不用去知道有哪些具体的暴露接口名，就用export default命令，为模块指定默认输出。
    
    //example.js -- export 
    var f = 'Miel';
    var name = 'Jack';
    var data= 1988;
    export {f, name, data};
    // import.js 
    // main.js
    import {f, name, data} from './testA';
    引入的时候使用解构赋值的方法 放在大括号里引用
    
    //example.js -- export default
    export default function () {
      console.log('foo');
    }
    // 或者写成
    
    function foo() {
      console.log('foo');
    }
    export default foo;
    
    
    // import-default.js
    导出一个整体的接口，返回的就是一个函数，可以自己命名调用
    import customName from './export-default';
    customName(); // 'foo'

>### 前端的垃圾回收机制
    
##### 1、标记清除
###### 这是JavaScript最常见的垃圾回收方式。当变量进入执行环境的时候，比如在函数中声明一个变量，垃圾回收器将其标记为“进入环境”。当变量离开环境的时候（函数执行结束），将其标记为“离开环境”。垃圾回收器会在运行的时候给存储在内存中的所有变量加上标记，然后去掉环境中的变量，以及被环境中变量所引用的变量（闭包）的标记。在完成这些之后仍然存在的标记就是要删除的变量。

##### 2、引用计数
###### 引用计数的策略是跟踪记录每个值被使用的次数。当声明了一个变量并将一个引用类型赋值给该变量的时候，这个值的引用次数就加1。如果该变量的值变成了另外一个，则这个值的引用次数减1。当这个值的引用次数变为0的时候，说明没有变量在使用，这个值没法被访问。

>### 几种DOM类型的节点
##### 整个html文档是一个文档节点
##### 每个HTML标签是一个元素节点
##### 每一个HTML属性是一个属性节点
##### 包含在html中的文本是一个文本节点


>### 说说你对闭包的理解
##### 闭包的形成是在一个函数中return 了一个函数出来。
##### 它可以在函数内部调用外部的变量。它也可以避免全局变量污染
##### 但是它会将变量一直保存在内存中，不会根据JS的垃圾回收机制被销毁，容易造成内存的泄漏
##### 特征如下
######  函数嵌套函数
######  在函数内部可以引用外部的变量
######  变量和参数不会参与垃圾回收机制

>### 在JavaScript中使用innerHTML的弊端
#####  通过innerHtml改变的内容更新每次都会刷新页面，效率很低，而且innerHtml 没有验证机制，如果在文档中插入错误的代码很容易造成一些未知的错误，使页面变得不稳定。

>### 如何在不支持JavaScript的旧浏览器中隐藏JavaScript代码?
##### 在标签之前添加  // -->，代码中没有引号。
##### 旧浏览器现在将JavaScript代码视为一个长的HTML注释，而支持JavaScript的浏览器则将“”作为一行注释。

>### DOM操作中怎样创建、添加、移除、替换、插入节点
##### document.createDocumentFragment 创建DOM片段
##### document.createElement 创建一个元素
##### document.createTextNode  创建一个文本节点
