## With Stages, below pipeline will ❌ fail. 

```yaml
stages:
    - build
    - test

build_aapache:
    stage: build
    script:
        - mkdir build
        - echo "Welcome to devops-wala" > build/index.html

test_aapache:
    stage: test
    script:
        - echo "Testing"
        - test -f "build/index.html"
```

### Both the jobs may use the same Gitlab runner or use differnet runner. 
- It means that jobs `build_aapache` and `test_aapache` will have separate container and may be separate runners too.
- Now, questions comes to our mind that how we can send data of first container to 2nd container?
- Answer is **`artifacts`**

```yaml
stages:
    - build
    - test

build_aapache:
    stage: build
    script:
        - mkdir build
        - echo "Welcome to devops-wala" > build/index.html
    artifacts:           # ✅ Line Added
        paths:           # ✅ Line Added
            - build/     # ✅ It will create a new dir `build` and other containers will see the content.
test_aapache:
    stage: test
    script:
        - echo "Testing"
        - test -f "build/index.html"
```


---
---

## ✅ build: passed | ⏳ test: pending | ❌ deploy: failed
