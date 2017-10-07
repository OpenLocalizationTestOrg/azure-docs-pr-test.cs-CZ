---
title: "aaaMonitor, diagnostiku a řešení potíží s Azure Storage | Microsoft Docs"
description: "Použití funkce, jako jsou úložiště analýzu straně klienta protokolování, a další nástroje jiných výrobců tooidentify diagnostikovat a vyřešit problémy související s Azure Storage."
services: storage
documentationcenter: 
author: fhryo-msft
manager: jahogg
editor: tysonn
ms.assetid: d1e87d98-c763-4caa-ba20-2cf85f853303
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/11/2017
ms.author: fhryo-msft
ms.openlocfilehash: 43f34a4ccbc3071cdb489958252498a1f88e1a34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-diagnose-and-troubleshoot-microsoft-azure-storage"></a>Monitorování, diagnostika a řešení problémů s Microsoft Azure Storage
[!INCLUDE [storage-selector-portal-monitoring-diagnosing-troubleshooting](../../../includes/storage-selector-portal-monitoring-diagnosing-troubleshooting.md)]

## <a name="overview"></a>Přehled
Diagnostika a řešení potíží v distribuované aplikace hostované v cloudovém prostředí může být složitější než v tradiční prostředích. V infrastruktuře PaaS nebo IaaS místně na mobilním zařízení, nebo určitou kombinaci těchto mohou být aplikace nasazeny. Obvykle síťový provoz vaší aplikace můžou procházet veřejné a privátní sítě a vaše aplikace může používat více technologií úložiště, jako je například Microsoft Azure úložiště tabulek, objekty BLOB, fronty nebo soubory v přidání tooother data ukládá, například jako relační a dokumentové databáze.

toomanage takové aplikace úspěšně měli byste je aktivně sledovat a pochopit, jak toodiagnose a řešení potíží s všechny aspekty je a jejich závislé technologie. Jako uživatel služby Azure Storage, doporučujeme nepřetržitě sledování služeb úložiště hello vaše aplikace používá pro všechny neočekávané změny v chování (například nižší než obvyklé odezvy) a použít protokolování toocollect podrobnější data a tooanalyze problém podrobněji. pomůže vám toodetermine hello hlavní příčinu hello vydání aplikace došlo k Hello diagnostické informace, které můžete získat z sledování a protokolování. Pak můžete vyřešit problém hello a určení příslušné kroky hello může trvat tooremediate ho. Úložiště Azure je základní služby Azure a je důležitou součástí hello většina řešení, aby zákazníci nasadit toohello infrastruktury Azure. Úložiště Azure zahrnuje možnosti toosimplify monitorování, Diagnostika a řešení potíží problémů s úložištěm ve vaší cloudové aplikace.

> [!NOTE]
> Hello Azure File storage protokolování v tuto chvíli nepodporuje.
> 

Praktických Průvodce tooend až do konce odstraňování potíží v aplikacích Azure Storage, najdete v části [na kompletní řešení problémů pomocí Azure Storage Metrics a protokolování, AzCopy a Message Analyzer](../storage-e2e-troubleshooting.md).

* [Úvod]
  * [Uspořádání Tato příručka]
* [monitorování vaší služby úložiště]
  * [Monitorování stavu služby]
  * [Monitorování kapacity]
  * [Monitorování dostupnosti]
  * [Sledování výkonu]
* [diagnostice problémů s úložištěm]
  * [Problémy v oblasti služby stavu]
  * [Problémy s výkonem]
  * [Diagnostikování chyb]
  * [Emulátor problémů s úložištěm]
  * [Nástroje protokolování úložiště]
  * [Pomocí nástroje protokolování]
* [začátku do konce trasování]
  * [Korelace data protokolu]
  * [ID žádosti klienta]
  * [ID žádosti serveru]
  * [Časová razítka]
* [pokyny při řešení potíží]
  * [metriky ukazují AverageE2ELatency vysoké a nízké AverageServerLatency]
  * [Metriky ukazují nízkou AverageE2ELatency a nízkou AverageServerLatency ale dochází k vysoké latenci hello klienta]
  * [Metrika ukazuje vysokou hodnotu AverageServerLatency]
  * [Dochází k neočekávaným zpožděním při doručování zpráv ve frontě]
  * [metriky způsobit nárůst PercentThrottlingError]
  * [metriky způsobit nárůst PercentTimeoutError]
  * [Metrika ukazuje zvýšení u PercentNetworkError]
  * [Klient Hello přijímá zprávy HTTP 403 (zakázáno)]
  * [Klient Hello přijímá zprávy HTTP 404 (není nalezena)]
  * [Hello klienta je přijímání zpráv protokolu HTTP 409 (konflikt)]
  * [metriky ukazují nízkou PercentSuccess nebo položky protokolu analýzy mít operací s Stav transakce ClientOtherErrors]
  * [Metriky kapacity způsobit neočekávané nárůst využití kapacity úložiště]
  * [Dochází k neočekávané restartování virtuálních počítačů, které mají velký počet připojených virtuálních pevných disků]
  * [Problém vyplývá z pomocí emulátoru úložiště hello pro vývoj nebo testování]
  * [Narazíte na potíže s instalací hello Azure SDK pro .NET]
  * [Máte jiný problém se službou úložiště]
  * [Řešení potíží s Azure File storage s Windows](../files/storage-troubleshoot-windows-file-connection-problems.md)   
  * [Řešení potíží s Azure File storage s Linuxem](../files/storage-troubleshoot-linux-file-connection-problems.md)
* [přílohy]
  * [dodatku 1: použití Fiddler toocapture HTTP a HTTPS provozy]
  * [příloze 2: pomocí Wireshark toocapture síťový provoz]
  * [příloha 3: použití Microsoft Message Analyzer toocapture síťový provoz]
  * [Dodatek 4: Použití tooview metriky a protokolu data z Excelu.]
  * [Dodatek 5: Monitorování pomocí služby Application Insights pro Visual Studio Team Services]

## <a name="introduction"></a>Úvod
Tato příručka obsahuje můžete jak toouse funkce, například Azure Storage Analytics, protokolování na straně klienta v hello Klientská knihovna pro úložiště Azure a další nástroje jiných výrobců tooidentify diagnostice a řešení potíží s Azure Storage související s problémy.

![][1]

Tento průvodce je určený toobe číst především vývojáři online služeb, které používají služby úložiště Azure a IT Profesionálové zodpovědní za správu těchto online služeb. jsou Hello cílem této příručky:

* toohelp udržovat dobrý stav a výkon účty úložiště Azure.
* tooprovide vám hello nezbytné procesy a nástroje pro toohelp rozhodnete, že pokud se problém nebo potíže v aplikaci vztahem tooAzure úložiště.
* tooprovide vám řešitelné pokyny pro řešení problémů souvisejících s tooAzure úložiště.

### <a name="how-this-guide-is-organized"></a>Uspořádání Tato příručka
Hello část "[monitorování vaší služby úložiště]" popisuje, jak toomonitor hello stav a výkon vašich služeb Azure Storage pomocí Azure Storage Analytics Metrics (Storage Metrics).

Hello část "[diagnostice problémů s úložištěm]" popisuje, jak toodiagnose problémy pomocí Storage Analytics protokolování v Azure (úložiště protokolování). Také popisuje, jak pomocí klienta protokolování tooenable hello zařízení v jedné z knihoven klienta hello například hello Klientská knihovna pro úložiště pro .NET nebo hello Azure SDK pro jazyk Java.

Hello část "[začátku do konce trasování]" popisuje, jak mohou korelovat hello informace obsažené v různých metriky, dat a souborů protokolu.

Hello část "[pokyny při řešení potíží]" poskytuje pokyny k odstraňování potíží pro některé hello běžné úložiště problémy související s můžete setkat.

Hello "[přílohy]" obsahují informace o použití jiných nástrojů, například Wireshark nebo Netmon pro analýzu síťových paketů data, Fiddler pro analýzu zprávy HTTP/HTTPS, a Microsoft Message Analyzer pro korelace protokolovat data.

## <a name="monitoring-your-storage-service"></a>Monitorování služby úložiště
Pokud jste obeznámeni s sledování výkonu systému Windows, si můžete představit metriky úložiště jako ekvivalentní čítačů sledování výkonu systému Windows Azure Storage. Ve Storage Metrics zjistíte komplexní sadu metriky (čítače v terminologii sledování výkonu systému Windows), například dostupnost služby, celkový počet požadavků tooservice nebo procento tooservice úspěšné požadavky. Úplný seznam hello dostupné metriky, najdete v části [schématu tabulky metriky Analytics úložiště](http://msdn.microsoft.com/library/azure/hh343264.aspx). Můžete zadat, zda chcete hello úložiště služby toocollect a agregovaná metrika každou hodinu nebo každou minutu. Další informace o tom, jak tooenable metriky a monitorování účtů úložiště, najdete v části [zapnutí metrik úložiště a prohlížení dat metrik](http://go.microsoft.com/fwlink/?LinkId=510865).

Můžete zvolit, které hodinové metriky chcete toodisplay v hello [portál Azure](https://portal.azure.com) a nakonfigurovat pravidla, která upozorní správce e-mailem, kdykoli po hodinách metrika překračuje prahovou hodnotu konkrétní. Další informace najdete v tématu [dostávat oznámení o výstrahách](/azure/monitoring-and-diagnostics/monitoring-overview-alerts.md). 

Služba úložiště Hello shromažďuje metriky pomocí nejlepší úsilí, ale nemusí záznam každé operace úložiště.

Hello portálu Azure uvidíte metriky, například dostupnost, celkový počet požadavků a čísla Průměrná latence pro účet úložiště. Pravidla oznámení je rovněž nastaven tooalert správce Pokud dostupnosti klesne pod určitou úroveň. Zobrazit tato data, je jednou z možných oblastí pro zkoumání hello tabulky služby procento úspěšnosti je nižší, než 100 % (Další informace najdete v tématu hello "[metriky ukazují nízkou PercentSuccess nebo položky protokolu analýzy mít operací s Stav transakce ClientOtherErrors]").

Měli byste sledovat nepřetržitě tooensure vaše aplikace Azure jsou v pořádku a provádění podle očekávání tím:

* Zřízení některé základní metriky pro aplikaci, které bude umožňují toocompare aktuální data a identifikovat veškeré významné změny v chování hello úložiště Azure a aplikace. v mnoha případech bude Hello hodnoty vaše základní metriky konkrétní aplikace a je měli vytvořit, když jsou výkonu testování vaší aplikace.
* Záznam minutu metriky a jejich používání toomonitor aktivně pro neočekávaným chybám a anomálií, jako je například špičky v chybě počty nebo požadavků.
* Záznam hodinové metriky a jejich používání toomonitor průměrné hodnoty jako například průměrný počet chyb a požadovat sazby.
* Na odstranění příčin možných problémů pomocí diagnostiky nástrojů, jak je popsáno v části hello později "[diagnostice problémů s úložištěm]."

grafy Hello v hello následující obrázek znázorňují, jak hello průměrování, ke kterému dochází po hodinách metrik, které můžete skrýt špičky v aktivitě. Hello hodinové metrika se zobrazí tooshow konstantní počet požadavků, při hello minutu metriky odhalit hello kolísání, které skutečně dala.

![][3]

Hello zbytek této části popisuje, jaké metriky, měli byste sledovat a proč.

### <a name="monitoring-service-health"></a>Monitorování stavu služby
Můžete použít hello [portál Azure](https://portal.azure.com) hello tooview hello stav služby úložiště hello (a jinými službami Azure) ve všech oblastech Azure kolem hello, world. To vám umožní toosee okamžitě Pokud problém mimo váš dosah ovlivňuje hello služby úložiště v hello oblasti, které používáte pro vaši aplikaci.

Hello [portál Azure](https://portal.azure.com) zadat také oznámení incidentů, které ovlivňují hello různé služby Azure.
Poznámka: Tyto informace byl dříve k dispozici společně s historická data na hello [řídicího panelu služby Azure](http://status.azure.com).

Při hello [portál Azure](https://portal.azure.com) shromažďuje informace o stavu z uvnitř hello datových centrech Azure (zevnitř monitorování), může taky zvážit, který pravidelně přijetí syntetické transakce přístup mimo v toogenerate přístup k Azure hostovaná webové aplikace z více umístění. Hello službách nabízených [Dynatrace](http://www.dynatrace.com/en/synthetic-monitoring) a Application Insights pro Visual Studio Team Services jsou příklady tohoto mimo přístupu. Další informace o Application Insights pro Visual Studio Team Services, naleznete v dodatku hello "[příloha 5: monitorování pomocí Application Insights pro Visual Studio Team Services](#appendix-5)."

### <a name="monitoring-capacity"></a>Monitorování kapacity
Metriky úložiště ukládá metriky kapacity pro služby objektů blob hello jenom, protože objekty BLOB obvykle účet hello největší část uložená data (v době hello zápis, není možné toouse Storage Metrics toomonitor hello kapacity tabulek a front) . Tato data můžete najít v hello **$MetricsCapacityBlob** tabulky, pokud jste povolili monitorování hello služby objektů Blob. Metriky úložiště zaznamenává tato data jednou za den, a můžete použít hodnotu hello hello **RowKey** toodetermine jestli hello řádek obsahuje entity, která má vztah toouser dat (hodnota **data**) nebo analýzy dat ( Hodnota **analytics**). Každá entita uložené obsahuje informace o hello množství úložiště používané (**kapacity** měřená v bajtech) a aktuální počet kontejnerů hello (**ContainerCount**) a objekty BLOB ( **ObjectCount**) používá v účtu úložiště hello. Další informace o metriky kapacity hello uložené v hello **$MetricsCapacityBlob** tabulky najdete v tématu [schématu tabulky metriky Analytics úložiště](http://msdn.microsoft.com/library/azure/hh343264.aspx).

> [!NOTE]
> Měli byste sledovat tyto hodnoty včasného varování, že se blíží hello limity kapacity účtu úložiště. V hello portálu Azure můžete přidat toonotify pravidla výstrah, které pokud použití agregační úložiště překračuje nebo nedosahuje prahové hodnoty, které určíte.
> 
> 

Odhad velikosti hello různých objektů úložiště, jako je například objekty BLOB nápovědu najdete v příspěvku blogu hello [Principy Azure úložiště fakturace – šířku pásma, transakce a kapacity](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

### <a name="monitoring-availability"></a>Monitorování dostupnosti
Měli byste sledovat hello dostupnost služeb úložiště hello ve vašem účtu úložiště podle hello hodnota v hello monitorování **dostupnosti** sloupec v hello hodinových nebo minutu metriky tabulky – **$ MetricsHourPrimaryTransactionsBlob**, **$MetricsHourPrimaryTransactionsTable**, **$MetricsHourPrimaryTransactionsQueue**, **$ MetricsMinutePrimaryTransactionsBlob**, **$MetricsMinutePrimaryTransactionsTable**, **$MetricsMinutePrimaryTransactionsQueue**, **$ MetricsCapacityBlob**. Hello **dostupnosti** sloupec obsahuje procentuální hodnotu, která určuje hello dostupnost služby hello nebo operaci hello API reprezentována hello řádek (hello **RowKey** zobrazí, pokud obsahuje řádek hello metriky pro službu hello jako celek nebo pro určité operace rozhraní API).

Všechny hodnoty a menší než 100 % označuje, že dochází k selhání některých požadavků úložiště. Zobrazí se, proč se nedaří prověřením hello ostatních sloupců v hello metriky data, která ukazují hello počet požadavků s typy jiné chyby, jako **ServerTimeoutError**. Byste měli očekávat toosee **dostupnosti** patří dočasně nižší než 100 % z důvodů, jako je například časové limity přechodný serveru při hello služby přesune oddíly toobetter Vyrovnávání zatížení požadavek; hello opakujte logiku v klientské aplikace by měla řídit tyto přerušované podmínky. článek Hello [stavové zprávy a Storage Analytics protokolované](http://msdn.microsoft.com/library/azure/hh343260.aspx) seznamy hello typy transakcí, které obsahuje úložiště metriky v jeho **dostupnosti** výpočtu.

V hello [portál Azure](https://portal.azure.com), můžete přidat pravidla výstrah toonotify jste Pokud **dostupnosti** pro službu klesne pod prahovou hodnotou, který určíte.

Hello "[pokyny při řešení potíží]" část tato příručka popisuje některé běžné úložiště služby problémy související tooavailability.

### <a name="monitoring-performance"></a>Sledování výkonu
toomonitor hello výkonu služby hello úložiště, můžete použít hello následujících metriky z hello každou hodinu a minutu metriky tabulky.

* Hello hodnoty v hello **AverageE2ELatency** a **AverageServerLatency** sloupcích se zobrazují hello Průměrná doba hello úložiště služby nebo typ operace rozhraní API trvá tooprocess požadavky. **AverageE2ELatency** je míra začátku do konce latence zahrnující hello doba tooread hello požadavku a odeslat hello odpovědi v žádosti o přidání toohello doba tooprocess hello (proto zahrnuje latence sítě po požadavku hello dosáhne služba úložiště hello); **AverageServerLatency** se rozumí míra právě hello doba zpracování a proto nezahrnuje žádné latence sítě související toocommunicating s klientem hello. Části hello "[metriky ukazují AverageE2ELatency vysoké a nízké AverageServerLatency]" dál v této příručce diskuzi o důvod, proč může být velký rozdíl mezi tyto dvě hodnoty.
* Hello hodnoty v hello **TotalIngress** a **TotalEgress** sloupcích se zobrazují hello celkové množství dat, v bajtech brzo tooand přejdete mimo vaší služby úložiště nebo konkrétní typ operace rozhraní API.
* Hello hodnoty v hello **TotalRequests** přijímá sloupec zobrazit hello celkový počet požadavků, které hello služba úložiště operace rozhraní API. **TotalRequests** je celkový počet požadavků, které obdrží služba úložiště hello hello.

Budete obvykle monitorovat neočekávané změny v některém z těchto hodnot jako indikátoru, abyste měli problém, který vyžaduje šetření.

V hello [portál Azure](https://portal.azure.com), můžete přidat pravidla výstrah toonotify vám, zda text hello metrik výkonu pro tuto službu klesnou pod nebo překročí prahovou hodnotu, která zadáte.

Hello "[pokyny při řešení potíží]" část tato příručka popisuje některé běžné úložiště služby problémy související tooperformance.

## <a name="diagnosing-storage-issues"></a>Diagnostika problémů s úložištěm
Existuje několik způsobů, že jste může se dozvěděli o problém nebo problém v aplikaci, patří:

* Hlavní selhání, které způsobí, že aplikace hello toocrash nebo toostop práci.
* Významné změny z hodnoty standardních hodnot v hello metriky, které monitorujete, jak je popsáno v předchozí části hello "[monitorování vaší služby úložiště]."
* Sestavy z uživatelů vaší aplikace, která nějaké konkrétní operace nebyla dokončena, podle očekávání nebo že některé funkce nefunguje.
* Chyby generované v rámci vaší aplikace, který zobrazí v souborech protokolů nebo prostřednictvím jiné metody oznámení.

Problémy související tooAzure úložiště služby obvykle spadají do jedné ze čtyř kategorií:

* Aplikace má problém výkonu, buď hlášené vaši uživatelé nebo zjištěná změny v metriky výkonu hello.
* Došlo k potížím s infrastrukturou Azure Storage hello v jedné nebo více oblastech.
* Aplikace je zjištění stavu chyby, a to buď hlášené vaši uživatelé nebo zjištěná o zvýšení v jednom z hello chyba počet metriky, které sledujete.
* Během vývoj a testování může být pomocí emulátoru místního úložiště hello; můžete některé problémy, které se týkají konkrétně toousage emulátor úložiště hello narazit.

Hello následující oddíly popisují kroky hello měli postupovat podle toodiagnose a vyřešit problémy v každé z těchto čtyř kategorií. Hello část "[pokyny při řešení potíží]" dál v této příručce poskytující víc podrobností pro některé běžné problémy, se můžete setkat.

### <a name="service-health-issues"></a>Problémy v oblasti služby stavu
Problémům se stavem služby jsou obvykle mimo vlastního ovládacího prvku. Hello [portál Azure](https://portal.azure.com) poskytuje informace o probíhající problémy se službami Azure včetně služby úložiště. Když jste se rozhodli pro geograficky redundantní úložiště s přístupem pro čtení při vytvoření účtu úložiště, pak v případě hello vašich dat je k dispozici v primárním umístění hello, vaše aplikace může dočasně přepnout toohello kopie jen pro čtení v hello sekundárního umístění. toodo, aplikace musí být schopný tooswitch mezi pomocí hello úložiště primární a sekundární umístění a být schopný toowork v režimu omezené funkčnosti daty jen pro čtení. knihovny klienta úložiště Azure Hello povolit toodefine zásady opakovaných pokusů, který může číst ze sekundární úložiště v případě, že pro čtení z primárního úložiště se nezdaří. Aplikace také musí toobe hello data v sekundárního umístění hello je, že nakonec byl konzistentní. Další informace najdete v tématu hello blogu [možnosti redundance úložiště Azure a geograficky redundantní úložiště s přístupem pro čtení](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

### <a name="performance-issues"></a>Problémy s výkonem
Hello výkonu aplikace, může být subjektivní, zejména z hlediska uživatelů. Proto je důležité toohave základní metriky dostupné toohelp identifikujete tam, kde může být problém s výkonem. Mnoha faktorech může ovlivnit výkon služby Azure storage z perspektivy klienta aplikace hello hello. Tyto faktory mohou pracovat v úložišti služby hello, hello klienta nebo hello síťovou infrastrukturu; je proto důležité toohave strategie pro identifikaci hello původu hello týkající se výkonu.

Po zjištění hello pravděpodobně umístění hello příčina problému výkonu hello z hello metriky, můžete použít hello protokolu soubory toofind podrobné informace o toodiagnose a řešení problému hello Další.

Hello část "[pokyny při řešení potíží]" dál v této příručce poskytuje další informace o některých běžných výkon související problémy, můžete setkat.

### <a name="diagnosing-errors"></a>Diagnostikování chyb
Uživatelům vaší aplikace může upozorňovat na chyby oznámené službou hello klientské aplikace. Úložiště metriky taky zaznamenává počty typů různé chyby z vaší služby úložiště, jako **NetworkError**, **ClientTimeoutError**, nebo **AuthorizationError**. Zatímco úložiště metriky pouze zaznamenává počty typů různých chyb, můžete získat více podrobností o jednotlivých požadavků prověřením na straně serveru, klienta a protokoly sítě. Stavový kód HTTP hello vrácený hello úložiště služby obvykle získáte údajem o proč hello požadavek se nezdařil.

> [!NOTE]
> Mějte na paměti, že byste měli očekávat toosee chybami přerušované: například chyby kvůli stavu sítě tootransient nebo chyby aplikace.
> 
> 

Hello následující prostředky jsou užitečné pro pochopení související s úložištěm stavu a chybové kódy:

* [Běžné kódy chyb rozhraní API REST](http://msdn.microsoft.com/library/azure/dd179357.aspx)
* [Kódy chyb služby objektů BLOB](http://msdn.microsoft.com/library/azure/dd179439.aspx)
* [Kódy chyb služby fronty](http://msdn.microsoft.com/library/azure/dd179446.aspx)
* [Kódy chyb služby Table](http://msdn.microsoft.com/library/azure/dd179438.aspx)
* [Kódy chyb služby souboru](https://msdn.microsoft.com/library/azure/dn690119.aspx)

### <a name="storage-emulator-issues"></a>Emulátor problémů s úložištěm
Hello Azure SDK zahrnuje emulátor úložiště, můžete spouštět na pracovní stanici. Tato emulátoru simuluje většinu hello chování služby Azure storage hello a jsou užitečné při vývoj a testování, umožňuje toorun aplikace, které používají služby Azure storage bez hello potřebovat pro předplatné Azure a účet úložiště Azure.

Hello "[pokyny při řešení potíží]" část tato příručka popisuje některé běžné problémy došlo pomocí emulátoru úložiště hello.

### <a name="storage-logging-tools"></a>Nástroje protokolování úložiště
Protokolování úložiště poskytuje serverové protokolování požadavků na úložiště v účtu úložiště Azure. Další informace o tom, jak tooenable serverové protokolování a přístup hello protokolovat data najdete v tématu [povolení protokolování úložiště a přístup k datům protokolu](http://go.microsoft.com/fwlink/?LinkId=510867).

Hello Klientská knihovna pro úložiště pro .NET vám umožní toocollect data protokolu na straně klienta, která má vztah toostorage operací prováděných v aplikaci. Další informace najdete v tématu [klienta protokolování s hello Klientská knihovna pro .NET úložiště](http://go.microsoft.com/fwlink/?LinkId=510868).

> [!NOTE]
> V některých případech (například selháním autorizace SAS) může uživatel sestavy k chybě, pro které můžete najít data žádosti nejsou v hello protokol úložiště na straně serveru. Můžete použít možnosti protokolování hello tooinvestigate hello Klientská knihovna pro úložiště, pokud hello příčinu problému hello je v klientovi hello nebo pomocí sítě hello tooinvestigate nástroje monitorování sítě.
> 
> 

### <a name="using-network-logging-tools"></a>Pomocí nástroje protokolování
Můžete zaznamenávat provoz hello mezi klientem a serverem tooprovide hello podrobné informace o hello data hello klienta a serveru jsou výměna a hello základní síťové podmínky. Nástroje protokolování užitečné sítě, patří:

* [Fiddler](http://www.telerik.com/fiddler) je bezplatných webových ladění proxy server, který vám umožní tooexamine hello hlavičky a datové části zpráv žádostí a odpovědí HTTP a HTTPS. Další informace najdete v tématu [dodatku 1: použití Fiddler toocapture HTTP a HTTPS provozy](#appendix-1).
* [Sledování sítě (Netmon)](http://www.microsoft.com/download/details.aspx?id=4865) a [Wireshark](http://www.wireshark.org/) jsou volné sítí analyzátorů protokolu, které umožňují tooview podrobné informace o paketů pro širokou škálu protokolů síťové. Další informace o Wireshark najdete v tématu "[příloze 2: pomocí Wireshark toocapture síťový provoz](#appendix-2)".
* Microsoft Message Analyzer je nástroj od společnosti Microsoft, která nahrazuje sledování sítě a že kromě toocapturing síťový paket dat, vám pomůže tooview a analyzovat data protokolu hello zaznamenaná z dalších nástrojů. Další informace najdete v tématu "[příloha 3: použití Microsoft Message Analyzer toocapture síťový provoz](#appendix-3)".
* Pokud chcete tooperform toocheck test základní připojení, že klientský počítač může připojit toohello služba úložiště Azure přes síť hello, nelze to provést pomocí standardní hello **ping** nástroje v klientovi hello. Můžete však použít hello [ **tcping** nástroj](http://www.elifulkerson.com/projects/tcping.php) toocheck připojení.

V mnoha případech hello dat protokolu z úložiště protokolování a hello Klientská knihovna pro úložiště bude dostatečná toodiagnose problém, ale v některých scénářích musíte hello podrobnější informace, které může poskytnout tyto nástroje protokolování. Například pomocí Fiddler tooview HTTP a HTTPS zpráv umožňuje tooview hlavičky a datové datech odeslaný tooand hello služby úložiště, které by vám tooexamine jak klientskou aplikaci opakování operace úložiště. Protokol analyzátorů například Wireshark fungovat na úrovni paketů hello umožňuje tooview TCP data, která by mohla tootroubleshoot ke ztrátě paketů a problémy s připojením. Message Analyzer může fungovat na protokolu HTTP a TCP vrstvy.

## <a name="end-to-end-tracing"></a>Konec Konec trasování
Trasování začátku do konce pomocí různých souborů protokolu je užitečné pro na odstranění příčin možných problémů. Hello datum a čas informace z vašich dat metriky slouží jako ukazatel kde toostart hledání v souborech protokolu hello hello podrobné informace, které vám pomohou vyřešit problém hello.

### <a name="correlating-log-data"></a>Korelace data protokolu
Při prohlížení protokolů z klientských aplikací, trasování sítě a úložiště na straně serveru ji protokolování je důležité toobe možné toocorrelate požadavky v rámci hello různých protokolových souborech. soubory protokolu Hello zahrnují několik různých polí, které lze použít jako identifikátory korelace. id žádosti klienta Hello je hello nejužitečnější pole toouse toocorrelate položek v různé protokoly hello. Může se stát, může být užitečné toouse id požadavku server hello nebo časová razítka. Hello následující části obsahují další podrobnosti o těchto možnostech.

### <a name="client-request-id"></a>ID žádosti klienta
Hello Klientská knihovna pro úložiště automaticky vygeneruje id žádosti klienta jedinečný pro každý požadavek.

* V hello vytvoří protokolu na straně klienta, který hello Klientská knihovna pro úložiště, id žádosti klienta hello se zobrazí v hello **ID žádosti klienta** pole každé položky protokolu týkající se toohello požadavku.
* Trasování sítě, například ten, který zachycenou aplikaci Fiddler, id žádosti klienta hello je zobrazen v žádosti o zprávy jako hello **x-ms-client-request-id** hodnotu hlavičky protokolu HTTP.
* V protokolu protokolování úložiště hello serverové id žádosti klienta hello zobrazí ve sloupci ID žádosti klienta hello.

> [!NOTE]
> Je možné pro více požadavků tooshare hello stejné id žádosti klienta, protože klient hello se může přiřadit tuto hodnotu (i když hello Klientská knihovna pro úložiště automaticky přiřadí novou hodnotu). V případě hello opakování z klienta hello všechny pokusy sdílet hello stejné id žádosti klienta. V případě hello dávky odeslaný klientem hello hello batch má id požadavek jednoho klienta.
> 
> 

### <a name="server-request-id"></a>ID žádosti serveru
Služba úložiště Hello automaticky vygeneruje ID žádosti serveru.

* V protokolu protokolování úložiště hello serverové id požadavku server hello se zobrazí hello **ID žádosti hlavičky** sloupce.
* V síťové trasování, například ten, který zachycenou aplikaci Fiddler, id žádosti hello server zobrazí v odpovědi na zprávy jako hello **x-ms-request-id** hodnotu hlavičky protokolu HTTP.
* V hello vytvoří protokolu na straně klienta, který hello Klientská knihovna pro úložiště, id žádosti server hello se zobrazí v hello **operaci Text** sloupec pro položku protokolu hello s podrobnostmi o hello odpověď serveru.

> [!NOTE]
> Hello úložiště služby vždy přiřadí jedinečné server žádost id tooevery požadavků, které obdrží, takže každý pokus opakovat z hello klienta a každé operace zahrnuté v dávce má id požadavku jedinečný serveru.
> 
> 

Pokud hello vyvolá Klientská knihovna pro úložiště **StorageException** v klientovi hello hello **RequestInformation** vlastnost obsahuje **RequestResult** objekt, který zahrnuje **ServiceRequestID** vlastnost. Můžete taky Přejít **RequestResult** objektu z **informacím OperationContext** instance.

Následující ukázka kódu Hello ukazuje, jak tooset vlastní **ClientRequestId** hodnotu připojením **informacím OperationContext** objektu hello požadavek toohello úložiště služby. Také ukazuje, jak tooretrieve hello **ServerRequestId** hodnotu z uvítací zprávu odpovědi.

```csharp
//Parse hello connection string for hello storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Create an Operation Context that includes custom ClientRequestId string based on constants defined within hello application along with a Guid.
OperationContext oc = new OperationContext();
oc.ClientRequestID = String.Format("{0} {1} {2} {3}", HOSTNAME, APPNAME, USERID, Guid.NewGuid().ToString());

try
{
    CloudBlobContainer container = blobClient.GetContainerReference("democontainer");
    ICloudBlob blob = container.GetBlobReferenceFromServer("testImage.jpg", null, null, oc);  
    var downloadToPath = string.Format("./{0}", blob.Name);
    using (var fs = File.OpenWrite(downloadToPath))
    {
        blob.DownloadToStream(fs, null, null, oc);
        Console.WriteLine("\t Blob downloaded toofile: {0}", downloadToPath);
    }
}
catch (StorageException storageException)
{
    Console.WriteLine("Storage exception {0} occurred", storageException.Message);
    // Multiple results may exist due tooclient side retry logic - each retried operation will have a unique ServiceRequestId
    foreach (var result in oc.RequestResults)
    {
            Console.WriteLine("HttpStatus: {0}, ServiceRequestId {1}", result.HttpStatusCode, result.ServiceRequestID);
    }
}
```

### <a name="timestamps"></a>Časová razítka
Můžete také použít časová razítka toolocate související položky protokolu, ale buďte opatrní z jakékoli posun mezi hello klientem a serverem, který může existovat hodin. Vyhledávejte plus nebo minus 15 minut pro párování serverové položky podle časové razítko hello hello klienta. Mějte na paměti, že hello metadata objektu blob pro objekty BLOB hello obsahující metriky označuje hello časové rozmezí hello metrik, které jsou uložené v objektu blob hello; To je užitečné, pokud máte mnoho objektů BLOB metriky pro hello stejné minutu nebo hodinu.

## <a name="troubleshooting-guidance"></a>Pokyny k odstraňování problémů
V této části vám pomůžou s hello diagnostiku a řešení potíží s některé běžné problémy, hello vaše aplikace může dojít při používání služby Azure storage hello. Pomocí seznamu hello níže toolocate hello informace relevantní tooyour určitého problému.

**Řešení potíží s rozhodovací strom.**

---
Souvisí problém výkonu toohello jednoho služby hello úložiště?

* [metriky ukazují AverageE2ELatency vysoké a nízké AverageServerLatency]
* [Metriky ukazují nízkou AverageE2ELatency a nízkou AverageServerLatency ale dochází k vysoké latenci hello klienta]
* [Metrika ukazuje vysokou hodnotu AverageServerLatency]
* [Dochází k neočekávaným zpožděním při doručování zpráv ve frontě]

---
Souvisí problém toohello dostupnost jednoho služby hello úložiště?

* [metriky způsobit nárůst PercentThrottlingError]
* [metriky způsobit nárůst PercentTimeoutError]
* [Metrika ukazuje zvýšení u PercentNetworkError]

---
 Klientské aplikace přijímá odpovědi HTTP 4XX (například 404) ze služby úložiště?

* [Klient Hello přijímá zprávy HTTP 403 (zakázáno)]
* [Klient Hello přijímá zprávy HTTP 404 (není nalezena)]
* [Hello klienta je přijímání zpráv protokolu HTTP 409 (konflikt)]

---
[metriky ukazují nízkou PercentSuccess nebo položky protokolu analýzy mít operací s Stav transakce ClientOtherErrors]

---
[Metriky kapacity způsobit neočekávané nárůst využití kapacity úložiště]

---
[Dochází k neočekávané restartování virtuálních počítačů, které mají velký počet připojených virtuálních pevných disků]

---
[Problém vyplývá z pomocí emulátoru úložiště hello pro vývoj nebo testování]

---
[Narazíte na potíže s instalací hello Azure SDK pro .NET]

---
[Máte jiný problém se službou úložiště]

---
### <a name="metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency"></a>Metriky ukazují AverageE2ELatency vysoké a nízké AverageServerLatency
Obrázek níže z hello Hello [portál Azure](https://portal.azure.com) monitorování nástroj ukazuje příklad, kde hello **AverageE2ELatency** je výrazně vyšší než hello **AverageServerLatency**.

![][4]

Poznámka: Služba úložiště hello pouze vypočítá hello metrika **AverageE2ELatency** pro úspěšné požadavky a na rozdíl od **AverageServerLatency**, zahrnuje hello čas trvá klientovi hello toosend hello data a přijímat potvrzení ze služby úložiště hello. Proto rozdíl mezi **AverageE2ELatency** a **AverageServerLatency** může být buď z důvodu toohello klienta aplikace se zpomalit toorespond nebo kvůli tooconditions v síti hello.

> [!NOTE]
> Můžete také zobrazit **E2ELatency** a **ServerLatency** pro jednotlivé úložiště operace v hello protokolování úložiště protokolovat data.
> 
> 

#### <a name="investigating-client-performance-issues"></a>Prozkoumat problémy s výkonem klienta
Možné důvody pro klienta hello reagovat pomalu zahrnují mají omezený počet připojení k dispozici nebo vláken, nebo se nedostatek prostředků, jako je například CPU, paměť či síťové šířky pásma. Možnost tooresolve hello problém může být změnou hello klienta kód toobe efektivnější (například pomocí služby úložiště toohello asynchronní volání) nebo pomocí větší virtuální počítač (s více jader a další paměť).

Pro služby tabulky a fronty hello, algoritmus Nagle hello taky může způsobit vysokou **AverageE2ELatency** jako porovnání příliš**AverageServerLatency**: Další informace najdete v příspěvku hello [na Nagle Algoritmus není popisný směrem malé požadavky](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/06/25/nagle-s-algorithm-is-not-friendly-towards-small-requests.aspx). Algoritmus Nagle hello v kódu můžete zakázat pomocí hello **ServicePointManager –** třídy v hello **System.Net** oboru názvů. Bude třeba provést dřív, než provedete žádné tabulce toohello volání nebo fronty služby ve vaší aplikaci, protože to nemá vliv na připojení, které jsou již otevřít. Hello následující příklad pochází z hello **Application_Start** metoda v roli pracovního procesu.

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);
ServicePoint tableServicePoint = ServicePointManager.FindServicePoint(storageAccount.TableEndpoint);
tableServicePoint.UseNagleAlgorithm = false;
ServicePoint queueServicePoint = ServicePointManager.FindServicePoint(storageAccount.QueueEndpoint);
queueServicePoint.UseNagleAlgorithm = false;
```

Hello klientské protokoly toosee kolik klientské aplikace je odeslání požadavků a zkontrolujte Obecné .NET související kritické body v vašeho klienta, například CPU, uvolňování paměti .NET, využití sítě, paměť, byste měli zkontrolovat. Jako výchozí bod pro řešení potíží s .NET klientské aplikace, najdete v části [ladění, trasování a profilování](http://msdn.microsoft.com/library/7fe0dd2y).

#### <a name="investigating-network-latency-issues"></a>Prozkoumat problémy s latencí sítě
Obvykle vysokou latencí začátku do konce způsobené hello sítě je z důvodu tootransient podmínky. Obě problémy se síťovým přechodný a trvalé například vynechaných paketů můžete prozkoumat pomocí nástrojů, jako je například Wireshark nebo Microsoft Message Analyzer.

Další informace o používání problémy se síťovým tootroubleshoot Wireshark najdete v tématu "[příloze 2: pomocí Wireshark toocapture síťový provoz]."

Další informace o používání problémy se síťovým tootroubleshoot Microsoft Message Analyzer najdete v tématu "[příloha 3: použití Microsoft Message Analyzer toocapture síťový provoz]."

### <a name="metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency"></a>Metriky ukazují nízkou AverageE2ELatency a nízkou AverageServerLatency ale dochází k vysoké latenci hello klienta
V tomto scénáři hello nejpravděpodobnější příčinou je zpoždění v žádostech o úložiště hello dosažení hello služby úložiště. Proč žádosti z klienta hello nejsou díky tomu prostřednictvím toohello služby objektů blob, které byste měli prozkoumat.

Jedním z možných důvodů pro hello klienta, čímž dochází ke zpoždění odesílání požadavků je, že jsou omezený počet připojení k dispozici nebo vláken.

Musí také zkontrolujte, jestli hello klienta je provádění více pokusů a zjistěte důvod hello je tomu hello. toodetermine zda hello klienta provádí více opakování, můžete:

* Zkontrolujte protokoly Storage Analytics hello. Pokud první pokus se děje, zobrazí se více operací s hello stejné ID žádosti klienta, ale s jinou serveru požádat ID.
* Zkontrolujte protokoly hello klienta. Podrobné protokolování bude znamenat, že došlo k chybě a k opakování.
* Ladění kódu a zkontrolujte vlastnosti hello hello **informacím OperationContext** objekt přidružený k požadavku hello. Pokud hello operace se pokusila akci opakovat, hello **RequestResults** vlastnost bude obsahovat více požadavek serveru jedinečné identifikátory. Můžete také zkontrolovat hello spuštění a ukončení pro každý požadavek. Další informace najdete v tématu hello ukázka kódu v části hello [ID žádosti serveru].

Pokud v klientovi hello nejsou žádné problémy, které byste měli prozkoumat potenciální problémy se síťovým například ztráta paketů. Můžete použít nástroje, jako je Wireshark nebo Microsoft Message Analyzer tooinvestigate problémů se sítí.

Další informace o používání problémy se síťovým tootroubleshoot Wireshark najdete v tématu "[příloze 2: pomocí Wireshark toocapture síťový provoz]."

Další informace o používání problémy se síťovým tootroubleshoot Microsoft Message Analyzer najdete v tématu "[příloha 3: použití Microsoft Message Analyzer toocapture síťový provoz]."

### <a name="metrics-show-high-AverageServerLatency"></a>Metriky ukazují vysoké AverageServerLatency
V případě hello vysokou **AverageServerLatency** žádostí o stažení objektů blob, měli byste použít hello protokolování úložiště protokolů toosee, pokud existují opakované žádosti pro hello stejný objekt blob (nebo sadu objektů BLOB). Pro objekt blob odesílat žádosti, které byste měli prozkoumat co bloku velikost hello klient používá (například bloky menší než 64 tisíc velikost může vést režie Pokud hello čte jsou i ve méně než 64 tisíc bloků dat), a pokud nahráváte více klientů blokuje toohello stejné objekt BLOB paralelně. Také byste měli zkontrolovat hello za minutu metriky špiček v hello počet požadavků, jejichž výsledkem je vyšší než hello za druhé cíle škálovatelnosti: také najdete v části "[metriky způsobit nárůst PercentTimeoutError]."

Pokud vidíte základní **AverageServerLatency** pro žádostí o stažení objektů blob při opakují požadavky hello stejné objektů blob nebo sadu objektů BLOB, pak jste měli zvažte ukládání do mezipaměti tyto objekty BLOB pomocí Azure Cache nebo hello doručování obsahu Azure Sítě (CDN). Pro nahrávání požadavky můžete zlepšit propustnost hello pomocí větší velikost bloku. Pro dotazy tootables, je také možné tooimplement použití mezipaměti klienta na klientských počítačích, které provádějí hello stejný dotaz operace a kde hello data nemění příliš často.

Vysoká **AverageServerLatency** hodnoty mohou být také příznakem nesprávně navržen tabulky nebo dotazy, výsledek v operacích kontroly nebo který postupujte hello připojit nebo předřazení proti vzor. Najdete v části "[metriky způsobit nárůst PercentThrottlingError]" Další informace.

> [!NOTE]
> Můžete najít komplexní kontrolní seznam výkonu kontrolní seznam zde: [Microsoft Azure Storage výkon a škálovatelnost kontrolní seznam](storage-performance-checklist.md).
> 
> 

### <a name="you-are-experiencing-unexpected-delays-in-message-delivery"></a>Dochází k neočekávané zpoždění při doručování zpráv ve frontě
Pokud dochází ke zpoždění mezi hello čas aplikace přidá tooa frontu a hello čas zprávy, které bude k dispozici tooread z hello fronty, a poté můžete provést následující kroky toodiagnose hello problém hello:

* Ověřte, že aplikace hello je úspěšně přidání hello zprávy toohello fronty. Zkontrolujte, zda není aplikace hello opakování hello **AddMessage** metody několikrát před úspěšné. protokoly Hello Klientská knihovna pro úložiště se zobrazí všechny opakovaných pokusech operace úložiště.
* Ověřte, zda jsou zkosení mezi hello role pracovního procesu, který přidává fronta toohello hello zpráv a hello role pracovního procesu, který čte uvítací zprávu z fronty hello, která usnadňuje se zobrazují jako když dochází ke zpoždění při zpracování žádné hodiny.
* Zkontrolujte, pokud se nedaří hello role pracovního procesu, který čte hello zprávy z fronty hello. Pokud klient fronty volá hello **GetMessage** metoda ale selže toorespond potvrzením, uvítací zprávu zůstane ve frontě hello neviditelná dokud hello **invisibilityTimeout** období vyprší platnost. V tomto okamžiku uvítací zprávu bude k dispozici pro zpracování znovu.
* Zkontrolujte, pokud se časem zvětšuje délka fronty hello. Tato situace může nastat, pokud nemáte dostatečná tooprocess dostupné pracovní procesy, které se všechny hello zprávy, že ostatní zaměstnanci jsou umístění na hello fronty. Také byste měli zkontrolovat, že toosee metriky hello, pokud se odstranění dochází k selhání požadavků a hello dequeue – počet na zprávy, jež mohou indikovat opakuje neúspěšných pokusů o přihlášení toodelete uvítací zprávu.
* V protokolech hello protokolování úložiště pro všechny operace fronty, které mají vyšší, než se očekávalo **E2ELatency** a **ServerLatency** hodnoty po dobu delší dobu než obvykle.

### <a name="metrics-show-an-increase-in-PercentThrottlingError"></a>Metriky způsobit nárůst PercentThrottlingError
Dochází k omezení chybám, když překročíte hello cíle škálovatelnosti služby úložiště. Služba úložiště Hello nemá tato tooensure, který žádná jednoho klienta nebo klienta můžete použít službu hello na náklady hello jiných. Další informace najdete v tématu [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md) podrobné informace o cíle škálovatelnosti pro účty úložiště a cíle výkonnosti pro oddíly v rámci účty úložiště.

Pokud hello **PercentThrottlingError** metrika způsobit nárůst hello procento žádostí, které se selháním kvůli omezení chybě, bude nutné tooinvestigate jednu z těchto dvou scénářů:

* [Přechodný zvýšení PercentThrottlingError]
* [Trvalé zvýšení PercentThrottlingError chyba]

Zvýšení **PercentThrottlingError** často k dochází v hello stejný čas jako zvýšení hello počet požadavků na úložiště, nebo pokud jste původně zátěžové testování vaší aplikace. To může také projevit v klientovi hello jako "503 Server zaneprázdněn" nebo "500 časový limit operace" HTTP stavové zprávy z operace úložiště.

#### <a name="transient-increase-in-PercentThrottlingError"></a>Přechodný zvýšení PercentThrottlingError
Pokud vidíte špičky v hello hodnotu **PercentThrottlingError** , shoduje s období vysoké aktivity pro hello aplikaci, byste měli implementovat exponenciální (ne lineární) regrese strategie pro opakování vašeho klienta: to bude snížit hello okamžitou zatížení oddílu hello a pomoci toosmooth vaší aplikace se špičky v provozu. Další informace o tom, jak pomocí zásady opakování tooimplement hello Klientská knihovna pro úložiště najdete v tématu [Microsoft.WindowsAzure.Storage.RetryPolicies Namespace](http://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.retrypolicies.aspx).

> [!NOTE]
> Může se také zobrazit špičky v hello hodnotu **PercentThrottlingError** který nekolidují s období vysoké aktivity pro aplikaci hello: hello zde nejpravděpodobnější příčinou je služba úložiště hello přesunutí oddíly tooimprove zatížení vyrovnávání.
> 
> 

#### <a name="permanent-increase-in-PercentThrottlingError"></a>Trvalé zvýšení PercentThrottlingError chyba
Pokud vidíte konzistentně vysoké hodnoty pro **PercentThrottlingError** následující trvalé nárůstem svazků transakce, nebo při provádění počáteční zatížení testů ve vaší aplikaci, pak je nutné tooevaluate jak vaše aplikace používá oddílů pro úložiště a zda se přiblíží hello cíle škálovatelnosti účtu úložiště. Například pokud vidíte omezení chyby ve frontě (který se počítá jako jeden oddíl), pak měli byste zvážit použití další fronty toospread hello transakce napříč více oddílů. Pokud vidíte omezení chyby v tabulce, je třeba tooconsider pomocí různých rozdělení toospread schéma vaší transakce napříč více oddílů s použitím širší rozsah hodnot klíče oddílu. Jednou z běžných příčin tohoto problému je hello předřazení připojit proti vzor kde vybrat datum hello jako klíč oddílu hello a pak se všechna data v určitý den zapíše tooone oddílu: zátěži, může být výsledkem kritický bod zápisu. Si zvažte různé rozdělení návrh nebo vyhodnocení, zda pomocí úložiště objektů blob může být lepším řešením. Také byste měli zkontrolovat Pokud hello omezení dochází v důsledku špičky v provozu a prozkoumat způsoby vyhlazení vaší vzor požadavků.

Pokud distribuujete vaší transakce napříč více oddílů, musíte stále být informace o limitech škálovatelnosti hello nastavit pro účet úložiště hello. Například pokud jste použili deset fronty každé zpracování hello maximálně 2 000 1KB zpráv za sekundu, které budou v hello celkový limit 20 000 zpráv za sekundu pro účet úložiště hello. Pokud potřebujete tooprocess více než 20 000 entity za sekundu, měli byste zvážit použití více účtů úložiště. Vezměte také na paměti tato velikost hello žádostí o a entity má vliv na když služba úložiště hello omezí generovaný vaši klienti: Pokud máte větší požadavky a entity, které může omezeny dříve.

Návrh neefektivní dotazu může také způsobit toohit hello limitech škálovatelnosti pro tabulku oddílů. Dotaz s filtrem jedno procento hello entit který vybere jen v oddílu, ale který prohledá všechny hello entit v oddílu například bude nutné tooaccess každé entity. Každou entitu číst se bude započítávat hello celkový počet transakcí v tomto oddílu. Proto můžete snadno dosáhnout cíle škálovatelnosti hello.

> [!NOTE]
> Testování výkonu by měl odhalit jakékoli návrhy neefektivní dotazu v aplikaci.
> 
> 

### <a name="metrics-show-an-increase-in-PercentTimeoutError"></a>Metriky způsobit nárůst PercentTimeoutError
Vaše metriky způsobit nárůst v **PercentTimeoutError** pro jednu z vaší služby úložiště. V hello stejný čas, hello klient obdrží k velkému počtu stavové zprávy "500 časový limit operace" HTTP z operace úložiště.

> [!NOTE]
> Může se zobrazit chyby vypršení časového limitu dočasně jako službu úložiště hello zatížení u žádostí o zůstatky přesunutím nový server tooa oddílu.
> 
> 

Hello **PercentTimeoutError** metrika je agregaci hello následující metriky: **ClientTimeoutError**, **AnonymousClientTimeoutError**,  **SASClientTimeoutError**, **ServerTimeoutError**, **AnonymousServerTimeoutError**, a **SASServerTimeoutError**.

časový limit serveru Hello jsou způsobeny chybu na serveru hello. překročení časového limitu klienta Hello dojít, protože operace na serveru hello překročil limit hello zadaný klientem hello; Například můžete nastavit časový limit operace pomocí hello klient používá hello Klientská knihovna pro úložiště **ServerTimeout** vlastnost hello **QueueRequestOptions** třídy.

Časový limit serveru znamenat problém související s hello úložiště služby, která vyžaduje další šetření. Pokud jste nedosáhli hello limitech škálovatelnosti pro službu hello a tooidentify žádné špičky v provozu, který může být příčinou tohoto problému můžete použít toosee metriky. Pokud problém hello přerušované, může být kvůli vyrovnávání tooload aktivity ve službě hello. Pokud problém hello je trvalé a není způsobené aplikace nedosáhli omezení škálovatelnosti hello hello služby, by měla vyvolat podporu problém. Pro překročení časového limitu klienta musíte rozhodnout, pokud časový limit hello je nastaven tooan odpovídající hodnotu v klientovi hello a hodnota časového limitu hello buď změnit nastavení v klientovi hello nebo zjistěte, jak může zlepšit výkon hello hello operací v úložišti služby hello, pro Příklad optimalizace své dotazy tabulky nebo zmenšení velikosti hello zprávy.

### <a name="metrics-show-an-increase-in-PercentNetworkError"></a>Metriky způsobit nárůst PercentNetworkError
Vaše metriky způsobit nárůst v **PercentNetworkError** pro jednu z vaší služby úložiště. Hello **PercentNetworkError** metrika je agregaci hello následující metriky: **NetworkError**, **AnonymousNetworkError**, a  **SASNetworkError**. Tyto nastane, když služba úložiště hello zjistí chyba sítě při hello klient požádá úložiště.

Hello Nejčastější příčinou této chyby je klient odpojení vyprší časový limit v úložišti služby hello. Hello kód byste měli prozkoumat v vašeho klienta toounderstand, proč a kdy hello klient neodpojí ze služby úložiště hello. Můžete také použít Wireshark, Microsoft Message Analyzer nebo Tcping tooinvestigate problémů s připojením z klienta hello. Tyto nástroje jsou popsány v hello [přílohy].

### <a name="the-client-is-receiving-403-messages"></a>Klient Hello přijímá zprávy HTTP 403 (zakázáno)
Pokud klientské aplikace způsobující chyby protokolu HTTP 403 (zakázáno), pravděpodobnou příčinou je, že tento klient hello používá při odesílání požadavku na úložiště (i když další možné příčiny patří hodiny zkosení, neplatné klíče a prázdný vypršenou platností sdíleného přístupového podpisu (SAS) hlavičky). Pokud vypršela platnost SAS klíč hello příčina, neuvidíte žádné položky v protokolování úložiště dat protokolu aplikace hello straně serveru. Hello následující tabulka uvádí ukázku z protokolu klienta hello generované hello Klientská knihovna pro úložiště, který znázorňuje výskytu tohoto problému:

| Zdroj | Podrobností | Podrobností | Id žádosti klienta | Operace textu |
| --- | --- | --- | --- | --- |
| Microsoft.WindowsAzure.Storage |Informace |3 |85d077ab-... |Spuštění operace s umístěním primární umístění režim PrimaryOnly podle. |
| Microsoft.WindowsAzure.Storage |Informace |3 |85d077ab-... |Spouštění synchronní požadavku toohttps://domemaildist.blob.core.windows.netazureimblobcontainer/blobCreatedViaSAS.txt?sv=2014-02-14&amp;sr = c&amp;ma = mypolicy&amp;sig = OFnd4Rd7z01fIvh % 2BmcR6zbudIH2F5Ikm % 2FyhNYZEmJNQ % 3D&amp;rozhraní api-version = 2014-02-14. |
| Microsoft.WindowsAzure.Storage |Informace |3 |85d077ab-... |Čekání na odpověď. |
| Microsoft.WindowsAzure.Storage |Upozornění |2 |85d077ab-... |Došlo k výjimce při čekání na odpověď: hello na vzdálený server vrátil chybu: (403) zakázán... |
| Microsoft.WindowsAzure.Storage |Informace |3 |85d077ab-... |Odpověď. Stavový kód = 403, ID žádosti = 9d67c64a-64ed-4b0d-9515-3b14bbcdc63d, obsah MD5 =, značka ETag =. |
| Microsoft.WindowsAzure.Storage |Upozornění |2 |85d077ab-... |Došlo k výjimce během operace hello: hello na vzdálený server vrátil chybu: (403) zakázán... |
| Microsoft.WindowsAzure.Storage |Informace |3 |85d077ab-... |Kontrola, pokud hello operaci je třeba opakovat. Počet opakování = 0, stavový kód HTTP = 403, výjimka = hello vzdálený server vrátil chybu: (403) zakázán... |
| Microsoft.WindowsAzure.Storage |Informace |3 |85d077ab-... |Další umístění Hello nastavený tooPrimary, na základě hello umístění režimu. |
| Microsoft.WindowsAzure.Storage |Chyba |1 |85d077ab-... |Zásady opakování neumožňuje pro opakovaný pokus. Došlo k selhání s hello vzdálený server vrátil chybu: (403) zakázán. |

V tomto scénáři které byste měli prozkoumat, proč je před hello klient odešle serveru tokenu toohello hello vypršení platnosti tokenu SAS hello:

* Obvykle neměli nastavit čas spuštění, při vytváření SAS pro klienta toouse okamžitě. Pokud existují malé hello hodiny rozdíly mezi hostiteli hello generování hello SAS pomocí aktuálního času a služba úložiště hello, je možné pro hello úložiště služby tooreceive SAS, který dosud není platný.
* Čas vypršení platnosti velmi krátké nesmí nastavit na SAS. Znovu hodiny malé rozdíly mezi hostiteli hello generování hello SAS a služba úložiště hello může způsobit tooa SAS zjevně vypršení platnosti dříve, než se očekává.
* Hello parametr verze v hello SAS klíč (například **sv = 2015-04-05**) shodu hello verzi hello Klientská knihovna pro úložiště používáte? Doporučujeme vždy použít nejnovější verzi hello hello [Klientská knihovna pro úložiště](https://www.nuget.org/packages/WindowsAzure.Storage/).
* Pokud jste znovu vygenerovat přístupové klíče k úložišti, tím zneplatnit všechny existující tokeny SAS. Pokud generovat tokeny SAS s dobou vypršení platnosti dlouhé pro klientské aplikace toocache to může být problém.

Pokud používáte tokeny SAS toogenerate hello Klientská knihovna pro úložiště, je snadné toobuild platný token. Ale pokud používáte hello REST API pro úložiště a vytváření Tokeny SAS hello ručně měli pečlivě si přečtěte téma hello [delegování přístupu k pomocí sdíleného přístupového podpisu](http://msdn.microsoft.com/library/azure/ee395415.aspx).

### <a name="the-client-is-receiving-404-messages"></a>Klient Hello přijímá zprávy HTTP 404 (není nalezena)
Pokud hello klientská aplikace obdrží zprávu HTTP 404 (není nalezena) ze serveru hello, z toho vyplývá, že hello objekt hello klient se pokoušel toouse (například entity, tabulka, objektů blob, kontejneru nebo fronty) neexistuje v úložišti služby hello. Existuje několik z možných důvodů, jako například:

* [Hello klienta nebo jiný proces dřív odstranit objekt hello]
* [Chybu ověřování sdíleného přístupového podpisu (SAS)]
* [Kód jazyka JavaScript na straně klienta nemá oprávnění tooaccess hello objekt]
* [Selhání sítě]

#### <a name="client-previously-deleted-the-object"></a>Hello klienta nebo jiný proces dřív odstranit objekt hello
Ve scénářích, kde klient hello pokouší tooread, aktualizace nebo odstranění dat v úložišti služby, který je obvykle zaznamená snadno tooidentify v hello serverové předchozí operace, který odstraněn objekt hello dotyčném ze služby úložiště hello. Velmi často hello protokolu data zobrazují objektu odstraněné hello jiný uživatel nebo proces. V protokolu protokolování úložiště hello serverové hello typ operace a požadovaný klíč objektu sloupce ukazují, když klient odstraněn objekt.

V hello scénář, kde se klient pokouší tooinsert objektu nemusí být hned zjevné Proč to vede odpovědi HTTP 404 (není nalezena), vzhledem k tomu, že hello klienta je vytvoření nového objektu. Ale pokud hello klienta je vytvoření objektu blob, které musí být schopný toofind kontejner objektů blob hello, pokud hello klienta je vytvoření zprávy, musí být schopný toofind fronty, a pokud klient hello je přidání řádku musí být schopný toofind hello tabulky.

Můžete použít protokol klienta hello z toogain hello Klientská knihovna pro úložiště, které podrobnější přehled o Pokud hello klient odešle požadavky na konkrétní toohello úložiště služby.

Hello následující klienta protokolu vygenerovaných knihovny klienta úložiště hello ilustruje hello problém při hello klient nemůže najít kontejner hello pro objekt blob hello, které se vytváří. Tento protokol obsahuje podrobnosti o hello následující operace úložiště:

| ID požadavku | Operace |
| --- | --- |
| 07b26a5d-... |**DeleteIfExists** kontejner objektů blob hello toodelete metoda. Všimněte si, že tato operace zahrnuje **HEAD** požadavku toocheck hello existence hello kontejneru. |
| e2d06d78... |**CreateIfNotExists** kontejner objektů blob hello toocreate metoda. Všimněte si, že tato operace zahrnuje **HEAD** požadavek, který zkontroluje existenci hello hello kontejneru. Hello **HEAD** vrací zprávu 404, ale bude pokračovat. |
| de8b1c3c-... |**UploadFromStream** metoda toocreate hello blob. Hello **PUT** požadavek selže s zprávu 404 |

Položky protokolu:

| ID požadavku | Operace textu |
| --- | --- |
| 07b26a5d-... |Spouštění toohttps://domemaildist.blob.core.windows.net/azuremmblobcontainer synchronní požadavku. |
| 07b26a5d-... |StringToSign = HEAD...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 června 2014 10:33:11 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Čekání na odpověď. |
| 07b26a5d-... |Odpověď. Stavový kód = 200, ID žádosti = eeead849-... Obsah MD5 =, značka ETag = &quot;0x8D14D2DC63D059B&quot;. |
| 07b26a5d-... |Hlavičky odpovědi počet úspěšně zpracovaných, budete pokračovat s hello zbytek hello operaci. |
| 07b26a5d-... |Při stahování text odpovědi. |
| 07b26a5d-... |Operace byla úspěšně dokončena. |
| 07b26a5d-... |Spouštění toohttps://domemaildist.blob.core.windows.net/azuremmblobcontainer synchronní požadavku. |
| 07b26a5d-... |StringToSign = DELETE...x-ms-client-request-id:07b26a5d-...x-ms-date:Tue, 03 června 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| 07b26a5d-... |Čekání na odpověď. |
| 07b26a5d-... |Odpověď. Stavový kód = 202, ID žádosti = 6ab2a4cf-..., obsah MD5 =, značka ETag =. |
| 07b26a5d-... |Hlavičky odpovědi počet úspěšně zpracovaných, budete pokračovat s hello zbytek hello operaci. |
| 07b26a5d-... |Při stahování text odpovědi. |
| 07b26a5d-... |Operace byla úspěšně dokončena. |
| e2d06d78-... |Spouští Asynchronní požadavek toohttps://domemaildist.blob.core.windows.net/azuremmblobcontainer.</td> |
| e2d06d78-... |StringToSign = HEAD...x-ms-client-request-id:e2d06d78-...x-ms-date:Tue, 03 června 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Čekání na odpověď. |
| de8b1c3c-... |Spouštění toohttps://domemaildist.blob.core.windows.net/azuremmblobcontainer/blobCreated.txt synchronní požadavku. |
| de8b1c3c-... |StringToSign = PUT... 64.qCmF+TQLPhq/YYK50mP9ZQ==...x-MS-BLOB-Type:BlockBlob.x-MS-Client-Request-ID:de8b1c3c-...x-MS-Date:TUE, 03 června 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer/blobCreated.txt. |
| de8b1c3c-... |Příprava dat toowrite požadavku. |
| e2d06d78-... |Došlo k výjimce při čekání na odpověď: hello na vzdálený server vrátil chybu: (404) nebyl nalezen... |
| e2d06d78-... |Odpověď. Stavový kód 404, ID žádosti = = 353ae3bc-..., obsah MD5 =, značka ETag =. |
| e2d06d78-... |Hlavičky odpovědi počet úspěšně zpracovaných, budete pokračovat s hello zbytek hello operaci. |
| e2d06d78-... |Při stahování text odpovědi. |
| e2d06d78-... |Operace byla úspěšně dokončena. |
| e2d06d78-... |Spouští Asynchronní požadavek toohttps://domemaildist.blob.core.windows.net/azuremmblobcontainer. |
| e2d06d78-... |StringToSign = PUT... 0...x-MS-Client-Request-ID:e2d06d78-...x-MS-Date:TUE, 03 června 2014 10:33:12 GMT.x-ms-version:2014-02-14./domemaildist/azuremmblobcontainer.restype:container. |
| e2d06d78-... |Čekání na odpověď. |
| de8b1c3c-... |Data žádosti o zápis. |
| de8b1c3c-... |Čekání na odpověď. |
| e2d06d78-... |Došlo k výjimce při čekání na odpověď: hello na vzdálený server vrátil chybu: (409) – konflikt... |
| e2d06d78-... |Odpověď. Stavový kód = 409, ID žádosti = c27da20e-..., obsah MD5 =, značka ETag =. |
| e2d06d78-... |Probíhá stahování textu odpovědi na chyby. |
| de8b1c3c-... |Došlo k výjimce při čekání na odpověď: hello na vzdálený server vrátil chybu: (404) nebyl nalezen... |
| de8b1c3c-... |Odpověď. Stavový kód 404, ID žádosti = = 0eaeab3e-..., obsah MD5 =, značka ETag =. |
| de8b1c3c-... |Došlo k výjimce během operace hello: hello na vzdálený server vrátil chybu: (404) nebyl nalezen... |
| de8b1c3c-... |Zásady opakování neumožňuje pro opakovaný pokus. Došlo k selhání s hello vzdálený server vrátil chybu: (404) nebyl nalezen... |
| e2d06d78-... |Zásady opakování neumožňuje pro opakovaný pokus. Došlo k selhání s hello vzdálený server vrátil chybu: (409) – konflikt... |

V tomto příkladu hello protokolu ukazuje, že klient hello je prokládání požadavky od hello **CreateIfNotExists** – metoda (žádost o id e2d06d78...) s požadavky hello z hello **UploadFromStream** (– metoda de8b1c3c-...); k této situaci dochází hello klientská aplikace je asynchronně volání těchto metod. Upravte kód asynchronní hello v tooensure hello klienta, vytvoří kontejner hello před pokusem o tooupload žádné datový objekt blob tooa v tomto kontejneru. V ideálním případě byste měli předem vytvořit všechny kontejnery.

#### <a name="SAS-authorization-issue"></a>Chybu ověřování sdíleného přístupového podpisu (SAS)
Pokud klientská aplikace hello pokusí toouse SAS klíč, který nezahrnuje hello potřebná oprávnění pro operaci hello, vrátí služba úložiště hello klientem toohello zprávy HTTP 404 (není nalezena). V hello stejný čas, zobrazí se také nenulové hodnoty pro **SASAuthorizationError** v hello metriky.

Hello následující tabulka uvádí ukázkovou zprávu protokolu na straně serveru ze souboru protokolu protokolování úložiště hello:

| Name (Název) | Hodnota |
| --- | --- |
| Žádost o spuštění | 2014-05-30T06:17:48.4473697Z |
| Typ operace     | GetBlobProperties            |
| Stav žádosti o     | SASAuthorizationError        |
| Stavový kód protokolu HTTP   | 404                          |
| Typ ověřování| SAS                          |
| Typ služby       | Objekt blob                         |
| Adresa URL požadavku        | https://domemaildist.BLOB.Core.Windows.NET/azureimblobcontainer/blobCreatedViaSAS.txt |
| nbsp;              |   ? sv = 2014-02-14 & sr = c & ma = mypolicy & sig = XXXXX&;rozhraní api-version = 2014-02-14 |
| Hlavičky id žádosti  | a1f348d5-8032-4912-93EF-b393e5252a3b |
| ID žádosti klienta  | 2d064953-8436-4ee0-aa0c-65cb874f7929 |


Proč klientské aplikace se pokouší tooperform, které jí nebylo uděleno oprávnění pro operace, které byste měli prozkoumat.

#### <a name="JavaScript-code-does-not-have-permission"></a>Kód jazyka JavaScript na straně klienta nemá oprávnění tooaccess hello objekt
Pokud používáte klienta JavaScript a služba úložiště hello vrací zprávy HTTP 404, můžete zkontrolovat následující chyby jazyka JavaScript v prohlížeči hello hello:

```
SEC7120: Origin http://localhost:56309 not found in Access-Control-Allow-Origin header.
SCRIPT7002: XMLHttpRequest: Network Error 0x80070005, Access is denied.
```

> [!NOTE]
> V Internet Exploreru tootrace hello zprávy se vyměňují mezi hello prohlížeče a služba úložiště hello při odstraňování problémů, JavaScript na straně klienta, můžete použít nástroje pro vývojáře F12 hello.
> 
> 

K těmto chybám, protože webový prohlížeč hello implementuje hello [stejné zásady počátek](http://www.w3.org/Security/wiki/Same_Origin_Policy) omezení zabezpečení, které brání volání rozhraní API v jiné doméně než stránku hello hello domény na webové stránce pochází z.

toowork kolem hello JavaScript problém, můžete nakonfigurovat křížové sdílení prostředků zdroji (CORS) pro klienta hello služby úložiště hello přistupuje. Další informace najdete v tématu [Podpora sdílení prostředků různých původů (CORS) pro úložiště služby Azure](http://msdn.microsoft.com/library/azure/dn535601.aspx).

Hello následující ukázka kódu ukazuje, jak tooconfigure objektu blob služby služby tooallow JavaScript spuštěné v hello tooaccess domény Contoso do objektu blob ve službě úložiště objektů blob:

```csharp
CloudBlobClient client = new CloudBlobClient(blobEndpoint, new StorageCredentials(accountName, accountKey));
// Set hello service properties.
ServiceProperties sp = client.GetServiceProperties();
sp.DefaultServiceVersion = "2013-08-15";
CorsRule cr = new CorsRule();
cr.AllowedHeaders.Add("*");
cr.AllowedMethods = CorsHttpMethods.Get | CorsHttpMethods.Put;
cr.AllowedOrigins.Add("http://www.contoso.com");
cr.ExposedHeaders.Add("x-ms-*");
cr.MaxAgeInSeconds = 5;
sp.Cors.CorsRules.Clear();
sp.Cors.CorsRules.Add(cr);
client.SetServiceProperties(sp);
```

#### <a name="network-failure"></a>Selhání sítě
V některých případech může vést ke ztrátě síťové pakety služba úložiště toohello vrácení klienta toohello zprávy HTTP 404. Například když klientské aplikace je odstranění entity ze služby table hello uvidíte hello klienta throw výjimka úložiště vytváření sestav "HTTP 404 (není nalezena)" stavovou zprávu ze služby table hello. Pokud byste prozkoumat hello tabulky v hello tabulka úložiště služby, uvidíte požadované odstranění služby hello hello entity.

Podrobnosti o výjimce Hello v klientovi hello zahrnují id žádosti hello (7e84f12d...) přiřadila hello služby table pro požadavek hello: Podrobnosti požadavku hello toolocate tato informace můžete použít v protokolech úložiště serverové hello podle hledání v hello  **hlavička požadavku id** sloupec v souboru protokolu hello. Tooidentify hello metriky můžete také použít při selhání, jako to dojít a pak vyhledejte hello soubory protokolu na základě hello čas hello metriky zaznamenávají této chybě. Tato položka obsahuje protokolu, které hello odstranění se nezdařila se zpráva stav "Klient jiné chyby protokolu HTTP (404)". Hello stejnou položku protokolu také zahrnuje id žádosti hello generované klientem hello v hello **client-request-id** sloupce (813ea74f...).

Hello serverové protokolu obsahuje také další položka se hello stejné **client-request-id** operace pro odstranění hodnotu (813ea74f...) úspěšně hello stejné entity a z hello stejné klienta. Tato operace úspěšné odstranění došlo krátce před hello Neúspěšné odstranění požadavku.

Hello nejpravděpodobnější příčinou tohoto scénáře je tento klient hello odeslal žádost o odstranění služby hello entity toohello tabulky, který byl úspěšný, avšak neobdržel potvrzení ze serveru hello (pravděpodobně z důvodu problému tooa dočasné sítě). Hello klienta pak automaticky opakovat operaci hello (pomocí stejné hello **client-request-id**), a tento opakování se nezdařilo, protože hello entity byl již odstraněn.

Pokud tento problém opakuje často, které byste měli prozkoumat, proč klient hello nemůže tooreceive potvrzení ze služby table hello. Pokud problém hello přerušované, by měl depeše chyby "HTTP (404) nebyl nalezen" hello a protokolu v klientovi hello ale povolit toocontinue hello klienta.

### <a name="the-client-is-receiving-409-messages"></a>Hello klienta je přijímání zpráv protokolu HTTP 409 (konflikt)
Hello následující tabulka ukazuje výpis z protokolu serverové hello dva klientské operace: **DeleteIfExists** a okamžitě nástrojem **CreateIfNotExists** pomocí hello stejný název kontejneru objektu blob. Všimněte si, že každý klient výsledkem operace dva požadavky odeslané toohello serveru, nejprve **GetContainerProperties** toocheck požadavek Pokud hello kontejneru existuje, za nímž následuje hello **DeleteContainer** nebo  **CreateContainer** požadavku.

| časové razítko | Operace | výsledek | Název kontejneru | Id žádosti klienta |
| --- | --- | --- | --- | --- |
| 05:10:13.7167225 |GetContainerProperties |200 |mmcont |c9f52c89-... |
| 05:10:13.8167325 |DeleteContainer |202 |mmcont |c9f52c89-... |
| 05:10:13.8987407 |GetContainerProperties |404 |mmcont |bc881924-... |
| 05:10:14.2147723 |CreateContainer |409 |mmcont |bc881924-... |

Hello kódu v aplikaci klienta hello odstraní a okamžitě vytvoří kontejner objektů blob pomocí hello stejným názvem: hello **CreateIfNotExists** – metoda (klient v požadavku ID bc881924-...) nakonec selže s hello HTTP 409 (konflikt) došlo k chybě. Když klient odstraní kontejnery objektů blob, tabulek nebo fronty, který je na krátkou dobu před název hello opět k dispozici.

klientská aplikace Hello měli používat názvy kontejnerů jedinečný vždy, když se vytvoří nové kontejnery, pokud je běžné vzor hello odstranit nebo znovu vytvoří.

### <a name="metrics-show-low-percent-success"></a>Metriky ukazují nízkou PercentSuccess nebo položky protokolu analýzy mít operací s stav transakce ClientOtherErrors
Hello **PercentSuccess** metrika zaznamená v procentech hello operací, které byly úspěšné podle jejich stavový kód HTTP. Počet operací s kódy stavu 2XX jako úspěšné, zatímco operací s kódy stavu v 3XX, 4XX a 5XX rozsahy, se počítají jako neúspěšná a nižší hello **PercentSucess** hodnota metriky. V souborech protokolů hello úložiště na straně serveru, se zaznamenávají tyto operace se stavem transakce **ClientOtherErrors**.

Je důležité toonote, že tyto operace byly úspěšně dokončeny a proto nemají vliv na jiné metriky, jako je dostupnost. Některé příklady operací, které úspěšně provést, ale můžete výsledkem neúspěšných stavové kódy HTTP patří:

* **ResourceNotFound** (není nalezen 404), například z GET požadavek tooa objektů blob, který neexistuje.
* **ResouceAlreadyExists** (409 konflikt), například ze **CreateIfNotExist** operaci kde hello prostředek již existuje.
* **ConditionNotMet** (ne upravit 304), například z podmíněného operace, například když klient odešle **značka ETag** hodnota a HTTP **If-None-Match** záhlaví toorequest pouze pokud bitové kopie byla aktualizována od poslední operace hello.

Můžete najít seznam běžné kódy chyb rozhraní REST API, které služby úložiště hello vrátit na stránku hello [běžné kódy chyb rozhraní API REST](http://msdn.microsoft.com/library/azure/dd179357.aspx).

### <a name="capacity-metrics-show-an-unexpected-increase"></a>Metriky kapacity způsobit neočekávané nárůst využití kapacity úložiště
Pokud se zobrazí nečekané, neočekávané změny využití kapacity ve vašem účtu úložiště můžete prozkoumat hello důvodů nejprve kontrolou vaše dostupnosti metriky; například můžou způsobit zvýšení tooan hello množství úložiště objektů blob, který používáte jako operace čištění konkrétní aplikace, které může mít očekává, že toobe uvolnění místa na nemusí fungovat podle očekávání (pro zvýšení hello počet neúspěšných žádostí o odstranění Příklad, protože vypršela platnost tokeny SAS hello používá pro uvolnění místa).

### <a name="you-are-experiencing-unexpected-reboots"></a>Dochází k neočekávané restartování virtuálních počítačů Azure, které mají velký počet připojených virtuálních pevných disků
Pokud virtuální počítač Azure (VM) má velký počet připojených virtuálních pevných disků, které jsou v hello stejný účet úložiště, může překročit cíle škálovatelnosti hello k účtu úložiště jednotlivých způsobuje toofail hello virtuálních počítačů. Měli byste zkontrolovat hello minutu metriky pro účet úložiště hello (**TotalRequests**/**TotalIngress**/**TotalEgress**) špiček který překročit hello cíle škálovatelnosti účtu úložiště. Části hello "[metriky způsobit nárůst PercentThrottlingError]" pro pomoc při určování, zda omezení došlo k chybě na vašem účtu úložiště.

Obecně platí, každé jednotlivé vstupní nebo výstupní operace na virtuální pevný disk z virtuálního počítače překládá příliš**získat stránky** nebo **Put stránky** operací na hello základní objekt blob stránky. Proto můžete použít hello odhadované IOPS pro vaše prostředí tootune kolik virtuální pevné disky vám může mít v jednom úložném účtu na základě hello konkrétní chování vaší aplikace. Nedoporučujeme mít víc než 40 disky v jednom úložném účtu. V tématu [a cíle výkonnosti služby Azure Storage Scalability](storage-scalability-targets.md) podrobnosti hello aktuální cíle škálovatelnosti pro účty úložiště, zejména hello celkový požadavek rychlost a celková šířka pásma pro hello typ účtu úložiště používáte .
Pokud jsou překročení hello cíle škálovatelnosti účtu úložiště, měli byste umístit virtuální pevné disky v několika různých úložiště účtů tooreduce hello aktivity v jednotlivé účty.

### <a name="your-issue-arises-from-using-the-storage-emulator"></a>Problém vyplývá z pomocí emulátoru úložiště hello pro vývoj nebo testování
Obvykle pomocí emulátoru úložiště hello během vývoje a testování tooavoid hello požadavek pro účet úložiště Azure. Hello běžné problémy, které může dojít, když používáte emulátor úložiště hello jsou:

* [Funkce "X" nepracuje v emulátoru úložiště hello]
* [Chyba "hello hodnotu pro jednu z hlaviček hello HTTP není ve správném formátu hello" při použití hello emulátor úložiště]
* [Spuštěného emulátoru úložiště hello vyžaduje oprávnění správce]

#### <a name="feature-X-is-not-working"></a>Funkce "X" nepracuje v emulátoru úložiště hello
emulátor úložiště Hello nepodporuje všechny funkce hello hello služeb úložiště Azure, jako je služba souborů hello. Další informace najdete v tématu [hello pomocí emulátoru úložiště Azure pro vývoj a testování](storage-use-emulator.md).

Pro tyto funkce, které hello úložiště emulátoru neobsahuje podporu, použijte hello služby Azure storage v cloudu hello.

#### <a name="error-HTTP-header-not-correct-format"></a>Chyba "hello hodnotu pro jednu z hlaviček hello HTTP není ve správném formátu hello" při použití hello emulátor úložiště
Testování aplikace pomocí hello Klientská knihovna pro úložiště proti hello místní úložiště emulátoru a metoda volání, jako například **CreateIfNotExists** selhání s chybou hello zprávou "hello hodnota pro jednu z hlaviček hello HTTP není v Hello správný formát." To znamená, že hello verzi používáte emulátor úložiště hello nepodporuje hello verzi klientské knihovny pro hello úložiště používáte. Hello Klientská knihovna pro úložiště přidá hlavičku hello **x-ms-version** tooall hello požadavky umožňuje. Pokud emulátor úložiště hello nebyla rozpoznána hodnota hello v hello **x-ms-version** záhlaví, se odmítne žádost hello.

Můžete použít hello knihovny klienta úložiště protokolů toosee hello hodnotu hello **hlavičky x-ms-version** je odesílání. Můžete také zjistit hodnotu hello hello **hlavičky x-ms-version** Pokud používáte aplikaci Fiddler tootrace hello požadavky z klientské aplikace.

Tento scénář obvykle dochází, je-li nainstalovat a použít nejnovější verzi hello hello Klientská knihovna pro úložiště bez aktualizace emulátor úložiště hello. Měli buď nainstalovat nejnovější verze emulátoru úložiště hello hello nebo použijte cloudového úložiště místo hello emulátor pro vývoj a testování.

#### <a name="storage-emulator-requires-administrative-privileges"></a>Spuštěného emulátoru úložiště hello vyžaduje oprávnění správce
Budete vyzváni k zadání pověření správce při spuštění emulátor úložiště hello. K tomu dochází pouze při hello emulátor úložiště pro hello jsou inicializaci poprvé. Po inicializaci emulátor úložiště hello toorun oprávnění pro správu není nutné ho znovu.

Další informace najdete v tématu [hello pomocí emulátoru úložiště Azure pro vývoj a testování](storage-use-emulator.md). Všimněte si, že můžete také inicializovat hello emulátor úložiště v sadě Visual Studio, který bude také vyžadují oprávnění správce.

### <a name="you-are-encountering-problems-installing-the-Windows-Azure-SDK"></a>Narazíte na potíže s instalací hello Azure SDK pro .NET
Při pokusu o tooinstall hello SDK, dojde k chybě snažíme emulátor úložiště hello tooinstall na místním počítači. protokol instalace Hello obsahuje jednu z následujících zpráv hello:

* CAQuietExec: Chyba: nelze tooaccess instance systému SQL
* CAQuietExec: Chyba: nelze toocreate databáze

Hello příčinou je problém se stávající instalací LocalDB. Ve výchozím nastavení používá emulátor úložiště hello LocalDB toopersist dat při simuluje hello služby Azure storage. Spuštěním následujících příkazů v okně příkazového řádku a teprve potom zkusili tooinstall hello SDK hello můžete resetovat vaší instanci LocalDB.

```
sqllocaldb stop v11.0
sqllocaldb delete v11.0
delete %USERPROFILE%\WAStorageEmulatorDb3*.*
sqllocaldb create v11.0
```

Hello **odstranit** příkaz odebere žádné staré soubory databáze z předchozí instalace emulátor úložiště hello.

### <a name="you-have-a-different-issue-with-a-storage-service"></a>Máte jiný problém se službou úložiště
Pokud předchozí části řešení potíží hello neobsahují hello problém, který máte službou úložiště, by měl přijmout hello následující toodiagnosing přístup a řešení potíží problém.

* Vaše metriky toosee zaškrtněte, pokud dojde ke změně z vaší očekávané chování základní line. Z metriky hello může být schopný toodetermine, zda je problém hello přechodný nebo trvalé, a které operace úložiště hello problém ovlivňuje.
* Můžete použít informace metriky hello toohelp můžete prohledat data protokolu na straně serveru pro podrobnější informace o chyby, ke kterým dochází. Tyto informace pomohou vyřešit problém hello.
* Pokud hello informace hello serverové protokolů úspěšně není dostatečná tootroubleshoot hello problém, můžete použít hello Klientská knihovna pro úložiště klientské protokoly tooinvestigate hello chování vaší aplikace klienta a nástrojů, například aplikaci Fiddler, Wireshark, a Microsoft Message Analyzer tooinvestigate vaší sítě.

Další informace o používání aplikaci Fiddler, najdete v části "[dodatku 1: použití Fiddler toocapture HTTP a HTTPS provozy]."

Další informace o používání Wireshark najdete v tématu "[příloze 2: pomocí Wireshark toocapture síťový provoz]."

Další informace o používání Microsoft Message Analyzer najdete v tématu "[příloha 3: použití Microsoft Message Analyzer toocapture síťový provoz]."

## <a name="appendices"></a>Přílohy
přílohy Hello popisují několik nástrojů, které mohou být užitečné při diagnostikování a řešení potíží s Azure Storage (a další služby). Tyto nástroje nejsou součástí Azure Storage a některé produkty třetích stran. Jako takový hello nástrojů popsaných v těchto přílohy nejsou zahrnuty jakékoli smlouvy podpory, kterou můžete mít s Microsoft Azure nebo Azure Storage, a proto jako součást procesu vyhodnocení byste měli zkontrolovat hello licencování a podporu možnosti dostupné z Zprostředkovatelé Hello těchto nástrojů.

### <a name="appendix-1"></a>Dodatek 1: Použití Fiddler toocapture HTTP a komunikaci přes protokol HTTPS
[Fiddler](http://www.telerik.com/fiddler) je užitečným nástrojem pro analýzu hello HTTP a HTTPS provozy mezi klientská aplikace a hello služba úložiště Azure, kterou používáte.

> [!NOTE]
> Fiddler může dekódovat provoz HTTPS; Přečtěte si dokumentaci Fiddler hello pečlivě toounderstand jak funguje a vliv na zabezpečení toounderstand hello.
> 
> 

Tento dodatek obsahuje stručný návod, jak aplikaci Fiddler toocapture tooconfigure provoz mezi hello místního počítače, kam jste nainstalovali aplikaci Fiddler ale hello služby Azure storage.

Po spuštění Fiddler zahájíte zaznamenávání HTTP a HTTPS provozy na místním počítači. Hello Toto jsou některé užitečné příkazy pro řízení Fiddler:

* Zastavení a spuštění zachycení provozu. V hlavní nabídce hello přejděte příliš**soubor** a pak klikněte na **zachycování provozu** tootoggle zaznamenávání zapnout a vypnout.
* Zachycená data data uložte. V hlavní nabídce hello přejděte příliš**soubor**, klikněte na tlačítko **Uložit**a potom klikněte na **všechny relace**: To vám umožní toosave hello provoz v souboru archivu relace. Můžete znovu načíst relace archivu později pro analýzu, nebo pokud vyžaduje podporu tooMicrosoft odeslat.

toolimit hello objem provozu, který zachycuje aplikaci Fiddler, můžete použít filtry, které nakonfigurujete v hello **filtry** kartě hello následující snímek obrazovky ukazuje filtr, který zachytí pouze provoz odeslaný toohello  **contosoemaildist.Table.Core.Windows.NET** koncový bod úložiště:

![][5]

### <a name="appendix-2"></a>Dodatek 2: Pomocí Wireshark toocapture síťový provoz
[Wireshark](http://www.wireshark.org/) je analyzátoru protokolu sítě, která vám umožní tooview podrobné informace o paketů pro širokou škálu protokolů síťové.

Hello následující postup ukazuje, jak toocapture podrobné informace o paketů pro provoz z místního počítače hello nainstalovanou služby table toohello Wireshark ve vašem účtu úložiště Azure.

1. Spusťte Wireshark na místním počítači.
2. V hello **spustit** části, vyberte hello rozhraní místní sítě nebo rozhraní, které jsou připojené toohello Internetu.
3. Klikněte na tlačítko **zaznamenat možnosti**.
4. Přidat filtr toohello **filtr pro sběr dat** textové pole. Například **hostitele contosoemaildist.table.core.windows.net** nakonfiguruje Wireshark toocapture jen pakety odeslané z hello koncový bod služby tabulek v hello tooor **contosoemaildist** úložiště účet. Podívejte se na hello [úplný seznam filtrů zaznamenat](http://wiki.wireshark.org/CaptureFilters).
   
   ![][6]
5. Klikněte na tlačítko **spustit**. Wireshark teď zaznamená všechny tooor odeslat pakety hello z koncový bod služby tabulek hello při používání klientské aplikace na místním počítači.
6. Po dokončení, na hello hlavní nabídce klikněte na tlačítko **zaznamenat** a potom **Zastavit**.
7. toosave hello zachycená data v Wireshark zaznamenat souboru na hello hlavní nabídce klikněte na tlačítko **soubor** a potom **Uložit**.

WireShark bude zvýrazněte všechny chyby, které existují v hello **packetlist** okno. Můžete taky hello **Expert informace** okno (klikněte na tlačítko **analyzovat**, pak **Expert informace**) tooview souhrn chyby a upozornění.

![][7]

Můžete také zvolit tooview hello TCP data jako hello aplikační vrstvu, uvidí ho kliknutím pravým tlačítkem myši na hello TCP dat a výběrem **podle datového proudu TCP**. To je zvlášť užitečné, pokud zaznamenat vaše výpisu bez zachycení filtru. Další informace najdete v tématu [následující datové proudy TCP](http://www.wireshark.org/docs/wsug_html_chunked/ChAdvFollowTCPSection.html).

![][8]

> [!NOTE]
> Další informace o používání Wireshark najdete v tématu hello [Wireshark uživatelé průvodce](http://www.wireshark.org/docs/wsug_html_chunked).
> 
> 

### <a name="appendix-3"></a>Dodatek 3: Použití Microsoft Message Analyzer toocapture síťový provoz
Můžete použít Microsoft Message Analyzer toocapture HTTP a provoz HTTPS v podobným způsobem tooFiddler a zachycení síťového provozu v podobným způsobem tooWireshark.

#### <a name="configure-a-web-tracing-session-using-microsoft-message-analyzer"></a>Konfigurovat webovou relaci trasování pomocí Microsoft Message Analyzer
tooconfigure a relace trasování webu pro HTTP a HTTPS provozy pomocí Microsoft Message Analyzer, spusťte aplikaci Microsoft Message Analyzer hello a potom na hello **soubor** nabídky, klikněte na tlačítko **zachycení/trasování**. V hello seznam scénářů, k dispozici trasování, vyberte **webový proxy server**. Potom v hello **trasování scénáře konfigurace** panelu v hello **HostnameFilter** textovému poli, přidejte hello názvy koncových bodů úložiště (můžete vyhledat tyto názvy v hello [portál Azure](https://portal.azure.com)). Například pokud hello název účtu úložiště Azure je **contosodata**, měli byste přidat hello následující toohello **HostnameFilter** textové pole:

```
contosodata.blob.core.windows.net contosodata.table.core.windows.net contosodata.queue.core.windows.net
```

> [!NOTE]
> Znak mezery odděluje hello názvy hostitelů.
> 
> 

Pokud jste připravené toostart shromažďování dat trasování, klikněte na tlačítko hello **Start With** tlačítko.

Další informace o hello Microsoft Message Analyzer **webový proxy server** trasování najdete v tématu [zprostředkovatele Microsoft-PEF-WebProxy](http://technet.microsoft.com/library/jj674814.aspx).

Hello předdefinované **webový proxy server** trasování v Microsoft Message Analyzer je založena na aplikaci Fiddler, můžete zaznamenávat provoz HTTPS na straně klienta a zobrazit nezašifrované zpráv protokolu HTTPS. Hello **webový proxy server** trasování funguje tak, že nakonfigurujete místní proxy server pro veškerý provoz protokolu HTTP a HTTPS, který provede jeho přístup toounencrypted zprávy.

#### <a name="diagnosing-network-issues-using-microsoft-message-analyzer"></a>Diagnostika sítě problémy při použití Microsoft Message Analyzer
Kromě toho toousing hello Microsoft Message Analyzer **webový proxy server** trasování toocapture podrobnosti o hello přenosy HTTP/HTTPs mezi hello klientská aplikace a služby úložiště hello, můžete také použít integrované hello  **Místní vrstvy odkaz** toocapture síťových paketů informace trasování. Tato umožňuje vám toocapture data podobné toothat které můžete zaznamenat s Wireshark a diagnostikovat sítě problémy, jako jsou vyřazené pakety.

Hello následující snímek obrazovky ukazuje příklad **místní vrstvy odkaz** trasování s některými **informační** zprávy v hello **DiagnosisTypes** sloupce. Kliknutím na ikonu v hello **DiagnosisTypes** sloupci se zobrazuje hello podrobnosti zprávy hello. V tomto příkladu hello server opakovaně odeslané zprávy #305 protože nepřijala potvrzení z klienta hello:

![][9]

Když vytvoříte relace trasování hello v Microsoft Message Analyzer, můžete zadat filtry tooreduce hello množství šumu v hello trasování. Na hello **zachycení / trasování** stránky, kde můžete definovat hello trasování, klikněte na hello **konfigurace** odkaz na další příliš**Microsoft-Windows-NDIS-PacketCapture**. Hello následující snímek obrazovky ukazuje konfigurace, který filtruje přenosy TCP pro IP adresy hello tři služby úložiště:

![][10]

Další informace o hello Microsoft zpráva analyzátor místní odkaz vrstvy trasování najdete v tématu [zprostředkovatele Microsoft PEF NDIS PacketCapture](http://technet.microsoft.com/library/jj659264.aspx).

### <a name="appendix-4"></a>Dodatek 4: Použití tooview metriky a protokolu data z Excelu.
Celou řadu nástrojů povolit toodownload hello úložiště metriky, dat z úložiště tabulek Azure s oddělovači formátu, který umožňuje snadno tooload hello data do Excelu pro zobrazení a analýza. Úložiště protokolování dat z Azure blob storage je již ve formátu s oddělovači, který můžete načíst do aplikace Excel. Však budete potřebovat založené na hello informace na záhlaví příslušných sloupců tooadd [úložiště analýzy protokolů formátu](http://msdn.microsoft.com/library/azure/hh343259.aspx) a [schématu tabulky metriky Analytics úložiště](http://msdn.microsoft.com/library/azure/hh343264.aspx).

tooimport dat protokolování úložiště do aplikace Excel po můžete, který ji stáhnout z úložiště objektů blob:

* Na hello **Data** nabídky, klikněte na tlačítko **z textu**.
* Soubor protokolu toohello procházet tooview a klikněte na **Import**.
* V kroku 1 tohoto hello **Průvodce importem textu**, vyberte **odděleného**.

V kroku 1 tohoto hello **Průvodce importem textu**, vyberte **středník** jako hello pouze oddělovač a zvolte dvojité uvozovky jako hello **Text kvalifikátor**. Pak klikněte na tlačítko **Dokončit** a zvolte, kde tooplace hello data v sešitu.

### <a name="appendix-5"></a>Dodatek 5: Monitorování pomocí služby Application Insights pro Visual Studio Team Services
Také můžete funkce hello Application Insights pro Visual Studio Team Services jako součást výkonu a dostupnosti monitorování. Tento nástroj můžete:

* Ujistěte se, že webová služba je k dispozici a dobře reagovaly. Jestli je vaše aplikace na web nebo aplikaci ze zařízení, která používá webovou službu, je otestovat adresu URL každých několik minut z umístění kolem hello, world a umožňují vědět, pokud dojde k problému.
* Rychle diagnostikuje všechny problémy s výkonem nebo výjimky v webové služby. Zjistěte, pokud jsou jejich protažení procesoru nebo jiným prostředkům, sám trasování zásobníku výjimky a snadno prohledávat protokolu trasování. Pokud hello vyřazuje výkonu aplikace pod přijatelnou omezení, může vám můžeme poslat e-mailu. Můžete monitorovat webové služby .NET a Java.

Další informace najdete [co je Application Insights?](../../application-insights/app-insights-overview.md).

<!--Anchors-->
[Úvod]: #introduction
[Uspořádání Tato příručka]: #how-this-guide-is-organized

[monitorování vaší služby úložiště]: #monitoring-your-storage-service
[Monitorování stavu služby]: #monitoring-service-health
[Monitorování kapacity]: #monitoring-capacity
[Monitorování dostupnosti]: #monitoring-availability
[Sledování výkonu]: #monitoring-performance

[diagnostice problémů s úložištěm]: #diagnosing-storage-issues
[Problémy v oblasti služby stavu]: #service-health-issues
[Problémy s výkonem]: #performance-issues
[Diagnostikování chyb]: #diagnosing-errors
[Emulátor problémů s úložištěm]: #storage-emulator-issues
[Nástroje protokolování úložiště]: #storage-logging-tools
[Pomocí nástroje protokolování]: #using-network-logging-tools

[začátku do konce trasování]: #end-to-end-tracing
[Korelace data protokolu]: #correlating-log-data
[ID žádosti klienta]: #client-request-id
[ID žádosti serveru]: #server-request-id
[Časová razítka]: #timestamps

[pokyny při řešení potíží]: #troubleshooting-guidance
[metriky ukazují AverageE2ELatency vysoké a nízké AverageServerLatency]: #metrics-show-high-AverageE2ELatency-and-low-AverageServerLatency
[Metriky ukazují nízkou AverageE2ELatency a nízkou AverageServerLatency ale dochází k vysoké latenci hello klienta]: #metrics-show-low-AverageE2ELatency-and-low-AverageServerLatency
[Metrika ukazuje vysokou hodnotu AverageServerLatency]: #metrics-show-high-AverageServerLatency
[Dochází k neočekávaným zpožděním při doručování zpráv ve frontě]: #you-are-experiencing-unexpected-delays-in-message-delivery

[metriky způsobit nárůst PercentThrottlingError]: #metrics-show-an-increase-in-PercentThrottlingError
[Přechodný zvýšení PercentThrottlingError]: #transient-increase-in-PercentThrottlingError
[Trvalé zvýšení PercentThrottlingError chyba]: #permanent-increase-in-PercentThrottlingError
[metriky způsobit nárůst PercentTimeoutError]: #metrics-show-an-increase-in-PercentTimeoutError
[Metrika ukazuje zvýšení u PercentNetworkError]: #metrics-show-an-increase-in-PercentNetworkError

[Klient Hello přijímá zprávy HTTP 403 (zakázáno)]: #the-client-is-receiving-403-messages
[Klient Hello přijímá zprávy HTTP 404 (není nalezena)]: #the-client-is-receiving-404-messages
[Hello klienta nebo jiný proces dřív odstranit objekt hello]: #client-previously-deleted-the-object
[Chybu ověřování sdíleného přístupového podpisu (SAS)]: #SAS-authorization-issue
[Kód jazyka JavaScript na straně klienta nemá oprávnění tooaccess hello objekt]: #JavaScript-code-does-not-have-permission
[Selhání sítě]: #network-failure
[Hello klienta je přijímání zpráv protokolu HTTP 409 (konflikt)]: #the-client-is-receiving-409-messages

[metriky ukazují nízkou PercentSuccess nebo položky protokolu analýzy mít operací s Stav transakce ClientOtherErrors]: #metrics-show-low-percent-success
[Metriky kapacity způsobit neočekávané nárůst využití kapacity úložiště]: #capacity-metrics-show-an-unexpected-increase
[Dochází k neočekávané restartování virtuálních počítačů, které mají velký počet připojených virtuálních pevných disků]: #you-are-experiencing-unexpected-reboots
[Problém vyplývá z pomocí emulátoru úložiště hello pro vývoj nebo testování]: #your-issue-arises-from-using-the-storage-emulator
[Funkce "X" nepracuje v emulátoru úložiště hello]: #feature-X-is-not-working
[Chyba "hello hodnotu pro jednu z hlaviček hello HTTP není ve správném formátu hello" při použití hello emulátor úložiště]: #error-HTTP-header-not-correct-format
[Spuštěného emulátoru úložiště hello vyžaduje oprávnění správce]: #storage-emulator-requires-administrative-privileges
[Narazíte na potíže s instalací hello Azure SDK pro .NET]: #you-are-encountering-problems-installing-the-Windows-Azure-SDK
[Máte jiný problém se službou úložiště]: #you-have-a-different-issue-with-a-storage-service

[přílohy]: #appendices
[dodatku 1: použití Fiddler toocapture HTTP a HTTPS provozy]: #appendix-1
[příloze 2: pomocí Wireshark toocapture síťový provoz]: #appendix-2
[příloha 3: použití Microsoft Message Analyzer toocapture síťový provoz]: #appendix-3
[Dodatek 4: Použití tooview metriky a protokolu data z Excelu.]: #appendix-4
[Dodatek 5: Monitorování pomocí služby Application Insights pro Visual Studio Team Services]: #appendix-5

<!--Image references-->
[1]: ./media/storage-monitoring-diagnosing-troubleshooting/overview.png
[3]: ./media/storage-monitoring-diagnosing-troubleshooting/hour-minute-metrics.png
[4]: ./media/storage-monitoring-diagnosing-troubleshooting/high-e2e-latency.png
[5]: ./media/storage-monitoring-diagnosing-troubleshooting/fiddler-screenshot.png
[6]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-1.png
[7]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-2.png
[8]: ./media/storage-monitoring-diagnosing-troubleshooting/wireshark-screenshot-3.png
[9]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-1.png
[10]: ./media/storage-monitoring-diagnosing-troubleshooting/mma-screenshot-2.png
