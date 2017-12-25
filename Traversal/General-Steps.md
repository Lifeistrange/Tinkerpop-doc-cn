# 一般步骤

这里有五种步骤，每一种都有一个遍历与一个匿名表达式，通过描述其他的具体步骤来扩展。

| 步骤                                       | 描述                         |
| ---------------------------------------- | -------------------------- |
| `map(Traversal<S,E>)` `map(Function<Traverser<S>,E>)` | 为下一步的过程，映射遍历到`E`类型对象上。     |
| `flatMap(Traversal<S,E>)` `flatMap(Function<Traverser<S>,Iterator<E>>)` | 映射遍历到一个`E`类型对象的生成器上，流入下一步。 |
| `filter(Traversal<?,?>)` `filter(Predicate<Traverser<S>>)` | 映射遍历到真或假，为假时不会遍历至下一步。      |
| `sideEffect<Traversal<S,S>>` `sideEffect(Consumer<Traverser<S>>)` | 在遍历上执行一些操作后进入下一步。          |
| `branch(Traversal<S,M>)` `branch(Function<Traverser<S>,M>)` | 分割遍历器至所有的被`M`标记索引的遍历       |

> 匿名步骤提出的教育目的左右他们所代表的Gremlin语言的基础结构。在实践中，匿名步骤应该避免在遍历中使用，并且遍历验证策略不允许使用，除非明确关闭验证。想要获取更多关于匿名表达式，请看[A Note on Lambdas](A-Note-On-Lambdas.html)

`Traverser<S>`对象提供以下操作：

1. 目前遍历到的`S`对象——`Traverser.get()`
2. 目前通过遍历器遍历的路径——`Traverser.path()`
   1. 一个获取特定历史路径对象的缩写——`Traverser.path(String) == Traverser.path().get(String)`
3. 遍历器循环遍历目前循环的次数——`Traverser.loops()`
4. 遍历器遍历到的对象的数量——`Traverser.bulk()`
5. 遍历器关联的本地数据结构——`Traverser.sack()`
6. 遍历相关的副作用——`Traverser.sideEffects()`
   1. 一个获取特定副作用的缩写——`Traverser.sideEffect(String) == Tarverser.sideEffects().get(String)`

```groovy
gremlin> g.V(1).out().values('name') //1
==>lop
==>vadas
==>josh
gremlin> g.V(1).out().map {it.get().value('name')} //2
==>lop
==>vadas
==>josh
gremlin> g.V(1).out().map(values('name')) //3
==>lop
==>vadas
==>josh
```

1. 节点1的一个向外关系关联节点的name属性
2. 同样情况下，使用匿名函数获取name属性
3. 再次同样情况，但是使用`map()`函数

```groovy
gremlin> g.V().filter {it.get().label() == 'person'} //1\
==>v[1]
==>v[2]
==>v[4]
==>v[6]
gremlin> g.V().filter(label().is('person')) //2\
==>v[1]
==>v[2]
==>v[4]
==>v[6]
gremlin> g.V().hasLabel('person') //3\
==>v[1]
==>v[2]
==>v[4]
==>v[6]
```

1. 一个过滤器，用来获取所有label为person的节点
2. 同样的操作，使用`filter()`函数
3. 使用专用的`has()`步骤，由`filter()`实现

```groovy
gremlin> g.V().hasLabel('person').sideEffect(System.out.&println) //1\
v[1]
==>v[1]
v[2]
==>v[2]
v[4]
==>v[4]
v[6]
==>v[6]
gremlin> g.V().sideEffect(outE().count().store("o")).
               sideEffect(inE().count().store("i")).cap("o","i") //2\
==>[i:[0,0,1,1,1,3],o:[3,0,0,0,2,1]]
```

1. 不论什么输入进`sideEffect()`都会被传输给下一步骤，当然也有可能发生一些干预过程。
2. 计算每个节点出和进的边的数量。两个`sideEffect()`对节点都进行了统计

```groovy
gremlin> g.V().branch {it.get().value('name')}.
               option('marko', values('age')).
               option(none, values('name')) //1\
==>29
==>vadas
==>lop
==>josh
==>ripple
==>peter
gremlin> g.V().branch(values('name')).
               option('marko', values('age')).
               option(none, values('name')) //2\
==>29
==>vadas
==>lop
==>josh
==>ripple
==>peter
gremlin> g.V().choose(has('name','marko'),
                      values('age'),
                      values('name')) //3\
==>29
==>vadas
==>lop
==>josh
==>ripple
==>peter
```

1. 如果节点是“marko”，获取他的“age”属性，否则获取他的“name”属性
2. 同样的操作，但是使用`branch()`函数
3. 更加具体的方法是一个基于boolean的`choose()`步骤，由`branch()`实现。

