---
layout: post
title: Js Event Loop
---

**JavaScript 异步单线程是如何工作的？** 最简短的回答是，Js这个语言是单线程的，而异步行为并不是js语言自己的行为，相反他是构建在浏览器（或编程环境）中的核心JavaScript语言之上，并通过浏览器API进行访问。

### Basic Architecture
![start/end](https://cdn-images-1.medium.com/max/1455/1*7GXoHZiIUhlKuKGT22gHmA.png)

看看浏览器主要的组成部分：
* Heap - 堆。对象被分配到堆中，这只是一个名称，表示一个很大的非结构化内存区域。
* Stack - 调用栈。这代表着提供给Js代码执行的单线程。函数从栈中被调用。
* Browser or Web APIs - （DOM，ajax，setTimeout）他们内置在您的浏览器中，并且能够公开来自浏览器和周围计算机环境的数据，并且能够用它来做有用的复杂的事情。他并不是js语言的一部分，相反他们被构建在核心js之上，给你的js代码提供强大的支持。例如，Geolocation API提供简单的js构建使你可以获取地理位置信心，因此你可以获取Google Map上你的位置。在后台，浏览器确使用了复杂的底层的代码（e.g. C++）来与设备的GPS硬件（或其他可以获取位置数据）联系，并返回给浏览器环境以得到数据。但再次说明，这一切你都只需通过api使用。

### Code Snippet 1: intrigue the mind
现在我们有2个函数调用了console.log


### 参考
https://medium.com/front-end-hacking/javascript-event-loop-explained-4cd26af121d4