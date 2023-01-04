# Service Account
 Whenever a Pod contacts API server, it get authenticated with a service account.
 1. Each namespace in k8s has one default service account.
 2. If you don't specify the service account while creating the Pod, the default service account is automatically assigned to the pod.
 3. Applications which are running inside the Pod can access the API server using attached service account to the Pod.

### List all the service account in current namespace.
```k8s
kubectl get sa
```
