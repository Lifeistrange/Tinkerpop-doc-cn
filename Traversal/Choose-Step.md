# choose步骤

`choose()`步骤(branch)引导目前的遍历到一个特殊的遍历分支上。`choose()`能够实现if/then/else等复杂语法。

```groovy
gremlin> g.V().hasLabel('person').
               choose(values('age').is(lte(30)),
                 __.in(),
                 __.out()).values('name') //1\
==>marko
==>ripple
==>lop
==>lop
gremlin> g.V().hasLabel('person').
               choose(values('age')).
                 option(27, __.in()).
                 option(32, __.out()).values('name') //2\
==>marko
==>ripple
==>lop
```

1. 如果遍历到一个元素，那么就做`in`，否则做`out`（以真假为基础的选择）。
2. 使用遍历的结果作为键来做遍历的映射选项。(以值为基础的选项)。

如果“false”分支没有提供，if/then的语法就实现了。

```groovy
gremlin> g.V().choose(hasLabel('person'), out('created')).values('name') //1\
==>lop
==>lop
==>ripple
==>lop
==>ripple
==>lop
gremlin> g.V().choose(hasLabel('person'), out('created'), identity()).values('name') //2\
==>lop
==>lop
==>ripple
==>lop
==>ripple
==>lop
```

1. 如果顶点是一个人，返回顶点所创建的点，否则返回这个顶点。
2. if/then/else使用`identity()`在false分支等于if/then的非false分支。

记住，`choose()`可以有任意数量的选项，甚至可以使用一个匿名的遍历作为选择函数。

```groovy
gremlin> g.V().hasLabel('person').
               choose(values('name')).
                 option('marko', values('age')).
                 option('josh', values('name')).
                 option('vadas', valueMap()).
                 option('peter', label())
==>29
==>[name:[vadas],age:[27]]
==>josh
==>person
```

`choose()`步骤可以利用`Pick.none`来做选项匹配。在其他选项都匹配不到时，`none`选项就会被使用。

```groovy
gremlin> g.V().hasLabel('person').
               choose(values('name')).
                 option('marko', values('age')).
                 option(none, values('name'))
==>29
==>vadas
==>josh
==>peter
```

