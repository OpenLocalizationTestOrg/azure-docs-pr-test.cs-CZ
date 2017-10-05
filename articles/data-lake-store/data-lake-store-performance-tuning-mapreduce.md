---
title: "Ladění pravidla výkonu MapReduce Azure Data Lake Store | Microsoft Docs"
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
ms.openlocfilehash: 9528148792f083cb0e48d356e61cf61762ee954f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="performance-tuning-guidance-for-mapreduce-on-hdinsight-and-azure-data-lake-store"></a><span data-ttu-id="30f5d-103">Pokyny pro MapReduce na HDInsight a Azure Data Lake Store optimalizace výkonu</span><span class="sxs-lookup"><span data-stu-id="30f5d-103">Performance tuning guidance for MapReduce on HDInsight and Azure Data Lake Store</span></span>


## <a name="prerequisites"></a><span data-ttu-id="30f5d-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="30f5d-104">Prerequisites</span></span>

* <span data-ttu-id="30f5d-105">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="30f5d-105">**An Azure subscription**.</span></span> <span data-ttu-id="30f5d-106">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="30f5d-106">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="30f5d-107">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="30f5d-107">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="30f5d-108">Pokyny o tom, jak vytvořit najdete v tématu [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="30f5d-108">For instructions on how to create one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="30f5d-109">**Azure HDInsight cluster** s přístupem k účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="30f5d-109">**Azure HDInsight cluster** with access to a Data Lake Store account.</span></span> <span data-ttu-id="30f5d-110">V tématu [vytvoření clusteru HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="30f5d-110">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="30f5d-111">Ujistěte se, že povolení vzdálené plochy pro cluster.</span><span class="sxs-lookup"><span data-stu-id="30f5d-111">Make sure you enable Remote Desktop for the cluster.</span></span>
* <span data-ttu-id="30f5d-112">**Použití prostředí MapReduce v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="30f5d-112">**Using MapReduce on HDInsight**.</span></span>  <span data-ttu-id="30f5d-113">Další informace najdete v tématu [použití MapReduce v Hadoop v HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-mapreduce)</span><span class="sxs-lookup"><span data-stu-id="30f5d-113">For more information, see [Use MapReduce in Hadoop on HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-mapreduce)</span></span>  
* <span data-ttu-id="30f5d-114">**Ladění pokyny na ADLS výkonu**.</span><span class="sxs-lookup"><span data-stu-id="30f5d-114">**Performance tuning guidelines on ADLS**.</span></span>  <span data-ttu-id="30f5d-115">Obecný výkon koncepty, najdete v části [Data Lake Store výkonu ladění pokyny](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span><span class="sxs-lookup"><span data-stu-id="30f5d-115">For general performance concepts, see [Data Lake Store Performance Tuning Guidance](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span></span>  

## <a name="parameters"></a><span data-ttu-id="30f5d-116">Parametry</span><span class="sxs-lookup"><span data-stu-id="30f5d-116">Parameters</span></span>

<span data-ttu-id="30f5d-117">Při spuštění úloh MapReduce, zde jsou nejdůležitější parametry, které můžete nakonfigurovat a zvyšuje výkon na ADLS:</span><span class="sxs-lookup"><span data-stu-id="30f5d-117">When running MapReduce jobs, here are the most important parameters that you can configure to increase performance on ADLS:</span></span>

* <span data-ttu-id="30f5d-118">**Mapreduce.map.Memory.MB** – množství paměti pro každý mapper</span><span class="sxs-lookup"><span data-stu-id="30f5d-118">**Mapreduce.map.memory.mb** – The amount of memory to allocate to each mapper</span></span>
* <span data-ttu-id="30f5d-119">**Mapreduce.job.Maps** – počet úloh mapy na úlohu</span><span class="sxs-lookup"><span data-stu-id="30f5d-119">**Mapreduce.job.maps** – The number of map tasks per job</span></span>
* <span data-ttu-id="30f5d-120">**Mapreduce.reduce.Memory.MB** – množství paměti pro každý reduktorem</span><span class="sxs-lookup"><span data-stu-id="30f5d-120">**Mapreduce.reduce.memory.mb** – The amount of memory to allocate to each reducer</span></span>
* <span data-ttu-id="30f5d-121">**Mapreduce.job.reduces** – počet úloh snižte na úlohu</span><span class="sxs-lookup"><span data-stu-id="30f5d-121">**Mapreduce.job.reduces** – The number of reduce tasks per job</span></span>

<span data-ttu-id="30f5d-122">**Mapreduce.map.Memory / Mapreduce.reduce.memory** toto číslo je třeba upravit v závislosti na tom, kolik paměti je potřeba pro mapu a k omezení počtu úloh.</span><span class="sxs-lookup"><span data-stu-id="30f5d-122">**Mapreduce.map.memory / Mapreduce.reduce.memory** This number should be adjusted based on how much memory is needed for the map and/or reduce task.</span></span>  <span data-ttu-id="30f5d-123">Výchozí hodnoty mapreduce.map.memory a mapreduce.reduce.memory lze zobrazit v Ambari prostřednictvím konfigurace Yarn.</span><span class="sxs-lookup"><span data-stu-id="30f5d-123">The default values of mapreduce.map.memory and mapreduce.reduce.memory can be viewed in Ambari via the Yarn configuration.</span></span>  <span data-ttu-id="30f5d-124">V Ambari přejděte na YARN a zobrazit kartu konfigurací.</span><span class="sxs-lookup"><span data-stu-id="30f5d-124">In Ambari, navigate to YARN and view the Configs tab.</span></span>  <span data-ttu-id="30f5d-125">Zobrazí se YARN paměti.</span><span class="sxs-lookup"><span data-stu-id="30f5d-125">The YARN memory will be displayed.</span></span>  

<span data-ttu-id="30f5d-126">**Mapreduce.job.Maps / Mapreduce.job.reduces** to bude určit maximální počet mappers nebo přechodky, který se má vytvořit.</span><span class="sxs-lookup"><span data-stu-id="30f5d-126">**Mapreduce.job.maps / Mapreduce.job.reduces** This will determine the maximum number of mappers or reducers to be created.</span></span>  <span data-ttu-id="30f5d-127">Počet rozdělení určí, kolik mappers budou vytvořeny pro úlohu MapReduce.</span><span class="sxs-lookup"><span data-stu-id="30f5d-127">The number of splits will determine how many mappers will be created for the MapReduce job.</span></span>  <span data-ttu-id="30f5d-128">Proto může získat méně mappers, než jste požadovali, pokud jsou menší rozdělení než počet mappers požadovaný.</span><span class="sxs-lookup"><span data-stu-id="30f5d-128">Therefore, you may get less mappers than you requested if there are less splits than the number of mappers requested.</span></span>       

## <a name="guidance"></a><span data-ttu-id="30f5d-129">Doprovodné materiály</span><span class="sxs-lookup"><span data-stu-id="30f5d-129">Guidance</span></span>

<span data-ttu-id="30f5d-130">**Krok 1: Určení počet úloh spuštěných** – ve výchozím nastavení, použije MapReduce celý cluster pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="30f5d-130">**Step 1: Determine number of jobs running** - By default, MapReduce will use the entire cluster for your job.</span></span>  <span data-ttu-id="30f5d-131">Můžete použít menší clusteru pomocí méně mappers, než je k dispozici kontejnery.</span><span class="sxs-lookup"><span data-stu-id="30f5d-131">You can use less of the cluster by using less mappers than there are available containers.</span></span>  <span data-ttu-id="30f5d-132">Pokyny v tomto dokumentu se předpokládá, že aplikace je pouze aplikace běžící v clusteru.</span><span class="sxs-lookup"><span data-stu-id="30f5d-132">The guidance in this document assumes that your application is the only application running on your cluster.</span></span>      

<span data-ttu-id="30f5d-133">**Krok 2: Nastavení mapreduce.map.memory/mapreduce.reduce.memory** – velikost paměti pro mapu a snížit úlohy budou závislé na určité úlohy.</span><span class="sxs-lookup"><span data-stu-id="30f5d-133">**Step 2: Set mapreduce.map.memory/mapreduce.reduce.memory** –  The size of the memory for map and reduce tasks will be dependent on your specific job.</span></span>  <span data-ttu-id="30f5d-134">Pokud chcete zvýšit souběžnost můžete snížit velikost paměti.</span><span class="sxs-lookup"><span data-stu-id="30f5d-134">You can reduce the memory size if you want to increase concurrency.</span></span>  <span data-ttu-id="30f5d-135">Počet současně spuštěných úloh závisí na počet kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="30f5d-135">The number of concurrently running tasks depends on the number of containers.</span></span>  <span data-ttu-id="30f5d-136">Snížením množství paměti na mapper nebo reduktorem víc kontejnerů mohou být vytvořeny, která umožňují více mappers nebo přechodky spuštěny současně.</span><span class="sxs-lookup"><span data-stu-id="30f5d-136">By decreasing the amount of memory per mapper or reducer, more containers can be created, which enable more mappers or reducers to run concurrently.</span></span>  <span data-ttu-id="30f5d-137">Příliš mnoho snižují množství paměti může způsobit, že některé procesy spuštění nedostatek paměti.</span><span class="sxs-lookup"><span data-stu-id="30f5d-137">Decreasing the amount of memory too much may cause some processes to run out of memory.</span></span>  <span data-ttu-id="30f5d-138">Pokud dojde k chybě haldy při spuštění vaší úlohy, měli byste zvýšit paměť na mapper nebo reduktorem.</span><span class="sxs-lookup"><span data-stu-id="30f5d-138">If you get a heap error when running your job, you should increase the memory per mapper or reducer.</span></span>  <span data-ttu-id="30f5d-139">Měli byste zvážit přidání další kontejnerů přidá, velmi starat se pro každý další kontejneru, který může potenciálně snížit výkon.</span><span class="sxs-lookup"><span data-stu-id="30f5d-139">You should consider that adding more containers will add extra overhead for each additional container, which can potentially degrade performance.</span></span>  <span data-ttu-id="30f5d-140">Další alternativou je získat více paměti pomocí clusteru, který má vyšší objemy paměti nebo zvýšením počtu uzlů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="30f5d-140">Another alternative is to get more memory by using a cluster that has higher amounts of memory or increasing the number of nodes in your cluster.</span></span>  <span data-ttu-id="30f5d-141">Více paměti umožní víc kontejnerů, které mají být použity, což znamená, že další souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="30f5d-141">More memory will enable more containers to be used, which means more concurrency.</span></span>  

<span data-ttu-id="30f5d-142">**Krok 3: Určení celkový YARN paměti** – Pokud chcete ladit mapreduce.job.maps/mapreduce.job.reduces, měli byste zvážit celkové YARN paměti k dispozici pro použití.</span><span class="sxs-lookup"><span data-stu-id="30f5d-142">**Step 3: Determine Total YARN memory** - To tune mapreduce.job.maps/mapreduce.job.reduces, you should consider the amount of total YARN memory available for use.</span></span>  <span data-ttu-id="30f5d-143">Tyto informace jsou k dispozici v Ambari.</span><span class="sxs-lookup"><span data-stu-id="30f5d-143">This information is available in Ambari.</span></span>  <span data-ttu-id="30f5d-144">Přejděte do YARN a zobrazit kartu konfigurací.</span><span class="sxs-lookup"><span data-stu-id="30f5d-144">Navigate to YARN and view the Configs tab.</span></span>  <span data-ttu-id="30f5d-145">V tomto okně se zobrazí YARN paměti.</span><span class="sxs-lookup"><span data-stu-id="30f5d-145">The YARN memory is displayed in this window.</span></span>  <span data-ttu-id="30f5d-146">Měli vynásobte paměti YARN počtu uzlů v clusteru se získat celkovou velikost paměti YARN.</span><span class="sxs-lookup"><span data-stu-id="30f5d-146">You should multiply the YARN memory with the number of nodes in your cluster to get the total YARN memory.</span></span>

    Total YARN memory = nodes * YARN memory per node
<span data-ttu-id="30f5d-147">Pokud používáte prázdný clusteru, paměti, může být celkové paměti YARN pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="30f5d-147">If you are using an empty cluster, then memory can be the total YARN memory for your cluster.</span></span>  <span data-ttu-id="30f5d-148">Pokud jiné aplikace používají paměť, je možné použít pouze část paměti váš cluster snížením počtu mappers nebo přechodky počet kontejnerů, které chcete použít.</span><span class="sxs-lookup"><span data-stu-id="30f5d-148">If other applications are using memory, then you can choose to only use a portion of your cluster’s memory by reducing the number of mappers or reducers to the number of containers you want to use.</span></span>  

<span data-ttu-id="30f5d-149">**Krok 4: Vypočítat počet kontejnerů YARN** – YARN kontejnery stanovují množství souběžnost, které jsou k dispozici pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="30f5d-149">**Step 4: Calculate number of YARN containers** – YARN containers dictate the amount of concurrency available for the job.</span></span>  <span data-ttu-id="30f5d-150">Trvat celkové paměti YARN, vydělte mapreduce.map.memory.</span><span class="sxs-lookup"><span data-stu-id="30f5d-150">Take total YARN memory and divide that by mapreduce.map.memory.</span></span>  

    # of YARN containers = total YARN memory / mapreduce.map.memory

<span data-ttu-id="30f5d-151">**Krok 5: Nastavení mapreduce.job.maps/mapreduce.job.reduces** mapreduce.job.maps/mapreduce.job.reduces nastavit na alespoň počet dostupné kontejnery.</span><span class="sxs-lookup"><span data-stu-id="30f5d-151">**Step 5: Set mapreduce.job.maps/mapreduce.job.reduces** Set mapreduce.job.maps/mapreduce.job.reduces to at least the number of available containers.</span></span>  <span data-ttu-id="30f5d-152">Můžete experimentovat další zvýšením počtu mappers a přechodky zobrazit, pokud chcete získat lepší výkon.</span><span class="sxs-lookup"><span data-stu-id="30f5d-152">You can experiment further by increasing the number of mappers and reducers to see if you get better performance.</span></span>  <span data-ttu-id="30f5d-153">Uvědomte si, že další mappers budou mít dalších zásahů, má příliš mnoho mappers může snížit výkon.</span><span class="sxs-lookup"><span data-stu-id="30f5d-153">Keep in mind that more mappers will have additional overhead so having too many mappers may degrade performance.</span></span>  

<span data-ttu-id="30f5d-154">Plánování CPU a využití procesoru izolace jsou ve výchozím nastavení vypnuté, počet kontejnerů YARN je omezené paměti.</span><span class="sxs-lookup"><span data-stu-id="30f5d-154">CPU scheduling and CPU isolation are turned off by default so the number of YARN containers is constrained by memory.</span></span>

## <a name="example-calculation"></a><span data-ttu-id="30f5d-155">Příklad výpočtu</span><span class="sxs-lookup"><span data-stu-id="30f5d-155">Example Calculation</span></span>

<span data-ttu-id="30f5d-156">Řekněme, že aktuálně máte cluster skládá z 8 D14 uzly a chcete spustit úlohu intenzivním vstupně-výstupní operace.</span><span class="sxs-lookup"><span data-stu-id="30f5d-156">Let’s say you currently have a cluster composed of 8 D14 nodes and you want to run an I/O intensive job.</span></span>  <span data-ttu-id="30f5d-157">Zde jsou výpočty, které byste měli udělat:</span><span class="sxs-lookup"><span data-stu-id="30f5d-157">Here are the calculations you should do:</span></span>

<span data-ttu-id="30f5d-158">**Krok 1: Určení počet úloh spuštěných** -pro náš příklad předpokládáme, že naše úloha je spuštěna pouze jedna.</span><span class="sxs-lookup"><span data-stu-id="30f5d-158">**Step 1: Determine number of jobs running** - for our example, we assume that our job is the only one running.</span></span>  

<span data-ttu-id="30f5d-159">**Krok 2: Nastavení mapreduce.map.memory/mapreduce.reduce.memory** – pro náš příklad běží úlohy náročné vstupně-výstupních operací a rozhodnout, že 3 GB paměti pro mapování úlohy bude dostatečná.</span><span class="sxs-lookup"><span data-stu-id="30f5d-159">**Step 2: Set mapreduce.map.memory/mapreduce.reduce.memory** – for our example, you are running an I/O intensive job and decide that 3GB of memory for map tasks will be sufficient.</span></span>

    mapreduce.map.memory = 3GB
<span data-ttu-id="30f5d-160">**Krok 3: Určení celkový YARN paměti**</span><span class="sxs-lookup"><span data-stu-id="30f5d-160">**Step 3: Determine Total YARN memory**</span></span>

    total memory from the cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB
<span data-ttu-id="30f5d-161">**Krok 4: Vypočítat počet YARN kontejnery**</span><span class="sxs-lookup"><span data-stu-id="30f5d-161">**Step 4: Calculate # of YARN containers**</span></span>

    # of YARN containers = 768GB of available memory / 3 GB of memory =   256

<span data-ttu-id="30f5d-162">**Krok 5: Nastavení mapreduce.job.maps/mapreduce.job.reduces**</span><span class="sxs-lookup"><span data-stu-id="30f5d-162">**Step 5: Set mapreduce.job.maps/mapreduce.job.reduces**</span></span>

    mapreduce.map.jobs = 256

## <a name="limitations"></a><span data-ttu-id="30f5d-163">Omezení</span><span class="sxs-lookup"><span data-stu-id="30f5d-163">Limitations</span></span>

<span data-ttu-id="30f5d-164">**ADLS omezení**</span><span class="sxs-lookup"><span data-stu-id="30f5d-164">**ADLS throttling**</span></span>

<span data-ttu-id="30f5d-165">Jako víceklientské služby nastaví ADLS limity účtu úrovně šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="30f5d-165">As a multi-tenant service, ADLS sets account level bandwidth limits.</span></span>  <span data-ttu-id="30f5d-166">Jestli jste nedosáhli tyto limity, začněte zobrazíte selhání úkolů.</span><span class="sxs-lookup"><span data-stu-id="30f5d-166">If you hit these limits, you will start to see task failures.</span></span> <span data-ttu-id="30f5d-167">To lze identifikovat podle sledování omezení chyby v protokolech úloh.</span><span class="sxs-lookup"><span data-stu-id="30f5d-167">This can be identified by observing throttling errors in task logs.</span></span>  <span data-ttu-id="30f5d-168">Pokud potřebujete větší šířku pásma pro úlohu, kontaktujte nás.</span><span class="sxs-lookup"><span data-stu-id="30f5d-168">If you need more bandwidth for your job, please contact us.</span></span>   

<span data-ttu-id="30f5d-169">Pokud chcete zkontrolovat, pokud jste jsou získávání omezené, musíte povolit ladění na straně klienta protokolování.</span><span class="sxs-lookup"><span data-stu-id="30f5d-169">To check if you are getting throttled, you need to enable the debug logging on the client side.</span></span> <span data-ttu-id="30f5d-170">Zde je, jak můžete to udělat:</span><span class="sxs-lookup"><span data-stu-id="30f5d-170">Here’s how you can do that:</span></span>

1. <span data-ttu-id="30f5d-171">Uveďte následující vlastnost ve vlastnostech log4j v Ambari > YARN > Konfigurace > Upřesnit yarn log4j: log4j.logger.com.microsoft.azure.datalake.store=DEBUG</span><span class="sxs-lookup"><span data-stu-id="30f5d-171">Put the following property in the log4j properties in Ambari > YARN > Config > Advanced yarn-log4j: log4j.logger.com.microsoft.azure.datalake.store=DEBUG</span></span>

2. <span data-ttu-id="30f5d-172">Restartujte všechny uzly nebo služby, konfigurace se projeví.</span><span class="sxs-lookup"><span data-stu-id="30f5d-172">Restart all the nodes/service for the config to take effect.</span></span>

3. <span data-ttu-id="30f5d-173">Pokud jste jsou získávání omezené, se zobrazí kód HTTP 429 chyby v souboru protokolu YARN.</span><span class="sxs-lookup"><span data-stu-id="30f5d-173">If you are getting throttled, you’ll see the HTTP 429 error code in the YARN log file.</span></span> <span data-ttu-id="30f5d-174">Soubor protokolu YARN je v /tmp/&lt;uživatele&gt;/yarn.log</span><span class="sxs-lookup"><span data-stu-id="30f5d-174">The YARN log file is in /tmp/&lt;user&gt;/yarn.log</span></span>

## <a name="examples-to-run"></a><span data-ttu-id="30f5d-175">Příklady pro spuštění</span><span class="sxs-lookup"><span data-stu-id="30f5d-175">Examples to Run</span></span>

<span data-ttu-id="30f5d-176">K předvedení, jak MapReduce běží na Azure Data Lake Store, zde jsou některé ukázkový kód, která byla spuštěna na cluster s následujícím nastavením:</span><span class="sxs-lookup"><span data-stu-id="30f5d-176">To demonstrate how MapReduce runs on Azure Data Lake Store, below is some sample code that was run on a cluster with the following settings:</span></span>

* <span data-ttu-id="30f5d-177">16 uzlu D14v2</span><span class="sxs-lookup"><span data-stu-id="30f5d-177">16 node D14v2</span></span>
* <span data-ttu-id="30f5d-178">Spuštění HDI 3.6 clusteru Hadoop</span><span class="sxs-lookup"><span data-stu-id="30f5d-178">Hadoop cluster running HDI 3.6</span></span>

<span data-ttu-id="30f5d-179">Pro počáteční bod tady jsou příklady některých příkazů ke spuštění MapReduce Teragen, Terasort a Teravalidate.</span><span class="sxs-lookup"><span data-stu-id="30f5d-179">For a starting point, here are some example commands to run MapReduce Teragen, Terasort, and Teravalidate.</span></span>  <span data-ttu-id="30f5d-180">Můžete upravit tyto příkazy podle vašich prostředků.</span><span class="sxs-lookup"><span data-stu-id="30f5d-180">You can adjust these commands based on your resources.</span></span>

<span data-ttu-id="30f5d-181">**Teragen**</span><span class="sxs-lookup"><span data-stu-id="30f5d-181">**Teragen**</span></span>

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 10000000000 adl://example/data/1TB-sort-input

<span data-ttu-id="30f5d-182">**Terasort**</span><span class="sxs-lookup"><span data-stu-id="30f5d-182">**Terasort**</span></span>

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 -Dmapreduce.job.reduces=512 -Dmapreduce.reduce.memory.mb=3072 adl://example/data/1TB-sort-input adl://example/data/1TB-sort-output

<span data-ttu-id="30f5d-183">**Teravalidate**</span><span class="sxs-lookup"><span data-stu-id="30f5d-183">**Teravalidate**</span></span>

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapreduce.job.maps=512 -Dmapreduce.map.memory.mb=3072 adl://example/data/1TB-sort-output adl://example/data/1TB-sort-validate
