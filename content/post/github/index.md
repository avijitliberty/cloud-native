---
title: Working with Multiple GitHub Accounts from the Same Machine
subtitle: A step by step to link multiple github accounts

# Summary for listings and search engines
summary: A step by step to link multiple github accounts

# Link this post with a project
projects: []

# Date published
date: "2024-01-16T00:00:00Z"

toc: true

# Date updated
lastmod: "2024-01-16T00:00:00Z"

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

# Featured image
# Place an image named `featured.jpg/png` in this page's folder and customize its options here.
image:
  caption:
  focal_point: "Center"
  placement: 1
  preview_only: false

authors:
- admin

tags:
- CICD
- GitHub

categories:

---

<!--more-->

### Overview

When you need to work with different GitHub accounts—like one for personal projects and another for work—things can get tricky. How do you manage them without running into permission issues? 🤔

In this post, we’ll walk through how to configure Git on your machine so you can seamlessly switch between multiple GitHub accounts without any headaches! 🤕

Let suppose you have two github accounts, https://github.com/<github-work> and https://github.com/<github-personal>. Now you want to setup your machine to easily talk to both the github accounts.

Let's dive in!🐬

{{% stepper %}}

<div class="step">

  ## Step 1: Generate SSH Keys🔑 for Both Accounts

</div>
<div class="step">
  
  ## Step 2: Add Your SSH Keys to the SSH Agent

</div>
<div class="step">
  
  ## Step 3: Add SSH Keys to GitHub

</div>
<div class="step">
  
  ## Step 4: Create separate directories (one personal and one for work) each with a separate .gitconfig

  ```bash
  ~
  ├── .gitconfig <-- global
  └── Developer/
    ├── personal/
    │   ├── project_1/
    │   ├── project_2/
    │   ├── project_#/
    │   └── .gitconfig <-- personal
    └── company/
        ├── project_1/
        ├── project_2/
        ├── project_#/
        └── .gitconfig <-- company
  ```
  ```bash
  # ~/Developer/avijitliberty/.gitconfig

  [core]
      sshCommand = ssh -i ~/.ssh/avijitliberty -F /dev/null
  [credential]
      username = avijitliberty
  [user]
      name = avijitliberty
      email = avijit.liberty@gmail.com
  ```

  ```bash
  # ~/Developer/cloud-borne/.gitconfig

  [core]
      sshCommand = ssh -i ~/.ssh/cloud-borne -F /dev/null
  [credential]
      username = cloud-borne
  [user]
      name = cloud-borne
      email = cloud.native.wiki@gmail.com
  ```
</div>
<div class="step">
  
  ## Step 5: Update the Global config

  ```bash
  [includeIf "gitdir/i:D:/Repos/Developer/avijitliberty/"]
      path = D:/Repos/Developer/avijitliberty/.gitconfig

  [includeIf "gitdir/i:D:/Repos/Developer/cloud-borne/"]
      path = D:/Repos/Developer/cloud-borne/.gitconfig
  ```

</div>

{{% /stepper %}}

## Wrapping It Up 🎁

So, it will take the user configuration profile per path, and you can create or clone projects inside each profile path without dealing with manual configurations and avoid using the amend command to fix mistakes.