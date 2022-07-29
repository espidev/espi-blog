---
title: "Setting up a personal website with Hugo and Cloudflare Pages"
date: 2022-07-28T19:20:29-04:00
draft: false
tags: [tutorials]
---

So recently I decided that I really needed an easy workflow for my website, since I just wanted to write blog posts without needing to worry about site deployments. I ended up using Hugo and Cloudflare Pages to do so, which automatically deploys my website as soon as I push to my Git repository.

First, some background...

**[Hugo](https://gohugo.io/)** is a site generator, that can create static sites simply from a folder of files, with support for [markdown](https://www.markdownguide.org/). This makes the website very portable without a database to worry about, and so it can easily be hosted from a Git repository. Markdown support also makes blogging very easy!

[**Cloudflare Pages**](https://pages.cloudflare.com/) is a service to quickly deploy websites on Cloudflare's infrastructure, and is free (with some [restrictions](https://pages.cloudflare.com/)). Deployment is completely automated, and can be triggered from new commits in a Git repository. 

## Getting started

First, create a Git repository on either **GitLab** or **GitHub** to host the source files for your website. 

Then, clone the repository to your machine:
```bash
git clone https://gitlab.com/your-username/your-repo.git
cd your-repo
```

Now, we need to install **Hugo** locally in order to have access to the CLI tool. See [this page](https://gohugo.io/getting-started/installing/) on instructions for how to install.

With the CLI tool installed, we now create the website from the root of the repo:
```bash
hugo new site new-site-name
mv new-site-name/* ./
rm -rf new-site-name
```

Follow the Hugo [quickstart](https://gohugo.io/getting-started/quick-start/) in order to finish the initial website setup.

Now, push your changes to the remote repository:
```bash
git add .
git commit -m "add initial site files"
git push
```

## Setting up the workflow
Now, we want to setup the **Cloudflare Pages** workflow.

Login to your Cloudflare account (or register, if you have not set one up already), and navigate to the **Pages** category on the left sidebar.

Click the **Create a project->Connect to Git** button, and enter the details for your created website repository. Then proceed to the next page.

On the build setup page, you can configure which branch you want Cloudflare to watch for updates, where new commits will trigger a deployment.

In the **Build settings** category, select the **Hugo** Framework preset, and use `hugo` as the build command.

Be sure to also set the `HUGO_VERSION` environment variable to the latest version (ex. `0.101.0`, because the default version that Cloudflare uses is quite old. You can view the latest releases [here](https://github.com/gohugoio/hugo/releases).

Then, click **Save and Deploy**. The website should now be building, and Cloudflare will generate a link (ex. ) to a public version of the site for every commit.

If you have a domain already configured on Cloudflare, you can go to the **Custom Domains** tab for the page, and add a permanent domain that points to the latest version of the site!

