---
title: "aaaAzure Data Lake Store Hive výkonu ladění pokyny | Microsoft Docs"
description: "Ladění pravidla výkonu Hive Azure Data Lake Store"
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
ms.openlocfilehash: e44daeb6ad3b64e893c709df63b56444a330729f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-store"></a>Pokyny pro Hive v HDInsight a Azure Data Lake Store optimalizace výkonu

Hello výchozí nastavení tooprovide dobrý výkon v řadě případů použití v odlišných.  Pro dotazy náročné na vstupně-výstupních operací může být Hive ujít tooget lepší výkon s ADLS.  

## <a name="prerequisites"></a>Požadavky

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Účet Azure Data Lake Store**. Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Azure HDInsight cluster** s tooa přístup k účtu Data Lake Store. V tématu [vytvoření clusteru HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Ujistěte se, že povolení vzdálené plochy pro hello clusteru.
* **Spuštění Hive v HDInsight**.  toolearn o spuštění úlohy Hive v HDInsight, najdete v části [použití Hive v HDInsight] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)
* **Ladění pokyny na ADLS výkonu**.  Obecný výkon koncepty, najdete v části [Data Lake Store výkonu ladění pokyny](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)

## <a name="parameters"></a>Parametry

Zde jsou hello nejdůležitější tootune nastavení pro zlepšení výkonu ADLS:

* **Hive.tez.Container.size** – hello množství paměti, které každý úlohy

* **velikost tez.Grouping.min** – minimální velikost každé mapper

* **velikost tez.Grouping.Max** – maximální velikost každé mapper

* **Hive.Exec.reducer.bytes.per.reducer** – velikost každé reduktorem

**Hive.tez.Container.size** -hello kontejneru velikost určuje, kolik paměti je k dispozici pro každou úlohu.  Toto je hello hlavní vstup pro řízení hello souběžnost v Hive.  

**velikost tez.Grouping.min** – tento parametr umožňuje tooset hello minimální velikost každé mapper.  Pokud hello počet mappers, které vybral Tez je menší než hello hodnota tohoto parametru, Tez použije nastavená hodnota hello.  

**velikost tez.Grouping.Max** – parametr hello vám umožní tooset hello maximální velikost každé mapper.  Pokud hello počet mappers, které vybral Tez je větší než hello hodnota tohoto parametru, Tez použije nastavená hodnota hello.  

**Hive.Exec.reducer.bytes.per.reducer** – tento parametr nastaví hello velikost každé reduktorem.  Ve výchozím nastavení je každý reduktorem 256MB.  

## <a name="guidance"></a>Doprovodné materiály

**Nastavit hive.exec.reducer.bytes.per.reducer** – hello výchozí hodnota funguje dobře, když nekomprimovaných dat hello.  Pro data, která je komprimován by měl snížení velikosti hello reduktorem hello.  

**Nastavit hive.tez.container.size** – v každém uzlu, je zadána yarn.nodemanager.resource.memory mb paměti a musí být správně v clusteru HDI ve výchozím nastavení.  Další informace o nastavení hello příslušné paměti v YARN najdete [post](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).

Zatížení s intenzivním vstupně-výstupních operací můžete těžit z další paralelismus snížením velikosti kontejneru Tez hello. To dává hello uživatele víc kontejnerů, což zvyšuje souběžnosti.  Ale některé dotazy Hive vyžadovat značné množství paměti (např. MapJoin).  Pokud úloha hello nemá dostatek paměti, zobrazí se nedostatku paměti výjimky za běhu.  Pokud se zobrazí nedostatek paměti výjimky, měli byste zvýšit hello paměti.   

Hello souběžných počet úloh spuštěných nebo paralelismus bude možné ohraničené hello celkové paměti YARN.  Hello počet kontejnerů YARN se určují, jak velký počet souběžných úloh můžete spustit.  toofind hello YARN paměti na jeden uzel, můžete přejít tooAmbari.  Přejděte tooYARN a zobrazit kartu konfigurací hello.  v tomto okně se zobrazí Hello YARN paměti.  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
Hello klíče tooimproving výkonu pomocí ADLS je tooincrease hello souběžnosti co nejvíc.  Tez automaticky vypočítá hello počet úloh, které mají být vytvořeny, takže není nutné tooset ho.   

## <a name="example-calculation"></a>Příklad výpočtu

Řekněme, že máte cluster D14 8 uzlu.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a>Omezení
**ADLS omezení** 

UIf dosáhl hello omezení šířky pásma, zadaný ve ADLS, byste začali toosee selhání úkolů. To může být identifikován sledování omezení chyby v protokolech úloh.  Paralelismus hello může snížit zvýšením velikosti kontejneru Tez.  Pokud potřebujete další souběžnosti pro úlohu, kontaktujte nás.   

toocheck, pokud jste jsou získávání omezené, musíte tooenable hello ladění na straně klienta hello protokolování. Zde je, jak můžete to udělat:

1. Uveďte následující vlastnosti v hello log4j vlastnosti v konfiguraci Hive hello. To lze provést ze zobrazení Ambari: log4j.logger.com.microsoft.azure.datalake.store=DEBUG restartujte všechny hello uzly nebo službu pro hello konfigurace tootake vliv.

2. Pokud jste jsou získávání omezené, se zobrazí kód chyby HTTP 429 hello v souboru protokolu hello hive. soubor protokolu hive Hello je v /tmp/&lt;uživatele&gt;/hive.log

## <a name="further-information-on-hive-tuning"></a>Další informace o ladění Hive

Tady je několik blogy, které vám pomůžou optimalizovat dotazy Hive:
* [Optimalizace dotazů Hive pro Hadoop v HDInsight](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Řešení potíží s výkon dotazů Hive](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [Ignite obraťte na optimalizaci Hive v HDInsight](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
