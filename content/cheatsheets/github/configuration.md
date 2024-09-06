---
title: Configuration
linktitle: Configuration
type: book
date: "2019-05-05T00:00:00+01:00"
toc: true
tags:
  - GitHub

# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 1
---

Git Configuration

<!--more-->

## Connecting to GitHub with SSH

You can connect to GitHub using the Secure Shell Protocol (SSH), which provides a secure channel over an unsecured network.

1. Checking for existing SSH keys
    ```
    # Lists the files in your .ssh directory, if they exist
    ls -al ~/.ssh
    ```

2. Generating a new SSH key
    ```
    ## -C stands for comment to help identify your ssh key
    ## -f stands for the file name where your ssh key get saved
    ssh-keygen -t ed25519 -C "your_email@example.com" -f "github-username"
    ```
    When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.

{{% callout note %}}

Note: If you are using a legacy system that doesn't support the ed25519 algorithm, use:
```
$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
{{% /callout %}}

    ```
    > Enter a file in which to save the key (/c/Users/you/.ssh/id_algorithm):[Press enter]
    At the prompt, type a secure passphrase. For more information, see "Working with SSH key passphrases."
    > Enter passphrase (empty for no passphrase): [Type a passphrase]
    > Enter same passphrase again: [Type passphrase again]
    ```

3. Adding SSH key to the ```ssh-agent```

    Ensure the ssh-agent is running. Start it manually:
      ```
      # start the ssh-agent in the background
      eval "$(ssh-agent -s)"
      ```

    Add your SSH private key to the ssh-agent. If you created your key with a different name, or if you are adding an existing key that has a different name, replace id_ed25519 in the command with the name of your private key file.

      ```
      ssh-add ~/.ssh/id_ed25519
      ```

4. Add the SSH key to your account on GitHub.

    * Copy the SSH public key to your clipboard.
      ```
      # Copies the contents of the id_ed25519.pub file to your clipboard
      clip < ~/.ssh/id_ed25519.pub
      ```

    * In the upper-right corner of any page, click your profile photo, then click ```Settings```
      {{< figure src="images/uploads/userbar-account-settings.png" width="250" height="400">}}

    * In the **Access** section of the sidebar, click ```SSH and GPG keys```.
      {{< figure src="images/uploads/settings-sidebar-ssh-keys.png" width="250" height="250">}}

    * Click ```New SSH key``` or ```Add SSH key```.
      {{< figure src="images/uploads/ssh-add-ssh-key.png" width="500" height="500">}}

    * In the **Title** field, add a descriptive label for the new key.
      For example, if you're using a personal laptop, call this key "Personal Laptop".💡

    * Paste your key into the **Key** field.
      {{< figure src="images/uploads/ssh-key-paste.png" width="500" height="500">}}

    * Click ```Add SSH key```.

{{% callout note %}}

- While cloning/pulling/pushing code from remote use your username and generated token when prompted for username and password respectively.
- On Windows this stores your credentials in the Windows credential store which has a Control Panel interface where you can delete or edit your stored credentials. With this store, your details are secured by your Windows login and can persist over multiple sessions.

{{% /callout %}}

## Initialize a new local repository

  ```bash
  git init
  ```

  {{% callout note %}}
  It will create the hidden .git folder which is used by git to manage the repository in your working directory
  {{% /callout %}}

## Developer Configuration

* If you would not like to provide credentials for every "repo" operations you could cache them. The Git credential cache runs a daemon process which caches your credentials in memory and hands them out on demand.

  ```bash
  git config --global credential.helper manager
  ```

* Configure the author name and email address to be used with your commits:

  ```bash
  git config --global user.name "LAST_NAME.FIRST_NAME"
  git config --global user.email "WORK_EMAIL"
  ```
* Normalize your line endings:

  ```bash
  git config --global core.autocrlf false
  ```
* Colorized output:

  ```bash
  git config --global color.ui auto
  ```
* Setup KDiff as the merge tool:

  ```bash
  git config --global --add merge.tool kdiff3
  git config --global --add mergetool.kdiff3.path "C:/Program Files/KDiff3/kdiff3.exe"
  ```
* Change the default core editor from vim to VSCode:
  ```bash
  git config --global core.editor 'code --wait'
  ```
* Setup proxies to work with Git:
    * Option1: Git Proxy without Cntlm: (Not secure as our credentials are being sent "over the wire")
      ```bash
      git config --global http.proxy http://MyUserID:MyPassword@www-proxy.company.com:80
      git config --global https.proxy http://MyUserID:MyPassword@www-proxy.company.com:80
      ```
    * Option2: Git Proxy w/ Cntlm: (Safe since our passwords are encrypted. Generic enough to be used with a host of tools like NPM, Git, AWS, Eclipse, Docker etc.)
      ```bash
      git config --global http.proxy http://localhost:3128
      git config --global https.proxy http://localhost:3128
      ```
* Display current settings

  ```bash
  git config --list
  ```

## Further Read

More info here: <http://git-scm.com/docs/git-config>
