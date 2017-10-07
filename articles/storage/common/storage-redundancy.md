---
title: "aaaData replikace ve službě Azure Storage | Microsoft Docs"
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
ms.openlocfilehash: ce48d1992fec7bd5ed32feb96b86bef88ca759df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-storage-replication"></a>Účet replikace Azure Storage

Hello data v Microsoft Azure účet úložiště se vždy replikují tooensure odolnost a vysokou dostupnost. Replikace kopie dat, buď v rámci hello stejném datovém centru nebo tooa druhý datového centra, v závislosti na možnosti replikace, kterou zvolíte. Replikace chrání vaše data a chrání vaše aplikace doby provozu v případě hello selhání hardwaru. Pokud vaše data jsou replikované tooa druhý datové centrum, je chráněný před závažné chybě v primárním umístění hello.

Replikace zajišťuje, že váš účet úložiště splňuje hello [smlouvy úrovně služeb (SLA) pro úložiště](https://azure.microsoft.com/support/legal/sla/storage/) i v hello vzhled chyb. Zjistit informace o službě Azure Storage hello SLA zaručuje pro odolnost a dostupnost.

Při vytváření účtu úložiště, můžete vybrat jednu z hello následující možnosti replikace:

* [Místně redundantní úložiště (LRS)](#locally-redundant-storage)
* [Zónově redundantní úložiště (ZRS)](#zone-redundant-storage)
* [Geograficky redundantní úložiště (GRS)](#geo-redundant-storage)
* [Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS)](#read-access-geo-redundant-storage)

Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS) je hello výchozí možnost, při vytváření účtu úložiště.

Hello následující tabulka poskytuje rychlý přehled o hello rozdíly mezi LRS, ZRS, GRS a RA-GRS, zatímco dalších částech adres každý typ replikace podrobněji.

| Strategie replikace | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Data se replikují přes několik datových center. |Ne |Ano |Ano |Ano |
| Ze sekundárního umístění a také hello primární umístění můžete číst data. |Ne |Ne |Ne |Ano |
| Počet kopií dat uchovávaných na samostatných uzlech |3 |3 |6 |6 |

V tématu [Azure Storage – ceny](https://azure.microsoft.com/pricing/details/storage/) pro informace o cenách za hello redundance různé možnosti.

> [!NOTE]
> Premium Storage podporuje pouze místně redundantní úložiště (LRS). Informace o Premium Storage najdete v tématu [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../storage-premium-storage.md).
>

## <a name="locally-redundant-storage"></a>Místně redundantní úložiště
Místně redundantní úložiště (LRS) replikuje data třikrát v rámci jednotky škálování úložiště, který je hostován v datacentru v hello oblasti, ve které jste vytvořili účet úložiště. Žádost o zápis vrátí úspěšně pouze po jejím napsané tooall tři repliky. Tyto tři repliky každý nacházet v domén samostatné selhání a upgradu domén v rámci jedné škálovací jednotky úložiště.

Jednotka škálování úložiště je kolekce stojany uzlů úložiště. (FD) domény selhání je skupina uzlů, které představují fyzická jednotka selhání a lze považovat za uzlech náležících toohello stejné fyzické racku. Upgradovací doméně (UD) je skupina uzlů, které jsou upgradovány společně hello během upgradu služby (zavedení). Hello tři repliky jsou rozloženy UDs a FDs v rámci jednoho úložiště škálovací jednotky tooensure data jsou k dispozici i v případě selhání hardwaru má dopad na jednom racku, nebo když jsou uzly upgradován během zavedení.

LRS se hello nejnižší náklady na možnost a nabízí možnosti tooother minimálně odolnost porovnání. V případě hello úrovně katastrofě datového centra (aktivuje, zahlcení atd.) může být všechny tři repliky ztracena nebo neopravitelné. toomitigate toto riziko, geograficky redundantní úložiště (GRS) se doporučuje pro většinu aplikací.

Místně redundantní úložiště, stále může být žádoucí v některých scénářích:

* Poskytuje nejvyšší maximální šířka pásma možnosti replikace Azure Storage.
* Pokud aplikace ukládá data, která může být snadno znovu vytvořena, můžete zvolit LRS.
* Některé aplikace jsou data s omezeným přístupem tooreplicating pouze v rámci země z důvodu toodata požadavky zásad správného řízení. Spárované oblast může být v jiné zemi. Další informace o páry oblast, najdete v části [oblastí Azure](https://azure.microsoft.com/regions/).

## <a name="zone-redundant-storage"></a>Zónově redundantní úložiště
Zónově redundantní úložiště (ZRS) asynchronně replikuje data mezi datovými centry v rámci jednoho nebo dvou oblastí v přidání toostoring tři repliky podobný tooLRS, a zajišťuje tak větší odolnost LRS. Data uložená v ZRS byla odolná i v případě, že primární datové centrum hello je k dispozici nebo je nejde obnovit.
Zákazníky, kteří plánují toouse ZRS měli vědět:

* ZRS je dostupná jenom pro objekty BLOB bloku v účtech úložiště pro obecné účely a je podporována pouze v úložišti služby verze 2014-02-14 a novější.
* Vzhledem k tomu, že asynchronní replikaci zahrnuje zpoždění, hello události místní po havárii, je možné, že změny, které ještě nebyly replikovány toohello sekundární bude ztracena, pokud hello data nelze obnovit z primární hello.
* Hello repliky nemusí být dostupné, dokud Microsoft zahájí převzetí služeb při selhání toohello sekundární.
* ZRS účty nelze převést novější tooLRS nebo GRS. Podobně existující účet LRS nebo GRS nelze převést tooa ZRS účtu.
* ZRS účty nemají metriky nebo možnosti protokolování.

## <a name="geo-redundant-storage"></a>Geograficky redundantní úložiště
Geograficky redundantní úložiště (GRS) replikuje vaše data tooa sekundární oblast, která je stovky miles od primární oblasti hello. Pokud má váš účet úložiště GRS povoleno, vaše data byla odolná i v případě hello dokončení regionální výpadku nebo havárii, ve které hello primární oblast není použitelná pro obnovení.

Účet úložiště GRS povolena je aktualizace první potvrdit toohello primární oblasti, kde se replikují třikrát. Pak je hello aktualizace replikují asynchronně toohello sekundární oblasti, kde se také replikují třikrát.

S GRS obou hello primární a sekundární oblasti spravovat repliky v rámci domén samostatné selhání a upgradu domén v rámci jednotce škálování úložiště, jak je popsáno s LRS.

Aspekty:

* Vzhledem k tomu, že asynchronní replikaci zahrnuje zpoždění, hello události regionální po havárii, které je možné, že se změní, která ještě nebyla replikována toohello sekundární oblasti se ztratí, pokud hello data nelze obnovit z primární oblasti hello.
* Hello repliky není k dispozici, dokud Microsoft zahájí sekundární oblasti toohello převzetí služeb při selhání. Pokud Microsoft iniciovat sekundární oblasti toohello převzetí služeb při selhání, budete mít oprávnění ke čtení a zápisu dat toothat po dokončení převzetí služeb při selhání hello. Další informace najdete v tématu [pokyny pro zotavení po havárii](../storage-disaster-recovery-guidance.md). 
* Pokud aplikace chce tooread ze sekundární oblasti hello, hello uživatel by měl povolit RA-GRS.

Když vytvoříte účet úložiště, vyberete hello primární oblasti pro účet hello. sekundární oblast Hello je určen na základě hello primární oblasti a nelze změnit. Hello následující tabulka ukazuje dvojice primární a sekundární oblasti hello.

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
> USA – verze pro státní správu Virginia sekundární oblast je Texas nám verze pro státní správu. Dříve nám verze pro státní správu Virginia využité nám verze pro státní správu Iowa jako sekundární oblasti. Účty úložiště stále využití nám verze pro státní správu Iowa jako sekundární oblasti probíhá migrovat tooUS verze pro státní správu Texas jako sekundární oblast. 
> 
> 

## <a name="read-access-geo-redundant-storage"></a>Geograficky redundantní úložiště s přístupem pro čtení
Geograficky redundantní úložiště s přístupem pro čtení (RA-GRS) maximalizuje dostupnost pro váš účet úložiště, tím, že poskytuje přístup jen pro čtení toohello data v hello sekundárního umístění, kromě replikace toohello rámci dvou oblastí poskytované GRS.

Když povolíte přístup jen pro čtení dat tooyour v sekundární oblasti hello, vaše data jsou k dispozici na koncový bod sekundární v přidání toohello primární koncový bod pro váš účet úložiště. sekundární koncový bod Hello je podobné primární koncový bod toohello, ale připojí přípona hello `–secondary` toohello název účtu. Například pokud váš primární koncový bod pro hello služby objektů Blob je `myaccount.blob.core.windows.net`, pak je sekundární koncový bod `myaccount-secondary.blob.core.windows.net`. Hello přístupové klíče účtu úložiště jsou hello stejné pro obě hello primární a sekundární koncové body.

Aspekty:

* Aplikace má toomanage kterému koncovému bodu je interakci s při použití RA-GRS.
* Vzhledem k tomu, že asynchronní replikaci zahrnuje zpoždění, hello události regionální po havárii, které je možné, že se změní, která ještě nebyla replikována toohello sekundární oblasti se ztratí, pokud hello data nelze obnovit z primární oblasti hello.
* Iniciuje-li Microsoft sekundární oblasti toohello převzetí služeb při selhání, budete mít oprávnění ke čtení a zápisu dat toothat po dokončení převzetí služeb při selhání hello. Další informace najdete v tématu [pokyny pro zotavení po havárii](../storage-disaster-recovery-guidance.md). 
* RA-GRS, je určena pro účely vysokou dostupnost. Škálovatelnost pokyny najdete v tématu hello [kontrolní seznam výkonu](../storage-performance-checklist.md).

## <a name="frequently-asked-questions"></a>Nejčastější dotazy

<a id="howtochange"></a>
#### <a name="1-how-can-i-change-hello-geo-replication-type-of-my-storage-account"></a>1. Jak můžete změnit typ replikace geograficky hello svůj účet úložiště?

   Můžete změnit hello geografické replikace typ účtu úložiště mezi LRS, GRS a RA-GRS pomocí hello [portál Azure](https://portal.azure.com/), [prostředí Azure Powershell](storage-powershell-guide-full.md) nebo programově pomocí jednoho z našich mnoho klienta úložiště Knihovny. Upozorňujeme, že účty ZRS nemůže být převedená LRS nebo GRS. Podobně existující účet LRS nebo GRS nelze převést tooa ZRS účtu.

<a id="changedowntime"></a>
#### <a name="2-will-there-be-any-down-time-if-i-change-hello-replication-type-of-my-storage-account"></a>2. Bude k dispozici žádný výpadek Pokud dojde ke změně hello typ replikace pro svůj účet úložiště?

   Ne, nebude dostupný žádný výpadek.

<a id="changecost"></a>
#### <a name="3-will-there-be-any-additional-cost-if-i-change-hello-replication-type-of-my-storage-account"></a>3. Bude k dispozici žádné další náklady Pokud dojde ke změně hello typ replikace pro svůj účet úložiště?

   Ano. Pokud změníte z LRS tooGRS (nebo RA-GRS) pro účet úložiště, by to být účtovány dalších poplatků odchozí účastnící se kopírování existujících dat z primárního umístění toohello sekundárního umístění. Jakmile hello počáteční data se zkopírují neexistuje žádné další další odchozí zdarma pro geografickou replikaci dat hello z umístění primární toosecondary hello. Hello podrobnosti poplatky šířky pásma můžete najít na hello [Azure Storage – ceny stránky](https://azure.microsoft.com/pricing/details/storage/blobs/). Pokud změníte z GRS tooLRS, je bez dalších nákladů, ale data se odstraní ze sekundárního umístění hello.

<a id="ragrsbenefits"></a>
#### <a name="4-how-can-ra-grs-help-me"></a>4. Jak RA-GRS mi může pomoci?
   
   GRS úložiště poskytuje replikaci vašich dat z primární tooa sekundární oblasti, která je stovky miles od primární oblasti hello. V takovém případě vaše data byla odolná i v případě hello dokončení regionální výpadku nebo havárii, ve které hello primární oblast není použitelná pro obnovení. RA-GRS úložiště zahrnuje to a přidá hello možnost tooread hello data ze sekundárního umístění hello. Pro některé své nápady, jak tooleverage tuto možnost, naleznete v příliš[navrhování vysoce dostupné aplikace pomocí úložiště RA-GRS](../storage-designing-ha-apps-with-ragrs.md). 

<a id="lastsynctime"></a>
#### <a name="5-is-there-a-way-for-me-toofigure-out-how-long-it-takes-tooreplicate-my-data-from-hello-primary-toohello-secondary-region"></a>5. Existuje způsob, jak pro mě nejlepší toofigure na jak dlouho trvalo tooreplicate svá data ze sekundární oblasti hello primární toohello?
   
   Pokud používáte RA-GRS úložiště, můžete zkontrolovat hello čas poslední synchronizace účtu úložiště. Čas poslední synchronizace je hodnota, datum a čas GMT; všechny primární zápisy před hello čas poslední synchronizace mít byla úspěšně zapsána toohello sekundárního umístění, což znamená, že jsou k dispozici toobe čtení ze sekundárního umístění hello. Primární zapíše po hello čas poslední synchronizace může nebo nemusí být k dispozici pro čtení ještě. Můžete dát dotaz na tuto hodnotu pomocí hello [portál Azure](https://portal.azure.com/), [prostředí Azure PowerShell](storage-powershell-guide-full.md), nebo programově pomocí rozhraní REST API nebo jeden z hello knihovny klienta úložiště hello. 

<a id="outage"></a>
#### <a name="6-how-can-i-switch-toohello-secondary-region-if-there-is-an-outage-in-hello-primary-region"></a>6. Jak můžete přepnout toohello sekundární oblast, pokud dojde k výpadku v primární oblasti hello?
   
   Naleznete v článku toohello [co toodo, když dojde k výpadku Azure Storage](../storage-disaster-recovery-guidance.md) další podrobnosti.

<a id="rpo-rto"></a>
#### <a name="7-what-is-hello-rpo-and-rto-with-grs"></a>7. Co je hello plánovaný bod obnovení a RTO s GRS?
   
   Cíl bodu obnovení (RPO): V GRS a RA-GRS, hello úložiště služby asynchronně geograficky replikují hello data ze sekundárního umístění primární toohello hello. Pokud dojde k havárii hlavní místní a převzetí služeb při selhání má toobe provést, mohou být ztraceny poslední rozdílové změny, které nebyly geograficky replikované. Hello počet minut potenciální – ztráta dat je odkazované tooas hello RPO (což znamená, může být obnoven hello bod v časových toowhich dat). Obvykle máme plánovaný bod obnovení méně než 15 minut, i když je aktuálně trvá žádné SLA na jak dlouho se geografická replikace.

   Cíli času obnovení (RTO): Toto je míra jak dlouho trvá toodo hello převzetí služeb při selhání a získat účet úložiště hello zpět online Pokud bychom měli toodo převzetí služeb při selhání. převzetí služeb při selhání Hello čas toodo hello zahrnuje hello následující:
    * čas Hello přebírá nám tooinvestigate a určete, jestli jsme můžete obnovit data hello hello primární umístění, nebo pokud bychom někdy potřebovali toodo převzetí služeb při selhání.
    * Převzetí služeb při selhání hello účet změnou hello primární DNS položky toopoint toohello sekundárního umístění.

   Jsme trvat velmi vážně zachování dat tak, aby pokud je pravděpodobné, že obnovení dat hello, jsme zdržovat provádění hello převzetí služeb při selhání a zaměřit se na obnovení dat hello v primárním umístění hello zodpovědností hello. V budoucích hello, plánujeme tooallow tooprovide rozhraní API můžete tootrigger převzetí služeb při selhání na úrovni účtu, který pak umožní toocontrol hello RTO sami, ale to ještě není k dispozici.
   
## <a name="next-steps"></a>Další kroky
* [Návrh aplikace s vysokou dostupností pomocí RA-GRS úložiště](../storage-designing-ha-apps-with-ragrs.md)
* [Ceny za Azure Storage](https://azure.microsoft.com/pricing/details/storage/)
* [Informace o účtech úložiště Azure](../storage-create-storage-account.md)
* [Azure Storage škálovatelnosti a cílech výkonnosti](storage-scalability-targets.md)
* [Microsoft Azure Storage redundance možnosti a přístup pro čtení geograficky redundantní úložiště](http://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx)
* [STUDIE Sosp - Azure Storage: Vysoce dostupný cloudové úložiště služby se silnou konzistenci](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

