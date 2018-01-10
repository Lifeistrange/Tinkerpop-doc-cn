# barrier步骤

`barrier()`步骤将遍历从一个懒处理管道转如批量同步处理管道。这个步骤在以下情况中很有用：

* 当`barrier()`前的步骤需要在`barrier()`之后的步骤前先执行完。
* 当”慢速“遍历多次重复访问相同元素导致其效率低下，需要为这种情况做”批量优化“遍历。

```groovy
gremlin> g.V().sideEffect{println "first: ${it}"}.sideEffect{println "second: ${it}"}.iterate()
first: v[1]
second: v[1]
first: v[2]
second: v[2]
first: v[3]
second: v[3]
first: v[4]
second: v[4]
first: v[5]
second: v[5]
first: v[6]
second: v[6]
gremlin> g.V().sideEffect{println "first: ${it}"}.barrier().sideEffect{println "second: ${it}"}.iterate()
first: v[1]
first: v[2]
first: v[3]
first: v[4]
first: v[5]
first: v[6]
second: v[1]
second: v[2]
second: v[3]
second: v[4]
second: v[5]
second: v[6]
```

”批量优化“背后的理论很简单。如果在顶点1有一百万个遍历，那么我们并不需要真的去计算一百万次。取而代之的，使用一个遍历`Traverser.bulk()`即可代表这一百万的遍历。批量优化的好处在巨大的图中会更为突出。因此，下面的例子基于一个巨大的图：

```groovy
gremlin> graph = TinkerGraph.open()
==>tinkergraph[vertices:0 edges:0]
gremlin> graph.io(graphml()).readGraph('data/grateful-dead.xml')
gremlin> g = graph.traversal().withoutStrategies(LazyBarrierStrategy) //1\
==>graphtraversalsource[tinkergraph[vertices:808 edges:8049], standard]
gremlin> clockWithResult(1){g.V().both().both().both().count().next()} //2\
==>10503.063725
==>126653966
gremlin> clockWithResult(1){g.V().repeat(both()).times(3).count().next()} //3\
==>19.157953
==>126653966
gremlin> clockWithResult(1){g.V().both().barrier().both().barrier().both().barrier().count().next()} //4\
==>19.658645
==>126653966
```

1. 在批量优化中，很明显`LazyBarrierStrategy`会被忽略。
2. 在非批量遍历中，每一个遍历都会执行。
3. 每一个遍历进入`repeat()`都会递归处理。
4. 批量遍历中相同的遍历不会执行。

如果给`barrier()`一个整型参数，隔离将只保持`n`个独立的遍历，在隔离的结果流入下一个步骤前。这对于对批量优化做分析时防止内存泄漏等危险很有用。

`LazyBarrierStrategy`插入`barrier()`步骤到每一个遍历间来做“批量优化”。

```groovy
gremlin> graph = TinkerGraph.open()
==>tinkergraph[vertices:0 edges:0]
gremlin> graph.io(graphml()).readGraph('data/grateful-dead.xml')
gremlin> g = graph.traversal() //1\
==>graphtraversalsource[tinkergraph[vertices:808 edges:8049], standard]
gremlin> clockWithResult(1){g.V().both().both().both().count().next()}
==>15.189326999999999
==>126653966
gremlin> g.V().both().both().both().count().iterate().toString() //2\
==>[TinkerGraphStep(vertex,[]), VertexStep(BOTH,vertex), NoOpBarrierStep(2500), VertexStep(BOTH,vertex), NoOpBarrierStep(2500), VertexStep(BOTH,edge), CountGlobalStep]
```

1. `LazyBarrierStrategy`为默认策略，不需要显式激活。
2. `LazyBarrierStrategy`启用时，`barrier()`步骤会自动插入到适当的位置。