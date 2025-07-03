#ReCaS JupyterHub with more Resources

*Updated on 03Jul2025*

## 0 User Support
If you need support for your application, please use this [link](https://www.recas-bari.it/index.php/en/recas-bari-servizi-en/support-request) to create a ticket with title “ReCaS HPC/GPU: Jupyter Lab support” and then describe your issue.


**It is STRONGLY advised to subscribe to the recas-hpu-gpu mailing list. Create a ticket with the title “ReCaS HPC/GPU: subscribe to the mailing list”.**

## 1 Introduction
When requesting GPU resources in the ReCaS JupyterHub service, users are provisioned with a modestly-powered virtual GPU. The rationale behind this is twofold: firstly, the majority of analytical workloads do not necessitate dedicated access to an entire GPU. Secondly, for applications requiring substantial computational power, this virtual GPU provides an environment to prototype and validate code against smaller datasets. Subsequent execution can then be offloaded to higher-performance GPUs using the [Job Orchestration service](https://jvino.github.io/cluster-hpc-gpu-guides/job_submission/k8s-jobs/). There are cases that needs more computation resources during the code development. If this is your case, you are in the right place. 

## 2 Service request
More compuration resources are available only for those users with a ReCaS-Bari HPC/HTC account active. Users without such an account MUST register using this [link](https://www.recas-bari.it/index.php/en/recas-bari-servizi-en/richiesta-credenziali-2) (check the box "**Account for access to ReCas-Bari compute services (HTC/HPC)**").

You can check if the registration is successfully completed by access to the `frontend.recas.ba.infn.it` server via ssh:

`ssh <username>@frontend.recas.ba.infn.it`

After that, you can request GPUs using this [link](https://www.recas-bari.it/index.php/en/recas-bari-servizi-en/support-request).

Please provide the following information:

```bash
Title: “ReCaS HPC/GPU: Jupyter Lab instance request”
Issue:
- Name and Surname
- Username
- Email
- number of required CPU
- number and GPU model
- amount of RAM
- other info, if you believe could be useful (like python version)
```

!!! note
    Please identify only the resource you need (we don't have infinite resources for all users!)

After your request is accepted, a confirmation email will arrive. Then, just access the JupyterHub service, and your requested GPU will be listed among the usable resources.

## 3 Important information

Your instance of JupyterLab is executed inside a container and **ONLY** the files stored in your HOME directory in the ReCaS-Bari GPFS file system will be preserved if faults occur, namely /lustrehome. Any local file content or module installation are lost if the container is stopped or crashes. So, use the GPFS file system for all your important files.

**Containers could crash at any time also during the execution of the code**, partial results CAN NOT be restored and will be lost. To manage this situation, consider storing in your HOME directory in GPFS all partial results.

Also consider that you do not have infinite space in the GPFS file system. Use it wisely.

Delete files using Jupyter Lab will create a *.Trash* hidden folder in your HOME directory. To remove completely files, please access using SSH to `frontend.recas.ba.infn.it`. The content of the *.Trash* folder contribute to your quota.
