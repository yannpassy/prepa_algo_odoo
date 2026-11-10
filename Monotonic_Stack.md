# La pile monotone (*monotonic stack*)

## 1. Les bases

Une **pile monotone** est une pile ordinaire (LIFO) à laquelle on impose un **invariant** : les valeurs qu'elle contient restent toujours triées du fond vers le sommet.

- **Décroissante** : chaque élément est plus petit que celui du dessous. Elle sert à trouver le *prochain élément plus grand*.
- **Croissante** : chaque élément est plus grand que celui du dessous. Elle sert à trouver le *prochain élément plus petit*.

Avant d'empiler un nouvel élément, on **dépile tous ceux qui casseraient l'ordre**. Chaque élément dépilé vient de trouver sa réponse : c'est l'élément qu'on est en train d'insérer.

### Le squelette

```python
stack = []                                  # on stocke souvent des INDICES
for i, x in enumerate(arr):
    while stack and arr[stack[-1]] < x:      # x casse l'ordre décroissant
        j = stack.pop()
        # -> x est le prochain élément plus grand que arr[j]
    stack.append(i)
# ce qui reste dans la pile n'a pas de réponse
```

- On parcourt le tableau avec `enumerate`, qui donne l'index et l'élément.
  - Tant que la pile n'est pas vide et que l'élément actuel est plus grand que l'élément au sommet :
    - on enlève (`pop`) l'élément au sommet, qui vient de trouver sa réponse.
  - Quand la boucle `while` s'arrête, on ajoute l'élément actuel.

### Pourquoi ça marche ?

Quand `x` fait sortir un élément `arr[j]`, cet élément ne pourra **jamais** être la réponse d'un élément futur. `x` est plus grand que lui **et** plus proche de tout ce qui vient après. Il le « masque » donc définitivement.

On peut jeter `arr[j]` sans risque. La pile ne garde que les candidats encore utiles, et c'est précisément ce qui la rend triée.

### Strict ou non strict : `<` ou `<=` ?

« Triée » ne dit pas ce qui arrive aux **valeurs égales**. Ce choix change le résultat dès qu'il y a des doublons :

| Condition de pop | Les égaux… | On trouve le prochain élément… |
|---|---|---|
| `arr[stack[-1]] < x` | restent dans la pile | **strictement** plus grand |
| `arr[stack[-1]] <= x` | sont dépilés | plus grand **ou égal** |

Exemple avec `[2, 2, 3]` :
- avec `<` : `[3, 3, -1]`, car le premier 2 attend le 3 ;
- avec `<=` : `[2, 3, -1]`, car le premier 2 est « résolu » par le second 2.

Il faut toujours relire l'énoncé pour savoir si l'égalité compte.

### Les 4 variantes

| On cherche… | Pile | Condition de pop | Où lire la réponse |
|---|---|---|---|
| prochain plus grand (à droite) | décroissante | `arr[top] < x` | pour chaque élément **dépilé** |
| prochain plus petit (à droite) | croissante | `arr[top] > x` | pour chaque élément **dépilé** |
| précédent plus grand (à gauche) | décroissante | `arr[top] <= x` | le **sommet après les pops** |
| précédent plus petit (à gauche) | croissante | `arr[top] >= x` | le **sommet après les pops** |

Toutes se font en un seul parcours de gauche à droite. Après la boucle `while`, le sommet restant est le **précédent** plus grand (ou plus petit) de `x`. Une même passe peut donc donner à la fois le « prochain » des éléments dépilés et le « précédent » de l'élément courant.

### Ce qui reste dans la pile à la fin

Les éléments encore dans la pile n'ont **pas** de prochain plus grand. On n'a rien à faire pour eux : il suffit d'initialiser le tableau de résultats avec une **valeur par défaut** (`-1`, `0`…) qui veut dire « pas de réponse ». C'est ce que fait *Daily Temperatures* plus bas.

### Complexité

**O(n) en temps** malgré la boucle `while` imbriquée : chaque élément est empilé une fois et dépilé au plus une fois, soit au total au plus 2n opérations. La mémoire est en **O(n)**.

### Quand y penser ?

Quand l'énoncé demande, pour chaque élément, le **premier élément à sa droite (ou à sa gauche) plus grand ou plus petit que lui**. L'approche naïve, qui teste chaque paire, coûte O(n²).

---

## 2. Différence avec une « simple » pile

On compare avec la pile utilisée pour *Valid Parentheses* :

```python
# Pile simple : Valid Parentheses
for c in s:
    if c in '([{':
        stack.append(c)                        # on empile toujours
    elif not stack or stack.pop() != paires[c]:  # UN seul pop
        return False
```

| | Pile simple | Pile monotone |
|---|---|---|
| Contenu | ce qui est ouvert ou en attente | des candidats **triés**, en attente de leur réponse |
| Condition pour dépiler | un événement précis (une parenthèse fermante) | une **comparaison de valeurs** |
| Pops par élément lu | 0 ou 1 (`if`) | 0 à plusieurs (`while`) |
| Sens d'un pop | « apparié, terminé » | « j'ai trouvé ta réponse » |

La structure est la même. Ce qui change, c'est la **règle de dépilement** et ce qu'elle **signifie**.

---

## 3. Application : *Daily Temperatures* (LeetCode 739)

### Énoncé

Étant donné un tableau `temperatures`, il faut renvoyer `answer`, où `answer[i]` est le **nombre de jours à attendre** après le jour `i` pour avoir une température plus chaude. S'il n'y en a pas, on met `0`.

```
Entrée : [73, 74, 75, 71, 69, 72, 76, 73]
Sortie : [ 1,  1,  4,  2,  1,  1,  0,  0]
```

### Raisonnement

- On cherche le **prochain élément plus grand**, il faut donc une pile **décroissante**.
- On veut une **distance** en jours, donc on empile des **indices** et pas des valeurs.
- Quand le jour `i` est plus chaud que le sommet `j`, le jour `j` a trouvé sa réponse : `answer[j] = i - j`.

### Code

```python
def dailyTemperatures(temperatures: list[int]) -> list[int]:
    answer = [0] * len(temperatures)    # 0 par défaut = pas de jour plus chaud
    stack = []                          # indices, températures décroissantes

    for i, t in enumerate(temperatures):
        while stack and temperatures[stack[-1]] < t:
            j = stack.pop()
            answer[j] = i - j           # le jour j attendait le jour i
        stack.append(i)

    return answer
```

### Déroulé

| i | temp | Dépilés (réponse) | Pile après (indices → temp) |
|---|---|---|---|
| 0 | 73 | – | [0→73] |
| 1 | 74 | 0 (1) | [1→74] |
| 2 | 75 | 1 (1) | [2→75] |
| 3 | 71 | – | [2→75, 3→71] |
| 4 | 69 | – | [2→75, 3→71, 4→69] |
| 5 | 72 | 4 (1), 3 (2) | [2→75, 5→72] |
| 6 | 76 | 5 (1), 2 (4) | [6→76] |
| 7 | 73 | – | [6→76, 7→73] |

À la fin, les indices 6 et 7 sont encore dans la pile. Aucun jour plus chaud ne les suit, donc leur réponse reste `0`.

Résultat : `[1, 1, 4, 2, 1, 1, 0, 0]`, obtenu en **O(n)**.

---

## À retenir

1. Il faut **un invariant de tri** sur la pile.
2. On dépile avec un `while`, et **chaque pop donne une réponse**.
3. On choisit `<` ou `<=` selon que l'égalité compte ou non.
4. On stocke des **indices** dès qu'on a besoin d'une distance ou d'une position.
5. Pour aller plus loin, essayer *Next Greater Element II* (LC 503) et *Online Stock Span* (LC 901).
