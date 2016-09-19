---
layout: post
ref: manage-dotfiles
date:   2016-09-19 00:00:00 +0900
title: Manage Dotfiles With a Bare Git Repository
lang: en
---

After switching from Sublime Text to vim, I started looking for a way to manage my dotfiles. It took me a lot of work to configure and understand them, so I wanted to back them up against emergency. And of course I wanted to have those personal settings wherever I work. 

At first I considered Vagrant, but it seemed like an overkill for just a few dotfiles. My next idea was to symlink dotfiles to a single directory and manage that folder with git. But that also felt unnecessarily complex - surely there would be a simpler way to do this, right?

And then I found what I was looking for. This method uses a <a href="http://www.saintsjd.com/2011/01/what-is-a-bare-git-repository/">git bare repository</a>, and aliasing git commands to send them to that repo. Simple setup process? Check. Version control in git? Check. Easy installation on different machine? Check. Awesome!

### Setup

Git is the only dependency. The following four lines will set up the bare repository.

{% highlight bash linenos %}
git init --bare $HOME/.config
echo "alias config='/usr/bin/git --git-dir=$HOME/.config/ --work-tree=$HOME'" >> $HOME/.zshrc
reload ~/.zshrc
config config --local status.showUntrackedFiles no
{% endhighlight %}

1. Create a git bare repository at ~/.config to track files.
2. Add alias setting to shell configuration file. I use zsh so it's .zshrc. For bash, it'd be .bashrc. Note how the paths for git directory and working tree are set.
3. Reload the shell setting.
4. Prevent untracked files from showing up when we call config status.

That finishes the setup. Use the aliased command from the home directory to manage files, and use git remote repo if you want to manage the files online.

{% highlight bash linenos %}
config status
config add .vimrc
config commit -m "Add vimrc"
config remote add origin https://www.github.com/username/repo.git
config push origin master
{% endhighlight %}

### Installing dotfiles to another system

It just needs two shell commands before fetching the remote repo.

{% highlight bash linenos %}
echo "alias config='/usr/bin/git --git-dir=$HOME/.config/ --work-tree=$HOME'" >> $HOME/.zshrc
reload ~/.zshrc
echo ".config" >> .gitignore
git clone --bare https://www.github.com/username/repo.git $HOME/.config
config checkout
config config --local status.showUntrackedFiles no
{% endhighlight %}

1. Create alias to ensure that the git bare repository works without problem.
2. Reload the shell setting to use that alias.
3. Add .config directory to gitignore to prevent recursion issues.
4. Clone the repo.
5. Check if it works fine.
6. Prevent untracked files from showing up on config status.

That's about it! Credit goes to anonymous giants in the Internet for coming up with this, and Nicola Paolucci for elegantly documenting the steps. Here's the <a href="https://developer.atlassian.com/blog/2016/02/best-way-to-store-dotfiles-git-bare-repo/">link</a> to his original post.
