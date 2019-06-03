Protocole Ogar
===============

Messages Serveurs
------------------

## Ajouter une Cellule

### Paquet

| 1 Octet   | 4 Octets |
| :-------: |:--------:|
| 32        | Id       |

* **32 (unsigned int)** Identifiant du paquet.
* **Id (unsigned int)** Identifiant de la cellule.

## Supprimer toutes les Cellules

### Paquet

| 1 Octet   |
| :-------: |
| 18 ou 20  |

* **18 (unsigned int)** Identifiant du paquet.
* **20 (unsigned int)** Identifiant du paquet pour la **version 5** du protocole.

## Dessiner une ligne (Non utilisé)

### Paquet

| 1 Octet   | 2 Octets  | 2 Octets  |
| :-------: | :-------: | :-------: |
| 21        | X         | Y         |

* **21 (unsigned int)** Identifiant du paquet.
* **X (unsigned int)** Coordonnée X
* **Y (unsigned int)** Coordonnée Y

## Placer les bordures

### Paquet

| 1 Octet   | 8 Octets  | 8 Octets  | 8 Octets  | 8 Octets  |
| :-------: | :-------: | :-------: | :-------: | :-------: |
| 64        | Left      | Top       | Right     | Bottom    |

* **64 (unsigned int)** Identifiant du paquet.
* **Left (float)** Distance entre le centre et la bordure gauche.
* **Top (float)** Distance entre le centre et la bordure haute.
* **Right (float)** Distance entre le centre et la bordure droite.
* **Bottom (float)** Distance entre le centre et la bordure basse.

## Mettre à jour le tableau des scores

On n'utilisera pas ce paquet.

## Mettre à jour les Cellules

### Paquet
| 1 Octet   | 2 Octets  | ? Octets        | ? Octets | 4 Octet  | 2 ou 4 Octets`*` | ? x 4 Octets |
| :-------: | :-------: | :-------------: | :------: | :------: | :------------: | :----------: |
| 16        | DeSize    | Cellules Mortes | Cellules | 0        | RmSize         | Id           |


* **16 (unsigned int)** Identifiant du paquet.
* **DeSize (unsigned int)** Nombre de cellules mortes.
* **0 (unsigned int)** Valeur de fin pour la liste des cellules.
* **RmSize (unsigned int)** Nombre de cellules à désafficher.
* **Id (unsigned int)** Identifiant de la cellule à désafficher. Il y a au total (_RmSize_ cellules à désafficher)

`*` Cela dépend de la version du protocol : 4 Octets pour la version 5 et 2 pour les autres.

#### Cellules Mortes

| 4 Octets  | 4 Octets  |
| :-------: | :-------: |
| Id (Eaten)| Id (Eater)|

* **Id (Eaten) (unsigned int)** Identifiant de la cellule mangée.
* **Id (Eater) (unsigned int)** Identifiant de la cellule mangeuse.


#### Cellules

##### Protocole Version 5

| 4 Octets  | 4 Octets  | 4 Octets  | 2 Octets  | 1 Octet   | 1 Octet   | 1 Octet   | 1 Octet   | ? Octets      | 1 Octet   |
| :-------: | :-------: | :-------: | :-------: | :-------: | :-------: | :-------: | :-------: | :-----------: | :-------: |
| Id        | X         | Y         | Size      | R         | G         | B         | Flags     | Name (Unicode)| 0         |

##### Protocole Version ~

| 4 Octets  | 4 Octets  | 4 Octets  | 2 Octets  | 1 Octet   | 1 Octet   | 1 Octet   | 1 Octet   | ? Octets   | 1 Octet   |
| :-------: | :-------: | :-------: | :-------: | :-------: | :-------: | :-------: | :-------: | :--------: | :-------: |
| Id        | X         | Y         | Size      | Flags     | R         | G         | B         | Name (UTF8)| 0         |

* **Id (unsigned int)** Identifiant de la cellule.
* **X (int)** Coordonnée X de la cellule.
* **Y (int)** Coordonnée Y de la cellule.
* **Size (unsigned int)** Taille de la cellule.
* **R (unsigned int)** Composante rouge de la cellule.
* **G (unsigned int)** Composante verte de la cellule.
* **B (unsigned int)** Composante bleu de la cellule.
* **Flags (unsigned int)** Masque des valeurs suivantes
  * **1** La cellule possède des épines.
  * **2 `*`** Toujours présent.
  * **8 `*`** La cellule possède un nom
* **Name** Nom de la cellule.
* **0 (unsigned int)** Valeur de fin pour le nom de la cellule.

`*` Ces valeures ne sont pas présentes dans la **version 5** du protocole.

## Mettre à jour la Position

### Paquet

| 1 Octet   | 4 Octets  | 4 Octets  | 4 Octets  |
| :-------: | :-------: | :-------: | :-------: |
| 17        | X         | Y         | Size      |

* **17 (unsigned int)** Identifiant du paquet.
* **X (float)** Coordonnée X de la cellule.
* **Y (float)** Coordonnée Y de la cellule.
* **Size (float)** Taille de la cellule.


Messages Clients
-----------------

## Ouvrir une connexion

### Paquet

| 1 Octet   | 4 Octets        |
| :-------: | :-------------: |
| 254       | ProtocolVersion |

* **254 (unsigned int)** Identifiant du paquet.
* **ProtocolVersion (unsigned int)** Version du protocole à utiliser.


## Choisir un Nom

### Paquet

##### Protocole Version 5

| 1 Octet   | ? x 2 Octets |
| :-------: | :----------: |
| 0         | Name         |

* **Id (unsigned int)** Identifiant du paquet.
* **ProtocolVersion (unsigned int)** Version du protocole à utiliser.

##### Protocole Version ~

| 1 Octet   | ? Octets     |
| :-------: | :----------: |
| 0         | Name (UTF8)  |

* **0 (unsigned int)** Identifiant du paquet.
* **Name (unsigned int)** Nom du joueur.

## Passer en mode Spectateur

### Paquet

| 1 Octet   |
| :-------: |
| 1         |

* **1 (unsigned int)** Identifiant du paquet.

## Definir la position Cible

### Paquet

| 1 Octet   | 4 Octets  | 4 Octets  | 4 Octets |
| :-------: | :-------: | :-------: | :------: |
| 16        | X         | Y         | Cellules (0 pour toutes celles possédées)|

* **16 (unsigned int)** Identifiant du paquet.

## Split

### Paquet

| 1 Octet   |
| :-------: |
| 17        |

* **17 (unsigned int)** Identifiant du paquet.

## Touche Q enfoncée

### Paquet

| 1 Octet   |
| :-------: |
| 18        |

* **18 (unsigned int)** Identifiant du paquet.

## Touche Q relâchée

### Paquet

| 1 Octet   |
| :-------: |
| 19        |

* **19 (unsigned int)** Identifiant du paquet.

## Éjecter de la masse

### Paquet

| 1 Octet   |
| :-------: |
| 21        |

* **21 (unsigned int)** Identifiant du paquet.
