# We will going to create two files, first is `index.html` which has our web server page and second one is our pipeline file i.e. `.gitlab-ci.yml`.

- Index.html file.
```
cat <<EOF > index.html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>Project_1: My First Web Page</title>
</head>
<body>
    <h1>Welcome to My Website under Project_1</h1>
    <p>This is my first paragraph on a basic HTML page.</p>
    <p style="color: red; font-size: 1.5em;">Test Line2 for Project_1</p>
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
    - systemctl enable --now nginx.

deploy_to_server:
	stage: deploy
	script:
		- scp -i ~/.ssh/id_rsa -o StrictHostKeyChecking=no index.html student@10.10.10.16:/var/www/html/
	only:
		- main
EOF
