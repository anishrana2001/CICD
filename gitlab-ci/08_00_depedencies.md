## We have already login into the `glab`
```
mkdir 08_00_depedencies
cd 08_00_depedencies
git init
glab repo create --public --name "08_00_depedencies"
```
### Creating a simple 2 stage **`.gitlab-ci.yml`** file.
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
  script:
    - test -f webserver/index.html
EOF
```
### ACP ==> (A) Add, (C) Commit and (P) Push  the file.
```
git add . && git commit -m "Creating a .gitlab-ci.yml file"
git push -u origin main
```

### Creating a 2 stage **`.gitlab-ci.yml`** file with NO depedencies
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
  depedencies: []   # ==> ✔︎
  script:
    - test -f webserver/index.html
EOF
```
### ACP ==> (A) Add, (C) Commit and (P) Push  the file.
```
git add . && git commit -m "Modify .gitlab-ci.yml file with depedencies: []"
git push -u origin main
```
### Creating a 2 stage **`.gitlab-ci.yml`** file with how to **call depedencies**.
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

test_job1:
  stage: test
  image: node:22-alpine
  script:
    - test -f webserver/index.html

test_job2:
  stage: test
  image: node:22-alpine
  depedencies: []
  script:
    - test -f webserver/index.html


test_job3:
  stage: test
  image: node:22-alpine
  depedencies:
    - build_job2
  script:
    - test -f webserver/index.html
	

test_job4:
  stage: test
  image: node:22-alpine
  depedencies:
    - build_job1
  script:
    - test -f webserver/index.html
EOF
```
### ACP ==> (A) Add, (C) Commit and (P) Push  the file.
```
git add . && git commit -m "Modify .gitlab-ci.yml file with depedencies: []"
git push -u origin main
```

