---
title: Integrace Azure Stream Analytics a Machine Learning | Microsoft Docs
description: "Jak používat uživatelsky definované funkce a Machine Learning v úloze Stream Analytics"
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: cfced01f-ccaa-4bc6-81e2-c03d1470a7a2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 07/06/2017
ms.author: jeffstok
ms.openlocfilehash: 023033d5479fcf0e2dff168b6604431eef283d3b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="performing-sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a><span data-ttu-id="d5c1e-103">Provedení analýzy postojích pomocí Azure Stream Analytics nebo Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="d5c1e-103">Performing sentiment analysis by using Azure Stream Analytics and Azure Machine Learning</span></span>
<span data-ttu-id="d5c1e-104">Tento článek popisuje, jak rychle nastavit jednoduchou úlohu Azure Stream Analytics, která integruje Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-104">This article describes how to quickly set up a simple Azure Stream Analytics job that integrates Azure Machine Learning.</span></span> <span data-ttu-id="d5c1e-105">Používáte model Machine Learning postojích analytics z webu Cortana Intelligence Gallery a analyzovat data streamovaná text určení skóre postojích v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-105">You use a Machine Learning sentiment analytics model from the Cortana Intelligence Gallery to analyze streaming text data and determine the sentiment score in real time.</span></span> <span data-ttu-id="d5c1e-106">Pomocí Cortana Intelligence Suite umožňuje provedení této úlohy bez obav o rozbor všech vytváření model postojích analytics.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-106">Using the Cortana Intelligence Suite lets you accomplish this task without worrying about the intricacies of building a sentiment analytics model.</span></span>

<span data-ttu-id="d5c1e-107">Můžete použít co dozvíte z tohoto článku pro scénáře, jako je například tyto:</span><span class="sxs-lookup"><span data-stu-id="d5c1e-107">You can apply what you learn from this article to scenarios such as these:</span></span>

* <span data-ttu-id="d5c1e-108">Analýza v reálném čase postojích na streamování dat Twitteru.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-108">Analyzing real-time sentiment on streaming Twitter data.</span></span>
* <span data-ttu-id="d5c1e-109">Analýza záznamy zákazníka konverzace s pracovníky technické podpory.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-109">Analyzing records of customer chats with support staff.</span></span>
* <span data-ttu-id="d5c1e-110">Vyhodnocení komentáře na fóra, blogy a videa.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-110">Evaluating comments on forums, blogs, and videos.</span></span> 
* <span data-ttu-id="d5c1e-111">Mnoho dalších v reálném čase, prediktivní vyhodnocování scénářů.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-111">Many other real-time, predictive scoring scenarios.</span></span>

<span data-ttu-id="d5c1e-112">Ve scénáři reálného by získat data přímo z datového proudu Twitter.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-112">In a real-world scenario, you would get the data directly from a Twitter data stream.</span></span> <span data-ttu-id="d5c1e-113">Pro zjednodušení tento kurz, jsme jste jej tak, aby úlohy streamování Analytics získá tweetů ze souboru CSV v úložišti objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-113">To simplify the tutorial, we've written it so that the Streaming Analytics job gets tweets from a CSV file in Azure Blob storage.</span></span> <span data-ttu-id="d5c1e-114">Můžete vytvořit svůj vlastní soubor CSV, nebo můžete použít ukázkový soubor CSV, jak je znázorněno na následujícím obrázku:</span><span class="sxs-lookup"><span data-stu-id="d5c1e-114">You can create your own CSV file, or you can use a sample CSV file, as shown in the following image:</span></span>

![Ukázka tweetů v souboru CSV](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

<span data-ttu-id="d5c1e-116">Úlohy streamování Analytics, který vytvoříte platí model analýzy postojích jako uživatelem definované funkce (UDF) na ukázková data text z úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-116">The Streaming Analytics job that you create applies the sentiment analytics model as a user-defined function (UDF) on the sample text data from the blob store.</span></span> <span data-ttu-id="d5c1e-117">Výstup (výsledek analýzy postojích) je zapsán do stejné úložiště objektů blob v jiném souboru CSV.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-117">The output (the result of the sentiment analysis) is written to the same blob store in a different CSV file.</span></span> 

<span data-ttu-id="d5c1e-118">Následující obrázek ukazuje tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-118">The following figure demonstrates this configuration.</span></span> <span data-ttu-id="d5c1e-119">Jak jsme uvedli, realističtější scénáři můžete nahradit úložiště objektů blob streamování Twitter dat z Azure Event Hubs vstup.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-119">As noted, for a more realistic scenario, you can replace blob storage with streaming Twitter data from an Azure Event Hubs input.</span></span> <span data-ttu-id="d5c1e-120">Kromě toho je sestavení [Microsoft Power BI](https://powerbi.microsoft.com/) v reálném čase vizualizace agregační postojích.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-120">Additionally, you could build a [Microsoft Power BI](https://powerbi.microsoft.com/) real-time visualization of the aggregate sentiment.</span></span>    

![Přehled integrace Stream Analytics Machine Learning](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a><span data-ttu-id="d5c1e-122">Požadavky</span><span class="sxs-lookup"><span data-stu-id="d5c1e-122">Prerequisites</span></span>
<span data-ttu-id="d5c1e-123">Než začnete, ujistěte se, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="d5c1e-123">Before you start, make sure you have the following:</span></span>

* <span data-ttu-id="d5c1e-124">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-124">An active Azure subscription.</span></span>
* <span data-ttu-id="d5c1e-125">Soubor CSV s některá data v ní.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-125">A CSV file with some data in it.</span></span> <span data-ttu-id="d5c1e-126">Si můžete stáhnout soubor uvedena výše z [Githubu](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), nebo můžete vytvořit svůj vlastní soubor.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-126">You can download the file shown earlier from [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), or you can create your own file.</span></span> <span data-ttu-id="d5c1e-127">V tomto článku předpokládáme, že používáte soubor z Githubu.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-127">For this article, we assume that you're using the file from GitHub.</span></span>

<span data-ttu-id="d5c1e-128">Na vysoké úrovni k dokončení úkolů ukázáno v tomto článku jste takto:</span><span class="sxs-lookup"><span data-stu-id="d5c1e-128">At a high level, to complete the tasks demonstrated in this article, you do the following:</span></span>

1. <span data-ttu-id="d5c1e-129">Vytvořit účet úložiště Azure a kontejner úložiště objektů blob a nahrajte vstupní soubor ve formátu CSV do kontejneru.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-129">Create an Azure storage account and a blob storage container, and upload a CSV-formatted input file to the container.</span></span>
3. <span data-ttu-id="d5c1e-130">Přidání modelu analytics postojích z webu Cortana Intelligence Gallery do pracovního prostoru Azure Machine Learning a nasadit tento model jako webovou službu v pracovním prostoru Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-130">Add a sentiment analytics model from the Cortana Intelligence Gallery to your Azure Machine Learning workspace and deploy this model as a web service in the Machine Learning workspace.</span></span>
5. <span data-ttu-id="d5c1e-131">Vytvořte úlohu služby Stream Analytics, který volání této webové služby jako funkci, aby bylo možné zjistit postojích pro zadávání textu.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-131">Create a Stream Analytics job that calls this web service as a function in order to determine sentiment for the text input.</span></span>
6. <span data-ttu-id="d5c1e-132">Spustit úlohu služby Stream Analytics a zkontrolujte výstup.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-132">Start the Stream Analytics job and check the output.</span></span>

## <a name="create-a-storage-container-and-upload-the-csv-input-file"></a><span data-ttu-id="d5c1e-133">Vytvoření kontejneru úložiště a načtěte vstupní soubor CSV</span><span class="sxs-lookup"><span data-stu-id="d5c1e-133">Create a storage container and upload the CSV input file</span></span>
<span data-ttu-id="d5c1e-134">Pro tento krok můžete použít všechny souboru CSV, jako je třeba k dispozici z Githubu.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-134">For this step, you can use any CSV file, such as the one available from GitHub.</span></span>

1. <span data-ttu-id="d5c1e-135">Na portálu Azure klikněte na tlačítko **nový** &gt; **úložiště** &gt; **účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-135">In the Azure portal, click **New** &gt; **Storage** &gt; **Storage account**.</span></span>

   ![Vytvořit nový účet úložiště](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-create-storage-account.png)

2. <span data-ttu-id="d5c1e-137">Zadejte název (`samldemo` v příkladu).</span><span class="sxs-lookup"><span data-stu-id="d5c1e-137">Provide a name (`samldemo` in the example).</span></span> <span data-ttu-id="d5c1e-138">Název můžete použít jenom malá písmena a čísla a musí být jedinečný v Azure.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-138">The name can use only lowercase letters and numbers, and it must be unique across Azure.</span></span> 

3. <span data-ttu-id="d5c1e-139">Zadejte existující skupinu prostředků a zadejte umístění.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-139">Specify an existing resource group and specify a location.</span></span> <span data-ttu-id="d5c1e-140">Pro umístění doporučujeme všechny prostředky, které jsou vytvořené v tomto kurzu použít stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-140">For location, we recommend that all the resources created in this tutorial use the same location.</span></span>

    ![Zadejte podrobnosti o účtu úložiště](./media/stream-analytics-machine-learning-integration-tutorial/create-sa1.png)

4. <span data-ttu-id="d5c1e-142">Na portálu Azure vyberte účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-142">In the Azure portal, select the storage account.</span></span> <span data-ttu-id="d5c1e-143">V okně účtu úložiště, klikněte na tlačítko **kontejnery** a pak klikněte na  **+ &nbsp;kontejneru** k vytvoření úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-143">In the storage account blade, click **Containers** and then click **+&nbsp;Container** to create blob storage.</span></span>

    ![Vytvoření kontejneru objektů blob](./media/stream-analytics-machine-learning-integration-tutorial/create-sa2.png)

5. <span data-ttu-id="d5c1e-145">Zadejte název kontejneru (`azuresamldemoblob` v příkladu) a ověřte, že **přistupovat typu** je nastaven na **Blob**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-145">Provide a name for the container (`azuresamldemoblob` in the example) and verify that **Access type** is set to **Blob**.</span></span> <span data-ttu-id="d5c1e-146">Až to budete mít, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-146">When you're done, click **OK**.</span></span>

    ![Zadejte podrobnosti kontejner objektů blob](./media/stream-analytics-machine-learning-integration-tutorial/create-sa3.png)

6. <span data-ttu-id="d5c1e-148">V **kontejnery** okně vyberte nový kontejner, který se otevře v okně pro tento kontejner.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-148">In the **Containers** blade, select the new container, which opens the blade for that container.</span></span>

7. <span data-ttu-id="d5c1e-149">Klikněte na **Odeslat**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-149">Click **Upload**.</span></span>

    ![Nahrát tlačítko pro kontejner](./media/stream-analytics-machine-learning-integration-tutorial/create-sa-upload-button.png)

8. <span data-ttu-id="d5c1e-151">V **nahrávání blob** okno, zadejte soubor CSV, který chcete použít pro tento kurz.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-151">In the **Upload blob** blade, specify the CSV file that you want to use for this tutorial.</span></span> <span data-ttu-id="d5c1e-152">Pro **Blob typ**, vyberte **objekt blob bloku** a nastavit velikost bloku na 4 MB, který je dostatečný pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-152">For **Blob type**, select **Block blob** and set the block size to 4 MB, which is sufficient for this tutorial.</span></span>

    ![Nahrát soubor blob](./media/stream-analytics-machine-learning-integration-tutorial/create-sa4.png)

9. <span data-ttu-id="d5c1e-154">Klikněte **nahrát** tlačítko v dolní části okna.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-154">Click the **Upload** button at the bottom of the blade.</span></span>

## <a name="add-the-sentiment-analytics-model-from-the-cortana-intelligence-gallery"></a><span data-ttu-id="d5c1e-155">Přidat model analýzy postojích z webu Cortana Intelligence Gallery</span><span class="sxs-lookup"><span data-stu-id="d5c1e-155">Add the sentiment analytics model from the Cortana Intelligence Gallery</span></span>

<span data-ttu-id="d5c1e-156">Teď, když je ukázková data do objektu BLOB, můžete povolit model postojích analýzy v Cortana Intelligence Gallery.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-156">Now that the sample data is in a blob, you can enable the sentiment analysis model in Cortana Intelligence Gallery.</span></span>

1. <span data-ttu-id="d5c1e-157">Přejděte na [model postojích prediktivní analýzy](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) stránky v Cortana Intelligence Gallery.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-157">Go to the [predictive sentiment analytics model](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) page in the Cortana Intelligence Gallery.</span></span>  

2. <span data-ttu-id="d5c1e-158">Klikněte na tlačítko **Open in Studio**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-158">Click **Open in Studio**.</span></span>  
   
   ![Stream Analytics Machine Learning, otevřete Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. <span data-ttu-id="d5c1e-160">Přihlaste se k přejděte do pracovního prostoru.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-160">Sign in to go to the workspace.</span></span> <span data-ttu-id="d5c1e-161">Vyberte umístění.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-161">Select a location.</span></span>

4. <span data-ttu-id="d5c1e-162">Klikněte na tlačítko **spustit** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-162">Click **Run** at the bottom of the page.</span></span> <span data-ttu-id="d5c1e-163">Spustí se proces, který trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-163">The process runs, which takes about a minute.</span></span>

   ![Spusťte experiment v nástroji Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-run-experiment.png)  

5. <span data-ttu-id="d5c1e-165">Poté, co proces proběhla úspěšně, vyberte **nasazení webové služby** v dolní části stránky.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-165">After the process has run successfully, select **Deploy Web Service** at the bottom of the page.</span></span>

   ![experiment v nástroji Machine Learning Studio nasaďte jako webovou službu](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-deploy-web-service.png)  

6. <span data-ttu-id="d5c1e-167">Chcete-li ověřit, zda model analýzy postojích připravené k použití, klikněte na tlačítko **Test** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-167">To validate that the sentiment analytics model is ready to use, click the **Test** button.</span></span> <span data-ttu-id="d5c1e-168">Zadejte textový vstup, jako je "I rádi Microsoft".</span><span class="sxs-lookup"><span data-stu-id="d5c1e-168">Provide text input such as "I love Microsoft".</span></span> 

   ![Test experimentu v nástroji Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test.png)  

    <span data-ttu-id="d5c1e-170">Pokud test funguje, zobrazí výsledek podobně jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="d5c1e-170">If the test works, you see a result similar to the following example:</span></span>

   ![výsledky testu v nástroji Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test-results.png)  

7. <span data-ttu-id="d5c1e-172">V **aplikace** sloupce, klikněte na tlačítko **Excel 2010 nebo starší sešitu** odkaz ke stažení sešitu aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-172">In the **Apps** column, click the **Excel 2010 or earlier workbook** link to download an Excel workbook.</span></span> <span data-ttu-id="d5c1e-173">Sešit obsahuje klíč rozhraní API a adresu URL, kterou budete potřebovat později nastavit úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-173">The workbook contains the an API key and the URL that you need later to set up the Stream Analytics job.</span></span>

    ![Stream Analytics Machine Learning, rychlého přehledu.](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  


## <a name="create-a-stream-analytics-job-that-uses-the-machine-learning-model"></a><span data-ttu-id="d5c1e-175">Vytvořit úlohu služby Stream Analytics, který používá model Machine Learning</span><span class="sxs-lookup"><span data-stu-id="d5c1e-175">Create a Stream Analytics job that uses the Machine Learning model</span></span>

<span data-ttu-id="d5c1e-176">Nyní můžete vytvořit úlohu služby Stream Analytics, která načte tweetů ukázka ze souboru CSV v úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-176">You can now create a Stream Analytics job that reads the sample tweets from the CSV file in blob storage.</span></span> 

### <a name="create-the-job"></a><span data-ttu-id="d5c1e-177">Vytvoření úlohy</span><span class="sxs-lookup"><span data-stu-id="d5c1e-177">Create the job</span></span>

1. <span data-ttu-id="d5c1e-178">Přejděte na [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d5c1e-178">Go to the [Azure portal](https://portal.azure.com).</span></span>  

2. <span data-ttu-id="d5c1e-179">Klikněte na tlačítko **nový** > **Internet věcí** > **úlohy služby Stream Analytics**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-179">Click **New** > **Internet of Things** > **Stream Analytics job**.</span></span> 

   ![Cesty Azure portálu pro získání do nové úlohy Stream Analytics](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-new-iot-sa-job.png)
   
3. <span data-ttu-id="d5c1e-181">Název úlohy `azure-sa-ml-demo`, zadejte předplatné, zadejte existující skupinu prostředků nebo vytvořte novou a vyberte umístění pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-181">Name the job `azure-sa-ml-demo`, specify a subscription, specify an existing resource group or create a new one, and select the location for the job.</span></span>

   ![Zadejte nastavení pro nové úlohy Stream Analytics](./media/stream-analytics-machine-learning-integration-tutorial/create-job-1.png)
   

### <a name="configure-the-job-input"></a><span data-ttu-id="d5c1e-183">Konfigurace vstupu úlohy</span><span class="sxs-lookup"><span data-stu-id="d5c1e-183">Configure the job input</span></span>
<span data-ttu-id="d5c1e-184">Úloha získá vstupní ze souboru CSV, který jste dříve nahráli do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-184">The job gets its input from the CSV file that you uploaded earlier to blob storage.</span></span>

1. <span data-ttu-id="d5c1e-185">Po úloha byla vytvořena, v části **úlohy topologie** v okně úlohy klikněte **vstupy** pole.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-185">After the job has been created, under **Job Topology** in the job blade, click the **Inputs** box.</span></span>  
   
   !["Vstupy" pole v okně úlohy Stream Analytics](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input.png)  

2. <span data-ttu-id="d5c1e-187">V **vstupy** okně klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-187">In the **Inputs** blade, click **+ Add**.</span></span>

   ![Přidání tlačítka pro přidání vstup do úlohy Stream Analytics](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input-button.png)  

3. <span data-ttu-id="d5c1e-189">Vyplňte **nové vstup** okno s těmito hodnotami:</span><span class="sxs-lookup"><span data-stu-id="d5c1e-189">Fill out the **New input** blade with these values:</span></span>

    * <span data-ttu-id="d5c1e-190">**Vstupní alias**: použijte název `datainput`.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-190">**Input alias**: Use the name `datainput`.</span></span>
    * <span data-ttu-id="d5c1e-191">**Typ zdroje**: vyberte **datový proud**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-191">**Source type**: Select **Data stream**.</span></span>
    * <span data-ttu-id="d5c1e-192">**Zdroj**: vyberte **úložiště objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-192">**Source**: Select **Blob storage**.</span></span>
    * <span data-ttu-id="d5c1e-193">**Import možnost**: vyberte **pomocí úložiště objektů blob z aktuálního předplatného**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-193">**Import option**: Select **Use blob storage from current subscription**.</span></span> 
    * <span data-ttu-id="d5c1e-194">**Účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-194">**Storage account**.</span></span> <span data-ttu-id="d5c1e-195">Vyberte účet úložiště, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-195">Select the storage account you created earlier.</span></span>
    * <span data-ttu-id="d5c1e-196">**Kontejner**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-196">**Container**.</span></span> <span data-ttu-id="d5c1e-197">Vyberte kontejner, který jste vytvořili dříve (`azuresamldemoblob`).</span><span class="sxs-lookup"><span data-stu-id="d5c1e-197">Select the container you created earlier (`azuresamldemoblob`).</span></span>
    * <span data-ttu-id="d5c1e-198">**Formát serializace událostí**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-198">**Event serialization format**.</span></span> <span data-ttu-id="d5c1e-199">Vyberte **CSV**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-199">Select **CSV**.</span></span>

    ![Nastavení pro nové úlohy vstup](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-create-sa-input-new-portal.png)

4. <span data-ttu-id="d5c1e-201">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-201">Click **Create**.</span></span>

### <a name="configure-the-job-output"></a><span data-ttu-id="d5c1e-202">Konfigurace výstup úlohy</span><span class="sxs-lookup"><span data-stu-id="d5c1e-202">Configure the job output</span></span>
<span data-ttu-id="d5c1e-203">Úloha odešle výsledky na stejné úložiště objektů blob kde získá vstup.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-203">The job sends results to the same blob storage where it gets input.</span></span> 

1. <span data-ttu-id="d5c1e-204">V části **úlohy topologie** v okně úlohy klikněte na tlačítko **výstupy** pole.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-204">Under **Job Topology** in the job blade, click the **Outputs** box.</span></span>  
  
   ![Vytvořit nový výstupní úlohy streamování Analytics](./media/stream-analytics-machine-learning-integration-tutorial/create-output.png)  

2. <span data-ttu-id="d5c1e-206">V **výstupy** okně klikněte na tlačítko **+ přidat**a poté přidejte výstup s aliasem `datamloutput`.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-206">In the **Outputs** blade, click **+ Add**, and then add an output with the alias `datamloutput`.</span></span> 

3. <span data-ttu-id="d5c1e-207">Pro **jímky**, vyberte **úložiště objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-207">For **Sink**, select **Blob storage**.</span></span> <span data-ttu-id="d5c1e-208">Pak zadejte ve zbývající části Nastavení výstupní pomocí stejné hodnoty, které jste použili pro úložiště objektů blob pro vstup:</span><span class="sxs-lookup"><span data-stu-id="d5c1e-208">Then fill in the rest of the output settings using the same values that you used for the blob storage for input:</span></span>

    * <span data-ttu-id="d5c1e-209">**Účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-209">**Storage account**.</span></span> <span data-ttu-id="d5c1e-210">Vyberte účet úložiště, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-210">Select the storage account you created earlier.</span></span>
    * <span data-ttu-id="d5c1e-211">**Kontejner**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-211">**Container**.</span></span> <span data-ttu-id="d5c1e-212">Vyberte kontejner, který jste vytvořili dříve (`azuresamldemoblob`).</span><span class="sxs-lookup"><span data-stu-id="d5c1e-212">Select the container you created earlier (`azuresamldemoblob`).</span></span>
    * <span data-ttu-id="d5c1e-213">**Formát serializace událostí**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-213">**Event serialization format**.</span></span> <span data-ttu-id="d5c1e-214">Vyberte **CSV**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-214">Select **CSV**.</span></span>

   ![Nastavení pro nové výstup úlohy](./media/stream-analytics-machine-learning-integration-tutorial/create-output2.png) 

4. <span data-ttu-id="d5c1e-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-216">Click **Create**.</span></span>   


### <a name="add-the-machine-learning-function"></a><span data-ttu-id="d5c1e-217">Přidání funkce Machine Learning</span><span class="sxs-lookup"><span data-stu-id="d5c1e-217">Add the Machine Learning function</span></span> 
<span data-ttu-id="d5c1e-218">Dříve jste publikovali model Machine Learning k webové službě.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-218">Earlier you published a Machine Learning model to a web service.</span></span> <span data-ttu-id="d5c1e-219">V tomto scénáři při spuštění úlohy analýzy datového proudu odešle každý ukázka tweet ze vstupu k webové službě pro analýzu postojích.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-219">In our scenario, when the Stream Analysis job runs, it sends each sample tweet from the input to the web service for sentiment analysis.</span></span> <span data-ttu-id="d5c1e-220">Vrátí webové službě Machine Learning postojích (`positive`, `neutral`, nebo `negative`) a pravděpodobnost vzniku tweet se kladné.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-220">The Machine Learning web service returns a sentiment (`positive`, `neutral`, or `negative`) and a probability of the tweet being positive.</span></span> 

<span data-ttu-id="d5c1e-221">V této části kurzu definujete funkce v Úloha analýzy datového proudu.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-221">In this section of the tutorial, you define a function in the Stream Analysis job.</span></span> <span data-ttu-id="d5c1e-222">Funkci nelze vyvolat odeslala tweet k webové službě a získat odpověď zpět.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-222">The function can be invoked to send a tweet to the web service and get the response back.</span></span> 

1. <span data-ttu-id="d5c1e-223">Ujistěte se, že máte webové adresy URL a rozhraní API klíč služby, který jste si stáhli dříve v sešitu aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-223">Make sure you have the web service URL and API key that you downloaded earlier in the Excel workbook.</span></span>

2. <span data-ttu-id="d5c1e-224">Vraťte se do okna Přehled úlohy.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-224">Return to the job overview blade.</span></span>

3. <span data-ttu-id="d5c1e-225">V části **nastavení**, vyberte **funkce** a pak klikněte na **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-225">Under **Settings**, select **Functions** and then click **+ Add**.</span></span>

   ![Přidání funkce do úlohy Stream Analytics](./media/stream-analytics-machine-learning-integration-tutorial/create-function1.png) 

4. <span data-ttu-id="d5c1e-227">Zadejte `sentiment` jako alias funkce a vyplňte na zbytek okna pomocí těchto hodnot:</span><span class="sxs-lookup"><span data-stu-id="d5c1e-227">Enter `sentiment` as the function alias and fill out the rest of the blade using these values:</span></span>

    * <span data-ttu-id="d5c1e-228">**Typ funkce**: vyberte **Azure ML**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-228">**Function type**: Select **Azure ML**.</span></span>
    * <span data-ttu-id="d5c1e-229">**Import možnost**: vyberte **Import z jiného předplatného**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-229">**Import option**: Select **Import from a different subscription**.</span></span> <span data-ttu-id="d5c1e-230">To vám dává možnost zadat adresu URL a klíč.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-230">This gives you a chance to enter the URL and key.</span></span>
    * <span data-ttu-id="d5c1e-231">**Adresa URL**: vložte adresu URL webové služby.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-231">**URL**: Paste in the web service URL.</span></span>
    * <span data-ttu-id="d5c1e-232">**Klíč**: vložte klíč rozhraní API.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-232">**Key**: Paste in the API key.</span></span>
  
    ![Nastavení pro přidání funkce Machine Learning do úlohy Stream Analytics](./media/stream-analytics-machine-learning-integration-tutorial/add-function.png)  
    
5. <span data-ttu-id="d5c1e-234">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-234">Click **Create**.</span></span>

### <a name="create-a-query-to-transform-the-data"></a><span data-ttu-id="d5c1e-235">Vytvořit dotaz, který transformace dat</span><span class="sxs-lookup"><span data-stu-id="d5c1e-235">Create a query to transform the data</span></span>

<span data-ttu-id="d5c1e-236">Stream Analytics používá k ověření vstupu a zpracovat dotaz deklarativní, na základě SQL.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-236">Stream Analytics uses a declarative, SQL-based query to examine the input and process it.</span></span> <span data-ttu-id="d5c1e-237">V této části vytvoříte dotaz, který čte každý tweet ze vstupu a pak zavolá funkci Machine Learning k analýze postojích.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-237">In this section, you create a query that reads each tweet from input and then invokes the Machine Learning function to perform sentiment analysis.</span></span> <span data-ttu-id="d5c1e-238">Výsledek dotazu pak odešle na výstupu, že jste definovali (úložiště objektů blob).</span><span class="sxs-lookup"><span data-stu-id="d5c1e-238">The query then sends the result to the output that you defined (blob storage).</span></span>

1. <span data-ttu-id="d5c1e-239">Vraťte se do okna Přehled úlohy.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-239">Return to the job overview blade.</span></span>

2.  <span data-ttu-id="d5c1e-240">V části **úlohy topologie**, klikněte na tlačítko **dotazu** pole.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-240">Under **Job Topology**, click the **Query** box.</span></span>

    ![Vytvoření dotazu úlohy streamování Analytics](./media/stream-analytics-machine-learning-integration-tutorial/create-query.png)  

3. <span data-ttu-id="d5c1e-242">Zadejte následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="d5c1e-242">Enter the following query:</span></span>

    ```
    WITH sentiment AS (  
    SELECT text, sentiment(text) as result from datainput  
    )  

    Select text, result.[Score]  
    Into datamloutput
    From sentiment  
    ```    

    <span data-ttu-id="d5c1e-243">Dotaz, vyvolá funkce, které jste vytvořili dříve (`sentiment`) Chcete-li provést analýzu postojích na každý tweet ve vstupu.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-243">The query invokes the function you created earlier (`sentiment`) in order to perform sentiment analysis on each tweet in the input.</span></span> 

4. <span data-ttu-id="d5c1e-244">Klikněte na tlačítko **Uložit** Uložte dotaz.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-244">Click **Save** to save the query.</span></span>


## <a name="start-the-stream-analytics-job-and-check-the-output"></a><span data-ttu-id="d5c1e-245">Spustit úlohu služby Stream Analytics a zkontrolujte výstup</span><span class="sxs-lookup"><span data-stu-id="d5c1e-245">Start the Stream Analytics job and check the output</span></span>

<span data-ttu-id="d5c1e-246">Nyní můžete spustit úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-246">You can now start the Stream Analytics job.</span></span>

### <a name="start-the-job"></a><span data-ttu-id="d5c1e-247">Spustit úlohu</span><span class="sxs-lookup"><span data-stu-id="d5c1e-247">Start the job</span></span>
1. <span data-ttu-id="d5c1e-248">Vraťte se do okna Přehled úlohy.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-248">Return to the job overview blade.</span></span>

2. <span data-ttu-id="d5c1e-249">Klikněte na tlačítko **spustit** v horní části okna.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-249">Click **Start** at the top of the blade.</span></span>

    ![Vytvoření dotazu úlohy streamování Analytics](./media/stream-analytics-machine-learning-integration-tutorial/start-job.png)  

3. <span data-ttu-id="d5c1e-251">V **spuštění úlohy**, vyberte **vlastní**a pak vyberte jeden den před při nahrát soubor CSV do úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-251">In the **Start job**, select **Custom**, and then select one day prior to when you uploaded the CSV file to blob storage.</span></span> <span data-ttu-id="d5c1e-252">Když jste hotovi, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-252">When you're done, click **Start**.</span></span>  


### <a name="check-the-output"></a><span data-ttu-id="d5c1e-253">Zkontrolujte výstup</span><span class="sxs-lookup"><span data-stu-id="d5c1e-253">Check the output</span></span>
1. <span data-ttu-id="d5c1e-254">Umožní má úloha spustit několik minut, dokud neuvidíte aktivity v **monitorování** pole.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-254">Let the job run for a few minutes until you see activity in the **Monitoring** box.</span></span> 

2. <span data-ttu-id="d5c1e-255">Pokud máte nástroj, který se standardně používáte k ověřit obsah úložiště objektů blob, že kontrola pomocí nástroje `azuresamldemoblob` kontejneru.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-255">If you have a tool that you normally use to examine the contents of blob storage, use that tool to examine the `azuresamldemoblob` container.</span></span> <span data-ttu-id="d5c1e-256">Případně proveďte následující kroky na portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="d5c1e-256">Alternatively, do the following steps in the Azure portal:</span></span>

    1. <span data-ttu-id="d5c1e-257">V portálu, najdete `samldemo` úložiště účet a v rámci účtu, najděte `azuresamldemoblob` kontejneru.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-257">In the portal, find the `samldemo` storage account, and within the account, find the `azuresamldemoblob` container.</span></span> <span data-ttu-id="d5c1e-258">Zobrazí dva soubory v kontejneru: soubor, který obsahuje ukázkové tweetů a soubor CSV generované úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-258">You see two files in the container: the file that contains the sample tweets and a CSV file generated by the Stream Analytics job.</span></span>
    2. <span data-ttu-id="d5c1e-259">Klikněte pravým tlačítkem na vygenerovaný soubor a pak vyberte **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-259">Right-click the generated file and then select **Download**.</span></span> 

   ![Stáhněte si výstup úlohy CSV z úložiště objektů Blob](./media/stream-analytics-machine-learning-integration-tutorial/download-output-csv-file.png)  

3. <span data-ttu-id="d5c1e-261">Otevřete generovaný soubor CSV.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-261">Open the generated CSV file.</span></span> <span data-ttu-id="d5c1e-262">Vidět něco podobného jako v následujícím příkladu:</span><span class="sxs-lookup"><span data-stu-id="d5c1e-262">You see something like the following example:</span></span>  
   
   ![Stream Analytics Machine Learning, CSV zobrazení](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  


### <a name="view-metrics"></a><span data-ttu-id="d5c1e-264">Zobrazit metriky</span><span class="sxs-lookup"><span data-stu-id="d5c1e-264">View metrics</span></span>
<span data-ttu-id="d5c1e-265">Také můžete zobrazit související funkce metrik Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-265">You also can view Azure Machine Learning function-related metrics.</span></span> <span data-ttu-id="d5c1e-266">Následující metriky funkce související se zobrazují v **monitorování** pole v okně úlohy:</span><span class="sxs-lookup"><span data-stu-id="d5c1e-266">The following function-related metrics are displayed in the **Monitoring** box in the job blade:</span></span>

* <span data-ttu-id="d5c1e-267">**Funkce požadavky** označuje počet požadavky odeslané na webové službě Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-267">**Function Requests** indicates the number of requests sent to a Machine Learning web service.</span></span>  
* <span data-ttu-id="d5c1e-268">**Funkce události** označuje počet událostí v požadavku.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-268">**Function Events** indicates the number of events in the request.</span></span> <span data-ttu-id="d5c1e-269">Každý požadavek k webové službě Machine Learning obsahuje ve výchozím nastavení až 1 000 událostí.</span><span class="sxs-lookup"><span data-stu-id="d5c1e-269">By default, each request to a Machine Learning web service contains up to 1,000 events.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="d5c1e-270">Další kroky</span><span class="sxs-lookup"><span data-stu-id="d5c1e-270">Next steps</span></span>

* [<span data-ttu-id="d5c1e-271">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d5c1e-271">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="d5c1e-272">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="d5c1e-272">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="d5c1e-273">Integrace rozhraní REST API a strojového učení</span><span class="sxs-lookup"><span data-stu-id="d5c1e-273">Integrate REST API and Machine Learning</span></span>](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)
* [<span data-ttu-id="d5c1e-274">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="d5c1e-274">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)



