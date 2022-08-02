# 继承


## 原型链的知识

>只要创建一个函数,就会按照特定的规则为这个函数创建一个prototype属性(指向原型对象),所有的原型对象都会自动获取一个constructor的属性,指向与之关联的构造函数,例如Person.prototype.constructor指向Person。然后因为构造函数而异,可能会给原型对象添加其他属性和方法
>
><font color="italic">实例与构造函数原型之间有直接的联系，但实例与构造函数之间没有</font>
>
>3
---

# 下面创建对象的方式

## 工厂模式

>优点是可以解决创建多个类似对象的问题
>
>缺点是:没有解决对象标识问题（即新创建的对象是什么类型）。

```javascript
function createPerson(name,age){
  let obj = new Object();
  obj.name = name
  obj.age = age
  obj.sayHello = function(){
    console.log("Hello!");
  }
  return obj
}

let per1 = createPerson('liming',23)
let per2 = createPerson('xiaohong',24)

console.log(per1,per2);

```
--- 

## 构造函数模式

>缺点是: 定义的方法都要在每个实例上创建一遍,因为函数也是对象,每次定义函数的时候都会初始化一个对象,这就造成了虽然同名但是却不是同一个对象的浪费,解决方法也简单可以把这个公共的函数抽离出来放到外面这样都是引用的一个方法(对象),但是这样对全局作用域不好,全局作用域下定义多个函数,会造成污染.
>

>与工厂函数的不同点是 没有显示的创建对象  属性和方法都直接赋值给了this 没有return
>
> 引发的一个面试题 new做了什么 ?
>
> (1) 在内存中创建一个对象
>
> (2) 在这个新对象内部的____proto__被赋值未为构造函数的prototype属性
>
> (3) 构造函数内部的this被赋值为这个新的对象
>
> (4) 执行构造函数中的代码(也就是给新对象添加属性)
>
> (5) 如果构造函数返回的是非空的对象则返回该对象 否则返回创建好的新对象
>


```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.sayHello = function () {
    console.log("Hello!", this.name);
  };
  this.waibu = waibu
}

function waibu(){
  console.log("Hello",this.name)
}

let p1 = new Person("xiaoming", 32);
let p2 = new Person("Liming", 32);

p1.sayHello();//xiaoming
p2.sayHello();//Liming

p1.waibu();//xiaoming
p2.waibu();//Liming

```
--- 


## 原型模式

> 每个<font color="red">函数</font>都会创建一个prototype属性,这个属性是一个对象,包含应该由特定引用类型的实例共享的属性和方法,<font color="red">实际上这个对象就是通过调用构造函数创建的对象的原型</font>,使用原型对象的好处是在它上面定义的属性和方法可以被对象实例共享
>
>

```javascript

function PersonPlus(name, age) {
  PersonPlus.prototype.name = name;
  PersonPlus.prototype.age = age;
  PersonPlus.prototype.sayHello = function () {
    console.log("hello", this.name);
  };
}

let person1 = new PersonPlus("li", 23);
let person2 = new PersonPlus("li", 24);

console.log(person1.sayHello == person2.sayHello); // true

```
---
