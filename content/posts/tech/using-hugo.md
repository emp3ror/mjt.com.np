+++
title = "Switching to HUGO"
date = "2019-03-13T12:48:42Z"
tags = ["test"]
draft = false
author = "admin"
+++

From this day, I am using HUGO, switched from Wordpress to my own [Angularjs SPA static site](https://github.com/emp3ror/static-site-generator) to HUGO... yay

I did thought, "Oh why not just use medium.com and get rid of all setting up hassles", then god spoke with this message from medium.com
![Medium image](/img/medium.png)

At that point, I realised, I shouldn't host my amazing articles ( :P ) in others' platform :D :D

## Hugo

Hugo is open-source static site builder written in Go lang. To know more about check hugo official site [https://gohugo.io/](https://gohugo.io/)

### Getting Ready for Hugo in VSCODE

Hugo use mark down for writing blogs/article. Instead of using different IDE, I am using VSCODE which I already use for my works. Below are few plugins I added in Vscode.

#### Markdown plugin for vscode

I am using this plugin for easy typing/creating markdown
[Markdown All in One](https://github.com/yzhang-gh/vscode-markdown).

Seems, MD is already supported by vscode but I installed it anyway :D :D

#### Date plugin for vscode

While creating blog post in text-editor, it hard to add date so I installed this plugin.

[insert Date Time](https://marketplace.visualstudio.com/items?itemname=jsynowiec.vscode-insertdatestring)

    Ctrl+Alt+Shift+I then ISO // to insert iso date

### Git worktree

Amazingly I got to learn about git worktree. Its a feature of git to have a folder with different branch in same repository. Learn more about git worktree in [git website](https://git-scm.com/docs/git-worktree)

    // Setting up public directory with gh-pages branch
    $ git worktree add -B gh-pages public origin/gh-pages
