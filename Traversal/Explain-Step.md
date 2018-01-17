# explain步骤

`explain()`步骤(terminal)将会返回一个`TraversalExplanation`。一个遍历会使用已注册的遍历策略来解析出遍历会怎样被执行。一个`TraversalExplanation`可以`toString()`成三列。第一列是使用的遍历策略。第二列是遍历策略的分类：[D]ecoration（装饰），[O]ptimization（优化），[P]rovider optimization（供应优化），和[V]erfification（验证）。最后，第三列是遍历策略的遍历方式。最终遍历是最后选择的遍历。

```groovy
gremlin> g.V().hasLabel('person').outE().identity().inV().count().is(gt(5)).explain()
==>Traversal Explanation
=====================================================================================================================================================================================================
Original Traversal                 [GraphStep(vertex,[]), HasStep([~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), CountGlobalStep, IsStep(gt(5))]

ConnectiveStrategy           [D]   [GraphStep(vertex,[]), HasStep([~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), CountGlobalStep, IsStep(gt(5))]
RepeatUnrollStrategy         [O]   [GraphStep(vertex,[]), HasStep([~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), CountGlobalStep, IsStep(gt(5))]
MatchPredicateStrategy       [O]   [GraphStep(vertex,[]), HasStep([~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), CountGlobalStep, IsStep(gt(5))]
PathRetractionStrategy       [O]   [GraphStep(vertex,[]), HasStep([~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), CountGlobalStep, IsStep(gt(5))]
RangeByIsCountStrategy       [O]   [GraphStep(vertex,[]), HasStep([~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), RangeGlobalStep(0,6), CountGlobalStep, IsStep(gt(5))]
IncidentToAdjacentStrategy   [O]   [GraphStep(vertex,[]), HasStep([~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), RangeGlobalStep(0,6), CountGlobalStep, IsStep(gt(5))]
FilterRankingStrategy        [O]   [GraphStep(vertex,[]), HasStep([~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), RangeGlobalStep(0,6), CountGlobalStep, IsStep(gt(5))]
InlineFilterStrategy         [O]   [GraphStep(vertex,[]), HasStep([~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), RangeGlobalStep(0,6), CountGlobalStep, IsStep(gt(5))]
AdjacentToIncidentStrategy   [O]   [GraphStep(vertex,[]), HasStep([~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), RangeGlobalStep(0,6), CountGlobalStep, IsStep(gt(5))]
LazyBarrierStrategy          [O]   [GraphStep(vertex,[]), HasStep([~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), RangeGlobalStep(0,6), CountGlobalStep, IsStep(gt(5))]
TinkerGraphCountStrategy     [P]   [GraphStep(vertex,[]), HasStep([~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), RangeGlobalStep(0,6), CountGlobalStep, IsStep(gt(5))]
TinkerGraphStepStrategy      [P]   [TinkerGraphStep(vertex,[~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), RangeGlobalStep(0,6), CountGlobalStep, IsStep(gt(5))]
ProfileStrategy              [F]   [TinkerGraphStep(vertex,[~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), RangeGlobalStep(0,6), CountGlobalStep, IsStep(gt(5))]
StandardVerificationStrategy [V]   [TinkerGraphStep(vertex,[~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), RangeGlobalStep(0,6), CountGlobalStep, IsStep(gt(5))]

Final Traversal                    [TinkerGraphStep(vertex,[~label.eq(person)]), VertexStep(OUT,edge), IdentityStep, EdgeVertexStep(IN), RangeGlobalStep(0,6), CountGlobalStep, IsStep(gt(5))]
```

要查看遍历的详细信息，请看`profile()`步骤。