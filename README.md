# JavaScript
JavaScript应用记录
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
