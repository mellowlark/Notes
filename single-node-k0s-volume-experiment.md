### k0s volume experiment
**Enviroment**  
WSL Ubuntu Ubuntu 24.04.3 LTS  
  
**Setup:**  
curl -sSf https://get.k0s.sh | sudo sh  
sudo k0s install controller --single #--single will include the worker node tools  
sudo k0s start # wait a minute  
Install kubectl (optional to run kubectl directly)  
>curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"  
>sudo install kubectl /usr/local/bin/  
>rm ./kubectl  
>sudo cat /var/lib/k0s/pki/admin.conf > ~/.kube/config  

kubectl get nodes  
>NAME   STATUS   ROLES    AGE    VERSION  
>k0s    Ready    <none>   4m6s   v1.33.1+k0s  

**Onwards and Upwards**  
**Creating Volumes**  
configmap
> An API object used to store non-confidential data in key-value pairs

```
test code
```
