---
title: aaaUse data toocollect protokoly a metrik Azure Storage Analytics | Microsoft Docs
description: "Analytika úložiště vám umožní tootrack metriky dat pro všechny služby, úložiště a toocollect protokoly pro úložiště Blob, fronty a tabulky."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 7894993b-ca42-4125-8f17-8f6dfe3dca76
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: robinsh
ms.openlocfilehash: aba1a236cb69fa2e60bcb8fda5d34841e36e379a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="storage-analytics"></a>Storage Analytics

Analýza Azure Storage provádí protokolování a poskytuje data metriky pro účet úložiště. Můžete použít tento tootrace požadavky na data, analyzovat trendy využití a diagnostikovat problémy s vaším účtem úložiště.

toouse analytika úložiště, musíte povolit samostatně pro každou službu chcete toomonitor. Můžete ji povolit z hello [portálu Azure](https://portal.azure.com). Podrobnosti najdete v tématu [monitorovat účet úložiště na portálu Azure hello](storage-monitor-storage-account.md). Můžete také povolit Storage Analytics prostřednictvím kódu programu prostřednictvím hello REST API nebo knihovny klienta hello. Použití hello [získat vlastnosti objektu Blob služby](https://msdn.microsoft.com/library/hh452239.aspx), [získat vlastnosti fronty služby](https://msdn.microsoft.com/library/hh452243.aspx), [získat vlastnosti služby Table](https://msdn.microsoft.com/library/hh452238.aspx), a [získat File Service Properties](https://msdn.microsoft.com/library/mt427369.aspx) operations tooenable analytika úložiště pro každou službu.

Hello agregovaná data se ukládají v dobře známé objektu blob (pro protokolování) a dobře známé tabulky (pro metriky), které můžete získat přístup pomocí služby objektů Blob hello a služby Table rozhraní API.

Analytika úložiště může mít 20 TB na hello množství uložených dat, která je nezávislá hello celkový limit pro váš účet úložiště. Další informace o fakturaci a zásad uchovávání dat najdete v tématu [Storage Analytics a fakturace](https://msdn.microsoft.com/library/hh360997.aspx). Další informace o omezeních účtu úložiště najdete v tématu [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md).

Pro podrobné příručce pomocí analytika úložiště a dalších nástrojů tooidentify diagnostikovat a vyřešit problémy související s Azure Storage najdete v tématu [monitorování, Diagnostika a řešení Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="about-storage-analytics-logging"></a>O protokolování Storage Analytics
Úložiště analýzy protokolů podrobné informace o službě úložiště tooa úspěšné i neúspěšné požadavky. Tato informace může být použité toomonitor jednotlivých požadavků a toodiagnose problémy se službou úložiště. Na základě typu best effort protokolované požadavky.

Položky protokolu se vytvoří pouze v případě, že je aktivita služby úložiště. Například pokud je účet úložiště v jeho služby objektů Blob, ale není v jeho tabulku nebo frontu služby, bude vytvořena pouze protokoly, která se týkají toohello služby objektů Blob.

Není k dispozici pro úložiště Azure File Storage Analytics protokolování.

### <a name="logging-authenticated-requests"></a>Protokolování ověření požadavků
jsou zaznamenány následující typy požadavků na ověření Hello:

* Úspěšné požadavky.
* Neúspěšné požadavky, včetně vypršení časového limitu, omezení šířky pásma, sítě, autorizace a dalších chyb.
* Požadavky na použití sdíleného přístupového podpisu (SAS), včetně úspěšné a neúspěšné požadavky.
* Data tooanalytics požadavků.

Požadavky na úložiště Analytics samostatně, jako je například protokol vytvoření nebo odstranění, se neprotokolují. Úplný seznam hello protokolovat data je popsána v hello [stavové zprávy a Storage Analytics protokolované](https://msdn.microsoft.com/library/hh343260.aspx) a [úložiště analýzy protokolů formátu](https://msdn.microsoft.com/library/hh343259.aspx) témata.

### <a name="logging-anonymous-requests"></a>Protokolování anonymních požadavků
jsou zaznamenány Hello následující typy anonymních požadavků:

* Úspěšné požadavky.
* Chyby serveru.
* Chyby časového limitu pro klienta a serveru.
* Neúspěšné požadavky GET s kódem chyby 304 (upraveno).

Všechny ostatní selhání anonymních požadavků se neprotokolují. Úplný seznam hello protokolovat data je popsána v hello [stavové zprávy a Storage Analytics protokolované](https://msdn.microsoft.com/library/hh343260.aspx) a [úložiště analýzy protokolů formátu](https://msdn.microsoft.com/library/hh343259.aspx) témata.

### <a name="how-logs-are-stored"></a>Ukládání protokolů
Všechny protokoly se ukládají do objektů BLOB bloku v kontejneru nazvaném $logs, který se automaticky vytvoří, když je pro účet úložiště povolená analytika úložiště. Hello $logs kontejneru se nachází v oboru názvů objektů blob hello hello účtu úložiště, například: `http://<accountname>.blob.core.windows.net/$logs`. Tento kontejner nelze odstranit, jakmile Storage Analytics je povoleno, když jeho obsah může být odstraněny.

> [!NOTE]
> Hello $logs kontejneru se nezobrazí, pokud se provádí kontejner výpis operace, jako je například hello [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) metoda. Musí se přístup přímo. Například můžete použít hello [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) metoda tooaccess hello objektů BLOB v hello `$logs` kontejneru.
> Jako žádosti se protokolují, odešlete Storage Analytics mezilehlých výsledků jako bloky. Analytika úložiště bude pravidelně potvrdit tyto bloky a zpřístupnit jako objekt blob.
> 
> 

Pro protokoly, které jsou vytvořené v hello může existují duplicitní záznamy tutéž hodinu. Můžete určit, zda záznam je duplicitní kontrolou hello **Id_žádosti** a **operace** číslo.

### <a name="log-naming-conventions"></a>Přihlaste se zásady vytváření názvů
Každý protokol, bude napsán v hello formátu.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

Hello následující tabulka popisuje jednotlivé atributy v hello název protokolu.

| Atribut | Popis |
| --- | --- |
| < název služby > |Název Hello hello služby úložiště. Příklad: Objekt blob, tabulka nebo fronty. |
| RRRR |Hello čtyřmístný rok hello protokolu. Příklad: 2011. |
| MM |Hello měsíc pro protokol hello. Příklad: 07. |
| DD |Hello měsíc pro protokol hello. Příklad: 07. |
| hh |Hello letopočty hodiny, která určuje hello od hodinu hello protokoly ve formátu UTC 24 hodin. Příklad: 18. |
| mm |Hello dvoumístné číslo určující počáteční minutu pro protokoly hello hello. Tato hodnota není podporována v aktuální verzi Storage Analytics hello a jeho hodnota bude vždy 00. |
| <counter> |Počítaný od nuly čítač se šesti číslic určující hello počet objektů BLOB protokolu vygenerované hello úložiště služby za hodinu časové období. Tento čítač se spustí v 000000. Příklad: 000001. |

Hello následuje název protokolu ucelenou ukázku, která sloučí hello předchozích příkladech.

    blob/2011/07/31/1800/000001.log

Hello Následuje ukázka identifikátor URI, který lze použít tooaccess hello předchozím protokolu.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Při zaznamenání požadavku úložiště, výsledný název protokolu hello korelaci toohello hodinu při hello požadovaná operace byla dokončena. Například, pokud žádost o getblob – byla dokončena v 6:30 na 31/7/2011 by byla zapsána hello protokolu s hello následující předpony:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Metadata protokolu
Všechny objekty BLOB protokolu se ukládají s metadata, která lze použít tooidentify obsahuje objekt blob jaké protokolování dat hello. Hello následující tabulka popisuje každý atribut metadat.

| Atribut | Popis |
| --- | --- |
| LogType |Popisuje, zda text hello protokol obsahuje informace týkající se tooread zápisu, operace nebo odstranění. Tato hodnota může obsahovat jeden typ nebo kombinaci všechny tři, oddělených čárkami. Příklad 1: zápis; Příklad 2: Číst, zapisovat; Příklad 3: Číst, zapisovat, odstranit. |
| Čas spuštění |Hello denní čas, může záznam v protokolu hello hello tvar rrrr-MM-ddTHH. Příklad: 2011-07-31T18:21:46Z. |
| čas ukončení |Hello nejpozdější čas záznamu v protokolu hello hello tvar rrrr-MM-ddTHH. Příklad: 2011-07-31T18:22:09Z. |
| LogVersion |Hello verzi formátu protokolu hello. Aktuálně hello podporována pouze hodnota je 1.0. |

Hello následujícím seznamu zobrazí ucelenou ukázku metadat pomocí hello předchozích příkladech.

* LogType = zápisu
* StartTime = 2011-07-31T18:21:46Z
* Čas ukončení = 2011-07-31T18:22:09Z
* LogVersion = 1.0

### <a name="accessing-logging-data"></a>Přístup k datům protokolování
Všechna data v hello `$logs` kontejneru přístupná pomocí služby objektů Blob hello rozhraní API, včetně hello rozhraní API technologie .NET poskytované hello Azure spravovanou knihovnu. Správce účtu úložiště Hello může číst a odstranit protokoly, ale nelze vytvořit nebo aktualizovat je. Metadata hello protokolu a název protokolu lze použít při dotazování pro protokol. Je možné, tooappear protokoly zadané hodiny mimo pořadí, ale hello metadata vždy určuje časový interval hello položek protokolu v protokolu. Proto můžete použít kombinaci protokolu názvů a metadata při vyhledávání pro určitý protokol.

## <a name="about-storage-analytics-metrics"></a>O Storage Analytics metriky
Analytika úložiště můžete ukládat metriky, které zahrnují data statistiky a kapacity agregované transakcí týkající se požadavků tooa úložiště služby. Transakce jsou v úrovni operaci hello rozhraní API, a také na úrovni služby úložiště hello a hlásí kapacity na úrovni služby úložiště hello. Metriky dat můžete použít tooanalyze využití služby úložiště, diagnostikovat problémy s požadavků na službu úložiště hello a tooimprove hello výkon aplikací, které používají služby přístup.

toouse analytika úložiště, musíte povolit samostatně pro každou službu chcete toomonitor. Můžete ji povolit z hello [portálu Azure](https://portal.azure.com). Podrobnosti najdete v tématu [monitorovat účet úložiště na portálu Azure hello](storage-monitor-storage-account.md). Můžete také povolit Storage Analytics prostřednictvím kódu programu prostřednictvím hello REST API nebo knihovny klienta hello. Použití hello **získat vlastnosti služby** operations tooenable analytika úložiště pro každou službu.

### <a name="transaction-metrics"></a>Metriky transakce
Robustní sadu dat se zaznamenává v intervalech hodinových nebo minutu pro každou službu úložiště a požadovanou operaci rozhraní API, včetně vstupní/výstupní, dostupnosti, chyby a zařazené do kategorie procenta požadavku. Zobrazí seznam všech podrobností transakcí hello v hello [schématu tabulky metriky Analytics úložiště](https://msdn.microsoft.com/library/hh343264.aspx) tématu.

Data transakcí se zaznamenává na dvou úrovních – hello úroveň služby a hello API operaci. Na úrovni služby hello statistiky shrnutí všech požadovaného rozhraní API, zapíšou se operace entitu tabulky tooa každou hodinu i v případě, že žádné žádosti byly provedeny toohello služby. Na úrovni operaci hello rozhraní API statistiky se pouze napsané tooan entity, pokud byla vyžádána operace hello v rámci této hodinu.

Například, pokud provádíte **getblob –** operace služby Blob Storage Analytics Metrics bude protokolu hello žádost a její zahrnutí do hello agregovat data pro obě hello služby objektů Blob, jakož i hello **getblob –** operaci. Ale pokud žádné **getblob –** během hodiny hello je požadovaná operace, entity nebudou zapsána toohello `$MetricsTransactionsBlob` pro tuto operaci.

Metriky transakce se zaznamenávají pro uživatelské požadavky a požadavky na úložiště Analytics sám sebe. Například se zaznamenávají požadavky Storage Analytics toowrite protokoly a entity tabulky. Další informace o tom, jak se účtují tyto požadavky najdete v tématu [Storage Analytics a fakturace](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>Metriky kapacity
> [!NOTE]
> V současné době metriky kapacity jsou k dispozici pouze pro hello služby objektů Blob. Metriky kapacity pro hello tabulky a fronty služba bude k dispozici v budoucích verzích analytika úložiště.
> 
> 

Kapacity dat se zaznamenává každý den pro účet úložiště služby objektů Blob a se zapisují dvě entity tabulky. Jedna entita obsahuje statistiku pro data uživatelů a hello jiných poskytuje Statistika hello `$logs` kontejner objektů blob Storage Analytics používá. Hello `$MetricsCapacityBlob` tabulka obsahuje hello následující statistiky:

* **Kapacita**: hello velikost úložiště, které používá služby objektů Blob v účtu úložiště hello v bajtech.
* **ContainerCount**: hello počet kontejnerů objektů blob v účtu úložiště hello služby objektů Blob.
* **ObjectCount**: hello počet potvrzení a nepotvrzené stránka bloku nebo objektů BLOB v účtu úložiště hello služby objektů Blob.

Další informace o hello metriky kapacity najdete v tématu [schématu tabulky metriky Analytics úložiště](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Ukládání metriky
Veškerá data metriky pro jednotlivé služby úložiště hello je uložena v tři tabulky, které jsou vyhrazené pro tuto službu: jednu tabulku pro informací o transakcích, jednu tabulku pro informace o minutu transakce a jinou tabulkou pro informací o kapacitě. Transakce a minutu informací o transakcích se skládá z požadavku a odpovědi dat a informací o kapacitě se skládá z dat využití úložiště. Metriky hodinu, minutu metriky a kapacity pro účet úložiště služby objektů Blob je přístupná v tabulkách, které byly pojmenovány, jak je popsáno v následující tabulce hello.

| Metriky úrovně | Názvy tabulek | Podporované verze |
| --- | --- | --- |
| Hodinové metriky, primární umístění |$MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue |Verze předchozí too2013-08-15 jenom. Když tyto názvy jsou stále podporovány, je doporučeno přepnutí níže uvedené tabulky toousing hello. |
| Hodinové metriky, primární umístění |$MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue |Všechny verze, včetně 2013-08-15. |
| Minutu metriky, primární umístění |$MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue |Všechny verze, včetně 2013-08-15. |
| Hodinové metriky, sekundárních umístění |$MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue |Všechny verze, včetně 2013-08-15. Musí být povolen přístup pro čtení geo redundantní replikaci. |
| Minutu metriky, sekundárních umístění |$MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue |Všechny verze, včetně 2013-08-15. Musí být povolen přístup pro čtení geo redundantní replikaci. |
| Kapacita (pouze služby objektů Blob) |$MetricsCapacityBlob |Všechny verze, včetně 2013-08-15. |

Tyto tabulky se automaticky vytvoří, když je pro účet úložiště povolená analytika úložiště. Jsou přístupná prostřednictvím oboru názvů hello hello účtu úložiště, například:`https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Přístup k datům metriky
Všechna data v tabulkách metriky hello je přístupná pomocí služby Table hello rozhraní API, včetně hello rozhraní API technologie .NET poskytované hello Azure spravovanou knihovnu. Správce účtu úložiště Hello může číst a odstranit tabulku entity, ale nelze vytvořit nebo aktualizovat je.

## <a name="billing-for-storage-analytics"></a>Fakturace analytika úložiště
Všechna data metriky se zapisují službami hello účtu úložiště. V důsledku toho je fakturovatelný každou operaci zápisu provádí analytika úložiště. Hello množství úložiště používané metriky dat je navíc také fakturovatelného času.

Hello následující akce prováděné Storage Analytics jsou fakturovatelné:

* Objekty BLOB toocreate požadavky pro protokolování. 
* Požadavky toocreate tabulka entity pro metriky.

Pokud jste nakonfigurovali zásadu uchovávání dat, se vám neúčtují poplatky pro odstranění transakce když Storage Analytics odstraní stará data protokolování a metriky. Odstranění transakce z klienta jsou však fakturovatelného času. Další informace o zásady uchovávání informací najdete v tématu [nastavení zásad uchovávání dat úložiště Analytics](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Pochopení požadavků fakturovatelného času
Každý požadavek tooan účet úložiště služby je fakturovatelný nebo Nefakturovatelný. Úložiště analýzy protokolů každé jednotlivé požadavek tooa služby, včetně stavovou zprávu, která určuje, jak počet zpracovaných požadavků hello. Podobně analytika úložiště ukládá metriky pro operace rozhraní API služby i hello této služby, včetně hello procenta a počet určitých stavových zpráv. Společně tyto funkce vám může pomoct analyzovat fakturovatelný žádostí, zlepšení na vaší aplikace a diagnostikovat problémy s požadavky tooyour služby. Další informace o fakturaci, viz [Principy Azure úložiště fakturace - šířku pásma, transakce a kapacity](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Při prohlížení analytika úložiště dat, můžete použít hello tabulek v hello [stavové zprávy a Storage Analytics protokolované](https://msdn.microsoft.com/library/azure/hh343260.aspx) co požadavky jsou fakturovatelné toodetermine tématu. Pak můžete porovnat protokoly a metriky dat toohello stavové zprávy toosee případě, že byly účtovány pro konkrétní žádost. Můžete taky hello tabulek v hello předchozí téma tooinvestigate dostupnosti pro službu úložiště nebo jednotlivé operace rozhraní API.

## <a name="next-steps"></a>Další kroky
### <a name="setting-up-storage-analytics"></a>Nastavení služby Storage Analytics
* [Monitorování účtu úložiště v hello portálu Azure](storage-monitor-storage-account.md)
* [Povolení a konfigurace úložiště analýzy](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Analýza protokolování úložiště
* [O protokolování Analytics úložiště](https://msdn.microsoft.com/library/hh343262.aspx)
* [Formát úložiště analýzy protokolů](https://msdn.microsoft.com/library/hh343259.aspx)
* [Analytika úložiště protokolovat stavové zprávy a operace](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Metriky Analytics úložiště
* [O metriky Analytics úložiště](https://msdn.microsoft.com/library/hh343258.aspx)
* [Schéma tabulky metriky Analytics úložiště](https://msdn.microsoft.com/library/hh343264.aspx)
* [Analytika úložiště protokolovat stavové zprávy a operace](https://msdn.microsoft.com/library/hh343260.aspx)  

