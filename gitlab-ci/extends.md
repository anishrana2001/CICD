# How to use extends and why it is different from anchor?


```
cat <<EOF > .gitlab-ci.yml
stages:                   # Defines the pipeline order: first build, then deploy_staging, then deploy_prod.
  - build.                # Jobs assigned to `build` run `first`, then `staging jobs`, then `production jobs.`
  - deploy_staging
  - deploy_prod

build_file:
  stage: build
  script:
    - echo "Hi! Welcome to 'devops-wala' > initial-file.txt"
  artifacts:
    paths:
      - initial-file.txt

# Job name starts with a dot (.job-template) → hidden / not run directly.
.job-template: &job_template  # &job_template creates a YAML anchor named job_template that can be reused.
  stage: deploy_prod
  environment:
    name: production
  variables:
    DATABASE_TYPE: "Mariadb"
    DB_NAME: "prod-db"
  script:
    - echo "Deploying database  on  ..."
    - if [[ "$CI_ENVIRONMENT_NAME" == "production" ]]; then echo "This should run ONLY on production env."; fi

job-01_staging_new:
  extends: .job_template     # we can also use extends
  stage: deploy_staging      # So it initially gets:`stage: deploy_prod` , Then you override this fields:
  environment:
    name: staging
  variables:                 # We don't need to use "DATABASE_TYPE: Mariadb"
    DB_NAME: staging-db

job-01_production_new:   # &job_template creates a YAML anchor named job_template that can be reused.
  <<: *job_template.     # <<: *job_template → loads (extends) all fields from the anchor job_template.
EOF
```