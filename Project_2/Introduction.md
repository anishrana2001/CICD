
### CICD installation on local Machine.
```
https://docs.gitlab.com/install/package/almalinux/?tab=Community+Edition
```

### Create a new user `devops` and assign the admin privileges. 

### How to disable sing-up page?
```
http://10.10.10.16/users/sign_in
```


#### How to download Chrome Browser on CentOS Stream 9

```
[student@servera ~]$ wget https://dl.google.com/linux/linux_signing_key.pub
--2026-01-04 09:20:30--  https://dl.google.com/linux/linux_signing_key.pub
Resolving dl.google.com (dl.google.com)... 216.58.196.110, 2404:6800:4002:810::200e
Connecting to dl.google.com (dl.google.com)|216.58.196.110|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 17382 (17K) [application/vnd.exstream-package]
Saving to: ‘linux_signing_key.pub’

linux_signing_key.pub         100%[=================================================>]  16.97K  --.-KB/s    in 0.002s  

2026-01-04 09:20:30 (9.54 MB/s) - ‘linux_signing_key.pub’ saved [17382/17382]

[student@servera ~]$ sudo rpm --import linux_signing_key.pub
[student@servera ~]$ wget https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm
--2026-01-04 09:21:23--  https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm
Resolving dl.google.com (dl.google.com)... 216.58.196.110, 2404:6800:4002:805::200e
Connecting to dl.google.com (dl.google.com)|216.58.196.110|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 120908153 (115M) [application/x-rpm]
Saving to: ‘google-chrome-stable_current_x86_64.rpm’

google-chrome-stable_current_ 100%[=================================================>] 115.31M  7.86MB/s    in 9.1s    

2026-01-04 09:21:33 (12.6 MB/s) - ‘google-chrome-stable_current_x86_64.rpm’ saved [120908153/120908153]

[student@servera ~]$ sudo dnf install -y google-chrome-stable_current_x86_64.rpm
gitlab_gitlab-ce                                                                        222  B/s | 833  B     00:03    
gitlab_gitlab-ce-source

```


<img width="1405" height="600" alt="Screenshot 2026-01-03 at 7 23 30 PM" src="https://github.com/user-attachments/assets/8f8809bc-4c48-4a3b-946c-ca462476b6a3" />


## Prerequisite:

### Step 1: Create a Keys (Private & Public) on local machine. (Windows, Linux or MAC).


### Step 2: Add the private key as a variable on GitLab server under desired project. 


### Step 3: Copy the Public key in the destination server. In my case, its a VM (CentOS Stream 9).



#### Step 1: Create a Keys (Private & Public) on local machine. (Windows, Linux or MAC).

```
anishrana@Anishs-MacBook-Pro ~ % cd GitTest 
anishrana@Anishs-MacBook-Pro GitTest % ls -ltr
total 0
drwxr-xr-x  6 anishrana  staff  192  2 Jan 20:26 project_1

anishrana@Anishs-MacBook-Pro GitTest % mkdir project_2
anishrana@Anishs-MacBook-Pro GitTest % cd project_2 

anishrana@Anishs-MacBook-Pro project_2 % ls -a 
.	..
anishrana@Anishs-MacBook-Pro project_2 % git init 
Initialized empty Git repository in /Users/anishrana/GitTest/project_2/.git/

anishrana@Anishs-MacBook-Pro project_2 % ls -a   
.	..	.git


anishrana@Anishs-MacBook-Pro project_2 % ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/anishrana/.ssh/id_rsa): /Users/anishrana/GitTest/project_2/id_rsa
Enter passphrase for "/Users/anishrana/GitTest/project_2/id_rsa" (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /Users/anishrana/GitTest/project_2/id_rsa
Your public key has been saved in /Users/anishrana/GitTest/project_2/id_rsa.pub
The key fingerprint is:
SHA256:uEZmzpbq42YWaBHYsAkzastGSAW7c3otzTGPFa3HR40 anishrana@Anishs-MacBook-Pro.local
The key's randomart image is:
+---[RSA 3072]----+
|=*o.             |
|*=+     .   o    |
|=+ .   . . E .   |
|+ +    .+ .      |
| * + o=oSo .     |
|. * =**o. .      |
| o o *B.         |
|  . *+           |
|   *+.           |
+----[SHA256]-----+

anishrana@Anishs-MacBook-Pro project_2 % ls -lta
total 16
-rw-r--r--  1 anishrana  staff   588  3 Jan 20:03 id_rsa.pub
drwxr-xr-x  5 anishrana  staff   160  3 Jan 20:03 .
-rw-------  1 anishrana  staff  2635  3 Jan 20:03 id_rsa
drwxr-xr-x  9 anishrana  staff   288  3 Jan 20:01 .git
drwxr-xr-x  4 anishrana  staff   128  3 Jan 20:01 ..
anishrana@Anishs-MacBook-Pro project_2 %
```

#### Step 2: Add the private key as a variable on GitLab server under desired project. 

<img width="1697" height="943" alt="Screenshot 2026-01-03 at 8 25 15 PM" src="https://github.com/user-attachments/assets/474a50cc-c921-4b2a-86e0-c69cb737f832" />

### Step 3: Copy the Public key in the destination server. In my case, its a VM (CentOS Stream 9).
