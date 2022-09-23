# TS

# 泛型其实就是一个类型参数带入进去 不要想复杂了

# extends

```javascript

// 1、接口继承
interface T1 {
    name: string
  }
interface T2 {
  sex: number
}

// 多重继承，逗号隔开
interface T3 extends T1,T2 {
  age: number
}

// 合法
const t3: T3 = {
  name: 'xiaoming',
  sex: 1,
  age: 18
}

// 2、条件判断的普通用法
// 示例1
interface Animal {
  eat(): void
}

interface Dog extends Animal {
  bite(): void
}

// A的类型为string
type A = Dog extends Animal ? string : number

const a: A = 'this is string'
// 3、条件判断的泛型用法
对于使用extends关键字的条件类型（即上面的三元表达式类型），如果extends前面的参数是一个泛型类型，当传入该参数的是联合类型，则使用分配律计算最终的结果。分配律是指，将联合类型的联合项拆成单项，分别代入条件类型，然后将每个单项代入得到的结果再联合起来，得到最终的判断结果。
type P<T> = T extends 'x' ? string : number;
type A3 = P<'x' | 'y'>  // A3的类型是 string | number

防止条件判断中的分配
在条件判断类型的定义中，将泛型参数使用[]括起来，即可阻断条件判断类型的分配，此时，传入参数T的类型将被当做一个整体，不再分配。
type P<T> = [T] extends ['x'] ? string : number;
type A1 = P<'x' | 'y'> // number
type A2 = P<never> // string
```

# Exclude

```javascript

type A = Exclude<'key1' | 'key2', 'key2'> // 'key1'

type Exclude<T, U> = T extends U ? never : T


```

---

# Extract

```javascript

type Extract<T, U> = T extends U ? T : never
type A = Extract<'key1' | 'key2', 'key1'> // 'key1'


```

---

# Pick

```javascript
// 高级类型Pick的定义
type Pick<T, K extends keyof T> = {
    [P in K]: T[P]
}

interface A {
    name: string;
    age: number;
    sex: number;
}

type A1 = Pick<A, 'name'|'age'>
// 报错：类型“"key" | "noSuchKey"”不满足约束“keyof A”
type A2 = Pick<A, 'name'|'noSuchKey'>

```

---

# Record

```javascript
type Coord = Record<"x" | "y", number>;

// 等同于
type Coord = {
  x: number,
  y: number,
};
```

## 阅读神光的
