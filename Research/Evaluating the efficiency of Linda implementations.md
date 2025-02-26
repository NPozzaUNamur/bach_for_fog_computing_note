# Article
[[Evaluating the efficiency of Linda implementations.pdf]]
# Summary
This article **presents** multiple **implementation** of the **Linda** Model. Each implementation has its own feature, particularities but has some commun points shared with some of them. Like Security, Tuple clustering, eval operation, etc.
Then it **provides** **benchmark** to compare implementation and therefore summarized it into **2 advice**.

It can help by showing **state-of-the-art of the implementation** of coordination model and **what choice is crucial to have efficient implementation**
# Notes
Definition decoupling Linda [[Evaluating the efficiency of Linda implementations.pdf#page=1&annotation=756R|Evaluating the efficiency of Linda implementations, p.1]]

Coordination not used 'cause lack guarantees on consistency [[Evaluating the efficiency of Linda implementations.pdf#page=2&annotation=757R|Evaluating the efficiency of Linda implementations, p.2]]

No mechanism to handle faults [[Evaluating the efficiency of Linda implementations.pdf#page=2&selection=34,19,34,60&color=yellow|Evaluating the efficiency of Linda implementations, p.2]]

Look up by hashing value is good thing for performances [[Evaluating the efficiency of Linda implementations.pdf#page=3&selection=174,22,174,135&color=yellow|Evaluating the efficiency of Linda implementations, p.3]]

Possible to achieve pattern matching with SQL like query [[Evaluating the efficiency of Linda implementations.pdf#page=3&annotation=760R|Evaluating the efficiency of Linda implementations, p.3]]

Possibility to add process grant check on localities and operation [[Evaluating the efficiency of Linda implementations.pdf#page=3&selection=211,2,211,74&color=yellow|Evaluating the efficiency of Linda implementations, p.3]]

Possibility to change fetching strategy, pattern matching is the classical way, but you can use First In, First Out (FIFO), Last In, First Out (LIFO), etc. [[Evaluating the efficiency of Linda implementations.pdf#page=4&annotation=763R|Evaluating the efficiency of Linda implementations, p.4]]

Exists some solution to [Byzantine fault](https://en.wikipedia.org/wiki/Byzantine_fault) [[Evaluating the efficiency of Linda implementations.pdf#page=4&annotation=766R|Evaluating the efficiency of Linda implementations, p.4]]

Exists solution for P2P architecture: Distributed Hashed Tabled  ([DHT](https://docs.rs/kademlia-dht/latest/kademlia_dht/)) [[Evaluating the efficiency of Linda implementations.pdf#page=4&annotation=769R|Evaluating the efficiency of Linda implementations, p.4]]

Exists lock just on element not the whole table ([forumRust](https://users.rust-lang.org/t/locking-only-one-entry-in-hashmap/84764/3) [DashMapCrate](https://lib.rs/crates/dashmap)) [[Evaluating the efficiency of Linda implementations.pdf#page=4&annotation=772R|Evaluating the efficiency of Linda implementations, p.4]]

Exists history based search, use old communication to give likelihood value of tuple presence in tuple space [[Evaluating the efficiency of Linda implementations.pdf#page=4&annotation=775R|Evaluating the efficiency of Linda implementations, p.4]]

Provides some testing methodologies [[Evaluating the efficiency of Linda implementations.pdf#page=5&annotation=778R|Evaluating the efficiency of Linda implementations, p.5]]

Inter-process and Inter-machine communication strategy are critical choice to get efficient implementation [[Evaluating the efficiency of Linda implementations.pdf#page=19&annotation=781R|Evaluating the efficiency of Linda implementations, p.19]]

Implementation of the tuple space is also a critical choice to get efficient implementation [[Evaluating the efficiency of Linda implementations.pdf#page=19&annotation=784R|Evaluating the efficiency of Linda implementations, p.19]]



