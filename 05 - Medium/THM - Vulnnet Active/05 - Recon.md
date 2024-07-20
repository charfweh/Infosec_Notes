```bash
nmap -sC -sV -vvv -p- -Pn 10.10.190.115 -oN active.nmap
```

```bash
PORT      STATE SERVICE       REASON  VERSION                                                                                                                                                 
53/tcp    open  domain        syn-ack Simple DNS Plus                                                                                                                                         
135/tcp   open  msrpc         syn-ack Microsoft Windows RPC                                                                                                                                   
139/tcp   open  netbios-ssn   syn-ack Microsoft Windows netbios-ssn                                                                                                                           
445/tcp   open  microsoft-ds? syn-ack                                                                                                                                                         
464/tcp   open  kpasswd5?     syn-ack                                                                                                                                                         
6379/tcp  open  redis         syn-ack Redis key-value store 2.8.2402                                                                                                                          
9389/tcp  open  mc-nmf        syn-ack .NET Message Framing                                                                                                                                    
49665/tcp open  msrpc         syn-ack Microsoft Windows RPC                                                                                                                                   
49667/tcp open  msrpc         syn-ack Microsoft Windows RPC                                                                                                                                   
49669/tcp open  msrpc         syn-ack Microsoft Windows RPC                                                                                                                                   
49670/tcp open  ncacn_http    syn-ack Microsoft Windows RPC over HTTP 1.0                                                                                                                     
49683/tcp open  msrpc         syn-ack Microsoft Windows RPC                                                                                                                                   
49696/tcp open  msrpc         syn-ack Microsoft Windows RPC
49718/tcp open  msrpc         syn-ack Microsoft Windows RP
```

