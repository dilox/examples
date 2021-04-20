- Create GKE cluster
```
gcloud beta container --project "iconic-access-311309" clusters create "cluster-1" --zone "us-central1-c" --no-enable-basic-auth --cluster-version "1.18.16-gke.502" --release-channel "regular" --machine-type "e2-medium" --image-type "COS" --disk-type "pd-standard" --disk-size "100" --metadata disable-legacy-endpoints=true --scopes "https://www.googleapis.com/auth/devstorage.read_only","https://www.googleapis.com/auth/logging.write","https://www.googleapis.com/auth/monitoring","https://www.googleapis.com/auth/servicecontrol","https://www.googleapis.com/auth/service.management.readonly","https://www.googleapis.com/auth/trace.append" --num-nodes "3" --enable-stackdriver-kubernetes --enable-ip-alias --network "projects/iconic-access-311309/global/networks/default" --subnetwork "projects/iconic-access-311309/regions/us-central1/subnetworks/default" --default-max-pods-per-node "110" --no-enable-master-authorized-networks --addons HorizontalPodAutoscaling,HttpLoadBalancing,GcePersistentDiskCsiDriver --enable-autoupgrade --enable-autorepair --max-surge-upgrade 1 --max-unavailable-upgrade 0 --enable-shielded-nodes --node-locations "us-central1-c"
```
- Connect to the cluster
```
gcloud container clusters get-credentials cluster-1 --zone us-east4-a --project iconic-access-311309
```
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
