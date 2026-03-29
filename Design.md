☁️ AWS CLI Commands (with detailed explanations)
🔐 Configure AWS CLI
# Configures AWS CLI with your credentials (Access Key, Secret Key, region, output format)
# This allows your local machine or VM to authenticate and interact with AWS services securely
aws configure
🔍 Verify AWS Identity
# Confirms that AWS CLI is correctly authenticated by returning your account ID and IAM user/role
# Useful for debugging credential issues
aws sts get-caller-identity
📦 Create ECR Repository
# Creates a private container image repository in Amazon ECR
# This repository will store your Docker images in AWS
aws ecr create-repository --repository-name fastapi-app --region us-east-1
🔐 Login to ECR
# Authenticates Docker with Amazon ECR using a temporary token
# Required before pushing images to your private ECR repository
aws ecr get-login-password --region us-east-1 \
| docker login --username AWS --password-stdin <account-id>.dkr.ecr.us-east-1.amazonaws.com
🧹 Delete ECR Repository (optional cleanup)
# Deletes the ECR repository and all images inside it
# Helps avoid storage costs if you no longer need the images
aws ecr delete-repository --repository-name fastapi-app --force --region us-east-1
☸️ Update kubeconfig for EKS
# Updates your local kubeconfig file so kubectl can connect to your EKS cluster
# This is required before running any Kubernetes commands against EKS
aws eks update-kubeconfig --region us-east-1 --name fastapi-cluster-small
☸️ Kubernetes (kubectl) Commands
🔍 Check Cluster Nodes
# Lists all worker nodes in the Kubernetes cluster
# Confirms that your EKS cluster is up and accessible
kubectl get nodes
📦 Apply Kubernetes Configuration
# Applies the configuration defined in a YAML file (Deployment, Service, etc.)
# Creates or updates resources in the cluster declaratively
kubectl apply -f k8s/deployment.yaml
🔍 View Pods
# Lists all running pods (containers) in the cluster
# Used to verify if your application is deployed and running
kubectl get pods
👀 Watch Pods in Real-Time
# Continuously watches pod status changes (useful during deployments/rollouts)
# Shows new pods starting and old ones terminating
kubectl get pods -w
🔍 View Services
# Lists all services, including LoadBalancer and their external IPs
# Used to find the public endpoint of your application
kubectl get services
📊 Check Deployment Status
# Shows the status of your deployment (replicas, availability, etc.)
kubectl get deployments
🚀 Check Rollout Progress
# Monitors the rollout of a deployment to ensure it completes successfully
# Useful during CI/CD automated deployments
kubectl rollout status deployment/fastapi-deployment
🔁 Restart Deployment
# Forces a rolling restart of all pods in the deployment
# Useful when changes are not picked up or to refresh the application
kubectl rollout restart deployment fastapi-deployment
🐞 View Logs
# Displays logs from the running application pods
# Helps debug crashes, startup issues, or runtime errors
kubectl logs deployment/fastapi-deployment
🔍 Describe Resource (Deep Debug)
# Provides detailed information about a resource (pod, service, etc.)
# Includes events, errors, and configuration details
kubectl describe pod <pod-name>
🌐 Expose Deployment (Manual Service Creation)
# Creates a service to expose a deployment externally
# Used earlier before defining Service in YAML
kubectl expose deployment fastapi-deployment \
  --type=LoadBalancer \
  --port=80 \
  --target-port=80
🔌 Port Forward (Local Testing)
# Forwards a port from your local machine to a service inside the cluster
# Useful for debugging without needing a public LoadBalancer
kubectl port-forward service/fastapi-service 8080:80
🧠 Bonus — EKS Cluster Management (eksctl)
🚀 Create Cluster
# Creates a fully managed Kubernetes cluster on AWS (EKS)
# Includes networking, control plane, and worker nodes
eksctl create cluster \
  --name fastapi-cluster-small \
  --region us-east-1 \
  --node-type t3.small \
  --nodes 1
🧹 Delete Cluster
# Deletes the entire EKS cluster and all associated resources
# Important to avoid ongoing AWS charges
eksctl delete cluster --name fastapi-cluster-small --region us-east-1
🔍 List Clusters
# Lists all EKS clusters in your account/region
eksctl get cluster
🎯 Final takeaway

You now have hands-on experience with:

Amazon ECR (image registry)
Amazon EKS (Kubernetes)
AWS CLI + kubectl workflows
Real-world deployment lifecycle

If you want, I can turn this into a cheat sheet PDF or a GitHub README section for your project 👍