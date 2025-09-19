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
apiVersion: v1
kind: ConfigMap
metadata:
  name: my-app-config
  namespace: default
data:
  # Example 1: Simple key-value pair
  LOG_LEVEL: "INFO"
  
  # Example 2: Multiline data, like a configuration file
  my-app-config.properties: |
    server.port=8080
    database.url=jdbc:mysql://db-server/mydb
    cache.enabled=true
---
# pod-with-configmap.yaml
apiVersion: v1
kind: Pod
metadata:
  name: my-app-pod
  namespace: default
spec:
  containers:
  - name: my-app-container
    image: busybox
    command: ["/bin/sh", "-c", "env && cat /etc/config/my-app-config.properties" && "sleep 500"]
    
    # Method 1: Inject data as environment variables
    env:
      - name: APP_LOG_LEVEL
        valueFrom:
          configMapKeyRef:
            name: my-app-config
            key: LOG_LEVEL
            
    # Method 2: Mount the ConfigMap as a volume to access data as a file
    volumeMounts:
      - name: config-volume
        mountPath: /etc/config
        readOnly: true
        
  volumes:
    - name: config-volume
      configMap:
        name: my-app-config
```
