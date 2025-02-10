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
#### Les tests
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
#### Choix implémentation
- LifeTime: Ajout nécessaire d'une Lifetime dans `HashMap<&'a str,u32>` (`&'a` ). Celle-ci permet de définir la longévité de la référence afin quelle soit liée à l’existence de l'instance BachTStore qui la détient (au maximum, mais peut "mourir" avant). Rust à besoin de cela pour sa gestion de la mémoire. [refRustBook](https://doc.rust-lang.org/book/ch10-03-lifetime-syntax.html)
- &str instead of String: &str is more lightweight to process than String. Basically just (pointer, len) data structure. [rustFormum](https://users.rust-lang.org/t/str-vs-string-for-hashmap-key/102290) [sliceTypeRustBook](https://doc.rust-lang.org/book/ch04-03-slices.html) 
- Ré-implementation methode `tell`: Ici on j'utilise la puissance qu'offre Rust dans le cadre du paradigme fonctionnel.
- 