---
title: aaaConnect tooAzure SQL Data Warehouse | Microsoft Docs
description: "Jak název serveru hello toofind a připojovací řetězce pro vaše tooAzure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: e52872ca-ae74-4e25-9c56-d49c85c8d0f0
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 10/31/2016
ms.author: anvang;barbkess
ms.openlocfilehash: f15e098026afb7c5efbbbfaf62b681e8cd7936bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooazure-sql-data-warehouse"></a><span data-ttu-id="ba3ff-103">Připojit tooAzure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="ba3ff-103">Connect tooAzure SQL Data Warehouse</span></span>
<span data-ttu-id="ba3ff-104">Tento článek vám pomůže připojené tooSQL datového skladu pro hello poprvé.</span><span class="sxs-lookup"><span data-stu-id="ba3ff-104">This article helps you get connected tooSQL Data Warehouse for hello first time.</span></span>

## <a name="find-your-server-name"></a><span data-ttu-id="ba3ff-105">Vyhledání názvu serveru</span><span class="sxs-lookup"><span data-stu-id="ba3ff-105">Find your server name</span></span>
<span data-ttu-id="ba3ff-106">Hello první krok tooconnecting tooSQL datového skladu je zároveň budete vědět, jak toofind název serveru.</span><span class="sxs-lookup"><span data-stu-id="ba3ff-106">hello first step tooconnecting tooSQL Data Warehouse is knowing how toofind your server name.</span></span>  <span data-ttu-id="ba3ff-107">Název serveru hello v hello následující ukázka je například sample.database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="ba3ff-107">For example, hello server name in hello following example is sample.database.windows.net.</span></span> <span data-ttu-id="ba3ff-108">název plně kvalifikovaný serveru toofind hello:</span><span class="sxs-lookup"><span data-stu-id="ba3ff-108">toofind hello fully qualified server name:</span></span>

1. <span data-ttu-id="ba3ff-109">Přejděte toohello [portál Azure][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="ba3ff-109">Go toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="ba3ff-110">Klikněte na **Databáze SQL**.</span><span class="sxs-lookup"><span data-stu-id="ba3ff-110">Click on **SQL databases**</span></span> 
3. <span data-ttu-id="ba3ff-111">Klikněte na hello požadovaná tooconnect do databáze.</span><span class="sxs-lookup"><span data-stu-id="ba3ff-111">Click on hello database you want tooconnect to.</span></span>
4. <span data-ttu-id="ba3ff-112">Vyhledejte hello úplný název serveru.</span><span class="sxs-lookup"><span data-stu-id="ba3ff-112">Locate hello full server name.</span></span>
   
    ![Úplný název serveru][1]

## <a name="supported-drivers-and-connection-strings"></a><span data-ttu-id="ba3ff-114">Podporované ovladače a připojovací řetězce</span><span class="sxs-lookup"><span data-stu-id="ba3ff-114">Supported drivers and connection strings</span></span>
<span data-ttu-id="ba3ff-115">Azure SQL Data Warehouse podporuje [ADO.NET][ADO.NET], [ODBC][ODBC], [PHP][PHP] a [JDBC][JDBC].</span><span class="sxs-lookup"><span data-stu-id="ba3ff-115">Azure SQL Data Warehouse supports [ADO.NET][ADO.NET], [ODBC][ODBC], [PHP][PHP], and [JDBC][JDBC].</span></span> <span data-ttu-id="ba3ff-116">Klikněte na jednu z hello předcházející ovladače toofind hello nejnovější verzi a dokumentaci.</span><span class="sxs-lookup"><span data-stu-id="ba3ff-116">Click on one of hello preceding drivers toofind hello latest version and documentation.</span></span> <span data-ttu-id="ba3ff-117">tooautomatically negeneruje hello Azure hello připojovací řetězec pro hello ovladač, který používáte portál, můžete kliknutím na hello **zobrazit databázové připojovací řetězce** z předchozích příklad hello.</span><span class="sxs-lookup"><span data-stu-id="ba3ff-117">tooautomatically generate hello connection string for hello driver that you are using from hello Azure portal, you can click on hello **Show database connection strings** from hello preceding example.</span></span>  <span data-ttu-id="ba3ff-118">Následuje několik příkladů toho, jak připojovací řetězce vypadají pro jednotlivé ovladače.</span><span class="sxs-lookup"><span data-stu-id="ba3ff-118">Following are also some examples of what a connection string looks like for each driver.</span></span>

> [!NOTE]
> <span data-ttu-id="ba3ff-119">Zvažte nastavení hello připojení časový limit too300 sekund tooallow vaše připojení toosurvive krátkodobých období nedostupnosti.</span><span class="sxs-lookup"><span data-stu-id="ba3ff-119">Consider setting hello connection timeout too300 seconds tooallow your connection toosurvive short periods of unavailability.</span></span>
> 
> 

### <a name="adonet-connection-string-example"></a><span data-ttu-id="ba3ff-120">Příklad připojovacího řetězce pro ADO.NET</span><span class="sxs-lookup"><span data-stu-id="ba3ff-120">ADO.NET connection string example</span></span>
```C#
Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};User ID={your_user_name};Password={your_password_here};Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;
```

### <a name="odbc-connection-string-example"></a><span data-ttu-id="ba3ff-121">Příklad připojovacího řetězce pro ODBC</span><span class="sxs-lookup"><span data-stu-id="ba3ff-121">ODBC connection string example</span></span>
```C#
Driver={SQL Server Native Client 11.0};Server=tcp:{your_server}.database.windows.net,1433;Database={your_database};Uid={your_user_name};Pwd={your_password_here};Encrypt=yes;TrustServerCertificate=no;Connection Timeout=30;
```

### <a name="php-connection-string-example"></a><span data-ttu-id="ba3ff-122">Příklad připojovacího řetězce pro PHP</span><span class="sxs-lookup"><span data-stu-id="ba3ff-122">PHP connection string example</span></span>
```PHP
Server: {your_server}.database.windows.net,1433 \r\nSQL Database: {your_database}\r\nUser Name: {your_user_name}\r\n\r\nPHP Data Objects(PDO) Sample Code:\r\n\r\ntry {\r\n   $conn = new PDO ( \"sqlsrv:server = tcp:{your_server}.database.windows.net,1433; Database = {your_database}\", \"{your_user_name}\", \"{your_password_here}\");\r\n    $conn->setAttribute( PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION );\r\n}\r\ncatch ( PDOException $e ) {\r\n   print( \"Error connecting tooSQL Server.\" );\r\n   die(print_r($e));\r\n}\r\n\rSQL Server Extension Sample Code:\r\n\r\n$connectionInfo = array(\"UID\" => \"{your_user_name}\", \"pwd\" => \"{your_password_here}\", \"Database\" => \"{your_database}\", \"LoginTimeout\" => 30, \"Encrypt\" => 1, \"TrustServerCertificate\" => 0);\r\n$serverName = \"tcp:{your_server}.database.windows.net,1433\";\r\n$conn = sqlsrv_connect($serverName, $connectionInfo);
```

### <a name="jdbc-connection-string-example"></a><span data-ttu-id="ba3ff-123">Příklad připojovacího řetězce pro JDBC</span><span class="sxs-lookup"><span data-stu-id="ba3ff-123">JDBC connection string example</span></span>
```Java
jdbc:sqlserver://yourserver.database.windows.net:1433;database=yourdatabase;user={your_user_name};password={your_password_here};encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.database.windows.net;loginTimeout=30;
```

## <a name="connection-settings"></a><span data-ttu-id="ba3ff-124">Nastavení připojení</span><span class="sxs-lookup"><span data-stu-id="ba3ff-124">Connection settings</span></span>
<span data-ttu-id="ba3ff-125">SQL Data Warehouse během připojování a vytváření objektů používá několik standardních nastavení.</span><span class="sxs-lookup"><span data-stu-id="ba3ff-125">SQL Data Warehouse standardizes some settings during connection and object creation.</span></span> <span data-ttu-id="ba3ff-126">Tato nastavení nelze přepsat a zahrnují:</span><span class="sxs-lookup"><span data-stu-id="ba3ff-126">These settings cannot be overridden and include:</span></span>

| <span data-ttu-id="ba3ff-127">Nastavení databáze</span><span class="sxs-lookup"><span data-stu-id="ba3ff-127">Database Setting</span></span> | <span data-ttu-id="ba3ff-128">Hodnota</span><span class="sxs-lookup"><span data-stu-id="ba3ff-128">Value</span></span> |
|:--- |:--- |
| <span data-ttu-id="ba3ff-129">[ANSI_NULLS][ANSI_NULLS]</span><span class="sxs-lookup"><span data-stu-id="ba3ff-129">[ANSI_NULLS][ANSI_NULLS]</span></span> |<span data-ttu-id="ba3ff-130">ON</span><span class="sxs-lookup"><span data-stu-id="ba3ff-130">ON</span></span> |
| <span data-ttu-id="ba3ff-131">[QUOTED_IDENTIFIERS][QUOTED_IDENTIFIERS]</span><span class="sxs-lookup"><span data-stu-id="ba3ff-131">[QUOTED_IDENTIFIERS][QUOTED_IDENTIFIERS]</span></span> |<span data-ttu-id="ba3ff-132">ON</span><span class="sxs-lookup"><span data-stu-id="ba3ff-132">ON</span></span> |
| <span data-ttu-id="ba3ff-133">[DATEFORMAT][DATEFORMAT]</span><span class="sxs-lookup"><span data-stu-id="ba3ff-133">[DATEFORMAT][DATEFORMAT]</span></span> |<span data-ttu-id="ba3ff-134">mdy</span><span class="sxs-lookup"><span data-stu-id="ba3ff-134">mdy</span></span> |
| <span data-ttu-id="ba3ff-135">[DATEFIRST][DATEFIRST]</span><span class="sxs-lookup"><span data-stu-id="ba3ff-135">[DATEFIRST][DATEFIRST]</span></span> |<span data-ttu-id="ba3ff-136">7</span><span class="sxs-lookup"><span data-stu-id="ba3ff-136">7</span></span> |

## <a name="next-steps"></a><span data-ttu-id="ba3ff-137">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ba3ff-137">Next steps</span></span>
<span data-ttu-id="ba3ff-138">tooconnect a dotaz pomocí sady Visual Studio, najdete v části [dotazu pomocí sady Visual Studio][Query with Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="ba3ff-138">tooconnect and query with Visual Studio, see [Query with Visual Studio][Query with Visual Studio].</span></span> <span data-ttu-id="ba3ff-139">toolearn Další informace o možnosti ověřování najdete v části [tooAzure ověřování SQL Data Warehouse][Authentication tooAzure SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="ba3ff-139">toolearn more about authentication options, see [Authentication tooAzure SQL Data Warehouse][Authentication tooAzure SQL Data Warehouse].</span></span>

<!--Articles-->
[Query with Visual Studio]: ./sql-data-warehouse-query-visual-studio.md
[Authentication tooAzure SQL Data Warehouse]: ./sql-data-warehouse-authentication.md

<!--MSDN references-->
[ADO.NET]: https://msdn.microsoft.com/library/e80y5yhx(v=vs.110).aspx
[ODBC]: https://msdn.microsoft.com/library/jj730314.aspx
[PHP]: https://msdn.microsoft.com/library/cc296172.aspx?f=255&MSPPError=-2147217396
[JDBC]: https://msdn.microsoft.com/library/mt484311(v=sql.110).aspx
[ANSI_NULLS]: https://msdn.microsoft.com/library/ms188048.aspx
[QUOTED_IDENTIFIERS]: https://msdn.microsoft.com/library/ms174393.aspx
[DATEFORMAT]: https://msdn.microsoft.com/library/ms189491.aspx
[DATEFIRST]: https://msdn.microsoft.com/library/ms181598.aspx

<!--Other-->
[Azure portal]: https://portal.azure.com

<!--Image references-->
[1]: media/sql-data-warehouse-connect-overview/get-server-name.png


