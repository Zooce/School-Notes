# Git Stuff

## Cloning a public repo, but pushing to a private one

For school, most of the coding projects are hosted on the school's Github account. As a student, we have to clone that repo, work from it, submit from it, and pull new changes from it. However, where do we push our changes? How do we keep track of our work, separate from the public repo that all of the other students are using? A very simple way to do this, is to create an empty private repo and add it as a remote.

0. Create an empty private repo on Github

1. Clone the public repo on your local machine
```
$ git clone <public repo addr>
```

2. Add the empty private repo as a remote
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
3. Push an initial commit to the private repo's master branch
  a. This initial commit serves as a starting point
4. Create a working branch off the private master branch
5. Do all of my work on the private working branch
6. Whenever new changes are made on the public branch, pull and merge them into the private master branch, and then do the same from the private master branch into the private working branch
