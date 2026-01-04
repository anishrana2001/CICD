# CICD installation on local Machine.
```
https://docs.gitlab.com/install/package/almalinux/?tab=Community+Edition
```

- Create a new user `devops` and assign the admin privileges. 

- How to disable sing-up page?
```
http://10.10.10.16/users/sign_in
```

### How to enable the Runner ?
### If you are using "Centos Stream 9" OS on the top of you Mac book then you need to follow these steps.
#### Download the binary for your system
```
sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-arm64"
```
#### Grant execution permissions
```
sudo chmod +x /usr/local/bin/gitlab-runner
```
#### Install the runner as a system service

```
sudo /usr/local/bin/gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
```
#### Start the service
```
sudo /usr/local/bin/gitlab-runner start
```
### Verification 
```
/usr/local/bin/gitlab-runner --version
```

### Add the runner and select the VirtualBox 

### Referesh the page, you should see  your new runner on the GitLab page. Edit the runner, and select `Run untagged jobs`.


#### For your references.
```
[student@servera ~]$ sudo curl -L --output /usr/local/bin/gitlab-runner "https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-arm64"
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 98.3M  100 98.3M    0     0  1612k      0  0:01:02  0:01:02 --:--:-- 4826k
[student@servera ~]$ # Grant execution permissions
sudo chmod +x /usr/local/bin/gitlab-runner
[student@servera ~]$ # Install the runner as a system service
sudo /usr/local/bin/gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner

# Start the service
sudo /usr/local/bin/gitlab-runner start
Runtime platform                                    arch=arm64 os=linux pid=7277 revision=cc7f9277 version=18.7.1
Runtime platform                                    arch=arm64 os=linux pid=7346 revision=cc7f9277 version=18.7.1
[student@servera ~]$ /usr/local/bin/gitlab-runner --version
Version:      18.7.1
Git revision: cc7f9277
Git branch:   18-7-stable
GO version:   go1.24.11 X:cacheprog
Built:        2025-12-23T18:32:24Z
OS/Arch:      linux/arm64
[student@servera ~]$ 
```
