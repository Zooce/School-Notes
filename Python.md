# Python Stuff

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
