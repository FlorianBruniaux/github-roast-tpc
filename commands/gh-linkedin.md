---
description: Analyse un profil LinkedIn fourni par l'utilisateur et le croise avec son GitHub (zéro scraping)
argument-hint: [username GitHub pour le croisement]
allowed-tools: Read, Bash, Grep, Task
---

Analyse le profil LinkedIn de l'utilisateur en déléguant à l'agent `linkedin-analyst`.

L'utilisateur doit fournir lui-même son profil : screenshots à attacher, ou texte collé (headline, "À propos", expériences). Rien n'est récupéré en ligne, jamais de scraping LinkedIn.

L'agent doit :
1. Lire les screenshots et le texte fournis.
2. Si `$1` (username GitHub) est donné, croiser avec le profil GitHub via l'API.
3. Appliquer le skill `analyze-linkedin-profile`.
4. Rendre les trous par section, le verdict d'alignement GitHub vs LinkedIn, et des correctifs copiables.

Si l'utilisateur n'a rien fourni, lui demander de coller son headline et son "À propos", ou d'attacher des captures.
