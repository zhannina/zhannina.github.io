---
layout: post
title: Creating a new git repo from the existing 
---

Recently I needed to copy my existing project to a new git repository.
After trying some solutions suggested on StackOverflow this [one](https://stackoverflow.com/questions/9527999/how-do-i-create-a-new-github-repo-from-a-branch-in-an-existing-repo) worked for me. 


The steps are:
<li> Create a new repository on github </li>
<li> Go to the terminal and navigate to the local directory with the existing project </li>
<li> Now in the terminal type: git push https://github.com/accountname/new-repo.git +old-repo-branch:master ("master" - is the master branch in the new repository. "old-repo-branch" - is the branch that you want to use. In my case it was a master branch, so my command was +master:master)</li>
<li> That's it! Now you can go ahead and clone it to your local folder.
