## SSH (Secure Shell)

#### Fundamentals

Connect to server using SSH and install Git

```
// ssh {user}@{host}
// ssh into your server
ssh root@126.34.44.12

// install Git and Node.js on Linux server
sudo apt-get install git nodejs
```

Copy some files from your laptop to server

```
// cd into your site and copy all the files to server
~/D/yourSite rsync -av . root@167.34.44.12:~/projects/dennisxiao.com
```

#### Symmetrical Encryption

#### Asymmetrical Encryption

#### Hashing
