---
title: "aaaAzure SQL elastické škálování, – nejčastější dotazy | Microsoft Docs"
description: "Časté otázky k Azure SQL Database elastické škálování."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: e60dde9c-bb7b-4f2f-b52c-bdb506d49fcb
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 8c77902e8ce9cbbc5e081cd9d2c911d4c8dc9e5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="elastic-database-tools-faq"></a>Nástroje elastické databáze – nejčastější dotazy
#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-hello-sharding-key-for-hello-schema-info"></a>Jak se pokud je nutné klienta jednou za horizontálního oddílu a žádný klíč horizontálního dělení, naplnit hello horizontálního dělení klíč pro informace o schématu hello?
objekt informace o schématu Hello je pouze použité toosplit sloučení scénáře. Pokud aplikace je ze své podstaty jednoho klienta, nevyžaduje Nástroj sloučení rozdělení hello a proto neexistuje žádný nutné toopopulate hello schématu informace objektů.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Jste vybaveny databáze a už mám manažera mapování horizontálních, jak zaregistrovat novou databázi jako horizontálního oddílu?
Najdete v tématu  **[přidání aplikace tooan horizontálního oddílu pomocí klientské knihovny pro elastické databáze hello](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Kolik nástroje elastické databáze stojí?
Pomocí klientské knihovny pro elastické databáze hello nedojde veškeré náklady. Pouze pro databáze Azure SQL hello, které používáte pro horizontálních oddílů a hello správce mapy horizontálního oddílu, jakož i hello webové/role pracovního procesu, který zřídíte pro nástroj sloučení rozdělení hello naběhnou první náklady.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Proč po přidání horizontálního oddílu z jiného serveru nejsou k práci pomocí pověření?
Nepoužívejte přihlašovací údaje ve formě hello "ID uživatele =username@servername", jednoduše použijte "ID uživatele = uživatelské jméno".  Také ujistěte se, že tento přihlášení "username" hello má oprávnění k hello horizontálního oddílu.

#### <a name="do-i-need-toocreate-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Proveďte nutné toocreate manažera mapy horizontálního oddílu a naplnit horizontálních oddílů při každém spuštění aplikace?
Ne – hello vytvoření hello horizontálního oddílu mapa správce (například  **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) se o jednorázovou operaci.  Vaše aplikace by měla používat hello volání  **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)**  v době spuštění aplikace.  Existuje má pouze jedno takové volání za domény aplikace.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Otázky týkající se používání nástroje elastické databáze je nutné, jak je odpovědi získat?
Prosím oslovení toous na hello [fórum Azure SQL Database](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-hello-same-shard--is-this-by-design"></a>Při připojení k databázi pomocí klíče horizontálního dělení doručení, I můžete stále dotaz na data pro další horizontálního dělení klíče na hello stejné ID horizontálního oddílu.  Je to záměrné?
Hello elastické škálování rozhraní API získáte připojení správné databázi toohello horizontálního dělení klíče, ale neposkytuje klíče filtrování horizontálního dělení.  Přidat **kde** klauzule tooyour dotazu toorestrict hello oboru toohello zadali horizontálního dělení klíč, v případě potřeby.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Lze použít na jinou edici databáze Azure pro každý horizontálního oddílu v sadě horizontálního oddílu?
Ano, horizontálního oddílu je jednotlivé databáze a jeden horizontálních proto může být na edici Premium, zatímco jiné se edice Standard. Hello edici horizontálního oddílu Další, můžete škálovat nahoru nebo dolů vícekrát během hello životnost hello horizontálního oddílu.

#### <a name="does-hello-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Ukládá hello rozdělení sloučení nástroj zřídit (nebo odstranit) databáze během operace rozdělení nebo sloučení?
Ne. Pro **rozdělení** operace, hello cílová databáze pomocí příslušného schématu hello, musí existovat a být zaregistrována hello správce mapy horizontálního oddílu.  Pro **sloučení** operace, je potřeba odstranit hello horizontálních ze hello horizontálního oddílu mapa správce a pak odstraňte hello databáze.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

