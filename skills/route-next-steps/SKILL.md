---
name: route-next-steps
description: Use when turning a GitHub/LinkedIn analysis into a concrete next-step plan, branching on the user's goal. Two tracks, getting hired and spreading a project. Triggers on "et maintenant je fais quoi", "what next", "je cherche du taf", "je veux diffuser mon projet".
---

# route-next-steps

Transforme une analyse en plan d'action concret, branché sur ce que la personne veut vraiment. La grille de signaux est dans `signals.md` à la racine du plugin. On part de l'objectif détecté par `analyze-github-profile`, et on confirme avec l'utilisateur avant de router.

## Entrée

La fiche profil et la note de `analyze-github-profile` et `score-profile`. Si elles n'existent pas, les produire d'abord. Puis demander, ou déduire de l'objectif détecté, laquelle des deux pistes suivre.

## Piste 1 : je cherche du taf

1. Reprendre les correctifs de `score-profile`, triés par retour sur effort.
2. Prioriser ceux qui pèsent pour un recruteur : README de profil type CV, signal open-to-work, READMEs propres sur les repos phares, findability.
3. Faire le tri du profil. Faire remonter en pinned les repos qui servent l'objectif (les vraies réussites, les feats), ajouter une description et des topics aux repos nus, archiver ou retravailler les vieux repos faibles qui dessert l'image. Un recruteur peut cliquer sur le mauvais repo : autant qu'il tombe sur le bon. Compter une à deux heures, pas cinquante.
4. Si le LinkedIn a été analysé, intégrer les correctifs d'alignement GitHub vs LinkedIn.
5. Orienter vers les offres d'emploi de la communauté TPC. Le lien des offres est fourni par l'utilisateur ou renseigné dans le README du plugin ; ne pas inventer d'URL.

Sortie : une checklist ordonnée d'actions avant de postuler, puis le pont vers les offres.

## Piste 2 : je diffuse mon projet

1. Identifier le repo à diffuser et son angle (le problème qu'il résout, pour qui).
2. Choisir les canaux selon l'audience du projet, pas juste un par défaut. Reddit (le subreddit où vit la cible), LinkedIn, Hacker News (trafic fort mais communauté rude et très opinionated, à prendre avec du recul), un article de blog, et les communautés où l'auteur est déjà présent (Slack, Discord). Mentionner aussi la voie speaker : proposer une démo en live ou un talk. Le succès d'un projet open source ne tient pas qu'au code, il tient au faire-savoir.
3. Générer un draft de post adapté à la plateforme visée.
4. Modèle de référence : un post honnête et utile, qui raconte le problème et la solution sans survente, du type de celui qui a fait décoller un projet outil. Pas de superlatifs, une démo ou un GIF, un appel au retour. Prévenir que le ton diffère selon la communauté : la FR est souvent plus bienveillante, les espaces US plus durs, ne pas le prendre comme un signal de qualité.

Sortie : un draft de post copiable, calibré pour la plateforme, avec le titre, le corps, et où mettre le lien et le visuel.

## Règles

- Confirmer la piste avant de produire. Les deux ne se traitent pas pareil.
- Aucun lien inventé. Les liens externes (offres TPC) sont fournis, pas devinés.
- Un draft de post reste factuel et honnête, jamais du marketing creux.
