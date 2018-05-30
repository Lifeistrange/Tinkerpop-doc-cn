# inject步骤

![](image/inject-step.png)

注入步骤是TinkerPop3的主要功能.他使得我们可以任意的将对象插入到遍历流中。通常情况下，`inject()`步骤(sideEffect)会像下面这些例子一样使用。

```groovy
gremlin> g.V(4).out().values('name').inject('daniel')
==>daniel
==>ripple
==>lop
gremlin> g.V(4).out().values('name').inject('daniel').map {it.get().length()}
==>6
==>6
==>3
gremlin> g.V(4).out().values('name').inject('daniel').map {it.get().length()}.path()
==>[daniel,6]
==>[v[4],v[5],ripple,6]
==>[v[4],v[3],lop,3]
```

在上面的最后一个例子里，我们要注意到`daniel`的路径长度只有2。这是因为`daniel`是半路插入进遍历来的。最后，一个典型的用法在下面展示：遍历并没有从图开始。

```groovy
gremlin> inject(1,2)
==>1
==>2
gremlin> inject(1,2).map {it.get() + 1}
==>2
==>3
gremlin> inject(1,2).map {it.get() + 1}.map {g.V(it.get()).next()}.values('name')
==>vadas
==>lop
```

