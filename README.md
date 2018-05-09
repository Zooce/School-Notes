# Git Stuff

## Cloning a public repo, but pushing to a private one

For school, most of the coding projects are hosted on the school's Github account. As a student, we have to clone that repo, work from it, submit from it, and pull new changes from it. However, where do we push our changes? How do we keep track of our work, separate from the public repo that all of the other students are using? A very simple way to do this, is to create an empty private repo and add it as a remote.

First, create an empty private repo on Github. Next, clone the public repo on your local machine:
```
$ git clone <public repo addr>
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
$ git submodule update --init --recursive
```

#### Extra thoughts

You can add branches to the private repo and manage all of them just like you would in any other case. Typically for these school projects I use the following process:
1. Clone the public repo
2. Create a private repo and setup the private remote
3. Push an initial commit to the private repo's master branch, which serves as a starting point
4. Create a working branch off the private master branch
5. Do all of my work on the private working branch
6. Whenever new changes are made on the public branch, pull and merge them into the private working branch (you can also keep the private master branch updated as well - this is a little extra work that hasn't yet been necessary for me)

Here's an example `.git/config`:

```
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
[remote "origin"]
	url = https://github.gatech.edu/omscs6475/assignments.git
	fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
	remote = origin
	merge = refs/heads/master
[remote "private"]
	url = https://github.com/Zooce/OMSCS-CP-CS6475.git
	fetch = +refs/heads/*:refs/remotes/private/*
[branch "working"]
	remote = private
	merge = refs/heads/working
[lfs "https://github.com/Zooce/OMSCS-CP-CS6475.git/info/lfs"]
	access = basic
```
