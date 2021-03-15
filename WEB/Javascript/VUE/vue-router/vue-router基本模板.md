# vue-router

## 基本模板

```html
<div id="main">
	<router-link to="/">首页</router-link>
	<router-link to="/about">关于我们</router-link>
</div>
<div>
    //路由页面渲染在router-view标签内
    <router-view></router-view>
</div>
```

```js
var routes = [
    {
        path:'/',
        component:{
            template:`
			<div><h1>首页</h1></div>
            `,
        },
    },
    {
        path:'/about',
        component:{
            template:`
			<div><h1>关于</h1></div>
            `,
        },
    }
]
```

```js
var router = new VueRouter({
    routes:routes,
});
//把上面的路由定义声明定义
```

```js
var app = new Vue({
        el:'#main',
        route:router,
)}
```

## 传参

### 通过`{{$route.params.name}}`传参

```js
var routes = [
    {
        path:'/user/:name',
        component:{
            template:`
			<div>
				<div>我叫:{{$route.params.name}}</div>
			</div>
            `,
        },
    }
]
```

### 通过`{{$route.query.age}}`传参

```js
var routes = [
    {
        path:'/user/:name',
        component:{
            template:`
			<div>
				<div>我叫:{{$route.params.name}}</div>
				<div>我今年{{$route.query.age}}岁了</div>
			</div>
            `,
        },
    }
]
//点解的链接如果是whh?age=30,这输出的为我叫:whh 今年30岁了
```

## 子路由

```js
var routes = [
    {
        path:'/user/:name',
        component:{
            template:`
			<div>
				<div>我叫:{{$route.params.name}}</div>
				//无需重复填写路径,加上append属性后,可以直接追加地址
				<router-linke to="more" append>更多信息</router-link>
				<router-view></router-view>
			</div>
            `,
        },
        children:[
            {
                path:'more'
                component:{
                	template:`<div>用户{{$route.params.name}}的详细信息,xxxxxx</div>`
            }
            }
        ]
    }
]
```

