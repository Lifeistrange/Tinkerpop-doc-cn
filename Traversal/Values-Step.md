# values步骤

`values()`步骤(map)从遍历流的元素中获取属性的值。

```groovy
gremlin> g.V(1).values()
==>marko
==>san diego
==>santa cruz
==>brussels
==>santa fe
gremlin> g.V(1).values('location')
==>san diego
==>santa cruz
==>brussels
==>santa fe
gremlin> g.V(1).properties('location').values()
==>1997
==>2001
==>2001
==>2004
==>2004
==>2005
==>2005
```

