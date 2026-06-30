# Changelog

## [Unreleased]

### Added
- `scoring-card.html`: carte de résultat 1080×1080 px partageable, avec ring SVG coloré par seuil (vert ≥75, ambre 45-74, rouge <45), avatar GitHub, handle, et bouton "Télécharger l'image" (PNG via dom-to-image-more, SRI inclus). URL params `?handle=X&score=Y` pour l'ouverture directe; valeurs fallback dans le JS pour les copies générées par le skill.
- `signals.md`: section "Pondération /100 par objectif" avec 4 tableaux chiffrés (se faire recruter, lever des fonds, faire un gros projet, faire du business) et règle de renormalisation quand StarMapper est indisponible.
- `score-profile/SKILL.md`: 4e couche de sortie qui génère `scoring-card-<handle>.html` localement avec les valeurs injectées et affiche le chemin prêt à ouvrir.

### Changed
- Score migré de /5 vers /100 natif dans `score-profile`, `gh-score`, `README`, et `ROADMAP`. La note finale = Σ(score_catégorie × poids) arrondie à l'entier; le calcul détaillé reste affiché.
- `score-profile/SKILL.md`: la procédure référence désormais `signals.md` pour les poids (source unique), avec règle StarMapper absent explicite.
- `README.md`: méta-prompt full audit aligné sur /100 et mention de la carte générée en sortie de `score-profile`.
