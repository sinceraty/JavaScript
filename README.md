# JavaScript
JavaScript知识点
## 变量声明提升
所有声明变量或声明函数都会被提升到当前函数的顶部<br>
函数表达式：var getName = function(){}<br>
声明函数：function getName()

## call apply 

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
