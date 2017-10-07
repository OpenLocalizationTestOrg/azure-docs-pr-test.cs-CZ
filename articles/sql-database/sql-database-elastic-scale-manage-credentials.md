---
title: "přihlašovací údaje aaaManaging v klientské knihovny pro elastické databáze hello | Microsoft Docs"
description: "Jak tooset hello správnou úroveň přihlašovací údaje správce tooread jen pro elastické databáze aplikace"
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
ms.openlocfilehash: 218783ca2a07e3c0a4b089aa92634f32c41386e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="credentials-used-tooaccess-hello-elastic-database-client-library"></a>Přihlašovací údaje používají Klientská knihovna pro tooaccess hello elastické databáze
Hello [klientské knihovny pro elastické databáze](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/) používá tři různé druhy pověření tooaccess hello [správce mapy horizontálního oddílu](sql-database-elastic-scale-shard-map-management.md). V závislosti na hello nutnost použijte pověření hello s nejnižší úroveň hello možný přístup.

* **Přihlašovacích údajů pro správu**: pro vytvoření nebo manipulace s manažera mapy horizontálního oddílu. (Viz hello [Glosář](sql-database-elastic-scale-glossary.md).) 
* **Přístup k pověření**: tooaccess existující horizontálního oddílu mapování informací o tooobtain manager o horizontálních oddílů.
* **Přihlašovací údaje pro připojení**: tooconnect tooshards. 

Viz také [Správa databází a přihlašovacích údajů ve službě Azure SQL Database](sql-database-manage-logins.md). 

## <a name="about-management-credentials"></a>O přihlašovacích údajů pro správu
Přihlašovacích údajů pro správu jsou použité toocreate [ **ShardMapManager** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx) objekt pro aplikace, které pracují s horizontálního oddílu mapy. (Například najdete v části [přidání horizontálních, pomocí nástroje elastické databáze](sql-database-elastic-scale-add-a-shard.md) a [Data závislé směrování](sql-database-elastic-scale-data-dependent-routing.md)) uživatele hello Klientská knihovna pro elastické škálování hello vytvoří uživatelé hello SQL a přihlášeních SQL a zajišťuje, každý je uděleno oprávnění pro čtení a zápis hello na globální horizontálního oddílu hello mapovat databáze a také všechny databáze horizontálního oddílu. Tyto přihlašovací údaje jsou použité toomaintain hello globální horizontálních mapy a hello místní horizontálních mapy při jsou prováděny změny toohello horizontálního oddílu mapy. Například použití hello správu přihlašovací údaje toocreate hello horizontálního oddílu mapy manager objektu (pomocí [ **GetSqlShardMapManager**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx): 

    // Obtain a shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmAdminConnectionString, 
            ShardMapManagerLoadPolicy.Lazy 
    ); 

Proměnná Hello **smmAdminConnectionString** je připojovací řetězec, který obsahuje hello přihlašovacích údajů pro správu. Hello ID uživatele a heslo poskytuje databáze mapy horizontálního oddílu tooboth přístup pro čtení a zápis a jednotlivých horizontálních oddílů. Hello správu připojovací řetězec také zahrnuje hello serveru název a databáze název tooidentify hello globální horizontálních mapy databáze. Zde je typické připojovací řetězec pro tento účel:

     "Server=<yourserver>.database.windows.net;Database=<yourdatabase>;User ID=<yourmgmtusername>;Password=<yourmgmtpassword>;Trusted_Connection=False;Encrypt=True;Connection Timeout=30;” 

Nepoužívejte hodnoty ve formě hello "username@server" – místo toho použijte hodnotu "username" hello.  Je to proto, že přihlašovací údaje musí pracovat v hello horizontálního oddílu mapa správce databáze a jednotlivých horizontálních oddílů, které mohou být na různých serverech.

## <a name="access-credentials"></a>Přihlašovací údaje
Při vytváření horizontálního oddílu správce mapy v aplikaci, která není spravovat horizontálního oddílu mapy, použijte přihlašovací údaje, které mají oprávnění jen pro čtení na mapě globální horizontálních hello. Hello informace získané z mapy globální horizontálních hello pod tyto přihlašovací údaje se používají pro [závislé na data směrování](sql-database-elastic-scale-data-dependent-routing.md) a toopopulate hello horizontálních mapování mezipaměti na klientovi hello. Hello přihlašovací údaje jsou k dispozici prostřednictvím hello stejné příliš volání vzor**GetSqlShardMapManager** jako v příkladu nahoře: 

    // Obtain shard map manager. 
    ShardMapManager shardMapManager = ShardMapManagerFactory.GetSqlShardMapManager( 
            smmReadOnlyConnectionString, 
            ShardMapManagerLoadPolicy.Lazy
    );  

Všimněte si použití hello hello **smmReadOnlyConnectionString** tooreflect hello použít různé přihlašovací údaje pro tento přístup jménem **bez oprávnění správce** uživatelů: tyto přihlašovací údaje by neměly poskytovat zápisu oprávnění na mapě globální horizontálních hello. 

## <a name="connection-credentials"></a>Přihlašovací údaje pro připojení
Další přihlašovací údaje jsou potřeba při používání hello [ **OpenConnectionForKey** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx) metoda tooaccess horizontálních, přidružené k klíči horizontálního dělení. Tyto přihlašovací údaje potřebovat tooprovide oprávnění pro přístup jen pro čtení toohello místní horizontálních mapy tabulky umístěný na hello horizontálního oddílu. Toto je závislé na data směrování na hello horizontálních potřebné tooperform ověření připojení. Tento fragment kódu umožňuje přístup k datům v kontextu hello závislé směrování dat: 

    using (SqlConnection conn = rangeMap.OpenConnectionForKey<int>( 
    targetWarehouse, smmUserConnectionString, ConnectionOptions.Validate)) 

V tomto příkladu **smmUserConnectionString** obsahuje hello připojovací řetězec pro hello přihlašovací údaje uživatele. Pro databáze SQL Azure zde je typické připojovací řetězec pro přihlašovací údaje uživatele: 

    "User ID=<yourusername>; Password=<youruserpassword>; Trusted_Connection=False; Encrypt=True; Connection Timeout=30;”  

Stejně jako u hello přihlašovací údaje správce, nejsou hodnoty ve formě hello "username@server". Místo toho použijte "username".  Všimněte si také, zda text hello připojovací řetězec neobsahuje název serveru a název databáze. Je to způsobeno hello **OpenConnectionForKey** volání bude automaticky nasměrovat hello připojení toohello správné horizontálního oddílu na základě hello klíče. Proto nejsou zadány hello název databáze a název serveru. 

## <a name="see-also"></a>Viz také
[Správa databází a přihlášení ve službě Azure SQL Database](sql-database-manage-logins.md)

[Zabezpečení služby SQL Database](sql-database-security-overview.md)

[Začínáme s úlohami elastické databáze](sql-database-elastic-jobs-getting-started.md)

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

