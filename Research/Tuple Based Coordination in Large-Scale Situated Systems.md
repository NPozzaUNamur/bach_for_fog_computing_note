# Article
[[tuple-based_coordination_in_large-scale_situated_systems.pdf]]
2021
# Summary
Aims to define and specified spaciotemporal tuple notions. To then present an implementation via aggregate processus.
The authors define multiple notion of spaciotemporal tuple as region, augmented event structure or tuple space evolution. It follow those notion to implement its own implementation that will be evaluated.
They show that the implementation satisfied the spaciotemporal requirement and that it does the job within given delay (in case of blocking operation)
# Notes
Exists extension of linda to handle several tuple space [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=5&selection=31,12,31,37&color=yellow|p.5]]

Exists extension for network-oriented in Linda [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=5&selection=60,0,65,38&color=yellow|p.5]]

Distributed Linda extension that use MANETs (not really IoT but more P2P computational division) [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=5&selection=87,0,89,45&color=yellow|p.5]]

*Spatial Tuple* is a spacial based coordination model [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=6&selection=33,0,36,17&color=yellow|p.6]]
Use decoration to specify the space point of a tuple [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=6&selection=36,35,36,75&color=yellow|p.6]]

*Syntaxe of spacial primitives* : \<op\>(\<Tuple\> @ \<Region\>) [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=6&selection=74,0,74,10&color=yellow|p.6]]

*Predefined syntaxe*: [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=6&annotation=566R|p.6]]
- `t @ here`:  Place the tuple in tuple space where process is running
- `t @ me`: Bind the Tuple to the process (that means that if process moved, the tuple moves too)

There is a space concern for tuple based coordination. Model should handle association between space location and node [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=7&selection=22,23,24,42&color=yellow|p.7]]

There is a concern about time, but I don't understand [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=7&selection=26,2,26,6&color=yellow|p.7]]

The author's model aims to be heterogeneous. So that it can fit with client-server architecture. [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=7&selection=58,2,58,28&color=yellow|p.7]]

Augmented Event Structure: 
	Set of event, messages, mapping between device and event, and mapping between event and sensors. Used to modeling distributed system by considering event and messages between devices, and track the state of sensor too.  [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=7&annotation=572R|p.7]]
	Message in same device represent the time (sequential execution) but the message between device represent the ability to interact, so the proximity of those two devices [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=8&selection=243,45,245,73&color=yellow|p.8]]

Spatio-temporal Region:
	Region is a set of event that share time and space proximity with a given event. Regions are relative to an event. The proximity is determined by $r(\epsilon, \epsilon') ∈ \{⊤, ⊥\}$. If $⊤$ then $\epsilon'$ belongs to the region $\epsilon$, denoted $r_\epsilon$ [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=8&selection=386,0,415,1&color=yellow|p.8]]
	$me_k$ is a region of all event that share the same device
	$here_k$ is a region of all region that share same locality, which can be share by multiple device

The author extend the Linda grammar by adding a *got* operator. A process that use *got* can just use it, nothing else. So it will be called inert process. [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=9&selection=202,10,202,17&color=yellow|p.9]]

$got^{T_o T_i}$ allow to track the access of processus $T_i$ for the tuple emit by processus $T_o$. [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=9&selection=341,11,354,6&color=yellow|p.9]]

Author define the notion of Tuple Space Evolution. It is for each event, the state of all processus at that time. So it mean to represent through the sequence of event, the evolution of the state of the processus. [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=10&selection=87,0,87,11&color=yellow|p.10]]

Not understood completely, but the author explain how operation of Linda will evolve in spaciotemporal system w.r.t. Tuple space evolution. [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=10&selection=681,0,681,39&color=yellow|p.10]]

The author show how a processus can run on multiple devices, not very relevant for thesis. But he used that to implement spaciotemporal tuple. [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=12&selection=249,0,249,19&color=yellow|p.12]]

The authors implement spaciotemporal Tuple using Aggregate Processes. It can be interesting to see how he resolve some problem that not depend on aggregate stuff. Strategies can be interessting too but else not relevant. [[tuple-based_coordination_in_large-scale_situated_systems.pdf#page=14&selection=46,0,46,58&color=yellow|p.14]]

Not read the end of article
