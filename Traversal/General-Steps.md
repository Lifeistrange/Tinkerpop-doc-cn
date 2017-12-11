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

