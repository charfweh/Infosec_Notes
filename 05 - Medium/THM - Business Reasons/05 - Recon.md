```bash
nmap -sC -sV -p22,80 -oN business.nmap 10.10.218.113
```


```bash
PORT   STATE  SERVICE VERSION
22/tcp closed ssh
80/tcp open   http    Apache httpd 2.4.38 ((Debian))
| http-robots.txt: 1 disallowed entry 
|_/wp-admin/
|_http-generator: WordPress 5.4.2
|_http-title: MilkCo Test/POC site &#8211; Just another WordPress site
|_http-server-header: Apache/2.4.38 (Debian)
```