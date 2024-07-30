# PythonEnvironments

My Python environments.

## Management Method

Python applications in many fields are highly dependent on binary dynamic libraries. But Python's own ability to manage these libraries is weak. Therefore, Conda is mainly used to manage the Python development environments. In addition to this, Docker etc. are also possible options, but these are still under research and are not used as a primary choice.

## Install and Maintain Manager

### Miniconda

It is recommended to use the Miniconda package when installing Conde (just use the default options, no need to add Path on Windows).

Refer to:

* [Miniconda](https://docs.anaconda.com/free/miniconda/index.html)

### Init Conda in PowerShell

Generally speaking, the Miniconda installation package will register the Bash or Zsh used at that time. Windows will provide cmd and PowerShell usage entrances by default, but PowerShell will not be registered by default.

Init Conda in PowerShell:

```shell
conda init powershell
```

On Windows systems, if after initialization you still cannot see the Conda prompt in normal PowerShell, run in PowelShell (as administrator):

```PowerShell
Set-ExecutionPolicy unrestricted
```

This command enables PowerShell scripting. Windows’ built-in PowerShell has the scripting function turned off by default, but Conda relies on the scripting function of PowerShell so it needs to be turned on.

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

If you want to only update conda:

```powershell
conda update conda
```

## Environment Management

### Show Environments

Show created environments:

```powershell
conda info --envs
```

### Choose an Environment

Select an environment by its name with `--name`. Use `--prefix` to select the environment through the environment path. You can't use both at the same time. In general, name selection is only available for environments installed to the Conda default path. So I recommend using path.

### Create and Delete Environments

Create an environment:

```powershell
conda create --prefix ./env/hre
```

Delete an environment:

```powershell
conda env remove --prefix ./env/hre
```

### Activate and Deactivate Environments

Activate an environment (by paths or names, here using the path):

```powershell
conda activate ./env/hre
```

Deactivate the current environment:

```powershell
conda deactivate
```

### Environment YML File

To ensure the environment can be re-created, you should keep the environment YML file. Refer to: <https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html>。

Create an environment from an environment YML file:

```powershell
conda env create -f environment_hre.yml --prefix ./env/hre
```

Update an environment according to n environment YML file (`--prune` to remove packages that are not needed anymore):

```powershell
conda env update -f environment_hre.yml --prefix ./env/hre  --prune
```

Export an environment YML file based on the current environment:

```powershell
conda env export | Select-String 'name:|prefix:' -notmatch | Set-Content environment_hre.yml
```

The `Select-String 'name:|prefix:' -notmatch` remove the `name` line and `prefix` line. They are typically specified via command-line parameters, so they don't need to appear in the environment YML file.

### Conda-Pack Environment Package

For the same system, use Conda-Pack to provide environment packaging. This saves you the hassle of downloading the packages repeatedly. For more information, please refer to: <https://conda.github.io/conda-pack/>.

Package the specified environment (you can also specify the global environment by name with '-n'):

```powershell
conda pack --prefix ./env/hre -o env_hre.tar.gz
```

Unzip the zip package (Windows now has the basic unix-like decompression tools such as tar):

```powershell
New-Item -Path ./env -Name "hre" -ItemType "directory"
tar -xvzf env_hre.tar.gz -C ./env/hre
```

Enable the specified environment after unpacking ('conda-unpack' is used for some path settings after unzip):

```powershell
conda activate ./env/hre_windows
conda-unpack
```

## About version number

Version number syntax:

```text
major_number.minor_number.patch_number
```

A environment with same content for different platform should have save `major_number` and `minor_number` but the `patch_number` can be different.

Fully updating all packages changes the `minor_number`. Deleting or adding packages changes the `major_number`. Other small changes will change the `patch_number`.
