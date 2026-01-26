## We can also bind the runner with specific job.



```yaml
stages:
    - build
    - test

build_aapache:
    stage: build
    tags:                               # ✅ Line Added
        - saas-linux-small-arm64        # ✅ Line Added
    script:
        - mkdir build
        - echo "Welcome to devops-wala" > build/index.html
    artifacts:           
        paths:
            - build/
test_aapache:
    stage: test
    tags:                              # ✅ Line Added
        - gitlab-org-medium            # ✅ Line Added
    script:
        - echo "Testing"
        - test -f "build/index.html"
```

- Open the both jobs and check the runner name.
- Job must be succeeded.
- See the below PrintScreen for Runner tag names.

![alt text](image.png)