# unfold步骤

如果一个遍历器或可以遍历的对象，或者映射对象到了`unfold()`步骤(flatMap)他会展开成为一个线性的格式。如果不是这些类型，那么传递来的对象会简化的传递出去。

```groovy
gremlin> g.V(1).out().fold().inject('gremlin',[1.23,2.34])
==>gremlin
==>[1.23,2.34]
==>[v[3],v[2],v[4]]
gremlin> g.V(1).out().fold().inject('gremlin',[1.23,2.34]).unfold()
==>gremlin
==>1.23
==>2.34
==>v[3]
==>v[2]
==>v[4]
```

注意，`unfold()`不会递归展开遍历器，如果需要，请使用`repeat()`来递归展开。

```groovy
gremlin> inject(1,[2,3,[4,5,[6]]])
==>1
==>[2,3,[4,5,[6]]]
gremlin> inject(1,[2,3,[4,5,[6]]]).unfold()
==>1
==>2
==>3
==>[4,5,[6]]
gremlin> inject(1,[2,3,[4,5,[6]]]).repeat(unfold()).until(count(local).is(1)).unfold()
==>1
==>2
==>3
==>4
==>5
==>6
```