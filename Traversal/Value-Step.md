# value步骤

`value()`步骤(map)取出对象的值

```groovy
gremlin> g.V(1).properties().value()
==>marko
==>san diego
==>santa cruz
==>brussels
==>santa fe
gremlin> g.V(1).properties().properties().value()
==>1997
==>2001
==>2001
==>2004
==>2004
==>2005
==>2005
```

