---
layout: post
title: How computed properties work
---

* this的值取决于函数被执行时的上下文。
* 在全局域里，this指向全局对象（window）。当使用了new关键词（一个构造器），this将被绑定到新的对象上。
* 我们可以通过call(), bind(), apply()来手动设置this的指向。
* Arrow Functions don’t bind this — instead this is bound lexically (i.e. based on the original context)



### 参考
https://codeburst.io/javascript-the-keyword-this-for-beginners-fb5238d99f85