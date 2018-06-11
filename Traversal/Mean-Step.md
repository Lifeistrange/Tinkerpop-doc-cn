# mean步骤

`mean()`步骤(map)会返回流上的数值的平均值。

```groovy
gremlin> g.V().values('age').mean()
==>30.75
gremlin> g.V().repeat(both()).times(3).values('age').mean() //1\
==>30.645833333333332
gremlin> g.V().repeat(both()).times(3).values('age').dedup().mean()
==>30.75
```

1. 使用`repeat()`遍历有可能返回更多的数字，使得平均值变化。

> `mean(local)`会计算本地数值的平均值(而不是流中的平均值)。只能作用与集合或者数值对象。对于其他对象，会返回`Double`或者`NaN`。

