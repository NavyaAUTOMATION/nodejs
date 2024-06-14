# Service Deployment -
 This repository contains the necessary code and configuration files to deploy a simple Node JS HTTP service displaying a text using Docker, GitHub Repo, Amazon EKS using terraform, and Jenkins for CI/CD.

# Prerequisites

1. EC2 Instance 
2. AWS CLI configured with appropriate permissions.
3. Docker installed in EC2 instance.
4. EKS Cluster Set up using Terraform IaC.
5. kubectl installed in EC2 instance..
6. Jenkins installed and configured with Docker and Kubernetes plugins.
7. AWS IAM roles and policies for EKS.

# Step 1: Create a Simple Node.js Application

>>>Create a directory for your project
  mkdir nodejs
  cd nodejs
>>>Create the application file (server.js)
>>>Create a Dockerfile

# Step 2: Push the Code to GitHub

>>>Initialize a git repository
  git init
  git add .
  git commit -m "Initial commit"
>>>Create a new repository on GitHub and push the code
  git remote add origin https://github.com/NavyaAUTOMATION/nodejs.git
  git push -u origin main

# Step 3: Create the EKS Cluster Using Terraform

>>>Install Terraform
>>>Create a new directory for your Terraform configuration
    mkdir terraform infra
    cd terraform infra
>>>Create a eks.tf, provider.tf,vpc.tf files
>>>Initialize Terraform and apply the configuration
    terraform init
    terraform apply
>>>terraform state file will be generated

# Step4: Setup AWS EKS Cluster

>>>Install the AWS CLI
    curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    unzip awscliv2.zip
    sudo ./aws/install
>>>Install kubectl
    curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s 
    https://storage.googleapis.com/kubernetesrelease/release/stable.txt)/bin/linux/amd64/kubectl"
    chmod +x ./kubectl
    sudo mv ./kubectl /usr/local/bin/kubectl
>>>Install eksctl
    curl --silent --location "https://github.com/weaveworks/eksctl/releases/download/latest_release/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
    sudo mv /tmp/eksctl /usr/local/bin
>>>Create an EKS cluster
    eksctl create cluster --name Nodeapp --region eu-north-1 --nodes 2
>>>Configure kubectl for EKS
    aws eks --region eu-north-1 update-kubeconfig --name Nodeapp


# Step 5: Launch an EC2 Instance

>>>Launch an Instance
>>>Choose an Ubuntu
>>>Choose an Instance Type (t3.micro)
>>>Configure Instance Details
   -Accept the default settings for most options.
   -Under "Network", ensure you have a VPC selected.
   -Click "Next: Add Storage".
>>>Add Storage
    -Add any tags to your instance (e.g., Key: Name, Value: MyInstance) to help identify it.
    -Click "Next: Configure Security Group".
    -Configure Security Group:
>>>Create a new security group or select an existing one.
     -Add rules to allow SSH access (port 22) from your IP address.
     -Add rules to allow TCP access (port 80) from your IP address - For Jenkins
     -If you plan to host a web server, also add rules for HTTP (port 80) and HTTPS (port 443).
     -Click "Review and Launch".
>>>Review Instance Launch
     -Review your settings and click "Launch".
     -Select an Existing Key Pair or Create a New Key Pair
     -Create a new one and download it. This key pair will be used to SSH into your instance.
     -If you already have a key pair, you can use that.
     -Acknowledge that you have access to the key pair and click "Launch Instances".
>>>View Instances:
     -Click "View Instances" to see your newly launched instance.
     -Wait for the instance to enter the "running" state.

# Step 6: Connect to Your EC2 Instance

>>>Locate Instance
>>>Set Permissions on Your Key Pair
    chmod 400 key-pair.pem
    ssh -i /path/to/key-pair.pem ec2-user@your-ec2-public-dns
>>>Set Up Your EC2 Instance
    sudo yum update -y

# Step 7: Configure Jenkins for CI/CD

>>>Install Jenkins
 sudo yum update -y
 sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat-stable/jenkins.repo
 sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
 sudo yum install jenkins java-1.8.0-openjdk-devel -y
 sudo systemctl start jenkins
 sudo systemctl enable jenkins
>>>Access Jenkins:
   -Open your browser and navigate to http://<your-ec2-public-ip>:8080.
>>>Install Necessary Plugins
   -Kubernetes CLI Plugin
   -AWS CLI Plugin
   -Git Plugin
   -Docker Pipeline
>>>Create Jenkins Credentials
 Docker Hub Credentials: Add Docker Hub credentials as a Jenkins credential. 
 AWS Credentials: Add AWS access key and secret key as Jenkins credentials.
 Kubeconfig: Add the kubeconfig file for your EKS cluster as a Jenkins credential.
>>>Create a Jenkins Pipeline Job
    -Create a Pipeline Job
    -Go to Jenkins Dashboard.
    -Click on "New Item".
    -Enter a name for your job and select "Pipeline".
    -In Jenkins, create a new Pipeline job and configure it to use the following 'Jenkinsfile' from GitHub repository.
>>>Configure the Jenkins Pipeline
   - In the Pipeline section of the Jenkins job configuration, set the Pipeline script to use the Jenkinsfile from repository.

# Step 8: Trigger the Jenkins Pipeline
  
   - We can manually trigger the pipeline job in Jenkins or set up a webhook in GitHub to trigger the job on every push.
>>>Verify Deployment
   -Check Kubernetes Pods
     kubectl get pods
    -Expose the Service
     kubectl expose deployment node-app --type=LoadBalancer --name=Nodeapp-service
>>>Access the Application
     kubectl get svc node-app-service
    -Once the service is created, we can access your Node.js application using the external IP address of the service.
  
