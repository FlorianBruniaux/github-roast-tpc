---
description: Transforme l'analyse d'un profil en plan d'action, branché sur l'objectif (chercher du taf ou diffuser un projet)
argument-hint: <username>
allowed-tools: Bash, Read, Grep, WebFetch, Task
---

Construis le plan de prochaines étapes pour `$1` via le skill `route-next-steps`.

1. S'appuyer sur la fiche profil et la note (`analyze-github-profile`, `score-profile`). Les produire d'abord si absentes.
2. Confirmer la piste avec l'utilisateur : chercher du taf, ou diffuser un projet.
3. Piste taf : checklist de correctifs triés par retour sur effort, puis pont vers les offres TPC (lien fourni, jamais inventé).
4. Piste diffusion : draft de post Reddit ou LinkedIn copiable, factuel, calibré pour la plateforme.

Si `$1` est vide, demander le `username`.
