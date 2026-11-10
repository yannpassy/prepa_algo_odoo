# Exercices — 16. Programmation dynamique

📖 Avant de commencer : relis la fiche [16 — Programmation dynamique](../16_programmation_dynamique.md).

## Comment t'y prendre

1. Commence par l'exercice **🎯 à faire ensemble** : on le résout ensemble au tableau.
2. Enchaîne les exercices **✍️ à toi**, dans l'ordre (ils vont du plus facile au plus difficile).
3. Les exercices **⭐ bonus** sont pour ceux qui vont vite.
4. Pour chaque exercice, applique la méthode en 5 étapes : **Lire** les mots-clés → **écrire la solution naïve** → **choisir** le pattern → **dérouler** à la main sur l'exemple → **tester** les cas limites.
5. Si tu bloques, regarde la colonne **Piste** : elle indique la variante du pattern à utiliser.

## Les exercices

| # | Exercice | Difficulté | Rôle | Piste | Fait ? |
|---|---|---|---|---|---|
| 1 | [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/) | 🟢 Facile | 🎯 à faire ensemble | dp[i] = dp[i-1] + dp[i-2] | [ ] |
| 2 | [746. Min Cost Climbing Stairs](https://leetcode.com/problems/min-cost-climbing-stairs/) | 🟢 Facile | ✍️ à toi | min des deux précédents | [ ] |
| 3 | [509. Fibonacci Number](https://leetcode.com/problems/fibonacci-number/) | 🟢 Facile | ✍️ à toi | dp[i] = dp[i-1] + dp[i-2] | [ ] |
| 4 | [1137. N-th Tribonacci Number](https://leetcode.com/problems/n-th-tribonacci-number/) | 🟢 Facile | ✍️ à toi | 3 cases précédentes | [ ] |
| 5 | [118. Pascal's Triangle](https://leetcode.com/problems/pascals-triangle/) | 🟢 Facile | ✍️ à toi | ligne précédente | [ ] |
| 6 | [198. House Robber](https://leetcode.com/problems/house-robber/) | 🟠 Moyen | ✍️ à toi | prendre ou sauter | [ ] |
| 7 | [322. Coin Change](https://leetcode.com/problems/coin-change/) | 🟠 Moyen | ⭐ bonus | min sur chaque pièce | [ ] |
| 8 | [300. Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) | 🟠 Moyen | ⭐ bonus | dp[i] = plus longue suite qui finit en i | [ ] |
| 9 | [62. Unique Paths](https://leetcode.com/problems/unique-paths/) | 🟠 Moyen | ⭐ bonus | grille 2D | [ ] |
| 10 | [1143. Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/) | 🟠 Moyen | ⭐ bonus | tableau 2D | [ ] |

> 💡 Sur LeetCode, ta solution doit être écrite dans la méthode d'une `class Solution`, comme dans le code de départ proposé par le site. Tous ces exercices sont gratuits (pas besoin de compte Premium).
