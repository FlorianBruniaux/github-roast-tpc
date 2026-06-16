---
name: analyze-github-profile
description: Use when analyzing a GitHub profile through the lens of its author's objective (build a flagship, raise funds, get hired, run a business). Reads repos, pinned items, contribution timeline and commits, then produces a structured profile sheet. Triggers on "analyse ce profil GitHub", "what does this GitHub profile say", "audit my github".
---

# analyze-github-profile

Lit un profil GitHub à travers l'intention de son auteur, jamais la qualité du code dans l'absolu. La grille de référence est dans `signals.md` à la racine du plugin. Cette fiche est consommée ensuite par `score-profile`.

## Entrée

Un `username` GitHub. Optionnel : un ou deux repos phares déjà connus pour cibler l'analyse de commits.

## Intake interactif (avant l'analyse complète)

Ne pas dérouler tout l'audit d'un bloc. Faire d'abord une passe rapide (profil, repos phares, pinned), former une hypothèse, puis la refléter à l'utilisateur et attendre sa réponse avant de finaliser.

1. Passe rapide : récupérer profil, top repos par étoiles, pinned.
2. Refléter l'hypothèse en une fois, format court : "D'après ce que je vois, ton objectif principal est X (parce que A, B). Cible probable : Y. C'est bien ça ?" Poser au maximum trois questions ciblées qui changent vraiment l'analyse (objectif réel, audience visée, ce que tu attends de ce repo, contrainte particulière).
3. Attendre la réponse. L'utilisateur confirme, corrige, ou complète.
4. Recalibrer sur l'objectif confirmé, puis lancer l'analyse complète et le scoring.

Si l'utilisateur a déjà donné l'objectif dans sa demande, sauter la question d'objectif et confirmer seulement les points manquants. Ne pas transformer l'intake en interrogatoire : trois questions utiles maximum, sinon on devine et on signale qu'on a deviné.

## Données à récupérer (via `gh`)

Identité et présence :
```
gh api users/{username}
```
Récupère name, bio, blog, company, location, hireable, followers, public_repos, created_at.

Liens et comptes sociaux du profil :
```
gh api users/{username}/social_accounts --jq '.[] | {provider, url}'
```
Plus le champ `blog` de l'appel précédent (site web). Inventorier ce qui est présent : bio, site web, LinkedIn, email, autres. Juger : la bio décrit-elle clairement le positionnement, le site web et le LinkedIn sont-ils renseignés et cohérents avec l'objectif, les liens forment-ils une chaîne (GitHub vers site vers LinkedIn). Un profil orienté diffusion ou recrutement sans site ni LinkedIn renseignés rate une porte d'entrée. Signaler tout lien manquant ou incohérent.

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

Docs LLM et SEO (pour chaque repo phare) :
```
gh api repos/{username}/{repo}/contents/llms.txt --jq '.name' 2>/dev/null
gh api repos/{username}/{repo}/contents/llms-full.txt --jq '.name' 2>/dev/null
gh api repos/{username}/{repo} --jq '{desc: .description, topics: .topics, homepage: .homepage}'
```
Présence d'un fichier LLM (`llms.txt`, `llms-full.txt`, variantes) et densité de mots-clés réels dans la description et les topics.

## Analyse

Objectif détecté. Croiser les signaux de la grille `signals.md` pour proposer une intention principale parmi : faire un gros projet, lever des fonds, se faire recruter, faire du business. Toujours justifier par des faits observés, jamais affirmer sans preuve.

Cohérence de DA et de naming. Comparer les noms de repos, les descriptions, le style. Des variantes proches d'un même nom entre plusieurs repos brouillent l'image.

Pinned. Distinguer les repos dont l'auteur est vraiment le moteur de ceux où il a une contribution mineure sur un gros projet. Interroger sans condamner.

Timeline. Rythme weekend vs uniquement pro, et tout spike marqué à l'arrivée des assistants IA. Ni bien ni mal en soi, ça dépend du poste visé.

README de profil. Présent et type CV, ou absent. Un profil orienté recrutement sans README de profil rate une occasion.

Cross-check des chiffres. Comparer les compteurs annoncés dans le profile README (stars, forks) avec le live gh, et montrer les deux. Un chiffre périmé qui sous-vend un flagship est un correctif gratuit.

Docs LLM et SEO. Repérer la présence d'un fichier LLM (`llms.txt`, `llms-full.txt`) et juger la findability : description, topics et titre alignés sur ce qu'un humain ou un moteur cherche. Voir les sections dédiées de `signals.md`.

Discipline de preuve. Pour chaque chiffre quantitatif avancé (contributions, commits, stars), citer la commande gh qui l'a produit.

Signaux StarMapper (optionnel, ne pas recalculer). Si une intégration StarMapper est disponible, récupérer organic score, distribution géographique et gros poissons parmi les stargazers. Sinon, lire les signaux bruts gh (forks, watchers, releases). Rappel : l'organic score est gaté à 500 étoiles, en dessous il renvoie "insufficient".

## Sortie

Une fiche profil structurée, factuelle, réutilisable par `score-profile` :

1. **Objectif principal** détecté, avec deux ou trois faits qui le justifient.
2. **Signaux par catégorie** (README profil, repos phares, pinned, timeline, commits, cohérence DA), chacun avec un constat court.
3. **Repos phares** avec leur rôle dans la stratégie de l'auteur (vitrine, exercice, vrai produit, contribution).
4. **Points d'attention** : ce qui dessert l'objectif détecté.

Reste chirurgical. Cite les chiffres et les noms de repos observés. Pas de jugement moral, des constats.
