---
title: "Správa přihlašovacích údajů v knihovně klienta elastické databáze | Microsoft Docs"
description: "Jak nastavit správnou úroveň přihlašovací údaje správce jen pro čtení, pro elastické databáze aplikace"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 72e0edaf-795e-4856-84a5-6594f735fb7e
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 46908be2846062a0520d21e06db3091a4d711b0b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="credentials-used-to-access-the-elastic-database-client-library"></a><span data-ttu-id="7189e-103">Pověření použitá pro přístup k klientské knihovny pro elastické databáze</span><span class="sxs-lookup"><span data-stu-id="7189e-103">Credentials used to access the Elastic Database client library</span></span>
<span data-ttu-id="7189e-104">[Klientské knihovny pro elastické databáze](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) používá tři různé druhy přihlašovací údaje pro přístup [správce mapy horizontálního oddílu](sql-database-elastic-scale-shard-map-management.md).</span><span class="sxs-lookup"><span data-stu-id="7189e-104">The [Elastic Database client library](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) uses three different kinds  of credentials to access the [shard map manager](sql-database-elastic-scale-shard-map-management.md).</span></span> <span data-ttu-id="7189e-105">Podle potřeb s nejnižší úroveň přístupu je možné použijte přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="7189e-105">Depending on the need, use the credential with  the lowest level of access possible.</span></span>

* <span data-ttu-id="7189e-106">**Přihlašovacích údajů pro správu**: pro vytvoření nebo manipulace s manažera mapy horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="7189e-106">**Management credentials**: for creating or manipulating a shard map manager.</span></span> <span data-ttu-id="7189e-107">(Viz [Glosář](sql-database-elastic-scale-glossary.md).)</span><span class="sxs-lookup"><span data-stu-id="7189e-107">(See the [glossary](sql-database-elastic-scale-glossary.md).)</span></span> 
* <span data-ttu-id="7189e-108">**Přístup k pověření**: přístup k existující správce mapy horizontálního oddílu k získání informací o horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="7189e-108">**Access credentials**: to access an existing shard map manager to obtain information about shards.</span></span>
* <span data-ttu-id="7189e-109">**Přihlašovací údaje pro připojení**: pro připojení k horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="7189e-109">**Connection credentials**: to connect to shards.</span></span> 

<span data-ttu-id="7189e-110">Viz také [Správa databází a přihlašovacích údajů ve službě Azure SQL Database](sql-database-manage-logins.md).</span><span class="sxs-lookup"><span data-stu-id="7189e-110">See also [Managing databases and logins in Azure SQL Database](sql-database-manage-logins.md).</span></span> 

## <a name="about-management-credentials"></a><span data-ttu-id="7189e-111">O přihlašovacích údajů pro správu</span><span class="sxs-lookup"><span data-stu-id="7189e-111">About management credentials</span></span>
<span data-ttu-id="7189e-112">Přihlašovacích údajů pro správu se používají k vytváření [ **ShardMapManager** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) objekt pro aplikace, které pracují s horizontálního oddílu mapy.</span><span class="sxs-lookup"><span data-stu-id="7189e-112">Management credentials are used to create a [**ShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) object for applications that manipulate shard maps.</span></span> <span data-ttu-id="7189e-113">(Například najdete v části [přidání horizontálních, pomocí nástroje elastické databáze](sql-database-elastic-scale-add-a-shard.md) a [Data závislé směrování](sql-database-elastic-scale-data-dependent-routing.md)) uživatel Klientská knihovna pro elastické škálování vytvoří uživatelé SQL a přihlášeních SQL a zajišťuje, každý je povolen oprávnění pro čtení a zápis na globální horizontálního oddílu mapovat databáze a také všechny databáze horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="7189e-113">(For example, see [Adding a shard using Elastic Database tools](sql-database-elastic-scale-add-a-shard.md) and [Data dependent routing](sql-database-elastic-scale-data-dependent-routing.md)) The user of the elastic scale client library creates the SQL users and SQL logins and makes sure each is granted the read/write permissions on the global shard map database and all shard databases as well.</span></span> <span data-ttu-id="7189e-114">Tyto přihlašovací údaje se používají k udržování mapy horizontálního oddílu globální a místní horizontálních mapy při provádění změny mapy horizontálního oddílu jsou.</span><span class="sxs-lookup"><span data-stu-id="7189e-114">These credentials are used to maintain the global shard map and the local shard maps when changes to the shard map are performed.</span></span> <span data-ttu-id="7189e-115">Například použít k vytvoření objekt horizontálního oddílu mapa správce přihlašovacích údajů pro správu (pomocí [ **GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx):</span><span class="sxs-lookup"><span data-stu-id="7189e-115">For instance, use the management credentials to create the shard map manager object (using [**GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx):</span></span> 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

<span data-ttu-id="7189e-116">Proměnná **smmAdminConnectionString** je připojovací řetězec, který obsahuje přihlašovacích údajů pro správu.</span><span class="sxs-lookup"><span data-stu-id="7189e-116">The variable **smmAdminConnectionString** is a connection string that contains the management credentials.</span></span> <span data-ttu-id="7189e-117">ID uživatele a heslo poskytuje přístup pro čtení a zápis do databáze mapy horizontálního oddílu a jednotlivých horizontálních oddílů.</span><span class="sxs-lookup"><span data-stu-id="7189e-117">The user ID and password provides read/write access to both shard map database and individual shards.</span></span> <span data-ttu-id="7189e-118">Připojovací řetězec správu také zahrnuje název serveru a název databáze pro identifikaci databázi mapy globální horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="7189e-118">The management connection string also includes the server name and database name to identify the global shard map database.</span></span> <span data-ttu-id="7189e-119">Zde je typické připojovací řetězec pro tento účel:</span><span class="sxs-lookup"><span data-stu-id="7189e-119">Here is a typical connection string for that purpose:</span></span>

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

<span data-ttu-id="7189e-120">Nepoužívejte hodnoty ve formátu "username@server" – místo toho použijte jen hodnotu "username".</span><span class="sxs-lookup"><span data-stu-id="7189e-120">Do not use values in the form of "username@server"—instead just use the "username" value.</span></span>  <span data-ttu-id="7189e-121">Je to proto, že přihlašovací údaje musí pracovat v databázi manager mapy horizontálního oddílu i jednotlivé horizontálních oddílů, které mohou být na různých serverech.</span><span class="sxs-lookup"><span data-stu-id="7189e-121">This is because credentials must work against both the shard map manager database and individual shards, which may be on different servers.</span></span>

## <a name="access-credentials"></a><span data-ttu-id="7189e-122">Přihlašovací údaje</span><span class="sxs-lookup"><span data-stu-id="7189e-122">Access credentials</span></span>
<span data-ttu-id="7189e-123">Při vytváření horizontálního oddílu správce mapy v aplikaci, která není spravovat horizontálního oddílu mapy, použijte přihlašovací údaje, které mají oprávnění jen pro čtení na mapě globální horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="7189e-123">When creating a shard map manager in an application that does not administer shard maps, use credentials that have read-only permissions on the global shard map.</span></span> <span data-ttu-id="7189e-124">Informace získané z globální horizontálních mapy pod tyto přihlašovací údaje se používají pro [závislé na data směrování](sql-database-elastic-scale-data-dependent-routing.md) a k naplnění mezipaměti mapování horizontálních na straně klienta.</span><span class="sxs-lookup"><span data-stu-id="7189e-124">The information retrieved from the global shard map under these credentials are used for [data-dependent routing](sql-database-elastic-scale-data-dependent-routing.md) and to populate the shard map cache on the client.</span></span> <span data-ttu-id="7189e-125">Přihlašovací údaje je zajišťována prostřednictvím stejného vzoru volání do **GetSqlShardMapManager** jako v příkladu nahoře:</span><span class="sxs-lookup"><span data-stu-id="7189e-125">The credentials are provided through the same call pattern to **GetSqlShardMapManager** as shown above:</span></span> 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

<span data-ttu-id="7189e-126">Všimněte si použití **smmReadOnlyConnectionString** tak, aby odrážela použít různé přihlašovací údaje pro tento přístup jménem **bez oprávnění správce** uživatele: Tato pověření by nemělo zadejte oprávnění k zápisu na Mapa globální horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="7189e-126">Note the use of the **smmReadOnlyConnectionString** to reflect the use of different credentials for this access on behalf of **non-admin** users: these credentials should not provide write permissions on the global shard map.</span></span> 

## <a name="connection-credentials"></a><span data-ttu-id="7189e-127">Přihlašovací údaje pro připojení</span><span class="sxs-lookup"><span data-stu-id="7189e-127">Connection credentials</span></span>
<span data-ttu-id="7189e-128">Další přihlašovací údaje jsou potřeba při používání [ **OpenConnectionForKey** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) metody přístup horizontálního oddílu přidružené k klíči horizontálního dělení.</span><span class="sxs-lookup"><span data-stu-id="7189e-128">Additional credentials are needed when using the [**OpenConnectionForKey**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) method to access a shard associated with a sharding key.</span></span> <span data-ttu-id="7189e-129">Tyto přihlašovací údaje musí poskytnout oprávnění pro přístup jen pro čtení k místní horizontálního oddílu tabulky mapy umístěný na horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="7189e-129">These credentials need to provide permissions for read-only access to the local shard map tables residing on the shard.</span></span> <span data-ttu-id="7189e-130">To je nutné k provedení ověření platnosti připojení pro směrování dat závisí na horizontálního oddílu.</span><span class="sxs-lookup"><span data-stu-id="7189e-130">This is needed to perform connection validation for data-dependent routing on the shard.</span></span> <span data-ttu-id="7189e-131">Tento fragment kódu umožňuje přístup k datům v kontextu závislé směrování dat:</span><span class="sxs-lookup"><span data-stu-id="7189e-131">This code snippet allows data access in the context of data dependent routing:</span></span> 

    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

<span data-ttu-id="7189e-132">V tomto příkladu **smmUserConnectionString** obsahuje připojovací řetězec pro přihlašovací údaje uživatele.</span><span class="sxs-lookup"><span data-stu-id="7189e-132">In this example, **smmUserConnectionString** holds the connection string for the user credentials.</span></span> <span data-ttu-id="7189e-133">Pro databáze SQL Azure zde je typické připojovací řetězec pro přihlašovací údaje uživatele:</span><span class="sxs-lookup"><span data-stu-id="7189e-133">For Azure SQL DB, here is a typical connection string for user credentials:</span></span> 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

<span data-ttu-id="7189e-134">Stejně jako u přihlašovací údaje správce, nejsou hodnoty ve formátu "username@server".</span><span class="sxs-lookup"><span data-stu-id="7189e-134">As with the admin credentials, do not values in the form of "username@server".</span></span> <span data-ttu-id="7189e-135">Místo toho použijte "username".</span><span class="sxs-lookup"><span data-stu-id="7189e-135">Instead, just use "username".</span></span>  <span data-ttu-id="7189e-136">Všimněte si také, že připojovací řetězec neobsahuje název serveru a název databáze.</span><span class="sxs-lookup"><span data-stu-id="7189e-136">Also note that the connection string does not contain a server name and database name.</span></span> <span data-ttu-id="7189e-137">Důvodem je, že **OpenConnectionForKey** volání bude automaticky přímé připojení k správné horizontálního oddílu na základě klíče.</span><span class="sxs-lookup"><span data-stu-id="7189e-137">That is because the **OpenConnectionForKey** call will automatically direct the connection to the correct shard based on the key.</span></span> <span data-ttu-id="7189e-138">Proto nejsou zadány název databáze a název serveru.</span><span class="sxs-lookup"><span data-stu-id="7189e-138">Hence, the database name and server name are not provided.</span></span> 

## <a name="see-also"></a><span data-ttu-id="7189e-139">Viz také</span><span class="sxs-lookup"><span data-stu-id="7189e-139">See also</span></span>
[<span data-ttu-id="7189e-140">Správa databází a přihlášení ve službě Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="7189e-140">Managing databases and logins in Azure SQL Database</span></span>](sql-database-manage-logins.md)

[<span data-ttu-id="7189e-141">Zabezpečení služby SQL Database</span><span class="sxs-lookup"><span data-stu-id="7189e-141">Securing your SQL Database</span></span>](sql-database-security-overview.md)

[<span data-ttu-id="7189e-142">Začínáme s úlohami elastické databáze</span><span class="sxs-lookup"><span data-stu-id="7189e-142">Getting started with Elastic Database jobs</span></span>](sql-database-elastic-jobs-getting-started.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

