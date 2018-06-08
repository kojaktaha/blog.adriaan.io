---
title: What I do when I setup my new Mac
---

When I install my new Mac I do a few things to set it to my needs. Maybe you like those apps and settings and can use it as well. If I need a few essentials, hit my up [on Twitter](https://twitter.com/harianus).

## Install

- [Amphetamine](https://itunes.apple.com/nl/app/amphetamine/id937984704?mt=12) to keep your mac awake
- [CopyClip](https://itunes.apple.com/is/app/copyclip-clipboard-history-manager/id595191960?mt=12) for managing your clipboard
- [Chrome](https://www.google.com/chrome) for the dev tools (I tried Safari, but no)
- [Atom](https://atom.io) for editing text
- [Brew](https://brew.sh) for packages
- [nvm](https://github.com/creationix/nvm) for Node versions

## Setup git

```bash
# personal settings
git config --global user.name "Adriaan van Rossum"
git config --global user.email whatever@example.com

# if you want to use git flow
brew install git-flow

# some shortcuts, I don't like typing
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status

# theses settings change the default behaviour, know what it means before using it
git config --global pull.rebase true
git config --global push.default current

# if you have git < 1.7.9 (check with git --version)
git config --global branch.autosetuprebase always
```
