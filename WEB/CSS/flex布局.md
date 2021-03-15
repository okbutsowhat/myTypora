# flex布局

## 基本概念

![](https://pic4.zhimg.com/v2-54a0fc96ef4f455aefb8ee4bc133291b_r.jpg)

在 flex 容器中默认存在两条轴，水平主轴(main axis) 和垂直的交叉轴(cross axis)，这是默认的设置，当然你可以通过修改使垂直方向变为主轴，水平方向变为交叉轴，这个我们后面再说。

在容器中的每个单元块被称之为 flex item，每个项目占据的主轴空间为 (main size), 占据的交叉轴的空间为 (cross size)。

这里需要强调，不能先入为主认为宽度就是 main size，高度就是 cross size，这个还要取决于你主轴的方向，如果你垂直方向是主轴，那么项目的高度就是 main size。

## flex容器

首先，实现 flex 布局需要先指定一个容器，任何一个容器都可以被指定为 flex 布局，这样容器内部的元素就可以使用 flex 来进行布局。

```css
.container {
    display: flex | inline-flex;       //可以有两种取值
}
```

> **需要注意的是：当时设置 flex 布局之后，子元素的 float、clear、vertical-align 的属性将会失效。**

## flex布局的属性

有下面六种属性可以设置在容器上，它们分别是：

1. flex-direction
2. flex-wrap
3. flex-flow
4. justify-content
5. align-items
6. align-content

### 1. flex-direction: 决定主轴的方向(即项目的排列方向)

```css
.container {
    //左→右 | 右→左 | 上→下 | 下→上
    flex-direction: row | row-reverse | column | column-reverse;
}
```

默认值：row，主轴为水平方向，起点在左端。

### 2. flex-wrap: 决定容器内项目是否可换行

```css
.container {
    flex-wrap: nowrap | wrap | wrap-reverse;
}
```



### 3. justify-content：定义了项目在主轴的对齐方式

```css
.container {
    justify-content: flex-start | flex-end | center | space-between | space-around;
}
```



### 4. align-items: 定义了项目在交叉轴上的对齐方式

```css
.container {
    align-items: flex-start | flex-end | center | baseline | stretch;
}
```

