# Cookie
#### 含义
> 1. ookie是浏览器访问服务器后, 服务器传给浏览器的一段数据.
> 2. 浏览器需要保存这段数据, 不得轻易删除
> 3. 此后每次浏览器每次访问该服务器都需要带上这段数据.

#### 如何使用
Cookie一般有两个作用
1. 用作用户身份的识别
    
> 比如用户 A 用浏览器访问了 http://a.com，那么 http://a.com 的服务器就会立刻给 A 返回一段数据「uid=1」（这就是 Cookie）。当 A 再次访问 http://a.com 的其他页面时，就会附带上「uid=1」这段数据。同理，用户 B 用浏览器访问 http://a.com 时，http://a.com 发现 B 没有附带 uid 数据，就给 B 分配了一个新的 uid，为2，然后返回给 B 一段数据「uid=2」。B 之后访问 http://a.com 的时候，就会一直带上「uid=2」这段数据。借此，http://a.com 的服务器就能区分 A 和 B 两个用户了。
    
2. 记录历史
    
    > 假设 http://a.com 是一个购物网站，当 A 在上面将商品 A1 、A2 加入购物车时，JS 可以改写 Cookie，改为「uid=1; cart=A1,A2」，表示购物车里有 A1 和 A2 两样商品了。这样一来，当用户关闭网页，过三天再打开网页的时候，依然可以看到 A1、A2 躺在购物车里，因为浏览器并不会无缘无故地删除这个 Cookie。借此，就达到里记录用户操作历史的目的了。

#### 通过jQuery Cookie插件操作Cookie
1. 引入文件
    需要引入jquery.js和jquery.cooie.js
    
```html
<script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.js"></script>
```
***
```html
<script src="https://cdn.bootcss.com/jquery-cookie/1.4.1/jquery.cookie.js"></script>
```

2. 使用方法
创建cookie
   
```js
$.cookie('name', 'value');
```
需指定时间, 否则会在浏览器关闭时过期.
**创建cookie, 并设置在7天后过期**
```js
$.cookie('name', 'value', { expires: 7 });
```
创建 cookie，并设置 cookie 的有效路径，路径为网站的根目录：
```js
$.cookie('name', 'value', { expires: 7, path: '/' });
```
**注:** *在默认情况下，只有设置 cookie 的网页才能读取该 cookie。如果想让一个页面读取另一个页面设 置的cookie，必须设置 cookie 的路径。cookie 的路径用于设置能够读取 cookie 的顶级目录。将这 个路径设置为网站的根目录，可以让所有网页都能互相读取 cookie （一般不要这样设置，防止出现冲突）。*


读取 cookie：
```js
$.cookie('name'); // => "value"
$.cookie('nothing'); // => undefined
```
读取所有的 cookie 信息：
```js
$.cookie(); // => { "name": "value" }
```
删除cookie:
```js
// cookie 删除成功返回 true，否则返回 false 
$.removeCookie('name'); // => true 
$.removeCookie('nothing'); // => false 

// 写入使用了 path时，读取也需要使用相同的属性 (path, domain) 
$.cookie('name', 'value', { path: '/' }); 
// 以下代码【删除失败】 
$.removeCookie('name'); // => false 
// 以下代码【删除成功】 
$.removeCookie('name', { path: '/' }); // => true
```
**注:** *删除cookie时, 必须传递用于设置cookie的完全相同的路径, 域及安全选项*