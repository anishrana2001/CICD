# GitLab Runner Executors - Complete Comparison Guide

## Overview

GitLab Runners execute jobs using different **executors**, which determine how and where jobs run. Choosing the right executor is critical for CI/CD pipeline performance, isolation, and cost efficiency.

## Executor Types Comparison Table

| Feature | Shell | Docker | Kubernetes | Docker Machine | Virtualbox |
|---------|-------|--------|------------|-----------------|-----------|
| **Isolation Level** | None (Host OS) | Container Level | Pod Level | VM Level | VM Level |
| **Speed** | ⚡ Fastest | ⚡⚡ Fast | ⚡⚡ Fast | Slower | Slowest |
| **Resource Overhead** | Minimal | Low | Medium | High | Very High |
| **Setup Complexity** | Simple | Medium | Complex | Medium | Complex |
| **Concurrent Jobs** | Limited | Excellent | Excellent | Good | Fair |
| **Best For** | Simple scripts, quick tests | Containerized apps, Docker builds | Kubernetes deployments, multi-container | Legacy systems | Testing locally |
| **Cost Efficiency** | Highest | High | Medium | Low | Low |
| **Production Ready** | ⚠️ Limited | ✅ Yes | ✅ Yes | ⚠️ Limited | ❌ No |

---

## 1. SHELL EXECUTOR

### What It Does
Executes jobs directly on the host machine's shell (Bash on Linux/Mac, PowerShell on Windows).

### When to Use
- Simple CI/CD pipelines (linting, building source code)
- Fast feedback loops with minimal overhead
- Testing on specific OS/architecture
- Legacy systems without containerization support

### Pros
- ✅ Fastest execution (no overhead)
- ✅ Minimal resource requirements
- ✅ Direct access to host filesystem
- ✅ Best for rapid development/testing

### Cons
- ❌ No isolation between jobs (security risk)
- ❌ Builds can pollute host environment
- ❌ Dependency conflicts between jobs
- ❌ Hard to replicate environment locally

### Installation

```bash
# Install GitLab Runner
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh | bash
sudo apt-get install gitlab-runner

# Register runner
sudo gitlab-runner register \
  --url https://gitlab.com/ \
  --registration-token <YOUR_TOKEN> \
  --executor shell \
  --description "Shell Runner - My Local"
```

### Example **.gitlab-ci.yml**
```yaml
stages:
  - build
  - test

build_job:
  stage: build
  executor: shell
  script:
    - echo "Building with shell executor"
    - npm install
    - npm run build
  artifacts:
    paths:
      - dist/

test_job:
  stage: test
  executor: shell
  script:
    - npm test
```

### Configuration (config.toml)
```yaml
[[runners]]
  name = "shell-runner"
  url = "https://gitlab.com/"
  token = "YOUR_TOKEN"
  executor = "shell"
  
  [runners.machine]
    # Additional shell-specific settings
    privileged = false
```


# 2. DOCKER EXECUTOR
- **What It Does:**  
    - Runs each job in an isolated Docker container with specified image. Perfect for containerized applications.

- **When to Use:** 
    - Building & testing Docker images
    - Multi-language projects
    - Containerized microservices
    - Reproducible builds across environments
    - Most Common Choice for Modern CI/CD

### Pros
- ✅ Complete job isolation (each job = fresh container)
- ✅ No dependency conflicts
- ✅ Reproducible builds (same image = same environment)
- ✅ Supports Docker-in-Docker for building images
- ✅ Easy to scale horizontally
- ✅ Production-ready

### Cons
- ❌ Slower than shell (container startup overhead)
- ❌ Requires Docker daemon
- ❌ Higher resource usage per job
- ❌ Docker-in-Docker security considerations

### Installation
---
#### Install Docker
```
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```
#### Install GitLab Runner
```
sudo apt-get install gitlab-runner
```
#### Register Docker executor runner
```
sudo gitlab-runner register \
  --url https://gitlab.com/ \
  --registration-token <YOUR_TOKEN> \
  --executor docker \
  --docker-image ubuntu:latest \
  --description "Docker Runner"
```
### Example .gitlab-ci.yml - Node.js Project
```yaml
stages:
  - build
  - test
  - package

variables:
  REGISTRY: $CI_REGISTRY
  IMAGE_NAME: $CI_REGISTRY_IMAGE

build_job:
  stage: build
  image: node:18-alpine
  script:
    - npm install
    - npm run build
  artifacts:
    paths:
      - dist/
  cache:
    paths:
      - node_modules/

test_job:
  stage: test
  image: node:18-alpine
  script:
    - npm install
    - npm test
    - npm run lint
  coverage: '/Lines\s*:\s*(\d+\.?\d*)%/'

docker_build:
  stage: package
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker build -t $IMAGE_NAME:$CI_COMMIT_SHA .
    - docker push $IMAGE_NAME:$CI_COMMIT_SHA
    - docker tag $IMAGE_NAME:$CI_COMMIT_SHA $IMAGE_NAME:latest
    - docker push $IMAGE_NAME:latest
```

### Configuration (config.toml)
```yml
[[runners]]
  name = "docker-runner"
  url = "https://gitlab.com/"
  token = "YOUR_TOKEN"
  executor = "docker"
  
  [runners.docker]
    image = "ubuntu:latest"
    privileged = false
    volumes = ["/cache"]
    allowed_images = ["*"]
    allowed_services = ["*"]
    
    # For Docker-in-Docker builds
    # services = ["docker:dind"]
```

## **Docker Executor with Docker-in-Docker (DinD)**

```yaml
# Build Docker images within CI/CD pipeline
build_docker:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  variables:
    DOCKER_HOST: tcp://docker:2375
    DOCKER_DRIVER: overlay2
  script:
    - docker build -t my-app:$CI_COMMIT_SHA .
    - docker tag my-app:$CI_COMMIT_SHA my-app:latest
  only:
    - main
```


# 3. KUBERNETES EXECUTOR ⭐ (Advanced)

- **What It Does**
    - Runs each job as a Pod in Kubernetes cluster. Best for large-scale enterprise CI/CD.

- **When to Use**
- Kubernetes/OpenShift deployments ← YOUR EXPERTISE!
- Large-scale CI/CD with many concurrent jobs
- Microservices pipelines
- Dynamic resource allocation
- Multi-cluster deployments
- Cost-efficient horizontal scaling

### Pros

- ✅ Excellent scalability (unlimited concurrent jobs)
- ✅ Cost-efficient (only pay for resources used)
- ✅ Native Kubernetes integration
- ✅ Dynamic pod provisioning
- ✅ Multi-cluster support
- ✅ Advanced networking/security policies
- ✅ Production enterprise-grade

### Cons
- ❌ Complex setup and configuration
- ❌ Requires Kubernetes cluster knowledge
- ❌ Debugging more difficult
- ❌ RBAC & security considerations
- ❌ Longer startup time (pod creation)

### **Installation on Kubernetes**

#### 1. Create namespace for GitLab Runner
```
kubectl create namespace gitlab-runner
```
#### 2. Install Helm chart (easiest method)
```
helm repo add gitlab https://charts.gitlab.io
helm repo update

helm install gitlab-runner gitlab/gitlab-runner \
  --namespace gitlab-runner \
  --set gitlabUrl=https://gitlab.com/ \
  --set gitlabRunnerRegistrationToken=<YOUR_TOKEN> \
  --set runners.image=ubuntu:latest \
  --set runners.privileged=true
```
#### 3. Or: Manual registration on existing runner
```
gitlab-runner register \
  --url https://gitlab.com/ \
  --registration-token <YOUR_TOKEN> \
  --executor kubernetes \
  --kubernetes-host https://kubernetes-cluster-ip:6443 \
  --kubernetes-ca-file /path/to/ca.crt \
  --kubernetes-token <K8S_TOKEN>
```
#### Example .gitlab-ci.yml - Kubernetes Deployment
```yml
stages:
  - build
  - test
  - deploy

variables:
  DOCKER_IMAGE: $CI_REGISTRY_IMAGE:$CI_COMMIT_SHA
  KUBE_NAMESPACE: production

build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $DOCKER_IMAGE .
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker push $DOCKER_IMAGE

test:
  stage: test
  image: ubuntu:latest
  script:
    - apt-get update && apt-get install -y curl
    - ./run_tests.sh

deploy_k8s:
  stage: deploy
  image: bitnami/kubectl:latest
  script:
    # Set kubeconfig from secret
    - echo "$KUBECONFIG" | base64 -d > /tmp/config
    - kubectl --kubeconfig=/tmp/config set image deployment/my-app app=$DOCKER_IMAGE -n $KUBE_NAMESPACE
    - kubectl --kubeconfig=/tmp/config rollout status deployment/my-app -n $KUBE_NAMESPACE
  only:
    - main
```

#### Configuration (config.toml) - Kubernetes
```yml
[[runners]]
  name = "kubernetes-runner"
  url = "https://gitlab.com/"
  token = "YOUR_TOKEN"
  executor = "kubernetes"
  
  [runners.kubernetes]
    host = "https://kubernetes.example.com:6443"
    ca_file = "/etc/gitlab-runner/certs/ca.crt"
    cert_file = "/etc/gitlab-runner/certs/cert.crt"
    key_file = "/etc/gitlab-runner/certs/key.key"
    namespace = "gitlab-runner"
    privileged = true
    
    # Pod resource limits
    [runners.kubernetes.cpu_limit]
      limit = "1"
      request = "500m"
    
    [runners.kubernetes.memory_limit]
      limit = "1Gi"
      request = "512Mi"
```

# 4. DOCKER MACHINE EXECUTOR (Deprecated)
- ⚠️ Deprecated: Use Kubernetes executor for scaling instead.
- Automatically provisions Docker hosts using Docker Machine.

- **When to Use**
- Legacy CI/CD pipelines
- AWS/Azure auto-scaling needs
- NOT recommended for new projects
---
---
# Comparison: When to Choose What?
### ✅ Choose SHELL when:
```
- Simple scripts, no Docker needed
- Testing on specific OS
- Fast local development feedback
- Single machine, low security requirements
```
### ✅ Choose DOCKER when:
```
- Building/testing containerized apps
- Most modern CI/CD pipelines ← START HERE
- Multi-language projects
- 5-50 concurrent jobs
- Cost-conscious but needs isolation
```

### ✅ Choose KUBERNETES when:
```
- 50+ concurrent jobs
- Microservices deployments ← YOUR SCENARIO
- OpenShift/K8s cluster available
- Enterprise scalability needed
- Dynamic resource allocation required
```

### Performance Comparison
```
Job Startup Time:
  Shell:         10-50ms   (instant)
  Docker:        500ms-2s  (container startup)
  Kubernetes:    2-5s      (pod creation)
  Docker Machine:5-10s     (VM provisioning)

Resource Overhead per Job:
  Shell:         None
  Docker:        50-100MB  (small)
  Kubernetes:    100-500MB (medium)
  Docker Machine:500MB-1GB (large)
```
# Best Practices
### 1. Shell Executor
```
##### DO: Use for quick scripts
shell_job:
  executor: shell
  script:
    - echo "Hello"

# DON'T: Mix unrelated jobs
# Each can pollute the environment for others
```
#### 2. Docker Executor
```
# DO: Specify exact image version
docker_job:
  image: node:18-alpine
  script:
    - npm test

# DON'T: Use 'latest' tag (unpredictable)
```
#### 3. Kubernetes Executor
```
# DO: Set resource limits
k8s_job:
  image: ubuntu:latest
  resource_limits:
    cpu: 1
    memory: 1Gi
  script:
    - kubectl apply -f deploy.yaml

# DON'T: Run without RBAC restrictions
```

# Decision Tree
```
Do you have Docker installed and want isolation?
├─ YES → Use DOCKER Executor
│        └─ Good for: Most modern projects
│
├─ NO, need simple shell scripts?
│  └─ YES → Use SHELL Executor
│           └─ Good for: Basic testing
│
└─ Have Kubernetes/OpenShift cluster?
   └─ YES → Use KUBERNETES Executor
            └─ Good for: Enterprise scale (YOUR CASE!)
```

### Quick Setup Commands
#### Docker Executor (Recommended Start)
```
sudo gitlab-runner register \
  --url https://gitlab.com/ \
  --registration-token <TOKEN> \
  --executor docker \
  --docker-image ubuntu:latest \
  --description "Docker Runner"
```
Kubernetes Executor (Enterprise)
```
helm install gitlab-runner gitlab/gitlab-runner \
  --namespace gitlab-runner \
  --set gitlabUrl=https://gitlab.com/ \
  --set gitlabRunnerRegistrationToken=<TOKEN> \
  --set runners.tags="kubernetes"
```
Related Topics
Caching Strategies

Runner Tags

Docker Builds

Kubernetes Deployments

text

***

## **FILE 2: `11_00_docker-builds.md`**

```markdown
# Building Docker Images in GitLab CI/CD

## Overview

Building Docker images is one of the most common CI/CD tasks. This guide covers everything from basic Docker builds to advanced multi-stage, optimized builds in GitLab CI/CD.

## Prerequisites

- GitLab Runner with Docker executor
- Basic understanding of Docker & Dockerfile
- Docker registry access (GitLab Registry or Docker Hub)

---

## 1. BASIC DOCKER BUILD & PUSH

### Simple Dockerfile

```dockerfile
# Dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .

EXPOSE 3000

CMD ["node", "server.js"]
Basic .gitlab-ci.yml
text
stages:
  - build

docker_build:
  stage: build
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t my-app:$CI_COMMIT_SHA .
```
