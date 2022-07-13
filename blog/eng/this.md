# 💕 this指向问题

🤣 this优先级:箭头函数 -> new绑定 -> 显示绑定call/bind/apply -> 隐式绑定 -> 默认绑定

---
## 💮 默认绑定

默认绑定一般发生在回调函数,函数直接调用;
```javascript
function test() {
    //严格模式下是undefined
    //非严格模式下是window
    console.log(this);
}
setTimeout(function () {
    //setTimeout的比较特殊
    //严格模式和非严格模式下都是window
    console.log(this);
});

arr.forEach(function () {
    //严格模式下是undefined
    //非严格模式下是window
    console.log(this);
});

```
--- 

## 💤 隐式绑定
这个通俗点用一句话概括就是谁调用就是指向谁
```javascript
const obj = {
  name:'joy',
  getName(){
      console.log(this); //obj
      console.log(this.name); //joy
  }
};
obj.getName();

```
--- 

## 🍊 显示绑定call,apply,bind
```javascript
const obj1 = {
    name: 'joy',
    getName() {
        console.log(this); 
        console.log(this.name); 
    }
};

const obj2 = {
    name: 'sam'
};

obj1.getName.call(obj2); //obj2 sam
obj1.getName.apply(obj2); //obj2 sam
const fn = obj1.getName.bind(obj2);
fn();//obj2 sam


```
--- 

## 🍔 new绑定
```javascript
function Vehicle() {
    this.a = 2
    console.log(this);
}
new Vehicle(); //this指向Vehicle这个new出来的对象

```
--- 

## 🥔 ES6的箭头函数
es6的箭头函数比较特殊,箭头函数this为父作用域的this，不是调用时的this.要知道前四种方式,都是调用时确定,也就是动态的,而箭头函数的this指向是静态的,声明的时候就确定了下来.比较符合js的词法作用域吧
```javascript
window.name = 'win';
const obj = {
    name: 'joy',
    age: 12,
    getName: () => {
        console.log(this); //其父作用域this是window,所以就是window
        console.log(this.name); //win 
    },
    getAge: function () {
        //通过obj.getAge调用,这里面this是指向obj
        setTimeout(() => {
            //所以这里this也是指向obj 所以结果是12
            console.log(this.age); 
        });
    }
};
obj.getName();
obj.getAge();

```
--- 