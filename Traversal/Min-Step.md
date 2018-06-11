# min步骤

`min()`步骤(map)会返回流中数据的最小值。

```groovy
gremlin> g.V().values('age').min()
==>27
gremlin> g.V().repeat(both()).times(3).values('age').min()
==>27
```

> `min(local)`会计算本地数值的最小值(而不是流中的最小值)。只能作用与集合或者数值对象。对于其他对象，会返回`Double`或者`NaN`。

