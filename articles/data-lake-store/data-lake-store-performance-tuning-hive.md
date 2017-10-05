---
title: "Ladění pravidla výkonu Hive Azure Data Lake Store | Microsoft Docs"
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
ms.openlocfilehash: e10bf8f7cbae2b81d22823ff74fe652c6bcb2da3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-store"></a><span data-ttu-id="a2605-103">Pokyny pro Hive v HDInsight a Azure Data Lake Store optimalizace výkonu</span><span class="sxs-lookup"><span data-stu-id="a2605-103">Performance tuning guidance for Hive on HDInsight and Azure Data Lake Store</span></span>

<span data-ttu-id="a2605-104">K zajištění dobrý výkon v řadě případů použití v odlišných byly nastaveny výchozí nastavení.</span><span class="sxs-lookup"><span data-stu-id="a2605-104">The default settings have been set to provide good performance across many different use cases.</span></span>  <span data-ttu-id="a2605-105">Pro dotazy náročné na vstupně-výstupních operací lze získat lepší výkon s ADLS ladit Hive.</span><span class="sxs-lookup"><span data-stu-id="a2605-105">For I/O intensive queries, Hive can be tuned to get better performance with ADLS.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="a2605-106">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a2605-106">Prerequisites</span></span>

* <span data-ttu-id="a2605-107">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="a2605-107">**An Azure subscription**.</span></span> <span data-ttu-id="a2605-108">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a2605-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="a2605-109">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="a2605-109">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="a2605-110">Pokyny o tom, jak vytvořit najdete v tématu [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="a2605-110">For instructions on how to create one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="a2605-111">**Azure HDInsight cluster** s přístupem k účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a2605-111">**Azure HDInsight cluster** with access to a Data Lake Store account.</span></span> <span data-ttu-id="a2605-112">V tématu [vytvoření clusteru HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a2605-112">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="a2605-113">Ujistěte se, že povolení vzdálené plochy pro cluster.</span><span class="sxs-lookup"><span data-stu-id="a2605-113">Make sure you enable Remote Desktop for the cluster.</span></span>
* <span data-ttu-id="a2605-114">**Spuštění Hive v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="a2605-114">**Running Hive on HDInsight**.</span></span>  <span data-ttu-id="a2605-115">Další informace o probíhajících úloh Hive v HDInsight naleznete v tématu [použití Hive v HDInsight] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)</span><span class="sxs-lookup"><span data-stu-id="a2605-115">To learn about running Hive jobs on HDInsight, see [Use Hive on HDInsight] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)</span></span>
* <span data-ttu-id="a2605-116">**Ladění pokyny na ADLS výkonu**.</span><span class="sxs-lookup"><span data-stu-id="a2605-116">**Performance tuning guidelines on ADLS**.</span></span>  <span data-ttu-id="a2605-117">Obecný výkon koncepty, najdete v části [Data Lake Store výkonu ladění pokyny](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span><span class="sxs-lookup"><span data-stu-id="a2605-117">For general performance concepts, see [Data Lake Store Performance Tuning Guidance](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span></span>

## <a name="parameters"></a><span data-ttu-id="a2605-118">Parametry</span><span class="sxs-lookup"><span data-stu-id="a2605-118">Parameters</span></span>

<span data-ttu-id="a2605-119">Zde jsou nejdůležitější nastavení a vylaďte pro zlepšení výkonu ADLS:</span><span class="sxs-lookup"><span data-stu-id="a2605-119">Here are the most important settings to tune for improved ADLS performance:</span></span>

* <span data-ttu-id="a2605-120">**Hive.tez.Container.size** – množství paměti, které každý úlohy</span><span class="sxs-lookup"><span data-stu-id="a2605-120">**hive.tez.container.size** – the amount of memory used by each tasks</span></span>

* <span data-ttu-id="a2605-121">**velikost tez.Grouping.min** – minimální velikost každé mapper</span><span class="sxs-lookup"><span data-stu-id="a2605-121">**tez.grouping.min-size** – minimum size of each mapper</span></span>

* <span data-ttu-id="a2605-122">**velikost tez.Grouping.Max** – maximální velikost každé mapper</span><span class="sxs-lookup"><span data-stu-id="a2605-122">**tez.grouping.max-size** – maximum size of each mapper</span></span>

* <span data-ttu-id="a2605-123">**Hive.Exec.reducer.bytes.per.reducer** – velikost každé reduktorem</span><span class="sxs-lookup"><span data-stu-id="a2605-123">**hive.exec.reducer.bytes.per.reducer** – size of each reducer</span></span>

<span data-ttu-id="a2605-124">**Hive.tez.Container.size** – velikost kontejneru Určuje, kolik paměti je k dispozici pro každou úlohu.</span><span class="sxs-lookup"><span data-stu-id="a2605-124">**hive.tez.container.size** - The container size determines how much memory is available for each task.</span></span>  <span data-ttu-id="a2605-125">Toto je hlavní vstup pro řízení souběžnost v Hive.</span><span class="sxs-lookup"><span data-stu-id="a2605-125">This is the main input for controlling the concurrency in Hive.</span></span>  

<span data-ttu-id="a2605-126">**velikost tez.Grouping.min** – tento parametr můžete nastavit minimální velikost každé mapper.</span><span class="sxs-lookup"><span data-stu-id="a2605-126">**tez.grouping.min-size** – This parameter allows you to set the minimum size of each mapper.</span></span>  <span data-ttu-id="a2605-127">Pokud počet mappers, které vybral Tez je menší než hodnota tohoto parametru, Tez použije nastavená hodnota.</span><span class="sxs-lookup"><span data-stu-id="a2605-127">If the number of mappers that Tez chooses is smaller than the value of this parameter, then Tez will use the value set here.</span></span>  

<span data-ttu-id="a2605-128">**velikost tez.Grouping.Max** – parametr můžete nastavit maximální velikost každé mapper.</span><span class="sxs-lookup"><span data-stu-id="a2605-128">**tez.grouping.max-size** – The parameter allows you to set the maximum size of each mapper.</span></span>  <span data-ttu-id="a2605-129">Pokud počet mappers, které vybral Tez je větší než hodnota tohoto parametru, Tez použije nastavená hodnota.</span><span class="sxs-lookup"><span data-stu-id="a2605-129">If the number of mappers that Tez chooses is larger than the value of this parameter, then Tez will use the value set here.</span></span>  

<span data-ttu-id="a2605-130">**Hive.Exec.reducer.bytes.per.reducer** – tento parametr nastaví velikost každé reduktorem.</span><span class="sxs-lookup"><span data-stu-id="a2605-130">**hive.exec.reducer.bytes.per.reducer** – This parameter sets the size of each reducer.</span></span>  <span data-ttu-id="a2605-131">Ve výchozím nastavení je každý reduktorem 256MB.</span><span class="sxs-lookup"><span data-stu-id="a2605-131">By default, each reducer is 256MB.</span></span>  

## <a name="guidance"></a><span data-ttu-id="a2605-132">Doprovodné materiály</span><span class="sxs-lookup"><span data-stu-id="a2605-132">Guidance</span></span>

<span data-ttu-id="a2605-133">**Nastavit hive.exec.reducer.bytes.per.reducer** – výchozí hodnota funguje dobře, když nekomprimované data.</span><span class="sxs-lookup"><span data-stu-id="a2605-133">**Set hive.exec.reducer.bytes.per.reducer** – The default value works well when the data is uncompressed.</span></span>  <span data-ttu-id="a2605-134">Pro data, která je komprimován by měl zmenšete velikost reduktorem.</span><span class="sxs-lookup"><span data-stu-id="a2605-134">For data that is compressed, you should reduce the size of the reducer.</span></span>  

<span data-ttu-id="a2605-135">**Nastavit hive.tez.container.size** – v každém uzlu, je zadána yarn.nodemanager.resource.memory mb paměti a musí být správně v clusteru HDI ve výchozím nastavení.</span><span class="sxs-lookup"><span data-stu-id="a2605-135">**Set hive.tez.container.size** – In each node, memory is specified by yarn.nodemanager.resource.memory-mb and should be correctly set on HDI cluster by default.</span></span>  <span data-ttu-id="a2605-136">Další informace o nastavení odpovídající paměti v YARN najdete [post](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).</span><span class="sxs-lookup"><span data-stu-id="a2605-136">For additional information on setting the appropriate memory in YARN, see this [post](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).</span></span>

<span data-ttu-id="a2605-137">Zatížení s intenzivním vstupně-výstupních operací můžete těžit z další paralelismus snížením velikosti kontejneru Tez.</span><span class="sxs-lookup"><span data-stu-id="a2605-137">I/O intensive workloads can benefit from more parallelism by decreasing the Tez container size.</span></span> <span data-ttu-id="a2605-138">To umožňuje uživateli víc kontejnerů, což zvyšuje souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="a2605-138">This gives the user more containers which increases concurrency.</span></span>  <span data-ttu-id="a2605-139">Ale některé dotazy Hive vyžadovat značné množství paměti (např. MapJoin).</span><span class="sxs-lookup"><span data-stu-id="a2605-139">However, some Hive queries require a significant amount of memory (e.g. MapJoin).</span></span>  <span data-ttu-id="a2605-140">Pokud úloha nemá dostatek paměti, zobrazí se nedostatku paměti výjimky za běhu.</span><span class="sxs-lookup"><span data-stu-id="a2605-140">If the task does not have enough memory, you will get an out of memory exception during runtime.</span></span>  <span data-ttu-id="a2605-141">Pokud se zobrazí nedostatek paměti výjimky, měli byste zvýšit velikost paměti.</span><span class="sxs-lookup"><span data-stu-id="a2605-141">If you receive out of memory exceptions, then you should increase the memory.</span></span>   

<span data-ttu-id="a2605-142">Souběžné počet úloh spuštěných nebo paralelismus bude možné ohraničené celkovou velikost paměti YARN.</span><span class="sxs-lookup"><span data-stu-id="a2605-142">The concurrent number of tasks running or parallelism will be bounded by the total YARN memory.</span></span>  <span data-ttu-id="a2605-143">Počet kontejnerů YARN se určují, jak velký počet souběžných úloh můžete spustit.</span><span class="sxs-lookup"><span data-stu-id="a2605-143">The number of YARN containers will dictate how many concurrent tasks can run.</span></span>  <span data-ttu-id="a2605-144">Pokud chcete najít YARN paměti na jeden uzel, můžete přejít na Ambari.</span><span class="sxs-lookup"><span data-stu-id="a2605-144">To find the YARN memory per node, you can go to Ambari.</span></span>  <span data-ttu-id="a2605-145">Přejděte do YARN a zobrazit kartu konfigurací.</span><span class="sxs-lookup"><span data-stu-id="a2605-145">Navigate to YARN and view the Configs tab.</span></span>  <span data-ttu-id="a2605-146">V tomto okně se zobrazí YARN paměti.</span><span class="sxs-lookup"><span data-stu-id="a2605-146">The YARN memory is displayed in this window.</span></span>  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
<span data-ttu-id="a2605-147">Klíč k zlepšení výkonu pomocí ADLS je zvýšit souběžnost co nejvíc.</span><span class="sxs-lookup"><span data-stu-id="a2605-147">The key to improving performance using ADLS is to increase the concurrency as much as possible.</span></span>  <span data-ttu-id="a2605-148">Tez automaticky vypočítá počet úloh, které mají být vytvořeny, takže není potřeba ho nastavit.</span><span class="sxs-lookup"><span data-stu-id="a2605-148">Tez automatically calculates the number of tasks that should be created so you do not need to set it.</span></span>   

## <a name="example-calculation"></a><span data-ttu-id="a2605-149">Příklad výpočtu</span><span class="sxs-lookup"><span data-stu-id="a2605-149">Example Calculation</span></span>

<span data-ttu-id="a2605-150">Řekněme, že máte cluster D14 8 uzlu.</span><span class="sxs-lookup"><span data-stu-id="a2605-150">Let’s say you have an 8 node D14 cluster.</span></span>  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a><span data-ttu-id="a2605-151">Omezení</span><span class="sxs-lookup"><span data-stu-id="a2605-151">Limitations</span></span>
<span data-ttu-id="a2605-152">**ADLS omezení**</span><span class="sxs-lookup"><span data-stu-id="a2605-152">**ADLS throttling**</span></span> 

<span data-ttu-id="a2605-153">UIf dosáhl omezení šířky pásma poskytované ADLS, byste začali zobrazíte selhání úkolů.</span><span class="sxs-lookup"><span data-stu-id="a2605-153">UIf you hit the limits of bandwidth provided by ADLS, you would start to see task failures.</span></span> <span data-ttu-id="a2605-154">To může být identifikován sledování omezení chyby v protokolech úloh.</span><span class="sxs-lookup"><span data-stu-id="a2605-154">This could be identified by observing throttling errors in task logs.</span></span>  <span data-ttu-id="a2605-155">Paralelismus může snížit zvýšením velikosti kontejneru Tez.</span><span class="sxs-lookup"><span data-stu-id="a2605-155">You can decrease the parallelism by increasing Tez container size.</span></span>  <span data-ttu-id="a2605-156">Pokud potřebujete další souběžnosti pro úlohu, kontaktujte nás.</span><span class="sxs-lookup"><span data-stu-id="a2605-156">If you need more concurrency for your job, please contact us.</span></span>   

<span data-ttu-id="a2605-157">Pokud chcete zkontrolovat, pokud jste jsou získávání omezené, musíte povolit ladění na straně klienta protokolování.</span><span class="sxs-lookup"><span data-stu-id="a2605-157">To check if you are getting throttled, you need to enable the debug logging on the client side.</span></span> <span data-ttu-id="a2605-158">Zde je, jak můžete to udělat:</span><span class="sxs-lookup"><span data-stu-id="a2605-158">Here’s how you can do that:</span></span>

1. <span data-ttu-id="a2605-159">Uveďte následující vlastnost ve vlastnostech log4j v konfiguračním Hive.</span><span class="sxs-lookup"><span data-stu-id="a2605-159">Put the following property in the log4j properties in Hive config.</span></span> <span data-ttu-id="a2605-160">To lze provést ze zobrazení Ambari: log4j.logger.com.microsoft.azure.datalake.store=DEBUG restartovat všechny uzly nebo služby, konfigurace se projeví.</span><span class="sxs-lookup"><span data-stu-id="a2605-160">This can be done from Ambari view: log4j.logger.com.microsoft.azure.datalake.store=DEBUG Restart all the nodes/service for the config to take effect.</span></span>

2. <span data-ttu-id="a2605-161">Pokud jste jsou získávání omezené, se zobrazí kód HTTP 429 chyby v souboru protokolu hive.</span><span class="sxs-lookup"><span data-stu-id="a2605-161">If you are getting throttled, you’ll see the HTTP 429 error code in the hive log file.</span></span> <span data-ttu-id="a2605-162">Soubor protokolu hive je v /tmp/&lt;uživatele&gt;/hive.log</span><span class="sxs-lookup"><span data-stu-id="a2605-162">The hive log file is in /tmp/&lt;user&gt;/hive.log</span></span>

## <a name="further-information-on-hive-tuning"></a><span data-ttu-id="a2605-163">Další informace o ladění Hive</span><span class="sxs-lookup"><span data-stu-id="a2605-163">Further information on Hive tuning</span></span>

<span data-ttu-id="a2605-164">Tady je několik blogy, které vám pomůžou optimalizovat dotazy Hive:</span><span class="sxs-lookup"><span data-stu-id="a2605-164">Here are a few blogs that will help tune your Hive queries:</span></span>
* [<span data-ttu-id="a2605-165">Optimalizace dotazů Hive pro Hadoop v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a2605-165">Optimize Hive queries for Hadoop in HDInsight</span></span>](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [<span data-ttu-id="a2605-166">Řešení potíží s výkon dotazů Hive</span><span class="sxs-lookup"><span data-stu-id="a2605-166">Troubleshooting Hive query performance</span></span>](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [<span data-ttu-id="a2605-167">Ignite obraťte na optimalizaci Hive v HDInsight</span><span class="sxs-lookup"><span data-stu-id="a2605-167">Ignite talk on optimize Hive on HDInsight</span></span>](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
