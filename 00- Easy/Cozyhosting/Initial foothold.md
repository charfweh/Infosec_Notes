We get a foothold as `app`

## enumeration
- from /etc/passwd we find two users `root` and `josh`
 - looking around we find the `cloudhosting-0.0.1.jar` file we transfer it over to our machine and we decode it 
`jar xf cloudhosting-0.0.1.jar`

we get a bunch of files basically our source code
we look for password and we find

`grep -Rnw . -e "password"`

![[Pasted image 20231125090637.png]]
we find the password for postgres in `application.properties` and also the username

## postgres table
```bash
psql -U postgres -h 127.0.0.1
```
Logging into the table the listing databases it shows `cozyhosting` then we use `cozyhosting` to find table `users` we select data from it to find hashes
https://www.commandprompt.com/education/postgresql-basic-psql-commands/ for 

### commands
```
\l - list db
\dt -
```
we find hash
`kanderson:$2a$10$E/Vcd9ecflmPudWeLSEIv.cvK6QjxjWlWXpij1NVNV3Mm6eH58zim`
`admin:$2a$10$SpKYdHLB0FOaT7n3x72wtuS0yR8uqqbNNpIPjUb2MZib3H9kVO8dm`

cracking the hash we get the hash for admin `manchesterunited`

lets login with josh ( from /etc/passwd) 

[[user.txt root.txt]]
