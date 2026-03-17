---
title: "Kinji Leslie - PSTAT"
date: 2026-03-17T12:58:00-07:00
draft: false
---

# Welcome to my personal PSTAT web page

This is my first post on my new **Hugo** site! We're hosted on Github pages. Cool. 

## How did this get setup?

### Github setup
On Github, setup your Github pages and add your domain, you'll have to verify with a TXT record and CNAME. Settings -> Pages: Custom Domain; Add. Publish your site above and remember to come back here to switch from Deploy from branch to Github actions if you're doing something cool like using Hugo. 

`TXT` and `CNAME` example, in this case I'm editing `db.pstat` in bind for the `pstat.ucsb.edu` zone: 

`
_gh-kinjiucsb-o.kinjileslie 60m TXT "0ff442a428"
kinjileslie 60m    CNAME      kinjleslie.github.io.
`

Github will verify DNS. I created a new organization and added the `kinjilesle.pstat.ucsb.edu` domain there in Github. This is completes the verification steps on Github's side. You can see that reflected in the full name ("`kinjiucsb-o`"). Example verify with `dig`:

`
dig +short TXT _gh-kinjiucsb-o.kinjileslie.pstat.ucsb.edu.
`

### Hugo setup

On my machine: `dnf in hugo` and then `hugo new site web` and I'm calling it `web` which maps well t my project: 
`git clone git@github.com:kinjileslie/web.git`

Now we have a local copy of the repo setup. 
`
 kinji  ~  git-kinjileslie  hugo new site web --force                                                             1  ST 2   main
Congratulations! Your new Hugo site was created in /home/kinji/git-kinjileslie/web.

Just a few more steps...

1. Change the current directory to /home/kinji/git-kinjileslie/web.
2. Create or install a theme:
   - Create a new theme with the command "hugo new theme <THEMENAME>"
   - Or, install a theme from https://themes.gohugo.io/
3. Edit hugo.toml, setting the "theme" property to the theme name.
4. Create new content with the command "hugo new content <SECTIONNAME>/<FILENAME>.<FORMAT>".
5. Start the embedded web server with the command "hugo server --buildDrafts".

See documentation at https://gohugo.io/.
`

Git add and push!

### Github action setup
Setup hugo Github actions. New file:

`
.github/workflows/hugo.yaml
`

New file contents:

`
name: Deploy Hugo site to Page

on:
  push:
    branches: ["main"]

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          hugo-version: 'latest'
          extended: true
      - name: Build
        run: hugo --minify
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4

`

Now for actions: **Settings** -> **Pages** -> **Build and deployment** -> **Source**: Change from "branch" to Github Actions. 

Make a new file in `context/_index.md` as your default page and you're off!j


## cool tips:
* **Data:** Learn to use git and Github together. This is the _only_ way to preserve your data! See git lfs for lots of data.
* **Identity:** Use ssh keys with good passphrases! The public key is your padlock, install it where you wan to use your private key to unlock magic!
* **Networks:** "Bookmark" your remote servers and stuff by editing your ~/.ssh/config!
* **Resilience:** Be immortal! Be able to create your work environment from cloud-storage at any time! Git, keys, local configs! Re-instantiate yourself from nothingness to attain immortality. 
* **Agility:** Use tmux to preserve running jobs from the flaky campus Wi-Fi! Use tmux to run many things at once and become omniscient and omnipotent! 
