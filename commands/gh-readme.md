---
description: Évalue le README d'un repo GitHub (qualité de communication, marqueurs IA, correctifs)
argument-hint: <owner/repo>
allowed-tools: Bash, Read, WebFetch, Grep, Task
---

Évalue le README du repo `$1` en déléguant à l'agent `readme-critic`.

L'agent doit :
1. Récupérer le README de `$1` via l'API GitHub.
2. Appliquer la grille du skill `eval-readme`.
3. Rendre un verdict (humain / IA non assumée / IA assumée), une note sur 5, un tableau de signaux, et des correctifs applicables.

Si `$1` est vide, demander le `owner/repo` à analyser.
