# timeLimit步骤

在许多情况中，图遍历需要的结果不是一个确切的答案，而是一个相对的排序。一个经典的例子就是推荐，他需要一个相关的顶点排序，而不是确切的顺序。他也可能需要不超过2毫秒就返回数据。在这种情况下`timeLimit()`步骤(filter)就能派上用场了。

> clock(int runs, Closure code)方法是Gremlin Console预加载的工具，用来计算代码的执行时间。

```groovy
gremlin> g.V().repeat(both().groupCount('m')).times(16).cap('m').order(local).by(values,decr).next()
==>v[1]=2744208
==>v[3]=2744208
==>v[4]=2744208
==>v[2]=1136688
==>v[5]=1136688
==>v[6]=1136688
gremlin> clock(1) {g.V().repeat(both().groupCount('m')).times(16).cap('m').order(local).by(values,decr).next()}
==>2.498806
gremlin> g.V().repeat(timeLimit(2).both().groupCount('m')).times(16).cap('m').order(local).by(values,decr).next()
==>v[1]=2744208
==>v[3]=2744208
==>v[4]=2744208
==>v[2]=1136688
==>v[5]=1136688
==>v[6]=1136688
gremlin> clock(1) {g.V().repeat(timeLimit(2).both().groupCount('m')).times(16).cap('m').order(local).by(values,decr).next()}
==>2.0562
```

本质上，相对的顺序也是有价值的，即使没有遍历所有的节点。最直接的好处就是计算能够保证在指定时间内完成。我们需要注意，`timeLimit()`步骤的计时是从第一个遍历器进入开始的。当时限达到时，`next()`操作会生成`NoSuchElementException`所有的`hasNext()`操作都会返回`false`。