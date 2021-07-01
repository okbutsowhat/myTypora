# Vue基本指令介绍
### 常见指令
| 指令名称 | 指令作用                                                     |
| -------- | ------------------------------------------------------------ |
| v-model  | 将元素的value动态绑定到指令所指向的值,实现数据的双向绑定     |
| v-show   | 若该对象无值, 则修改`display`隐藏该对象,即元素本身会被渲染   |
| v-if     | 指令用于条件性地渲染一块内容。这块内容只会在指令的表达式返回 truthy 值的时候被渲染 |
| v-else   | 搭配v-if实现条件渲染,同理的还有v-else-if                     |
| v-for    | 迭代传递对象内的数据,可以是数组对象                          |
| v-bind   | 动态加载属性值, 可直接省略用":"表示, 例如 `:class="active"`  |
| v-on     | 绑定事件,可用"@"缩写                                         |
***

#### v-model, v-show, v-if
```html
<div id="app">
    <div>
        <input type="text" v-model="name" >
        <span v-show="name">你的名字是：{{name}}</span>
    </div>
    <div>
        <input type="text" v-model="age" >
        <span v-show="age">你的年龄是：{{age}}</span>
    </div>
    <div>
        <input type="text" v-model="sex" >
        <span v-show="sex">你的性别是：{{sex}}</span>
    </div>
</div>
```
```js
var app = new Vue({
    el:'#app',
    data:{
        name:'freechar',
        age:21,
        sex:'male'
    }
});
```

1. `v-model=""`, 使input的值绑定到`app.name.value`;
2. `v-show="sex"`, 如果`app.sex`存在值才显示该元素;
3. `v-if="sex"`, 如果`app.sex`存在值才显示该元素, 否则直接删除;
4. v-model.lazy, 惰性刷新;
5. v-model.trim, 去除输入数据前后的空格;
6. v-model.number, 使数字输入为number类型.
> **"v-xxx"称为vue的指令,即一种自定义属性**
#### 动态迭代指令v-for
```html
    <div id="app_2">
        <ul>
            <li v-for="food in foodList">{{food.name}}的价格是:{{food.discount?food.price*food.discount:food.price}}</li>
        </ul>
    </div>
```
```js
var app_2 = new Vue({
    el:'#app_2',
    data:{
        foodList:[
            {
                name:'青椒',
                price:'10',
                discount:'.4'
            },
            {
                name:'西红柿',
                price:'15',
                discount:'.8'
            },
            {
                name:'黄瓜',
                price:'16',
            }
        ]
    }
});
```
只输入一个li标签加上v-for完成迭代.
v-for同样是动态的数据绑定:
```js
app.foodList.push({
    name:'土豆', 
    price:'8', 
    discount:'.8'
    })
    //通过push在food列表里动态的添加"土豆"
```

#### v-bind指令
```html
<div id="app_3">
    <span v-bind:class="{active: isActive}">
        通过v-bind动态加载属性值
    </span>
</div>
```
```js
var app_3 = new Vue({
   el:'#app_3',
   data:{
       isActive:true,
   }
});
```

#### v-on
绑定事件
```html
    <div id="app_4">
        <button v-on="{mouseenter: mouseOn, mouseleave: mouseOut}">
            提交
        </button>
        <form action="" @submit.prevent="onSubmit"> <!--.prevent防止提交表单后浏览器的默认刷新行为-->
            <input type="text">
            <button type="submit">提交2</button>
        </form>
    </div>
```
```js
var app_4 =new Vue({
    el:'#app_4',
    methods:{
        mouseOn:function () {
            console.log("mouseenter")
        },
        mouseOut:function () {
            console.log("mouseleave")
        },
        onSubmit:function () {
            console.log("submitted")
        }
    }
});
```

#### v-for
在` v-for `块中，我们可以访问所有父作用域的属性。`v-for `还支持一个可选的第二个参数，即当前项的索引。
```html
<ul id="example-2">
    <li v-for="(item, index) in items"> 
        {{ parentMessage }} - {{ index }} - {{ item.message }} 
    </li> 
</ul>
```
```js

var example2 = new Vue({ 
    el: '#example-2',
    data: { 
    parentMessage: 'Parent',
    items: [ 
        { message: 'Foo' },
        { message: 'Bar' } 
    ] 
    }
    })
```

结果:
* Parent - 0 - Foo
* parent - 1 - Bar

你也可以用`v-for`来遍历一个对象的属性.
```html
<ul id="v-for-object" class="demo"> 
    <li v-for="value in object"> 
        {{ value }} 
    </li> 
</ul>
```
```js
new Vue({ 
    el: '#v-for-object', 
    data: { 
        object: {
            title: 'How to do lists in Vue', 
            author: 'Jane Doe', 
            publishedAt: '2016-04-10' 
         }
     }
 })
```
结果:
* How to do lists in Vue
* Jane Doe
* 2016-04-10