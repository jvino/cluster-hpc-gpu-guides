#RStudio

*Updated on 19Apr2022*	

## 1 Introduction
RStudio is an Integrated Development Environment (IDE) for R, a programming language for statistical computing and graphics. ReCaS-Bari provides RStudio Server that runs on a remote server and allows accessing RStudio using any web browser.

The following figure shows the provided IDE you can access using your web browser.

![rstudio1](images/rstudio1.png)

In the bottom right section you are able to browse graphically your HOME directory located in the ReCaS-Bari GPFS file system.

## 2 Required information

Rstudio on HPC/GPU cluster is available only for those users with a ReCaS-Bari HPC/HTC account active. Users without such an account MUST register using this [link](https://www.recas-bari.it/index.php/en/recas-bari-servizi-en/richiesta-credenziali-2) (check the box "**Account for access to ReCas-Bari compute services (HTC/HPC)**").

You can check if the registration is successfully completed by access to the `frontend.recas.ba.infn.it` server via ssh:

`ssh <username>@frontend.recas.ba.infn.it`

After that, you can request your personal Jupyter Notebook instance using this [link](https://www.recas-bari.it/index.php/en/recas-bari-servizi-en/support-request).

Please provide the following information:

```bash
Title: “ReCaS HPC/GPU: RStudio instance request”
Issue:
- Name and Surname
- Username
- Email
- number of required CPU
- number of required GPU
- amount of RAM
- other info, if you believe could be useful
```

After that you will receive an email to the same address containing your URL to use to access the remote Rstudio Server.

You should see this login web page and use your ReCaS username and password to access the RStudio IDE.

![rstudio@](images/rstudio2.png)

## 3 General information
- Your instance of RStudio is executed inside a container and **ONLY** the files stored in your HOME directory in the ReCaS-Bari GPFS file system will be preserved if faults occur, namely /lustrehome. Any local file content or module installation is lost if the container is stopped or crashes. So, use the GPFS file system for all your important files.

- **Containers could crash at any time also during the execution of the code**, partial results CAN NOT be restored and will be lost. To manage this situation, consider storing in your HOME directory in GPFS all partial results.

- Also consider that you do not have infinite space in the GPFS file system. Use it wisely.

## 4 User Support
If you need support for your application, please use this [link](https://www.recas-bari.it/index.php/en/recas-bari-servizi-en/support-request) to create a ticket with title “ReCaS HPC/GPU: RStudio support” and then describe your issue.

!!! tip
    It is **STRONGLY** advised to subscribe to the recas-hpu-gpu mailing list. Create a ticket with the title “ReCaS HPC/GPU: subscribe to the mailing list”.
