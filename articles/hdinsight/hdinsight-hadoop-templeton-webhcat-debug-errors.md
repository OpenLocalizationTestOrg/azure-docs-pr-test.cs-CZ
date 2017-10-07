---
title: "aaaUnderstand a vyřešte chyby WebHCat na HDInsight - Azure | Microsoft Docs"
description: "Zjistěte, jak tooabout běžné chyby vrácené WebHCat v HDInsight a tooresolve je."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1b3d94b1-207d-4550-aece-21dc45485549
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 0071a1e9ed448ae146b93c8f4f518e31b95d27c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a>Rady pro pochopení a řešení chyb oznámených z WebHCat v HDInsight

Další informace o chyb oznámených při použití WebHCat s HDInsight a jak tooresolve je. WebHCat se používá interně pomocí klienta nástroje, jako je Azure PowerShell a hello nástrojů Data Lake pro Visual Studio.

## <a name="what-is-webhcat"></a>Co je WebHCat

[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) je rozhraní REST API pro [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), tabulka a vrstva správy úložiště pro Hadoop. WebHCat je povoleno ve výchozím nastavení v clusterech prostředí HDInsight a používají různé úlohy toosubmit nástroje, bez přihlášení toohello clusteru můžete získat stav úlohy, atd.

## <a name="modifying-configuration"></a>Změny konfigurace

> [!IMPORTANT]
> Řadu hello chyby uvedené v tomto dokumentu dojít, protože byla překročena maximální nakonfigurované. Pokud krok řešení hello uvádí, že můžete změnit hodnotu, musí používat jednu z hello následující změnu tooperform hello:

* Pro **Windows** clustery: použijte hodnotu hello tooconfigure akce skriptu při vytváření clusteru. Další informace najdete v tématu [vývoj akcí skriptů](hdinsight-hadoop-script-actions.md).

* Pro **Linux** clustery: hodnota hello toomodify pomocí Ambari (web nebo REST API). Další informace najdete v tématu [spravovat HDInsight pomocí Ambari](hdinsight-hadoop-manage-ambari.md)

> [!IMPORTANT]
> Linux je hello pouze operační systém používaný v HDInsight verze 3.4 nebo novější. Další informace najdete v tématu [Vyřazení prostředí HDInsight ve Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

### <a name="default-configuration"></a>Výchozí konfigurace

Pokud se překročí hello následující výchozí hodnoty, může snížit výkon WebHCat nebo způsobit chyby:

| Nastavení | Výsledek | Výchozí hodnota |
| --- | --- | --- |
| [yarn.Scheduler.Capacity.maximum – aplikace][maximum-applications] |maximální počet úloh, které mohou být souběžně aktivní Hello (čekající na vyřízení nebo spuštěné) |10 000 |
| [templeton.Exec.Max-procs][max-procs] |maximální počet požadavků, které lze zpracovat současně Hello |20 |
| [mapreduce.jobhistory.Max stáří ms][max-age-ms] |Hello počet dní, které úlohy historii jsou uchovány |7 dní |

## <a name="too-many-requests"></a>Příliš mnoho požadavků

**Kód stavu HTTP**: 429

| Příčina | Řešení |
| --- | --- |
| Překročili jste maximální souběžných požadavků hello zpracovaných za minutu (výchozí 20) WebHCat |Snižte vaše zatížení tooensure, že nemáte odeslat více než hello maximální počet souběžných požadavků nebo zvyšte limit počtu souběžných požadavků hello úpravou `templeton.exec.max-procs`. Další informace najdete v tématu [Modifying konfigurace](#modifying-configuration) |

## <a name="server-unavailable"></a>Server není dostupný.

**Kód stavu HTTP**: 503

| Příčina | Řešení |
| --- | --- |
| Tento kód stavu obvykle dojde během převzetí služeb při selhání mezi hello primární a sekundární HeadNode pro hello cluster |Počkejte 2 minuty a potom opakujte operaci hello |

## <a name="bad-request-content-could-not-find-job"></a>Chybný požadavek obsahu: Nelze najít úlohu

**Kód stavu HTTP**: 400

| Příčina | Řešení |
| --- | --- |
| Podrobnosti úlohy byla vyčištěna podle historie úlohy hello čisticí |Hello výchozí dobu uchování historie úlohy je 7 dní. výchozí dobu uchování Hello lze změnit úpravou `mapreduce.jobhistory.max-age-ms`. Další informace najdete v tématu [Modifying konfigurace](#modifying-configuration) |
| Úlohy byl ukončen z důvodu tooa převzetí služeb při selhání |Opakujte odeslání úlohy pro až tootwo minut |
| Neplatné id práce. byl použit. |Kontrola, zda je správný hello id úlohy |

## <a name="bad-gateway"></a>Chybná brána

**Kód stavu HTTP**: 502

| Příčina | Řešení |
| --- | --- |
| Uvolňování paměti interní dochází v rámci hello proces WebHCat |Počkejte toofinish kolekce paměti nebo restartujte službu WebHCat hello |
| Časový limit čekání na odpověď od hello ResourceManager služby. K této chybě může dojít, když hello počet aktivních aplikací přejde maximální hello nakonfigurovaný (výchozí 10 000) |Počkejte aktuálně spuštěných úloh toocomplete nebo zvyšte limit souběžných úloh hello úpravou `yarn.scheduler.capacity.maximum-applications`. Další informace najdete v tématu hello [Modifying konfigurace](#modifying-configuration) části. |
| Probíhá pokus tooretrieve všechny úlohy prostřednictvím hello [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) volání při `Fields` je nastaven příliš`*` |Nenačítat *všechny* podrobnosti úlohy. Místo toho použít `jobid` tooretrieve podrobnosti úlohy pouze větší než id určité úlohy. Nebo, nepoužívejte`Fields` |
| Hello WebHCat služby je mimo provoz během převzetí služeb při selhání HeadNode |Počkejte dvou minut a opakujte operaci hello |
| Existuje více než 500 čekající úlohy, odeslané prostřednictvím WebHCat |Počkejte na dokončení aktuálně čeká na provedení úloh před odesláním další úlohy |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
