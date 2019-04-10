---
layout: post
title: Keep Fork Up To Date
date: 2019-04-10
categories: git
---

# Keep Fork Up To Date #

1. Clone your fork:  
    ```
    git clone git@github.com:YOUR-USERNAME/YOUR-FORKED-REPO.git
    ```
2. Add remote from original repository in your forked repository:  
    ```
    cd into/cloned/fork-repo  
    git remote add upstream git://github.com/ORIGINAL-DEV-USERNAME/  REPO-YOU-FORKED-FROM.git
    git fetch upstream
    ```
3. Updating your fork from original repo to keep up with their changes:  
    ```
    git pull upstream master
    ```