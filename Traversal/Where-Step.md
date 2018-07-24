# where步骤

`where()`步骤会基于对象本身或者路径历史来过滤当前的对象(filter)。这个步骤一般专门和`match()`或者`select()`一起用，但是也可以分开使用。

```groovy
gremlin> g.V(1).as('a').out('created').in('created').where(neq('a')) //1\
==>v[4]
==>v[6]
gremlin> g.withSideEffect('a',['josh','peter']).V(1).out('created').in('created').values('name').where(within('a')) //2\
==>josh
==>peter
gremlin> g.V(1).out('created').in('created').where(out('created').count().is(gt(1))).values('name') //3\
==>josh
```

1. 谁是marko的合作者，marko不能是自己的合作者？（预测）
2. 在marko的共同创造者中，只保留名为josh和peter的。（使用副作用）
3. marko的哪一个合作者不止一个作品？（使用遍历）

> 请注意`mathc().where()`和`select().where()`的`where()`如何在`Map<String, Object>`步骤后工作。

下面我们提供一些基于匿名遍历的多种对象的过滤。

```groovy


gremlin> g.V().where(out('created')).values('name') //1\
==>marko
==>josh
==>peter
gremlin> g.V().out('knows').where(out('created')).values('name') //2\
==>josh
gremlin> g.V().where(out('created').count().is(gte(2))).values('name') //3\
==>josh
gremlin> g.V().where(out('knows').where(out('created'))).values('name') //4\
==>marko
gremlin> g.V().where(__.not(out('created'))).where(__.in('knows')).values('name') //5\
==>vadas
gremlin> g.V().where(__.not(out('created')).and().in('knows')).values('name') //6\
==>vadas
gremlin> g.V().as('a').out('knows').as('b').
           where('a',gt('b')).
             by('age').
           select('a','b').
             by('name') //7\
==>[a:marko,b:vadas]
gremlin> g.V().as('a').out('knows').as('b').
           where('a',gt('b').or(eq('b'))).
             by('age').
             by('age').
             by(__.in('knows').values('age')).
           select('a','b').
             by('name') //8\
==>[a:marko,b:vadas]
==>[a:marko,b:josh]
```

1. 创建项目的人的名字是什么？
2. 创建项目并且被人知道的人的名字是什么？
3. 创建了超过两个项目的人的名字是什么？
4. 知道有人创建了项目的人的名字是什么？
5. 没有创建项目但是被人知道的人的名字是什么？
6. 级联的`where()`和单个`where()`中使用and语句的效果相同。
7. marko知道josh和vadas但是年龄只比vadas大。
8. marko比josh年轻，但是josh知道和marko年龄一样的人。

> 匿名遍历的`where()`在当前对象的“本地”执行。在OLAP中，计算的原子单位是顶点和其对应的“星图”，要注意，匿名遍历不会离开其星图。换句话说，他不能遍历到相邻顶点的属性和边。

