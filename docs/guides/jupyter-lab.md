#Jupyter Notebook

*Updated on 04Oct2022*

## 0 User Support
If you need support for your application, please use this [link](https://www.recas-bari.it/index.php/en/recas-bari-servizi-en/support-request) to create a ticket with title “ReCaS HPC/GPU: Jupyter Notebook support” and then describe your issue.


**It is STRONGLY advised to subscribe to the recas-hpu-gpu mailing list. Create a ticket with the title “ReCaS HPC/GPU: subscribe to the mailing list”.**

## 1 Introduction
The ReCaS JupyterHub provides Jupyter Lab instances for the ReCaS users. Jupyter Labs are open-source web applications that allows you to create and share documents that contain live code, equations, visualisations and narrative text.
Uses include: data cleaning and transformation, numerical simulation, statistical modelling, data visualisation, machine learning, and much more.

Jupyter Lab served through this service can access your files and directories stored in the ReCaS-Bari storage (based on GPFS distributed file system) and use high performance GPUs to speed up the execution of your application. 

## 2 Installing new Python modules
Addictional lines are needed in the code in order to install Python modules inside the notebook.

Following you can see an example of the installation of `pyarrow`

```bash
import os, sys, site
if not site.getusersitepackages() in sys.path:
    sys.path.append(site.getusersitepackages())

# Installations
!pip install pyarrow 

# Imports
import pyarrow
```
