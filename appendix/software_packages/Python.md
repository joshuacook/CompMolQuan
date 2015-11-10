## Python

While Python comes preinstalled with Mac OS X, installing through homebrew ensures that you have the most updated version as well as moves the management of Python to homebrew. 

```
brew install python 
```

Python comes installed by default, but best to get the latest and greatest. 

The above installs `pip`.

\paragraph{Install Basic Python Packages}

Most of the pip installs require that they be run as `sudo`. `brew` is not a fan of `sudo` and should not have the same requirements. 

```
pip install numpy
pip install scipy
pip install matplotlib
```

\paragraph{Python as a Tool for Developing C Algorithms}

emulating minunit in python [[Needs Work]]