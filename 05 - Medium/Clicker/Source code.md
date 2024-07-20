## Vulnerable code

```php
<?php

session_start();

include_once("db_utils.php");

  

if (isset($_SESSION['PLAYER']) && $_SESSION['PLAYER'] != "") {

$args = [];

foreach($_GET as $key=>$value) {
// THIS IS VULNERABLE
if (strtolower($key) === 'role') {

// prevent malicious users to modify role

header('Location: /index.php?err=Malicious activity detected!');

die;

}

$args[$key] = $value;

}

save_profile($_SESSION['PLAYER'], $_GET);

// update session info

$_SESSION['CLICKS'] = $_GET['clicks'];

$_SESSION['LEVEL'] = $_GET['level'];

header('Location: /index.php?msg=Game has been saved!');

}

?>
```

The checker for admin role is vulnerable to crlf carriage return and line feed basically it means were feeding in a new line as to deceive the webserver its a clean code

[[Webserver 80]]