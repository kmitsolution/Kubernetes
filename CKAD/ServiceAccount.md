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

#### To connect with API server using SA
1. Find the token of SA's secret
2. Decode base64 to this token (echo <<Token>> | base64 -d)
3. Find the cluster url from "kubectl config view command"
4. Run curl command to access the api server with service account (curl  https://172.16.16.100:6443/api --insecure --header "Authorization: Bearer <<decoded token>>)
5. You will find the SA is able to communicate with API server. It means that if you create a Pod with this service account then it has access to API server. 
 
### Permission to SA using Role(namespace level) and Rolebinding
Create a role which will have ReadOnly Permission to pod in default namespace
#### By Command
```
 kubectl create role myacc --verb=get --verb=list --verb=watch --resource=pods
```
### Using Manifest file
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  creationTimestamp: null
  name: myacc
rules:
- apiGroups:
  - ""
  resources:
  - pods
  verbs:
  - get
  - list
  - watch
```
Now Create the RoleBinding to bind the SA with myacc role
#### By Command
``` 
 kubectl create rolebinding readRoleBinding --serviceaccount='default:my-sa' --role='myacc'
```
#### By Manifest file
```
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  creationTimestamp: null
  name: readRoleBinding
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: myacc
subjects:
- kind: ServiceAccount
  name: my-sa
  namespace: default
```


