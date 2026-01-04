### We will going to create two files, first is `index.html` which has our web server page and second one is our pipeline file i.e. `.gitlab-ci.yml` on our Local Machine. For me its a MAC.

- Index.html file.
```
cat <<EOF > index.html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Project_2: My First Web Page</title>
</head>
<body>
    <h1>Welcome to My Website under Project_2</h1>
    <p>This is my first paragraph on a basic HTML page.</p>
    <p style="color: red; font-size: 1.5em;">Test Line2 for Project_2</p>
</body>
</html>
EOF
```
- `.gitlab-ci.yml` file.
```
cat <<EOF > .gitlab-ci.yml
image: node:latest		# Using latest image
stages: 				# We have only TWO stages, i.e. test & deploy
	- test
	- deploy

test-job:				# Job Name is "test-job"
	stage: test
	script:
		- npx html-validator-cli --file=index.html --verbose
	only:
		- main
variables:
	SSH_PRIVATE_KEY: "$SSH_PRIVATE_KEY"    ## Added the variable on Gitlab page.
before_script:
	- yum update -y && yum install -y openssh-client nginx
	- mkdir -p ~/.ssh
	- echo "$SSH_PRIVATE_KEY" > ~/.ssh/id_rsa
	- chmod 400 ~/.ssh/id_rsa
    - systemctl enable --now nginx.service

deploy_to_server:
	stage: deploy
	script:
		- scp -i ~/.ssh/id_rsa -o StrictHostKeyChecking=no index.html student@10.10.10.16:/var/www/html/
	only:
		- main
EOF
```

### Remove the old GIT.
```
git remote remove origin
```

- If you are using NAT in VM, then you need to add the port forwarding.
```
VBoxManage natnetwork modify --netname Airtel-network --port-forward-4 "web_rule:tcp:[]:8080:[10.10.10.16]:80"
```
### Add new GitLAB in GIT.
- Below command is for port-forwarding option.
```
git remote add origin http://127.0.0.1:8080/learning_cicd/project_2.git
```
- Below command is for without port forwarding. Or if you are using any Cloud VM.
```

```
- Add the files in the staging area.
```
git add .
```
- Commit the files. 
```
git commit -m "adding 2 files"
```
```
- Push the files to GitLab.
```
git push --set-upstream origin main
```

```
anishrana@Anishs-MacBook-Pro project_2 % git remote remove origin
anishrana@Anishs-MacBook-Pro project_2 % git remote add origin http://127.0.0.1:8080/learning_cicd/project_2.git

anishrana@Anishs-MacBook-Pro project_2 % git add .

anishrana@Anishs-MacBook-Pro project_2 % git commit -m "adding 2 files"
[main (root-commit) 1f1e46e] adding 2 files
 4 files changed, 78 insertions(+)
 create mode 100644 .gitlab-ci.yml
 create mode 100644 id_rsa
 create mode 100644 id_rsa.pub
 create mode 100644 index.html
anishrana@Anishs-MacBook-Pro project_2 % 

anishrana@Anishs-MacBook-Pro project_2 % git push --set-upstream origin main
Username for 'http://127.0.0.1:8080': devops
Password for 'http://devops@127.0.0.1:8080': 
Enumerating objects: 6, done.
Counting objects: 100% (6/6), done.
Delta compression using up to 12 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (6/6), 3.35 KiB | 3.35 MiB/s, done.
Total 6 (delta 0), reused 0 (delta 0), pack-reused 0
To http://127.0.0.1:8080/learning_cicd/project_2.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
anishrana@Anishs-MacBook-Pro project_2 % vim .gitlab-ci.yml 

```
