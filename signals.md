# Grille de signaux (source unique)

Référence partagée Florian + Gabriel. Chaque skill et agent du plugin lit cette grille. Les deux relecteurs ne regardent pas les mêmes signaux, ce fichier est là pour ne pas diverger. Ajoutez vos signaux ici plutôt que dans le code.

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

Essentiels : licence présente, quoi/pourquoi, installation, usage. Badge ou mention "en pause" si la maintenance est arrêtée (évite l'impression d'abandon).

## Marqueurs IA (réutilise le skill anti-AI existant)

Em-dash, `Co-Authored-By` dans les commits (transparence, à laisser, pas à masquer), spike de la timeline de contributions à la sortie de Claude Code ou Cursor.

## Commits

Langue (anglais attendu sur un repo vitrine), conventional commits, intention lisible (fix, feat), découpage propre. Signal de travail en équipe.

## Profil

README de profil type CV, cohérence de DA et de naming entre repos (cas vu en live : trois variantes proches d'un même nom de projet entre repos, qui brouillent l'image). Liens cross-platform LinkedIn vers GitHub vers Medium pour la findability et le SEO.

## Pinned repos

Les siens mis en avant vs une contribution à 1 commit sur un gros repo (cas vu en live : un pin à 50 étoiles dont l'auteur n'a fourni qu'une seule PR de doc). Interroger sans condamner : fierté, mise en avant, ou flou volontaire.

## Timeline de contributions

Commits le weekend = hobby et side projects vs uniquement professionnel. Spike à l'arrivée des assistants IA. Ni plus ni moins, ça dépend du poste recherché.

## Signaux StarMapper (à appeler, pas recalculer)

Organic score (forks, watchers, releases, contributeurs, ratio de comptes à 0 follower), distribution géographique, gros poissons (followers élevés parmi les stargazers). Rappel : l'organic score est gaté à 500 stars, en dessous il retourne "insufficient", lire les signaux bruts à la place.
