---
layout: post
title: FxLayout VS. Primeflex
---

**[Flex Layout](https://github.com/angular/flex-layout/wiki)**是基于Flex+mediaQuery的Angular模块，为复杂布局提供解决方案。不仅为使用者提供了自定义的布局API，和mediaQuery的Observables，还注入DOM flexbox-2016 css样式（并不知道这个是啥）。

**[Flex Grid CSS](https://github.com/primefaces/primeflex)**
Prime的FlexGrid是基于Flex的适用于多端设备的ui responsive layout工具库。它并不包含在PrimeNG里而是由PrimeFlex提供，并被在PrimeNG PrimeReact等框架里使用，可被单独引用。


## FxLayout API
### [Static API](https://github.com/angular/flex-layout/wiki/Declarative-API-Overview)

* Containers
    1. fxLayout[d]: ['row'(default)| 'column']
    2. fxLayoutGap[d]: 定义child的margin [%|px|vw|vh]
    3. fxLayoutAlign: 定义child的对齐方式，值为数组时可以分别定义x轴y轴的对齐方式，参数和css的justify-content一样。
* Child
    1. fxFlex[d]: fxFlex必须是fxLayout container的child。default为1.
    2. fxFlexOrder[d]: 排序方式。[int]
    3. fxFlexOffset[d]: child偏移量。
    4. fxFlexAlign[d]: child的y轴对齐方式。
    5. fxFlexFill[d]: child会撑满整个container
* Special
    1. fxShow[d]: 显示元素
    2. fxHide[d]: 隐藏元素
    3. ngClass[d]: @extends ngClass core
    4. ngStyle[d]: @extends ngStyle core
    5. imgSrc[d]: @extends src attribute

### [Responsive API](https://github.com/angular/flex-layout/wiki/Responsive-API)

TA！TA responsive API是FxLayout的强大特性，效果类似于bootstrap的栅格class。如 xs = 'screen and (max-width: 599px)'

_Example 1_
```
<div fxLayout='column' class="zero">

  <div fxFlex="33" [fxFlex.md]="box1Width" class="one" ></div>
  <div fxFlex="33" [fxLayout]="direction" fxLayout.md="row" class="two">

    <div fxFlex="22"    fxFlex.md="10px" fxHide.lg                       class="two_one"></div>
    <div fxFlex="205px" fxFlex.md="65"                                    class="two_two"></div>
    <div fxFlex="30px"  fxFlex.md="25"   fxShow [fxHide.md]="hideBox"   class="two_three"></div>

  </div>
  <div fxFlex class="three"></div>

</div>
```

_Example 2_
配合其他指令
```
<div fxShow fxHide.xs="false" fxHide.lg="true"> ... </div>
```
```
<div fxFlex="50%" fxFlex.gt-sm="100%"> ... </div>
```