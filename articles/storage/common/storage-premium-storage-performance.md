---
title: "Azure Premium Storage: Návrh pro výkonu | Microsoft Docs"
description: "Návrh vysoce výkonné aplikace pomocí Azure Premium Storage. Premium Storage nabízí podporu vysoce výkonné, nízkou latencí disku pro I náročnými úlohy běžící na virtuálních počítačích Azure."
services: storage
documentationcenter: na
author: aungoo-msft
manager: tadb
editor: tysonn
ms.assetid: e6a409c3-d31a-4704-a93c-0a04fdc95960
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/27/2017
ms.author: aungoo
ms.openlocfilehash: dde3e60ae4c8387150b65f0715166b5d549891e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-premium-storage-design-for-high-performance"></a>Úložiště Azure Premium: Návrh vysoce výkonné
## <a name="overview"></a>Přehled
Tento článek obsahuje pokyny pro vytváření vysoce výkonné aplikace pomocí Azure Premium Storage. Můžete použít hello pokyny uvedené v tomto dokumentu v kombinaci s výkonu osvědčených postupů použít tootechnologies používá vaše aplikace. tooillustrate hello pokyny, jsme použili SQL Server běžící na Storage úrovně Premium jako příklad v tomto dokumentu.

Když jsme adres scénáře výkonu pro vrstvy úložiště hello v tomto článku, budete potřebovat toooptimize hello aplikační vrstvu. Například pokud hostujete farmy služby SharePoint na Azure Premium Storage, můžete příklady systému SQL Server hello z tohoto článku toooptimize hello databáze serveru. Kromě toho optimalizace farmy služby SharePoint hello webového serveru a aplikace serveru tooget hello většina výkonu.

Tento článek vám pomůže odpovědí následující běžné otázky o optimalizaci výkonu aplikace na Azure Premium Storage

* Jak toomeasure výkon aplikace?  
* Proč se zobrazuje očekávané vysoký výkon?  
* Faktory, které mají vliv na výkon vaší aplikace na Storage úrovně Premium?  
* Jak se tyto faktory ovlivňují výkon aplikace na Storage úrovně Premium  
* Jak můžete můžete optimalizovat pro IOPS, šířky pásma a latencí?  

Uvádíme těchto pokynů speciálně pro Storage úrovně Premium, protože úlohy běžící na Storage úrovně Premium jsou vysoce citlivých výkonu. Kde je to vhodné uvádíme příklady. Můžete taky použít některé z těchto pokynů tooapplications s disky standardní úložiště pro virtuální počítače IaaS.

Než začnete, pokud jste tooPremium nové úložiště, nejdřív přečíst hello [úložiště Premium: vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../storage-premium-storage.md) a [Azure úložiště škálovatelnost a cíle výkonnosti](storage-scalability-targets.md)články.

## <a name="application-performance-indicators"></a>Ukazatele výkonu aplikace
Jsme vyhodnocení, zda aplikace pracuje správně nebo není pomocí, jako jsou ukazatele výkonu, jak rychle aplikace je zpracování požadavku uživatele, kolik dat aplikace zpracovává každý požadavek, kolik požadavků aplikace zpracovává v konkrétní dobu, jak dlouho má uživatel toowait tooget odpověď po odeslání žádosti. Hello technické podmínky pro tyto ukazatele výkonu jsou IOP, propustnosti nebo šířky pásma a latence.

V této části se budeme zabývat hello běžné ukazatele výkonu v kontextu hello úložiště Premium Storage. V hello následující části, shromažďování požadavky na aplikace, se dozvíte, jak toomeasure tyto ukazatele výkonu pro vaši aplikaci. Později v optimalizace výkonu aplikací se dozvíte o hello faktorů, které ovlivňují tyto indikátory a doporučení toooptimize výkonu je.

## <a name="iops"></a>IOPS
IOPS je počet požadavků, že vaše aplikace odesílá toohello disky úložiště za jednu sekundu. Vstupně výstupní operace by mohl číst nebo zapisovat sekvenční nebo náhodné. OLTP aplikace jako webem online prodejní nutné tooprocess mnoho souběžných uživatel požádá o okamžitě. žádosti uživatelů Hello jsou vložení a aktualizace náročné databázové transakce, které aplikace hello musí rychle zpracovat. Proto OLTP aplikace vyžadují velmi vysoký IOPS. Takové aplikace zpracovává miliony požadavků malé a náhodných vstupně-výstupní operace. Pokud máte takové aplikace, je třeba navrhnout toooptimize infrastruktury aplikace hello iops. V hello později části *optimalizace výkonu aplikace*, probereme podrobně všechny hello faktory, které je nutné zvážit tooget vysokou IOPS.

Když připojíte premium úložiště disku tooyour vysoké škálování virtuálních počítačů, Azure poskytuje pro vás zaručenou počet IOPS podle specifikace disku hello. Například P50 disk zřídí 7500 IOPS. Každý měřítko velikost virtuálního počítače má také konkrétní limit IOPS, která dokáže odolat. Například standardní virtuální počítač GS5 má 80 000 IOPS omezit.

## <a name="throughput"></a>Propustnost
Propustnost nebo šířky pásma je hello množství dat, že vaše aplikace odesílá toohello disky úložiště v určeném intervalu. Pokud vaše aplikace provádí vstupně-výstupních operací s velikostí jednotky velké vstupně-výstupní operace, vyžaduje Vysoká propustnost. Datový sklad aplikace mívají tooissue kontroly náročné přístup velká část dat najednou a operací běžně provádět hromadné operace. Jinými slovy tyto aplikace vyžadují vyšší propustnost. Pokud máte takové aplikace, je třeba navrhnout jeho toooptimize infrastruktury pro propustnost. V další části hello probereme v podrobností hello faktory musí tooachieve optimalizovat to.

Když připojíte premium úložiště disku tooa vysoké škálování virtuálních počítačů, Azure zřizuje propustnost podle specifikace tohoto disku. Například P50 disk zřídí 250 MB na druhý disk propustnost. Každý měřítko velikost virtuálního počítače má také jako konkrétní omezení propustnosti, které dokáže odolat. Například standardní GS5 virtuální počítač má maximální propustnost 2 000 MB za sekundu. 

Je vztah mezi propustnost a IOPS, jak je znázorněno v hello vzorec níže.

![](media/storage-premium-storage-performance/image1.png)

Proto je důležité toodetermine hello optimální propustnosti a IOPS hodnoty, které vaše aplikace vyžaduje. Jako zkusíte toooptimize jeden, získá vliv také hello jiné. V další části *optimalizace výkonu aplikace*, se budeme zabývat v podrobnější informace o optimalizaci IOPS a propustnosti.

## <a name="latency"></a>Latence
Latence je hello doby potřebné aplikaci tooreceive jedné žádosti, odešle disky úložiště toohello a odeslat hello odpovědi toohello klienta. Toto je důležité míra výkonu aplikace v přidání tooIOPS a propustnosti. Hello latence disk úložiště premium je čas hello přebírá tooretrieve hello informace pro žádost a komunikaci zpátky tooyour aplikace. Storage úrovně Premium poskytuje trvale nízké latence. Pokud povolíte ukládání do mezipaměti na discích úložiště premium hostitele jen pro čtení, můžete získat mnohem nižší latenci pro čtení. Se budeme zabývat ukládání do mezipaměti disku podrobněji v pozdější části na *optimalizace výkonu aplikace*.

Když jsou optimalizace vaší aplikace tooget vyšší IOPS a propustnost, bude mít vliv hello latence vaší aplikace. Po vyladění výkonu aplikace hello, vždy vyhodnoceny hello latence tooavoid aplikace hello chování neočekávané vysokou latencí.

## <a name="gather-application-performance-requirements"></a>Shromáždění požadavků na výkon aplikace
Hello prvním krokem při navrhování vysoký výkon aplikací běžících na Azure Premium Storage je toounderstand hello požadavcích na výkon vaší aplikace. Po shromáždění požadavků na výkon, můžete optimalizovat výkon aplikace tooachieve hello optimální.

V předchozí části hello jsme vysvětlené hello běžné ukazatele výkonu, IOP, propustnosti a latence. Musíte určit, které tyto ukazatele výkonu jsou kritické tooyour aplikace toodeliver hello potřeby činnost koncového uživatele. Například vysoké IOPS důležitý většinu aplikací tooOLTP zpracování miliony transakcí za sekundu. Vzhledem k tomu, Vysoká propustnost je velmi důležitá pro datový sklad aplikací zpracování velkých objemů dat za sekundu. Velmi nízkou latenci je zásadní pro aplikace v reálném čase jako živé video streamování weby.

V dalším kroku měření hello požadavky na maximální výkon vaší aplikace v průběhu své životnosti. Kontrolní seznam ukázka hello níže použijte jako spuštění. Požadavky na maximální výkon záznamů hello během normální, tečky zatížení ve špičce a počítačem nepracujete. Tím, že určíte požadavky pro všechny úrovně zatížení, budete moct toodetermine hello požadavek na celkový výkon vaší aplikace. Hello běžné pracovní zatížení webem elektronického obchodování například bude hello transakcí, které slouží během většina dnů v roce. zatížení ve špičce Hello hello webu bude hello transakcí, které slouží během sváteční sezóny nebo speciální prodej události. zatížení ve špičce Hello je obvykle zkušeného po omezenou dobu, ale může vyžadovat vaše aplikace tooscale dva nebo více krát jeho normální provoz. Zjistíte hello 50. percentil, 90 percentilu a 99 percentilu požadavky. To pomáhá vyfiltrovat odlehlé všechny hodnoty v požadavcích na výkon hello a vaše úsilí můžete soustředit na optimalizace pro správné hodnoty hello.

**Kontrolní seznam požadavků výkonu aplikace**

| **Požadavky na výkon** | **50. percentil** | **90 percentilu** | **99 percentilu** |
| --- | --- | --- | --- |
| Max. Transakce za sekundu | | | |
| Operace čtení % | | | |
| Operace zápisu % | | | |
| % Náhodné operace | | | |
| Sekvenční operace % | | | |
| Velikost požadavku vstupně-výstupní operace | | | |
| Průměrná propustnost | | | |
| Max. Propustnost | | | |
| Min. Latence | | | |
| Průměrná latence | | | |
| Max. Procesor | | | |
| Průměrné využití procesoru | | | |
| Max. Memory (Paměť) | | | |
| Průměrná paměti | | | |
| Hloubka fronty | | | |

> [!NOTE]
> Měli byste zvážit škálování tato čísla podle očekávané budoucímu růstu vaší aplikace. Je vhodné tooplan pro růst předem, protože by mohlo být těžší infrastrukturu hello toochange pro zlepšení výkonu později.
>
>

Pokud máte existující aplikaci a chcete toomove tooPremium úložiště, nejprve vytvoříte kontrolní seznam hello výše pro existující aplikace hello. Potom vytvořit prototyp vaší aplikace na Storage úrovně Premium a návrhu aplikace hello podle pokynů popsaných v *optimalizace výkonu aplikace* v další části tohoto dokumentu. Hello další část popisuje hello nástroje můžete použít měření výkonu toogather hello.

Vytvořte kontrolní seznam podobné tooyour stávající aplikaci pro hello prototypu. Pomocí nástrojů Benchmarking můžete simulovat hello úlohy a měření výkonu na prototypu aplikace hello. Části hello na [Benchmarking](#benchmarking) toolearn Další. Pomocí tohoto postupu, abyste mohli ověřit, zda úložiště Premium můžete odpovídat nebo překročí maximální vašim požadavkům na výkon aplikace. Pak můžete implementovat hello stejné pokyny pro produkční aplikace.

### <a name="counters-toomeasure-application-performance-requirements"></a>Čítače výkonu požadavky na toomeasure aplikace
Dobrý den nejlepší způsob, jak toomeasure požadavcích na výkon vaší aplikace, je toouse sledování výkonu nástroje poskytované operačním systémem hello hello serveru. Můžete použít nástroj PerfMon pro systém Windows a iostat pro Linux. Tyto nástroje zachytit čítače odpovídající tooeach měr podrobně hello nad sekcí. Hello hodnoty z těchto čítačů nutné zaznamenat, když aplikace běží jeho normální, zatížení ve špičce a počítačem nepracujete.

Hello PerfMon čítače jsou k dispozici pro procesor, paměť a každý logický disk a fyzický disk serveru. Pokud používáte prémiové disky úložiště v případě virtuálních počítačů, čítače hello fyzického disku se pro každý disk úložiště premium a čítače logický disk se pro každý svazek na discích úložiště premium hello vytvořit. Nutné zaznamenat hello hodnoty pro hello disky, které hostují vaše úlohy aplikace. Pokud existuje jedno mapování tooone mezi logické a fyzické disky, najdete čítače disku toophysical; v opačném případě najdete toohello logického disku čítače. V systému Linux příkaz iostat hello generuje sestavy využití procesoru a disku. Sestava využití disku Hello poskytuje statistické údaje za fyzického zařízení nebo oddíl. Pokud máte databázový server s protokolu a data na různých discích, shromážděte tato data pro oba disky. Následující tabulka popisuje čítače pro disky, procesoru a paměti:

| Čítač | Popis | PerfMon | Iostat |
| --- | --- | --- | --- |
| **IOPS nebo transakcí za sekundu** |Počet vstupně-výstupní požadavky vydané toohello úložiště disku za sekundu. |Čtení disku/s <br> Zápis disku/s |TPS <br> r/s <br> w/s |
| **Disk čtení a zápisu** |% čtení a zápisu operace provedené na disku hello. |Čas čtení disku v % <br> Čas zápisu disku v % |r/s <br> w/s |
| **Propustnost** |Množství dat číst nebo zapisovat toohello disku za sekundu. |Čtení z disku bajtů/s <br> Bajty zapisování na disk/s |kB_read/s <br> kB_wrtn/s |
| **Latence** |Celkový čas toocomplete žádost o vstupně-výstupní operace disku. |Průměrná doba disku/čtení <br> Doba průměrná disku/zápis |await <br> svctm |
| **Velikost vstupně-výstupní operace** |Hello velikost vstupně-výstupní požadavky vydává toohello disky úložiště. |Průměrná disku bajtů/čtení <br> Průměrná disku bajtů/zápis |avgrq sz |
| **Hloubka fronty** |Počet nezpracovaných vstupně-výstupních požadavků čekání toobe formuláře číst nebo zapisovat toohello úložiště disku. |Aktuální délka fronty disku |avgqu sz |
| **Max. Paměť** |Velikost paměti požadované toorun aplikace bez problémů |% Využití potvrzených bajtů |Použití vmstat |
| **Max. VYUŽITÍ PROCESORU** |Velikost procesoru požadované toorun aplikace bez problémů |% Času procesoru |% util |

Další informace o [iostat](http://linuxcommand.org/man_pages/iostat1.html) a [PerfMon](https://msdn.microsoft.com/library/aa645516.aspx).

## <a name="optimizing-application-performance"></a>Optimalizace výkonu aplikace
Hello hlavní faktory, které mají vliv na výkon aplikace běžící na Storage úrovně Premium jsou povaze z vstupně-výstupní požadavky, velikost virtuálního počítače, velikost disku, počet disků, ukládání do mezipaměti na disku, Multithreading a hloubku fronty. Můžete ovládat některé tyto faktory s knoflíky poskytované systémem hello. Většina aplikací nemusí dát možnost tooalter hello velikost vstupně-výstupní operace a hloubku fronty přímo. Například pokud používáte systém SQL Server, nemůžete hello hloubka velikost a fronty vstupně-výstupní operace. SQL Server zvolí hello optimální vstupně-výstupní operace velikost a fronty hloubka hodnoty tooget hello většina výkonu. Je důležité toounderstand hello účinky oba typy faktory na výkon aplikace tak, aby můžete zřídit požadavkům na výkon toomeet odpovídající prostředky.

V této části najdete toohello aplikace požadavky kontrolní seznam, který jste vytvořili, tooidentify kolik budete potřebovat toooptimize výkon aplikace. Na základě, že bude mít toodetermine který faktory z této části můžete potřebovat tootune. toowitness hello důsledky každý faktor na vaše aplikace výkon, spusťte srovnávací testy nástroje na nastavení aplikace. Odkazovat toohello [Benchmarking](#Benchmarking) části na konci hello tohoto článku kroky toorun běžné srovnávací testy nástroje na systém Windows a virtuální počítače s Linuxem.

### <a name="optimizing-iops-throughput-and-latency-at-a-glance"></a>Optimalizace IOP, propustnosti a latence na první pohled
Následující tabulka Hello shrnuje všechny faktory hello výkonu a hello kroky toooptimize IOPS, propustnosti a latence. Hello následující Toto shrnutí částech se popisují každou. faktor je mnohem víc hloubka.

| &nbsp; | **IOPS** | **Propustnost** | **Latence** |
| --- | --- | --- | --- |
| **Příklad scénáře** |Enterprise OLTP aplikace, které vyžadují velmi vysoký transakce za druhá míra. |Enterprise datového skladu aplikace zpracování velkých objemů dat. |Téměř v reálném čase aplikace vyžadující rychlých odpovědí toouser požadavků, jako je online herní. |
| Faktory výkonu | &nbsp; | &nbsp; | &nbsp; |
| **Velikost vstupně-výstupní operace** |Menší velikost vstupně-výstupní operace dosáhnout vyšší IOPS. |Větší velikost tooyields vstupně-výstupní operace vyšší propustnost. | &nbsp;|
| **Velikost virtuálního počítače** |Použijte velikost virtuálního počítače, který nabízí IOPS větší než požadavků vaší aplikace. Zobrazit velikosti virtuálních počítačů a jejich IOPS omezení. |Velikost virtuálního počítače pomocí omezení propustnosti větší než požadavků vaší aplikace. Zobrazit velikosti virtuálních počítačů a jejich propustnost omezení. |Použijte velikost virtuálního počítače, že nabízí škálování omezení větší než požadavků vaší aplikace. Zobrazit velikosti virtuálních počítačů a jejich omezení sem. |
| **Velikost disku** |Použijte velikost disku, který nabízí IOPS větší než požadavků vaší aplikace. Zobrazit velikosti disků a jejich IOPS omezení. |Velikost disku pomocí omezení propustnosti větší než požadavků vaší aplikace. Zobrazit velikosti disků a jejich propustnost omezení. |Použijte velikost disku, že nabízí škálování omezení větší než požadavků vaší aplikace. Zobrazit velikosti disků a jejich omezení sem. |
| **Virtuální počítač a limity škálování disku** |Vybrat velikost virtuálního počítače hello limit IOPS musí být větší než celkový počet IOPS doprovází prémiové disky úložiště připojené tooit. |Propustnost limit vybrat velikost virtuálního počítače hello musí být větší než celková propustnost doprovází prémiové disky úložiště připojené tooit. |Limity škálování služby vybrat velikost virtuálního počítače hello musí být větší limity škálování celkový disky úložiště připojené premium. |
| **Ukládání do mezipaměti na disku** |Povolit mezipaměť jen pro čtení na discích úložiště premium s tooget velkou operace čtení vyšší IOPS pro čtení. | &nbsp; |Povolte mezipaměť jen pro čtení na discích úložiště premium se připravené velkou operations tooget velmi nízkou latenci pro čtení. |
| **Prokládání disků** |Použít několik disků a jejich rozkládají společně tooget kombinované vyšší IOPS a omezení propustnosti. Všimněte si, že hello kombinovaný limit na jednu virtuální počítač musí být vyšší než hello kombinované omezení připojené prémiové disky. | &nbsp; | &nbsp; |
| **Velikost stripe** |Menší velikost stripe pro náhodné malé vstupně-výstupní operace vzor vidět v aplikacích OLTP. Například pro aplikaci SQL Server OLTP použijte stripe velikost 64KB. |Větší velikost stripe pro sekvenční velké vstupně-výstupní operace vzor vidět v aplikacích datového skladu. Například použijte velikost 256KB prokládání pro aplikaci SQL Server datového skladu. | &nbsp; |
| **Více vláken** |Použijte více vláken toopush vyšší počet požadavků tooPremium úložiště, které povede toohigher IOPS a propustnosti. Například na serveru SQL Server nastavit vysokou hodnotu tooallocate MAXDOP více procesorů tooSQL serveru. | &nbsp; | &nbsp; |
| **Hloubka fronty** |Větší hloubky fronty dosáhnout vyšší IOPS. |Větší hloubky fronty dosáhnout vyšší propustnost. |Menší hloubka fronty vypočítá nižší latenci. |

## <a name="nature-of-io-requests"></a>Povaha vstupně-výstupní požadavky
Žádost o vstupně-výstupní operace je jednotka vstupně výstupní operace, které vaše aplikace bude provádět. Identifikace hello povaha vstupně-výstupní požadavky, náhodných nebo sekvenčních, čtení a zápisu, malý nebo velký, vám pomůže určit hello požadavky na výkon vaší aplikace. Je velmi důležité toounderstand hello povaha vstupně-výstupní požadavky, toomake hello správné rozhodnutí při navrhování infrastruktury aplikací.

Velikost vstupně-výstupní operace je jedním z hello více důležité faktory. Hello velikost vstupně-výstupní operace je velikost hello hello vstupně výstupní operace žádosti vygenerované vaší aplikace. Hello velikost vstupně-výstupní operace má značný vliv na výkon, hlavně na hello IOPS a šířky pásma, která hello aplikace je možné tooachieve. Hello následující vzorec znázorňuje hello vztah mezi IOPS, velikost vstupně-výstupní operace a šířky pásma nebo propustnosti.  
    ![](media/storage-premium-storage-performance/image1.png)

Některé aplikace umožňují tooalter velikost jejich vstupně-výstupní operace, zatímco některé aplikace nepodporují. Určuje hello optimální velikost vstupů/výstupů samotné například SQL Server a všechny knoflíky toochange neposkytuje uživatelé ho. Na hello druhé straně, Oracle poskytuje parametr s názvem [DB\_BLOKOVAT\_velikost](https://docs.oracle.com/cd/B19306_01/server.102/b14211/iodesign.htm#i28815) pomocí které můžete nakonfigurovat hello velikost požadavku vstupně-výstupních operací hello databáze.

Pokud používáte aplikaci, která jste toochange hello velikost vstupně-výstupní operace není povolena, použijte tento článek toooptimize hello výkonu klíčového ukazatele výkonu, který je nejdůležitější aplikace tooyour hello pokyny. Například:

* OLTP aplikace vygeneruje miliony požadavků malé a náhodných vstupně-výstupní operace. požádá o toohandle tyto typ vstupně-výstupní operace, je třeba navrhnout vaše aplikace infrastruktury tooget vyšší IOPS.  
* Datového skladu aplikace generuje velké a sekvenčních vstupně-výstupní požadavky. toohandle tyto typ vstupně-výstupní požadavky, je třeba navrhnout vaše aplikace tooget infrastruktury větší šířku pásma a propustnosti.

Pokud používáte aplikaci, která vám umožní toochange hello vstupně-výstupní operace velikost, použijte toto pravidlo pro hello vstupně-výstupní operace velikost kromě pokynů pro tooother výkonu

* Menší velikost tooget vstupně-výstupní operace vyšší IOPS. Například 8 KB pro aplikaci OLTP.  
* Větší velikost tooget vstupně-výstupní operace vyšší šířku pásma nebo propustnost. Například 1 024 KB datového skladu aplikace.

Tady je příklad na tom, jak můžete vypočítat hello IOPS a propustnost nebo šířky pásma pro vaši aplikaci. Vezměte v úvahu aplikace pomocí P30 disku. Hello maximální IOPS a propustnost nebo šířky pásma můžete dosáhnout P30 disku je 5000 IOPS a 200 MB za sekundu v uvedeném pořadí. Nyní Pokud vaše aplikace vyžaduje hello maximální IOPS z hello P30 disk a použít menší velikost vstupně-výstupní operace, jako je 8 KB hello výsledná šířky pásma, nebudete moct tooget je 40 MB za sekundu. Ale pokud vaše aplikace vyžaduje hello maximální propustnosti nebo šířky pásma z disku P30 a použijete větší velikost vstupně-výstupní operace, jako je 1024 KB, hello výsledné IOPS bude menší, 200 IOPS. Tedy vylaďte velikost vstupně-výstupní operace hello tak, že splňuje požadavek IOPS a propustnost nebo šířky obě aplikace. Následující tabulka shrnuje různé velikosti vstupně-výstupní operace hello a jejich odpovídající IOPS a propustnost pro P30 disk.

| Požadavků aplikace | Velikost vstupně-výstupních operací | IOPS | Propustnost nebo šířky pásma |
| --- | --- | --- | --- |
| Maximální IOPS |8 kB |5,000 |40 MB za sekundu |
| Maximální propustnost |1024 KB |200 |200 MB za sekundu |
| Maximální propustnost + vysoké IOPS |64 kB |3,200 |200 MB za sekundu |
| Maximální IOPS + Vysoká propustnost |32 KB. |5,000 |160 MB za sekundu |

tooget IOPS a šířky pásma, vyšší než maximální hodnota hello úložiště disku jednoho premium, použijte více prémiové disky rozdělená společně. Například rozkládají dva disky tooget P30 součet IOPS v 10 000 IOPS nebo kombinované propustnost 400 MB za sekundu. Jak je vysvětleno v další části hello, je nutné použít velikost virtuálního počítače, který podporuje hello kombinaci disku IOPS a propustnosti.

> [!NOTE]
> Jak zvýšit buď IOPS nebo také zvyšuje propustnost hello jiné, zkontrolujte, zda že není stisknutí tlačítka propustnost nebo IOPS omezení hello disk nebo virtuální počítač při zvýšení buď jedné.
>
>

toowitness hello účinky velikost vstupů/výstupů na výkon aplikace, můžete spustit testu typovou úlohou nástroje na virtuální počítač a disky. Vytvořit více testovacích bězích a použít jinou velikost vstupně-výstupní operace pro každé spuštění toosee hello dopad. Odkazovat toohello [Benchmarking](#Benchmarking) oddíl hello konci tohoto článku Další podrobnosti.

## <a name="high-scale-vm-sizes"></a>Měřítko velikosti virtuálních počítačů
Když spustíte navrhování aplikace, jedním z první věcí toodo hello je, vyberte toohost virtuálních počítačů vaší aplikace. Storage úrovně Premium se dodává s velikosti vysoké škálování virtuálního počítače, které můžete spustit aplikace vyžadující vyšší výpočetní výkon a vysokou místního disku vstupně-výstupní výkon. Tyto virtuální počítače zadejte rychlejších procesorů vyšší poměr paměti jádra a Solid-State Drive (SSD) pro místní disk hello. Příklady vysoké virtuálních počítačů škálování. podpora Storage úrovně Premium jsou hello DS, DSv2 a GS řady virtuálních počítačů.

Vysoká škálování virtuálních počítačů jsou k dispozici v různých velikostech s různým počtem jader procesoru, paměti, operačního systému a velikost dočasné disku. Každý velikost virtuálního počítače má také maximální počet datových disků, můžete připojit toohello virtuálních počítačů. Proto hello vybraná velikost virtuálního počítače bude mít vliv na jakou kapacitu se zpracování, paměť a úložiště je k dispozici pro vaši aplikaci. Ovlivní také hello výpočetní a náklady na úložiště. Níže jsou například hello specifikace hello největší velikost virtuálního počítače v řady DS, DSv2 series a GS řady:

| Velikost virtuálního počítače | Procesorová jádra | Memory (Paměť) | Velikosti disků virtuálních počítačů | Max. Datové disky | Velikost mezipaměti | IOPS | Omezení šířky pásma vstupně-výstupní mezipaměti |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Standard_DS14 |16 |112 GB |OPERAČNÍHO SYSTÉMU = 1023 GB <br> Místní SSD = 224 GB |32 |576 GB |50 000 IOPS <br> 512 MB za sekundu |4000 IOPS a 33 MB za sekundu |
| Standard_GS5 |32 |448 GB |OPERAČNÍHO SYSTÉMU = 1023 GB <br> Místní SSD = 896 GB |64 |4224 GB |80 000 IOPS <br> 2 000 MB za sekundu |5 000 IOPS a 50 MB za sekundu |

tooview úplný seznam všech dostupných velikostí virtuálních počítačů Azure odkazovat příliš[velikosti virtuálních počítačů Windows](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) nebo [velikosti virtuálního počítače s Linuxem](../../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Zvolte velikost virtuálního počítače, které naplňují a škálování tooyour potřeby požadavky na výkon aplikace. Kromě toho toothis, vezměte v úvahu následující důležité zvážit při výběru velikosti virtuálních počítačů.

*Limity škálování*  
Hello maximální IOPS omezení na virtuální počítač a na disk je jiné a nezávisle na sobě navzájem. Ujistěte se, že aplikace hello je v souladu s limity hello hello virtuálních počítačů a také hello premium disky připojené tooit řídí IOPS. Výkon aplikace, jinak bude mít omezení.

Jako příklad předpokládejme, že požadavek na aplikaci je delší než 4 000 IOPS. tooachieve se můžete zřídit P30 disku na virtuálním počítači DS1. Hello P30 disku může poskytovat too5 000 IOPS. Hello DS1 virtuální počítač je však omezená too3, 200 IOPS. V důsledku toho hello výkonu aplikací bude ovlivněna hello limitu virtuálních počítačů v 3,200 IOPS a bude snížení výkonu. tooprevent této situaci, vyberte virtuální počítač a velikost, která budou obě plnění aplikace požadavky na disku.

*Náklady na operace*  
V mnoha případech je možné, že vaše celkové náklady na použití služby Premium Storage operace nižší než při použití standardní úložiště.

Představte si třeba aplikace, které vyžadují 16 000 IOPS. tooachieve tento výkon, budete potřebovat standardní\_virtuálního počítače Azure IaaS D14, které je možné maximální IOPS 16,000 pomocí 32 disků standardní úložiště 1 TB. Každý disk 1TB úložiště standard storage můžete dosáhnout maximálně 500 IOPS. Hello odhadované náklady tohoto virtuálního počítače za měsíc bude 1,570 $. Hello měsíční náklady na 32 disků standardní úložiště bude 1,638 $. Hello předpokládaná doba celkové měsíční náklady budou 3,208 $.

Ale pokud budete hostovaný hello, stejná aplikace na Storage úrovně Premium, budete potřebovat menší velikost virtuálního počítače a méně disky úložiště premium, proto snižuje celkové náklady hello. Standardní\_DS13 virtuálních počítačů můžete splňovat hello 16,000 IOPS požadavek pomocí čtyř P30 disků. Hello DS13 virtuální počítač má maximální IOPS 25,600 a každý disk P30 maximální IOPS 5 000. Celkově platí, tuto konfiguraci můžete dosáhnout 5 000 × 4 = 20 000 IOPS. Hello odhadované náklady tohoto virtuálního počítače za měsíc bude 1,003 $. Hello měsíční náklady na čtyři disků úložiště premium P30 bude 544.34 $. Hello předpokládaná doba celkové měsíční náklady budou 1,544 $.

Následující tabulka shrnuje rozpis nákladů hello tohoto scénáře pro Standard a Premium Storage.

| &nbsp; | **Standard** | **Premium** |
| --- | --- | --- |
| **Náklady virtuálního počítače za měsíc** |$1,570.58 (standardní\_D14) |$1,003.66 (standardní\_DS13) |
| **Ceny disků za měsíc** |1,638.40 (disky 32 × 1 TB) |544.34 (4 disky x P30) |
| **Celkové náklady za měsíc** |$3,208.98 |$1,544.34 |

*Distribucích systému Linux*  

S Azure Premium Storage, získáte hello stejnou úroveň výkonu pro virtuální počítače se systémem Windows a Linux. Podporujeme mnoho typů distribucích systému Linux, a zobrazí úplný seznam hello [zde](../../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Je důležité, že je lepší použít jiný distribucích toonote vhodné pro různé typy úloh. Zobrazí se různé úrovně výkonu v závislosti na hello distro, které vaše úlohy běží na. Testování hello distribucích systému Linux s vaší aplikací a zvolte hello jeden, který je nejvhodnější.

Pokud Premium Storage s Linuxem, zkontrolujte nejnovější aktualizace hello o požadované ovladače tooensure vysoký výkon.

## <a name="premium-storage-disk-sizes"></a>Velikosti disků úložiště Premium
Azure Premium Storage nabízí aktuálně sedm velikosti disku. Velikost každého disku může mít jiné měřítko pro IOPS, šířky pásma a úložiště. Vyberte správnou velikost disku úložiště Premium hello v závislosti na požadavcích aplikace hello a vysokým Škálováním hello velikost virtuálního počítače. Následující tabulka Hello ukazuje hello sedm disků velikosti a jejich funkce. P4 a P6 velikosti jsou aktuálně podporovány pouze pro spravované disky.

| Disky typu Premium  | P4    | P6    | P10   | P20   | P30   | P40   | P50   | 
|---------------------|-------|-------|-------|-------|-------|-------|-------|
| Velikost disku           | 32 GB | 64 GB | 128 GB| 512 GB            | 1024 GB (1 TB)    | 2 048 GB (2 TB)    | 4095 GB (4 TB)    | 
| Vstupně-výstupní operace za sekundu / disk       | 120   | 240   | 500   | 2300              | 5000              | 7500              | 7500              | 
| Propustnost / disk | 25 MB za sekundu  | 50 MB za sekundu  | 100 MB za sekundu | 150 MB za sekundu | 200 MB za sekundu | 250 MB za sekundu | 250 MB za sekundu | 


Kolik disků, které zvolíte, závisí na disku hello velikost zvolená. Můžete použít jeden disk P50 nebo více disků toomeet P10 požadavků vaší aplikace. Brát v níže uvedených při výběru hello důležité informace o účtu.

*Limity škálování (IOPS a propustnost)*  
Hello IOPS a propustnost limity velikosti disku každého Premium je jiné a nezávislé z hello limity škálování virtuálních počítačů. Zkontrolujte, zda text hello celkový počet IOPS a propustnost z hello disků jsou v souladu s limity škálování hello vybraná velikost virtuálního počítače.

Například, pokud požadavek na aplikaci je maximálně 250 MB za sekundu, propustnosti a používáte DS4 virtuální počítač s jediný disk P30. Hello DS4 virtuálních počítačů můžete uvolňovat propustnost too256 MB/s. Jediný disk P30 má však propustnost limit 200 MB/s. V důsledku toho budou se aplikace hello omezené na 200 MB/s z důvodu omezení toohello disku. tooovercome tento limit, přidělení více než jeden data toohello disky virtuálních počítačů nebo změně velikosti disků tooP40 nebo P50.

> [!NOTE]
> Čtení obsloužených hello mezipaměti nejsou součástí hello disku IOPS a propustnost, proto není subjektu toodisk omezení. Mezipaměť obsahuje samostatné IOPS a propustnost limitu na virtuální počítač.
>
> Například původně čtení a zápisu jsou 60MB/s a 40MB za sekundu v uvedeném pořadí. V průběhu času hello mezipaměti warms a slouží více a více hello čtení z mezipaměti hello. Potom můžete získat zápisu vyšší propustnost disku hello.
>
>

*Počet disků*  
Určete počet hello disky, které budete potřebovat tím, že posoudí požadavky aplikací. Limit velikost každého virtuálního počítače je také na hello počet disků, můžete připojit toohello virtuálních počítačů. Obvykle je to dvakrát hello počet jader. Ujistěte se, že hello velikost virtuálního počítače, který zvolíte, může podporovat hello počet disků, které jsou potřeba.

Pamatujte si, že se hello Storage úrovně Premium disky mají vyšší výkon možnosti porovnání tooStandard úložiště disky. Proto pokud migrujete aplikace z virtuálního počítače Azure IaaS pomocí standardního úložiště tooPremium úložiště, bude pravděpodobně nutné méně prémiové disky tooachieve hello stejný nebo i vyšší výkon pro vaši aplikaci.

## <a name="disk-caching"></a>Ukládání do mezipaměti na disku
Vysoká škálování virtuálních počítačů, které využívají Storage úrovně Premium mají vícevrstvé ukládání do mezipaměti technologie názvem BlobCache. BlobCache používá kombinaci hello RAM virtuálního počítače a místní SSD pro ukládání do mezipaměti. Tato mezipaměť je k dispozici pro trvalé a místní disky virtuálních počítačů hello hello Storage úrovně Premium. Ve výchozím nastavení je toto nastavení mezipaměti nastavené tooRead a zápis pro disky operačního systému a jen pro čtení pro datové disky hostované na Storage úrovně Premium. S disků na discích úložiště Premium hello povoleno ukládání do mezipaměti, hello měřítko virtuální počítače můžete dosáhnout velmi vysoké úrovně výkonu, které překračují hello základní výkon disku.

> [!WARNING]
> Změna nastavení mezipaměti hello Azure disku odpojí a znovu připojí hello cílový disk. Pokud je disk operačního systému hello, hello virtuální počítač se restartuje. Zastavte všechny aplikace a služby, které by mohly mít dopad tento přerušení před změnou nastavení mezipaměti hello disku.
>
>

Další informace o tom, jak funguje BlobCache, toolearn odkazovat toohello uvnitř [Azure Premium Storage](https://azure.microsoft.com/blog/azure-premium-storage-now-generally-available-2/) příspěvku na blogu.

Je důležité tooenable mezipaměti na hello správnou sadu disků. Tom, jestli by měl povolit ukládání do mezipaměti na disku na premium disk nebo nebude závisí na vzor zatížení hello tohoto disku bude zpracovávat. Následující tabulka zobrazuje hello výchozí nastavení mezipaměti pro disky operačního systému a Data.

| **Typ disku** | **Výchozí nastavení mezipaměti** |
| --- | --- |
| Disk OS |ReadWrite |
| Datový disk |Žádný |

Následující jsou hello nastavení mezipaměti doporučené disku pro datové disky

| **Nastavení ukládání do mezipaměti na disku** | **Doporučení když toouse toto nastavení** |
| --- | --- |
| Žádný |Konfigurace mezipaměti hostitele jako None jen pro zápis a zápis náročné disků. |
| Jen pro čtení |Konfigurace mezipaměti hostitele jako jen pro čtení pro disky jen pro čtení a zápisu pro čtení. |
| ReadWrite |Konfigurace mezipaměti hostitele ReadWrite pouze v případě, že vaše aplikace zpracovává správně zápisu do mezipaměti datových toopersistent disků v případě potřeby. |

*Jen pro čtení*  
Konfigurace ukládání do mezipaměti na Storage úrovně Premium data disky určené jen pro čtení, můžete dosáhnout nízkou latenci pro čtení a získat velmi vysoké IOPS pro čtení a propustnost pro vaši aplikaci. Toto je z důvodu ze dvou důvodů

1. Čtení provést z mezipaměti, která je na paměť virtuálního počítače hello a místní SSD, je mnohem rychlejší než čtení z hello datový disk, který je na hello úložiště objektů blob Azure.  
2. Storage úrovně Premium nepočítá hello čtení obsluhovat z mezipaměti směrem hello disku IOPS a propustnosti. Aplikace je tedy možné tooachieve vyšší celkový počet IOPS a propustnosti.

*ReadWrite*  
Hello OS disky mají ve výchozím nastavení ReadWrite povoleno ukládání do mezipaměti. Nedávno jsme doplnili podporu pro ukládání do mezipaměti na data i disky v režimu ReadWrite. Používáte-li ukládání do mezipaměti v režimu ReadWrite, musíte mít správný způsob toowrite hello dat z mezipaměti toopersistent disků. Například popisovačů systému SQL Server zápisu do mezipaměti datových disků trvalé úložiště toohello svoje vlastní. Pomocí aplikace, která zpracovává zachování hello ReadWrite mezipaměti požadované dat může vést ke ztrátě toodata, pokud dojde k chybě hello virtuálních počítačů.

Například můžete použít tyto pokyny tooSQL serveru systémem Storage úrovně Premium provedením následujících hello

1. Konfigurace mezipaměti "Jen pro čtení" na discích úložiště premium hostování datových souborů.  
   a.  Hello rychlé čte z doba dotazu systému SQL Server nižší hello mezipaměti vzhledem k tomu, že data stránky jsou mnohem rychleji načteny z mezipaměti porovnání toodirectly hello z hello datových disků.  
   b.  Obsluhuje čtení z mezipaměti, znamená, že není k dispozici z premium datových disků ke zvýšení propustnosti. SQL Server můžete použít tento ke zvýšení propustnosti směrem načítání více datových stránek a další operace, jako je zálohování a obnovení, dávky zatížení, a znovu sestaví index.  
2. Konfigurace "Žádný" mezipaměti na storage úrovně premium disky hostování hello soubory protokolu.  
   a.  Soubory protokolů mají především zápisu náročná operace. Proto že nejsou nijak přínosné hello mezipaměti jen pro čtení.

## <a name="disk-striping"></a>Prokládání disků
Když může být vysoká měřítka, které je připojený virtuální počítač s několika premium trvalé disky úložiště, disky hello rozdělená společně tooaggregate jejich IOPs, šířky pásma a kapacity úložiště.

V systému Windows můžete použít prostory úložiště toostripe disky společně. Je nutné nakonfigurovat jeden sloupec pro každý disk ve fondu. V opačném hello celkový výkon prokládaný svazek může být nižší, než se očekávalo, z důvodu toouneven distribuce přenosů mezi disky hello.

Důležité: Pomocí uživatelského rozhraní správce serveru, můžete nastavit hello celkový počet sloupců se too8 prokládané svazku. Při připojení více než 8 disky, pomocí prostředí PowerShell toocreate hello svazku. Pomocí prostředí PowerShell, můžete nastavit hello počet sloupců rovna toohello počet disků. Například, pokud existují 16 disků v sadě jeden stripe; Zadejte 16 sloupců v hello *NumberOfColumns* parametr hello *New-VirtualDisk* rutiny prostředí PowerShell.

V systému Linux použijte společně hello MDADM nástroj toostripe disky. Podrobné pokyny k proložení disků v systému Linux naleznete příliš[konfigurace RAID softwaru v systému Linux](../../virtual-machines/linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

*Velikost stripe*  
Důležité konfigurační v prokládání disků je velikost stripe hello. velikost stripe Hello nebo velikost bloku je hello nejmenší bloků dat, které aplikace můžete vyřešit na prokládané svazku. velikost Hello prokládání, které jste nakonfigurovali závisí na typu hello aplikace a jeho žádost vzor. Pokud si zvolíte hello nesprávný stripe velikost, mohla způsobit chybné zarovnání tooIO, což vede toodegraded výkon aplikace.

Například pokud požadavek vstupně-výstupní operace generované aplikace je větší než velikost stripe hello disku, systém úložiště hello zapíše ho napříč stripe jednotku hranice na více než jeden disk. Když je čas tooaccess dat, bude mít tooseek napříč více než jeden požadavek hello toocomplete stripe jednotky. Hello kumulativní účinku takové chování může způsobit snížení výkonu toosubstantial. Na hello na druhé straně, pokud je menší než velikost stripe hello velikost požadavku vstupně-výstupní operace, a pokud je náhodné, hello vstupně-výstupní požadavky může přidat na hello stejné disku problémové místo a nakonec docházelo k omezení výkonu hello vstupně-výstupní operace.

V závislosti na typu hello úlohy vaše aplikace běží, vyberte velikosti odpovídající stripe. Pro náhodné malé vstupně-výstupní požadavky použijte menší velikost stripe. Zatímco požadavky pro velkých sekvenčních vstupně-výstupní operace používat větší velikost stripe. Zjistěte hello stripe velikost doporučení pro hello aplikaci, že budete používat na Storage úrovně Premium. Pro systém SQL Server nakonfigurujte stripe velikosti 64KB pro úlohy OLTP a 256KB pro úlohy datového skladu. V tématu [osvědčené postupy z hlediska výkonu pro SQL Server na virtuálních počítačích Azure](../../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md#disks-guidance) toolearn Další.

> [!NOTE]
> Můžete společně rozkládají maximálně 32 prémiové disky úložiště na řady DS virtuálních počítačů a 64 prémiové disky úložiště na řadu GS virtuálních počítačů.
>
>

## <a name="multi-threading"></a>Více vláken
Azure Premium Storage platformy toobe massively parallel určený. Vícevláknové aplikace proto dosahuje mnohem vyšší výkon než na jednovláknový aplikaci. Vícevláknové aplikace rozdělí si její úkoly napříč více vláken a zvyšuje efektivitu jeho spuštění s využitím hello virtuálních počítačů a maximální toohello prostředky disku.

Například pokud aplikace běží na jednom jádru virtuálního počítače pomocí dvěma vlákny, hello procesoru přepínat mezi hello dvěma vlákny tooachieve efektivitu. Při jedno vlákno čeká na vstupně-výstupní operace toocomplete na disku, hello procesoru přepnout toohello jiné vlákno. Tímto způsobem můžete provádět dvěma vlákny více než jedno vlákno by. Pokud hello virtuálních počítačů má víc než jednoho jádra, další sníží běhu vzhledem k tomu, že každý základní paralelně můžete provádět úlohy.

Nemusí být možné toochange hello způsob dodávaných aplikace implementuje jeden vláken nebo více vláken. SQL Server je třeba, umožňuje zpracovávat více procesorů a s více jádry. SQL Server však rozhodne, za jakých podmínek využije jeden nebo více vláken tooprocess dotazu. Může spouštět dotazy a vytvářet indexy používající více vláken. Pro daný dotaz, který zahrnuje připojení velké tabulky a řazení dat před vrácením toohello uživatele SQL Server pravděpodobně používat více vláken. Uživatel však nemůže řídit, jestli SQL Server provede dotaz pomocí jedním vláknem nebo více vláken.

Existují nastavení konfigurace, můžete změnit tooinfluence více vláken nebo toto paralelní zpracování aplikace. Například v případě systému SQL Server je hello maximální úroveň paralelismus konfigurace. Toto nastavení nazývá MAXDOP, vám umožní tooconfigure hello maximální počet procesorů, které systém SQL Server můžete použít při paralelní zpracování. MAXDOP můžete nakonfigurovat pro jednotlivé dotazy nebo operace indexu. To je užitečné, pokud chcete toobalance prostředky systému pro kritické aplikace výkonu.

Předpokládejme například, že vaše aplikace pomocí systému SQL Server spouští velké dotazu a operaci indexu na hello stejnou dobu. Předpokládejme, dejte nám chtěli toobe operaci indexu hello další původce porovnání toohello velké dotazu. V takovém případě můžete nastavit MAXDOP hodnotu vyšší než hello MAXDOP hodnota pro dotaz hello toobe operaci indexu hello. Tímto způsobem, SQL Server má větší počet procesorů, které můžete využít pro hello index operace porovnání toohello počet procesorů můžete vyhradit toohello velké dotazu. Pamatujte si, že se nebudete řídit hello počet vláken, který bude používat systém SQL Server pro každou operaci. Můžete řídit hello maximální počet procesorů je určené pro více vláken.

Další informace o [stupňů paralelismus](https://technet.microsoft.com/library/ms188611.aspx) v systému SQL Server. Zjistěte taková nastavení, které ovlivňují více vláken ve vaší aplikace a jejich konfigurace toooptimize výkonu.

## <a name="queue-depth"></a>Hloubka fronty
Hello hloubku fronty nebo délka fronty nebo velikost fronty je v systému hello hello počet čekajících požadavků vstupně-výstupní operace. Hodnota Hello hloubku fronty určuje, kolik vstupně-výstupní operace operace můžete zarovnat vaší aplikace, které disky úložiště hello bude zpracovávat. Ovlivňuje všechny hello tři aplikace výkonu indikátory, které jsme popsané v této článku to IOP, propustnosti a latence.

Fronty hloubkou a více vláken jsou úzce souvisejí. Hello hloubku fronty hodnota určuje, kolik více vláken může dosáhnout pomocí aplikace hello. Pokud hello hloubku fronty velká, aplikace můžete provádět další operace souběžně, jinými slovy, více vláken. Pokud hello hloubku fronty je malý, i když je aplikace Vícevláknová, nebude mít dostatek požadavky srovnáte pro souběžné provádění.

Obvykle vypnout hello police aplikací vám nepovolují hloubku fronty hello toochange, protože pokud nastavte nesprávně provede další škodu než funkční. Aplikace nastaví hodnota pravé hello fronty hloubka tooget hello optimálního výkonu. Nicméně je tento koncept je důležité toounderstand, takže můžete řešit problémy s výkonem s vaší aplikací. Hello důsledky hloubku fronty můžete také sledovat spuštěním testu typovou úlohou nástrojů v systému.

Některé aplikace zadejte nastavení tooinfluence hello hloubku fronty. Například nastavení hello MAXDOP (maximální stupně paralelního zpracování) v systému SQL Server popsané v předchozí části. MAXDOP je způsob, jak tooinfluence hloubka fronty a více vláken, i když přímo nemění hodnotu hloubky fronty hello systému SQL Server.

*Hloubka fronty vysoké*  
Hloubka fronty vysoké zarovnán další operace na disku hello. Hello disk zná hello další požadavek v příslušné fronty předem. V důsledku toho hello disku můžete naplánovat operace dopředu a jejich zpracování v optimální pořadí. Vzhledem k tomu, že aplikace hello odesílá další požadavky toohello disk, hello disk může zpracovat více paralelních IOs. Nakonec, hello aplikace bude mít tooachieve vyšší IOPS. Vzhledem k tomu, že aplikace je zpracování více požadavků, hello celková propustnost aplikace hello se taky zvýší.

Obvykle se aplikace můžete dosáhnout maximální propustnost s 8-16 + nezpracovaných vstupně-výstupních na připojený disk. Pokud hloubce fronty, aplikace není vkládání dostatek toohello systému IOs a zpracuje menší množství v daném časovém období. Jinými slovy méně propustnost.

Například v systému SQL Server hello nastavení MAXDOP hodnotu pro dotaz příliš "4" informuje SQL Server, který můžete použít až toofour jader tooexecute hello dotazu. SQL Server lze určit, co je nejlepší fronty hloubka hodnota a hello počet jader pro spuštění dotazu hello.

*Optimální hloubky fronty*  
Fronty velmi vysokou hodnotu hloubky má také jeho nevýhody. Pokud hodnotu hloubky fronty je příliš vysoká, aplikace hello pokusí toodrive velmi vysokou IOPS. Pokud aplikace obsahuje trvalé disků se dostatečná zřízené IOPS, může to mít negativní vliv latencí aplikací. Následující vzorec znázorňuje hello vztah mezi IOPS, latence a hloubku fronty.  
    ![](media/storage-premium-storage-performance/image6.png)

Byste neměli konfigurovat hloubku fronty tooany vysoké hodnoty, ale tooan optimální hodnotu, která může poskytnout dostatek IOPS pro hello aplikací bez ovlivnění latenci. Například pokud latence aplikace hello potřebuje toobe 1 milisekundu, hello hloubku fronty potřeba je 5 000 IOPS, tooachieve hloubka fronty = 5000 x 0,001 = 5.

*Hloubka fronty pro prokládané svazek*  
Pro svazek prokládané udržovat tak, aby každý disk má ve špičce hloubce fronty jednotlivě hloubce fronty dostatečně vysoký. Představte si třeba aplikace, který by vložil hloubce fronty 2 a v hello stripe je 4 disky. dva požadavky vstupně-výstupní operace Hello přejde tootwo disky a zbývající dva disky bude nečinnosti. Proto konfigurovat hloubku fronty hello tak, aby všechny disky hello může být zaneprázdněn. Vzorec níže ukazuje, jak toodetermine hello hloubku fronty prokládané svazky.  
    ![](media/storage-premium-storage-performance/image7.png)

## <a name="throttling"></a>Omezování
Azure Premium Storage zřizuje zadaný počet IOPS a propustnost v závislosti na velikosti virtuálních počítačů hello a velikosti disků, které zvolíte. Kdykoliv se aplikace pokusí toodrive IOPS nebo propustnosti nad těchto omezení může zpracovat jaké hello virtuálního počítače nebo disku, bude omezení Storage úrovně Premium ho. To manifesty hello tvar snížení výkonu ve vaší aplikaci. To může znamenat vyšší latence, snížit propustnost nebo snižte IOPS. Pokud Storage úrovně Premium není omezení, vaše aplikace může úplně nezdaří podle překročení, jaké jsou schopné dosáhnout její prostředky. Ano, problémů s výkonem tooavoid kvůli toothrottling, vždy zřídit dostatečné prostředky pro vaši aplikaci. Vzít v úvahu, co jsme probírali v hello velikosti virtuálních počítačů a výše uvedených oddílech velikosti disku. Srovnávací testy je nejlepší způsob, jak toofigure hello na tom, jaké prostředky musíte toohost vaší aplikace.

## <a name="benchmarking"></a>Srovnávací testy
Srovnávací testy je proces hello simulaci různé úlohy ve vaší aplikaci a měření výkonu hello aplikace pro jednotlivá zatížení. Pomocí hello kroků popsaných v předchozí části, jste shromáždili požadavky na výkon aplikace hello. Spuštěním nástroje na virtuálních počítačích hello hostování aplikace hello srovnávací testy můžete určit hello úrovně výkonu, které aplikace můžete dosáhnout Storage úrovně Premium. V této části Poskytujeme vám příklady srovnávací testy standardní virtuální počítač DS14 zřizovat s disky Azure Premium Storage.

Použili jsme běžných testu typovou úlohou nástrojů Iometer a FIO, pro systém Windows a Linux v uvedeném pořadí. Tyto nástroje vytvořit více vláken simulaci provozní jako zatížení a výkon systému hello měr. Pomocí nástrojů hello můžete také nakonfigurovat parametry jako velikost a fronty bloku hloubku, který obvykle se nelze změnit pro aplikaci. To vám dává další flexibilitu toodrive hello maximální výkon na vysokou škálování virtuálních počítačů, které jsou zřizovány s prémiové disky pro různé typy úloh aplikací. toolearn najdete další informace o jednotlivých vzorová analytická pomůcka [Iometer](http://www.iometer.org/) a [FIO](http://freecode.com/projects/fio).

Následující příklady hello toofollow vytvořit standardní DS14 virtuální počítač a připojte 11 Storage úrovně Premium toohello disky virtuálních počítačů. Hello 11 disků nakonfigurujte 10 disky s hostitelem ukládání do mezipaměti jako "Žádný" a rozkládají je do svazku názvem NoCacheWrites. Konfigurace hostitele na disku zbývající hello ukládání do mezipaměti jako "Jen pro čtení" a vytvoření svazku volána čtení z mezipaměti s tento disk. Pomocí tohoto nastavení, bude možné toosee hello maximální ke čtení a zápisu výkonu z standardní DS14 virtuálního počítače. Podrobné informace o postupu vytvoření virtuálního počítače DS14 s prémiové disky, přejděte příliš[vytvoření a použití účtu Premium Storage pro datový disk virtuálního počítače](../storage-premium-storage.md).

*Zahájení práce s hello mezipaměti*  
Hello disk s použití mezipaměti u hostitele jen pro čtení bude mít toogive IOPS vyšší než maximální disku hello. tooget tímto maximem načten výkonu z mezipaměti hostitele hello, nejprve musí tedy vytvoří mezipaměť hello tohoto disku. Tím se zajistí, že tento hello IOs pro čtení, které vzorová analytická pomůcka vyvolají na čtení z mezipaměti svazku ve skutečnosti přístupů do mezipaměti hello a nikoli hello disk přímo. výsledek přístupů k mezipaměti Hello v další IOPS z jedné mezipaměti hello povoleno disku.

> **Důležité:**  
> Hello mezipaměti můžete musí tedy před spuštěním srovnávací testy, pokaždé, když je virtuální počítač restartovat.
>
>

#### <a name="iometer"></a>Iometer
[Stáhněte si nástroj Iometer hello](http://sourceforge.net/projects/iometer/files/iometer-stable/2006-07-27/iometer-2006.07.27.win32.i386-setup.exe/download) na hello virtuálních počítačů.

*Testovací soubor*  
Iometer používá testovací soubor, který je uložený na svazku hello, na kterém budete spouštět hello srovnávací testy testu. Ho jednotky čtení a zápisy na tomto testování disku se souborem toomeasure hello IOPS a propustnosti. Iometer vytvoří tento testovací soubor, pokud jste nezadali. Vytvořte 200GB testovací soubor s názvem iobw.tst na čtení z mezipaměti a NoCacheWrites svazky hello.

*Specifikace přístup*  
Hello specifikace, požádat o vstupně-výstupní operace velikost % pro čtení a zápis, % náhodných nebo sekvenčních jsou nakonfigurovány pomocí karty hello "Specifikace Access" v Iometer. Vytvořte specifikaci přístup pro jednotlivé scénáře hello popsané dole. Vytvoření specifikace hello přístup a "Uložit" s odpovídajícím názvem jako – RandomWrites\_8 kB, RandomReads\_8 kB. Vyberte odpovídající specifikaci hello při spuštění scénáře testu hello.

Níže je uveden příklad specifikací přístup pro scénář maximální IOPS zápisu  
    ![](media/storage-premium-storage-performance/image8.png)

*Specifikace maximální IOPS testu*  
toodemonstrate maximální IOPs, použít menší velikost žádosti. Použijte 8 kb velikost požadavku a vytvoření specifikace náhodných zápisů a čtení.

| Specifikace přístup | Velikost požadavku | Náhodné % | % Pro čtení |
| --- | --- | --- | --- |
| RandomWrites\_8 kb |8 KB |100 |0 |
| RandomReads\_8 kb |8 KB |100 |100 |

*Specifikace maximální propustnost testu*  
toodemonstrate maximální propustnost, použijte větší velikost žádosti. Použijte 64 tisíc velikost požadavku a vytvoření specifikace náhodných zápisů a čtení.

| Specifikace přístup | Velikost požadavku | Náhodné % | % Pro čtení |
| --- | --- | --- | --- |
| RandomWrites\_64 kb / s |64 KB |100 |0 |
| RandomReads\_64 kb / s |64 KB |100 |100 |

*Spuštění hello Iometer testu*  
Proveďte kroky hello níže toowarm do mezipaměti

1. Vytvořte dva specifikace přístup s hodnoty zobrazené níže,

   | Name (Název) | Velikost požadavku | Náhodné % | % Pro čtení |
   | --- | --- | --- | --- |
   | RandomWrites\_1 MB |1MB |100 |0 |
   | RandomReads\_1 MB |1MB |100 |100 |
2. Spusťte test hello Iometer pro inicializaci mezipaměti disku s následujícími parametry. Pomocí tří pracovních vláken pro hello cílový svazek a hloubce fronty 128. Nastavit hello hello too2hrs testu na kartě "Testování instalace" hello "Běh" dobu trvání.

   | Scénář | Cílový svazek | Name (Název) | Doba trvání |
   | --- | --- | --- | --- |
   | Inicializovat Disk mezipaměti |Čtení z mezipaměti |RandomWrites\_1 MB |2hrs |
3. Spusťte test hello Iometer pro zahájení práce s disku mezipaměti s následujícími parametry. Pomocí tří pracovních vláken pro hello cílový svazek a hloubce fronty 128. Nastavit hello hello too2hrs testu na kartě "Testování instalace" hello "Běh" dobu trvání.

   | Scénář | Cílový svazek | Name (Název) | Doba trvání |
   | --- | --- | --- | --- |
   | Horké až diskové mezipaměti |Čtení z mezipaměti |RandomReads\_1 MB |2hrs |

Po diskové mezipaměti je provozní teplotu, pokračujte v níže uvedených scénářů testů hello. hello toorun Iometer test, použijte aspoň tři pracovních vláken pro **každý** cíle svazku. Pro každý pracovní vlákno vyberte hello cílový svazek, nastavit hloubku fronty a vyberte jednu z hello uložit test specifikace, jsou uvedené v tabulce hello níže toorun hello odpovídající testovací scénář. Při spuštění tyto testy Hello tabulka také ukazuje očekávané výsledky pro IOPS a propustnosti. Ve všech scénářích se používá malé velikost vstupně-výstupní operace 8 kB a hloubce fronty vysoké 128.

| Testovací scénář | Cílový svazek | Name (Název) | výsledek |
| --- | --- | --- | --- |
| Max. Čtení IOPS |Čtení z mezipaměti |RandomWrites\_8 kb |50 000 IOPS |
| Max. Zápis IOPS |NoCacheWrites |RandomReads\_8 kb |64 000 IOPS |
| Max. Součet IOPS |Čtení z mezipaměti |RandomWrites\_8 kb |100 000 IOPS |
| NoCacheWrites |RandomReads\_8 kb | &nbsp; | &nbsp; |
| Max. Čtení MB/s |Čtení z mezipaměti |RandomWrites\_64 kb / s |524 MB/s |
| Max. Zápis MB/s |NoCacheWrites |RandomReads\_64 kb / s |524 MB/s |
| Kombinovaná MB/s |Čtení z mezipaměti |RandomWrites\_64 kb / s |1 000 MB/s |
| NoCacheWrites |RandomReads\_64 kb / s | &nbsp; | &nbsp; |

Níže jsou snímky obrazovky hello Iometer výsledků testů pro kombinované scénáře IOPS a propustnosti.

*Kombinovaná čtení a zápisy maximální IOPS.*  
![](media/storage-premium-storage-performance/image9.png)

*Kombinovaná čtení a zápisy maximální propustnost*  
![](media/storage-premium-storage-performance/image10.png)

### <a name="fio"></a>FIO
FIO je úložiště toobenchmark oblíbených nástroj na hello virtuální počítače s Linuxem. Má hello flexibilitu tooselect různé vstupně-výstupní operace velikosti, sekvenční nebo náhodné čtení a zápisy. Procesy tooperform hello Zadaná vstupně-výstupních operací, nebo ji vytvoří pracovních vláken. Můžete zadat typ hello vstupně-výstupních operací každý pracovní vlákno musíte provést pomocí úlohy souborů. Jsme vytvořili jeden soubor úlohy pro scénář ukazuje následující příklady hello. Specifikace hello v tyto úlohy soubory toobenchmark různé úlohy běžící na Storage úrovně Premium, můžete změnit. V příkladech hello se používá standardní DS 14 virtuální počítač spuštěný **Ubuntu**. Hello používá stejné nastavení popsané v hello začátku hello [srovnávací testy části](#Benchmarking) a záložním vytvoří mezipaměť hello před spuštěním hello srovnávací testy testy.

Než začnete, [stáhnout FIO](https://github.com/axboe/fio) a nainstalujte ji na virtuálním počítači.

Spusťte následující příkaz pro Ubuntu, hello

```
apt-get install fio
```

Pro řízení operací čtení na discích hello použijeme čtyři pracovních vláken pro řízení operace zápisu a čtyři pracovních vláken. Hello zápisu pracovníci bude se řídí provoz na svazku hello "nocache", který obsahuje 10 disky s mezipamětí nastavit také "Žádný". Hello pracovních procesů pro čtení se se řídí provoz na hello "readcache" svazek, který má 1 disk sadou mezipaměti příliš "Jen pro čtení".

*Maximální zápis IOPS*  
Vytvořit soubor úlohy hello se následující specifikace tooget maximální IOPS zápisu. Název "fiowrite.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[writer1]
rw=randwrite
directory=/mnt/nocache
[writer2]
rw=randwrite
directory=/mnt/nocache
[writer3]
rw=randwrite
directory=/mnt/nocache
[writer4]
rw=randwrite
directory=/mnt/nocache
```

Poznámka: hello podle klíče věcí, které odpovídají pokynů pro návrh hello popsané v předchozích částech. Tyto specifikace jsou nezbytné toodrive maximální IOPS,  

* Hloubka fronty vysoké 256.  
* Velikost malé bloku 8KB.  
* Více vláken provádění náhodných zápisů.

Spusťte následující příkaz tookick vypnout hello FIO otestovat 30 sekund, hello  

```
sudo fio --runtime 30 fiowrite.ini
```

V průběhu testovacího hello bude možné toosee hello počet zápisu, které poskytujete disky virtuálních počítačů a Premium hello IOPS. Jak znázorňuje následující ukázka hello, hello virtuálních počítačů DS14 doručování jeho zápisu maximální limit IOPS 50 000 IOPS.  
    ![](media/storage-premium-storage-performance/image11.png)

*Čtení maximální IOPS*  
Vytvořit soubor úlohy hello se následující specifikace tooget maximální IOPS pro čtení. Název "fioread.ini".

```
[global]
size=30g
direct=1
iodepth=256
ioengine=libaio
bs=8k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache
```

Poznámka: hello podle klíče věcí, které odpovídají pokynů pro návrh hello popsané v předchozích částech. Tyto specifikace jsou nezbytné toodrive maximální IOPS,

* Hloubka fronty vysoké 256.  
* Velikost malé bloku 8KB.  
* Více vláken provádění náhodných zápisů.

Spusťte následující příkaz tookick vypnout hello FIO otestovat 30 sekund, hello

```
sudo fio --runtime 30 fioread.ini
```

Během hello testovací běhy, nebudete moct toosee hello počet čtení hello IOPS virtuálního počítače a poskytujete prémiové disky. Jak znázorňuje následující ukázka hello, doručování hello DS14 virtuální počítač více než 64 000 IOPS pro čtení. Toto je kombinací hello disku a výkon mezipaměti hello.  
    ![](media/storage-premium-storage-performance/image12.png)

*Maximální počet čtení a zápisu IOPS*  
Vytvořit soubor úlohy hello s následující specifikace tooget maximální kombinaci oprávnění ke čtení a zápis IOPS. Název "fioreadwrite.ini".

```
[global]
size=30g
direct=1
iodepth=128
ioengine=libaio
bs=4k

[reader1]
rw=randread
directory=/mnt/readcache
[reader2]
rw=randread
directory=/mnt/readcache
[reader3]
rw=randread
directory=/mnt/readcache
[reader4]
rw=randread
directory=/mnt/readcache

[writer1]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer2]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer3]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
[writer4]
rw=randwrite
directory=/mnt/nocache
rate_iops=12500
```

Poznámka: hello podle klíče věcí, které odpovídají pokynů pro návrh hello popsané v předchozích částech. Tyto specifikace jsou nezbytné toodrive maximální IOPS,

* Hloubka fronty vysoké 128.  
* Velikost bloku malé 4KB.  
* Více vláken provádění náhodných čte a zapisuje.

Spusťte následující příkaz tookick vypnout hello FIO otestovat 30 sekund, hello

```
sudo fio --runtime 30 fioreadwrite.ini
```

V průběhu testovacího hello se být schopný toosee hello počet kombinované pro čtení a zápisu IOPS hello virtuálních počítačů a poskytujete prémiové disky. Jak znázorňuje následující ukázka hello, hello virtuálních počítačů DS14 doručování více než 100 000 kombinované oprávnění ke čtení a zápis IOPS. Toto je kombinací hello disku a výkon mezipaměti hello.  
    ![](media/storage-premium-storage-performance/image13.png)

*Nejvyšší možná propustnost*  
tooget hello maximální kombinovat pro čtení a zápisu propustnosti, použít větší velikost bloku a hloubku fronty velké s více vlákny provádění čtení a zápisy. Můžete použít velikost bloku 64KB a hloubku fronty 128.

## <a name="next-steps"></a>Další kroky
Další informace o službě Azure Premium Storage:

* [Storage úrovně Premium: Vysoce výkonné úložiště pro úlohy virtuálních počítačů Azure](../storage-premium-storage.md)  

Pro uživatele systému SQL Server, přečtěte si článek na nejlepší postupy z hlediska výkonu pro SQL Server:

* [Osvědčené postupy z hlediska výkonu pro SQL Server na virtuálních počítačích Azure](../../virtual-machines/windows/sql/virtual-machines-windows-sql-performance.md)
* [Azure Storage úrovně Premium poskytuje nejvyšší výkon pro SQL Server ve virtuálním počítači Azure](http://blogs.technet.com/b/dataplatforminsider/archive/2015/04/23/azure-premium-storage-provides-highest-performance-for-sql-server-in-azure-vm.aspx)
