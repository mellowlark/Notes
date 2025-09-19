apt-get install nfs-kernel-server  
sudo systemctl start nfs-kernel-server.service  
mkdir /folder/to/be/mounted  
chmod 777 /folder/to/be/mounted  
chown nobody:nogroup /folder/to/be/mounted  
vi /etc/exports  
/folder/to/be/mounted     <ip for host leveraging the mount or * for any host>(rw,no_subtree_check,no_root_squash)  
sudo exportfs -a  
