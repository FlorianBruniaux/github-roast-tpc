# github-roast-tpc

Plugin Claude Code qui analyse un profil GitHub à travers l'intention de son auteur. Il évalue les README, détecte les marqueurs IA, lit les signaux recruteur, et conseille des keywords pour la findability. Issu du live TPC GitHub Roast (ep. 2).

## Philosophie

On lit chaque repo via son objectif, pas via la qualité du code dans l'absolu. Quatre objectifs servent de grille : faire un gros projet, lever des fonds, se faire recruter, faire du business. Peu d'étoiles ne veut pas dire peu de valeur.

Principe technique : on orchestre, on ne réécrit pas. Les briques qui existent déjà (organic score et géo côté StarMapper, détection anti-AI côté skills perso) sont appelées, pas dupliquées.

## État

| Phase | Contenu | Statut |
|-------|---------|--------|
| 0 | Scaffold plugin | fait |
| 1 | eval-readme (skill + agent readme-critic) | fait |
| 2 | analyze-github-profile (objectif, pins, timeline, commits) | fait |
| 3 | score-profile (note /5 + tableau + correctifs) | fait |
| 4a | suggest-keywords (findability, SEO, cross-linking) | fait |
| 4b | analyze-linkedin-profile (entrée multimodale) | fait |
| 5 | route-next-steps (offres TPC / draft post Reddit) | fait |
| 6 | packaging communautaire | à venir |

## Composants Phase 0-1

- `commands/gh-readme.md` : commande `/gh-readme <repo>` pour évaluer un README seul
- `skills/eval-readme/SKILL.md` : critères et procédure d'évaluation
- `agents/readme-critic.md` : agent qui récupère et critique le README
- `signals.md` : grille de signaux partagée (source unique Florian + Gabriel)

## Usage

Évaluer un README seul :

```
/gh-readme owner/repo
```

L'agent récupère le README via l'API GitHub, applique la grille `eval-readme`, et rend un verdict human/IA, des points forts, et des correctifs prêts à appliquer.

Analyser un profil complet :

```
/gh-profile username
```

L'agent récupère repos, pinned, timeline et commits, détecte l'objectif principal de l'auteur, et rend une fiche profil avec les signaux par catégorie et les points d'attention.

Noter un profil /5 avec correctifs :

```
/gh-score username
```

Enchaîne analyse de profil, éval du README phare, puis note /5 pondérée par l'objectif détecté, tableau de signaux, et correctifs triés par retour sur effort.

Optimiser la findability :

```
/gh-keywords username
```

Propose des descriptions et topics riches en mots-clés réels par repo, une bio optimisée, et les liens cross-platform manquants. Tout est copiable directement dans GitHub.

Croiser ton LinkedIn avec ton GitHub :

```
/gh-linkedin username
```

Tu fournis ton propre profil (screenshots ou copier-coller), l'agent vérifie l'alignement avec ton GitHub et rend les correctifs. Zéro scraping, zéro stockage.

Passer à l'action :

```
/gh-next username
```

Branche sur ton objectif. Piste taf : checklist de correctifs puis pont vers les offres TPC. Piste diffusion : draft de post Reddit ou LinkedIn prêt à poster.

## Configuration

Le lien des offres d'emploi TPC (utilisé par `/gh-next`, piste taf) n'est pas codé en dur. Renseigne-le ici ou fournis-le au moment de l'usage :

```
TPC_OFFRES_URL = (à renseigner)
```
