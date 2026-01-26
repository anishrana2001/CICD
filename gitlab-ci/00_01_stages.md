## Default Stages in GitLab CI/CD

If you do **not** define `stages:` in your `.gitlab-ci.yml`, GitLab CI/CD uses these **4 default stages**:

```yml
.pre
build
test
deploy
.post
```

## What Each Stage Means
- **.pre**: Runs before all other stages (good for linting or global setup).
- **build**: Used to compile/package your application.
- **test**: Runs unit/integration tests.
- **deploy**: Used to deploy your application.
- **.post**: Runs after all other stages (cleanup, notifications, etc.).

## Example Without Custom stages:
```yml
lint_code:
  stage: .pre
  script:
    - echo "Linting..."

build_app:
  stage: build
  script:
    - echo "Building..."

test_app:
  stage: test
  script:
    - echo "Testing..."

deploy_app:
  stage: deploy
  script:
    - echo "Deploying..."
```

## Best Practice
### It is usually better to define your own stages explicitly:
```yml
stages:        # Override defaults
  - install
  - build
  - test
  - deploy
```
