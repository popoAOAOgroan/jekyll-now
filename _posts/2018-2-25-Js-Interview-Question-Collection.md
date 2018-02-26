---
layout: post
title: Js Interview Question Collection
---

## Difference between: function Person(){}, var person = Person(), and var person = new Person()?
[原文](https://medium.com/@rlynjb/js-interview-question-difference-between-function-person-var-person-person-and-var-ab6eb8c9ae88)
```javascript
  function Person(){}
```
### 函数定义
上面的代码声明了一个函数，但未执行，然而他会被注册斤全局命名空间。
```javascript
  var person = Person()
```
### 函数表达式
一个变量'var person'被定义，并且包含了一个到Person函数的引用值。任何JS表达式，包括函数表达式，永远返回一个值。如果没有名字，但通过一个括号包起来就是一个匿名函数。
```javascript
  var person = new Person()
```
### 函数构造器
通过增加了new关键字。实例化了一个新的Person类对象。一个函数申明通常只是一个函数，除非它被实例化，它就成了一个类构造函数。