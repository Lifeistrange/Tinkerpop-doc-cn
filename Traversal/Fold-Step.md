#  fold步骤

在遍历留需要一个“障碍”来聚合所有对象并触发一个聚合计算时，我们就需要用到`fold()`步骤(map)。相反的情况请看`unfold()`步骤。

```groovy
gremlin> g.V(1).out('knows').values('name')
==>vadas
==>josh
gremlin> g.V(1).out('knows').values('name').fold() //1\
==>[vadas,josh]
gremlin> g.V(1).out('knows').values('name').fold().next().getClass() //2\
==>class java.util.ArrayList
gremlin> g.V(1).out('knows').values('name').fold(0) {a,b -> a + b.length()} //3\
==>9
gremlin> g.V().values('age').fold(0) {a,b -> a + b} //4\
==>123
gremlin> g.V().values('age').fold(0, sum) //5\
==>123
gremlin> g.V().values('age').sum() //6\
==>123
```

1. 没有参数的`fold()`会聚合所有的对象到一个列表中，并返回这个列表。
2. 证明返回的类型为列表。
3. `fold()`可以输入两个参数--一个基础值和一个迭代函数（‘vadas’有5个字符加上‘josh’有四个字符）
4. 图中所有人的寿命和是多少？
5. 同样的情况，使用内建函数。
6. 同样情况，使用`sum()`步骤。