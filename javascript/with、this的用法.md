# with
> with 语句为一个或一组语句指定默认对象,通常用来缩短特定情形下必须写的代码量。<br/> 用法：with (<对象>) <语句>;

```javascript
x = Math.cos(3 * Math.PI) + Math.sin(Math.LN10);
y = Math.tan(14 * Math.E);

//当使用 with 语句时，代码变得更短且更易读：

with (Math) {
   x = cos(3 * PI) + sin(LN10);
   y = tan(14 * E);
}
```

注：strict 模式下已经禁用，因为无法再编译时确定对象属于哪个对象。

# this
> this 对象 返回"当前"对象。在不同的地方，this 代表不同的对象。如果在 JavaScript 的"主程序"中（不在任何 function 中，不在任何事件处理程序中）使用 this，它就代表 window 对象；如果在 with 语句块中使用 this，它就代表 with 所指定的对象；如果在事件处理程序中使用 this，它就代表发生事件的对象。

注：strict 模式下已经禁止this关键字指向全局对象，this的值为undefined
