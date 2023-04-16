# Configuration
## Routage opérateur

J'ai décidé de mettre en place du routage dynamique pour avoir une configuration modulaire. Parmi les protocoles de routage que je connais se trouvent RIP, OSPF et BGP. Pour choisir entre ces trois protocoles, j'ai procédé par élémination. RIP n'est pas recommandé en raison de son temps de convergence élevé et des problèmes de bouclage. Par ailleurs, BGP est un EGP (Exterior Gateway Protocol) c'est-à-dire qu'il est utile pour du routage inter-AS (Autonomous System). Or, nous voulons mettre en place du routage à l'intérieur d'un réseau opérateur donc à l'intérieur d'un AS. Ainsi, le protocole de routage le plus adéquat à mettre en place est OSPF qui est une IGP (Interior Gateway Protocol) et qui a un faible temps de convergence.  
## Vlan
commercial -> 10
administration -> 20
technique -> 30
interne -> 40
