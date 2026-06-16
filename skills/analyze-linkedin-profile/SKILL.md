---
name: analyze-linkedin-profile
description: Use when analyzing a LinkedIn profile that the user provides themselves (screenshots and pasted text), and cross-checking it with their GitHub. Never scrapes LinkedIn. Triggers on "analyse mon LinkedIn", "is my linkedin aligned with my github", "review my linkedin profile".
---

# analyze-linkedin-profile

Analyse un profil LinkedIn pour vérifier qu'il raconte la même histoire que le GitHub et qu'il sert l'objectif de son auteur. La grille de signaux est dans `signals.md` à la racine du plugin.

## Règle de conformité (non négociable)

LinkedIn n'a pas d'API publique propre pour lire un profil, et le scraping viole les conditions d'utilisation et le RGPD. Donc :

- L'utilisateur fournit lui-même son propre profil. C'est son consentement, ses données.
- Jamais de scraping, jamais de fetch d'une URL linkedin.com, jamais de WebFetch sur le profil.
- Aucun stockage des données fournies. Elles servent l'analyse de la session, point.
- Si un jour une API LinkedIn officielle sur scopes limités est disponible, elle pourra être ajoutée. En attendant, entrée manuelle uniquement.

## Entrée (fournie par l'utilisateur)

Au choix ou en combinaison :
- Des screenshots du profil (l'agent lit les images via Read).
- Le texte collé : headline, section "À propos", intitulés et descriptions d'expériences, compétences.
- Optionnel : le `username` GitHub pour le croisement.

Si rien n'est fourni, demander à l'utilisateur de coller son headline et son "À propos", ou d'attacher des captures.

## Analyse

Cohérence interne : le headline, l'"À propos" et les expériences racontent-ils une histoire nette, ou se contredisent-ils.

Alignement avec le GitHub : les deux plateformes pointent-elles vers le même objectif et la même identité. Si le `username` GitHub est fourni, comparer avec la fiche `analyze-github-profile`.

Storytelling enterré : un vrai bon parcours ou un projet fort présent dans la tête de la personne mais invisible ou noyé sur le profil.

Signal open-to-work : présent, cohérent avec l'objectif, ou absent alors que la personne cherche.

Keywords et SEO LinkedIn : le headline contient-il les termes qu'un recruteur cherche. LinkedIn pondère fortement le headline dans sa recherche interne.

Cohérence de DA et naming cross-platform : même nom, même photo, mêmes liens entre LinkedIn, GitHub et un éventuel blog.

## Sortie

1. **Trous identifiés** : ce qui manque ou se contredit, par section.
2. **Alignement GitHub vs LinkedIn** : convergent, divergent, et sur quoi.
3. **Correctifs** : faire remonter le storytelling enterré, réécrire le headline avec les bons keywords, harmoniser les liens et l'identité, activer ou clarifier open-to-work. Chaque correctif est concret et copiable.

Reste factuel. Tu analyses ce que l'utilisateur fournit, tu ne vas rien chercher en ligne sur lui.
