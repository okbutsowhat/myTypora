# 组件
## 组件基础
### 一个Vue组件的示例
```js
// 定义一个名为 button-counter 的新组件 
Vue.component('button-counter', { 
    data: function () { 
        return { 
            count: 0
         }
     },
     template: '<button v-on:click="count++">You clicked me {{ count }} times.</button>'
 })
```
组件是可复用的 Vue 实例，且带有一个名字：在这个例子中是 <button-counter>。我们可以在一个通过 new Vue 创建的 Vue 根实例中，把这个组件作为自定义元素来使用：
```html
<div id="components-demo"> 
    <button-counter></button-counter> 
</div>
```
```js
new Vue({ el: '#components-demo' })
```
因为组件是可复用的 Vue 实例，所以它们与 `new Vue `接收相同的选项，例如` data`、`computed`、`watch`、`methods` 以及生命周期钩子等。**仅有的例外是像 `el `这样根实例特有的选项。**
> **注意当点击按钮时，每个组件都会各自独立维护它的 `count`。因为你每用一次组件，就会有一个它的新实例被创建。**

### 组件复用,`data`**必须是一个函数**
一个组件的 `data` 选项必须是一个函数，因此每个实例可以维护一份被返回对象的独立的拷贝：
```js
data: function () { 
    return { 
        count: 0
     }
 }
```
> 如果没有这条规则, 复用组件时`data`的值会影响到其他的所有实例

## 组件的组织
通常一个应用会以一棵嵌套的组件树的形式来组织:
![b5c08269dfc26ae6d7db3801e9efd296.png](en-resource://database/2300:1)
为了能在模板中使用，这些组件必须先注册以便 Vue 能够识别。这里有两种组件的注册类型：**全局注册**和**局部注册**。至此，我们的组件都只是通过 `Vue.component` 全局注册的：
```js
Vue.component('my-component-name', {
    // ... options ... 
})
```

## 通过 Prop 向子组件传递数据
Prop可以在组件上注册一些自定义特性, 当一个值传递给一个prop特性的时候, 它就变成了那个组件实例的一个属性. 例如:
```js
Vue.component('blog-post', { 
    props: ['title'], 
    template: '<h3>{{ title }}</h3>'
})

```
一个组件默认可以拥有任意数量的 `prop`，任何值都可以传递给任何 `prop`。在上述模板中，你会发现我们能够在组件实例中访问这个值，就像访问 data 中的值一样。一个 `prop` 被注册之后，你就可以像这样把数据作为一个自定义特性传递进来：
```html
<blog-post title="My journey with Vue"></blog-post> 
<blog-post title="Blogging with Vue">
</blog-post> <blog-post title="Why Vue is so fun"></blog-post>
```

**每个组件必须有且只有一根元素**, 多个标签的模板内容需包裹在一个父元素内.
示例:
```html
<div id="app-12">
    <blog-content
        v-for="post of posts"
        v-bind:key="post.id"
        v-bind:post="post"
    ></blog-content>
</div>
```
```js
Vue.component('blog-content',{
    props:['post'],
    template: '<div style="display: inline-block; height: 180px; width: 180px; background: #0c4865;color: white;margin: 3px;padding: 6px; text-align: center;" id="app-13"><h4>{{post.title}}</h4><p>{{post.content}}</p></div>'
});
var app12 = new Vue({
    el:'#app-12',
    data:{
        posts: [
            { id: 1, title: 'My journey with Vue', content:'text1' },
            { id: 2, title: 'Blogging with Vue', content:'text2' },
            { id: 3, title: 'Why Vue is so fun', content:'text3' }
        ]
    }
});
```

## vm.$emit
**触发当前实例上的事件, 附加参数都会传给监听器回调**
```js
Vue.component('welcome-button', { 
    template: ` 
        <button v-on:click="$emit('welcome')"> 
            Click me to be welcomed 
        </button>
     ` 
})
```
有的时候用一个事件来抛出一个特定的值是非常有用的。例如我们可能想让 `<blg-content>` 组件决定它的文本要放大多少。这时可以使用 `$emit` 的第二个参数来提供这个值：
```html
<button v-on:click="$emit('enlarge-text', 0.1)"> 
    Enlarge text 
</button>
```
然后当在父级组件监听这个事件的时候，我们可以通过` $event` 访问到被抛出的这个值：
```js
<blog-content
    ...
    v-on:enlarge-text="postFontSize += $event" 
></blog-content>
```

## 通过插槽分发内容
和HTML元素一样, 我们也会经常需要向一个组件传递内容, 像这样:
```html
<alert-box> 
    Something bad happened. 
</alert-box>
```
但组件内部可能有多个元素, 需要指定一个元素把文本插入这个文本内
Vue自定义的`<slot>`元素正是这样的功能:
```js
Vue.component('alert-box', {
    template: ` 
        <div class="demo-alert-box"> 
            <strong>Error!</strong> 
            <slot></slot> 
        </div>
     ` 
})
```