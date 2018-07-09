# JavaScript
JavaScript知识点
## 变量声明提升
所有声明变量或声明函数都会被提升到当前函数的顶部<br>
函数表达式：var getName = function(){}<br>
声明函数：function getName()

## 委托函数call apply 

call(obj,params1,param2),apply(obj,argments),作用一样，只是调用的方式不一样。
```JavaScript
var parent = function(){
  this.name='yh';
  this.age=25;
};
var child={};
parent.call(child);
```
在上段代码中child会继承parent的属性，即child对象去执行parent方法，此时this指向child。
## 闭包
闭包就是能够读取其他函数内部变量的函数
```JavaScript
function boolFirst(){
  var list = [];
  return function(id){
    if(list.indexOf(id)>=0){
      return false;
    }else{
      list.push(id);
      return true;
    }
  }
}
var first = boolFirst();
first(1);
first(1);
```
其中var first 等于返回的函数，却能访问到变量list。
##JavaScript精度问题
原因是有些小数以二进制表示它的位数是无穷的，例如0.1用而二进制表示是000110011001...
##Math常用函数
Math.random,生成0~1的随机数<br>
Math.abs绝对值<br>
Math.ceil向上取整<br>
Math.floor向下取整<br>
##数组常用函数
array.map(),根据规则重新生成副本
```JavaScript
var list=['A','B','C','D'];
list.map(function(item,index){
  return item+'你好帅！';
})
```
## 原型
### 实例，原型，构造函数，原型链
* js一切皆是对象，对象都有原型对象，对象默认继承自其原型对象
* 所有的函数都是Function的实例即new Functon()，Function = new Function()
* 对象有隐式原型(_proto_)，函数有显示原型（prototype）<br>
* hasownProperty是Js中唯一处理属性并且不会遍历原型链的方式
* Object.create()，使用原型链的方式去创建对象
```JavaScript 
  var p={name:'p'};
  var o1= Object.create(p);
  o1._proto_===p//true
```

### new创建函数过程
![image](https://github.com/sinceraty/JavaScript/blob/master/new.png)
### 创建对象的方式
```JavaScript
   //字面量创建
   var o1 = {name:'o1'};
   var o2 = new Object({name:'o2'});
   //构造函数创建
   var gen = function(name){
     this.name=name
   };
   var o3 = new gen('o3');
   //原型链方式创建
   var o4 = Object.create({name:'o4'});
 ```
### 继承的方式
* 借助构造函数实现继承
```JavaScript
      function Parent1 () {
          this.name = 'parent1';
      }
      Parent1.prototype.say = function () {
      };
      function Child1 () {
          Parent1.call(this);
          this.type = 'child1';
      }
      console.log(new Child1());
	  //new Child1().say() 报错
```
 
此方法的确定无法继承原型链

* 借助原型链实现继承

```JavaScript
	function Parent2 () {
		  this.name = 'parent2';
          this.play = [1, 2, 3];
      }
      function Child2 () {
          this.type = 'child2';
      }
      Child2.prototype = new Parent2();
      var s1 = new Child2();
      var s2 = new Child2();
      console.log(s1.play, s2.play);
      s1.play.push(4);
```
此方法中s1,s2公用一个原型对象，s1.play.push(4)是往原型对象上增加数值，会共享.

* 组合方式继承
```JavaScript
	function Parent3 () {
	  this.name = 'parent3';
	  this.play = [1, 2, 3];
	}
	function Child3 () {
	  Parent3.call(this);
	  this.type = 'child3';
	}
	Child3.prototype = new Parent3();
	var s3 = new Child3();
	var s4 = new Child3();
	s3.play.push(4);
	console.log(s3.play, s4.play);
```
同时使用2种方式，call方法已经父类的属性继承。原型链也链上了，但是parent的构造函数执行了2次，影响性能。并且分辨s3到底是child创建的还是parent创建的。

* 组合方式优化1
```JavaScript
	function Parent4 () {
	  this.name = 'parent4';
	  this.play = [1, 2, 3];
	}
	function Child4 () {
	  Parent4.call(this);
	  this.type = 'child4';
	}
	Child4.prototype = Parent4.prototype;
	var s5 = new Child4();
	var s6 = new Child4();
	console.log(s5, s6);
	console.log(s5 instanceof Child4, s5 instanceof Parent4);
	console.log(s5.constructor);
```
解决了parent构造函数执行2次的问题，但是没有解决实例到底是谁创建的问题，即s4.__proto__.constructor===parent3

* 组合方式优化2
```JavaScript
	  function Parent5 () {
          this.name = 'parent5';
          this.play = [1, 2, 3];
      }
      function Child5 () {
          Parent5.call(this);
          this.type = 'child5';
      }
      Child5.prototype = Object.create(Parent5.prototype);
	  Child5.prototype.constructor = Child5();
```
使用object.create创建中间原型链，并制定其构造函数，不让其向上查找。
Parent5.prototype.constructor=Parent5,这条链断了？

## dom事件
### dom事件的级别
* dom0 element.onclick=function(){}
* dom2 element.addEventListener('click',function(){},false)
* dom3 element.addEventListener('keyup',function(){},false),事件类型增多
### dom事件模型
冒泡和捕获，一个冲下到上，一个从上到下
### dom事件流

### dom事件捕获的具体流程
* 捕获：window→document→html→body→...
* 冒泡：相反
### Event事件常见应用
* preventDefault 阻止默认事件
* stopPropagation 阻止事件冒泡
* currentTarget 绑定
* target 触发 委托
* stopImmediatePropagation 同一个元素绑定多个事件，可以指定让其执行一个，后面不在执行
### 自定义事件
* 不能进行参数传递
```JavaScript
	var custEvent1=new Event('c1');
	dom.addEventListenner('c1',fun..,false)
	dom.dispatchEvent(custEvent1);	
```
* 传递参数
```JavaScript
	var custEvent2 = new CustEvent('c2'{
		detail:{
			a:1
		}
	});
	dom.addEventListenner('c2',fun..,false)
	dom.dispatchEvent(custEvent2);	
```
## JS运行机制
	
1. 单线程
2. 任务队列(挂起)
3. 同步代码会先执行，同步代码执行完之后再解析异步代码，再把异步代码变为同步代码。

## 页面性能

### 代码压缩，Gzip
1. 代码合并压缩，容器使用Gzip等
### 非核心代码异步加载
1. 动态脚本（js中动态添加标签）
2. defer 引入js语句中加入 defer，多个顺序执行。文档解析完后执行
3. async 引入语句中加入 async,多个执行顺序不一定。
### 浏览器缓存
1. 强缓存
2. 协商缓存
### CDN
### DNS预解析 meta标签中强制开启，link标签中解析域名
```html
<meta http-equiv="x-dns-prefetch-control" content="on" />
<link rel="dns-prefetch" href="http://xxxxx.com" />
```
## 错误监控

### 代码错误
   代码在运行时发生错误<br>

1. try catch
2. window.onerror (dom0,也可以使用dom2方式)
### 资源加载错误
1. performance.getEntries();
2. 捕获方式（冒泡方式获取不到）;
### 错误上报
1. AJAX
2. Image对象，new Image().src="https://www.baidu.com/error?a='error'"