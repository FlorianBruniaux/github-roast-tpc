# Roadmap github-review (Phase 2 à 6)

Suite du scaffold Phase 0-1. Couvre l'analyse de profil GitHub complète, l'éval LinkedIn, et l'élargissement des signaux. Chaque phase liste skills, agents, entrées, sorties.

## Principe rappel

On orchestre, on ne réécrit pas. StarMapper fournit organic score / géo / gros poissons. Le skill anti-AI fournit les marqueurs. `signals.md` reste la source unique de la grille.

---

## Phase 2 : analyze-github-profile (le coeur)

Skill `analyze-github-profile` + agent `github-profile-analyst` (sonnet).

Entrée : un `username` GitHub.

Récupère via `gh` : profil (README de profil, bio, liens), liste des repos non-fork, pinned repos, contribution timeline, langues. Pour chaque repo phare : commits (langue, convention, intention), issues/PR (vivant vs stand-by), releases, licence, présence README.

Analyse :
- Objectif détecté (gros projet / lever des fonds / se faire recruter / business), avec justification.
- Cohérence DA et naming entre repos.
- Pinned : les siens vs contribution à 1 commit sur un gros repo.
- Timeline : weekend vs pro, spike assistants IA.
- README de profil type CV présent ou non.

Sortie : fiche profil structurée (objectif principal, signaux par catégorie, repos phares avec leur rôle). Réutilisable par Phase 3.

---

## Phase 3 : score-profile (synthèse)

Skill `score-profile`. Pas d'agent dédié, c'est de la synthèse (envisager opus pour l'arbitrage multi-signaux).

Entrée : sorties Phase 1 (README) + Phase 2 (profil) + signaux StarMapper.

Sortie en trois couches :
1. Note /5 globale, orientée objectif (la note d'un repo "se faire recruter" ne se juge pas comme un "lever des fonds").
2. Tableau de signaux vert/orange/rouge par catégorie.
3. Prompts correctifs prêts à appliquer (nettoyer em-dash, ajouter descriptions+keywords, faire remonter les bons pins, badge "en pause", aligner LinkedIn et GitHub).

---

## Phase 4 : keywords + LinkedIn

### suggest-keywords

Skill `suggest-keywords` + agent `keyword-strategist` (sonnet). Génère des descriptions de repo avec bons keywords, optimise la findability (un recruteur doit te trouver via Google et Perplexity, c'est du SEO concret), et propose le cross-platform linking LinkedIn vers GitHub vers Medium.

### analyze-linkedin-profile

Skill `analyze-linkedin-profile` + agent `linkedin-analyst` (sonnet). Pas d'API officielle propre, donc entrée multimodale fournie par l'utilisateur lui-même (son propre profil, son consentement, zéro scraping, zéro stockage) :

- Screenshots du profil (Claude lit les images).
- Copier-coller du texte (headline, about, expériences).
- Exploration API à creuser plus tard si une voie conforme existe (LinkedIn API officielle sur scopes limités), jamais de scraping.

Analyse : cohérence headline / about / expériences, alignement avec le GitHub (les deux racontent-ils la même histoire), storytelling enterré (le cas Michael, super histoire invisible sur le profil), signal open-to-work, keywords et SEO LinkedIn, cohérence de DA et naming cross-platform.

Sortie : trous identifiés, alignement GitHub/LinkedIn, correctifs (remonter le storytelling, harmoniser les liens, ajouter les keywords manquants).

---

## Phase 5 : routing (la valeur TPC)

Skill `route-next-steps`. Deux séquences :
- "je cherche du taf" : profil → score → correctifs → offres TPC (lien).
- "je diffuse mon projet" : profil → draft de post Reddit ou LinkedIn généré (le post Reddit qui a fait décoller RTK comme modèle).

---

## Phase 6 : packaging

Validation privée avec Gabriel (fusionner les deux grilles dans `signals.md`), puis ouverture communautaire. Contacter Ivan de Murard qui travaille sur un outil similaire pour mutualiser.

---

## Analyse de plus de choses (modules additionnels)

Au-delà du marketing/communication, des modules orientés substance technique. Chacun = un skill + éventuellement un agent.

| Module | Ce qu'il regarde | Source |
|--------|------------------|--------|
| tech-depth | tests présents, CI/CD, structure du code, couverture | gh API + clone léger |
| commit-quality | granularité, messages, conventional commits, ratio IA co-signé | git log |
| security-scan | secrets exposés, dépendances vulnérables, Dependabot actif | gh API + audit |
| dependency-health | dépendances à jour, lockfile, taille du bundle | manifestes |
| activity-maintenance | dernier commit, vélocité, issues ouvertes vs fermées, abandon | gh API |
| community-health | contributeurs, discussions, réactivité aux issues/PR | gh API |
| social-presence | findability cross-platform, Medium, Hacker News, Reddit, SEO | recherche web |
| traffic (futur) | vues et clones via GitHub Insights (demande Ivan de Murard) | gh traffic API |

Note : la détection des marqueurs IA dans le code (pas juste le README) est un module à part, plus délicat, à traiter avec prudence (un faux positif accuse à tort).

## Ordre de priorité conseillé

1. Phase 2 (coeur, débloque tout le reste).
2. Phase 3 (rend le tout actionnable avec une note).
3. Module activity-maintenance et community-health (signaux recruteur les plus cités par Gabriel).
4. Phase 4 LinkedIn + keywords (la demande explicite, multimodale).
5. Phase 5 routing (la valeur business TPC).
6. Modules tech-depth / security pour le prochain épisode orienté code.
