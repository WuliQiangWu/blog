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