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
**configmap**
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
**secrets**  
>Secrets are similar to ConfigMaps but are specifically intended to hold confidential data.
```
kubectl create secret docker-registry secret-tiger-docker \
  --docker-email=tiger@acme.example \
  --docker-username=tiger \
  --docker-password=pass1234 \
  --docker-server=my-registry.example:5000
  
yaml for the above:

kubectl apply -f - << EOF
apiVersion: v1
data:
  .dockerconfigjson: eyJhdXRocyI6eyJteS1yZWdpc3RyeS5leGFtcGxlOjUwMDAiOnsidXNlcm5hbWUiOiJ0aWdlciIsInBhc3N3b3JkIjoicGFzczEyMzQiLCJlbWFpbCI6InRpZ2VyQGFjbWUuZXhhbXBsZSIsImF1dGgiOiJkR2xuWlhJNmNHRnpjekV5TXpRPSJ9fX0=
kind: Secret
metadata:
  name: secret-tigers-docker
  namespace: default
type: kubernetes.io/dockerconfigjson
EOF
```
