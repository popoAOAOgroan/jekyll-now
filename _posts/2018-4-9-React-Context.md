---
layout: post
title: React New API Context
---

### [官方文档翻译笔记](https://reactjs.org/docs/context.html)

#### React.createContext
```javascript
const {Provider, Consumer} = React.createContext(defaultValue);
```
当React渲染一个Consumer时，会从层级树中找到最近的一个匹配Provider，并获取context。
defaultValue是用来当你在渲染Consumer时没有在层级上层找到匹配的Provider，可以帮助你在无需嵌套的隔离环境下测试组件。

#### Provider
```javascript
<Provider value={/* some value */}>
```
Provider是一个React组件，允许Consumer们订阅context的变化。
接受一个value Prop传递给当前Provider的子Consumers。一个Provider可以被关联到多个Consumers。Provider们可以被嵌套并覆盖层级树中深处的value

#### Consumer
```javascript
<Consumer>
  {value => /* render something based on the context value */}
</Consumer>
```
Consumer也是一个React组件，并订阅context的变化。
接收一个function作为子节点，此function获取一个最近的context value并返回一个React节点。
传入的value参数等于树层级上最近的Provider中的value prop。如果没有这个context没有Provider，value将等于createrContext()中传入的defaultValue。

所有的Consumers会在Provider值变化时重绘。所有的变化会像[Object.is](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is#Description)比较新旧值的算法一样被检查。（而这可能会导致当你的value是一个Object：[详情](https://reactjs.org/docs/context.html#caveats)）

#### 注意事项
有时候当Provider的value值改变时会导致customer的无意识渲染，因为value是以指针引用地址判断的。所以你也许可以把Provider的value加到state里来避免这个问题。
```javascript
class App extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      value: {something: 'something'},
    };
  }

  render() {
    return (
      <Provider value={this.state.value}>
        <Toolbar />
      </Provider>
    );
  }
}
```