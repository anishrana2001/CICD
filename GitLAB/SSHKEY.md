# How to add SSHKEY on GitLAB?
## 1. Create a sshkey on your machine.
## 2. Paste the public Key on your Github Account.

### Step 1. Create a sshkey on your machine.

```
anishrana@Anishs-MacBook-Pro GitTest % pwd
/Users/anishrana/GitTest

anishrana@Anishs-MacBook-Pro GitTest % ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/Users/anishrana/.ssh/id_rsa): /Users/anishrana/GitTest/id_rsa    <<== I added this path.
Enter passphrase for "/Users/anishrana/GitTest/id_rsa" (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /Users/anishrana/GitTest/id_rsa
Your public key has been saved in /Users/anishrana/GitTest/id_rsa.pub
The key fingerprint is:
SHA256:H+f76GCz9VnmSyC2RMaf2Jfs0Bh0O/jrLw2Vvlfd/cs anishrana@Anishs-MacBook-Pro.local
The key's randomart image is:
+---[RSA 3072]----+
|             . . |
|          . . o .|
|           + o o.|
|          o + B.+|
|        S .=.B.B+|
|         .o+o *.*|
|          =.o  **|
|         . = +=*+|
|          ..+.+EB|
+----[SHA256]-----+
anishrana@Anishs-MacBook-Pro GitTest %


anishrana@Anishs-MacBook-Pro GitTest % ls -ltra     
drwxr-x---+ 27 anishrana  staff   864 Jan 24 14:12 ..
drwxr-xr-x  13 anishrana  staff   416 Jan 24 17:53 .
-rw-------   1 anishrana  staff  2622 Jan 24 17:53 id_rsa             <<======= File Generated.
-rw-r--r--   1 anishrana  staff   588 Jan 24 17:53 id_rsa.pub         <<======= File Generated.
anishrana@Anishs-MacBook-Pro GitTest %
```
### Open the public key.
```
anishrana@Anishs-MacBook-Pro GitTest % cat id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDp9d9z6CsYAC+scpyuhGnSBkuEhKLTtjtFTcvCY0mwF0Rn/tLNsCuAJVwRoLKFHTx6LplNnFxnzU22cl1xIbdvKexAVVqfUS+ye4cNfZsjrNmolK8BCAykyEgTO+nY/VloVIVtRFRSyGjs8JRa+EtTVWk/yoMQ9nZQtG7bLDCGsnJoJOQazgarCnAeiXBnMDF4oJfL9CEOeIwnVaqKVl692qKADaN7jZNqKXKuk6nyp26bYXFlzcx9p3TT4xhuDKneryCVOk4HXr7iCW9Wn3nwpGIx8O4EvXoY5LPsBubrX2HQjJD/MFmfn55X6OAMLCeRbf7XhT3qfRrVCvbZd107lzEFez23KWWqXD7hxqf+yZgGqile9LZosEcQGmB77RNdyNHCudU8HJkRR+W1fR3meSKcfSUiwr4w2Q+aHVlXdMHkrOZl/RpKskDb4GK/FFszhCepDcPepKPVQrv6v4xvJaL3XI+H0DiYuKg3Kl9JBk1wWsP9fYjKwrdbekAP3/U= anishrana@Anishs-MacBook-Pro.local
anishrana@Anishs-MacBook-Pro GitTest %
```
### Step 2. Paste the public Key on your Github Account.

`**User Menu`** ==> `**Edit profile`** ==> `**SSH Keys`** ==> `**Add new key`**

<img width="1874" height="954" alt="Screenshot 2026-01-24 at 5 55 25 PM" src="https://github.com/user-attachments/assets/6072cad0-1adc-44ea-87ce-353b6619b902" />



<img width="1874" height="993" alt="Screenshot 2026-01-24 at 6 05 01 PM" src="https://github.com/user-attachments/assets/80bb0f92-533d-42be-b397-bf10b5d24b14" />
