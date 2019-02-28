---
layout: post
title: "Splitting a subdirectory into a new repository"
date: 2019-02-28 15:21:58 -0300
comments: true
categories: git
---

First you must to clone the repository target: <!--more-->

```bash
git clone https://lognull.com/PocProjectX.git
```

Filter the directory that will be send to another repository

```bash
git filter-branch --prune-empty --subdirectory-filter src/main/java/com/develdio/poc/robot/remote/ develop
```

Add the new URL in current repository

```bash
git remote set-url origin https://develdio.com/PocProjectXOnlyRemote.git
```

Update remote refs along with associated objects

```bash
git push origin develop
```

Undo the current changes and returns the original state:

```bash
git reset --hard refs/original/refs/heads/develop
```
