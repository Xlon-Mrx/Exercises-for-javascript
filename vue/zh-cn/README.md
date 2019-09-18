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

---

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

  <img src="https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=3847201757,539796013&fm=26&gp=0.jpg" alt="MVC结构图"/>
  
  - `View`将用户触发事件指令转送到`Controller`
  - `Controller` 完成业务相应的逻辑处理后，再触发`Model`改变状态
  - `Model` 将更新后的数据发送到 `View`,实现用户反馈
  
  <img src="https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2606542962,1945093350&fm=26&gp=0.jpg" alt="MVVC结构图"/>
  
  - `view`-`viewModel` and `viewModel`-`Model`各部分之间通信都是双向的

</p>
</details>

---

###### 5. Vue 生命周期的理解?

<details>
<summary><b>Answer</b></summary>
<p>

  Vue 生命周期指的是一个实例从开始创建到销毁的这一过程。
  - `beforeCreated()`: 在实例创建之间执行，数据未加载状态（此时this为undefined）。
  - `created()`: 在实例创建、数据加载后，能初始化数据，DOM渲染之前执行（此时已存在this）。
  - `beforeMount()`: 虚拟DOM已创建完成，在数据渲染前最后一次操作数据。
  - `mounted()`: 页面、数据渲染完成，并且真实DOM挂载完成。
  - `beforeUpdate()`: 组件重新渲染之前执行。
  - `updated()`: 数据已经更改完成，DOM也重新render完成。此时更改数据会陷入死循环。
  - `beforeDestory()`: 实例销毁前执行（此时实例仍然存在）。
  - `destoryed()`: 实例被销毁后执行。

</p>
</details>

---

###### 6. 组件间如何通信?

<details>
<summary><b>Answer</b></summary>
<p>

  - ###### 父组件向子组件通信
    子组件通过`props`属性绑定父组件传递的数据并实现双方通信。

  - ###### 子组件向父组件通信
    子组件通过`$emit`方法向父组件传递数据。

  - ###### 非父子组件、兄弟组件之间通信
    ```javascript
      const bus = new Vue()

      // 绑定事件
      bus.$on('eventName', () => {
        // ...todo
      })

      // 触发事件
      bus.$emit('eventName', params)
    ```
</p>
</details>

---

###### 7. vue-router 路由如何实现?

<details>
<summary><b>Answer</b></summary>
<p>

    通过改变地址栏的路径地址来请求不同的资源，从而请求不同的页面或组件。

</p>
</details>

---

###### 8. $route 与 $router 有什么区别?

<details>
<summary><b>Answer</b></summary>
<p>

  `$router`是`VueRouter`实例， 可以通过内置方法$router.push(...)跳转到指定路由地址下，或通过$router.history.back()返回上一级路由。
  `$route`返回当前路由的相关参数，包括但不限于（`path`、`params`、`query`以及`name`）。

</p>
</details>

---

###### 9. Vue 中 $nextTick 的作用是什么?

<details>
<summary><b>Answer</b></summary>
<p>

   首先`$nextTick`是在下次DOM更新循环结束之后执行的回调，在修改data数据之后使用`$nextTick`方法此时该方法的回调中拿到的是最新的DOM。

</p>
</details>

---

###### 10. Vue 组件中 data 为什么要以函数形式展示?

<details>
<summary><b>Answer</b></summary>
<p>

  默认组件内部`data`使用的是函数并返回对象的形式书写的，首先如若使用对象形式那么就相当于是一个全局对象，并且在每一个引用了该组件之间进行共享，其中一个引用的地方更改了data数据，其余的任何引用过该组件的地方都会同时更改。而使用函数形式能够很好的做到局部作用不被共享。

</p>
</details>

---

###### 11. computed 计算属性和 method 事件有什么区别?

<details>
<summary><b>Answer</b></summary>
<p>

  从结果的角度来看两种方式都是相同的，不同点在于`computed`计算属性是基于它自身的依赖进行相应缓存的，只有相关的依赖（某个或多个数据）改变时才会重新计算并返回结果。而`method`方法只有通过重新渲染并且被调用了才会执行。

</p>
  
</details>

---

###### 12. Vue 中如何自定义指令或过滤器?

<details>
<summary><b>Answer</b></summary>
<p>

- 全局定义自定义指令或过滤器

  ```javascript
    Vue.directive('directive-name', {
      // 被绑定的指令插入dom时
      inserted: function (el) {
        // ...todo
      }
    })

    Vue.filter('filterName', (val) => {
      return // ...todo
    })
  ```

- 局部（单个组件内部）定义自定义指令或过滤器

  ```javascript
    directives: {
      'directive-name': {
        inserted: function () {
          // ...todo
        }
      }
    }

    filters: {
      'filterName': (val) => {
        return // ...todo
      }
    }
  ```
</p>
</details>

---

###### 13. `keep-alive`的作用以及存在的生命周期?

<details>
<summary><b>Answer</b></summary>
<p>

  `keep-alive` 是Vue内置的组件，可以使被包裹在该组件内部的组件避免被重新渲染。

  ```javascript
    <keep-alive>
      <component /> // 多个组件并且会被缓存
    </keep-alive>
  ```
</p>
</details>

---

###### 14. Vue 中 key 的作用是什么?

<details>
<summary><b>Answer</b></summary>
<p>

  `key`的特殊性主要运用在Vue的虚拟DOM算法上，使其内部能够更方便、更快捷的辨别新旧`VNodes`，它会基于`key`的变化重新排列元素顺序，并且移除`key`不存在的元素。如果不使用`key`，Vue会使用一种最大限度减少动态元素并且尽可能的尝试修复/再利用相同类型元素的算法。当然`key`是唯一的，重复的`key`会造成渲染错误。

</p>
</details>
