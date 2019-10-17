## docker常用命令
* 根据imageid重命名repository名和tag名：docker tag 8750f95ea9f5 ahs-box:base
* 停止容器： docker stop xp
* 删除容器： docker rm xp
* 运行docker：
```
docker run -p 10001:10001 -p 10002:10002 -p 5556:5556 --name=xp --runtime=nvidia -it -v /data/xp/code/lab-server/box-GOD-new:/data -v /data/xp/code/models:/models ahs-box:base bash
```
* 进入某个docker： docker exec -it xp bash

```
docker rm `docker ps -a | grep Exited | awk '{print $1}'`    删除异常停止的docker容器
```

```
docker rmi -f  `docker images | grep '<none>' | awk '{print $3}'`  删除名称或标签为none的镜像
```


## docker 原生环境搭建
* docker-ce安装：
```
    sudo apt update
    sudo apt install apt-transport-https ca-certificates curl software-properties-common
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
    sudo apt update
    apt-cache policy docker-ce
```
* 剩下的步骤
  https://github.com/NVIDIA/nvidia-docker

## docker pycharm remote host
* docker container build 
```
  nvidia-docker run -p 8191:8191 -p 8192:22 --name=syshen -v /data:/data -v /home/ahs/ssy:/workspace -it python3.6 /bin/bash
                       host:container second port mapping must be exist  
```
* 在docker 容器操作
```
  apt update
  apt install openssh-server
  mkdir /var/run/sshd
  echo 'root:passwd' | chpasswd
  sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
  echo "export VISIBLE=now" >> /etc/profile
  service ssh restart
  vim /etc/ssh/sshd_config，添加：PermitRootLogin yes
```
* 在host测试sudo docker port [your_container_name] 22，例如：docker port xp 22
* ssh root@[your_host_ip] -p 8022，例如：ssh root@10.180.2.103 -p 8022
* 测试结束没有报错即可以成功配置
* [参考链接](https://zhuanlan.zhihu.com/p/63426143?utm_source=wechat_session&utm_medium=social&utm_oi=40106358472704)

## docker delete or kill container error
* unknown error after kill: nvidia-container-runtime did not terminate sucessfully
```
  sudo aa-remove-unknown
```

## docker apt update error
GPG error: https://download.docker.com/linux/ubuntu bionic InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 7EA0A9C3F273FCD8
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 7EA0A9C3F273FCD8

## docker私有仓库push和pull
私有仓库pull和push

login：
docker login <host>
pull：
docker pull <host>/<project>/<repo>:<tag>
一个例子：

docker pull dockerhub.xx.net/database/mysql:latest

push：

重新tag：

docker tag <img_name>:<tag> <host>/<project>/<repo>:<tag>

push：

docker push <host>/<project>/<repo>:<tag>