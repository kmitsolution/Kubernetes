# Service Account
 Whenever a Pod contacts API server, it get authenticated with a service account.
 1. Each namespace in k8s has one default service account.
 2. If you don't specify the service account while creating the Pod, the default service account is automatically assigned to the pod.
 3. Applications which are running inside the Pod can access the API server using attached service account to the Pod.

### List all the service account in current namespace.
```" 
kubectl get sa
```
<img src="https://github.com/kmitsolution/Kubernetes/blob/main/CKAD/images/01sa.jpg" width=300 height=30/>

### Create a service account
#### a) By kubectl command (creating service account with name my-sa)
```
 kubectl create sa my-sa
```
#### b) By manifest file
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-sa
```
### Secret with service Account
Whenever a service account is created, a secret also get created for service account. This secret has token information of service account.
```
kubectl get secrets
```
<img src="https://github.com/kmitsolution/Kubernetes/blob/main/CKAD/images/02sa.jpg" width=400 height=30/>
#### To find the token information
Describe the secret (it could be different in your case) to find the token information of the service account.
```
kubectl describe secret my-sa-token-xsrgd
```
<img src="https://github.com/kmitsolution/Kubernetes/blob/main/CKAD/images/03sa.jpg" width=700 height=200/>

