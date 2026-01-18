## Added the 2nd job in the `State: test`
## It should generate the error message while running the pipeline.
```
cat <<EOF > .gitlab-ci.yml
image: node:latest   # Using latest image

stages:              # We have only one stage, i.e. test
  - test

test-job1:           # Job Name is "test-job1"
  stage: test        # This job run under "test" stage.
  script:
    - npx html-validator-cli --file=index.html --verbose
	
test-job2:             # 2nd Job
  stage: test          # This job run under "test" stage.
  image: node:latest   # Use the same environment for consistency
  script:
    - echo "Running tests..."
    - npm install
    - npm run test
  
  only:
    - main       # It will use "main" branch.
EOF
```
#### Add, Commit (if needed) and then push the file.
```
git add  .gitlab-ci.yml ; git commit -m "2nd Job added" ; git push origin main
```
### Wait and watch the pipeline.
---


## Remove the hidden character.
#### Now, it should run the pipeline perfectly but only for job1.

```
vim .gitlab-ci.yml
```
```
:se list
```

#### Add, Commit (if needed) and then push the file.
```
git add  .gitlab-ci.yml ; git commit -m "2nd Job added & removed hidden character" ; git push origin main
```
### Wait and watch the pipeline.
---

## Now, correct the 2nd job in `.gitlab-ci.yml` file.
```
cat <<EOF > .gitlab-ci.yml
image: node:latest   # Using latest image

stages:              # We have only one stage, i.e. test
  - test

test-job1:           # Job Name is "test-job1"
  stage: test        # This job run under "test" stage.
  script:
    - npx html-validator-cli --file=index.html --verbose


test-job2:             # 2nd Job
  stage: test          # This job run under "test" stage.
  image: node:latest   # Use the same environment for consistency
  script:
    - echo "Running tests..."
  only:
    - main       # It will use "main" branch.
EOF
```
### npm (Node Package Manager): Used primarily to install and manage dependencies (e.g., npm install, npm ci). It permanently downloads packages into a node_modules folder.
### npx (Node Package Execute): Used to execute binaries or one-off scripts without permanently installing them.

### Add, Commit (if needed) and then push the file.
```
git add  .gitlab-ci.yml ; git commit -m "2nd Job added & removed the npm package lines" ; git push origin main
```

### Wait and watch the pipeline.
