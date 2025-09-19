** k0s volume experiment
Setup: 
curl -sSf https://get.k0s.sh | sudo sh 
sudo k0s install controller --single #--single will include the worker node tools 
sudo k0s start # wait a minute 
sudo cat /var/lib/k0s/pki/admin.conf > ~/.kube/config 
kubectl get nodes  
    NAME   STATUS   ROLES    AGE    VERSION  
    k0s    Ready    <none>   4m6s   v1.33.1+k0s  
