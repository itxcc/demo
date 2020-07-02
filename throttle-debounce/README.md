# JavaScript的节流、防抖使用场景

在一次面试中被问到，直接懵逼，没听过的概念，后面查了一下资料，自己做个Demo学习一下。



## 什么是节流和防抖

> 浏览器中某些计算和处理要比其他的昂贵很多。例如，DOM操作比起非DOM操作交互需要更多的内存和CPU时间。
>
> 函数节流背后的基本思想是指，某些代码不可以在没有间断的情况连续重复执行。第一次调用函数，创建一个定时器，在指定的时间间隔之后运行代码。当第二次调用该函数是，它会清除前一次的定时器并设置另一个。如果前一个定时器已经执行过了，这个操作就没有任何意义。然而，如果前一个定时器尚未执行，其实就是将其替换为一个新的定时器。目的是只有执行函数的请求停止了一段时间之后才执行。
>
> ```
> function throttle(method, context) {
>     clearTimeout(method.tId);
>         method.tId= setTimeout(function(){
>         method.call(context);
>     }, 100);
> }
> ```
>
> --- 《JavaScript高级程序设计》



我的理解：

节流：对于`input`、`scroll`等短时间内有大量操作的变为每隔一段时间执行。

防抖：对于`input`、`scroll`等短时间内有大量操作的变为执行最后那一次。比如：`input`输入，先让一个`setTimout `延时执行，如果再有输入，清除前一个`setTimeout`

防抖和节流的目的都是为了减少不必要的计算，不浪费资源，只在适合的时候再进行触发计算。

## [Demo](<https://itxcc.github.io/demo/throttle-debounce/index.html>)

## 应用场景

防抖：

- `input`输入时，不断输入值，要利用防抖来节约请求资源。
- 频繁操作点赞和取消点赞，因此需要获取最后一次操作结果并发送给服务器
- 使用scroll、touchmove进行操作室

节流：

- 鼠标不断点击触发，mousedown(单位时间内只触发一次)
- window触发resize的时候，不断的调整浏览器窗口大小会不断的触发这个事件，用防抖来让其只触发一次



## 参考

- [The Difference Between Throttling and Debouncing](<https://css-tricks.com/the-difference-between-throttling-and-debouncing/>)

- [浅谈 JavaScript 的函数节流](<http://www.alloyteam.com/2012/11/javascript-throttle/>)
