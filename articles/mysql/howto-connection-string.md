---
title: "Připojení aplikace k databázi Azure pro databázi MySQL | Microsoft Docs"
description: "Tento dokument obsahuje seznam aktuálně podporované připojovací řetězce pro aplikace pro připojení s Azure Database pro databázi MySQL, včetně ADO.NET (C#), JDBC, Node.js, rozhraní ODBC, PHP, Python a Ruby."
services: mysql
author: mswutao
ms.author: wuta
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 06/12/2017
ms.openlocfilehash: 2f40da41bcfda7e35f6fc63ead5d055246ab390c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-connect-applications-to-azure-database-for-mysql"></a><span data-ttu-id="8e269-103">Postup připojení aplikace k databázi Azure pro databázi MySQL</span><span class="sxs-lookup"><span data-stu-id="8e269-103">How to connect applications to Azure Database for MySQL</span></span>
<span data-ttu-id="8e269-104">Tento dokument uvádí typy řetězec připojení, které podporuje Azure databáze pro databázi MySQL, spolu s šablonami a příklady.</span><span class="sxs-lookup"><span data-stu-id="8e269-104">This document lists the connection string types that are supported by Azure Database for MySQL, together with templates and examples.</span></span> <span data-ttu-id="8e269-105">V připojovacím řetězci může mít různé parametry a různá nastavení.</span><span class="sxs-lookup"><span data-stu-id="8e269-105">You might have different parameters and different settings in your connection string.</span></span>

- <span data-ttu-id="8e269-106">K získání certifikátu, najdete v části [postup konfigurace protokolu SSL](./howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="8e269-106">To obtain the certificate, see [How to configure SSL](./howto-configure-ssl.md).</span></span>
- <span data-ttu-id="8e269-107">{your_host} = <servername>. mysql.database.azure.com</span><span class="sxs-lookup"><span data-stu-id="8e269-107">{your_host} = <servername>.mysql.database.azure.com</span></span>
- <span data-ttu-id="8e269-108">{your_user}@{servername} správně = formát ID uživatele pro ověřování.</span><span class="sxs-lookup"><span data-stu-id="8e269-108">{your_user}@{servername} = userID format for authentication correctly.</span></span>  <span data-ttu-id="8e269-109">Pomocí právě userID způsobí, že došlo k chybě ověřování.</span><span class="sxs-lookup"><span data-stu-id="8e269-109">Using just the userID will cause the authentication to fail.</span></span>

## <a name="adonet"></a><span data-ttu-id="8e269-110">ADO.NET</span><span class="sxs-lookup"><span data-stu-id="8e269-110">ADO.NET</span></span>
```ado.net
Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password};[SslMode=Required;]
```

<span data-ttu-id="8e269-111">V tomto příkladu je název serveru `myserver4demo`, název databáze je `wpdb`, uživatelské jméno je `WPAdmin`, a heslo je `mypassword!2`.</span><span class="sxs-lookup"><span data-stu-id="8e269-111">In this example, the server name is `myserver4demo`, database name is `wpdb`, user name is `WPAdmin`, and password is `mypassword!2`.</span></span> <span data-ttu-id="8e269-112">Pak by měl mít připojovací řetězec:</span><span class="sxs-lookup"><span data-stu-id="8e269-112">Then, the connection string should be:</span></span>

```ado.net
Server= "myserver4demo.mysql.database.azure.com"; Port=3306; Database= "wpdb"; Uid= "WPAdmin@myserver4demo"; Pwd="mypassword!2"; SslMode=Required;
```

## <a name="jdbc"></a><span data-ttu-id="8e269-113">JDBC</span><span class="sxs-lookup"><span data-stu-id="8e269-113">JDBC</span></span>
```jdbc
String url ="jdbc:mysql://%s:%s/%s[?verifyServerCertificate=true&useSSL=true&requireSSL=true]",{your_host},{your_port},{your_database}"; myDbConn = DriverManager.getConnection(url, {username@servername}, {your_password}";
```

## <a name="nodejs"></a><span data-ttu-id="8e269-114">Node.js</span><span class="sxs-lookup"><span data-stu-id="8e269-114">Node.js</span></span>
```node.js
var conn = mysql.createConnection({host: {your_host}, user: {username@servername}, password: {your_password}, database: {your_database}, Port: {your_port}[, ssl:{ca:fs.readFileSync({ca-cert filename})}}]);
```

## <a name="odbc"></a><span data-ttu-id="8e269-115">ODBC</span><span class="sxs-lookup"><span data-stu-id="8e269-115">ODBC</span></span>
```odbc
DRIVER={MySQL ODBC 5.3 UNICODE Driver};Server={your_host};Port={your_port};Database={your_database};Uid={username@servername};Pwd={your_password}; [sslca={ca-cert filename}; sslverify=1; Option=3;]
```

## <a name="php"></a><span data-ttu-id="8e269-116">PHP</span><span class="sxs-lookup"><span data-stu-id="8e269-116">PHP</span></span>
```php
$con=mysqli_init(); [mysqli_ssl_set($con, NULL, NULL, {ca-cert filename}, NULL, NULL);] mysqli_real_connect($con, {your_host}, {username@servername}, {your_password}, {your_database}, {your_port});
```

## <a name="python"></a><span data-ttu-id="8e269-117">Python</span><span class="sxs-lookup"><span data-stu-id="8e269-117">Python</span></span>
```python
cnx = mysql.connector.connect(user={username@servername}, password={your_password}, host={your_host}, port={your_port}, database={your_database}[, ssl_ca={ca-cert filename}, ssl_verify_cert=true])
```

## <a name="ruby"></a><span data-ttu-id="8e269-118">Ruby</span><span class="sxs-lookup"><span data-stu-id="8e269-118">Ruby</span></span>
```ruby
client = Mysql2::Client.new(username: {username@servername}, password: {your_password}, database: {your_database}, host: {your_host}, port: {your_port}[, sslca:{ca-cert filename}, sslverify:false, sslcipher:'AES256-SHA'])
```

## <a name="get-the-connection-string-details-from-the-azure-portal"></a><span data-ttu-id="8e269-119">Získat podrobnosti o řetězce připojení z portálu Azure</span><span class="sxs-lookup"><span data-stu-id="8e269-119">Get the connection string details from the Azure portal</span></span>
<span data-ttu-id="8e269-120">V [portál Azure](https://portal.azure.com), přejděte k vaší databázi Azure pro server databáze MySQL a pak klikněte na tlačítko **připojovací řetězce** k získání seznamu řetězec pro instanci: ![podokně řetězce připojení ve službě Azure portál](./media/howto-connection-strings/connection-strings-on-portal.png)</span><span class="sxs-lookup"><span data-stu-id="8e269-120">In the [Azure portal](https://portal.azure.com), go to your Azure Database for MySQL server, and then click **Connection strings** to get your string list for your instance: ![The Connection strings pane in the Azure portal](./media/howto-connection-strings/connection-strings-on-portal.png)</span></span>

<span data-ttu-id="8e269-121">Řetězec obsahuje podrobné informace jako jsou ovladače, serveru a jiné databázi parametrů připojení.</span><span class="sxs-lookup"><span data-stu-id="8e269-121">The string provides details such as the driver, server, and other database connection parameters.</span></span> <span data-ttu-id="8e269-122">Upravte tyto příklady pomocí vlastních parametrů, jako je název databáze, heslo a tak dále.</span><span class="sxs-lookup"><span data-stu-id="8e269-122">Modify these examples by using your own parameters, such as database name, password, and so on.</span></span> <span data-ttu-id="8e269-123">Pak můžete tento řetězec pro připojení k serveru z vašeho kódu a aplikace.</span><span class="sxs-lookup"><span data-stu-id="8e269-123">You can then use this string to connect to the server from your code and applications.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8e269-124">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8e269-124">Next steps</span></span>
- <span data-ttu-id="8e269-125">Další informace o knihovnách připojení najdete v tématu [koncepty - knihovny připojení](./concepts-connection-libraries.md).</span><span class="sxs-lookup"><span data-stu-id="8e269-125">For more information about connection libraries, see [Concepts - Connection libraries](./concepts-connection-libraries.md).</span></span>
