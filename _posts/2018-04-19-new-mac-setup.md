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

Add this to the settings file (`~/.gitconfig`):

```
[alias]
	co = checkout
	br = branch
	ci = commit
	st = status
[user]
	name = Adriaan van Rossum
	email = ...
[branch]
	autosetuprebase = always
[pull]
	rebase = true
[push]
	default = current
[color]
	branch = auto
	diff = auto
	status = auto
[color "branch"]
	current = yellow reverse
	local = yellow
	remote = green
[color "diff"]
	meta = yellow bold
	frag = magenta bold
	old = red bold
	new = green bold
[color "status"]
	added = yellow
	changed = green
	untracked = cyan
```

Maybe change your name :)

Create a file `~/.gitexcludes` with `.DS_Store` in it:

```
echo '.DS_Store' >> ~/.gitexcludes
```
