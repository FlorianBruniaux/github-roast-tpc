---
name: analyze-github-profile
description: Use when analyzing a GitHub profile through the lens of its author's objective (build a flagship, raise funds, get hired, run a business). Reads repos, pinned items, contribution timeline and commits, then produces a structured profile sheet. Triggers on "analyse ce profil GitHub", "what does this GitHub profile say", "audit my github".
---

# analyze-github-profile

Lit un profil GitHub à travers l'intention de son auteur, jamais la qualité du code dans l'absolu. La grille de référence est dans `signals.md` à la racine du plugin. Cette fiche est consommée ensuite par `score-profile`.

## Entrée

Un `username` GitHub. Optionnel : un ou deux repos phares déjà connus pour cibler l'analyse de commits.

## Données à récupérer (via `gh`)

Identité et présence :
```
gh api users/{username}
```
Récupère name, bio, blog, company, location, hireable, followers, public_repos, created_at.

README de profil (peut renvoyer 404, c'est une information en soi) :
```
gh api repos/{username}/{username}/readme --jq .content 2>/dev/null | base64 -d
```

Repos non-fork, triés par étoiles :
```
gh api "users/{username}/repos?per_page=100&sort=updated" --paginate \
  --jq '.[] | select(.fork==false) | {name, stars: .stargazers_count, lang: .language, desc: .description, updated: .updated_at, archived: .archived, license: .license.spdx_id}'
```

Pinned repos (ce que l'auteur choisit de montrer) :
```
gh api graphql -f query='{ user(login: "{username}") { pinnedItems(first: 6, types: REPOSITORY) { nodes { ... on Repository { name stargazerCount isFork owner { login } primaryLanguage { name } } } } } }'
```

Timeline de contributions (rythme, weekend vs pro, spike IA) :
```
gh api graphql -f query='{ user(login: "{username}") { contributionsCollection { contributionCalendar { totalContributions weeks { contributionDays { date contributionCount weekday } } } } } }'
```
weekday 0 = dimanche, 6 = samedi. Un volume notable sur 0 et 6 = side projects et hobby.

Pour chaque repo phare (1 à 3 max) :
```
gh api "repos/{username}/{repo}/commits?per_page=30" --jq '.[].commit.message'
gh api repos/{username}/{repo}/releases --jq 'length'
gh api repos/{username}/{repo}/languages
```
Les messages de commit donnent la langue, la convention (conventional commits ou non), l'intention lisible (feat, fix), et le `Co-Authored-By` éventuel.

## Analyse

Objectif détecté. Croiser les signaux de la grille `signals.md` pour proposer une intention principale parmi : faire un gros projet, lever des fonds, se faire recruter, faire du business. Toujours justifier par des faits observés, jamais affirmer sans preuve.

Cohérence de DA et de naming. Comparer les noms de repos, les descriptions, le style. Des variantes proches d'un même nom entre plusieurs repos brouillent l'image.

Pinned. Distinguer les repos dont l'auteur est vraiment le moteur de ceux où il a une contribution mineure sur un gros projet. Interroger sans condamner.

Timeline. Rythme weekend vs uniquement pro, et tout spike marqué à l'arrivée des assistants IA. Ni bien ni mal en soi, ça dépend du poste visé.

README de profil. Présent et type CV, ou absent. Un profil orienté recrutement sans README de profil rate une occasion.

Signaux StarMapper (optionnel, ne pas recalculer). Si une intégration StarMapper est disponible, récupérer organic score, distribution géographique et gros poissons parmi les stargazers. Sinon, lire les signaux bruts gh (forks, watchers, releases). Rappel : l'organic score est gaté à 500 étoiles, en dessous il renvoie "insufficient".

## Sortie

Une fiche profil structurée, factuelle, réutilisable par `score-profile` :

1. **Objectif principal** détecté, avec deux ou trois faits qui le justifient.
2. **Signaux par catégorie** (README profil, repos phares, pinned, timeline, commits, cohérence DA), chacun avec un constat court.
3. **Repos phares** avec leur rôle dans la stratégie de l'auteur (vitrine, exercice, vrai produit, contribution).
4. **Points d'attention** : ce qui dessert l'objectif détecté.

Reste chirurgical. Cite les chiffres et les noms de repos observés. Pas de jugement moral, des constats.
