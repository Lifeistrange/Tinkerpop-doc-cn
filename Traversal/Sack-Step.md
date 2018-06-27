# sack步骤

一个遍历器可以包含叫做“sack”的本地数据结构。`sack()`步骤用来读写结构(sideEffect或者map)。遍历器可以通过下面的方式创建结构。

``GraphTraversal.withSack(initialValueSupplier,splitOperator?,mergeOperator?)``

- 初始化值支持：`Supplier`提供给遍历器的结构初始值。
- 分割器：`UnaryOperator`在遍历器分割时复制遍历器的结构。如果没有提供分割器，会自动使用`UnaryOperator.identity()`。
- 合并器：`BinaryOperator`在两个遍历器组合时合并遍历器的结构。如果不指定则不会合并。

下面有两个初始化值的例子。在第一个例子里，遍历器和一个1.0结构创建于一个图中，并且结构的值被输出。在第二个例子里，我们用随机的浮点数来生成结构的值。

```groovy
gremlin> g.withSack(1.0f).V().sack()
==>1.0
==>1.0
==>1.0
==>1.0
==>1.0
==>1.0
gremlin> rand = new Random()
==>java.util.Random@6a4ba6f4
gremlin> g.withSack {rand.nextFloat()}.V().sack()
==>0.26806575
==>0.44459033
==>0.77047575
==>0.32030708
==>0.5653681
==>0.7913796
```

下面我们来看一个更复杂的初始化例子，这个例子使用在运行的计算当中，并且其生成了遍历的结果。当边被遍历到，变得权重会乘结构的值(`sack(mult).by('weight')`)。注意`by()`模块可以用于任意的遍历中。

```groovy
gremlin> g.withSack(1.0f).V().repeat(outE().sack(mult).by('weight').inV()).times(2)
==>v[5]
==>v[3]
gremlin> g.withSack(1.0f).V().repeat(outE().sack(mult).by('weight').inV()).times(2).sack()
==>1.0
==>0.4
gremlin> g.withSack(1.0f).V().repeat(outE().sack(mult).by('weight').inV()).times(2).path().
               by().by('weight')
==>[v[1],1.0,v[4],1.0,v[5]]
==>[v[1],1.0,v[4],0.4,v[3]]
```

当我们使用复杂的对象，我们需要定义分割器来确保每一个遍历器都有复制到一份其母遍历器的结构。第一个例子没有使用分割器，结果相同的映射分发到了所有的遍历器中。第二个例子，声明了`Map.clone()`如何运行，确保每个遍历器都有其独一无二的结构。

```groovy
gremlin> g.withSack {[:]}.V().out().out().
               sack {m,v -> m[v.value('name')] = v.value('lang'); m}.sack() // BAD: single map
==>[ripple:java]
==>[ripple:java,lop:java]
gremlin> g.withSack {[:]}{it.clone()}.V().out().out().
               sack {m,v -> m[v.value('name')] = v.value('lang'); m}.sack() // GOOD: cloned map
==>[ripple:java]
==>[lop:java]
```

> 对于基础类型，我们不需要分割器，因为基础类型在结构的内存中，而不是对象的引用。

如果使用合并器，遍历器的结构就不会统一。在一些情况中，合并两个遍历器的结构需要健全的算法以便提供良好的统计优化。在下面的例子里，二进制合并器为`Operator.sum`。因此，两个遍历器合并，他们的结构相加。

```groovy
gremlin> g.withSack(1.0d).V(1).out('knows').in('knows') //1\
==>v[1]
==>v[1]
gremlin> g.withSack(1.0d).V(1).out('knows').in('knows').sack() //2\
==>1.0
==>1.0
gremlin> g.withSack(1.0d, sum).V(1).out('knows').in('knows').sack() //3\
==>2.0
==>2.0
gremlin> g.withSack(1.0d).V(1).local(outE('knows').barrier(normSack).inV()).in('knows').barrier() //4\
==>v[1]
==>v[1]
gremlin> g.withSack(1.0d).V(1).local(outE('knows').barrier(normSack).inV()).in('knows').barrier().sack() //5\
==>0.5
==>0.5
gremlin> g.withSack(1.0d,sum).V(1).local(outE('knows').barrier(normSack).inV()).in('knows').barrier().sack() //6\
==>1.0
==>1.0
gremlin> g.withBulk(false).withSack(1.0f,sum).V(1).local(outE('knows').barrier(normSack).inV()).in('knows').barrier().sack() //7\
==>1.0
gremlin> g.withBulk(false).withSack(1.0f).V(1).local(outE('knows').barrier(normSack).inV()).in('knows').barrier().sack() //8\
==>0.5
==>0.5
```

1. 我们查询出了两个顶点1是因为他有两个knows关联的人。
2. 没有合并操作，结构的值为1.0.
3. 使用`sum`合并操作，结构的值变为2.0。
4. 和1一样，但是使用了栅栏间隔。
5. `local(...barrier(normSack)...)`确保了所有遍历器在顶点1后有初始的1.0“能量”(50-50)。
6. 和3一样，但是`sum`合并操作之后的结果为1.0。
7. 我们有一个单遍历器，和两个分块，结构为1.0，当我们把withBulk(false)时，返回为1.0。
8. 和7一样，没有和操作。