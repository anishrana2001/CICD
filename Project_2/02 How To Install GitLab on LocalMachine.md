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
- If you are using "Centos Stream 9" OS on the top of you Mac book then you need to follow these steps.
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


###
###
###
```
Here is the step-by-step guide to installing GitLab CE (Community Edition) and a GitLab Runner on your CentOS Stream 9 / RHEL 9 VM (servera).

Prerequisites
RAM: Ensure your VM has at least 4GB of RAM (8GB is recommended). GitLab is very resource-heavy.

Root Access: Run these commands as root (sudo -i).

Phase 1: Install GitLab Server
1. Install Dependencies & Enable SSH
bash
dnf install -y curl policycoreutils openssh-server perl
systemctl enable --now sshd
2. Configure Firewall
Allow HTTP and HTTPS traffic so you can access the web interface.

bash
firewall-cmd --permanent --add-service=http
firewall-cmd --permanent --add-service=https
firewall-cmd --reload
3. Add the GitLab Repository
bash
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
4. Install GitLab CE
This step will take a few minutes depending on your internet speed.

bash
dnf install -y gitlab-ce
5. Configure the URL
You need to configure the internal IP of your VM so both your Mac (via port forwarding) and the Runner can reach it.

Open the config file:

bash
vi /etc/gitlab/gitlab.rb
Find the line external_url.

Change it to your VM's internal NAT IP (usually 10.10.10.15 based on your previous logs).

ruby
external_url 'http://10.10.10.15'
(Do not use https yet to keep it simple).

6. Reconfigure GitLab
Apply the changes. This step will take 3–5 minutes.

bash
gitlab-ctl reconfigure
7. Retrieve the Initial Root Password
GitLab generates a random password for the root user.

bash
cat /etc/gitlab/initial_root_password
(Copy this password. It expires in 24 hours).

8. Access from your Mac
Since you have Port Forwarding (Mac 8080 -> VM 80), open your Mac browser and go to:
http://127.0.0.1:8080

Username: root

Password: (The one you copied in step 7)

Phase 2: Install and Register GitLab Runner
Now we install the runner on the same VM to execute your CI/CD pipelines.

1. Add Runner Repository
bash
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh" | sudo bash
2. Install the Runner
bash
dnf install -y gitlab-runner
3. Get Registration Token
Go to your GitLab UI on your Mac browser (http://127.0.0.1:8080).

Navigate to Menu (top left) -> Admin.

Go to CI/CD -> Runners.

Click New instance runner.

Add a tag (e.g., linux-runner), check "Run untagged jobs", and click Create runner.

Copy the Registration Token displayed on the screen.

4. Register the Runner
Run this command on your VM:

bash
gitlab-runner register
Answer the prompts as follows:

Enter the GitLab instance URL: http://10.10.10.15 (Use the internal VM IP).

Enter the registration token: (Paste the token from Step 3).

Enter a description for the runner: servera-runner.

Enter tags for the runner: shell,linux (optional).

Enter optional maintenance note: (Press Enter to skip).

Enter an executor: shell
(Note: We use shell for simplicity here so it runs commands directly on the VM. For production, docker is preferred).

5. Start the Runner
bash
gitlab-runner run
(It usually starts automatically as a service, but this ensures it runs).

Phase 3: Verification
Refresh the Runners page in your GitLab UI.

You should see a Green Circle next to your runner, indicating it is "Online".

You can now create a .gitlab-ci.yml file in your project, and this runner will pick up the jobs.
```
