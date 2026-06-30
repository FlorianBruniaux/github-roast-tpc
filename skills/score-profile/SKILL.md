---
name: score-profile
description: Use when turning a GitHub profile analysis into an actionable score. Produces a /100 grade weighted by the author's detected objective, a green/orange/red signal table, ready-to-paste corrective prompts, and a shareable score card. Triggers on "note ce profil", "score my github", "is my profile good for getting hired".
---

# score-profile

Transforme une analyse de profil en verdict actionnable. Une note ne vaut que rapportée à l'objectif de l'auteur : un profil "se faire recruter" ne se juge pas avec les critères d'un "lever des fonds". La grille de signaux est dans `signals.md` à la racine du plugin.

## Entrée

La fiche profil produite par `analyze-github-profile`, plus si disponible l'éval README de `eval-readme` et les signaux StarMapper. Si la fiche n'existe pas encore, lancer d'abord l'analyse de profil.

## Procédure

1. Confirmer l'objectif principal détecté. Toute la notation en découle.
2. Noter chaque catégorie de la grille ci-dessous, sur 100, pondérée par les poids définis dans `signals.md` (section « Pondération /100 par objectif »). Si StarMapper est indisponible, appliquer la règle de renormalisation décrite dans `signals.md`.
3. Agréger en une note /100 (Σ score_catégorie × poids, arrondie à l'entier) et lister les correctifs, du plus rentable au moins rentable.
4. Générer la carte de résultat (voir section Sortie).

Montrer le calcul de la note, pas seulement le résultat. Afficher le poids de chaque catégorie et l'arithmétique (par exemple `README 78×0,22 + findability 80×0,20 + repos phares 65×0,15 + … = 72/100`). Une note sans son détail n'est pas auditable. Tout chiffre repris dans la notation suit la discipline de preuve de `signals.md` : sourcé par une commande gh, ou marqué `[à vérifier]`.

## Pondération par objectif

Chaque objectif déplace le poids des catégories. Ce qui compte pour l'un est secondaire pour l'autre.

| Objectif | Ce qui pèse le plus | Ce qui pèse peu |
|----------|---------------------|-----------------|
| Se faire recruter | README de profil type CV, diversité du portfolio, READMEs propres, findability, signal open-to-work | nombre d'étoiles, traction externe |
| Lever des fonds | produit live, domaine propre, traction, communication externe, README qui vend | diversité du portfolio, exercices |
| Faire un gros projet | profondeur du flagship, releases, licence, commits soutenus, README solide | présence multi-plateforme |
| Faire du business | usage réel, domaine propre, valeur produit | nombre d'étoiles, fréquence de commits |

## Catégories notées

README (qualité de communication, via `eval-readme`). Repos phares (profondeur, releases, licence). Pinned (cohérence avec l'objectif). Timeline (rythme adapté au poste visé). Commits (langue, convention, intention). Cohérence DA et naming. Findability (présence multi-plateforme, descriptions et keywords). Signaux StarMapper si disponibles.

## Sortie

Quatre couches, dans cet ordre.

1. **Note /100** globale (`score_100: N`), avec une phrase qui rappelle l'objectif retenu et pourquoi la note est calibrée dessus.
2. **Tableau de signaux** : chaque catégorie en vert, orange ou rouge, avec la note partielle et une raison courte et factuelle.
3. **Correctifs prêts à appliquer**, triés par retour sur effort. Chacun est un prompt ou une action concrète, par exemple : nettoyer les em-dash du README, ajouter descriptions et topics aux repos, faire remonter les bons pins, ajouter un badge "en pause" sur un repo vitrine non maintenu, créer un README de profil type CV, aligner les liens LinkedIn et GitHub.
4. **Carte de résultat partageable.** Générer la carte en trois étapes :

   a. Télécharger l'avatar et l'encoder en base64 :
   ```bash
   curl -fsSL "https://github.com/<handle>.png?size=200" -o /tmp/<handle>-avatar.jpg
   B64=$(base64 -i /tmp/<handle>-avatar.jpg | tr -d '\n')
   ```

   b. Copier `scoring-card.html` (racine du plugin) vers `scoring-card-<handle>.html` dans le répertoire courant, puis effectuer trois substitutions dans le bloc JS :
   - `const HANDLE = 'torvalds'` → `const HANDLE = "<handle>"`
   - `const SCORE = 82` → `const SCORE = <score_100>`
   - `document.getElementById('avatar').src = 'https://github.com/' + HANDLE + '.png'` → `document.getElementById('avatar').src = 'data:image/jpeg;base64,' + B64` (avec la vraie valeur du B64 inline, pas la variable shell)

   L'embed base64 est obligatoire : la carte s'ouvre en `file://` et le navigateur bloque les requêtes externes dans ce contexte.

   c. Afficher le chemin :

```
Ta carte de score : ./scoring-card-<handle>.html
Ouvre le fichier dans un navigateur, puis "Télécharger l'image" ou fais un screenshot. Partageable sur LinkedIn.
```

Reste factuel. La note sert à prioriser l'action, pas à classer une personne.
