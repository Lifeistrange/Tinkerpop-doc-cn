# range步骤

当遍历时，我们可以使用`range()`步骤(filter)只传递指定数量的元素。当未指定下边界时，对象会持续生成。当对象处于上下范围之间时，遍历器会被触发。所有高于上边界的数据的遍历会被中断。

```groovy
gremlin> g.V().range(0,3)
==>v[1]
==>v[2]
==>v[3]
gremlin> g.V().range(1,3)
==>v[2]
==>v[3]
gremlin> g.V().repeat(both()).times(1000000).emit().range(6,10)
==>v[1]
==>v[5]
==>v[3]
==>v[1]
```

`range()`步骤也可以应用于`Scope.local`，当它操作传来的集合时。举个例子，我们可以创建一个`Map<String, String>`，但是只包含第二属性的值。

```groovy
gremlin> g.V().as('a').out().as('b').in().as('c').select('a','b','c').by('name').range(local,1,2)
==>[b:lop]
==>[b:lop]
==>[b:lop]
==>[b:vadas]
==>[b:josh]
==>[b:ripple]
==>[b:lop]
==>[b:lop]
==>[b:lop]
==>[b:lop]
==>[b:lop]
==>[b:lop]
```

下面这个例子创建了一个`List<String>`包含第二和第三个位置的属性。

```groovy
gremlin> g.V().valueMap().select('location').range(local, 1, 3)
==>[santa cruz,brussels]
==>[dulles,purcellville]
==>[baltimore,oakland]
==>[kaiserslautern,aachen]
```

