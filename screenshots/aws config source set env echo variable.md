export POSTGRES_USERNAME=postgres

export POSTGRES_PASSWORD=mypassword

export POSTGRES_HOST=database-1.cu89b3f2cbki.us-east-1.rds.amazonaws.com

export POSTGRES_DB=postgres

export AWS_BUCKET=awsbucketproject3

export AWS_REGION=us-east-1

export AWS_PROFILE=default

export JWT_SECRET=testing

export URL=http://localhost:8100

---

kubectl autoscale deployment <deployment_name> --cpu-percent=70 --min=2 --max=3

---

kubectl apply -f backend-feed-deployment.yml

kubectl apply -f backend-feed-service.yml

kubectl apply -f backend-user-deployment.yml

kubectl apply -f backend-user-service.yml

kubectl apply -f frontend-deployment.yml

kubectl apply -f frontend-service.yml

kubectl apply -f reverseproxy-deployment.yml

kubectl apply -f reverseproxy-service.yml

---

