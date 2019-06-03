# Configuration du serveur

## Liste des commandes serveur

Les commandes serveurs sont parsées dans *index.js* et tirées de *modules/CommandList.js*

|  Propriété                        |  Doc                                                   |
|-----------------------------------|--------------------------------------------------------|
| **addbot [number]**               |    Ajoute un bot au serveur                            |
| **kickbot [number]**              |    Kick number bots                                    |
| **board [string] [string] …**     |    Affiche le string du tableau des scores             |
| **boardreset**                    |    Réinitialise le string du tableau des scores        |
| **change [setting] [value]**      |    Change le paramètre settings                        |
| **clear**                         |    Clear la console                                    |
| **color [PlayerID] [R] [G] [B]**  |    Attribue une couleur à un joueur                    |
| **exit**                          |    Stoppe le serveur                                   |
| **food [X] [Y] [mass]**           |    Fait apparaître de la nourriture a l'endroit desire |
| **gamemode [id]**                 |    Change le mode de jeu                               |
| **kick [PlayerID]**               |    Kick un bot ou un client par ID                     |
| **kickall**                       |    Kick tous les bots et les clients                   |
| **kill [PlayerID]**               |    Tue une cellule par ID                              |
| **killall**                       |    Tue tout le monde                                   |
| **mass [PlayerID] [mass]**        |    Definit la masse d'un client par ID                 |
| **merge [PlayerID]**              |    Assemble un client splitte en une seule cellule     |
| **name [PlayerID] [name]**        |    Change le nom d'une cellule par ID                  |
| **playerlist**                    |    Reccupere la liste des cellules                     |
| **pause**                         |    PAUSE, plus personne ne bouge                       |
| **reload**                        |    Reload la configuration                             |
| **resetantiteam [PlayerID]**      |    ???? reset anti-team effect on client               |
| **status**                        |    Statut du serveur                                   |
| **tp [PlayerID] [X] [Y]**         |    Teleporte un joueur par ID                          |
| **virus [X] [Y] [mass]**          |    Spawn un virus                                      |
| **pl**                            |    Alias de playerlist                                 |
| **st**                            |    Alias de status                                     |


## Règles du jeu

***GameServer.js*** (doc) et ***gameserver.ini*** (valeurs et doc)

Une tick de serveur fait **50ms**

### Flags de cellule

| Code    | Type |
|---------|------|
| 1010    | Player |
| 0010    | Food |
| 0011    | Virus |
| 0010    | Masse ejectée |

### Cellules (toutes)

La taille d'une cellule est **sqrt(100*masse)**

Les cellules n'ont pas d'inertie

### Joueur

Le joueur voit à **1024 x 592** unités de distance

Le joueur peut se split jusqu'en **16** fois si il a au moins **36** de masse

Il faut **30 + 0.02 \* masse** secondes (arrondi au supérieur) avant que les cellules puissent être recombinées

Le joueur commence avec **10** de masse et ne peut pas tomber en dessous de **9**

La vitesse initiale d’un joueur est **35**

Le joueur a une vitesse v = **vitesse_initiale\*masse ^ (-π/π^2/1.5)**

Le joueur perd **0.002** de masse par seconde

La vitesse du split est Vsplit = **vitesse_initiale \* min(150, (masse ^ (-π/π^2/10))\* (3 + log(1 + mass) / (10 + log(1 + mass))))** (voir l. 681)

On ne peut manger que les cellules dont notre masse est **1.3** fois supérieure, ou soit même **1** fois plus petit

### Food

La food est de taille **1** et a **0.5** chance de grossir jusqu'à **5** avec **+1** toutes les **120** secondes


### Virus

Un virus est de taille **100** et et explose au bout de **7** tirs de masse


### Masse ejectée

Le joueur peut expulser **13** de masse toutes les **100** *ms ?* à **100** unités par seconde de vitesse si il a au moins **32** de masse

*l721*

L'angle d'ejection est aléatoire (**+- 5%**)




## Déplacement du joueur

### Map

La map fait **7200 x 3200** unités

On peut sortir de **0.5** rayon de cellule de la map

### Mouvement

La cellule se déplace en direction de la souris :
- **x = sin(arctan((sourisX - posX)/(sourisY - posY)))**
- **y = cos(arctan((sourisX - posX)/(sourisY - posY)))**
