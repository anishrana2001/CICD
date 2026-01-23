### GitLAB document link
```
https://docs.gitlab.com/ci/runners/hosted_runners/linux/
```

## **Example `.gitlab-ci.yml` file**
```
cat <<EOF>> > .gitlab-ci.yml
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
