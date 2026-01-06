# DevOps Master Course: From "Hello World" to Professional

**Project Name:** DevOpsWala Status Portal
**Instructor:** AI Mentor
**Student:** Future DevOps Engineer

---

# Phase 1: The Foundation (Getting it Working)

**Objective:** Build a functional CI/CD pipeline where Developers push code and Ops deploy it manually to Production.

## 1. Infrastructure Setup (Prerequisites)
**Scenario:**
- **Servera (10.10.10.16):** GitLab Server + Shell Runner.
- **Serverb (10.10.10.19):** Target Production Server.
- **Local Machine:** MacBook (Developer Workstation).

**Step 1.1: Establish Trust (SSH)**
Run this on **Servera** as the `gitlab-runner` user to allow password-less access to Serverb:
```bash
# On Servera
sudo su - gitlab-runner
ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa
ssh-copy-id root@10.10.10.19
# Verify
ssh root@10.10.10.19 "uptime"
