# select步骤

函数式语言组合函数并延后计算来从简单的操作中创建复杂的计算。这就是`Traversal`所做的事情。Gremlin的数据流和图计算不同的地方之一就是数据流不总是需要向下计算，我们可以回到之前计算的区域。下面的例子使用了`path()`和`select()`步骤(map)。有两个使用`select()`步骤的方式。

1. 在路径中选择标签步骤(在遍历中被`as()`定义的)。
2. 选择`Map<String, Object>`流中的数据。

下面是第一种用法：

```groovy
gremlin> g.V().as('a').out().as('b').out().as('c') // no select
==>v[5]
==>v[3]
gremlin> g.V().as('a').out().as('b').out().as('c').select('a','b','c')
==>[a:v[1],b:v[4],c:v[5]]
==>[a:v[1],b:v[4],c:v[3]]
gremlin> g.V().as('a').out().as('b').out().as('c').select('a','b')
==>[a:v[1],b:v[4]]
==>[a:v[1],b:v[4]]
gremlin> g.V().as('a').out().as('b').out().as('c').select('a','b').by('name')
==>[a:marko,b:josh]
==>[a:marko,b:josh]
gremlin> g.V().as('a').out().as('b').out().as('c').select('a') //1\
==>v[1]
==>v[1]
```

1. 如果选择一个步骤，那么就不会返回映射表。

当只有一个标签被选中那就只会返回一个对象。这对在计算中返回步骤和再次回到指定对象上很有用。

```groovy
gremlin> g.V().out().out()
==>v[5]
==>v[3]
gremlin> g.V().out().out().path()
==>[v[1],v[4],v[5]]
==>[v[1],v[4],v[3]]
gremlin> g.V().as('x').out().out().select('x')
==>v[1]
==>v[1]
gremlin> g.V().out().as('x').out().select('x')
==>v[4]
==>v[4]
gremlin> g.V().out().out().as('x').select('x') // pointless
==>v[5]
==>v[3]
```

> 当执行一个带有`select()`的遍历时，`select()`会尽量避免计算已经处理过的历史路径，并依赖与一个全局的数据结构来存储当前选中的对象。因此，如果我们只需要遍历路径中的一个子集，我们应该使用`select()`来作为一个加强的`path()`步骤。

当我们需要路径或映射表中键与值的集合，可以使用`select(keys)`和`select(values)`。当我们只需要在`groupCount()`步骤中的前N个元素时，这种调用方式十分好用。

```groovy
gremlin> graph.io(graphml()).readGraph('data/grateful-dead.xml')
gremlin> g = graph.traversal()
==>graphtraversalsource[tinkergraph[vertices:808 edges:8049], standard]
gremlin> g.V().hasLabel('song').out('followedBy').groupCount().by('name').
               order(local).by(values,decr).limit(local, 5)
==>[PLAYING IN THE BAND:107,JACK STRAW:99,TRUCKING:94,DRUMS:92,ME AND MY UNCLE:86]
gremlin> g.V().hasLabel('song').out('followedBy').groupCount().by('name').
               order(local).by(values,decr).limit(local, 5).select(keys)
==>[PLAYING IN THE BAND,JACK STRAW,TRUCKING,DRUMS,ME AND MY UNCLE]
gremlin> g.V().hasLabel('song').out('followedBy').groupCount().by('name').
               order(local).by(values,decr).limit(local, 5).select(keys).unfold()
==>PLAYING IN THE BAND
==>JACK STRAW
==>TRUCKING
==>DRUMS
==>ME AND MY UNCLE
```

相同的，从路径或映射表中取值。

```groovy
gremlin> graph.io(graphml()).readGraph('data/grateful-dead.xml')
gremlin> g = graph.traversal()
==>graphtraversalsource[tinkergraph[vertices:808 edges:8049], standard]
gremlin> g.V().hasLabel('song').out('sungBy').groupCount().by('name') //1\
==>[All:9,Weir_Garcia:1,Lesh:19,Weir_Kreutzmann:1,Pigpen_Garcia:1,Pigpen:36,Unknown:6,Weir_Bralove:1,Joan_Baez:10,Suzanne_Vega:2,Welnick:10,Lesh_Pigpen:1,Elvin_Bishop:4,Neil_Young:1,Garcia_Weir_Lesh:1,Hunter:3,Hornsby:4,Jon_Hendricks:2,Weir_Hart:3,Lesh_Mydland:1,Mydland_Lesh:1,instrumental:1,Garcia:146,Hart:2,Welnick_Bralove:1,Weir:99,Garcia_Dawson:1,Pigpen_Weir_Mydland:2,Jorma_Kaukonen:4,Joey_Covington:2,Allman_Brothers:1,Garcia_Lesh:3,Boz_Scaggs:1,Pigpen?:1,Keith_Godchaux:1,Etta_James:1,Weir_Wasserman:1,Hall_and_Oates:2,Grateful_Dead:17,Spencer_Davis:2,Pigpen_Mydland:3,Beach_Boys:3,Donna:4,Bo_Diddley:7,Bob_Dylan:22,Hart_Kreutzmann:2,Weir_Mydland:3,Lesh_Hart_Kreutzmann:1,Stephen_Stills:2,Mydland:18,Neville_Brothers:2,Weir_Hart_Welnick:1,Garcia_Lesh_Weir:1,Garcia_Weir:3,Neal_Cassady:1,John_Fogerty:5,Donna_Godchaux:2,Pigpen_Weir:8,Garcia_Kreutzmann:2,None:6]
gremlin> g.V().hasLabel('song').out('sungBy').groupCount().by('name').select(values) //2\
==>[9,1,19,1,1,36,6,1,10,2,10,1,4,1,1,3,4,2,3,1,1,1,146,2,1,99,1,2,4,2,1,3,1,1,1,1,1,2,17,2,3,3,4,7,22,2,3,1,2,18,2,1,1,3,1,5,2,8,2,6]
gremlin> g.V().hasLabel('song').out('sungBy').groupCount().by('name').select(values).unfold().
               groupCount().order(local).by(values,decr).limit(local, 5) //3\
==>[1:22,2:12,3:7,4:4,6:2]
```

1. 哪个艺术家唱了多少歌？
2. 获取匿名的歌曲曲目数量。
3. 获取唱的人最多的5首歌。

> 需要注意，`by()`模块并不支持`select(keys)`和`select(values)`。

我们也有一个选项来让`select()`支持`pop`操作来处理`List`对象`Traverser`:

```groovy
gremlin> g.V(1).as("a").repeat(out().as("a")).times(2).select(first, "a")
==>v[1]
==>v[1]
gremlin> g.V(1).as("a").repeat(out().as("a")).times(2).select(last, "a")
==>v[5]
==>v[3]
gremlin> g.V(1).as("a").repeat(out().as("a")).times(2).select(all, "a")
==>[v[1],v[4],v[5]]
==>[v[1],v[4],v[3]]
```

## 在`Select`中使用`Where`

像`match()`步骤，他能够使用`where()`作为过滤器来处理`Map<String, Object>`流。

```groovy
gremlin> g.V().as('a').out('created').in('created').as('b').select('a','b').by('name') //1\
==>[a:marko,b:marko]
==>[a:marko,b:josh]
==>[a:marko,b:peter]
==>[a:josh,b:josh]
==>[a:josh,b:marko]
==>[a:josh,b:josh]
==>[a:josh,b:peter]
==>[a:peter,b:marko]
==>[a:peter,b:josh]
==>[a:peter,b:peter]
gremlin> g.V().as('a').out('created').in('created').as('b').
               select('a','b').by('name').where('a',neq('b')) //2\
==>[a:marko,b:josh]
==>[a:marko,b:peter]
==>[a:josh,b:marko]
==>[a:josh,b:peter]
==>[a:peter,b:marko]
==>[a:peter,b:josh]
gremlin> g.V().as('a').out('created').in('created').as('b').
               select('a','b'). //3\
               where('a',neq('b')).
               where(__.as('a').out('knows').as('b')).
               select('a','b').by('name')
==>[a:marko,b:josh]
```

1. 标准的`select()`生成一个变量的`Map<String, Obejct>`对象，绑定在路径上来操作。
2. `select().by('name')`绑定了顶点的名称属性值，然后使用了`where()`操作来确定a和b不同。
3. 第一个`select()`绑定了一个顶点集合。另一个绑定了和`a`相同的`b`顶点。最后一个绑定筛选了知道`b`的`a`。第二个和最后一个`select()`都代表顶点。

