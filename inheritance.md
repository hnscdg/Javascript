# javascript面向对象中继承实现

面向对象的基本特征有：封闭、继承、多态。

在JavaScript中实现继承的方法：

1. 原型链（prototype chaining）

2. call()/apply()

3. 混合方式(prototype和call()/apply()结合)

4. 对象冒充

继承的方法如下：

## prototype原型链方式

```javascript
function teacher(name){
    this.name = name;
}
teacher.prototype.sayName = function(){
    console.log("name is "+this.name);
}
var teacher1 = new teacher("xiaoming");
teacher1.sayName();

function student(name){
    this.name = name;
}
student.prototype = new teacher()
var student1 = new student("xiaolan");
student1.sayName();
//  name is xiaoming
//  name is xiaolan
```

## call()/apply()方法

```javascript
function teacher(name,age){
    this.name = name;
    this.age = age;
    this.sayhi  = function(){
      alert('name:'+name+",  age:"+age);
   }
}
function student(){
  var args = arguments;
  teacher.call(this,args[0],args[1]);
  // teacher.apply(this,arguments);
}
var teacher1 = new teacher('xiaoming',23);
teacher1.sayhi();

var student1 = new student('xiaolan',12);
student1.sayhi();

// alert: name:xiaoming,  age:23
// alert: name:xiaolan,  age:12
```

## 混合方法 [prototype,call/apply]

```javascript
function teacher(name,age){
   this.name = name;
   this.age = age;
}
teacher.prototype.sayName = function(){
   console.log('name:'+this.name);
}
teacher.prototype.sayAge = function(){
   console.log('age:'+this.age);
}

function student(){
  var args = arguments;
  teacher.call(this,args[0],args[1]);
}
student.prototype = new teacher();

var student1 = new student('xiaolin',23);
student1.sayName();
student1.sayAge();
// name:xiaolin
// age:23
```

## 对象冒充

```javascript
function Person(name,age){
   this.name = name;
   this.age = age;
   this.show = function(){
         console.log(this.name+",  "+this.age);
   }
}

function Student(name,age){ 
   this.student = Person;       //将Person类的构造函数赋值给this.student
   this.student(name,age);   //js中实际上是通过对象冒充来实现继承的
   delete this.student;           //移除对Person的引用
}

var s = new Student("小明",17);
s.show();

var p = new Person("小花",18);
p.show();
// 小明,  17
// 小花,  18
```