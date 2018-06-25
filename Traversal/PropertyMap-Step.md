# propertyMap步骤

`propertyMap()`步骤生成元素的属性的图表示形式。

```groovy
gremlin> g.V().propertyMap()
==>[name:[vp[name->marko]],age:[vp[age->29]]]
==>[name:[vp[name->vadas]],age:[vp[age->27]]]
==>[name:[vp[name->lop]],lang:[vp[lang->java]]]
==>[name:[vp[name->josh]],age:[vp[age->32]]]
==>[name:[vp[name->ripple]],lang:[vp[lang->java]]]
==>[name:[vp[name->peter]],age:[vp[age->35]]]
gremlin> g.V().propertyMap('age')
==>[age:[vp[age->29]]]
==>[age:[vp[age->27]]]
==>[]
==>[age:[vp[age->32]]]
==>[]
==>[age:[vp[age->35]]]
gremlin> g.V().propertyMap('age','blah')
==>[age:[vp[age->29]]]
==>[age:[vp[age->27]]]
==>[]
==>[age:[vp[age->32]]]
==>[]
==>[age:[vp[age->35]]]
gremlin> g.E().propertyMap()
==>[weight:p[weight->0.5]]
==>[weight:p[weight->1.0]]
==>[weight:p[weight->0.4]]
==>[weight:p[weight->1.0]]
==>[weight:p[weight->0.4]]
==>[weight:p[weight->0.2]]
```

