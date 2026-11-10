# DFS et BFS – Explorer un graphe ou une grille

## 1. Quel est le but de l'algorithme DFS ?

**DFS** (*Depth-First Search*, ou parcours en profondeur) permet d'explorer une grille ou un graphe de manière systématique.

- DFS va **au bout d'un chemin avant de revenir en arrière**.
- Il utilise une **pile** ou la **récursion** pour mémoriser les chemins à explorer.

Par comparaison, **BFS** (*Breadth-First Search*, ou parcours en largeur) explore couche par couche autour du point de départ. Il utilise une **file**.

## 2. Comprendre avec l'image d'un labyrinthe

### DFS – Parcours en profondeur

![Illustration d'un parcours en profondeur DFS](https://upload.wikimedia.org/wikipedia/commons/f/ff/Graph_based_maze_animation.gif)

DFS suit un couloir jusqu'au bout, puis revient au dernier embranchement pour explorer un autre chemin.

**Objectif :** explorer systématiquement les sommets accessibles.

BFS s'étale comme une goutte d'encre :
- toutes les cases à 1 pas ;
- puis toutes les cases à 2 pas ;
- puis toutes les cases à 3 pas…

Si tous les déplacements ont le même coût, le premier contact avec la sortie donne le **plus court chemin** en nombre de déplacements.

## 3. Pourquoi est-ce rapide ?

Naïvement, on pourrait être tenté de parcourir tous les chemins possibles, puis de vérifier lequel est le plus court.

Le problème :
- on risque de repasser par les mêmes sommets ou cases ;
- le nombre de chemins possibles peut devenir très grand ;
- certains parcours naïfs peuvent alors avoir une complexité exponentielle.

### Avec DFS, on évite de revisiter les mêmes sommets

1. On mémorise les sommets (ou cases) déjà visités.
2. Lorsqu'on rencontre un sommet déjà visité, on ne relance pas son exploration.
3. Chaque sommet est donc traité une seule fois.

> **Complexité :** `O(V + E)` pour un graphe représenté avec des listes d'adjacence, où `V` est le nombre de sommets et `E` le nombre d'arêtes.
>
> Pour une grille de `R` lignes et `C` colonnes, le parcours est généralement en `O(R × C)`.

## 4. Comment choisir entre DFS et BFS ?

| Ce que demande l'énoncé | Algorithme |
|---|---|
| « Plus court chemin » (avec des coûts égaux) | **BFS** |
| « Nombre minimum de pas / coups » | **BFS** |
| « Combien de zones / îles / groupes ? » | DFS ou BFS |
| « Peut-on atteindre… ? » | DFS ou BFS |
| « Explorer toutes les possibilités avec retour en arrière » | **DFS** |

### À retenir

- **BFS :** trouver le plus court chemin en nombre de déplacements lorsque chaque déplacement a le même coût.
- **DFS :** explorer en profondeur, parcourir des composantes connexes ou effectuer un retour en arrière.

---

# 5. Application de DFS : compter les îles

## Énoncé

On dispose d'une grille composée de `1` (terre) et de `0` (eau).

Une île est un groupe de cases de terre reliées **horizontalement ou verticalement**.

**Question : combien y a-t-il d'îles dans la grille ?**

Exemple de grille :

```text
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
```

Cette grille contient **3 îles**.

## Idée de l'algorithme

On parcourt la grille case par case grâce à une double boucle.

Dès qu'on rencontre une terre (`1`) qui n'a pas encore été visitée :

1. C'est le début d'une nouvelle île : on incrémente le compteur de `1`.
2. On lance une exploration DFS depuis cette case.
3. DFS explore toutes les cases de terre connectées à cette case.
4. On marque toutes ces cases comme visitées.

Ainsi, les autres cases appartenant à cette même île ne seront pas recomptées.

## Déroulement

1. On trouve une case de terre non visitée.
2. On ajoute `1` au compteur d'îles.
3. DFS explore toute l'île et marque ses cases comme visitées.
4. On continue le parcours de la grille.
5. On recommence lorsqu'on trouve une nouvelle île.

**Résultat :** le compteur final correspond au nombre total d'îles.

## Pseudo-code simplifié

```python
compteur = 0

for i in range(nombre_de_lignes):
    for j in range(nombre_de_colonnes):
        if grille[i][j] == 1 and not visite[i][j]:
            compteur += 1
            DFS(i, j)
```

**Point essentiel :** on ne compte pas chaque case de terre, mais chaque nouvelle zone de terre connectée. DFS permet de marquer toute une zone en une seule exploration.

---

Exercices LeetCode:
--------------------

| Exo | Difficulté |
|---|---|  
| [100. Same Tree](https://leetcode.com/problems/same-tree/description/) | <span style="color:lightgreen">Easy</span> |
| [112. Path Sum](https://leetcode.com/problems/path-sum/description/) | <span style="color:lightgreen">Easy</span> |
| [226. Invert Binary Tree](https://leetcode.com/problems/invert-binary-tree/description/) | <span style="color:lightgreen">Easy</span> |  
| [463. Island Perimeter](https://leetcode.com/problems/island-perimeter/description/)| <span style="color:lightgreen">Easy</span> |  



## Sources et crédits des illustrations

Les images ci-dessus sont hébergées sur Wikimedia Commons.