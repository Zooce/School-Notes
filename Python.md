# Python Stuff

## Working Anaconda (preferably Miniconda) 

Anaconda is similar to `virtualenv` and `virtualenvwrapper` in that it allows you to create "pseudo" virtual environments for Python. You can download Anaconda from the [Anaconda distribution repository](https://repo.continuum.io/) or the [Anaconda download page](https://www.anaconda.com/distribution/). Once downloaded you can create environments like this (straight from the [documentation](https://conda.io/docs/user-guide/tasks/manage-environments.html#creating-an-environment-with-commands)):

```bash
$ conda create --name myenv
```

You can install whatever libraries you want as well with:

```bash
$ conda install --name myenv python=3.7 scipy=0.15.0
```

Better yet, you can create an `environments.yaml` file that defines all the libraries and packages you want installed in your environment.

`environment.yaml`
```yaml
name: myenv
channels:
  - anaconda
  - defaults
dependencies:
  - python=3.7.*
  - numpy=1.15.4
  - scipy=1.2.0
  - matplotlib=3.0.2
  - networkx=2.2
  - pgmpy=0.1.6
  - nelson=0.4.3
```

!!! WIP !!! <<<

## Working with `virtualenv and virtualenvwrapper`

`virtualevn` is a Python virtual environment manager. With this tool you can create pseudo virtual environments. These aren't real virtual environments, like the ones you run in VirtualBox, but instead they simply map a set of Python dependencies to a certain directory, and make sure that your Python environment uses the dependencies in that directory while the pseudo virtual environments is active.

`virtualenvwrapper` is just a wrapper around `virtualenv` that simplifies the management of these virtual environments.

## Setting up a new Python virtual environment
```
$ mkvirtualenv myenv
```

## Enter the Python virtual environment
```
$ workon myenv
(myenv) $ 
```

## Exit the Python virtual environment
```
(myenv) $ deactivate
$
```

TODO - more to do here
