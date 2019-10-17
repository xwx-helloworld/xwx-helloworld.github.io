# 日常工作中遇到的问题汇总

## 删除所有含有vscode的进程
- ps -ef | grep 'vscode' | awk '{print $2}' | xargs kill -9

##代码运行的时候不生成__pycache__

- 在程序执行开始的地方加入下面代码或者设置环境变量 export PYTHONDONTWRITEBYTECODE=1
- import sys; sys.dont_write_bytecode = True

## 使用GPU进程查询

当nvidia-smi命令输出的结果不够满意时，比如已经没有进程占用某个GPU，但是某个GPU显存仍然是被占用的。

这个时候使用该命令，可以查看GPU被使用情况。
```bash
sudo fuser -v /dev/nvidia*
```

## 关于Profile（cProfile）

- python -m cProfile -o result.out -s cumulative step.py  //性能分析, 分析结果保存到 result.out 文件；
- python gprof2dot.py -f pstats result.out | dot -Tpng -o result.png   //gprof2dot 将 result.out 转换为 dot 格式；再由 graphvix 转换为 png 图形格式。

## json dump error(【Python】TypeError: Object of type 'int64' is not JSON serializable (或者float32))
```python
class NpEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, np.integer):
            return int(obj)
        elif isinstance(obj, np.floating):
            return float(obj)
        elif isinstance(obj, np.ndarray):
            return obj.tolist()
        else:
            return super(NpEncoder, self).default(obj)
```

## 服务器安装显卡驱动，禁用nouveau
problem: the nouveau kernel driver is currently in use by your system
(reference：https://tutorials.technology/tutorials/85-How-to-remove-Nouveau-kernel-driver-Nvidia-install-error.html )
* sudo apt-get remove nvidia* && sudo apt autoremove
* sudo apt-get install dkms build-essential linux-headers-generic
* sudo vim /etc/modprobe.d/blacklist.conf and append the follow lines to blacklist.conf
```
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
```
* echo options nouveau modeset=0 | sudo tee -a /etc/modprobe.d/nouveau-kms.conf
* sudo update-initramfs -u
* reboot

## 源码编译opencv, opencv-contrib, opencv-python, opencv-contrib-python
problem: 源码编译python版本、contrib版本
* git clone https://github.com/opencv/opencv.git  && git checkout 3.4
* git clone https://github.com/opencv/opencv_contrib.git  && git checkout 3.4
* cd opencv && mkdir build && cd build 
* cmake -D OPENCV_ENABLE_NONFREE=ON -D CMAKE_BUILD_TYPE=RELEASE -D PYTHON_DEFAULT_EXECUTABLE=/usr/bin/python3 -D BUILD_opencv_python3=ON -D BUILD_opencv_python2=OFF \
<<<<<<< HEAD
  -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=ON -D BUILD_NEW_PYTHON_SUPPORT=ON -D WITH_V4L=OFF -D WITH_QT=OFF -D WITH_OPENGL=OFF \
  -DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules ..
=======
-D CMAKE_BUILD_TYPE=RELEASE -D PYTHON3_LIBRARY=/home/ahs/anaconda3/lib/libpython3.7m.so.1 -D CMAKE_INSTALL_PREFIX=/usr/local -D WITH_TBB=ON -D BUILD_NEW_PYTHON_SUPPORT=ON \
-D WITH_V4L=OFF -D WITH_QT=OFF -D WITH_OPENGL=OFF -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules ..
>>>>>>> f9f50d376c7703c160ed547bf83bc2ae15772b07

## 服务器多张显卡连接方式查看
* nvidia-smi topo -m
*

## 服务器挂在4T硬盘
problem: 无法挂在超过2T硬盘

(reference: https://www.tecmint.com/add-disk-larger-than-2tb-to-an-existing-linux/)

ps: the following command should be running with root.
* fdisk -l to get disk name, eg. /dev/sdb, /dev/sdb1
* fdisk /dev/sdb, d, w
* parted /dev/sdb, mklabel gpt, print, mkpart extend 0KB 4001GB
* mkfs.ext4 /dev/sdb1
* mount /dev/sdb1 /data

For permanent mounting add the entry in /etc/fstab file.

/dev/sdb1    /data      ext4      defaults  0   0

## 指定模型运行在某一张显卡上
* 不要这么做：os.environ['CUDA_VISIBLE_DEVICES'] = gpu_id
* 请这么做  ：self.net = DataParallel(self.net, device_ids=[int(gpu_id)])

## 任务定时执行

* sudo crontab -e
* 最后一行加入要执行的命令（注意相对路径和绝对路径的关系）
* 每天10:30定时执行： 30 10 * * * cd /data/ali-oss && python cnt.py >/dev/null 2>&1
 

## linux配置samba服务器windows访问

* sudo apt-get install samba, sudo apt-get install smbclient # Linux客户端测试用
* sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
* sudo vim /etc/samba/smb.conf
```
===== 文件内容, 在smb.conf最后添加： =====
[share]
    path = /home/share
    browseable = yes
    writable = yes
    comment = smb share test
===== 结束修改, 保存退出vim =====
```
* sudo smbpasswd -a $USER
* sudo service smbd restart
* smbclient -L //localhost/share


## ubuntu1804 静态ip方式

* 方法一：vim /etc/network/interfaces，sudo /etc/init.d/networking restart 
```
auto lo
iface lo inet loopback
auto eth0
iface eth0 inet static          # 使用静态地址
address  192.168.0.100          # 设置静态地址
netmask  255.255.255.0
gateway  192.168.0.1            # 网关
dns-nameservers   8.8.8.8  192.168.0.1 
```

* 方法二（推荐）：vim /etc/netplan/50-cloud-init.yaml， sudo netplan apply
```
network:
    ethernets:
        eno1:
            dhcp4: no
            dhcp6: no
            addresses: [10.180.2.101/24]
            gateway4: 10.180.2.30
            nameservers:
                    addresses: [10.180.2.55]
    version: 2
```

## anaconda release archive address
* https://repo.continuum.io/archive/

## 修改文件和文件夹的用户组和读写权限
* sudo chgrp -R ahs data
* sudo chown -R ahs data


## 获取文件名（不含路径和后缀）
base_name = os.path.splitext(os.path.basename(img_path))[0]
