# NFS server on Windows with WSL


https://www.youtube.com/watch?v=LoGDkVvR68w

######################
# WSL: NFS Client (remote server on 169.254.96.192 with endpoint /nfsshare)
######################
sudo apt install nfs-common 
sudo mkdir /mnt/nfsshare
sudo mount -t nfs 169.254.96.192:/nfsshare /mnt/nfsshare
######################
# WSL: NFS Server (/nfsshare shared with everyone)
######################
sudo mkdir /nfsshare
sudo chown nobody:nogroup /nfsshare
sudo chmod 777 /nfsshare
sudo sh -c "echo '/nfsshare *(rw,sync,no_subtree_check,insecure)' >> /etc/exports"
sudo service rpcbind start
sudo service nfs-kernel-server start
sudo exportfs -a
######################
# POWERSHELL (as admin): Map ports 443, 2049 to WSL instance on 172.21.124.183
######################
netsh interface portproxy add v4tov4 listenport=443 listenaddress=0.0.0.0 connectport=443 connectaddress=172.21.124.183
netsh interface portproxy add v4tov4 listenport=2049 listenaddress=0.0.0.0 connectport=2049 connectaddress=172.21.124.183




https://www.youtube.com/watch?v=zmDIfJtCKCk
