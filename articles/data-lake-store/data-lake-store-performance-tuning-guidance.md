---
title: "aaaAzure ladění pokyny pro Data Lake Store výkonu | Microsoft Docs"
description: "Ladění pravidla výkonu Azure Data Lake Store"
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: cgronlun
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: stewu
ms.openlocfilehash: 939faa068c0f81d18d9533956f4d336bc4d0cbe3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="tuning-azure-data-lake-store-for-performance"></a>Ladění výkonu Azure Data Lake Store

Data Lake Store podporuje vysokou propustností k analýze intenzivním vstupně-výstupních operací a přesun dat.  V Azure Data Lake Store pomocí všech dostupných propustnost – hello množství dat, která můžou číst nebo zapisovat za sekundu – je důležité tooget hello nejlepší výkon.  To se dosáhne provedením tolik čte a zapisuje paralelně nejdříve.

![Data Lake Store výkonu](./media/data-lake-store-performance-tuning-guidance/throughput.png)

Azure Data Lake Store můžete škálovat tooprovide hello nezbytné propustnost pro všechny scénáře analýzy. Ve výchozím nastavení účet Azure Data Lake Store poskytuje automaticky dostatek propustnost toomeet hello potřebám hlavních kategorií případy použití. Hello případech, kde zákazníci spustit do hello výchozí limit, hello účtu ADLS může být nakonfigurované tooprovide další propustnost že se obrátíte na podporu společnosti Microsoft.

## <a name="data-ingestion"></a>Přijímání dat

Při příjem dat z tooADLS systému zdroje, je důležité tooconsider, který hello zdrojového hardwaru, zdroj síťového hardwaru a tooADLS připojení k síti může být hello potíže.  

![Data Lake Store výkonu](./media/data-lake-store-performance-tuning-guidance/bottleneck.png)

Je důležité, že tooensure, který hello přesun dat nemá vliv tyto faktory.

### <a name="source-hardware"></a>Zdroj hardwaru

Ať používáte místní počítače nebo virtuální počítače v Azure, měli byste pečlivě vybrat vhodný hardware hello. Pro zdroj disku Hardware raději tooHDDs SSD a vyberte hardware disku s rychlejší diskových jednotek. Pro zdroj síťový Hardware použijte hello nejrychlejší možné síťové adaptéry.  V Azure doporučujeme D14 virtuálních počítačích Azure, které mají hello správně výkonné disku a síťového hardwaru.

### <a name="network-connectivity-tooazure-data-lake-store"></a>Síťové připojení tooAzure Data Lake Store

Hello síťové připojení mezi zdrojem dat a Azure Data Lake store může být někdy hello potíže. Pokud je zdroj dat na místě, zvažte použití vyhrazených odkaz s [Azure ExpressRoute](https://azure.microsoft.com/en-us/services/expressroute/) . Pokud je zdroj dat v Azure, bude hello výkon nejlepší při hello data jsou v hello stejné oblasti Azure jako hello Data Lake Store.

### <a name="configure-data-ingestion-tools-for-maximum-parallelization"></a>Konfigurace nástrojů přijímání dat pro maximální paralelizace

Jakmile vyřešili hello zdrojového hardwaru a sítě kritická místa připojení výše, jste připravené tooconfigure vaše přijímání nástroje. Hello následující tabulka shrnuje nastavení hello klíče pro několik oblíbených přijímání nástrojů a poskytuje podrobné ladění články pro ně výkonu.  informace o tom, které nástroj toouse pro váš scénář toolearn získáte na tomto [článku](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-data-scenarios).

| Nástroj               | Nastavení     | Další informace                                                                 |
|--------------------|------------------------------------------------------|------------------------------|
| PowerShell       | PerFileThreadCount ConcurrentFileCount |  [Odkaz](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-powershell#performance-guidance-while-using-powershell)   |
| AdlCopy    | Azure Data Lake Analytics jednotky  |   [Odkaz](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-copy-data-azure-storage-blob#performance-considerations-for-using-adlcopy)         |
| DistCp            | -m (Mapovač)   | [Odkaz](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-copy-data-wasb-distcp#performance-considerations-while-using-distcp)                             |
| Azure Data Factory| parallelCopies    | [Odkaz](../data-factory/data-factory-copy-activity-performance.md)                          |
| Sqoop           | FS.Azure.Block.Size -m (Mapovač)    |   [Odkaz](https://blogs.msdn.microsoft.com/bigdatasupport/2015/02/17/sqoop-job-performance-tuning-in-hdinsight-hadoop/)        |

## <a name="structure-your-data-set"></a>Struktury sadu dat

Pokud data uložená v Data Lake Store, hello velikost souboru, počet souborů a struktura složek mít vliv na výkon.  Hello následující část popisuje osvědčené postupy v těchto oblastech.  

### <a name="file-size"></a>Velikost souboru

Analýza stroje například HDInsight a Azure Data Lake Analytics obvykle mají režijní náklady na soubor.  Pokud data ukládáte jako mnoho malých souborů, to může mít negativní vliv na výkon.  

Obecně platí organizování vašich dat ve větší velikosti souborů pro lepší výkon.  Jako existuje pravidlo uspořádání datových sad v souborech 256 MB nebo větší. V některých případech, například bitové kopie a binární data, není možné tooprocess je paralelně.  V těchto případech se doporučuje tookeep jednotlivých souborů v části 2GB.

V některých případech datových kanálů mají omezenou kontrolu nad hello nezpracovaná data, která má velké množství malých souborů.  Je doporučeno toohave "vaření" proces, který generuje větší soubory toouse pro příjem dat aplikace.  

### <a name="organizing-time-series-data-in-folders"></a>Uspořádání data časové řady ve složkách

Pro úlohy Hive a ADLA oddílu vyřazování data časové řady může pomoct některé dotazy číst pouze podmnožinu dat hello, což zvyšuje výkon.    

Tyto kanály, které ingestují data časové řady, často umístit své soubory s velmi strukturovaných pojmenovávání souborů a složek. Níže je velmi běžné příklad, který vidíte pro data, která je strukturovaná data:

    \DataSet\YYYY\MM\DD\datafile_YYYY_MM_DD.tsv

Všimněte si, že hello data a času informace se zobrazí jako složky i v hello filename.

Datum a čas následující hello je běžný vzor

    \DataSet\YYYY\MM\DD\HH\mm\datafile_YYYY_MM_DD_HH_mm.tsv

Volba hello bude s hello složku a soubor organizace by měl znovu, optimalizovat pro hello velkých souborů a přiměřené počet souborů v každé složky.

## <a name="optimizing-io-intensive-jobs-on-hadoop-and-spark-workloads-on-hdinsight"></a>Optimalizace náročné úlohy vstupně-výstupních operací na úlohy Hadoop a Spark v HDInsight

Úlohy spadat do jednoho z následujících tří kategorií hello:

* **Náročné na prostředky procesoru.**  Tyto úlohy má dlouhou výpočetní dobu s minimálním časy vstupu a výstupu.  Mezi příklady patří strojového učení a přirozeného jazyka zpracování úloh.  
* **Velké množství paměti.**  Tyto úlohy použijte velké množství paměti.  Mezi příklady patří PageRank a analýzu v reálném čase úlohy.  
* **Vstupně-výstupní operace náročné na prostředky.**  Tyto úlohy tráví většinu jejich doby provádění vstupně-výstupní operace.  Běžným příkladem jsou kopie úlohu, u které pouze operacích čtení a zápisu.  Další příklady data přípravy úlohy, které číst velké množství dat, provádí některé transformaci dat, a pak zápisy hello back toohello úložišti.  

Hello pokynů je pouze použít tooI/O náročné úlohy.

### <a name="general-considerations-for-an-hdinsight-cluster"></a>Obecné aspekty pro cluster služby HDInsight

* **HDInsight verze.** Pro nejlepší výkon použijte hello nejnovější verze služby HDInsight.
* **Oblasti.** Umístit hello Data Lake Store hello stejné oblasti jako hello clusteru HDInsight.  

Cluster služby HDInsight se skládá ze dvou hlavních uzlech a některé uzly pracovního procesu. Každý pracovní uzel poskytuje určitý počet jader a paměti, což je dáno hello typ virtuálního počítače.  Při spuštění úlohy, je YARN vyjednavač hello prostředku, který přiděluje hello dostupná paměť a počet jader toocreate kontejnerů.  Každý kontejner spustí hello úkoly potřebné toocomplete hello úlohu.  Kontejnery spustit v úlohách paralelní tooprocess rychle. Proto spuštěním tolik paralelní kontejnery nejdříve vyšší výkon.

Existují tři vrstvy v rámci clusteru služby HDInsight, který může být ujít tooincrease hello počet kontejnerů a použít všechny dostupné propustnost.  

* **Fyzickou vrstvu**
* **YARN vrstvy**
* **Zatížení vrstvy**

### <a name="physical-layer"></a>Fyzickou vrstvu

**Spusťte clusteru s více uzly nebo větší velikosti virtuálních počítačů.**  Větší cluster vám umožní toorun je víc kontejnerů YARN, jak ukazuje následující obrázek hello.

![Data Lake Store výkonu](./media/data-lake-store-performance-tuning-guidance/VM.png)

**Použijte virtuální počítače s větší šířku pásma sítě.**  Hello šířku pásma sítě může být kritický bod, pokud je menší šířku pásma sítě než propustnost Data Lake Store.  Různé virtuální počítače budou mít různých velikosti šířky pásma sítě.  Vyberte virtuální počítač – typ, který má hello největší možné šířku pásma sítě.

### <a name="yarn-layer"></a>YARN vrstvy

**Použití menší YARN kontejnerů.**  Snížení velikosti hello každý kontejner toocreate YARN další kontejnery s hello stejný objem prostředků.

![Data Lake Store výkonu](./media/data-lake-store-performance-tuning-guidance/small-containers.png)

V závislosti na velikosti pracovní zátěže bude vždy minimální velikost YARN kontejneru, která je potřeba. Pokud vyberete příliš malá kontejner, vaše úlohy se spustí do problémy z důvodu nedostatku paměti. Kontejnery YARN obvykle by měla být menší než 1GB. Je běžné kontejnery YARN toosee 3GB. Pro některé úlohy pravděpodobně bude třeba větší kontejnery YARN.  

**Zvýšit jader na kontejner YARN.**  Zvýšíte číslo hello jádra přidělené tooeach kontejneru tooincrease hello počet paralelních úloh, které běží v každém kontejneru.  Tento postup funguje pro aplikace jako Spark, které se spustit několik úkolů na kontejneru.  Pro aplikace jako Hive, která spustit jeden vláken v jednotlivých kontejnerech, je lepší toohave víc kontejnerů než více jader na kontejneru.   

### <a name="workload-layer"></a>Zatížení vrstvy

**Použijte všechny dostupné kontejnery.**  Nastavit hello počet úloh toobe stejná nebo větší než počet hello dostupné kontejnery, takže jsou využité všechny prostředky.

![Data Lake Store výkonu](./media/data-lake-store-performance-tuning-guidance/use-containers.png)

**Neúspěšné úlohy jsou nákladné.** Pokud má každý úkol velké množství dat tooprocess, selhání úkolu výsledků v nákladné opakování.  Proto je lepší toocreate další úlohy, z nichž každý malé množství dat zpracovává.

V přidání toohello obecné pokyny výše každá aplikace má k dispozici tootune různé parametry pro konkrétní aplikace. Hello tabulce jsou uvedeny některé parametry a odkazy tooget hello začít s optimalizace výkonu pro každou aplikaci.

| Úloha               | Parametr tooset úlohy                                                         |
|--------------------|-------------------------------------------------------------------------------------|
| [Spark v HDInisight](data-lake-store-performance-tuning-spark.md)       | <ul><li>Poče vykonavatelů</li><li>Vykonavatele paměti</li><li>Vykonavatele jader</li></ul> |
| [Hive v HDInsight](data-lake-store-performance-tuning-hive.md)    | <ul><li>Hive.tez.Container.Size</li></ul>         |
| [MapReduce v HDInsight](data-lake-store-performance-tuning-mapreduce.md)            | <ul><li>mapreduce.map.Memory</li><li>Mapreduce.job.Maps</li><li>mapreduce.reduce.Memory</li><li>Mapreduce.job.reduces</li></ul> |
| [Storm v HDInsight](data-lake-store-performance-tuning-storm.md)| <ul><li>Počet pracovních procesů</li><li>Počet instancí vykonavatele spout</li><li>Počet instancí vykonavatele bolt </li><li>Počet funkcí spout úlohy</li><li>Počet úloh bolt</li></ul>|

## <a name="see-also"></a>Viz také
* [Přehled Azure Data Lake Storu](data-lake-store-overview.md)
* [Začínáme s Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
