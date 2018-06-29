---
layout: post
title: Skeleton CSS 学习笔记
---


## The Basics

### difference between :blank and :empty
:empty, :blank will select elements that contain nothing at all, or contain only an HTML comment. But, :blank will also select elements that include whitespace, which :empty will not.

### :root
In HTML, :root represents the <html> element and is identical to the selector html, except that its specificity is higher.
:root can be useful for declaring global CSS variables:

### [custom properties (variables)](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_variables)
CSS variables are entities defined by CSS authors that contain specific values to be reused throughout a document. 
#### calc计算函数
```
--footer-position: 0 calc(var(--card-height) - var(--footer-height));
```
#### preprocesser VS custom properties
  *  You can use them without the need of a preprocessor.
  *  They cascade. You can set a variable inside any selector to set or override its current value.
  *  When their values change (e.g. media query or other state), the browser repaints as needed.
  *  You can access and manipulate them in JavaScript.


### radial-gradient
Its shape may be a circle or an ellipse. 

[参考](https://css-tricks.com/building-skeleton-screens-css-custom-properties/)