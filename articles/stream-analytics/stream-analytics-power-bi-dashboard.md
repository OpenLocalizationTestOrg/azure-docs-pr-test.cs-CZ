---
title: "řídicí panel BI aaaPower na Azure Stream Analytics | Microsoft Docs"
description: "Použijte v reálném čase streamování Power BI řídicí panel toogather business intelligence a analýze velkých objemů dat z úlohy Stream Analytics."
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
ms.openlocfilehash: cb9127576230e9d327b437b674f31cc23869bfff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="stream-analytics-and-power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a><span data-ttu-id="ab649-104">Stream Analytics a Power BI: řídicí panel analýzy v reálném čase pro datový proud</span><span class="sxs-lookup"><span data-stu-id="ab649-104">Stream Analytics and Power BI: A real-time analytics dashboard for streaming data</span></span>
<span data-ttu-id="ab649-105">Azure Stream Analytics můžete využít jeden z hello úvodní nástroje business intelligence tootake [Microsoft Power BI](https://powerbi.com/).</span><span class="sxs-lookup"><span data-stu-id="ab649-105">Azure Stream Analytics enables you tootake advantage of one of hello leading business intelligence tools, [Microsoft Power BI](https://powerbi.com/).</span></span> <span data-ttu-id="ab649-106">V tomto článku se dozvíte, jak vytvořit nástroje business intelligence pomocí Power BI jako výstup pro úlohy Azure Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="ab649-106">In this article, you learn how create business intelligence tools by using Power BI as an output for your Azure Stream Analytics jobs.</span></span> <span data-ttu-id="ab649-107">Také zjistíte, jak toocreate a použití řídicího panelu v reálném čase.</span><span class="sxs-lookup"><span data-stu-id="ab649-107">You also learn how toocreate and use a real-time dashboard.</span></span>

<span data-ttu-id="ab649-108">Tento článek pokračuje z hello Stream Analytics [odhalování podvodů v reálném čase](stream-analytics-real-time-fraud-detection.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="ab649-108">This article continues from hello Stream Analytics [real-time fraud detection](stream-analytics-real-time-fraud-detection.md) tutorial.</span></span> <span data-ttu-id="ab649-109">Staví na hello pracovní postup vytvořený v tomto kurzu, přičemž přidá Power BI výstup, takže můžete vizualizovat podvodné telefonních hovorů, které byly zjištěny nástrojem úlohu streamování Analytics.</span><span class="sxs-lookup"><span data-stu-id="ab649-109">It builds on hello workflow created in that tutorial and adds a Power BI output so that you can visualize fraudulent phone calls that are detected by a Streaming Analytics job.</span></span> 

<span data-ttu-id="ab649-110">Můžete sledovat [video](https://www.youtube.com/watch?v=SGUpT-a99MA) který znázorňuje tento scénář.</span><span class="sxs-lookup"><span data-stu-id="ab649-110">You can watch [a video](https://www.youtube.com/watch?v=SGUpT-a99MA)  that illustrates this scenario.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="ab649-111">Požadavky</span><span class="sxs-lookup"><span data-stu-id="ab649-111">Prerequisites</span></span>

<span data-ttu-id="ab649-112">Než začnete, ujistěte se, že máte hello následující:</span><span class="sxs-lookup"><span data-stu-id="ab649-112">Before you start, make sure you have hello following:</span></span>

* <span data-ttu-id="ab649-113">Účet Azure.</span><span class="sxs-lookup"><span data-stu-id="ab649-113">An Azure account.</span></span>
* <span data-ttu-id="ab649-114">Účet pro Power BI.</span><span class="sxs-lookup"><span data-stu-id="ab649-114">An account for Power BI.</span></span> <span data-ttu-id="ab649-115">Můžete použít účet pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="ab649-115">You can use a work account or a school account.</span></span>
* <span data-ttu-id="ab649-116">Dokončené verzi hello [odhalování podvodů v reálném čase](stream-analytics-real-time-fraud-detection.md) kurzu.</span><span class="sxs-lookup"><span data-stu-id="ab649-116">A completed version of hello [real-time fraud detection](stream-analytics-real-time-fraud-detection.md) tutorial.</span></span> <span data-ttu-id="ab649-117">kurz Hello zahrnuje aplikaci, která generuje metadata fiktivní telefonní hovor.</span><span class="sxs-lookup"><span data-stu-id="ab649-117">hello tutorial includes an app that generates fictitious telephone-call metadata.</span></span> <span data-ttu-id="ab649-118">V kurzu hello vytvoření centra událostí a odeslání hello streamování centra událostí toohello data telefonního hovoru.</span><span class="sxs-lookup"><span data-stu-id="ab649-118">In hello tutorial, you create an event hub and send hello streaming phone call data toohello event hub.</span></span> <span data-ttu-id="ab649-119">Můžete vytvořit dotaz, který zjistí podvodné volání (volání z hello stejné číslo na hello stejný čas v různých umístěních).</span><span class="sxs-lookup"><span data-stu-id="ab649-119">You write a query that detects fraudulent calls (calls from hello same number at hello same time in different locations).</span></span> 


## <a name="add-power-bi-output"></a><span data-ttu-id="ab649-120">Přidat výstup Power BI</span><span class="sxs-lookup"><span data-stu-id="ab649-120">Add Power BI output</span></span>
<span data-ttu-id="ab649-121">V kurzu zjišťování podvodů v reálném čase hello výstup hello je odeslán tooAzure úložiště objektů Blob.</span><span class="sxs-lookup"><span data-stu-id="ab649-121">In hello real-time fraud detection tutorial, hello output is sent tooAzure Blob storage.</span></span> <span data-ttu-id="ab649-122">V této části přidáte výstupu, který odesílá informace tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="ab649-122">In this section, you add an output that sends information tooPower BI.</span></span>

1. <span data-ttu-id="ab649-123">V hello portálu Azure otevřete úlohy streamování Analytics hello, kterou jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="ab649-123">In hello Azure portal, open hello Streaming Analytics job that you created earlier.</span></span> <span data-ttu-id="ab649-124">Pokud jste použili hello navrhovaný název, má název úlohy hello `sa_frauddetection_job_demo`.</span><span class="sxs-lookup"><span data-stu-id="ab649-124">If you used hello suggested name, hello job is named `sa_frauddetection_job_demo`.</span></span>

2. <span data-ttu-id="ab649-125">Vyberte hello **výstupy** uprostřed hello hello úlohy řídicího panelu a potom vyberte položku **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="ab649-125">Select hello **Outputs** box in hello middle of hello job dashboard and then select **+ Add**.</span></span>

3. <span data-ttu-id="ab649-126">Pro **Alias pro výstup**, zadejte `CallStream-PowerBI`.</span><span class="sxs-lookup"><span data-stu-id="ab649-126">For **Output Alias**, enter `CallStream-PowerBI`.</span></span> <span data-ttu-id="ab649-127">Můžete použít jiný název.</span><span class="sxs-lookup"><span data-stu-id="ab649-127">You can use a different name.</span></span> <span data-ttu-id="ab649-128">Pokud tak učiníte, poznamenejte si ho, protože potřebujete název hello později.</span><span class="sxs-lookup"><span data-stu-id="ab649-128">If you do, make a note of it, because you need hello name later.</span></span> 

4. <span data-ttu-id="ab649-129">V části **jímky**, vyberte **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="ab649-129">Under **Sink**, select **Power BI**.</span></span>

   ![Vytvoření výstupu pro Power BI](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut.png)

5. <span data-ttu-id="ab649-131">Klikněte na tlačítko **Autorizovat**.</span><span class="sxs-lookup"><span data-stu-id="ab649-131">Click **Authorize**.</span></span>

    <span data-ttu-id="ab649-132">Otevře okno se kterém můžete zadat vaše přihlašovací údaje Azure pro pracovní nebo školní účet.</span><span class="sxs-lookup"><span data-stu-id="ab649-132">A window opens where you can provide your Azure credentials for a work or school account.</span></span> 

    ![Zadejte přihlašovací údaje pro přístup k tooPower BI](./media/stream-analytics-power-bi-dashboard/authorize-area.png)

6. <span data-ttu-id="ab649-134">Zadejte své přihlašovací údaje.</span><span class="sxs-lookup"><span data-stu-id="ab649-134">Enter your credentials.</span></span> <span data-ttu-id="ab649-135">Upozorňujeme, pak po zadání přihlašovacích údajů, můžete se také udělíte oprávnění toohello streamování Analytics úlohy tooaccess vaší oblasti Power BI.</span><span class="sxs-lookup"><span data-stu-id="ab649-135">Be aware then when you enter your credentials, you're also giving permission toohello Streaming Analytics job tooaccess your Power BI area.</span></span>

7. <span data-ttu-id="ab649-136">Když se vrátil toohello **nový výstupní** okno, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="ab649-136">When you're returned toohello **New output** blade, enter hello following information:</span></span>

    * <span data-ttu-id="ab649-137">**Skupina pracovního prostoru**: Vyberte pracovní prostor v klientovi služby Power BI, kde chcete datovou sadu toocreate hello.</span><span class="sxs-lookup"><span data-stu-id="ab649-137">**Group Workspace**: Select a workspace in your Power BI tenant where you want toocreate hello dataset.</span></span>
    * <span data-ttu-id="ab649-138">**Název datové sady**: Zadejte `sa-dataset`.</span><span class="sxs-lookup"><span data-stu-id="ab649-138">**Dataset Name**:  Enter `sa-dataset`.</span></span> <span data-ttu-id="ab649-139">Můžete použít jiný název.</span><span class="sxs-lookup"><span data-stu-id="ab649-139">You can use a different name.</span></span> <span data-ttu-id="ab649-140">Pokud tak učiníte, poznamenejte si ho na později.</span><span class="sxs-lookup"><span data-stu-id="ab649-140">If you do, make a note of it for later.</span></span>
    * <span data-ttu-id="ab649-141">**Název tabulky**: Zadejte `fraudulent-calls`.</span><span class="sxs-lookup"><span data-stu-id="ab649-141">**Table Name**: Enter `fraudulent-calls`.</span></span> <span data-ttu-id="ab649-142">Power BI výstup z úlohy Stream Analytics v současné době může mít pouze jednu tabulku v datové sadě.</span><span class="sxs-lookup"><span data-stu-id="ab649-142">Currently, Power BI output from Stream Analytics jobs can have only one table in a dataset.</span></span>

    ![Pracovní prostor PBI](./media/stream-analytics-power-bi-dashboard/create-pbi-ouptut-with-dataset-table.png)

    > [!WARNING]
    > <span data-ttu-id="ab649-144">Pokud má Power BI datové sady a tabulky, které mají hello stejné názvy jako hello ta, která zadáte v úloze Stream Analytics hello, se přepíší existující hello.</span><span class="sxs-lookup"><span data-stu-id="ab649-144">If Power BI has a dataset and table that have hello same names as hello ones that you specify in hello Stream Analytics job, hello existing ones are overwritten.</span></span>
    > <span data-ttu-id="ab649-145">Doporučujeme vám, že nevytvoříte explicitně tuto datovou sadu a tabulky ve vašem účtu Power BI.</span><span class="sxs-lookup"><span data-stu-id="ab649-145">We recommend that you do not explicitly create this dataset and table in your Power BI account.</span></span> <span data-ttu-id="ab649-146">Se automaticky vytvoří při spuštění úlohy Stream Analytics a hello úloha spustí čerpací výstup do Power BI.</span><span class="sxs-lookup"><span data-stu-id="ab649-146">They are automatically created when you start your Stream Analytics job and hello job starts pumping output into Power BI.</span></span> <span data-ttu-id="ab649-147">Pokud úloha dotaz nevrátí žádné výsledky, nejsou vytvořeny hello datovou sadu a tabulku.</span><span class="sxs-lookup"><span data-stu-id="ab649-147">If your job query doesn't return any results, hello dataset and table are not  created.</span></span>
    >

8. <span data-ttu-id="ab649-148">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ab649-148">Click **Create**.</span></span>

<span data-ttu-id="ab649-149">Datová sada Hello je vytvořen s hello následující nastavení:</span><span class="sxs-lookup"><span data-stu-id="ab649-149">hello dataset is created with hello following settings:</span></span>

* <span data-ttu-id="ab649-150">**defaultRetentionPolicy: BasicFIFO**: Data jsou FIFO, s maximálně 200 000 řádků.</span><span class="sxs-lookup"><span data-stu-id="ab649-150">**defaultRetentionPolicy: BasicFIFO**: Data is FIFO, with a maximum of 200,000 rows.</span></span>
* <span data-ttu-id="ab649-151">**Vlastnost defaultMode: pushStreaming**: hello datovou sadu (také známa jako podporuje streamování dlaždice a tradiční vizuální prvky založené na sestavy</span><span class="sxs-lookup"><span data-stu-id="ab649-151">**defaultMode: pushStreaming**: hello dataset supports both streaming tiles and traditional report-based visuals (a.k.a.</span></span> <span data-ttu-id="ab649-152">nabídka).</span><span class="sxs-lookup"><span data-stu-id="ab649-152">push).</span></span>

<span data-ttu-id="ab649-153">V současné době nelze vytvořit datové sady s další příznaky.</span><span class="sxs-lookup"><span data-stu-id="ab649-153">Currently, you can't create datasets with other flags.</span></span>

<span data-ttu-id="ab649-154">Další informace o datových sadách Power BI najdete v tématu hello [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) odkaz.</span><span class="sxs-lookup"><span data-stu-id="ab649-154">For more information about Power BI datasets, see hello [Power BI REST API](https://msdn.microsoft.com/library/mt203562.aspx) reference.</span></span>


## <a name="write-hello-query"></a><span data-ttu-id="ab649-155">Zápis dotazů hello</span><span class="sxs-lookup"><span data-stu-id="ab649-155">Write hello query</span></span>

1. <span data-ttu-id="ab649-156">Zavřít hello **výstupy** okno a okno návratový toohello úlohy.</span><span class="sxs-lookup"><span data-stu-id="ab649-156">Close hello **Outputs** blade and return toohello job blade.</span></span>

2. <span data-ttu-id="ab649-157">Klikněte na tlačítko hello **dotazu** pole.</span><span class="sxs-lookup"><span data-stu-id="ab649-157">Click hello **Query** box.</span></span> 

3. <span data-ttu-id="ab649-158">Zadejte následující dotaz hello.</span><span class="sxs-lookup"><span data-stu-id="ab649-158">Enter hello following query.</span></span> <span data-ttu-id="ab649-159">Tento dotaz je podobné toohello spojení sama na sebe dotazu, kterou jste vytvořili v kurzu hello odhalování podvodů.</span><span class="sxs-lookup"><span data-stu-id="ab649-159">This query is similar toohello self-join query you created in hello fraud-detection tutorial.</span></span> <span data-ttu-id="ab649-160">Hello rozdílem je, že tento dotaz odešle výsledky toohello nový výstupní jste vytvořili (`CallStream-PowerBI`).</span><span class="sxs-lookup"><span data-stu-id="ab649-160">hello difference is that this query sends results toohello new output you created (`CallStream-PowerBI`).</span></span> 

    >[!NOTE]
    ><span data-ttu-id="ab649-161">Pokud není název vstupu hello `CallStream` v kurzu hello odhalování podvodů, nahraďte název pro `CallStream` v hello **FROM** a **připojení** klauzule v dotazu hello.</span><span class="sxs-lookup"><span data-stu-id="ab649-161">If you did not name hello input `CallStream` in hello fraud-detection tutorial, substitute your name for `CallStream` in hello **FROM** and **JOIN** clauses in hello query.</span></span>

        /* Our criteria for fraud:
        Calls made from hello same caller tootwo phone switches in different locations (for example, Australia and Europe) within five seconds */

        SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
        INTO "CallStream-PowerBI"
        FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
        JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime

        /* Where hello caller is hello same, as indicated by IMSI (International Mobile Subscriber Identity) */
        ON CS1.CallingIMSI = CS2.CallingIMSI

        /* ...and date between CS1 and CS2 is between one and five seconds */
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5

        /* Where hello switch location is different */
        WHERE CS1.SwitchNum != CS2.SwitchNum
        GROUP BY TumblingWindow(Duration(second, 1))

4. <span data-ttu-id="ab649-162">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="ab649-162">Click **Save**.</span></span>


## <a name="test-hello-query"></a><span data-ttu-id="ab649-163">Test hello dotazu</span><span class="sxs-lookup"><span data-stu-id="ab649-163">Test hello query</span></span>
<span data-ttu-id="ab649-164">Tato část je volitelný, ale doporučujeme.</span><span class="sxs-lookup"><span data-stu-id="ab649-164">This section is optional, but recommended.</span></span> 

1. <span data-ttu-id="ab649-165">Pokud aplikace hello TelcoStreaming není aktuálně spuštěná, spusťte ji pomocí následujících kroků:</span><span class="sxs-lookup"><span data-stu-id="ab649-165">If hello TelcoStreaming app is not currently running, start it by following these steps:</span></span>

    * <span data-ttu-id="ab649-166">Otevřete okno příkazového řádku.</span><span class="sxs-lookup"><span data-stu-id="ab649-166">Open a command window.</span></span>
    * <span data-ttu-id="ab649-167">Přejděte toohello složku, kde jsou hello telcogenerator.exe a upravené telcodatagen.exe.config soubory.</span><span class="sxs-lookup"><span data-stu-id="ab649-167">Go toohello folder where hello telcogenerator.exe and modified telcodatagen.exe.config files are.</span></span>
    * <span data-ttu-id="ab649-168">Spusťte následující příkaz hello:</span><span class="sxs-lookup"><span data-stu-id="ab649-168">Run hello following command:</span></span>

            telcodatagen.exe 1000 .2 2

2. <span data-ttu-id="ab649-169">V hello **dotazu** okně klikněte na tlačítko Další toohello hello tečky `CallStream` vstup a pak vyberte **vzorová data ze vstupu**.</span><span class="sxs-lookup"><span data-stu-id="ab649-169">In hello **Query** blade, click hello dots next toohello `CallStream` input and then select **Sample data from input**.</span></span>

3. <span data-ttu-id="ab649-170">Určete, zda má tři minut za dat a klikněte na tlačítko **OK**.</span><span class="sxs-lookup"><span data-stu-id="ab649-170">Specify that you want three minutes' worth of data and click **OK**.</span></span> <span data-ttu-id="ab649-171">Počkejte, dokud se oznámení, že hello data odebírají vzorky.</span><span class="sxs-lookup"><span data-stu-id="ab649-171">Wait until you're notified that hello data has been sampled.</span></span>

4. <span data-ttu-id="ab649-172">Klikněte na tlačítko **Test** a zajistěte, aby vám výsledky.</span><span class="sxs-lookup"><span data-stu-id="ab649-172">Click **Test** and make sure you're getting results.</span></span>


## <a name="run-hello-job"></a><span data-ttu-id="ab649-173">Spustit úlohu hello</span><span class="sxs-lookup"><span data-stu-id="ab649-173">Run hello job</span></span>

1. <span data-ttu-id="ab649-174">Ujistěte se, že tuto aplikaci TelcoStreaming hello běží.</span><span class="sxs-lookup"><span data-stu-id="ab649-174">Make sure that hello TelcoStreaming app is running.</span></span>

2. <span data-ttu-id="ab649-175">Zavřít hello **dotazu** okno.</span><span class="sxs-lookup"><span data-stu-id="ab649-175">Close hello **Query** blade.</span></span>

3. <span data-ttu-id="ab649-176">V okně úlohy hello, klikněte na tlačítko **spustit**.</span><span class="sxs-lookup"><span data-stu-id="ab649-176">In hello job blade, click **Start**.</span></span>

    ![Spuštění úlohy Stream Analytics hello](./media/stream-analytics-power-bi-dashboard/stream-analytics-sa-job-start-output.png)

<span data-ttu-id="ab649-178">Úlohu streamování Analytics spustí vyhledávání podvodné volání v příchozím datovém proudu hello.</span><span class="sxs-lookup"><span data-stu-id="ab649-178">Your Streaming Analytics job starts looking for fraudulent calls in hello incoming stream.</span></span> <span data-ttu-id="ab649-179">Úloha Hello také vytvoří hello datovou sadu a tabulkou v Power BI a spustí odesílání dat o toothem podvodné volání hello.</span><span class="sxs-lookup"><span data-stu-id="ab649-179">hello job also creates hello dataset and table in Power BI and starts sending data about hello fraudulent calls toothem.</span></span>


## <a name="create-hello-dashboard-in-power-bi"></a><span data-ttu-id="ab649-180">Vytvořit hello řídicí panel v Power BI</span><span class="sxs-lookup"><span data-stu-id="ab649-180">Create hello dashboard in Power BI</span></span>

1. <span data-ttu-id="ab649-181">Přejděte příliš[Powerbi.com](https://powerbi.com) a přihlaste se pomocí svého pracovního nebo školního účtu.</span><span class="sxs-lookup"><span data-stu-id="ab649-181">Go too[Powerbi.com](https://powerbi.com) and sign in with your work or school account.</span></span> <span data-ttu-id="ab649-182">Pokud dotaz úlohy Stream Analytics hello výstupy výsledků, zobrazí vaše datová sada je už vytvořený:</span><span class="sxs-lookup"><span data-stu-id="ab649-182">If hello Stream Analytics job query outputs results, you see that your dataset is already created:</span></span>

    ![Streamovanou datovou sadu v Power BI](./media/stream-analytics-power-bi-dashboard/streaming-dataset.png)

2. <span data-ttu-id="ab649-184">V pracovním prostoru, klikněte na tlačítko  **+ &nbsp;vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="ab649-184">In your workspace, click **+&nbsp;Create**.</span></span>

    ![tlačítko pro vytvoření Hello v prostoru Power BI](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard.png)

3. <span data-ttu-id="ab649-186">Vytvořit nový řídicí panel a pojmenujte ji `Fraudulent Calls`.</span><span class="sxs-lookup"><span data-stu-id="ab649-186">Create a new dashboard and name it `Fraudulent Calls`.</span></span>

    ![Vytvořit řídicí panel a pojmenujte ho v prostoru Power BI](./media/stream-analytics-power-bi-dashboard/pbi-create-dashboard-name.png)

4. <span data-ttu-id="ab649-188">Hello horní části okna hello, klikněte na tlačítko **přidat dlaždice**, vyberte **datových proudů vlastní**a potom klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ab649-188">At hello top of hello window, click **Add tile**, select **CUSTOM STREAMING DATA**, and then click **Next**.</span></span>

    ![Vlastní streamování datové sady](./media/stream-analytics-power-bi-dashboard/custom-streaming-data.png)

5. <span data-ttu-id="ab649-190">V části **si DATSETS**, vyberte datovou sadu a pak klikněte na tlačítko **Další**.</span><span class="sxs-lookup"><span data-stu-id="ab649-190">Under **YOUR DATSETS**, select your dataset and then click **Next**.</span></span>

    ![Vaši streamovanou datovou sadu](./media/stream-analytics-power-bi-dashboard/your-streaming-dataset.png)

6. <span data-ttu-id="ab649-192">V části **typ vizualizace**, vyberte **karty**a potom v hello **pole** seznamu, vyberte **fraudulentcalls**.</span><span class="sxs-lookup"><span data-stu-id="ab649-192">Under **Visualization Type**, select **Card**, and then in hello **Fields** list, select **fraudulentcalls**.</span></span>

    ![Vizualizace podrobnosti nové dlaždice.](./media/stream-analytics-power-bi-dashboard/add-fraud.png)

7. <span data-ttu-id="ab649-194">Klikněte na **Další**.</span><span class="sxs-lookup"><span data-stu-id="ab649-194">Click **Next**.</span></span>

8. <span data-ttu-id="ab649-195">Zadejte podrobnosti o dlaždici jako nadpis a podnadpis.</span><span class="sxs-lookup"><span data-stu-id="ab649-195">Fill in tile details like a title and subtitle.</span></span>

    ![Nadpis a podnadpis nové dlaždice.](./media/stream-analytics-power-bi-dashboard/pbi-new-tile-details.png)

9. <span data-ttu-id="ab649-197">Klikněte na tlačítko **Použít**.</span><span class="sxs-lookup"><span data-stu-id="ab649-197">Click **Apply**.</span></span>

    <span data-ttu-id="ab649-198">Nyní máte čítač podvod!</span><span class="sxs-lookup"><span data-stu-id="ab649-198">Now you have a fraud counter!</span></span>

    ![Čítač podvod](./media/stream-analytics-power-bi-dashboard/fraud-counter.png)

8. <span data-ttu-id="ab649-200">Postupujte podle hello kroky opakujte tooadd dlaždici (počínaje krok 4).</span><span class="sxs-lookup"><span data-stu-id="ab649-200">Follow hello steps again tooadd a tile (starting with step 4).</span></span> <span data-ttu-id="ab649-201">Tentokrát hello následující:</span><span class="sxs-lookup"><span data-stu-id="ab649-201">This time, do hello following:</span></span>

    * <span data-ttu-id="ab649-202">Když získáte příliš**typ vizualizace**, vyberte **spojnicový graf**.</span><span class="sxs-lookup"><span data-stu-id="ab649-202">When you get too**Visualization Type**, select **Line chart**.</span></span> 
    * <span data-ttu-id="ab649-203">Přidejte osy a vyberte **windowend**.</span><span class="sxs-lookup"><span data-stu-id="ab649-203">Add an axis and select **windowend**.</span></span> 
    * <span data-ttu-id="ab649-204">Přidejte hodnotu a vyberte **fraudulentcalls**.</span><span class="sxs-lookup"><span data-stu-id="ab649-204">Add a value and select **fraudulentcalls**.</span></span>
    * <span data-ttu-id="ab649-205">Pro **časové okno toodisplay**, vyberte hello posledních 10 minut.</span><span class="sxs-lookup"><span data-stu-id="ab649-205">For **Time window toodisplay**, select hello last 10 minutes.</span></span>

    ![Vytvoření dlaždici spojnicový graf](./media/stream-analytics-power-bi-dashboard/pbi-create-tile-line-chart.png)

9. <span data-ttu-id="ab649-207">Klikněte na tlačítko **Další**, přidat nadpis a podnadpis a klikněte na **použít**.</span><span class="sxs-lookup"><span data-stu-id="ab649-207">Click **Next**, add a title and subtitle, and click **Apply**.</span></span>

    <span data-ttu-id="ab649-208">řídicí panel Power BI Hello teď umožňuje dvě zobrazení dat o podvodné volání jako zjistil v hello streamovaných dat užitečné.</span><span class="sxs-lookup"><span data-stu-id="ab649-208">hello Power BI dashboard now gives you two views of data about fraudulent calls as detected in hello streaming data.</span></span>

    ![Dokončení zobrazující dvě dlaždice pro podvodné volání řídicí panel Power BI](./media/stream-analytics-power-bi-dashboard/pbi-dashboard-fraudulent-calls-finished.png)


## <a name="learn-more-about-power-bi"></a><span data-ttu-id="ab649-210">Další informace o Power BI</span><span class="sxs-lookup"><span data-stu-id="ab649-210">Learn more about Power BI</span></span>

<span data-ttu-id="ab649-211">Tento kurz ukazuje, jak toocreate pouze několik druhů vizualizace pro datovou sadu.</span><span class="sxs-lookup"><span data-stu-id="ab649-211">This tutorial demonstrates how toocreate only a few kinds of visualizations for a dataset.</span></span> <span data-ttu-id="ab649-212">Power BI můžete vytvořit další nástroje business intelligence zákazníka pro vaši organizaci.</span><span class="sxs-lookup"><span data-stu-id="ab649-212">Power BI can help you create other customer business intelligence tools for your organization.</span></span> <span data-ttu-id="ab649-213">Další nápady najdete hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="ab649-213">For more ideas, see hello following resources:</span></span>

* <span data-ttu-id="ab649-214">Další příklad řídicí panel Power BI, podívejte se na hello [Začínáme s Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) videa.</span><span class="sxs-lookup"><span data-stu-id="ab649-214">For another example of a Power BI dashboard, watch hello [Getting Started with Power BI](https://youtu.be/L-Z_6P56aas?t=1m58s) video.</span></span>
* <span data-ttu-id="ab649-215">Další informace o konfiguraci streamování Analytics úlohy výstup tooPower BI a používání skupin Power BI, zkontrolujte hello [Power BI](stream-analytics-define-outputs.md#power-bi) části hello [Stream Analytics výstupy](stream-analytics-define-outputs.md) článku.</span><span class="sxs-lookup"><span data-stu-id="ab649-215">For more information about configuring Streaming Analytics job output tooPower BI and using Power BI groups, review hello [Power BI](stream-analytics-define-outputs.md#power-bi) section of hello [Stream Analytics outputs](stream-analytics-define-outputs.md) article.</span></span> 
* <span data-ttu-id="ab649-216">Informace o používání Power BI obecně najdete v tématu [řídicí panely v Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).</span><span class="sxs-lookup"><span data-stu-id="ab649-216">For information about using Power BI generally, see [Dashboards in Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).</span></span>


## <a name="learn-about-limitations-and-best-practices"></a><span data-ttu-id="ab649-217">Další informace o omezení a doporučené postupy</span><span class="sxs-lookup"><span data-stu-id="ab649-217">Learn about limitations and best practices</span></span>
<span data-ttu-id="ab649-218">V současné době Power BI je možné volat přibližně jednou za sekundu.</span><span class="sxs-lookup"><span data-stu-id="ab649-218">Currently, Power BI can be called roughly once per second.</span></span> <span data-ttu-id="ab649-219">Streamování vizuály podporu paketů 15 kb.</span><span class="sxs-lookup"><span data-stu-id="ab649-219">Streaming visuals support packets of 15 KB.</span></span> <span data-ttu-id="ab649-220">Kromě toho streamování vizuály nezdaří (ale stále nabízené toowork).</span><span class="sxs-lookup"><span data-stu-id="ab649-220">Beyond that, streaming visuals fail (but push continues toowork).</span></span> <span data-ttu-id="ab649-221">Z důvodu tato omezení Power BI různě konfigurovat nejvíce přirozeně toocases, kde Azure Stream Analytics nepodporuje snížení zatížení významné data.</span><span class="sxs-lookup"><span data-stu-id="ab649-221">Because of these limitations, Power BI lends itself most naturally toocases where Azure Stream Analytics does a significant data load reduction.</span></span> <span data-ttu-id="ab649-222">Doporučujeme používat Přeskakující okno nebo Hopping okno tooensure, datová oznámení jsou maximálně jeden nabízené za sekundu a že váš dotaz pojmenováváme v rámci požadavky na propustnost hello.</span><span class="sxs-lookup"><span data-stu-id="ab649-222">We recommend using a Tumbling window or Hopping window tooensure that data push is at most one push per second, and that your query lands within hello throughput requirements.</span></span>

<span data-ttu-id="ab649-223">Vaše okno můžete použít následující rovnice toocompute hello hodnota toogive hello v sekundách:</span><span class="sxs-lookup"><span data-stu-id="ab649-223">You can use hello following equation toocompute hello value toogive your window in seconds:</span></span>

![Equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

<span data-ttu-id="ab649-225">Například:</span><span class="sxs-lookup"><span data-stu-id="ab649-225">For example:</span></span>

* <span data-ttu-id="ab649-226">Máte 1 000 zařízení odesílání dat v intervalech jednu sekundu.</span><span class="sxs-lookup"><span data-stu-id="ab649-226">You have 1,000 devices sending data at one-second intervals.</span></span>
* <span data-ttu-id="ab649-227">Používáte hello Power BI Pro skladová položka podporující 1 000 000 řádků za hodinu.</span><span class="sxs-lookup"><span data-stu-id="ab649-227">You are using hello Power BI Pro SKU that supports 1,000,000 rows per hour.</span></span>
* <span data-ttu-id="ab649-228">Chcete toopublish hello množství průměr dat na zařízení tooPower BI.</span><span class="sxs-lookup"><span data-stu-id="ab649-228">You want toopublish hello amount of average data per device tooPower BI.</span></span>

<span data-ttu-id="ab649-229">V důsledku toho hello rovnici:</span><span class="sxs-lookup"><span data-stu-id="ab649-229">As a result, hello equation becomes:</span></span>

![Equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

<span data-ttu-id="ab649-231">V této konfiguraci můžete změnit hello původní dotaz toohello následující:</span><span class="sxs-lookup"><span data-stu-id="ab649-231">Given this configuration, you can change hello original query toohello following:</span></span>

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


### <a name="renew-authorization"></a><span data-ttu-id="ab649-232">Obnovit ověřování</span><span class="sxs-lookup"><span data-stu-id="ab649-232">Renew authorization</span></span>
<span data-ttu-id="ab649-233">Pokud došlo ke změně hesla hello vzhledem k tomu, že vaše úlohy vytvoření nebo poslední ověření, musíte tooreauthenticate svůj účet Power BI.</span><span class="sxs-lookup"><span data-stu-id="ab649-233">If hello password has changed since your job was created or last authenticated, you need tooreauthenticate your Power BI account.</span></span> <span data-ttu-id="ab649-234">Pokud Azure Multi-Factor Authentication nakonfigurován v klientovi služby Azure Active Directory (Azure AD), musíte taky toorenew Power BI autorizace každé dva týdny.</span><span class="sxs-lookup"><span data-stu-id="ab649-234">If Azure Multi-Factor Authentication is configured on your Azure Active Directory (Azure AD) tenant, you also need toorenew Power BI authorization every two weeks.</span></span> <span data-ttu-id="ab649-235">Pokud neobnovíte, se může zobrazit příznaky například chybějících výstup úlohy nebo `Authenticate user error` v protokoly operací hello.</span><span class="sxs-lookup"><span data-stu-id="ab649-235">If you don't renew, you could see symptoms such as a lack of job output or an `Authenticate user error` in hello operation logs.</span></span>

<span data-ttu-id="ab649-236">Podobně pokud se úloha spustí po vypršení platnosti tokenu hello, dojde k chybě a hello úloha selže.</span><span class="sxs-lookup"><span data-stu-id="ab649-236">Similarly, if a job starts after hello token has expired, an error occurs and hello job fails.</span></span> <span data-ttu-id="ab649-237">tooresolve tento problém, zastavte hello úlohu, která je spuštěna a přejděte tooyour, které výstup Power BI.</span><span class="sxs-lookup"><span data-stu-id="ab649-237">tooresolve this issue, stop hello job that's running and go tooyour Power BI output.</span></span> <span data-ttu-id="ab649-238">tooavoid ztrátě dat, vyberte hello **obnovit autorizace** propojit a pak restartujte úlohu z hello **naposledy Zastaveno**.</span><span class="sxs-lookup"><span data-stu-id="ab649-238">tooavoid data loss, select hello **Renew authorization** link, and then restart your job from hello **Last Stopped Time**.</span></span>

<span data-ttu-id="ab649-239">Po aktualizaci hello autorizaci s Power BI, Zelená výstraha se zobrazí v hello autorizace oblasti tooreflect, že hello problém byl vyřešen.</span><span class="sxs-lookup"><span data-stu-id="ab649-239">After hello authorization has been refreshed with Power BI, a green alert appears in hello authorization area tooreflect that hello issue has been resolved.</span></span>

## <a name="get-help"></a><span data-ttu-id="ab649-240">Podpora</span><span class="sxs-lookup"><span data-stu-id="ab649-240">Get help</span></span>
<span data-ttu-id="ab649-241">Pro další pomoc, vyzkoušejte naše [fórum Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="ab649-241">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ab649-242">Další kroky</span><span class="sxs-lookup"><span data-stu-id="ab649-242">Next steps</span></span>
* [<span data-ttu-id="ab649-243">Úvod tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ab649-243">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="ab649-244">Začínáme používat službu Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ab649-244">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="ab649-245">Škálování služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ab649-245">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="ab649-246">Azure Stream Analytics query language – referenční informace</span><span class="sxs-lookup"><span data-stu-id="ab649-246">Azure Stream Analytics query language reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="ab649-247">Referenční dokumentace Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="ab649-247">Azure Stream Analytics Management REST API reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)
