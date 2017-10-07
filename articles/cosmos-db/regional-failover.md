---
title: "aaaRegional převzetí služeb při selhání v Azure Cosmos DB | Microsoft Docs"
description: "Další informace o tom, jak ruční a automatické převzetí služeb při selhání funguje s Azure Cosmos DB."
services: cosmos-db
documentationcenter: 
author: arramac
manager: jhubbard
editor: 
ms.assetid: 446e2580-ff49-4485-8e53-ae34e08d997f
ms.service: cosmos-db
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2017
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d2fdc7b0e8958d129ab027e4b11193b12961ddae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="automatic-regional-failover-for-business-continuity-in-azure-cosmos-db"></a>Automatické regionální převzetí služeb při selhání pro kontinuitu podnikových procesů v Azure Cosmos DB
Azure Cosmos DB zjednodušuje hello globální distribuci dat tím, že nabídka plně spravované, [účty databáze více oblast](distribute-data-globally.md) , poskytovat jasné kompromisy mezi konzistencí, dostupnosti a výkonu, všechny odpovídající záruky. Účty cosmos DB nabízí vysokou dostupnost, jednu číslici ms latenci, [dobře definované úrovně konzistence](consistency-levels.md), transparentní regionální převzetí služeb při selhání s více funkci rozhraní API a hello možnost tooelastically škálování propustnost a úložiště mezi hello zeměkouli. 

Cosmos DB podporuje obě explicitní a převzetí služeb při selhání na základě zásad, umožňují chování systému začátku do konce hello toocontrol hello události chyb. V tomto článku se podíváme na to:

* Jak fungují ruční převzetí služeb při selhání v systému Cosmos DB?
* Jak pracovní automatické převzetí služeb při selhání v systému Cosmos DB a co se stane, když data center přejde dolů?
* Jak můžete použít ruční převzetí služeb při selhání v architekturách aplikací?

Můžete si také přečíst o místní převzetí služeb při selhání v této Azure pátek video s Scott Hanselman a Karthik Raman hlavní manažer inženýrství.

>[!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Planet-Scale-NoSQL-with-DocumentDB/player]  

## <a id="ConfigureMultiRegionApplications"></a>Konfigurace aplikací s více oblast
Před jsme ponořte do režimy převzetí služeb při selhání, podíváme se na tom, jak můžete konfigurovat výhodou aplikace tootake dostupnost v několika oblastech a být odolné hello stěně regionální převzetí služeb při selhání.

* Nejdřív Nasaďte aplikaci do několika oblastí
* přístup s nízkou latencí tooensure z každé oblasti vaše aplikace je nasazená, nakonfigurujte hello odpovídající [seznam upřednostňovaných oblasti](https://msdn.microsoft.com/library/microsoft.azure.documents.client.connectionpolicy.preferredlocations.aspx#P:Microsoft.Azure.Documents.Client.ConnectionPolicy.PreferredLocations) pro každou oblast prostřednictvím jednoho hello nepodporuje sady SDK.

Následující fragment kódu ukazuje, jak Hello tooinitialize více oblastech aplikace. Zde hello účet Azure Cosmos DB `contoso.documents.azure.com` je nakonfigurovaný se dvěma oblastmi - západní USA a Severní Evropa. 

* nasazení aplikace Hello v oblasti západní USA hello (například pomocí služby Azure App Services) 
* Nakonfigurované s `West US` jako první upřednostňovaná oblast pro s nízkou latencí hello čte
* Nakonfigurované s `North Europe` jako druhý upřednostňovaná oblast hello (pro vysokou dostupnost při místní selhání)

Hello DocumentDB rozhraní API tato konfigurace vypadá hello následující fragment kódu:

```cs
ConnectionPolicy usConnectionPolicy = new ConnectionPolicy 
{ 
    ConnectionMode = ConnectionMode.Direct,
    ConnectionProtocol = Protocol.Tcp
};

usConnectionPolicy.PreferredLocations.Add(LocationNames.WestUS);
usConnectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope);

DocumentClient usClient = new DocumentClient(
    new Uri("https://contosodb.documents.azure.com"),
    "memf7qfF89n6KL9vcb7rIQl6tfgZsRt5gY5dh3BIjesarJanYIcg2Edn9uPOUIVwgkAugOb2zUdCR2h0PTtMrA==",
    usConnectionPolicy);
```

aplikace Hello je také nasazený v oblastech severní Evropa hello s pořadím hello oblastí upřednostňované vrátit zpět. To znamená oblast Severní Evropa hello je nejdřív určené pro čtení s nízkou latencí. Potom oblast západní USA hello je zadán jako hello druhý upřednostňovaná oblast pro zajištění vysoké dostupnosti při selhání místní.

Hello architektura znázorňuje následující obrázek k nasazení aplikace s více oblasti, kde Cosmos databáze a aplikace hello jsou nakonfigurované toobe k dispozici v čtyři Azure zeměpisné oblasti.  

![Globálně distribuované nasazení s Azure Cosmos DB](./media/regional-failover/app-deployment.png)

Teď se podíváme, jak hello Cosmos DB služba zpracovává místní selhání prostřednictvím automatické převzetí služeb při selhání. 

## <a id="AutomaticFailovers"></a>Automatické převzetí služeb při selhání
V hello výjimečná událost Azure regionální výpadku nebo výpadku datového centra Cosmos DB automaticky aktivuje převzetí služeb při selhání všech účtů Cosmos DB s přítomnosti v oblasti hello vliv. 

**Co se stane, pokud má čtení oblast výpadku?**

Cosmos DB účty s čtení oblastí v jedné oblasti hello vliv jsou automaticky odpojen od jejich oblast zápisu a označeny offline. Hello Cosmos DB SDK implementace protokolu regionální zjišťování, který vám umožní tooautomatically rozpoznat, kdy je k dispozici v oblasti a přesměrování číst volání toohello další dostupné oblasti v seznamu upřednostňovaná oblast hello. Žádná z oblasti hello v hello v případě, že seznam oblast je k dispozici, volání automaticky vrátit toohello aktuální zápisu oblasti. Vaše aplikace kód toohandle regionální převzetí služeb při selhání se nevyžadují žádné změny. Během celého tohoto procesu dál konzistence záruky toobe berou v úvahu Cosmos DB.

![Chyby čtení oblasti v Azure Cosmos DB](./media/regional-failover/read-region-failures.png)

Jakmile hello vliv oblast obnoví z hello výpadku, se automaticky obnoví všechny účty Cosmos DB hello vliv v oblasti hello službou hello. Cosmos DB účty, které měl čtení oblast v oblasti hello vliv se pak automaticky synchronizovat s aktuální oblast zápisu a zapněte online. Hello Cosmos DB sady SDK zjišťovat dostupnost hello hello nové oblasti a vyhodnocení, zda hello oblast, měl by být vybrán jako hello aktuální čtení oblast na základě seznamu upřednostňovaná oblast hello nakonfigurovaná hello aplikací. Další čtení jsou přesměrovaného toohello obnovené oblasti bez nutnosti kódu aplikace tooyour žádné změny.

**Co se stane, pokud oblast zápisu má výpadek?**

Pokud hello ovlivněných oblast je oblast hello aktuální zápis pro daný účet Cosmos DB, pak hello oblast budou automaticky označeny jako offline. Oblast alternativní se poté vyzval jako hello zápisu oblast každý ovlivněných účet Cosmos DB. Plnou kontrolu nad hello oblast výběr pořadí pro vaše účty Cosmos DB prostřednictvím hello portál Azure nebo [prostřednictvím kódu programu](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_FailoverPriorityChange). 

![Priorit převzetí služeb při selhání pro Azure Cosmos DB](./media/regional-failover/failover-priorities.png)

Při automatické převzetí služeb při selhání, Cosmos DB automaticky zvolí takovou hello další oblasti zápisu pro danou DB Cosmos účet podle hello zadané pořadí podle priority. 

![Oblast chyby zápisu v Azure Cosmos DB](./media/regional-failover/write-region-failures.png)

Jakmile hello vliv oblast obnoví z hello výpadku, se automaticky obnoví všechny účty Cosmos DB hello vliv v oblasti hello službou hello. 

* Účty cosmos DB s jejich předchozí oblast zápisu v oblasti hello vliv zůstanou v offline režimu s dostupností pro čtení i po obnovení hello hello oblasti. 
* Můžete dát dotaz na tuto oblast toocompute všech nereplikovaných zápisu během výpadku hello porovnáním s daty hello k dispozici v aktuální oblasti zápisu hello. Podle potřeb hello vaší aplikace, můžete proveďte sloučení nebo konflikt překladu a zápis hello poslední sadu změn zpět toohello aktuální zápisu oblasti. 
* Po dokončení sloučení změn, můžete zahrnout hello vliv oblast zpět online odebráním a nové přidání hello oblast tooyour Cosmos DB účtu. Oblast hello je přidána zpět, můžete ho nakonfigurovat zpět jako oblast zápisu hello provedením ruční převzetí služeb při selhání prostřednictvím hello portál Azure nebo [prostřednictvím kódu programu](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_CreateOrUpdate).

## <a id="ManualFailovers"></a>Ruční převzetí služeb při selhání

Kromě toho tooautomatic převzetí služeb při selhání, hello aktuální zápisu oblast daný účet Cosmos DB můžete ručně změnit dynamicky tooone existující čtení oblastí hello. Ruční převzetí služeb při selhání lze inicializovat prostřednictvím hello portál Azure nebo [prostřednictvím kódu programu](https://docs.microsoft.com/rest/api/documentdbresourceprovider/databaseaccounts#DatabaseAccounts_CreateOrUpdate). 

Ruční převzetí služeb při selhání zkontrolujte **nula ztráty dat** a **nula dostupnosti** ztrátě a řádně stav zápisu přenos ze staré hello zapisovat oblast toohello nový pro hello zadán účet Cosmos DB. Jako v automatické převzetí služeb při selhání, hello Cosmos DB SDK automaticky zápis obslužné rutiny oblasti změny během ruční převzetí služeb při selhání a zajišťuje, aby volání automaticky přesměrovaného toohello nový zápis oblast. Převzetí služeb vaší aplikace toomanage se nevyžadují žádné změny kódu nebo konfigurace. 

![Ruční převzetí služeb při selhání v Azure Cosmos DB](./media/regional-failover/manual-failovers.png)

Jsou některé běžné scénáře hello kde ruční převzetí služeb při selhání může být užitečná:

**Postupujte podle modelu hodiny hello**: Pokud vaše aplikace vzory předvídatelný přenosů dat podle hello hello denní dobu, můžete změnit pravidelně hello zápisu stavu toohello Nejaktivnější geografické oblasti na základě času dne hello.

**Aktualizace služby**: určité nasazení globálně distribuované aplikace může zahrnovat přesměrování spojnic oblast toodifferent provoz prostřednictvím Správce provozu během jejich aktualizace plánované služby. Nasazení takové aplikace teď můžete použít ruční převzetí služeb při selhání tookeep hello zápisu stavu toohello oblast existuje kam toobe active provozu během aktualizace hello správu a údržbu.

**Kontinuita podnikových procesů a zotavení po havárii (BCDR) a vysokou dostupnost a zotavení po havárii (HADR) cvičení**: většinu aplikací enterprise zahrnují testy kontinuity obchodních jako součást procesu jejich vývoj a verzi. BCDR a testování HADR je často důležitým krokem při certifikace dodržování předpisů a záruční dostupnost služeb v případě hello regionální výpadků. Můžete otestovat hello BCDR připravenosti vaší aplikace, které používají Cosmos DB pro úložiště podle aktivován ruční převzetí služeb při selhání účtu Cosmos DB nebo přidávání a odebírání oblast dynamicky.

V tomto článku jsme přečetli jak ruční a automatické převzetí služeb při selhání práci v Cosmos DB, a způsob konfigurace vaší databáze Cosmos účty a aplikace toobe globálně dostupnou. S využitím podpory Cosmos DB globální replikace, můžete zlepšit latenci začátku do konce a zajistěte, aby vysoce dostupné ani v případě selhání oblast hello. 

## <a id="NextSteps"></a>Další kroky
* Další informace o tom, jak Cosmos DB podporuje [globální distribuční](distribute-data-globally.md)
* Další informace o [globální konzistence s Azure Cosmos DB](consistency-levels.md)
* Vývoj s více oblastí pomocí Azure Cosmos DB [DocumentDB rozhraní API](../cosmos-db/tutorial-global-distribution-documentdb.md)
* Zjistěte, jak toobuild [více oblast zapisovače architektury](multi-region-writers.md) pomocí Azure DocumentDB

