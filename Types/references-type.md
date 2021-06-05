## Reference types

Tout reference type a une annotation supplémentaire (la data location) sur l'endroit où elles sont stockées. Il y a trois options possibles : memory, storage et calldata.

`storage` : le type d'emplacement où les state variables sont stockées sur la blockchain de manière permanente, ce qui signifie que les types qui ont un `storage` location sont persistants.

memory: Variables are in memory and they exists during the function call which means variables that got this location are temporary and after function execution finished, they won’t exist.

`memory` : les variables qui sont stockées en `memory` existent pendant l'appel de la fonction, ce qui signifie que les valeurs qui ont cet emplacement sont temporaires et qu'une fois l'exécution de la fonction terminée, elles n'existeront plus.
