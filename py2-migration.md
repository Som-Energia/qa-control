# Python 2

## Dependencies: Last Version still supporting Python2

The following libraries have stopped supporting Py2.
After dropping Py2 support, Pypi selects new incompatible versions to be instal·led in Py2 enviroments.
making the installation fail or, worse, failing on run-time.
While we are still supporting those legacy environments,
in order to stop this from happening you should restrain their versions, conditionally to being in a py2 env.

```python
'pymongo<4' # Py2, indirect flask-pymongo
'redis<4' # Py2, indirect rq
'rq<1.4.0' # Py2, indirect amoniak
'contextlib2<21' # Py2, indirect by many libs
'marshmallow<3' # Py2, indirect empowering
'arrow<1' # Py2, indirect empowering
'click<8' # Py2, indirect pytest, flask
'MarkupSafe<2' # Py2, indirect jinja2 <- flask
'numpy<1.17' # Py2
'decorator<5' # Py2
'tqdm<4.63.0' # Py2, depends on import-resources, not supported by py2
'pytest<5' # Py2 support dropped
'pytest-cov<3' # Py2
'mock<4' # Py2, indirect from pytest
'importlib-metadata<3' # Py2, indirect of pytest
'configparser<5' Py2, indirect of importlib-metadata, of pytest
'pyparsing<3' # Py2, indirect of packaging, of pytest
'zipp<2' # Py2, indirect of pytest
'packaging<21' # Py2, indirect importlib-metadata, zipp
'coverage<6' # Py2, indirect of pytest-cov
```

### How to include that?

#### In `setup.py`

```pythonimport sys
py2 = sys.version_info<(3,)                                                       
```

Then try to differentiate direct dependencies and indirect.

- For direct, keep the alternative for Py3.
- For indirect, the alternative should be an empty string and try to keep track of the direct dependency that leads to it.

```python
    'pytest<5' if py2 else 'pytest' # Py2
    'zipp<2' if py2 else '' # Py2, indirect of pytest
```

#### As requirements
```
pytest<5; python_version < '3.0'
pytest; python_version >= '3.0'
zipp<2; python_version < '3.0'
```

Caution, if your `setup.py` reads `requirements.txt` in order to fill `install_requires`, this would break `setup.py`,
since this syntax of conditional dependencies is not supported by `setuptools` v44, the last one available for Py2.
In this case you coul do a third approach.

#### As requirements also used in setup.py

- Place your regular dependencies in requirements.txt
```
pytest
...
```
- Place your constrained dependencies in requirements-py2.txt
```
pytest<5 # Py2
zipp<2 # Py2, indirect pytest
...
```

- Modify the `setup.py` to load one or another depending on the current python version.
```
requirements_file = 'requirements-py2.txt' if py2 else 'requirements.txt'
# and then do what you did with requirements.txt
```


### How to discover a new Py2 dropping library?

Local development environments usually do not detect such incompatibilities
because if a library is installed it won't be updated by default.
In order to trigger and clean those errors you should use a clean environment.


- Run the line
```bash 
$ deactivate ; rmvirtualenv py2; mkvirtualenv --python $(which python2) py2; pip install pipdeptree; ./setup.py develop
```
- Look in the log for the last library starting its installation, is usually the problematic one.
- Run pipdeptree to see which packages trigger the installation of the problematic one, in order to annotate any indirect
- Search the package in https://pypi.org
- Go to the repository and the CHANGES file
- Look for "Python 2" or "Python" in order to see which is the last version suporting Python 2.
- Start over again




