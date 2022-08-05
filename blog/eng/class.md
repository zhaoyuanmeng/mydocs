# 类
## 基础知识(实例 静态方法属性 原型)

```javascript
class Person {
  // 构造函数里面可以为新创建的实例添加自由属性
  // age:10 这样是错误的
  // age = 10;这样是对的 会弄到实例属性里面
  // 这个是静态属性
  static age = 10;
  constructor(name) {
    this.name = name;
    this.sayHello = function () {
      console.log("hello", this.name);
    };
  }
  // 这里面定义的方法都会定义在类的原型上
  test() {
    console.log("test");
  }
  //静态类成员方法
  static test() {
    console.log("test:static", this);
  }
  //静态类成员方法 这里面的this指的是Person类自身
  static test2() {
    // 这个this.age 代表的静态属性
    console.log("test:static2", this.age);
  }
}
let p1 = new Person("zyd");
console.log(p1, "p1---");
console.log(Person.test(), "static---");
console.log(Person.test2(), "static22---");

```

---


## 继承 

> super关键字注意事项:这个关键字只能在派生类中使用，而且仅限于类构造函数、实例方法和静态方法内部,在类构造函数中使用 super 可以调用父类构造函数
>
> <font color="red">不要在调用 super()之前引用 this，否则会抛出 ReferenceError </font>
>
> <font color="red">super()的行为如同调用构造函数，如果需要给父类构造函数传参，则需要手动传入。</font> 
>
><font color="red">如果没有定义类构造函数，在实例化派生类时会调用 super()，而且会传入所有传给派生类的参数。</font>
>
><font color="red">如果在派生类中显式定义了构造函数，则要么必须在其中调用 super()，要么必须在其中返回一个对象</font>
>
>

```javascript
// 基类
class Base {
  constructor(name) {
    this.flag = true;
    this.name = name;
  }
}

// 派生类
class T extends Base {
  constructor(name) {
    // 不要在调用 super()之前引用 this，否则会抛出 ReferenceError
    // 如果显式的写了constructor那么就必须写一个super或者返回一个对象
    super(name); // 相当于super.constructor()
    console.log(this instanceof Base); //true
    console.log(this); // T { flag:true,name:'zyd'}
  }
}
let t = new T("zyd");



```


--- 

