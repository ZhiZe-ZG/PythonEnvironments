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
conda env export | Select-String 'name:|prefix:' -notmatch | Set-Content environment_hre_windows.yml
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
New-Item -Path ./env -Name "hre_windows" -ItemType "directory"
tar -xvzf env_hre_windows.tar.gz -C ./env/hre_windows
```

Enable the specified environment after unpacking ('conda-unpack' is used for some path settings after unzip):

```powershell
conda activate ./env/hre_windows
conda-unpack
```



## 开发环境 | Development Environment

### 管理方式

使用 Conda 进行开发环境管理。该项目的环境设置为一个专用的 Conda 虚拟环境，安装于指定目录。

默认项目所用环境安装于项目内相对路径 `./env/<environment_name>` 文件夹中。其中 `<environment_name>` 表示环境名称。如果有需求，可以设置为其他路径。

考虑到项目可能在多个平台运行，不同平台的环境使用不同的命名：

* `hre_windows`: Windows 系统默认，按 GPU 配置但是也可以使用 CPU。
* `hre_linux`: Linux 系统默认，按 GPU 配置但是也可以使用 CPU。

### 通用管理命令

### `hre_linux` 常用管理命令

按照环境记录文件安装环境到指定路径：

```powershell
conda env create -f environment_hre_linux.yml --prefix ./env/hre_linux
```

按环境记录文件更新指定路径的环境（`--prune` 参数用于移除不再需要的包）：

```powershell
conda env update -f environment_hre_linux.yml --prefix ./env/hre_linux  --prune
```

导出当前激活环境记录文件（prefix 行和 name 行会直接删掉）：

```powershell
conda env export | Select-String 'name:|prefix:' -notmatch | Set-Content environment_hre_linux.yml
```

打包指定环境（也可以用 `-n` 通过名称指定打印全局环境）：

```powershell
conda pack --prefix ./env/hre_linux -o env_hre_linux.tar.gz
```

解压压缩包：

```powershell
New-Item -Path ./env -Name "hre_linux" -ItemType "directory"
tar -xvzf env_hre_linux.tar.gz -C ./env/hre_linux
```

解压后启用指定环境（`conda-unpack` 用于打包时的一些路径设置）：

```powershell
conda activate ./env/hre_linux
conda-unpack
```

### 基本环境描述

这里简单列举本项目所需要的开发环境：

