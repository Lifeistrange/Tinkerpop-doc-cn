# optional 步骤

`optional()`步骤(map)如果遍历经过指定操作返回了结果，那么则返回该结果，如果没有则返回原对象。

```groovy
gremlin> g.V(2).optional(out('knows')) //1\
==>v[2]
gremlin> g.V(2).optional(__.in('knows')) //2\
==>v[1]
```

1. vadas没有通过“knows”边指向其他节点，所以他自己被返回了。
2. vadas有一个被“knows”指向的节点marko，所以marko被返回了。

`optional`在组合使用`path`和`tree`时很有用。

```groovy
gremlin> g.V().hasLabel('person').optional(out("knows").optional(out("created"))).path() //1\
==>[v[1],v[2]]
==>[v[1],v[4],v[5]]
==>[v[1],v[4],v[3]]
==>[v[2]]
==>[v[4]]
==>[v[6]]
```

1. 返回每个人的路径，跟着他们知道的人，接着他们创造了什么。

