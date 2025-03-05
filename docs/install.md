# Installing pySNOW
Installing pySNOW is (*should be*) quite easy.

The first step (suggested) is to create a python environment for installation to avoid confilcts with previously installed libraries or different versions of required modules:

=== "Linux"

    ```bash
    python3 -m venv snowenv
    source snowenv/bin/activate
    ```

=== "Mac"

    ```bash
    python3 -m venv snowenv
    source snowenv/bin/activate
    ```

=== "Windows"

    ```bash
    python -m venv snowenv
    .\snowenv\Scripts\activate
    ```

After creating and activating the virtual environment you can clone the repository (or download the code as a zip and uncompressing it in any deisred location):

``` bash
git clone https://github.com/nanoMLMS/pySNOW.git pysnow && cd pysnow
```

From here you should be able to easily install the package by running 
```bash
pip install .
```

Automated tests can be run using pytest with the following command:
```bash
pytest .
```
from the pysnow folder.

