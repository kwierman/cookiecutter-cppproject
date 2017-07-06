# cookiecutter-cppproject
Cookiecutter for C++ Projects


## Requirements

~~~ bash
pip install cookiecutter
~~~

## Usage

Initial setup:

~~~ bash
 cookiecutter gh:kwierman/cookiecutter-cppproject
~~~

Setup google test:

~~~ bash
git clone https://github.com/google/googletest googletest
~~~

And you should be good to go. For standalone development, the typical workflow is to create `build` and `install` subdirectories.

Tests can be run from the build directory via:


~~~ bash
make test
~~~

