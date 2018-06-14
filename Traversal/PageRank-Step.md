# pageRank步骤

`pageRank()`步骤(map/sideEffect)会使用`PageRankVertexProgram`计算`PageRank`。

> `PageRank()`步骤是计算定点步骤，所以必须要在支持图计算的图上操作。

```groovy
gremlin> g = graph.traversal().withComputer()
==>graphtraversalsource[tinkergraph[vertices:6 edges:6], graphcomputer]
gremlin> g.V().pageRank().by('pageRank').values('pageRank')
==>0.19250000000000003
==>0.19250000000000003
==>0.23181250000000003
==>0.15000000000000002
==>0.15000000000000002
==>0.4018125
gremlin> g.V().hasLabel('person').
           pageRank().
             by(outE('knows')).
             by('friendRank').
           order().by('friendRank',decr).valueMap('name','friendRank')
==>[friendRank:[0.21375000000000002],name:[vadas]]
==>[friendRank:[0.21375000000000002],name:[josh]]
==>[friendRank:[0.15000000000000002],name:[marko]]
==>[friendRank:[0.15000000000000002],name:[peter]]
```

`explain()`步骤可以显示图计算的过程。

```groovy
gremlin> g = graph.traversal().withComputer()
==>graphtraversalsource[tinkergraph[vertices:6 edges:6], graphcomputer]
gremlin> g.V().hasLabel('person').
           pageRank().
             by(outE('knows')).
             by('friendRank').
           order().by('friendRank',decr).valueMap('name','friendRank').explain()
==>Traversal Explanation
=============================================================================================================================================================================================================================================
Original Traversal                    [GraphStep(vertex,[]), HasStep([~label.eq(person)]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), OrderGlobalStep([[value(friendRank), decr]]), PropertyM
                                         apStep([name, friendRank],value)]

ConnectiveStrategy              [D]   [GraphStep(vertex,[]), HasStep([~label.eq(person)]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), OrderGlobalStep([[value(friendRank), decr]]), PropertyM
                                         apStep([name, friendRank],value)]
VertexProgramStrategy           [D]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
RepeatUnrollStrategy            [O]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
MatchPredicateStrategy          [O]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
PathProcessorStrategy           [O]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
PathRetractionStrategy          [O]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
RangeByIsCountStrategy          [O]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
IncidentToAdjacentStrategy      [O]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
FilterRankingStrategy           [O]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
InlineFilterStrategy            [O]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
AdjacentToIncidentStrategy      [O]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
LazyBarrierStrategy             [O]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
MessagePassingReductionStrategy [O]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
OrderLimitStrategy              [O]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
TinkerGraphCountStrategy        [P]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
TinkerGraphStepStrategy         [P]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
ProfileStrategy                 [F]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
ComputerVerificationStrategy    [V]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
StandardVerificationStrategy    [V]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
ComputerFinalizationStrategy    [T]   [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]

Final Traversal                       [TraversalVertexProgramStep([GraphStep(vertex,[]), HasStep([~label.eq(person)])],graphfilter[none]), PageRankVertexProgramStep([VertexStep(OUT,[knows],edge)],friendRank,30,graphfilter[none]), Travers
                                         alVertexProgramStep([OrderGlobalStep([[value(friendRank), decr]]), PropertyMapStep([name, friendRank],value)],graphfilter[none]), ComputerResultStep]
```

