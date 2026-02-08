
https://www.npmjs.com/package/serve?activeTab=readme

```
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
  before_script:
    - apk add curl
    - npm install -g serve
  script:
    - test -f webserver/index.html
    - serve webserver/ & 
    - sleep 5
    - curl http://localhost:3000 | grep "My website"
```
