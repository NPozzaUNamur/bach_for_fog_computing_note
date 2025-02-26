1. Three Levels Blackboard
	1. Concept
		Use syntaxique extension tell(Token @ Region)
		Region can be either *me*, *local* or *global*.
		*me* target the device level blackboard
		*local* targets the blackboard that belongs to the local region of device, devices that share same router
		*global* targets the blackboard share by the whole system
	2. **Cons**:
		- Can't reach blackboard form another region
		- Limited number of blackboard
		- Lack of security
	3. **Pro**:
		- Simple solution
			- Don't need to handle tracking each blackboard
			- syntaxe keep simple form
			- Only 3 blackboard, so don't need to retrieve the target blackboard id
	4. Unknown aspect
		- Does it allows to resolve same number of problem that solution with id ?

2. Id based tracking
	1. Concept
		Uses syntaxique extension tell(Token @ BBId)
		BBId, aka blackboard id, is an unique identifier of a blackboard. It allows to target a specific blackboard
		When can add region feature from [1] but with *me* and *here*.
		*me* blackboard linked to the process.
		*here* blackboard linked to the local region
	2. **Cons**:
		- Need to tracked all blackboard, so it must always exist a service that do that jobs for all (what if not available ?)
	3. **Pro**:
		- Process can access to all blackboard (if allowed)
		- 