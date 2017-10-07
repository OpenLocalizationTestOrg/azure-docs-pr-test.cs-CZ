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
# <a name="how-tooconnect-applications-tooazure-database-for-mysql"></a><span data-ttu-id="42b76-103">Jak aplikace tooAzure tooconnect databáze pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="42b76-103">How tooconnect applications tooAzure Database for MySQL</span></span>
<span data-ttu-id="42b76-104">Tento dokument uvádí hello připojovací řetězec typy, které podporuje Azure databáze pro databázi MySQL, spolu s šablonami a příklady.</span><span class="sxs-lookup"><span data-stu-id="42b76-104">This document lists hello connection string types that are supported by Azure Database for MySQL, together with templates and examples.</span></span> <span data-ttu-id="42b76-105">V připojovacím řetězci může mít různé parametry a různá nastavení.</span><span class="sxs-lookup"><span data-stu-id="42b76-105">You might have different parameters and different settings in your connection string.</span></span>

- <span data-ttu-id="42b76-106">tooobtain hello certifikátu, najdete v části [jak tooconfigure SSL](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="42b76-106">tooobtain hello certificate, see [How tooconfigure SSL](./howto-configure-ssl.md).</span></span>
- <span data-ttu-id="42b76-107">{your_host} = <servername>. mysql.database.azure.com</span><span class="sxs-lookup"><span data-stu-id="42b76-107">{your_host} = <servername>.mysql.database.azure.com</span></span>
- <span data-ttu-id="42b76-108">{your_user}@{servername} správně = formát ID uživatele pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="42b76-108">{your_user}@{servername} = userID format for authentication correctly.</span></span>  <span data-ttu-id="42b76-109">Použití právě hello userID způsobí toofail ověřování hello.</span><span class="sxs-lookup"><span data-stu-id="42b76-109">Using just hello userID will cause hello authentication toofail.</span></span>

## <a name="adonet"></a><span data-ttu-id="42b76-110">ADO.NET</span><span class="sxs-lookup"><span data-stu-id="42b76-110">ADO.NET</span></span>
```ado.net
Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password};[SslMode=Required;]
```

<span data-ttu-id="42b76-111">V tomto příkladu je název serveru hello `myserver4demo`, název databáze je `wpdb`, uživatelské jméno je `WPAdmin`, a heslo je `mypassword!2`.</span><span class="sxs-lookup"><span data-stu-id="42b76-111">In this example, hello server name is `myserver4demo`, database name is `wpdb`, user name is `WPAdmin`, and password is `mypassword!2`.</span></span> <span data-ttu-id="42b76-112">Potom by měla být hello připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="42b76-112">Then, hello connection string should be:</span></span>

```ado.net
Server= "myserver4demo.mysql.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@myserver4demo"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a><span data-ttu-id="42b76-113">JDBC</span><span class="sxs-lookup"><span data-stu-id="42b76-113">JDBC</span></span>
```jdbc
String url ="jdbc:mysql://%s:%s/%s[?verifyServerCertificate=true&useSSL=true&requireSSL=true]",{your_host},{your_port},{your_database}"; myDbConn = DriverManager.getConnection(url, {username@servername}, {your_password}";
```

## <a name="nodejs"></a><span data-ttu-id="42b76-114">Node.js</span><span class="sxs-lookup"><span data-stu-id="42b76-114">Node.js</span></span>
```node.js
var conn = mysql.createConnection({host: {your_host}, user: {username@servername}, password: {your_password}, database: {your_database}, Port: {your_port}[, ssl:{ca:fs.readFileSync({ca-cert filename})}}]);
```

## <a name="odbc"></a><span data-ttu-id="42b76-115">ODBC</span><span class="sxs-lookup"><span data-stu-id="42b76-115">ODBC</span></span>
```odbc
DRIVER={MySQL ODBC 5.3 UNICODE Driver};Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password}; [sslca={ca-cert filename}; sslverify=1; Option=3;]
```

## <a name="php"></a><span data-ttu-id="42b76-116">PHP</span><span class="sxs-lookup"><span data-stu-id="42b76-116">PHP</span></span>
```php
$con=mysqli_init(); [mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL);] mysqli_real_connect($con, {your_host}, {username@servername}, {your_password}, {your_database}, {your_port});
```

## <a name="python"></a><span data-ttu-id="42b76-117">Python</span><span class="sxs-lookup"><span data-stu-id="42b76-117">Python</span></span>
```python
cnx = mysql.connector.connect(user={username@servername}, password={your_password}, host={your_host}, port={your_port}, database={your_database}[, ssl_ca={ca-cert filename}, ssl_verify_cert=true])
```

## <a name="ruby"></a><span data-ttu-id="42b76-118">Ruby</span><span class="sxs-lookup"><span data-stu-id="42b76-118">Ruby</span></span>
```ruby
client = Mysql2::Client.new(username: {username@servername}, password: {your_password}, database: {your_database}, host: {your_host}, port: {your_port}[, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA'])
```

## <a name="get-hello-connection-string-details-from-hello-azure-portal"></a><span data-ttu-id="42b76-119">Získat podrobnosti o řetězce hello připojení z hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="42b76-119">Get hello connection string details from hello Azure portal</span></span>
<span data-ttu-id="42b76-120">V hello [portál Azure](https://portal.azure.com), přejděte tooyour Azure databáze pro server databáze MySQL a pak klikněte na tlačítko **připojovací řetězce** tooget řetězec vašeho seznamu pro instanci: ![hello připojovací řetězce podokně hello Portál Azure](./media/howto-connection-strings/connection-strings-on-portal.png)</span><span class="sxs-lookup"><span data-stu-id="42b76-120">In hello [Azure portal](https://portal.azure.com), go tooyour Azure Database for MySQL server, and then click **Connection strings** tooget your string list for your instance: ![hello Connection strings pane in hello Azure portal](./media/howto-connection-strings/connection-strings-on-portal.png)</span></span>

<span data-ttu-id="42b76-121">řetězec Hello poskytuje podrobnosti, například hello ovladačů, serveru a další parametry připojení databáze.</span><span class="sxs-lookup"><span data-stu-id="42b76-121">hello string provides details such as hello driver, server, and other database connection parameters.</span></span> <span data-ttu-id="42b76-122">Upravte tyto příklady pomocí vlastních parametrů, jako je název databáze, heslo a tak dále.</span><span class="sxs-lookup"><span data-stu-id="42b76-122">Modify these examples by using your own parameters, such as database name, password, and so on.</span></span> <span data-ttu-id="42b76-123">Pak můžete použít tento řetězec tooconnect toohello server z vašeho kódu a aplikace.</span><span class="sxs-lookup"><span data-stu-id="42b76-123">You can then use this string tooconnect toohello server from your code and applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="42b76-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42b76-124">Next steps</span></span>
- <span data-ttu-id="42b76-125">Další informace o knihovnách připojení najdete v tématu [koncepty - knihovny připojení](./concepts-connection-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="42b76-125">For more information about connection libraries, see [Concepts - Connection libraries](./concepts-connection-libraries.md).</span></span>
