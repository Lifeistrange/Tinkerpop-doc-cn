# sum步骤

`sum()`步骤(map)会将流上的数字计算和并返回一个double类型。需要注意的是，当前遍历器的数字会乘以遍历器的数量来确定出现了多少数字。

```groovy
gremlin> g.V().values('age').sum()
==>123
gremlin> g.V().repeat(both()).times(3).values('age').sum()
==>1471
```

> `sum(local)`声明目前本地对象的和（不是遍历流中的）。这可以作用于集合类型的对象。对于其他对象，都会返回`Double.NaN`。

