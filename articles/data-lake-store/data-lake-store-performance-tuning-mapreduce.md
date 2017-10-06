---
title: "aaaAzure Data Lake Store MapReduce výkonu ladění pokyny pro | Microsoft Docs"
description: "Ladění pravidla výkonu MapReduce Azure Data Lake Store"
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
ms.openlocfilehash: e21414a23530e65613c85156af0209c88ec54d2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tuning-guidance-for-mapreduce-on-hdinsight-and-azure-data-lake-store"></a>Pokyny pro MapReduce na HDInsight a Azure Data Lake Store optimalizace výkonu


## <a name="prerequisites"></a>Požadavky

* **Předplatné Azure**. Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).
* **Účet Azure Data Lake Store**. Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Azure HDInsight cluster** s tooa přístup k účtu Data Lake Store. V tématu [vytvoření clusteru HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md). Ujistěte se, že povolení vzdálené plochy pro hello clusteru.
* **Použití prostředí MapReduce v HDInsight**.  Další informace najdete v tématu [použití MapReduce v Hadoop v HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-mapreduce)  
* **Ladění pokyny na ADLS výkonu**.  Obecný výkon koncepty, najdete v části [Data Lake Store výkonu ladění pokyny](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)  

## <a name="parameters"></a>Parametry

Při spuštění úloh MapReduce, zde je nejdůležitější parametry hello tooincrease výkonu můžete konfigurovat v ADLS:

* **Mapreduce.map.Memory.MB** – hello množství paměti tooallocate tooeach mapper
* **Mapreduce.job.Maps** – hello počet úloh mapy na úlohu
* **Mapreduce.reduce.Memory.MB** – hello množství paměti tooallocate tooeach reduktorem
* **Mapreduce.job.reduces** – hello počet úloh snižte na úlohu

**Mapreduce.map.Memory / Mapreduce.reduce.memory** toto číslo je třeba upravit podle množství paměti, je potřeba pro mapu hello nebo snížit úloh.  výchozí hodnoty Hello mapreduce.map.memory a mapreduce.reduce.memory lze zobrazit v Ambari prostřednictvím konfigurace Yarn hello.  V Ambari přejděte tooYARN a zobrazit kartu konfigurací hello.  Zobrazí se Hello YARN paměti.  

**Mapreduce.job.Maps / Mapreduce.job.reduces** určí hello maximální počet mappers nebo přechodky toobe vytvořili.  Hello počet rozdělení určí, kolik mappers budou vytvořeny pro úlohu MapReduce hello.  Proto může získat méně mappers, než jste požadovali, pokud jsou menší rozdělení než hello počet mappers požadovaný.       

## <a name="guidance"></a>Doprovodné materiály

**Krok 1: Určení počet úloh spuštěných** – ve výchozím nastavení, použije MapReduce hello celý cluster pro úlohu.  Můžete použít menší hello clusteru pomocí méně mappers, než je k dispozici kontejnery.  Hello pokyny v tomto dokumentu se předpokládá, že aplikace je hello pouze aplikace běžící v clusteru.      

**Krok 2: Nastavení mapreduce.map.memory/mapreduce.reduce.memory** – hello velikosti hello paměti pro mapu a snížit úlohy budou závislé na určité úlohy.  Pokud chcete, aby tooincrease souběžnosti, můžete snížit velikost paměti hello.  Hello počet souběžně spuštěných úloh závisí na hello počet kontejnerů.  Snížením hello množství paměti na jeden mapper nebo reduktorem víc kontejnerů mohou být vytvořeny, které další mappers nebo přechodky toorun povolit souběžně.  Příliš mnoho klesá hello množství paměti, může způsobit některé procesy toorun nedostatek paměti.  Pokud dojde k chybě haldy při spuštění vaší úlohy, měli byste zvýšit hello paměť na mapper nebo reduktorem.  Měli byste zvážit přidání další kontejnerů přidá, velmi starat se pro každý další kontejneru, který může potenciálně snížit výkon.  Další možností je tooget více paměti pomocí clusteru, který má vyšší objemy paměti nebo zvýšení hello počet uzlů v clusteru.  Více paměti umožní více kontejnerů toobe používá, což znamená další souběžnosti.  

**Krok 3: Určení celkový YARN paměti** -tootune mapreduce.job.maps/mapreduce.job.reduces, měli byste zvážit hello množství celkový YARN paměti k dispozici pro použití.  Tyto informace jsou k dispozici v Ambari.  Přejděte tooYARN a zobrazit kartu konfigurací hello.  v tomto okně se zobrazí Hello YARN paměti.  Měli vynásobte hello YARN paměti hello počtu uzlů v clusteru tooget hello celkový YARN paměti.

    Total YARN memory = nodes * YARN memory per node
Pokud používáte prázdný clusteru, může být paměti hello celkové paměti YARN pro váš cluster.  Pokud používáte jiné aplikace paměti, pak můžete použít tooonly část paměti váš cluster snížením hello počet mappers nebo přechodky toohello počet kontejnerů chcete toouse.  

**Krok 4: Vypočítat počet kontejnerů YARN** – YARN kontejnery stanovují množství hello k dispozici pro úlohy hello souběžnosti.  Trvat celkové paměti YARN, vydělte mapreduce.map.memory.  

    # of YARN containers = total YARN memory / mapreduce.map.memory

**Krok 5: Nastavení mapreduce.job.maps/mapreduce.job.reduces** nastavit mapreduce.job.maps/mapreduce.job.reduces tooat minimálně hello počet kontejnerů k dispozici.  Můžete experimentovat další zvýšením hello počet mappers a přechodky toosee, je-li získat lepší výkon.  Uvědomte si, že další mappers budou mít dalších zásahů, má příliš mnoho mappers může snížit výkon.  

Plánování CPU a využití procesoru izolace jsou ve výchozím nastavení vypnuté, hello počet kontejnerů YARN je omezené paměti.

## <a name="example-calculation"></a>Příklad výpočtu

Řekněme, že teď máte cluster skládá z 8 D14 uzly a chcete toorun úlohy náročné vstupně-výstupních operací.  Zde jsou hello výpočty, které byste měli udělat:

**Krok 1: Určení počet úloh spuštěných** -pro náš příklad předpokládáme, že je naše úlohy hello pouze systémem.  

**Krok 2: Nastavení mapreduce.map.memory/mapreduce.reduce.memory** – pro náš příklad běží úlohy náročné vstupně-výstupních operací a rozhodnout, že 3 GB paměti pro mapování úlohy bude dostatečná.

    mapreduce.map.memory = 3GB
**Krok 3: Určení celkový YARN paměti**

    total memory from hello cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB
**Krok 4: Vypočítat počet YARN kontejnery**

    # of YARN containers = 768GB of available memory / 3 GB of memory =   256

**Krok 5: Nastavení mapreduce.job.maps/mapreduce.job.reduces**

    mapreduce.map.jobs = 256

## <a name="limitations"></a>Omezení

**ADLS omezení**

Jako víceklientské služby nastaví ADLS limity účtu úrovně šířky pásma.  Jestli jste nedosáhli tyto limity, začněte toosee selhání úkolů. To lze identifikovat podle sledování omezení chyby v protokolech úloh.  Pokud potřebujete větší šířku pásma pro úlohu, kontaktujte nás.   

toocheck, pokud jste jsou získávání omezené, musíte tooenable hello ladění na straně klienta hello protokolování. Zde je, jak můžete to udělat:

1. Vložení hello následující vlastnosti v hello log4j vlastnosti v Ambari > YARN > Konfigurace > Upřesnit yarn log4j: log4j.logger.com.microsoft.azure.datalake.store=DEBUG

2. Restartujte všechny hello uzly nebo službu pro hello konfigurace tootake vliv.

3. Pokud jste jsou získávání omezené, se zobrazí kód chyby HTTP 429 hello v souboru protokolu YARN hello. soubor protokolu YARN Hello je v /tmp/&lt;uživatele&gt;/yarn.log

## <a name="examples-toorun"></a>Příklady tooRun

toodemonstrate jak MapReduce běží na Azure Data Lake Store, zde jsou některé ukázkový kód, která byla spuštěna na cluster s hello následující nastavení:

* 16 uzlu D14v2
* Spuštění HDI 3.6 clusteru Hadoop

Pro počáteční bod tady jsou některé příklad příkazy toorun MapReduce Teragen, Terasort a Teravalidate.  Můžete upravit tyto příkazy podle vašich prostředků.

**Teragen**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 10000000000 adl://example/data/1TB-sort-input

**Terasort**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 -Dmapreduce.job.reduces=512 -Dmapreduce.reduce.memory.mb=3072 adl://example/data/1TB-sort-input adl://example/data/1TB-sort-output

**Teravalidate**

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapreduce.job.maps=512 -Dmapreduce.map.memory.mb=3072 adl://example/data/1TB-sort-output adl://example/data/1TB-sort-validate
