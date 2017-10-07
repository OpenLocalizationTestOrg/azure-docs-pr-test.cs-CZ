---
title: "aaaConnect aplikace tooAzure databáze pro databázi MySQL | Microsoft Docs"
description: "Tento dokument uvádí hello aktuálně podporované připojovací řetězce pro tooconnect aplikací s Azure Database pro databázi MySQL, včetně ADO.NET (C#), JDBC, Node.js, rozhraní ODBC, PHP, Python a Ruby."
services: mysql
author: mswutao
ms.author: wuta
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 06/12/2017
ms.openlocfilehash: bbcb2c0ddb4f8e5c225ebef53781e073f5c7b1a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconnect-applications-tooazure-database-for-mysql"></a>Jak aplikace tooAzure tooconnect databáze pro databázi MySQL
Tento dokument uvádí hello připojovací řetězec typy, které podporuje Azure databáze pro databázi MySQL, spolu s šablonami a příklady. V připojovacím řetězci může mít různé parametry a různá nastavení.

- tooobtain hello certifikátu, najdete v části [jak tooconfigure SSL](./howto-configure-ssl.md).
- {your_host} = <servername>. mysql.database.azure.com
- {your_user}@{servername} správně = formát ID uživatele pro ověřování.  Použití právě hello userID způsobí toofail ověřování hello.

## <a name="adonet"></a>ADO.NET
```ado.net
Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password};[SslMode=Required;]
```

V tomto příkladu je název serveru hello `myserver4demo`, název databáze je `wpdb`, uživatelské jméno je `WPAdmin`, a heslo je `mypassword!2`. Potom by měla být hello připojovací řetězec:

```ado.net
Server= "myserver4demo.mysql.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@myserver4demo"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a>JDBC
```jdbc
String url ="jdbc:mysql://%s:%s/%s[?verifyServerCertificate=true&useSSL=true&requireSSL=true]",{your_host},{your_port},{your_database}"; myDbConn = DriverManager.getConnection(url, {username@servername}, {your_password}";
```

## <a name="nodejs"></a>Node.js
```node.js
var conn = mysql.createConnection({host: {your_host}, user: {username@servername}, password: {your_password}, database: {your_database}, Port: {your_port}[, ssl:{ca:fs.readFileSync({ca-cert filename})}}]);
```

## <a name="odbc"></a>ODBC
```odbc
DRIVER={MySQL ODBC 5.3 UNICODE Driver};Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password}; [sslca={ca-cert filename}; sslverify=1; Option=3;]
```

## <a name="php"></a>PHP
```php
$con=mysqli_init(); [mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL);] mysqli_real_connect($con, {your_host}, {username@servername}, {your_password}, {your_database}, {your_port});
```

## <a name="python"></a>Python
```python
cnx = mysql.connector.connect(user={username@servername}, password={your_password}, host={your_host}, port={your_port}, database={your_database}[, ssl_ca={ca-cert filename}, ssl_verify_cert=true])
```

## <a name="ruby"></a>Ruby
```ruby
client = Mysql2::Client.new(username: {username@servername}, password: {your_password}, database: {your_database}, host: {your_host}, port: {your_port}[, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA'])
```

## <a name="get-hello-connection-string-details-from-hello-azure-portal"></a>Získat podrobnosti o řetězce hello připojení z hello portálu Azure
V hello [portál Azure](https://portal.azure.com), přejděte tooyour Azure databáze pro server databáze MySQL a pak klikněte na tlačítko **připojovací řetězce** tooget řetězec vašeho seznamu pro instanci: ![hello připojovací řetězce podokně hello Portál Azure](./media/howto-connection-strings/connection-strings-on-portal.png)

řetězec Hello poskytuje podrobnosti, například hello ovladačů, serveru a další parametry připojení databáze. Upravte tyto příklady pomocí vlastních parametrů, jako je název databáze, heslo a tak dále. Pak můžete použít tento řetězec tooconnect toohello server z vašeho kódu a aplikace.

## <a name="next-steps"></a>Další kroky
- Další informace o knihovnách připojení najdete v tématu [koncepty - knihovny připojení](./concepts-connection-libraries.md).
