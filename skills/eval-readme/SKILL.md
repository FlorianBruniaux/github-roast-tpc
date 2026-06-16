---
name: eval-readme
description: Use when evaluating the quality of a GitHub README, judging whether it reads as human-written or AI-generated, and producing concrete fixes. Triggers on requests like "eval this README", "is this readme AI-generated", "review my repo readme".
---

# eval-readme

Évalue un README GitHub à travers un prisme simple : sert-il son objectif de communication vers un humain pressé ? Référence des signaux dans `signals.md` à la racine du plugin.

## Entrée

Soit un `owner/repo` (l'agent récupère le README via l'API GitHub), soit le texte du README collé directement.

## Procédure

1. Récupérer le contenu. Pour un repo : `gh api repos/{owner}/{repo}/readme --jq .content | base64 -d`, ou le README brut via raw.githubusercontent.com.
2. Passer les trois grilles ci-dessous.
3. Trancher un verdict et lister des correctifs applicables tout de suite.

### Grille 1 : signaux humains (présence = bon)

Screenshots, diagrammes, records de terminal (asciinema, SVG), vidéos, GIF de démo. Structure scannable : titres clairs, exemples de commandes, sections courtes. Un humain sait qu'on lit un README en diagonale et favorise l'image sur le pavé.

### Grille 2 : marqueurs IA non assumée (présence = drapeau)

Em-dash en pagaille, paragraphes de longueur uniforme, tournures type LLM, ouvertures stéréotypées ("In today's...", "It's worth noting"), buzzwords vides, sur-structuration et emojis trop léchés. Réutiliser le skill anti-AI markers s'il est disponible.

Nuance : un fichier `llms.txt` ou un README explicitement destiné à être ingéré par un LLM échappe à cette grille. Là le contenu généré est légitime et assumé.

### Grille 3 : essentiels

Licence présente. Le quoi et le pourquoi en haut. Installation. Usage avec exemples. Mention ou badge "en pause" si la maintenance est arrêtée, pour éviter l'impression d'abandon sur un repo vitrine. Docs LLM : présence d'un `llms.txt` ou `llms-full.txt` à la racine, signal de discoverability moderne (voir `signals.md`). Absent sur un projet outil, c'est un correctif rapide à proposer.

Cohérence des chiffres. Un même chiffre répété (nombre de fonctionnalités, étoiles dans une table comparative, version) doit être identique partout : header, tableaux, roadmap, description GitHub. Si les sources divergent, ne pas élire une valeur comme vraie : signaler la divergence, dire où trancher (le code, le compteur live), puis aligner. Voir la discipline de preuve dans `signals.md`.

## Sortie

Rendre dans cet ordre :

1. **Verdict** : lecture humaine, lecture IA non assumée, ou IA assumée légitime.
2. **Note /5** sur la qualité de communication (pas la qualité du code).
3. **Tableau de signaux** : chaque grille en vert, orange ou rouge avec une raison courte.
4. **Correctifs** : actions concrètes et rapides. Exemple : "nettoyer les 14 em-dash", "ajouter un GIF de démo en haut", "mettre une ligne de licence", "ajouter un record terminal à la place du pavé d'install".

Rester factuel, citer les lignes ou passages précis. Ne jamais juger la qualité du code, seulement la communication.
