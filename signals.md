# Grille de signaux (source unique)

Référence partagée Florian + Gabriel. Chaque skill et agent du plugin lit cette grille. Les deux relecteurs ne regardent pas les mêmes signaux, ce fichier est là pour ne pas diverger. Ajoutez vos signaux ici plutôt que dans le code.

## Discipline de preuve (règle transversale)

S'applique à tous les skills, au reporting comme aux recommandations.

Tout chiffre avancé (étoiles, forks, contributions, downloads, speedup, taille de binaire) cite la commande gh ou la source qui l'a produit. Un chiffre non vérifiable se marque `[à vérifier]`, jamais avancé comme un fait.

La même rigueur vaut pour ce qu'on recommande de publier. Un agrégat de stars dans une bio se calcule en additionnant des compteurs vérifiés et on montre le détail. Un intitulé de poste ou d'employeur proposé doit être réel, pas inventé. L'outil ne doit jamais faire commettre à l'utilisateur le claim gonflé qu'il reproche ailleurs.

Quand plusieurs sources d'un même chiffre divergent (header, tableau, roadmap, description), ne pas en élire une comme vérité. Signaler la divergence, dire où trancher (le code, l'enum, le compteur live), puis aligner toutes les sources sur la valeur vérifiée.

## Objectif détecté (le fil rouge)

Quatre intentions, cumul possible. L'agent doit toujours en proposer une principale.

| Objectif | Signaux |
|----------|---------|
| Faire un gros projet | flagship profond, commits soutenus, releases, README solide, licence |
| Lever des fonds | produit live, domaine propre, traction, communication externe |
| Se faire recruter | portfolio d'exercices, README de profil type CV, "open to work", diversité des projets |
| Faire du business | site sur domaine propre, vrai usage, parfois peu d'étoiles |

## README

Humain : screenshots, diagrammes, records terminal, vidéos, GIF, structure scannable. Un humain favorise l'image pour donner quelques minutes de cerveau à un lecteur pressé.

IA non assumée : em-dash en pagaille, pavés uniformes, tournures LLM, emojis et structure trop léchée, ouvertures stéréotypées.

IA assumée et OK : fichier `llms.txt` ou README machine destiné à être ingéré par un LLM. Là le contenu généré est légitime.

Essentiels : licence présente, quoi/pourquoi, installation, usage. Badge ou mention "en pause" si la maintenance est arrêtée (évite l'impression d'abandon). Le README n'est pas un changelog : l'historique de versions va dans CHANGELOG.md ou les releases, pas en haut du README où il noie le quoi/pourquoi. Métadonnées du repo : description GitHub renseignée et topics présents, sinon le repo est invisible à la recherche et au premier coup d'œil.

## Marqueurs IA (réutilise le skill anti-AI existant)

Em-dash, `Co-Authored-By` dans les commits (transparence, à laisser, pas à masquer), spike de la timeline de contributions à la sortie de Claude Code ou Cursor.

## Commits

Langue (anglais attendu sur un repo vitrine), conventional commits, intention lisible (fix, feat), découpage propre. Signal de travail en équipe.

## Profil

README de profil type CV, cohérence de DA et de naming entre repos (cas vu en live : trois variantes proches d'un même nom de projet entre repos, qui brouillent l'image). Liens cross-platform LinkedIn vers GitHub vers Medium pour la findability et le SEO. Vérifier aussi que les chiffres annoncés dans le profile README (stars, forks) collent au live : un compteur périmé qui sous-vend un flagship est un correctif gratuit, cas réel vu en audit, 2 000 annoncé contre 5 000 réels.

## Pinned repos

Les siens mis en avant vs une contribution à 1 commit sur un gros repo (cas vu en live : un pin à 50 étoiles dont l'auteur n'a fourni qu'une seule PR de doc). Interroger sans condamner : fierté, mise en avant, ou flou volontaire.

## Timeline de contributions

Commits le weekend = hobby et side projects vs uniquement professionnel. Spike à l'arrivée des assistants IA. Ni plus ni moins, ça dépend du poste recherché.

## Vie du projet (issues et PR)

Le repo vit-il ou semble-t-il abandonné. Croiser la date du dernier push, qui ouvre les issues (l'auteur pour son suivi vs de vrais utilisateurs qui rapportent des bugs ou demandent des features), et la nature des PR (contributions externes vs Dependabot ou pure maintenance). Un repo vitrine sans activité récente mérite une mention "en pause" assumée plutôt qu'un doute sur l'abandon. Cas vu en live : un repo en stand-by, l'auteur freelance n'a juste pas le temps, le dire haut et clair vaut mieux que de laisser croire à un projet mort.

## Outil pour soi (scratch your own itch)

L'auteur a construit le projet pour son propre besoin et l'utilise au quotidien. Signal fort côté recrutement, même avec peu d'étoiles : ça prouve une vraie problématique et un usage réel, pas un exercice. Cas vus en live : un outil de code review et un job crawler, tous deux construits pour résoudre un problème que l'auteur vivait. Indices : issues que l'auteur s'ouvre lui-même, dogfooding visible, un projet qui colle à un besoin clairement vécu.

## Docs LLM (discoverability AEO)

De plus en plus de projets publient un fichier machine pour les moteurs de réponse et les assistants (ChatGPT, Perplexity, Claude). Vérifier à la racine la présence de `llms.txt`, `llms-full.txt` et leurs variantes (`llm.txt`, `.well-known/llms.txt`). Présent : bon signal, un dev qui pense à la façon dont les LLM citent son projet. Absent sur un projet outil : occasion ratée, correctif rapide à proposer. Format de référence sur llmstxt.org.

## SEO et mots-clés

Findability au sens recherche, pas seulement la recherche interne GitHub. Le nom du repo, la description, les topics et le titre du README contiennent-ils les termes qu'un humain ou un moteur taperait pour trouver ce projet. Vérifier l'alignement entre l'intention de recherche de la cible et le vocabulaire réel du repo. Une description vide, un titre non descriptif ou l'absence de topics sont des trous SEO directs. Le projet doit ressortir sur Google et les moteurs de réponse, pas juste dans GitHub.

## Signaux StarMapper (à appeler, pas recalculer)

Organic score (forks, watchers, releases, contributeurs, ratio de comptes à 0 follower), distribution géographique, gros poissons (followers élevés parmi les stargazers). Rappel : l'organic score est gaté à 500 stars, en dessous il retourne "insufficient", lire les signaux bruts à la place. Réconcilier trois compteurs distincts plutôt que les juxtaposer : stars live GitHub, stars au moment du calcul du badge, et stargazers réellement scannés et géocodés. Expliquer l'écart au lecteur.

## Pondération /100 par objectif

Source unique pour le skill `score-profile`. Chaque catégorie est notée de 0 à 100, la note finale = Σ(score_catégorie × poids), arrondie à l'entier.

### Se faire recruter

| Catégorie | Poids |
|-----------|-------|
| README de profil | 0,22 |
| Findability | 0,20 |
| Repos phares | 0,15 |
| Cohérence DA/naming | 0,12 |
| Commits | 0,10 |
| Pinned | 0,10 |
| Timeline | 0,08 |
| StarMapper | 0,03 |
| **Total** | **1,00** |

### Lever des fonds

| Catégorie | Poids |
|-----------|-------|
| Repos phares | 0,25 |
| StarMapper | 0,20 |
| Findability | 0,20 |
| README | 0,18 |
| Commits | 0,08 |
| Cohérence DA/naming | 0,05 |
| Pinned | 0,04 |
| **Total** | **1,00** |

### Faire un gros projet

| Catégorie | Poids |
|-----------|-------|
| Repos phares | 0,28 |
| Commits | 0,18 |
| README | 0,18 |
| Timeline | 0,14 |
| Findability | 0,10 |
| Cohérence DA/naming | 0,07 |
| Pinned | 0,05 |
| **Total** | **1,00** |

StarMapper non inclus pour cet objectif (traction externe non prioritaire sur la profondeur technique).

### Faire du business

| Catégorie | Poids |
|-----------|-------|
| Repos phares | 0,22 |
| StarMapper | 0,20 |
| Findability | 0,20 |
| README | 0,15 |
| Commits | 0,10 |
| Cohérence DA/naming | 0,08 |
| Pinned | 0,05 |
| **Total** | **1,00** |

### Règle StarMapper absent

Quand StarMapper est indisponible (repo sous 500 stars ou non scanné), multiplier le poids de chaque catégorie restante par `1 / (1 - poids_StarMapper)` pour renormaliser à 1,0. Exemples : pour "lever des fonds" (StarMapper 0,20), multiplier par 1,25 ; pour "faire du business" (StarMapper 0,20), multiplier par 1,25 ; pour "se faire recruter" (StarMapper 0,03), multiplier par 1/0,97 ≈ 1,031. Appliquer l'arrondi à l'entier sur la note finale uniquement, pas sur les poids intermédiaires. Mentionner la renormalisation dans la sortie ("StarMapper indisponible, poids redistribués").
