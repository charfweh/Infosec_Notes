Nmap
```bash
nmap -sC -sV -p22,80,289,443,5667 10.10.11.248 -oN monitored.nmap
```
Output
```bash
PORT     STATE  SERVICE    VERSION
22/tcp   open   ssh        OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 61e2e7b41b5d46dc3b2f9138e66dc5ff (RSA)
|   256 2973c5a58daa3f60a94aa3e59f675c93 (ECDSA)
|_  256 6d7af9eb8e45c2026ad58d4db3a3376f (ED25519)
80/tcp   open   http       Apache httpd 2.4.56
|_http-title: Did not follow redirect to https://nagios.monitored.htb
289/tcp  closed unknown
443/tcp  open   ssl/http   Apache httpd 2.4.56 ((Debian))
|_ssl-date: TLS randomness does not represent time
|_http-title: 400 Bad Request
|_http-server-header: Apache/2.4.56 (Debian)
| tls-alpn: 
|_  http/1.1
| ssl-cert: Subject: commonName=nagios.monitored.htb/organizationName=Monitored/stateOrProvinceName=Dorset/countryName=UK
| Not valid before: 2023-11-11T21:46:55
|_Not valid after:  2297-08-25T21:46:55
5667/tcp open   tcpwrapped
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

