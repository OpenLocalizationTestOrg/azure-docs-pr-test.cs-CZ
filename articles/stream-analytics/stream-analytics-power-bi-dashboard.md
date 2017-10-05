---
title: "Řídicí panel Power BI na Azure Stream Analytics | Microsoft Docs"
description: "Použijte v reálném čase streamování řídicí panel Power BI shromažďovat business intelligence a analýze velkých objemů dat z úlohy Stream Analytics."
keywords: "Analýza řídicího panelu, řídicí panel v reálném čase"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: fe8db732-4397-4e58-9313-fec9537aa2ad
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/27/2017
ms.author: jeffstok
ms.openlocfilehash: 874d9b8701a24deb3cc21aa74cb51870155c7c9c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a><span data-ttu-id="ceb15-104">Stream Analytics a Power BI: řídicí panel analýzy v reálném čase pro datový proud</span><span class="sxs-lookup"><span data-stu-id="ceb15-104">Stream Analytics and Power BI: A real-time analytics dashboard for streaming data</span></span>
<span data-ttu-id="ceb15-105">Azure Stream Analytics můžete využít jeden z úvodní nástroje business intelligence [Microsoft Power BI](https://powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="ceb15-105">Azure Stream Analytics enables you to take advantage of one of the leading business intelligence tools, [Microsoft Power BI](https://powerbi.com/).</span></span> <span data-ttu-id="ceb15-106">V tomto článku se dozvíte, jak vytvořit nástroje business intelligence pomocí Power BI jako výstup pro úlohy Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="ceb15-106">In this article, you learn how create business intelligence tools by using Power BI as an output for your Azure Stream Analytics jobs.</span></span> <span data-ttu-id="ceb15-107">Také zjistíte, jak vytvořit a použít v reálném čase řídicí panel.</span><span class="sxs-lookup"><span data-stu-id="ceb15-107">You also learn how to create and use a real-time dashboard.</span></span>

<span data-ttu-id="ceb15-108">Tento článek pokračuje ze služby Stream Analytics [odhalování podvodů v reálném čase](stream-analytics-real-time-fraud-detection.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="ceb15-108">This article continues from the Stream Analytics [real-time fraud detection](stream-analytics-real-time-fraud-detection.md) tutorial.</span></span> <span data-ttu-id="ceb15-109">Staví na pracovní postup vytvořený v tomto kurzu, přičemž přidá Power BI výstup, takže můžete vizualizovat podvodné telefonních hovorů, které byly zjištěny nástrojem úlohu streamování Analytics.</span><span class="sxs-lookup"><span data-stu-id="ceb15-109">It builds on the workflow created in that tutorial and adds a Power BI output so that you can visualize fraudulent phone calls that are detected by a Streaming Analytics job.</span></span> 

<span data-ttu-id="ceb15-110">Můžete sledovat [video](https://www.youtube.com/watch?v=SGUpT-a99MA) který znázorňuje tento scénář.</span><span class="sxs-lookup"><span data-stu-id="ceb15-110">You can watch [a video](https://www.youtube.com/watch?v=SGUpT-a99MA)  that illustrates this scenario.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="ceb15-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ceb15-111">Prerequisites</span></span>

<span data-ttu-id="ceb15-112">Než začnete, ujistěte se, že máte následující:</span><span class="sxs-lookup"><span data-stu-id="ceb15-112">Before you start, make sure you have the following:</span></span>

* <span data-ttu-id="ceb15-113">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="ceb15-113">An Azure account.</span></span>
* <span data-ttu-id="ceb15-114">Účet pro Power BI.</span><span class="sxs-lookup"><span data-stu-id="ceb15-114">An account for Power BI.</span></span> <span data-ttu-id="ceb15-115">Můžete použít účet pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="ceb15-115">You can use a work account or a school account.</span></span>
* <span data-ttu-id="ceb15-116">Dokončené verzi [odhalování podvodů v reálném čase](stream-analytics-real-time-fraud-detection.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="ceb15-116">A completed version of the [real-time fraud detection](stream-analytics-real-time-fraud-detection.md) tutorial.</span></span> <span data-ttu-id="ceb15-117">Tento kurz zahrnuje aplikaci, která generuje metadata fiktivní telefonní hovor.</span><span class="sxs-lookup"><span data-stu-id="ceb15-117">The tutorial includes an app that generates fictitious telephone-call metadata.</span></span> <span data-ttu-id="ceb15-118">V tomto kurzu vytvoříte centra událostí a odesílání dat telefonního hovoru do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="ceb15-118">In the tutorial, you create an event hub and send the streaming phone call data to the event hub.</span></span> <span data-ttu-id="ceb15-119">Můžete vytvořit dotaz, který zjistí podvodné volání (volání ze stejného čísla ve stejnou dobu v různých umístěních).</span><span class="sxs-lookup"><span data-stu-id="ceb15-119">You write a query that detects fraudulent calls (calls from the same number at the same time in different locations).</span></span> 


## <a name="add-power-bi-output"></a><span data-ttu-id="ceb15-120">Přidat výstup Power BI</span><span class="sxs-lookup"><span data-stu-id="ceb15-120">Add Power BI output</span></span>
<span data-ttu-id="ceb15-121">V tomto kurzu zjišťování podvodů v reálném čase je výstup odeslán do úložiště objektů Blob Azure.</span><span class="sxs-lookup"><span data-stu-id="ceb15-121">In the real-time fraud detection tutorial, the output is sent to Azure Blob storage.</span></span> <span data-ttu-id="ceb15-122">V této části přidáte výstupu, který odesílá informace do Power BI.</span><span class="sxs-lookup"><span data-stu-id="ceb15-122">In this section, you add an output that sends information to Power BI.</span></span>

1. <span data-ttu-id="ceb15-123">Na portálu Azure otevřete úlohu streamování analýzy, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="ceb15-123">In the Azure portal, open the Streaming Analytics job that you created earlier.</span></span> <span data-ttu-id="ceb15-124">Pokud jste použili navrhovaný název, má název úlohy `sa_frauddetection_job_demo`.</span><span class="sxs-lookup"><span data-stu-id="ceb15-124">If you used the suggested name, the job is named `sa_frauddetection_job_demo`.</span></span>

2. <span data-ttu-id="ceb15-125">Vyberte **výstupy** uprostřed řídicím panelu úlohy a pak vyberte položku **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-125">Select the **Outputs** box in the middle of the job dashboard and then select **+ Add**.</span></span>

3. <span data-ttu-id="ceb15-126">Pro **Alias pro výstup**, zadejte `CallStream-PowerBI`.</span><span class="sxs-lookup"><span data-stu-id="ceb15-126">For **Output Alias**, enter `CallStream-PowerBI`.</span></span> <span data-ttu-id="ceb15-127">Můžete použít jiný název.</span><span class="sxs-lookup"><span data-stu-id="ceb15-127">You can use a different name.</span></span> <span data-ttu-id="ceb15-128">Pokud tak učiníte, poznamenejte si, protože potřebujete název později.</span><span class="sxs-lookup"><span data-stu-id="ceb15-128">If you do, make a note of it, because you need the name later.</span></span> 

4. <span data-ttu-id="ceb15-129">V části **jímky**, vyberte **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-129">Under **Sink**, select **Power BI**.</span></span>

   ![Vytvoření výstupu pro Power BI](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut.png)

5. <span data-ttu-id="ceb15-131">Klikněte na tlačítko **Autorizovat**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-131">Click **Authorize**.</span></span>

    <span data-ttu-id="ceb15-132">Otevře okno se kterém můžete zadat vaše přihlašovací údaje Azure pro pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="ceb15-132">A window opens where you can provide your Azure credentials for a work or school account.</span></span> 

    ![Zadejte přihlašovací údaje pro přístup k Power BI.](./media/stream-analytics-power-bi-dashboard/authorize-area.png)

6. <span data-ttu-id="ceb15-134">Zadejte své přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="ceb15-134">Enter your credentials.</span></span> <span data-ttu-id="ceb15-135">Upozorňujeme, pak po zadání přihlašovacích údajů se udělíte oprávnění k úlohy streamování Analytics pro přístup k vaší oblasti Power BI.</span><span class="sxs-lookup"><span data-stu-id="ceb15-135">Be aware then when you enter your credentials, you're also giving permission to the Streaming Analytics job to access your Power BI area.</span></span>

7. <span data-ttu-id="ceb15-136">Když se vrátíte na **nový výstupní** okno, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="ceb15-136">When you're returned to the **New output** blade, enter the following information:</span></span>

    * <span data-ttu-id="ceb15-137">**Skupina pracovního prostoru**: Vyberte pracovní prostor v klientovi služby Power BI, kde chcete vytvořit datová sada.</span><span class="sxs-lookup"><span data-stu-id="ceb15-137">**Group Workspace**: Select a workspace in your Power BI tenant where you want to create the dataset.</span></span>
    * <span data-ttu-id="ceb15-138">**Název datové sady**: Zadejte `sa-dataset`.</span><span class="sxs-lookup"><span data-stu-id="ceb15-138">**Dataset Name**:  Enter `sa-dataset`.</span></span> <span data-ttu-id="ceb15-139">Můžete použít jiný název.</span><span class="sxs-lookup"><span data-stu-id="ceb15-139">You can use a different name.</span></span> <span data-ttu-id="ceb15-140">Pokud tak učiníte, poznamenejte si ho na později.</span><span class="sxs-lookup"><span data-stu-id="ceb15-140">If you do, make a note of it for later.</span></span>
    * <span data-ttu-id="ceb15-141">**Název tabulky**: Zadejte `fraudulent-calls`.</span><span class="sxs-lookup"><span data-stu-id="ceb15-141">**Table Name**: Enter `fraudulent-calls`.</span></span> <span data-ttu-id="ceb15-142">Power BI výstup z úlohy Stream Analytics v současné době může mít pouze jednu tabulku v datové sadě.</span><span class="sxs-lookup"><span data-stu-id="ceb15-142">Currently, Power BI output from Stream Analytics jobs can have only one table in a dataset.</span></span>

    ![Pracovní prostor PBI](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut-with-dataset-table.png)

    > [!WARNING]
    > <span data-ttu-id="ceb15-144">Pokud má Power BI datovou sadu a tabulky, které mají stejné názvy jako ty, které zadáte v úloze Stream Analytics, se přepíší existující.</span><span class="sxs-lookup"><span data-stu-id="ceb15-144">If Power BI has a dataset and table that have the same names as the ones that you specify in the Stream Analytics job, the existing ones are overwritten.</span></span>
    > <span data-ttu-id="ceb15-145">Doporučujeme vám, že nevytvoříte explicitně tuto datovou sadu a tabulky ve vašem účtu Power BI.</span><span class="sxs-lookup"><span data-stu-id="ceb15-145">We recommend that you do not explicitly create this dataset and table in your Power BI account.</span></span> <span data-ttu-id="ceb15-146">Se automaticky vytvoří při spuštění úlohy Stream Analytics a úloha spustí čerpací výstup do Power BI.</span><span class="sxs-lookup"><span data-stu-id="ceb15-146">They are automatically created when you start your Stream Analytics job and the job starts pumping output into Power BI.</span></span> <span data-ttu-id="ceb15-147">Pokud úloha dotaz nevrátí žádné výsledky, datové sady a tabulky nejsou vytvořeny.</span><span class="sxs-lookup"><span data-stu-id="ceb15-147">If your job query doesn't return any results, the dataset and table are not  created.</span></span>
    >

8. <span data-ttu-id="ceb15-148">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-148">Click **Create**.</span></span>

<span data-ttu-id="ceb15-149">Datová sada je vytvořen s následujícími nastaveními:</span><span class="sxs-lookup"><span data-stu-id="ceb15-149">The dataset is created with the following settings:</span></span>

* <span data-ttu-id="ceb15-150">**defaultRetentionPolicy: BasicFIFO**: Data jsou FIFO, s maximálně 200 000 řádků.</span><span class="sxs-lookup"><span data-stu-id="ceb15-150">**defaultRetentionPolicy: BasicFIFO**: Data is FIFO, with a maximum of 200,000 rows.</span></span>
* <span data-ttu-id="ceb15-151">**Vlastnost defaultMode: pushStreaming**: datovou sadu (také známa jako podporuje streamování dlaždice a tradiční vizuální prvky založené na sestavy</span><span class="sxs-lookup"><span data-stu-id="ceb15-151">**defaultMode: pushStreaming**: The dataset supports both streaming tiles and traditional report-based visuals (a.k.a.</span></span> <span data-ttu-id="ceb15-152">nabídka).</span><span class="sxs-lookup"><span data-stu-id="ceb15-152">push).</span></span>

<span data-ttu-id="ceb15-153">V současné době nelze vytvořit datové sady s další příznaky.</span><span class="sxs-lookup"><span data-stu-id="ceb15-153">Currently, you can't create datasets with other flags.</span></span>

<span data-ttu-id="ceb15-154">Další informace o datových sadách Power BI, najdete v článku [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) odkaz.</span><span class="sxs-lookup"><span data-stu-id="ceb15-154">For more information about Power BI datasets, see the [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) reference.</span></span>


## <a name="write-the-query"></a><span data-ttu-id="ceb15-155">Dotaz zapište</span><span class="sxs-lookup"><span data-stu-id="ceb15-155">Write the query</span></span>

1. <span data-ttu-id="ceb15-156">Zavřít **výstupy** okno a vraťte se do okna úlohy.</span><span class="sxs-lookup"><span data-stu-id="ceb15-156">Close the **Outputs** blade and return to the job blade.</span></span>

2. <span data-ttu-id="ceb15-157">Klikněte **dotazu** pole.</span><span class="sxs-lookup"><span data-stu-id="ceb15-157">Click the **Query** box.</span></span> 

3. <span data-ttu-id="ceb15-158">Zadejte následující dotaz.</span><span class="sxs-lookup"><span data-stu-id="ceb15-158">Enter the following query.</span></span> <span data-ttu-id="ceb15-159">Tento dotaz je podobná dotaz spojení sama na sebe, kterou jste vytvořili v tomto kurzu odhalování podvodů.</span><span class="sxs-lookup"><span data-stu-id="ceb15-159">This query is similar to the self-join query you created in the fraud-detection tutorial.</span></span> <span data-ttu-id="ceb15-160">Rozdílem je, že tento dotaz odešle výsledky do nové výstup, který jste vytvořili (`CallStream-PowerBI`).</span><span class="sxs-lookup"><span data-stu-id="ceb15-160">The difference is that this query sends results to the new output you created (`CallStream-PowerBI`).</span></span> 

    >[!NOTE]
    ><span data-ttu-id="ceb15-161">Pokud není název vstupu `CallStream` v tomto kurzu odhalování podvodů, nahraďte název pro `CallStream` v **FROM** a **připojení** klauzule v dotazu.</span><span class="sxs-lookup"><span data-stu-id="ceb15-161">If you did not name the input `CallStream` in the fraud-detection tutorial, substitute your name for `CallStream` in the **FROM** and **JOIN** clauses in the query.</span></span>

        /* Our criteria for fraud:
        Calls made from the same caller to two phone switches in different locations (for example, Australia and Europe) within five seconds */

        SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
        INTO "CallStream-PowerBI"
        FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
        JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime

        /* Where the caller is the same, as indicated by IMSI (International Mobile Subscriber Identity) */
        ON CS1.CallingIMSI = CS2.CallingIMSI

        /* ...and date between CS1 and CS2 is between one and five seconds */
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5

        /* Where the switch location is different */
        WHERE CS1.SwitchNum != CS2.SwitchNum
        GROUP BY TumblingWindow(Duration(second, 1))

4. <span data-ttu-id="ceb15-162">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-162">Click **Save**.</span></span>


## <a name="test-the-query"></a><span data-ttu-id="ceb15-163">Otestujte dotaz</span><span class="sxs-lookup"><span data-stu-id="ceb15-163">Test the query</span></span>
<span data-ttu-id="ceb15-164">Tato část je volitelný, ale doporučujeme.</span><span class="sxs-lookup"><span data-stu-id="ceb15-164">This section is optional, but recommended.</span></span> 

1. <span data-ttu-id="ceb15-165">Pokud aplikace TelcoStreaming není aktuálně spuštěná, spusťte ji pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="ceb15-165">If the TelcoStreaming app is not currently running, start it by following these steps:</span></span>

    * <span data-ttu-id="ceb15-166">Otevřete okno příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="ceb15-166">Open a command window.</span></span>
    * <span data-ttu-id="ceb15-167">Přejděte do složky, kde jsou telcogenerator.exe a telcodatagen.exe.config změněné soubory.</span><span class="sxs-lookup"><span data-stu-id="ceb15-167">Go to the folder where the telcogenerator.exe and modified telcodatagen.exe.config files are.</span></span>
    * <span data-ttu-id="ceb15-168">Spusťte následující příkaz:</span><span class="sxs-lookup"><span data-stu-id="ceb15-168">Run the following command:</span></span>

            telcodatagen.exe 1000 .2 2

2. <span data-ttu-id="ceb15-169">V **dotazu** okně klikněte na tlačítko se tečkami vedle `CallStream` vstup a pak vyberte **vzorová data ze vstupu**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-169">In the **Query** blade, click the dots next to the `CallStream` input and then select **Sample data from input**.</span></span>

3. <span data-ttu-id="ceb15-170">Určete, zda má tři minut za dat a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-170">Specify that you want three minutes' worth of data and click **OK**.</span></span> <span data-ttu-id="ceb15-171">Počkejte, dokud se oznámení, že data odebírají vzorky.</span><span class="sxs-lookup"><span data-stu-id="ceb15-171">Wait until you're notified that the data has been sampled.</span></span>

4. <span data-ttu-id="ceb15-172">Klikněte na tlačítko **Test** a zajistěte, aby vám výsledky.</span><span class="sxs-lookup"><span data-stu-id="ceb15-172">Click **Test** and make sure you're getting results.</span></span>


## <a name="run-the-job"></a><span data-ttu-id="ceb15-173">Spustit úlohu</span><span class="sxs-lookup"><span data-stu-id="ceb15-173">Run the job</span></span>

1. <span data-ttu-id="ceb15-174">Ujistěte se, zda je spuštěna aplikace TelcoStreaming.</span><span class="sxs-lookup"><span data-stu-id="ceb15-174">Make sure that the TelcoStreaming app is running.</span></span>

2. <span data-ttu-id="ceb15-175">Zavřít **dotazu** okno.</span><span class="sxs-lookup"><span data-stu-id="ceb15-175">Close the **Query** blade.</span></span>

3. <span data-ttu-id="ceb15-176">V okně úlohy klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-176">In the job blade, click **Start**.</span></span>

    ![Spustit úlohu služby Stream Analytics](./media/stream-analytics-power-bi-dashboard/stream-analytics-sa-job-start-output.png)

<span data-ttu-id="ceb15-178">Úlohu streamování Analytics spustí vyhledávání podvodné volání v příchozím datovém proudu.</span><span class="sxs-lookup"><span data-stu-id="ceb15-178">Your Streaming Analytics job starts looking for fraudulent calls in the incoming stream.</span></span> <span data-ttu-id="ceb15-179">Úloha také vytvoří datovou sadu a tabulkou v Power BI a spustí odesílání dat o podvodné volání k nim.</span><span class="sxs-lookup"><span data-stu-id="ceb15-179">The job also creates the dataset and table in Power BI and starts sending data about the fraudulent calls to them.</span></span>


## <a name="create-the-dashboard-in-power-bi"></a><span data-ttu-id="ceb15-180">Vytvořit řídicí panel v Power BI</span><span class="sxs-lookup"><span data-stu-id="ceb15-180">Create the dashboard in Power BI</span></span>

1. <span data-ttu-id="ceb15-181">Přejděte na [Powerbi.com](https://powerbi.com) a přihlaste se pomocí svého pracovního nebo školního účtu.</span><span class="sxs-lookup"><span data-stu-id="ceb15-181">Go to [Powerbi.com](https://powerbi.com) and sign in with your work or school account.</span></span> <span data-ttu-id="ceb15-182">Pokud dotaz úlohy Stream Analytics výstupy výsledků, zobrazí vaše datová sada je už vytvořený:</span><span class="sxs-lookup"><span data-stu-id="ceb15-182">If the Stream Analytics job query outputs results, you see that your dataset is already created:</span></span>

    ![Streamovanou datovou sadu v Power BI](./media/stream-analytics-power-bi-dashboard/streaming-dataset.png)

2. <span data-ttu-id="ceb15-184">V pracovním prostoru, klikněte na tlačítko  **+ &nbsp;vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-184">In your workspace, click **+&nbsp;Create**.</span></span>

    ![Tlačítko vytvořit v pracovním prostoru Power BI](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard.png)

3. <span data-ttu-id="ceb15-186">Vytvořit nový řídicí panel a pojmenujte ji `Fraudulent Calls`.</span><span class="sxs-lookup"><span data-stu-id="ceb15-186">Create a new dashboard and name it `Fraudulent Calls`.</span></span>

    ![Vytvořit řídicí panel a pojmenujte ho v prostoru Power BI](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard-name.png)

4. <span data-ttu-id="ceb15-188">V horní části okna klikněte na tlačítko **přidat dlaždice**, vyberte **datových proudů vlastní**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-188">At the top of the window, click **Add tile**, select **CUSTOM STREAMING DATA**, and then click **Next**.</span></span>

    ![Vlastní streamování datové sady](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

5. <span data-ttu-id="ceb15-190">V části **si DATSETS**, vyberte datovou sadu a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-190">Under **YOUR DATSETS**, select your dataset and then click **Next**.</span></span>

    ![Vaši streamovanou datovou sadu](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

6. <span data-ttu-id="ceb15-192">V části **typ vizualizace**, vyberte **karty**a potom v **pole** seznamu, vyberte **fraudulentcalls**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-192">Under **Visualization Type**, select **Card**, and then in the **Fields** list, select **fraudulentcalls**.</span></span>

    ![Vizualizace podrobnosti nové dlaždice.](./media/stream-analytics-power-bi-dashboard/add-fraud.png)

7. <span data-ttu-id="ceb15-194">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-194">Click **Next**.</span></span>

8. <span data-ttu-id="ceb15-195">Zadejte podrobnosti o dlaždici jako nadpis a podnadpis.</span><span class="sxs-lookup"><span data-stu-id="ceb15-195">Fill in tile details like a title and subtitle.</span></span>

    ![Nadpis a podnadpis nové dlaždice.](./media/stream-analytics-power-bi-dashboard/pbi-new-tile-details.png)

9. <span data-ttu-id="ceb15-197">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-197">Click **Apply**.</span></span>

    <span data-ttu-id="ceb15-198">Nyní máte čítač podvod!</span><span class="sxs-lookup"><span data-stu-id="ceb15-198">Now you have a fraud counter!</span></span>

    ![Čítač podvod](./media/stream-analytics-power-bi-dashboard/fraud-counter.png)

8. <span data-ttu-id="ceb15-200">Postup znovu přidat dlaždici (počínaje krok 4).</span><span class="sxs-lookup"><span data-stu-id="ceb15-200">Follow the steps again to add a tile (starting with step 4).</span></span> <span data-ttu-id="ceb15-201">Tentokrát, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="ceb15-201">This time, do the following:</span></span>

    * <span data-ttu-id="ceb15-202">Když dojde k **typ vizualizace**, vyberte **spojnicový graf**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-202">When you get to **Visualization Type**, select **Line chart**.</span></span> 
    * <span data-ttu-id="ceb15-203">Přidejte osy a vyberte **windowend**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-203">Add an axis and select **windowend**.</span></span> 
    * <span data-ttu-id="ceb15-204">Přidejte hodnotu a vyberte **fraudulentcalls**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-204">Add a value and select **fraudulentcalls**.</span></span>
    * <span data-ttu-id="ceb15-205">Pro **časový interval pro zobrazení**, vyberte posledních 10 minut.</span><span class="sxs-lookup"><span data-stu-id="ceb15-205">For **Time window to display**, select the last 10 minutes.</span></span>

    ![Vytvoření dlaždici spojnicový graf](./media/stream-analytics-power-bi-dashboard/pbi-create-tile-line-chart.png)

9. <span data-ttu-id="ceb15-207">Klikněte na tlačítko **Další**, přidat nadpis a podnadpis a klikněte na **použít**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-207">Click **Next**, add a title and subtitle, and click **Apply**.</span></span>

    <span data-ttu-id="ceb15-208">Řídicí panel Power BI teď umožňuje dvě zobrazení dat o podvodné volání jako v dat zjištěn.</span><span class="sxs-lookup"><span data-stu-id="ceb15-208">The Power BI dashboard now gives you two views of data about fraudulent calls as detected in the streaming data.</span></span>

    ![Dokončení zobrazující dvě dlaždice pro podvodné volání řídicí panel Power BI](./media/stream-analytics-power-bi-dashboard/pbi-dashboard-fraudulent-calls-finished.png)


## <a name="learn-more-about-power-bi"></a><span data-ttu-id="ceb15-210">Další informace o Power BI</span><span class="sxs-lookup"><span data-stu-id="ceb15-210">Learn more about Power BI</span></span>

<span data-ttu-id="ceb15-211">Tento kurz ukazuje, jak vytvořit pouze několik druhů vizualizace pro datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="ceb15-211">This tutorial demonstrates how to create only a few kinds of visualizations for a dataset.</span></span> <span data-ttu-id="ceb15-212">Power BI můžete vytvořit další nástroje business intelligence zákazníka pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="ceb15-212">Power BI can help you create other customer business intelligence tools for your organization.</span></span> <span data-ttu-id="ceb15-213">Další nápady najdete v následujících materiálech:</span><span class="sxs-lookup"><span data-stu-id="ceb15-213">For more ideas, see the following resources:</span></span>

* <span data-ttu-id="ceb15-214">Další příklad řídicí panel Power BI, podívejte se [Začínáme s Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) videa.</span><span class="sxs-lookup"><span data-stu-id="ceb15-214">For another example of a Power BI dashboard, watch the [Getting Started with Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) video.</span></span>
* <span data-ttu-id="ceb15-215">Další informace o konfiguraci streamování Analytics úlohy výstup do Power BI a používání skupin Power BI, zkontrolujte [Power BI](stream-analytics-define-outputs.md#power-bi) části [Stream Analytics výstupy](stream-analytics-define-outputs.md) článku.</span><span class="sxs-lookup"><span data-stu-id="ceb15-215">For more information about configuring Streaming Analytics job output to Power BI and using Power BI groups, review the [Power BI](stream-analytics-define-outputs.md#power-bi) section of the [Stream Analytics outputs](stream-analytics-define-outputs.md) article.</span></span> 
* <span data-ttu-id="ceb15-216">Informace o používání Power BI obecně najdete v tématu [řídicí panely v Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).</span><span class="sxs-lookup"><span data-stu-id="ceb15-216">For information about using Power BI generally, see [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).</span></span>


## <a name="learn-about-limitations-and-best-practices"></a><span data-ttu-id="ceb15-217">Další informace o omezení a doporučené postupy</span><span class="sxs-lookup"><span data-stu-id="ceb15-217">Learn about limitations and best practices</span></span>
<span data-ttu-id="ceb15-218">V současné době Power BI je možné volat přibližně jednou za sekundu.</span><span class="sxs-lookup"><span data-stu-id="ceb15-218">Currently, Power BI can be called roughly once per second.</span></span> <span data-ttu-id="ceb15-219">Streamování vizuály podporu paketů 15 kb.</span><span class="sxs-lookup"><span data-stu-id="ceb15-219">Streaming visuals support packets of 15 KB.</span></span> <span data-ttu-id="ceb15-220">Kromě toho selhání streamování vizuály (ale nabízené bude pořád fungovat,).</span><span class="sxs-lookup"><span data-stu-id="ceb15-220">Beyond that, streaming visuals fail (but push continues to work).</span></span> <span data-ttu-id="ceb15-221">Z důvodu tato omezení Power BI různě nejvíce přirozeně případech, kde Azure Stream Analytics nepodporuje snížení zatížení významné data.</span><span class="sxs-lookup"><span data-stu-id="ceb15-221">Because of these limitations, Power BI lends itself most naturally to cases where Azure Stream Analytics does a significant data load reduction.</span></span> <span data-ttu-id="ceb15-222">Doporučujeme, abyste pomocí Přeskakující okno nebo okno Hopping a ujistěte se, že datová oznámení jsou maximálně jeden nabízené za sekundu a že váš dotaz pojmenováváme v rámci požadavky na propustnost.</span><span class="sxs-lookup"><span data-stu-id="ceb15-222">We recommend using a Tumbling window or Hopping window to ensure that data push is at most one push per second, and that your query lands within the throughput requirements.</span></span>

<span data-ttu-id="ceb15-223">Vzorce můžete použít k výpočtu hodnoty umožnit okně aplikace v sekundách:</span><span class="sxs-lookup"><span data-stu-id="ceb15-223">You can use the following equation to compute the value to give your window in seconds:</span></span>

![Equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

<span data-ttu-id="ceb15-225">Například:</span><span class="sxs-lookup"><span data-stu-id="ceb15-225">For example:</span></span>

* <span data-ttu-id="ceb15-226">Máte 1 000 zařízení odesílání dat v intervalech jednu sekundu.</span><span class="sxs-lookup"><span data-stu-id="ceb15-226">You have 1,000 devices sending data at one-second intervals.</span></span>
* <span data-ttu-id="ceb15-227">Používáte SKU Pro Power BI, který podporuje 1 000 000 řádků za hodinu.</span><span class="sxs-lookup"><span data-stu-id="ceb15-227">You are using the Power BI Pro SKU that supports 1,000,000 rows per hour.</span></span>
* <span data-ttu-id="ceb15-228">Chcete publikovat množství průměr dat na zařízení k Power BI.</span><span class="sxs-lookup"><span data-stu-id="ceb15-228">You want to publish the amount of average data per device to Power BI.</span></span>

<span data-ttu-id="ceb15-229">V důsledku toho rovnici:</span><span class="sxs-lookup"><span data-stu-id="ceb15-229">As a result, the equation becomes:</span></span>

![Equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

<span data-ttu-id="ceb15-231">V této konfiguraci můžete změnit původní dotaz na následující:</span><span class="sxs-lookup"><span data-stu-id="ceb15-231">Given this configuration, you can change the original query to the following:</span></span>

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO "CallStream-PowerBI"
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl


### <a name="renew-authorization"></a><span data-ttu-id="ceb15-232">Obnovit ověřování</span><span class="sxs-lookup"><span data-stu-id="ceb15-232">Renew authorization</span></span>
<span data-ttu-id="ceb15-233">Pokud došlo ke změně hesla vzhledem k tomu, že vaše úlohy vytvoření nebo poslední ověření, musíte k novému ověření svůj účet Power BI.</span><span class="sxs-lookup"><span data-stu-id="ceb15-233">If the password has changed since your job was created or last authenticated, you need to reauthenticate your Power BI account.</span></span> <span data-ttu-id="ceb15-234">Pokud Azure Multi-Factor Authentication nakonfigurován v klientovi služby Azure Active Directory (Azure AD), musíte taky obnovit Power BI autorizace každé dva týdny.</span><span class="sxs-lookup"><span data-stu-id="ceb15-234">If Azure Multi-Factor Authentication is configured on your Azure Active Directory (Azure AD) tenant, you also need to renew Power BI authorization every two weeks.</span></span> <span data-ttu-id="ceb15-235">Pokud neobnovíte, se může zobrazit příznaky například chybějících výstup úlohy nebo `Authenticate user error` v protokoly operací.</span><span class="sxs-lookup"><span data-stu-id="ceb15-235">If you don't renew, you could see symptoms such as a lack of job output or an `Authenticate user error` in the operation logs.</span></span>

<span data-ttu-id="ceb15-236">Podobně pokud úloha spustí po vypršení platnosti tokenu, dojde k chybě a úloha se nezdaří.</span><span class="sxs-lookup"><span data-stu-id="ceb15-236">Similarly, if a job starts after the token has expired, an error occurs and the job fails.</span></span> <span data-ttu-id="ceb15-237">Chcete-li vyřešit tento problém, přejděte na výstupu Power BI a zastavit úlohu, která je spuštěná.</span><span class="sxs-lookup"><span data-stu-id="ceb15-237">To resolve this issue, stop the job that's running and go to your Power BI output.</span></span> <span data-ttu-id="ceb15-238">Aby nedošlo ke ztrátě dat, vyberte **obnovit autorizace** propojit a pak restartujte úlohu z **naposledy Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="ceb15-238">To avoid data loss, select the **Renew authorization** link, and then restart your job from the **Last Stopped Time**.</span></span>

<span data-ttu-id="ceb15-239">Po aktualizaci autorizaci s Power BI, Zelená výstraha se zobrazí v oblasti autorizace tak, aby odrážela, že byl problém vyřešen.</span><span class="sxs-lookup"><span data-stu-id="ceb15-239">After the authorization has been refreshed with Power BI, a green alert appears in the authorization area to reflect that the issue has been resolved.</span></span>

## <a name="get-help"></a><span data-ttu-id="ceb15-240">Podpora</span><span class="sxs-lookup"><span data-stu-id="ceb15-240">Get help</span></span>
<span data-ttu-id="ceb15-241">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="ceb15-241">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ceb15-242">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ceb15-242">Next steps</span></span>
* [<span data-ttu-id="ceb15-243">Úvod do služby Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ceb15-243">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="ceb15-244">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ceb15-244">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="ceb15-245">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ceb15-245">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="ceb15-246">Azure Stream Analytics query language – referenční informace</span><span class="sxs-lookup"><span data-stu-id="ceb15-246">Azure Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="ceb15-247">Referenční dokumentace Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="ceb15-247">Azure Stream Analytics Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
