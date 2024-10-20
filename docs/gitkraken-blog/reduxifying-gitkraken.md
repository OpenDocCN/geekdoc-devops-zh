# 还原 GitKraken:从流动走向还原

> 原文：<https://www.gitkraken.com/blog/reduxifying-gitkraken>

[GitKraken](https://www.gitkraken.com/) 是一款 [React](https://facebook.github.io/react/) app。当我们从 Angular.js 迁移到 0.12.2 版本(2015 年 1 月)时，我们就一直使用 React。当我们开始使用 React 时，我们以脸书的 flux 库作为我们的状态模型进行架构，并稳步前进到 glory。一开始，还不错。*多表现。很多代码。哇！*

最初的兴奋消退了，蜜月也结束了。我们回顾了我们奇怪的混乱状态，并决定采取行动，以 Redux。公平地说，我们在扩展通量时遇到了问题。

GitKraken 团队现在正在完成从 Flux 到 Redux 的过渡，一切看起来真的很棒。当我们为应用程序编写新代码时，我们已经从 Redux 中看到了很多好处。

**还原与通量**

## 对于不熟悉 Redux，但熟悉 Flux 的人，可以把 Redux 看作是 Flux 的一个更严格的实现。有一个存储保存应用程序中的所有状态，而不是多个存储和一个调度程序将所有存储绑定在一起。

对于那些不熟悉这两个库的人来说，Flux 库是 Flux 模式的实现。流量模式类似于模型视图控制器(MVC)，但是有严格的单向数据流限制。

*通量模式*

在流程中的每一步，数据仅限于一个移动方向。视图可以应用户的请求启动一个操作，该操作可以生成新数据并将其传递给调度程序，调度程序将操作的结果分派给存储，然后存储可以发出对视图的更新。

在 Redux 库中，我们已经将总存储数量减少到一个，因此我们根本不需要调度程序。dispatcher 最初是用来管理我们用动作结果更新存储的顺序，并保持存储正常运行。相反，我们只是直接通知 Redux 存储已经发生的动作。因此，我们保留了流量模式的基本前提，但是通过合并调度程序和存储来缩减模式的流量。

*还原状态*

由于我们现在只有一个商店，我的第一反应是商店将是一个维护问题的巨大地狱，但我们实际上通过使用 Redux 的 reducer 模式为我们的分离关注点保持了一些表面上的秩序。Redux 的主要机制是 reducer 模式；我们有一个顶级缩减器，我们可以将子状态树分支成更小的缩减器。

缩减器是状态和状态间消息的纯函数。我们采用先前的状态树、消息，并应用状态的一些转换来产生新的状态树。顶层减速器具有这种形状，任何子减速器也具有这种形状。

减少的过程利用了我们在 Redux 状态上设置的约束，即它是不可变的。当一个消息被传递到 Redux land 时，该消息通过一系列这些 Redux 传递。这些 reducers 然后根据消息决定是否执行更新。

在我们的例子中，还原者决定是否产生一个全新的物体。为了澄清，我们沿着一条路径将每个引用更新到一个更新的值，这样我们没有对先前的状态树进行任何改变。当我们的归约器产生一个全新的对象时，我们*知道*那个特定的子状态树发生了变化。事实上，我们可以跟踪新对象引用到在前一个状态和下一个状态之间发生的一组确切的变化。

In our case, the reducers decide whether or not to produce a brand new object. To clarify, we update every reference along a path to an updated value, such that we have made no mutations to the previous state tree. When our reducers produce a brand new object, we *know* that changes happened to that particular substate tree. In fact, we can trace the new object references to the exact set of changes that have taken place between the previous state and the next state.

**Redux 的好处**

## 这种转换之所以如此好用，是因为每次只有一条消息以非常一致和直接的方式改变状态(a → b)。乍一看，将应用程序的所有状态提升到一个状态树中似乎有点令人生畏，但好处是一个惊人的权衡。

像时间旅行这样的事情可以用简单的方式实现(只需存储状态更新的序列)。将所有状态保存在一个不可变的数据结构中也允许 React to，呃， *react* 更好！当 Redux 存储发出更改时，我们可以利用引用透明性。React 可以在更新前执行检查，查看顶级对象引用是否已经更改，如果没有，则短路整个渲染树。

当我们围绕 Redux 状态组织视图时，我们用 Redux 构建的模式的另一个好处开始发挥作用。我们构建监听 Redux 状态的容器，当状态更新发生时，这些容器检索相关的更改并选择是否做出反应。这些容器将它们关心的任何状态传递给表示层。这些层很大程度上是无状态的视图组件(只接收道具的组件)。

好吧！好的。我所描述的好处是视图层通过顶层容器水平扩展，顶层容器保持了纯渲染树。当我们想要添加更多的容器时，我们可以以干净的方式完成(一个容器负责一个完整的视图)，即使每个容器都与同一个存储对话。我们基本上构建了一个架构，在伸缩时为每个新的 UI 容器添加一个连接。*真干净！*

这不是我们扩展得更好的唯一地方。Redux 状态本身按缩减器进行缩放。我们可以通过构建带有子状态的新 reducer 来增加总状态的大小，但是我们不必增加已经编写的 reducer 的复杂性，也不必像在 Flux 中那样管理显式的调度顺序。只有一个存储，我们的 reducers 同步运行，一次产生一个新的状态。

*缩放示例*

所以你有它。我们的架构现在在 GitKraken 中提供了更清晰的扩展体验。

So there you have it. We have an architecture that now provides a cleaner scaling experience in GitKraken.