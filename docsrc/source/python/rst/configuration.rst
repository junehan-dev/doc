Configuration
=================

intro
-----

The Configuration directory must contain a file named ``conf.py``.
this file is called the "build Configuration file" and, contains all confs to customize sphinx input and output behavior

Keynotes
--------

- if not otherwise documented, values must be strings N theire default is empty stirng.
- the term "fully-qualified name" refers to stirng that names an importable ``Python object`` inside a module;
    - FQN "sphinx.buildes.Builder" means, the ``Builder`` class in real exist module.

- Remember that document names use "/" as the path seperator and dont contain the file name extension.
