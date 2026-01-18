```
stages:
  - build
  - deploy_staging
  - deploy_prod

# Make sure, Repo, Group and Project are in public.
include:
  - local: 'build_stage.yaml'
  - project: 

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