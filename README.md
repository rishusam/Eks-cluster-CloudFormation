# Eks-cluster-CloudFormation
This repository contains CloudFormation templates for creating and managing Amazon Elastic Kubernetes Service (EKS) clusters.

**1. Creating an EKS Cluster using CloudFormation:**

a.	Define the CloudFormation template: Create a YAML or JSON CloudFormation template to define your EKS cluster resources, including the VPC, subnets, security groups, IAM roles, and the EKS cluster itself.

 





 

b.	Define the EKS Cluster: Add a resource definition for the EKS cluster, specifying its properties such as the cluster name, version, IAM role for service accounts, and networking configuration.


 

c.	Configure Autoscaling: Include configurations for the Kubernetes Horizontal Pod Autoscaler (HPA) in the CloudFormation template. Set the desired minimum and maximum replicas for your application deployments.
 
d.	Launch the CloudFormation stack: Use the AWS Management Console or AWS CLI to deploy the CloudFormation stack. Monitor the progress and ensure that the stack creation is successful.
e.	 
 
IAM
Create a user “eks-admin” with AdministratorAccess
Create Security Credentials Access Key and Secret access key 

EC2

Create an ubuntu instance (region us-west-2)

ssh to the instance from local

Install AWS CLI v2
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
sudo apt install unzip
unzip awscliv2.zip
sudo ./aws/install -i /usr/local/aws-cli -b /usr/local/bin --update

Setup your access by


aws configure

Install Docker

sudo apt-get update
sudo apt install docker.io
docker ps
sudo chown $USER /var/run/docker.sock
Install kubectl

curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin
kubectl version --short --client


Install eksctl

curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
sudo mv /tmp/eksctl /usr/local/bin
eksctl version

Setup EKS Cluster


eksctl create cluster --name three-tier-cluster --region us-west-2 --node-type t2.medium --nodes-min 2 --nodes-max 2
aws eks update-kubeconfig --region us-west-2 --name three-tier-cluster
kubectl get nodes

Run Manifests

kubectl create namespace two-tier-ns
kubectl apply -f .
Kubectl delete -f .

eksctl delete cluster --name my-cluster --region us-west-2

 



 

**2. Deploying Applications within the EKS Cluster:**
a.	Create Dockerfiles: Write Dockerfiles for your "Hello World" applications. These Dockerfiles should contain instructions to build your application images.
b.	 
c.	
d.	 

e.	Build Docker Images: Use the `docker build` command to build Docker images from your Dockerfiles. Make sure to tag the images appropriately.

 
 


Sure, here are the steps to set up a two-tier Flask application in a Docker container:

**Step 1: Prepare Your Flask Application**

Ensure you have a Flask application ready to be containerized. Here's a simple example:

```python
# app.py
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello, World!"

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0')
```

**Step 2: Create a Dockerfile**

Create a Dockerfile in the root directory of your project. This file will contain instructions for building your Docker image.

```Dockerfile
# Use an official Python runtime as a parent image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed dependencies specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 5000 available to the world outside this container
EXPOSE 5000

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

**Step 3: Create a requirements.txt File**

Create a requirements.txt file listing all Python dependencies required by your Flask application.

```
Flask==2.0.2
```

**Step 4: Build Your Docker Image**

Open a terminal and navigate to the directory containing your Dockerfile and application code. Then run the following command to build your Docker image:

```bash
docker build -t my-flask-app .
```

**Step 5: Run Your Docker Container**

Once the image is built, you can run a container based on that image:

```bash
docker run -p 5000:5000 my-flask-app
```

This command maps port 5000 on your local machine to port 5000 in the Docker container.

**Step 6: Access Your Flask Application**

Open a web browser and navigate to `http://localhost:5000` to access your Flask application running in the Docker container.

That's it! You've successfully set up a two-tier Flask application in a Docker container. You can now deploy and run this container on any platform that supports Docker.
 
Certainly! Here's how you can set up another Flask application named "philippaul sweet candy 01" in a Docker container listening on port 3000:

**Step 1: Prepare Your Flask Application**

Ensure you have a Flask application ready to be containerized. Here's a simple example:

```python
# app.py
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello from Philippaul Sweet Candy 01!"

if __name__ == '__main__':
    app.run(debug=True, host='0.0.0.0', port=3000)
```

**Step 2: Create a Dockerfile**

Create a Dockerfile in the root directory of your project. This file will contain instructions for building your Docker image.

```Dockerfile
# Use an official Python runtime as a parent image
FROM python:3.9-slim

# Set the working directory in the container
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed dependencies specified in requirements.txt
RUN pip install --no-cache-dir -r requirements.txt

# Make port 3000 available to the world outside this container
EXPOSE 3000

# Define environment variable
ENV NAME Philippaul

# Run app.py when the container launches
CMD ["python", "app.py"]
```

**Step 3: Create a requirements.txt File**

Create a requirements.txt file listing all Python dependencies required by your Flask application.

```
Flask==2.0.2
```

**Step 4: Build Your Docker Image**

Open a terminal and navigate to the directory containing your Dockerfile and application code. Then run the following command to build your Docker image:

```bash
docker build -t sweet-candy-app .
```

**Step 5: Run Your Docker Container**

Once the image is built, you can run a container based on that image:

```bash
docker run -p 80:3000 sweet-candy-app
```

This command maps port 3000 in the container to port 80 on your local machine.

**Step 6: Access Your Flask Application**

Open a web browser and navigate to `http://localhost` to access your Flask application running in the Docker container.

That's it! You've successfully set up the "philippaul sweet candy 01" Flask application in a Docker container listening on port 80.

 




 

**3. Testing Autoscaling Functionality:**
 


a.	Generate Load on Applications: Use a load testing tool like Apache JMeter or k6 to simulate load on your applications. Send HTTP requests to the applications' endpoints to generate traffic.
b.	 
c.	Monitor Autoscaling Behavior: Monitor the EKS cluster and observe how the Horizontal Pod Autoscaler (HPA) responds to the increased load. You should see the number of pod replicas increase as the load increases and decrease as the load decreases.

To deploy a Horizontal Pod Autoscaler (HPA) in an Amazon EKS cluster, you'll need to follow these steps:

**Step 1: Create an HPA YAML file**

Create a YAML file defining your HPA configuration. Here's an example:

```yaml
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: my-hpa
  namespace: your-namespace
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: your-deployment
  minReplicas: 1
  maxReplicas: 3
  metrics:
  - type: Resource
    resource:
      name: cpu
      targetAverageUtilization: 50
```

In this YAML file:
- `metadata.name` specifies the name of the HPA.
- `metadata.namespace` specifies the namespace where your deployment resides.
- `spec.scaleTargetRef` specifies the target deployment for scaling.
- `spec.minReplicas` specifies the minimum number of replicas.
- `spec.maxReplicas` specifies the maximum number of replicas.
- `spec.metrics` specifies the metric used for autoscaling. In this example, it scales based on CPU utilization.

**Step 2: Apply the HPA YAML file**

Apply the YAML file using `kubectl apply`:

```bash
kubectl apply -f your-hpa-file.yaml
```

Replace `your-hpa-file.yaml` with the path to your HPA YAML file.

**Step 3: Verify the HPA**

Verify that the HPA has been created successfully:

```bash
kubectl get hpa -n your-namespace
```

Replace `your-namespace` with the namespace where your HPA is deployed.

That's it! You've deployed an HPA in your Amazon EKS cluster, which will automatically scale your deployment based on CPU utilization. Adjust the YAML file as needed for your specific deployment requirements.
d.	 
e.	


 


