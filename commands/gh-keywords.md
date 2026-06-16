---
description: Optimise la findability d'un profil GitHub (descriptions, topics, bio, cross-linking)
argument-hint: <username>
allowed-tools: Bash, Read, Grep, WebFetch, Task
---

Optimise la findability du profil GitHub `$1` en déléguant à l'agent `keyword-strategist`.

L'agent doit :
1. Récupérer bio, descriptions et topics actuels des repos de `$1`.
2. Appliquer le skill `suggest-keywords`.
3. Rendre une bio avant/après, un tableau de descriptions et topics proposés par repo, et les liens cross-platform manquants. Tout doit être copiable directement dans GitHub.

Si `$1` est vide, demander le `username` à analyser.
