---
id: P8YvYr7NnFrLmtucJwTvT
title: Setup
desc: ''
updated: 1646286362705
created: 1644510559529
stub: true
---

## Setup dendron

## about dendron-cli
reference: https://wiki.dendron.so/notes/23a1b942-99af-45c8-8116-4f4bb7dccd21/
## Upgrade dendron-cli
```sh
npm install --save @dendronhq/dendron-cli@latest
```
## Upgrade to latest version of publishing
```sh
cd {workspace_dir}
cd .next
echo "reset workspace..."
git reset --hard
echo "pulling latest version..."
git pull
echo "checking for new dependencies..."
npm install
``