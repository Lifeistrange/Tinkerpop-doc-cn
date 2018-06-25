# propertiers步骤

`properties()`步骤(map)从遍历流的元素中去除多个属性。

```groovy
gremlin> g.V(1).properties()
==>vp[name->marko]
==>vp[location->san diego]
==>vp[location->santa cruz]
==>vp[location->brussels]
==>vp[location->santa fe]
gremlin> g.V(1).properties('location').valueMap()
==>[startTime:1997,endTime:2001]
==>[startTime:2001,endTime:2004]
==>[startTime:2004,endTime:2005]
==>[startTime:2005]
gremlin> g.V(1).properties('location').has('endTime').valueMap()
==>[startTime:1997,endTime:2001]
==>[startTime:2001,endTime:2004]
==>[startTime:2004,endTime:2005]
```

