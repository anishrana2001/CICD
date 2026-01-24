## Skip cloning the git repo.


## We have already login into the `glab`, SSHKEY is added 
==> ✔︎✔︎ [How to add SSHKEY](../GitLAB/SSHKEY.md)==> ✔︎✔︎ 
```
mkdir 07_00_skip_cloning
cd 07_00_skip_cloning
git init
glab repo create --public --name "07_00_skip_cloning"
git remote add origin git@gitlab.com:anishrana2001/07_00_skip_cloning.git

```

```
cat <<EOF > .gitlab-ci.yml
job1_with_cloning:
  tags:
    - saas-linux-medium-amd64
  script:
    - ls -la
    - echo "This is my job1"

job2_without_cloning:
  tags:
    - saas-linux-medium-amd64
  variables:
    GIT_STRATEGY: none
  script:
    - ls -la  
    - echo "This is my job2"
EOF
```
### Add, Commit and Push the file.
```
git add . && git commit -m "Creating a .gitlab-ci.yml file"
git push -u origin main
```

### You will not observe any differences. 



## Fork below project.
```
GitLab.com
```

### Update the **`.gitlab-cl.yml**` file.
```
job1_with_cloning:
  tags:
    - saas-linux-medium-amd64
  script:
    - ls -la
    - echo "This is my job1"

job2_without_cloning:
  tags:
    - saas-linux-medium-amd64
  variables:
    GIT_STRATEGY: none
  script:
    - ls -la  
    - echo "This is my job2"
```

<img width="1525" height="988" alt="Screenshot 2026-01-23 at 10 46 42 PM" src="https://github.com/user-attachments/assets/cf8b3daf-24d9-4a9e-8a46-25050aaa86c8" />
