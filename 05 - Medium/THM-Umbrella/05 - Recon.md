
lfull port scan
```bash
PORT     STATE SERVICE
22/tcp   open  ssh
3306/tcp open  mysql
5000/tcp open  upnp
8080/tcp open  http-proxy
```

service scan
```bash
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 f0142fd6f6768c589a8e846ab1fbb99f (RSA)
|   256 8a52f1d6ea6d18b26f26ca8987c9496d (ECDSA)
|_  256 4b0d622a795ca07bc4f46c763c227ff9 (ED25519)
3306/tcp open  mysql   MySQL 5.7.40
| ssl-cert: Subject: commonName=MySQL_Server_5.7.40_Auto_Generated_Server_Certificate
| Not valid before: 2022-12-22T10:04:49
|_Not valid after:  2032-12-19T10:04:49
|_ssl-date: TLS randomness does not represent time
| mysql-info: 
|   Protocol: 10
|   Version: 5.7.40
|   Thread ID: 7
|   Capabilities flags: 65535
|   Some Capabilities: Support41Auth, Speaks41ProtocolOld, SupportsCompression, SupportsTransactions, LongColumnFlag, LongPassword, ODBCClient, IgnoreSpaceBeforeParenthesis, InteractiveClient, ConnectWithDatabase, DontAllowDatabaseTableColumn, SupportsLoadDataLocal, Speaks41ProtocolNew, FoundRows, SwitchToSSLAfterHandshake, IgnoreSigpipes, SupportsAuthPlugins, SupportsMultipleStatments, SupportsMultipleResults
|   Status: Autocommit
|   Salt: F1    \x15>\x13vMGa#k\x01tE9b\x0E)G
|_  Auth Plugin Name: mysql_native_password
5000/tcp open  http    Docker Registry (API: 2.0)
|_http-title: Site doesn't have a title.
8080/tcp open  http    Node.js (Express middleware)
|_http-title: Login
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
