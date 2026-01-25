

# GitLab CI: Cache vs Artifacts

## Key Differences

| Feature      | **Cache** 🎯                          | **Artifacts** 📦                       |
|--------------|---------------------------------------|----------------------------------------|
| **Purpose**  | Speed up same job next pipeline      | Pass files between jobs                |
| **Storage**  | Runner machine (fast)                | GitLab server                          |
| **Expires**  | Never (manual cleanup)               | 30 days default                        |
| **Download** | Optional, best-effort                | Required, guaranteed                   |
| **Example**  | `node_modules/`, `.gradle/`          | `dist/`, `coverage-report.html`        |



## We have already login into the `glab`, SSHKEY is added 
✅✅ ✔︎✔︎ [How to add SSHKEY](../GitLAB/SSHKEY.md) ✔︎✔︎ ✅✅
```
mkdir 10_00_cache
cd 10_00_cache
git init
glab repo create --public --name "10_00_cache"
git remote set-url origin git@gitlab.com:anishrana2001/10_00_cache.git
```

### Creating a simple 3 stages **`.gitlab-ci.yml`** file
```
cat <<EOF > .gitlab-ci.yml
image: node:latest  # ✅ Global image (DRY principle)

stages:
  - install
  - build
  - test

install:
  stage: install
  script:
    - npm install
  cache:  # ✅ Perfect for node_modules
    paths:
      - node_modules/

build:
  stage: build
  script:
    - npm run build  # Now exists
  cache:
    paths:
      - node_modules/
  artifacts:  # ✅ Passes to test
    paths:
      - dist/
    expire_in: 1 hour

test:
  stage: test
  script:
    - npm test  # Now exists
  dependencies:
    - build
EOF
```

### Creating a **`.package.json`** file so that we can install the npm
```
cat <<EOF > package.json
{
  "name": "10_00_cache",
  "version": "1.0.0",
  "scripts": {
    "build": "mkdir -p dist && echo 'Built app' > dist/index.html",
    "test": "ls dist/ && echo 'Tests passed!'"
  },
  "devDependencies": {
    "lodash": "^4.17.21"
  }
}
EOF
```
### ACP ==> (A) Add, (C) Commit and (P) Push  the file.
```
git add . && git commit -m "Modify .gitlab-ci.yml file with package.json"
git push -u origin main
```


### **1st Pipeline**: install (5min ↓download) → build (2min) → test (1min)
### **2nd Pipeline**: install (30sec ↑cache hit) → build (2min) → test (1min)
