## Full Stack For Frontend Engineers

Notes from a front end master course - Full Stack For Frontend Engineers by Jem Young

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

__SSH (Secure Shell)__

There are two ways to use `ssh` to log in a server.

- by using username/password such as `ssh userName@65.33.44.112`
- by using `ssh -i generatedPrivateKey userName@65.33.44.112`

Public Key Authentication

Your computer stores the private key while the server holds the public key.

Create an SSH Key

```
cd ~/.ssh/
ssh-keygen -t rsa  // '-t rsa' means to use rsa algorithm
```

Once you run the `ssh-keygen` command, two files will be generated, one is your public key with `.pub` extension, the other is your private key. You can copy your public key by `cat keyName.pub | pbcopy`.

__Server__

- Dedicated Server
- VPS (Virtual Private Server)

Normally we put our public key onto VPS to leverage public key authentication.

Once you put your public key onto VPS, now you can log in your server using the private key stored in your computer.

`ssh -i ~/.ssh/myPrivateKey USER_NAME@YOUR_SERVER_IP`. The `-i` flag here means identity.

Once you have logged in your server, you can update all your packages by running `apt-get update`.

You can create new user by using `adduser USER_NAME`. We can then assign the newly created user `sudo` permission by using `usermod -aG sudo USER_NAME`. `-aG` flag means adding to a group called sudo.

`su USER_NAME` to switch between users. (`su` means switch user, `sudo su` means to switch to root user)

Now we have a new user, we also need to add the public key to the server.

```
$ cat ~/.ssh/MyPublicKey.pub | ssh $USERNAME@$SERVER_IP "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
```

Command breakdown:

- `cat ~/.ssh/MyPublicKey.pub |` (prints out the public key and pipe that result to the next command)
- `ssh $USERNAME@$SERVER_IP` (ssh into the server)
- `"mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"` (create a .ssh folder and add the public key into the authorized_keys file)

Now we have set everything up, the new user can log in the server using its private SSH key. The next security fix is to disable root login.

Steps to disable root login and password login (only allow SSH login)
- login into your server
- run `sudo vi /etc/ssh/sshd_config`
- set `PermitRootLogin` to no
- set `PasswordAuthentication` to no
- restert OpenSSH server process `sudo service sshd restart`

__Domain__

Now we have our server set up, we need to bind our server IP to a domain.

- Get the IP address of your VPS
- Add 2 `A records` with your IP address, namely, `@` and `www`

`A record` maps a domain name to one or more IP addresses.

`CNAME record` maps a domain name to another domain name. It should only be used when there are no other records on that domain name.

`www` means when you visit `www.yourDomain.com`, the domain would be resolved to your server IP.

`@` means when you visit `yourDomain.com`, the domain would be resolved to your server IP.

__Nginx__

Nginx can act as a `reverse proxy`, which means it takes all the traffic from internet, and then route the traffic to appropriate servers.

Once you install Nginx by `apt-get install nginx`, you can start it `service start nginx`.

The default Nginx configuration is located at `/etc/nginx/sites-available/default`

Clone the Nodejs app into `/var/www` directory

Start a Nodejs app that is listening on port 3001, then set up Nginx as a proxy server.

```
location / {
  proxy_pass http://127.0.0.1:3001
}
```

Restart Nginx `service restart nginx`, now you should see your website by hitting your domain name.

This is how we link up everything

YourDomainName.com -> 23.23.182.23 (Your VPS IP) -> Nginx -> Node app

__Firewall__

You can set up a firewall on your server to control inbound and outbound traffic. On AWS, for example, you can set up a security group acts as a virtual firewall for your EC2 instance.

__Cron Job__

- `crontab -l` to list all the cron jobs for current user

- `sudo crontab -e` to open crontab for editing

- `00 1 * * 1 certbot renew` to renew certificate every week at 12pm on Monday
