# Publishing a Python Package to PyPi

The Python Package Index (PyPI) has become the go-to place for searching and downloading python packages. This guide will take you through all of the steps to take a python project and upload it so that others may use it.

## Preparing Project

### Folder Structure

To help us illustrate how the folder structure is handled, let us use an example. Say we want to deploy the package with the name `<package-name>` to PyPI. The root folder of our repo is called `base` and inside, we should have the following structure:

```
.
├── LICENSE
├── README.md
├── demo/
│   ├── __init__.py
│   ├── demo.py
│   └── ...
├── <package-name>/
│   ├── __init__.py
│   ├── package_file.py
│   └── ...
├── requirements.txt
├── setup.py
└── tests/
    ├── __init__.py
    ├── test_file.py
    └── ...
```

The core files of the library should all be within the `<package-name>/` folder. The `demo` and `tests` folders are places where you can add example scripts and tests if desired. The contents of the `setup.py` script is covered in the next section.

The `__init__.py` files allow external scripts to 'see' the `<package-name>/` folder. Examples of the contents of these files are shown below:

=== "`<package-name>/__init__.py`"
    ``` python
    from .package_file import PackageClass
    # Add other files and their classes here
    ```

=== "`demo/__init__.py`"
    ``` python
    from sys import path
    from os.path import dirname, join, abspath
    path.insert(0, abspath(join(dirname(__file__), '..')))
    ```

=== "`tests/__init__.py`"
    ``` python
    from sys import path
    from os.path import dirname, join, abspath
    path.insert(0, abspath(join(dirname(__file__), '..')))
    ```

By setting the `__init__.py` files this way you can just call the package classes in a similar way to other packages:

``` python
from <package_name>.package_file import PackageClass
```

### Creating Setup Script
The setup script is what will be used to create the distribution that will be uploaded to PyPI. It should be located in the root directory of you repository. Below you can find a template of the `setup.py` script.

``` python title="setup.py"
from setuptools import setup, find_packages

VERSION = '1.1.0'
DESCRIPTION = 'Transforming and handling poses.'

# read the contents of your README file
from pathlib import Path
this_directory = Path(__file__).parent
long_description = (this_directory / "README.md").read_text()

# Setting up
setup(
    name="<package-name>",
    version=VERSION,
    author="Firstname Lastname",
    author_email="<email@address.com>",
    description=DESCRIPTION,
    long_description=long_description,
    long_description_content_type='text/markdown',
    packages=find_packages(),
    install_requires=['numpy', 'scipy'],
    keywords=['python'],
    classifiers=[
        "Intended Audience :: Developers",
        "Programming Language :: Python :: 3",
        "Operating System :: Unix",
        "Operating System :: MacOS :: MacOS X",
        "Operating System :: Microsoft :: Windows",
    ]
)

```

This setup file defines the version number, which operating systems are allowed to install it, and which package dependencies this published package has. This setup file is also configured to show the `README.md` file of the repository as the long description of the package of PyPI.

!!! info
    Future improvement should be made to:
    
    - Include a link to the documentation of the project
    - Automatically import the requirements from the `requirements.txt` file
    - Automatically increment the version number on a new build

## Publishing

### Create Account on PyPi
If you don't already have an account on PyPi, you will need to create one (you can do so [here](https://pypi.org/account/register/)). Make sure to save your username and password as you will need it for the following steps.

### Publish Package

Now that you project is ready, and you have an account on PyPI, we can now move onto publishing the package.

#### Building Project
Let us start by building the project, so that it is saved as an executable. Go to the root folder of your project (`base` in the example), and run the commands below:

``` bash
rm -rf build dist *.egg-info
python3 setup.py sdist bdist_wheel
```

!!! info
    Make sure to change the version number of your package in the `setup.py` file before building, otherwise PyPI will reject the new upload.

The command with create three new folders:

- `build`
- `dist`
- `<package-name>.egg-info`

The contents of the `dist` folder is what we will upload to PyPI.

#### Publishing the Build using Twine
To publish this new build, we will use the `twine` python package (`pip3 install twine`).

To upload, simply type in the following command:

``` bash
twine upload dist/*
```

`twine` will then ask for your PyPI username and password, and then upload the new build!