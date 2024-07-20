```bash
PORT      STATE    SERVICE REASON
21/tcp    open     ftp     syn-ack
22/tcp    open     ssh     syn-ack
80/tcp    open     http    syn-ack
41312/tcp filtered unknown no-response

```

```bash
PORT      STATE    SERVICE VERSION
21/tcp    open     ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0             318 Mar 14  2023 update.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.17.55.0
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp    open     ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 47712b907d89b8e9b46a76c1504943cf (RSA)
|   256 cb2997dcfd85d9eaf884980b66105e6f (ECDSA)
|_  256 123f3892a7ba7fdaa7184f0dff56c11f (ED25519)
80/tcp    open     http    Apache httpd 2.4.41 ((Ubuntu))
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: Welcome!!
41312/tcp filtered unknown
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

