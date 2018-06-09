# loops步骤

`loops()`步骤(map)会提取当前遍历循环指定次数的结果。

```groovy
gremlin> g.V().emit(__.has("name", "marko").or().loops().is(2)).repeat(__.out()).values("name")
==>marko
==>ripple
==>lop
```

