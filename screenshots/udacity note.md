set up database postgres: version < 13.7

mở port inbound out bound

---

Creating an EKS (Amazon Elastic Kubernetes Service) cluster and deploying applications on Kubernetes can be a complex task. Below is a step-by-step tutorial based on the provided instructions for creating an EKS cluster and deploying applications to Kubernetes. This tutorial assumes you already have an AWS account set up.

**Step 1: Check kubectl Utility**

Before you start, make sure you have the `kubectl` utility installed locally. Run the following command to check its version:

```
bashCopy code
kubectl version --short
```

This should return a response with a Client Version if it's successfully installed.

**Step 2: Create an EKS Cluster**

In this step, you'll create an EKS cluster using the AWS Management Console. This involves setting up the necessary IAM roles and VPC.

- Follow the instructions provided by AWS for Creating an Amazon EKS Cluster using the AWS Management Console.
- Make sure to create an AWS role for EKS, attaching the policies for `AmazonEKSClusterPolicy`, `AmazonEC2ContainerServiceFullAccess`, and `AmazonEKSServicePolicy`.
- If you don't have a VPC, create one with the IPv4 CIDR block value `10.0.0.0/16` (no IPv6 CIDR block).
- Set your cluster endpoint access to Public.
- It might take approximately 20 minutes for your cluster to be created. Once ready, it should have an Active status.

**Step 3: Create EKS Node Groups**

Now that your EKS cluster is created, you need to add Node Groups to it. Node Groups provide the EC2 instances that will process your workloads.

- Follow the instructions provided by AWS for Creating a Managed Node Group using the AWS Management Console.
- When creating the Node IAM Role, attach the policies for `AmazonEKSWorkerNodePolicy`, `AmazonEC2ContainerRegistryReadOnly`, and `AmazonEKS_CNI_Policy`.
- Recommended: Use `m5.large` instance types.
- Set a minimum of 2 nodes and a maximum of 3 nodes.

**Step 4: Connect kubectl with EKS**

To interact with your EKS cluster using `kubectl`, follow these instructions:

- Follow the instructions provided by AWS on Creating a kubeconfig for Amazon EKS to configure `kubectl` to communicate with your newly-created EKS cluster.
- After configuration, you can validate the connection to the cluster by running:

```

kubectl get nodes
```

This should return information about the nodes in your EKS cluster.

**Step 5: Deployment**

In this step, you will deploy Docker containers for the frontend web application and backend API applications in their respective pods on your EKS cluster.

**ConfigMap: Create `env-configmap.yaml`**

Create an `env-configmap.yaml` file and store all your configuration values (non-confidential environment variables) in that file.

**Secrets: Create `env-secret.yaml`**

Create an `env-secret.yaml` file to store confidential values, such as login credentials. Do not store PostgreSQL usernames and passwords in the `env-configmap.yaml` file.

**Secrets: Create `aws-secret.yaml`**

Create an `aws-secret.yaml` file to store your AWS login credentials. Replace `___INSERT_AWS_CREDENTIALS_FILE__BASE64____` with the Base64 encoded AWS credentials. To obtain the Base64-encoded credentials, you can follow the instructions for your platform (Mac/Linux or Windows) as mentioned in the original instructions.

**Deployment Configuration:**

Create a `deployment.yaml` file individually for each service (frontend and backend). Specify the Docker images you pushed to Dockerhub earlier in the deployment files.

**Service Configuration:**

Similarly, create a `service.yaml` file for each service, defining the correct services/ports mapping.

Once all deployment and service files are ready, apply them using the following commands:

```bash
kubectl apply -f aws-secret.yaml
kubectl apply -f env-secret.yaml
kubectl apply -f env-configmap.yaml
kubectl apply -f backend-feed-deployment.yaml
# Repeat for other deployment files
kubectl apply -f backend-feed-service.yaml
# Repeat for other service files
```

Make sure the image names in the deployment files match the ones in your Dockerhub repository.

**Step 6: Connect to Kubernetes Services**

If the deployment is successful, you can access the application using one of the following methods:

**Port Forwarding:**

You can forward a local port to a port on the "frontend" pod.

**Expose External IP:**

You can expose the "frontend" deployment using a Load Balancer's External IP. This is explained in detail in the provided instructions.

**Step 7: Update Environment Variables and Re-Deploy the Application**

After both the services (frontend and reverseproxy) have an External IP, update the API endpoints in your application's configuration files:

- Update `udagram-frontend/src/environments/environment.ts` with the External IP of the reverse proxy deployment.
- Build a new frontend image, tag it with a new version, and push it to Dockerhub.
- Update the image tag in the `frontend-deployment.yaml` file.
- Apply the changes to Kubernetes using `kubectl`.

**Step 8: Verify Deployment**

Check the deployed application at the External IP of your `publicfrontend` service.

**Step 9: Troubleshoot**

If you encounter any issues, follow the troubleshooting instructions provided in the original document.

**Step 10: Screenshots**

Take screenshots of the following to verify your project:

- Kubernetes pods are deployed properly: `kubectl get pods`
- Kubernetes services are set up properly: `kubectl describe services`
- You have horizontal scaling set against CPU usage: `kubectl describe hpa`

With this step-by-step guide, you should be able to create an EKS cluster and deploy applications on Kubernetes as described in the original document. Please adapt the steps as needed for your specific project and requirements.
