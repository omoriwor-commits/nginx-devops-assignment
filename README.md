# Nginx DevOps Assignment

A hands-on DevOps lab that takes a simple Nginx web application through source control, containerization, image publishing, Kubernetes deployment, and CI/CD automation.

## Architecture

```text
Developer workstation
        |
        | git push
        v
GitHub repository
        |
        | GitHub Actions
        v
Docker image build
        |
        | push
        v
Docker Hub
rotezsolutions/nginx-devops-assignment
        |
        | pull
        v
Kubernetes Deployment
        |
        +--> Pod 1: Nginx
        |
        +--> Pod 2: Nginx
        |
        v
Kubernetes Service
        |
        v
Application access
```

## Repository Structure

```text
nginx-devops-assignment/
├── .github/
│   └── workflows/
│       └── docker-build-push.yml
├── k8s/
│   ├── deployment.yaml
│   └── service.yaml
├── .dockerignore
├── .gitignore
├── Dockerfile
└── index.html
```

## Application

The application is a static HTML page served by Nginx.

The page identifies the lab components used:

- Git
- Docker
- CI/CD
- Kubernetes

## Git Workflow

The repository was initialized locally and pushed to GitHub.

Typical commands used:

```bash
git init
git status
git add .
git commit -m "Create initial Nginx application"
git remote -v
git push -u origin main
```

Later changes were committed separately, including Docker configuration, Kubernetes manifests, and the GitHub Actions workflow.

## Docker

### Dockerfile

The application uses the Alpine-based Nginx image:

```dockerfile
FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

### Build the image

```bash
docker build -t nginx-devops-assignment:v1 .
```

### Run the container

```bash
docker run -d --name nginx-devops-app -p 8080:80 nginx-devops-assignment:v1
```

### Verify the container

```bash
docker ps
docker logs nginx-devops-app
docker inspect nginx-devops-app
```

The application was successfully tested at:

```text
http://localhost:8080
```

### Inspect the running container

```bash
docker exec -it nginx-devops-app sh
ls -l /usr/share/nginx/html
cat /usr/share/nginx/html/index.html
exit
```

This confirmed that the custom `index.html` was present inside the running Nginx container.

## Docker Hub

Docker Hub repository:

```text
rotezsolutions/nginx-devops-assignment
```

### Manual tag and push

```bash
docker login
docker tag nginx-devops-assignment:v1 rotezsolutions/nginx-devops-assignment:v1
docker push rotezsolutions/nginx-devops-assignment:v1
```

### Published tags

The repository currently includes:

```text
v1
latest
961dbbd7ce916e5b7910a07313b1e849cde8d3c7
```

The `latest` tag and commit-SHA tag were produced by GitHub Actions.

Example pulls:

```bash
docker pull rotezsolutions/nginx-devops-assignment:v1
docker pull rotezsolutions/nginx-devops-assignment:latest
```

## Kubernetes

Docker Desktop Kubernetes was used as the local Kubernetes environment.

Cluster verification:

```bash
kubectl config current-context
kubectl get nodes
```

The cluster context was:

```text
docker-desktop
```

and the control-plane node reached `Ready` state.

### Deployment

The Kubernetes Deployment runs two replicas of the application:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-devops-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx-devops
  template:
    metadata:
      labels:
        app: nginx-devops
    spec:
      containers:
        - name: nginx-devops
          image: rotezsolutions/nginx-devops-assignment:v1
          ports:
            - containerPort: 80
```

Apply it with:

```bash
kubectl apply -f k8s/deployment.yaml
```

### Service

The application is exposed through a NodePort Service:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-devops-service
spec:
  selector:
    app: nginx-devops
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
```

Apply it with:

```bash
kubectl apply -f k8s/service.yaml
```

### Verify Kubernetes resources

```bash
kubectl get deployments
kubectl get pods
kubectl get services
kubectl describe deployment nginx-devops-deployment
kubectl describe service nginx-devops-service
```

Validation showed:

```text
Deployment replicas: 2/2 available
Pods:               2 Running
Service type:        NodePort
```

The Service discovered both Pod endpoints.

### Test through Kubernetes

A local port-forward was used:

```bash
kubectl port-forward service/nginx-devops-service 8081:80
```

The application was successfully tested at:

```text
http://localhost:8081
```

This validated the path:

```text
Browser
  -> localhost:8081
  -> kubectl port-forward
  -> Kubernetes Service
  -> Kubernetes Pods
  -> Nginx
  -> index.html
```

## GitHub Actions CI/CD

Workflow file:

```text
.github/workflows/docker-build-push.yml
```

The workflow runs on pushes to `main`.

It performs the following steps:

1. Checks out the repository.
2. Logs in to Docker Hub.
3. Builds the Docker image.
4. Pushes the image to Docker Hub.
5. Publishes both `latest` and commit-SHA tags.

The workflow uses these GitHub repository secrets:

```text
DOCKERHUB_USERNAME
DOCKERHUB_TOKEN
```

The Docker Hub token is stored only as a GitHub Actions secret and is not committed to the repository.

### CI/CD Result

The final workflow run completed successfully.

The GitHub Actions pipeline automatically published:

```text
rotezsolutions/nginx-devops-assignment:latest
rotezsolutions/nginx-devops-assignment:961dbbd7ce916e5b7910a07313b1e849cde8d3c7
```

Both tags point to the same image digest, confirming that the same build artifact was published under a floating tag and an immutable commit-specific tag.

## End-to-End Flow

```text
Source code
   |
   v
Git commit
   |
   v
GitHub main branch
   |
   v
GitHub Actions
   |
   v
Docker build
   |
   v
Docker Hub
   |
   v
Kubernetes Deployment
   |
   v
2 Nginx Pods
   |
   v
Kubernetes Service
   |
   v
Application
```

## Validation Summary

The following controls were validated successfully:

- Git repository initialized and synchronized with GitHub.
- Docker image built successfully.
- Nginx container started successfully.
- Application served over host port 8080.
- Container contents inspected directly.
- Docker image published manually to Docker Hub.
- Kubernetes cluster reached Ready state.
- Two application Pods ran successfully.
- Kubernetes Service discovered both Pod endpoints.
- Application served successfully through Kubernetes on localhost:8081.
- GitHub Actions authenticated to Docker Hub using repository secrets.
- CI/CD workflow built and pushed the image automatically.
- Docker Hub contains `v1`, `latest`, and commit-SHA image tags.

## Security Notes

- Docker Hub credentials are not stored in source code.
- GitHub Actions uses repository secrets for registry authentication.
- The Docker Hub personal access token should be scoped only to the permissions required for this lab and rotated when no longer needed.
- Secrets, passwords, SSH private keys, and tokens must never be committed to Git.

## Status

**Assignment completed successfully: Git + Docker + Docker Hub + Kubernetes + GitHub Actions CI/CD.**
