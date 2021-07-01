# ES6 

## 1. 箭头函数

### 1.1箭头函数中，this是静态的，this始终指向函数声明式所在作用域下的this值

```js
function getName1 () {
    console.log(this.name);
}
let getName2 = () => {
    console.log(this.name);
}

// 设置 window 对象的 name 属性
window.name = 'name1'
const static = {
    name: 'name2'
}

// 直接调用
getName1() // 'name1'
getName2() // 'name2'

// call方法调用
getName1.call(static) // 'name2'
getName2.call(static) // 'name1' 在window下调用,指向始终不变
```

### 1.2箭头函数不能作为构造示例化对象

```js
let Person = (name, age) => {
    this.name = name;
    this.age = age
}
let me = new Person('me', 22);
console.log(me); // person is not a constructor
```

### 1.3 不能使用 arguments  变量

### 1.4 箭头函数的简写

* 省略小括号: 只有一个形参时;
* 省略花括号: 只有一条代码语句时, 省略花括号时`return`也需要省略.

### 1.5指向改变示例

```js
    const ad = document.getElementById('home')
    ad.addEventListener('click', function () {
      console.log(this)
      setTimeout(() => {
        this.style.background = 'pink'
      }, 1000)
    })
	// 这里的this将指向ad容器
```

> 箭头函数适合与 this 无关的回调. 定时器, 数组的方法回调.
> 不适合与 this 有关的回调. 事件回调, 对象的方法.

## 2.形参的初始值

### 2.1 形参初始值具有默认值的参数, 一般位置靠后.

### 2.2与解构赋值结合



## 3. rest参数

rest参数用于获取函数的实参, 用来代替arguments

```js
// 传统arguments获取方式, 结果为对象
function date() {
    console.log(arguments)
}
// rest 参数, 结果为数组, 可以使用filter some every map等方法, 方便处理参数
function (...args) {
    console.log(args)
}
```

> rest 参数必须放到参数的最后 `fn(a,b, ...args)`

## 4. Symbol

* Symbol 的值是唯一的, 用来解决命名冲突的问题
* Symbol 值不能与其他数据进行运算
* Symbol 定义的对象属性不能使用`for ...in`循环遍历, 但是可以使用`Reflect.ownKeys`来获取对象的所有键名

```js
// 创建Symbol
let Symbol1 = Symbol
let Symbol2 = Symbol('someString')
let Symbol3 = Symbol('someString')
console.log(Symbol2 === Symbol3) // false

let Symbol4 = Symbol.for('someString')
let Symbol5 = Symbol.for('someString')
console.log(Symbol4 === Symbol5) // true
```

### 4.1 迭代器iterator的使用

用于自定义遍历数据

```js
    const iteratorTest = {
      name: 'iterator',
      list: [
        'value1',
        'value2',
        'value3',
        'value4'
      ],
      [Symbol.iterator] () { //iterator声明方式需要添加[]
        let index = 0
        return {
          next: () => {
            if (index < this.list.length) {
              const result = { value: this.list[index], done: false }
              index++
              return result
            } else {
              return { value: undefined, done: true }
            }
          }
        }
      }
    }
    for (const v of iteratorTest) {
      console.log(v)
      // value1
      // value2
      // value3
      // value4
    }
```



## 5. 生成器

生成器其实就是一个特殊的函数
是异步编程的一种新的解决方案

```js
// 生成器函数声明方式 加*
function * gen () {
    console.log('代码块1')
    yield '语句一'
    console.log('代码块1')
    yield '语句二'
    console.log('代码块1')
    yield '语句三'
}
// 调用函数
let iterator = gen()
iterator.next() // next 方法可以传入实参, 作为上一个yield语句的返回结果

```

### 5.1 生成器例子

使用生成器配合iterator完成多个异步函数, 避免回调地狱和难以维护的代码

```js
    function interval1 () {
      setTimeout(() => {
        console.log('interval1')
        iterator.next()
      }, 1000)
    }
    function interval2 () {
      setTimeout(() => {
        console.log('interval2')
        iterator.next()
      }, 1000)
    }
    function interval3 () {
      setTimeout(() => {
        console.log('interval3')
        iterator.next()
      }, 1000)
    }
    function * gen () {
      yield interval1()
      yield interval2()
      yield interval3()
    }
    const iterator = gen()

    iterator.next() // 每间隔一秒打印结果
```

> 可以配合iterator.next(params)的方式来进行异步传参, 将上一步的运行结果传递到下一步

## 6. Promise

### 6.1 Promise基本介绍与使用

Promise是一种异步编程解决方案, 实例化方式为`new Promis(fuction (resolve, reject) { reslve('something') reject('someting') })`

```js
const P = new Promise((resolve, reject) => {
    setTimeout(() => {
        let data = 'something'
        resolve(data)
    })
    // let err = 'something error'
    // reject(err)
})
```

### 6.2 then 方法的返回结果也是 Promise 对象

如果回调函数中返回结果是 非Promise 类型的属性, 状态为成功, 返回值为对象的成功值
如果为Promise类型, 则状态为返回Promise的状态.
throw 抛出错误也为rejected状态.

## 7. Set 集合

基本方法:

```js
let s = new Set()
// 增加
s.add()
// 删除
s.delet()
// 判断有无
s.has()
```



### 7.1 数组去重

```js
let arr = [1, 2, 3, 1]
let result = [...new Set(arr)] // [1, 2, 3]
```

### 7.2 求数组交集

```js
let arr1 = [1, 2, 3, 4, 5, 4, 3, 2, 1]
let arr2 = [4, 5, 6, 5, 4]
let result = [...new Set(arr1)].filter(item => {
    let s2 = new Set(arr2).has(item)
})
// [4, 5]
```

### 7.3 求数组并集

```js
let arr1 = [1, 2, 3, 4, 5, 4, 3, 2, 1]
let arr2 = [4, 5, 6, 5, 4]
let union = [...new Set([...arr1, ...arr2])]
```

## 8. Map

基本使用方法

```js
let m = new Map()
// 添加元素
m.set('itemName', 'itemValue')
// size
m.size
// 删除
m.delete('itemName')
// 获取
m.get('itemName')
// 清空
m.clear()
// 遍历
for (const v of m) {
    console.log(v)
}
```

> Map的键名不限于字符串

## 9. class

ES6 的 class可以看作是一个语法糖, 他的绝大部分功能都可以用ES5做到, 新的class写法只是让对象的原型的写法更加清晰\更像面向对象编程的语法而已

```js
// 一个传统的构造函数的写法
function Phone(brand, price) {
    this.brand = brand
    this.price = price
}
// 添加方法
Phone.prototype.call = function () {
    console.log('华为手机就是牛')
}
// 实例化对象
let huawei = new Phone('华为', 4599)
huawei.call()
console.log(huawei)
```

```js
class Phone {
    // 构造方法 名字不能修改, new时执行
    constructor (brand, price) {
    	this.brand = brand
    	this.price = price
	}
    // 方法必须使用该语法, 不能使用es5的完整形式
    call () {
    	console.log('华为手机就是牛')
	}
}
let onePlus = new Phone('1+', 3999)
```

### 9.1 static属性

static 标注的属性属于类, 不属于实例化的对象

```js
class Phone {
    // 静态属性
    static name = '手机'
    static change () {
        console.log('我是手机')
    }
}
let nokia = new Phone()
console.log(nokia.name) // undefined
console.log(Phone.name) // 手机
```

