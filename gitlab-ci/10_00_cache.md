

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
git remote add origin git@gitlab.com:anishrana2001/10_00_cache.git
```


### Creating a simple 3 stages **`.gitlab-ci.yml`** file
```
cat <<EOF > .gitlab-ci.yml
stages:
  - install
  - build  
  - test

install:
  stage: install
  script:
    - npm install     # Downloads node_modules (5 minutes)
  cache:              # ✅ Reuses across pipelines
    paths:
      - node_modules/
  artifacts:          # ❌ Don't artifact downloads
    paths: []
build:
  stage: build
  script:
    - npm run build   # Creates dist/ folder (2 minutes)
  cache:              # ✅ Reuse node_modules from cache
    paths:
      - node_modules/
  artifacts:          # ✅ Pass built files to test job
    paths:
      - dist/
  dependencies:
    - install
test:
  stage: test
  script:
    - npm test       # Needs dist/ from build job
  dependencies:      # ✅ Downloads artifacts from build
    - build
EOF
```
### ACP ==> (A) Add, (C) Commit and (P) Push  the file.
```
git add . && git commit -m "Modify .gitlab-ci.yml file with without `needs`"
git push -u origin main
```

### **1st Pipeline**: install (5min ↓download) → build (2min) → test (1min)
### **2nd Pipeline**: install (30sec ↑cache hit) → build (2min) → test (1min)
