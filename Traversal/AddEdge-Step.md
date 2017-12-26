# 新增边步骤

推理是把隐藏在数据中隐含的东西弄清楚的过程。那么，什么东西在图中是明确的——是点和边。而遍历在图中是隐含的。换句话说，遍历暴露出的知识是由遍历的定义来决定的。举个例子，“合作开发者”的概念。两个人如果在同一个项目中工作，那么他们就是合作开发者。这个概念可以在遍历中体现，因此，“合作开发者”的概念就可以被发现。此外，最一开始的隐藏在数据中可以被显现出来的是`addE()`步骤（map/sideEffect）。

![](image/addedge-step.png)

```groovy
gremlin> g.V(1).as('a').out('created').in('created').where(neq('a')).
           addE('co-developer').from('a').property('year',2009) //1\
==>e[13][1-co-developer->4]
==>e[14][1-co-developer->6]
gremlin> g.V(3,4,5).aggregate('x').has('name','josh').as('a').
           select('x').unfold().hasLabel('software').addE('createdBy').to('a') //2\
==>e[15][3-createdBy->4]
==>e[16][5-createdBy->4]
gremlin> g.V().as('a').out('created').addE('createdBy').to('a').property('acl','public') //3\
==>e[17][3-createdBy->1]
==>e[18][5-createdBy->4]
==>e[19][3-createdBy->4]
==>e[20][3-createdBy->6]
gremlin> g.V(1).as('a').out('knows').
           addE('livesNear').from('a').property('year',2009).
           inV().inE('livesNear').values('year') //4\
==>2009
==>2009
gremlin> g.V().match(
                 __.as('a').out('knows').as('b'),
                 __.as('a').out('created').as('c'),
                 __.as('b').out('created').as('c')).
               addE('friendlyCollaborator').from('a').to('b').
                 property(id,23).property('project',select('c').values('name')) //5\
==>e[23][1-friendlyCollaborator->4]
gremlin> g.E(23).valueMap()
==>[project:lop]
```

1. 在marko和他的同事之间添加一个带有year属性的合作开发者边。
2. 在josh(软件开发者)与lop(软件)和ripple(软件)节点间添加两个指向josh的”创造“边。
3. 对所有”创造“边添加反向的“被创造”边。
4. 我们新创造的边是可以遍历的对象。
5. 两个在同意遍历中任意的绑定的对象可以使用`from()`->`to()`来做联结，为支持ID的用户添加id属性。