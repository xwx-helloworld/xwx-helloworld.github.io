## [nfs安装](https://vitux.com/install-nfs-server-and-client-on-ubuntu/)

for server:

* sudo apt-get update
* sudo apt install nfs-kernel-server
* sudo mkdir -p /mnt/sharedfolder
* sudo chown nobody:nogroup /mnt/sharedfolder
* sudo chmod 777 /mnt/sharedfolder
* sudo nano /etc/exports
* /mnt/sharedfolder clientIP(rw,sync,no_subtree_check)
* sudo exportfs -a
* sudo systemctl restart nfs-kernel-server
* sudo ufw allow from [clientIP or clientSubnetIP] to any port nfs
* sudo ufw status

for client:
* sudo apt-get update
* sudo apt-get install nfs-common
* sudo mkdir -p /mnt/sharedfolder_client
* sudo mount serverIP:/exportFolder_server /mnt/mountfolder_client

## [k8s安装](https://www.kubernetes.org.cn/5012.html)
* 修改/etc/hosts，添加ip和计算机名对应关系，如：10.180.2.102 master1
* 生成ssh key，确保本机可以ssh链接，否则部署失败ssh-keygen -t rsa，cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
* 安装ansible，由于要求版本大于2.7，建议从源码安装
`
git clone https://github.com/ansible/ansible.git
cd ./ansible
source ./hacking/env-setup
`
* 用kubespray安装k8s（kubectl，kubeadm，kubelet）
`
git clone https://github.com/kubernetes-sigs/kubespray
cd kubespray
pip install -r requirements.txt
cp -rfp inventory/sample inventory/mycluster
修改配置文件：vim inventory/mycluster/hosts.ini
替换下载源，具体参考链接
ansible-playbook -i inventory/mycluster/hosts.ini cluster.yml -b -v -k
`
* 查看所有pod和网络：kubectl get pods,svc --all-namespace
* 创建namespace：kubectl create namespace cv-service
* 安装nvidia插件，参考[链接] (https://github.com/NVIDIA/k8s-device-plugin) kubectl apply -f nvidia-device-plugin.yml

* 安装服务：main，kps，detect：kubectl apply -f main.yml/kps.yml/detect.yml（修改replicate启动多个服务，指定镜像）

* 查看admin的token：kubectl -n kube-system describe $(kubectl -n kube-system get secret -n kube-system -o name | grep namespace) | grep token

* 访问dashboard查看服务情况：https://10.182.13.213:6443/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/#!/overview?namespace=cv-service
