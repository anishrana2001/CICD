
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

## 2. Team & Project Management
**Admin Task:** Create these users in GitLab and assign them to the project:

| User | Role | Responsibility |
| :--- | :--- | :--- |
| \`dev_pawan\` | Developer | Writes HTML code. |
| \`ops_monu\` | Reporter | L1 Monitoring (Watches Pipelines). |
| \`ops_support\`| Developer | L2 Support (Fixes Staging). |
| \`ops_lead\` | Maintainer | L3 Expert (Approves Prod). |

**Manager Task:** Create Milestone \`v1.0 Launch\`.

## 3. The Workflow (Phase 1)
**Issue #1:** "Create Homepage for Dashboard"
**Assignee:** \`dev_pawan\`

1.  **Dev:** Clones repo, branches to \`feature/dev-team-1\`, creates \`index.html\`.
2.  **Ops L3:** Creates \`.gitlab-ci.yml\` (Basic Version).

**Phase 1 Pipeline Code (\`.gitlab-ci.yml\`):**
\`\`\`yaml
stages:
  - test
  - deploy_staging
  - deploy_prod

variables:
  TARGET_SERVER: "10.10.10.19"
  TARGET_USER: "root"

code_quality:
  stage: test
  tags: [shell]
  script:
    - test -f index.html || exit 1

deploy_staging:
  stage: deploy_staging
  tags: [shell]
  script:
    - scp index.html \$TARGET_USER@\$TARGET_SERVER:/tmp/index_test.html

deploy_prod:
  stage: deploy_prod
  tags: [shell]
  script:
    - ssh \$TARGET_USER@\$TARGET_SERVER "yum install -y nginx && systemctl enable --now nginx"
    - scp index.html \$TARGET_USER@\$TARGET_SERVER:/usr/share/nginx/html/index.html
  when: manual
  only: [main]
\`\`\`

---

# Phase 2: The Professional Upgrade (Advanced Tasks)

**Objective:** Transform the basic pipeline into a secure, automated, and robust enterprise solution.

## Module 1: Security Hardening (Kill the Root User)
**Issue Title:** "Implement Least Privilege Deployment User"
**Assignee:** \`ops_lead\` (L3)

**Why:** Deploying as \`root\` is a security risk. We need a restricted user.

**Lab Steps:**
1.  **On Serverb (Target):**
    \`\`\`bash
    useradd deployer
    # Allow NGINX reload without password
    echo "deployer ALL=(root) NOPASSWD: /usr/bin/systemctl reload nginx, /usr/bin/cp" >> /etc/sudoers.d/deployer
    \`\`\`
2.  **On Servera (Runner):**
    *   Update SSH keys to copy ID to \`deployer@10.10.10.19\`.
3.  **Update CI/CD:**
    *   Change \`TARGET_USER\` variable to \`deployer\`.
    *   Update script to use \`sudo systemctl reload nginx\` instead of full installation commands.

## Module 2: Artifact Management
**Issue Title:** "Implement Build Artifacts Strategy"
**Assignee:** \`ops_support\` (L2)

**Why:** We must ensure the *exact* file tested in Stage 1 is the file deployed in Stage 3. Don't rely on git checkout in every stage.

**Lab Steps:**
1.  Add a **Build** stage.
2.  Move the HTML file into a \`public/\` folder.
3.  Update YAML:
\`\`\`yaml
build_app:
  stage: build
  script:
    - mkdir public
    - cp index.html public/
  artifacts:
    paths:
      - public/
    expire_in: 1 hour
\`\`\`
4.  Update Deploy stages to use the file from \`public/\` folder.

## Module 3: Infrastructure as Code (Ansible)
**Issue Title:** "Migrate Deployment Scripts to Ansible"
**Assignee:** \`ops_lead\` (L3)

**Why:** Shell scripts are messy. Ansible is declarative and idempotent.

**Lab Steps:**
1.  **On Servera (Runner):** Install Ansible (\`sudo dnf install ansible-core\`).
2.  **Create \`inventory.ini\`:**
    \`\`\`ini
    [web]
    10.10.10.19 ansible_user=deployer
    \`\`\`
3.  **Create \`deploy.yml\` (Playbook):**
    *   Task 1: Install Nginx (requires root/sudo).
    *   Task 2: Copy \`index.html\` to \`/usr/share/nginx/html\`.
4.  **Update CI/CD:**
    \`\`\`yaml
    deploy_prod:
      script:
        - ansible-playbook -i inventory.ini deploy.yml
    \`\`\`

## Module 4: Quality Gates (Linting)
**Issue Title:** "Add HTML Linting to Test Stage"
**Assignee:** \`dev_pawan\` (Dev)

**Why:** Prevent broken code (syntax errors) from ever leaving the Dev branch.

**Lab Steps:**
1.  **Break the code:** Ideally, remove a \`>\` tag in \`index.html\`.
2.  **Update CI/CD Test Stage:**
    *   Install \`tidy\` on the runner (\`sudo dnf install tidy\`).
    *   Run: \`tidy -e -q index.html\` in the script.
3.  **Observe:** The pipeline fails immediately.
4.  **Fix:** Correct the HTML and watch it pass.

## Module 5: Feedback Loops (Notifications)
**Issue Title:** "Setup Discord/Slack Alerts"
**Assignee:** \`ops_monu\` (L1)

**Why:** Ops teams shouldn't stare at dashboards. They should be notified of failures.

**Lab Steps:**
1.  Create a Discord Server (Free).
2.  Go to Server Settings > Integrations > Webhooks > **Create Webhook**. Copy the URL.
3.  **Update CI/CD:**
    *   Add the Webhook URL as a CI/CD Variable (Masked).
    *   Add an \`after_script\` or \`on_failure\` block:
    \`\`\`yaml
    notify_fail:
      stage: .post
      script:
        - curl -H "Content-Type: application/json" -d '{"content": "Pipeline Failed! Check GitLab."}' \$DISCORD_WEBHOOK_URL
      when: on_failure
    \`\`\`

---

# Final Exam: The "Golden Run"
Once all modules are complete:

1.  **Dev** pushes clean code to a feature branch.
2.  **Linting** passes automatically.
3.  **Build** creates an artifact.
4.  **Ansible** deploys it to Staging.
5.  **Notification** is sent: "Staging Ready".
6.  **Ops L3** approves Production.
7.  **Ansible** deploys as \`deployer\` user.
8.  **Notification** is sent: "Production Live".

**Congratulations! You are now operating as a Junior DevOps Engineer.**

