# Python 2

## Last Version of libraries still supporting by Python2

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
