---
id: QZNqH8gcdxV2C4mLRsZF7
title: Git
desc: ''
updated: 1644506977206
created: 1644504069260
---
# Create Git page
Reference page: [GitHub Pages with GitHub Actions](https://wiki.dendron.so/notes/FnK2ws6w1uaS1YzBUY3BR/)

### Setup Git
1. Create the Git repo
2. Locally in VSCode, initialize a workspace
3. On workspace level (where dendron.yml locates)
   ```sh
   git init
   git add .
   git commit -m"Initial commit"
   git branch -M main
   git remote add origin <the github repo address>
   git push -u origin main
   ```

### Setup dendron (for building the webpage with next.js)
1. On workspace level, initialize dendron-cli
   ```sh
   npm init -y 
   npm install @dendronhq/dendron-cli@latest
   echo .next >> .gitignore
   npx dendron publish init # init the publish
   npx dendron publish dev # for checking if the publish works with the md note, browse http://localhost:3000/
   ```
2. Configure your `dendron.yml` - Ctrl+Shift+P \<Dendron: Configure (yaml)>
   ```yaml
   site:
    siteUrl: https://<username>.github.io
    assetsPrefix: /<repo-name>
   ```
    Update like this
   ```yaml
   site:
    siteUrl: https://kevinslin.github.io
    assetsPrefix: /dendron-publish-sample
   ```

## Setup Github Page
1. Create a branch for dendron publishing.
   ```sh
   git checkout -b pages
   git push -u origin HEAD
   ```
2. Go to Github Setting to setup the Pages. Select `pages` branch, and select `/root` folder
3. After building the page by `dendron-cli` (publish export), the html and style files will be stored in this `pages` branch. And Github Page will be built from those in `pages` branch

## Setup the `docs` folder
1. `docs` folder is needed in the git repo for dendron-cli export done in the github action.
2. Copy the `docs` folder from [template.publish.github](https://github.com/dendronhq/template.publish.github/tree/main/docs)
   ```sh
   git remote add -f temp https://github.com/dendronhq/template.publish.github.git
   git sparse-checkout init # enable sparse-checkout
   git sparse-checkout set docs/   # only pull `docs`
   git sparse-checkout list
   git pull temp main # pull the temp remote repo main branch
   git status # check if `docs` is added
   git remote remove temp # remove the temp remote repo
   git sparse-checkout set # remove the target
   git sparse-checkout disable # disable sparse-checkout
   ```
## Setup Github Action
1. Go to `main` branch, at workspace level
   ```sh
   git checkout main
   mkdir -p .github/workflows
   touch .github/workflows/publish.yml
   ```
2. [publish.yml](https://wiki.dendron.so/notes/FnK2ws6w1uaS1YzBUY3BR/#steps---setup-github-actions)
3. Commit and push the change
   ```sh
   git add .
   git commit -m "add workflow"
   git push
   ```