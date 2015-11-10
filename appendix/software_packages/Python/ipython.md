\paragraph{IPython}

```
pip install ipython
```

This did not install all of the dependencies required to run IPython notebook. It was fairly straightforward to identify the missing dependencies, however, by attempting to run `IPython Notebook`.

```
ipython notebook 
```

The following three dependencies were succesively required:

```
pip install pyzmqi
pip install jinjaz
pip install tornado
```