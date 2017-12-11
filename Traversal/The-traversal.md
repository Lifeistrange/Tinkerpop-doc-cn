# 遍历

遍历最基础的是实现了接口`Iterator<E>`的类`Traversal<S,E>`，`S`是起点，`E`是终点。一个遍历过程由以下四种组件构成：

1. `Step<S,E>`:一个独立的函数，用来从`S`生成`E`。Steps在一个遍历中被连接起来。

2. `TraversalStrategy`:拦截方法，用来改变遍历的执行（比如查询重写）。

3. `TraversalSideEffects`:存储关于遍历的全局信息的键值对。

4. `Traverser<T>`:通过`Traversal`传播的代表`T`类型的对象。

经典的图遍历概念由`GraphTraversal<S,E>`提供，实现了`Traversal<S,E>`。`GraphTraversal`提供一个图数据的解释器，依据点，边等等，因此，成为了一种图遍历的DSL。

> 基本的步骤实现通过Tinkerpop提供，包括绝大部分的DSL的作者的函数式的需求。DSL作者利用所提供的步骤是很重要的，因为常见的优化和装饰策略可以由底层遍历序列进行推理。如果新的步骤引入，常见的遍历策略或许会无法正常使用。