### We can combine both stages if possible. 
```
stages:
  - build

build_job1:
  stage: build
  image: node:22-alpine
  before_script:
    - apk add curl
    - npm install -g serve
  script:
    - mkdir webserver
    - echo 'My website "Devops-wala" ' > webserver/index.html
    - test -f webserver/index.html
    - serve webserver/ & 
    - sleep 5
    - curl http://localhost:3000 | grep "My website"
```
