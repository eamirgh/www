---
categories:
- Tooling
date: 2020-07-31T14:51:00.000+00:00
description: How to use make file as a task runner?
keywords:
- development task
- automated task run
- development
layout: blog
tags:
- development
- task-runner
- makefile
- make
title: Using Makefile as task runner

---
In a routine work day life of a programmer there are many tasks that are running repetitively, especially when you deal with multiple programming languages.

How about running a magic command like `Wubba Lubba Dub-Dub dev` to fire a development server without hesitation?

This is how I do it  in my laravel projects:

```Makefile
dev:

    @php artisan serve
```

Or This is how I do it in my javascript projects:

```Makefile
dev:

    @npm run dev
```

and both of them run with a singe command: `make dev`