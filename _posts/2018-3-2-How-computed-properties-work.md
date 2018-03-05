---
layout: post
title: How computed properties work
---

[原文](https://skyronic.com/blog/vuejs-internals-computed-properties)


通过简单例子学习Vue Computed属性的工作原理
--

### JS Properties
js中又一个叫```Object.defineProperty```的属性。他可以干很多事！但我们只需要知道：

```javascript
var person = {};

Object.defineProperty (person, 'age', {
  get: function () {
    console.log ("Getting the age");
    return 25;
  }
});

console.log ("The age is ", person.age);

// Prints:
//
// Getting the age
// The age is 25

```
虽然```person.age```看起来就像获取一个对象的属性，但我们实际上是运行了一个function。

### 一个基础Vue.js观察者
Vue.js的基本构造可以让你把一个普通对象变成一个“可观察的”值，称为“观察者”。这里实现了一个超简单的版本来增加一个响应式属性。
```javascript
function defineReactive (obj, key, val) {
  Object.defineProperty (obj, key, {
    get: function () {
      return val;
    },
    set: function (newValue) {
      val = newValue;
    }
  })
};

// 创建一个person对象
var person = {};

// 增加一个响应式熟悉 'age' 和 'country'
defineReactive (person, 'age', 25);
defineReactive (person, 'country', 'Brazil');

// 现在你可以从任何地方调用它
if (person.age < 18) {
  return 'minor';
}
else {
  return 'adult';
}

// set一个值
person.country = 'Russia';
```
有趣的是，真正的值25和'Brazil'仍旧在'闭包变量'里，并且在你set value的时候会被修改。person.country并不包含真正的值，包含值的是getter方法内的闭包。

### 定义一个计算属性
创建一个函数defineComputed来定义一个计算属性。下面是用法：
```javascript
defineComputed (
  person, // the object to create computed property on
  'status', // the name of the computed property
  function () { // the function which actually computes the property
    console.log ("status getter called")
    if (person.age < 18) {
      return 'minor';
    }
    else {
      return 'adult';
    }
  },
  function (newValue) {
    // called when the computed value is updated
    console.log ("status has changed to", newValue)
  }
});

// We can use the computed property like a regular property
console.log ("The person's status is: ", person.status);
```
现在，让我们写个简单的defineComputed来实现功能。将会支持呼叫compute函数，我们暂时不支持updateCallback。
```javascript
function defineComputed (obj, key, computeFunc, updateCallback) {
  Object.defineProperty (obj, key, {
    get: function () {
      // call the compute function and return the value
      return computeFunc ();
    },
    set: function () {
      // don't do anything. can't set computed funcs
    }
  })
}
```
这里有一些问题：
  * 每次这个属性被访问的时候都将运行计算函数
  * 不知道什么时候会更新

```javascript
// We want something like this to happen

person.age = 17;
// console: status has changed to: minor

person.age = 22;
// console: status has changed to: adult
```
### 增加一个依赖追踪器
现在加一个全局对象Dep
```javascript
var Dep = {
  target: null
};
``` 
This is the 'dependency tracker'. Let's modify the defineComputed function with one key trick.
这就是'依赖追踪器'。现在让我们用这个关键的小技巧修改一下defineComputed函数。
```javascript
function defineComputed (obj, key, computeFunc, updateCallback) {
  var onDependencyUpdated = function () {
    // TODO
  }
  Object.defineProperty (obj, key, {
    get: function () {
      // 增加一个依赖跟踪
      Dep.target = onDependencyUpdated;
      var value = computeFunc ();
      Dep.target = null;
    },
    set: function () {
      // 啥都不干，计算属性没有set
    }
  })
}
``` 
现在，让我们回到reactive属性
```javascript
function defineReactive (obj, key, val) {
  // 这个key的所有依赖方法集合
  var deps = [];

  Object.defineProperty (obj, key, {
    get: function () {
      // 检查是否存在Dep
      // 并且不在已有的依赖中
      if (Dep.target && && deps.indexOf (Dep.target) == -1) {
        // 增加一个依赖
        deps.push (target);
      }

      return val;
    },
    set: function (newValue) {
      val = newValue;

      // 通知所有依赖属性更新
      deps.forEach ((changeFunction) => {
        // 请求并通知更新
        changeFunction ();
      });
    }
  })
};
```
我们可以把计算属性中的onDependencyUpdated函数更新一下，现在它可以追踪更新回调了！
```javascript
var onDependencyUpdated = function () {
  // 再一次计算一下值
  var value = computeFunc ();
  updateCallback (value);
}
```
### 合体
让我们重新来访问一下person.status
```javascript
person.age = 22;

defineComputed (
  person,
  'status',
  function () {
    // 计算方法
    if (person.age > 18) {
      return 'adult';
    }
  },
  // 更新回调
  function (newValue) { 
    console.log ("status has changed to", newValue)
  }
});

console.log ("Status is ", person.status);
```