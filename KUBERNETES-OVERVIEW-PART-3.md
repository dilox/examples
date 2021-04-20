- deploy the guestbook app
```
cd guestbook
kubectl apply -f frontend-deployment.yaml
kubectl apply -f frontend-service.yaml
kubectl apply -f redis-master-deployment.yaml
kubectl apply -f redis-master-service.yaml
kubectl apply -f redis-slave-deployment.yaml
kubectl apply -f redis-slave-service.yaml
```
- open the browser


- delete the guestbook app


- deploy wordpress
