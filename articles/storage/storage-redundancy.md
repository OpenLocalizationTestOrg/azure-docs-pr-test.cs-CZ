---
title: Replikace dat v Azure Storage | Microsoft Docs
description: "Data ve vašem účtu úložiště Microsoft Azure se replikují pro odolnost a vysokou dostupnost. Možnosti replikace zahrnují místně redundantní úložiště (LRS), zónově redundantní úložiště (ZRS), geograficky redundantní úložiště (GRS) a geograficky redundantní úložiště s přístupem pro čtení (RA-GRS)."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: 86bdb6d4-da59-4337-8375-2527b6bdf73f
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: marsma
ms.openlocfilehash: b9354484ff5b81e2561d017d039bf2c07a21a423
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-storage-replication"></a>Účet replikace Azure Storage

Data na vašem účtu Microsoft Azure Storage se vždy replikují, aby byla zajištěna jejich stálost a vysoká dostupnost. Replikace zkopíruje data, a to buď v rámci stejného datového centra, nebo do druhého datového centra (v závislosti na možnosti replikace, kterou zvolíte). Replikace chrání vaše data a udrží vaše aplikace v provozu v případě krátkodobého selhání hardwaru. Pokud vaše data se replikují do druhého datového centra, je chráněn v primárním umístění závažné selhání.

Replikace zajišťuje, že váš účet úložiště splňuje [smlouvu o úrovni služeb (SLA) pro Storage](https://azure.microsoft.com/support/legal/sla/storage/) i při selhání. Podívejte se do smlouvy SLA na informace o zárukách služby Azure Storage na stálost a dostupnost.

Při vytvoření účtu úložiště si můžete vybrat jednu z těchto možností replikace:

* [Místně redundantní úložiště (LRS)](#locally-redundant-storage)
* [Zónově redundantní úložiště (ZRS)](#zone-redundant-storage)
* [Geograficky redundantní úložiště (GRS)](#geo-redundant-storage)
* [Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS)](#read-access-geo-redundant-storage)

Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS) je výchozí možností při vytváření účtu úložiště.

Následující tabulka poskytuje rychlý přehled o rozdílech mezi LRS, ZRS, GRS a RA-GRS, zatímco dalších částech adres každý typ replikace podrobněji.

| Strategie replikace | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Data se replikují přes několik datových center. |Ne |Ano |Ano |Ano |
| Ze sekundární lokality, stejně jako primární umístění můžete číst data. |Ne |Ne |Ne |Ano |
| Počet kopií dat uchovávaných na samostatných uzlech |3 |3 |6 |6 |

V tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/) pro informace o cenách pro možnosti různých redundance.

> [!NOTE]
> Premium Storage podporuje pouze místně redundantní úložiště (LRS). Informace o Premium Storage najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](storage-premium-storage.md).
>

> [!NOTE]
> Úložiště Azure File podporuje pouze místně redundantní úložiště (LRS) a georedundant úložiště (GRS). Informace o Azure File Storage najdete v tématu [přehled Azure File storage](storage-files-introduction.md).
>

## <a name="locally-redundant-storage"></a>Místně redundantní úložiště
Místně redundantní úložiště (LRS) replikuje data třikrát v rámci jednotky škálování úložiště, který je hostován v datacentru v oblasti, ve které jste vytvořili účet úložiště. Žádost o zápis vrátí úspěšně pouze po jeho byla zapsána na všechny tři repliky. Tyto tři repliky každý nacházet v domén samostatné selhání a upgradu domén v rámci jedné škálovací jednotky úložiště.

Jednotka škálování úložiště je kolekce stojany uzlů úložiště. (FD) domény selhání je skupina uzlů, které představují fyzická jednotka selhání a lze považovat za uzly, které patří do stejné fyzické racku. Upgradovací doméně (UD) je skupina uzlů, které jsou upgradovány společně během procesu upgradu služby (zavedení). Tři repliky jsou rozloženy UDs a FDs v rámci jedné jednotky škálování úložiště zajistit, že data je k dispozici i v případě selhání hardwaru má dopad na jednom racku, nebo když jsou uzly upgradován během zavedení.

LRS je nejnižší náklady na možnost a nabízí minimálně odolnost ve srovnání s dalšími možnostmi. V případě havárie úrovně datového centra (aktivuje, zahlcení atd.) může být všechny tři repliky ztracena nebo neopravitelné. Chcete-li toto riziko snížilo, geograficky redundantní úložiště (GRS) se doporučuje pro většinu aplikací.

Místně redundantní úložiště, stále může být žádoucí v některých scénářích:

* Poskytuje nejvyšší maximální šířka pásma možnosti replikace Azure Storage.
* Pokud aplikace ukládá data, která může být snadno znovu vytvořena, můžete zvolit LRS.
* Některé aplikace jsou omezeny na replikaci dat pouze v rámci země z důvodu požadavky zásad správného řízení data. Spárované oblast může být v jiné zemi. Další informace o páry oblast, najdete v části [oblastí Azure](https://azure.microsoft.com/regions/).

## <a name="zone-redundant-storage"></a>Zónově redundantní úložiště
Zónově redundantní úložiště (ZRS) asynchronně replikuje data mezi datovými centry v rámci jednoho nebo dvou oblastech kromě ukládání tři repliky podobný LRS, a zajišťuje tak větší odolnost LRS. Data uložená v ZRS byla odolná i v případě, že primární datové centrum není k dispozici nebo neopravitelné.
Zákazníci, kteří v úmyslu používat ZRS měli vědět:

* ZRS je dostupná jenom pro objekty BLOB bloku v účtech úložiště pro obecné účely a je podporována pouze v úložišti služby verze 2014-02-14 a novější.
* Vzhledem k tomu, že asynchronní replikaci zahrnuje zpoždění, v případě havárie místní je možné, že změny, které ještě nebyla replikována na sekundární se ztratí možné obnovit data z primární.
* Repliky nemusí být k dispozici, dokud Microsoft zahájí převzetí služeb při selhání na sekundární.
* ZRS účty nelze převést na LRS nebo GRS později. Podobně existující účet LRS nebo GRS nelze převést na účet ZRS.
* ZRS účty nemají metriky nebo možnosti protokolování.

## <a name="geo-redundant-storage"></a>Geograficky redundantní úložiště
Geograficky redundantní úložiště (GRS) replikuje data do sekundární oblasti, která je stovky miles od primární oblasti. Pokud má váš účet úložiště GRS povoleno, vaše data byla odolná i v případě výpadku dokončení místní nebo havárii, ve kterém není použitelná pro obnovení primární oblasti.

Účet úložiště GRS povolena je nejprve aktualizace potvrzena pro primární oblasti, kde se replikují třikrát. Aktualizace se pak replikují asynchronně sekundární oblast, kde se také replikují třikrát.

S GRS primární a sekundární oblasti spravovat repliky v rámci domén samostatné selhání a upgradu domén v rámci jednotce škálování úložiště, jak je popsáno s LRS.

Aspekty:

* Vzhledem k tomu, že asynchronní replikaci zahrnuje zpoždění, v případě havárie regionální je možné, že od primární oblasti nelze obnovit data budou ztracena změny, které ještě nebyla replikována do sekundární oblasti.
* Replika není k dispozici, pokud Microsoft zahájí převzetí služeb při selhání pro sekundární oblast. Pokud Microsoft zahájit převzetí služeb při selhání pro sekundární oblast, budete mít ke čtení a zápis do dat po převzetí služeb po dokončení. Další informace najdete v tématu [pokyny pro zotavení po havárii](storage-disaster-recovery-guidance.md). 
* Pokud aplikace chce číst ze sekundární oblasti, uživatel by měl povolit RA-GRS.

Pokud vytvoříte účet úložiště, vyberte primární oblasti pro účet. Sekundární oblast je určen na základě primární oblasti a nelze změnit. V následující tabulce jsou uvedeny dvojice primární a sekundární oblast.

| Primární | Sekundární |
| --- | --- |
| Střed USA – sever |Střed USA – jih |
| Střed USA – jih |Střed USA – sever |
| Východ USA |Západní USA |
| Západní USA |Východ USA |
| USA – východ 2 |Střed USA |
| Střed USA |USA – východ 2 |
| Severní Evropa |Západní Evropa |
| Západní Evropa |Severní Evropa |
| Jihovýchodní Asie |Východní Asie |
| Východní Asie |Jihovýchodní Asie |
| Východní Čína |Severní Čína |
| Severní Čína |Východní Čína |
| Japonsko – východ |Japonsko – západ |
| Japonsko – západ |Japonsko – východ |
| Brazílie – jih |Střed USA – jih |
| Austrálie – východ |Austrálie – jihovýchod |
| Austrálie – jihovýchod |Austrálie – východ |
| Indie – jih |Indie – střed |
| Indie – střed |Indie – jih |
| Indie – západ |Indie – jih |
| USA (Gov) – Iowa |USA (Gov) – Virginia |
| USA (Gov) – Virginia |USA (Gov) – Texas |
| USA (Gov) – Texas |USA (Gov) – Arizona |
| USA (Gov) – Arizona |USA (Gov) – Texas |
| Střední Kanada |Východní Kanada |
| Východní Kanada |Střední Kanada |
| Spojené království – západ |Spojené království – jih |
| Spojené království – jih |Spojené království – západ |
| Německo – střed |Německo – severovýchod |
| Německo – severovýchod |Německo – střed |
| Západní USA 2 |Západní střed USA |
| Západní střed USA |Západní USA 2 |

Aktuální informace o oblastech podporovaných v Azure najdete v tématu [oblastí Azure](https://azure.microsoft.com/regions/).

>[!NOTE]  
> USA – verze pro státní správu Virginia sekundární oblast je Texas nám verze pro státní správu. Dříve nám verze pro státní správu Virginia využité nám verze pro státní správu Iowa jako sekundární oblasti. Účty úložiště stále využití nám verze pro státní správu Iowa jako sekundární oblasti se migruje nám verze pro státní správu Texas jako sekundární oblast. 
> 
> 

## <a name="read-access-geo-redundant-storage"></a>Geograficky redundantní úložiště s přístupem pro čtení
Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS) maximalizuje dostupnost pro váš účet úložiště, tím, že poskytuje přístup jen pro čtení k datům v sekundárním umístění, kromě replikace rámci dvou oblastí poskytované GRS.

Když povolíte přístup jen pro čtení k datům v sekundární oblasti, vaše data jsou k dispozici na sekundárním koncovým bodem, kromě primární koncový bod pro váš účet úložiště. Sekundární koncový bod je podobná primární koncový bod, ale připojí přípona `–secondary` k názvu účtu. Například, pokud je váš primární koncový bod služby objektů Blob `myaccount.blob.core.windows.net`, pak je sekundární koncový bod `myaccount-secondary.blob.core.windows.net`. Přístupové klíče účtu úložiště jsou stejné pro obě primární a sekundární koncové body.

Aspekty:

* Aplikace má ke správě kterému koncovému bodu je interakci s při použití RA-GRS.
* Vzhledem k tomu, že asynchronní replikaci zahrnuje zpoždění, v případě havárie regionální je možné, že od primární oblasti nelze obnovit data budou ztracena změny, které ještě nebyla replikována do sekundární oblasti.
* Pokud Microsoft zahájí převzetí služeb při selhání pro sekundární oblast, budete mít ke čtení a zápis do dat po převzetí služeb po dokončení. Další informace najdete v tématu [pokyny pro zotavení po havárii](storage-disaster-recovery-guidance.md). 
* RA-GRS, je určena pro účely vysokou dostupnost. Pokyny k škálovatelnost, přečtěte si [kontrolní seznam výkonu](storage-performance-checklist.md).

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

<a id="howtochange"></a>
#### <a name="1-how-can-i-change-the-geo-replication-type-of-my-storage-account"></a>1. Jak můžete změnit typ geografické replikace pro svůj účet úložiště?

   Můžete změnit typ geografická replikace účtu úložiště mezi LRS, GRS a RA-GRS pomocí [portál Azure](https://portal.azure.com/), [prostředí Azure Powershell](storage-powershell-guide-full.md) nebo programově pomocí jednoho z našich mnoho knihovny klienta úložiště . Upozorňujeme, že účty ZRS nemůže být převedená LRS nebo GRS. Podobně existující účet LRS nebo GRS nelze převést na účet ZRS.

<a id="changedowntime"></a>
#### <a name="2-will-there-be-any-down-time-if-i-change-the-replication-type-of-my-storage-account"></a>2. Bude k dispozici žádný výpadek když změníte typ replikace pro svůj účet úložiště?

   Ne, nebude dostupný žádný výpadek.

<a id="changecost"></a>
#### <a name="3-will-there-be-any-additional-cost-if-i-change-the-replication-type-of-my-storage-account"></a>3. Bude k dispozici žádné další náklady když změníte typ replikace pro svůj účet úložiště?

   Ano. Pokud změníte z LRS GRS (nebo RA-GRS) pro účet úložiště, by to být účtovány dalších poplatků odchozí účastnící se kopírování existujících dat z primárního umístění do sekundárního umístění. Po počáteční data se zkopírují je bezplatná další další odchozí pro geografickou replikaci dat z primárního na sekundární umístění. Podrobnosti o poplatky šířky pásma můžete najít na [Azure Storage – ceny stránky](https://azure.microsoft.com/pricing/details/storage/blobs/). Pokud změníte na LRS GRS, je bez dalších nákladů, ale data se odstraní ze sekundárního umístění.

<a id="ragrsbenefits"></a>
#### <a name="4-how-can-ra-grs-help-me"></a>4. Jak RA-GRS mi může pomoci?
   
   GRS úložiště poskytuje replikaci vašich dat z primárního na sekundární oblasti, která je stovky miles od primární oblasti. V takovém případě vaše data byla odolná i v případě výpadku dokončení místní nebo havárii, ve kterém není použitelná pro obnovení primární oblasti. RA-GRS úložiště zahrnuje to a přidá možnost číst data ze sekundárního umístění. Některé měli představu o tom, jak využít tuto možnost, naleznete v [navrhování vysoce dostupné aplikace pomocí úložiště RA-GRS](storage-designing-ha-apps-with-ragrs.md). 

<a id="lastsynctime"></a>
#### <a name="5-is-there-a-way-for-me-to-figure-out-how-long-it-takes-to-replicate-my-data-from-the-primary-to-the-secondary-region"></a>5. Existuje způsob, jak mi zjistěte, jak dlouho trvá replikaci Moje dat z primárního na sekundární oblast?
   
   Pokud používáte RA-GRS úložiště, můžete zkontrolovat, čas poslední synchronizace účtu úložiště. Čas poslední synchronizace je hodnota, datum a čas GMT; všechny primární zápisy před čas poslední synchronizace byla úspěšně zapsána do sekundárního umístění, které střední jsou k dispozici čtení ze sekundárního umístění. Primární zapíše po čas poslední synchronizace může nebo nemusí být k dispozici pro čtení ještě. Tato hodnota pomocí se můžete dotazovat [portál Azure](https://portal.azure.com/), [prostředí Azure PowerShell](storage-powershell-guide-full.md), nebo programově pomocí rozhraní REST API nebo jeden z knihovny klienta úložiště. 

<a id="outage"></a>
#### <a name="6-how-can-i-switch-to-the-secondary-region-if-there-is-an-outage-in-the-primary-region"></a>6. Jak lze přepnout sekundární oblast, pokud dojde k výpadku v primární oblasti?
   
   Podrobnosti najdete v článku na [co dělat, když dojde k výpadku Azure Storage](storage-disaster-recovery-guidance.md) další podrobnosti.

<a id="rpo-rto"></a>
#### <a name="7-what-is-the-rpo-and-rto-with-grs"></a>7. Co je plánovaný bod obnovení a RTO s GRS?
   
   Cíl bodu obnovení (RPO): V GRS a RA-GRS, úložiště služby asynchronně geo replikují data z primární na sekundární umístění. Pokud dojde k havárii hlavní místní a převzetí služeb při selhání musí být provedena, může dojít ke ztrátě poslední rozdílové změny, které nebyly geograficky replikované. Počet minut potenciální – ztráta dat se označuje jako plánovaný bod obnovení (což znamená bod v čase, do kterých můžete obnovit data). Obvykle máme plánovaný bod obnovení méně než 15 minut, i když je aktuálně trvá žádné SLA na jak dlouho se geografická replikace.

   Cíli času obnovení (RTO): Toto je míra o tom, jak dlouho trvalo nám dělat převzetí služeb při selhání a získat zpět do režimu online účet úložiště, pokud bychom měli udělat převzetí služeb při selhání. Čas převzetí služeb při selhání zahrnují následující:
    * Čas, kdy trvá nám prozkoumat a určete, jestli jsme můžete obnovit data v primární lokalitě nebo pokud je potřeba udělat převzetí služeb při selhání.
    * Převzetí služeb při selhání účet změnou primární záznamy DNS tak, aby odkazovaly do sekundárního umístění.

   Jsme převzít odpovědnost velmi vážně uchování dat tak, aby pokud je pravděpodobné, že obnovení dat, jsme zdržovat provádění převzetí služeb při selhání a zaměřit se na obnovení dat v primárním umístění. V budoucnu plánujeme zajistit rozhraní API, která umožňuje spustit převzetí služeb při selhání na úrovni účtu, který by pak umožňuje kontrolovat RTO sami, ale to ještě není k dispozici.
   
## <a name="next-steps"></a>Další kroky
* [Návrh aplikace s vysokou dostupností pomocí RA-GRS úložiště](storage-designing-ha-apps-with-ragrs.md)
* [Ceny za Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Informace o účtech úložiště Azure](storage-create-storage-account.md)
* [Azure Storage škálovatelnosti a cílech výkonnosti](storage-scalability-targets.md)
* [Microsoft Azure Storage redundance možnosti a přístup pro čtení geograficky redundantní úložiště](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
* [STUDIE Sosp - Azure Storage: Vysoce dostupný cloudové úložiště služby se silnou konzistenci](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

