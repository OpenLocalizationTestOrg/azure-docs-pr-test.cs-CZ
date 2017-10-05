---
title: "Vizualizace dat v reálném čase s daty ze snímačů z Azure IoT Hub – Power BI | Microsoft Docs"
description: "Pomocí Power BI vizualizovat data teploty a vlhkosti, který se shromažďují ze senzoru a odesílá do služby Azure IoT hub."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "vizualizace dat reálném čase, vizualizace dat za provozu, vizualizace dat snímačů"
ms.assetid: e67c9c09-6219-4f0f-ad42-58edaaa74f61
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: xshi
ms.openlocfilehash: b190fea06ffc2406d781c7edad091f097cca9c2d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a><span data-ttu-id="fc6c3-104">Vizualizovat data snímačů v reálném čase ze služby Azure IoT Hub pomocí Power BI</span><span class="sxs-lookup"><span data-stu-id="fc6c3-104">Visualize real-time sensor data from Azure IoT Hub using Power BI</span></span>

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/4.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="fc6c3-106">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="fc6c3-106">What you learn</span></span>

<span data-ttu-id="fc6c3-107">Zjistíte, jak k vizualizaci dat snímačů v reálném čase, který Azure IoT hub přijímá pomocí Power BI.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-107">You learn how to visualize real-time sensor data that your Azure IoT hub receives by Power BI.</span></span> <span data-ttu-id="fc6c3-108">Pokud budete chtít zkusit znázorňovat data ve službě IoT hub s webovými aplikacemi, naleznete v tématu [použití Azure Web Apps k vizualizaci dat snímačů v reálném čase ze služby Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="fc6c3-108">If you want to try visualize the data in your IoT hub with Web Apps, please see [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="fc6c3-109">Co dělat</span><span class="sxs-lookup"><span data-stu-id="fc6c3-109">What you do</span></span>

- <span data-ttu-id="fc6c3-110">Služby IoT hub připravte pro přístup k datům přidáním skupiny příjemců.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-110">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="fc6c3-111">Vytvoření, konfiguraci a spusťte úlohu služby Stream Analytics pro přenos dat ze služby IoT hub ke svému účtu Power BI.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-111">Create, configure and run a Stream Analytics job for data transfer from your IoT hub to your Power BI account.</span></span>
- <span data-ttu-id="fc6c3-112">Vytvoření a publikování sestavy Power BI můžete vizualizovat data.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-112">Create and publish a Power BI report to visualize the data.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="fc6c3-113">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="fc6c3-113">What you need</span></span>

- <span data-ttu-id="fc6c3-114">Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="fc6c3-114">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers the following requirements:</span></span>
  - <span data-ttu-id="fc6c3-115">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-115">An active Azure subscription.</span></span>
  - <span data-ttu-id="fc6c3-116">V rámci svého předplatného služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-116">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="fc6c3-117">Klientská aplikace, která odesílá zprávy do služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-117">A client application that sends messages to your Azure IoT hub.</span></span>
- <span data-ttu-id="fc6c3-118">Účet Power BI.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-118">A Power BI account.</span></span> <span data-ttu-id="fc6c3-119">([Vyzkoušejte zdarma Power BI](https://powerbi.microsoft.com/))</span><span class="sxs-lookup"><span data-stu-id="fc6c3-119">([Try Power BI for free](https://powerbi.microsoft.com/))</span></span>

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a><span data-ttu-id="fc6c3-120">Vytvoření, konfigurace a spuštění úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="fc6c3-120">Create, configure, and run a Stream Analytics job</span></span>

### <a name="create-a-stream-analytics-job"></a><span data-ttu-id="fc6c3-121">Vytvoření úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="fc6c3-121">Create a Stream Analytics job</span></span>

1. <span data-ttu-id="fc6c3-122">Na portálu Azure klikněte na nový > Internet věcí > úlohy služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-122">In the Azure portal, click New > Internet of Things > Stream Analytics job.</span></span>
1. <span data-ttu-id="fc6c3-123">Zadejte následující informace pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-123">Enter the following information for the job.</span></span>

   <span data-ttu-id="fc6c3-124">**Název úlohy**: název úlohy.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-124">**Job name**: The name of the job.</span></span> <span data-ttu-id="fc6c3-125">Název musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-125">The name must be globally unique.</span></span>

   <span data-ttu-id="fc6c3-126">**Skupina prostředků**: použijte stejnou skupinu prostředků, která používá službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-126">**Resource group**: Use the same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="fc6c3-127">**Umístění**: používalo stejné umístění jako vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-127">**Location**: Use the same location as your resource group.</span></span>

   <span data-ttu-id="fc6c3-128">**Připnout na řídicí panel**: zaškrtnete tuto možnost pro snadný přístup do služby IoT hub z řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-128">**Pin to dashboard**: Check this option for easy access to your IoT hub from the dashboard.</span></span>

   ![Vytvořit úlohu služby Stream Analytics v Azure](media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

1. <span data-ttu-id="fc6c3-130">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-130">Click **Create**.</span></span>

### <a name="add-an-input-to-the-stream-analytics-job"></a><span data-ttu-id="fc6c3-131">Přidat vstup do úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="fc6c3-131">Add an input to the Stream Analytics job</span></span>

1. <span data-ttu-id="fc6c3-132">Spusťte úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-132">Open the Stream Analytics job.</span></span>
1. <span data-ttu-id="fc6c3-133">V části **úlohy topologie**, klikněte na tlačítko **vstupy**.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-133">Under **Job Topology**, click **Inputs**.</span></span>
1. <span data-ttu-id="fc6c3-134">V **vstupy** podokně klikněte na tlačítko **přidat**a potom zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="fc6c3-134">In the **Inputs** pane, click **Add**, and then enter the following information:</span></span>

   <span data-ttu-id="fc6c3-135">**Vstupní alias**: jedinečný odkaz pro vstup.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-135">**Input alias**: The unique alias for the input.</span></span>

   <span data-ttu-id="fc6c3-136">**Zdroj**: vyberte **služby IoT hub**.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-136">**Source**: Select **IoT hub**.</span></span>

   <span data-ttu-id="fc6c3-137">**Skupiny příjemců**: Vyberte skupinu příjemců, kterou jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-137">**Consumer group**: Select the consumer group you just created.</span></span>
1. <span data-ttu-id="fc6c3-138">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-138">Click **Create**.</span></span>

   ![Přidat vstup do úlohy Stream Analytics v Azure](media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-to-the-stream-analytics-job"></a><span data-ttu-id="fc6c3-140">Přidat výstup do úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="fc6c3-140">Add an output to the Stream Analytics job</span></span>

1. <span data-ttu-id="fc6c3-141">V části **úlohy topologie**, klikněte na tlačítko **výstupy**.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-141">Under **Job Topology**, click **Outputs**.</span></span>
1. <span data-ttu-id="fc6c3-142">V **výstupy** podokně klikněte na tlačítko **přidat**a potom zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="fc6c3-142">In the **Outputs** pane, click **Add**, and then enter the following information:</span></span>

   <span data-ttu-id="fc6c3-143">**Alias pro výstup**: jedinečný alias pro výstup.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-143">**Output alias**: The unique alias for the output.</span></span>

   <span data-ttu-id="fc6c3-144">**Jímky**: vyberte **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-144">**Sink**: Select **Power BI**.</span></span>
1. <span data-ttu-id="fc6c3-145">Klikněte na tlačítko **Authorize**a pak se přihlaste ke svému účtu Power BI.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-145">Click **Authorize**, and then sign into your Power BI account.</span></span>
1. <span data-ttu-id="fc6c3-146">Po ověření, zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="fc6c3-146">Once authorized, enter the following information:</span></span>

   <span data-ttu-id="fc6c3-147">**Skupina pracovního prostoru**: Vyberte pracovní prostor cílové skupiny.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-147">**Group Workspace**: Select your target group workspace.</span></span>

   <span data-ttu-id="fc6c3-148">**Název datové sady**: Zadejte název datové sady.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-148">**Dataset Name**: Enter a dataset name.</span></span>

   <span data-ttu-id="fc6c3-149">**Název tabulky**: Zadejte název tabulky.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-149">**Table Name**: Enter a table name.</span></span>
1. <span data-ttu-id="fc6c3-150">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-150">Click **Create**.</span></span>

   ![Přidat výstup do úlohy Stream Analytics v Azure](media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-the-query-of-the-stream-analytics-job"></a><span data-ttu-id="fc6c3-152">Konfigurace dotazu úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="fc6c3-152">Configure the query of the Stream Analytics job</span></span>

1. <span data-ttu-id="fc6c3-153">V části **úlohy topologie**, klikněte na tlačítko **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-153">Under **Job Topology**, click **Query**.</span></span>
1. <span data-ttu-id="fc6c3-154">Nahraďte `[YourInputAlias]` s alias vstupu úlohy.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-154">Replace `[YourInputAlias]` with the input alias of the job.</span></span>
1. <span data-ttu-id="fc6c3-155">Nahraďte `[YourOutputAlias]` s aliasem výstupu úlohy.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-155">Replace `[YourOutputAlias]` with the output alias of the job.</span></span>
1. <span data-ttu-id="fc6c3-156">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-156">Click **Save**.</span></span>

   ![Přidat dotaz do úlohy Stream Analytics v Azure](media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-the-stream-analytics-job"></a><span data-ttu-id="fc6c3-158">Spustit úlohu služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="fc6c3-158">Run the Stream Analytics job</span></span>

<span data-ttu-id="fc6c3-159">V úloze Stream Analytics, klikněte na tlačítko **spustit** > **nyní** > **spustit**.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-159">In the Stream Analytics job, click **Start** > **Now** > **Start**.</span></span> <span data-ttu-id="fc6c3-160">Jakmile se úloha úspěšně spustí, stav úlohy změní z **Zastaveno** k **systémem**.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-160">Once the job successfully starts, the job status changes from **Stopped** to **Running**.</span></span>

![Spustit úlohu služby Stream Analytics v Azure](media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-to-visualize-the-data"></a><span data-ttu-id="fc6c3-162">Vytvoření a publikování sestavy Power BI k vizualizaci dat</span><span class="sxs-lookup"><span data-stu-id="fc6c3-162">Create and publish a Power BI report to visualize the data</span></span>

1. <span data-ttu-id="fc6c3-163">Zkontrolujte, jestli že ukázkové aplikace běží na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-163">Ensure the sample application is running on your device.</span></span> <span data-ttu-id="fc6c3-164">Pokud není, můžete se podívat do kurzy pod [nastavit vaše zařízení](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).</span><span class="sxs-lookup"><span data-stu-id="fc6c3-164">If not, you can refer to the tutorials under [Setup your device](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).</span></span>
1. <span data-ttu-id="fc6c3-165">Přihlaste se k vaší [Power BI](https://powerbi.microsoft.com/en-us/) účtu.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-165">Sign in to your [Power BI](https://powerbi.microsoft.com/en-us/) account.</span></span>
1. <span data-ttu-id="fc6c3-166">Přejděte do pracovního prostoru skupiny, které jste nastavili při vytváření výstupu pro úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-166">Go to the group workspace that you set when you created the output for the Stream Analytics job.</span></span>
1. <span data-ttu-id="fc6c3-167">Klikněte na tlačítko **streamování datové sady**.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-167">Click **Streaming datasets**.</span></span>

   <span data-ttu-id="fc6c3-168">Měli byste vidět uvedené datovou sadu, která jste zadali při vytváření výstupu pro úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-168">You should see the listed dataset that you specified when you created the output for the Stream Analytics job.</span></span>
1. <span data-ttu-id="fc6c3-169">V části **akce**, klikněte na ikonu první pro vytvoření sestavy.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-169">Under **ACTIONS**, click the first icon to create a report.</span></span>

   ![Vytvoření sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

1. <span data-ttu-id="fc6c3-171">Vytvořte na spojnicový graf a zobrazit v reálném čase teploty v čase.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-171">Create a line chart to show real-time temperature over time.</span></span>
   1. <span data-ttu-id="fc6c3-172">Na stránce pro vytvoření sestavy přidáte spojnicový graf.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-172">On the report creation page, add a line chart.</span></span>
   1. <span data-ttu-id="fc6c3-173">Na **pole** podokně rozbalte položku v tabulce, kterou jste zadali při vytváření výstupu pro úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-173">On the **Fields** pane, expand the table that you specified when you created the output for the Stream Analytics job.</span></span>
   1. <span data-ttu-id="fc6c3-174">Přetáhněte **EventEnqueuedUtcTime** k **osy** na **vizualizace** podokně.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-174">Drag **EventEnqueuedUtcTime** to **Axis** on the **Visualizations** pane.</span></span>
   1. <span data-ttu-id="fc6c3-175">Přetáhněte **teploty** k **hodnoty**.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-175">Drag **temperature** to **Values**.</span></span>

      <span data-ttu-id="fc6c3-176">Teď se vytvoří spojnicový graf.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-176">Now a line chart is created.</span></span> <span data-ttu-id="fc6c3-177">Osy x zobrazí datum a čas v časovém pásmu UTC.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-177">The x-axis displays date and time in the UTC time zone.</span></span> <span data-ttu-id="fc6c3-178">Osy y zobrazí z senzoru teploty.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-178">The y-axis displays temperature from the sensor.</span></span>

      ![Přidání spojnicový graf pro teploty do sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

1. <span data-ttu-id="fc6c3-180">Vytvořte jiný spojnicový graf zobrazíte v reálném čase vlhkosti v čase.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-180">Create another line chart to show real-time humidity over time.</span></span> <span data-ttu-id="fc6c3-181">To pokud chcete udělat, postupujte podle stejných kroků výše a umístěte **EventEnqueuedUtcTime** na ose x a **vlhkosti** na ose y.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-181">To do this, follow the same steps above and place **EventEnqueuedUtcTime** on the x-axis and **humidity** on the y-axis.</span></span>

   ![Přidání spojnicový graf pro vlhkosti do sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

1. <span data-ttu-id="fc6c3-183">Klikněte na tlačítko **Uložit** pro uložení sestavy.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-183">Click **Save** to save the report.</span></span>
1. <span data-ttu-id="fc6c3-184">Klikněte na tlačítko **soubor** > **publikovat na webu**.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-184">Click **File** > **Publish to web**.</span></span>
1. <span data-ttu-id="fc6c3-185">Klikněte na tlačítko **kód pro vložení vytvořit**a potom klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-185">Click **Create embed code**, and then click **Publish**.</span></span>

<span data-ttu-id="fc6c3-186">Odkaz na sestavu jste zadali, že můžete sdílet s kýmkoli pro přístup k sestavě a fragmentu kódu pro integraci sestavy na blog nebo Web.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-186">You're provided the report link that you can share with anyone for report access and a code snippet to integrate the report into your blog or website.</span></span>

![Publikovat sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

<span data-ttu-id="fc6c3-188">Společnost Microsoft nabízí [mobilních aplikací Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) pro zobrazení a interakci s řídicí panely Power BI a sestavy na vašem mobilním zařízení.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-188">Microsoft also offers the [Power BI mobile apps](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) for viewing and interacting with your Power BI dashboards and reports on your mobile device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fc6c3-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fc6c3-189">Next steps</span></span>

<span data-ttu-id="fc6c3-190">Power BI jste úspěšně použili k vizualizaci dat snímačů v reálném čase ze služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-190">You’ve successfully used Power BI to visualize real-time sensor data from your Azure IoT hub.</span></span>
<span data-ttu-id="fc6c3-191">Neexistuje jiný způsob, jak k vizualizaci dat ze služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="fc6c3-191">There is an alternate way to visualize data from Azure IoT Hub.</span></span> <span data-ttu-id="fc6c3-192">V tématu [použití Azure Web Apps k vizualizaci dat snímačů v reálném čase ze služby Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="fc6c3-192">See [Use Azure Web Apps to visualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
