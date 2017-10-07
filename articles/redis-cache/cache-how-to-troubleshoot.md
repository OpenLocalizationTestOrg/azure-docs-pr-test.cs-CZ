---
title: aaaHow tootroubleshoot Azure Redis Cache | Microsoft Docs
description: "Zjistěte, jak tooresolve běžné problémy s Azure Redis Cache."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 928b9b9c-d64f-4252-884f-af7ba8309af6
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 01/06/2017
ms.author: sdanie
ms.openlocfilehash: 4e736fce2b6d5200a2a8d802f3f1384b63458cab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-tootroubleshoot-azure-redis-cache"></a>Jak tootroubleshoot Azure mezipaměti Redis
Tento článek obsahuje pokyny k odstraňování potíží hello následující kategorie problémy s Azure Redis Cache.

* [Řešení potíží s straně klienta](#client-side-troubleshooting) – Tato část obsahuje pokyny pro identifikaci a řešení potíží se nezdařila z důvodu aplikace hello připojení tooAzure Redis Cache.
* [Řešení potíží s serveru straně](#server-side-troubleshooting) – Tato část obsahuje pokyny pro identifikaci a řešení potíží se nezdařila z důvodu na hello Azure Redis Cache na straně serveru.
* [Výjimkám časového limitu StackExchange.Redis](#stackexchangeredis-timeout-exceptions) – Tato část obsahuje informace o odstraňování problémů při používání klienta StackExchange.Redis hello.

> [!NOTE]
> Řadu hello řešení potíží s kroky v této příručce obsahovat pokyny toorun Redis příkazy a monitorování různých metrik výkonu. Další informace a pokyny najdete v tématu hello články v hello [Další informace o](#additional-information) části.
> 
> 

## <a name="client-side-troubleshooting"></a>Řešení potíží s straně klienta
Tato část popisuje řešení problémů, ke kterým dochází kvůli podmínce na hello klientské aplikace.

* [Přetížení paměti v klientovi hello](#memory-pressure-on-the-client)
* [Shluků provozu](#burst-of-traffic)
* [Klient vysoké využití procesoru](#high-client-cpu-usage)
* [Překročení šířky pásma straně klienta](#client-side-bandwidth-exceeded)
* [Velikost velké požadavků a odpovědí](#large-requestresponse-size)
* [Co se stalo toomy data v Redis?](#what-happened-to-my-data-in-redis)

### <a name="memory-pressure-on-hello-client"></a>Přetížení paměti v klientovi hello
#### <a name="problem"></a>Problém
Přetížení paměti na klientský počítač hello vede druhů tooall problémy s výkonem, které můžou zdržet zpracování dat, který vám byl zaslán instancí hello Redis bez jakéhokoli zpoždění. Pokud se dotkne přetížení paměti, systém hello obvykle obsahuje toopage data z paměti toovirtual fyzické paměti, která je na disku. To *stránky chybující* příčiny hello systému tooslow dolů výrazně.

#### <a name="measurement"></a>Měření
1. Monitorování využití paměti na počítač toomake jistotu, že nepřekročí dostupné paměti. 
2. Monitorování hello `Page Faults/Sec` čítače výkonu. Většina systémů bude mít některé chyb stránek i při běžném provozu, takže sledování špiček v tomto čítači výkonu chyb stránky, odpovídajících s vypršení časových limitů.

#### <a name="resolution"></a>Řešení
Upgrade vašeho klienta tooa větší klienta velikost virtuálního počítače s více paměti nebo proniknout do vašeho paměti využití vzory tooreduce paměti consuption.

### <a name="burst-of-traffic"></a>Shluků provozu
#### <a name="problem"></a>Problém
Shluky provozu v kombinaci s nízká `ThreadPool` nastavení může vést k prodlevám při zpracování dat již poslal hello serveru Redis, ale ještě nebyla použije na straně klienta hello.

#### <a name="measurement"></a>Měření
Monitorování jak vaše `ThreadPool` časem pomocí kódu změnit statistiky [podobné výjimky](https://github.com/JonCole/SampleCode/blob/master/ThreadPoolMonitor/ThreadPoolLogger.cs). Můžete také prohlédnout hello `TimeoutException` zprávu od StackExchange.Redis. Tady je příklad:

    System.TimeoutException: Timeout performing EVAL, inst: 8, mgr: Inactive, queue: 0, qu: 0, qs: 0, qc: 0, wr: 0, wq: 0, in: 64221, ar: 0, 
    IOCP: (Busy=6,Free=999,Min=2,Max=1000), WORKER: (Busy=7,Free=8184,Min=2,Max=8191)

V hello výše zpráva existuje několik problémů, které jsou zajímavé:

1. Všimněte si, že v hello `IOCP` části a hello `WORKER` části máte `Busy` hodnotu, která je větší než hello `Min` hodnotu. To znamená, že vaše `ThreadPool` nastavení je třeba úprava.
2. Můžete také zjistit `in: 64221`. To znamená, že byly přijaty ve vrstvě soketu jádra hello 64211 bajtů, ale nebyly přečteny hello aplikace (např. StackExchange.Redis). To obvykle znamená, že vaše aplikace není čtení dat z hello sítě rychle hello server je odeslání tooyou.

#### <a name="resolution"></a>Řešení
Konfigurace vaší [nastavení fondu podprocesů](https://gist.github.com/JonCole/e65411214030f0d823cb) toomake jistotu, že se rychle v části škálování fondu vláken burst scénáře.

### <a name="high-client-cpu-usage"></a>Klient vysoké využití procesoru
#### <a name="problem"></a>Problém
Vysoké využití procesoru v klientovi hello je znamená, že hello systému nelze přečtěte si, že byl požádaný tooperform pracovní hello. To znamená, že tento klient hello může selhat tooprocess včas odpověď od Redis, i když Redis odeslal odpověď hello velmi rychle.

#### <a name="measurement"></a>Měření
Hello monitorování využití procesoru široké systému prostřednictvím hello portálu Azure nebo hello související čítače výkonu. Dávejte pozor, není toomonitor *proces* procesoru vzhledem k tomu, že jeden proces může mít nízké využití procesoru na hello stejný čas, celého systému procesoru může být vysoká. Sledování špiček využití procesoru, které odpovídají s vypršení časových limitů. V důsledku vysoké využití procesoru, může se také zobrazit základní `in: XXX` hodnoty v `TimeoutException` chybové zprávy, jak je popsáno v hello [shluků provozu](#burst-of-traffic) části.

> [!NOTE]
> StackExchange.Redis 1.1.603 a dále zahrnuje hello `local-cpu` metriky v `TimeoutException` chybové zprávy. Ujistěte se, používáte nejnovější verzi hello hello [balíčku StackExchange.Redis NuGet](https://www.nuget.org/packages/StackExchange.Redis/). Zde nejsou chyby neustále probíhá pevná ve hello kód toomake ho robustnější tootimeouts tak nutnosti hello nejnovější verze je důležité.
> 
> 

#### <a name="resolution"></a>Řešení
Upgradujte tooa větší velikost virtuálního počítače s větší kapacitu procesoru nebo zjistěte, co ho způsobuje vzroste využití procesoru. 

### <a name="client-side-bandwidth-exceeded"></a>Překročení šířky pásma straně klienta
#### <a name="problem"></a>Problém
Různé velikostí klientské počítače mají omezení na tom, kolik šířku pásma sítě mají k dispozici. Pokud překročí hello klienta hello dostupnou šířku pásma a potom dat nebudou zpracována na straně klienta hello tak rychle, jak hello server je odeslání. To může způsobit tootimeouts.

#### <a name="measurement"></a>Měření
Monitorování, jak změnit použití šířky pásma v čase pomocí kódu [podobné výjimky](https://github.com/JonCole/SampleCode/blob/master/BandWidthMonitor/BandwidthLogger.cs). Všimněte si, že tento kód nemusí spustit, v některých prostředích s omezenými oprávněními (například weby Azure).

#### <a name="resolution"></a>Řešení
Zvětšete velikost virtuálního počítače klienta nebo snížení využití šířky pásma sítě.

### <a name="large-requestresponse-size"></a>Velikost velké požadavků a odpovědí
#### <a name="problem"></a>Problém
Velké požadavků a odpovědí může způsobit překročení časového limitu. Předpokládejme například, klientem hodnota časového limitu je 1 sekunda. Vaše aplikace požaduje dva klíče (např.) "A" a "B") na hello současně (pomocí hello stejné fyzické síti připojení). Většina klienti podporují "Pipelining" požadavků, tak, aby oba požadavky "A" a "B", budou odeslány na hello přenosová toohello serveru jeden po hello jiných bez čekání na odezvu hello. Hello server bude odesílat hello odpovědí zpět v hello stejné pořadí. Pokud je odpověď "A" velký dostatečně ho vyžadovat značné množství většinu hello časový limit pro následné požadavky. 

Hello následující příklad ukazuje, tento scénář. V tomto scénáři hello požadavek "A" a "B" odešlou rychle hello server se spustí rychle odesílání odpovědí "A" a "B", ale kvůli doba přenosu dat, "B" uváznout za další požadavek a časy se to i v případě, že hello server odpověděl rychle.

    |-------- 1 Second Timeout (A)----------|
    |-Request A-|
         |-------- 1 Second Timeout (B) ----------|
         |-Request B-|
                |- Read Response A --------|
                                           |- Read Response B-| (**TIMEOUT**)



#### <a name="measurement"></a>Měření
Toto je obtížné jeden toomeasure. V podstatě máte tooinstrument kód tootrack velké požadavky a odpovědi. 

#### <a name="resolution"></a>Řešení
1. Redis je optimalizovaná pro velké množství malých hodnoty, nikoli několik velkých hodnot. Hello upřednostňovaná řešení je toobreak dat do související menší hodnoty. V tématu hello [co je rozsah velikost hello ideální hodnot pro redis? Je příliš velký 100KB? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) post podrobnosti kolem Proč se doporučují menší hodnoty.
2. Zvětšete velikost hello vašeho virtuálního počítače (pro klienta a Server mezipaměti Redis) tooget vyšší šířku pásma schopnosti, omezení dat přenosu časy pro větší odpovědi. Všimněte si, že získávání větší šířku pásma na právě hello serveru nebo jenom na klienta hello nemusí stačit. Měření použití šířky pásma a porovnávají ho toohello možnosti hello velikost virtuálního počítače, které máte aktuálně.
3. Zvyšte počet hello `ConnectionMultiplexer` objekty můžete použít a kruhového dotazování požadavky přes jiné připojení.

### <a name="what-happened-toomy-data-in-redis"></a>Co se stalo toomy data v Redis?
#### <a name="problem"></a>Problém
Očekávání pro určité data toobe instance Moje Azure Redis Cache, ale to nebylo zdát toobe existuje.

#### <a name="resolution"></a>Řešení
V tématu [jaká data se stalo toomy v Redis?](https://gist.github.com/JonCole/b6354d92a2d51c141490f10142884ea4#file-whathappenedtomydatainredis-md) možné příčiny a řešení.

## <a name="server-side-troubleshooting"></a>Řešení potíží s straně serveru
Tato část popisuje řešení problémů, ke kterým dochází z důvodu stavu na serveru mezipaměti hello.

* [Přetížení paměti na hello server](#memory-pressure-on-the-server)
* [Vysoké využití procesoru / Server načíst](#high-cpu-usage-server-load)
* [Překročení šířky pásma straně serveru](#server-side-bandwidth-exceeded)

### <a name="memory-pressure-on-hello-server"></a>Přetížení paměti na hello server
#### <a name="problem"></a>Problém
Přetížení paměti na straně serveru hello vede druhů tooall problémy s výkonem, které můžou zdržet zpracování požadavků. Pokud se dotkne přetížení paměti, systém hello obvykle obsahuje toopage data z paměti toovirtual fyzické paměti, která je na disku. To *stránky chybující* příčiny hello systému tooslow dolů výrazně. Existuje několik možných příčin této přetížení paměti: 

1. Jste vyplnili hello mezipaměti toofull kapacitu s daty. 
2. Redis se zobrazuje fragmentace paměti vysoké - nejčastěji způsobeno ukládání velkých objektů (Redis je optimalizovaná pro malé objekty – viz hello [co je rozsah velikost hello ideální hodnot pro redis? Je příliš velký 100KB? ](https://groups.google.com/forum/#!searchin/redis-db/size/redis-db/n7aa2A4DZDs/3OeEPHSQBAAJ) post podrobnosti). 

#### <a name="measurement"></a>Měření
Redis zpřístupní dvě metriky, které pomáhají identifikovat potíže. Nejprve je Hello `used_memory` a hello jiných `used_memory_rss`. [Tyto metriky](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) jsou k dispozici v hello portálu Azure nebo prostřednictvím hello [Redis informace](http://redis.io/commands/info) příkaz.

#### <a name="resolution"></a>Řešení
Existuje několik možných změny, které můžete provádět využití paměti zachovat toohelp v pořádku:

1. [Nakonfigurujte zásady paměti](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) a nastavit čas vypršení platnosti na vaše klíče. Všimněte si, že to nemusí být dostatečná, pokud máte fragmentace.
2. [Nakonfigurovat hodnotu vyhrazené maxmemory](cache-configure.md#maxmemory-policy-and-maxmemory-reserved) tedy dostatečně velké na to toocompensate pro fragmentace paměti.
3. Rozdělte vaší rozsáhlé objekty uložené v mezipaměti do menších související objekty.
4. [Škálování](cache-how-to-scale.md) tooa větší velikost mezipaměti.
5. Pokud používáte [premium mezipaměť Redis clusteru povolena](cache-how-to-premium-clustering.md) můžete [zvýšit hello počet horizontálních oddílů](cache-how-to-premium-clustering.md#change-the-cluster-size-on-a-running-premium-cache).

### <a name="high-cpu-usage--server-load"></a>Vysoké využití procesoru / Server načíst
#### <a name="problem"></a>Problém
Vysoké využití procesoru může znamenat, že hello na straně klienta může selhat tooprocess na odpověď od Redis včas, přestože Redis odeslal odpověď hello velmi rychle.

#### <a name="measurement"></a>Měření
Hello monitorování využití procesoru široké systému prostřednictvím hello portálu Azure nebo hello související čítače výkonu. Dávejte pozor, není toomonitor *proces* procesoru vzhledem k tomu, že jeden proces může mít nízké využití procesoru na hello stejný čas, celého systému procesoru může být vysoká. Sledování špiček využití procesoru, které odpovídají s vypršení časových limitů.

#### <a name="resolution"></a>Řešení
[Škálování](cache-how-to-scale.md) větší mezipaměti tooa vrstvy s větší kapacitu procesoru nebo zjistěte, co ho způsobuje vzroste využití procesoru. 

### <a name="server-side-bandwidth-exceeded"></a>Překročení šířky pásma straně serveru
#### <a name="problem"></a>Problém
Různé velikostí instance mají omezení na tom, kolik šířku pásma sítě mají k dispozici. Pokud hello server překračuje dostupnou šířku pásma hello, pak data nebudou odeslány toohello klienta jako rychle. To může způsobit tootimeouts.

#### <a name="measurement"></a>Měření
Můžete monitorovat hello `Cache Read` metriku, což je hello množství dat načten z mezipaměti hello v MB za sekundu (MB/s) během zadaného intervalu sestavy hello. Tato hodnota odpovídá toohello šířku pásma sítě používané této mezipaměti. Pokud chcete tooset si výstrahy pro omezení šířky pásma sítě straně serveru, můžete je vytvořit pomocí této `Cache Read` čítače. Porovnání vaší odečty s hodnotami hello v [Tato tabulka](cache-faq.md#cache-performance) pro hello zjištěnými omezení šířky pásma pro různé mezipaměti cenové úrovně a velikosti.

#### <a name="resolution"></a>Řešení
Pokud jste téměř hello dodržen maximální šířka pásma pro cenovou úroveň a mezipaměti velikost konzistentně, vezměte v úvahu [škálování](cache-how-to-scale.md) tooa cenová úroveň nebo velikost, která má větší šířku pásma sítě, pomocí hodnoty hello v [tuto tabulku](cache-faq.md#cache-performance) jako vodítko.

## <a name="stackexchangeredis-timeout-exceptions"></a>Výjimkám časového limitu StackExchange.Redis
Název nastavení používá StackExchange.Redis `synctimeout` pro synchronní operace, které má výchozí hodnotu 1000 ms. Pokud nedokončí synchronní volání v hello stanoveno čas, vyvolává klient StackExchange.Redis hello toohello podobné chyby vypršení časového limitu následující ukázka.

    System.TimeoutException: Timeout performing MGET 2728cc84-58ae-406b-8ec8-3f962419f641, inst: 1,mgr: Inactive, queue: 73, qu=6, qs=67, qc=0, wr=1/1, in=0/0 IOCP: (Busy=6, Free=999, Min=2,Max=1000), WORKER (Busy=7,Free=8184,Min=2,Max=8191)


Tato chybová zpráva obsahuje metriky, který vám pomůže nasměrovat vás toohello příčina a jejich možná řešení problému, hello. Hello následující tabulka obsahuje podrobnosti o hello chybová zpráva metriky.

| Metrika chybová zpráva | Podrobnosti |
| --- | --- |
| INST |V posledním časovém intervalu hello: 0 příkazy vydané |
| Mgr |Správce soketu Hello provádí `socket.select` což znamená, že je dotazem hello OS tooindicate soketu, který má něco toodo; v podstatě: hello čtečky není čtení aktivně ze sítě hello vzhledem k tomu, že není myslíte, není nic toodo |
| Fronty |73 celkový průběh operace |
| qu |6 hello v průběhu operací jsou ve frontě neodeslaných hello a nebyly dosud zapsány toohello odchozí sítě |
| QS |67 he v průběhu operací byly odeslány toohello serveru, ale odpověď ještě není k dispozici. Hello odpovědí může být `Not yet sent by hello server` nebo`sent by hello server but not yet processed by hello client.` |
| QC |0 hello v průběhu operací viděli jste, odpoví, ale nebyly dosud byla označena jako dokončená z důvodu toowaiting na hello dokončení smyčky |
| WR |Je aktivní zapisovače (znamená hello 6 neodeslaných požadavky nejsou ignorovány) bajtů/activewriters |
| V |Neexistují žádné aktivní čtečky a nula bajtů je k dispozici toobe číst na hello seskupování bajtů/activereaders |

### <a name="steps-tooinvestigate"></a>Kroky tooinvestigate
1. Jako osvědčený postup zajistěte, aby používáte hello následující vzor tooconnect při používání klienta StackExchange.Redis hello.

    ```c#
    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    
    });
    
    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }
    ````

    Další informace najdete v tématu [připojit toohello mezipaměti StackExchange.Redis](cache-dotnet-how-to-use-azure-redis-cache.md#connect-to-the-cache).

1. Zajistěte, aby byly v hello Azure Redis Cache a klientská aplikace hello stejné oblasti v Azure. Například se vám může zobrazovat časové limity Pokud vaše mezipaměť je ve východní USA, ale klient hello západní USA a hello žádost není dokončena v rámci hello `synctimeout` interval, nebo může být získávání překročení časového limitu při ladění z místním vývojovém počítači. 
   
    To se důrazně doporučuje toohave hello mezipaměti a v klientovi hello v hello stejné oblasti Azure. Pokud máte scénáře, který zahrnuje volání různých oblastí, měli byste nastavit hello `synctimeout` interval tooa hodnotu vyšší než hello výchozí 1000 ms, interval zahrnutím `synctimeout` vlastnost hello připojovací řetězec. Hello následující příklad ukazuje, fragment StackExchange.Redis mezipaměti připojovací řetězec s `synctimeout` z 2000 ms.
   
        synctimeout=2000,cachename.redis.cache.windows.net,abortConnect=false,ssl=true,password=...
2. Ujistěte se, používáte nejnovější verzi hello hello [balíčku StackExchange.Redis NuGet](https://www.nuget.org/packages/StackExchange.Redis/). Zde nejsou chyby neustále probíhá pevná ve hello kód toomake ho robustnější tootimeouts tak nutnosti hello nejnovější verze je důležité.
3. Pokud jsou požadavky, které jsou získávání svázaná s omezení šířky pásma na hello serveru nebo klienta, bude trvat déle pro ně toocomplete a tím způsobit překročení časového limitu. toosee Pokud vaše vypršení časového limitu z důvodu toonetwork šířky pásma na serveru hello, zobrazí se [šířky pásma serveru straně překročena](#server-side-bandwidth-exceeded). toosee, pokud je vaše vypršení časového limitu z důvodu tooclient šířku pásma sítě, najdete v části [šířky pásma straně klienta, která je překročena](#client-side-bandwidth-exceeded).
4. Je můžete získávání procesoru vázána na hello serveru nebo na hello klienta?
   
   * Zkontrolujte, jestli jste se získávání svázaná s procesoru na vašeho klienta, což by mohlo způsobit hello požadavek toonot zpracovat v rámci hello `synctimeout` interval, což způsobuje vypršení časového limitu. Přesunutí tooa větší velikost klienta nebo distribuci zatížení hello může pomoct toocontrol to. 
   * Zkontrolujte, jestli se zobrazuje procesoru vázaný na hello server sledováním hello `CPU` [mezipaměti metrika výkonu](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Žádosti brzo při Redis, hranice procesoru může způsobit ty žádostí tootimeout. tooaddress to distribuujete hello zatížení v rámci více horizontálních oddílů v mezipaměti premium, nebo upgradujte tooa větší velikost nebo cenová úroveň. Další informace najdete v tématu [překročení šířky pásma serveru straně](#server-side-bandwidth-exceeded).
5. Existují příkazů trvá dlouho tooprocess na hello serveru? Dlouho běžící příkazy, které trvá dlouho tooprocess na hello redis serveru může způsobit překročení časového limitu. Některé příklady příkazů dlouhotrvající `mget` s velkým počtem klíče, `keys *` nebo chybně napsané skripty lua. Můžete se připojit tooyour instanci Azure Redis Cache pomocí klienta hello redis rozhraní příkazového řádku nebo pomocí hello [konzola Redis](cache-configure.md#redis-console) a spuštění hello [SlowLog](http://redis.io/commands/slowlog) příkaz toosee Jestliže žádosti o trvá déle, než se očekávalo. Serveru redis a StackExchange.Redis jsou optimalizované pro mnoho malých požadavků místo méně velké požadavky. Rozdělení na menší bloky dat může zvýšit věcí sem. 
   
    Informace o připojení koncového bodu Azure Redis Cache SSL toohello pomocí rozhraní příkazového řádku redis a stunnel najdete v tématu hello [uvedení ASP.NET poskytovatele stavu relace pro Redis verze Preview](http://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx) příspěvku na blogu. Další informace najdete v tématu [SlowLog](http://redis.io/commands/slowlog).
6. Vysoké zatížení serveru Redis může způsobit překročení časového limitu. Zatížení serveru hello můžete monitorovat pomocí monitorování hello `Redis Server Load` [mezipaměti metrika výkonu](cache-how-to-monitor.md#available-metrics-and-reporting-intervals). Zatížení serveru 100 (maximální hodnota) označuje, že tento server hello redis byl zaneprázdněn prováděním žádné doba nečinnosti, po zpracování požadavků. toosee Pokud určité požadavky jsou zabírají všechny hello funkce serveru, spusťte příkaz SlowLog hello, jak je popsáno v předchozím odstavci hello. Další informace najdete v tématu [vysoké zatížení CPU / Server načíst](#high-cpu-usage-server-load).
7. Byl na straně klienta hello, která by mohla způsobit sítě blip jiná událost? Zkontrolujte na klientovi hello (web, role pracovního procesu nebo virtuálních počítačů Iaas), pokud byla událost jako nahoru nebo dolů škálování hello počet instancí klienta nebo nasazení nové verze klienta hello nebo automatické škálování je povolené? V našich testech, které zjistili jsme, že může způsobit škálování nebo škálování nahoru/dolů může být odchozí síťové připojení ke ztrátě pro několik sekund. Kód StackExchange.Redis je odolný toosuch události a bude znovu připojit. Během této doby opakovaného připojení může všech požadavků ve frontě hello vypršení časového limitu.
8. Byla žádost big předcházející několik malých požadavky toohello Redis Cache, který vypršel časový limit? Hello parametr `qs` hello chyba zpráva znamená, kolik žádosti byly odeslány z hello klienta toohello serveru, ale nebyly dosud zpracovány odpověď. Tuto hodnotu můžete zachovat narůstají, protože StackExchange.Redis používá jednoho připojení TCP a může číst pouze jedna odpověď najednou. To i v případě, že vypršel časový limit operace první hello, ale nezastaví hello dat odeslaných ze serveru hello a ostatní požadavky jsou zablokované to dokončení, která způsobila časové limity. Jedno řešení, je hello toominimize pravděpodobnost, že vypršení časových limitů zajistíte, že vaše mezipaměť je dostatečně velký pro úlohy a rozdělení na menší bloky velké hodnoty. Další možnou příčinou je toouse fond `ConnectionMultiplexer` objekty v vašeho klienta a vyberte hello alespoň načíst `ConnectionMultiplexer` při odesílání novou žádost. To by měl jeden časový limit zabránit v způsobuje vypršení časového limitu tooalso jiných požadavků.
9. Pokud používáte `RedisSessionStateprovider`, ujistěte se, nastavíte časový limit opakování hello správně. `retrytimeoutInMilliseconds`musí být vyšší než `operationTimeoutinMilliseonds`, jinak dojde k žádné opakování. V následující ukázka hello `retrytimeoutInMilliseconds` nastavena too3000. Další informace najdete v tématu [poskytovatele stavu relace ASP.NET pro Azure Redis Cache](cache-aspnet-session-state-provider.md) a [jak toouse hello parametry konfigurace poskytovatele stavu relace a poskytovatel výstupní mezipaměti](https://github.com/Azure/aspnet-redis-providers/wiki/Configuration).

    <add
      name="AFRedisCacheSessionStateProvider"
      type="Microsoft.Web.Redis.RedisSessionStateProvider"
      host="enbwcache.redis.cache.windows.net"
      port="6380"
      accessKey="…"
      ssl="true"
      databaseId="0"
      applicationName="AFRedisCacheSessionState"
      connectionTimeoutInMilliseconds = "5000"
      operationTimeoutInMilliseconds = "1000"
      retryTimeoutInMilliseconds="3000" />


1. Zkontrolujte využití paměti na serveru Azure Redis Cache hello [monitorování](cache-how-to-monitor.md#available-metrics-and-reporting-intervals) `Used Memory RSS` a `Used Memory`. Pokud zásady vyřazení je v místě, Redis spustí při vyřazení klíče `Used_Memory` dosáhnou hello velikost mezipaměti. V ideálním případě `Used Memory RSS` by měly být pouze mírně větší, než `Used memory`. Velký rozdíl znamená, že fragmentace paměti (interní nebo externí. Když `Used Memory RSS` je menší než `Used Memory`, znamená to, součástí hello mezipaměť byla vzájemně zaměněny hello operačním systému. V takovém případě můžete očekávat některé důležité latenci. Protože Redis nemá řízení přes způsobu jeho přidělení namapované toomemory stránky, vysoké `Used Memory RSS` je často výsledek hello špička využití paměti. Když Redis uvolní paměť, hello paměti je uveden zpět toohello allocator a hello allocator může nebo nemusí poskytnout hello paměti back toohello systému. Může být nesoulad mezi hello `Used Memory` hodnota a paměti spotřeba vykazované hello operačního systému. Může být kvůli toohello fakt paměti využité a vydané ve Redis, ale není zadaný back toohello systému. toohelp zmírnit problémy s pamětí můžete provést následující kroky hello.
   
   * Upgrade hello mezipaměti tooa větší velikost tak, že nejsou spuštěné zobrazení omezení paměti systému hello.
   * Nastavit dobu vypršení platnosti hello klíčům, aby starší hodnoty jsou proaktivně vyřazování.
   * Monitorování hello hello `used_memory_rss` metrika do mezipaměti. Když se tato hodnota blíží hello velikost mezipaměti, budete pravděpodobně toostart zobrazuje problémy s výkonem. Pokud používáte cache ve verzi premium, nebo upgradujte tooa větší velikost mezipaměti, distribuujte hello data mezi víc horizontálních oddílů.
   
   Další informace najdete v tématu [přetížení paměti na serveru hello](#memory-pressure-on-the-server).

## <a name="additional-information"></a>Další informace
* [Jaké nabídky a velikosti Redis Cache mám použít?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)
* [Jak mohu srovnávací test a otestovat hello výkonu Moje mezipaměti?](cache-faq.md#how-can-i-benchmark-and-test-the-performance-of-my-cache)
* [Jak můžete spouštět příkazy Redis?](cache-faq.md#how-can-i-run-redis-commands)
* [Jak toomonitor Azure mezipaměti Redis](cache-how-to-monitor.md)

