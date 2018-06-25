## SSH (Secure Shell)

#### Connect to a server using SSH

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

#### Generate a public/private key pair

```
// generate a pair of keys
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"

// copy the public key
pbcopy < ~/.ssh/id_rsa.pub

// add the public key to ssh-agent
ssh-add ~/.ssh/id_rsa.pub
```
