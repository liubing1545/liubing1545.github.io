---
title: 认识javascript
date: 2016-08-08 17:08:33
tags: [js, 闭包, 作用域, 上下文]
---
## javascript基础

#### 声明
* 声明变量不用var时，该变量为全局变量

#### 数据类型
* Number  
 * *整型常量*(10进制\8进制\16进制)  
 	 十六进制以0x或0X开头, 例如: 0x8a  
    八进制必须以0开头, 例如: 0123  
    十进制的第一位不能是0(数字0除外), 例如: 123
 * *实型常量*  
   12.32, 193.98, 5E7, 4e5等
  
* Boolean
* String  
 * "abc"，'abc'  
   双引号会搜索引号内的内容是否含有变量，有则输出其值，没有则输出原有内容  
   单引号则不会检测内容，因此效率更高
 * 特殊字符，需要以反斜杠(\)后跟一个普通字符来表示  
   例如: \r, \n, \t, \b, \'
* null常量
* undefined常量
* 特殊数值  
  NaN, Infinity(无穷大), isNaN(), isFinite()

#### 逻辑运算符
* && 逻辑与
* || 逻辑或
* ! 逻辑非

## 定义一个类
    function Person(name) {
    	this.name = name;
    }
## 一切都是对象
类(函数)也是对象

## 创建对象
    var p = new Person("张三")

## 闭包closure
函数内部可以直接读取全局变量。  
     
        var n = 999;
        
        function f1() {
        	alert(n);
        }
        
        f1(); // 999
函数外部无法读取函数内的局部变量。  
        
        function f1() {
        	var n = 999;
        }
        
        alert(n); // error
利用闭包，从函数外部读取函数内的局部变量。

        function f1() {
        	var n = 999;
        	
        	function f2() {
        		alert(n);
        	}
        	
        	return f2;
        }
        
        var result = f1();
        result(); // 999

## 作用域scope（上下文）
上下文对象就是使用this指针，即被调用函数所处的环境。上下文对象在一个函数内部引用调用它的对象本身。  

    var someuser = {
    	name: 'byvoid',
    	func: function() {
    		console.log(this.name);
    	}
    };
    
    var foo = {
    	name: 'foobar'
    };
    
    someuser.func(); // byvoid
    
    foo.func = someuser.func;
    foo.func(); // foobar
    
    name = 'global';
    func = someuser.func;
    func(); // globar

## prototype
利用prototype可以扩展js类。  
    
    Number.prototype.add = function(v) {
    	return this + v;
    }    
    
    var d = 6;
    d.add(8).add(9); // d = 6 + 8 + 9

扩展自定义的类。

    function Person(name) {
    	this.name = name;
    }
    
    Person.prototype.sayHello = function() {
    	alert(this.name);
    }
    
    var p = new Person("zhangsan");
    p.sayHello();

## 继承
classB继承classA

    function classA(name) {
    	this.name = name;
    	this.sayHello = function() {
    		alert(this.name);
    	}
    }
方法1
    
    function classB(name) {
    	this.tempMethod = classA;
    	this.tempMethod(name);
    }
方法2

    function classB(name) {
    	classA.call(this, name);
    }
方法3

    function classB(name) {
    	classA.apply(this, [name]);
    }
调用classB

    var b = new classB("lisa");
    b.sayHello();

## 可变参数
在js的世界里，内置属性arguments可以接收可辨参数。

    function sum() {
    	var s = 0;
    	for(var i = 0; i < arguments.length; i++) {
    		s+ = arguments[i];
    	}
    	
    	return s;
    }
    
    alert(sum(1,4,5));
    alert(sum(12,15,19,21,51));


