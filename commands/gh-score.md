---
description: Note un profil GitHub /5 orienté objectif, avec tableau de signaux et correctifs prêts à appliquer
argument-hint: <username>
allowed-tools: Bash, Read, Grep, WebFetch, Task
---

Note le profil GitHub `$1` de bout en bout.

1. Délègue à l'agent `github-profile-analyst` pour produire la fiche profil de `$1` (objectif détecté, signaux, repos phares).
2. Pour le repo phare principal, applique la grille `eval-readme` afin d'alimenter la catégorie README.
3. Applique le skill `score-profile` sur la fiche : note /5 pondérée par l'objectif détecté, tableau de signaux vert/orange/rouge, et correctifs triés par retour sur effort.

Si `$1` est vide, demander le `username` à analyser.
