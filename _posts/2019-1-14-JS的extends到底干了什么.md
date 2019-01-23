---
layout: post
title: 当我们在extend（ts）时，我们干了些啥？
---
## 前情提要
**对es6中class的定义**
JavaScript classes, introduced in ECMAScript 2015, are primarily syntactical sugar over JavaScript's existing prototype-based inheritance. The class syntax does not introduce a new object-oriented inheritance model to JavaScript.

## 原始代码
**B extend A**
```
class A {
    a: number = 123;
    getA() { return this.a; }
}

class B extends A {
    b: number = 321;
    getB() { return this.b; }
}

const c = new B();
console.log('c', c);
```
分为几个问题来看，从简单到复杂：
1. new做了什么
2. class做了什么
3. extends做了什么

### new做了什么？
```
// new Test():
    create new Object() obj
    set obj.__proto__ to Test.prototype
    return Test.call(obj) || obj;
    // normally obj is returned but constructors in JS can return a value
```
最常用的new做了3部！
1. 创建一个空的Object
2. 把1中的Oject的原型链（__proto__）指向目标（Test）的prototype。
3. 执行call，通过自己（Object）的this，调用了目标的构造函数，也就是获得了目标的property的值。

### class做了什么？
这里是babel对class A的解析
```
var A = /** @class */ (function () {
    function A() {
        this.a = 123;
    }
    A.prototype.getA = function () { return this.a; };
    return A;
}());
```
方法会被作为prototype，其余则是property。

### extends做了什么？
```
var B = /** @class */ (function (_super) {
    __extends(B, _super);
    function B() {
        var _this = _super !== null && _super.apply(this, arguments) || this;
        _this.b = 321;
        return _this;
    }
    B.prototype.getB = function () { return this.b; };
    return B;
}(A));
```
和A的Class不同的是，B作为extend出来的class，编译后的代码中有一行__extends和_this的赋值。其余和A的代码是一样的。_（B()作为构造函数把变量作为property赋值，而方法则作为B的prototype。）_
所以我们具体来看看这两部做了啥。
**_this**
apply了父类的构造函数，如果没有父类则返回this。
也就是说先把父类的property复制过来，如果自身有重复的则会覆盖。
**__extends**
可以看到extends的函数接受2个参数，第一个是B的构造函数，第二个是super也就是A的构造函数。
```
var __extends = (this && this.__extends) || (function () {
    var extendStatics = function (d, b) {
        extendStatics = Object.setPrototypeOf ||
            ({ __proto__: [] } instanceof Array && function (d, b) { d.__proto__ = b; }) ||
            function (d, b) { for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p]; };
        return extendStatics(d, b);
    };
    return function (d, b) {
        extendStatics(d, b);
        function __() { this.constructor = d; }
        d.prototype = b === null ? Object.create(b) : (__.prototype = b.prototype, new __());
    };
})();
```
为了使B可以继承A，__extends里没少做骚操作，简单来说。
当你使用B去new一个新实例时，你可以在实例中的__prop__中找到B，还有B的构造函数，B的prototype，B的__prop__里又能找到A的prototype。
而且，在上面的class里，B也继承了A的所有property，所以你能在实例中发现B构造函数中的所有属性。
下面是得到的结果
```
B {a: 123, b: 321}a: 123b: 321__proto__: Aconstructor: ƒ B()arguments: nullcaller: nulllength: 0name: "B"prototype: A {constructor: ƒ, getB: ƒ}constructor: ƒ B()getB: ƒ ()__proto__: Object__proto__: ƒ A()arguments: nullcaller: nulllength: 0name: "A"prototype: {getA: ƒ, constructor: ƒ}__proto__: ƒ ()[[FunctionLocation]]: VM62878:15[[Scopes]]: Scopes[1][[FunctionLocation]]: VM62878:23[[Scopes]]: Scopes[2]getB: ƒ ()__proto__: getA: ƒ ()constructor: ƒ A()arguments: nullcaller: nulllength: 0name: "A"prototype: {getA: ƒ, constructor: ƒ}getA: ƒ ()constructor: ƒ A()arguments: nullcaller: nulllength: 0name: "A"prototype: {getA: ƒ, constructor: ƒ}__proto__: ƒ ()[[FunctionLocation]]: VM62878:15[[Scopes]]: Scopes[1]__proto__: Object__proto__: ƒ ()[[FunctionLocation]]: VM62878:15[[Scopes]]: Scopes[1]__proto__: Object
```
**值得注意的是**，在这里利用了临时构造函数去打断了父对象和子对象的直接链接，解决了共享原型时的问题（如果子对象或者在继承关系中的某个地方的任何一个子对象修改这个原型，将影响所有的继承关系中的父对象）。
这种模式通常情况下都是一种很棒的选择，因为原型本来就是存放复用成员的地方。在这种模式中，父构造函数添加到this中的任何成员都不会被继承。

## 总结
当你在用extends时，你的子类可以获得父类的所有property，和所有prototype，有重复的就会覆盖。听起来简单但实际过程挺复杂。完结散花～

编译后的代码源码
```
var __extends = (this && this.__extends) || (function () {
    var extendStatics = function (d, b) {
        extendStatics = Object.setPrototypeOf ||
            ({ __proto__: [] } instanceof Array && function (d, b) { d.__proto__ = b; }) ||
            function (d, b) { for (var p in b) if (b.hasOwnProperty(p)) d[p] = b[p]; };
        return extendStatics(d, b);
    };
    return function (d, b) {
        extendStatics(d, b);
        function __() { this.constructor = d; }
        d.prototype = b === null ? Object.create(b) : (__.prototype = b.prototype, new __());
    };
})();
var A = /** @class */ (function () {
    function A() {
        this.a = 123;
    }
    A.prototype.getA = function () { return this.a; };
    return A;
}());
var B = /** @class */ (function (_super) {
    __extends(B, _super);
    function B() {
        var _this = _super !== null && _super.apply(this, arguments) || this;
        _this.b = 321;
        return _this;
    }
    B.prototype.getB = function () { return this.b; };
    return B;
}(A));
var c = new B();
console.log('c', c);

```

## 补充
这里的extends和原生JS中的extends还是有区别的

When using the class keyword in TypeScript, you are actually creating two things with the same identifier:

    A TypeScript interface containing all the instance methods and properties of the class; and
    A JavaScript variable with a different (anonymous) constructor function type


## 参考
https://github.com/zhongsp/TypeScript/blob/master/doc/handbook/Classes.md