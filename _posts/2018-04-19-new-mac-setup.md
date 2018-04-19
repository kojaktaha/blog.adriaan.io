---
title: What I do when I setup my new Mac
---

## Install

- [Amphetamine](https://itunes.apple.com/nl/app/amphetamine/id937984704?mt=12) to keep your mac awake
- [CopyClip](https://itunes.apple.com/is/app/copyclip-clipboard-history-manager/id595191960?mt=12) for managing your clipboard
- [Chrome](https://www.google.com/chrome) for the dev tools
- [Atom](https://atom.io) for editing text
- [Brew](https://brew.sh) for packages
- [nvm](https://github.com/creationix/nvm) for Node versions

## Setup git

```bash
brew install git-flow
git config --global user.name "Adriaan van Rossum"
git config --global user.email whatever@example.com
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
git config --global branch.autosetuprebase always
```
