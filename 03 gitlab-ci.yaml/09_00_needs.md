## We have already login into the `glab`, SSHKEY is added 
✅✅ ✔︎✔︎ [How to add SSHKEY](../GitLAB/SSHKEY.md) ✔︎✔︎ ✅✅
```
mkdir 09_00_needs
cd 09_00_needs
git init
glab repo create --public --name "09_00_needs"
git remote add origin git@gitlab.com:anishrana2001/09_00_needs.git
```



### Creating a simple 3 stages **`.gitlab-ci.yml`** file (without `needs`)
```
cat <<EOF > .gitlab-ci.yml
stages:              # ✅✅✅ We have 3 stages.
  - build
  - test
  - prod
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
    - sleep 30

test_job1:
  stage: test
  image: node:22-alpine
  script:
    - test -f webserver/index.html

test_job2:
  stage: test
  image: node:22-alpine
  script:
    - test -f webserver/index.html


test_job3:
  stage: test
  image: node:22-alpine
  script:
    - test -f webserver/index.html

test_job4:
  stage: test
  image: node:22-alpine
  dependencies:
    - build_job1
  script:
    - test -f webserver/index.html

prod_job1:
  stage: prod
  image: alpine
  script:
    - echo "Deploying to production server..."
EOF
```
### ACP ==> (A) Add, (C) Commit and (P) Push  the file.
```
git add . && git commit -m "Modify .gitlab-ci.yml file with without `needs`"
git push -u origin main
```



### Modify the **`.gitlab-ci.yml`** file ( `needs`)
- Jobs `test_job1` , `test_job4` and `prod_job1` will not wait to complete all the jobs in Stage 1 i.e. build.


```
cat <<EOF > .gitlab-ci.yml
stages:              # We have 3 stages.
  - build
  - test
  - prod
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
    - sleep 30

test_job1:
  stage: test
  image: node:22-alpine
  needs:                 # ==> ✔︎
    - build_job1         # ==> ✔︎
  script:
    - test -f webserver/index.html

test_job2:
  stage: test
  image: node:22-alpine
  script:
    - test -f webserver/index.html


test_job3:
  stage: test
  image: node:22-alpine
  script:
    - test -f webserver/index.html

test_job4:
  stage: test
  image: node:22-alpine
  needs:                     # ✅✅ 
    - build_job1             # ✅✅
  dependencies:
    - build_job1
  script:
    - test -f webserver/index.html

prod_job1:
  stage: prod
  image: alpine
  needs:                      # ✅✅
    - build_job1              # ✅✅
  script:
    - echo "Deploying to production server..."
EOF
```
### ACP ==> (A) Add, (C) Commit and (P) Push  the file.
```
git add . && git commit -m "Modify .gitlab-ci.yml file with without `needs`"
git push -u origin main
```
