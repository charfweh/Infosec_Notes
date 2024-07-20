# martin
we get the foothold of the machine by ssh-ing into the machine
![[Pasted image 20231129070104.png]]
we find that there are three other local users, lets further enumerate the machine

![[Pasted image 20231129070157.png]]

we can run any commands as sudo
```bash
sudo -l
users and group
```

lets see the process list
cmd:
```bash
ps -ef --forest
```
o/p
![[Pasted image 20231201080415.png]]

let's see what ports are active 
```bash
ss -anltp
```
![[Pasted image 20231129075006.png]]
we see theres `3306 33060` which is `mysql` and `3000` port. we can curl it to confirm its a gitea instance

From here on, we port forward it
# chisel reverse port forward
```bash
on target machine
./chisel client 10.10.16.68:9001 R:3000:127.0.0.1:3000
```

```bash
on attacking machine
./chisel_1.9.1_linux_amd64 server -p 9001 --reverse
```

## gitea
![[Pasted image 20231129075116.png]]
well, we dont have any password for any user, so we are still missing a piece.
Now if we remember from the fuzzing we found a file that talked about databases in `/var/www/backups`, lets cd into it to find
## database file
![[Pasted image 20231129075234.png]]
the backups db are password protected and we couldnt crack it, but if we try the `db.sqlite3` db we can see theres a table called `accounts_customer`

![[Pasted image 20231129075348.png]]

![[Pasted image 20231129075640.png]]
The hash format is `124` `Django (SHA-1)`,we could only crack the hash for
`tom@drive.htb:[REDACTED]` but oh bummer, we cant ssh as tom.
lets try gitea creds for martinCruz since we know his username now and we get logged in 

![[Pasted image 20231129082656.png]]

![[Pasted image 20231129082712.png]]

we find the password for 7z zip file
`H@ckThisP@ssW0rDIfY0uC@n:)`
we transfer the db  in `/var/www/backups` files form target machine to our machine and use it to extract the db files. The passwords are under `accounts_customuser` table.

now, one thing here is the `1_Dec_db_backup.sqlite3.7z` had a different hash type and it was taking its sweet time to crack it so we skipped that one, the other hashes we found were able to crack
```bash
hash from oct sqlite
sha1$Ri2bP6RVoZD5XYGzeYWr7c$71eb1093e10d8f7f4d1eb64fa604e6050f8ad141:[REDACTED]

hash from Nov sqlite
sha1$Ri2bP6RVoZD5XYGzeYWr7c$4053cb928103b6a9798b2521c4100db88969525a:[REDACTED]
```

only one password works for tom and thats in `1_Nov_db_backup.sqlite3.7z` and we get user.txt

[[binary]]