---
title: "ladění pokyny aaaAzure Data Lake Store Storm výkonu | Microsoft Docs"
description: "Ladění pravidla výkonu Storm Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: 5412fd46cf2373f5877030913df4fe1fc6f5473a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tuning-guidance-for-storm-on-hdinsight-and-azure-data-lake-store"></a>Pokyny pro Storm v HDInsight a Azure Data Lake Store optimalizace výkonu

Pochopení hello faktory, které je potřeba zvážit při optimalizaci výkonu hello topologie Azure Storm. Například je důležité toounderstand hello charakteristika hello práci ve funkcích spouts hello a hello funkce bolts (zda pracovní hello je vstupně-výstupních operací nebo náročná na paměť). Tento článek se zabývá řadu pokyny, včetně řešení běžných potíží optimalizace výkonu.

## <a name="prerequisites"></a>Požadavky

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Účet Azure Data Lake Store**. Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md).
* **Cluster Azure HDInsight** s tooa přístup k účtu Data Lake Store. V tématu [vytvoření clusteru HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Ujistěte se, že povolení vzdálené plochy pro hello clusteru.
* **Spuštění clusteru Storm v Data Lake Store**. Další informace najdete v tématu [Storm v HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-storm-overview).
* **Pokyny v Data Lake Store ladění výkonu**.  Obecný výkon koncepty, najdete v části [Data Lake Store výkonu ladění pokyny](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance).  

## <a name="tune-hello-parallelism-of-hello-topology"></a>Vyladění hello paralelismu topologie hello

Je možné tooimprove výkonu roste souběžnosti hello z hello vstupně-výstupních operací tooand z Data Lake Store. Topologie Storm má nastavení konfigurace, které určení stupně paralelního zpracování hello:
* Počet pracovních procesů (hello pracovníci jsou rovnoměrně rozdělené mezi hello virtuálních počítačů).
* Počet instancí vykonavatele funkcí spout.
* Počet instancí vykonavatele funkcí bolt.
* Počet funkcí spout úlohy.
* Počet úloh funkcí bolt.

Například v clusteru s 4 virtuální počítače a 4 pracovních procesů, vykonavatelů 32 spout a 32 funkcí spout úkoly a 256 vykonavatelů bolt a 512 bolt úlohy, zvažte následující hello:

Každý nadřízeného, což je pracovního uzlu, má jednoho pracovního procesu Java virtual machine (JVM). Tento proces JVM spravuje 4 spout vláken a 64 bolt vláken. V rámci každé vlákno se úkoly spouštějí postupně. Hello předcházející konfigurace každou funkcí spout vlákno je 1 úkol a každé vlákno bolt má 2 úlohy.

Tady jsou hello různé součásti související se situací Storm, a jejich vliv na úrovni hello paralelního zpracování, které máte:
* Hello hlavního uzlu (volané Nimbus v Storm) je použité toosubmit a spravovat úlohy. Tyto uzly nemají žádný vliv na hello stupně paralelního zpracování.
* uzly nadřízeného Hello. V prostředí HDInsight odpovídá tooa pracovního uzlu virtuálního počítače Azure.
* Hello pracovní úkoly jsou Storm procesů spuštěných ve hello virtuálních počítačů. Každý úkol pracovní odpovídá tooa JVM instance. Storm distribuuje hello počet pracovních procesů zadáte uzlů pracovního procesu toohello jako rovnoměrně.
* Spout a funkce bolt vykonavatele instancí. Každá instance vykonavatele odpovídá tooa vlákno běžící v rámci pracovních procesů hello (JVMs).
* Storm úlohy. Toto jsou logické úlohy, aby každá z těchto vláken spustit. Nezmění se hello úroveň paralelismu, takže byste měli zvážit, pokud potřebujete více úkolů na prováděcího modulu, nebo ne.

### <a name="get-hello-best-performance-from-data-lake-store"></a>Získání hello nejlepší výkon z Data Lake Store

Při práci se službou Data Lake Store, získáte nejlepší výkon hello, pokud hello následující:
* Sloučení malou připojí do větší velikosti (v ideálním případě 4 MB).
* Počet souběžných požadavků, jak můžete proveďte. Protože každý podproces bolt provádí blokování čtení, budete chtít toohave někde v rozsahu hello 8 12 vláken na jádra. Díky tomu hello síťových Adaptérů a hello procesoru dobře využito. Větší virtuální počítač umožňuje více souběžných požadavků.  

### <a name="example-topology"></a>Příklad topologie

Předpokládejme, že máte cluster 8 pracovní uzel s D13v2 virtuální počítač Azure. Má tento virtuální počítač s 8 jádry, takže mezi hello 8 uzlů pracovního procesu, máte 64 celkový počet jader.

Řekněme, že provedeme 8 vláken bolt na jádra. Zadaný 64 jader, která znamená, že chceme 512 celkový bolt vykonavatele instancí (vlákna). V takovém případě Řekněme, že jsme začínat jeden JVM na virtuální počítač a používají převážně hello souběžnosti vláken v rámci hello JVM tooachieve souběžnosti. To znamená, že potřebujeme 8 úlohy pracovního procesu (jeden na každý virtuální počítač Azure) a 512 vykonavatelů funkcí bolt. V této konfiguraci Storm pokusí toodistribute hello pracovníci rovnoměrně mezi uzly pracovního procesu (také označované jako nadřízeného uzlů), která poskytuje každý pracovní uzel 1 JVM. Teď v rámci hello vedoucí pokusí Storm toodistribute hello vykonavatelů rovnoměrně mezi vedoucí, udělíte každý nadřízeného (JVM) 8 vláken každý.

## <a name="tune-additional-parameters"></a>Vyladění dalších parametrů
Až budete mít hello základní topologie, můžete zvážit, jestli chcete tootweak některý z parametrů hello:
* **Počet JVMs za pracovního uzlu.** Pokud máte velké datové struktury (například vyhledávací tabulky) vyžaduje, aby je hostitel v paměti, každý JVM samostatná kopie. Alternativně můžete použít hello datová struktura napříč velký počet vláken, pokud máte méně JVMs. Pro vstupně-výstupních operací hello bolt hello počet JVMs neprovede tolik rozdíl jako hello počet vláken přidat mezi tyto JVMs. Pro jednoduchost, je vhodné toohave, jeden JVM za pracovního procesu. V závislosti na akci vaše bolt, nebo jaké aplikace zpracování můžete vyžadovat, i když může být nutné toochange toto číslo.
* **Počet vykonavatelů funkcí spout.** Protože hello předchozí příklad používá funkce bolts pro zápis tooData Lake Store, hello počet funkcích spouts není přímo relevantní toohello bolt výkonu. V závislosti na množství hello zpracování nebo vstupně-výstupních operací děje v hello spout, je vhodné se však tootune hello spouts pro nejlepší výkon. Ujistěte se, že máte dostatek funkcích spouts toobe možné tookeep hello funkce bolts zaneprázdněný. míry výstup Hello funkcích spouts hello by měl odpovídat hello propustnost funkce bolts hello. skutečné konfigurace Hello závisí na funkcích spout hello.
* **Počet úloh.** Každý bolt spouští jako jedno vlákno. Další úlohy za bolt neposkytují žádné další souběžnosti. Dobrý den, které jsou benefitu je pouze pokud má váš proces to v úvahu řazené kolekce členů hello velká část doby provádění funkcí bolt. Je vhodné toogroup, které mnoho řazených kolekcí členů do větší připojit před odesláním potvrzení z hello bolt. Ano ve většině případů poskytují více úkolů žádné další výhody.
* **Místní nebo náhodně seskupení.** Pokud je toto nastavení povoleno, řazené kolekce členů jsou odesílány toobolts v rámci hello stejný pracovní proces. Tím se snižuje volání mezi procesy komunikace a sítě. Toto nastavení se doporučuje pro většinu topologií.

Tento základní scénář je to dobrý výchozí bod. Testování s vlastními hello tootweak data předcházející parametry tooachieve optimální výkon.

## <a name="tune-hello-spout"></a>Vyladění hello spout

Můžete upravit následující nastavení tootune hello spout hello.

- **Časový limit řazené kolekce členů: topology.message.timeout.secs**. Toto nastavení určuje hello množství času zprávu trvá toocomplete a přijímat potvrzení, než je považována za se nezdařilo.

- **Maximální paměť na pracovní proces: worker.childopts**. Toto nastavení umožňuje určit další parametry příkazového řádku toohello Java pracovních procesů. Hello nejčastěji používaná nastavení tady je XmX, určující haldy hello maximální paměť přidělená tooa JVM společnosti.

- **Maximální počet funkcí spout čekající na vyřízení: topology.max.spout.pending**. Toto nastavení určuje hello počet řazených kolekcí členů, která může být v cestě (ještě nebyla potvrzena na všechny uzly v topologii hello) na vlákno spout kdykoli.

 Toodo dobrý výpočtu je tooestimate hello velikost jednotlivých vaší řazené kolekce členů. Potom rozmyslete si, kolik paměti jeden spout vlákno má. Hello celkové paměti přidělené tooa vlákno, dělený tato hodnota by měla získáte hello horní mez pro maximální počet funkcí spout hello čekající na vyřízení parametr.

## <a name="tune-hello-bolt"></a>Vyladění hello bolt
Když píšete tooData Lake Store, nastavit zásady synchronizace velikost (vyrovnávací paměti na straně klienta hello) too4 MB. Abyste vyprázdnili nebo hsync() pak se provádí jenom v případě, že velikost vyrovnávací paměti hello je hello v této hodnotě. Hello Data Lake Store je ovladač na pracovní hello virtuálního počítače automaticky nemá tato ukládání do vyrovnávací paměti, pokud provádíte explicitně hsync().

Hello výchozí Data Lake Store Storm bolt obsahuje parametr zásady synchronizace velikost (fileBufferSize), může být použité tootune tento parametr.

V I náročnými topologie, je vhodné toohave každé vlákno bolt zápisu vlastní soubor tooits a tooset zásadu otočení souboru (fileRotationSize). Když hello soubor dosáhne určité velikosti, automaticky vyprázdní hello datového proudu a je zapsaný nový soubor. Doporučená velikost souboru pro otočení Hello je 1 GB.

### <a name="handle-tuple-data"></a>Zpracování dat řazené kolekce členů

V Storm obsahuje spout na tooa řazené kolekce členů, dokud je explicitně potvrzen hello bolt. Pokud řazené kolekce členů byl načten pomocí hello bolt, ale nebyla ještě potvrzeny, nemusí mít hello spout jako trvalý, do Data Lake Store back-end. Po řazené kolekce členů potvrzen, hello spout můžete záruku stálosti podle hello bolt a můžete pak odstraňte hello zdrojová data z jakéhokoli zdroje je čtení z.  

Chcete-li dosáhnout optimálního výkonu v Data Lake Store, máte hello bolt vyrovnávací paměti 4 MB dat řazené kolekce členů. Zapište toohello Data Lake Store zpět ukončit jeden zápis 4 MB. Po hello data byla úspěšně napsané toohello úložiště (ve volání hflush()) hello bolt můžete na vědomí hello data zpět toohello spout. Toto je příklad jaké hello funkce bolt zadaný sem nepodporuje. Je také přijatelné toohold větší počet řazených kolekcí členů, než Přišla žádost hflush() hello a hello potvrzené řazené kolekce členů. Nicméně tím se zvyšuje hello počet řazených kolekcí členů v cestě hello spout musí toohold, a proto zvyšuje hello na JVM požadované množství paměti.

> [!NOTE]
Aplikace může mít řazených kolekcí členů tooacknowledge požadavek častěji (na velikosti dat menší než 4 MB) z jiných důvodů bez výkonu. Nicméně, který může ovlivnit hello vstupně-výstupních operací propustnost toohello úložiště back-end. Pečlivě zvažte tato kompromis proti hello bolt vstupně-výstupní výkon.

Pokud počet příchozích hello řazené kolekce členů není vysoké, takže vyrovnávací paměti 4 MB hello trvá dlouho toofill, vezměte v úvahu zmírnění tím, že:
* Snižuje počet hello funkce bolts, takže jsou méně toofill vyrovnávací paměti.
* S založené na čase nebo na základě počtu zásad, kde je hflush() spustí každých x vyprázdnění nebo každých milisekundách y a hello řazených kolekcí členů nashromáždila, pokud jsou potvrzené zpět.

Všimněte si, že hello propustnost v tomto případě je nižší, ale s pomalým rychlost událostí, maximální propustnost není hello největších cíl přesto. Tyto způsoby zmírnění vám pomůže zmenšit hello celkový čas, která je potřebná pro řazené kolekce členů tooflow prostřednictvím toohello úložiště. To může být důležité, zda chcete, aby kanál v reálném čase i pomocí rychlost nízkou událostí. Všimněte si také, zda vaše míra příchozích řazené kolekce členů je nízká, je třeba upravit parametr topology.message.timeout_secs hello, takže hello řazených kolekcí členů není vypršení časového limitu při jejich získávání uložená do vyrovnávací paměti nebo zpracovat.

## <a name="monitor-your-topology-in-storm"></a>Monitorování topologie v Storm  
Když topologie běží, je možné jej monitorovat v uživatelském rozhraní Storm hello. Zde jsou hlavní parametry toolook hello na:

* **Celkový proces spuštění latence.** Toto je průměrný čas hello, jeden záznam trvá toobe vysílaných hello spout, zpracovává hello bolt a potvrzené.

* **Celkový počet bolt proces latence.** Toto je průměrný čas hello spotřebovaného hello řazené kolekce členů v hello bolt, dokud neobdrží potvrzení.

* **Celkový počet bolt spustit latence.** Toto je průměrná hello času stráveného hello bolt v hello spuštění metody.

* **Počet chyb.** Vztahuje se toohello počet řazených kolekcí členů, která se nezdařila toobe plně zpracovány před vypršením časového limitu.

* **Kapacity.** Toto je míra je zatížení systému. Pokud tento počet je 1, fungují tak rychle, jak mohou vaše funkce bolts. Pokud je menší než 1, zvýšit paralelismus hello. Pokud je větší než 1, snížit paralelismus hello.

## <a name="troubleshoot-common-problems"></a>Řešení běžných potíží
Tady jsou několik běžných scénářů řešení potíží.
* **Mnoho řazených kolekcí členů jsou vypršení časového limitu.** Podívejte se na každém uzlu v hello topologie toodetermine kde hello problémovým místem je. Hello nejběžnější důvodem je, že funkce bolts hello nejsou možné tookeep až s funkcích spouts hello. To vede tootuples zdokonaleným hello vnitřní vyrovnávací paměti při zpracování toobe čekání. Zvažte možnost zvýšení hodnoty časového limitu hello nebo snížení hello maximální spout čekající na vyřízení.

* **Je latence vysoké celkový proces spouštění, ale proces latence nízkou bolt.** V takovém případě je možné, že hello řazené kolekce členů nejsou potvrzování dostatečně fast. Zkontrolujte, zda jsou k dispozici dostatečný počet acknowledgers. Další možností je, že při před hello bolts zahájení zpracování je čekají ve frontě hello příliš dlouho. Snížit maximální spout hello čekající na vyřízení.

* **Je vysoká bolt provést latence.** To znamená, že metoda execute() hello vaší bolt trvá příliš dlouho. Optimalizace kódu hello nebo podívejte se na velikosti zápisu a vyprázdnit chování.

### <a name="data-lake-store-throttling"></a>Omezení data Lake Store
Jestli jste nedosáhli hello omezení šířky pásma poskytuje Data Lake Store, může se zobrazit selhání úkolů. Omezení chyby v protokolech úloh. Paralelismus hello může snížit zvýšením velikosti kontejneru.    

Pokud jste jsou získávání omezené, toocheck povolení protokolování na straně klienta hello ladění hello:

1. V **Ambari** > **Storm** > **konfigurace** > **Advanced storm-worker-log4j**, změnit  **&lt;kořenový úroveň = "informace"&gt;**  příliš**&lt;kořenový úroveň = "ladění"&gt;**. Restartujte všechny hello uzly nebo službu pro hello konfigurace tootake vliv.
2. Topologie Storm hello monitorování protokolů na uzly pracovního procesu (v části /var/log/storm/worker-artifacts /&lt;TopologyName&gt;/&lt;port&gt;/worker.log) pro Data Lake Store omezení výjimky.

## <a name="next-steps"></a>Další kroky
Optimalizace další výkonu pro Storm může být odkazováno v [tomto blogu](https://blogs.msdn.microsoft.com/shanyu/2015/05/14/performance-tuning-for-hdinsight-storm-and-microsoft-azure-eventhubs/).

Toorun Další příklady najdete v části [následujícímu na Githubu](https://github.com/hdinsight/storm-performance-automation).
