---
title: "aaaSupported typy prostředků pomocí Azure Resource Health | Microsoft Docs"
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
ms.date: 03/19/2017
ms.author: BernardoAMunoz
ms.openlocfilehash: fc7bef214702f8ba6954b5aca62236b38023d27a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="resource-types-and-health-checks-in-azure-resource-health"></a>Typy prostředků a stav kontrol ve stavu prostředků Azure.
Níže je úplný seznam kontrol hello provedený typy prostředků stav prostředku.

## <a name="microsoftcacheredisredis"></a>Microsoft.CacheRedis/Redis
|Spuštění kontroly|
|---|
|<ul><li>Jsou všechny uzly mezipaměti hello a systémem?</li><li>Lze hello mezipaměti přejít z v rámci datového centra hello?</li><li>Má hello dosaženo maximálního počtu připojení hello mezipaměti?</li><li> Vyčerpala hello mezipaměti jeho dostupné paměti? </li><li>Dochází k hello mezipaměti velký počet chyb stránek?</li><li>V případě velkého zatížení je hello mezipaměti?</li></ul>|

## <a name="microsoftcdnprofile"></a>Microsoft.CDN/profile
|Spuštění kontroly|
|---|
|<ul> <li>Žádné koncové body hello byla zastavena, odebrané nebo nesprávně nakonfigurované?</li><li>Je dostupná pro operací konfigurace CDN doplňkovém portálu hello?</li><li>Existují probíhající doručení problémy s hello koncové body CDN?</li><li>Uživatelé změnit konfiguraci hello jejich CDN prostředky?</li><li>Jsou změny konfigurace šíření rychlostí hello očekávání?</li><li>Mohou uživatelé spravovat konfiguraci hello CDN pomocí hello portálu Azure, PowerShell nebo hello rozhraní API?</li> </ul>|

## <a name="microsoftclassiccomputevirtualmachines"></a>Microsoft.classiccompute/virtualmachines
|Spuštění kontroly|
|---|
|<ul><li>Hostitelský server hello i spuštěny?</li><li>Spuštění operačního systému hostitele hello dokončil?</li><li>Kontejner hello virtuálního počítače je zřízený a zapnut?</li><li>Je k dispozici síťové připojení mezi hello hostitele a účet úložiště hello?</li><li>Spouštění ze hello hello hostovaný operační systém dokončil?</li><li>Je k dispozici probíhající plánované údržby?</li></ul>|

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.cognitiveservices/accounts
|Spuštění kontroly|
|---|
|<ul><li>Lze hello účet přejít z v rámci datového centra hello?</li><li>Je hello kognitivní služby poskytovatele prostředků k dispozici?</li><li>Je hello kognitivní služby k dispozici v příslušné oblasti hello?</li><li>Můžete přečíst na účet úložiště hello podržíte hello prostředků metadata provádět operace?</li><li>Bylo dosaženo kvóty volání hello rozhraní API?</li><li>Bylo dosaženo volání hello rozhraní API pro čtení limit?</li></ul>|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.COMPUTE/virtualmachines
|Spuštění kontroly|
|---|
|<ul><li>Je hello server hostování tohoto virtuálního počítače nahoru a systémem?</li><li>Spuštění operačního systému hostitele hello dokončil?</li><li>Kontejner hello virtuálního počítače je zřízený a zapnut?</li><li>Je k dispozici síťové připojení mezi hello hostitele a účet úložiště hello?</li><li>Spouštění ze hello hello hostovaný operační systém dokončil?</li><li>Je k dispozici probíhající plánované údržby?</li></ul>|

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.datalakeanalytics/accounts
|Spuštění kontroly|
|---|
|<ul><li>Může uživatele, které odeslání úlohy tooData Lake Analytics v hello oblasti?</li><li>Proveďte základní úlohy spuštění a dokončení úspěšně v hello oblasti?</li><li>Uživatelé, můžete seznam položek katalogu v oblasti hello?</li>|


## <a name="microsoftdatalakestoreaccounts"></a>Microsoft.datalakestore/accounts
|Spuštění kontroly|
|---|
|<ul><li>Uživatele můžete nahrát data tooData Lake Store v oblasti hello?</li><li>Mohou uživatelé stáhnout dat z Data Lake Store v oblasti hello?</li></ul>|

## <a name="microsoftdocumentdbdatabaseaccounts"></a>Microsoft.documentdb/databaseAccounts
|Spuštění kontroly|
|---|
|<ul><li>Existuje byly všechny databáze nebo kolekce žádosti není zpracovat z důvodu nedostupnosti služby tooa DocumentDB?</li><li>Existuje byly všechny žádosti dokument není zpracovat z důvodu nedostupnosti služby tooa DocumentDB?</li></ul>|

## <a name="microsoftnetworkconnections"></a>Microsoft.Network/Connections
|Spuštění kontroly|
|---|
|<ul><li>Je připojen hello tunelového připojení sítě VPN?</li><li>Jsou konflikty konfigurace hello připojení?</li><li>Jsou hello předsdílených klíčů správně nakonfigurovaná?</li><li>Je zařízení místní VPN hello dostupný?</li><li>Existují neshody v zásadách zabezpečení protokolu IPSec/IKE hello?</li><li>Připojení S2S VPN hello je správně zřízený, nebo ve stavu selhání?</li><li>Správně zřízený, nebo ve stavu selhání, je připojení hello VNET-to-VNET?</li></ul>|

## <a name="microsoftnetworkvirtualnetworkgateways"></a>Microsoft.network/virtualNetworkGateways
|Spuštění kontroly|
|---|
|<ul><li>Je dostupný z brány sítě VPN hello hello Internetu?</li><li>Brána sítě VPN v pohotovostním režimu je hello?</li><li>Běží hello služby VPN na bráně hello?</li></ul>|

## <a name="microsoftnotificationhubsnamespace"></a>Microsoft.NotificationHubs/namespace
|Spuštění kontroly|
|---|
|<ul><li> Modul runtime operací, jako je registrace, instalaci nebo odeslání provést v oboru názvů hello?</li></ul>|

## <a name="microsoftpowerbiworkspacecollections"></a>Microsoft.PowerBI/workspaceCollections
|Spuštění kontroly|
|---|
|<ul><li>Hello hostitelský operační systém i spuštěny?</li><li>Je dostupné z oblasti mimo datové centrum hello hello workspaceCollection?</li><li>Poskytovatel prostředků PowerBI k dispozici je hello?</li><li>Služba PowerBI v příslušné oblasti hello je hello?</li></ul>|

## <a name="microsoftsearchsearchservices"></a>Microsoft.search/searchServices
|Spuštění kontroly|
|---|
|<ul><li>Můžete na hello clusteru provádět operace diagnostiky?</li></ul>|

## <a name="microsoftsqlserverdatabase"></a>Microsoft.SQL/Server/database
|Spuštění kontroly|
|---|
|<ul><li> Byly existuje databáze toohello přihlášení?</li></ul>|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs
|Spuštění kontroly|
|---|
|<ul><li>Jsou všichni hostitelé hello, kdy je úloha hello provádění nahoru a používá?</li><li>Byl hello úlohy nelze toostart?</li><li>Existují probíhající runtime upgrady?</li><li>Úloha hello očekávanému stavu, (například spuštění nebo zastavení zákazníkem)?</li><li>Hello úlohy zjistil limitu paměti výjimek?</li><li>Existují probíhající výpočetní naplánované aktualizace?</li><li>Správce spuštění (plán řízení) k dispozici je hello?</li></ul>|

## <a name="microsoftwebserverfarms"></a>Microsoft.web/serverFarms
|Spuštění kontroly|
|---|
|<ul><li>Hostitelský server hello i spuštěny?</li><li>Běží Internetová informační služba</li><li>Je spuštěn nástroj pro vyrovnávání zatížení hello?</li><li>Lze hello plán služby Web Service přejít z v rámci datového centra hello?</li><li>Je hello úložiště účet hostování hello lokality obsah pro serverovou farmu hello k dispozici??</li></ul>|

## <a name="microsoftwebsites"></a>Microsoft.Web/Sites
|Spuštění kontroly|
|---|
|<ul><li>Hostitelský server hello i spuštěny?</li><li>Je spuštěn server Internet Information server?</li><li>Je spuštěn nástroj pro vyrovnávání zatížení hello?</li><li>Lze hello webové aplikace přejít z v rámci datového centra hello?</li><li>Účet úložiště hello je hostitelem obsahu webu hello k dispozici?</li></ul>|

# <a name="next-steps"></a>Další kroky
-  V tématu [Úvod tooAzure stav služby](service-health-overview.md) a [Úvod tooAzure stav prostředku](resource-health-overview.md) toounderstand více o nich. 
-  [Nejčastější dotazy o stavu prostředků Azure](resource-health-faq.md)
- Nastavení výstrah, takže je znázorněna problémy v oblasti stavu. Další informace najdete v tématu [konfigurovat výstrahy pro stav služby](../monitoring-and-diagnostics/monitoring-activity-log-alerts-on-service-notifications.md). 