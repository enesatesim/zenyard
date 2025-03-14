---
title: Example Title
draft: false
tags:
  - example-tag
---
The rest of your content lives here. You can use **Markdown** here :)
In this guide, I'm explaining how to publish your obsidian notes on GitHub Pages using [Quartz](https://quartz.jzhao.xyz/).

Install [Visual Studio Code](https://code.visualstudio.com/download)
Install [Git](https://git-scm.com/downloads)
Install [Node.js](https://nodejs.org)
Create a folder and open it in VSCode
	File > Open Folder
Open terminal
	View > Terminal
Check if you have at least Node v20 and npm v9.3.1 right.
```
node -v
npm -v
```

Clone the Quartz repo using terminal:
```
git clone https://github.com/jackyzha0/quartz.git
```
A folder named "quartz" should appear in your parent folder

Now you can either open the folder named "quartz" as a working directory in VSCode or you can paste the following into the terminal. The latter has to be done every time the terminal is closed, so it is advised to follow the former path for a more convenient in long term solution.
```
cd quartz
```

Paste the following and wait until "added x packages, and audited y packages" shows up. This doesn't happen instantly but shouldn't take much more than about a minute.
```
npm i
```

Paste the following:
```
npx quartz create
```
Choose using arrow keys and enter.
Empty quartz > Treat links as shortest path
"You're all set!" should come up.


https://quartz.jzhao.xyz/setting-up-your-GitHub-repository

Open up a [new GitHub repo](https://github.com/new). Name it whatever you want. You can change this later and surprisingly it won't break anything. However, you absolutely must not "Add a README file" or "Choose a licence" while creating it.

Copy this newly created repo's link **with .git extension** at the end. It should look something like this:
https://github.com/enesatesim/zenyard.git

Edit the following code with your repo's link in place of REMOTE-URL and paste into terminal:
```
git remote set-url origin REMOTE-URL
```
It should look something like this:
```
git remote set-url origin https://github.com/enesatesim/zenyard.git
```
Then paste this to sync your local quartz repo to your newly created GitHub repo:
```
npx quartz sync --no-pull
```
From now on every time you want to sync changes you can paste it in this salt form:
```
npx quartz sync
```

Now if you check your GitHub repo, all the files should be transferred there.

To create deploy.yml file in the appropriate folder paste the code into terminal. It can be done manually as well, but why note faster?
```
code .github/workflows/deploy.yml
```
It should create and open the empty file. Then paste the following into it and **save it**.
```
name: Deploy Quartz site to GitHub Pages
 
on:
  push:
    branches:
      - v4
 
permissions:
  contents: read
  pages: write
  id-token: write
 
concurrency:
  group: "pages"
  cancel-in-progress: false
 
jobs:
  build:
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
        with:
          fetch-depth: 0 # Fetch all history for git info
      - uses: actions/setup-node@v4
        with:
          node-version: 22
      - name: Install Dependencies
        run: npm ci
      - name: Build Quartz
        run: npx quartz build
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: public
 
  deploy:
    needs: build
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

Now go to your repo's settings and head pages. Select GitHub actions.

now sync again using the good old command
```
npx quartz sync
```

Your site is now deployed. It should be here:
`<github-username>.github.io/<repository-name>` or in my case here:
enesatesim.github.io/zenyard

If you get an error about not being allowed to deploy to `github-pages` due to environment protection rules, make sure you remove any existing GitHub pages environments.

You can do this by going to your Settings page on your GitHub fork and going to the Environments tab and pressing the trash icon. The GitHub action will recreate the environment for you correctly the next time you sync your Quartz.

Settins > Enviroments > Delete

This should finish running (yellow), successfully execute (green) if everything goes right
[Quartz sync: Mar 14, 2025, 5:08 PM](https://github.com/enesatesim/zenyard/actions/runs/13858369442)Deploy Quartz site to GitHub Pages #2: Commit [dfc0930](https://github.com/enesatesim/zenyard/commit/dfc0930551b7e3e1241de3da9d1d1392b984e361) pushed by [enesatesim](https://github.com/enesatesim)
