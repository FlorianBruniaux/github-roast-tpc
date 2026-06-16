---
description: Analyse un profil GitHub complet (objectif détecté, repos phares, pinned, timeline, commits)
argument-hint: <username>
allowed-tools: Bash, Read, Grep, WebFetch, Task
---

Analyse le profil GitHub `$1` en déléguant à l'agent `github-profile-analyst`.

L'agent doit :
1. Récupérer profil, repos non-fork, pinned, timeline de contributions et commits des repos phares de `$1` via l'API GitHub.
2. Appliquer la grille du skill `analyze-github-profile` et la grille partagée `signals.md`.
3. Rendre une fiche profil : objectif principal détecté avec justification, signaux par catégorie, repos phares avec leur rôle, et points d'attention.

Si `$1` est vide, demander le `username` à analyser.
