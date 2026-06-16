---
name: suggest-keywords
description: Use when improving the findability of a GitHub profile and its repos. Generates keyword-rich repo descriptions and topics, optimizes the profile bio for search, and proposes cross-platform linking. Triggers on "améliore mes keywords", "make my repo findable", "seo for my github".
---

# suggest-keywords

Travaille la findability d'un dev : un recruteur ou un pair doit te trouver via Google et Perplexity, pas seulement via la recherche GitHub. C'est du SEO concret appliqué à un profil. La grille de signaux est dans `signals.md` à la racine du plugin.

## Entrée

Un `username`, ou la fiche profil déjà produite par `analyze-github-profile`. Si rien n'est fourni, récupérer repos et bio via `gh`.

## Procédure

1. Récupérer la bio du profil, et pour chaque repo non-fork : nom, description actuelle, topics actuels, langage principal.
```
gh api "users/{username}/repos?per_page=100" --paginate \
  --jq '.[] | select(.fork==false) | {name, desc: .description, topics: .topics, lang: .language, stars: .stargazers_count}'
```
2. Pour chaque repo phare, proposer une description courte et dense en mots-clés réels (ce que quelqu'un taperait pour trouver ce projet), et une liste de topics GitHub pertinents.
3. Proposer une bio de profil optimisée : rôle, stack, et un ou deux mots-clés de niche qui te différencient.
4. Vérifier le cross-platform linking : le profil pointe-t-il vers LinkedIn, et inversement, et vers un blog ou Medium si présent. Une chaîne de liens cohérente renforce le SEO et la findability.
5. Vérifier la présence d'un fichier LLM (`llms.txt`, `llms-full.txt`). Absent sur un projet outil, proposer de l'ajouter : c'est de l'AEO, les moteurs de réponse et les assistants s'en servent pour citer le projet. Format sur llmstxt.org.

## Règles

- Des mots-clés réels, pas du bourrage. Une description doit rester lisible par un humain.
- Coller au vocabulaire que la cible utilise vraiment (un recruteur cherche "React Native", pas "application mobile cross-platform moderne").
- Topics GitHub : entre 5 et 10 par repo, du général au spécifique.
- Pas de promesse marketing, des termes techniques précis.
- Discipline de preuve sur ce qu'on recommande de publier. Tout chiffre (étoiles cumulées, speedup, taille de binaire, downloads) et tout intitulé de poste ou d'employeur proposé dans une bio ou une description doit être sourcé par une commande gh ou par le repo, jamais inventé ni agrégé à la louche. Un agrégat de stars se calcule en additionnant des compteurs vérifiés, et on montre le détail. Un chiffre non vérifiable se marque `[à vérifier]` plutôt que d'être avancé. Ne jamais faire commettre à l'utilisateur le claim gonflé que l'audit reproche ailleurs.

## Sortie

1. **Bio de profil** proposée, avant et après.
2. **Tableau par repo phare** : description actuelle, description proposée, topics à ajouter.
3. **Cross-linking** : les liens manquants à ajouter (GitHub vers LinkedIn, LinkedIn vers GitHub, blog), avec où les mettre.

Tout doit être copiable et collable directement dans GitHub.
