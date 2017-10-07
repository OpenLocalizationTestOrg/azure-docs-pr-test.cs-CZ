---
title: "aaaStream data ze služby Stream Analytics do Data Lake Store | Microsoft Docs"
description: "Použití Azure Stream Analytics toostream data do Azure Data Lake Store"
services: data-lake-store,stream-analytics
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: edb58e0b-311f-44b0-a499-04d7e6c07a90
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 68c727d4807db0abe6efa90145d68c78902eb789
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a><span data-ttu-id="c7db8-103">Streamování dat z Azure Storage Blob do služby Data Lake Store pomocí Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="c7db8-103">Stream data from Azure Storage Blob into Data Lake Store using Azure Stream Analytics</span></span>
<span data-ttu-id="c7db8-104">V tomto článku se dozvíte, jak toouse Azure Data Lake uložit jako výstup pro úlohu služby Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="c7db8-104">In this article you will learn how toouse Azure Data Lake Store as an output for an Azure Stream Analytics job.</span></span> <span data-ttu-id="c7db8-105">Tento článek ukazuje jednoduchého scénáře, který čte data z objektu blob Azure Storage (vstup) a zápisy hello data tooData Lake Store (výstup).</span><span class="sxs-lookup"><span data-stu-id="c7db8-105">This article demonstrates a simple scenario that reads data from an Azure Storage blob (input) and writes hello data tooData Lake Store (output).</span></span>

> [!NOTE]
> <span data-ttu-id="c7db8-106">V tomto okamžiku vytvoření a konfigurace Data Lake Store výstupy Stream Analytics je podporováno pouze v hello [portálu Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="c7db8-106">At this time, creation and configuration of Data Lake Store outputs for Stream Analytics is supported only in hello [Azure Classic Portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="c7db8-107">Proto některé části tohoto kurzu použije hello portálu Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="c7db8-107">Hence, some parts of this tutorial will use hello Azure Classic Portal.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="c7db8-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="c7db8-108">Prerequisites</span></span>
<span data-ttu-id="c7db8-109">Než začnete tento kurz, musíte mít následující hello:</span><span class="sxs-lookup"><span data-stu-id="c7db8-109">Before you begin this tutorial, you must have hello following:</span></span>

* <span data-ttu-id="c7db8-110">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-110">**An Azure subscription**.</span></span> <span data-ttu-id="c7db8-111">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c7db8-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="c7db8-112">**Účet služby Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-112">**Azure Storage account**.</span></span> <span data-ttu-id="c7db8-113">Kontejner objektů blob z těchto dat tooinput účet budete používat pro úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="c7db8-113">You will use a blob container from this account tooinput data for a Stream Analytics job.</span></span> <span data-ttu-id="c7db8-114">Pro tento kurz předpokládá, že máte účet úložiště s názvem **storageforasa** a nazývá kontejner v rámci účtu hello **storageforasacontainer**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-114">For this tutorial, assume you have a storage account called **storageforasa** and a container within hello account called **storageforasacontainer**.</span></span> <span data-ttu-id="c7db8-115">Po vytvoření kontejneru hello nahrajte ukázková data souboru tooit.</span><span class="sxs-lookup"><span data-stu-id="c7db8-115">Once you have created hello container, upload a sample data file tooit.</span></span> 
  
* <span data-ttu-id="c7db8-116">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-116">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="c7db8-117">Postupujte podle pokynů hello [Začínáme s Azure Data Lake Store pomocí portálu Azure hello](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c7db8-117">Follow hello instructions at [Get started with Azure Data Lake Store using hello Azure Portal](data-lake-store-get-started-portal.md).</span></span> <span data-ttu-id="c7db8-118">Předpokládejme, máte účet Data Lake Store s názvem **asadatalakestore**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-118">Let's assume you have a Data Lake Store account called **asadatalakestore**.</span></span> 

## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="c7db8-119">Vytvoření úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="c7db8-119">Create a Stream Analytics Job</span></span>
<span data-ttu-id="c7db8-120">Začněte vytvořením úlohy služby Stream Analytics, který obsahuje vstupní zdroj a cíl výstupu.</span><span class="sxs-lookup"><span data-stu-id="c7db8-120">You start by creating a Stream Analytics job that includes an input source and an output destination.</span></span> <span data-ttu-id="c7db8-121">V tomto kurzu hello zdroj je kontejner objektů blob v Azure a cíl hello je Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c7db8-121">For this tutorial, hello source is an Azure blob container and hello destination is Data Lake Store.</span></span>

1. <span data-ttu-id="c7db8-122">Přihlaste se toohello [portálu Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c7db8-122">Sign on toohello [Azure Portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="c7db8-123">V levém podokně hello, klikněte na **úlohy Stream Analytics**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-123">From hello left pane, click **Stream Analytics jobs**, and then click **Add**.</span></span>

    <span data-ttu-id="c7db8-124">![Vytvoření úlohy Stream Analytics](./media/data-lake-store-stream-analytics/create.job.png "vytvořit úlohu služby Stream Analytics")</span><span class="sxs-lookup"><span data-stu-id="c7db8-124">![Create a Stream Analytics Job](./media/data-lake-store-stream-analytics/create.job.png "Create a Stream Analytics job")</span></span>

    > [!NOTE]
    > <span data-ttu-id="c7db8-125">Ujistěte se, vytvoření úlohy v hello stejné oblasti jako účet úložiště hello nebo je pro vás znamená další náklady na přesun dat mezi oblastmi.</span><span class="sxs-lookup"><span data-stu-id="c7db8-125">Make sure you create job in hello same region as hello storage account or you will incur additional cost of moving data between regions.</span></span>
    >

## <a name="create-a-blob-input-for-hello-job"></a><span data-ttu-id="c7db8-126">Vytvoření vstupní objekt Blob pro úlohu hello</span><span class="sxs-lookup"><span data-stu-id="c7db8-126">Create a Blob input for hello job</span></span>

1. <span data-ttu-id="c7db8-127">Otevřete hello stránka pro úlohy Stream Analytics hello hello levém podokně klikněte na tlačítko hello **vstupy** a pak klikněte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-127">Open hello page for hello Stream Analytics job, from hello left pane click hello **Inputs** tab, and then click **Add**.</span></span>

    <span data-ttu-id="c7db8-128">![Přidat úlohu vstupní tooyour](./media/data-lake-store-stream-analytics/create.input.1.png "přidat úloha vstupní tooyour")</span><span class="sxs-lookup"><span data-stu-id="c7db8-128">![Add an input tooyour job](./media/data-lake-store-stream-analytics/create.input.1.png "Add an input tooyour job")</span></span>

2. <span data-ttu-id="c7db8-129">Na hello **nové vstup** okno, zadejte následující hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="c7db8-129">On hello **New input** blade, provide hello following values.</span></span>

    <span data-ttu-id="c7db8-130">![Přidat úlohu vstupní tooyour](./media/data-lake-store-stream-analytics/create.input.2.png "přidat úloha vstupní tooyour")</span><span class="sxs-lookup"><span data-stu-id="c7db8-130">![Add an input tooyour job](./media/data-lake-store-stream-analytics/create.input.2.png "Add an input tooyour job")</span></span>

    * <span data-ttu-id="c7db8-131">Pro **vstupní alias**, zadejte jedinečný název pro vstupní úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="c7db8-131">For **Input alias**, enter a unique name for hello job input.</span></span>
    * <span data-ttu-id="c7db8-132">Pro **typ zdroje**, vyberte **datový proud**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-132">For **Source type**, select **Data stream**.</span></span>
    * <span data-ttu-id="c7db8-133">Pro **zdroj**, vyberte **úložiště objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-133">For **Source**, select **Blob storage**.</span></span>
    * <span data-ttu-id="c7db8-134">Pro **předplatné**, vyberte **pomocí úložiště objektů blob z aktuálního předplatného**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-134">For **Subscription**, select **Use blob storage from current subscription**.</span></span>
    * <span data-ttu-id="c7db8-135">Pro **účet úložiště**, vyberte účet úložiště hello, kterou jste vytvořili v rámci požadavků hello.</span><span class="sxs-lookup"><span data-stu-id="c7db8-135">For **Storage account**, select hello storage account that you created as part of hello prerequisites.</span></span> 
    * <span data-ttu-id="c7db8-136">Pro **kontejneru**vyberte hello kontejneru, který jste vytvořili v hello vybraný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="c7db8-136">For **Container**, select hello container that you created in hello selected storage account.</span></span>
    * <span data-ttu-id="c7db8-137">Pro **formát serializace událostí**, vyberte **CSV**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-137">For **Event serialization format**, select **CSV**.</span></span>
    * <span data-ttu-id="c7db8-138">Pro **oddělovač**, vyberte **kartě**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-138">For **Delimiter**, select **tab**.</span></span>
    * <span data-ttu-id="c7db8-139">Pro **kódování**, vyberte **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-139">For **Encoding**, select **UTF-8**.</span></span>

    <span data-ttu-id="c7db8-140">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-140">Click **Create**.</span></span> <span data-ttu-id="c7db8-141">portál Hello teď přidá hello vstup a testy tooit hello připojení.</span><span class="sxs-lookup"><span data-stu-id="c7db8-141">hello portal now adds hello input and tests hello connection tooit.</span></span>


## <a name="create-a-data-lake-store-output-for-hello-job"></a><span data-ttu-id="c7db8-142">Vytvoření výstupní Data Lake Store pro úlohu hello</span><span class="sxs-lookup"><span data-stu-id="c7db8-142">Create a Data Lake Store output for hello job</span></span>

1. <span data-ttu-id="c7db8-143">Otevřete stránku hello hello úlohy Stream Analytics, klikněte na tlačítko hello **výstupy** a pak klikněte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-143">Open hello page for hello Stream Analytics job, click hello **Outputs** tab, and then click **Add**.</span></span>

    <span data-ttu-id="c7db8-144">![Přidat úlohu tooyour výstup](./media/data-lake-store-stream-analytics/create.output.1.png "přidat úloha tooyour výstup")</span><span class="sxs-lookup"><span data-stu-id="c7db8-144">![Add an output tooyour job](./media/data-lake-store-stream-analytics/create.output.1.png "Add an output tooyour job")</span></span>

2. <span data-ttu-id="c7db8-145">Na hello **nový výstupní** okno, zadejte následující hodnoty hello.</span><span class="sxs-lookup"><span data-stu-id="c7db8-145">On hello **New output** blade, provide hello following values.</span></span>

    <span data-ttu-id="c7db8-146">![Přidat úlohu tooyour výstup](./media/data-lake-store-stream-analytics/create.output.2.png "přidat úloha tooyour výstup")</span><span class="sxs-lookup"><span data-stu-id="c7db8-146">![Add an output tooyour job](./media/data-lake-store-stream-analytics/create.output.2.png "Add an output tooyour job")</span></span>

    * <span data-ttu-id="c7db8-147">Pro **alias pro výstup**, zadejte jedinečný název pro výstup úlohy hello.</span><span class="sxs-lookup"><span data-stu-id="c7db8-147">For **Output alias**, enter a a unique name for hello job output.</span></span> <span data-ttu-id="c7db8-148">Toto je popisný název používaný v dotazy toodirect hello dotazu výstup toothis Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c7db8-148">This is a friendly name used in queries toodirect hello query output toothis Data Lake Store.</span></span>
    * <span data-ttu-id="c7db8-149">Pro **jímky**, vyberte **Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-149">For **Sink**, select **Data Lake Store**.</span></span>
    * <span data-ttu-id="c7db8-150">Bude výzvami tooauthorize přístup k účtu tooData Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c7db8-150">You will be prompted tooauthorize access tooData Lake Store account.</span></span> <span data-ttu-id="c7db8-151">Klikněte na tlačítko **Autorizovat**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-151">Click **Authorize**.</span></span>

3. <span data-ttu-id="c7db8-152">Na hello **nový výstupní** okně pokračovat tooprovide hello následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="c7db8-152">On hello **New output** blade, continue tooprovide hello following values.</span></span>

    <span data-ttu-id="c7db8-153">![Přidat úlohu tooyour výstup](./media/data-lake-store-stream-analytics/create.output.3.png "přidat úloha tooyour výstup")</span><span class="sxs-lookup"><span data-stu-id="c7db8-153">![Add an output tooyour job](./media/data-lake-store-stream-analytics/create.output.3.png "Add an output tooyour job")</span></span>

    * <span data-ttu-id="c7db8-154">Pro **název účtu**, vyberte účet Data Lake Store hello jste již vytvořili, kam chcete odeslat toobe pro výstup hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="c7db8-154">For **Account name**, select hello Data Lake Store account you already created where you want hello job output toobe sent to.</span></span>
    * <span data-ttu-id="c7db8-155">Pro **vzorek cesty předponu**, zadejte toowrite cesty použité souboru zadány vaše soubory v rámci hello účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="c7db8-155">For **Path prefix pattern**, enter a file path used toowrite your files within hello specified Data Lake Store account.</span></span>
    * <span data-ttu-id="c7db8-156">Pro **formát data**, pokud jste použili token kalendářního data v cestě hello předponu, můžete vybrat formát data hello, ve kterém jsou uspořádány soubory.</span><span class="sxs-lookup"><span data-stu-id="c7db8-156">For **Date format**, if you used a date token in hello prefix path, you can select hello date format in which your files are organized.</span></span>
    * <span data-ttu-id="c7db8-157">Pro **formát času**, pokud jste použili token čas v cestě hello předponu, zadejte hello čas formát, ve kterém jsou uspořádány soubory.</span><span class="sxs-lookup"><span data-stu-id="c7db8-157">For **Time format**, if you used a time token in hello prefix path, specify hello time format in which your files are organized.</span></span>
    * <span data-ttu-id="c7db8-158">Pro **formát serializace událostí**, vyberte **CSV**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-158">For **Event serialization format**, select **CSV**.</span></span>
    * <span data-ttu-id="c7db8-159">Pro **oddělovač**, vyberte **kartě**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-159">For **Delimiter**, select **tab**.</span></span>
    * <span data-ttu-id="c7db8-160">Pro **kódování**, vyberte **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-160">For **Encoding**, select **UTF-8**.</span></span>
    
    <span data-ttu-id="c7db8-161">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-161">Click **Create**.</span></span> <span data-ttu-id="c7db8-162">portál Hello teď přidá výstup hello a otestuje připojení tooit hello.</span><span class="sxs-lookup"><span data-stu-id="c7db8-162">hello portal now adds hello output and tests hello connection tooit.</span></span>
    
## <a name="run-hello-stream-analytics-job"></a><span data-ttu-id="c7db8-163">Spustit úlohu služby Stream Analytics hello</span><span class="sxs-lookup"><span data-stu-id="c7db8-163">Run hello Stream Analytics job</span></span>

1. <span data-ttu-id="c7db8-164">toorun úloha Stream Analytics, je nutné spustit dotaz z hello **dotazu** kartě. V tomto kurzu můžete spustit ukázkový dotaz hello nahraďte zástupné symboly hello hello úlohy vstupní a výstupní aliasy, jak je znázorněno v následujícím snímku obrazovky hello.</span><span class="sxs-lookup"><span data-stu-id="c7db8-164">toorun a Stream Analytics job, you must run a query from hello **Query** tab. For this tutorial, you can run hello sample query by replacing hello placeholders with hello job input and output aliases, as shown in hello screen capture below.</span></span>

    <span data-ttu-id="c7db8-165">![Spuštění dotazu](./media/data-lake-store-stream-analytics/run.query.png "spuštění dotazu")</span><span class="sxs-lookup"><span data-stu-id="c7db8-165">![Run query](./media/data-lake-store-stream-analytics/run.query.png "Run query")</span></span>

2. <span data-ttu-id="c7db8-166">Klikněte na tlačítko **Uložit** shora hello hello obrazovky a pak z hello **přehled** , klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-166">Click **Save** from hello top of hello screen, and then from hello **Overview** tab, click **Start**.</span></span> <span data-ttu-id="c7db8-167">Dialogové okno hello, vyberte **vlastní čas**a poté nastavte hello aktuální datum a čas.</span><span class="sxs-lookup"><span data-stu-id="c7db8-167">From hello dialog box, select **Custom Time**, and then set hello current date and time.</span></span>

    <span data-ttu-id="c7db8-168">![Nastavit čas úlohy](./media/data-lake-store-stream-analytics/run.query.2.png "nastavit čas úlohy")</span><span class="sxs-lookup"><span data-stu-id="c7db8-168">![Set job time](./media/data-lake-store-stream-analytics/run.query.2.png "Set job time")</span></span>

    <span data-ttu-id="c7db8-169">Klikněte na tlačítko **spustit** toostart hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="c7db8-169">Click **Start** toostart hello job.</span></span> <span data-ttu-id="c7db8-170">To může trvat až tooa několik minut toostart hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="c7db8-170">It can take up tooa couple minutes toostart hello job.</span></span>

3. <span data-ttu-id="c7db8-171">data hello tootrigger hello úlohy toopick z objektu blob hello, zkopírujte kontejneru objektů blob souboru toohello ukázková data.</span><span class="sxs-lookup"><span data-stu-id="c7db8-171">tootrigger hello job toopick hello data from hello blob, copy a sample data file toohello blob container.</span></span> <span data-ttu-id="c7db8-172">Ukázkový datový soubor můžete získat z hello [úložiště Git Azure Data Lake](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span><span class="sxs-lookup"><span data-stu-id="c7db8-172">You can get a sample data file from hello [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span></span> <span data-ttu-id="c7db8-173">V tomto kurzu budeme zkopírujte soubor hello **vehicle1_09142014.csv**.</span><span class="sxs-lookup"><span data-stu-id="c7db8-173">For this tutorial, let's copy hello file **vehicle1_09142014.csv**.</span></span> <span data-ttu-id="c7db8-174">Můžete používat různé klienty, jako třeba [Azure Storage Explorer](http://storageexplorer.com/), kontejner objektů blob tooa tooupload data.</span><span class="sxs-lookup"><span data-stu-id="c7db8-174">You can use various clients, such as [Azure Storage Explorer](http://storageexplorer.com/), tooupload data tooa blob container.</span></span>

4. <span data-ttu-id="c7db8-175">Z hello **přehled** v části **monitorování**, najdete v části jak zpracování dat hello.</span><span class="sxs-lookup"><span data-stu-id="c7db8-175">From hello **Overview** tab, under **Monitoring**, see how hello data was processed.</span></span>

    <span data-ttu-id="c7db8-176">![Úloha monitoru](./media/data-lake-store-stream-analytics/run.query.3.png "úlohy monitorování")</span><span class="sxs-lookup"><span data-stu-id="c7db8-176">![Monitor job](./media/data-lake-store-stream-analytics/run.query.3.png "Monitor job")</span></span>

5. <span data-ttu-id="c7db8-177">Nakonec můžete ověřit, zda je k dispozici v účtu Data Lake Store hello hello úlohy výstupní data.</span><span class="sxs-lookup"><span data-stu-id="c7db8-177">Finally, you can verify that hello job output data is available in hello Data Lake Store account.</span></span> 

    <span data-ttu-id="c7db8-178">![Zkontrolujte výstup](./media/data-lake-store-stream-analytics/run.query.4.png "ověřte výstup")</span><span class="sxs-lookup"><span data-stu-id="c7db8-178">![Verify output](./media/data-lake-store-stream-analytics/run.query.4.png "Verify output")</span></span>

    <span data-ttu-id="c7db8-179">V podokně Průzkumník dat hello, Všimněte si, že hello výstup jako cestu ke složce napsané tooa zadaný v hello, nastavení výstupu Data Lake Store (`streamanalytics/job/output/{date}/{time}`).</span><span class="sxs-lookup"><span data-stu-id="c7db8-179">In hello Data Explorer pane, notice that hello output is written tooa folder path as specified in hello Data Lake Store output settings (`streamanalytics/job/output/{date}/{time}`).</span></span>  

## <a name="see-also"></a><span data-ttu-id="c7db8-180">Viz také</span><span class="sxs-lookup"><span data-stu-id="c7db8-180">See also</span></span>
* [<span data-ttu-id="c7db8-181">Vytvoření toouse clusteru HDInsight Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="c7db8-181">Create an HDInsight cluster toouse Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
