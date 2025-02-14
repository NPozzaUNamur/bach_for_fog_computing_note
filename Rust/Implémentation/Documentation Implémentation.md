# Objectif
L'implémentation de l’interpréteur Bach en Rust doit être réalisé en plusieurs étapes.
1. Re-implémentation du code Scala
2. Ajout des features d'accès réseaux
3. Ajouter feature sécurité
4. *To be determined*
# Phase 1
Re-implémentation du code Scala en Rust
## Motivation
Le code ne dépassant pas le milliers de lignes, la proximité entre le language Scala et le language Rust et l'utilisation de module spécifique à Scala m'a motivé à choisir la stratégie de ré implementation à la main. [[Implementation strategies|Détails]]
## Méthodologie
Pour chaque module, au nombre de trois à savoir:
1. The store (aka blackboard)
2. The parser
3. The simulator
Je commence par écrire les tests pour le programme Scala.
J'ajoute ses tests en premier lieu dans Rust afin de les utiliser comme des spécifications.
J'implémente le module en rust feature par feature selon les tests.
## Module 1 : The store
### Les tests
1. Tell opération
	1. Tell peut être effectuer qu'importe l'état du store [[BachT.pdf#page=2&annotation=258R|BachT, p.2]]
	2. Tell doit ajouter un token dans le store s'il n'existe pas [[BachT.pdf#page=8&selection=215,101,220,19&color=yellow|BachT, p.8]]
	3. Tell doit incrémenter le nombre d’occurrence si le token exist (Int signed est sur 32bit, que se passe-t-il si on dépasse le maximum) [[BachT.pdf#page=8&selection=215,55,215,87&color=yellow|BachT, p.8]]
	4. Tell ne doit pas permettre le dépassement de la valeur max Int (J'ai fais le choix d'ignorer. c-a-d la fonction s'execute mais n'a pas d'effet sur le store. Pour ne pas contredire [[BachT.pdf#page=2&annotation=258R|BachT, p.2]] et pour ne pas utiliser d'erreur) [[Implementation strategies#Strategies#Solutions#Map|MapScala2Rust]]
2. Ask opération
	1. Ask peut être exécuter que si le token est présent dans le store [[BachT.pdf#page=2&selection=161,88,166,23|BachT, p.2]]
	2. Ask ne peut donc pas s’exécuter s'il n'y a pas de token (ou 0 occurrence) dans le store.
3. Get Opération
	1. Get doit suivre les règles d'exécution de Ask sauf qu'il decrement le nombre d’occurrence du token cible [[BachT.pdf#page=2&selection=171,4,171,44|BachT, p.2]]
	2. Get ne peut donc pas s'éxécuter s'il n'y a pas de token (ou 0 occurrence) dans le store.
4. Nask opération
	1. Nask ne peut s'excécuter que s'il n'y a pas une ou plusieurs occurrence du token cible [[BachT.pdf#page=2&selection=177,6,181,27|BachT, p.2]]
	2. Nask ne s'exécute pas s'il y a au moins une occurrence du token cible
### Choix implémentation
- LifeTime: Ajout nécessaire d'une Lifetime dans `HashMap<&'a str,u32>` (`&'a` ). Celle-ci permet de définir la longévité de la référence afin quelle soit liée à l’existence de l'instance BachTStore qui la détient (au maximum, mais peut "mourir" avant). Rust à besoin de cela pour sa gestion de la mémoire. [refRustBook](https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html)
- ~~&str instead of String: &str is more lightweight to process than String. Basically just (pointer, len) data structure. [rustFormum](https://users.rust-lang.org/t/str-vs-string-for-hashmap-key/102290) [sliceTypeRustBook](https://doc.rust-lang.org/book/ch04-03-slices.html) ~~~ 
	- *&str* is a reference to a slice of string, that means that you have to get a String data that outlive (live more or same) the reference(&str) In our case, we can't use it to store token but we can still use it for manipulating it (ask and nask)
- Ré-implementation méthode `tell`: Ici on j'utilise la puissance qu'offre Rust dans le cadre du paradigme fonctionnel.
- Re-implementation of `get` method: As for the `tell` refactoring, we want to use the functional paradigm power.
- Trait: Using trait will allow more efficiency when updating the project and testing it (mocking).
### Question
- Not desallocation mechanism: Currently there is no desallocation of entry of zero occurrence token. That can have bad effect on long term
## Module 2 : The parser
### Tests
Tests are mostly based on Scala implementation
1. Primitives
	1. Le parser doit accepter tell
	2. Le parser doit accepter ask
	3. Le parser doit accepter get
	4. Le parser doit accepter nask
	5. Le parser doit refuser d'autre primitives hallucinées
2. Token
	1. Has to accept basic token syntax with lowercase
	2. Has to accept more complex syntaxe with capital and digit (but not on first character)
	3. Has to refuse token with special characters
	4. Has to refuse token that first character is not lowercase but digit or capitals
3. Agents
	1. Empty agent has to be handle by the parser ? TODO: Ask Prof. Jaquet why not implemented in Scala code but specified in the code and present in the theorical aspect [[On the Expressiveness of Coordination Models.pdf#page=6&selection=742,0,751,2&color=yellow|On the Expressiveness of Coordination Models, p.6]] [[BachT.pdf#page=2&selection=147,0,151,14&color=yellow|BachT, p.2]]
	2. Has to parse an basic agent (like primitives)
	3. Has to parse an agent in brackets
	4. Has to handle the sequence operation
	5. Has to handle the parallel operation
	6. Has to handle the choice operation
	7. Has to handle multiple operators (same one)
	8. Has to respect priority of operators [[BachT.pdf#page=6&selection=0,44,0,71&color=yellow|BachT, p.6]]
	9. Has to refuse hallucinate operators
	10. Has to consume the whole input
### Implementation choices
- Crate: Using [*nom*](https://docs.rs/nom/latest/nom/index.html) and [*regex*](https://docs.rs/regex/latest/regex/index.html), *nom* match with the regex parser of Scala in the way that each function is a parser. But *nom* doesn't support regex, so I uses *regex* crate to handle that part.
- Expr: There is no case class in rust, but enum handle some case class use of case. It fit with Expr because there is no parent parameters [[The Rust Programming Language.pdf#page=134&annotation=9732R|Rust01]]
	- Box: Expr in the BachtAstAgent is in Box du to recursive typing [[The Rust Programming Language.pdf#page=394&selection=36,36,45,7&color=yellow|Rust01, p.394]]
	- Trait: The enum implement two trait (Debug and partialEq), it for printing and equals operations [[The Rust Programming Language.pdf#page=622&selection=123,5,125,21&color=yellow|Rust01, p.622]] [[The Rust Programming Language.pdf#page=623&selection=12,1,18,24&color=yellow|Rust01, p.623]]
	- lifetime: As we work with references, rust wants us to specify a relative lifetime ('b)
- Regex: As *nom* doesn't implement regex parser, I've created mine by finding matches in the input and return the token (with non consumed characters in Ok structure). If no match then return error.
- Map: The scala implementation uses `^^` operators that applies a function on a value. In rust I use `.map` with anonyme function.
- Combinator: Scala uses four types of combinators, `~`, `rep`, `~>` and `|`
	- `~`: It is the sequence, in rust we make a tuple `(parser1, parser2).parse(i)` [nomDocumentation](https://docs.rs/nom/latest/nom/sequence/fn.tuple.html)
	- `rep`: It is the repetition (including zero), rust has the `many0` but for the use of case, I will use `opt` because the scala implementation uses `rep` to know if there is or not an input that can be parsed. That can be handle by `opt`, stands for optional. [nomDocumentation](https://docs.rs/nom/latest/nom/combinator/fn.opt.html)
	- `~>`: Its the lose sequence, it means that we don't want to keep what is produced by the first parser (`<~` exists too). Rust has `delimited` that handle the use of case. [nomDocumentation](https://docs.rs/nom/latest/nom/sequence/fn.delimited.html)
	- `|`: It is use to handle a set of possible parser for one input. In rust, we have `or_else` that can be applied to the result as scala does.
	- Other combinators are used by rust to handle the regex parser behavior
		- `all_consuming`: Allow to send Error if the input isn't all consumed.
		- `complete`: Handle the End Of File Error that can raise with optional parser.
### Question
- Empty agent: Parsing for empty agent aren't implemented, do I have to ?
- Opt instead of many0: many0 could be used to reduce recursivity into iteration.
## Module 3 : The simulator
### Tests
Using a Mock of the store to check the call count
1. Primitives
	1. Has to handle the tell, ask, nask and get primitive
	2. Has to refuse hallucinate primitive (should panic)
2. Agent (execute_all)
	1. Has to handle primitives
	2. Handle sequence combination
	3. Handle parallel combination
	4. Handle choice combination
	5. Refuse impossible execution (nask and ask for same token)
	6. Handle choice even if one side is not executable
	7. Refuse parallel if one side is not executable
	8. Handle complexe expression
### Implementation Choice
- Restructuration of initial agent: Due to rust ownership policy, I can't moved sub expression and moved the whole expression. In fact, this case happen only when nothing can be executed, so I use returned value that equals the sub expression to restructured the whole agent. [sourceCode](https://github.com/NPozzaUNamur/BachT_rust/blob/v25.2.0/src/bacht/bacht_simulator.rs#L17-L18)
- Panic if not matching: Rust doesn't handle auto raising error if not match found. So I specified panic instruction 
### Question
- Wake up mechanism: Currently, the implementation just return true/false wrt execution state. But we can implement observer pattern to wake up process as soon as a token appear (or disappear) from the blackboard.
