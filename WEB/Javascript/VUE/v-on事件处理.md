# v-on事件处理
## 事件修饰符

在事件处理程序中调用 `event.preventDefault()` 或 `event.stopPropagation()` 是非常常见的需求。尽管我们可以在方法中轻松实现这点，但更好的方式是：方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。为了解决这个问题，Vue.js 为 `v-on` 提供了事件修饰符。之前提过，修饰符是由点开头的指令后缀来表示的。
```html
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a> 

<!-- 提交事件不再重载页面 --> 
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 --> 
<a v-on:click.stop.prevent="doThat"></a> 

<!-- 只有修饰符 --> 
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 --> 
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 --> 
<div v-on:click.capture="doThis">...</div> 

<!-- 只当在 event.target 是当前元素自身时触发处理函数 --> 
<!-- 即事件不是从内部元素触发的 --> 
<div v-on:click.self="doThat">...</div>
```

## 按键修饰符
在监听键盘事件时，我们经常需要检查详细的按键。Vue 允许为 v-on 在监听键盘事件时添加按键修饰符：
```html
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` --> 
<input v-on:keyup.enter="submit">
```

为了在必要的情况下支持旧浏览器，Vue 提供了绝大多数常用的按键码的别名：
* `.enter`
* `.tab`
* `.delet` 不捕获删除和退格键
* `.esc`
* `.space`
* `.up`
* `.down`
* `.left`
* `.right`
## 系统修饰符
* `.ctrl`
* `.alt`
* `.shift`
* `meta` mac上对应command键, windows上对应win键
> .exact 修饰符允许你控制由精确的系统修饰符组合触发的事件。即两个按键无法触发,如:`@click.ctrl.exact=""`只会在只有ctrl被按下的时候会触发;`@click.exact=""`只会在没有任何系统修饰符被按下时会触发.

## v-on的优势
> 1. 扫一眼 HTML 模板便能轻松定位在 JavaScript 代码里对应的方法。
> 2. 因为你无须在 JavaScript 里手动绑定事件，你的 ViewModel 代码可以是非常纯粹的逻辑，和 DOM 完全解耦，更易于测试。
> 3.当一个 ViewModel 被销毁时，所有的事件处理器都会自动被删除。你无须担心如何清理它们。