# 新增属性步骤

`property()`步骤用来在图中的元素上新增属性(sideEffect)。和`addV()`与`addE()`不同的是，`property()`是一个完全的副作用步骤，他不会返回他所创建的属性，而是返回他所依赖的元素。如果`property()`后跟随`addV()`或`addE()`，他会“折叠”进前一个步骤来开启节点与边的创建，并在所有点与边新增该属性。

```groovy
gremlin> g.V(1).property('country','usa')
==>v[1]
gremlin> g.V(1).property('city','santa fe').property('state','new mexico').valueMap()
==>[country:[usa],city:[santa fe],name:[marko],state:[new mexico],age:[29]]
gremlin> g.V(1).property(list,'age',35) //1\
==>v[1]
gremlin> g.V(1).valueMap()
==>[country:[usa],city:[santa fe],name:[marko],state:[new mexico],age:[29,35]]
gremlin> g.V(1).property('friendWeight',outE('knows').values('weight').sum(),'acl','private') //2\
==>v[1]
gremlin> g.V(1).properties('friendWeight').valueMap() //3\
==>[acl:private]
```

1. 对于节点，这是一个使用属性的基础操作。
2. 在遍历时可以选择属性值（也可以选择键）。
3. 对于节点，`property()`步骤可以添加元属性。