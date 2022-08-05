# 继承


## 原型链的知识

>只要创建一个函数,就会按照特定的规则为这个函数创建一个prototype属性(指向原型对象),所有的原型对象都会自动获取一个constructor的属性,指向与之关联的构造函数,例如Person.prototype.constructor指向Person。然后因为构造函数而异,可能会给原型对象添加其他属性和方法
>
><font color="italic">实例与构造函数原型之间有直接的联系，但实例与构造函数之间没有</font>
>
>构造函数/原型对象/和实例是三个完全不同的对象(会new的底层原理就好理解了)
>
><font color="italic">实例通过____proto__链接到原型对象,实际上指向prototype (person1.____proto__ === Person.prototype),原理是new的作用,实例通过__proto__链接到原型对象, 它实际上指向隐藏特性[[Prototype]] </font>
>
>同一个构造函数创建的两个实例 共享同一个原型对象 
>
>构造函数通过prototype属性链接到原型对象
>
>
><font color="italic">原型层级(原型链)</font>
>
>在通过对象访问属性时，会按照这个属性的名称开始搜索。搜索开始于对象实例本身。如果在这个实例上发现了给定的名称，则返回该名称对应的值。如果没有找到这个属性，则搜索会沿着指针进入原型对象，然后在原型对象上找到属性后，再返回对应的值。这就是原型用于在多个对象实例间共享属性和方法的原理。
>
>
><font color="italic">原型链的问题1:原先的实例会变为原型属性</font>
>
><font color="italic">原型链的问题2:原型链的第二个问题是，子类型在实例化时不能给父类型的构造函数传参。事实上，我们无法在不影响所有对象实例的情况下把参数传进父类的构造函数。再加上之前提到的原型中包含引用值的问题，就导致原型链基本不会被单独使用</font>
>
>


```javascript

// 只要给对象实例添加一个属性，这个属性就会遮蔽（shadow）原型对象上的同名属性，也就是虽然
// 不会修改它，但会屏蔽对它的访问。即使在实例上把这个属性设置为 null，也不会恢复它和原型的联
// 系。不过，使用 delete 操作符可以完全删除实例上的这个属性，从而让标识符解析过程能够继续搜索
// 原型对象。
function Person() {} 
Person.prototype.name = "Nicholas"; 
Person.prototype.age = 29; 
Person.prototype.job = "Software Engineer"; 
Person.prototype.sayName = function() { 
 console.log(this.name); 
}; 
let person1 = new Person(); 
let person2 = new Person(); 
person1.name = "Greg"; 
console.log(person1.name); // "Greg"，来自实例
console.log(person2.name); // "Nicholas"，来自原型
delete person1.name; 
console.log(person1.name); // "Nicholas"，来自原型


// 原型链的问题1例子
function SuperType() { 
 this.colors = ["red", "blue", "green"]; 
} 
function SubType() {} 
// 继承 SuperType 
SubType.prototype = new SuperType(); 
let instance1 = new SubType(); 
instance1.colors.push("black"); 
console.log(instance1.colors); // "red,blue,green,black" 
let instance2 = new SubType(); 
console.log(instance2.colors); // "red,blue,green,black" 


```

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
> (1) 在内存中创建一个对象 new Object()
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


---


## 原型和原型链

>ECMA-262 把原型链定义为 ECMAScript 的主要继承方式。其基本思想就是通过原型继承多个引用类型的属性和方法。
>
>基本思想:通过原型本身的内部指针指向另一个原型,相应地另一个原型也有一个指针指向另一个构造函数,这样在实例和原型之间构造了一个原型链
>
><font color="italic">重要的一条理论(之前疏忽的):重写构造函数上的原型之后再创建的实例才会引用新的原型。而在此之前创建的实例仍然会引用最初的原型,实例只有指向原型的指针,没有指向构造函数的指针</font>
>
><font color="red">原型的问题:它弱化了向构造函数传递初始化参数的能力,会导致所有实例默认都取到相同的属性值.最大的问题是他的共享特性,以及包含引用值的属性.</font>
>
> 每个<font color="red">函数</font>都会创建一个prototype属性,这个属性是一个对象,包含应该由特定引用类型的实例共享的属性和方法,<font color="red">实际上这个对象就是通过调用构造函数创建的对象的原型</font>,使用原型对象的好处是在它上面定义的属性和方法可以被对象实例共享
>
>
>
>

```javascript

// 原型链的例子
function Per(name) {
  this.name = name;
}
Per.prototype.sayHello = function () {
  console.log("hello", this.name);
};

function Stu() {
  this.s = false;
}
Stu.prototype = new Per("li");

Stu.prototype.getS = function () {
  console.log(this.s);
};
let stu1 = new Stu();
console.log("st1", stu1);

// 针对上面那个理论的举例

function Person() {} 
let friend = new Person(); 
Person.prototype = { 
 constructor: Person, 
 name: "Nicholas", 
 age: 29, 
 job: "Software Engineer", 
 sayName() { 
 console.log(this.name); 
 } 
}; 
friend.sayName(); // 错误

// 原型问题的举例

function Person() {} 
Person.prototype = { 
 constructor: Person, 
 name: "Nicholas", 
 age: 29, 
 job: "Software Engineer", 
 friends: ["Shelby", "Court"], 
 sayName() { 
 console.log(this.name); 
 } 
}; 
let person1 = new Person(); 
let person2 = new Person(); 
person1.friends.push("Van"); 
console.log(person1.friends); // "Shelby,Court,Van" 
console.log(person2.friends); // "Shelby,Court,Van" 
console.log(person1.friends === person2.friends); // true 

```

---

## 盗用构造函数

> 目的是为了解决原型包含引用值导致的继承问题
>
>基本思路很简单：在子类构造函数中调用父类构造函数。因为毕竟函数就是在特定上下文中执行代码的简单对象，所以可以使用apply()和 call()方法以新创建的对象为上下文执行构造函数.而且还可以传递参数
>

```javascript
function SuperType(name){ 
 this.name = name; 
} 
function SubType() { 
 // 继承 SuperType 并传参
 SuperType.call(this, "Nicholas"); 
 // 实例属性
 this.age = 29; 
} 
let instance = new SubType(); 
console.log(instance.name); // "Nicholas"; 
console.log(instance.age); // 29 

```

---



## 组合继承

> 目的是为了综合了原型链和盗用构造函数，将两者的优点集中了起来
>
>基本的思路是使用原型链继承原型上的属性和方法，而通过盗用构造函数继承实例属性。这样既可以把方法定义在原型上以实现重用，又可以让每个实例都有自己的属性。
>
>

```javascript
function SuperType(name){ 
 this.name = name; 
 this.colors = ["red", "blue", "green"]; 
} 
SuperType.prototype.sayName = function() { 
 console.log(this.name); 
}; 
function SubType(name, age){ 
 // 继承属性
 SuperType.call(this, name); 
 this.age = age; 
} 
// 继承方法
SubType.prototype = new SuperType(); 
SubType.prototype.sayAge = function() { 
 console.log(this.age); 
}; 
let instance1 = new SubType("Nicholas", 29); 
instance1.colors.push("black"); 
console.log(instance1.colors); // "red,blue,green,black" 
instance1.sayName(); // "Nicholas"; 
instance1.sayAge(); // 29 
let instance2 = new SubType("Greg", 27); 
console.log(instance2.colors); // "red,blue,green" 
instance2.sayName(); // "Greg"; 
instance2.sayAge(); // 27 

```

---


## 原型式继承

> 这个实际上就是Object.create()函数

```javascript
let person = { 
 name: "Nicholas", 
 friends: ["Shelby", "Court", "Van"] 
}; 
let anotherPerson = Object.create(person, { 
 name: { 
 value: "Greg" 
 } 
}); 
console.log(anotherPerson.name); // "Greg" 

```

---


## 寄生式继承
待写

## 寄生式组合继承

待写
