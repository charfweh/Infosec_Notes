```bash
nmap -p- 10.10.11.213 --min-rate=2000 -vvv -oN formats.allports
```

```bash
PORT     STATE SERVICE REASON
22/tcp   open  ssh     syn-ack
80/tcp   open  http    syn-ack
3000/tcp open  ppp     syn-ack
```

```bash
nmap -sC -sV -p22,80,3000 -oN format.nmap 10.10.11.213
```

```bash
22/tcp   open  ssh     OpenSSH 8.4p1 Debian 5+deb11u1 (protocol 2.0)                                                                                                                    [0/10]
| ssh-hostkey: 
|   3072 c397ce837d255d5dedb545cdf20b054f (RSA) 
|   256 b3aa30352b997d20feb6758840a517c1 (ECDSA)
|_  256 fab37d6e1abcd14b68edd6e8976727d7 (ED25519)
80/tcp   open  http    nginx 1.18.0
|_http-server-header: nginx/1.18.0
|_http-title: 404 Not Found
3000/tcp open  http    nginx 1.18.0
|_http-title:  Microblog
|_http-server-header: nginx/1.18.0
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```