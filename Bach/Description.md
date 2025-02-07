# Quoi
**BachT** est un language implémentant une **version simplifiée** du modèle **Linda**. Il existe plusieurs implémentation de ce modèle.
**Linda** est définie comme un **language de coordination distribué**. Il faut donc comprendre que celui-ci **permet** une **coordination** de plusieurs **appareils** entre eux. Ceci est permis par la création d'un **espace partagé**, aussi connu comme *tuplespace* ou *blackboard*, et la définition de 4 **primitives**. Les voici:
- *tell(t)* -> **Ajouter** une **occurrence** de *t* dans l'*EP* (espace partagé)
- *get(t)* -> **Retirer** une **occurrence** de *t* dans l'*EP*
- *ask(t)* -> **Vérifier** la **présence** de *t* dans l'*EP*
- *nask(t)* -> **Vérifié** l'**absence** de *t* dans l'*EP*
L'une des différences entre Linda et Bach est le type de données accepté par l'EP.
Linda -> Tuple (ex. ("home", 3, 1, "Belgique"))
BachT -> Token (ex Home_3_1_Belgique)


# Pourquoi
# Comment
**Bach** est un language **interprété**, et nécessite donc un **interpréteur**. Celui-ci est décrit dans l'**article [suivant](BachT.pdf)**. L'un des objectif du mémoire est de [traduire cette interpreter](Implementation%20strategies.md) écrit en Scala vers le language Rust. 