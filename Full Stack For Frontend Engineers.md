## Full Stack For Frontend Engineers

Notes from a front end master course - Full Stack For Frontend Engineers

__DNS__

IP Address - 127.0.0.1, IP stands for `Internet Protocol`.

DNS (Domain Name Systen) maps IP address to domains, you can think of it as an internet phonebook.

`ping` is the command to find out if your website is still up and running. Sometimes your website is not accessible through a domain name, this might be caused by the DNS server is down so your domain name cannot be resolved to an IP address.

DNS has many level of caches so you do not need to resolve a domain to an IP address every time. 

- Local cache is on your own computer
- Router cache is on your router
- LAN DNS cache is on the Local Area Network server
- ISP DNS cache is on the Internet Service Provider server 

DNS cache poisoning (DNS spoofing) means the DNS cache is changed maliciously. For example, dennisxiao.com originally points to 23.23.185.61, but it is changed to 104.24.234.22. Using HTTPS (Hyper Text Transfer Protocol Secure) is one way to mitigate DNS cache poisoning.

`traceroute` tells you how you get to a website.

__SSH__

There are two ways to use `ssh` to log in a server.

- by using username/password such as `ssh accountName@65.33.44.112`
- by using `ssh key`

Public Key Authentication

Your computer stores the private key while the server holds the public key.

Create an SSH Key

```
cd ~/.ssh/
ssh-keygen -t rsa  // -t rsa means use rsa type
```

Once you run the `ssh-keygen` command, two files will be generated, one is your public key with `.pub` extension, the other is your private key. You can copy your public key by `cat keyName.pub | pbcopy`.

__Server__

- Dedicated Server
- VPS (Virtual Private Server)

Normally we put our public key onto the VPS to leverage public key authentication.

Once you put your public key onto the VPS, now you can log in your server using the private key stored in your computer.

`ssh -i ~/.ssh/my_key USER_NAME@YOUR_SERVER_IP`. The `-i` flag here means identity.

Once you have logged in your server, you can update all your packages by running `apt-get update`.

You can create new user by using `adduser USER_NAME`. We can then assign the newly created user sudo permission by using `usermod -aG sudo USER_NAME`. `-aG` flag means adding to a group called sudo.

`su USER_NAME` to switch between users. (su means switch user)

Now we have a new user, we also need to add the public key to the server.

```
$ cat ~/.ssh/my_key.pub | ssh $USERNAME@$SERVER_IP "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

- `cat ~/.ssh/my_key.pub |` (prints out the public key and pipe that result to the next command)
- `ssh $USERNAME@$SERVER_IP` (ssh into the server)
- `"mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"` (create a .ssh folder and add the public key into the authorized_keys file)

Now we have set everything up, the new user can log in the server using its private SSH key. The next security fix is to disable root login.

Steps to disable root login
- login into your server
- run `sudo vi /etc/ssh/sshd_config`
- set `PermitRootLogin` to no
- restert ssh `sudo service sshd restart`

__Domain__

Now we have our server set up, we need to bind our server IP to a domain.

- Get the IP address of your VPS
- Add 2 `A Records` with your IP address, `@` and `www`

`A record` maps a name to one or more IP addresses.

`CNAME record` maps a name to another domain name.

`www` means when you visit www.xxxx.com, the domain would be resolved to your server IP.

`@` means when you visit xxxx.com, the domain would also be resolved to your server IP.
