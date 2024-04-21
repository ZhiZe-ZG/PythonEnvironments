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

## 开发环境 | Development Environment

### 管理方式

使用 Conda 进行开发环境管理。该项目的环境设置为一个专用的 Conda 虚拟环境，安装于指定目录。

默认项目所用环境安装于项目内相对路径 `./env/<environment_name>` 文件夹中。其中 `<environment_name>` 表示环境名称。如果有需求，可以设置为其他路径。

考虑到项目可能在多个平台运行，不同平台的环境使用不同的命名：

* `hre_windows`: Windows 系统默认，按 GPU 配置但是也可以使用 CPU。
* `hre_linux`: Linux 系统默认，按 GPU 配置但是也可以使用 CPU。

对于每一个环境，至少提供环境说明文档以确保能重建环境。详情参考： <https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html>。

此外，对于相同系统环境，如果可以，使用 Conda-Pack 提供环境打包。这样可以避免反复下载安装包的麻烦。详情参考： <https://conda.github.io/conda-pack/>。

### 通用管理命令

查看当前已创建的环境：

```powershell
conda info --envs
```

对环境做操作时可以使用 `--name` 参数后接环境名指定，也可以使用 `--prefix` 参数后接路径指定。这两种选择方式不能同时使用。一般来说使用环境名指定的是对于 Conda 来说全局安装的环境。

创建环境使用命令：

```powershell
conda env create --prefix ./env/hre
```

删除环境使用命令：

```powershell
conda env remove --prefix ./env/hre
```

激活指定环境使用命令：

```powershell
conda activate ./env/hre
```

取消当前环境激活状态使用命令：

```powershell
conda deactivate
```

### `hre_windows` 常用管理命令

按照环境记录文件安装环境到指定路径：

```powershell
conda env create -f environment_hre_windows.yml --prefix ./env/hre_windows
```

按环境记录文件更新指定路径的环境（`--prune` 参数用于移除不再需要的包）：

```powershell
conda env update -f environment_hre_windows.yml --prefix ./env/hre_windows  --prune
```

导出当前激活环境记录文件（prefix 行和 name 行会直接删掉）：

```powershell
conda env export | Select-String 'name:|prefix:' -notmatch | Set-Content environment_hre_windows.yml
```

打包指定环境（也可以用 `-n` 通过名称指定打印全局环境）：

```powershell
conda pack --prefix ./env/hre_windows -o env_hre_windows.tar.gz
```

解压压缩包（Windows 现在自带了 tar 等基本解压工具）：

```powershell
New-Item -Path ./env -Name "hre_windows" -ItemType "directory"
tar -xvzf env_hre_windows.tar.gz -C ./env/hre_windows
```

解压后启用指定环境（`conda-unpack` 用于打包时的一些路径设置）：

```powershell
conda activate ./env/hre_windows
conda-unpack
```

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

| 名称                           | Conda 包名   | Pip 包名 | 用途                                        |
| ------------------------------ | ------------ | -------- | ------------------------------------------- |
| PyTorch                        |              |          | 张量运算引擎                                |
| PyTorchLightning               |              |          | 方便编写 PyTorch 训练脚本和数据可视化       |
| TensorBoard                    | tensorboard  |          | 用于记录和查看训练记录                      |
| Matplotlib                     | matplotlib   |          | 图表绘制                                    |
| IPython                        | ipython      |          | 更好用一点的 Python Shell                   |
| black                          | black        |          | Code formatting tools.  由于 VS Code Python插件的更新，该包不再是必要的。 |
| Sklearn                        | scikit-learn |          | 计算评价指标                                |

* PyTorch 按照官方的给出的 Conda 命令安装，连同 `torchvision` 和最新支持的 CUDA 一起安装。
* PyTorchLightning 按照官方给出的 Conda 命令安装。
