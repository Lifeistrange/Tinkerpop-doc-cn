# program步骤

`program()`步骤(map/sideEffect)是图计算任务的匿名步骤。这个步骤接收一个`VertexProgram`参数并在图中执行。因此，用户可以创建他们的`VertexProgram`并应用于遍历。我们能够使用的节点程序设置有：

- `gremlin.vertexProgramStep.rootTraversal`是根遍历`PureTraversal`的串行化。
- `gremlin.vertexProgramStep.stepId`是`program()`步骤的步骤id。

用户提供`VertexProgram`可以相对的利用这个程序。下面是一些例子。

> 开发`VertexProgram`对用户来说十分专业。要开发一个既满足需求又足够有效率的程序就更加困难。所以我们建议高级用户最好可以对Gremlin OLAP有一个比较深的理解。

```java
private TraverserSet<Object> haltedTraversers;

public void loadState(final Graph graph, final Configuration configuration) {
  VertexProgram.super.loadState(graph, configuration);
  this.traversal = PureTraversal.loadState(configuration, VertexProgramStep.ROOT_TRAVERSAL, graph);
  this.programStep = new TraversalMatrix<>(this.traversal.get()).getStepById(configuration.getString(ProgramVertexProgramStep.STEP_ID));
  // if the traversal sideEffects will be used in the computation, add them as memory compute keys
  this.memoryComputeKeys.addAll(MemoryTraversalSideEffects.getMemoryComputeKeys(this.traversal.get()));
  // if master-traversal traversers may be propagated, create a memory compute key
  this.memoryComputeKeys.add(MemoryComputeKey.of(TraversalVertexProgram.HALTED_TRAVERSERS, Operator.addAll, false, false));
  // returns an empty traverser set if there are no halted traversers
  this.haltedTraversers = TraversalVertexProgram.loadHaltedTraversers(configuration);
}

public void storeState(final Configuration configuration) {
  VertexProgram.super.storeState(configuration);
  // if halted traversers is null or empty, it does nothing
  TraversalVertexProgram.storeHaltedTraversers(configuration, this.haltedTraversers);
}

public void setup(final Memory memory) {
  if(!this.haltedTraversers.isEmpty()) {
    // do what you like with the halted master traversal traversers
  }
  // once used, no need to keep that information around (master)
  this.haltedTraversers = null;
}

public void execute(final Vertex vertex, final Messenger messenger, final Memory memory) {
  // once used, no need to keep that information around (workers)
  if(null != this.haltedTraversers)
    this.haltedTraversers = null;
  if(vertex.property(TraversalVertexProgram.HALTED_TRAVERSERS).isPresent()) {
    // haltedTraversers in execute() represent worker-traversal traversers
    // for example, from a traversal of the form g.V().out().program(...)
    TraverserSet<Object> haltedTraversers = vertex.value(TraversalVertexProgram.HALTED_TRAVERSERS);
    // create a new halted traverser set that can be used by the next OLAP job in the chain
    // these are worker-traversers that are distributed throughout the graph
    TraverserSet<Object> newHaltedTraversers = new TraverserSet<>();
    haltedTraversers.forEach(traverser -> {
       newHaltedTraversers.add(traverser.split(traverser.get().toString(), this.programStep));
    });
    vertex.property(VertexProperty.Cardinality.single, TraversalVertexProgram.HALTED_TRAVERSERS, newHaltedTraversers);
    // it is possible to create master-traversers that are localized to the master traversal (this is how results are ultimately delivered back to the user)
    memory.add(TraversalVertexProgram.HALTED_TRAVERSERS,
               new TraverserSet<>(this.traversal().get().getTraverserGenerator().generate("an example", this.programStep, 1l)));
  }

public boolean terminate(final Memory memory) {
  // the master-traversal will have halted traversers
  assert memory.exists(TraversalVertexProgram.HALTED_TRAVERSERS);
  final TraverserSet<String> haltedTraversers = memory.get(TraversalVertexProgram.HALTED_TRAVERSERS);
  // it will only have the traversers sent to the master traversal via memory
  assert haltedTraversers.stream().map(Traverser::get).filter(s -> s.equals("an example")).findAny().isPresent();
  // it will not contain the worker traversers distributed throughout the vertices
  assert !haltedTraversers.stream().map(Traverser::get).filter(s -> !s.equals("an example")).findAny().isPresent();
  return true;
}
```

最后，我们提供一个使用`PageRankVertexProgram`代替`pageRank()`的例子：

```groovy
gremlin> g = graph.traversal().withComputer()
==>graphtraversalsource[tinkergraph[vertices:6 edges:6], graphcomputer]
gremlin> g.V().hasLabel('person').
           program(PageRankVertexProgram.build().property('rank').create(graph)).
             order().by('rank', incr).
           valueMap('name', 'rank')
==>[name:[marko],rank:[0.15000000000000002]]
==>[name:[peter],rank:[0.15000000000000002]]
==>[name:[vadas],rank:[0.19250000000000003]]
==>[name:[josh],rank:[0.19250000000000003]]
```

