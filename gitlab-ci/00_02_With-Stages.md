

```yaml
stages:
    - build
    - test

build:
    stage: build
    script:
        - mkdir build
        - echo "Welcome to devops-wala" > build/index.html

test:
    stage: test
    script:
        - echo "Testing"
        - test -f "build/index.html"
```
