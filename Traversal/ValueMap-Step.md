# valueMap步骤

`valueMap()`步骤生成一个元素属性的图表示方式。

```groovy
gremlin> g.V().valueMap()
==>[name:[marko],age:[29]]
==>[name:[vadas],age:[27]]
==>[name:[lop],lang:[java]]
==>[name:[josh],age:[32]]
==>[name:[ripple],lang:[java]]
==>[name:[peter],age:[35]]
gremlin> g.V().valueMap('age')
==>[age:[29]]
==>[age:[27]]
==>[]
==>[age:[32]]
==>[]
==>[age:[35]]
gremlin> g.V().valueMap('age','blah')
==>[age:[29]]
==>[age:[27]]
==>[]
==>[age:[32]]
==>[]
==>[age:[35]]
gremlin> g.E().valueMap()
==>[weight:0.5]
==>[weight:1.0]
==>[weight:0.4]
==>[weight:1.0]
==>[weight:0.4]
==>[weight:0.2]
```

我们需要注意的是，顶点的映射维护了他每个键对应的值的列表。顶点或者边的属性映射到一个单独的属性上（不是列表）。这是因为TinkerPop3的顶点属性支持一个键对应多个值。

```groovy
gremlin> g.V().valueMap()
==>[name:[marko],location:[san diego,santa cruz,brussels,santa fe]]
==>[name:[stephen],location:[centreville,dulles,purcellville]]
==>[name:[matthias],location:[bremen,baltimore,oakland,seattle]]
==>[name:[daniel],location:[spremberg,kaiserslautern,aachen]]
==>[name:[gremlin]]
==>[name:[tinkergraph]]
gremlin> g.V().has('name','marko').properties('location')
==>vp[location->san diego]
==>vp[location->santa cruz]
==>vp[location->brussels]
==>vp[location->santa fe]
gremlin> g.V().has('name','marko').properties('location').valueMap()
==>[startTime:1997,endTime:2001]
==>[startTime:2001,endTime:2004]
==>[startTime:2004,endTime:2005]
==>[startTime:2005]
```

如果需要查看元素的`id`、`label`、`key`、`value`，那么就给`valueMap`传一个布尔参数`true`。

```groovy
gremlin> g.V().hasLabel('person').valueMap(true)
==>[name:[marko],label:person,location:[san diego,santa cruz,brussels,santa fe],id:1]
==>[name:[stephen],label:person,location:[centreville,dulles,purcellville],id:7]
==>[name:[matthias],label:person,location:[bremen,baltimore,oakland,seattle],id:8]
==>[name:[daniel],label:person,location:[spremberg,kaiserslautern,aachen],id:9]
gremlin> g.V().hasLabel('person').valueMap(true,'name')
==>[name:[marko],label:person,id:1]
==>[name:[stephen],label:person,id:7]
==>[name:[matthias],label:person,id:8]
==>[name:[daniel],label:person,id:9]
gremlin> g.V().hasLabel('person').properties('location').valueMap(true)
==>[key:location,value:san diego,startTime:1997,id:6,endTime:2001]
==>[key:location,value:santa cruz,startTime:2001,id:7,endTime:2004]
==>[key:location,value:brussels,startTime:2004,id:8,endTime:2005]
==>[key:location,value:santa fe,startTime:2005,id:9]
==>[key:location,value:centreville,startTime:1990,id:10,endTime:2000]
==>[key:location,value:dulles,startTime:2000,id:11,endTime:2006]
==>[key:location,value:purcellville,startTime:2006,id:12]
==>[key:location,value:bremen,startTime:2004,id:13,endTime:2007]
==>[key:location,value:baltimore,startTime:2007,id:14,endTime:2011]
==>[key:location,value:oakland,startTime:2011,id:15,endTime:2014]
==>[key:location,value:seattle,startTime:2014,id:16]
==>[key:location,value:spremberg,startTime:1982,id:17,endTime:2005]
==>[key:location,value:kaiserslautern,startTime:2005,id:18,endTime:2009]
==>[key:location,value:aachen,startTime:2009,id:19]
```

