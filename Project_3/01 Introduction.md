# Here is a step-by-step process to create a "DevOps Status Dashboard" project. This guide uses a story-driven approach to integrate your Dev and Ops teams.

- 1. The Story: "DevOpsWala Status Portal"
- Objective: The Development team needs to launch a simple web portal to display system status.
- The Challenge: The Code is on the Mac, the Pipeline runs on Servera, and the App must live on Serverb.

### Team Roles:

- Developers: Write the code (HTML) and push changes.
- Ops L1 (Monitoring): Monitor the pipeline for "Green" status.
- Ops L2 (Support): Investigate failed jobs (logs) and fix runner issues.
- Ops L3 (Expert): Manually approve and trigger the final deployment to Production.

## 2. Infrastructure Prerequisites (One-Time Setup)
### Before creating the project, you must ensure Servera (Runner) can talk to Serverb (Target) without passwords.

### Step 2.1: Configure SSH between Servers (Ops L3 Task)
### Login to Servera (10.10.10.16) as the gitlab-runner user.
- (Note: If you cannot log in as gitlab-runner directly, switch to root first, then su - gitlab-runner).

### On Servera (10.10.10.16)
```
sudo su - gitlab-runner
ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa
```

### # Copy the key to Serverb (10.10.10.19)
### # Assuming 'student' or 'root' is the user on Serverb you want to deploy as.
```
ssh-copy-id student@10.10.10.19
```
### Step 2.2: Verify Connectivity
- Still as `gitlab-runner` on `Servera`, test the connection:

```
ssh student@10.10.10.19 "uptime"
```

#### Output should show Serverb uptime without asking for a password.
## 3. Project Management Setup (MacBook)

- Role: Project Manager / Team Lead
- Create Project: Log in to GitLab and create a project named status-portal.
- Create Milestone: Go to Issues > Milestones and create v1.0 Launch.
- Create Labels: Go to Manage > Labels and create:

- Team::Dev (Blue)
- Team::Ops-L3 (Red)
- Status::Ready-for-Prod (Green)

### Create Issue: Go to Issues > New Issue.

- Title: "Create Homepage for Dashboard"
- Assignee: Yourself (acting as Dev)
- Milestone: v1.0 Launch
- Label: Team::Dev

## 4. Developer Workflow (Local Mac)
### Role: Developer

### On your MacBook, clone the repo and create the feature branches.


#### Clone the repo
```
git clone http://10.10.10.16/your-username/status-portal.git
cd status-portal
```
#### Create two feature branches
```
git checkout -b feature/dev-team-1
git checkout -b feature/dev-team-2
```
#### Switch to Team 1 branch to start work
```
git checkout feature/dev-team-1
```
### Create the Application (index.html):
```
xml
<!DOCTYPE html>
<html>
<body>
<h1>DevOpsWala Status Portal</h1>
<p>System is Operational. Deployed by GitLab CI.</p>
</body>
</html>
```
### 5. The CI/CD Pipeline Configuration
- Role: Ops L3 (Expert)
- Create a `.gitlab-ci.yml` file on your Mac. This defines the workflow for all teams.

```
stages:
  - test          # Dev Team check
  - deploy_staging # Ops L2 verification
  - deploy_prod   # Ops L3 Approval

variables:
  TARGET_SERVER: "10.10.10.19"
  TARGET_USER: "root"

# --- Dev Team Job ---
# Automatically runs on all branches to check code syntax
code_quality_check:
  stage: test
  tags:
    - shell       # Your runner tag on Servera
  script:
    - echo "Dev Team: Checking HTML syntax..."
    - test -f index.html || (echo "Missing index.html" && exit 1)
  only:
    - branches

# --- Ops L2 (Support) Job ---
# Deploys to a temporary folder to ensure connection works
deploy_to_staging:
  stage: deploy_staging
  tags:
    - shell
  script:
    - echo "Ops L2: Deploying to Staging area on Serverb..."
    - scp index.html $TARGET_USER@$TARGET_SERVER:/tmp/index_test.html
  environment:
    name: staging

# --- Ops L3 (Expert) Job ---
# ONLY runs on 'main' branch and requires MANUAL click
deploy_to_production:
  stage: deploy_prod
  tags:
    - shell
  script:
    - echo "Ops L3: Authorizing Production Deployment..."
    - echo "Installing NGINX if missing..."
    - ssh $TARGET_USER@$TARGET_SERVER "yum install -y nginx && systemctl enable --now nginx"
    - echo "Deploying file..."
    - scp index.html $TARGET_USER@$TARGET_SERVER:/usr/share/nginx/html/index.html
  environment:
    name: production
  when: manual      # <--- Key feature: L3 Expert must click this
  only:
    - main
```
### 6. Executing the Story (The Process)
- Step 1: Dev Pushes Code
### On your Mac:
```
git add .
git commit -m "Added homepage for Issue #1"
git push origin feature/dev-team-1
```
### Step 2: L1 Monitoring (The First Pipeline)

- Go to Build > Pipelines on GitLab.
- Ops L1 Observation: You will see a pipeline running for feature/dev-team-1.

### It will run code_quality_check and deploy_to_staging.

### If deploy_to_staging turns Green, L1 reports "Staging looks good."

### Step 3: Merge Request (Dev to Ops)

### Create a Merge Request from feature/dev-team-1 into main.

### Assign Reviewer: Ops L3.

### Step 4: The Production Deployment (Ops L3 Action)

- Merge the request. A new pipeline starts on main.
- The Pause: The pipeline will run test and deploy_staging, but it will stop at deploy_prod because it is set to manual.
- Ops L3 Action:
- Go to the Pipeline view.
- Click the Play button (▶) next to deploy_to_production.
- This simulates the Expert giving final approval.

## Step 5: Verification
#### Open your browser or curl from Servera:

```
curl http://10.10.10.19
```
## Output: <h1>DevOpsWala Status Portal</h1>
