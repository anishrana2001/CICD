```
cat <<EOF > .gitlab-ci.yml
stages:
  - build
  - deploy_staging
  - deploy_prod

include:
- local: 'gitlab-ci/build_stage.yaml'
- remote: 'https://raw.githubusercontent.com/anishrana2001/CICD/refs/heads/main/job_template.yaml'


job-01_staging_new:
  <<: *job_template          # <<: *job_template → loads (extends) all fields from the anchor job_template.
  stage: deploy_staging      # So it initially gets:`stage: deploy_prod` , Then you override this fields:
  environment:
    name: staging
  variables:
    DATABASE_TYPE: Mariadb
    DB_NAME: staging-db
EOF
```