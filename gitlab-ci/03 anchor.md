



git clone http://127.0.0.1:8080/learning_cicd1/gitlab-ci_yaml/03-ancor.md.git
cd 03-ancor.md 
git switch --create main


cat <<EOF > .gitlab-ci.yml
stages:
  - build
  - deploy_staging
  - deploy_prod

build_file:
  stage: build
  script:
    - echo "Hi! Welcome to "devops-wala" > initial-file.txt
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
    - echo "Deploying database $DATABASE_TYPE on $CI_ENVIRONMENT_NAME ..."

job-01_prod:
  stage: deploy_prod
  environment:
    name: production
  variables:
    DATABASE_TYPE: "Mariadb"
    DB_NAME: "prod-db"
  script:
    - echo "Deploying database $DATABASE_TYPE on $CI_ENVIRONMENT_NAME ..."
    - echo "This should run ONLY on prod."
EOF

git add . ; git commit -m "added .gitlab-ci.yml" ; git push --set-upstream origin main



>.gitlab-ci.yml
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

git add . ; git commit -m "added double quotes" ; git push --set-upstream origin main
