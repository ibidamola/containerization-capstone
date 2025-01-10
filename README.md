# Capstone Project - Containerization and Container Orchestration

This project demonstrates the process of containerizing a basic frontend application and deploying it to a Kubernetes cluster using Nginx as a web server.

---

## Table of Contents

- [Capstone Project - Containerization and Container Orchestration](#capstone-project---containerization-and-container-orchestration)
  - [Table of Contents](#table-of-contents)
  - [Phase 1: Basic Frontend Application with Docker and Kubernetes](#phase-1-basic-frontend-application-with-docker-and-kubernetes)
    - [Task 1: Set Up Your Project](#task-1-set-up-your-project)
    - [Task 2: Initialize Git](#task-2-initialize-git)
    - [Task 3: Dockerizinhg the application](#task-3-dockerizinhg-the-application)
    - [Task 4: Build and Push Docker Image](#task-4-build-and-push-docker-image)
    - [Install Kind Kubernetes Cluster](#install-kind-kubernetes-cluster)
    - [Task 7: Create a Service (ClusterIP)](#task-7-create-a-service-clusterip)
    - [Task 8: Access the Application](#task-8-access-the-application)
  - [Conclusion](#conclusion)

---

1. Project Structure

2. Hypothetical Use Case
   Develop a simple static website for a company's landing page using HTML and CSS. The application will be containerized using Docker, deployed to a Kubernetes cluster, and made accessible through Nginx.

---

## Phase 1: Basic Frontend Application with Docker and Kubernetes

### Task 1: Set Up Your Project

- Created a new project directory called 'Container capstone'
- Added the following files:
  - `index.html` - The main HTML file for the application.
  - `styles.css` - The CSS file for styling.

### Task 2: Initialize Git

- Initialize a Git repository in the project directory.

  ```bash
  git init
  git add .
  git commit -m "first commit"
  git branch  -u origin main
  git remote add orin https://....
  git push -u origin main

  ```
![git Initializing](images/Screenshot%202024-12-13%20104059.png)
### Task 3: Dockerizinhg the application

- Created a Dockerfile using Nginx as the base image.
- Added the dockerfile in the same directory as the indesx and style sheeet
- Copied the index.html and styles.css files into the appropriate Nginx HTML directory (/usr/share/nginx/html). Example Dockerfile:

```bash
FROM nginx:latest
COPY index.html /usr/share/nginx/html/index.html
COPY styles.css /usr/share/nginx/html/styles.css

```
### Task 4: Build and Push Docker Image

- Build the Docker image.
```bash
docker build -t dockerfile -f ./Dockerfile .
```
- login to docker Hub 
- push the image to Docker Hub

```bash
docker push <aeeshar01>/simple-frontend:latest .
```

![docker file](images/Screenshot%202024-12-13%20104120.png)
### Install Kind Kubernetes Cluster
- installing the kind cluster was a bit different for me

- step 1: i followed this link "https://github.com/kubernetes-sigs/kind"
- step 2: go to RELEASES at the right hand tab of the git page, click on it.
- step 3: Go to assets
- step 4: Download the Kind-windows-amd64
- go to your windows explorer
- right click on the PC
- select Properties
- scrool down to addanced system settings and click on it
- click on environment variables
- under system varaibales click on Path and select edit
- for me i placed the downloadedm kind and placed it the file directory Owner.
- rename the kind file to kind.exe
- Go into the powershell window and type kind
- go back into our CMD window and type in
```bash
kind create cluster

```

![Create Kind Cluster](images/Screenshot%202024-12-13%20114343.png)
```bash 
kubectl get nodes
```

- cd into our container-apstone folder
```bash
cd  container-capstone
```
- create deployment.yaml file:
```bash
code deployment.yaml
```

- specify the image and desired replicas to the deploymnet.yaml file;
```bash apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
        - name: web-app
          image: aeeshar01/my-nginx:1.1
          ports:
            - containerPort: 80

```

- apply the deployment configuration
```bash
code deployment.yaml
kubectl apply -f deployment.yaml

```

### Task 7: Create a Service (ClusterIP)
- Create a Kubernetes service YAML file (service.yaml) to expose the deployment internally.
- Service.yaml
```bash
apiVersion: v1
kind: Service
metadata:
  name: web-app-service
spec:
  selector:
    app: web-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP

```

- Apply the service.
```bash
kubectl apply -f service.yaml
```

### Task 8: Access the Application

- Port-forward the service to access the application locally.
```bash
kubectl port-forward service/web-app-service 8080:80
```
![web page](images/Screenshot%202024-12-13%20114410.png)
___
## Conclusion
This project covers the essential steps to containerize a basic frontend applictation and deploy it to a Kubernetes cluster. It demonstrates the workflow from local development to container orchestration using modern DevOps tools. 