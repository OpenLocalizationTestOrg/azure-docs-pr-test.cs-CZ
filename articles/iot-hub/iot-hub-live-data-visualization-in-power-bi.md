---
title: "vizualizace dat aaaReal čas dat snímačů z Azure IoT Hub – Power BI | Microsoft Docs"
description: "Pomocí Power BI toovisualize teploty a vlhkosti data, která se shromažďují ze snímačů hello a odesílá tooyour Azure IoT hub."
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
ms.openlocfilehash: d79ce757a9f2ab7a4744e8a0c523106e0f72cecd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-real-time-sensor-data-from-azure-iot-hub-using-power-bi"></a><span data-ttu-id="1e5b3-104">Vizualizovat data snímačů v reálném čase ze služby Azure IoT Hub pomocí Power BI</span><span class="sxs-lookup"><span data-stu-id="1e5b3-104">Visualize real-time sensor data from Azure IoT Hub using Power BI</span></span>

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/4.png)


[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="1e5b3-106">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="1e5b3-106">What you learn</span></span>

<span data-ttu-id="1e5b3-107">Zjistíte, jak toovisualize snímačů v reálném čase data, která Azure IoT hub přijímá pomocí Power BI.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-107">You learn how toovisualize real-time sensor data that your Azure IoT hub receives by Power BI.</span></span> <span data-ttu-id="1e5b3-108">Pokud chcete vizualizovat tootry hello data ve službě IoT hub s webovými aplikacemi, najdete v tématu [dat snímačů v reálném čase pomocí Azure Web Apps toovisualize ze služby Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="1e5b3-108">If you want tootry visualize hello data in your IoT hub with Web Apps, please see [Use Azure Web Apps toovisualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span></span>

## <a name="what-you-do"></a><span data-ttu-id="1e5b3-109">Co dělat</span><span class="sxs-lookup"><span data-stu-id="1e5b3-109">What you do</span></span>

- <span data-ttu-id="1e5b3-110">Služby IoT hub připravte pro přístup k datům přidáním skupiny příjemců.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-110">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="1e5b3-111">Vytvoření, konfiguraci a spusťte úlohu služby Stream Analytics pro přenos dat z vaší IoT hub tooyour účet Power BI.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-111">Create, configure and run a Stream Analytics job for data transfer from your IoT hub tooyour Power BI account.</span></span>
- <span data-ttu-id="1e5b3-112">Vytvoření a publikování dat hello toovisualize sestavy Power BI.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-112">Create and publish a Power BI report toovisualize hello data.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="1e5b3-113">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="1e5b3-113">What you need</span></span>

- <span data-ttu-id="1e5b3-114">Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="1e5b3-114">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers hello following requirements:</span></span>
  - <span data-ttu-id="1e5b3-115">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-115">An active Azure subscription.</span></span>
  - <span data-ttu-id="1e5b3-116">V rámci svého předplatného služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-116">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="1e5b3-117">Klientská aplikace, která odesílá zprávy tooyour Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-117">A client application that sends messages tooyour Azure IoT hub.</span></span>
- <span data-ttu-id="1e5b3-118">Účet Power BI.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-118">A Power BI account.</span></span> <span data-ttu-id="1e5b3-119">([Vyzkoušejte zdarma Power BI](https://powerbi.microsoft.com/))</span><span class="sxs-lookup"><span data-stu-id="1e5b3-119">([Try Power BI for free](https://powerbi.microsoft.com/))</span></span>

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a><span data-ttu-id="1e5b3-120">Vytvoření, konfigurace a spuštění úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="1e5b3-120">Create, configure, and run a Stream Analytics job</span></span>

### <a name="create-a-stream-analytics-job"></a><span data-ttu-id="1e5b3-121">Vytvoření úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="1e5b3-121">Create a Stream Analytics job</span></span>

1. <span data-ttu-id="1e5b3-122">V hello portálu Azure, klikněte na nový > Internet věcí > úlohy služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-122">In hello Azure portal, click New > Internet of Things > Stream Analytics job.</span></span>
1. <span data-ttu-id="1e5b3-123">Zadejte následující informace pro úlohu hello hello.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-123">Enter hello following information for hello job.</span></span>

   <span data-ttu-id="1e5b3-124">**Název úlohy**: název hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-124">**Job name**: hello name of hello job.</span></span> <span data-ttu-id="1e5b3-125">Název Hello musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-125">hello name must be globally unique.</span></span>

   <span data-ttu-id="1e5b3-126">**Skupina prostředků**: použití hello stejné skupiny prostředků, která používá službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-126">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="1e5b3-127">**Umístění**: použití hello stejné umístění jako vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-127">**Location**: Use hello same location as your resource group.</span></span>

   <span data-ttu-id="1e5b3-128">**PIN kód toodashboard**: zaškrtnete tuto možnost pro Centrum IoT tooyour snadný přístup z řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-128">**Pin toodashboard**: Check this option for easy access tooyour IoT hub from hello dashboard.</span></span>

   ![Vytvořit úlohu služby Stream Analytics v Azure](media/iot-hub-live-data-visualization-in-power-bi/2_create-stream-analytics-job-azure.png)

1. <span data-ttu-id="1e5b3-130">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-130">Click **Create**.</span></span>

### <a name="add-an-input-toohello-stream-analytics-job"></a><span data-ttu-id="1e5b3-131">Přidat úloha Stream Analytics vstupní toohello</span><span class="sxs-lookup"><span data-stu-id="1e5b3-131">Add an input toohello Stream Analytics job</span></span>

1. <span data-ttu-id="1e5b3-132">Úloha Stream Analytics otevřete hello.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-132">Open hello Stream Analytics job.</span></span>
1. <span data-ttu-id="1e5b3-133">V části **úlohy topologie**, klikněte na tlačítko **vstupy**.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-133">Under **Job Topology**, click **Inputs**.</span></span>
1. <span data-ttu-id="1e5b3-134">V hello **vstupy** podokně klikněte na tlačítko **přidat**a pak zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="1e5b3-134">In hello **Inputs** pane, click **Add**, and then enter hello following information:</span></span>

   <span data-ttu-id="1e5b3-135">**Vstupní alias**: hello jedinečný odkaz pro vstup hello.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-135">**Input alias**: hello unique alias for hello input.</span></span>

   <span data-ttu-id="1e5b3-136">**Zdroj**: vyberte **služby IoT hub**.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-136">**Source**: Select **IoT hub**.</span></span>

   <span data-ttu-id="1e5b3-137">**Skupiny příjemců**: Skupina uživatelů vyberte hello jste právě vytvořili.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-137">**Consumer group**: Select hello consumer group you just created.</span></span>
1. <span data-ttu-id="1e5b3-138">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-138">Click **Create**.</span></span>

   ![Přidat úloha Stream Analytics vstupní tooa v Azure](media/iot-hub-live-data-visualization-in-power-bi/3_add-input-to-stream-analytics-job-azure.png)

### <a name="add-an-output-toohello-stream-analytics-job"></a><span data-ttu-id="1e5b3-140">Přidat úloha Stream Analytics toohello výstup</span><span class="sxs-lookup"><span data-stu-id="1e5b3-140">Add an output toohello Stream Analytics job</span></span>

1. <span data-ttu-id="1e5b3-141">V části **úlohy topologie**, klikněte na tlačítko **výstupy**.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-141">Under **Job Topology**, click **Outputs**.</span></span>
1. <span data-ttu-id="1e5b3-142">V hello **výstupy** podokně klikněte na tlačítko **přidat**a pak zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="1e5b3-142">In hello **Outputs** pane, click **Add**, and then enter hello following information:</span></span>

   <span data-ttu-id="1e5b3-143">**Alias pro výstup**: hello jedinečný odkaz pro výstup hello.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-143">**Output alias**: hello unique alias for hello output.</span></span>

   <span data-ttu-id="1e5b3-144">**Jímky**: vyberte **Power BI**.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-144">**Sink**: Select **Power BI**.</span></span>
1. <span data-ttu-id="1e5b3-145">Klikněte na tlačítko **Authorize**a pak se přihlaste ke svému účtu Power BI.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-145">Click **Authorize**, and then sign into your Power BI account.</span></span>
1. <span data-ttu-id="1e5b3-146">Po ověření, zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="1e5b3-146">Once authorized, enter hello following information:</span></span>

   <span data-ttu-id="1e5b3-147">**Skupina pracovního prostoru**: Vyberte pracovní prostor cílové skupiny.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-147">**Group Workspace**: Select your target group workspace.</span></span>

   <span data-ttu-id="1e5b3-148">**Název datové sady**: Zadejte název datové sady.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-148">**Dataset Name**: Enter a dataset name.</span></span>

   <span data-ttu-id="1e5b3-149">**Název tabulky**: Zadejte název tabulky.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-149">**Table Name**: Enter a table name.</span></span>
1. <span data-ttu-id="1e5b3-150">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-150">Click **Create**.</span></span>

   ![Přidat úloha Stream Analytics tooa výstupu v Azure](media/iot-hub-live-data-visualization-in-power-bi/4_add-output-to-stream-analytics-job-azure.png)

### <a name="configure-hello-query-of-hello-stream-analytics-job"></a><span data-ttu-id="1e5b3-152">Konfigurace hello dotazu úlohy Stream Analytics hello</span><span class="sxs-lookup"><span data-stu-id="1e5b3-152">Configure hello query of hello Stream Analytics job</span></span>

1. <span data-ttu-id="1e5b3-153">V části **úlohy topologie**, klikněte na tlačítko **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-153">Under **Job Topology**, click **Query**.</span></span>
1. <span data-ttu-id="1e5b3-154">Nahraďte `[YourInputAlias]` s aliasem hello vstupní hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-154">Replace `[YourInputAlias]` with hello input alias of hello job.</span></span>
1. <span data-ttu-id="1e5b3-155">Nahraďte `[YourOutputAlias]` s alias pro výstup hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-155">Replace `[YourOutputAlias]` with hello output alias of hello job.</span></span>
1. <span data-ttu-id="1e5b3-156">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-156">Click **Save**.</span></span>

   ![Přidat úloha Stream Analytics tooa dotazu v Azure](media/iot-hub-live-data-visualization-in-power-bi/5_add-query-stream-analytics-job-azure.png)

### <a name="run-hello-stream-analytics-job"></a><span data-ttu-id="1e5b3-158">Spustit úlohu služby Stream Analytics hello</span><span class="sxs-lookup"><span data-stu-id="1e5b3-158">Run hello Stream Analytics job</span></span>

<span data-ttu-id="1e5b3-159">V úloze Stream Analytics hello, klikněte na tlačítko **spustit** > **nyní** > **spustit**.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-159">In hello Stream Analytics job, click **Start** > **Now** > **Start**.</span></span> <span data-ttu-id="1e5b3-160">Jakmile hello úloha úspěšně spustí, změní se stav úlohy hello ze **Zastaveno** příliš**systémem**.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-160">Once hello job successfully starts, hello job status changes from **Stopped** too**Running**.</span></span>

![Spustit úlohu služby Stream Analytics v Azure](media/iot-hub-live-data-visualization-in-power-bi/6_run-stream-analytics-job-azure.png)

## <a name="create-and-publish-a-power-bi-report-toovisualize-hello-data"></a><span data-ttu-id="1e5b3-162">Vytvoření a publikování dat hello toovisualize sestavy Power BI</span><span class="sxs-lookup"><span data-stu-id="1e5b3-162">Create and publish a Power BI report toovisualize hello data</span></span>

1. <span data-ttu-id="1e5b3-163">Zkontrolujte, jestli hello ukázkové aplikace běží na vašem zařízení.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-163">Ensure hello sample application is running on your device.</span></span> <span data-ttu-id="1e5b3-164">Pokud ne, může odkazovat toohello kurzy pod [nastavit vaše zařízení](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).</span><span class="sxs-lookup"><span data-stu-id="1e5b3-164">If not, you can refer toohello tutorials under [Setup your device](https://docs.microsoft.com/azure/iot-hub/iot-hub-raspberry-pi-kit-node-get-started).</span></span>
1. <span data-ttu-id="1e5b3-165">Přihlaste se tooyour [Power BI](https://powerbi.microsoft.com/en-us/) účtu.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-165">Sign in tooyour [Power BI](https://powerbi.microsoft.com/en-us/) account.</span></span>
1. <span data-ttu-id="1e5b3-166">Přejděte toohello pracovní prostor skupiny, který jste nastavili při vytváření hello výstup úlohy služby Stream Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-166">Go toohello group workspace that you set when you created hello output for hello Stream Analytics job.</span></span>
1. <span data-ttu-id="1e5b3-167">Klikněte na tlačítko **streamování datové sady**.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-167">Click **Streaming datasets**.</span></span>

   <span data-ttu-id="1e5b3-168">Měli byste vidět hello uvedené datovou sadu, která jste zadali při vytvoření hello výstup úlohy Stream Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-168">You should see hello listed dataset that you specified when you created hello output for hello Stream Analytics job.</span></span>
1. <span data-ttu-id="1e5b3-169">V části **akce**, klikněte na tlačítko hello první ikona toocreate sestavy.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-169">Under **ACTIONS**, click hello first icon toocreate a report.</span></span>

   ![Vytvoření sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/7_create-power-bi-report-microsoft.png)

1. <span data-ttu-id="1e5b3-171">Vytvořte řádku grafu tooshow v reálném čase teplotu v průběhu času.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-171">Create a line chart tooshow real-time temperature over time.</span></span>
   1. <span data-ttu-id="1e5b3-172">Na stránce vytváření sestav hello přidáte spojnicový graf.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-172">On hello report creation page, add a line chart.</span></span>
   1. <span data-ttu-id="1e5b3-173">Na hello **pole** podokně rozbalte hello tabulku, kterou jste zadali při vytvoření hello výstup úlohy služby Stream Analytics hello.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-173">On hello **Fields** pane, expand hello table that you specified when you created hello output for hello Stream Analytics job.</span></span>
   1. <span data-ttu-id="1e5b3-174">Přetáhněte **EventEnqueuedUtcTime** příliš**osy** na hello **vizualizace** podokně.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-174">Drag **EventEnqueuedUtcTime** too**Axis** on hello **Visualizations** pane.</span></span>
   1. <span data-ttu-id="1e5b3-175">Přetáhněte **teploty** příliš**hodnoty**.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-175">Drag **temperature** too**Values**.</span></span>

      <span data-ttu-id="1e5b3-176">Teď se vytvoří spojnicový graf.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-176">Now a line chart is created.</span></span> <span data-ttu-id="1e5b3-177">osy x Hello zobrazí datum a čas v časovém pásmu UTC hello.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-177">hello x-axis displays date and time in hello UTC time zone.</span></span> <span data-ttu-id="1e5b3-178">osy y Hello zobrazí teploty ze snímačů hello.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-178">hello y-axis displays temperature from hello sensor.</span></span>

      ![Přidat spojnicový graf pro teploty tooa sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/8_add-line-chart-for-temperature-to-power-bi-report-microsoft.png)

1. <span data-ttu-id="1e5b3-180">Vytvořte další řádek grafu tooshow v reálném čase vlhkosti v čase.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-180">Create another line chart tooshow real-time humidity over time.</span></span> <span data-ttu-id="1e5b3-181">toodo, postupujte podle hello stejné kroky výše a umístěte **EventEnqueuedUtcTime** na ose x hello a **vlhkosti** na ose y hello.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-181">toodo this, follow hello same steps above and place **EventEnqueuedUtcTime** on hello x-axis and **humidity** on hello y-axis.</span></span>

   ![Přidat spojnicový graf pro vlhkosti tooa sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/9_add-line-chart-for-humidity-to-power-bi-report-microsoft.png)

1. <span data-ttu-id="1e5b3-183">Klikněte na tlačítko **Uložit** toosave hello sestavy.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-183">Click **Save** toosave hello report.</span></span>
1. <span data-ttu-id="1e5b3-184">Klikněte na tlačítko **soubor** > **publikování tooweb**.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-184">Click **File** > **Publish tooweb**.</span></span>
1. <span data-ttu-id="1e5b3-185">Klikněte na tlačítko **kód pro vložení vytvořit**a potom klikněte na **publikovat**.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-185">Click **Create embed code**, and then click **Publish**.</span></span>

<span data-ttu-id="1e5b3-186">Jste zadat odkaz na sestavu hello, které můžete sdílet s kýmkoli pro přístup k sestavě a sestavy hello toointegrate fragment kódu do blogu nebo Web.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-186">You're provided hello report link that you can share with anyone for report access and a code snippet toointegrate hello report into your blog or website.</span></span>

![Publikovat sestavy Microsoft Power BI](media/iot-hub-live-data-visualization-in-power-bi/10_publish-power-bi-report-microsoft.png)

<span data-ttu-id="1e5b3-188">Společnost Microsoft nabízí hello [mobilních aplikací Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) pro zobrazení a interakci s řídicí panely Power BI a sestavy na vašem mobilním zařízení.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-188">Microsoft also offers hello [Power BI mobile apps](https://powerbi.microsoft.com/en-us/documentation/powerbi-power-bi-apps-for-mobile-devices/) for viewing and interacting with your Power BI dashboards and reports on your mobile device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1e5b3-189">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1e5b3-189">Next steps</span></span>

<span data-ttu-id="1e5b3-190">Úspěšně jste použili Power BI toovisualize snímačů v reálném čase data ze služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-190">You’ve successfully used Power BI toovisualize real-time sensor data from your Azure IoT hub.</span></span>
<span data-ttu-id="1e5b3-191">Nejsou jiný způsob, jak toovisualize data ze služby Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1e5b3-191">There is an alternate way toovisualize data from Azure IoT Hub.</span></span> <span data-ttu-id="1e5b3-192">V tématu [dat snímačů v reálném čase pomocí Azure Web Apps toovisualize ze služby Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span><span class="sxs-lookup"><span data-stu-id="1e5b3-192">See [Use Azure Web Apps toovisualize real-time sensor data from Azure IoT Hub](iot-hub-live-data-visualization-in-web-apps.md).</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
