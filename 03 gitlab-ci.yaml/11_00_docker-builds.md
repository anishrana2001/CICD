

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
```

### Basic .gitlab-ci.yml
```yaml
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