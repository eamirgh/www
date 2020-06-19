+++
date = 2020-06-18T19:30:00Z
description = "A tutorial for creating a website with Hugo, Netlify and Forestry using github or gitlab."
draft = true
keywords = ["static cms", "netlify", "hugo site generator", "static website", "free static website"]
layout = "blog"
tags = ["Forestry", "Netlify", "Hugo"]
title = "Creating a website with Hugo, Netlify and Forestry."

+++
Having a personal website is a must for almost everyone nowadays especially if you are a software developer or related into computer. In most cases having a simple blog/website will cost money and time, I've developed five or more personal websites for myself but always it came up with a problem and they did not last more than a couple of months, so I decided to share my super easy and fast experience with you.

First of all, let me tell you that we will use [Hugo](https://gohugo.io/ "Hugo"), an open-source and super fast static site generator written in Golang, for converting structured markdown files into styled and themed HTML pages. Let's pick a theme from official themes in [Hugo themes](https://themes.gohugo.io/ "themes"), don't be sensitive you can customize all of it later, I pick a minimal theme named [Vanilla](https://themes.gohugo.io/vanilla-bootstrap-hugo-theme/ "Vanilla bootstrap theme").

**Step 1:**

Make a new directory and `git init` inside it. Then I add the theme as a git sub-module:

    git submodule add https://github.com/zwbetz-gh/vanilla-bootstrap-hugo-theme.git themes/vanilla-bootstrap-hugo-theme

**Step 2:**

Grab the `config` file with any of these extentions:  `.yaml` `.yml` `.json` `.toml`  from your chosen theme's `exampleSite` directory and paste it to root directory of project also similarly copy the `content` folder into root project, take a look at the config file and change them as you wish.


You can also create a `netlify.toml` file with following content to have a build script:

    [build]
    publish = "public"
    command = "hugo --gc --minify"
    
    [context.production.environment]
    HUGO_VERSION = "0.72.0"
    HUGO_ENV = "production"
    HUGO_ENABLEGITINFO = "true"
    
    [context.split1]
    command = "hugo --gc --minify --enableGitInfo"
    
    [context.split1.environment]
    HUGO_VERSION = "0.72.0"
    HUGO_ENV = "production"
    
    [context.deploy-preview]
    command = "hugo --gc --minify --buildFuture -b $DEPLOY_PRIME_URL"
    
    [context.deploy-preview.environment]
    HUGO_VERSION = "0.72.0"
    
    [context.branch-deploy]
    command = "hugo --gc --minify -b $DEPLOY_PRIME_URL"
    
    [context.branch-deploy.environment]
    HUGO_VERSION = "0.72.0"
    
    [context.next.environment]
    HUGO_ENABLEGITINFO = "true"


Create a new repo on gitlab or github and push the project into it.


**Step 3:**

Ez huh? Let's deploy it into awesome [Netlify](https://app.netlify.com "Netlify PaaS").

Go to Netlify website and login via github or gitlab, after login, click on new site from Git:

![](/uploads/netlify-new-site-from-git.jpg)

Then select the git server of your site repository for a Continuous Deployment:

![](/uploads/netlify-create-new-site.jpg)

Then allow giving permission to Netlify and select the repo you had created:

![](/uploads/netlify-pick-repo.jpg)

In the build options enter the shown config:

![](/uploads/netlify-build-settings.jpg)

