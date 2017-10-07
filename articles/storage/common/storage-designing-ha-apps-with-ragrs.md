---
title: "aaaDesigning vysoce dostupné aplikace pomocí Azure přístup pro čtení geograficky redundantní úložiště (RA-GRS) | Microsoft Docs"
description: "Jak toouse Azure RA-GRS úložiště tooarchitect vysokou dostupností aplikace flexibilní dostatek toohandle výpadků."
services: storage
documentationcenter: .net
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 8f040b0f-8926-4831-ac07-79f646f31926
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 1/19/2017
ms.author: robinsh
ms.openlocfilehash: e4a9fe7ef33eecd894408b3c1ef59920a248d1bd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="designing-highly-available-applications-using-ra-grs"></a>Navrhování pomocí RA-GRS vysoce dostupných aplikací.

Běžné funkce cloudové infrastruktury je poskytují vysokou dostupností platformu pro hostování aplikací. Vývojáři cloudové aplikace musíte zvážit, jak pečlivě tooleverage toto platformy toodeliver aplikace s vysokou dostupností tootheir uživatelé. Tento článek zaměřuje konkrétně na tom, jak mohou vývojáři použít hello Azure úložiště přístup pro čtení geograficky redundantní úložiště (RA-GRS) toomake svých aplikací, které jsou k dispozici další.

Existují čtyři možnosti pro redundanci – LRS (Locally Redundant Storage), ZRS (zóny redundantní úložiště), (Geo-Redundant Storage) GRS a RA-GRS (Geo-Redundant Storage přístup pro čtení). Přidáme toodiscuss GRS a RA-GRS v tomto článku. S GRS udržovaly tři kopie dat v hello primární oblasti, které jste vybrali při nastavování účtu úložiště hello. Tři další kopie je udržováno asynchronně v sekundární oblasti definované v Azure. RA-GRS je hello samé jako GRS, s tím rozdílem, že máte kopii sekundární toohello přístup pro čtení. Další informace o hello různé možnosti redundance úložiště Azure najdete v tématu [replikace Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-redundancy). Hello replikace článek také ukazuje hello dvojice primární a sekundární oblasti hello.

Existují fragmenty kódu, které jsou zahrnuté v tomto článku a ucelenou ukázku tooa odkaz na konci hello, který lze stáhnout a spustit.

## <a name="key-features-of-ra-grs"></a>Klíčové funkce RA-GRS

Než budeme mluvit o tom, toouse úložiště RA-GRS, budeme mluvit o svých vlastnostech a chování.

* Úložiště Azure udržuje kopii hello data, která ukládáte ve vaší primární oblasti v sekundární oblasti; jen pro čtení Jak jsme uvedli výše, služba úložiště hello Určuje umístění hello hello sekundární oblasti.

* Hello kopie jen pro čtení je [nakonec byl konzistentní](https://en.wikipedia.org/wiki/Eventual_consistency) s daty hello v primární oblasti hello.

* Pro objekty BLOB, tabulek a front, se můžete dotazovat hello sekundární oblast *čas poslední synchronizace* hodnotu, která oznamuje, kdy došlo k chybě hello poslední replikace z hello primární toohello sekundární oblasti. (To není podporováno pro úložiště Azure File, které nemá RA-GRS redundance v tuto chvíli).

* Klientská knihovna pro úložiště toointeract hello můžete používat s daty hello v primární nebo sekundární oblasti buď hello. Taky můžete přesměrovat čtení požadavky automaticky sekundární oblasti toohello když požadavků na čtení primární oblasti toohello časový limit.

* Pokud je hlavní problém ovlivňující usnadnění hello hello dat v hello primární oblasti, mohou aktivovat hello tým Azure geo-převzetí služeb při selhání, okamžiku budou položky DNS hello odkazující primární oblasti toohello změněné toopoint toohello sekundární oblast.

* Pokud dojde k selhání geograficky, bude Azure vyberte nové sekundární umístění a replikovat umístění toothat dat hello potom bodu hello tooit položky sekundární DNS. sekundární koncový bod Hello bude k dispozici, dokud účet úložiště hello dokončení replikace. Další informace najdete v tématu [co toodo, když dojde k výpadku Azure Storage](https://docs.microsoft.com/en-us/azure/storage/storage-disaster-recovery-guidance).

## <a name="application-design-considerations-when-using-ra-grs"></a>Aspekty návrhu aplikace při používání RA-GRS

hlavním účelem Hello tohoto článku je tooshow můžete jak toodesign aplikace, která bude pokračovat toofunction (i když v omezené kapacitou) ani v případě významnější havárie v primárním datovém hello hello center. To uděláte tak, že vaše aplikace toohandle přechodná nebo dlouhotrvající problémy tak, že došlo k potížím a přepnete tooread ze sekundární oblasti hello a přepnout zpět, když primární oblasti hello opět k dispozici.

### <a name="using-eventually-consistent-data"></a>Pomocí nakonec byl konzistentní dat

Toto navrhované řešení předpokládá, že případe tooreturn co může být zastaralá data toohello volající aplikace. Vzhledem k tomu, že jsou data sekundární hello nakonec byl konzistentní, je možné, že hello data byla zapsána toohello primární, ale toohello aktualizace hello sekundární kdyby dokončení replikace při hello primární oblasti jsou nedostupné.

Například zákazník může odeslat aktualizaci, která je úspěšné, a potom může hello primární přejděte před hello aktualizace šířený toohello sekundární. V takovém případě Pokud zákazník hello následně požádá tooread hello zpět data, dostane hello zastaralá data místo hello aktualizovat data. Pokud to je přijatelné a pokud ano, musíte rozhodnout, jak se zprávy hello zákazníka. Uvidíte, jak hello toocheck čas poslední synchronizace na sekundární data hello později v této toosee článku Pokud hello sekundární je aktuální.

### <a name="handling-services-separately-or-all-together"></a>Zpracování služby samostatně nebo společně všechny

Když není pravděpodobné, je možné toobecome jedna služba není k dispozici při hello jiných služeb jsou stále plně funkční. Vám může popisovač hello opakování režimu jen pro čtení pro jednotlivé služby a samostatně (objekty BLOB, fronty, tabulky), nebo může zpracovat opakování obecně pro všechny služby úložiště hello společně.

Například pokud používáte fronty a objekty BLOB ve vaší aplikaci, můžete rozhodnout tooput v opakovatelné chyby toohandle samostatné kódu pro každou z nich. Pak můžete získat z hello služby objektů blob zkuste to znovu, ale stále funguje hello fronty služby, bude ovlivněn pouze hello součástí aplikace, který zpracovává objekty BLOB. Pokud se rozhodnete toohandle obecně opakování všechny úložiště služby a služby objektů blob toohello volání vrátí Opakovatelná chyba a potom požadavky tooboth hello objektů blob a hello fronty služba bude mít vliv.

Nakonec to závisí na složitosti hello vaší aplikace. Můžete se rozhodnout není selhání hello toohandle službou, ale místo toho tooredirect požadavků pro všechna úložiště služby toohello sekundární oblast na čtení a hello aplikaci spustit v režimu jen pro čtení, když je v primární oblasti hello k problému s jakoukoli službu, úložiště.

### <a name="other-considerations"></a>Další důležité informace

Tyto jsou hello další důležité informace, které se budeme zabývat v hello zbývající části tohoto článku.

*   Zpracování opakování požadavků na čtení pomocí vzoru jistič hello

*   Nakonec konzistentní dat a hello čas poslední synchronizace

*   Testování

## <a name="running-your-application-in-read-only-mode"></a>Spuštění aplikace v režimu jen pro čtení

toouse RA-GRS úložiště, musí být schopný toohandle obou se nezdařilo požadavky na čtení a žádosti o aktualizaci se nezdařilo (s aktualizací v tomto případě znamená vložení, aktualizace a odstranění). Pokud hello selže primární datové centrum, přečtěte si požadavky se dají přesměrovaného toohello sekundárního datového centra, ale nelze žádosti o aktualizaci, protože hello sekundární je jen pro čtení. Z tohoto důvodu musíte některé způsob toorun aplikace v režimu jen pro čtení.

Například můžete nastavit příznak, který bude ověřen před odesláním služba úložiště toohello žádosti žádné aktualizace. Jeden z žádosti o aktualizaci hello vycházejí, můžete ji přeskočit a vrátit odpovídající odpověď toohello zákazníka. Může i chcete toodisable úplně určité funkce, dokud hello problém nevyřešíte a upozorněte uživatele, že tyto funkce jsou dočasně nedostupné.

Pokud se rozhodnete toohandle chyby pro každou službu samostatně, je také nutné toohandle hello možnost toorun aplikace v režimu jen pro čtení službou. Můžete mít pro každou službu, která se dá nastavit jen pro čtení příznaky a zakázán a popisovač hello odpovídající příznak hello příslušná místa v kódu.

Je možné toorun aplikace v režimu jen pro čtení obsahuje další výhody straně – nabízí možnost tooensure hello omezenou funkčnost během upgradu hlavní aplikace. Můžete aktivovat toorun vaší aplikace v jen pro čtení režim a toohello bod sekundární datové středisko, o zajištění, že když děláte upgrady, je nikdo přístup k datům hello v primární oblasti hello.

## <a name="handling-updates-when-running-in-read-only-mode"></a>Zpracování aktualizací při spuštění v režimu jen pro čtení

Existuje mnoho způsobů toohandle aktualizace požaduje při spuštění v režimu jen pro čtení. To jsme nebude komplexně pokrývat, ale obecně platí, existuje několik vzorů, které považujete za.

1.  Můžete reagovat tooyour uživatele a sdělte jim, že nejsou aktuálně přijímat aktualizace. Například systém správy kontaktů může povolit zákazníkům tooaccess kontaktní informace, ale zajistěte, aby nebyly aktualizace.

2.  Můžete zařadit vaše aktualizace v jiné oblasti. V takovém případě by zápisu tooa fronty požadavků čekající aktualizace v jiné oblasti a pak mít tooprocess způsob, jak tyto požadavky po hello primární datové centrum bude znovu online. V tomto scénáři by měl necháte zákazník hello vědět, že hello aktualizace požadované zařazených do fronty pro pozdější zpracování.

3.  Vaše aktualizace může zapisovat tooa účet úložiště v jiné oblasti. Pak když hello primární datové centrum přejde do režimu online, můžete mít toomerge způsob, jak tyto aktualizace na primární data hello, v závislosti na hello strukturu dat hello. Například pokud vytvoříte samostatné soubory pomocí razítka data a času v názvu text hello, můžete zkopírovat tyto soubory back toohello primární oblasti. Tento postup funguje pro některé úlohy, jako jsou data protokolování a iOT.

## <a name="handling-retries"></a>Zpracování opakování

Jak budete vědět, které chyby se opakovatelná? To je dáno hello Klientská knihovna pro úložiště. Například chybu 404 (prostředek nebyl nalezen) není opakovatelná, protože ho opakování není pravděpodobné tooresult v úspěch. Na hello druhé straně, 500 Chyba je opakovatelné protože je k serverové chybě a může být přechodný problém. Další podrobnosti, projděte si hello [otevřít zdrojový kód pro hello ExponentialRetry třída](https://github.com/Azure/azure-storage-net/blob/87b84b3d5ee884c7adc10e494e2c7060956515d0/Lib/Common/RetryPolicies/ExponentialRetry.cs) v Klientská knihovna pro úložiště hello .NET. (Vyhledejte hello metoda ShouldRetry.)

### <a name="read-requests"></a>Požadavky pro čtení

Požadavky pro čtení může být přesměrovaného toosecondary úložiště, pokud dojde k problému s primární úložiště. Uvedené výše v [pomocí nakonec konzistentních dat](#using-eventually-consistent-data), musí být přijatelné pro vaši aplikaci toopotentially číst zastaralá data. Pokud používáte hello úložiště klienta knihovny tooaccess RA-GRS data, můžete zadat hello chování opakovat požadavek čtení nastavením hodnoty pro hello **LocationMode** tooone vlastnost z hello následující:

*   **PrimaryOnly** (hello výchozí)

*   **PrimaryThenSecondary**

*   **SecondaryOnly**

*   **SecondaryThenPrimary**

Když nastavíte hello **LocationMode** příliš**PrimaryThenSecondary**, pokud počáteční hello číst primární koncový bod toohello požadavek selže s opakovatelnou chybou, hello klient automaticky provede další čtení žádost toohello sekundárního koncového bodu. Pokud chyba hello vypršení časového limitu serveru, pak hello klienta bude mít toowait pro časový limit tooexpire hello před obdrží Opakovatelná chyba od služby hello.

Při výběru jsou v podstatě dva scénáře tooconsider jak toorespond tooa Opakovatelná chyba:

*   Toto je problém s izolované a následné žádosti toohello primární endpoint nevrátí Opakovatelná chyba. Příklad, kdy k tomu může dojít je, když dojde k chybě přechodný síťový.

    V tomto scénáři není žádné snížení výkonu výrazné tomu, aby **LocationMode** nastavit příliš**PrimaryThenSecondary** jako tato situace nastane pouze zřídka.

*   Tento problém s alespoň jedním z hello služby úložiště v primární oblasti hello je a všechny následné žádosti toothat služby v rámci primární oblasti hello jsou pravděpodobně tooreturn opakovatelné chyby pro určitou dobu. Příkladem je, pokud je primární oblasti hello úplně nedostupná.

    V tomto scénáři je snížení výkonu vzhledem k tomu, že všechny vaše požadavky na čtení se pokuste se nejdřív hello primární koncový bod, počkejte tooexpire hello vypršení časového limitu a pak přepínače toohello sekundárního koncového bodu.

U těchto scénářů byste měli určit, existuje se probíhající problém s hello primární koncový bod a odeslání všech požadavků na čtení přímo sekundárním koncovým bodem toohello nastavením hello **LocationMode** vlastnost příliš **SecondaryOnly**. V tomto okamžiku by také změnit hello toorun aplikace v režimu jen pro čtení. Tento postup se označuje jako hello [jistič vzor](https://msdn.microsoft.com/library/dn589784.aspx).

### <a name="update-requests"></a>Žádosti o aktualizaci

vzor jistič Hello může být také použité tooupdate požadavky. Žádosti o aktualizaci však nemůže být přesměrovaného toosecondary úložiště, která je jen pro čtení. Pro tyto požadavky, je třeba ponechat hello **LocationMode** vlastností nastavenou příliš**PrimaryOnly** (hello výchozí). toohandle tyto chyby můžete použít metriky toothese požadavků – například 10 selhání v řádku – a vaše prahová hodnota při splnění, přepnout hello aplikace do režimu jen pro čtení. Můžete použít stejné metody pro vrácení tooupdate režimu tak, jak je popsáno níže v další části hello o hello jistič vzor hello.

## <a name="circuit-breaker-pattern"></a>Vzor jistič

Použití vzoru jistič hello v aplikaci, můžete zabránit v opakování operace, která je pravděpodobně toofail opakovaně. To umožňuje toorun toocontinue aplikace hello místo zabírají čas během operace hello je exponenciálnímu opakovat. Navíc rozpozná, když byl opraven hello odolnost, kdy aplikace hello čas můžete zkusit hello operaci opakujte.

### <a name="how-tooimplement-hello-circuit-breaker-pattern"></a>Jak tooimplement hello vzor jistič

tooidentify probíhající potížím s primární koncový bod, můžete sledovat, jak často hello klient zaznamená opakovatelné chyby. Protože každý případ se liší, máte toodecide na prahová hodnota hello toouse pro hello rozhodnutí tooswitch toohello sekundárního koncového bodu a spustit aplikaci hello v režimu jen pro čtení. Přepínač hello tooperform může například rozhodnout, pokud jsou v řádek s žádné úspěchů 10 selhání. Dalším příkladem je tooswitch, pokud selže 90 % hello požadavků v období 2 minut.

Pro hello prvního scénáře můžete jednoduše zachovat a počet selhání hello a pokud je úspěšné dříve, než dorazila hello hello maximální, nastavte počet back toozero. Pro druhý scénář hello jedním ze způsobů tooimplement je toouse hello MemoryCache objektu (v rozhraní .NET). Pro každý požadavek přidat do mezipaměti toohello CacheItem, nastavte hodnotu toosuccess hello (1) nebo selhání (0) a nastavte minut too2 čas vypršení platnosti hello od nyní (nebo vše, co je vaším omezením čas). Když je dosaženo času vypršení platnosti položky, položka hello je automaticky odebrána. Tím získáte okno postupného 2 minut. Pokaždé, když provedete služba úložiště toohello požadavek nejprve použijete dotaz Linq napříč hello MemoryCache objekt toocalculate hello procenta úspěšnosti souhrnné zpracování hello hodnoty a vydělí počet hello. Když je procento úspěšnosti hello klesne pod některé prahové hodnoty (například 10 %), nastavte hello **LocationMode** vlastnost požadavků na čtení příliš**SecondaryOnly** a přepněte hello aplikace do režimu jen pro čtení před budete pokračovat.

Prahová hodnota Hello chyb používá toodetermine při toomake hello přepínač může lišit tooservice služby ve vaší aplikaci, takže byste měli zvážit přitom konfigurovatelné parametry. Toto je také kde rozhodnete toohandle opakovatelné chyby z každé služby samostatně nebo jako jeden, jak je popsáno dříve.

Je potřeba vzít v úvahu jak toohandle více instancí aplikace a jaké toodo při zjištění opakovatelné chyby v každé instanci. Například můžete mít 20 virtuálních počítačů s hello načíst stejná aplikace. Samostatně každá instance zpracovávat? Pokud jedna instance spustí, dochází k problémům, chcete toolimit hello odpovědi toojust dané jedné instance nebo chcete tootry toohave všechny instance odpovídají v hello stejný způsobem v případě, že jedna instance má problém? Zpracování hello instancí samostatně je mnohem jednodušší než při odpovědi hello toocoordinate mezi nimi, ale tento postup závisí na architektuře vaší aplikace.

### <a name="options-for-monitoring-hello-error-frequency"></a>Možnosti sledování četnost chyb hello

Máte tři hlavní možnosti monitorování hello frekvenci opakování v hello primární oblasti v pořadí toodetermine tooswitch přes toohello sekundární oblasti a změňte hello toorun aplikace v režimu jen pro čtení.

*   Přidejte obslužnou rutinu pro hello [ **bude opakován** ](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.operationcontext.retrying.aspx) událostí na hello [ **informacím OperationContext** ](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.operationcontext.aspx) objektu předáte požadavků na úložiště tooyour – to je hello Metoda zobrazí v tomto článku a používá se v hello doplňujícími ukázka. Tyto události fire vždy, když klient hello opakovat žádost, umožňuje tootrack jak často hello klient zaznamená opakovatelné chyby na primární koncový bod.

    ```csharp 
    operationContext.Retrying += (sender, arguments) =>
    {
        // Retrying in hello primary region
        if (arguments.Request.Host == primaryhostname)
            ...
    };
    ```

*   V hello [ **Evaluate** ](http://msdn.microsoft.com/en-us/library/microsoft.windowsazure.storage.retrypolicies.iextendedretrypolicy.evaluate.aspx) metoda v zásadách vlastní opakování, můžete spustit vlastní kód vždy, když probíhá opakování. V přidání toorecording při opakovaném pokusu se stane, také to poskytuje hello možnost toomodify chování vaší opakování.

    ```csharp 
    public RetryInfo Evaluate(RetryContext retryContext,
    OperationContext operationContext)
    {
        var statusCode = retryContext.LastRequestResult.HttpStatusCode;
        if (retryContext.CurrentRetryCount >= this.maximumAttempts
        || ((statusCode &gt;= 300 && statusCode &lt; 500 && statusCode != 408)
        || statusCode == 501 // Not Implemented
        || statusCode == 505 // Version Not Supported
            ))
        {
        // Do not retry
            return null;
        }

        // Monitor retries in hello primary location
        ...

        // Determine RetryInterval and TargetLocation
        RetryInfo info =
            CreateRetryInfo(retryContext.CurrentRetryCount);

        return info;
    }
    ```

*   Hello třetí přístup je tooimplement vlastní monitorovací komponentu v aplikaci, které průběžně odešle příkaz ping váš koncový bod primárního úložiště s fiktivní číst toodetermine požadavků (jako je například čtení malých objektů blob), jeho stav. Bude to trvat až některé prostředky, ale není značné množství. Pokud problém je zjistit, která dosáhne vaší prahové hodnoty, by pak proveďte hello přepínač příliš**SecondaryOnly** a režimu jen pro čtení.

V určitém okamžiku budete chtít tooswitch back toousing hello primární koncový bod a že aktualizace. Pokud pomocí jedné z výše uvedených první dvě metody hello, můžete jednoduše přepnout zpět toohello primární koncový bod a povolit režim aktualizace po provedení nahodile vybrané množství času nebo počet operací. Pak můžete nechat ji znovu projít logika opakovaných pokusů hello. Pokud hello problém byl opraven, bude pokračovat primární koncový bod hello toouse a povolit aktualizace. Pokud stále existuje problém, ji budou jednou přepnout zpět toohello sekundární koncový bod a režimu jen pro čtení po selhání hello kritéria, které jste nastavili.

Třetí scénář hello, jakmile příkaz ping koncový bod primární úložiště hello opět úspěšné, můžete aktivovat přepínač hello zpět příliš**PrimaryOnly** a pokračovat v povolení aktualizace.

## <a name="handling-eventually-consistent-data"></a>Zpracování nakonec byl konzistentní dat

RA-GRS funguje tak, že replikace transakce z hello primární toohello sekundární oblast. Tento proces replikace zaručuje, že jsou data hello v sekundární oblasti hello *nakonec byl konzistentní*. To znamená, že všechny transakce hello v primární oblasti hello se nakonec objeví v sekundární oblasti hello, ale že mohou být prodleva předtím, než se objeví, a že neexistuje žádná záruka hello transakce přicházející do sekundární oblasti hello ve stejné pořadí jako hello ve kterém byly původně uplatňují v primární oblasti hello. Pokud vaše transakce přicházejí v sekundární oblasti hello mimo pořadí, můžete *může* zvažte vaše data v sekundární oblasti toobe hello v nekonzistentním stavu, dokud vyrovnat služby hello.

Hello následující tabulka znázorňuje příklad co může dojít v případě, že aktualizujete hello podrobnosti zaměstnanec toomake jí členem hello *správci* role. Hello zájmu v tomto příkladu, to vyžaduje, můžete aktualizovat hello **zaměstnanec** entitu a aktualizace **role správce** entity s počtem hello celkový počet správců. Všimněte si, jak hello se aktualizace mimo pořadí v sekundární oblasti hello.

| **Čas** | **Transakce**                                            | **Replikace**                       | **Čas poslední synchronizace** | **Výsledek** |
|----------|------------------------------------------------------------|---------------------------------------|--------------------|------------| 
| T0       | Transakce A: <br> Vložit zaměstnance <br> entity v primární |                                   |                    | Transakce A vložit tooprimary,<br> není ještě nereplikovaly. |
| T1       |                                                            | Transakce A <br> replikovat do<br> sekundární | T1 | Transakce A replikují toosecondary. <br>Čas poslední synchronizace aktualizovat.    |
| T2       | Transakce B:<br>Aktualizace<br> Zaměstnanec entity<br> v primární  |                                | T1                 | Transakce B zapsána tooprimary,<br> není ještě nereplikovaly.  |
| T3       | Transakce C:<br> Aktualizace <br>Správce<br>entitu role v<br>primární |                    | T1                 | Transakce zapsána tooprimary, C<br> není ještě nereplikovaly.  |
| *T4*     |                                                       | Transakce C <br>replikovat do<br> sekundární | T1         | Transakce C replikují toosecondary.<br>Nelze aktualizovat, protože LastSyncTime <br>ještě nebyla replikována transakce B.|
| *T5*     | Ke čtení entit <br>ze sekundární                           |                                  | T1                 | Získat hello zastaralé hodnotu pro zaměstnance <br> entity vzhledem k tomu, že nebyla transakce B <br> ještě nereplikovaly. Získat novou hodnotu hello<br> entitu role správce vzhledem k tomu, že má C<br> replikovat. Čas poslední synchronizace stále ještě<br> byla aktualizovat, protože transakce B<br> nebyla replikována. Můžete zjistit<br>Entita role správce je nekonzistentní <br>protože hello entity datum a čas je po <br>Hello čas poslední synchronizace. |
| *T6*     |                                                      | Transakce B<br> replikovat do<br> sekundární | T6                 | *T6* – mít všechny transakce prostřednictvím C <br>byla replikovat, čas poslední synchronizace<br> je aktualizována. |

V tomto příkladu se předpokládá hello klienta přepínače tooreading ze sekundární oblasti hello v T5. Ho můžou číst hello **role správce** entity v tuto chvíli hello entity však obsahuje hodnotu pro počet hello správců, který není konzistentní s počtem hello **zaměstnanec** entity v tuto chvíli jsou označené jako správci v sekundární oblasti hello. Váš klient jednoduše zobrazit tuto hodnotu s hello riziko, že je nekonzistentní informace. Alternativně hello klienta může pokusit toodetermine této hello **role správce** je ve stavu potenciálně nekonzistentní, protože aktualizace hello dojít mimo pořadí a potom informovat uživatele hello o této skutečnosti.

toorecognize, že obsahuje potenciálně nekonzistentní data, hello klienta můžete použít hodnotu hello hello *čas poslední synchronizace* , můžete se kdykoli dotazováním služby úložiště. Znamená to, kdy byl naposledy hello data v sekundární oblasti hello čas hello konzistentní a kdy se měl použít hello služby hello transakce předchozí toothat bodu v čase. Hello příkladu výše, jakmile služba hello vloží hello **zaměstnanec** entity v sekundární oblasti hello hello čas poslední synchronizace je nastaven příliš*T1*. Zůstane v *T1* dokud aktualizace služby pro hello hello **zaměstnanec** entity v hello sekundární oblasti, kde se nastaví příliš*T6*. Pokud klient hello načte hello čas poslední synchronizace, když přečte hello entity v *T5*, ho můžete porovnat s časovým razítkem hello u hello entity. Pokud je novější než hello hello časové razítko hello entity od poslední synchronizace času, potom hello entita je ve stavu potenciálně nekonzistentní a může trvat vše, co je hello vhodnou pro vaši aplikaci. Použít toto pole je nutné vědět, kdy byla dokončena hello poslední aktualizace toohello primární.

## <a name="testing"></a>Testování

Je důležité tootest, který vaše aplikace funguje podle očekávání, pokud se setká s opakovatelnou chyby. Například musíte tootest, který hello sekundární toohello přepínače aplikace a do režimu jen pro čtení při zjistí problém a přepínače zpět když primární oblasti hello opět k dispozici. toodo, způsob toosimulate opakovatelné chyby a budete řídit, jak často k nim dojde.

Můžete použít [Fiddler](http://www.telerik.com/fiddler) toointercept a upravit odpovědi HTTP ve skriptu. Tento skript můžete identifikovat odpovědi, které pocházejí z váš primární koncový bod a změnit tooone kód stavu HTTP hello této hello Klientská knihovna pro úložiště rozpozná jako Opakovatelná chyba. Tento fragment kódu ukazuje jednoduchý příklad Fiddler skriptu, který zabrání odpovědí tooread požadavky DHCP proti hello **employeedata** tooreturn tabulky 502 stavu:

```java
static function OnBeforeResponse(oSession: Session) {
    ...
    if ((oSession.hostname == "\[yourstorageaccount\].table.core.windows.net")
      && (oSession.PathAndQuery.StartsWith("/employeedata?$filter"))) {
        oSession.responseCode = 502;
    }
}
```

Můžete rozšířit tento příklad toointercept většímu počtu požadavků a změnit jedině tak hello **responseCode** u některých z nich toobetter simulovat scénářem z reálného prostředí. Další informace o přizpůsobení Fiddler skriptů najdete v tématu [úprava požadavek nebo odpověď](http://docs.telerik.com/fiddler/KnowledgeBase/FiddlerScript/ModifyRequestOrResponse) v hello dokumentace k aplikaci Fiddler.

Pokud jste provedli hello prahové hodnoty pro přepínání vaší aplikace tooread režimu pouze konfigurovat, bude jednodušší chování hello tootest se svazky mimo produkční transakce.

## <a name="next-steps"></a>Další kroky

* Další informace o přístup pro čtení geografická redundance, včetně další příklad nastavení hello LastSyncTime, najdete v tématu [možnosti redundance úložiště Windows Azure a geograficky redundantní úložiště s přístupem pro čtení](https://blogs.msdn.microsoft.com/windowsazurestorage/2013/12/11/windows-azure-storage-redundancy-options-and-read-access-geo-redundant-storage/).

* Kompletní příklad znázorňující, jak toomake hello přepínač přepínat mezi koncovými body hello primární a sekundární, najdete v tématu [vzorků Azure – použití hello jistič vzor s úložištěm RA-GRS](https://github.com/Azure-Samples/storage-dotnet-circuit-breaker-pattern-ha-apps-using-ra-grs).
