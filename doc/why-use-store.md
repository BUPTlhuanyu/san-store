为什么要进行应用状态管理
======

使用 san-store 进行应用状态管理，就要先接受它的理念：

1. 单向流
2. 全局唯一的应用状态源
3. 状态更新模式单一，不能通过store直接更新应用状态

那么，使用 san-store 进行应用状态管理，和自己在组件里完成所有事情，有什么区别呢？


![why use](https://raw.githubusercontent.com/baidu/san-store/master/doc/why-use.png)

自己管理你的应用状态
--------

自己在组件里完成所有事情，意味着你需要自己管理你的应用状态。经验丰富的开发人员能够凭着设计经验和直觉让应用良构，但在不断的迭代与新需求开发的过程，他们需要持续的思考和回答这些问题：

1. 应用状态数据保存在一个顶层组件还是分散在各个组件内？
1. 应用状态数据怎么下发给需要使用的组件？
1. 某个子组件区域的应用状态是否需要和外部绝缘？
1. 如何更新应用状态数据？双向绑定还是消息通知？
1. 更深层次组件的交互行为如何通知保存应用状态的组件更新？

于是，我们很容易把应用做成上面的样子：

1. 数据的更新流自顶向下
2. 底层组件的用户交互通过双向绑定更新到上层组件，上层组件刷新所有子组件的视图，同时通过双向绑定继续往上更新
3. 底层组件的用户交互通过消息向上传递，顶层组件处理消息，更新自身状态数据，然后自顶向下更新

我们看到，向下的数据更新流和向上的更新流以及消息流是夹杂在一起的。如果管理得当，这样做其实并没有什么问题。但你需要小心翼翼的在不断增加的需求中维护应用的消息流转，你需要清醒的认识到每一个操作带来的状态变更最终将更新到哪个组件，再由它下发。有没有觉得心很累？

使用 san-store 管理应用状态
------

如果使用 san-store 进行应用状态管理，这个流会变得清晰很多：

1. 所有的应用状态保存在 store 中
2. 用户交互的唯一出口只有 dispatch action，不是更新双向绑定的上层组件，不是向上派发的消息
3. dispatch action 带来的应用状态变更，将更新到 connect 的组件中

从图中可以看到，组件树上的流将变得清晰，只有自顶向下的更新流。

是不是要使用 san-store
-------

我们并不认为 san-store 适合所有场景。统一的进行应用状态管理，只有当你的应用足够大时，它带来维护上的便利才会逐渐显现出来。如果你只是开发一个小系统，并且预期不会有陆续的新需求，那我们并不推荐你使用它。大多数增加可维护性的手段意味着拆分代码到多处，意味着你没有办法在实现一个功能的时候一路到尾畅快淋漓，意味着开发成本可能会上升。

所以，你应该根据你要做的是一个什么样的应用，决定要不要使用 san-store。


你需要做什么
------

san-store 只是提供了全局唯一状态管理和状态更新方式，你可以天然的实现单向流。但是更重要的是，你需要 `为自己的应用划分应用状态`。所以你至少需要考虑以下问题：

1. 你的应用有哪些业务场景？
2. 每个业务场景有哪些应用状态？
3. 不同业务场景之间的应用状态是否有相通复用的？
4. 应用状态数据应该保存成什么结构？

基于这些，你基本上可以划分应用状态，设计结构，并为它们起一些清晰的命名了。这都是让你的应用良构，你所必须做的设计。
