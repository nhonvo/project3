### 1 eks create

### 2 eks apply kubeconfig

aws eks update-kubeconfig --region us-east-1 --name truongnhoncluster



### 3 check connect

kubectl get nodes



### 5.1 Apply Secrets



kubectl apply -f set-env-configmap.yaml

kubectl apply -f set-env-secret.yaml

kubectl apply -f aws-secret.yaml



### 5.2 Apply Deployments



kubectl apply -f backend-feed-deployment.yaml
kubectl apply -f backend-user-deployment.yaml
kubectl apply -f reverseproxy.yaml

kubectl apply -f 

### 5.4.1 Expose pubReverse

kubectl expose deployment reverseproxy --type=LoadBalancer --name=publicreverseproxy  --port=8080



### 5.4.2 Get pubReverse hosting and then replace hosting of FE/Env files

kubectl get services

### 5.5 Apply late FE 

kubectl apply -f frontend.yaml



### 5.6.1 Expose pubFE

kubectl expose deployment frontend --type=LoadBalancer --name=publicfrontendnodegroup --port=80

### 5.6.2 Get pubFE hosting and replace hosting of env-configmap.yml

kubectl get services

### 5.7 Apply the last config

kubectl apply -f env-configmap.yml	

### 5.8.1 For docker login if needing (then typing password)

docker login --username=henrynguyen17

### 5.8.2 update docker FE to v2

docker build . -t henrynguyen17/udagram-frontend:v2
docker push henrynguyen17/udagram-frontend:v2
kubectl set image deployment frontend frontend=henrynguyen17/udagram-frontend:v2

### 5.9 Travis FE deployment tag is v2 and commit code.

### 6.0 Complete scaling

kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/download/v0.6.4/components.yaml
kubectl autoscale deployment backend-feed --cpu-percent=70 --min=1 --max=10
kubectl autoscale deployment backend-user --cpu-percent=70 --min=1 --max=10
kubectl get hpa
kubectl describe hpa


### Notes:

### 1. if pod has bad status you can rerun

kubectl rollout restart deployment <deployment_name> -n <namespace>

### 2. Delete cluster when finishing

eksctl delete cluster -f setup-cluster.yml --profile Admin

### 3. all pods must be running after applying Services

kubectl get pods

### 4. view deployments if needing

kubectl get deployments

### 5. view pod log if feel something strange

### 5.1 view log: kubectl get <your pod>

### 5.2 force run and view log: kubectl logs -f <your pod>



STT| Command| Description 
-|-|-
1| kubectl get deployment                        |Get all deployment 
2| kubectl get pods                              |Get all pods 
3| kubectl get svc                               |Get all service 
4| kubectl get hpa                               |Get all HPA 
5| kubectl describe deployment                   |Xem chi tiết về 1 deployment 
6| kubectl describe pod      |Xem chi tiết về 1 pod 
6| kubectl describe svc                          |Xem chi tiết về 1 pod 
7| kubectl exec --stdin --tty -- /bin/bash       |SSH vào 1 pod
8| kubectl describe configmap env-config |check in env variables