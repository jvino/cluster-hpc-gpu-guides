# Docker containers and dockerfile

## 1 Introduction
Docker is an open source project that automates the deployment of software applications inside **containers** by providing an additional layer of abstraction and automation of OS-level virtualization on Linux. A Docker container packages an application with all of its dependencies into a standardized unit for software development.

## 2 Building custom Docker containers in ReCaS-Bari
The ReCaS Bari data center makes available a dedicated machine where to develop containers to be deployed on the HPC/GPU Cluster.

This machine is accessible only to users with a ReCaS-Bari HPC/HTC account active. Users without such an account should register using this [link](https://www.recas-bari.it/index.php/en/recas-bari-servizi-en/richiesta-credenziali-2) (check the box "**Account for access to ReCas-Bari compute services (HTC/HPC)**").

In order to access the machine, first access to the frontend using SSH in a command prompt/shell (Linux, Mac, and Windows ≥10) and insert your password when prompted.

```bash
ssh <username>@frontend.recas.ba.infn.it
```

After that, access the dedicated machine:

```bash
ssh <username>@tesla02.recas.ba.infn.it
```

## 3 Docker most important commands
In this document, the most important aspects of Docker will be covered.

### 3.1 Docker pull
The `pull` command fetches an image from the Docker registry (a place where the docker images are stored and can be downloaded/pulled) and saves it to a host for using it.

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

You can use the `docker images ls` command to list all images on your system.

```bash
docker image ls
```
Output:
```bash
REPOSITORY   TAG   	IMAGE ID   	  CREATED   	  SIZE
ubuntu   	20.04 	7e0aa2d69a15   3 weeks ago   72.7MB
```

### 3.2 Docker run
Now that the `ubuntu:20.04` Docker image is on your host, you are able to run it using the command:

```bash
docker run ubuntu:20.04
```

As you can see, this command does nothing. When you execute `docker run`, the Docker client finds the image (ubuntu:20.04 in this case), loads up the container and then runs a command in that container. When we ran `docker run ubuntu:20.04`, we didn't provide a command, so the container booted up, ran an empty command and then exited.

```bash
docker run ubuntu:20.04 echo "My first command in a container"
```
Output:
```bash
My first command in a container
```

As you can see, an output has been printed.

If you want to execute multiple commands inside the container, it is possibile open a terminal inside it.

```bash
docker run -it ubuntu:20.04 /bin/bash
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
docker ps -a
CONTAINER ID   IMAGE      	COMMAND              	CREATED     	STATUS                   	PORTS 	NAMES
afcac6f85b26   ubuntu:20.04   "bash"               	3 minutes ago   Exited (130) 4 seconds ago         	blissful\_chaum
ee288eeb2a67   ubuntu:20.04   "echo 'My first comm…"   4 minutes ago   Exited (0) 4 minutes ago           	flamboyant\_spence
fac38035f7bc   ubuntu:20.04   "/bin/bash"          	6 minutes ago   Exited (0) 6 minutes ago           	peaceful\_kepler
```

The `-a` flag is used to show all the containers (while the default shows just the ones currently running), so what we see above is a list of all containers that we ran (and not deleted) up to now. Please notice that the STATUS column shows that these containers exited a few minutes ago.

### 3.4 Docker container rm
To remove non-running containers, use this command:

```bash
docker container rm afcac6f85b26 ee288eeb2a67 fac38035f7bc
afcac6f85b26
ee288eeb2a67
fac38035f7bc
```

You can check using `docker ps -a` that the selected container is not on the host anymore

```bash
docker ps -a
CONTAINER ID   IMAGE 	COMMAND   CREATED   STATUS	PORTS 	NAMES
```

If you want remove a container immediately after having executed some commands inside it you can use the `--rm` flag:

```bash
docker run -it --rm ubuntu:20.04 /bin/bash
ls /
bin  boot  dev  etc  home  lib  lib32  lib64  libx32  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
exit

docker ps -a
CONTAINER ID   IMAGE 	COMMAND   CREATED   STATUS	PORTS 	NAMES
```

!!! warning "Periodical clean up of docker images"
    The machine `tesla02.recas.ba.infn.it` is used **ONLY** to test container to be deployed on the HPC/GPU Cluster. Periodically all saved container images will be removed. Be aware of it.

### 3.5 Docker exec
Docker gives the possibility to force a container in the background even if no command is passed.

For example:

```bash
docker run -d -it ubuntu:20.04
048f685e181b2064538d482fa48fc553aa6821b5f51d254e79aca8a5b6069834
```

The `-d` flag tells Docker to DETACH the container and run it in background and to print its ID. Since the container is still running, `docker ps` prints its information

```bash
docker ps
CONTAINER ID   IMAGE      	COMMAND   	CREATED     	STATUS     	PORTS 	NAMES
048f685e181b   ubuntu:20.04   "/bin/bash"   4 seconds ago   Up 3 seconds         	laughing_proskuriakova
```

Using `docker exec` is possible to enter inside the container with a shell:

```bash
docker exec -it 048f685e181b /bin/bash
hostname
048f685e181b
```

### 3.6 Docker port mapping
If inside the container there is a service exposing a specific port, you need to map that port (inside the container) to a port on the host where the container is run. In order to do that, use the `-p` flag in `docker run`:

```bash
docker run -p 11111:12345 python:3 python3 -m http.server 12345
```

The above command executes a http server using the container image `python:3`. The server is linked to the port 12345 **inside** the container. Using -p 11111:12345, you are mapping the port 11111 on the host to the port 12345 of the container. So, in order to access the served web page you need to connect to `tesla02.recas.ba.infn.it:11111` using your browser.

### 4 ReCaS Docker registry

ReCaS provides a dedicated Docker registry for containers to be used on the GPU cluster.

On the `tesla02` machine, login to the docker registry using the following command:

```bash
docker login registry-clustergpu.recas.ba.infn.it
Username: <type your ReCaS-Bari HPC/HTC account username>
Password: <type your ReCaS-Bari HPC/HTC account password>
Login Succeeded
```

The followings commands allow to store an official Docker image from Docker Hub (the official and public Docker registry) to the local ReCaS Docker registry:

```bash
docker pull ubuntu
docker image tag ubuntu registry-clustergpu.recas.ba.infn.it/<your username>/myubuntu:0.1
docker push registry-clustergpu.recas.ba.infn.it/<your username>/myubuntu:0.1
docker pull registry-clustergpu.recas.ba.infn.it/<your username>/myubuntu:0.1
```

!!! warning "Storage quota"
    Each user has a storage quota on the ReCaS-Bari container registry. There is a limit of pushed container images

!!! tip
    Using the same name (such as `myubuntu:0.1`) the pushing docker image will overwrite the existing


## 5 Dockerfile
Using the commands `docker run -d` and `docker exec` is possible to execute a container in detached mode and execute commands inside it, e.g. to create files or install packages. Docker also gives the possibility to save this customized container. Unfortunately this kind of containers can not be deployed in the ReCaS GPU cluster. Dockerfile is the solution for this issue.

A Dockerfile is a simple text file that contains a list of commands that the Docker client calls while creating an image. It's a simple way to automate the image creation process. An advantage is that the commands you write in a Dockerfile are almost identical to their equivalent Linux commands. This makes Dockerfiles easier to write.

Let’s start with an example. The goal is to customize the Docker image `python:3.9.9-slim` in order to make it run with your ReCaS account user, needed in order to access your personal directory on ReCaS GPFS, and your desired python module.

In order to do so, access  the `tesla02.recas.ba.infn.it` machine, create a dedicated directory:

```bash
mkdir /home/<username>/my_python3.9
cd /home/<username>/my_python3.9
```

Use your favorite editor and create a file named `requirements` and copy this content inside.

```bash
pandas
numpy
tables
matplotlib
```

In this file you should put a list of python modules you want to install in the container.

Now, create another file named `Dockerfile` and copy the following content inside it.

```bash
FROM python:3.9.9-slim
# User section (Mandatory)
ENV USERNAME=<your username>
ENV USERID=<your user-id>
ENV GROUPID=<your group-id>

RUN groupadd -g $GROUPID $USERNAME && adduser --disabled-password --gecos '' --uid $USERID --gid $GROUPID $USERNAME

USER $USERNAME
# python modules installation
ADD requirements /home/$USERNAME/requirements
RUN python3 -m pip install -r /home/$USERNAME/requirements
```

Let’s explain the Dockerfile.

`FROM python:3.9.9-slim`

The `FROM` statement defines the Docker image to be used as a starting point. Every Dockerfile starts with the `FROM` statement.

```bash
\# User section (Mandatory)
ENV USERNAME=<your username>
ENV USERID=<your user-id>
ENV GROUPID=<your group-id>
RUN groupadd -g $GROUPID $USERNAME && adduser --disabled-password --gecos '' --uid $USERID --gid $GROUPID $USERNAME
```

The above section is **MANDATORY** and creates your user inside the container in order to access your data on the ReCaS-Bari GPFS file system. (/lustre/home and /lustrehome directories).

If you don’t know your userid and/or groupid, execute the following command on `tesla02.recas.ba.infn.it`:

`id <username>`

And get the numbers, now names/words.

`USER $USERNAME`

The `USER` statement sets the username for any commands that follow it in the Dockerfile.

```bash
ADD requirements /home/$USERNAME/requirements
RUN python3 -m pip install -r /home/$USERNAME/requirements
```

In above lines, the `requirements` file is copied in the container using the `ADD` statement and the python modules listed inside it are installed using pip, through the RUN statement:

Now all files have been written and you are ready to build your customized tensorflow docker image using the following command

`docker build -t registry-clustergpu.recas.ba.infn.it/<username>/mypython3.9:0.1 .`

Finally, upload the image to the docker registry:

`docker push registry-clustergpu.recas.ba.infn.it/<username>/mypython3.9:0.1`

## 6 Use case: use a custom docker image with Chronos
Prefer to this [guide](https://jvino.github.io/cluster-hpc-gpu-guides/job_submission/chronos/) for more details on Chronos.

Create a JSON file in your HOME directory in the ReCaS-Bari Storage (e.g. `/lustrehome/<username>/my-python3.9-job.json`) containing:

```bash
{
  "name": "<username>-my-python3.9-job",
  "command": "python3 /lustrehome/<username>/my_script.py > /lustrehome/<username>/my_script.output",
  "shell": true,
  "retries": 4,
  "description": "",
  "cpus": 1,
  "disk": 0,
  "mem": 1024,
  "gpus": 0,
  "environmentVariables": [],
  "arguments": [],
  "runAsUser": "<username>",
  "owner": "<username>",
  "ownerName": "<username>",
  "container": {
    "type": "mesos",
    "image": "registry-clustergpu.recas.ba.infn.it/<username>/mypython3.9:0.1",
    "volumes": [{"containerPath": "/lustrehome/<username>", "hostPath": "/lustrehome/<username>", "mode": "RW"}]
  },
  "schedule": "R1//P1Y"
}
```

Create a python script in your HOME directory in the ReCaS-Bari Storage (e.g. `/lustrehome/<username>/my_script.py`) containing: 

```bash
import pandas as pd
data = {
    'apples': [3, 2, 0, 1], 
    'oranges': [0, 3, 7, 2]
}
df = pd.DataFrame(data)
print(df.shape)
```

Finally, you can submit the job using the folling command:

```bash
bash submit_chronos /lustrehome/<username>/my-python3.9-job.json
```

Wait few seconds and the file `/lustrehome/<username>/my_script.output` should be in your directory. Watch it using:

` cat /lustrehome/<username>/my_script.output`