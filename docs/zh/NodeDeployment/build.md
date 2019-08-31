<extoc></extoc>
# WaykiChain 节点构建

**请先看普通节点部署操作视频，可以节省您更多时间**
* https://study.163.com/course/introduction/1006498013.htm

## 本地 build 
**For now, support Linux-like 64bit systems, such as Ubuntu 14.x/Ubuntu 16.x/Ubuntu 18.x/Cent OS 7.x only.**

---

### 准备自动化下载编译脚本

脚本内容如下, 脚本请保存为 `build_local.sh` 

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

### 下载编译
运行如下命令执行脚本进行下载源码编译

```sh build_local.sh```

### 检查节点程序是否构建成功

请检查/opt/wicc 路径下是否有**coind** (可执行文件)和 **WaykiChain.conf**文件
* 关于配置文件信息说明，请查看[配置文件说明](./conf.md)

**example**
```
root@ubuntu:/opt/wicc# ls
bin  coind  WaykiChain  WaykiChain.conf
root@ubuntu:/opt/wicc#
```
在 本地`/opt/wicc` 有以上**coind** 和 **WaykiChain.conf** 说明节点程序构建成功了

参考[配置文件说明](./conf.md)并开始部署吧


## 拉取节点程序 for Docker
---

### 从Dockerhub 上拉取官方镜像

```
docker pull wicc/waykicoind
```

### 检查节点镜像是否拉取成功
```
docker images
```

**example**
```
~/workspace/wicc$docker images
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
wicc/waykicoind       latest              b18a36f2f574        2 days ago          623MB
```
如上则说明拉取成功，参考[配置文件说明](./conf.md)并开始部署吧

如果想要根据`Dockerfile`构建docker 镜像,请参考 [Dockerfile构建镜像](https://github.com/WaykiChain/WaykiChain/tree/master/Docker)
