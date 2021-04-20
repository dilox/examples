- Create GKE cluster

- Configmap example
```
pushd configmap
kubectl apply -f configmap.yaml
kubectl apply -f pod-test.yaml
kubectl logs test
```
- Delete configmap example
```
kubectl delete -f configmap.yaml
kubectl delete -f pod-test.yaml
popd
```
- Deploy the guestbook app
```
pushd guestbook
kubectl apply -f frontend-deployment.yaml
kubectl apply -f frontend-service.yaml
kubectl apply -f redis-master-deployment.yaml
kubectl apply -f redis-master-service.yaml
kubectl apply -f redis-slave-deployment.yaml
kubectl apply -f redis-slave-service.yaml
```
- Get loadBalancer IP
```
kubectl get svc frontend  -o json | jq -r .status.loadBalancer.ingress[].ip
```
- Delete the guestbook app
```
kubectl delete -f frontend-deployment.yaml
kubectl delete -f frontend-service.yaml
kubectl delete -f redis-master-deployment.yaml
kubectl delete -f redis-master-service.yaml
kubectl delete -f redis-slave-deployment.yaml
kubectl delete -f redis-slave-service.yaml
popd guestbook
```
- Deploy wordpress
```
pushd mysql-wordpress-pd
kubectl apply -k ./
```
- Get loadBalacer IP
```
IP=$(kubectl get svc wordpress -o json | jq -r .status.loadBalancer.ingress[].ip)
echo http://$IP
```
- Delete wordpress
```
kubectl delete -k ./
```
