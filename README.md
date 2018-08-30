# Git Repo Management Strategies

## Strategy 1: Using a private "master" repo to manage one or more class project repos

This approach is rather difficult without the help of a tool called [git-subrepo](https://github.com/ingydotnet/git-subrepo). git-subrepo allows you to track changes in the class project repos you clone into your private "master" repo, as if it were just a plain subdirectory. This can actually make things a bit confusing because after you clone these class project repos, they're no longer git repos...it's kind of weird, so stick with me and I'll try and show a simple workflow to keep things nice and easy.

_A quick note before we start. I'll use some dummy directories, branch names, etc. where necessary so don't count on this being an actual working tutorial. Also, I'm doing this on a Mac, but it should work for Linux distros as well (sorry I'm not a Windows kinda guy)._

### Install git-subrepo

First you need to install git-subrepo. The instructions are in the repo's README, but I'm duplicating them here. Note that you can use `~/.bashrc` instead of `~/.bash_profile` if you're on Linux.
```
$ git clone https://github.com/ingydotnet/git-subrepo.git ~/Developer/Other/
$ echo 'source ~/Developer/Other/git-subrepo/.rc' >> ~/.bash_profile
$ source ~/.bash_profile
```
Here's what's happening. `git clone ...` is just cloning the git-subrepo project onto your machine. No big deal. The second command is actually adding a line to your `~/.bash_profile` script to bring in all the goodies from git-subrepo, so you don't have to do it manually everytime you open the terminal. Finally, you're bringing in the changes you made to `~/.bash_profile` so you don't have to restart the terminal to start using git-subrepo.

### Create your "master" private repo

Next, create a private repo. You can do this several different ways, but for this example let's say you create one on GitHub and then clone it.
```
$ git clone https://github.com/Zooce/GATECH-CV.git ~/Developer/OMSCS/GATECH-CV
$ cd ~/Developer/OMSCS/GATECH-CV
```
This is going to be the repo that you're developing on - your're not going to be creating brances inside the class project repos that you clone (because git-subrepo doesn't really work like that). You'll have two main branches, `master` and `development`. _(You can also create experimental branches to test out ideas, but that's besides the point.)_ The `master` branch will be used for cloning new class project repos and pulling changes from them. When either of these two events happen, you'll merge `master` into `development` to keep `development` up-to-date. `development` will be the branch you actually develop on (uh..duh).

### Clone a class project repo (you'll do this for each project you want to clone)

Now clone a class project repo into your private "master" repo, with git-subrepo.
```
$ git subrepo clone https://github.gatech.edu/omscs6476/ps01.git
Subrepo 'https://github.gatech.edu/omscs6476/ps01.git' (master) cloned into 'ps01'.
```

_Unfortunately, `subrepo clone` squashes all of class repo's current history into a single "clone" commit, so you can't see the commit history, but whatever, right?_

> Here's a safety tip: you probably don't want to accidently push changes to the class repo upstream remote (honestly, you probably can't push directly to a repo you didn't create, but just play it safe), so you can prevent this by setting the push url for the class project repo to some invalid url (like `notaurl`). This will cause pushes to that remote to fail.
> ```
> $ git remote -v
> origin https://github.com/Zooce/GATECH-CV.git (fetch)
> origin https://github.com/Zooce/GATECH-CV.git (push)
> subrepo/ps01 https://github.gatech.edu/omscs6476/ps01.git (fetch)
> subrepo/ps01 https://github.gatech.edu/omscs6476/ps01.git (push)
> $ git remote set-url --push subrepo/ps01 notaurl
> $ git remote -v
> origin https://github.com/Zooce/GATECH-CV.git (fetch)
> origin https://github.com/Zooce/GATECH-CV.git (push)
> subrepo/ps01 https://github.gatech.edu/omscs6476/ps01.git (fetch)
> subrepo/ps01 notaurl (push)
> ```
> Here's an example of what happens when you try to push to the class project repo now.
> ```
> $ echo "I'm really cool, trust me." > test-file.txt
> $ git add . && git commit -m "I'm not that cool...blah"
> $ git push subrepo/ps01 master
> fatal: 'notaurl' does not appear to be a git repository
> fatal: Could not read from remote repository.
>
> Please make sure you have the correct access rights
> and the repository exists.
> ```

Now push the new "clone" commit to the private "master" repo.
```
$ git push origin master
```

### Start working in the class project repo

You can now start working in the class repo. Do this on the `development` branch.
```
$ git checkout -b development
...make some changes...
$ git add . && git commit -m "Almost finsihed with ps01"
$ git push origin development
```

### Pulling changes from class project repos

At this point, let's say that the TAs for the class updated or added a new test file to `ps01` and you want those changes. I mentioned earlier that you'll use the `master` branch for these things, so you don't have to worry about merge conflicts when using git-subrepo (it's easer to deal with these outside of git-subrepo). Also, you'll push the new changes you get up to our private "master" repo (for good measure) - you don't really have to do this now, but in this case, there's not really any reason not to.
```
$ git checkout master
$ git subrepo pull ps01
Subrepo 'ps01' pulled from 'https://github.gatech.edu/omscs6476/ps01.git' (master).
$ git push origin master
```

_This creates a new commit (even if there's no new changes upstream), so just be aware of that._

Now you can bring these changes over to your `development` branch with a simple git merge. If there are any conflicts, you can handle them as you always would with git (easy f-ing peasy).
```
$ git checkout development
$ git merge master
...(if necessary) fix any conflicts and finish the merge...
$ git push origin development
```

### Wrapping it all up

And that's about it. When a new project is available, you just follow the steps to clone the new project in the `master` branch, merge the `master` branch with the `development` branch and do you work.

## Strategy 2: Cloning a public class repo, but pushing to a private one

If the class repo is organized such that there is a single class repo with all of the projects inside (the way it should be), then there's a very simple way to manaage this. Essentially, you create an empty private repo and add it as a remote to the cloned class repo.

First, create an empty private repo on Github. Next, clone the public repo on your local machine.
```
$ git clone <public repo addr> <public repo name>
```
Finally, add the empty private repo as a remote:
```
$ cd <public repo name>
$ git remote add <private remote name> <private repo addr>
```
That's it. Now for some management stuff.

### Pushing changes to the private remote
```
$ git add <whatever you want to add>
$ git commit
$ git push <private remote name> <private remote branch>
```

### Pulling in changes from the public class repo
```
$ git fetch --all
$ git pull
$ git submodule update --init --recursive  # if the public class repo has submodules
```
