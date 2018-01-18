# graph步骤

`V()`步骤常常被用来开始一个`GraphTraversal`，但是也可以在遍历中使用。

```groovy
gremlin> g.V().has('name', within('marko', 'vadas', 'josh')).as('person').
           V().has('name', within('lop', 'ripple')).addE('uses').from('person')
==>e[13][1-uses->3]
==>e[14][1-uses->5]
==>e[15][2-uses->3]
==>e[16][2-uses->5]
==>e[17][4-uses->3]
==>e[18][4-uses->5]
```

> 在遍历中的`V()`是否使用索引，取决于1.是否有合适的索引存在2.是否特定的图系统实现了这个功能。

```groovy
gremlin> g.V().has('name', within('marko', 'vadas', 'josh')).as('person').
           V().has('name', within('lop', 'ripple')).addE('uses').from('person').toString() //1\
==>[GraphStep(vertex,[]), HasStep([name.within([marko, vadas, josh])])@[person], GraphStep(vertex,[]), HasStep([name.within([lop, ripple])]), AddEdgeStep({~from=[[SelectOneStep(person)]], label=[uses]})]
gremlin> g.V().has('name', within('marko', 'vadas', 'josh')).as('person').
           V().has('name', within('lop', 'ripple')).addE('uses').from('person').iterate().toString() //2\
==>[TinkerGraphStep(vertex,[name.within([marko, vadas, josh])])@[person], TinkerGraphStep(vertex,[name.within([lop, ripple])]), AddEdgeStep({~from=[[SelectOneStep(person)]], label=[uses]})]
```

1. 普通的`V()`步骤会迭代生成所有的顶点。然而，图策略可以吧容器折叠进一个`GraphStep`来使用索引。
2. 图系统是否会提供遍历中的`V()`的索引使用，可以通过使用`toString()`输出生成的遍历。如果`has`条件被折叠进了`V()`步骤，那么就会使用存在的索引。