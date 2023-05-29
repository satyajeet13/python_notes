# Useful `pip` commands

* Navigate to the folder where the python project will be saved.

```
C:\Users\username\Desktop> mkdir project
```

* Create a virtual environment for the project.

```
C:\Users\username\Desktop\project> python -m venv venv
```

* Active virtual environment.

```
C:\Users\spadhi\Desktop\project> .\venv\Scripts\activate
(venv) PS C:\Users\spadhi\Desktop\project>
```
* To deactive the virtual environment, type `deactivate`.

```
(venv) PS C:\Users\spadhi\Desktop\project> deactivate
C:\Users\spadhi\Desktop\project>
```
* List of packages installed in virtual environment.

```
(venv) PS C:\Users\spadhi\Desktop\project> pip list
Package    Version
---------- -------
pip        23.1.2
setuptools 63.2.0
```
* Installing packages in a virtual environment. First activate virtual environment if not activated already.

```
C:\Users\spadhi\Desktop\project> venv\Scripts\activate
(venv) PS C:\Users\spadhi\Desktop\project> pip install requests
```

* Result after intalling `requests` package

```
(venv) PS C:\Users\spadhi\Desktop\project> pip list
Package            Version
------------------ --------
certifi            2023.5.7
charset-normalizer 3.1.0
idna               3.4
pip                23.1.2
requests           2.31.0
setuptools         63.2.0
urllib3            2.0.2
```
The additional packages are needed for `requests` to work properly and are called dependencies for `requests` package. 

* To show requirements/dependecies of a package, type

```
(venv) PS C:\Users\spadhi\Desktop\project> pip show requests
Name: requests
Version: 2.31.0
Summary: Python HTTP for Humans.
Home-page: https://requests.readthedocs.io
Author: Kenneth Reitz
Author-email: me@kennethreitz.org
License: Apache 2.0
Location: c:\users\spadhi\desktop\project\venv\lib\site-packages
Requires: certifi, charset-normalizer, idna, urllib3
Required-by:
```
* Uninstalling packages

```
(venv) PS C:\Users\spadhi\Desktop\project> pip uninstall requests
Found existing installation: requests 2.31.0
Uninstalling requests-2.31.0:
  Would remove:
    c:\users\spadhi\desktop\project\venv\lib\site-packages\requests-2.31.0.dist-info\*
    c:\users\spadhi\desktop\project\venv\lib\site-packages\requests\*
Proceed (Y/n)? y
  Successfully uninstalled requests-2.31.0
(venv) PS C:\Users\spadhi\Desktop\project>
```
`pip` uninstalls only the requested package and not the dependencies.

```
(venv) PS C:\Users\spadhi\Desktop\project> pip list
Package            Version
------------------ --------
certifi            2023.5.7
charset-normalizer 3.1.0
idna               3.4
pip                23.1.2
setuptools         63.2.0
urllib3            2.0.2
```
To uninstall other packages, run the following command.

```
(venv) PS C:\Users\spadhi\Desktop\project> pip uninstall certifi charset-normalizer idna urllib3
```
Verify by typing `pip list`

```
(venv) PS C:\Users\spadhi\Desktop\project> pip list
Package    Version
---------- -------
pip        23.1.2
setuptools 63.2.0
```

* Declaring Requirements - used when working on shared projects.

The list of packages installed can be shown using `pip list`.
```
(venv) PS C:\Users\spadhi\Desktop\project> pip install requests rich
(venv) PS C:\Users\spadhi\Desktop\project> pip list
Package            Version
------------------ --------
certifi            2023.5.7
charset-normalizer 3.1.0
idna               3.4
markdown-it-py     2.2.0
mdurl              0.1.2
pip                23.1.2
Pygments           2.15.1
requests           2.31.0
rich               13.3.5
setuptools         63.2.0
urllib3            2.0.2
```
There is another way to show the packages installed using `pip freeze`.

```
(venv) PS C:\Users\spadhi\Desktop\project> pip freeze
certifi==2023.5.7
charset-normalizer==3.1.0
idna==3.4
markdown-it-py==2.2.0
mdurl==0.1.2
Pygments==2.15.1
requests==2.31.0
rich==13.3.5
urllib3==2.0.2
```
To save the output to file, use the `>` to redirect the output to a file.

```
(venv) PS C:\Users\spadhi\Desktop\project> pip freeze > requirements.txt
```
Creates a new `requirements.txt` file. If the file is already present, it is overwritten with the new output.

To view the contents of the `requirements.txt` file, type the following command.

```
(venv) PS C:\Users\spadhi\Desktop\project> cat requirements.txt
certifi==2023.5.7
charset-normalizer==3.1.0
idna==3.4
markdown-it-py==2.2.0
mdurl==0.1.2
Pygments==2.15.1
requests==2.31.0
rich==13.3.5
urllib3==2.0.2
```

* To uninstall package install at global level, use a `requirements` file to uninstall all at once.

```
PS C:\> pip freeze > requirements.txt
PS C:\> pip uninstall -r .\requirements.txt -y
```
Check the remaining package using `pip list`

```
PS C:\> pip list
Package    Version
---------- -------
pip        23.1.2
setuptools 65.6.3
wheel      0.38.4
```
* In addition to `pip freeze`, you can also use `xargs` to uninstall all the `pip` packages.

```
pip freeze | xargs pip uninstall -y
```

* To recreate a previously created development environment, copy the `requirements.txt` file from the former development environment into the new directory

```
(venv) PS C:\Users\spadhi\Desktop\same_project> cp C:\Users\spadhi\Desktop\project\requirements.txt ./requirements.txt
```
Install packages using the `requirements.txt` file
```
(venv) PS C:\Users\spadhi\Desktop\same_project> pip install -r requirements.txt
```
