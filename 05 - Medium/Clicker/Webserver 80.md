
# Clicker 
php application

## potential attack vectors

- check username xss (not  vulnerable)
- save_profile params (Role param vulnerable)


## assign admin role
- vulnerable to CRLF injection in the get parameter
- modify the param
```php
/save_game.php?clicks=32&level=1&role%0D%0A=Admin
```
- logout and login again to get the Administration page

[[05 - Medium/Clicker/Initial foothold]]