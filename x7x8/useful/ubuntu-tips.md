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