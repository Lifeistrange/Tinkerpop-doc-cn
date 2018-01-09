# as步骤

`as()`步骤并不是一个实际的步骤，他有些类似与`by()`、`option()`这样的“步骤调节器”。使用`as()`，能够给该步骤提供一个标签，供之后的步骤使用，一些数据结构也会使用这些标签，比如`select()`、`match()`和路径。

```groovy
gremlin> g.V().as('a').out('created').as('b').select('a','b') //1\
==>[a:v[1],b:v[3]]
==>[a:v[4],b:v[5]]
==>[a:v[4],b:v[3]]
==>[a:v[6],b:v[3]]
gremlin> g.V().as('a').out('created').as('b').select('a','b').by('name') //2\
==>[a:marko,b:lop]
==>[a:josh,b:ripple]
==>[a:josh,b:lop]
==>[a:peter,b:lop]
```

1. 从路径中选择“a”与“b”标签的对象。
2. 从路径中选择“a”与“b”标签的对象，并返回他们的name值。

一个步骤可以有多个标签。这在以后的步骤中，多次引用同一步骤非常有用。

```groovy
gremlin> g.V().hasLabel('software').as('a','b','c').
            select('a','b','c').
              by('name').
              by('lang').
              by(__.in('created').values('name').fold())
==>[a:lop,b:java,c:[marko,josh,peter]]
==>[a:ripple,b:java,c:[josh]]
```

