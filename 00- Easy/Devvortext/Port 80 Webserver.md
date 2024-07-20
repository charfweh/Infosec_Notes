
![[Pasted image 20231127021159.png]]



# Subdomain
Cmd:
```bash
wfuzz -u http://devvortex.htb -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -H 'Host: FUZZ.devvortex.htb' --hw 10
```

O/p:
![[Pasted image 20231127021808.png]]

# gobuster
cmd:
```bash
gobuster dir -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt -t 100 -u http://devvortex.htb -oN dev.gobuster
```

o/p
![[Pasted image 20231127021845.png]]

[[dev.devvortex.htb]]