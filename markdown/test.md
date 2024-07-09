## 7.9

首先是路由配置，按照正常的配置情况发现路由点击并不生效，发现实路由路径加了#写的方式为：

```js
http://localhost:5173/#subnote1-1
//从http://localhost:5173跳转到第一个笔记页面，地址后面加上/#subnote1-1
但是这里没有成功跳转。去掉#就行了。
```

**为什么带#跳不过去啊。**
如何定位到上次浏览的位置呢，这个学习笔记网站，每次进来都是首页部分。

### typeof 和 instanceof 的区别

typeof 用来检测**未经计算**的数据的类型，返回一个字符串。typeof null`为`object 是一个 bug 吗。所以判断 null 直接用===null 来就行了。**引用数据**类型**除了 function**其他检测都会为 object。如果要判断一个变量是否存在的话可以用 typeof 而不能用 if，if 判断的话，如果未声明会报错。instanceof 运算符用于检测**构造函数**的**prototype**是否在一个原型链上。

`typeof`会返回一个变量的基本类型，`instanceof`返回的是一个布尔值

需要统一的数据类型检测可以用`Object.prototype.toString`。加上`.call`会指向当前的数据类型 this。

```js
 object instanceof constructor; //object为实例对象，constructor为构造函数
```

### 原型和原型链(prototype，constructor，`__proto__`)

JS 声明构造函数时，会在内存中创造一个**对应的对象**，这个**对象**就是原函数的原型。构造函数中有一个默认的 prototype 属性，指向的就是**函数原型**。原型中还有一个 constructor 属性，**constructor 指向函数对象**。
