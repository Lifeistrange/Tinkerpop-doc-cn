# 图遍历步骤

![](image/step-types.png)

一个`GraphTraversal<S,E>`在`GraphTraversalSource`中调用。他也可以匿名的被调用通过`__`。一个图遍历由由一系列有序的步骤组成。所有的步骤都由`GraphTraversal`继承自更加通用的图格式提供。TinkerPop3的[GraphTraversal JavaDoc](http://tinkerpop.apache.org/javadocs/3.2.6/core/org/apache/tinkerpop/gremlin/process/traversal/dsl/graph/GraphTraversal.html)提供了所有的步骤列表（与描述）。后面的小节将使用[Gremlin Console](/Gremlin-applications/Gremlin-Console.html)来演示图遍历的步骤。

> 基础的遍历教程在[The Graph Process](/Intruduction/The-Graph-Process.html)或者[Getting Started](http://tinkerpop.apache.org/docs/3.2.6/tutorials/getting-started/)

> 为了减少冗长的表达，最好使用`import static org.apache.tinkerpop.gremlin.process.traversal.dsl.graph..*`，这样能够用`inE()`代替在匿名遍历中使用`.inE()`。同时也要注意当使用匿名遍历时语言的保留关键字。举例来说，`in`和`as`在Groovy中为保留关键字，因此你必须要使用复杂的语法`.in()`与`.as()`来避免冲突。

