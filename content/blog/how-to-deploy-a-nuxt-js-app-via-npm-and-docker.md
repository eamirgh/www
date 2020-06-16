---
layout: blog
title: How to deploy a NUXT.JS app via NPM and docker?
description: A tutorial to dockerize a nuxtjs app via npm and publish it
date: 2020-06-15T07:50:42.726+00:00
tags:
- nuxt.js
- docker
- npm
keywords:
- nuxt.js
- docker
- npm
- deploy
- devops

---
Since I saw [Nuxt.js](https://nuxtjs.org/), I was fascinated by the performance and speed and I always wanted to use it in a project. These days [Docker](https://www.docker.com/) has a mandatory to deploy and ship all kind of apps. So I decided to make a simple template for myself and deploy it via docker, since this post's title is about dockerizing, I skip the Nuxt part.

So let's begin!

**Step 1:**

At first we add a empty `Dockerfile` to project's **root** folder.

```bash 
$ touch Dockerfile
```

**Step 2:**

Open the Dockerfile with your preffered editor and paste the code below:

```docker
FROM node:14.4.0-alpine3.12 AS BASE

# Create app directory
RUN mkdir -p /usr/src/app/.nuxt
WORKDIR /usr/src/app

# Install app dependencies
COPY package.json /usr/src/app/
COPY package-lock.json /usr/src/app/

# BUILD STAGE
FROM BASE AS BUILD

# Install all dependencies
RUN npm i

# Set environment variables
ENV NODE_ENV production
ENV NUXT_HOST 0.0.0.0
ENV NUXT_PORT 3000

# Bundle app source
COPY . /usr/src/app

# Build command
RUN npm run build

# PRODUCTION STAGE
FROM BASE AS PROD
COPY --from=BUILD /usr/src/app/.nuxt/ /usr/src/app/.nuxt/

# Set environment variables again to ensure
ENV NODE_ENV production
ENV NUXT_HOST 0.0.0.0
ENV NUXT_PORT 3000

# Bundle app source
COPY . /usr/src/app

# Installing needed packages only and clearing cache
RUN npm install --only=production && \
    npm cache clean --force

EXPOSE 3000
CMD [ "npm", "start" ]
```

**Step 3:**

Build the image with a single command but before that ensure that there is no `node_modules` or `.nuxt` forlder, because they will be copied to the docker image if they exist and that will increase the size of the docker image.

```bash
$ docker image build -t my-handle/appname:tag .
```

After a couple of minutes (depending on your device performance and internet speed) your minimalistic docker image is ready.