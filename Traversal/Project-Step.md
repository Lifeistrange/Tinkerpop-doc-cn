# project步骤

`project()`步骤(map)会将当前的对象汇入一个`Map<String, Object>`中。他和`select()`步骤相似，但是他所处理的不是遍历器的历史状态，而是遍历器的当前状态。

```groovy
gremlin> g.V().out('created').
           project('a','b').
             by('name').
             by(__.in('created').count()).
           order().by(select('b'),decr).
           select('a')
==>lop
==>lop
==>lop
==>ripple
gremlin> g.V().has('name','marko').
                        project('out','in').
                          by(outE().count()).
                          by(inE().count())
==>[out:3,in:0]
```

