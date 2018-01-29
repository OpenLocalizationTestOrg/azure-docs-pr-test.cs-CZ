---
title: "Podporované typy prostředků pomocí Azure Resource Health | Microsoft Docs"
description: "Podporované typy prostředků prostřednictvím stavu prostředků Azure"
services: Resource health
documentationcenter: 
author: BernardoAMunoz
manager: 
editor: 
ms.assetid: 85cc88a4-80fd-4b9b-a30a-34ff3782855f
ms.service: service-health
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: Supportability
ms.date: 10/09/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: b67e72af8a34799af97cd46f968636050f4ce485
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 12/11/2017
---
# <a name="resource-types-and-health-checks-in-azure-resource-health"></a>Typy prostředků a stav kontrol ve stavu prostředků Azure.
Níže je úplný seznam všechny kontroly provedené prostřednictvím stav prostředku pro typy prostředků.

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service
|Spuštění kontroly|
|---|
|<ul><li>Služba Api Management i spuštěny?</li></ul>|

## <a name="microsoftcacheredisredis"></a>Microsoft.CacheRedis/Redis
|Spuštění kontroly|
|---|
|<ul><li>Jsou všechny uzly mezipaměti provozu?</li><li>Lze mezipaměti přejít z v rámci datového centra?</li><li>Mezipaměti byl dosažen maximální počet připojení?</li><li> Vyčerpala mezipaměti jeho dostupné paměti? </li><li>Dochází k mezipaměti velký počet chyb stránek?</li><li>V případě velkého zatížení je mezipaměti?</li></ul>|

## <a name="microsoftcdnprofile"></a>Microsoft.CDN/profile
|Spuštění kontroly|
|---|
|<ul> <li>Žádné koncové body byla zastavena, odebrané nebo nesprávně nakonfigurované?</li><li>Je dostupná pro CDN operací konfigurace na doplňkovém portálu?</li><li>Existují probíhající doručení problémy s koncovými body CDN?</li><li>Mohou uživatelé změnit konfiguraci jejich CDN prostředky?</li><li>Jsou změny konfigurace šíření očekávané rychlostí?</li><li>Mohou uživatelé spravovat konfiguraci CDN pomocí portálu Azure, PowerShell nebo rozhraní API?</li> </ul>|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.classiccompute/virtualmachines
|Spuštění kontroly|
|---|
|<ul><li>Hostitelský server je spuštěný a funkční?</li><li>Spuštění operačního systému hostitele dokončil?</li><li>Kontejner virtuálního počítače je zřízený a zapnut?</li><li>Je k dispozici síťové připojení mezi hostitelem a účet úložiště?</li><li>Spouštění hostovaný operační systém dokončil?</li><li>Je k dispozici probíhající plánované údržby?</li></ul>|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.cognitiveservices/accounts
|Spuštění kontroly|
|---|
|<ul><li>Lze účet přejít z v rámci datového centra?</li><li>Je k dispozici kognitivní poskytovatelem prostředků služby?</li><li>Je k dispozici služba kognitivní v příslušné oblasti?</li><li>Můžete přečíst na účet úložiště, která uchovává metadata prostředků provádět operace?</li><li>Bylo dosaženo kvóty volání rozhraní API?</li><li>Bylo dosaženo čtení limit volání rozhraní API?</li></ul>|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.COMPUTE/virtualmachines
|Spuštění kontroly|
|---|
|<ul><li>Je server hostování tohoto virtuálního počítače nahoru a systémem?</li><li>Spuštění operačního systému hostitele dokončil?</li><li>Kontejner virtuálního počítače je zřízený a zapnut?</li><li>Je k dispozici síťové připojení mezi hostitelem a účet úložiště?</li><li>Spouštění hostovaný operační systém dokončil?</li><li>Je k dispozici probíhající plánované údržby?</li></ul>|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.datalakeanalytics/accounts
|Spuštění kontroly|
|---|
|<ul><li>Mohou uživatelé odeslání úloh Data Lake Analytics v oblasti?</li><li>Proveďte základní úlohy spustit a úspěšně dokončit v oblasti?</li><li>Uživatelé, můžete seznam položek katalogu v oblasti?</li>|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.datalakestore/accounts
|Spuštění kontroly|
|---|
|<ul><li>Můžete uživatelům odesílání dat do Data Lake Store v oblasti?</li><li>Mohou uživatelé stáhnout dat z Data Lake Store v oblasti?</li></ul>|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/iothubs

|Spuštění kontroly|
|---|
|<ul><li>Je spuštěná služba IoT hub?</li></ul>|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.documentdb/databaseAccounts
|Spuštění kontroly|
|---|
|<ul><li>Existuje byly všechny databáze nebo kolekce žádosti není zpracovat z důvodu nedostupnosti služby databázi Cosmos Azure?</li><li>Existuje byly všechny žádosti dokument není zpracovat z důvodu nedostupnosti služby databázi Cosmos Azure?</li></ul>|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/Connections
|Spuštění kontroly|
|---|
|<ul><li>Je připojen tunelového připojení sítě VPN?</li><li>Jsou konfliktům v konfiguraci připojení?</li><li>Jsou předsdílených klíčů správně nakonfigurovaná?</li><li>Je dostupná místní zařízení VPN?</li><li>Existují neshody v zásadách zabezpečení protokolu IPSec/IKE?</li><li>Správně zřízený, nebo ve stavu selhání, je připojení S2S VPN?</li><li>Správně zřízený, nebo ve stavu selhání, je připojení VNET-to-VNET?</li></ul>|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.network/virtualNetworkGateways
|Spuštění kontroly|
|---|
|<ul><li>Brána VPN je dosažitelný z Internetu?</li><li>Brána VPN je v pohotovostním režimu?</li><li>Je spuštěna služba VPN na bráně?</li></ul>|

## <a name="microsoftnotificationhubsnamespace"></a>Microsoft.NotificationHubs/namespace
|Spuštění kontroly|
|---|
|<ul><li> Modul runtime operací, jako je registrace, instalaci nebo odeslání provést v oboru názvů?</li></ul>|

## <a name="microsoftpowerbiworkspacecollections"></a>Microsoft.PowerBI/workspaceCollections
|Spuštění kontroly|
|---|
|<ul><li>Hostitelský operační systém je v provozu?</li><li>Je workspaceCollection dosažitelný z mimo datové centrum?</li><li>Zprostředkovatel prostředků PowerBI je k dispozici?</li><li>Je k dispozici služba PowerBI v příslušné oblasti?</li></ul>|

## <a name="microsoftsearchsearchservices"></a>Microsoft.search/searchServices
|Spuštění kontroly|
|---|
|<ul><li>Diagnostika operací provést v clusteru?</li></ul>|

## <a name="microsoftsqlserverdatabase"></a>Microsoft.SQL/Server/database
|Spuštění kontroly|
|---|
|<ul><li> Existuje byli přihlášení k databázi?</li></ul>|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs
|Spuštění kontroly|
|---|
|<ul><li>Jsou všichni hostitelé, kdy je úloha provádění nahoru a používá?</li><li>Nebylo možné spustit úlohu?</li><li>Existují probíhající runtime upgrady?</li><li>Úloha v očekávané stavu (například spuštění nebo zastavení zákazníkem)?</li><li>Úloha došlo limitu paměti výjimek?</li><li>Existují probíhající výpočetní naplánované aktualizace?</li><li>Správce spuštění (plán řízení) je k dispozici?</li></ul>|

## <a name="microsoftwebserverfarms"></a>Microsoft.web/serverFarms
|Spuštění kontroly|
|---|
|<ul><li>Hostitelský server je spuštěný a funkční?</li><li>Běží Internetová informační služba</li><li>Je spuštěn nástroj pro vyrovnávání zatížení?</li><li>Webové služby plánování dosažitelný z v rámci datového centra?</li><li>Účet úložiště je hostitelem lokality obsah pro serverovou farmu, která je k dispozici??</li></ul>|

## <a name="microsoftwebsites"></a>Microsoft.Web/Sites
|Spuštění kontroly|
|---|
|<ul><li>Hostitelský server je spuštěný a funkční?</li><li>Je spuštěn server Internet Information server?</li><li>Je spuštěn nástroj pro vyrovnávání zatížení?</li><li>Webové aplikace dosažitelný z v rámci datového centra?</li><li>Účet úložiště je hostitelem obsahu webu, který je k dispozici?</li></ul>|

# <a name="next-steps"></a>Další kroky
-  V tématu [Úvod do stavu služby Azure](service-health-overview.md) a [Úvod do Azure Resource Health](resource-health-overview.md) bližší informace o nich. 
-  [Nejčastější dotazy o stavu prostředků Azure](resource-health-faq.md)
- Nastavení výstrah, takže je znázorněna problémy v oblasti stavu. Další informace najdete v tématu [konfigurovat výstrahy pro stav služby](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md). 