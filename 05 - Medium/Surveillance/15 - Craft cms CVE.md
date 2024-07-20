

## version detection
![[Pasted image 20231210020634.png]]

## cve poc
https://github.com/zaenhaxor/CVE-2023-41892/blob/main/cve.sh

for further information
https://threatprotect.qualys.com/2023/09/25/craft-cms-remote-code-execution-vulnerability-cve-2023-41892/

![[Pasted image 20231210020548.png]]

## RCE
https://gist.github.com/gmh5225/8fad5f02c2cf0334249614eb80cbf4ce

Document root
`/var/www/html/craft/web`

![[Pasted image 20231211004222.png]]

Craftdb userpass
![[Pasted image 20231211004539.png]]
`craftuser:CraftCMSPassword2023!`