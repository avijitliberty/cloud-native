---
title: Local
linktitle: Local
type: book
date: "2019-05-05T00:00:00+01:00"
toc: false
tags:
  - GitHub
# Prev/next pager order (if `docs_section_pager` enabled in `params.toml`)
weight: 2
---

Working with Git Locally

<!--more-->

* Show status:

   ```bash
     $ git status
   ```
        
* Add file to index:
   ```sh
     $ git add <filename>
   ```
        
* Add all files to index:

        $ git add --all/-A
* Add all updated files to index:

        $ git add -u
* Remove a file from index:

        $ git reset <filename>
* Commit changes to repo:

        $ git commit -m "Initial commit"
* Fix commit messages:
  > Note: Will change the git history and will create a new commit hash

        $ git commit --amend -m "Correct commit message"

* If you have missed to include a particular file in the previous commit, you could add the file to the index with git add first and then include it to the previous commit.

        $ git commit --amend  
* Shows history of commits in a repo:

        $ git log
* Shows history of commits one line per commit:

        $ git log --oneline
* Shows history of commits with stats like which files have been changed in the commit:

        $ git log --stat  
* Shows history of commits as a graph:

        $ git log --oneline --graph
* Create a global alias for log:

        $ git config --global alias.lga "log --graph --oneline --all --decorate"
        $ git lga
* Search git log:

        $ git log --name-status

{{% callout note %}}

Note: Above command in git-bash shows git history of the branch you are in. The '--name-status' flag lists files that are changed in each commit. In this view you will notice at the bottom of the screen a prompt that start with a colon. To search for a text, do the following: /<word-to-search>

 From this point:
 \- To find next match, press 'n'.<br>
 \- To find previous match, press 'N'.<br>
 \- To exit the log view, press 'q'.<br>

Look up how to search in vi/vim for more details. It should help a lot as it's very similar, if not the same.

{{% /callout %}}

* Shows details of the last commit:

        $ git show HEAD
* Shows difference between two commits:

        $ git diff <commit hash1>..<commit hash2>
* Show difference between latest two commits:

        $ git diff HEAD\~1..HEAD
* Show files that have changes that you have added but not yet committed:

        $ git diff --name-only --cachedf
* List all files in a commit:

        $ git diff-tree --no-commit-id --name-only -r <Commit number>
* Show file names that you've changed in your working set, but not yet added to the index/staging area.

        $ git diff --name-only
* Retrieves last committed file from HEAD:

        $ git checkout <filename>
* Cleans files from working copy:

        $ git clean -f
* Show local branches:

        $ git branch -l
* Show remote branches:

        $ git branch -r
* Create a local branch out of the HEAD:content/cheatsheets/kubernetes/architecture.md

        $ git branch <branch-name>
* Create a local branch based out of a particular commit or tag

        $ git branch <branch-name> <commit-hash>/<tag-name>
* Move your working copy to the local branch:

        $ git checkout <branch-name>
* Create a new local branch and move your working copy to the local branch

        $ git checkout -b <branch-name>
* Rename a local branch:

        $ git branch -m <old-branch-name> <new-branch-name>
* Delete a local branch if it's already merged to another branch.

        $ git branch -d <branch-name>

{{% callout note %}}

Note: This will fail if the branch has not yet merged

{{% /callout %}}

* Shows all the tags for a branch:

         $ git tag
* Create a tag:

        $ git tag <tagname>
* Create a tag with a message:

        $ git tag -a <tagname>
* Create a signed tag with a message

        $ git tag -s <tagname>
* Checkout a tagged version and move the working copy on a branch named after the revision number of tag

        $ git checkout tags/<tag_name>
* Checkout a tagged version and move the working copy on new branch:

        $ git checkout tags/<tag-name> -b <branch-name>
* Recover a directory or file you accidentally deleted in your local repo:
  * List deleted files:

        $ git ls-files --deleted
  * Recover deleted dir:

        $ git checkout -- <dirname>
* Merge branch to current branch:

        $ git merge <branch-name>

{{% callout note %}}

Note: This creates a new commit which you will need to push to remote explicitly.

{{% /callout %}}

* If the merge fails due to conflicts you can resolve them via mergetool:

        $ git mergetool
* After a git merge stops due to merge conflicts you can fix merge issues via mergetool, save the file, and conclude the merge:

        $ git merge --continue
* Revert the last commit:

        $ git revert HEAD

{{% callout note %}}

Note: This creates a new commit that is the opposite of an existing commit. It leaves the files in the same state as if the commit that has been reverted never existed. You will need to push the revert to remote explicitly.

{{% /callout %}}

* Revert a merge commit:

        $ git revert -m <1/2> <bad-commit>

{{% callout note %}}

Note: A merge commit would typically have two parents which could be found using git log. When you revert a merge commit you would want to revert to one of these parents.
  
 \-m 1 --> first parent <br>
 \-m 2 --> second parent
  
As always you will need to push the revert to remote explicitly.

{{% /callout %}}

* Stashing changes not ready for commit yet:

        $ git stash save "Stash Name"

{{% callout note %}}

Note: The latest stash you created is stored in refs/stash; older stashes are found in the reflog of this reference and can be named using the usual reflog syntax (e.g. stash@{0} is the most recently created stash, stash@{1} is the one before it, stash@{2.hours.ago} is also possible). Stashes may also be referenced by specifying just the stash index (e.g. the integer n is equivalent to stash@{n}).

{{% /callout %}}

* Show list of stashes:

        $ git stash list

* Stash Pop: Remove a single stashed state from the stash list and apply it on top of the current working tree state

        $ git stash pop

* Stash Apply: Like ```pop```, but do not remove the state from the stash list.

        $ git stash apply stash@{0}



