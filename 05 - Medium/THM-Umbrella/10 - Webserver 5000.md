Looks like a docker registry

found
http://10.10.25.32:5000/v2/_catalog

```bash
curl -s http://10.10.182.157:5000/v2/_catalog
```
output
![[Pasted image 20240121084441.png]]

```bash
curl -s http://10.10.182.157:5000/v2/umbrella/timetracking/manifests/latest
```

output
![[Pasted image 20240121084354.png]]