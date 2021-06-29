# Architectyre Styles

## Pipes and filters


### Overview
In a pipe and filter style each component has a set of inputs and a set of outputs. A component reads streams of data on its inputs and produces streams of data on its outputs
Among the important invariants of the style, filters must be independent entities: in particular, they should not share state with other filters. Another important invariant is that filters do not know the identity of their upstream and downstream filters. Their specifications might restrict what appears on the input pipes or make guarantees about what appears on the output pipes, but they may not identify the components at the ends of those pipes. Furthermore, the correctness of the output of a pipe and filter network should not depend on the order in which the filters perform their incremental processing—although fair scheduling can be assumed.
Not suitablefor systems that are interactive.

### Example
traditionally compilers have been viewed as a pipeline systems (though the phases are often not incremental). The stages in the pipeline include lexical analysis, parsing, semantic analysis, code generation. (We return to this example in the case studies.) Other examples of pipes and filters occur in signal processing domains [17], functional programming [18], and distributed systems

### Advantage
- First, they allow the designer to understand the overall input/output behavior of a system as a simple composition of the behaviors of the individual filters. 
- Second, they support reuse: any two filters can be hooked together, provided they agree on the data that is being transmitted between them. 
- Third, systems can be easily maintained and enhanced: new filters can be added to existing systems and old filters can be replaced by improved ones. 
- Fourth, they permit certain kinds of specialized analysis, such as throughput and deadlock analysis. 
- Finally, they naturally support concurrent execution. Each filter can be implemented as a separate task and potentially executed in parallel with other filters

### Disadvantage
- First, pipe and filter systems often lead to a batch organization of processing. 

- process data incrementally: since filters are inherently independent, the designer is forced to think of each filter as providing a complete transformation of input data to output data. In particular, because of their transformational character, pipe and filter systems are typically not good at handling interactive applications. This problem is most severe when incremental display updates are required, because the output pattern for incremental updates is radically different from the pattern for filter output. 

- lastly, they may be hampered by having to maintain correspondences between two separate, but related streams. Third, depending on the implementation, they may force a lowest common denominator on data transmission, resulting in added work for each filter to parse and unparse its data. This, in turn, can lead both to loss of performance and to increased complexity in writing the filters themselves.

## Object Oriented organization

In this style data representations and their associated primitive operations are encapsulated in an abstract data type or object. The components of this style are the objects—or, instances of the abstract data types. Objects interact through function and procedure invocations.

object is responsible for preserving the integrity of its representation (usually by maintaining some invariant over it) the representation is hidden from other objects.

## Event based

### Overview
Traditionally, in a system in which the component interfaces provide a collection of procedures and functions, components interact with each other by explicitly invoking those routines

Iinstead of invoking a procedure directly, a component can announce (or broadcast) one or more events. Other components in the system can register an interest in an event by associating a procedure with the event. When the event is announced the system itself invokes all of the procedures that have been registered for the event. Thus an event announcement implicitly causes the invocation of procedures in other modules.

*The main invariant of this style is that announcers of events do not know which components will be affected by those events*

### Example

For example, in the Field system [23], tools such as editors and variable monitors register for a debugger’s breakpoint events. When a debugger stops at a breakpoint, it announces an event that allows the system to automatically invoke methods in those registered tools. These methods might scroll an editor to the appropriate source line or redisplay the value of monitored variables. In this scheme, the debugger simply announces an event, but does
Garlan & Shaw: An Introduction to Software Architecture 
not know what other tools (if any) are concerned with that event, or what they will do when that event is announced.

- database management systems to ensure consistency constraints [22, 25], 
- in user interfaces to separate presentation of data from applications that manage the data [26, 27], 
- and by syntax-directed editors to support incremental semantic checking [28, 29

### advantage

### disadvantage

The primary disadvantage of implicit invocation is that 
- components relinquish control over the computation performed by the system. When a component announces an event, it has no idea what other components will respond to it. Worse, even if it does know what other components are interested in the events it announces, it cannot rely on the order in which they are invoked. Nor can it know when they are finished. 
- Sometimes data can be passed with the event. But in other situations event systems must rely on a shared repository for interaction. In these cases global performance and resource management can become a serious issue. 
- Finally, reasoning about correctness can be problematic, since the meaning of a procedure that announces events will



## layered archutecture

### Overview
A layered system is organized hierarchically, each layer providing service to the layer above it and serving as a client to the layer below. In some layered systems inner layers are hidden from all except the adjacent outer layer, except for certain functions carefully selected for export. Thus in these systems the components implement a virtual machine at some layer in the hierarchy. (In other layered systems the layers may be only partially opaque.) The connectors are defined by the protocols that determine how the layers will interact. Topological constraints include limiting interactions to adjacent layers.

### advantage

First, they support design based on increasing levels of abstraction. This allows implementors to partition a complex problem into a sequence of incremental steps. Second, they support enhancement. Like pipelines, because each layer interacts with at most the layers below and above, changes to the function of one layer affect at most two other layers. Third, they support reuse. Like abstract data types, different implementations of the same layer can be used interchangeably, provided they support the same interfaces to their adjacent layers. This leads to the possibility of defining standard layer interfaces to which different implementors can build. (A good example is the OSI ISO model and some of the X Window System protocols.)

### disadvantage

But layered systems also have disadvantages. Not all systems are easily structured in a layered fashion. (We will see an example of this later in the case studies.) And even if a system can logically be structured as layers, considerations of performance may require closer coupling between logically high-level functions and their lower-level implementations. Additionally, it can be quite difficult to find the right levels of abstraction. This is particularly true for standardized layered models. One notes that the communications community has had some difficulty mapping existing protocols into the ISO framework: many of those protocols bridge several layers.


## Repositories

In a repository style there are two quite distinct kinds of components: a central data structure represents the current state, and a collection of independent components operate on the central data store. Interactions between the repository and its external components can vary significantly between systems.
The choice of control discipline leads to major subcategories. If the types of transactions in an input stream of transactions trigger selection of processes to execute, the repository can be a traditional database. If the current state of the central data structure is the main trigger of selecting processes to execute, the repository can be a blackboard.

## Distributed systems 

have developed a number of common organizations for multi-process systems. Some can be characterized primarily by their topological features, such as ring and star organizations. Others are better characterized in terms of the kinds of inter-process protocols that are used for communication (e.g., heartbeat algorithms).

One common form of distributed system architecture is a “client-server” organization. In these systems a server represents a process that provides services to other processes (the clients). Usually the server does not know in advance the identities or number of clients that will access it at run time. On the other hand, clients know the identity of a server (or can find it out through some other server) and access it by remote procedure call.

## State transition systems

A common organization for many reactive systems is the state transition system. These systems are defined in terms a set of states and a set of named transitions that move a system from one state to another.

## Process control systems: 
Systems intended to provide dynamic control of a physical environment are often organized as process control systems. These systems are roughly characterized as a feedback loop in which inputs from sensors are used by the process control system to determine a set of outputs that will produce a new state of the environment.
