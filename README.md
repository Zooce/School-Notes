# Git Stuff

## Cloning a public repo, but pushing to a private one

For school, most of the coding projects are hosted on the school's Github account. As a student, we have to clone that repo, work from it, submit from it, and pull new changes from it. However, where do we push our changes? How do we keep track of our work, but keep it separate from the public repo that all of the other students are using? A very simple way to do this, is to create an empty private repo and add it as a remote to the cloned, public repo. _Note this works well when there's only a single public repo you need to worry about. There's another way if you have to deal with multiple public repos...keep reading._

First, create an empty private repo on Github. Next, clone the public repo on your local machine:
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

### Pulling in changes from the public repo (remote)

```
$ git fetch --all
$ git pull
$ git submodule update --init --recursive  # if the public repo has submodules
```

## Using a "master" private repo to manage more than one public repo

As mentioned earlier, things might not work out as nicely if you have to clone multiple repos, like if each project in the course is in its own repo. No need to worry, there's a solution to this, and you don't have to use any complicated features of git, like submodules.

First, create a private repo, and add a README.md. _(You don't have to create a README.md, but why not? You can add some comments in there that help you remember what you're doing.)_

Next, clone that private repo onto your local machine:
```
$ git clone <private repo addr> <private repo name>
```

Then, clone a public repo into the "master" private repo:
```
$ cd <private repo name>
$ git clone <public repo addr> <public repo name>
```

Now create a working branch in the public repo, and get to work. When you're ready to commit your changes, do so in the public repo, on the working branch. You may be asking, "Why am I commiting changes to a working branch in the public repo, when I have nowhere to push them from there?" The answer is because if you need to pull changes from the public repo, you'll be able to do so with no issues, and you'll be able to simply merge the public master branch with your working one, and all will be well.
```
$ cd <public repo name>
$ git checkout -b working
# ...make some changes...
$ git add .
$ git commit
```

At this point we want to push our changes so we have them recorded in our private "master" repo. Let's take this one step at a time. First, change directories into the private repo, and run `git status`:
```
$ cd ..
$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	<public repo name>

nothing added to commit but untracked files present (use "git add" to track)
```

Even though we commited the changes in the public repo, those same changes haven't been recorded in the private "master" repo. This is great! This is exactly what we wanted, because now we can push the same changes to our private repo. So let's commit these changes in the private repo:
```
$ git add .
warning: adding embedded git repository: <public repo name>
hint: You've added another git repository inside your current repository.
hint: Clones of the outer repository will not contain the contents of
hint: the embedded repository and will not know how to obtain it.
hint: If you meant to add a submodule, use:
hint: 
hint: 	git submodule add <url> <public repo name>
hint: 
hint: If you added this path by mistake, you can remove it from the
hint: index with:
hint: 
hint: 	git rm --cached <public repo name>
hint: 
hint: See "git help submodule" for more information.
```

Okay, WTF is this!? Don't worry, git is just telling us that it knows that we're trying to add changes from an embedded git repo, and it's warning us that our private "master" repo will not know how to detect changes from the public repo's upstream remote. Lucky for us, we don't want our private repo knowing how to do this, so we're all good. _Just to be clear, if we change directory into the public repo, it will still be able to detect changes from its own upstream remote, so need to worry._

Let be absolutely certain that we all understand something...and this is very **important**...DO NOT turn the public repo into a submodule of the private repo! Again, DO NOT (**DO NOT**) add the _public_ repo as a submodule of the _private_ repo!

Now just commit the changes, and push them to the private repo, F@#K YEA!
```
$ git commit
$ git push origin <branch>
```

## Troubleshooting

The instructions above have been verified with git version 2.18.0. If you don't have this version, your commands may have to change a bit. Also, for the "master" private repos, I typically only push to master, because I'm almost always exclusively pushing to the repo, and not pulling.

Here's a few things to try if you're having issues (this list will grow overtime).

### Check your version of git
```
$ git --version
```

### Check which refs you have
```
$ git show-ref
```

### Push with a specific ref
```
$ git push origin HEAD:<branch name>

OR

$ git push origin HEAD:refs/heads/<branch name>
```
