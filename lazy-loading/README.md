# 图片懒加载

图片”懒“加载其实就是延时加载，原理就是判断图片当前的位置是否在你看的区域里。

实现一个图片懒加载涉及到JavaScript的知识点：视口位置判断、防抖节流

## 懒加载原理

图片能够被用户看到，是因为浏览器向服务器发出了请求，能够发出请求是根据`<img>`标签中`src`属性是否存在。

那么我们就让初始的`<img>`标签所有`src`属性都为空，判断当图片的位置进入到可视区域中的时候去给`<img>`标签赋值`src`属性，这样就实现懒加载了。

## 懒加载的实现

## 方法一

1. 通过`document.documentElement.clientHeight`获取屏幕可视窗口高度
2. 通过`element.offsetTop`获取元素相对于文档顶部的距离
3. 通过`document.documentElement.scrollTop`获取浏览器窗口顶部与文档顶部之间的距离，也就是滚动条滚动的距离

公式：元素距离文档顶部的距离-滚动条滚动的具体<屏幕可视窗口高度 = 2-1<3

缺点：只适用于下拉的时候去判断，如果当前在页面最底部，用户跳转到其他页面，返回过来还在最底部的话，就会加载所有的图片了

[Demo](<https://itxcc.github.io/demo/lazy-loading/index.html>)

## 方法二

利用`getBoundingClientRect()`方法来获取元素的大小以及位置，这个方法返回一个名为`ClientRect`的`DOMRect`对象，包含了`top`、`right`、`botton`、`left`、`width`、`height`这些值。

MDN上有这样一张图：

![img](https://mdn.mozillademos.org/files/15087/rect.png)

```
let rect = el.getBoundingClientRect() //元素的距离各位方向的距离
let viewHeight = document.documentElement.clientHeight； // 屏幕可视窗口高度
```

也就是说`rect.top = viewHeight`时，说明元素的顶部刚好在可视窗口的底部，那么`rect.top < viewHeight`时，元素就肯定在可视窗口内了。

公式：`rect.top < viewHeight`

问题来了：在方法一种如果页面在底部，我们去刷新就会一直加载全部的图片。就要确保这个元素确确实实要在我们的可视窗口内，从上图中可以看到，再加一个判断，`rect.bottom`>=`0`

升级我们的公式：

公式：`rect.top < viewHeight && rect.bottom >= 0`

[Demo](<https://itxcc.github.io/demo/lazy-loading/index1.html>)

## 方法三

`IntersectionObserver`可以自动观察元素是否在视口内。

```
var io = new IntersectionObserver(callback, option);
// 开始观察
io.observe(document.getElementById('example'));
// 停止观察
io.unobserve(element);
// 关闭观察器
io.disconnect();
```

callback的参数是一个数组，每个数组都是一个`IntersectionObserverEntry`对象，包括以下属性：

| 属性               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| time               | 可见性发生变化的时间，单位为毫秒                             |
| rootBounds         | 与getBoundingClientRect()方法的返回值一样                    |
| boundingClientRect | 目标元素的矩形区域的信息                                     |
| intersectionRect   | 目标元素与视口（或根元素）的交叉区域的信息                   |
| intersectionRatio  | 目标元素的可见比例，即intersectionRect占boundingClientRect的比例，完全可见时为1，完全不可见时小于等于0 |
| target             | 被观察的目标元素，是一个 DOM 节点对象                        |

我们需要用到`intersectionRatio`来判断是否在可视区域内，当`intersectionRatio > 0 && intersectionRatio <= 1`即在可视区域内。

公式：`intersectionRatio > 0 && intersectionRatio <= 1`

[Demo](<https://itxcc.github.io/demo/lazy-loading/index2.html>)

## 参考

- [阮一峰：IntersectionObserver API 使用教程](<http://www.ruanyifeng.com/blog/2016/11/intersectionobserver_api.html>)

- [原生JS实现最简单的图片懒加载](<https://blog.csdn.net/ITzhongzi/article/details/77466779>)
