# group步骤

我们通过遍历来定义遍历者在图中的遍历过程，那么副作用的计算在有些时候也是需要的。实际上，我们的遍历所到达和输出的地方，也不是我们最终需要的计算，有一些其他的东西能够代表这个遍历。`group()`步骤(map/sideEffect)就有这样的副作用，能够通过一些函数来组织对象。如果需要的话，组织的方式也可以是迭代的。下面提供了一个示例。

```groovy
gremlin> g.V().group().by(label) //1\
==>[software:[v[3],v[5]],person:[v[1],v[2],v[4],v[6]]]
gremlin> g.V().group().by(label).by('name') //2\
==>[software:[lop,ripple],person:[marko,vadas,josh,peter]]
gremlin> g.V().group().by(label).by(count()) //3\
==>[software:2,person:4]
```

1. 通过标签来组织顶点。
2. 遍历组中的每一个顶点来获取他的名字。
3. 每一个组的大小。

我们可以使用`by()`来给`group()`注入参数：

1. 键注入：使用什么来分组？
2. 值注入：在分组的键后存什么存储什么？

