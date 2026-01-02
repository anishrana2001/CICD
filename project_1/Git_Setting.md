


```
cd /Users/anishrana/GitTest
```
```
ls -a
```
```
mkdir project_1
```
```
cd project_1
```
```
git init
```

```
ls -a
```
```
vi index.html
```
```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Project_1: Web Page</title>
<link href="styles.css" rel="stylesheet">
</head>
<body>
<h1 class="title">My Website for Project_1</h1>
<p class="centered">Test Line1 for Project_1</p>
<p style="color: red; font-size: 1.5em;">Test Line2 for Project_1</p>
</body>
```
```
vi .gitlab-ci.yml
```
```
image: node:latest

stages:
  - test

test-job:
  stage: test
  script:
    # Option 1: Use npx (cleaner, no install needed if just running once)
    - npx html-validator-cli --file=index.html --verbose
    
    # Option 2: Local install (if you prefer explicit install)
    # - npm install html-validator-cli
    # - ./node_modules/.bin/html-validator --file=index.html --verbose
  only:
    - main
```

```
ls -la
```
```
git remote add origin  https://gitlab.com/anishrana2001/project_1.git
```

```
git add index.html .gitlab-ci.yml
```

```
git commit -m "First Commit"
```


```
git push origin main
```

<img width="1897" height="913" alt="Screenshot 2026-01-02 at 6 12 33 PM" src="https://github.com/user-attachments/assets/ecc3c95b-73d1-4f61-bea3-dc4d27457a18" />




```
anishrana@Anishs-MacBook-Pro GitTest % pwd
/Users/anishrana/GitTest
anishrana@Anishs-MacBook-Pro GitTest % ls -a
.	..
anishrana@Anishs-MacBook-Pro GitTest % mkdir project_1
anishrana@Anishs-MacBook-Pro GitTest % cd project_1 
anishrana@Anishs-MacBook-Pro project_1 % git init 
Initialized empty Git repository in /Users/anishrana/GitTest/project_1/.git/
anishrana@Anishs-MacBook-Pro project_1 % ls -a
.	..	.git
anishrana@Anishs-MacBook-Pro project_1 % vi index.html
anishrana@Anishs-MacBook-Pro project_1 % ls -la
total 8
drwxr-xr-x  4 anishrana  staff  128  2 Jan 17:30 .
drwxr-xr-x  3 anishrana  staff   96  2 Jan 17:28 ..
drwxr-xr-x  9 anishrana  staff  288  2 Jan 17:28 .git
-rw-r--r--  1 anishrana  staff  330  2 Jan 17:30 index.html
anishrana@Anishs-MacBook-Pro project_1 % git remote add origin  https://gitlab.com/anishrana2001/project_1.git
anishrana@Anishs-MacBook-Pro project_1 %
```
