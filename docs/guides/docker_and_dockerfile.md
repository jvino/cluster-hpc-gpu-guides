# Docker containers and dockerfile

## 1 Introduction
Docker is an open-source project that automates the deployment of software applications inside **containers** by providing an additional layer of abstraction and automation of OS-level virtualization on Linux. Docker containers package an application with all of its dependencies into a standardized unit for software development.

## 2 Access to the machine for building custom Docker containers
The ReCaS Bari data center make available a dedicated machine to develop containers to be deployed on the HPC/GPU Cluster.

This machine is accessible only to users with a ReCaS-Bari HPC/HTC account active. Users without such an account should register using this [link](https://www.recas-bari.it/index.php/en/recas-bari-servizi-en/richiesta-credenziali-2) (check the box "**Account for access to ReCas-Bari compute services (HTC/HPC)**").

In order to access the machine, first access to the frontend using the following command in a command prompt/shell (Linux, Mac, and Windows ≥10) and then insert your password when prompted.

```bash
ssh <username>@frontend.recas.ba.infn.it
```

and then access to the dedicated machine:

```bash
ssh <username>@tesla02.recas.ba.infn.it
```

## 3 Docker most important commands
In this document, the most important aspects of Docker will be covered.

### 3.1 Docker pull
The `pull` command fetches an image from the Docker registry (a place where the docker images are stored and can be downloaded/pulled) and saves it to our host in order to use it.

```bash
docker pull ubuntu:20.04
```
Output:
```bash
20.04: Pulling from library/ubuntu
345e3491a907: Pull complete
57671312ef6f: Pull complete
5e9250ddb7d0: Pull complete
Digest: sha256:cf31af331f38d1d7158470e095b132acd126a7180a54f263d386da88eb681d93
Status: Downloaded newer image for ubuntu:20.04
docker.io/library/ubuntu:20.04
```

You can use the `docker images ls` command to see the list of all images on your system.

```bash
docker image ls

REPOSITORY   TAG   	IMAGE ID   	  CREATED   	  SIZE
ubuntu   	20.04 	7e0aa2d69a15   3 weeks ago   72.7MB
```

### 3.2 Docker run
Now that the `ubuntu:20.04` Docker image is on the host, it is possible to run it using the command:

```bash
docker run ubuntu:20.04
```

As you can see, this command does nothing. When you call run, the Docker client finds the image (ubuntu:20.04 in this case), loads up the container and then runs a command in that container. When we ran `docker run ubuntu:20.04`, we didn't provide a command, so the container booted up, ran an empty command and then exited.

```bash
docker run ubuntu:20.04 echo "My first command in a container"

My first command in a container
```

As you can see, an output has been printed.

If you want to execute multiple commands inside the container, it is possibile open a terminal inside it so you can type any commands you want.

```bash
**docker run -it ubuntu:20.04 /bin/bash**
ls /
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
uptime
15:37:57 up  7:20,  0 users,  load average: 0.00, 0.03, 0.05
exit
```

### 3.3 Docker ps
The `docker ps` command shows you all containers that are currently running.

```bash
docker ps

CONTAINER ID   IMAGE 	COMMAND   CREATED   STATUS	PORTS 	NAMES
```

Since no containers are running, we see a blank line. Let's try a more useful variant:

```bash
**docker ps -a**
CONTAINER ID   IMAGE      	COMMAND              	CREATED     	STATUS                   	PORTS 	NAMES
afcac6f85b26   ubuntu:20.04   "bash"               	3 minutes ago   Exited (130) 4 seconds ago         	blissful\_chaum
ee288eeb2a67   ubuntu:20.04   "echo 'My first comm…"   4 minutes ago   Exited (0) 4 minutes ago           	flamboyant\_spence
fac38035f7bc   ubuntu:20.04   "/bin/bash"          	6 minutes ago   Exited (0) 6 minutes ago           	peaceful\_kepler
```

The `-a` flag is used to show all the containers (while the default shows just the ones currently running), so what we see above is a list of all containers that we ran (and not deleted) up to now. Please notice that the STATUS column shows that these containers exited a few minutes ago.

### 3.4 Docker container rm
To remove a non-running container, use this command:

```bash
**docker container rm afcac6f85b26 ee288eeb2a67 fac38035f7bc**
afcac6f85b26
ee288eeb2a67
fac38035f7bc
```

You can check using `docker ps -a` that the selected container is not on the host anymore

```bash
**docker ps -a**
CONTAINER ID   IMAGE 	COMMAND   CREATED   STATUS	PORTS 	NAMES
```

If you want remove a container immediately after having executed some commands inside it you can use the --rm flag:

```bash
**docker run -it --rm ubuntu:20.04 /bin/bash**
ls /
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
exit

**docker ps -a**
CONTAINER ID   IMAGE 	COMMAND   CREATED   STATUS	PORTS 	NAMES
```

### 3.5 Docker exec
Docker gives the possibility to force a container in the background even if no command is passed.

For example:

```bash
**docker run -d -it ubuntu:20.04**
048f685e181b2064538d482fa48fc553aa6821b5f51d254e79aca8a5b6069834
```

The `-d` flag tells Docker to DETACH the container and run it in background and to print its ID. Since the container is still running, `docker ps` prints its information

```bash
**docker ps**
CONTAINER ID   IMAGE      	COMMAND   	CREATED     	STATUS     	PORTS 	NAMES
048f685e181b   ubuntu:20.04   "/bin/bash"   4 seconds ago   Up 3 seconds         	laughing\_proskuriakova
```

Using `docker exec` is possible to enter inside the container using a shell:

```bash
**docker exec -it 048f685e181b /bin/bash**
hostname
048f685e181b
```

### 3.6 Docker port mapping
If inside the container there is a service exposing a specific port, you need to map that port (inside the container) to a port on the host where the container is run. In order to do that, use the `-p` flag in `docker run`:

```bash
docker run -p 11111:12345 python:3 python3 -m http.server 12345
```

The above command executes a http server using the container image `python:3`. The server is linked to the port 12345 **inside** the container. Using -p 11111:12345, you are mapping the port 11111 on the host to the port 12345 of the container. So, in order to access the served web page you need to connect to tesla02.recas.ba.infn.it:11111 using your browser.

### 4 ReCaS Docker registry

ReCaS provides a dedicated Docker registry for containers to be used on the GPU cluster.

On the tesla02 machine, login to the docker registry using the following command:

```bash
**docker login registry-clustergpu.recas.ba.infn.it**
Username: <type your ReCaS-Bari HPC/HTC account username>
Password: <type your ReCaS-Bari HPC/HTC account password>
Login Succeeded
```

The followings commands allow to store an official Docker image from Docker Hub (the official and public Docker registry) to the local ReCaS Docker registry:

```bash
docker pull ubuntu
docker image tag ubuntu registry-clustergpu.recas.ba.infn.it/<your username>/myubuntu
docker push registry-clustergpu.recas.ba.infn.it/<your username>/myubuntu
docker pull registry-clustergpu.recas.ba.infn.it/<your username>/myubuntu
```

## 5 Dockerfile
Using the commands `docker run -d` and `docker exec` is possible to execute a container in detached mode and execute commands inside it, e.g. to create files or install packages. Docker also gives the possibility to save this customized container. Unfortunately this kind of container can not be deployed in the ReCaS GPU cluster. Dockerfile is the solution for this issue.

A Dockerfile is a simple text file that contains a list of commands that the Docker client calls while creating an image. It's a simple way to automate the image creation process. An advantage is that the commands you write in a Dockerfile are almost identical to their equivalent Linux commands. This makes Dockerfiles easier to write.

Let’s start with an example. The goal is to customize the Docker image ‘**tensorflow/tensorflow:2.5.0-gpu-jupyter**’ in order to make it run with your ReCaS account user, needed in order to access your personal directory on ReCaS GPFS, and your desired python module.

In order to do so, access  the `tesla02` machine, create a dedicated directory:

```bash
mkdir /home/<username>/mytf-2.5.0
cd /home/<username>/mytf-2.5.0
```

Use your favorite editor and create a file named **requirements** and copy this content inside.

```bash
scikit-learn
pandas
seaborn
matplotlib
scipy
numpy
keras
tensorflow-gpu
six
notebook
```

In this file we put a list of python modules we want to install in the container.

Now, create another file named `Dockerfile` and copy the following content inside it.

```bash
FROM tensorflow/tensorflow:2.5.0-gpu-jupyter
\# User section (Mandatory)
ENV USERNAME=\<your username>
ENV USERID=\<your user-id>
ENV GROUPID=\<your group-id>

RUN groupadd -g $GROUPID $USERNAME && adduser --disabled-password --gecos '' --uid $USERID --gid $GROUPID $USERNAME
RUN apt install python3-pip

USER $USERNAME
\# python modules installation
ADD requirements /home/$USERNAME/requirements
RUN python3 -m pip install -r /home/$USERNAME/requirements
ENV NOT\_DIR="/home/$USERNAME"
ENV ENCRYPTED\_PASS='argon2:$argon2id$v=19$m=10240,t=10,p=8$nNK7QMKyBjri0bVd9IehVg$MhxTUXl0dIQAthxmZP7HUA'
CMD ["sh", "-c", "jupyter notebook --ip=\* --NotebookApp.password=$ENCRYPTED\_PASS --notebook-dir=$NOT\_DIR --no-browser --port=$PORT\_JUPYTER"]
```

Let’s explain the Dockerfile.

The `FROM` statement defines the Docker image to be used as a starting point. Every Dockerfile starts with the ‘FROM’ statement.

```bash
FROM tensorflow/tensorflow:2.5.0-gpu-jupyter
```

This section is **MANDATORY** and creates your user inside the container in order to enable the possibility to access your data on the ReCaS-Bari GPFS file system. (/lustre/home and /lustrehome directories).

```bash
\# User section (Mandatory)
ENV USERNAME=\<your username>
ENV USERID=\<your user-id>
ENV GROUPID=\<your group-id>
RUN groupadd -g $GROUPID $USERNAME && adduser --disabled-password --gecos '' --uid $USERID --gid $GROUPID $USERNAME
```

If you don’t know your userid and/or groupid, execute the following command on **tesla02**:

```bash
id \<username>
```

And get the numbers, now names/words.

The `USER` statement sets the username for any commands that follow it in the Dockerfile.

`USER $USERNAME`

In next lines, the **requirement** file is copied in the container using the ADD statement and the python modules listed inside it are installed using pip, through the RUN statement),

```bash
ADD requirements /home/$USERNAME/requirements
RUN python3 -m pip install -r /home/$USERNAME/requirements
```

In the last section, two environmental variables are set for the notebook directory and encrypted password options. Section 5.1 contains the procedure to create an encrypted password to be used for authentication in Jupyter Notebook. Finally, the CMD statement defines the command to be executed when the Docker container is deployed on a machine.

```bash
ENV NOT\_DIR="/home/$USERNAME"
ENV ENCRYPTED\_PASS='argon2:$argon2id$v=19$m=10240,t=10,p=8$nNK7QMKyBjri0bVd9IehVg$MhxTUXl0dIQAthxmZP7HUA'
CMD ["sh", "-c", "jupyter notebook --ip=\* --NotebookApp.password=$ENCRYPTED\_PASS --notebook-dir=$NOT\_DIR --no-browser --port=$PORT\_JUPYTER"]
```

Now all files have been written and we are ready to build your customized tensorflow docker image using the following command

`docker build -t registry-clustergpu.recas.ba.infn.it/<username>/mytf:0.1 .`

Finally, upload the image to the docker registry:

`docker push registry-clustergpu.recas.ba.infn.it/<username>/mytf:0.1`

### 5.1 Preparing a hashed password
You can prepare a hashed password manually.

Open a shell, install the notebook python module (python3 -m pip install notebook) and type the following lines in a python shell:

```bash
[root@your-machine ~]# python3
\>>> from notebook.auth import passwd
\>>> passwd()
Enter password:
Verify password:

'argon2:$argon2id$v=19$m=10240,t=10,p=8$+Gzqn+ZgyvjrXo9eJTIe3w$z0fzG6RZgSbcSXkCAYb3vw'
```

Finally, save the string 'argon2:$argon2id$v=19$m=10240,t=10,p=8$+Gzqn+ZgyvjrXo9eJTIe3w$z0fzG6RZgSbcSXkCAYb3vw'

And provide it among the required information.

For reference, the [official web page](https://jupyter-notebook.readthedocs.io/en/stable/public_server.html#preparing-a-hashed-password).

## 6 Request
You can request your personal Jupyter Notebook instance using this [link](https://www.recas-bari.it/index.php/en/recas-bari-servizi-en/support-request).
Please provide the following information:
```bash
Title: “ReCaS HPC Jupyter Notebook request - custom docker image”
Issue:

- Name and Surname
- Username
- Email
- Number of required CPU
- Number of required GPU
- Amount of RAM
- Image name\* (E.g. registry-clustergpu.recas.ba.infn.it/fake/mytf:0.1 )
- Other info, if you believe could be useful
```

After having completed the above steps you will receive an email containing the URL needed to access the remote Jupyter Notebook.

Pointing your browser to such URL, you should see the  login page below, where you can use the password you chose when executing the commands described in section 5.1

![](Aspose.Words.fd9b9dee-f664-46a5-a580-27484bb80f3c.001.png)

