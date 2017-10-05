---
title: "Streamovat data ze služby Stream Analytics do Data Lake Store | Microsoft Docs"
description: "Pomocí Azure Stream Analytics datový proud dat do Azure Data Lake Store"
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
ms.openlocfilehash: 90104aaacf24a5a7156900fc3848a27f60329814
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="stream-data-from-azure-storage-blob-into-data-lake-store-using-azure-stream-analytics"></a><span data-ttu-id="a8bd9-103">Streamování dat z Azure Storage Blob do služby Data Lake Store pomocí Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a8bd9-103">Stream data from Azure Storage Blob into Data Lake Store using Azure Stream Analytics</span></span>
<span data-ttu-id="a8bd9-104">V tomto článku se dozvíte, jak používat Azure Data Lake Store jako výstup pro úlohu služby Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-104">In this article you will learn how to use Azure Data Lake Store as an output for an Azure Stream Analytics job.</span></span> <span data-ttu-id="a8bd9-105">Tento článek ukazuje jednoduchého scénáře, který čte data z objektu blob Azure Storage (vstup) a zapisuje data do Data Lake Store (výstup).</span><span class="sxs-lookup"><span data-stu-id="a8bd9-105">This article demonstrates a simple scenario that reads data from an Azure Storage blob (input) and writes the data to Data Lake Store (output).</span></span>

> [!NOTE]
> <span data-ttu-id="a8bd9-106">V tomto okamžiku vytvoření a konfigurace Data Lake Store výstupy Stream Analytics je podporováno pouze v [portálu Azure Classic](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="a8bd9-106">At this time, creation and configuration of Data Lake Store outputs for Stream Analytics is supported only in the [Azure Classic Portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="a8bd9-107">Proto některé části tohoto kurzu použije portál Azure Classic.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-107">Hence, some parts of this tutorial will use the Azure Classic Portal.</span></span>
>
>

## <a name="prerequisites"></a><span data-ttu-id="a8bd9-108">Požadavky</span><span class="sxs-lookup"><span data-stu-id="a8bd9-108">Prerequisites</span></span>
<span data-ttu-id="a8bd9-109">Je nutné, abyste před zahájením tohoto kurzu měli tyto položky:</span><span class="sxs-lookup"><span data-stu-id="a8bd9-109">Before you begin this tutorial, you must have the following:</span></span>

* <span data-ttu-id="a8bd9-110">**Předplatné Azure**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-110">**An Azure subscription**.</span></span> <span data-ttu-id="a8bd9-111">Viz [Získání bezplatné zkušební verze Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a8bd9-111">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="a8bd9-112">**Účet služby Azure Storage**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-112">**Azure Storage account**.</span></span> <span data-ttu-id="a8bd9-113">Kontejner objektů blob z tohoto účtu se použít jako vstup data úlohy Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-113">You will use a blob container from this account to input data for a Stream Analytics job.</span></span> <span data-ttu-id="a8bd9-114">Pro tento kurz předpokládá, že máte účet úložiště s názvem **storageforasa** kontejner v rámci účtu s názvem **storageforasacontainer**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-114">For this tutorial, assume you have a storage account called **storageforasa** and a container within the account called **storageforasacontainer**.</span></span> <span data-ttu-id="a8bd9-115">Po vytvoření kontejneru nahrajte do ní ukázkový datový soubor.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-115">Once you have created the container, upload a sample data file to it.</span></span> 
  
* <span data-ttu-id="a8bd9-116">**Účet Azure Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-116">**Azure Data Lake Store account**.</span></span> <span data-ttu-id="a8bd9-117">Postupujte podle pokynů v tématu [Začínáme s Azure Data Lake Store s použitím webu Azure Portal](data-lake-store-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="a8bd9-117">Follow the instructions at [Get started with Azure Data Lake Store using the Azure Portal](data-lake-store-get-started-portal.md).</span></span> <span data-ttu-id="a8bd9-118">Předpokládejme, máte účet Data Lake Store s názvem **asadatalakestore**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-118">Let's assume you have a Data Lake Store account called **asadatalakestore**.</span></span> 

## <a name="create-a-stream-analytics-job"></a><span data-ttu-id="a8bd9-119">Vytvoření úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a8bd9-119">Create a Stream Analytics Job</span></span>
<span data-ttu-id="a8bd9-120">Začněte vytvořením úlohy služby Stream Analytics, který obsahuje vstupní zdroj a cíl výstupu.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-120">You start by creating a Stream Analytics job that includes an input source and an output destination.</span></span> <span data-ttu-id="a8bd9-121">V tomto kurzu je zdroj kontejner objektů blob v Azure a jako cíl byla Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-121">For this tutorial, the source is an Azure blob container and the destination is Data Lake Store.</span></span>

1. <span data-ttu-id="a8bd9-122">Přihlaste se k [webu Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a8bd9-122">Sign on to the [Azure Portal](https://portal.azure.com).</span></span>

2. <span data-ttu-id="a8bd9-123">V levém podokně klikněte na tlačítko **úlohy Stream Analytics**a potom klikněte na **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-123">From the left pane, click **Stream Analytics jobs**, and then click **Add**.</span></span>

    <span data-ttu-id="a8bd9-124">![Vytvoření úlohy Stream Analytics](./media/data-lake-store-stream-analytics/create.job.png "vytvořit úlohu služby Stream Analytics")</span><span class="sxs-lookup"><span data-stu-id="a8bd9-124">![Create a Stream Analytics Job](./media/data-lake-store-stream-analytics/create.job.png "Create a Stream Analytics job")</span></span>

    > [!NOTE]
    > <span data-ttu-id="a8bd9-125">Zajistěte, aby ve stejné oblasti jako účet úložiště vytvoříte úlohu nebo je zpoplatněná další náklady na přesun dat mezi oblastmi.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-125">Make sure you create job in the same region as the storage account or you will incur additional cost of moving data between regions.</span></span>
    >

## <a name="create-a-blob-input-for-the-job"></a><span data-ttu-id="a8bd9-126">Vytvoření vstupní objekt Blob pro úlohu</span><span class="sxs-lookup"><span data-stu-id="a8bd9-126">Create a Blob input for the job</span></span>

1. <span data-ttu-id="a8bd9-127">Otevřete stránku pro úlohu služby Stream Analytics v levém podokně klikněte **vstupy** a pak klikněte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-127">Open the page for the Stream Analytics job, from the left pane click the **Inputs** tab, and then click **Add**.</span></span>

    <span data-ttu-id="a8bd9-128">![Přidat vstup do úlohy](./media/data-lake-store-stream-analytics/create.input.1.png "přidat vstup do úlohy")</span><span class="sxs-lookup"><span data-stu-id="a8bd9-128">![Add an input to your job](./media/data-lake-store-stream-analytics/create.input.1.png "Add an input to your job")</span></span>

2. <span data-ttu-id="a8bd9-129">Na **nový vstupní** okno, zadejte následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-129">On the **New input** blade, provide the following values.</span></span>

    <span data-ttu-id="a8bd9-130">![Přidat vstup do úlohy](./media/data-lake-store-stream-analytics/create.input.2.png "přidat vstup do úlohy")</span><span class="sxs-lookup"><span data-stu-id="a8bd9-130">![Add an input to your job](./media/data-lake-store-stream-analytics/create.input.2.png "Add an input to your job")</span></span>

    * <span data-ttu-id="a8bd9-131">Pro **vstupní alias**, zadejte jedinečný název pro vstupní úlohu.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-131">For **Input alias**, enter a unique name for the job input.</span></span>
    * <span data-ttu-id="a8bd9-132">Pro **typ zdroje**, vyberte **datový proud**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-132">For **Source type**, select **Data stream**.</span></span>
    * <span data-ttu-id="a8bd9-133">Pro **zdroj**, vyberte **úložiště objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-133">For **Source**, select **Blob storage**.</span></span>
    * <span data-ttu-id="a8bd9-134">Pro **předplatné**, vyberte **pomocí úložiště objektů blob z aktuálního předplatného**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-134">For **Subscription**, select **Use blob storage from current subscription**.</span></span>
    * <span data-ttu-id="a8bd9-135">Pro **účet úložiště**, vyberte účet úložiště, který jste vytvořili v rámci požadované součásti.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-135">For **Storage account**, select the storage account that you created as part of the prerequisites.</span></span> 
    * <span data-ttu-id="a8bd9-136">Pro **kontejneru**, vyberte kontejner, který jste vytvořili v na vybraný účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-136">For **Container**, select the container that you created in the selected storage account.</span></span>
    * <span data-ttu-id="a8bd9-137">Pro **formát serializace událostí**, vyberte **CSV**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-137">For **Event serialization format**, select **CSV**.</span></span>
    * <span data-ttu-id="a8bd9-138">Pro **oddělovač**, vyberte **kartě**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-138">For **Delimiter**, select **tab**.</span></span>
    * <span data-ttu-id="a8bd9-139">Pro **kódování**, vyberte **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-139">For **Encoding**, select **UTF-8**.</span></span>

    <span data-ttu-id="a8bd9-140">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-140">Click **Create**.</span></span> <span data-ttu-id="a8bd9-141">Na portálu teď přidá vstupu a otestuje připojení k němu.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-141">The portal now adds the input and tests the connection to it.</span></span>


## <a name="create-a-data-lake-store-output-for-the-job"></a><span data-ttu-id="a8bd9-142">Vytvoření výstupní Data Lake Store pro úlohu</span><span class="sxs-lookup"><span data-stu-id="a8bd9-142">Create a Data Lake Store output for the job</span></span>

1. <span data-ttu-id="a8bd9-143">Klikněte na Otevřít stránku pro úlohu služby Stream Analytics **výstupy** a pak klikněte **přidat**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-143">Open the page for the Stream Analytics job, click the **Outputs** tab, and then click **Add**.</span></span>

    <span data-ttu-id="a8bd9-144">![Přidat výstup do vaší úlohy](./media/data-lake-store-stream-analytics/create.output.1.png "přidat výstup do úlohy")</span><span class="sxs-lookup"><span data-stu-id="a8bd9-144">![Add an output to your job](./media/data-lake-store-stream-analytics/create.output.1.png "Add an output to your job")</span></span>

2. <span data-ttu-id="a8bd9-145">Na **nový výstupní** okno, zadejte následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-145">On the **New output** blade, provide the following values.</span></span>

    <span data-ttu-id="a8bd9-146">![Přidat výstup do vaší úlohy](./media/data-lake-store-stream-analytics/create.output.2.png "přidat výstup do úlohy")</span><span class="sxs-lookup"><span data-stu-id="a8bd9-146">![Add an output to your job](./media/data-lake-store-stream-analytics/create.output.2.png "Add an output to your job")</span></span>

    * <span data-ttu-id="a8bd9-147">Pro **alias pro výstup**, zadejte jedinečný název pro výstup úlohy.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-147">For **Output alias**, enter a a unique name for the job output.</span></span> <span data-ttu-id="a8bd9-148">Toto je popisný název používaný v dotazech na přesměrujte výstup dotazu do tohoto Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-148">This is a friendly name used in queries to direct the query output to this Data Lake Store.</span></span>
    * <span data-ttu-id="a8bd9-149">Pro **jímky**, vyberte **Data Lake Store**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-149">For **Sink**, select **Data Lake Store**.</span></span>
    * <span data-ttu-id="a8bd9-150">Zobrazí se výzva k autorizaci přístupu k účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-150">You will be prompted to authorize access to Data Lake Store account.</span></span> <span data-ttu-id="a8bd9-151">Klikněte na tlačítko **Autorizovat**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-151">Click **Authorize**.</span></span>

3. <span data-ttu-id="a8bd9-152">Na **nový výstupní** okně pokračovat v poskytování následující hodnoty.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-152">On the **New output** blade, continue to provide the following values.</span></span>

    <span data-ttu-id="a8bd9-153">![Přidat výstup do vaší úlohy](./media/data-lake-store-stream-analytics/create.output.3.png "přidat výstup do úlohy")</span><span class="sxs-lookup"><span data-stu-id="a8bd9-153">![Add an output to your job](./media/data-lake-store-stream-analytics/create.output.3.png "Add an output to your job")</span></span>

    * <span data-ttu-id="a8bd9-154">Pro **název účtu**, vyberte účet Data Lake Store, jste již vytvořili, kam chcete úlohy, výstup k odeslání.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-154">For **Account name**, select the Data Lake Store account you already created where you want the job output to be sent to.</span></span>
    * <span data-ttu-id="a8bd9-155">Pro **vzorek cesty předponu**, zadejte cestu k souboru umožňuje zapisovat vaše soubory v rámci zadaného účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-155">For **Path prefix pattern**, enter a file path used to write your files within the specified Data Lake Store account.</span></span>
    * <span data-ttu-id="a8bd9-156">Pro **formát data**, pokud jste použili token kalendářního data v cestě předponu, můžete vybrat formát data, ve kterém jsou uspořádány soubory.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-156">For **Date format**, if you used a date token in the prefix path, you can select the date format in which your files are organized.</span></span>
    * <span data-ttu-id="a8bd9-157">Pro **formát času**, pokud jste použili token čas v cestě předponu, zadejte formát času, ve kterém jsou uspořádány soubory.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-157">For **Time format**, if you used a time token in the prefix path, specify the time format in which your files are organized.</span></span>
    * <span data-ttu-id="a8bd9-158">Pro **formát serializace událostí**, vyberte **CSV**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-158">For **Event serialization format**, select **CSV**.</span></span>
    * <span data-ttu-id="a8bd9-159">Pro **oddělovač**, vyberte **kartě**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-159">For **Delimiter**, select **tab**.</span></span>
    * <span data-ttu-id="a8bd9-160">Pro **kódování**, vyberte **UTF-8**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-160">For **Encoding**, select **UTF-8**.</span></span>
    
    <span data-ttu-id="a8bd9-161">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-161">Click **Create**.</span></span> <span data-ttu-id="a8bd9-162">Na portálu se teď přidá výstup a otestuje připojení k němu.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-162">The portal now adds the output and tests the connection to it.</span></span>
    
## <a name="run-the-stream-analytics-job"></a><span data-ttu-id="a8bd9-163">Spustit úlohu služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a8bd9-163">Run the Stream Analytics job</span></span>

1. <span data-ttu-id="a8bd9-164">Chcete-li spustit úlohu služby Stream Analytics, je nutné spustit dotaz z **dotazu** kartě.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-164">To run a Stream Analytics job, you must run a query from the **Query** tab.</span></span> <span data-ttu-id="a8bd9-165">V tomto kurzu můžete spustit ukázkový dotaz nahraďte zástupné symboly úlohu vstupní a výstupní aliasy, jak je znázorněno v následujícím snímku obrazovky.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-165">For this tutorial, you can run the sample query by replacing the placeholders with the job input and output aliases, as shown in the screen capture below.</span></span>

    <span data-ttu-id="a8bd9-166">![Spuštění dotazu](./media/data-lake-store-stream-analytics/run.query.png "spuštění dotazu")</span><span class="sxs-lookup"><span data-stu-id="a8bd9-166">![Run query](./media/data-lake-store-stream-analytics/run.query.png "Run query")</span></span>

2. <span data-ttu-id="a8bd9-167">Klikněte na tlačítko **Uložit** z horní části obrazovky a pak z **přehled** , klikněte na **spustit**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-167">Click **Save** from the top of the screen, and then from the **Overview** tab, click **Start**.</span></span> <span data-ttu-id="a8bd9-168">V dialogovém okně vyberte **vlastní čas**a poté nastavte aktuální datum a čas.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-168">From the dialog box, select **Custom Time**, and then set the current date and time.</span></span>

    <span data-ttu-id="a8bd9-169">![Nastavit čas úlohy](./media/data-lake-store-stream-analytics/run.query.2.png "nastavit čas úlohy")</span><span class="sxs-lookup"><span data-stu-id="a8bd9-169">![Set job time](./media/data-lake-store-stream-analytics/run.query.2.png "Set job time")</span></span>

    <span data-ttu-id="a8bd9-170">Klikněte na tlačítko **spustit** spustíte úlohu.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-170">Click **Start** to start the job.</span></span> <span data-ttu-id="a8bd9-171">Ho může trvat až několik minut spustíte úlohu.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-171">It can take up to a couple minutes to start the job.</span></span>

3. <span data-ttu-id="a8bd9-172">Aktivuje úlohu a vyberte data z objektu blob, zkopírujte ukázkový datový soubor do kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-172">To trigger the job to pick the data from the blob, copy a sample data file to the blob container.</span></span> <span data-ttu-id="a8bd9-173">Můžete získat ukázkový soubor dat z [úložiště Git Azure Data Lake](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span><span class="sxs-lookup"><span data-stu-id="a8bd9-173">You can get a sample data file from the [Azure Data Lake Git Repository](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceData/Drivers.txt).</span></span> <span data-ttu-id="a8bd9-174">V tomto kurzu budeme zkopírujte soubor **vehicle1_09142014.csv**.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-174">For this tutorial, let's copy the file **vehicle1_09142014.csv**.</span></span> <span data-ttu-id="a8bd9-175">Můžete používat různé klienty, jako třeba [Azure Storage Explorer](http://storageexplorer.com/), odesílat data do kontejneru objektů blob.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-175">You can use various clients, such as [Azure Storage Explorer](http://storageexplorer.com/), to upload data to a blob container.</span></span>

4. <span data-ttu-id="a8bd9-176">Z **přehled** v části **monitorování**, najdete v části zpracovávání data.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-176">From the **Overview** tab, under **Monitoring**, see how the data was processed.</span></span>

    <span data-ttu-id="a8bd9-177">![Úloha monitoru](./media/data-lake-store-stream-analytics/run.query.3.png "úlohy monitorování")</span><span class="sxs-lookup"><span data-stu-id="a8bd9-177">![Monitor job](./media/data-lake-store-stream-analytics/run.query.3.png "Monitor job")</span></span>

5. <span data-ttu-id="a8bd9-178">Nakonec můžete ověřit, že data výstup úlohy je k dispozici v účtu Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-178">Finally, you can verify that the job output data is available in the Data Lake Store account.</span></span> 

    <span data-ttu-id="a8bd9-179">![Zkontrolujte výstup](./media/data-lake-store-stream-analytics/run.query.4.png "ověřte výstup")</span><span class="sxs-lookup"><span data-stu-id="a8bd9-179">![Verify output](./media/data-lake-store-stream-analytics/run.query.4.png "Verify output")</span></span>

    <span data-ttu-id="a8bd9-180">V podokně Průzkumník dat, Všimněte si, že je výstup zapsán do cesty ke složce uvedeného v Data Lake Store nastavení výstupu (`streamanalytics/job/output/{date}/{time}`).</span><span class="sxs-lookup"><span data-stu-id="a8bd9-180">In the Data Explorer pane, notice that the output is written to a folder path as specified in the Data Lake Store output settings (`streamanalytics/job/output/{date}/{time}`).</span></span>  

## <a name="see-also"></a><span data-ttu-id="a8bd9-181">Viz také</span><span class="sxs-lookup"><span data-stu-id="a8bd9-181">See also</span></span>
* [<span data-ttu-id="a8bd9-182">Vytvoření clusteru HDInsight používat Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="a8bd9-182">Create an HDInsight cluster to use Data Lake Store</span></span>](data-lake-store-hdinsight-hadoop-use-portal.md)
