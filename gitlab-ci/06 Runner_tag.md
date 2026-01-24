### GitLAB document link 🍺 ==>  ✔︎
```
https://docs.gitlab.com/ci/runners/hosted_runners/linux/
```
### First login into glab account.
```
glab auth status
```
```
glab auth login
```
```
anishrana@Anishs-MacBook-Pro % glab auth login
- Signing into gitlab.com
- glab config set -h gitlab.com git_protocol https
✓ Configured Git protocol.
- glab config set -h gitlab.com api_protocol https
✓ Configured API protocol.
✓ Logged in as anishrana2001
✓ Configuration saved to /Users/anishrana/Library/Application Support/glab-cli/config.yml
anishrana@Anishs-MacBook-Pro % 
```

## Create a Project from **`git`**
## We have already login into the `glab`, SSHKEY is added 
==> ✔︎✔︎ [How to add SSHKEY](../GitLAB/SSHKEY.md)==> ✔︎✔︎ 
```
mkdir 06_Runner_tag
cd 06_Runner_tag
git init
glab repo create --public --name "06_Runner_tag"
git remote add origin git@gitlab.com:anishrana2001/06_Runner_tag.git

```

## **Example `.gitlab-ci.yml` file**
```
cat <<EOF > .gitlab-ci.yml
job_small:
  script:
    - echo "This job is untagged and runs on the default small Linux x86-64 instance"

job_medium:
  tags:
    - saas-linux-medium-amd64
  script:
    - echo "This job runs on the medium Linux x86-64 instance"

job_large:
  tags:
    - saas-linux-large-arm64
  script:
    - echo "This job runs on the large Linux Arm64 instance"
EOF
```
### Add, Commit and Push the file.
```
git add . && git commit -m "Creating a .gitlab-ci.yml file"
git push -u origin main
```

### Runners with the gitlab-org tag do not run in privileged mode and cannot be used for Docker-in-Docker builds.
- Privileged mode grants the job container full host-level access, enabling:
- Docker daemon startup inside the job (service docker start)
- docker build, docker run from within CI jobs
- Kubernetes-style pod spawning
