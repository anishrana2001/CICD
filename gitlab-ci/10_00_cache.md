

# GitLab CI: Cache vs Artifacts

## Key Differences

| Feature      | **Cache** 🎯                          | **Artifacts** 📦                       |
|--------------|---------------------------------------|----------------------------------------|
| **Purpose**  | Speed up same job next pipeline      | Pass files between jobs                |
| **Storage**  | Runner machine (fast)                | GitLab server                          |
| **Expires**  | Never (manual cleanup)               | 30 days default                        |
| **Download** | Optional, best-effort                | Required, guaranteed                   |
| **Example**  | `node_modules/`, `.gradle/`          | `dist/`, `coverage-report.html`        |



```
stages:
  - install
  - build  
  - test

install:
  stage: install
  script:
    - npm install     # Downloads node_modules (5 minutes)
  cache:            # ✅ Reuses across pipelines
    paths:
      - node_modules/
  artifacts:        # ❌ Don't artifact downloads
    paths: []
build:
  stage: build
  script:
    - npm run build  # Creates dist/ folder (2 minutes)
  cache:            # ✅ Reuse node_modules from cache
    paths:
      - node_modules/
  artifacts:        # ✅ Pass built files to test job
    paths:
      - dist/
  dependencies:
    - install
test:
  stage: test
  script:
    - npm test       # Needs dist/ from build job
  dependencies:     # ✅ Downloads artifacts from build
    - build
```

### **1st Pipeline**: install (5min ↓download) → build (2min) → test (1min)
### **2nd Pipeline**: install (30sec ↑cache hit) → build (2min) → test (1min)
