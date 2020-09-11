* `Vue`响应式原理

> 首先在`Observer`中使用`Object.defineProperty`对数据对象所有属性进行`get`、`set`劫持。在对属性进行劫持的时候，会实例化一个`dep`用来存储该属性`watcher`依赖以及在数据更新的时候用来通知`watcher`更新视图。组件在`mountding`阶段会将`template`编译成一个`render`函数，`render`函数中会读取数据属性，这时候就会触发该属性的`get`操作并进行依赖收集。当该属性发生变化，会触发该属性的`set`操作，在`set`中会调用`dep`的`notify`方法来通知收集的`watcher`进行视图更新。

* `Vue`是如何观察数组变化

> `Vue`重写了一些数组方法，方法中首先会执行数组原始的方法，然后调用`dep`的`notify`方法通知`watcher`更新视图，如果是往数组中添加数据还会对这些新增的数据进行响应式处理。如果数组中的数据是对象，就会遍历这些对象进行响应式处理

* 为什么`Vue`会采用异步渲染

> 在`Vue`组件中可能会存在大量的数据，如果采用同步渲染，当这些数据在同一个事件循环中被多次更改，就会触发多次`watcher`，导致组件的多次渲染和一些不必要的计算。带来性能问题。`Vue`在侦听到数据的变更，将会开启一个队列，并缓冲同一事件循环中发生的所有数据变更，同一个`watcher`被多次触发，只会被推入到队列一次。然后在下一次事件循环中去刷新队列并重新渲染组件。在异步队列中，`Vue`内部会尝试使用`Promise.then`、`MutationObserver`、`setImmediate`，如果不支持，则使用`setTimeout`。

* `coumpted`的原理

> `coumpted`本质上就是一个`watcher`,这个`watcher`的属性`lazy`为`true`。代表这个`watch`是一个计算`watcher`。默认属性`dirty`为`true`。只有`dirty`为`true`的时候才会去对这个数据进行求值计算。然后对这个`computed`进行`Object.defineProperty`劫持进行依赖收集。`get`的时候回判断`watcher`的`dirty`是否为`true`，如果为`true`则执行求值计算，并缓存计算后的值，否则直接返回缓存中的的值。当依赖的数据发生变更，会先将这个`watcher`的`dirty`的值设为`true`，以便`get`中去重新计算新的值。并将`watcher`的`dirtry`设为`false`。