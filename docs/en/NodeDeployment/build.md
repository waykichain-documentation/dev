
<extoc></extoc>

# Bulid WaykiChain Nodes


## build node
**For now, support Linux-like 64bit systems, such as Ubuntu 14.x/Ubuntu 16.x/Ubuntu 18.x/Cent OS 7.x only.**

---

### Prepare an auto download compilation script

The script `build_local.sh`content is as follows. 

```shell
#!/bin/bash
sudo mkdir -p /opt/wicc
cd /opt/wicc

#build waykicoind
sudo curl https://raw.githubusercontent.com/WaykiChain/WaykiChain/master/linuxshell/prepare_prerequisites.sh|bash 
sudo rm -rf WaykiChain
git clone -b release https://github.com/WaykiChain/WaykiChain.git
cd ./WaykiChain/linuxshell
sh linux.sh
cd ..
sh autogen-coin-man.sh coin
make

#copy the coind and WaykiChain.conf
cd ..
cp ./WaykiChain/src/coind coind
cp ./WaykiChain/Docker/WaykiChain.conf WaykiChain.conf

```

### Download and compile
Run the following command to execute the script to download the source code.

```sh build_local.sh```

### Check
* please check if have **coind** (executable file) and **WaykiChain.conf** (config file) is in the directory `/opt/wicc/`
* Refer to [WaykiChain.conf](conf.md) to undetstand Parameter Description

**example**
```
root@ubuntu:/opt/wicc# ls
coind  WaykiChain  WaykiChain.conf
root@ubuntu:/opt/wicc\#
```
Please start to deploy to privatenet , testnet, or mainnet for your development.


## Build image for Docker
---

### Pull image from Dockerhub

```
docker pull wicc/waykicoind
```

### Check
```
docker images
```

**example**
```
\~/workspace/wicc$docker images
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
wicc/waykicoind       latest              b18a36f2f574        2 days ago          623MB
\`\`\`
As above, the image be pulled successfully. Please refer to [Waykichain.conf][1] to deploy it.

Please refer to [Build image by Dockerfile][2] if you want to build docker image by Dockerfile


[1]:	./conf.md
[2]:	https://github.com/WaykiChain/WaykiChain/tree/master/Docker
