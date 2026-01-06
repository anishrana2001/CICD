# DevOps_Project_Guide.md
## Project: DevOpsWala Status Portal (GitLab CI/CD End-to-End)

### Story
The Development team will build a simple static web page (`index.html`) that shows system status, and the Operations team will deploy it using GitLab CI/CD.  
- GitLab + Runner (shell executor) runs on **Servera: 10.10.10.16**  
- The application will be installed/deployed on **Serverb: 10.10.10.19**  
- Developers use **MacBook** as the local Git workstation

---

## Team structure (Dev + Ops)
### Dev Team
- **Developers**: Write code, push to feature branches, raise Merge Requests (MRs).

### Ops Team
- **L1 (Monitoring)**: Monitors pipeline health, reports failures (no fixes).
- **L2 (Support)**: Investigates failed jobs/logs, fixes common issues, validates staging.
- **L3 (Expert / Ops Lead)**: Final approver, triggers manual production deployment, controls protected branches.

---

## 1) User creation + project access (Admin tasks)

### 1.1 Create users in GitLab (Admin)
Create these users from GitLab **Admin Area → Users → New user**:

| Team | Name | Username | Suggested Role in Project |
|---|---|---|---|
| Dev | Pawan | `dev_pawan` | Developer |
| Ops L1 | Monu | `ops_l1_monu` | Reporter |
| Ops L2 | Sam | `ops_l2_sam` | Developer |
| Ops L3 | Lead | `ops_l3_lead` | Maintainer |

> Notes:
> - Keep passwords as per your lab policy (example: `password123`).
> - Email can be dummy (example: `pawan@lab.com`).

### 1.2 Create project and add members (Maintainer/Admin)
1. Create project: **`status-portal`**
2. Go to **Project → Manage → Members → Invite members**
3. Add users and assign roles as per the table above.

### 1.3 (Recommended) Protect `main` branch (Maintainer/Admin)
1. Go to **Project → Settings → Repository → Protected branches**
2. Protect `main` so that:
   - Only **Maintainers** can merge/push to `main` (Ops L3)
   - Developers work only via Merge Requests

This makes the workflow realistic: Dev creates branches; Ops L3 controls production.

---

## 2) Infrastructure prerequisites (one-time)

### 2.1 SSH access: Runner (Servera) → Target (Serverb)
Goal: the shell runner on Servera must SSH to Serverb without password prompts.

On **Servera (10.10.10.16)**:
```bash
sudo su - gitlab-runner
ssh-keygen -t rsa -N "" -f ~/.ssh/id_rsa
