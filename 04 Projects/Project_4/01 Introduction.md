
# DevOps Master Course: From "Hello World" to Professional

- **Project Name:** DevOpsWala Status Portal
- **Instructor:** AI Mentor
- **Student:** Future DevOps Engineer

---

# Phase 1: The Foundation (Getting it Working)

**Objective:** Build a functional CI/CD pipeline where Developers push code and Ops deploy it manually to Production.

## 1. Infrastructure Setup (Prerequisites)
**Scenario:**
- **Servera (10.10.10.16):** GitLab Server + Shell Runner.
- **Serverb (10.10.10.19):** Target Production Server.
- **Local Machine:** MacBook (Developer Workstation).

**Step 1.1: Establish Trust (SSH)**
Run this on **Servera** as the \`gitlab-runner\` user to allow password-less access to Serverb:

### On Servera
```
sudo su - gitlab-runner
ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa
ssh-copy-id root@10.10.10.19
```
### Verify
```
ssh root@10.10.10.19 "uptime"
```

## 2.1 Team & Project Management
**Admin Task:** Create these users in GitLab and assign them to the project:
- Go to Admin Area (the wrench icon).
- Navigate to **Overview** > **Users**.
- Click **New User**.
- Manually create the user i.e. `dev_pawan` with a dummy email and password mentioned in the below tabel.
- Once created, return to your project and add them.
- **Manage** -> **Members** -> **Invite members**

| User | Role | Responsibility | Password |
| :--- | :--- | :--- | :---|
| `dev_pawan` | Developer | Writes HTML code. | Password@1234|
| `ops_monu` | Reporter | L1 Monitoring (Watches Pipelines). | Password@1234|
| `ops_support`| Developer | L2 Support (Fixes Staging). | Password@1234|
| `ops_lead` | Maintainer | L3 Expert (Approves Prod). | Password@1234|

## 2.2 Plan work in GitLab (Milestone, labels, issue)

### 2.2.1 **Create milestone**
**Manager Task:**  
Go to **Plan** → **Milestones** → **New milestone**
Title: `v1.0 Launch`.

---
### 2.2.2 Create labels
Go to **Manage** → **Labels** and create:
- Team::Dev
- Team::Ops-L1
- Team::Ops-L2
- Team::Ops-L3
- Status::Ready-for-Prod
---
### 2.2.3 Create the main issue (the “story task”)
Go to **Plan** ➡️  **Issues** ➡️  **New item**:
- **Title**: `Create Homepage for Dashboard`
- **Assignee**: `dev_pawan`
- **Labels**: `Team::Dev`
- **Milestone**: `v1.0 Launch`

<img width="1000" height="832" alt="Screenshot 2026-01-10 at 6 52 08 PM" src="https://github.com/user-attachments/assets/2cb1bfd7-0063-4230-baba-2c691643decb" />


---
### 2.2.4 Branching model (2 developer branches)
**Required branches**
- `feature/dev-team-1` (Developer 1 work)
- `feature/dev-team-2` (Developer 2 work)
- `main` (production)

**Developer workflow on MacBook**
- Clone repo, please note that if you are not using port forwarding then you should use only your Gitlab IP only (not with port):
```
git clone http://127.0.0.1:8080/learning_cicd1/status-portal.git
ls -ltr
cd status-portal
ls -ltr
git remote -v
```

- Create main and 2 feature branches :

```
git checkout -b main
git checkout -b feature/dev-team-1
git checkout -b feature/dev-team-2
git branch

```


---
### 2.2.5 App code `index.html`
- On feature/dev-team-1, create index.html:

#### Switch to branch ().
```
git switch feature/dev-team-1 
```
```
cat <<EOF > index.html
<!DOCTYPE html>
<html>
  <body>
    <h1>DevOpsWala Status Portal</h1>
    <p>System is Operational. Deployed by GitLab CI.</p>
  </body>
</html>
EOF
```


**Phase 1 Pipeline Code `.gitlab-ci.yml`:**
```yaml
cat <<EOF > .gitlab-ci.yml
stages:
  - test
  - deploy_staging
  - deploy_prod

variables:
  TARGET_SERVER: "10.10.10.19"
  TARGET_USER: "root"

code_quality:
  stage: test
  tags:
    - linux-runner
  script:
    - test -f index.html || exit 1

deploy_staging:
  stage: deploy_staging
  tags:
    - linux-runner
  script:
    - scp index.html $TARGET_USER@$TARGET_SERVER:/tmp/index_test.html

deploy_prod:
  stage: deploy_prod
  tags:
    - linux-runner
  script:
    - |
      echo "Ops L3: Installing NGINX and deploying to production path..."
      ssh $TARGET_USER@$TARGET_SERVER "sudo yum install -y nginx && sudo systemctl enable --now nginx"
      scp index.html $TARGET_USER@$TARGET_SERVER:/tmp/index_prod.html
      ssh $TARGET_USER@$TARGET_SERVER "sudo mv /tmp/index_prod.html /usr/share/nginx/html/index.html"
      ssh $TARGET_USER@$TARGET_SERVER "sudo restorecon -v /usr/share/nginx/html/index.html || true"
  when: manual
  only:
    - main
EOF
```
- Commit + push .gitlab-ci.yml on your feature branch:
```
git add .gitlab-ci.yml
git commit -m "Add CI/CD pipeline"
git push
```

## As per the developer, the code is working fine on `test` and `deploy_staging` stage.
### Step 1: Create a Merge Request (Dev to Ops)
- The developer (`dev_pawan`) needs to request that their code be merged into the production branch.

- Go to your project `status-portal` in GitLab.
- Expand **`Code`** and select **`Merge requests`**.
- Click **`Create merge request`**
- Under **Source branch** 
- Source branch: `feature/dev-team-1` -> Target branch: main.

<img width="1488" height="519" alt="Screenshot 2026-01-10 at 10 06 32 PM" src="https://github.com/user-attachments/assets/c4bf0951-333e-45b7-891c-142f394b160f" />

- Fill in the details:

	- **Title:** `Feature/dev team 1: Add Dashboard Homepage (Closes Issue #1)`
	- **Description**: `Added the initial index.html and CI/CD pipeline. Verified on staging.`
	- **Assignee**: `ops_lead` (The L3 Expert who approves Prod).
	- **Milestone**: Select `v1.0 Launch`.
	- **Labels**: Add `Status::Ready-for-Prod`.
	- Click **Create merge request**.


## GitLab login must be L3 user
## Step 2: **Review and Merge (Ops L3 Action)?**
- The Ops Lead (`ops_lead`) reviews the merges request.
- Log in (or act) as `ops_lead`.
- Go to Merge requests	and open the request created above.
- Review the changes (Files tab).
- `ops_lead` observed that developer used **root** user which is a security threat. So, he re-assing the task to `dev_pawan` and ask developer to update the code and use restricted user. 

# Below are the tasks that need to be done by Admin user **`ops_lead`** so that developer user will able to modify the Pipeline and perform her test. 


## Phase 2: The Professional Upgrade (Advanced Tasks)

**Objective:** Transform the basic pipeline into a secure, automated, and robust enterprise solution.

### Module 1: Security Hardening (Kill the Root User)
**Issue Title:** "Implement Least Privilege Deployment User"
**Assignee:** `ops_lead` (L3)

**Why:** Deploying as `root` is a security risk. We need a restricted user.

**Lab Steps:**
1.  **On Serverb (Target):** login as `root` user.
    ```bash
    useradd deployer
    ```
    ## Give the password `redhat`
    ```
    passwd deployer
    ```
    # Allow NGINX reload without password
    ```
    echo "deployer ALL=(root) NOPASSWD: /usr/bin/systemctl reload nginx, /usr/bin/cp" >> /etc/sudoers.d/deployer
    ```
3.  **On Servera (Runner)**: switch to root `gitlab-runner`. This user created when we add this node as a runner. 
    *   Update SSH keys to copy ID to `deployer@10.10.10.19`.
```
sudo su - gitlab-runner
cd .ssh/
ssh-copy-id deployer@10.10.10.19
```
4.  **Update CI/CD:** : Below task will be done by **`dev_pawan`** user as he is the developer. 
    *   Change `TARGET_USER` variable to `deployer`.
    *   Update script to use `sudo systemctl reload nginx` instead of full installation commands.

## Module 2: Artifact Management
**Issue Title:** "Implement Build Artifacts Strategy"
**Assignee:** `ops_support` (L2)

**Why:** We must ensure the *exact* file tested in Stage 1 is the file deployed in Stage 3. Don't rely on git checkout in every stage.

**Lab Steps:**
1.  Add a **Build** stage.
2.  Move the HTML file into a `public/` folder.
3.  Update YAML:
```yaml
build_app:
  stage: build
  script:
    - mkdir public
    - cp index.html public/
  artifacts:
    paths:
      - public/
    expire_in: 1 hour
```
4.  Update Deploy stages to use the file from `public/` folder.

## Module 3: Infrastructure as Code (Ansible)
**Issue Title:** "Migrate Deployment Scripts to Ansible"
**Assignee:** `ops_lead` (L3)

**Why:** Shell scripts are messy. Ansible is declarative and idempotent.

**Lab Steps:**
1.  **On Servera (Runner):** Install Ansible (`sudo dnf install ansible-core`).
2.  **Create `inventory.ini`:**
```ini
    [web]
    10.10.10.19 ansible_user=deployer
```
3.  **Create `deploy.yml` (Playbook):**
    *   Task 1: Install Nginx (requires root/sudo).
    *   Task 2: Copy `index.html` to `/usr/share/nginx/html`.
4.  **Update CI/CD:**
```yaml
    deploy_prod:
      script:
        - ansible-playbook -i inventory.ini deploy.yml
```

## Module 4: Quality Gates (Linting)
**Issue Title:** "Add HTML Linting to Test Stage"
**Assignee:** `dev_pawan` (Dev)

**Why:** Prevent broken code (syntax errors) from ever leaving the Dev branch.

**Lab Steps:**
1.  **Break the code:** Ideally, remove a `>` tag in `index.html`.
2.  **Update CI/CD Test Stage:**
    *   Install `tidy` on the runner (`sudo dnf install tidy`).
    *   Run: `tidy -e -q index.html` in the script.
3.  **Observe:** The pipeline fails immediately.
4.  **Fix:** Correct the HTML and watch it pass.

## Module 5: Feedback Loops (Notifications)
**Issue Title:** "Setup Discord/Slack Alerts"
**Assignee:** `ops_monu` (L1)

**Why:** Ops teams shouldn't stare at dashboards. They should be notified of failures.

**Lab Steps:**
1.  Create a Discord Server (Free).
2.  Go to Server Settings > Integrations > Webhooks > **Create Webhook**. Copy the URL.
3.  **Update CI/CD:**
    *   Add the Webhook URL as a CI/CD Variable (Masked).
    *   Add an `after_script` or `on_failure` block:
    ```yaml
    notify_fail:
      stage: .post
      script:
        - curl -H "Content-Type: application/json" -d '{"content": "Pipeline Failed! Check GitLab."}' \$DISCORD_WEBHOOK_URL
      when: on_failure
    ```

---

# Final Exam: The "Golden Run"
Once all modules are complete:

1.  **Dev** pushes clean code to a feature branch.
2.  **Linting** passes automatically.
3.  **Build** creates an artifact.
4.  **Ansible** deploys it to Staging.
5.  **Notification** is sent: "Staging Ready".
6.  **Ops L3** approves Production.
7.  **Ansible** deploys as `deployer` user.
8.  **Notification** is sent: "Production Live".

**Congratulations! You are now operating as a Junior DevOps Engineer.**

