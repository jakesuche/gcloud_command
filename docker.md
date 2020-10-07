docker run -p 8080:80 nginx:latest 
docker run  -d -p 8080:80 nginx:latest run docker in background
docker cp index.html 54e5cfd03129:usr/share/nginx/html/
docker commit 5e350bdf5398 cad/web:v2
docker tag cad/web:v2 us.gcr.io/cloud-functions-289612/cad-site:v2
docker push us.gcr.io/cloud-functions-289612/cad-site:v2
docker images
gcloud config set compute/zone asia-east1-b
gcloud container clusters create gk-cluster --num-nodes=4
gcloud container clusters get-credentials gk-cluster
kubectl create deployment web-server --image=us.gcr.io/cloud-functions-289612/cad-site:v2
kubectl expose deployment web-server --type LoadBalancer --port 80 --target-port 80
kubectl get services
kubectl get pods
kubectl scale deployment web-server --replicas 20

gcloud container clusters list