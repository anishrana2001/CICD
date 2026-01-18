## Make the Repo private again.
```
stages:
  - build
  - deploy_staging
  - deploy_prod

include:
  - local: 'build_stage.yaml'
  - project: 'learning_cicd1/project_2'
    file:
      - job-template.yaml

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

<img width="756" height="543" alt="Screenshot 2026-01-18 at 1 22 11 PM" src="https://github.com/user-attachments/assets/b447810d-82d0-4f66-bf30-2846a5308bb6" />



https://docs.gitlab.com/ci/yaml/includes/
