```
stages:
  - build
  - deploy_staging
  - deploy_prod

# Make sure, Repo, Group and Project are in public.
include:
  - local: 'build_stage.yaml'
  - remote: 'https://raw.githubusercontent.com/anishrana2001/CICD/refs/heads/main/job_template.yaml'

job-01_staging_new:
  extends: .job-template
  stage: deploy_staging
  environment:
    name: staging
  variables:
    DATABASE_TYPE: Mariadb
    DB_NAME: staging-db
EOF
```