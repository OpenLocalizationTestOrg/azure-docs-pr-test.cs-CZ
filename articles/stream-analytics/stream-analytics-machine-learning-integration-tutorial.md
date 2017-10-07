---
title: aaaAzure integrace Stream Analytics a Machine Learning | Microsoft Docs
description: "Jak toouse uživatelsky definované funkce a Machine Learning v úloze Stream Analytics"
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
ms.openlocfilehash: e1ba7ab51ece80719839793e1320a7666cfc4181
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="performing-sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a><span data-ttu-id="8114a-103">Provedení analýzy postojích pomocí Azure Stream Analytics nebo Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="8114a-103">Performing sentiment analysis by using Azure Stream Analytics and Azure Machine Learning</span></span>
<span data-ttu-id="8114a-104">Tento článek popisuje, jak tooquickly nastavit jednoduchou úlohu Azure Stream Analytics, která integruje Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="8114a-104">This article describes how tooquickly set up a simple Azure Stream Analytics job that integrates Azure Machine Learning.</span></span> <span data-ttu-id="8114a-105">Použijte model Machine Learning postojích analytics z hello Cortana Intelligence Gallery tooanalyze streamování textová data a určení skóre postojích hello v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="8114a-105">You use a Machine Learning sentiment analytics model from hello Cortana Intelligence Gallery tooanalyze streaming text data and determine hello sentiment score in real time.</span></span> <span data-ttu-id="8114a-106">Pomocí hello Cortana Intelligence Suite umožňuje provedení této úlohy bez obav o rozbor všech hello vytváření model postojích analytics.</span><span class="sxs-lookup"><span data-stu-id="8114a-106">Using hello Cortana Intelligence Suite lets you accomplish this task without worrying about hello intricacies of building a sentiment analytics model.</span></span>

<span data-ttu-id="8114a-107">Můžete použít informace z tohoto článku tooscenarios například tyto:</span><span class="sxs-lookup"><span data-stu-id="8114a-107">You can apply what you learn from this article tooscenarios such as these:</span></span>

* <span data-ttu-id="8114a-108">Analýza v reálném čase postojích na streamování dat Twitteru.</span><span class="sxs-lookup"><span data-stu-id="8114a-108">Analyzing real-time sentiment on streaming Twitter data.</span></span>
* <span data-ttu-id="8114a-109">Analýza záznamy zákazníka konverzace s pracovníky technické podpory.</span><span class="sxs-lookup"><span data-stu-id="8114a-109">Analyzing records of customer chats with support staff.</span></span>
* <span data-ttu-id="8114a-110">Vyhodnocení komentáře na fóra, blogy a videa.</span><span class="sxs-lookup"><span data-stu-id="8114a-110">Evaluating comments on forums, blogs, and videos.</span></span> 
* <span data-ttu-id="8114a-111">Mnoho dalších v reálném čase, prediktivní vyhodnocování scénářů.</span><span class="sxs-lookup"><span data-stu-id="8114a-111">Many other real-time, predictive scoring scenarios.</span></span>

<span data-ttu-id="8114a-112">Ve scénáři reálného by získat hello data přímo z datového proudu Twitter.</span><span class="sxs-lookup"><span data-stu-id="8114a-112">In a real-world scenario, you would get hello data directly from a Twitter data stream.</span></span> <span data-ttu-id="8114a-113">toosimplify hello kurzu jsme jste jej tak, aby hello úlohy streamování Analytics získá tweetů ze souboru CSV v úložišti objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="8114a-113">toosimplify hello tutorial, we've written it so that hello Streaming Analytics job gets tweets from a CSV file in Azure Blob storage.</span></span> <span data-ttu-id="8114a-114">Můžete vytvořit svůj vlastní soubor CSV, nebo můžete použít ukázkový soubor CSV, jak ukazuje následující obrázek hello:</span><span class="sxs-lookup"><span data-stu-id="8114a-114">You can create your own CSV file, or you can use a sample CSV file, as shown in hello following image:</span></span>

![Ukázka tweetů v souboru CSV](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

<span data-ttu-id="8114a-116">úlohy streamování Analytics Hello, který vytvoříte platí model analýzy postojích hello jako uživatelem definované funkce (UDF) na hello ukázka textová data z úložiště objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-116">hello Streaming Analytics job that you create applies hello sentiment analytics model as a user-defined function (UDF) on hello sample text data from hello blob store.</span></span> <span data-ttu-id="8114a-117">výstup Hello (hello výsledek analýzy postojích hello) je zapsán toohello stejné úložiště objektů blob v jiném souboru CSV.</span><span class="sxs-lookup"><span data-stu-id="8114a-117">hello output (hello result of hello sentiment analysis) is written toohello same blob store in a different CSV file.</span></span> 

<span data-ttu-id="8114a-118">Hello následující obrázek znázorňuje tuto konfiguraci.</span><span class="sxs-lookup"><span data-stu-id="8114a-118">hello following figure demonstrates this configuration.</span></span> <span data-ttu-id="8114a-119">Jak jsme uvedli, realističtější scénáři můžete nahradit úložiště objektů blob streamování Twitter dat z Azure Event Hubs vstup.</span><span class="sxs-lookup"><span data-stu-id="8114a-119">As noted, for a more realistic scenario, you can replace blob storage with streaming Twitter data from an Azure Event Hubs input.</span></span> <span data-ttu-id="8114a-120">Kromě toho je sestavení [Microsoft Power BI](https://powerbi.microsoft.com/) v reálném čase vizualizace agregační postojích hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-120">Additionally, you could build a [Microsoft Power BI](https://powerbi.microsoft.com/) real-time visualization of hello aggregate sentiment.</span></span>    

![Přehled integrace Stream Analytics Machine Learning](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a><span data-ttu-id="8114a-122">Požadavky</span><span class="sxs-lookup"><span data-stu-id="8114a-122">Prerequisites</span></span>
<span data-ttu-id="8114a-123">Než začnete, ujistěte se, že máte hello následující:</span><span class="sxs-lookup"><span data-stu-id="8114a-123">Before you start, make sure you have hello following:</span></span>

* <span data-ttu-id="8114a-124">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="8114a-124">An active Azure subscription.</span></span>
* <span data-ttu-id="8114a-125">Soubor CSV s některá data v ní.</span><span class="sxs-lookup"><span data-stu-id="8114a-125">A CSV file with some data in it.</span></span> <span data-ttu-id="8114a-126">Si můžete stáhnout soubor hello uvedena výše z [Githubu](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), nebo můžete vytvořit svůj vlastní soubor.</span><span class="sxs-lookup"><span data-stu-id="8114a-126">You can download hello file shown earlier from [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), or you can create your own file.</span></span> <span data-ttu-id="8114a-127">V tomto článku předpokládáme, že používáte hello souboru z Githubu.</span><span class="sxs-lookup"><span data-stu-id="8114a-127">For this article, we assume that you're using hello file from GitHub.</span></span>

<span data-ttu-id="8114a-128">Na vysoké úrovni toocomplete hello úlohy ukázáno v tomto článku jste hello následující:</span><span class="sxs-lookup"><span data-stu-id="8114a-128">At a high level, toocomplete hello tasks demonstrated in this article, you do hello following:</span></span>

1. <span data-ttu-id="8114a-129">Vytvořit účet úložiště Azure a kontejner úložiště objektů blob a nahrajte kontejner toohello vstupní soubor ve formátu CSV.</span><span class="sxs-lookup"><span data-stu-id="8114a-129">Create an Azure storage account and a blob storage container, and upload a CSV-formatted input file toohello container.</span></span>
3. <span data-ttu-id="8114a-130">Přidání modelu analytics postojích z pracovního prostoru hello Cortana Intelligence Gallery tooyour Azure Machine Learning a nasadit tento model jako webovou službu v pracovního prostoru Machine Learning hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-130">Add a sentiment analytics model from hello Cortana Intelligence Gallery tooyour Azure Machine Learning workspace and deploy this model as a web service in hello Machine Learning workspace.</span></span>
5. <span data-ttu-id="8114a-131">Vytvořte úlohu služby Stream Analytics, který volání této webové služby jako funkci v pořadí toodetermine postojích pro zadávání textu hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-131">Create a Stream Analytics job that calls this web service as a function in order toodetermine sentiment for hello text input.</span></span>
6. <span data-ttu-id="8114a-132">Spuštění úlohy Stream Analytics hello a zkontrolujte výstup hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-132">Start hello Stream Analytics job and check hello output.</span></span>

## <a name="create-a-storage-container-and-upload-hello-csv-input-file"></a><span data-ttu-id="8114a-133">Vytvořit kontejner úložiště a odeslat vstupní soubor CSV hello</span><span class="sxs-lookup"><span data-stu-id="8114a-133">Create a storage container and upload hello CSV input file</span></span>
<span data-ttu-id="8114a-134">Pro tento krok můžete použít všechny souboru CSV, jako je například hello nějaký k dispozici z Githubu.</span><span class="sxs-lookup"><span data-stu-id="8114a-134">For this step, you can use any CSV file, such as hello one available from GitHub.</span></span>

1. <span data-ttu-id="8114a-135">V hello portálu Azure, klikněte na **nový** &gt; **úložiště** &gt; **účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="8114a-135">In hello Azure portal, click **New** &gt; **Storage** &gt; **Storage account**.</span></span>

   ![Vytvořit nový účet úložiště](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-create-storage-account.png)

2. <span data-ttu-id="8114a-137">Zadejte název (`samldemo` v příkladu hello).</span><span class="sxs-lookup"><span data-stu-id="8114a-137">Provide a name (`samldemo` in hello example).</span></span> <span data-ttu-id="8114a-138">v názvu Hello lze použít pouze malá písmena a čísla a musí být jedinečný v Azure.</span><span class="sxs-lookup"><span data-stu-id="8114a-138">hello name can use only lowercase letters and numbers, and it must be unique across Azure.</span></span> 

3. <span data-ttu-id="8114a-139">Zadejte existující skupinu prostředků a zadejte umístění.</span><span class="sxs-lookup"><span data-stu-id="8114a-139">Specify an existing resource group and specify a location.</span></span> <span data-ttu-id="8114a-140">Pro umístění, doporučujeme, aby všechny prostředky hello vytvořené v tomto kurzu hello stejné umístění.</span><span class="sxs-lookup"><span data-stu-id="8114a-140">For location, we recommend that all hello resources created in this tutorial use hello same location.</span></span>

    ![Zadejte podrobnosti o účtu úložiště](./media/stream-analytics-machine-learning-integration-tutorial/create-sa1.png)

4. <span data-ttu-id="8114a-142">V hello portálu Azure vyberte účet úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-142">In hello Azure portal, select hello storage account.</span></span> <span data-ttu-id="8114a-143">V okně účtu úložiště hello, klikněte na tlačítko **kontejnery** a pak klikněte na  **+ &nbsp;kontejneru** toocreate úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="8114a-143">In hello storage account blade, click **Containers** and then click **+&nbsp;Container** toocreate blob storage.</span></span>

    ![Vytvoření kontejneru objektů blob](./media/stream-analytics-machine-learning-integration-tutorial/create-sa2.png)

5. <span data-ttu-id="8114a-145">Zadejte název pro kontejner hello (`azuresamldemoblob` v příkladu hello) a ověřte, že **přistupovat typu** je nastaven příliš**Blob**.</span><span class="sxs-lookup"><span data-stu-id="8114a-145">Provide a name for hello container (`azuresamldemoblob` in hello example) and verify that **Access type** is set too**Blob**.</span></span> <span data-ttu-id="8114a-146">Až to budete mít, klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="8114a-146">When you're done, click **OK**.</span></span>

    ![Zadejte podrobnosti kontejner objektů blob](./media/stream-analytics-machine-learning-integration-tutorial/create-sa3.png)

6. <span data-ttu-id="8114a-148">V hello **kontejnery** okně, vyberte hello nový kontejner, což otevře okno hello pro tento kontejner.</span><span class="sxs-lookup"><span data-stu-id="8114a-148">In hello **Containers** blade, select hello new container, which opens hello blade for that container.</span></span>

7. <span data-ttu-id="8114a-149">Klikněte na **Odeslat**.</span><span class="sxs-lookup"><span data-stu-id="8114a-149">Click **Upload**.</span></span>

    ![Nahrát tlačítko pro kontejner](./media/stream-analytics-machine-learning-integration-tutorial/create-sa-upload-button.png)

8. <span data-ttu-id="8114a-151">V hello **nahrávání blob** okno, zadejte soubor CSV hello chcete toouse pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="8114a-151">In hello **Upload blob** blade, specify hello CSV file that you want toouse for this tutorial.</span></span> <span data-ttu-id="8114a-152">Pro **Blob typ**, vyberte **objekt blob bloku** a zároveň se zablokují hello sadu velikost too4 MB, který je dostatečný pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="8114a-152">For **Blob type**, select **Block blob** and set hello block size too4 MB, which is sufficient for this tutorial.</span></span>

    ![Nahrát soubor blob](./media/stream-analytics-machine-learning-integration-tutorial/create-sa4.png)

9. <span data-ttu-id="8114a-154">Klikněte na tlačítko hello **nahrát** tlačítko v hello dolní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-154">Click hello **Upload** button at hello bottom of hello blade.</span></span>

## <a name="add-hello-sentiment-analytics-model-from-hello-cortana-intelligence-gallery"></a><span data-ttu-id="8114a-155">Přidat model analýzy postojích hello z hello Cortana Intelligence Gallery</span><span class="sxs-lookup"><span data-stu-id="8114a-155">Add hello sentiment analytics model from hello Cortana Intelligence Gallery</span></span>

<span data-ttu-id="8114a-156">Teď, když je hello ukázková data do objektu BLOB, můžete povolit model analýzy postojích hello v Cortana Intelligence Gallery.</span><span class="sxs-lookup"><span data-stu-id="8114a-156">Now that hello sample data is in a blob, you can enable hello sentiment analysis model in Cortana Intelligence Gallery.</span></span>

1. <span data-ttu-id="8114a-157">Přejděte toohello [model postojích prediktivní analýzy](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) stránku hello Cortana Intelligence Gallery.</span><span class="sxs-lookup"><span data-stu-id="8114a-157">Go toohello [predictive sentiment analytics model](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) page in hello Cortana Intelligence Gallery.</span></span>  

2. <span data-ttu-id="8114a-158">Klikněte na tlačítko **Open in Studio**.</span><span class="sxs-lookup"><span data-stu-id="8114a-158">Click **Open in Studio**.</span></span>  
   
   ![Stream Analytics Machine Learning, otevřete Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. <span data-ttu-id="8114a-160">Přihlaste se toogo toohello prostoru.</span><span class="sxs-lookup"><span data-stu-id="8114a-160">Sign in toogo toohello workspace.</span></span> <span data-ttu-id="8114a-161">Vyberte umístění.</span><span class="sxs-lookup"><span data-stu-id="8114a-161">Select a location.</span></span>

4. <span data-ttu-id="8114a-162">Klikněte na tlačítko **spustit** v hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-162">Click **Run** at hello bottom of hello page.</span></span> <span data-ttu-id="8114a-163">Hello proces spouští, což trvá několik minut.</span><span class="sxs-lookup"><span data-stu-id="8114a-163">hello process runs, which takes about a minute.</span></span>

   ![Spusťte experiment v nástroji Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-run-experiment.png)  

5. <span data-ttu-id="8114a-165">Jakmile proces hello proběhl úspěšně, vyberte **nasazení webové služby** na hello dolní části stránky hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-165">After hello process has run successfully, select **Deploy Web Service** at hello bottom of hello page.</span></span>

   ![experiment v nástroji Machine Learning Studio nasaďte jako webovou službu](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-deploy-web-service.png)  

6. <span data-ttu-id="8114a-167">toovalidate, který hello postojích model analýzy je připraven toouse, klikněte na tlačítko hello **Test** tlačítko.</span><span class="sxs-lookup"><span data-stu-id="8114a-167">toovalidate that hello sentiment analytics model is ready toouse, click hello **Test** button.</span></span> <span data-ttu-id="8114a-168">Zadejte textový vstup, jako je "I rádi Microsoft".</span><span class="sxs-lookup"><span data-stu-id="8114a-168">Provide text input such as "I love Microsoft".</span></span> 

   ![Test experimentu v nástroji Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test.png)  

    <span data-ttu-id="8114a-170">Pokud testovací hello funguje, zobrazí výsledek podobné toohello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8114a-170">If hello test works, you see a result similar toohello following example:</span></span>

   ![výsledky testu v nástroji Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test-results.png)  

7. <span data-ttu-id="8114a-172">V hello **aplikace** sloupce, klikněte na tlačítko hello **Excel 2010 nebo starší sešitu** odkaz toodownload sešitu aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="8114a-172">In hello **Apps** column, click hello **Excel 2010 or earlier workbook** link toodownload an Excel workbook.</span></span> <span data-ttu-id="8114a-173">Hello sešit obsahuje klíč hello rozhraní API a adresy URL hello, je nutné, aby novější tooset do úlohy Stream Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-173">hello workbook contains hello an API key and hello URL that you need later tooset up hello Stream Analytics job.</span></span>

    ![Stream Analytics Machine Learning, rychlého přehledu.](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  


## <a name="create-a-stream-analytics-job-that-uses-hello-machine-learning-model"></a><span data-ttu-id="8114a-175">Vytvořit úlohu služby Stream Analytics, který používá model Machine Learning hello</span><span class="sxs-lookup"><span data-stu-id="8114a-175">Create a Stream Analytics job that uses hello Machine Learning model</span></span>

<span data-ttu-id="8114a-176">Nyní můžete vytvořit úlohu Stream Analytics, která načte hello ukázka tweetů ze souboru CSV hello v úložišti objektů blob.</span><span class="sxs-lookup"><span data-stu-id="8114a-176">You can now create a Stream Analytics job that reads hello sample tweets from hello CSV file in blob storage.</span></span> 

### <a name="create-hello-job"></a><span data-ttu-id="8114a-177">Vytvořit úlohu hello</span><span class="sxs-lookup"><span data-stu-id="8114a-177">Create hello job</span></span>

1. <span data-ttu-id="8114a-178">Přejděte toohello [portál Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="8114a-178">Go toohello [Azure portal](https://portal.azure.com).</span></span>  

2. <span data-ttu-id="8114a-179">Klikněte na tlačítko **nový** > **Internet věcí** > **úlohy služby Stream Analytics**.</span><span class="sxs-lookup"><span data-stu-id="8114a-179">Click **New** > **Internet of Things** > **Stream Analytics job**.</span></span> 

   ![Cesty Azure portálu pro získání tooa nová úloha Stream Analytics](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-new-iot-sa-job.png)
   
3. <span data-ttu-id="8114a-181">Název úlohy hello `azure-sa-ml-demo`, zadejte předplatné, zadejte existující skupinu prostředků nebo vytvořte novou a vyberte umístění hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="8114a-181">Name hello job `azure-sa-ml-demo`, specify a subscription, specify an existing resource group or create a new one, and select hello location for hello job.</span></span>

   ![Zadejte nastavení pro nové úlohy Stream Analytics](./media/stream-analytics-machine-learning-integration-tutorial/create-job-1.png)
   

### <a name="configure-hello-job-input"></a><span data-ttu-id="8114a-183">Konfigurace vstupu úlohy hello</span><span class="sxs-lookup"><span data-stu-id="8114a-183">Configure hello job input</span></span>
<span data-ttu-id="8114a-184">Úloha Hello získá vstupní ze souboru CSV hello, který jste nahráli starší tooblob úložiště.</span><span class="sxs-lookup"><span data-stu-id="8114a-184">hello job gets its input from hello CSV file that you uploaded earlier tooblob storage.</span></span>

1. <span data-ttu-id="8114a-185">Po hello byla vytvořena úloha, v části **úlohy topologie** v okně úlohy hello, klikněte na tlačítko hello **vstupy** pole.</span><span class="sxs-lookup"><span data-stu-id="8114a-185">After hello job has been created, under **Job Topology** in hello job blade, click hello **Inputs** box.</span></span>  
   
   !["Vstupy" pole v okně úlohy Stream Analytics](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input.png)  

2. <span data-ttu-id="8114a-187">V hello **vstupy** okně klikněte na tlačítko **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="8114a-187">In hello **Inputs** blade, click **+ Add**.</span></span>

   ![Přidání tlačítka pro přidání úlohu služby Stream Analytics vstupní toohello](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input-button.png)  

3. <span data-ttu-id="8114a-189">Vyplňte hello **nové vstup** okno s těmito hodnotami:</span><span class="sxs-lookup"><span data-stu-id="8114a-189">Fill out hello **New input** blade with these values:</span></span>

    * <span data-ttu-id="8114a-190">**Vstupní alias**: použití hello název `datainput`.</span><span class="sxs-lookup"><span data-stu-id="8114a-190">**Input alias**: Use hello name `datainput`.</span></span>
    * <span data-ttu-id="8114a-191">**Typ zdroje**: vyberte **datový proud**.</span><span class="sxs-lookup"><span data-stu-id="8114a-191">**Source type**: Select **Data stream**.</span></span>
    * <span data-ttu-id="8114a-192">**Zdroj**: vyberte **úložiště objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="8114a-192">**Source**: Select **Blob storage**.</span></span>
    * <span data-ttu-id="8114a-193">**Import možnost**: vyberte **pomocí úložiště objektů blob z aktuálního předplatného**.</span><span class="sxs-lookup"><span data-stu-id="8114a-193">**Import option**: Select **Use blob storage from current subscription**.</span></span> 
    * <span data-ttu-id="8114a-194">**Účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="8114a-194">**Storage account**.</span></span> <span data-ttu-id="8114a-195">Vyberte účet úložiště hello, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="8114a-195">Select hello storage account you created earlier.</span></span>
    * <span data-ttu-id="8114a-196">**Kontejner**.</span><span class="sxs-lookup"><span data-stu-id="8114a-196">**Container**.</span></span> <span data-ttu-id="8114a-197">Vyberte hello kontejneru, které jste vytvořili dříve (`azuresamldemoblob`).</span><span class="sxs-lookup"><span data-stu-id="8114a-197">Select hello container you created earlier (`azuresamldemoblob`).</span></span>
    * <span data-ttu-id="8114a-198">**Formát serializace událostí**.</span><span class="sxs-lookup"><span data-stu-id="8114a-198">**Event serialization format**.</span></span> <span data-ttu-id="8114a-199">Vyberte **CSV**.</span><span class="sxs-lookup"><span data-stu-id="8114a-199">Select **CSV**.</span></span>

    ![Nastavení pro nové úlohy vstup](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-create-sa-input-new-portal.png)

4. <span data-ttu-id="8114a-201">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8114a-201">Click **Create**.</span></span>

### <a name="configure-hello-job-output"></a><span data-ttu-id="8114a-202">Konfigurace hello výstup úlohy</span><span class="sxs-lookup"><span data-stu-id="8114a-202">Configure hello job output</span></span>
<span data-ttu-id="8114a-203">toohello výsledky odešle úlohu Hello stejný objekt blob úložiště, kde získá vstup.</span><span class="sxs-lookup"><span data-stu-id="8114a-203">hello job sends results toohello same blob storage where it gets input.</span></span> 

1. <span data-ttu-id="8114a-204">V části **úlohy topologie** v okně úlohy hello, klikněte na tlačítko hello **výstupy** pole.</span><span class="sxs-lookup"><span data-stu-id="8114a-204">Under **Job Topology** in hello job blade, click hello **Outputs** box.</span></span>  
  
   ![Vytvořit nový výstupní úlohy streamování Analytics](./media/stream-analytics-machine-learning-integration-tutorial/create-output.png)  

2. <span data-ttu-id="8114a-206">V hello **výstupy** okně klikněte na tlačítko **+ přidat**a poté přidejte výstup s aliasem hello `datamloutput`.</span><span class="sxs-lookup"><span data-stu-id="8114a-206">In hello **Outputs** blade, click **+ Add**, and then add an output with hello alias `datamloutput`.</span></span> 

3. <span data-ttu-id="8114a-207">Pro **jímky**, vyberte **úložiště objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="8114a-207">For **Sink**, select **Blob storage**.</span></span> <span data-ttu-id="8114a-208">Potom vyplňte hello zbytek hello výstup hello nastavení pomocí stejné hodnoty, které jste použili pro hello úložiště objektů blob pro vstup:</span><span class="sxs-lookup"><span data-stu-id="8114a-208">Then fill in hello rest of hello output settings using hello same values that you used for hello blob storage for input:</span></span>

    * <span data-ttu-id="8114a-209">**Účet úložiště**.</span><span class="sxs-lookup"><span data-stu-id="8114a-209">**Storage account**.</span></span> <span data-ttu-id="8114a-210">Vyberte účet úložiště hello, které jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="8114a-210">Select hello storage account you created earlier.</span></span>
    * <span data-ttu-id="8114a-211">**Kontejner**.</span><span class="sxs-lookup"><span data-stu-id="8114a-211">**Container**.</span></span> <span data-ttu-id="8114a-212">Vyberte hello kontejneru, které jste vytvořili dříve (`azuresamldemoblob`).</span><span class="sxs-lookup"><span data-stu-id="8114a-212">Select hello container you created earlier (`azuresamldemoblob`).</span></span>
    * <span data-ttu-id="8114a-213">**Formát serializace událostí**.</span><span class="sxs-lookup"><span data-stu-id="8114a-213">**Event serialization format**.</span></span> <span data-ttu-id="8114a-214">Vyberte **CSV**.</span><span class="sxs-lookup"><span data-stu-id="8114a-214">Select **CSV**.</span></span>

   ![Nastavení pro nové výstup úlohy](./media/stream-analytics-machine-learning-integration-tutorial/create-output2.png) 

4. <span data-ttu-id="8114a-216">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8114a-216">Click **Create**.</span></span>   


### <a name="add-hello-machine-learning-function"></a><span data-ttu-id="8114a-217">Přidání funkce Machine Learning hello</span><span class="sxs-lookup"><span data-stu-id="8114a-217">Add hello Machine Learning function</span></span> 
<span data-ttu-id="8114a-218">Dříve jste publikovali webovou službu tooa model Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="8114a-218">Earlier you published a Machine Learning model tooa web service.</span></span> <span data-ttu-id="8114a-219">V tomto scénáři při spuštění úlohy analýzy datového proudu hello odešle každý ukázka tweet z hello vstupní toohello webové služby pro analýzu postojích.</span><span class="sxs-lookup"><span data-stu-id="8114a-219">In our scenario, when hello Stream Analysis job runs, it sends each sample tweet from hello input toohello web service for sentiment analysis.</span></span> <span data-ttu-id="8114a-220">Vrátí Hello webové službě Machine Learning postojích (`positive`, `neutral`, nebo `negative`) a pravděpodobnost vzniku tweet hello se kladné.</span><span class="sxs-lookup"><span data-stu-id="8114a-220">hello Machine Learning web service returns a sentiment (`positive`, `neutral`, or `negative`) and a probability of hello tweet being positive.</span></span> 

<span data-ttu-id="8114a-221">V této části kurzu hello definujete funkce v hello Úloha analýzy datového proudu.</span><span class="sxs-lookup"><span data-stu-id="8114a-221">In this section of hello tutorial, you define a function in hello Stream Analysis job.</span></span> <span data-ttu-id="8114a-222">Funkce Hello můžete být vyvolaná toosend tweet toohello webové služby a získat odpovědi hello zpět.</span><span class="sxs-lookup"><span data-stu-id="8114a-222">hello function can be invoked toosend a tweet toohello web service and get hello response back.</span></span> 

1. <span data-ttu-id="8114a-223">Ujistěte se, že máte hello webové služby adresy URL a rozhraní API klíč, který jste si stáhli dříve v sešitu aplikace Excel hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-223">Make sure you have hello web service URL and API key that you downloaded earlier in hello Excel workbook.</span></span>

2. <span data-ttu-id="8114a-224">Vrátí okno Přehled toohello úlohy.</span><span class="sxs-lookup"><span data-stu-id="8114a-224">Return toohello job overview blade.</span></span>

3. <span data-ttu-id="8114a-225">V části **nastavení**, vyberte **funkce** a pak klikněte na **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="8114a-225">Under **Settings**, select **Functions** and then click **+ Add**.</span></span>

   ![Přidat úloha Stream Analytics toohello – funkce](./media/stream-analytics-machine-learning-integration-tutorial/create-function1.png) 

4. <span data-ttu-id="8114a-227">Zadejte `sentiment` jako hello funkce alias a vyplňte hello zbytek hello okno pomocí těchto hodnot:</span><span class="sxs-lookup"><span data-stu-id="8114a-227">Enter `sentiment` as hello function alias and fill out hello rest of hello blade using these values:</span></span>

    * <span data-ttu-id="8114a-228">**Typ funkce**: vyberte **Azure ML**.</span><span class="sxs-lookup"><span data-stu-id="8114a-228">**Function type**: Select **Azure ML**.</span></span>
    * <span data-ttu-id="8114a-229">**Import možnost**: vyberte **Import z jiného předplatného**.</span><span class="sxs-lookup"><span data-stu-id="8114a-229">**Import option**: Select **Import from a different subscription**.</span></span> <span data-ttu-id="8114a-230">To vám dává možnost tooenter hello URL a klíč.</span><span class="sxs-lookup"><span data-stu-id="8114a-230">This gives you a chance tooenter hello URL and key.</span></span>
    * <span data-ttu-id="8114a-231">**Adresa URL**: vložte adresu URL webové služby hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-231">**URL**: Paste in hello web service URL.</span></span>
    * <span data-ttu-id="8114a-232">**Klíč**: vložte klíč rozhraní API hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-232">**Key**: Paste in hello API key.</span></span>
  
    ![Nastavení pro přidání úloha Stream Analytics toohello funkce Machine Learning](./media/stream-analytics-machine-learning-integration-tutorial/add-function.png)  
    
5. <span data-ttu-id="8114a-234">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="8114a-234">Click **Create**.</span></span>

### <a name="create-a-query-tootransform-hello-data"></a><span data-ttu-id="8114a-235">Vytvoření dotazu tootransform hello dat</span><span class="sxs-lookup"><span data-stu-id="8114a-235">Create a query tootransform hello data</span></span>

<span data-ttu-id="8114a-236">Stream Analytics používá deklarativní, na základě SQL dotaz tooexamine hello vstup a zpracovat.</span><span class="sxs-lookup"><span data-stu-id="8114a-236">Stream Analytics uses a declarative, SQL-based query tooexamine hello input and process it.</span></span> <span data-ttu-id="8114a-237">V této části vytvoříte dotaz, který čte každý tweet ze vstupu a potom se vyvolá hello Machine Learning funkce tooperform postojích analýzy.</span><span class="sxs-lookup"><span data-stu-id="8114a-237">In this section, you create a query that reads each tweet from input and then invokes hello Machine Learning function tooperform sentiment analysis.</span></span> <span data-ttu-id="8114a-238">dotaz Hello pak odešle hello výsledek toohello výstup, že jste definovali (úložiště objektů blob).</span><span class="sxs-lookup"><span data-stu-id="8114a-238">hello query then sends hello result toohello output that you defined (blob storage).</span></span>

1. <span data-ttu-id="8114a-239">Vrátí okno Přehled toohello úlohy.</span><span class="sxs-lookup"><span data-stu-id="8114a-239">Return toohello job overview blade.</span></span>

2.  <span data-ttu-id="8114a-240">V části **úlohy topologie**, klikněte na tlačítko hello **dotazu** pole.</span><span class="sxs-lookup"><span data-stu-id="8114a-240">Under **Job Topology**, click hello **Query** box.</span></span>

    ![Vytvoření dotazu úlohy streamování Analytics](./media/stream-analytics-machine-learning-integration-tutorial/create-query.png)  

3. <span data-ttu-id="8114a-242">Zadejte hello následující dotaz:</span><span class="sxs-lookup"><span data-stu-id="8114a-242">Enter hello following query:</span></span>

    ```
    WITH sentiment AS (  
    SELECT text, sentiment(text) as result from datainput  
    )  

    Select text, result.[Score]  
    Into datamloutput
    From sentiment  
    ```    

    <span data-ttu-id="8114a-243">dotaz Hello vyvolá hello funkce, které jste vytvořili dříve (`sentiment`) v pořadí tooperform postojích analýzy na každý tweet ve vstupu hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-243">hello query invokes hello function you created earlier (`sentiment`) in order tooperform sentiment analysis on each tweet in hello input.</span></span> 

4. <span data-ttu-id="8114a-244">Klikněte na tlačítko **Uložit** toosave hello dotazu.</span><span class="sxs-lookup"><span data-stu-id="8114a-244">Click **Save** toosave hello query.</span></span>


## <a name="start-hello-stream-analytics-job-and-check-hello-output"></a><span data-ttu-id="8114a-245">Spuštění úlohy Stream Analytics hello a zkontrolujte výstup hello</span><span class="sxs-lookup"><span data-stu-id="8114a-245">Start hello Stream Analytics job and check hello output</span></span>

<span data-ttu-id="8114a-246">Nyní můžete spustit úlohy služby Stream Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-246">You can now start hello Stream Analytics job.</span></span>

### <a name="start-hello-job"></a><span data-ttu-id="8114a-247">Spuštění úlohy hello</span><span class="sxs-lookup"><span data-stu-id="8114a-247">Start hello job</span></span>
1. <span data-ttu-id="8114a-248">Vrátí okno Přehled toohello úlohy.</span><span class="sxs-lookup"><span data-stu-id="8114a-248">Return toohello job overview blade.</span></span>

2. <span data-ttu-id="8114a-249">Klikněte na tlačítko **spustit** hello horní části okna hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-249">Click **Start** at hello top of hello blade.</span></span>

    ![Vytvoření dotazu úlohy streamování Analytics](./media/stream-analytics-machine-learning-integration-tutorial/start-job.png)  

3. <span data-ttu-id="8114a-251">V hello **spuštění úlohy**, vyberte **vlastní**a pak vyberte předchozí toowhen jeden den, který jste nahráli úložiště tooblob soubor CSV hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-251">In hello **Start job**, select **Custom**, and then select one day prior toowhen you uploaded hello CSV file tooblob storage.</span></span> <span data-ttu-id="8114a-252">Když jste hotovi, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="8114a-252">When you're done, click **Start**.</span></span>  


### <a name="check-hello-output"></a><span data-ttu-id="8114a-253">Zkontrolujte výstup hello</span><span class="sxs-lookup"><span data-stu-id="8114a-253">Check hello output</span></span>
1. <span data-ttu-id="8114a-254">Úlohy umožňují hello spustit několik minut, dokud neuvidíte aktivity v hello **monitorování** pole.</span><span class="sxs-lookup"><span data-stu-id="8114a-254">Let hello job run for a few minutes until you see activity in hello **Monitoring** box.</span></span> 

2. <span data-ttu-id="8114a-255">Pokud máte nástroj normálně používat obsah hello tooexamine úložiště objektů blob, použijte tento nástroj tooexamine hello `azuresamldemoblob` kontejneru.</span><span class="sxs-lookup"><span data-stu-id="8114a-255">If you have a tool that you normally use tooexamine hello contents of blob storage, use that tool tooexamine hello `azuresamldemoblob` container.</span></span> <span data-ttu-id="8114a-256">Alternativně hello, proveďte kroky v hello portálu Azure:</span><span class="sxs-lookup"><span data-stu-id="8114a-256">Alternatively, do hello following steps in hello Azure portal:</span></span>

    1. <span data-ttu-id="8114a-257">Hello portálu, najde hello `samldemo` úložiště účtu a v rámci účtu hello najde hello `azuresamldemoblob` kontejneru.</span><span class="sxs-lookup"><span data-stu-id="8114a-257">In hello portal, find hello `samldemo` storage account, and within hello account, find hello `azuresamldemoblob` container.</span></span> <span data-ttu-id="8114a-258">Zobrazí dva soubory v kontejneru hello: hello soubor, který obsahuje hello ukázka tweetů a soubor CSV generované úlohy služby Stream Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-258">You see two files in hello container: hello file that contains hello sample tweets and a CSV file generated by hello Stream Analytics job.</span></span>
    2. <span data-ttu-id="8114a-259">Klikněte pravým tlačítkem hello vygeneruje soubor a pak vyberte **Stáhnout**.</span><span class="sxs-lookup"><span data-stu-id="8114a-259">Right-click hello generated file and then select **Download**.</span></span> 

   ![Stáhněte si výstup úlohy CSV z úložiště objektů Blob](./media/stream-analytics-machine-learning-integration-tutorial/download-output-csv-file.png)  

3. <span data-ttu-id="8114a-261">Otevřete hello vygeneruje soubor CSV.</span><span class="sxs-lookup"><span data-stu-id="8114a-261">Open hello generated CSV file.</span></span> <span data-ttu-id="8114a-262">Vidět něco podobného jako hello následující ukázka:</span><span class="sxs-lookup"><span data-stu-id="8114a-262">You see something like hello following example:</span></span>  
   
   ![Stream Analytics Machine Learning, CSV zobrazení](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  


### <a name="view-metrics"></a><span data-ttu-id="8114a-264">Zobrazit metriky</span><span class="sxs-lookup"><span data-stu-id="8114a-264">View metrics</span></span>
<span data-ttu-id="8114a-265">Také můžete zobrazit související funkce metrik Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="8114a-265">You also can view Azure Machine Learning function-related metrics.</span></span> <span data-ttu-id="8114a-266">Hello následující metriky funkce související se zobrazují v hello **monitorování** pole v okně úlohy hello:</span><span class="sxs-lookup"><span data-stu-id="8114a-266">hello following function-related metrics are displayed in hello **Monitoring** box in hello job blade:</span></span>

* <span data-ttu-id="8114a-267">**Funkce požadavky** označuje hello počet odeslaných požadavků tooa webové službě Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="8114a-267">**Function Requests** indicates hello number of requests sent tooa Machine Learning web service.</span></span>  
* <span data-ttu-id="8114a-268">**Funkce události** označuje hello počet událostí v žádosti o hello.</span><span class="sxs-lookup"><span data-stu-id="8114a-268">**Function Events** indicates hello number of events in hello request.</span></span> <span data-ttu-id="8114a-269">Ve výchozím nastavení každý tooa žádost webové službě Machine Learning obsahuje too1 000 událostí.</span><span class="sxs-lookup"><span data-stu-id="8114a-269">By default, each request tooa Machine Learning web service contains up too1,000 events.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="8114a-270">Další kroky</span><span class="sxs-lookup"><span data-stu-id="8114a-270">Next steps</span></span>

* [<span data-ttu-id="8114a-271">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8114a-271">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="8114a-272">Referenční příručka k jazyku Azure Stream Analytics Query Language</span><span class="sxs-lookup"><span data-stu-id="8114a-272">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="8114a-273">Integrace rozhraní REST API a strojového učení</span><span class="sxs-lookup"><span data-stu-id="8114a-273">Integrate REST API and Machine Learning</span></span>](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)
* [<span data-ttu-id="8114a-274">Referenční příručka k rozhraní REST API pro správu služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="8114a-274">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)



