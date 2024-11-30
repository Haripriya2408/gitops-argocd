
# Multi-Cluster Deployment Using Argo CD

This project demonstrates the deployment of a sample YAML file into a Kubernetes cluster using Argo CD, leveraging its auto-healing and synchronization capabilities.

## Key Features
1. **Auto-Healing Capability**  
   Any manual changes made directly to the deployed code are reverted automatically by Argo CD.  
2. **Auto-Sync from Git Repository**  
   Changes made to the deployment configuration in the Git repository are automatically synced to the Kubernetes cluster.  
3. **Multi-Cluster Architecture**  
   A central **hub cluster** and two **spoke clusters** are created, allowing centralized management of multiple Kubernetes clusters.

---

## Steps to Set Up and Deploy

### 1. Create Kubernetes Clusters
Run the following command to create the spoke clusters:
```bash
eksctl create cluster --name hub --region ap-southeast-1
eksctl create cluster --name spoke-cluster-1 --region ap-southeast-1
eksctl create cluster --name spoke-cluster-2 --region ap-southeast-1
```

### 2. Install Argo CD
Follow the official [Argo CD documentation](https://argo-cd.readthedocs.io/en/stable/) to install Argo CD in your Kubernetes clusters.

### 3. Verify Argo CD Installation
Check the status of Argo CD pods using:
```bash
kubectl get pods -n argocd
```

### 4. Change the Argo CD Server Service to NodePort
Edit the Argo CD service to use NodePort:
```bash
kubectl edit svc argocd-server -n argocd
```
After making this change, copy the exposed IP address.

### 5. Configure Security Group
Ensure the security group for your EC2 instance allows access to the NodePort IP. This step is critical for accessing the Argo CD web interface.

### 6. Log in to Argo CD
Access the Argo CD UI using the NodePort IP. Log in and configure:
- **Cluster Name**
- **Git Repository Name**
- **Deployment Path**

Sync the repository, and Argo CD will automatically apply the changes to your clusters.

---

## Observing Auto-Sync and Auto-Healing
- **Auto-Sync**: Any updates in the Git repository are automatically reflected in the Kubernetes deployment.
- **Auto-Healing**: Manual changes in the deployment code are reverted to match the configuration in the Git repository.

---

