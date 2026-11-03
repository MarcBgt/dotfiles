---
name: dev-flow
description: Point d'entrée obligatoire de tout travail de dev (feature, bug, refacto, changement de comportement), dans n'importe quel dépôt. Orchestre les skills Matt Pocock (grilling → spec → tickets → TDD → review) et applique les règles du dépôt déclarées dans docs/dev-flow.md. À invoquer avant toute modification de code ; ne pas l'invoquer pour une simple question ou une lecture.
argument-hint: "Ce qu'on veut construire, corriger ou améliorer"
---

# Dev flow

Orchestrateur des skills Matt Pocock installés dans `~/.claude/skills/`. Le routage vient de `ask-matt` ; ce fichier dit comment les enchaîner. Ce qui est propre à un projet (tests, branches, langue, tracker, livraison) vit dans le dépôt, pas ici.

## 1. Charger les règles du dépôt

Lire `docs/dev-flow.md` à la racine du dépôt courant, au premier passage de la session. Ces **règles du dépôt** priment sur les skills Pocock et sur les valeurs par défaut ci-dessous.

S'il n'existe pas : le signaler en une ligne, déduire ce qu'on peut du dépôt (`package.json`, `Gemfile`, CI, `CLAUDE.md`), et proposer de créer `docs/dev-flow.md` avec le gabarit de la section 5 avant de coder.

Fini quand chaque rubrique du gabarit (tests, review, tracker, commits, livraison, code) a une valeur, lue ou proposée.

## 2. Appeler un skill

- **Invocables via l'outil Skill** : `grilling`, `domain-modeling`, `tdd`, `codebase-design`, `diagnosing-bugs`, `two-axis-review`, `prototype`, `research`, `pr`, `wizard`, `writing-for-agents`.
- **Les autres** (`grill-with-docs`, `to-spec`, `to-tickets`, `implement`, `implement-spec`, `ask-matt`, `triage`, `wayfinder`, `handoff`, `retro`, `improve-codebase-architecture`…) sont `disable-model-invocation` : lire `~/.claude/skills/<nom>/SKILL.md` et le suivre sur place.
- `two-axis-review` est le `code-review` de Pocock renommé ; `/code-review` reste la review native.

## 3. Router

Lire `~/.claude/skills/ask-matt/SKILL.md` au premier passage de la session, puis :

| Situation | Entrée |
|---|---|
| Feature ou changement de comportement | `grill-with-docs` (= `grilling` + `domain-modeling`) |
| Bug | `diagnosing-bugs` ; si un skill du projet couvre le domaine, il porte le diagnostic |
| Chantier flou, multi-sessions | `wayfinder` |
| Santé du code, refacto de fond | `improve-codebase-architecture` |
| Question, lecture, avis | pas de flow : répondre, puis s'arrêter |

Puis le flux principal : une session → `implement` ici ; plusieurs sessions → `to-spec` → `to-tickets` → `implement` par ticket. Annoncer en une ligne l'étape en cours et la suivante.

Même un petit changement passe par un round de grilling (un seul round suffit quand l'arbre est petit). Rien n'est codé avant que l'utilisateur confirme la compréhension partagée.

## 4. Valeurs par défaut

Elles s'appliquent quand `docs/dev-flow.md` ne dit rien. Ne pas lancer `/setup-matt-pocock-skills` : il écrit dans `CLAUDE.md` ; sa configuration est ici et dans `docs/dev-flow.md`.

- **Tracker** : Markdown local. « Publier sur le tracker » = écrire `docs/plans/<feature-slug>/spec.md` et `docs/plans/<feature-slug>/tickets/NN-<slug>.md`. Le label `ready-for-agent` devient la ligne `**Status:**` ; un ticket fini passe en `done`.
- **Glossaire** : `docs/current/GLOSSARY.md`. **ADR** : `docs/current/adr/NNNN-<slug>.md`. Créés à la demande.
- **Commit** : jamais d'office, contrairement à `implement`. Proposer le commit et attendre la demande ; jamais sur la branche principale.
- **Pas de worktree** : on travaille dans le checkout principal ; `implement-spec` en parallèle seulement sur accord explicite.
- **Review** : `two-axis-review` contre la branche principale, avec la spec de `docs/plans/<feature-slug>/` ; sources de standards : `AGENTS.md`, `CLAUDE.md`, `CODING_STANDARDS.md` s'il existe, et la mémoire utilisateur.
- **Messages** : aucune mention de l'IA dans les commits ni les descriptions de merge request.

## 5. Gabarit de `docs/dev-flow.md`

```markdown
# Règles dev-flow de ce dépôt

Lues par le skill dev-flow ; elles priment sur ses valeurs par défaut.

- **Tests** : commande(s), périmètre de la « suite complète », seams préférés.
- **Review** : branche de référence ; contrôles en plus (sécurité…).
- **Tracker** : où vivent specs, tickets, glossaire, ADR ; langue.
- **Commits et branches** : convention, langue, nommage des branches.
- **Livraison** : MR/PR ou merge direct ; déploiement ; qui décide de committer.
- **Code** : conventions que l'outillage n'impose pas (commentaires, style).
- **Données et services** : ce qu'il ne faut jamais toucher.
```

## Clôture

À la fin d'un build qui a dérapé, proposer `retro` en une ligne, sans le lancer d'office.

## Mise à jour des skills Pocock

Installés depuis `mattpocock/skills@a496f55` (dossiers `agents/` exclus, `code-review` renommé en `two-axis-review` et ses références dans `ask-matt`, `implement`, `implement-spec` et `tdd`). Pour mettre à jour : re-cloner, relire le diff, recopier en refaisant ce renommage.
