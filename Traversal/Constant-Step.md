# constant步骤

使用`constant()`步骤在遍历中定义一个常量。这通常用在需要条件的步骤中，比如`choose()`或者`coalesce()`。

```groovy
gremlin> g.V().choose(hasLabel('person'),
             values('name'),
             constant('inhuman')) //1\
==>marko
==>vadas
==>inhuman
==>josh
==>inhuman
==>peter
gremlin> g.V().coalesce(
             hasLabel('person').values('name'),
             constant('inhuman')) //2\
==>marko
==>vadas
==>inhuman
==>josh
==>inhuman
==>peter
```

1. 如果是人则展示人的名字，不是人则显示“inhuman”。
2. 和第一种情况一样（除非这个人节点没有名字）。

