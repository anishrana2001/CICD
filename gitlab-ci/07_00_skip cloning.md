## Skip cloning the git repo.

## Fork below project.
```
GitLab.com
```

## We have already login into the `glab`
```
mkdir 07_00_skip_cloning
cd 07_00_skip_cloning
git init
glab repo create --public --name "07_00_skip_cloning"
```

```
cat <<EOF > .gitlab-ci.yml
job1_medium:
  tags:
    - saas-linux-medium-amd64
  script:
    - ls -la
    - echo "This is my job1"

job2_medium:
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

