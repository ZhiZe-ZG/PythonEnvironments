# PythonEnvironments

My Python environments.

## Management Method

Python applications in many fields are highly dependent on binary dynamic libraries. But Python's own ability to manage these libraries is weak. Therefore, Conda is mainly used to manage the Python development environments. In addition to this, Docker etc. are also possible options, but these are still under research and are not used as a primary choice.

## Install and Maintain Manager

### Miniconda

Install Miniconda, refer to:

* [Miniconda](https://docs.anaconda.com/free/miniconda/index.html)

### Install Conda-Pack

Conda-Pack need be installed in base environment.

Refer to:

* [Conda-Pack](https://conda.github.io/conda-pack/)

Install Conda-Pack command:

```powershell
conda install -c conda-forge conda-pack
```

### Update Manager

In `base` environment:

```powershell
conda update --all
```
