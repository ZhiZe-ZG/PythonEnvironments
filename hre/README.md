# hre Environment

Used for HRE-related researches on Windows system and Linux systems.

Main packages:

| Name             | Conda Package Name | Conda Channel | Pip Package Name | Usage                                       |
| ---------------- | ------------------ | ------------- | ---------------- | ------------------------------------------- |
| PyTorch          | pytorch            | pytorch       |                  | Tensor engine.                              |
| Pytorch Vision   | torchvision        | pytorch       |                  | PyTorch vision utils.                       |
| Pytorch Audio    | torchaudio         | pytorch       |                  | PyTorch audio utils.                        |
| CUDA             | pytorch-cuda=11.8  | pytorch       |                  | Support PyTorch.                            |
| PyTorchLightning | lightning          | conda-forge   |                  | PyTorch models training utils.              |
| TensorBoard      | tensorboard        | conda-forge   |                  | Logging training.                           |
| Matplotlib       | matplotlib         | conda-forge   |                  | Draw figures.                               |
| Seaborn          | seaborn            | conda-forge   |                  | Draw statistical figures                    |
| Pandas           | pandas             | confa-forge   |                  | Load table data                             |
| IPython          | ipython            | conda-forge   |                  | Better Python interactive shell.            |
| TorchMetrics     | torchmetrics       | conda-forge   |                  | Calculate metrics. Support PyTorchLightning |
| Sklearn          | scikit-learn       | conda-forge   |                  | Some classic machine learning method.       |
| Skimage          | scikit-image       | conda-forge   |                  | Handle image files.                         |

* PyTorch, Pytorch Vision, Pytorch Audio were installed according to Conda command at [official web pages](https://pytorch.org/).
* PyTorchLightning was installed according to Conda command at [official web pages](https://lightning.ai/docs/pytorch/stable/).

## Update log

### 2.1.0

* Download tensorborad from conda-forge to get the newest version. (use: `conda install conda-forge::tensorboard`)
