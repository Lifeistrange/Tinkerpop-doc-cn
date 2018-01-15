# coin步骤

使用`coin()`步骤可以随机过滤一个遍历。提供一个double参数来控制概率。

```groovy
gremlin> g.V().coin(0.5)
==>v[1]
==>v[5]
==>v[6]
gremlin> g.V().coin(0.0)
gremlin> g.V().coin(1.0)
==>v[1]
==>v[2]
==>v[3]
==>v[4]
==>v[5]
==>v[6]
```

