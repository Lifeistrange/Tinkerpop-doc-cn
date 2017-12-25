# 终端步骤

通常，当一个步骤链接到一个遍历时，将返回一个遍历。基于这个方式，一个遍历就可以以一个流畅的一元的方式构建。然而，有一些步骤并不返回遍历，而是执行遍历并返回其结果。这些步骤被称作终端步骤，我们会用以下的例子来说明他。

```groovy
gremlin> g.V().out('created').hasNext() //1\
==>true
gremlin> g.V().out('created').next() //2\
==>v[3]
gremlin> g.V().out('created').next(2) //3\
==>v[3]
==>v[5]
gremlin> g.V().out('nothing').tryNext() //4\
==>Optional.empty
gremlin> g.V().out('created').toList() //5\
==>v[3]
==>v[5]
==>v[3]
==>v[3]
gremlin> g.V().out('created').toSet() //6\
==>v[3]
==>v[5]
gremlin> g.V().out('created').toBulkSet() //7\
==>v[3]
==>v[3]
==>v[3]
==>v[5]
gremlin> results = ['blah',3]
==>blah
==>3
gremlin> g.V().out('created').fill(results) //8\
==>blah
==>3
==>v[3]
==>v[5]
==>v[3]
==>v[3]
```

1. `hasNext()`判断是否还有有效的结果。
2. `next()`将返回下一个结果。
3. `next(n)`将以列表的形式返回下`n`个结果。
4. `tryNext()`将返回一个`Optional`，因此，他是`hasNext()/next()`的组合。
5. `toList()`将以列表的形式返回所有结果。
6. `toSet()`将以集合的形式返回所有结果（当然会去重）。
7. `toBulkSet()`将以加权集合的形式返回所有的结果（因此会通过加权来保留）。
8. `fill(collection)`将所有的结果放入提供的容器中，并返回整个容器。

最后，`explain()`步骤也是一个终端步骤，我们会在他的章节再讨论。