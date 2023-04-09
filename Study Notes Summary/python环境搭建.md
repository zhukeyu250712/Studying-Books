# 一、环境搭建

1、创建py3.9环境

```powershell
(base) C:\Users\ZKY>conda info --envs
# conda environments:
#
base                  *  D:\software\sf\Ana
zky                      D:\software\sf\Ana\envs\zky


(base) C:\Users\ZKY>conda create --name d2l python=3.9 -y
```

```powershell
(base) C:\Users\ZKY>conda info --envs
# conda environments:
#
base                  *  D:\software\sf\Ana
d2l                      D:\software\sf\Ana\envs\d2l
zky                      D:\software\sf\Ana\envs\zky


(base) C:\Users\ZKY>activate d2l

(d2l) C:\Users\ZKY>
```

查看镜像源配置

```powershell
(d2l) C:\Users\ZKY>conda config --show channels
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
```

2、py3.9 相应的库安装

```powershell
conda install pytorch torchvision torchaudio cpuonly -c pytorch
```

done为安装成功

```powershell
done
Retrieving notices: ...working... done

(d2l) C:\Users\bigdata>python
Python 3.9.16 (main, Mar  8 2023, 10:39:24) [MSC v.1916 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import torch
>>>
```

参考：https://blog.csdn.net/qq_45281807/article/details/112442423

3、使用jupyter报错解决

https://blog.csdn.net/qq_43678005/article/details/122509756

```powershell
$ conda install -c conda-forge ipykernel
```

4、打开jupyter

```powershell
(base) C:\Users\bigdata>conda info --envs
# conda environments:
#
base                  *  D:\Software\sf\Ana
d2l                      D:\Software\sf\Ana\envs\d2l
zky                      D:\Software\sf\Ana\envs\zky


(base) C:\Users\bigdata>conda config -show channel
usage: conda-script.py [-h] [-V] command ...
conda-script.py: error: unrecognized arguments: -show channel

(base) C:\Users\bigdata>conda config --show channel

ArgumentError: Invalid configuration parameters:
  - channel


(base) C:\Users\bigdata>conda config --show channels
channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/

(base) C:\Users\bigdata>activate d2l

(d2l) C:\Users\bigdata>jupyter notebook
[W 20:14:12.648 NotebookApp] Loading JupyterLab as a classic notebook (v6) extension.
[I 2023-03-19 20:14:12.654 LabApp] JupyterLab extension loaded from D:\Software\sf\Ana\lib\site-packages\jupyterlab
[I 2023-03-19 20:14:12.654 LabApp] JupyterLab application directory is D:\Software\sf\Ana\share\jupyter\lab
[I 20:14:13.811 NotebookApp] Serving notebooks from local directory: C:\Users\bigdata
[I 20:14:13.811 NotebookApp] Jupyter Notebook 6.5.2 is running at:
[I 20:14:13.812 NotebookApp] http://localhost:8888/?token=095dc4382e8653f3b39aa2fec60383712ccee6d8a8945052
[I 20:14:13.812 NotebookApp]  or http://127.0.0.1:8888/?token=095dc4382e8653f3b39aa2fec60383712ccee6d8a8945052
[I 20:14:13.812 NotebookApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 20:14:13.887 NotebookApp]

    To access the notebook, open this file in a browser:
        file:///C:/Users/bigdata/AppData/Roaming/jupyter/runtime/nbserver-50756-open.html
    Or copy and paste one of these URLs:
        http://localhost:8888/?token=095dc4382e8653f3b39aa2fec60383712ccee6d8a8945052
     or http://127.0.0.1:8888/?token=095dc4382e8653f3b39aa2fec60383712ccee6d8a8945052
```

