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
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-store"></a><span data-ttu-id="d52e5-103">Pokyny pro Hive v HDInsight a Azure Data Lake Store optimalizace výkonu</span><span class="sxs-lookup"><span data-stu-id="d52e5-103">Performance tuning guidance for Hive on HDInsight and Azure Data Lake Store</span></span>

<span data-ttu-id="d52e5-104">Hello výchozí nastavení tooprovide dobrý výkon v řadě případů použití v odlišných.</span><span class="sxs-lookup"><span data-stu-id="d52e5-104">hello default settings have been set tooprovide good performance across many different use cases.</span></span>  <span data-ttu-id="d52e5-105">Pro dotazy náročné na vstupně-výstupních operací může být Hive ujít tooget lepší výkon s ADLS.</span><span class="sxs-lookup"><span data-stu-id="d52e5-105">For I/O intensive queries, Hive can be tuned tooget better performance with ADLS.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="d52e5-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d52e5-106">Prerequisites</span></span>

* <span data-ttu-id="d52e5-107">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="d52e5-107">**An Azure subscription**.</span></span> <span data-ttu-id="d52e5-108">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d52e5-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="d52e5-109">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="d52e5-109">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="d52e5-110">Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="d52e5-110">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="d52e5-111">**Azure HDInsight cluster** s tooa přístup k účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="d52e5-111">**Azure HDInsight cluster** with access tooa Data Lake Store account.</span></span> <span data-ttu-id="d52e5-112">V tématu [vytvoření clusteru HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d52e5-112">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="d52e5-113">Ujistěte se, že povolení vzdálené plochy pro hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="d52e5-113">Make sure you enable Remote Desktop for hello cluster.</span></span>
* <span data-ttu-id="d52e5-114">**Spuštění Hive v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="d52e5-114">**Running Hive on HDInsight**.</span></span>  <span data-ttu-id="d52e5-115">toolearn o spuštění úlohy Hive v HDInsight, najdete v části [použití Hive v HDInsight] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)</span><span class="sxs-lookup"><span data-stu-id="d52e5-115">toolearn about running Hive jobs on HDInsight, see [Use Hive on HDInsight] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)</span></span>
* <span data-ttu-id="d52e5-116">**Ladění pokyny na ADLS výkonu**.</span><span class="sxs-lookup"><span data-stu-id="d52e5-116">**Performance tuning guidelines on ADLS**.</span></span>  <span data-ttu-id="d52e5-117">Obecný výkon koncepty, najdete v části [Data Lake Store výkonu ladění pokyny](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span><span class="sxs-lookup"><span data-stu-id="d52e5-117">For general performance concepts, see [Data Lake Store Performance Tuning Guidance](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span></span>

## <a name="parameters"></a><span data-ttu-id="d52e5-118">Parametry</span><span class="sxs-lookup"><span data-stu-id="d52e5-118">Parameters</span></span>

<span data-ttu-id="d52e5-119">Zde jsou hello nejdůležitější tootune nastavení pro zlepšení výkonu ADLS:</span><span class="sxs-lookup"><span data-stu-id="d52e5-119">Here are hello most important settings tootune for improved ADLS performance:</span></span>

* <span data-ttu-id="d52e5-120">**Hive.tez.Container.size** – hello množství paměti, které každý úlohy</span><span class="sxs-lookup"><span data-stu-id="d52e5-120">**hive.tez.container.size** – hello amount of memory used by each tasks</span></span>

* <span data-ttu-id="d52e5-121">**velikost tez.Grouping.min** – minimální velikost každé mapper</span><span class="sxs-lookup"><span data-stu-id="d52e5-121">**tez.grouping.min-size** – minimum size of each mapper</span></span>

* <span data-ttu-id="d52e5-122">**velikost tez.Grouping.Max** – maximální velikost každé mapper</span><span class="sxs-lookup"><span data-stu-id="d52e5-122">**tez.grouping.max-size** – maximum size of each mapper</span></span>

* <span data-ttu-id="d52e5-123">**Hive.Exec.reducer.bytes.per.reducer** – velikost každé reduktorem</span><span class="sxs-lookup"><span data-stu-id="d52e5-123">**hive.exec.reducer.bytes.per.reducer** – size of each reducer</span></span>

<span data-ttu-id="d52e5-124">**Hive.tez.Container.size** -hello kontejneru velikost určuje, kolik paměti je k dispozici pro každou úlohu.</span><span class="sxs-lookup"><span data-stu-id="d52e5-124">**hive.tez.container.size** - hello container size determines how much memory is available for each task.</span></span>  <span data-ttu-id="d52e5-125">Toto je hello hlavní vstup pro řízení hello souběžnost v Hive.</span><span class="sxs-lookup"><span data-stu-id="d52e5-125">This is hello main input for controlling hello concurrency in Hive.</span></span>  

<span data-ttu-id="d52e5-126">**velikost tez.Grouping.min** – tento parametr umožňuje tooset hello minimální velikost každé mapper.</span><span class="sxs-lookup"><span data-stu-id="d52e5-126">**tez.grouping.min-size** – This parameter allows you tooset hello minimum size of each mapper.</span></span>  <span data-ttu-id="d52e5-127">Pokud hello počet mappers, které vybral Tez je menší než hello hodnota tohoto parametru, Tez použije nastavená hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="d52e5-127">If hello number of mappers that Tez chooses is smaller than hello value of this parameter, then Tez will use hello value set here.</span></span>  

<span data-ttu-id="d52e5-128">**velikost tez.Grouping.Max** – parametr hello vám umožní tooset hello maximální velikost každé mapper.</span><span class="sxs-lookup"><span data-stu-id="d52e5-128">**tez.grouping.max-size** – hello parameter allows you tooset hello maximum size of each mapper.</span></span>  <span data-ttu-id="d52e5-129">Pokud hello počet mappers, které vybral Tez je větší než hello hodnota tohoto parametru, Tez použije nastavená hodnota hello.</span><span class="sxs-lookup"><span data-stu-id="d52e5-129">If hello number of mappers that Tez chooses is larger than hello value of this parameter, then Tez will use hello value set here.</span></span>  

<span data-ttu-id="d52e5-130">**Hive.Exec.reducer.bytes.per.reducer** – tento parametr nastaví hello velikost každé reduktorem.</span><span class="sxs-lookup"><span data-stu-id="d52e5-130">**hive.exec.reducer.bytes.per.reducer** – This parameter sets hello size of each reducer.</span></span>  <span data-ttu-id="d52e5-131">Ve výchozím nastavení je každý reduktorem 256MB.</span><span class="sxs-lookup"><span data-stu-id="d52e5-131">By default, each reducer is 256MB.</span></span>  

## <a name="guidance"></a><span data-ttu-id="d52e5-132">Doprovodné materiály</span><span class="sxs-lookup"><span data-stu-id="d52e5-132">Guidance</span></span>

<span data-ttu-id="d52e5-133">**Nastavit hive.exec.reducer.bytes.per.reducer** – hello výchozí hodnota funguje dobře, když nekomprimovaných dat hello.</span><span class="sxs-lookup"><span data-stu-id="d52e5-133">**Set hive.exec.reducer.bytes.per.reducer** – hello default value works well when hello data is uncompressed.</span></span>  <span data-ttu-id="d52e5-134">Pro data, která je komprimován by měl snížení velikosti hello reduktorem hello.</span><span class="sxs-lookup"><span data-stu-id="d52e5-134">For data that is compressed, you should reduce hello size of hello reducer.</span></span>  

<span data-ttu-id="d52e5-135">**Nastavit hive.tez.container.size** – v každém uzlu, je zadána yarn.nodemanager.resource.memory mb paměti a musí být správně v clusteru HDI ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="d52e5-135">**Set hive.tez.container.size** – In each node, memory is specified by yarn.nodemanager.resource.memory-mb and should be correctly set on HDI cluster by default.</span></span>  <span data-ttu-id="d52e5-136">Další informace o nastavení hello příslušné paměti v YARN najdete [post](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).</span><span class="sxs-lookup"><span data-stu-id="d52e5-136">For additional information on setting hello appropriate memory in YARN, see this [post](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).</span></span>

<span data-ttu-id="d52e5-137">Zatížení s intenzivním vstupně-výstupních operací můžete těžit z další paralelismus snížením velikosti kontejneru Tez hello.</span><span class="sxs-lookup"><span data-stu-id="d52e5-137">I/O intensive workloads can benefit from more parallelism by decreasing hello Tez container size.</span></span> <span data-ttu-id="d52e5-138">To dává hello uživatele víc kontejnerů, což zvyšuje souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="d52e5-138">This gives hello user more containers which increases concurrency.</span></span>  <span data-ttu-id="d52e5-139">Ale některé dotazy Hive vyžadovat značné množství paměti (např. MapJoin).</span><span class="sxs-lookup"><span data-stu-id="d52e5-139">However, some Hive queries require a significant amount of memory (e.g. MapJoin).</span></span>  <span data-ttu-id="d52e5-140">Pokud úloha hello nemá dostatek paměti, zobrazí se nedostatku paměti výjimky za běhu.</span><span class="sxs-lookup"><span data-stu-id="d52e5-140">If hello task does not have enough memory, you will get an out of memory exception during runtime.</span></span>  <span data-ttu-id="d52e5-141">Pokud se zobrazí nedostatek paměti výjimky, měli byste zvýšit hello paměti.</span><span class="sxs-lookup"><span data-stu-id="d52e5-141">If you receive out of memory exceptions, then you should increase hello memory.</span></span>   

<span data-ttu-id="d52e5-142">Hello souběžných počet úloh spuštěných nebo paralelismus bude možné ohraničené hello celkové paměti YARN.</span><span class="sxs-lookup"><span data-stu-id="d52e5-142">hello concurrent number of tasks running or parallelism will be bounded by hello total YARN memory.</span></span>  <span data-ttu-id="d52e5-143">Hello počet kontejnerů YARN se určují, jak velký počet souběžných úloh můžete spustit.</span><span class="sxs-lookup"><span data-stu-id="d52e5-143">hello number of YARN containers will dictate how many concurrent tasks can run.</span></span>  <span data-ttu-id="d52e5-144">toofind hello YARN paměti na jeden uzel, můžete přejít tooAmbari.</span><span class="sxs-lookup"><span data-stu-id="d52e5-144">toofind hello YARN memory per node, you can go tooAmbari.</span></span>  <span data-ttu-id="d52e5-145">Přejděte tooYARN a zobrazit kartu konfigurací hello.  v tomto okně se zobrazí Hello YARN paměti.</span><span class="sxs-lookup"><span data-stu-id="d52e5-145">Navigate tooYARN and view hello Configs tab.  hello YARN memory is displayed in this window.</span></span>  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
<span data-ttu-id="d52e5-146">Hello klíče tooimproving výkonu pomocí ADLS je tooincrease hello souběžnosti co nejvíc.</span><span class="sxs-lookup"><span data-stu-id="d52e5-146">hello key tooimproving performance using ADLS is tooincrease hello concurrency as much as possible.</span></span>  <span data-ttu-id="d52e5-147">Tez automaticky vypočítá hello počet úloh, které mají být vytvořeny, takže není nutné tooset ho.</span><span class="sxs-lookup"><span data-stu-id="d52e5-147">Tez automatically calculates hello number of tasks that should be created so you do not need tooset it.</span></span>   

## <a name="example-calculation"></a><span data-ttu-id="d52e5-148">Příklad výpočtu</span><span class="sxs-lookup"><span data-stu-id="d52e5-148">Example Calculation</span></span>

<span data-ttu-id="d52e5-149">Řekněme, že máte cluster D14 8 uzlu.</span><span class="sxs-lookup"><span data-stu-id="d52e5-149">Let’s say you have an 8 node D14 cluster.</span></span>  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a><span data-ttu-id="d52e5-150">Omezení</span><span class="sxs-lookup"><span data-stu-id="d52e5-150">Limitations</span></span>
<span data-ttu-id="d52e5-151">**ADLS omezení**</span><span class="sxs-lookup"><span data-stu-id="d52e5-151">**ADLS throttling**</span></span> 

<span data-ttu-id="d52e5-152">UIf dosáhl hello omezení šířky pásma, zadaný ve ADLS, byste začali toosee selhání úkolů.</span><span class="sxs-lookup"><span data-stu-id="d52e5-152">UIf you hit hello limits of bandwidth provided by ADLS, you would start toosee task failures.</span></span> <span data-ttu-id="d52e5-153">To může být identifikován sledování omezení chyby v protokolech úloh.</span><span class="sxs-lookup"><span data-stu-id="d52e5-153">This could be identified by observing throttling errors in task logs.</span></span>  <span data-ttu-id="d52e5-154">Paralelismus hello může snížit zvýšením velikosti kontejneru Tez.</span><span class="sxs-lookup"><span data-stu-id="d52e5-154">You can decrease hello parallelism by increasing Tez container size.</span></span>  <span data-ttu-id="d52e5-155">Pokud potřebujete další souběžnosti pro úlohu, kontaktujte nás.</span><span class="sxs-lookup"><span data-stu-id="d52e5-155">If you need more concurrency for your job, please contact us.</span></span>   

<span data-ttu-id="d52e5-156">toocheck, pokud jste jsou získávání omezené, musíte tooenable hello ladění na straně klienta hello protokolování.</span><span class="sxs-lookup"><span data-stu-id="d52e5-156">toocheck if you are getting throttled, you need tooenable hello debug logging on hello client side.</span></span> <span data-ttu-id="d52e5-157">Zde je, jak můžete to udělat:</span><span class="sxs-lookup"><span data-stu-id="d52e5-157">Here’s how you can do that:</span></span>

1. <span data-ttu-id="d52e5-158">Uveďte následující vlastnosti v hello log4j vlastnosti v konfiguraci Hive hello. To lze provést ze zobrazení Ambari: log4j.logger.com.microsoft.azure.datalake.store=DEBUG restartujte všechny hello uzly nebo službu pro hello konfigurace tootake vliv.</span><span class="sxs-lookup"><span data-stu-id="d52e5-158">Put hello following property in hello log4j properties in Hive config. This can be done from Ambari view: log4j.logger.com.microsoft.azure.datalake.store=DEBUG Restart all hello nodes/service for hello config tootake effect.</span></span>

2. <span data-ttu-id="d52e5-159">Pokud jste jsou získávání omezené, se zobrazí kód chyby HTTP 429 hello v souboru protokolu hello hive.</span><span class="sxs-lookup"><span data-stu-id="d52e5-159">If you are getting throttled, you’ll see hello HTTP 429 error code in hello hive log file.</span></span> <span data-ttu-id="d52e5-160">soubor protokolu hive Hello je v /tmp/&lt;uživatele&gt;/hive.log</span><span class="sxs-lookup"><span data-stu-id="d52e5-160">hello hive log file is in /tmp/&lt;user&gt;/hive.log</span></span>

## <a name="further-information-on-hive-tuning"></a><span data-ttu-id="d52e5-161">Další informace o ladění Hive</span><span class="sxs-lookup"><span data-stu-id="d52e5-161">Further information on Hive tuning</span></span>

<span data-ttu-id="d52e5-162">Tady je několik blogy, které vám pomůžou optimalizovat dotazy Hive:</span><span class="sxs-lookup"><span data-stu-id="d52e5-162">Here are a few blogs that will help tune your Hive queries:</span></span>
* [<span data-ttu-id="d52e5-163">Optimalizace dotazů Hive pro Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d52e5-163">Optimize Hive queries for Hadoop in HDInsight</span></span>](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [<span data-ttu-id="d52e5-164">Řešení potíží s výkon dotazů Hive</span><span class="sxs-lookup"><span data-stu-id="d52e5-164">Troubleshooting Hive query performance</span></span>](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [<span data-ttu-id="d52e5-165">Ignite obraťte na optimalizaci Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="d52e5-165">Ignite talk on optimize Hive on HDInsight</span></span>](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
