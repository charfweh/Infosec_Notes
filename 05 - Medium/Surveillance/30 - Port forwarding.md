![[Pasted image 20231211012930.png]]
8080 running
wget-ing it to find its running zoneminder a kind of monitoring application

so lets port forward

```bash
on target machine
./chisel_1.9.1_linux_amd64 client 10.10.16.18:1234 R:8081:127.0.0.1:8080
```


```bash
on attacking machine
./chisel_1.9.1_linux_amd64 server -p 1234 --reverse
```

![[Pasted image 20231211013057.png]]

suddenly we cant connect with zm
zmsessid b9r6jjttpkdtqmn31dnmeor7ir


## Zoneminder

Running linpeas on the box we get an interesting file
`cat /usr/share/zoneminder/www/api/app/Config/database.php` and in it had a password for zoneminder
![[Pasted image 20231211020613.png]]

also in bootstrap.php we find the version
![[Pasted image 20231217053634.png]]


## POC
https://github.com/heapbytes/CVE-2023-26035/blob/main/poc.py

Trigger the exploit
![[Pasted image 20231218024520.png]]


Get the shell
![[Pasted image 20231218024536.png]]

## POC analysis

What the exploit does is:
- gets the csrf token using `lxml`, its required to prevent csrf attacks 
- create the payload to hit the `snapshot action handler` by POST method with `monitor_ids` as our RCE 
- we get the reverse shell
as to why that works can be found in the zoneminder github issues,
https://github.com/ZoneMinder/zoneminder/security/advisories/GHSA-72rg-h4vf-29gr 
the main github issue talks about this, its an unauthenticated RCE in the snapshot function 

more information can be found here
https://vuldb.com/?id.221768


`zmuser:ZoneMinderPassword2023`

