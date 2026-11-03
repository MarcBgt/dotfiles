This repository is used by [Le Wagon](https://www.lewagon.com) students during our 9-week [coding bootcamp](https://www.lewagon.com).

## Toolset

- [oh-my-zsh](http://ohmyz.sh/)
- [Sublime Text](https://www.sublimetext.com/)
- [git](https://git-scm.com/)
- Ruby via [`rbenv`](https://github.com/rbenv/rbenv)

## Skills Claude Code

`claude/skills/` contient les skills de Claude Code (dev-flow, grilling, two-axis-review, pr, tdd…, issus de [mattpocock/skills](https://github.com/mattpocock/skills) avec des adaptations locales). `install.sh` relie `~/.claude/skills` à ce dossier : un skill créé ou modifié sur le Mac atterrit directement dans le dépôt, à commiter comme le reste.

`claude/skills/synced/` est rempli par l'app Claude (skills synchronisés depuis claude.ai) et n'est pas versionné. Ce dépôt est public : jamais de secret dans un skill, seulement des références à des variables d'environnement.

### Synchronisation sur le VPS de la Code Factory

Le script de déploiement de `code-factory` (ticket 02) installe les mêmes skills pour l'utilisateur qui fait tourner les agents. À chaque déploiement, il :

1. clone ce dépôt dans un dossier de travail, ou le met à jour (`git fetch && git reset --hard origin/master`) ;
2. copie les skills, sans lien symbolique pour que le dépôt du VPS puisse bouger sans casser les agents en cours :

   ```bash
   rsync -a --delete --exclude synced/ dotfiles/claude/skills/ ~/.claude/skills/
   ```

`--delete` retire du VPS un skill supprimé ici. Les skills du VPS ne se modifient pas sur place : tout changement passe par ce dépôt puis un redéploiement.
