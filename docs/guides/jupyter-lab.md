#Jupyter Lab Guide

*Updated on 12Dic2022*

## 0 User Support
If you need support for your application, please use this [link](https://www.recas-bari.it/index.php/en/recas-bari-servizi-en/support-request) to create a ticket with title “ReCaS HPC/GPU: Jupyter Lab support” and then describe your issue.


**It is STRONGLY advised to subscribe to the recas-hpu-gpu mailing list. Create a ticket with the title “ReCaS HPC/GPU: subscribe to the mailing list”.**

## 1 Introduction
Jupyter Labs are open-source web applications that allows you to create and share documents that contain live code, equations, visualizations and narrative text.
Uses include: data cleaning and transformation, numerical simulation, statistical modelling, data visualization, machine learning, and much more.

The ReCaS Jupyter Lab can access your files and directories stored in the ReCaS-Bari storage (based on GPFS distributed file system) and use high performance GPUs to speed up the execution of your application. 

The ReCaS Jupyter Lab instances are based on the [RAPIDS Docker container](https://hub.docker.com/r/rapidsai/rapidsai/).

## 2 Installing new Python modules
`pip` can be used to install new Python packages. 

Specific lines can be put inside the code, like followings:

```python
# Installation
! pip install graphviz

# Import 
import graphviz
```

!!! warning 
	Remember to put a `!` before the `pip install` command. 

Alternately you can use also `conda` (environment named `rapids`) and install packages using the Terminal (you can access to it by opening a new Lancher and selecting `Terminal`). 

The following lines show how to install `mysql` package:

```bash
conda activate rapids
conda install -y mysql
```

After the installation, return in the notebook and type  `import mysql`.

!!! note
    Consider `pip` as first option.

## 3 Upload file from local file system

To Upload files from your local file system to the JupyterLab workspace, press the button shown in the following figure and select the files to would like to upload.

![jupyterlab-upload-file](images/jupyterlab-upload-file.png)

## 4 Enable Resource Usage Dashboards

The ReCaS Jupyter Lab gives you the possibility to monitor the resource usage of your application in real-time. 

Few simple steps are needed to build your personal dashboard.

First, click in the third tab on the left, named "System Dashboards", as shown in the following figure.

![jupyterlab-monitor-resources-1](images/jupyterlab-monitor-resources-1.png)

Second, double click on the favorite blue button and it will be places as a tab, near the notebook one.
Grab and move it and select your favorite position.

In the following figure it is shown the "Machine Resources" plots.

![jupyterlab-monitor-resources-2](images/jupyterlab-monitor-resources-2.png)

Additional plots can be put on the screen.

In the following figure it is shown also the "GPU Resources" plots.

![jupyterlab-monitor-resources-3](images/jupyterlab-monitor-resources-3.png)

## 5 Dask

Dask is a flexible open-source Python library for parallel computing. Dask scales Python code from multi-core local machines to large distributed clusters in the cloud. Dask provides a familiar user interface by mirroring the APIs of other libraries in the PyData ecosystem including: `Pandas`, `Scikit-learn` and `NumPy`. It also exposes low-level APIs that help programmers run custom algorithms in parallel. (Wikipedia)

Dask gives the possibility to split the workload among multiple workers. 
So the first step concerns the creation of a cluster. 
Workers can be executed on different machines or on the same machine.

### 5.1 Start a new Dask cluster

The easiest way to create a Dask cluster is through the following lines:

```python
from dask.distributed import Client, LocalCluster
cluster = LocalCluster()  # Launches a scheduler and workers locally
client = Client(cluster)  # Connect to distributed cluster and override default
```

After that, computation on Dask DataFrames can be executed. 

In the following line is shown the evaluation of the sum of the column `x` of the dataframe `df`.

```python
df.x.sum().compute()  # This now runs on the distributed system
```

It is recommended to read the [official Dask guide](https://distributed.dask.org/en/stable/quickstart.html) to learn all provided capabilities.

### 5.2 Enable Dask Resource Usage Dashboards

Dash provides additional graphical objects to monitor in real-time application information like resource usage and application progress.

Firstly, the creation of a Dask cluster through the Jupyter Lab interface. 

This is done by selecting the forth tabs on the left, the `Dask Tab`, and create a new cluster by clicking on the `NEW` button, as shown in the following figure.

![jupyterlab-monitor-dask-1](images/jupyterlab-monitor-dask-1.png)

Secondly, the importing of the created cluster in the code.

This is easily done by clicking the `< >` button on the bottom left after having selected the cell inside the notebook, as shown in the following figure.

![jupyterlab-monitor-dask-2](images/jupyterlab-monitor-dask-2.png)

Following it is copied the generated code for convenience.

```python
from dask.distributed import Client

client = Client("tcp://127.0.0.1:44359")
client

```

Multiple Dask graphical objects can be enabled and moved inside the window.

If the code exploit Dask objects, during its execution the information about the application (resources usage, application progress, ...) are shown in real-time, as shown in the following figure.

![jupyterlab-monitor-dask-3](images/jupyterlab-monitor-dask-3.png)

## 6 RAPIDS

The RAPIDS suite of open source software libraries gives you the freedom to execute end-to-end data science and analytics pipelines entirely on GPUs. 

Seamlessly scale from GPU workstations to multi-GPU servers and multi-node clusters with Dask.

Accelerate your Python data science toolchain with minimal code changes and no new tools to learn.

RAPIDS is an open source project. Supported by NVIDIA, it also relies on Numba, Apache Arrow, and many more open source projects. 

[Reference](https://rapids.ai/)

## 7 Useful links

[Dask Webpage](https://www.dask.org/)

[Dask GitHub page](https://github.com/dask/dask)

[RAPIDS Webpage](https://rapids.ai/)

[RAPIDS Docker container Webpage](https://hub.docker.com/r/rapidsai/rapidsai/)