# DevOps Project Setup

This repository contains the setup and deployment instructions for a DevOps project using AWS EC2, Kubernetes, Minikube, Nginx Ingress, Ansible, and TLS.

## Table of Contents

1. [AWS EC2 Setup](#aws-ec2-setup)
2. [Prerequisites Installation](#prerequisites-installation)
3. [Kubernetes Cluster with Minikube](#kubernetes-cluster-with-minikube)
4. [Nginx Ingress Setup with Helm](#nginx-ingress-setup-with-helm)
5. [Deploy Sample Application](#deploy-sample-application)
6. [Automate with Ansible](#automate-with-ansible)
7. [TLS Setup with Self-Signed Certificate](#tls-setup-with-self-signed-certificate)
8. [Documentation and GitHub](#documentation-and-github)

## AWS EC2 Setup

### Launch EC2 Instance

1. **Instance Type**: Choose `t3.large`.
2. **AMI**: Use `Ubuntu 20.04`.
3. **Key Pair**: Create or download a key pair for SSH access.
4. **Security Group**: Allow:
   - SSH (Port 22) from your IP.
   - HTTP (Port 80) and HTTPS (Port 443) for web access.
5. **Storage**: Default 8GB is sufficient but can be increased if needed.

## SSH into EC2
`ssh -i your-key.pem ubuntu@<your-ec2-public-ip> `


## Prerequisites Installation 

1. **Update System**: `sudo apt update && sudo apt upgrade -y` 

2. **Install Docker** `sudo apt install docker.io -y sudo systemctl start docker sudo systemctl enable docker` 

3. **Istall Minikube**: `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 
sudo install minikube-linux-amd64 /usr/local/bin/minikube` 

4. **Install Kubectl**: `sudo snap install kubectl --classic` 

 5. **Install Helm**: `curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash` 

## Kubernetes Cluster with Minikube

1. **Start Minikube**: `minikube start --driver=docker` 

2. **Verify Setup**: `kubectl get nodes` 

## Nginx Ingress Setup with Helm 

1. **Add Helm Repo and Install Nginx Ingress** `helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx helm repo update helm install nginx-ingress ingress-nginx/ingress-nginx` 

2. **Verify Nginx Ingress Controller**: `kubectl get pods -n default -l app.kubernetes.io/name=ingress-nginx` 

## Deploy Sample Application  

 1. **Create Deployment YAML (`deployment.yaml`) yaml**:

 2. **Apply Deployment**: `kubectl apply -f deployment.yaml` 

 3. **Create Ingress YAML (`ingress.yaml`) yaml**:'

 4. **Apply Ingress**:`kubectl apply -f ingress.yaml` 

## Automate with Ansible 

 1. **Install Ansible**: `sudo apt install ansible -y` 

 2. **Write Playbook (`playbook.yml`) yaml**: 

 3. **Run Playbook**:`ansible-playbook playbook.yml` 

## TLS Setup with Self-Signed Certificate  

 1. **Generate a private key**: `openssl genrsa -out tls.key 2048`
 2. **Genrate tls Certificate**:`openssl req -new -x509 -key tls.key -out tls.crt -days 365 -subj "/CN=hello-world.local"` This command creates a certificate valid for 365 days and specifies the hello-world.local domain as the Common Name (CN).
### Create a Kubernetes Secret to Store the TLS Certificate
 1. **Create a secret with the TLS key and certificate**:`kubectl create secret tls hello-world-tls --key tls.key --cert tls.crt` 
 5. **Changing Ingress YAML (`ingress.yaml`) yaml**: 
 6. **Apply Ingress with TLS**:`kubectl apply -f ingress-tls.yaml` 

### Refrance:
`https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/`
`https://helm.sh/docs/intro/install/`
`https://docs.nginx.com/nginx-ingress-controller/installation/installing-nic/installation-with-helm/`

**Ansible: 
`https://docs.ansible.com/ansible/4/collections/community/kubernetes/k8s_module.html`
`https://docs.ansible.com/ansible/2.9/modules/helm_module.html`







