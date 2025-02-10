# Objectives
- Find for each module, and each feature, the best strategies to merge Bach from Scala to Rust
	- Can be from ground
	- Can use the Scala implementation and re code it in rust
	- Using transpiler. (Issue: Only one repertory on GitHub that has not begin development yet [Scala2Rust](https://github.com/doofin/scala2rust))
# Ressources
- BM01 - [[Ancien_mémoire.pdf|Bever's Master Thesis]]
- BM02 - [Server implementation for LiF, Berver's Master Thesis](https://github.com/Maxbever/LIF_Interpreter/blob/master/src/server.rs)
- JMJ01 - [[BachT.pdf|BachT Implementation Documentation]]
- JMJ02 - [[slideReactiveApplication.pdf|Slide Reactive Application Class]]
- Rust01 - [[The Rust Programming Language.pdf|RustBook]]
- DJAB01 - [[distributed_communication_via_global_buffer.pdf|Distributed Communication via Global Buffer]]
- GCLR01 - [[@res/TuSoW_Tuple_Spaces_for_Edge_Computing.pdf|Tuple Spaces for Edge Computing]]
# Strategies
## Module : The Store
---
### Description
Its responsibilities is to :
- Create and manage the tuple space
- Allow interaction with tuple space through Bach's primitives (tell, get, ask, nask) [[BachT.pdf#page=8&selection=200,0,200,9&color=important|JMJ01]]
- Allow multiple thread to access it (concurrency) [BM01](obsidian://open?vault=M%C3%A9moire&file=%40res%2FAncien_m%C3%A9moire.pdf)
- Handle externe processus accessing the space [BM01](obsidian://open?vault=M%C3%A9moire&file=%40res%2FAncien_m%C3%A9moire.pdf)
### Scala Implementation
- Uses Mutable Map collection (token -> Nbr occurrences)
- Uses Basic operation like concatenation of Map, key base access to Map,  
- Don't handle concurrency securely
- Don't handle IP based access
### Characteristics
Following [[distributed_communication_via_global_buffer.pdf|DJAB01]], there are 4 characteristics that belong to shared space.
1. It can be access only via predefined operation.
2. Can be implemented on single or multiple physicals memories.
3. Content can't be altered, so that process can be executed simultaneously.
4. Content aren't accessed by address but by name/content.
### Solutions
#### Map
The Map class in Scala instanced HashMap if keys number are higher than 4. [ref](https://stackoverflow.com/a/31685958)
Those two share characteristics [refScala](https://docs.scala-lang.org/overviews/collections-2.13/maps.html) [refRust](https://doc.rust-lang.org/std/collections/struct.HashMap.html):
	- get method return Option type (Some if key present, None else)
- exists an mutable version of the hashmap
- operation has similarities (+=/insert, ++=/extend, remove, etc)

It worth noting that Int in Scala is a 32-bit signed integer. Therefore, and regarding the fact that we don't allow negative number of occurrence, we should use 32-bit or more unsigned integer. ([ref](https://www.scala-lang.org/api/current/scala/Int.html))
#### Concurrency
The current state of Scala implementation doesn't seems to handle concurrency.
Rust handle concurrency by encapsulating value in Mutex (or RwLock), which is in (a)rc.
Mutex ([[The Rust Programming Language.pdf#page=456&selection=33,0,37,16|Rust01]]) allow 1 access to the value at a time and arc ([[The Rust Programming Language.pdf#page=462&annotation=9729R|Rust01]]) allow multiple owner for the value. Useful when processing multiple threads.
RwLock ([[Ancien_mémoire.pdf#page=47&annotation=1875R|BM01]]) is an alternative to Mutex that are allow multiple read but one write at the time. 
Question: Does you need to use mutex if content can't be altered, but removed, added and read ? 
#### Ip-based access
![[slideReactiveApplication.pdf#page=87|Schema of distributed use case]]
[[slideReactiveApplication.pdf|JMJ02]]
There are multiple solutions to bring remote access to shared spaces.
In [[distributed_communication_via_global_buffer.pdf|DJAB01]], there is a network of processor, that have to communicate with each others.

Following [BM01](@res/Ancien_mémoire.pdf), the communication system is based on client server architecture. [[@res/TuSoW_Tuple_Spaces_for_Edge_Computing.pdf|GCLR01]]
## Module : Parser
---
### Description
It has the responsibility to parse the programme into data that can be used by the simulator module. [[BachT.pdf#page=8&selection=200,0,200,9&color=important|JMJ01]]
### Scala Implementation
It is based on a parser called RegexParser.
It return case class representation of the input.
It use some parser's operations specific:
- Sequence (~) => Specify parsing sequence, that return a bigger parser. ([ref](https://www.scala-lang.org/api/2.12.6/scala-parser-combinators/scala/util/parsing/combinator/Parsers.html))
- Map (^^) => Apply function to the return expression
### Solution
Can't just translate code from Scala to Rust because of RegexParser usage.
Use Scala implementation to re-implemente with [*nom*](https://docs.rs/nom/latest/nom/) crate in Rust.
[*nom*](https://docs.rs/nom/latest/nom/) is the most popular parser in Rust. But there are some critics to it. ([ref](https://www.reddit.com/r/rust/comments/129qohw/should_i_revisit_my_choice_to_use_nom/))
*nom* seems to allow what regexParser allows. (exp.\*/many(exp), exp.?/opt(exp), exp~exp/and(exp, exp)), and he is also based on function like regexParser.
Scala brings priority information, that has to be respected in the rust implementation.
Scala's *Case Class* can be handle by Rust's enum, but it is not a perfect fit. [Rust01](The Rust Programming Language.pdf#page=134&annotation=9732R) 

## Module: Simulator
---
### Description
Aims to execute agent.
### Scala implementation
Uses Case class and matching flow.
Depends on the return of parsing. (CaseClass)
Not using high level of Scala's feature.
### Solution 
Basic One-to-One translation.
Case class can be represented as enum
Worth noting that rust has memory security feature that doesn't allow enum to get stored data in the enum but in the instance of the enum.
## General
---
### Object
*object* is the Scala implementation for singleton. Rust doesn't have something similar. ([ref](https://stackoverflow.com/questions/27791532/how-do-i-create-a-global-mutable-singleton))

# Questions
---
1. What's the best style of architecture for the network based approch ? (Conc As A Service) [[@res/TuSoW_Tuple_Spaces_for_Edge_Computing.pdf|GCLR01]]
	- Client - Server (What we want to avoid in fog computing)
	- N-layer (Each share space has to be connected to each others)
	- P2P (Each device as blackboard)
2. Do we have to implement some concurrency feature (mutex) if a value isn't altered ?
3. How to translate Scala's object structure in Rust ?
4. Nom is that really the best solution ? Not using ANTLR or other parsing generator.
5. Owner/borrower can still bring issues.
