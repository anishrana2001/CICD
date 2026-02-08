# How to remove dependencies from jobs.

## We have already login into the `glab`, SSHKEY is added 
✅✅ ✔︎✔︎ [How to add SSHKEY](../GitLAB/SSHKEY.md) ✔︎✔︎ ✅✅
```
mkdir 08_00_dependencies
cd 08_00_dependencies
git init
glab repo create --public --name "08_00_dependencies"
git remote add origin git@gitlab.com:anishrana2001/08_00_dependencies.git
```
### Creating a simple 2 stage **`.gitlab-ci.yml`** file.
```
cat <<EOF > .gitlab-ci.yml
stages:              # ✅✅ We have 2 stages.
  - build
  - test

build_job1:
  stage: build
  image: alpine
  script:
    - mkdir webserver
    - echo 'My website "Devops-wala" ' > webserver/index.html
  artifacts:
    paths:
      - webserver/

test_job1:
  stage: test
  image: node:22-alpine
  script:
    - test -f webserver/index.html
EOF
```
### ACP ==> (A) Add, (C) Commit and (P) Push  the file.
```
git add . && git commit -m "Creating a .gitlab-ci.yml file"
git push -u origin main
```

###  ❌ ❌ Creating a 2 stage **`.gitlab-ci.yml`** file with NO dependencies
- So, this pipeline should be failed
```
cat <<EOF > .gitlab-ci.yml
stages:              # We have 2 stages.
  - build
  - test

build_job1:
  stage: build
  image: alpine
  script:
    - mkdir webserver
    - echo 'My website "Devops-wala" ' > webserver/index.html
  artifacts:
    paths:
      - webserver/

test_job1:
  stage: test
  image: node:22-alpine
  dependencies: []   # ==> ✔︎
  script:
    - test -f webserver/index.html
EOF
```
### ACP ==> (A) Add, (C) Commit and (P) Push  the file.
```
git add . && git commit -m "Modify .gitlab-ci.yml file with dependencies: []"
git push -u origin main
```
### Creating a 2 stage **`.gitlab-ci.yml`** file with how to **call dependencies**.
```
cat <<EOF > .gitlab-ci.yml
build_job1:
  stage: build
  image: alpine
  script:
    - mkdir webserver
    - echo 'My website "Devops-wala" ' > webserver/index.html
  artifacts:
    paths:
      - webserver/

build_job2:
  stage: build
  image: alpine
  script:
    - mkdir nginx_web
    - echo 'My website "Devops-wala" ' > nginx_web/index.html
  artifacts:
    paths:
      - nginx_web/

test_job1:                # ✅ This job will run as we are downloading the artifacts
  stage: test
  image: node:22-alpine
  script:
    - test -f webserver/index.html

test_job2:                # ❌ This job will be failed as we aren't downloading any artifacts.  
  stage: test
  image: node:22-alpine
  dependencies: []
  script:
    - test -f webserver/index.html


test_job3:                #  ❌ This job will also be failed as we aren't downloading right artifacts.
  stage: test
  image: node:22-alpine
  dependencies:
    - build_job2
  script:
    - test -f webserver/index.html

test_job4:                # ✅ This job will run sucessfully as we are downloading right artifacts.
  stage: test
  image: node:22-alpine
  dependencies:
    - build_job1
  script:
    - test -f webserver/index.html
EOF
```
### ACP ==> (A) Add, (C) Commit and (P) Push  the file.
```
git add . && git commit -m "Modify .gitlab-ci.yml file with dependencies: []"
git push -u origin main
```

## See the below print screen.

<img width="1821" height="954" alt="Screenshot 2026-01-24 at 2 16 52 PM" src="https://github.com/user-attachments/assets/1f411b9b-3e63-484c-9d29-895cf7481ea2" />
