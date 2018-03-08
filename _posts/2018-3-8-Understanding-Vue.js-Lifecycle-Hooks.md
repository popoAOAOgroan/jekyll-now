---
layout: post
title: Understanding Vue.js Lifecycle Hooks
---

[原文](https://alligator.io/vuejs/component-lifecycle/)


> 对每个组件来说生命周期钩子都是非常重要的。你常常需要知道你的组件是什么时候被创建，增加到DOM，更新和销毁。生命周期能让你了解你所用框架的幕后是如何运行的，但也让菜鸟们望而却步。

不过还好，看下面这张流程图，还是很方便去理解的。
![vue lifecycle](https://d33wubrfki0l68.cloudfront.net/435786c6cbd23e078c35c2b21f40e1756b2c3d30/2098f/images/vuejs/external/component-lifecycle.png)

## Creation (初始化)
creation的钩子们是你组件中第一个运行的钩子。在这里你可以在组件被加到DOM前执行一些动作。不像其他的钩子们，创建钩子同样也可以运行在SSR中（[server-side rendering](https://vuejs.org/v2/guide/ssr.html)）。

如果你希望你的组件同时在客户端和服务端都做一些事情时可以用这些钩子。但你没办法在这里访问DOM或者触发 _挂载_ 元素（this.$el）。

### beforeCreate 
beforeCreate是组件创建最初期运行的钩子。此时，data还没被创建为响应式([reactive](https://vuejs.org/v2/guide/reactivity.html))，事件也还没被准备好。

Example:
```javascript
<script>
export default {
  beforeCreate() {
    console.log('Nothing gets called before me!')
  }
}
</script>
```
### created
在这个勾子中你已经可以访问data以及events了！但template和虚拟DOM还没有被挂载和渲染。

Example:
```javascript
<script>
export default {
  data() {
    return {
      property: 'Blank'
    }
  },

  computed: {
    propertyComputed() {
      console.log('I change when this.property changes.')
      return this.property
    }
  },

  created() {
    this.property = 'Example property update.'
    console.log('propertyComputed will update, as this.property is now reactive.')
  }
}
</script>
```

## Mounting(插入DOM)
mounting的狗子们无论好坏 也算是比较常用的钩子了。他们允许你在第一次渲染后即刻访问到你的组件。然而，他们不能在SSR中使用。

正确使用场景：需要在第一次渲染前后立即访问或修改你组件的DOM。

错误使用场景：你需要为你的组件在初始化时fetch一些数据。使用created（或created+组件keep-alive中的activated）代替，特别你在SSR中也需要这个数据。

### beforeMount
这个钩子将会在第一次渲染发生前，template或渲染方法编译后触发。差不多可以说，这玩意儿你永远都用不到。记住，SSR中不会触发。

Example:
```javascript
<script>
export default {
  beforeMount() {
    console.log(`this.$el doesn't exist yet, but it will soon!`)
  }
}
</script>
```

### mounted
在这个钩子中你可以自由访问相应式component，template和渲染DOM(via. this.$el)。mounted是一个比较常用的生命周期钩子。常用在给组件fetching数据（可以使用created代替），或修改DOM，使用一些非Vue的库。
当你的组件上被挂上了v-if，每次发生改变，都会触发mounted。

Example:
```javascript
<template>
  <p>I'm text inside the component.</p>
</template>

<script>
export default {
  mounted() {
    console.log(this.$el.textContent) // I'm text inside the component.
  }
}
</script>
```

## Updating（Diff & Re-render）
更新钩子是当组件中任一个相应式属性发生改变时或是其他任何导致组件重新渲染时触发，在这里你可以观察到组件 _watch-compute-render_ 的周期。

正确使用场景：你需要知道你的组建何时re-renders，比如debugging和优化性能时。

错误使用场景：你想知道你组件中的某个相应式属性是什么时候发生了变化。可以用 _computed properties_ 或 _watchers_ 代替。

## beforeUpdate
这个钩子将在组件数据发生变化后触发，并开始整个更新周期，一切都在DOM修改和re-rendered之前！正在真正重绘前，你可以在这里获取到组件中响应式数据的最新状态。

Example：
```javascript
<script>
export default {
  data() {
    return {
      counter: 0
    }
  },

  beforeUpdate() {
    console.log(this.counter) // Logs the counter value every second, before the DOM updates.
  },

  created() {
    setInterval(() => {
      this.counter++
    }, 1000)
  }
}
</script>
```

### updated
这个勾子呢会在你的组件和DOM重新被渲染后调用。如果你需要在某属性发生变化后访问DOM，在这里就比较何时。

Example：
```javascript
<template>
  <p ref="dom-element">{{counter}}</p>
</template>
<script>
export default {
  data() {
    return {
      counter: 0
    }
  },

  updated() {
    // Fired every second, should always be true
    console.log(+this.$refs['dom-element'].textContent === this.counter)
  },

  created() {
    setInterval(() => {
      this.counter++
    }, 1000)
  }
}
</script>
```

## Destruction（Teardown卸载）
当你需要在组件被销毁时运行一些动作时可以用到销毁狗子们，例如清理和发送分析数据。
当你的组件被卸载或被从DOM中移除时触发。
如果v-if会导致销毁吗？

### beforeDestroy
卸载前触发。你的组件将任然十分完整和可用。如果你需要清理事件或相应式订阅，就应该在此刻进行。

Example：
```javascript
<script>
export default {
  data() {
    return {
      someLeakyProperty: 'I leak memory if not cleaned up!'
    }
  },

  beforeDestroy() {
    // Perform the teardown procedure for someLeakyProperty.
    // (In this case, effectively nothing)
    this.someLeakyProperty = null
    delete this.someLeakyProperty
  }
}
</script>
```

### destroyed
啥都不会留下，任何东西都被销毁了！你可能想在最后时刻做一下清理或通知后台服务器你的组件被销毁了！

Example：
```javascript
<script>
import MyCreepyAnalyticsService from './somewhere-bad'

export default {
  destroyed() {
    console.log(this) // There's practically nothing here!
    MyCreepyAnalyticsService.informService('Component destroyed. All assets move in on target on my mark.')
  }
}
</script>
```

## Other Hooks
其实还有2个狗子，activated和deactivated，你会在keep-alive中用到他们，他们允许你获取在组件被<keep-alive>包裹下时进入或离开的状态。你可以用他们fetch数据或处理状态改变，将比在created和beforeDestroy中处理要有效，因为不需要做整个组件的重新构建。