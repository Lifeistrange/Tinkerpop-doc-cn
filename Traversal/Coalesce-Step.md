#  coalesce步骤

`coalesce()`步骤会有序评估所提供的遍历，并返回第一个至少有一个元素的遍历。

```groovy
gremlin> g.V(1).coalesce(outE('knows'), outE('created')).inV().path().by('name').by(label)
==>[marko,knows,vadas]
==>[marko,knows,josh]
gremlin> g.V(1).coalesce(outE('created'), outE('knows')).inV().path().by('name').by(label)
==>[marko,created,lop]
gremlin> g.V(1).property('nickname', 'okram')
==>v[1]
gremlin> g.V().hasLabel('person').coalesce(values('nickname'), values('name'))
==>okram
==>vadas
==>josh
==>peter
```

