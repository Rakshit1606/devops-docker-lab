# Containerization and Orchestration Lab Documentation

## 1. Tools Used

- Docker Desktop
- Docker CLI
- Docker Compose
- Docker Hub
- Git
- GitHub
- Jenkins
- Kubernetes
- Minikube
- kubectl
- Python Flask

---

## 2. Docker Containerization

### Files Used

- Dockerfile
- app.py
- requirements.txt

### Dockerfile

The application was containerized using Python 3.12-slim.

The Dockerfile:
- Uses Python 3.12-slim as the base image.
- Sets `/app` as the working directory.
- Installs dependencies from requirements.txt.
- Copies the Flask application.
- Exposes port 5000.
- Starts the application using Python.

### Commands Used

```bash
docker build -t docker-lab-app .
docker images
docker run -d -p 5001:5000 --name my-docker-container docker-lab-app
docker ps

Observation

The Flask application successfully ran inside a Docker container and was accessible through the mapped host port.

docker tag docker-lab-app:latest rakshit1606/docker-lab-app:latest
docker push rakshit1606/docker-lab-app:latest

Observation

The Docker image was successfully tagged and pushed to Docker Hub.

docker compose config
docker compose up -d
docker compose ps

Observation

Docker Compose successfully started the web application and Redis as multiple services.

echo "This data will be lost" > /app/container-data.txt
docker volume create my-data-volume

docker run --name volume-test \
-v my-data-volume:/data \
docker-lab-app \
sh -c 'echo "This data is permanent" > /data/volume-data.txt'
mkdir bind-data

docker run --rm \
-v "$(pwd)/bind-data:/data" \
docker-lab-app \
sh -c 'echo "This data is stored on my Mac" > /data/bind-data.txt'
Observation

Container-layer data is temporary, while named volumes and bind mounts provide persistent storage.

docker network create lab-network
docker run -d \
--name container-one \
--network lab-network \
docker-lab-app
docker run --rm \
--network lab-network \
docker-lab-app \
python -c "import socket; print(socket.gethostbyname('container-one'))"
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' container-one
172.19.0.2

IP Communication

Connectivity was verified using the container IP address.

Observation

Docker containers connected to the same user-defined network can communicate using container names and IP addresses.

kubectl apply -f deployment.yaml
kubectl get deployments
kubectl get pods

kubectl apply -f service.yaml
kubectl get services
minikube service docker-lab-service --url
Observation

The application successfully ran inside Minikube and was exposed using a Kubernetes NodePort Service

docker tag docker-lab-app:latest docker-lab-app:v2
minikube image load docker-lab-app:v2

kubectl set image deployment/docker-lab-deployment \
docker-lab-app=docker-lab-app:v2

kubectl rollout status deployment/docker-lab-deployment

kubectl rollout undo deployment/docker-lab-deployment
kubectl rollout status deployment/docker-lab-deployment

Observation

Kubernetes successfully performed a rolling update and rollback using the Deployment controller.

kubectl scale deployment docker-lab-deployment --replicas=4

kubectl get deployment docker-lab-deployment
kubectl get pods

Deployment: 4/4
Pods: 4 Running

Observation

Kubernetes successfully increased the number of application replicas from two to four.

kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

kubectl get pods
kubectl get deployment
kubectl get service

Finished: SUCCESS

Observation

Jenkins successfully integrated GitHub with Kubernetes and automated the application of Kubernetes manifests.

Repository:

https://github.com/Rakshit1606/devops-docker-lab

The repository contains the main project files, Docker configuration, Kubernetes manifests, storage example, and Jenkins pipeline.

12. Screenshot Record

The following screenshots were captured during the experiments.

Q1 - Docker Containerization
Docker image and container commands
Running Docker container
Application running in browser
Q2 - Docker Compose and Docker Hub
Docker image tagging and push
Docker Compose configuration and running services
GitHub repository
Q3 - Docker Storage
Container-layer data and persistent storage demonstration
GitHub commit and push
Q4 - Docker Networking
Docker network and containers
Container-name resolution and IP communication
Q5 - Kubernetes Deployment
Kubernetes Pods, Deployment and Service
Application running through Minikube
Q6 - Kubernetes Deployment Strategies
Rolling update
Rollback
Scaling from 2 to 4 replicas
Q8 - Continuous Deployment
Kubernetes deployment verification
Jenkinsfile Git push
Jenkins pipeline configuration
Jenkins Console Output showing successful Kubernetes deployment

13. Overall Observations
Docker provided a portable environment for running the application.
Docker Compose simplified multi-container application management.
Named volumes and bind mounts provided persistent storage.
Docker networks enabled container-to-container communication.
Kubernetes Deployment managed application replicas and updates.
Kubernetes Service provided network access to the application.
Rolling updates and rollbacks allowed controlled application version changes.
Kubernetes scaling increased application replicas from two to four.
Jenkins successfully automated the application of Kubernetes manifests from the GitHub repository.


14. Conclusion

The experiments demonstrated the complete workflow from application containerization using Docker to orchestration using Kubernetes and continuous deployment using Jenkins. The project successfully demonstrated container management, persistent storage, networking, Kubernetes deployment, rolling updates, rollback, scaling, and CI/CD-based automated deployment.
