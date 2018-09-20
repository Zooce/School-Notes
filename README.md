# Git Repo Management Strategies

## Cloning a public class repo, but pushing to a private one

If the class repo is organized such that there is a single class repo with all of the projects inside (the way it should be), then there's a very simple way to manaage this. Essentially, you create an empty private repo and add it as a remote to the cloned class repo.

First, create an empty private repo on Github. Next, clone the _public_ repo on your local machine.
```
$ git clone <public repo addr> <public repo name>
$ git submodule update --init --recursive  # if the public class repo has submodules
```
Finally, add the empty private repo as a remote, and for safety, set the origin push remote to an invalid url (so you can't accidentally push to the origin):
```
$ cd <public repo name>
$ git remote set-url --push origin <some invalid arr - could just be word>
$ git remote add <private remote name> <private repo addr>
```
That's it. Now for some management stuff.

### Pushing changes to the private remote

After adding your changes (which you should be doing on a separate branch), you can add your changes and push them to the private repo.
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
## Handling separate projects as submodules (only if necessary)

In the really crappy case that the course admin decide to use separate repositories for each project, you have one clean way to do this, submodules. Using submodules isn't really that bad - it's just multiple git repos inside another one. But it can be hard to get used to ad first. The biggest downside is that, using the method above - pushing to a private remote - means you have to set up a new private git repo for _EVERY_ project...balls... Whatever, it is what it is.

You could just keep each project separate - which is totally fine, but personally I like to have keep them together under a common repo and even use that common repo to put extra stuff like helpful tools that I write during the course, or notes, or whatever. This way I can keep track of everything in git, and when the class is over I can remove the files from my computer to create space but still have everything up in GitHub.

### Private master repo

Create a private repo on GitHub and clone it, that's it for this step.
```
$ git clone <private repo addr> <private repo name>
```

### Add each project as a submodule

Create another private repo on GitHub for each project, then add each _public_ project repo as a submodule to the private master repo.
```
$ git submodule add <public repo addr> <public repo name>
```

Now follow the steps above to setup the private remotes.

### Managing submodules

The only thing extra thing you have to do in terms of management is an extra commit and push in the master repo. For example, let's say you're finished with project 1. You've commited your changes in the project 1 submodule, and you've pushed them up to your private remote for project 1. At this point it might be a good idea to update the private master repo as well, so change directory up into the master repo, commit the changes (you'll see that the project 1 submodule has modifications), and push to the private master repo.
