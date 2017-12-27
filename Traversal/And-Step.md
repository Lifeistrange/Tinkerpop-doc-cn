# 与步骤

`and()`步骤会确认其承载的所有遍历都会提供输出（filter），若要看或的话请看`or()`。

```groovy
gremlin> g.V().and(
            outE('knows'),
            values('age').is(lt(30))).
              values('name')
==>marko
```

`and()`步骤可以承载任意数量的遍历步骤。所有的遍历都需要产生至少一个输出来进入下一步骤。

中缀表示法可以