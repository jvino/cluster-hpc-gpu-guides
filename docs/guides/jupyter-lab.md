#Jupyter Lab Guide

*Updated on 06Dic2022*

## 0 User Support
If you need support for your application, please use this [link](https://www.recas-bari.it/index.php/en/recas-bari-servizi-en/support-request) to create a ticket with title “ReCaS HPC/GPU: Jupyter Notebook support” and then describe your issue.


**It is STRONGLY advised to subscribe to the recas-hpu-gpu mailing list. Create a ticket with the title “ReCaS HPC/GPU: subscribe to the mailing list”.**

## 1 Introduction
The ReCaS JupyterHub provides Jupyter Lab instances for the ReCaS users. Jupyter Labs are open-source web applications that allows you to create and share documents that contain live code, equations, visualisations and narrative text.
Uses include: data cleaning and transformation, numerical simulation, statistical modelling, data visualisation, machine learning, and much more.

Jupyter Lab served through this service can access your files and directories stored in the ReCaS-Bari storage (based on GPFS distributed file system) and use high performance GPUs to speed up the execution of your application. 

## 2 Installing new Python modules
To install new Python packages you can use  `pip` .

!!! warning 
	Remember to put a `!` before the command. 

You can install Python packages through pip also inside the code.

For example:

```bash
# Installation
! pip install graphviz

# Import 
import graphviz
```

Alternately you can use also conda (environment named `rapids`) and install packages using the Terminal, accessible opening a new Lancher and selecting `Terminal`. 

For example:

```bash
conda activate rapids
conda install -y mysql
```

Then return in the notebook and `import mysql`.

When possible, prefer `pip`.

## 3 Upload file from local file system

To Upload files from local file system to the JupyterLab workspace you can use the specific button shown in the following figure.

![jupyterlab-upload-file](images/jupyterlab-upload-file.png)

## 4 Enable Resource Usage Dashboards

It is enable the possibility to monitor in real-time the JupyterLab resources usage.

First of all, click in the third button starting from the top named "System Dashboards", shown in the following figure.

![jupyterlab-monitor-resources-1](images/jupyterlab-monitor-resources-1.png)

Then with a double click on the blue buttons, they will be put on a tab close to the one of the code.

There is the possibility to move it in order to create a separate window.

In the following figure, you can see the "Machine Resources" window put on the right.

![jupyterlab-monitor-resources-2](images/jupyterlab-monitor-resources-2.png)

Additional charts can be put on the screen.

![jupyterlab-monitor-resources-3](images/jupyterlab-monitor-resources-3.png)

## 5 Dask



### 5.1 Enable Dask Resource Usage Dashboards

