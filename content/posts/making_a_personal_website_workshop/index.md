---
# Documentation: https://wowchemy.com/docs/managing-content/

title: "Making a personal website workshop"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2023-04-27T21:54:48+12:00
lastmod: 2023-04-27T21:54:48+12:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

Today we will use [Hugo and Wowchemy](https://wowchemy.com/docs/getting-started/hugo-github-quickstart/) to make a personal website (hosted on github). 

By the end of this workshop, you all should have a website but may need to spend more time customising it to your liking.

{{% callout note %}}
This will require some coding/usage of `git`. Here are some non-git alternatives:
- https://sites.google.com/ (no coding necessary)
- https://jsonresume.org/
{{% /callout %}}


One can now use [netlify](https://wowchemy.com/docs/getting-started/hugo-cms/) and hugo's "CMS" (whatever that stands for) to make a website without using git. However, I have not tried this yet, so I can't help you with it. :sweat_smile:



## Lets get started!

1. Make a github account -- make sure to use a username that you are happy to have as part of your website url (e.g. https://avivajpeyi.github.io/).
2. Download a [hugo template](https://wowchemy.com/templates/) 
   - The ['academic' template](https://github.com/wowchemy/starter-academic.git)
   - The [minimalist template](https://github.com/wowchemy/hugo-minimal-theme.git)
3. Make a repo called `your_username.github.io` (e.g. `avivajpeyi.github.io`) and clone it to your computer (e.g. `git clone git@github.com:your_username/your_username.github.io.git`).
4. Copy the contents of the hugo template into your repo (e.g. `cp -r starter-academic/* your_username.github.io.git/`).
5. Download and install [Hugo](https://wowchemy.com/docs/getting-started/install-hugo-extended/#prerequisites)
6. `cd your_username.github.io` and run `hugo server` to see your website at `localhost:1313` (or whatever port it says).
7. Edit the `config.toml` file to change the title, etc. of your website and edit content from the demo site to make it your own. See [here](https://wowchemy.com/docs/getting-started/quick-start/) for more details. The local build of the website should update automatically.
8. Add the [`.github/workflows/deploy-pages.yml`](https://github.com/avivajpeyi/dev_site/blob/main/.github/workflows/deploy-pages.yml) file to your repo. Take a look at the file and make the necessary changes (the main being where the website will be deployed to). This file will automatically deploy your website to github pages whenever you push your code (the site will be built in the `gh_pages` branch).
9. Push your changes to github + activate 'pages' in your repository settings on the `gh_pages` branch: `https://github.com/your_username/your_username.github.io/settings/pages`
10. Your website should now be live at `https://your_username.github.io/`


Here is a small bash script that you may find helpful:
```bash
git clone https://github.com/wowchemy/starter-academic.git
git clone git@github.com:your_username/your_username.github.io.git
cp -r starter-academic/* your_username.github.io.git/
rm -rf starter-academic
cd your_username.github.io
touch .github/workflows/deploy-pages.yml
wget https://raw.githubusercontent.com/avivajpeyi/dev_site/main/.github/workflows/deploy-pages.yml -O .github/workflows/deploy-pages.yml
git add -A
git commit -m 'init website'
git push
echo "activate 'pages' in https://github.com/your_username/your_username.github.io/settings/pages"
echo "edit .github/workflows/deploy-pages.yml to change the deploy location & push"
```

Now that you have a website, you can start making more changes to it's contents. Over the course of the remaining workshop Amin and I will be available to help you with any issues you run into.


## Generating a publication list with a bib file

Given a bib file with your publications (e.g. `publications.bib`), you can autogenerate the publication list on your website using the `academic` python package:
```bash
pip3 install -U academic
cd <your_website_repo's root dir>
academic import --bibtex <path/to/publications.bib>
```
This will create a `publication` folder with a markdown file for each publication. You can then edit these files to add a summary, etc. See [here](https://wowchemy.com/docs/content/publications/) for more details.
