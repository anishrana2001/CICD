# How to use extends and why it is different from anchor?
## We have already login into the `glab`, SSHKEY is added 
✅ ✅  ✔︎✔︎ [How to add SSHKEY](../GitLAB/SSHKEY.md) ✔︎✔︎ ✅ ✅ 

```
mkdir 05-01_extends
cd 05-01_extends
git init
glab repo create --public --name "05-01_extends"
git remote add origin git@gitlab.com:anishrana2001/05-01_extends.git
```

## Test file.

```
cat <<EOF > .gitlab-ci.yml
stages:
  - build
  - deploy_staging
  - deploy_prod

build_file:
  stage: build
  script:
    - echo "Hi! Welcome to 'devops-wala' > initial-file.txt"
  artifacts:
    paths:
      - initial-file.txt

job-01_staging:
  stage: deploy_staging
  environment:
    name: staging
  variables:
    DATABASE_TYPE: "Mariadb"
    DB_NAME: "staging-db"
  script:
    - echo "Deploying database  on  ..."

job-01_prod:
  stage: deploy_prod
  environment:
    name: production
  variables:
    DATABASE_TYPE: "Mariadb"
    DB_NAME: "prod-db"
  script:
    - echo "Deploying database  on  ..."
    - echo "This should run ONLY on prod."
EOF
```

### Using extends as single key-value pair
```
stages:
  - build
  - deploy_staging
  - deploy_prod

build_file:
  stage: build
  script:
    - echo "Hi! Welcome to 'devops-wala' > initial-file.txt"
  artifacts:
    paths:
      - initial-file.txt
# ✅ Job name starts with a dot (.job-template) → hidden / not run directly.
.job-template:
  stage: deploy_prod
  environment:
    name: production
  variables:
    DATABASE_TYPE: "Mariadb"
    DB_NAME: "prod-db"
  script:
    - echo "Deploying database  on  ..."
    - if [[ "$CI_ENVIRONMENT_NAME" == "production" ]]; then echo "This should run ONLY on production env."; fi


job-01_staging_new:         # ✅ Creating a new job 'job-01_staging_new'
  extends: .job-template    # ✅ We use `extends` and call our hidden job.
  stage: deploy_staging     # ✅ Overwrite the variable
  environment:
    name: staging
  variables:
    DB_NAME: "staging-db"    # ❌ No need to define variable `DATABASE_TYPE: "Mariadb"`

job-01_production_new:       # ✅ Creating a new job 'job-01_production_new'
  extends: .job-template

```
### Using extends as a list of variable
```
cat <<EOF > .gitlab-ci.yml
stages:
  - build
  - deploy_staging
  - deploy_prod

build_file:
  stage: build
  script:
    - echo "Hi! Welcome to 'devops-wala' > initial-file.txt"
  artifacts:
    paths:
      - initial-file.txt

# Hidden Job 1: Variables & Stage
.job-template:
  stage: deploy_prod
  environment:
    name: production
  variables:
    DATABASE_TYPE: "Mariadb"
    DB_NAME: "prod-db"

# Hidden Job 2: The Script Logic
.script_template:
  script:
    - echo "Deploying database on..."
    - |
      if [[ "$CI_ENVIRONMENT_NAME" == "production" ]]; then
        echo "This should run ONLY on production env."
      fi

# Job using extends
job-01_staging_new:
  extends: 
    - .job-template
    - .script_template
  stage: deploy_staging
  environment:
    name: staging
  variables:
    DB_NAME: "staging-db"

# Job using extends
job-01_production_new:
  extends: 
    - .job-template
    - .script_template
EOF
```
