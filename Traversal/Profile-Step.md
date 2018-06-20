# profile步骤

`profile()`步骤(sideEffect)可以让开发者查看他们的遍历的静态信息，比如步骤运行时间，计数等。

> 分析遍历将降低遍历的性能。这种开销已经被排除在分析结果之外，但运行时间依然不精确。因此，最好关联起来考虑步骤的运行时间。

```groovy
gremlin> g.V().out('created').repeat(both()).times(3).hasLabel('person').values('age').sum().profile()
==>Traversal Metrics
Step                                                               Count  Traversers       Time (ms)    % Dur
=============================================================================================================
TinkerGraphStep(vertex,[])                                             6           6           0.120    13.21
VertexStep(OUT,[created],vertex)                                       4           4           0.125    13.77
NoOpBarrierStep(2500)                                                  4           2           0.079     8.69
VertexStep(BOTH,vertex)                                               10           4           0.048     5.38
NoOpBarrierStep(2500)                                                 10           3           0.022     2.49
VertexStep(BOTH,vertex)                                               24           7           0.029     3.24
NoOpBarrierStep(2500)                                                 24           5           0.028     3.10
VertexStep(BOTH,vertex)                                               58          11           0.043     4.83
NoOpBarrierStep(2500)                                                 58           6           0.047     5.17
HasStep([~label.eq(person)])                                          48           4           0.084     9.30
PropertiesStep([age],value)                                           48           4           0.091    10.10
SumGlobalStep                                                          1           1           0.188    20.72
                                            >TOTAL                     -           -           0.909        -
```

`profile()`步骤生成了一个`TraversalMetrics`副作用对象，他包含以下信息：

- `Step`: 遍历中的步骤
- `Count`: 遍历器通过这个步骤的数量
- `Traversers`: 通过过这个步骤的遍历器的数量
- `Time (ms)`: 这个步骤所花费的所有的时间
- `% Dur`: 这个步骤占整体的时间的百分比

理解`Count`和`Traversers`之间的差别十分重要。遍历器可以合并，因此，当两个遍历器相同时，他们就可能被合并到一个遍历器。新的遍历器有一个`Traverser.bulk()`为这两个遍历器各自大小的和。另一方面，`Count`展示的就是`Traverser.bulk()`的结果，因此，他表示的就是展示到的遍历器的数量。`Traversers`总是小于等于`Count`。

有一个副作用的键可以传给`profile()`步骤来迭代遍历结果并检索`TraversalMetrics`指标。

```groovy
gremlin> t = g.V().out('created').profile('metrics')
==>v[3]
==>v[3]
==>v[3]
==>v[5]
gremlin> t.iterate()
gremlin> metrics = t.getSideEffects().get('metrics')
==>Traversal Metrics
Step                                                               Count  Traversers       Time (ms)    % Dur
=============================================================================================================
TinkerGraphStep(vertex,[])                                             6           6           0.095  -293.37
VertexStep(OUT,[created],vertex)                                       4           4          -0.145   449.19
NoOpBarrierStep(2500)                                                  4           2           0.018   -55.81
                                            >TOTAL                     -           -          -0.032        -
```

