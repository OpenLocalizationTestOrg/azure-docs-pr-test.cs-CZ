---
title: "Azure metriky monitorování - podporované metriky na typ prostředku | Microsoft Docs"
description: "Seznam metriky, které jsou dostupné pro jednotlivé typy prostředků s Azure monitorování."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 63d4ac65-1688-40d1-85c8-7cd408285b0f
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/05/2017
ms.author: johnkem
ms.openlocfilehash: 4cd72c8193d66f164d9afa53af4b5203369b32dc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/03/2017
---
# <a name="supported-metrics-with-azure-monitor"></a>Podporované metriky s monitorováním Azure
Monitorování Azure poskytuje několik způsobů, jak pracovat s metriky, včetně grafů, je na portálu, k nim přistupovat pomocí rozhraní REST API nebo je dotazování pomocí prostředí PowerShell nebo rozhraní příkazového řádku. Níže je úplný seznam všech metriky aktuálně k dispozici s Azure monitorování metriky kanálu.

> [!NOTE]
> Další metriky může být k dispozici v portálu nebo pomocí starší verze rozhraní API. Tento seznam obsahuje pouze ve verzi public preview metriky, které jsou dostupné pomocí verzi public preview konsolidované monitorování Azure metriky kanálu.
>
>

## <a name="microsoftanalysisservicesservers"></a>Microsoft.AnalysisServices/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|qpu_metric|QPU|Počet|Průměr|QPU. Rozsah 0-100 S1, 0 – 200 S2 a 0 – 400 pro S4|
|memory_metric|Memory (Paměť)|Bajty|Průměr|Paměť. V rozsahu 0-25 GB pro S1, 0 – 50 GB pro S2 a 0 – 100 GB pro S4|
|TotalConnectionRequests|Žádosti o připojení (celkem)|Počet|Průměr|Požadavky na celkový počet připojení. Jedná se o doručení.|
|SuccessfullConnectionsPerSec|Úspěšné připojení za sekundu|CountPerSecond|Průměr|Počet dokončených úspěšné připojení.|
|TotalConnectionFailures|Chyby připojení (celkem)|Počet|Průměr|Celkový počet neúspěšných pokusů o připojení.|
|CurrentUserSessions|Aktuální relace uživatele|Počet|Průměr|Aktuální počet uživatelských relací navázat.|
|QueryPoolBusyThreads|Dotaz z fondu podprocesů zaneprázdněn|Počet|Průměr|Počet vytížených vláken ve fondu vláken dotazu.|
|CommandPoolJobQueueLength|Příkaz délku fronty úloh|Počet|Průměr|Počet úloh ve frontě příkaz fondu vláken.|
|ProcessingPoolJobQueueLength|Délka fronty úloh zpracování fondu|Počet|Průměr|Počet úloh jiný I/O ve frontě fondu zpracování vláken.|
|CurrentConnections|Připojení: Aktuální připojení|Počet|Průměr|Aktuální počet připojení klienta.|
|CleanerCurrentPrice|Paměť: Čisticí aktuální cena|Počet|Průměr|Aktuální cena paměti, $a bajtů/čas, normalizovány na 1000.|
|CleanerMemoryShrinkable|Paměť: Čisticí paměti vypočítat|Bajty|Průměr|Množství paměti v bajtech, podstoupí čisticí vyprazdňování pozadím.|
|CleanerMemoryNonshrinkable|Paměť: Čisticí nonshrinkable paměti|Bajty|Průměr|Množství paměti v bajtech, není v souladu čisticí vyprazdňování pozadím.|
|Parametru MemoryUsage|Paměti: Využití paměti|Bajty|Průměr|Využití paměti procesem serveru v rámci výpočet ceny čisticí paměti. Rovná se čítač Process\PrivateBytes plus velikost dat mapované paměti, ignoruje všechny paměti, které bylo namapované nebo přidělené modul xVelocity analýzy v paměti (VertiPaq) překračující modul xVelocity Limit paměti.|
|MemoryLimitHard|Paměti: Pevný Limit paměti|Bajty|Průměr|Omezení pevné paměti z konfiguračního souboru.|
|MemoryLimitHigh|Paměť: Omezení paměti vysoká|Bajty|Průměr|Limit velkého množství paměti, z konfiguračního souboru.|
|MemoryLimitLow|Paměti: Nízká Limit paměti|Bajty|Průměr|Limit nedostatek paměti z konfiguračního souboru.|
|MemoryLimitVertiPaq|Paměti: VertiPaq Limit paměti|Bajty|Průměr|Omezení v paměti z konfiguračního souboru.|
|Kvóta|Paměť: kvóty|Bajty|Průměr|Aktuální kvótu paměti, v bajtech. Kvótu paměti se také označuje jako rezervace paměti grant nebo paměti.|
|QuotaBlocked|Paměti: Blokované kvótu|Počet|Průměr|Aktuální počet požadavků kvóty, které jsou blokovaný, dokud jsou uvolněny kvóty další paměť.|
|VertiPaqNonpaged|Paměť: VertiPaq nestránkovaného fondu|Bajty|Průměr|Bajtů paměti uzamčena v pracovní sadě pro použití stroj v paměti.|
|VertiPaqPaged|Paměť: VertiPaq stránkovaného fondu|Bajty|Průměr|Bajty stránkovaného paměti používané pro data v paměti.|
|RowsReadPerSec|Zpracování: Řádky čtení za sekundu|CountPerSecond|Průměr|Počet řádků přečíst ze všech relačních databází.|
|RowsConvertedPerSec|Zpracování: Řádky převést za sekundu|CountPerSecond|Průměr|Počet řádků převést během zpracování.|
|RowsWrittenPerSec|Zpracování: Řádků zapsaných za sekundu|CountPerSecond|Průměr|Počet řádků zapsaných během zpracování.|
|CommandPoolBusyThreads|Vláken: Příkaz zaneprázdněn z fondu podprocesů|Počet|Průměr|Počet vytížených vláken ve fondu vláken příkaz.|
|CommandPoolIdleThreads|Vláken: Příkaz nečinných vláken fondu|Počet|Průměr|Počet nečinných vláken ve fondu vláken příkaz.|
|LongParsingBusyThreads|Vláken: Analýza zaneprázdněn vláken dlouho|Počet|Průměr|Počet vytížených vláken ve fondu vláken dlouho analýzy.|
|LongParsingIdleThreads|Vláken: Analýza nečinných vláken dlouho|Počet|Průměr|Počet nečinných vláken ve fondu vláken dlouho analýzy.|
|LongParsingJobQueueLength|Vláken: Analýza dlouho délka fronty úloh|Počet|Průměr|Počet úloh ve frontě dlouho analýzy fondu vláken.|
|ProcessingPoolBusyIOJobThreads|Vláken: Fond zaneprázdněn vstupně-výstupní úlohy vláken zpracování|Počet|Průměr|Počet vláken, které jsou spuštěné úlohy vstupně-výstupních operací ve fondu zpracování vláken.|
|ProcessingPoolBusyNonIOThreads|Vláken: Zaneprázdněný jiný I/O vláken fondu zpracování|Počet|Průměr|Počet vláken, které jsou spuštěné úlohy bez I/O ve fondu zpracování vláken.|
|ProcessingPoolIOJobQueueLength|Vláken: Fond délka fronty vstupně-výstupní úlohy zpracování|Počet|Průměr|Počet vstupně-výstupních úloh ve frontě fondu zpracování vláken.|
|ProcessingPoolIdleIOJobThreads|Vláken: Fond nečinnosti vstupně-výstupní úlohy vláken zpracování|Počet|Průměr|Počet vstupně-výstupních úloh ve fondu zpracování vláken nečinných vláken.|
|ProcessingPoolIdleNonIOThreads|Vláken: Nečinnosti vláken jiný I/O fondu zpracování|Počet|Průměr|Počet nečinných vláken ve fondu zpracování vláken, který je vyhrazený pro jiný I/O úlohy.|
|QueryPoolIdleThreads|Vláken: Dotaz nečinných vláken fondu|Počet|Průměr|Počet vstupně-výstupních úloh ve fondu zpracování vláken nečinných vláken.|
|QueryPoolJobQueueLength|Vláken: Dotaz lengt fronty úloh fondu|Počet|Průměr|Počet úloh ve frontě fondu vláken dotazu.|
|ShortParsingBusyThreads|Vláken: Analýza zaneprázdněn vláken krátké|Počet|Průměr|Počet vytížených vláken v krátké analýzy fondu vláken.|
|ShortParsingIdleThreads|Vláken: Analýza nečinných vláken krátké|Počet|Průměr|Počet nečinných vláken v krátké analýzy fondu vláken.|
|ShortParsingJobQueueLength|Vláken: Analýza délka fronty úloh krátké|Počet|Průměr|Počet úloh ve frontě krátké analýzy fondu vláken.|
|memory_thrashing_metric|Zahlcení paměti|Procento|Průměr|Průměrná paměti zahlcení.|

## <a name="microsoftapimanagementservice"></a>Microsoft.ApiManagement/service

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|TotalRequests|Celkový počet brány požadavků|Počet|Celkem|Počet požadavků na bránu|
|SuccessfulRequests|Požadavky pro úspěšné brány|Počet|Celkem|Počet požadavků úspěšná brány|
|UnauthorizedRequests|Požadavky neoprávněným brány|Počet|Celkem|Počet požadavků neoprávněným brány|
|FailedRequests|Požadavky na bránu|Počet|Celkem|Počet selhání v žádostech o brány|
|OtherRequests|Ostatní požadavky brány|Počet|Celkem|Počet požadavků na jiné brány|

## <a name="microsoftbatchbatchaccounts"></a>Microsoft.Batch/batchAccounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|CoreCount|Vyhrazené počet jader|Počet|Celkem|Celkový počet vyhrazených jader na účtu batch|
|TotalNodeCount|Počet vyhrazených uzlů|Počet|Celkem|Celkový počet vyhrazených uzlů na účtu batch|
|LowPriorityCoreCount|Počet jader LowPriority|Počet|Celkem|Celkový počet nízkou prioritu jader na účtu batch|
|TotalLowPriorityNodeCount|Počet uzlů s nízkou prioritou|Počet|Celkem|Celkový počet uzlů nízkou prioritu na účtu batch|
|CreatingNodeCount|Vytváření počet uzlů|Počet|Celkem|Počet uzlů, které vytváří|
|StartingNodeCount|Počáteční počet uzlů|Počet|Celkem|Počet uzlů spouštění|
|WaitingForStartTaskNodeCount|Čeká se na pro počet uzlů spuštění úloh|Počet|Celkem|Počet uzlů čekání na dokončení úlohy spustit|
|StartTaskFailedNodeCount|Spuštění úlohy se nezdařilo počet uzlů|Počet|Celkem|Počet uzlů, kde se nezdařilo spustit úlohu|
|IdleNodeCount|Počet nečinných uzlů|Počet|Celkem|Počet nečinných uzlů|
|OfflineNodeCount|Offline počet uzlů|Počet|Celkem|Počet offline uzlů|
|RebootingNodeCount|Restartování počet uzlů|Počet|Celkem|Počet restartování uzlů|
|ReimagingNodeCount|Obnovování počet uzlů|Počet|Celkem|Počet uzlů obnovování|
|RunningNodeCount|Spuštění počet uzlů|Počet|Celkem|Počet spuštěných uzly|
|LeavingPoolNodeCount|Opouštění počet uzlů fondu|Počet|Celkem|Počet uzlů a fondu|
|UnusableNodeCount|Nepoužitelná počet uzlů|Počet|Celkem|Počet nepoužitelná uzlů|
|PreemptedNodeCount|Zrušené počet uzlů|Počet|Celkem|Počet uzlů zrušené|
|TaskStartEvent|Úloha spuštění události|Počet|Celkem|Celkový počet úloh, které byly zahájeny|
|TaskCompleteEvent|Dokončení událostí úlohy|Počet|Celkem|Celkový počet úloh, které byly dokončeny|
|TaskFailEvent|Událostí selhání úlohy|Počet|Celkem|Celkový počet úloh, které byly dokončeny ve stavu selhání|
|PoolCreateEvent|Události vytváření fondu|Počet|Celkem|Celkový počet fondy, které byly vytvořeny|
|PoolResizeStartEvent|Události spuštění změny velikosti fondu|Počet|Celkem|Celkový počet změní velikost fondu, které byly zahájeny|
|PoolResizeCompleteEvent|Dokončení události změny velikosti fondu|Počet|Celkem|Celkový počet změní velikost fondu, které byly dokončeny|
|PoolDeleteStartEvent|Události spuštění odstranění fondu|Počet|Celkem|Celkový počet odstranění fondu, které byly zahájeny|
|PoolDeleteCompleteEvent|Události dokončení odstranění fondu|Počet|Celkem|Celkový počet odstranění fondu, které byly dokončeny|

## <a name="microsoftcacheredis"></a>Microsoft.Cache/redis

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|connectedclients|Připojení klienti|Počet|Maximální počet||
|totalcommandsprocessed|Celkový počet operací|Počet|Celkem||
|mezipaměti|Přístupů k mezipaměti|Počet|Celkem||
|cachemisses|Neúspěšné přístupy do mezipaměti|Počet|Celkem||
|getcommands|Získá|Počet|Celkem||
|setcommands|Nastaví|Počet|Celkem||
|evictedkeys|Vyřazené klíče|Počet|Celkem||
|totalkeys|Celkový počet klíčů|Počet|Maximální počet||
|expiredkeys|Vypršela platnost klíče|Počet|Celkem||
|usedmemory|Použitá paměť|Bajty|Maximální počet||
|usedmemoryRss|Použitá paměť RSS|Bajty|Maximální počet||
|serverLoad|Zatížení serveru|Procento|Maximální počet||
|cacheWrite|Mezipaměť zápisu|BytesPerSecond|Maximální počet||
|cacheRead|Mezipaměti pro čtení|BytesPerSecond|Maximální počet||
|percentProcessorTime|Procesor|Procento|Maximální počet||
|connectedclients0|Připojených klientů (horizontálního oddílu 0)|Počet|Maximální počet||
|totalcommandsprocessed0|Celkový počet operací (horizontálního oddílu 0)|Počet|Celkem||
|cachehits0|Přístupů k mezipaměti (horizontálního oddílu 0)|Počet|Celkem||
|cachemisses0|Neúspěšné přístupy do mezipaměti (horizontálního oddílu 0)|Počet|Celkem||
|getcommands0|Získá (horizontálního oddílu 0)|Počet|Celkem||
|setcommands0|Nastaví (horizontálního oddílu 0)|Počet|Celkem||
|evictedkeys0|Vyřazené klíče (horizontálního oddílu 0)|Počet|Celkem||
|totalkeys0|Celkový počet klíčů (horizontálního oddílu 0)|Počet|Maximální počet||
|expiredkeys0|Vypršela platnost klíče (horizontálního oddílu 0)|Počet|Celkem||
|usedmemory0|Použitá paměť (horizontálního oddílu 0)|Bajty|Maximální počet||
|usedmemoryRss0|Použitá paměť RSS (horizontálního oddílu 0)|Bajty|Maximální počet||
|serverLoad0|Zatížení serveru (horizontálního oddílu 0)|Procento|Maximální počet||
|cacheWrite0|Mezipaměť zápisu (horizontálního oddílu 0)|BytesPerSecond|Maximální počet||
|cacheRead0|Mezipaměti pro čtení (horizontálního oddílu 0)|BytesPerSecond|Maximální počet||
|percentProcessorTime0|Procesor (horizontálního oddílu 0)|Procento|Maximální počet||
|connectedclients1|Připojených klientů (horizontálních 1)|Počet|Maximální počet||
|totalcommandsprocessed1|Celkový počet operací (horizontálních 1)|Počet|Celkem||
|cachehits1|Přístupů k mezipaměti (horizontálních 1)|Počet|Celkem||
|cachemisses1|Neúspěšné přístupy do mezipaměti (horizontálních 1)|Počet|Celkem||
|getcommands1|Získá (horizontálních 1)|Počet|Celkem||
|setcommands1|Nastaví (horizontálních 1)|Počet|Celkem||
|evictedkeys1|Vyřazené klíče (horizontálních 1)|Počet|Celkem||
|totalkeys1|Celkový počet klíčů (horizontálních 1)|Počet|Maximální počet||
|expiredkeys1|Vypršela platnost klíče (horizontálních 1)|Počet|Celkem||
|usedmemory1|Použitá paměť (horizontálních 1)|Bajty|Maximální počet||
|usedmemoryRss1|Použitá paměť RSS (horizontálních 1)|Bajty|Maximální počet||
|serverLoad1|Zatížení serveru (horizontálních 1)|Procento|Maximální počet||
|cacheWrite1|Mezipaměť zápisu (horizontálních 1)|BytesPerSecond|Maximální počet||
|cacheRead1|Mezipaměti pro čtení (horizontálních 1)|BytesPerSecond|Maximální počet||
|percentProcessorTime1|Procesor (horizontálních 1)|Procento|Maximální počet||
|connectedclients2|Připojených klientů (horizontálních 2)|Počet|Maximální počet||
|totalcommandsprocessed2|Celkový počet operací (horizontálních 2)|Počet|Celkem||
|cachehits2|Přístupů k mezipaměti (horizontálních 2)|Počet|Celkem||
|cachemisses2|Neúspěšné přístupy do mezipaměti (horizontálních 2)|Počet|Celkem||
|getcommands2|Získá (horizontálních 2)|Počet|Celkem||
|setcommands2|Nastaví (horizontálních 2)|Počet|Celkem||
|evictedkeys2|Vyřazené klíče (horizontálních 2)|Počet|Celkem||
|totalkeys2|Celkový počet klíčů (horizontálních 2)|Počet|Maximální počet||
|expiredkeys2|Vypršela platnost klíče (horizontálních 2)|Počet|Celkem||
|usedmemory2|Použitá paměť (horizontálních 2)|Bajty|Maximální počet||
|usedmemoryRss2|Použitá paměť RSS (horizontálních 2)|Bajty|Maximální počet||
|serverLoad2|Zatížení serveru (horizontálních 2)|Procento|Maximální počet||
|cacheWrite2|Mezipaměť zápisu (horizontálních 2)|BytesPerSecond|Maximální počet||
|cacheRead2|Mezipaměti pro čtení (horizontálních 2)|BytesPerSecond|Maximální počet||
|percentProcessorTime2|Procesor (horizontálních 2)|Procento|Maximální počet||
|connectedclients3|Připojených klientů (horizontálních 3)|Počet|Maximální počet||
|totalcommandsprocessed3|Celkový počet operací (horizontálních 3)|Počet|Celkem||
|cachehits3|Přístupů k mezipaměti (horizontálních 3)|Počet|Celkem||
|cachemisses3|Neúspěšné přístupy do mezipaměti (horizontálních 3)|Počet|Celkem||
|getcommands3|Získá (horizontálních 3)|Počet|Celkem||
|setcommands3|Nastaví (horizontálních 3)|Počet|Celkem||
|evictedkeys3|Vyřazené klíče (horizontálních 3)|Počet|Celkem||
|totalkeys3|Celkový počet klíčů (horizontálních 3)|Počet|Maximální počet||
|expiredkeys3|Vypršela platnost klíče (horizontálních 3)|Počet|Celkem||
|usedmemory3|Použitá paměť (horizontálních 3)|Bajty|Maximální počet||
|usedmemoryRss3|Použitá paměť RSS (horizontálních 3)|Bajty|Maximální počet||
|serverLoad3|Zatížení serveru (horizontálních 3)|Procento|Maximální počet||
|cacheWrite3|Mezipaměť zápisu (horizontálních 3)|BytesPerSecond|Maximální počet||
|cacheRead3|Mezipaměti pro čtení (horizontálních 3)|BytesPerSecond|Maximální počet||
|percentProcessorTime3|Procesor (horizontálních 3)|Procento|Maximální počet||
|connectedclients4|Připojených klientů (horizontálních 4)|Počet|Maximální počet||
|totalcommandsprocessed4|Celkový počet operací (horizontálních 4)|Počet|Celkem||
|cachehits4|Přístupů k mezipaměti (horizontálních 4)|Počet|Celkem||
|cachemisses4|Neúspěšné přístupy do mezipaměti (horizontálních 4)|Počet|Celkem||
|getcommands4|Získá (horizontálních 4)|Počet|Celkem||
|setcommands4|Nastaví (horizontálních 4)|Počet|Celkem||
|evictedkeys4|Vyřazené klíče (horizontálních 4)|Počet|Celkem||
|totalkeys4|Celkový počet klíčů (horizontálních 4)|Počet|Maximální počet||
|expiredkeys4|Vypršela platnost klíče (horizontálních 4)|Počet|Celkem||
|usedmemory4|Použitá paměť (horizontálních 4)|Bajty|Maximální počet||
|usedmemoryRss4|Použitá paměť RSS (horizontálních 4)|Bajty|Maximální počet||
|serverLoad4|Zatížení serveru (horizontálních 4)|Procento|Maximální počet||
|cacheWrite4|Mezipaměť zápisu (horizontálních 4)|BytesPerSecond|Maximální počet||
|cacheRead4|Mezipaměti pro čtení (horizontálních 4)|BytesPerSecond|Maximální počet||
|percentProcessorTime4|Procesor (horizontálních 4)|Procento|Maximální počet||
|connectedclients5|Připojených klientů (horizontálních 5)|Počet|Maximální počet||
|totalcommandsprocessed5|Celkový počet operací (horizontálních 5)|Počet|Celkem||
|cachehits5|Přístupů k mezipaměti (horizontálních 5)|Počet|Celkem||
|cachemisses5|Neúspěšné přístupy do mezipaměti (horizontálních 5)|Počet|Celkem||
|getcommands5|Získá (horizontálních 5)|Počet|Celkem||
|setcommands5|Nastaví (horizontálních 5)|Počet|Celkem||
|evictedkeys5|Vyřazené klíče (horizontálních 5)|Počet|Celkem||
|totalkeys5|Celkový počet klíčů (horizontálních 5)|Počet|Maximální počet||
|expiredkeys5|Vypršela platnost klíče (horizontálních 5)|Počet|Celkem||
|usedmemory5|Použitá paměť (horizontálních 5)|Bajty|Maximální počet||
|usedmemoryRss5|Použitá paměť RSS (horizontálních 5)|Bajty|Maximální počet||
|serverLoad5|Zatížení serveru (horizontálních 5)|Procento|Maximální počet||
|cacheWrite5|Mezipaměť zápisu (horizontálních 5)|BytesPerSecond|Maximální počet||
|cacheRead5|Mezipaměti pro čtení (horizontálních 5)|BytesPerSecond|Maximální počet||
|percentProcessorTime5|Procesor (horizontálních 5)|Procento|Maximální počet||
|connectedclients6|Připojených klientů (horizontálních 6)|Počet|Maximální počet||
|totalcommandsprocessed6|Celkový počet operací (horizontálních 6)|Počet|Celkem||
|cachehits6|Přístupů k mezipaměti (horizontálních 6)|Počet|Celkem||
|cachemisses6|Neúspěšné přístupy do mezipaměti (horizontálních 6)|Počet|Celkem||
|getcommands6|Získá (horizontálních 6)|Počet|Celkem||
|setcommands6|Nastaví (horizontálních 6)|Počet|Celkem||
|evictedkeys6|Vyřazené klíče (horizontálních 6)|Počet|Celkem||
|totalkeys6|Celkový počet klíčů (horizontálních 6)|Počet|Maximální počet||
|expiredkeys6|Vypršela platnost klíče (horizontálních 6)|Počet|Celkem||
|usedmemory6|Použitá paměť (horizontálních 6)|Bajty|Maximální počet||
|usedmemoryRss6|Použitá paměť RSS (horizontálních 6)|Bajty|Maximální počet||
|serverLoad6|Zatížení serveru (horizontálních 6)|Procento|Maximální počet||
|cacheWrite6|Mezipaměť zápisu (horizontálních 6)|BytesPerSecond|Maximální počet||
|cacheRead6|Mezipaměti pro čtení (horizontálních 6)|BytesPerSecond|Maximální počet||
|percentProcessorTime6|Procesor (horizontálních 6)|Procento|Maximální počet||
|connectedclients7|Připojených klientů (horizontálních 7)|Počet|Maximální počet||
|totalcommandsprocessed7|Celkový počet operací (horizontálních 7)|Počet|Celkem||
|cachehits7|Přístupů k mezipaměti (horizontálních 7)|Počet|Celkem||
|cachemisses7|Neúspěšné přístupy do mezipaměti (horizontálních 7)|Počet|Celkem||
|getcommands7|Získá (horizontálních 7)|Počet|Celkem||
|setcommands7|Nastaví (horizontálních 7)|Počet|Celkem||
|evictedkeys7|Vyřazené klíče (horizontálních 7)|Počet|Celkem||
|totalkeys7|Celkový počet klíčů (horizontálních 7)|Počet|Maximální počet||
|expiredkeys7|Vypršela platnost klíče (horizontálních 7)|Počet|Celkem||
|usedmemory7|Použitá paměť (horizontálních 7)|Bajty|Maximální počet||
|usedmemoryRss7|Použitá paměť RSS (horizontálních 7)|Bajty|Maximální počet||
|serverLoad7|Zatížení serveru (horizontálních 7)|Procento|Maximální počet||
|cacheWrite7|Mezipaměť zápisu (horizontálních 7)|BytesPerSecond|Maximální počet||
|cacheRead7|Mezipaměti pro čtení (horizontálních 7)|BytesPerSecond|Maximální počet||
|percentProcessorTime7|Procesor (horizontálních 7)|Procento|Maximální počet||
|connectedclients8|Připojených klientů (horizontálních 8)|Počet|Maximální počet||
|totalcommandsprocessed8|Celkový počet operací (horizontálních 8)|Počet|Celkem||
|cachehits8|Přístupů k mezipaměti (horizontálních 8)|Počet|Celkem||
|cachemisses8|Neúspěšné přístupy do mezipaměti (horizontálních 8)|Počet|Celkem||
|getcommands8|Získá (horizontálních 8)|Počet|Celkem||
|setcommands8|Nastaví (horizontálních 8)|Počet|Celkem||
|evictedkeys8|Vyřazené klíče (horizontálních 8)|Počet|Celkem||
|totalkeys8|Celkový počet klíčů (horizontálních 8)|Počet|Maximální počet||
|expiredkeys8|Vypršela platnost klíče (horizontálních 8)|Počet|Celkem||
|usedmemory8|Použitá paměť (horizontálních 8)|Bajty|Maximální počet||
|usedmemoryRss8|Použitá paměť RSS (horizontálních 8)|Bajty|Maximální počet||
|serverLoad8|Zatížení serveru (horizontálních 8)|Procento|Maximální počet||
|cacheWrite8|Mezipaměť zápisu (horizontálních 8)|BytesPerSecond|Maximální počet||
|cacheRead8|Mezipaměti pro čtení (horizontálních 8)|BytesPerSecond|Maximální počet||
|percentProcessorTime8|Procesor (horizontálních 8)|Procento|Maximální počet||
|connectedclients9|Připojených klientů (horizontálních 9)|Počet|Maximální počet||
|totalcommandsprocessed9|Celkový počet operací (horizontálních 9)|Počet|Celkem||
|cachehits9|Přístupů k mezipaměti (horizontálních 9)|Počet|Celkem||
|cachemisses9|Neúspěšné přístupy do mezipaměti (horizontálních 9)|Počet|Celkem||
|getcommands9|Získá (horizontálních 9)|Počet|Celkem||
|setcommands9|Nastaví (horizontálních 9)|Počet|Celkem||
|evictedkeys9|Vyřazené klíče (horizontálních 9)|Počet|Celkem||
|totalkeys9|Celkový počet klíčů (horizontálních 9)|Počet|Maximální počet||
|expiredkeys9|Vypršela platnost klíče (horizontálních 9)|Počet|Celkem||
|usedmemory9|Použitá paměť (horizontálních 9)|Bajty|Maximální počet||
|usedmemoryRss9|Použitá paměť RSS (horizontálních 9)|Bajty|Maximální počet||
|serverLoad9|Zatížení serveru (horizontálních 9)|Procento|Maximální počet||
|cacheWrite9|Mezipaměť zápisu (horizontálních 9)|BytesPerSecond|Maximální počet||
|cacheRead9|Mezipaměti pro čtení (horizontálních 9)|BytesPerSecond|Maximální počet||
|percentProcessorTime9|Procesor (horizontálních 9)|Procento|Maximální počet||

## <a name="microsoftcognitiveservicesaccounts"></a>Microsoft.CognitiveServices/accounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|TotalCalls|Celkový počet volání|Počet|Celkem|Celkový počet volání.|
|SuccessfulCalls|Úspěšné volání|Počet|Celkem|Počet úspěšných volání.|
|TotalErrors|Celkový počet chyb|Počet|Celkem|Celkový počet volání s odpovědi na chybu (4xx kód odpovědi HTTP nebo 5xx).|
|BlockedCalls|Blokované volání|Počet|Celkem|Počet volání dané překročil míry nebo limit kvóty.|
|ServerErrors|Chyby serveru|Počet|Celkem|Počet volání s vnitřní chybou služby (5xx kód odpovědi HTTP).|
|ClientErrors|Chyby klienta|Počet|Celkem|Počet volání s chyba na straně klienta (4xx kód odpovědi HTTP).|
|DataIn|Data v|Bajty|Celkem|Velikost příchozích dat v bajtech.|
|DataOut|Data|Bajty|Celkem|Velikost odesílaných dat v bajtech.|
|Latence|Latence|Počet milisekund|Průměr|Latence v milisekundách.|

## <a name="microsoftcomputevirtualmachines"></a>Microsoft.Compute/virtualMachines

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|Procento využití procesoru|Procento využití procesoru|Procento|Průměr|Procento přidělené výpočetní jednotky, které jsou aktuálně používán virtuálním počítačům|
|Sítě v|Sítě v|Bajty|Celkem|Počet bajtů přijatých virtuální počítače (příchozí provoz) na všech síťových rozhraních|
|Sítě Out|Sítě Out|Bajty|Celkem|Počet bajtů odhlašování na všech síťových rozhraní pomocí virtuální počítače (odchozí provoz)|
|Bajty čtení disku|Bajty čtení disku|Bajty|Celkem|Celkový počet bajtů přečtených z disku během období sledování|
|Bajty zápisu disku|Bajty zápisu disku|Bajty|Celkem|Celkový počet bajtů zapsaných na disk během období sledování|
|Čtení z disku operace/s|Čtení z disku operace/s|CountPerSecond|Průměr|Čtení disku IOPS|
|Operace zápisu disku/s|Operace zápisu disku/s|CountPerSecond|Průměr|Zápis disku IOPS|

## <a name="microsoftcomputevirtualmachinescalesets"></a>Microsoft.Compute/virtualMachineScaleSets

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|Procento využití procesoru|Procento využití procesoru|Procento|Průměr|Procento přidělené výpočetní jednotky, které jsou aktuálně používán virtuálním počítačům|
|Sítě v|Sítě v|Bajty|Celkem|Počet bajtů přijatých virtuální počítače (příchozí provoz) na všech síťových rozhraních|
|Sítě Out|Sítě Out|Bajty|Celkem|Počet bajtů odhlašování na všech síťových rozhraní pomocí virtuální počítače (odchozí provoz)|
|Bajty čtení disku|Bajty čtení disku|Bajty|Celkem|Celkový počet bajtů přečtených z disku během období sledování|
|Bajty zápisu disku|Bajty zápisu disku|Bajty|Celkem|Celkový počet bajtů zapsaných na disk během období sledování|
|Čtení z disku operace/s|Čtení z disku operace/s|CountPerSecond|Průměr|Čtení disku IOPS|
|Operace zápisu disku/s|Operace zápisu disku/s|CountPerSecond|Průměr|Zápis disku IOPS|

## <a name="microsoftcomputevirtualmachinescalesetsvirtualmachines"></a>Microsoft.Compute/virtualMachineScaleSets/virtualMachines

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|Procento využití procesoru|Procento využití procesoru|Procento|Průměr|Procento přidělené výpočetní jednotky, které jsou aktuálně používán virtuálním počítačům|
|Sítě v|Sítě v|Bajty|Celkem|Počet bajtů přijatých virtuální počítače (příchozí provoz) na všech síťových rozhraních|
|Sítě Out|Sítě Out|Bajty|Celkem|Počet bajtů odhlašování na všech síťových rozhraní pomocí virtuální počítače (odchozí provoz)|
|Bajty čtení disku|Bajty čtení disku|Bajty|Celkem|Celkový počet bajtů přečtených z disku během období sledování|
|Bajty zápisu disku|Bajty zápisu disku|Bajty|Celkem|Celkový počet bajtů zapsaných na disk během období sledování|
|Čtení z disku operace/s|Čtení z disku operace/s|CountPerSecond|Průměr|Čtení disku IOPS|
|Operace zápisu disku/s|Operace zápisu disku/s|CountPerSecond|Průměr|Zápis disku IOPS|

## <a name="microsoftcustomerinsightshubs"></a>Microsoft.CustomerInsights/hubs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|DCIApiCalls|Přehled rozhraní API volání zákazníka|Počet|Celkem||
|DCIMappingImportOperationSuccessfulLines|Mapování importu operace úspěšné řádky|Počet|Celkem||
|DCIMappingImportOperationFailedLines|Mapování importu se nezdařila řádky|Počet|Celkem||
|DCIMappingImportOperationTotalLines|Mapování importu operaci celkový počet řádků|Počet|Celkem||
|DCIMappingImportOperationRuntimeInSeconds|Mapování importu operaci za běhu v sekundách|Sekundy|Celkem||
|DCIOutboundProfileExportSucceeded|Odchozí profilu exportu byla úspěšná|Počet|Celkem||
|DCIOutboundProfileExportFailed|Odchozí profilu exportu se nezdařila|Počet|Celkem||
|DCIOutboundProfileExportDuration|Doba trvání odchozí profilu exportu|Sekundy|Celkem||
|DCIOutboundKpiExportSucceeded|Odchozí klíčového ukazatele výkonu Export byl úspěšný|Počet|Celkem||
|DCIOutboundKpiExportFailed|Odchozí klíčového ukazatele výkonu Export se nezdařil|Počet|Celkem||
|DCIOutboundKpiExportDuration|Doba trvání odchozí Export klíčového ukazatele výkonu|Sekundy|Celkem||
|DCIOutboundKpiExportStarted|Export odchozí klíčový ukazatel výkonu spustit|Sekundy|Celkem||
|DCIOutboundKpiRecordCount|Počet záznamů odchozí klíčového ukazatele výkonu|Sekundy|Celkem||
|DCIOutboundProfileExportCount|Počet Export odchozí profilů|Sekundy|Celkem||
|DCIOutboundInitialProfileExportFailed|Odchozí počáteční profilu exportu se nezdařila|Sekundy|Celkem||
|DCIOutboundInitialProfileExportSucceeded|Odchozí počáteční profilu exportu byla úspěšná|Sekundy|Celkem||
|DCIOutboundInitialKpiExportFailed|Odchozí počáteční klíčového ukazatele výkonu Export se nezdařil|Sekundy|Celkem||
|DCIOutboundInitialKpiExportSucceeded|Odchozí počáteční klíčového ukazatele výkonu Export byl úspěšný|Sekundy|Celkem||
|DCIOutboundInitialProfileExportDurationInSeconds|Odchozí počáteční profilu exportu doby v sekundách|Sekundy|Celkem||
|AdlaJobForStandardKpiFailed|Úloha Adla pro standardní klíčový ukazatel výkonu se nezdařilo s|Sekundy|Celkem||
|AdlaJobForStandardKpiTimeOut|Úloha Adla pro standardní klíčový ukazatel výkonu časový limit v sekundách|Sekundy|Celkem||
|AdlaJobForStandardKpiCompleted|Úloha Adla pro standardní klíčový ukazatel výkonu dokončit v sekundách|Sekundy|Celkem||
|ImportASAValuesFailed|Hodnoty ASA importu se nezdařila, počet|Počet|Celkem||
|ImportASAValuesSucceeded|Import ASA hodnoty počtu bylo úspěšně dokončeno.|Počet|Celkem||

## <a name="microsoftdatalakeanalyticsaccounts"></a>Microsoft.DataLakeAnalytics/accounts

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|JobEndedSuccess|Úspěšné úlohy|Počet|Celkem|Počet neúspěšných úloh.|
|JobEndedFailure|Neúspěšné úlohy|Počet|Celkem|Počet nezdařených úloh.|
|JobEndedCancelled|Zrušené úlohy|Počet|Celkem|Počet zrušených úloh.|
|JobAUEndedSuccess|Úspěšné AU čas|Sekundy|Celkem|Celkový čas AU pro úspěšné úlohy.|
|JobAUEndedFailure|Neúspěšné AU čas|Sekundy|Celkem|Celkový čas AU pro neúspěšné úlohy.|
|JobAUEndedCancelled|Zrušené AU čas|Sekundy|Celkem|Celkový čas AU pro zrušené úlohy.|

## <a name="microsoftdbformysqlservers"></a>Microsoft.DBforMySQL/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|cpu_percent|Procento využití procesoru|Procento|Průměr|Procento využití procesoru|
|compute_limit|Výpočetní jednotka limit|Počet|Průměr|Výpočetní jednotka limit|
|compute_consumption_percent|Výpočetní jednotka procento|Procento|Průměr|Výpočetní jednotka procento|
|memory_percent|Paměť v procentech|Procento|Průměr|Paměť v procentech|
|io_consumption_percent|Vstupně-výstupní operace v procentech|Procento|Průměr|Vstupně-výstupní operace v procentech|
|storage_percent|Procento úložiště|Procento|Průměr|Procento úložiště|
|storage_used|Využité úložiště|Bajty|Průměr|Využité úložiště|
|storage_limit|Limit úložiště|Bajty|Průměr|Limit úložiště|
|active_connections|Celkový počet aktivních připojení|Počet|Průměr|Celkový počet aktivních připojení|
|connections_failed|Celkový počet selhání připojení|Počet|Průměr|Celkový počet selhání připojení|

## <a name="microsoftdbforpostgresqlservers"></a>Microsoft.DBforPostgreSQL/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|cpu_percent|Procento využití procesoru|Procento|Průměr|Procento využití procesoru|
|compute_limit|Výpočetní jednotka limit|Počet|Průměr|Výpočetní jednotka limit|
|compute_consumption_percent|Výpočetní jednotka procento|Procento|Průměr|Výpočetní jednotka procento|
|memory_percent|Paměť v procentech|Procento|Průměr|Paměť v procentech|
|io_consumption_percent|Vstupně-výstupní operace v procentech|Procento|Průměr|Vstupně-výstupní operace v procentech|
|storage_percent|Procento úložiště|Procento|Průměr|Procento úložiště|
|storage_used|Využité úložiště|Bajty|Průměr|Využité úložiště|
|storage_limit|Limit úložiště|Bajty|Průměr|Limit úložiště|
|active_connections|Celkový počet aktivních připojení|Počet|Průměr|Celkový počet aktivních připojení|
|connections_failed|Celkový počet selhání připojení|Počet|Průměr|Celkový počet selhání připojení|

## <a name="microsoftdevicesiothubs"></a>Microsoft.Devices/IotHubs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|d2c.telemetry.ingress.allProtocol|Pokusy o odeslání zprávy telemetrie|Počet|Celkem|Počet zařízení cloud telemetrické zprávy se pokusil odeslat do služby IoT hub|
|d2c.telemetry.ingress.Success|Telemetrické zprávy odeslané|Počet|Celkem|Počet zařízení cloud telemetrické zprávy úspěšně odeslán do služby IoT hub|
|c2d.Commands.egress.COMPLETE.Success|Příkazy byla dokončena|Počet|Celkem|Počet příkazů typu cloud zařízení úspěšně dokončila, a zařízení|
|c2d.Commands.egress.Abandon.Success|Příkazy opuštění|Počet|Celkem|Počet příkazů typu cloud zařízení opuštěny v rámci zařízení|
|c2d.Commands.egress.Reject.Success|Příkazy odmítnut|Počet|Celkem|Počet zařízení byl odmítnut příkazů typu cloud zařízení|
|devices.totalDevices|Celkový počet zařízení|Počet|Celkem|Počet zařízení zaregistrovat do služby IoT hub|
|devices.connectedDevices.allProtocol|Připojená zařízení|Počet|Celkem|Počet zařízení připojených ke službě IoT hub|
|d2c.telemetry.egress.Success|Telemetrické zprávy doručit|Počet|Celkem|Počet zpráv byla úspěšně zapsána do koncových bodů (celkem)|
|d2c.telemetry.egress.dropped|Vyřazené zprávy|Počet|Celkem|Počet zpráv vyřadit, protože koncový bod doručení byla neaktivní|
|d2c.telemetry.egress.orphaned|Osamocené zprávy|Počet|Celkem|Počet zpráv, na které se neshodují žádné cesty, včetně záložní trasy|
|d2c.telemetry.egress.invalid|Neplatné zprávy|Počet|Celkem|Počet zpráv, které nejsou doručeny z důvodu nekompatibility s nástrojem koncový bod|
|d2c.telemetry.egress.Fallback|Zprávy odpovídající záložního stavu|Počet|Celkem|Počet zpráv zapsaných do záložního koncového bodu|
|d2c.endpoints.egress.eventHubs|Zpráv doručených do koncové body centra událostí|Počet|Celkem|Počet zpráv byla úspěšně zapsána do koncové body centra událostí|
|d2c.endpoints.latency.eventHubs|Latence zprávy pro koncové body centra událostí|počet milisekund|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příchozí zprávy do koncového bodu centra událostí, v milisekundách|
|d2c.endpoints.egress.serviceBusQueues|Doručování zpráv do fronty Service Bus koncové body|Počet|Celkem|Počet zpráv byla úspěšně zapsána do koncových bodů frontou Service Bus|
|d2c.endpoints.latency.serviceBusQueues|Latence zprávy pro koncové body frontou Service Bus|počet milisekund|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příjem příchozích dat zpráv do fronty Service Bus koncový bod, v milisekundách|
|d2c.endpoints.egress.serviceBusTopics|Doručování zpráv do tématu Service Bus koncové body|Počet|Celkem|Počet zpráv byly úspěšně zapsána do tématu Service Bus koncové body|
|d2c.endpoints.latency.serviceBusTopics|Latence zprávy pro koncové body témata sběrnice|počet milisekund|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příchozí zprávy do tématu Service Bus koncový bod, v milisekundách|
|d2c.endpoints.egress.builtIn.events|Zpráv doručených do vestavěným koncovým bodem (zprávy nebo události)|Počet|Celkem|Počet zpráv byla úspěšně zapsána do vestavěným koncovým bodem (zprávy nebo události)|
|d2c.endpoints.latency.builtIn.events|Zpráva latence pro předdefinovaný koncový bod (zprávy nebo události)|počet milisekund|Průměr|Průměrná latence mezi příchozí zprávy do služby IoT hub a příjem příchozích dat zpráva do vestavěným koncovým bodem (zprávy událostí), v milisekundách |
|d2c.Twin.Read.Success|Úspěšné twin čte ze zařízení|Počet|Celkem|Počet čtení všechny úspěšné twin spouštěná zařízení.|
|d2c.Twin.Read.failure|Čtení twin ze zařízení se nezdařila|Počet|Celkem|Počet všech nepodařilo spouštěná zařízení twin čtení.|
|d2c.Twin.Read.Size|Velikost odpovědi twin čtení ze zařízení|Bajty|Průměr|Průměr, min a max všechny úspěšné, spouštěná zařízení twin čtení.|
|d2c.Twin.Update.Success|Aktualizace úspěšná twin ze zařízení|Počet|Celkem|Počet aktualizací všechny úspěšné twin spouštěná zařízení.|
|d2c.Twin.Update.failure|Aktualizace twin ze zařízení se nezdařila|Počet|Celkem|Počet všech se nezdařila aktualizace twin spouštěná zařízení.|
|d2c.Twin.Update.Size|Velikost twin aktualizace ze zařízení|Bajty|Průměr|Průměr, minimální a maximální velikost všech úspěšné, spouštěná zařízení twin aktualizace.|
|c2d.Methods.Success|Volání úspěšné přímá metoda|Počet|Celkem|Počet všech úspěšných volání přímá metoda.|
|c2d.Methods.failure|Přímá metoda volání se nezdařilo|Počet|Celkem|Počet všech se nezdařila, volání metod direct.|
|c2d.Methods.requestSize|Žádost o velikosti volání přímá metoda|Bajty|Průměr|Průměr, min a max všechny úspěšné požadavky přímá metoda.|
|c2d.Methods.responseSize|Velikost odpovědi volání přímá metoda|Bajty|Průměr|Průměr, min a max všechny úspěšné odpovědi přímá metoda.|
|c2d.Twin.Read.Success|Úspěšné twin čte z back-end|Počet|Celkem|Počet všech úspěšně spustil back-end twin čtení.|
|c2d.Twin.Read.failure|Neúspěšné twin čte z back-end|Počet|Celkem|Počet všech se nezdařilo čtení twin iniciované back-end.|
|c2d.Twin.Read.Size|Velikost odpovědi twin čtení z back-end|Bajty|Průměr|Průměr, min a max všechny úspěšné, iniciované back-end twin čtení.|
|c2d.Twin.Update.Success|Aktualizace úspěšná twin z back-end|Počet|Celkem|Počet aktualizací všechny úspěšné twin iniciované back-end.|
|c2d.Twin.Update.failure|Aktualizace se nezdařila twin z back-end|Počet|Celkem|Počet všech se nezdařila aktualizace twin iniciované back-end.|
|c2d.Twin.Update.Size|Velikost twin aktualizace z back-end|Bajty|Průměr|Průměr, minimální a maximální velikost všech úspěšné, iniciované back-end twin aktualizace.|
|twinQueries.success|Úspěšné twin dotazy|Počet|Celkem|Počet všech úspěšné twin dotazů.|
|twinQueries.failure|Neúspěšné twin dotazy|Počet|Celkem|Počet všechny neúspěšné twin dotazy.|
|twinQueries.resultSize|Objem výsledků dotazů Twin|Bajty|Průměr|Průměr, minimální a maximální velikosti výsledek všechny úspěšné twin dotazy.|
|jobs.createTwinUpdateJob.success|Úspěšné vytvoření úloh aktualizace twin|Počet|Celkem|Počet všech úspěšném vytvoření twin aktualizace úloh.|
|jobs.createTwinUpdateJob.failure|Neúspěšné vytvoření úloh aktualizace twin|Počet|Celkem|Počet všech se nezdařilo vytvoření twin aktualizace úlohy.|
|jobs.createDirectMethodJob.success|Úspěšné vytvoření úloh volání – metoda|Počet|Celkem|Počet všech úspěšném vytvoření přímá metoda volání úloh.|
|jobs.createDirectMethodJob.failure|Neúspěšné vytvoření úloh volání – metoda|Počet|Celkem|Počet všech se nezdařilo vytvoření přímá metoda vyvolání úlohy.|
|jobs.listJobs.success|Úspěšné volání do seznamu úloh|Počet|Celkem|Počet všech úspěšných volání do seznamu úloh.|
|jobs.listJobs.failure|Volání do seznamu úloh se nezdařilo|Počet|Celkem|Počet všech selhání volání do seznamu úloh.|
|jobs.cancelJob.success|Zrušení úloh úspěšné|Počet|Celkem|Počet všech úspěšných volání zrušení úlohy.|
|jobs.cancelJob.failure|Informace o neúspěšné úloze zrušení|Počet|Celkem|Počet všech volání se nezdařilo zrušit úlohu.|
|jobs.queryJobs.success|Až se úloha úspěšně dotazy|Počet|Celkem|Počet všech úspěšných volání do úlohy dotaz.|
|jobs.queryJobs.failure|Úloha se dotazuje se nezdařilo|Počet|Celkem|Počet všech selhání volání do úlohy dotaz.|
|jobs.completed|Dokončené úlohy|Počet|Celkem|Počet všech dokončené úlohy.|
|jobs.Failed|Neúspěšné úlohy|Počet|Celkem|Počet všechny neúspěšné úlohy.|
|d2c.telemetry.ingress.sendThrottle|Počet omezení chyb|Počet|Celkem|Omezí počet omezení chyb z důvodu propustnost zařízení generovaný|
|dailyMessageQuotaUsed|Celkový počet zpráv, které používá|Počet|Průměr|Počet celkový počet zpráv v současné době používá|

## <a name="microsofteventhubnamespaces"></a>Microsoft.EventHub/namespaces

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|INREQS|Příchozích požadavků na odeslání|Počet|Celkem|Celkový počet příchozích odesílat požadavky pro Centrum oznámení|
|SUCCREQ|Úspěšné požadavky|Počet|Celkem|Celkový počet úspěšných požadavků pro obor názvů|
|FAILREQ|Neúspěšné požadavky|Počet|Celkem|Celkový počet neúspěšných požadavků pro obor názvů|
|SVRBSY|Chyby zaneprázdněný server|Počet|Celkem|Celkový počet server zaneprázdněn chyby pro obor názvů|
|INTERR|Vnitřní chyby serveru|Počet|Celkem|Celkový počet vnitřní chyby serveru pro obor názvů|
|MISCERR|Další chyby|Počet|Celkem|Celkový počet neúspěšných požadavků pro obor názvů|
|INMSGS|Příchozí zprávy|Počet|Celkem|Celkový počet příchozích zpráv pro obor názvů|
|OUTMSGS|Odchozí zprávy|Počet|Celkem|Celkový počet odchozích zpráv pro obor názvů|
|EHINMBS|Příchozí bajty|Bajty|Celkem|Události rozbočovače příchozí zprávy propustnost pro obor názvů|
|EHOUTMBS|Odchozí bajty|Bajty|Celkem|Celkový počet odchozích zpráv pro obor názvů|
|EHABL|Nevyřízené položky zpráv archivu|Počet|Celkem|Zprávy archivu centra událostí v nevyřízených položek pro obor názvů|
|EHAMSGS|Archivace zpráv|Počet|Celkem|Centra událostí archivované zprávy v oboru názvů|
|EHAMBS|Propustnost zpráv archivu|Bajty|Celkem|Centra událostí archivovat propustnost zpráv v oboru názvů|

## <a name="microsoftlogicworkflows"></a>Microsoft.Logic/workflows

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|RunsStarted|Spustí spuštění|Počet|Celkem|Počet spuštěných běhů pracovního postupu.|
|RunsCompleted|Spustí byla dokončena|Počet|Celkem|Počet dokončených běhů pracovního postupu.|
|RunsSucceeded|Spustí bylo úspěšné|Počet|Celkem|Počet úspěšných běhů pracovního postupu.|
|RunsFailed|Spustí se nezdařilo|Počet|Celkem|Počet neúspěšných běhů pracovního postupu.|
|RunsCancelled|Spustí zrušena|Počet|Celkem|Počet zrušených běhů pracovního postupu.|
|RunLatency|Spustit latence|Sekundy|Průměr|Latence dokončených běhů pracovního postupu.|
|RunSuccessLatency|Spustit latence úspěch|Sekundy|Průměr|Latence úspěšných běhů pracovního postupu.|
|RunThrottledEvents|Spustit omezenému události|Počet|Celkem|Počet akcí pracovního postupu nebo omezených událostí triggeru.|
|RunFailurePercentage|Spustit procento selhání|Procento|Celkem|Procento pracovního postupu, spustí se nezdařilo.|
|ActionsStarted|Akce spustila |Počet|Celkem|Počet spuštěných akcí pracovního postupu.|
|ActionsCompleted|Akce |Počet|Celkem|Počet dokončených akcí pracovního postupu.|
|ActionsSucceeded|Akce byla úspěšná |Počet|Celkem|Počet úspěšných akcí pracovního postupu.|
|ActionsFailed|Akce se nezdařilo|Počet|Celkem|Počet neúspěšných akcí pracovního postupu.|
|ActionsSkipped|Akce přeskočena |Počet|Celkem|Počet vynechaných akcí pracovního postupu.|
|ActionLatency|Latence akce |Sekundy|Průměr|Latence dokončených akcí pracovního postupu.|
|ActionSuccessLatency|Latence úspěch akce |Sekundy|Průměr|Latence úspěšných akcí pracovního postupu.|
|ActionThrottledEvents|Akce omezení událostí|Počet|Celkem|Počet omezených událostí akcí pracovního postupu.|
|TriggersStarted|Spuštění aktivační události |Počet|Celkem|Počet spuštěných triggerů pracovního postupu.|
|TriggersCompleted|Aktivace dokončena |Počet|Celkem|Počet dokončených triggerů pracovního postupu.|
|TriggersSucceeded|Aktivační události bylo úspěšně dokončeno |Počet|Celkem|Počet úspěšných triggerů pracovního postupu.|
|TriggersFailed|Aktivace se nezdařila |Počet|Celkem|Počet neúspěšných triggerů pracovního postupu.|
|TriggersSkipped|Aktivační události přeskočena|Počet|Celkem|Počet vynechaných triggerů pracovního postupu.|
|TriggersFired|Aktivační události aktivováno |Počet|Celkem|Počet aktivovaných triggerů pracovního postupu.|
|TriggerLatency|Latence aktivační události |Sekundy|Průměr|Latence dokončených triggerů pracovního postupu.|
|TriggerFireLatency|Aktivační událost ještě efektivněji latence |Sekundy|Průměr|Latence aktivovaných triggerů pracovního postupu.|
|TriggerSuccessLatency|Aktivační událost úspěch latence |Sekundy|Průměr|Latence úspěšných triggerů pracovního postupu.|
|TriggerThrottledEvents|Aktivaci omezenému událostí|Počet|Celkem|Počet omezených událostí triggeru pracovního postupu.|
|BillableActionExecutions|Fakturovatelný akce spuštění|Počet|Celkem|Počet spuštěních akce pracovního postupu získávání účtují.|
|BillableTriggerExecutions|Fakturovatelný aktivační událost spuštění|Počet|Celkem|Počet spuštěních aktivační událost pracovního postupu získávání účtují.|
|TotalBillableExecutions|Celkový počet spuštěních fakturovatelného času|Počet|Celkem|Počet spuštěních pracovního postupu získávání účtují.|

## <a name="microsoftnetworkapplicationgateways"></a>Microsoft.Network/applicationGateways

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|Propustnost|Propustnost|BytesPerSecond|Průměr||

## <a name="microsoftnetworkexpressroutecircuits"></a>Microsoft.Network/expressRouteCircuits

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|BytesIn|BytesIn|Počet|Celkem||
|BytesOut|BytesOut|Počet|Celkem||

## <a name="microsoftnotificationhubsnamespacesnotificationhubs"></a>Microsoft.NotificationHubs/Namespaces/NotificationHubs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|Registration.all|Operace Registrování|Počet|Celkem|Počet všech operací úspěšnou registraci (vytváření dotazů aktualizace a odstranění). |
|Registration.Create|Registrace vytvoření operací|Počet|Celkem|Počet vytváření všech úspěšnou registraci.|
|Registration.Update|Operace aktualizace registrace|Počet|Celkem|Počet všech aktualizací úspěšnou registraci.|
|Registration.Get|Operace čtení registrace|Počet|Celkem|Počet všech dotazů úspěšnou registraci.|
|Registration.delete|Operace odstranění registrace|Počet|Celkem|Počet všech odstranění úspěšnou registraci.|
|příchozí|Příchozí zprávy|Počet|Celkem|Počet všech úspěšné odeslání volání rozhraní API. |
|Incoming.Scheduled|Naplánované nabízená oznámení odesílaná|Počet|Celkem|Naplánované nabízená oznámení zrušena|
|Incoming.Scheduled.Cancel|Naplánované nabízená oznámení zrušena|Počet|Celkem|Naplánované nabízená oznámení zrušena|
|Scheduled.Pending|Čekající naplánované oznámení|Počet|Celkem|Čekající naplánované oznámení|
|Installation.all|Operace správy instalace|Počet|Celkem|Operace správy instalace|
|Installation.Get|Získat operace instalace|Počet|Celkem|Získat operace instalace|
|Installation.upsert|Vytvořit nebo aktualizovat operace instalace|Počet|Celkem|Vytvořit nebo aktualizovat operace instalace|
|Installation.patch|Operace instalace opravy|Počet|Celkem|Operace instalace opravy|
|Installation.delete|Instalace operace odstranění|Počet|Celkem|Instalace operace odstranění|
|outgoing.allpns.Success|Úspěšné oznámení|Počet|Celkem|Počet všech úspěšné oznámení.|
|outgoing.allpns.invalidpayload|Datová část chyby|Počet|Celkem|Počet oznámení, které se nezdařila, protože systém PNS vrátil chybu chybná datová část.|
|outgoing.allpns.pnserror|Chyby systému externí oznámení|Počet|Celkem|Počet oznámení, které se nezdařila, protože došlo k potížím při komunikaci se Správou (nezahrnuje problémy ověřování).|
|outgoing.allpns.channelerror|Kanál chyby|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože kanál byla neplatná nejsou přidružené aplikace správné omezeny nebo jeho platnost.|
|outgoing.allpns.badorexpiredchannel|Chyby – chybný kanál nebo vypršení časového limitu kanálu|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože vypršela platnost, nebo neplatný kanál/token nebo registrationId v registraci.|
|outgoing.wns.Success|Úspěšné oznámení WNS|Počet|Celkem|Počet všech úspěšné oznámení.|
|outgoing.wns.invalidcredentials|Chyb autorizace WNS (Neplatné přihlašovací údaje)|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože systém PNS nebyl přijat zadané přihlašovací údaje nebo přihlašovací údaje jsou blokovány. (Windows Live nerozpoznal přihlašovací údaje).|
|outgoing.wns.badchannel|Chyba WNS chybný kanál|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože nebyla rozpoznána ChannelURI v registraci (WNS stav: 404 nebyl nalezen).|
|outgoing.wns.expiredchannel|WNS platnost Chyba kanálu|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože vypršela platnost ChannelURI (WNS stav: 410 Gone).|
|outgoing.wns.throttled|WNS omezeny oznámení|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože WNS je omezení této aplikace (WNS stav: 406 Nepřijatelný).|
|outgoing.wns.tokenproviderunreachable|Chyb autorizace WNS (nedostupné)|Počet|Celkem|Windows Live není dostupný.|
|outgoing.wns.invalidtoken|Chyb autorizace WNS (neplatný Token)|Počet|Celkem|Zadaný pro WNS token není platný (WNS stav: 401 – Neověřeno).|
|outgoing.wns.wrongtoken|Chyb autorizace WNS (chybný Token)|Počet|Celkem|Zadaný pro WNS token je platný, ale pro jinou aplikaci (WNS stav: 403 Zakázáno). To může nastat, když ChannelURI v registraci souvisí s jinou aplikací. Zkontrolujte, jestli klientská aplikace je přidružená ke stejné aplikaci, jehož pověření se v centru oznámení.|
|outgoing.wns.invalidnotificationformat|Formát neplatný oznámení WNS|Počet|Celkem|Formát oznámení není platný (stav WNS: 400). Všimněte si, že WNS není odmítnout všechny neplatné datové části.|
|outgoing.wns.invalidnotificationsize|WNS – chyba neplatné velikosti oznámení|Počet|Celkem|Datová část oznámení je moc velký (WNS stav: 413).|
|outgoing.wns.channelthrottled|Kanál WNS omezení|Počet|Celkem|Oznámení byla vyřazena, protože se omezuje ChannelURI v registraci (hlavička odpovědi WNS: X-WNS-NotificationStatus:channelThrottled).|
|outgoing.wns.channeldisconnected|Kanál WNS odpojení|Počet|Celkem|Oznámení byla vyřazena, protože se omezuje ChannelURI v registraci (hlavička odpovědi WNS: X-WNS-DeviceConnectionStatus: odpojení).|
|outgoing.wns.dropped|WNS vyřadit oznámení|Počet|Celkem|Oznámení byla vyřazena, protože se omezuje ChannelURI v registraci (X-WNS-NotificationStatus: vynechaných, ale není X-WNS-DeviceConnectionStatus: odpojení).|
|outgoing.wns.pnserror|Chyby WNS|Počet|Celkem|Oznámení nejsou doručeny z důvodu chyby komunikace s WNS.|
|outgoing.wns.authenticationerror|Chyby ověřování WNS|Počet|Celkem|Oznámení nejsou doručeny z důvodu chyby komunikace s Windows Live neplatné přihlašovací údaje nebo chybný token.|
|outgoing.apns.Success|Úspěšné oznámení APNS|Počet|Celkem|Počet všech úspěšné oznámení.|
|outgoing.apns.invalidcredentials|Chyb autorizace služby APN|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože systém PNS nebyl přijat zadané přihlašovací údaje nebo přihlašovací údaje jsou blokovány.|
|outgoing.apns.badchannel|Chyba služby APN chybný kanál|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože token je neplatný (kód stavu služby APN: 8).|
|outgoing.apns.expiredchannel|APNS vypršela platnost Chyba kanálu|Počet|Celkem|Počet token, který došlo ke zrušení platnosti podle kanálu zpětné vazby služby APN.|
|outgoing.apns.invalidnotificationsize|APNS – chyba neplatné velikosti oznámení|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože byla příliš velká datová část (APNS stavový kód: 7).|
|outgoing.apns.pnserror|Chyby služby APN|Počet|Celkem|Počet nabízených oznámení, která se nezdařila z důvodu chyb, komunikaci se službou APNS.|
|outgoing.gcm.Success|Úspěšné oznámení GCM|Počet|Celkem|Počet všech úspěšné oznámení.|
|outgoing.gcm.invalidcredentials|Chyb autorizace služby GCM (Neplatné přihlašovací údaje)|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože systém PNS nebyl přijat zadané přihlašovací údaje nebo přihlašovací údaje jsou blokovány.|
|outgoing.gcm.badchannel|Chyba GCM chybný kanál|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože nebyla rozpoznána registrationId v registraci (GCM výsledek: Neplatná registrační).|
|outgoing.gcm.expiredchannel|GCM platnost Chyba kanálu|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože platnost vypršela registrationId v registraci (GCM výsledek: NotRegistered).|
|outgoing.gcm.throttled|Omezení oznámení GCM|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože GCM omezuje tuto aplikaci (GCM stavový kód: 501-599 nebo výsledek: není k dispozici).|
|outgoing.gcm.invalidnotificationformat|Formát neplatný oznámení GCM|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože nebyla správně naformátována datové části (GCM výsledek: InvalidDataKey nebo InvalidTtl).|
|outgoing.gcm.invalidnotificationsize|GCM – chyba neplatné velikosti oznámení|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože byla příliš velká datová část (GCM výsledek: MessageTooBig).|
|outgoing.gcm.wrongchannel|Chyba GCM nesprávné kanálu|Počet|Celkem|Počet oznámení, které se nezdařila, protože není přidružen k aktuální aplikaci registrationId v registraci (GCM výsledek: InvalidPackageName).|
|outgoing.gcm.pnserror|Chyby GCM|Počet|Celkem|Počet nabízených oznámení, která se nezdařila z důvodu chyb, komunikaci se službou GCM.|
|outgoing.gcm.authenticationerror|Chyby ověřování GCM|Počet|Celkem|Počet oznámení, které se nezdařila, protože systém PNS nebyl přijat zadaných pověření přihlašovací údaje jsou zablokované nebo ID odesílatele není správně nakonfigurována v aplikaci (GCM výsledek: MismatchedSenderId).|
|outgoing.mpns.Success|Úspěšné oznámení MPNS|Počet|Celkem|Počet všech úspěšné oznámení.|
|outgoing.mpns.invalidcredentials|Neplatné přihlašovací údaje MPNS|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože systém PNS nebyl přijat zadané přihlašovací údaje nebo přihlašovací údaje jsou blokovány.|
|outgoing.mpns.badchannel|Chyba MPNS chybný kanál|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože nebyla rozpoznána ChannelURI v registraci (MPNS stav: 404 nebyl nalezen).|
|outgoing.mpns.throttled|MPNS omezeny oznámení|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože MPNS se omezení této aplikace (WNS MPNS: 406 Nepřijatelný).|
|outgoing.mpns.invalidnotificationformat|Formát neplatný oznámení MPNS|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože datová část oznámení byla příliš velká.|
|outgoing.mpns.channeldisconnected|Kanál MPNS odpojení|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože byla odpojena ChannelURI v registraci (MPNS stav: 412 nebyl nalezen).|
|outgoing.mpns.dropped|MPNS vyřadit oznámení|Počet|Celkem|Počet oznámení, které byly vynechány podle MPNS (MPNS hlavičku odpovědi: X NotificationStatus: QueueFull nebo Suppressed).|
|outgoing.mpns.pnserror|Chyby MPNS|Počet|Celkem|Počet nabízených oznámení, která se nezdařilo z důvodu chyby komunikace se MPNS.|
|outgoing.mpns.authenticationerror|Chyby ověřování MPNS|Počet|Celkem|Počet nabízených oznámení, která se nezdařila, protože systém PNS nebyl přijat zadané přihlašovací údaje nebo přihlašovací údaje jsou blokovány.|
|notificationhub.pushes|Všechny odchozí oznámení|Počet|Celkem|Všechny odchozí oznámení centra oznámení|
|Incoming.all.Requests|Všechny příchozí požadavky|Počet|Celkem|Celkový počet příchozích požadavků pro Centrum oznámení|
|Incoming.all.failedrequests|Všechny příchozí neúspěšné požadavky|Počet|Celkem|Celkový počet příchozích neúspěšných požadavků pro Centrum oznámení|

## <a name="microsoftpowerbidedicatedcapacities"></a>Microsoft.PowerBIDedicated/capacities

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|qpu_metric|QPU|Počet|Průměr|QPU. Rozsah 0-100 S1, 0 – 200 S2 a 0 – 400 pro S4|
|memory_metric|Memory (Paměť)|Bajty|Průměr|Paměť. V rozsahu 0-25 GB pro S1, 0 – 50 GB pro S2 a 0 – 100 GB pro S4|
|TotalConnectionRequests|Žádosti o připojení (celkem)|Počet|Průměr|Požadavky na celkový počet připojení. Jedná se o doručení.|
|SuccessfullConnectionsPerSec|Úspěšné připojení za sekundu|CountPerSecond|Průměr|Počet dokončených úspěšné připojení.|
|TotalConnectionFailures|Chyby připojení (celkem)|Počet|Průměr|Celkový počet neúspěšných pokusů o připojení.|
|CurrentUserSessions|Aktuální relace uživatele|Počet|Průměr|Aktuální počet uživatelských relací navázat.|
|QueryPoolBusyThreads|Dotaz z fondu podprocesů zaneprázdněn|Počet|Průměr|Počet vytížených vláken ve fondu vláken dotazu.|
|CommandPoolJobQueueLength|Příkaz délku fronty úloh|Počet|Průměr|Počet úloh ve frontě příkaz fondu vláken.|
|ProcessingPoolJobQueueLength|Délka fronty úloh zpracování fondu|Počet|Průměr|Počet úloh jiný I/O ve frontě fondu zpracování vláken.|
|CurrentConnections|Připojení: Aktuální připojení|Počet|Průměr|Aktuální počet připojení klienta.|
|CleanerCurrentPrice|Paměť: Čisticí aktuální cena|Počet|Průměr|Aktuální cena paměti, $a bajtů/čas, normalizovány na 1000.|
|CleanerMemoryShrinkable|Paměť: Čisticí paměti vypočítat|Bajty|Průměr|Množství paměti v bajtech, podstoupí čisticí vyprazdňování pozadím.|
|CleanerMemoryNonshrinkable|Paměť: Čisticí nonshrinkable paměti|Bajty|Průměr|Množství paměti v bajtech, není v souladu čisticí vyprazdňování pozadím.|
|Parametru MemoryUsage|Paměti: Využití paměti|Bajty|Průměr|Využití paměti procesem serveru v rámci výpočet ceny čisticí paměti. Rovná se čítač Process\PrivateBytes plus velikost dat mapované paměti, ignoruje všechny paměti, které bylo namapované nebo přidělené modul xVelocity analýzy v paměti (VertiPaq) překračující modul xVelocity Limit paměti.|
|MemoryLimitHard|Paměti: Pevný Limit paměti|Bajty|Průměr|Omezení pevné paměti z konfiguračního souboru.|
|MemoryLimitHigh|Paměť: Omezení paměti vysoká|Bajty|Průměr|Limit velkého množství paměti, z konfiguračního souboru.|
|MemoryLimitLow|Paměti: Nízká Limit paměti|Bajty|Průměr|Limit nedostatek paměti z konfiguračního souboru.|
|MemoryLimitVertiPaq|Paměti: VertiPaq Limit paměti|Bajty|Průměr|Omezení v paměti z konfiguračního souboru.|
|Kvóta|Paměť: kvóty|Bajty|Průměr|Aktuální kvótu paměti, v bajtech. Kvótu paměti se také označuje jako rezervace paměti grant nebo paměti.|
|QuotaBlocked|Paměti: Blokované kvótu|Počet|Průměr|Aktuální počet požadavků kvóty, které jsou blokovaný, dokud jsou uvolněny kvóty další paměť.|
|VertiPaqNonpaged|Paměť: VertiPaq nestránkovaného fondu|Bajty|Průměr|Bajtů paměti uzamčena v pracovní sadě pro použití stroj v paměti.|
|VertiPaqPaged|Paměť: VertiPaq stránkovaného fondu|Bajty|Průměr|Bajty stránkovaného paměti používané pro data v paměti.|
|RowsReadPerSec|Zpracování: Řádky čtení za sekundu|CountPerSecond|Průměr|Počet řádků přečíst ze všech relačních databází.|
|RowsConvertedPerSec|Zpracování: Řádky převést za sekundu|CountPerSecond|Průměr|Počet řádků převést během zpracování.|
|RowsWrittenPerSec|Zpracování: Řádků zapsaných za sekundu|CountPerSecond|Průměr|Počet řádků zapsaných během zpracování.|
|CommandPoolBusyThreads|Vláken: Příkaz zaneprázdněn z fondu podprocesů|Počet|Průměr|Počet vytížených vláken ve fondu vláken příkaz.|
|CommandPoolIdleThreads|Vláken: Příkaz nečinných vláken fondu|Počet|Průměr|Počet nečinných vláken ve fondu vláken příkaz.|
|LongParsingBusyThreads|Vláken: Analýza zaneprázdněn vláken dlouho|Počet|Průměr|Počet vytížených vláken ve fondu vláken dlouho analýzy.|
|LongParsingIdleThreads|Vláken: Analýza nečinných vláken dlouho|Počet|Průměr|Počet nečinných vláken ve fondu vláken dlouho analýzy.|
|LongParsingJobQueueLength|Vláken: Analýza dlouho délka fronty úloh|Počet|Průměr|Počet úloh ve frontě dlouho analýzy fondu vláken.|
|ProcessingPoolBusyIOJobThreads|Vláken: Fond zaneprázdněn vstupně-výstupní úlohy vláken zpracování|Počet|Průměr|Počet vláken, které jsou spuštěné úlohy vstupně-výstupních operací ve fondu zpracování vláken.|
|ProcessingPoolBusyNonIOThreads|Vláken: Zaneprázdněný jiný I/O vláken fondu zpracování|Počet|Průměr|Počet vláken, které jsou spuštěné úlohy bez I/O ve fondu zpracování vláken.|
|ProcessingPoolIOJobQueueLength|Vláken: Fond délka fronty vstupně-výstupní úlohy zpracování|Počet|Průměr|Počet vstupně-výstupních úloh ve frontě fondu zpracování vláken.|
|ProcessingPoolIdleIOJobThreads|Vláken: Fond nečinnosti vstupně-výstupní úlohy vláken zpracování|Počet|Průměr|Počet vstupně-výstupních úloh ve fondu zpracování vláken nečinných vláken.|
|ProcessingPoolIdleNonIOThreads|Vláken: Nečinnosti vláken jiný I/O fondu zpracování|Počet|Průměr|Počet nečinných vláken ve fondu zpracování vláken, který je vyhrazený pro jiný I/O úlohy.|
|QueryPoolIdleThreads|Vláken: Dotaz nečinných vláken fondu|Počet|Průměr|Počet vstupně-výstupních úloh ve fondu zpracování vláken nečinných vláken.|
|QueryPoolJobQueueLength|Vláken: Dotaz lengt fronty úloh fondu|Počet|Průměr|Počet úloh ve frontě fondu vláken dotazu.|
|ShortParsingBusyThreads|Vláken: Analýza zaneprázdněn vláken krátké|Počet|Průměr|Počet vytížených vláken v krátké analýzy fondu vláken.|
|ShortParsingIdleThreads|Vláken: Analýza nečinných vláken krátké|Počet|Průměr|Počet nečinných vláken v krátké analýzy fondu vláken.|
|ShortParsingJobQueueLength|Vláken: Analýza délka fronty úloh krátké|Počet|Průměr|Počet úloh ve frontě krátké analýzy fondu vláken.|
|memory_thrashing_metric|Zahlcení paměti|Procento|Průměr|Průměrná paměti zahlcení.|

## <a name="microsoftsearchsearchservices"></a>Microsoft.Search/searchServices

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|SearchLatency|Latence vyhledávání|Sekundy|Průměr|Hledání Průměrná latence pro službu vyhledávání.|
|SearchQueriesPerSecond|Vyhledávací dotazy za sekundu|CountPerSecond|Průměr|Vyhledávací dotazy za sekundu pro službu vyhledávání.|
|ThrottledSearchQueriesPercentage|Procento omezenému vyhledávací dotazy|Procento|Průměr|Procento vyhledávací dotazy, které byly omezené pro službu vyhledávání.|

## <a name="microsoftservicebusnamespaces"></a>Microsoft.ServiceBus/namespaces

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|CPUXNS|Využití procesoru na obor názvů|Procento|Maximální počet|Service bus premium obor názvů procesoru využití metrika|
|WSXNS|Využití paměti na obor názvů|Procento|Maximální počet|Service bus premium obor názvů paměti využití metrika|

## <a name="microsoftsqlserversdatabases"></a>Microsoft.Sql/servers/databases

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|cpu_percent|Procento CPU|Procento|Průměr|Procento CPU|
|physical_data_read_percent|Procento datových V/V|Procento|Průměr|Procento datových V/V|
|log_write_percent|Procento vstupně-výstupní operace protokolu|Procento|Průměr|Procento vstupně-výstupní operace protokolu|
|dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|
|Úložiště|Velikost celkový databáze|Bajty|Maximální počet|Velikost celkový databáze|
|connection_successful|Úspěšné připojení|Počet|Celkem|Úspěšné připojení|
|connection_failed|Neúspěšné připojení|Počet|Celkem|Neúspěšné připojení|
|blocked_by_firewall|Blokováno bránou Firewall|Počet|Celkem|Blokováno bránou Firewall|
|zablokování|Zablokování|Počet|Celkem|Zablokování|
|storage_percent|Procento velikosti databáze|Procento|Maximální počet|Procento velikosti databáze|
|xtp_storage_percent|Procento úložiště OLTP v paměti|Procento|Průměr|Procento úložiště OLTP v paměti|
|workers_percent|Procento pracovních procesů|Procento|Průměr|Procento pracovních procesů|
|sessions_percent|Procento relací|Procento|Průměr|Procento relací|
|dtu_limit|Omezení jednotek DTU|Počet|Průměr|Omezení jednotek DTU|
|dtu_used|Použít DTU|Počet|Průměr|Použít DTU|
|dwu_limit|DWU limit|Počet|Maximální počet|DWU limit|
|dwu_consumption_percent|Procento DWU|Procento|Maximální počet|Procento DWU|
|dwu_used|DWU použít|Počet|Maximální počet|DWU použít|

## <a name="microsoftsqlserverselasticpools"></a>Microsoft.Sql/servers/elasticPools

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|cpu_percent|Procento CPU|Procento|Průměr|Procento CPU|
|database_cpu_percent|Procento CPU|Procento|Průměr|Procento CPU|
|physical_data_read_percent|Procento datových V/V|Procento|Průměr|Procento datových V/V|
|database_physical_data_read_percent|Procento datových V/V|Procento|Průměr|Procento datových V/V|
|log_write_percent|Procento vstupně-výstupní operace protokolu|Procento|Průměr|Procento vstupně-výstupní operace protokolu|
|database_log_write_percent|Procento vstupně-výstupní operace protokolu|Procento|Průměr|Procento vstupně-výstupní operace protokolu|
|dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|
|database_dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|
|storage_percent|Procento úložiště|Procento|Průměr|Procento úložiště|
|workers_percent|Procento pracovních procesů|Procento|Průměr|Procento pracovních procesů|
|database_workers_percent|Procento pracovních procesů|Procento|Průměr|Procento pracovních procesů|
|sessions_percent|Procento relací|Procento|Průměr|Procento relací|
|database_sessions_percent|Procento relací|Procento|Průměr|Procento relací|
|eDTU_limit|omezení eDTU|Počet|Průměr|omezení eDTU|
|storage_limit|Limit úložiště|Bajty|Průměr|Limit úložiště|
|eDTU_used|eDTU použít|Počet|Průměr|eDTU použít|
|storage_used|Využité úložiště|Bajty|Průměr|Využité úložiště|
|database_storage_used|Využité úložiště|Bajty|Průměr|Využité úložiště|
|xtp_storage_percent|Procento úložiště OLTP v paměti|Procento|Průměr|Procento úložiště OLTP v paměti|

## <a name="microsoftsqlservers"></a>Microsoft.Sql/servers

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|
|database_dtu_consumption_percent|Procento DTU|Procento|Průměr|Procento DTU|
|storage_used|Využité úložiště|Bajty|Průměr|Využité úložiště|
|database_storage_used|Využité úložiště|Bajty|Průměr|Využité úložiště|

## <a name="microsoftstreamanalyticsstreamingjobs"></a>Microsoft.StreamAnalytics/streamingjobs

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|ResourceUtilization|% Využití SU|Procento|Maximální počet|% Využití SU|
|InputEvents|Vstupní události|Počet|Celkem|Vstupní události|
|InputEventBytes|Vstupní událost bajtů|Bajty|Celkem|Vstupní událost bajtů|
|LateInputEvents|Pozdní vstupní události|Počet|Celkem|Pozdní vstupní události|
|OutputEvents|Výstup události|Počet|Celkem|Výstup události|
|ConversionErrors|Chyby při převodu dat|Počet|Celkem|Chyby při převodu dat|
|Chyby|Chyby za běhu|Počet|Celkem|Chyby za běhu|
|DroppedOrAdjustedEvents|Události mimo pořadí|Počet|Celkem|Události mimo pořadí|
|AMLCalloutRequests|Požadavky funkce|Počet|Celkem|Požadavky funkce|
|AMLCalloutFailedRequests|Požadavky neúspěšné funkce|Počet|Celkem|Požadavky neúspěšné funkce|
|AMLCalloutInputEvents|Události – funkce|Počet|Celkem|Události – funkce|

## <a name="microsoftwebserverfarms"></a>Microsoft.Web/serverfarms

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|CpuPercentage|Procento procesoru|Procento|Průměr|Procento procesoru|
|MemoryPercentage|Procento paměti|Procento|Průměr|Procento paměti|
|DiskQueueLength|Délka fronty disku|Počet|Celkem|Délka fronty disku|
|HttpQueueLength|Délka fronty http|Počet|Celkem|Délka fronty http|
|BytesReceived|Data v|Bajty|Celkem|Data v|
|BytesSent|Data|Bajty|Celkem|Data|

## <a name="microsoftwebsites-excluding-functions"></a>Microsoft.Web/sites (s výjimkou funkce)

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|CpuTime|Čas procesoru|Sekundy|Celkem|Čas procesoru|
|Požadavky|Požadavky|Počet|Celkem|Požadavky|
|BytesReceived|Data v|Bajty|Celkem|Data v|
|BytesSent|Data|Bajty|Celkem|Data|
|Http101|HTTP 101|Počet|Celkem|HTTP 101|
|Http2xx|Http 2xx|Počet|Celkem|Http 2xx|
|Http3xx|Http 3xx|Počet|Celkem|Http 3xx|
|Http401|HTTP 401|Počet|Celkem|HTTP 401|
|Http403|HTTP 403|Počet|Celkem|HTTP 403|
|Http404|HTTP 404|Počet|Celkem|HTTP 404|
|Http406|HTTP 406|Počet|Celkem|HTTP 406|
|Http4xx|Http 4xx|Počet|Celkem|Http 4xx|
|Http5xx|Chyby protokolu HTTP serveru|Počet|Celkem|Chyby protokolu HTTP serveru|
|MemoryWorkingSet|Paměť pracovní sady|Bajty|Průměr|Paměť pracovní sady|
|AverageMemoryWorkingSet|Průměrná paměti pracovní sady|Bajty|Průměr|Průměrná paměti pracovní sady|
|AverageResponseTime|Průměrná doba odezvy|Sekundy|Průměr|Průměrná doba odezvy|

## <a name="microsoftwebsites-functions"></a>Microsoft.Web/sites (funkce)

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|BytesReceived|Data v|Bajty|Celkem|Data v|
|BytesSent|Data|Bajty|Celkem|Data|
|Http5xx|Chyby protokolu HTTP serveru|Počet|Celkem|Chyby protokolu HTTP serveru|
|MemoryWorkingSet|Paměť pracovní sady|Bajty|Průměr|Paměť pracovní sady|
|AverageMemoryWorkingSet|Průměrná paměti pracovní sady|Bajty|Průměr|Průměrná paměti pracovní sady|
|FunctionExecutionUnits|Funkce spuštění jednotky|Počet|Průměr|Funkce spuštění jednotky|
|FunctionExecutionCount|Počet provedení – funkce|Počet|Průměr|Počet provedení – funkce|

## <a name="microsoftwebsitesslots"></a>Microsoft.Web/sites/slots

|Metrika|Metriky zobrazovaný název|Jednotka|Typ agregace|Popis|
|---|---|---|---|---|
|CpuTime|Čas procesoru|Sekundy|Celkem|Čas procesoru|
|Požadavky|Požadavky|Počet|Celkem|Požadavky|
|BytesReceived|Data v|Bajty|Celkem|Data v|
|BytesSent|Data|Bajty|Celkem|Data|
|Http101|HTTP 101|Počet|Celkem|HTTP 101|
|Http2xx|Http 2xx|Počet|Celkem|Http 2xx|
|Http3xx|Http 3xx|Počet|Celkem|Http 3xx|
|Http401|HTTP 401|Počet|Celkem|HTTP 401|
|Http403|HTTP 403|Počet|Celkem|HTTP 403|
|Http404|HTTP 404|Počet|Celkem|HTTP 404|
|Http406|HTTP 406|Počet|Celkem|HTTP 406|
|Http4xx|Http 4xx|Počet|Celkem|Http 4xx|
|Http5xx|Chyby protokolu HTTP serveru|Počet|Celkem|Chyby protokolu HTTP serveru|
|MemoryWorkingSet|Paměť pracovní sady|Bajty|Průměr|Paměť pracovní sady|
|AverageMemoryWorkingSet|Průměrná paměti pracovní sady|Bajty|Průměr|Průměrná paměti pracovní sady|
|AverageResponseTime|Průměrná doba odezvy|Sekundy|Průměr|Průměrná doba odezvy|
|FunctionExecutionUnits|Funkce spuštění jednotky|Počet|Průměr|Funkce spuštění jednotky|
|FunctionExecutionCount|Počet provedení – funkce|Počet|Průměr|Počet provedení – funkce|

## <a name="next-steps"></a>Další kroky
* [Přečtěte si informace o metriky v Azure monitorování](monitoring-overview-metrics.md)
* [Vytvářet upozornění na metriky](insights-receive-alert-notifications.md)
* [Export metriky pro úložiště, centra událostí nebo analýzy protokolů](monitoring-overview-of-diagnostic-logs.md)
