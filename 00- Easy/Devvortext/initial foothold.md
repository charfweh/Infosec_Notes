# www-data

- from /etc/passwd we find `root,logan` are user accounts with bash
- from the lewis username and password we log in to myqsl
```bash
mysql -u lewis -p
```

## mysql 
`show databases;` -> shows 
![[Pasted image 20231127032152.png]]

`use joomla`, `show tables` -> shows
*snipped output*
![[Pasted image 20231127032239.png]]
`select * from sd4fg_users;` -> shows the hash

pop that hash to hashcat and after we crack it we get the password for logan

ssh -> `logan:tequieromucho`
[[logan]]