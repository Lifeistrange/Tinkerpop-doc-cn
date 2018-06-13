# or步骤

`or()`步骤会保证所提供的遍历中至少有一个会返回结果(filter)。

```groovy
gremlin> g.V().or(
            __.outE('created'),
            __.inE('created').count().is(gt(1))).
              values('name')
==>marko
==>lop
==>josh
==>peter
```

`or()`步骤可以接受任意数量的遍历。至少有一个遍历要返回至少一个输出来将结果传递给下一个步骤。

我们也可以使用中缀表示法，不过中缀表示法只支持两个遍历。

```groovy
gremlin> g.V().where(outE('created').or().outE('knows')).values('name')
==>marko
==>josh
==>peter
```

