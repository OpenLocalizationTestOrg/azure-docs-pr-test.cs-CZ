---
title: "Počasí prognózy pomocí Azure Machine Learning s daty ze služby IoT Hub | Microsoft Docs"
description: "Použití Azure Machine Learning k předvídání riziko déšť podle služby IoT hub shromažďuje ze senzoru teploty a vlhkosti data."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "předpověď počasí machine learning"
ms.assetid: 8ba7d9e7-699c-4448-b353-0f3e1429d198
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: xshi
ms.openlocfilehash: 50ae54b9476c49b80236e295c0bf244df8236cff
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="weather-forecast-using-the-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a><span data-ttu-id="27df2-104">Počasí prognózy používající senzor data ze služby IoT hub v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="27df2-104">Weather forecast using the sensor data from your IoT hub in Azure Machine Learning</span></span>

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="27df2-106">Machine learning je technika, kdy vědecké zpracování dat, která pomáhá počítače informace z existující data předpovídat budoucí chování, výsledky a trendy.</span><span class="sxs-lookup"><span data-stu-id="27df2-106">Machine learning is a technique of data science that helps computers learn from existing data to forecast future behaviors, outcomes, and trends.</span></span> <span data-ttu-id="27df2-107">Azure Machine Learning je cloudová služba pro prediktivní analýzu, která umožňuje rychle vytvářet a nasazovat prediktivní modely jako analytická řešení.</span><span class="sxs-lookup"><span data-stu-id="27df2-107">Azure Machine Learning is a cloud predictive analytics service that makes it possible to quickly create and deploy predictive models as analytics solutions.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="27df2-108">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="27df2-108">What you learn</span></span>

<span data-ttu-id="27df2-109">Naučte se používat Azure Machine Learning na informace o počasí prognózy (riziko déšť) používající teploty a vlhkosti data ze služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="27df2-109">You learn how to use Azure Machine Learning to do weather forecast (chance of rain) using the temperature and humidity data from your Azure IoT hub.</span></span> <span data-ttu-id="27df2-110">Riziko déšť je výstup z modelu předpovědi počasí připravené.</span><span class="sxs-lookup"><span data-stu-id="27df2-110">The chance of rain is the output of a prepared weather prediction model.</span></span> <span data-ttu-id="27df2-111">Model je založena na historických dat prognózy riziko déšť vychází z teploty a vlhkosti.</span><span class="sxs-lookup"><span data-stu-id="27df2-111">The model is built upon historic data to forecast chance of rain based on temperature and humidity.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="27df2-112">Co dělat</span><span class="sxs-lookup"><span data-stu-id="27df2-112">What you do</span></span>

- <span data-ttu-id="27df2-113">Model předpovědi počasí nasaďte jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="27df2-113">Deploy the weather prediction model as a web service.</span></span>
- <span data-ttu-id="27df2-114">Služby IoT hub připravte pro přístup k datům přidáním skupiny příjemců.</span><span class="sxs-lookup"><span data-stu-id="27df2-114">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="27df2-115">Vytvořit úlohu služby Stream Analytics a nakonfigurujte úlohy:</span><span class="sxs-lookup"><span data-stu-id="27df2-115">Create a Stream Analytics job and configure the job to:</span></span>
  - <span data-ttu-id="27df2-116">Čtení dat teploty a vlhkosti ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="27df2-116">Read temperature and humidity data from your IoT hub.</span></span>
  - <span data-ttu-id="27df2-117">Volání webové služby, bude mít déšť možnost.</span><span class="sxs-lookup"><span data-stu-id="27df2-117">Call the web service to get the rain chance.</span></span>
  - <span data-ttu-id="27df2-118">Uložte výsledek do Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="27df2-118">Save the result to an Azure blob storage.</span></span>
- <span data-ttu-id="27df2-119">Chcete-li zobrazit předpovědi počasí pomocí Microsoft Azure Storage Explorer.</span><span class="sxs-lookup"><span data-stu-id="27df2-119">Use Microsoft Azure Storage Explorer to view the weather forecast.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="27df2-120">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="27df2-120">What you need</span></span>

- <span data-ttu-id="27df2-121">Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="27df2-121">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers the following requirements:</span></span>
  - <span data-ttu-id="27df2-122">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="27df2-122">An active Azure subscription.</span></span>
  - <span data-ttu-id="27df2-123">V rámci svého předplatného služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="27df2-123">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="27df2-124">Klientská aplikace, která odesílá zprávy do služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="27df2-124">A client application that sends messages to your Azure IoT hub.</span></span>
- <span data-ttu-id="27df2-125">Účet Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="27df2-125">An Azure Machine Learning Studio account.</span></span> <span data-ttu-id="27df2-126">([Zkuste Machine Learning Studio zdarma](https://studio.azureml.net/)).</span><span class="sxs-lookup"><span data-stu-id="27df2-126">([Try Machine Learning Studio for free](https://studio.azureml.net/)).</span></span>

## <a name="deploy-the-weather-prediction-model-as-a-web-service"></a><span data-ttu-id="27df2-127">Model předpovědi počasí nasadit jako webovou službu</span><span class="sxs-lookup"><span data-stu-id="27df2-127">Deploy the weather prediction model as a web service</span></span>

1. <span data-ttu-id="27df2-128">Přejděte na [stránky modelu předpovědi počasí](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).</span><span class="sxs-lookup"><span data-stu-id="27df2-128">Go to the [weather prediction model page](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).</span></span>
1. <span data-ttu-id="27df2-129">Klikněte na tlačítko **Open in Studio** v Microsoft Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="27df2-129">Click **Open in Studio** in Microsoft Azure Machine Learning Studio.</span></span>
   <span data-ttu-id="27df2-130">![Otevřete stránku modelu předpovědi počasí v Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)</span><span class="sxs-lookup"><span data-stu-id="27df2-130">![Open the weather prediction model page in Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)</span></span>
1. <span data-ttu-id="27df2-131">Klikněte na tlačítko **spustit** ověření kroky v modelu.</span><span class="sxs-lookup"><span data-stu-id="27df2-131">Click **Run** to validate the steps in the model.</span></span> <span data-ttu-id="27df2-132">Tento krok může trvat 2 minut.</span><span class="sxs-lookup"><span data-stu-id="27df2-132">This step might take 2 minutes to complete.</span></span>
   <span data-ttu-id="27df2-133">![Otevřete modelu předpovědi počasí v Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="27df2-133">![Open the weather prediction model in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="27df2-134">Klikněte na tlačítko **nastavení webové služby** > **prediktivní webové služby**.</span><span class="sxs-lookup"><span data-stu-id="27df2-134">Click **SET UP WEB SERVICE** > **Predictive Web Service**.</span></span>
   <span data-ttu-id="27df2-135">![Nasazení modelu předpovědi počasí v Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="27df2-135">![Deploy the weather prediction model in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="27df2-136">V diagramu, přetáhněte **webové služby vstup** modulu někde téměř **Score Model** modulu.</span><span class="sxs-lookup"><span data-stu-id="27df2-136">In the diagram, drag the **Web service input** module somewhere near the **Score Model** module.</span></span>
1. <span data-ttu-id="27df2-137">Připojení **webové služby vstup** modulu **Score Model** modulu.</span><span class="sxs-lookup"><span data-stu-id="27df2-137">Connect the **Web service input** module to the **Score Model** module.</span></span>
   <span data-ttu-id="27df2-138">![Připojení dvě moduly v nástroji Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="27df2-138">![Connect two modules in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="27df2-139">Klikněte na tlačítko **spustit** ověření kroky v modelu.</span><span class="sxs-lookup"><span data-stu-id="27df2-139">Click **RUN** to validate the steps in the model.</span></span>
1. <span data-ttu-id="27df2-140">Klikněte na tlačítko **nasazení webové služby** nasadit model jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="27df2-140">Click **DEPLOY WEB SERVICE** to deploy the model as a web service.</span></span>
1. <span data-ttu-id="27df2-141">Na řídicím panelu modelu Stáhnout **Excel 2010 nebo starší sešitu** pro **požadavků a odpovědí**.</span><span class="sxs-lookup"><span data-stu-id="27df2-141">On the dashboard of the model, download the **Excel 2010 or earlier workbook** for **REQUEST/RESPONSE**.</span></span>

   > [!Note]
   > <span data-ttu-id="27df2-142">Ujistěte se, abyste si stáhli **Excel 2010 nebo starší sešitu** i v případě, že používáte novější verzi aplikace Excel ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="27df2-142">Ensure that you download the **Excel 2010 or earlier workbook** even if you are running a later version of Excel on your computer.</span></span>

   ![Stažení aplikace Excel pro koncový bod REQUEST RESPONSE](media/iot-hub-weather-forecast-machine-learning/5_download-endpoint-app-excel-for-request-response.png)

1. <span data-ttu-id="27df2-144">Otevřete sešit aplikace Excel, poznamenejte si **adresa URL webové služby** a **přístupový klíč**.</span><span class="sxs-lookup"><span data-stu-id="27df2-144">Open the Excel workbook, make a note of the **WEB SERVICE URL** and **ACCESS KEY**.</span></span>

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a><span data-ttu-id="27df2-145">Vytvoření, konfigurace a spuštění úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="27df2-145">Create, configure, and run a Stream Analytics job</span></span>

### <a name="create-a-stream-analytics-job"></a><span data-ttu-id="27df2-146">Vytvoření úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="27df2-146">Create a Stream Analytics job</span></span>

1. <span data-ttu-id="27df2-147">V [portál Azure](https://ms.portal.azure.com/), klikněte na tlačítko **nový** > **Internet věcí** > **úlohy služby Stream Analytics**.</span><span class="sxs-lookup"><span data-stu-id="27df2-147">In the [Azure portal](https://ms.portal.azure.com/), click **New** > **Internet of Things** > **Stream Analytics job**.</span></span>
1. <span data-ttu-id="27df2-148">Zadejte následující informace pro úlohu.</span><span class="sxs-lookup"><span data-stu-id="27df2-148">Enter the following information for the job.</span></span>

   <span data-ttu-id="27df2-149">**Název úlohy**: název úlohy.</span><span class="sxs-lookup"><span data-stu-id="27df2-149">**Job name**: The name of the job.</span></span> <span data-ttu-id="27df2-150">Název musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="27df2-150">The name must be globally unique.</span></span>

   <span data-ttu-id="27df2-151">**Skupina prostředků**: použijte stejnou skupinu prostředků, která používá službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="27df2-151">**Resource group**: Use the same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="27df2-152">**Umístění**: používalo stejné umístění jako vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="27df2-152">**Location**: Use the same location as your resource group.</span></span>

   <span data-ttu-id="27df2-153">**Připnout na řídicí panel**: zaškrtnete tuto možnost pro snadný přístup do služby IoT hub z řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="27df2-153">**Pin to dashboard**: Check this option for easy access to your IoT hub from the dashboard.</span></span>

   ![Vytvořit úlohu služby Stream Analytics v Azure](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. <span data-ttu-id="27df2-155">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="27df2-155">Click **Create**.</span></span>

### <a name="add-an-input-to-the-stream-analytics-job"></a><span data-ttu-id="27df2-156">Přidat vstup do úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="27df2-156">Add an input to the Stream Analytics job</span></span>

1. <span data-ttu-id="27df2-157">Spusťte úlohu služby Stream Analytics.</span><span class="sxs-lookup"><span data-stu-id="27df2-157">Open the Stream Analytics job.</span></span>
1. <span data-ttu-id="27df2-158">V části **úlohy topologie**, klikněte na tlačítko **vstupy**.</span><span class="sxs-lookup"><span data-stu-id="27df2-158">Under **Job Topology**, click **Inputs**.</span></span>
1. <span data-ttu-id="27df2-159">V **vstupy** podokně klikněte na tlačítko **přidat**a potom zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="27df2-159">In the **Inputs** pane, click **Add**, and then enter the following information:</span></span>

   <span data-ttu-id="27df2-160">**Vstupní alias**: jedinečný odkaz pro vstup.</span><span class="sxs-lookup"><span data-stu-id="27df2-160">**Input alias**: The unique alias for the input.</span></span>

   <span data-ttu-id="27df2-161">**Zdroj**: vyberte **služby IoT hub**.</span><span class="sxs-lookup"><span data-stu-id="27df2-161">**Source**: Select **IoT hub**.</span></span>

   <span data-ttu-id="27df2-162">**Skupiny příjemců**: Vyberte skupinu uživatelů, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="27df2-162">**Consumer group**: Select the consumer group you created.</span></span>

   ![Přidat vstup do úlohy Stream Analytics v Azure](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. <span data-ttu-id="27df2-164">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="27df2-164">Click **Create**.</span></span>

### <a name="add-an-output-to-the-stream-analytics-job"></a><span data-ttu-id="27df2-165">Přidat výstup do úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="27df2-165">Add an output to the Stream Analytics job</span></span>

1. <span data-ttu-id="27df2-166">V části **úlohy topologie**, klikněte na tlačítko **výstupy**.</span><span class="sxs-lookup"><span data-stu-id="27df2-166">Under **Job Topology**, click **Outputs**.</span></span>
1. <span data-ttu-id="27df2-167">V **výstupy** podokně klikněte na tlačítko **přidat**a potom zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="27df2-167">In the **Outputs** pane, click **Add**, and then enter the following information:</span></span>

   <span data-ttu-id="27df2-168">**Alias pro výstup**: jedinečný alias pro výstup.</span><span class="sxs-lookup"><span data-stu-id="27df2-168">**Output alias**: The unique alias for the output.</span></span>

   <span data-ttu-id="27df2-169">**Jímky**: vyberte **úložiště objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="27df2-169">**Sink**: Select **Blob Storage**.</span></span>

   <span data-ttu-id="27df2-170">**Účet úložiště**: účet úložiště pro úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="27df2-170">**Storage account**: The storage account for your blob storage.</span></span> <span data-ttu-id="27df2-171">Můžete vytvořit účet úložiště nebo použijte existující.</span><span class="sxs-lookup"><span data-stu-id="27df2-171">You can create a storage account or use an existing one.</span></span>

   <span data-ttu-id="27df2-172">**Kontejner**: uložení objektu blob kontejneru.</span><span class="sxs-lookup"><span data-stu-id="27df2-172">**Container**: The container where the blob is saved.</span></span> <span data-ttu-id="27df2-173">Můžete vytvořit kontejner, nebo použijte existující.</span><span class="sxs-lookup"><span data-stu-id="27df2-173">You can create a container or use an existing one.</span></span>

   <span data-ttu-id="27df2-174">**Formát serializace událostí**: vyberte **CSV**.</span><span class="sxs-lookup"><span data-stu-id="27df2-174">**Event serialization format**: Select **CSV**.</span></span>

   ![Přidat výstup do úlohy Stream Analytics v Azure](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. <span data-ttu-id="27df2-176">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="27df2-176">Click **Create**.</span></span>

### <a name="add-a-function-to-the-stream-analytics-job-to-call-the-web-service-you-deployed"></a><span data-ttu-id="27df2-177">Přidání funkce do úlohy Stream Analytics k volání webové služby, které jste nasadili</span><span class="sxs-lookup"><span data-stu-id="27df2-177">Add a function to the Stream Analytics job to call the web service you deployed</span></span>

1. <span data-ttu-id="27df2-178">V části **úlohy topologie**, klikněte na tlačítko **funkce** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="27df2-178">Under **Job Topology**, click **Functions** > **Add**.</span></span>
1. <span data-ttu-id="27df2-179">Zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="27df2-179">Enter the following information:</span></span>

   <span data-ttu-id="27df2-180">**Funkce Alias**: Zadejte `machinelearning`.</span><span class="sxs-lookup"><span data-stu-id="27df2-180">**Function Alias**: Enter `machinelearning`.</span></span>

   <span data-ttu-id="27df2-181">**Typ funkce**: vyberte **Azure ML**.</span><span class="sxs-lookup"><span data-stu-id="27df2-181">**Function Type**: Select **Azure ML**.</span></span>

   <span data-ttu-id="27df2-182">**Import možnost**: vyberte **Import z jiného předplatného**.</span><span class="sxs-lookup"><span data-stu-id="27df2-182">**Import option**: Select **Import from a different subscription**.</span></span>

   <span data-ttu-id="27df2-183">**Adresa URL**: Zadejte adresu URL webové služby, kterou jste si poznamenali dolů ze sešitu aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="27df2-183">**URL**: Enter the WEB SERVICE URL that you noted down from the Excel workbook.</span></span>

   <span data-ttu-id="27df2-184">**Klíč**: Zadejte přístupový klíč, který jste si poznamenali dolů ze sešitu aplikace Excel.</span><span class="sxs-lookup"><span data-stu-id="27df2-184">**Key**: Enter the ACCESS KEY that you noted down from the Excel workbook.</span></span>

   ![Přidání funkce do úlohy Stream Analytics v Azure](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. <span data-ttu-id="27df2-186">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="27df2-186">Click **Create**.</span></span>

### <a name="configure-the-query-of-the-stream-analytics-job"></a><span data-ttu-id="27df2-187">Konfigurace dotazu úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="27df2-187">Configure the query of the Stream Analytics job</span></span>

1. <span data-ttu-id="27df2-188">V části **úlohy topologie**, klikněte na tlačítko **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="27df2-188">Under **Job Topology**, click **Query**.</span></span>
1. <span data-ttu-id="27df2-189">Existujícího kódu nahraďte následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="27df2-189">Replace the existing code with the following code:</span></span>

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   <span data-ttu-id="27df2-190">Nahraďte `[YourInputAlias]` s alias vstupu úlohy.</span><span class="sxs-lookup"><span data-stu-id="27df2-190">Replace `[YourInputAlias]` with the input alias of the job.</span></span>

   <span data-ttu-id="27df2-191">Nahraďte `[YourOutputAlias]` s aliasem výstupu úlohy.</span><span class="sxs-lookup"><span data-stu-id="27df2-191">Replace `[YourOutputAlias]` with the output alias of the job.</span></span>

1. <span data-ttu-id="27df2-192">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="27df2-192">Click **Save**.</span></span>

### <a name="run-the-stream-analytics-job"></a><span data-ttu-id="27df2-193">Spustit úlohu služby Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="27df2-193">Run the Stream Analytics job</span></span>

<span data-ttu-id="27df2-194">V úloze Stream Analytics, klikněte na tlačítko **spustit** > **nyní** > **spustit**.</span><span class="sxs-lookup"><span data-stu-id="27df2-194">In the Stream Analytics job, click **Start** > **Now** > **Start**.</span></span> <span data-ttu-id="27df2-195">Jakmile se úloha úspěšně spustí, stav úlohy změní z **Zastaveno** k **systémem**.</span><span class="sxs-lookup"><span data-stu-id="27df2-195">Once the job successfully starts, the job status changes from **Stopped** to **Running**.</span></span>

![Spustit úlohu služby Stream Analytics](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-to-view-the-weather-forecast"></a><span data-ttu-id="27df2-197">Použít Microsoft Azure Storage Explorer zobrazíte předpovědi počasí</span><span class="sxs-lookup"><span data-stu-id="27df2-197">Use Microsoft Azure Storage Explorer to view the weather forecast</span></span>

<span data-ttu-id="27df2-198">Spusťte klientskou aplikaci spustit shromažďování a odesílání teploty a vlhkosti dat do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="27df2-198">Run the client application to start collecting and sending temperature and humidity data to your IoT hub.</span></span> <span data-ttu-id="27df2-199">Pro každou zprávu, která přijímá služby IoT hub zavolá úlohu služby Stream Analytics webovou službu předpověď počasí k vytvoření riziko dešti.</span><span class="sxs-lookup"><span data-stu-id="27df2-199">For each message that your IoT hub receives, the Stream Analytics job calls the weather forecast web service to produce the chance of rain.</span></span> <span data-ttu-id="27df2-200">Výsledkem pak je uložena do Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="27df2-200">The result is then saved to your Azure blob storage.</span></span> <span data-ttu-id="27df2-201">Azure Storage Explorer je nástroj, který můžete použít k zobrazení výsledek.</span><span class="sxs-lookup"><span data-stu-id="27df2-201">Azure Storage Explorer is a tool that you can use to view the result.</span></span>

1. <span data-ttu-id="27df2-202">[Stáhněte a nainstalujte Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="27df2-202">[Download and install Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>
1. <span data-ttu-id="27df2-203">Otevřete Průzkumníka úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="27df2-203">Open Azure Storage Explorer.</span></span>
1. <span data-ttu-id="27df2-204">Přihlaste se k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="27df2-204">Sign in to your Azure account.</span></span>
1. <span data-ttu-id="27df2-205">Vyberte své předplatné.</span><span class="sxs-lookup"><span data-stu-id="27df2-205">Select your subscription.</span></span>
1. <span data-ttu-id="27df2-206">Klikněte na předplatné > **účty úložiště** > váš účet úložiště > **kontejnery objektů Blob** > vaše kontejneru.</span><span class="sxs-lookup"><span data-stu-id="27df2-206">Click your subscription > **Storage Accounts** > your storage account > **Blob Containers** > your container.</span></span>
1. <span data-ttu-id="27df2-207">Otevřete soubor .csv zobrazíte výsledek.</span><span class="sxs-lookup"><span data-stu-id="27df2-207">Open a .csv file to see the result.</span></span> <span data-ttu-id="27df2-208">Poslední sloupec zaznamenává riziko dešti.</span><span class="sxs-lookup"><span data-stu-id="27df2-208">The last column records the chance of rain.</span></span>

   ![Získání výsledku předpověď počasí pomocí Azure Machine Learning](media/iot-hub-weather-forecast-machine-learning/12_get-weather-forecast-result-azure-machine-learning.png)

## <a name="summary"></a><span data-ttu-id="27df2-210">Souhrn</span><span class="sxs-lookup"><span data-stu-id="27df2-210">Summary</span></span>

<span data-ttu-id="27df2-211">Azure Machine Learning jste úspěšně použili k vytvoření riziko déšť na základě dat teploty a vlhkosti, která přijímá služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="27df2-211">You’ve successfully used Azure Machine Learning to produce the chance of rain based on the temperature and humidity data that your IoT hub receives.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]