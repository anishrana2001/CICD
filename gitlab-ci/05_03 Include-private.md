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


![alt text](<Screenshot 2026-01-13 at 9.33.52 PM.png>)