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
# <a name="performance-tuning-guidance-for-mapreduce-on-hdinsight-and-azure-data-lake-store"></a><span data-ttu-id="cd95d-103">Pokyny pro MapReduce na HDInsight a Azure Data Lake Store optimalizace výkonu</span><span class="sxs-lookup"><span data-stu-id="cd95d-103">Performance tuning guidance for MapReduce on HDInsight and Azure Data Lake Store</span></span>


## <a name="prerequisites"></a><span data-ttu-id="cd95d-104">Požadavky</span><span class="sxs-lookup"><span data-stu-id="cd95d-104">Prerequisites</span></span>

* <span data-ttu-id="cd95d-105">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="cd95d-105">**An Azure subscription**.</span></span> <span data-ttu-id="cd95d-106">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cd95d-106">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="cd95d-107">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="cd95d-107">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="cd95d-108">Návod, jak jeden, viz toocreate [Začínáme s Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="cd95d-108">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="cd95d-109">**Azure HDInsight cluster** s tooa přístup k účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="cd95d-109">**Azure HDInsight cluster** with access tooa Data Lake Store account.</span></span> <span data-ttu-id="cd95d-110">V tématu [vytvoření clusteru HDInsight s Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="cd95d-110">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="cd95d-111">Ujistěte se, že povolení vzdálené plochy pro hello clusteru.</span><span class="sxs-lookup"><span data-stu-id="cd95d-111">Make sure you enable Remote Desktop for hello cluster.</span></span>
* <span data-ttu-id="cd95d-112">**Použití prostředí MapReduce v HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="cd95d-112">**Using MapReduce on HDInsight**.</span></span>  <span data-ttu-id="cd95d-113">Další informace najdete v tématu [použití MapReduce v Hadoop v HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-mapreduce)</span><span class="sxs-lookup"><span data-stu-id="cd95d-113">For more information, see [Use MapReduce in Hadoop on HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-mapreduce)</span></span>  
* <span data-ttu-id="cd95d-114">**Ladění pokyny na ADLS výkonu**.</span><span class="sxs-lookup"><span data-stu-id="cd95d-114">**Performance tuning guidelines on ADLS**.</span></span>  <span data-ttu-id="cd95d-115">Obecný výkon koncepty, najdete v části [Data Lake Store výkonu ladění pokyny](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span><span class="sxs-lookup"><span data-stu-id="cd95d-115">For general performance concepts, see [Data Lake Store Performance Tuning Guidance](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span></span>  

## <a name="parameters"></a><span data-ttu-id="cd95d-116">Parametry</span><span class="sxs-lookup"><span data-stu-id="cd95d-116">Parameters</span></span>

<span data-ttu-id="cd95d-117">Při spuštění úloh MapReduce, zde je nejdůležitější parametry hello tooincrease výkonu můžete konfigurovat v ADLS:</span><span class="sxs-lookup"><span data-stu-id="cd95d-117">When running MapReduce jobs, here are hello most important parameters that you can configure tooincrease performance on ADLS:</span></span>

* <span data-ttu-id="cd95d-118">**Mapreduce.map.Memory.MB** – hello množství paměti tooallocate tooeach mapper</span><span class="sxs-lookup"><span data-stu-id="cd95d-118">**Mapreduce.map.memory.mb** – hello amount of memory tooallocate tooeach mapper</span></span>
* <span data-ttu-id="cd95d-119">**Mapreduce.job.Maps** – hello počet úloh mapy na úlohu</span><span class="sxs-lookup"><span data-stu-id="cd95d-119">**Mapreduce.job.maps** – hello number of map tasks per job</span></span>
* <span data-ttu-id="cd95d-120">**Mapreduce.reduce.Memory.MB** – hello množství paměti tooallocate tooeach reduktorem</span><span class="sxs-lookup"><span data-stu-id="cd95d-120">**Mapreduce.reduce.memory.mb** – hello amount of memory tooallocate tooeach reducer</span></span>
* <span data-ttu-id="cd95d-121">**Mapreduce.job.reduces** – hello počet úloh snižte na úlohu</span><span class="sxs-lookup"><span data-stu-id="cd95d-121">**Mapreduce.job.reduces** – hello number of reduce tasks per job</span></span>

<span data-ttu-id="cd95d-122">**Mapreduce.map.Memory / Mapreduce.reduce.memory** toto číslo je třeba upravit podle množství paměti, je potřeba pro mapu hello nebo snížit úloh.</span><span class="sxs-lookup"><span data-stu-id="cd95d-122">**Mapreduce.map.memory / Mapreduce.reduce.memory** This number should be adjusted based on how much memory is needed for hello map and/or reduce task.</span></span>  <span data-ttu-id="cd95d-123">výchozí hodnoty Hello mapreduce.map.memory a mapreduce.reduce.memory lze zobrazit v Ambari prostřednictvím konfigurace Yarn hello.</span><span class="sxs-lookup"><span data-stu-id="cd95d-123">hello default values of mapreduce.map.memory and mapreduce.reduce.memory can be viewed in Ambari via hello Yarn configuration.</span></span>  <span data-ttu-id="cd95d-124">V Ambari přejděte tooYARN a zobrazit kartu konfigurací hello.  Zobrazí se Hello YARN paměti.</span><span class="sxs-lookup"><span data-stu-id="cd95d-124">In Ambari, navigate tooYARN and view hello Configs tab.  hello YARN memory will be displayed.</span></span>  

<span data-ttu-id="cd95d-125">**Mapreduce.job.Maps / Mapreduce.job.reduces** určí hello maximální počet mappers nebo přechodky toobe vytvořili.</span><span class="sxs-lookup"><span data-stu-id="cd95d-125">**Mapreduce.job.maps / Mapreduce.job.reduces** This will determine hello maximum number of mappers or reducers toobe created.</span></span>  <span data-ttu-id="cd95d-126">Hello počet rozdělení určí, kolik mappers budou vytvořeny pro úlohu MapReduce hello.</span><span class="sxs-lookup"><span data-stu-id="cd95d-126">hello number of splits will determine how many mappers will be created for hello MapReduce job.</span></span>  <span data-ttu-id="cd95d-127">Proto může získat méně mappers, než jste požadovali, pokud jsou menší rozdělení než hello počet mappers požadovaný.</span><span class="sxs-lookup"><span data-stu-id="cd95d-127">Therefore, you may get less mappers than you requested if there are less splits than hello number of mappers requested.</span></span>       

## <a name="guidance"></a><span data-ttu-id="cd95d-128">Doprovodné materiály</span><span class="sxs-lookup"><span data-stu-id="cd95d-128">Guidance</span></span>

<span data-ttu-id="cd95d-129">**Krok 1: Určení počet úloh spuštěných** – ve výchozím nastavení, použije MapReduce hello celý cluster pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="cd95d-129">**Step 1: Determine number of jobs running** - By default, MapReduce will use hello entire cluster for your job.</span></span>  <span data-ttu-id="cd95d-130">Můžete použít menší hello clusteru pomocí méně mappers, než je k dispozici kontejnery.</span><span class="sxs-lookup"><span data-stu-id="cd95d-130">You can use less of hello cluster by using less mappers than there are available containers.</span></span>  <span data-ttu-id="cd95d-131">Hello pokyny v tomto dokumentu se předpokládá, že aplikace je hello pouze aplikace běžící v clusteru.</span><span class="sxs-lookup"><span data-stu-id="cd95d-131">hello guidance in this document assumes that your application is hello only application running on your cluster.</span></span>      

<span data-ttu-id="cd95d-132">**Krok 2: Nastavení mapreduce.map.memory/mapreduce.reduce.memory** – hello velikosti hello paměti pro mapu a snížit úlohy budou závislé na určité úlohy.</span><span class="sxs-lookup"><span data-stu-id="cd95d-132">**Step 2: Set mapreduce.map.memory/mapreduce.reduce.memory** –  hello size of hello memory for map and reduce tasks will be dependent on your specific job.</span></span>  <span data-ttu-id="cd95d-133">Pokud chcete, aby tooincrease souběžnosti, můžete snížit velikost paměti hello.</span><span class="sxs-lookup"><span data-stu-id="cd95d-133">You can reduce hello memory size if you want tooincrease concurrency.</span></span>  <span data-ttu-id="cd95d-134">Hello počet souběžně spuštěných úloh závisí na hello počet kontejnerů.</span><span class="sxs-lookup"><span data-stu-id="cd95d-134">hello number of concurrently running tasks depends on hello number of containers.</span></span>  <span data-ttu-id="cd95d-135">Snížením hello množství paměti na jeden mapper nebo reduktorem víc kontejnerů mohou být vytvořeny, které další mappers nebo přechodky toorun povolit souběžně.</span><span class="sxs-lookup"><span data-stu-id="cd95d-135">By decreasing hello amount of memory per mapper or reducer, more containers can be created, which enable more mappers or reducers toorun concurrently.</span></span>  <span data-ttu-id="cd95d-136">Příliš mnoho klesá hello množství paměti, může způsobit některé procesy toorun nedostatek paměti.</span><span class="sxs-lookup"><span data-stu-id="cd95d-136">Decreasing hello amount of memory too much may cause some processes toorun out of memory.</span></span>  <span data-ttu-id="cd95d-137">Pokud dojde k chybě haldy při spuštění vaší úlohy, měli byste zvýšit hello paměť na mapper nebo reduktorem.</span><span class="sxs-lookup"><span data-stu-id="cd95d-137">If you get a heap error when running your job, you should increase hello memory per mapper or reducer.</span></span>  <span data-ttu-id="cd95d-138">Měli byste zvážit přidání další kontejnerů přidá, velmi starat se pro každý další kontejneru, který může potenciálně snížit výkon.</span><span class="sxs-lookup"><span data-stu-id="cd95d-138">You should consider that adding more containers will add extra overhead for each additional container, which can potentially degrade performance.</span></span>  <span data-ttu-id="cd95d-139">Další možností je tooget více paměti pomocí clusteru, který má vyšší objemy paměti nebo zvýšení hello počet uzlů v clusteru.</span><span class="sxs-lookup"><span data-stu-id="cd95d-139">Another alternative is tooget more memory by using a cluster that has higher amounts of memory or increasing hello number of nodes in your cluster.</span></span>  <span data-ttu-id="cd95d-140">Více paměti umožní více kontejnerů toobe používá, což znamená další souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="cd95d-140">More memory will enable more containers toobe used, which means more concurrency.</span></span>  

<span data-ttu-id="cd95d-141">**Krok 3: Určení celkový YARN paměti** -tootune mapreduce.job.maps/mapreduce.job.reduces, měli byste zvážit hello množství celkový YARN paměti k dispozici pro použití.</span><span class="sxs-lookup"><span data-stu-id="cd95d-141">**Step 3: Determine Total YARN memory** - tootune mapreduce.job.maps/mapreduce.job.reduces, you should consider hello amount of total YARN memory available for use.</span></span>  <span data-ttu-id="cd95d-142">Tyto informace jsou k dispozici v Ambari.</span><span class="sxs-lookup"><span data-stu-id="cd95d-142">This information is available in Ambari.</span></span>  <span data-ttu-id="cd95d-143">Přejděte tooYARN a zobrazit kartu konfigurací hello.  v tomto okně se zobrazí Hello YARN paměti.</span><span class="sxs-lookup"><span data-stu-id="cd95d-143">Navigate tooYARN and view hello Configs tab.  hello YARN memory is displayed in this window.</span></span>  <span data-ttu-id="cd95d-144">Měli vynásobte hello YARN paměti hello počtu uzlů v clusteru tooget hello celkový YARN paměti.</span><span class="sxs-lookup"><span data-stu-id="cd95d-144">You should multiply hello YARN memory with hello number of nodes in your cluster tooget hello total YARN memory.</span></span>

    Total YARN memory = nodes * YARN memory per node
<span data-ttu-id="cd95d-145">Pokud používáte prázdný clusteru, může být paměti hello celkové paměti YARN pro váš cluster.</span><span class="sxs-lookup"><span data-stu-id="cd95d-145">If you are using an empty cluster, then memory can be hello total YARN memory for your cluster.</span></span>  <span data-ttu-id="cd95d-146">Pokud používáte jiné aplikace paměti, pak můžete použít tooonly část paměti váš cluster snížením hello počet mappers nebo přechodky toohello počet kontejnerů chcete toouse.</span><span class="sxs-lookup"><span data-stu-id="cd95d-146">If other applications are using memory, then you can choose tooonly use a portion of your cluster’s memory by reducing hello number of mappers or reducers toohello number of containers you want toouse.</span></span>  

<span data-ttu-id="cd95d-147">**Krok 4: Vypočítat počet kontejnerů YARN** – YARN kontejnery stanovují množství hello k dispozici pro úlohy hello souběžnosti.</span><span class="sxs-lookup"><span data-stu-id="cd95d-147">**Step 4: Calculate number of YARN containers** – YARN containers dictate hello amount of concurrency available for hello job.</span></span>  <span data-ttu-id="cd95d-148">Trvat celkové paměti YARN, vydělte mapreduce.map.memory.</span><span class="sxs-lookup"><span data-stu-id="cd95d-148">Take total YARN memory and divide that by mapreduce.map.memory.</span></span>  

    # of YARN containers = total YARN memory / mapreduce.map.memory

<span data-ttu-id="cd95d-149">**Krok 5: Nastavení mapreduce.job.maps/mapreduce.job.reduces** nastavit mapreduce.job.maps/mapreduce.job.reduces tooat minimálně hello počet kontejnerů k dispozici.</span><span class="sxs-lookup"><span data-stu-id="cd95d-149">**Step 5: Set mapreduce.job.maps/mapreduce.job.reduces** Set mapreduce.job.maps/mapreduce.job.reduces tooat least hello number of available containers.</span></span>  <span data-ttu-id="cd95d-150">Můžete experimentovat další zvýšením hello počet mappers a přechodky toosee, je-li získat lepší výkon.</span><span class="sxs-lookup"><span data-stu-id="cd95d-150">You can experiment further by increasing hello number of mappers and reducers toosee if you get better performance.</span></span>  <span data-ttu-id="cd95d-151">Uvědomte si, že další mappers budou mít dalších zásahů, má příliš mnoho mappers může snížit výkon.</span><span class="sxs-lookup"><span data-stu-id="cd95d-151">Keep in mind that more mappers will have additional overhead so having too many mappers may degrade performance.</span></span>  

<span data-ttu-id="cd95d-152">Plánování CPU a využití procesoru izolace jsou ve výchozím nastavení vypnuté, hello počet kontejnerů YARN je omezené paměti.</span><span class="sxs-lookup"><span data-stu-id="cd95d-152">CPU scheduling and CPU isolation are turned off by default so hello number of YARN containers is constrained by memory.</span></span>

## <a name="example-calculation"></a><span data-ttu-id="cd95d-153">Příklad výpočtu</span><span class="sxs-lookup"><span data-stu-id="cd95d-153">Example Calculation</span></span>

<span data-ttu-id="cd95d-154">Řekněme, že teď máte cluster skládá z 8 D14 uzly a chcete toorun úlohy náročné vstupně-výstupních operací.</span><span class="sxs-lookup"><span data-stu-id="cd95d-154">Let’s say you currently have a cluster composed of 8 D14 nodes and you want toorun an I/O intensive job.</span></span>  <span data-ttu-id="cd95d-155">Zde jsou hello výpočty, které byste měli udělat:</span><span class="sxs-lookup"><span data-stu-id="cd95d-155">Here are hello calculations you should do:</span></span>

<span data-ttu-id="cd95d-156">**Krok 1: Určení počet úloh spuštěných** -pro náš příklad předpokládáme, že je naše úlohy hello pouze systémem.</span><span class="sxs-lookup"><span data-stu-id="cd95d-156">**Step 1: Determine number of jobs running** - for our example, we assume that our job is hello only one running.</span></span>  

<span data-ttu-id="cd95d-157">**Krok 2: Nastavení mapreduce.map.memory/mapreduce.reduce.memory** – pro náš příklad běží úlohy náročné vstupně-výstupních operací a rozhodnout, že 3 GB paměti pro mapování úlohy bude dostatečná.</span><span class="sxs-lookup"><span data-stu-id="cd95d-157">**Step 2: Set mapreduce.map.memory/mapreduce.reduce.memory** – for our example, you are running an I/O intensive job and decide that 3GB of memory for map tasks will be sufficient.</span></span>

    mapreduce.map.memory = 3GB
<span data-ttu-id="cd95d-158">**Krok 3: Určení celkový YARN paměti**</span><span class="sxs-lookup"><span data-stu-id="cd95d-158">**Step 3: Determine Total YARN memory**</span></span>

    total memory from hello cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB
<span data-ttu-id="cd95d-159">**Krok 4: Vypočítat počet YARN kontejnery**</span><span class="sxs-lookup"><span data-stu-id="cd95d-159">**Step 4: Calculate # of YARN containers**</span></span>

    # of YARN containers = 768GB of available memory / 3 GB of memory =   256

<span data-ttu-id="cd95d-160">**Krok 5: Nastavení mapreduce.job.maps/mapreduce.job.reduces**</span><span class="sxs-lookup"><span data-stu-id="cd95d-160">**Step 5: Set mapreduce.job.maps/mapreduce.job.reduces**</span></span>

    mapreduce.map.jobs = 256

## <a name="limitations"></a><span data-ttu-id="cd95d-161">Omezení</span><span class="sxs-lookup"><span data-stu-id="cd95d-161">Limitations</span></span>

<span data-ttu-id="cd95d-162">**ADLS omezení**</span><span class="sxs-lookup"><span data-stu-id="cd95d-162">**ADLS throttling**</span></span>

<span data-ttu-id="cd95d-163">Jako víceklientské služby nastaví ADLS limity účtu úrovně šířky pásma.</span><span class="sxs-lookup"><span data-stu-id="cd95d-163">As a multi-tenant service, ADLS sets account level bandwidth limits.</span></span>  <span data-ttu-id="cd95d-164">Jestli jste nedosáhli tyto limity, začněte toosee selhání úkolů.</span><span class="sxs-lookup"><span data-stu-id="cd95d-164">If you hit these limits, you will start toosee task failures.</span></span> <span data-ttu-id="cd95d-165">To lze identifikovat podle sledování omezení chyby v protokolech úloh.</span><span class="sxs-lookup"><span data-stu-id="cd95d-165">This can be identified by observing throttling errors in task logs.</span></span>  <span data-ttu-id="cd95d-166">Pokud potřebujete větší šířku pásma pro úlohu, kontaktujte nás.</span><span class="sxs-lookup"><span data-stu-id="cd95d-166">If you need more bandwidth for your job, please contact us.</span></span>   

<span data-ttu-id="cd95d-167">toocheck, pokud jste jsou získávání omezené, musíte tooenable hello ladění na straně klienta hello protokolování.</span><span class="sxs-lookup"><span data-stu-id="cd95d-167">toocheck if you are getting throttled, you need tooenable hello debug logging on hello client side.</span></span> <span data-ttu-id="cd95d-168">Zde je, jak můžete to udělat:</span><span class="sxs-lookup"><span data-stu-id="cd95d-168">Here’s how you can do that:</span></span>

1. <span data-ttu-id="cd95d-169">Vložení hello následující vlastnosti v hello log4j vlastnosti v Ambari > YARN > Konfigurace > Upřesnit yarn log4j: log4j.logger.com.microsoft.azure.datalake.store=DEBUG</span><span class="sxs-lookup"><span data-stu-id="cd95d-169">Put hello following property in hello log4j properties in Ambari > YARN > Config > Advanced yarn-log4j: log4j.logger.com.microsoft.azure.datalake.store=DEBUG</span></span>

2. <span data-ttu-id="cd95d-170">Restartujte všechny hello uzly nebo službu pro hello konfigurace tootake vliv.</span><span class="sxs-lookup"><span data-stu-id="cd95d-170">Restart all hello nodes/service for hello config tootake effect.</span></span>

3. <span data-ttu-id="cd95d-171">Pokud jste jsou získávání omezené, se zobrazí kód chyby HTTP 429 hello v souboru protokolu YARN hello.</span><span class="sxs-lookup"><span data-stu-id="cd95d-171">If you are getting throttled, you’ll see hello HTTP 429 error code in hello YARN log file.</span></span> <span data-ttu-id="cd95d-172">soubor protokolu YARN Hello je v /tmp/&lt;uživatele&gt;/yarn.log</span><span class="sxs-lookup"><span data-stu-id="cd95d-172">hello YARN log file is in /tmp/&lt;user&gt;/yarn.log</span></span>

## <a name="examples-toorun"></a><span data-ttu-id="cd95d-173">Příklady tooRun</span><span class="sxs-lookup"><span data-stu-id="cd95d-173">Examples tooRun</span></span>

<span data-ttu-id="cd95d-174">toodemonstrate jak MapReduce běží na Azure Data Lake Store, zde jsou některé ukázkový kód, která byla spuštěna na cluster s hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="cd95d-174">toodemonstrate how MapReduce runs on Azure Data Lake Store, below is some sample code that was run on a cluster with hello following settings:</span></span>

* <span data-ttu-id="cd95d-175">16 uzlu D14v2</span><span class="sxs-lookup"><span data-stu-id="cd95d-175">16 node D14v2</span></span>
* <span data-ttu-id="cd95d-176">Spuštění HDI 3.6 clusteru Hadoop</span><span class="sxs-lookup"><span data-stu-id="cd95d-176">Hadoop cluster running HDI 3.6</span></span>

<span data-ttu-id="cd95d-177">Pro počáteční bod tady jsou některé příklad příkazy toorun MapReduce Teragen, Terasort a Teravalidate.</span><span class="sxs-lookup"><span data-stu-id="cd95d-177">For a starting point, here are some example commands toorun MapReduce Teragen, Terasort, and Teravalidate.</span></span>  <span data-ttu-id="cd95d-178">Můžete upravit tyto příkazy podle vašich prostředků.</span><span class="sxs-lookup"><span data-stu-id="cd95d-178">You can adjust these commands based on your resources.</span></span>

<span data-ttu-id="cd95d-179">**Teragen**</span><span class="sxs-lookup"><span data-stu-id="cd95d-179">**Teragen**</span></span>

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 10000000000 adl://example/data/1TB-sort-input

<span data-ttu-id="cd95d-180">**Terasort**</span><span class="sxs-lookup"><span data-stu-id="cd95d-180">**Terasort**</span></span>

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 -Dmapreduce.job.reduces=512 -Dmapreduce.reduce.memory.mb=3072 adl://example/data/1TB-sort-input adl://example/data/1TB-sort-output

<span data-ttu-id="cd95d-181">**Teravalidate**</span><span class="sxs-lookup"><span data-stu-id="cd95d-181">**Teravalidate**</span></span>

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapreduce.job.maps=512 -Dmapreduce.map.memory.mb=3072 adl://example/data/1TB-sort-output adl://example/data/1TB-sort-validate
