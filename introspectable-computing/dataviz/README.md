
Software development should be more visual, and interactive - engaging our spatial-visual cognitive abilities more,
as well as our ability to explore and develop intuitive understandings of complex systems.

There are several aspects of this

1. Structure of programs. What pieces they consist of and how these pieces connected and interact.
2. The data processed by the program. Including the possible ranges of data, and how data changes during execution.
3. Models of the domain that the program operates on/within. How program behavior influence the environment, and is influenced by it.

Each of these aspects may be useful on its own, however the major benefits might not appear until
each of the apects are present *and* that they are interconnected, supporting eachother.

## Visual programming

* Node-based dataflow programming, as used in [Flowhub](http://flowhub.io).
* Imperative visual programs, like MIT scratch.
* Ladder-based programming, used for PLS
* Visual graph notation used in finite state machines.
* Callstack analysis/visualiztion of imperative/functional code.


## Datavisualizion

`{ x: 1.0, y: 3.0 }` (most likely) represents a point in 2d space.
It should be visualizable and manipulatable as such.

Data in the FBP protocol is primarily represented as JSON, and this is
a common serialization format also outside.

TODO: include notes from paper, 2015-2016.

Existing:

* Workbooks with built in plotting, like Matlab/Octave, iPython/Jupyter
* Spreadsheets, with built-in graphing tools. Like Excel/Google Docs.
* Data-visualization frameworks, like D3
* Metrics platforms with support for graphs. Like New Relic Insights
* Debuggers with support for graphs. Like [ddd](https://www.gnu.org/software/ddd/)
* Scripting debugger, like Python scripts in gdb generate graphs. TODO: link blogpost, from HackerNews
* Visualizer nodes, or custom UI on nodes in node-based-programming. Like PureData
* [Custom visualization plugins for Kibana](http://logz.io/blog/kibana-visualizations/)

Wanted

* Reverse-engineering of dataformats and/or protocols, visually?

## Detecting datatypes

{
    a: 1.0
    b: {
     inner: "foo",
    },
    c: [ 'FOO', 'bar' ],
}

* Flatten the data, giving each piece a `path` that uniquely identifies it, and can be used for lookup
* For each piece of data, run it through a set of feature detectors.
* A feature detector is a predicate function. It which gets the data and returns true|false(|Error)
* Object key names and array indice (at least leaves) should also be stored as features
* Feature detector funtions are registered with library and name. Ex 'json'.'number' or ''
* Can have API for registering a whole set with same prefix, defaulting name to the name of function
* Reasoning about composite data. Should be able to query to find data with N items, each having M features.
The related data should be within D distance from eachother. Sibling=1,uncle=2,cousin=3,...
* If need to give *one* answer about what type of data something is, what would it be?
The composite which is the most specific (matches the most features)?
What to detect things like: Timeseries (has datapoints, each associated with a timestamp) 
* Could one **PULL** in the detectors to run based on a query?
Avoids calculating many features which are never used
Still should be possible to specify 'all'
* Need to run this on large datastructures in-the-wild to see if it works/is-useful/scales.
Just grab random JSONs from Github,Stack-Overflow etc?
* Can maybe use memoization to good effect to avoid recomputing detectors?

### JSON tools

Transformations

* Json Schema
* [JsonPath](http://goessner.net/articles/JsonPath), XPath for JSON. Spec with multiple implementations, including JS
* [jq](https://stedolan.github.io/jq/) "like sed for JSON data". Portable C.

Viewers

* Interactive drill-down graphs. Lots of these, http://visualizer.json2html.com/, http://jsonviewer.stack.hu/. Tiny bit better than a text-editor and pretty formatted JSON..
* Tabular view. http://jabulr.com nice but no sorting support?
* [jsonviewer](http://jsonviewer.codeplex.com/), with support for plugins for custom object visualizations. .NET
* [JSON Editor](https://github.com/jdorn/json-editor). Creates HTML UI for manipulation data given a JSON Schema.
* [json-schema-viewer](http://jlblcc.github.io/json-schema-viewer/). Interactive drill-down viewer of JSON Schemas, with details shown on the side.

Other

* [jsonDiscovered](https://github.com/SOM-Research/jsonDiscoverer/), can find/build schemas given examples of JSON documents.

Dataformats / semantic tools

* [JSON-LD](https://en.wikipedia.org/wiki/JSON-LD)
* [schema.org schemas](http://schema.org/docs/schemas.html)
* [json-stat](https://json-stat.org/), standarized format for datasets, especially those used as basis for visualizations. Defined via JSON schema

General graphing/dataviz

* [DynaGraph](http://dygraphs.com/gallery/#g/temperature-sf-ny). Basic looks, good for timelines with lots of data. Has error-bars/variation indication.
* [Paper.JS](http://paperjs.org/features/). Interactive vector-graphics library, with SVG import/export
* [Vega](http://vega.github.io/vega-editor/index.html?mode=vega&spec=falkensee), specify data and their visualizations in JSON.

### Ideas

Use "structural typing" (ref TypeScript) to decide how data may be treated.
Benefit: Explicit type information, and agreement on type system not neeed.

Plugins with a matching/ranking predicate ran against the data.
Support both single-value visualizers, as well as sets (array/object)

JSON Path for specifying search/filters
Could maybe also allow JS functions for mapping.
Also support JS for reductions? Basically creating derived-data in that case

Tool should persist the view/visualization selected, usable on data updates.
Maybe remember earlier views as history in LocalStorage
URL should contain data and view parameters, so it can be sent to someone else


### Prerequisites

For datavisualization, there needs to be a way in the programming language/framework/toolchain
to find the places where there is data of interest, and to be able to observe the data there.
In order to allow visual manipulation of the data, must also be able to inject manipulated data at such points.

The exact mechanism is not important, however:
Since programs often run on devices, the data-observation should be network capable.
Since issues often happen in production, production systems should have the system enabled.
This implies that it cannot disturb normal production, including performance.

## Models

Most evident when working with programs that interact with the "real-world",
for instance robotics, 3d-modelling, transportation.

Other programs which operate on/within an "environment".
This can be software-defined networking, cloud computing orchestration, user behavior analysis (for sales or UX).

With virtual reality and augmented reality, need and drive for more of these.


Support development of mental models that are executable. Both the initial creation and refinement.
Right now these only exist within heads of people, or in best case in an outdated documentation artifact.

### Simulation-driven development
Start by building/using an interactive model, and specifying a few usage scenarios.
Run simulations, driven programatically and with user-interface.
Build up learning about the domain, exploring what is perceived as known (assumptions),
and mapping what is unknown "terra incognita" - use to decide areas of focused research/testing,
to (in)validate assumptions and make more things known.

Similar to "model-based" development, but not attempting to be a mathematically rigorous system.
Model to the extent that is practical. Start rough, refine as needed and as understanding improves.
Use the model as part of the implementation, as part of (automated) verification, as part of
debugging tools for live system.

Existing

* Physics-based component simulators in EDA (electronic design automation) tools, like SPICE.
* Physics engines, as found in games
* FEM/FEA (finite element analysis), used for structural engineering in CAD (computer aided design), like Calculix in FreeCAD
* Raytracing engines, simulating how light behaves. Cycles in Blender
* Sales funnel, like in Google Analytics

Wanted

* Computer networking, including latency, throughput, unreliability. Wireless
* Energy consumption of embedded device. Communication, computation. Storage
* Processor memory, including caches
* Distributed computing systems, both networks, computational resources, program behavior.
* Flying-robotics, like multicopters
* Wheel-based-robotics, like folkrace
* Energy production, via solar cells.
* Energy production, via windpower
* Digital fabrication machines, subtractive (CNC/laser) and additive (3dprinter)
* Sound propagation in space
* Propagation of information though a social network. Facebook social graph?

Related

* Brett Victor "Seeing Spaces"
* Idea of first building a toolkit for the problem domain, that allows to experiment in that domain. Alan Kay?

References

* [On the industrial adoptiation of model driven engineering](http://www.uajournals.com/ijisebc/journal/1/4.pdf).
Thourough analysis of current state of model-driven software engineering, including benefits, inhibitors.
* [](https://www.cs.cmu.edu/~music/cmsip/readings/intro-discrete-event-sim.html)

[Simulation-Based Test Adequacy Criteria for Distributed Systems](http://mjrutherford.org/files/RutherfordCarzanigaWolfFSE06.pdf)

> Hypothesis: [discrete-event] Simulations can be used within a specification-based testing regime to help
developers of distributed systems define and apply effective system-level test suites.
> ...
> Simulations are used to understand and evaluate the functionality and performance
> of complex inter-component protocols and algorithms.
> They abstract away low-level details of the implementation of a dis-tributed system,
> as well as details of the operational environment, yet still provide a faithful model of the
> expected behavior of the system in its environment.
> ...
> Simulations embody abstractions for the underlying mechanisms and environmental conditions
> that affect the distribution properties of systems.
> In addition to operating on the normal functional inputs of a system, simulations are parameterized
> by a set of inputs for controlling a wide range of environmental phenomena,
> such as message sequences, delays, and bandwidths.
> ...
> In summary, a simulation is an abstract, executable specification of a distributed system,
> where the specification language happens to be a programming language.
> ..
> We are interested in the relationship of simulation-code coverage to measures of
> effectiveness in causing fault-revealing implementation failures.
> We argue that a test suite with a higher level of simulation-code coverage,
> under a valid adequacy criterion, will have a greater effectiveness
> at causing such failures in the implementation.
> ...
> Moreover, we are able to show that we can successfully establish an effectiveness ranking
> among adequate test suites, as well as among the adequacy criteria themselves.

[Alloy](http://alloy.mit.edu/alloy) is a model checker. Its syntax maps very closely to Object-Oriented Programming,
and seems a very practically and pragmatic tool.
D.Jackson argues for such "lightweight methods" in [Dependable Software by Design](https://pdfs.semanticscholar.org/fb88/0db022e60ae3bfc07636b356dccf4b1d7853.pdf), especially in using it to verify *design* of a system.

> First, regarding checking versus proof. Alloy is indeed a model finder and not a prover, so it cannot prove theorems without additional help (eg, a small model theorem). 
[Lambda the Ultimate: Alloy the book](http://lambda-the-ultimate.org/node/1555).

It argues that (small scope hypothesis):
> a high proportion of bugs can be found by testing a program for all test inputs within some small scope

[On the Small-Scope Hypothesis for Testing Answer-Set Programming](https://publik.tuwien.ac.at/files/PubDat_211480.pdf)
and [Evaluating the Small Scope Hypothesis, Andoni, 2002](http://citeseerx.ist.psu.edu/viewdoc/summary?doi=10.1.1.63.9997) (PostScript only).

http://alloy.mit.edu/alloy/tutorials/online/sidenote-operational-declarative.html
> Roughly speaking, there are two ways to describe a system: operationally (also called "imperative") and declaratively.
> An operational modeller asks "how would I make X happen?".
> A declarative modeller asks "how would I recognize that X has happened?". 

[An Approach for Embedded Software Generation Based in Declarative Alloy Models](http://www.lume.ufrgs.br/bitstream/handle/10183/22812/000740300.pdf). Models are developed in Alloy, and then Java is generated, and JVM bytecode runs on the embedded target.
A model+generation pattern is implemented for finite state machines.

To review

* Model Building in Mathematical Programming, H. PAUL WILLIAMS

### Prerequisites

Need a way to observe a programs effects on its environment, and.
For simple programs/models this may be just input/outputs of the program.
For more complex, multi-faceted programs some environment interactions may be "internal",
important that these still can be found and observed.

For simulations, important that one can inject a model into a program.
Drive program, observe changes in model. Or drive model, observe changes in program.
For pure simulations, desirable to disable the real environment influence/manipulation.
Desirable to drive model and real behavior in the same system, in order to directly observe
discrepancies.

Since models represent a system behavior over a (wide) range, important that one can
collect data about the real system, to validate the model with.


## Unrelated

[Tools for Thought by Howard Rheingold](http://www.rheingold.com/texts/tft/1.html)

