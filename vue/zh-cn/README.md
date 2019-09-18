###### 1. Vue 数据双向绑定原理是什么?

<details>
<summary><b>Answer</b></summary>
<p>

  Vue 数据双向绑定实现原理: 采用数据劫持并结合"发布者-订阅者"模式的方法实现，主要通过`Object.defineProperty()`来劫持各个属性的`getter`、`setter`，
  在数据变动的时候发布消息给订阅者并触发相应的监听回调。

</p>
</details>

---

###### 2. 如何解释单向数据流和双向数据绑定?

<details>
<summary><b>Answer</b></summary>
<p>

  单项数据流：顾名思义数据流是单向的。
             优点：数据流动方向可以跟踪，流动单一，追查问题的时候可以更快捷。
             缺点：维护起来不是很方便，要使view层发生变更就必需创建各种`action`来维护、操作相应的`state`。
  
  双向数据绑定：数据是相通的`view`-`viewModal`之间双向绑定，将数据变更的操作隐藏在框架内部。
             优点：在表单数据交互过多的场景下，会大大简化不必要的代码。
             缺点：无法追踪局部状态的变化，排查问题 debug 起来相对比较麻烦。

</p>
</details>

###### 3. Vue 如何去除URL中的`#`?

<details>
<summary><b>Answer</b></summary>
<p>

  `vue-router`默认使用的是`hash`模式, 该模式下在路由被加载时，项目URL后面会默认跟上一个`#`，如果不想让路由后面自带`#`，可以使用路由另一种模式`history`。
  
  ```javascript
    new Router({
      // 模式
      mode: 'history',
      routes: []
    })
  ```
  
  当使用`history`模式需要注意的是，因为vue构建的项目是单页面应用，所以当路由进行跳转的时候，就会出现访问不到静态资源（即`404`）的情况，遇到这种情况要解决
  该问题就需要服务端增加一个能够覆盖所有情况的候选资源，如果当前访问的URL匹配不到任何的静态资源，则应该返回同一个页面或指定一个页面。

</p>
</details>

---

###### 4. 如何理解`MVC`、`MVVC`?

<details>
<summary><b>Answer</b></summary>
<p>

  <img src="" alt="MVC结构图"/>
  
  - `View`将用户触发事件指令转送到`Controller`
  - `Controller` 完成业务相应的逻辑处理后，再触发`Model`改变状态
  - `Model` 将更新后的数据发送到 `View`,实现用户反馈
  
  <img src="" alt="MVVC结构图"/>
  
  - `view`-`viewModel` and `viewModel`-`Model`各部分之间通信都是双向的

</p>
</details>
