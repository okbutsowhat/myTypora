# ES6
> ES6就是JavaScript的第六个版本, 是JavaScript的标准

## let和const
  1. let只在代码块内有效, let只能声明一次变量; var是在全局范围内有效, var可以声明多次. for循环计数器很适合使用let
  2. let不存在变量提升, var会变量提升:
```js
console.log(a);  //ReferenceError: a is not defined
let a = "apple";
 
console.log(b);  //undefined
var b = "banana";
```
> 变量提升: 声明的变量会提升至所有代码之前, 这样可以做到"先使用, 再声明"

  3. const声明一个只读变量, 声明之后不允许改变
> 注意要点: const 如何做到变量在声明初始化之后不允许改变的？其实 const 其实保证的不是变量的值不变，而是保证变量指向的内存地址所保存的数据不允许改动。

## 赋值更名

```js
varobject= {
  a: 1,
  b: 2
}
let {a:A, b} = obj;

//A:1 , b:b
```
## 新增字符串方法
	1. `includes()`是否包括某字符, 等价于原来的 `xxx.indexOf( "x") !== -1`
	2. `startsWith()`是否以某某字符为开始
	3. `endsWith()`是否以某某字符为末尾

## 模板字符串
```js
let title = '今年是';
let tpl = `
<div>
  <span>${title + `<span>2016</span>`}</span>
</div>
`;
<!--可嵌套模板-->
```

## Symbol类型
以前的数据类型: undefined, null, Boolean, String, Number, Object
Symbol不可以被重赋值
```js
let a = Symbol('this is a symbol');
//描述对值无影响
let a = Symbol();
let b = Symbol();
//a === b  false
```

## Proxy
```js
var user = new Proxy({},{
  get: function(obj, prop){
    return
  }
})
```

## Set
特点：值唯一
```js
var s = new Set([1, 2, 3, 3])
//值为[1, 2, 3]
```
常用方法:`add` , `delete` , `has` , 分别对应增加一个元素和删除一个元素和检查是否含有一个元素(返回值为布尔值)

