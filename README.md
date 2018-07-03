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
js一切皆是对象，对象都有原型对象，对象默认继承自其原型对象，所有的函数都是Function的实例即new Functon()，Function = new Function()?
对象有隐式原型(_proto_)，函数有显示原型（prototype）<br>
hasownProperty是Js中唯一处理属性并且不会遍历原型链的方式
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
