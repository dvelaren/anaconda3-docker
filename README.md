# Anaconda 3 Docker Install instructions

Instructions to mount Anaconda3 docker with Ubuntu 18.04

## Installing docker

1. Update Software Repositories

```bash
sudo apt-get update
sudo apt-get upgrade
```

2. Uninstall old versions of Docker

```bash
sudo apt-get remove docker docker-engine docker.io
```

3. Install required packages

```bash
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

4. Add GPG docker key to OS

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add
```

5. Add Docker repository to apt sources

```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
```

6. Install Docker

```bash
sudo apt install docker-ce
```

7. Add your username to Docker group to execute without sudo

```bash
sudo usermod -aG docker ${USER}
```

Log out and then log in to reflect the new user group changes or execute the following command:
```bash
su - ${USER}
```

## Installing Ubuntu+Anaconda3 Docker

1. Pull Ubuntu from Docker hub

```bash
docker pull ubuntu
```

2. Run ubuntu container

```bash
docker run -it ubuntu
```

3. Update all packages inside Ubuntu docker shell

```bash
apt update
apt upgrade
```

4. Install required packages for downloading Anaconda3

```bash
apt install curl
```

5. Download Anaconda3

```bash
cd /tmp
curl -O https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh
```

6. Install Anaconda

```bash
bash Anaconda3-2019.10-Linux-x86_64.sh
```

Follow all the instructions and write yes to all.

7. Activate new bash with conda inside the Docker container

```bash
source ~/.bashrc
```

8. Update all conda packages

```bash
conda update --all
```

9. Create a new directory and a password for jupyter notebooks

```bash
mkdir /home/notebooks
jupyter notebook password
```

Remember the created password to access later the Jupyter notebook.

10. Create a new Docker image based on the recently updated container

Press <kbd>Ctrl</kbd>+<kbd>D</kbd> to close the container, now find the ID of the previously used container:
```bash
docker ps -a
```

Copy the CONTAINERID and execute the following command, assigning a new name to new created image in IMAGENAME
```bash
docker commit CONTAINERID IMAGENAME
```

Wait until it finishes, usually it takes a couple of mins.

11. Run a new container with a desired CONTAINERNAME based on the previously created image IMAGENAME

```bash
docker run -itd --name CONTAINERNAME -p 8888:8888 IMAGENAME /bin/bash -c "/root/anaconda3/bin/jupyter notebook --notebook-dir=/home/notebooks --ip='*' --port=8888 --no-browser --allow-root"
```

12. Navigate to a browser to http://localhost:8888 and put the previously set password

## Starting or stopping the previously used container

To start the previously used container, use:
```bash
docker start CONTAINERNAME
```

To stop it:
```bash
docker stop CONTAINERNAME
```