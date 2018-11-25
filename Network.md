## Network

#### Fundamentals

TCP - reliable transport, if a packet is not acknowledged on the other end, it gets resent.

UDP - unreliable transport, packets are sent but there is no confirmation that the packet was received at the other end

#### Protocols

HTTP - how web servers and web browsers communicate (port 80)

Hypertext Transfer Protocol

We can try to use `netcat` or `curl` to send a request to a server.

```
nc google.com 80
GET / HTTP/1.0
Host: google.com
```

```
// curl the website in silent mode
curl -s google.com
```

- HTTPS - browser web pages with encryption (port 443)
- SMTP - send and receive emails (port 25)
- IMAP, POP3 - load emails from an inbox
- IRC - chat (port 6667)
- FTP - file transfer (port 21)
- SSH - remote shell over an encrypted connection (port 22)

A port is a number between 1 and 65535.

Run `nc -l 5000`, aka `netcat`, to launch a server on your machine listening to incoming connections on port 5000. After that, use `nc localhost 5000` to connect to your server. Now you can type messages in each session to have a chat with yourself. You can also open your browser and visit `localhost:9999`, your server should receive the request from your browser. Try to respond the request using a redirection.

```
HTTP/1.1 307 Temporary Redirect
Location: https://dennisxiao.com
```
