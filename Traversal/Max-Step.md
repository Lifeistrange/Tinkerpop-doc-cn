# max步骤

`max()`步骤(map)会将流中最大的数值返回。

```groovy
gremlin> g.V().values('age').max()
==>35
gremlin> g.V().repeat(both()).times(3).values('age').max()
==>35
```

> `max(local)`决定了目前本地对象最大的值(不是遍历流中的数据)。只能作用与集合或者数值对象。对于其他对象，会返回`Double`或者`NaN`。

