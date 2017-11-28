---
title: "aaaWeather prognózy pomocí Azure Machine Learning s daty ze služby IoT Hub | Microsoft Docs"
description: "Použití Azure Machine Learning toopredict hello riziko déšť založené na hello teploty a vlhkosti data, která shromažďuje služby IoT hub ze senzoru."
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
ms.openlocfilehash: 04abe97558ccfc152bae2e0d435033433c0023dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="weather-forecast-using-hello-sensor-data-from-your-iot-hub-in-azure-machine-learning"></a><span data-ttu-id="01927-104">Předpověď počasí pomocí hello senzor dat z centra IoT v Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="01927-104">Weather forecast using hello sensor data from your IoT hub in Azure Machine Learning</span></span>

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/6.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="01927-106">Machine learning je technika, kdy vědecké zpracování dat, která pomáhá počítače dozvědět se od existujícího data tooforecast budoucí chování, výsledky a trendy.</span><span class="sxs-lookup"><span data-stu-id="01927-106">Machine learning is a technique of data science that helps computers learn from existing data tooforecast future behaviors, outcomes, and trends.</span></span> <span data-ttu-id="01927-107">Azure Machine Learning je Cloudová služba prediktivní analýzy, která je možné tooquickly vytvářet a nasazovat prediktivní modely jako analytická řešení.</span><span class="sxs-lookup"><span data-stu-id="01927-107">Azure Machine Learning is a cloud predictive analytics service that makes it possible tooquickly create and deploy predictive models as analytics solutions.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="01927-108">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="01927-108">What you learn</span></span>

<span data-ttu-id="01927-109">Zjistíte, jak toouse Azure Machine Learning toodo předpověď počasí (riziko déšť) pomocí hello teploty a vlhkosti data ze služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="01927-109">You learn how toouse Azure Machine Learning toodo weather forecast (chance of rain) using hello temperature and humidity data from your Azure IoT hub.</span></span> <span data-ttu-id="01927-110">Hello riziko déšť je výstup hello modelu předpovědi počasí připravené.</span><span class="sxs-lookup"><span data-stu-id="01927-110">hello chance of rain is hello output of a prepared weather prediction model.</span></span> <span data-ttu-id="01927-111">Hello model je založena na historických datech tooforecast riziko déšť vychází z teploty a vlhkosti.</span><span class="sxs-lookup"><span data-stu-id="01927-111">hello model is built upon historic data tooforecast chance of rain based on temperature and humidity.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="01927-112">Co dělat</span><span class="sxs-lookup"><span data-stu-id="01927-112">What you do</span></span>

- <span data-ttu-id="01927-113">Nasazení modelu předpovědi počasí hello jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="01927-113">Deploy hello weather prediction model as a web service.</span></span>
- <span data-ttu-id="01927-114">Služby IoT hub připravte pro přístup k datům přidáním skupiny příjemců.</span><span class="sxs-lookup"><span data-stu-id="01927-114">Get your IoT hub ready for data access by adding a consumer group.</span></span>
- <span data-ttu-id="01927-115">Vytvořit úlohu služby Stream Analytics a nakonfigurujte hello úlohy:</span><span class="sxs-lookup"><span data-stu-id="01927-115">Create a Stream Analytics job and configure hello job to:</span></span>
  - <span data-ttu-id="01927-116">Čtení dat teploty a vlhkosti ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="01927-116">Read temperature and humidity data from your IoT hub.</span></span>
  - <span data-ttu-id="01927-117">Volání hello webové služby tooget hello déšť příležitosti.</span><span class="sxs-lookup"><span data-stu-id="01927-117">Call hello web service tooget hello rain chance.</span></span>
  - <span data-ttu-id="01927-118">Uložte hello výsledek tooan Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="01927-118">Save hello result tooan Azure blob storage.</span></span>
- <span data-ttu-id="01927-119">Použít Microsoft Azure Storage Explorer tooview hello počasí prognózu.</span><span class="sxs-lookup"><span data-stu-id="01927-119">Use Microsoft Azure Storage Explorer tooview hello weather forecast.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="01927-120">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="01927-120">What you need</span></span>

- <span data-ttu-id="01927-121">Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="01927-121">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers hello following requirements:</span></span>
  - <span data-ttu-id="01927-122">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="01927-122">An active Azure subscription.</span></span>
  - <span data-ttu-id="01927-123">V rámci svého předplatného služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="01927-123">An Azure IoT hub under your subscription.</span></span>
  - <span data-ttu-id="01927-124">Klientská aplikace, která odesílá zprávy tooyour Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="01927-124">A client application that sends messages tooyour Azure IoT hub.</span></span>
- <span data-ttu-id="01927-125">Účet Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="01927-125">An Azure Machine Learning Studio account.</span></span> <span data-ttu-id="01927-126">([Zkuste Machine Learning Studio zdarma](https://studio.azureml.net/)).</span><span class="sxs-lookup"><span data-stu-id="01927-126">([Try Machine Learning Studio for free](https://studio.azureml.net/)).</span></span>

## <a name="deploy-hello-weather-prediction-model-as-a-web-service"></a><span data-ttu-id="01927-127">Nasazení modelu předpovědi počasí hello jako webovou službu</span><span class="sxs-lookup"><span data-stu-id="01927-127">Deploy hello weather prediction model as a web service</span></span>

1. <span data-ttu-id="01927-128">Přejděte toohello [stránky modelu předpovědi počasí](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).</span><span class="sxs-lookup"><span data-stu-id="01927-128">Go toohello [weather prediction model page](https://gallery.cortanaintelligence.com/Experiment/Weather-prediction-model-1).</span></span>
1. <span data-ttu-id="01927-129">Klikněte na tlačítko **Open in Studio** v Microsoft Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="01927-129">Click **Open in Studio** in Microsoft Azure Machine Learning Studio.</span></span>
   <span data-ttu-id="01927-130">![Otevřete hello počasí předpovědi modelu stránky v Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)</span><span class="sxs-lookup"><span data-stu-id="01927-130">![Open hello weather prediction model page in Cortana Intelligence Gallery](media/iot-hub-weather-forecast-machine-learning/2_weather-prediction-model-in-cortana-intelligence-gallery.png)</span></span>
1. <span data-ttu-id="01927-131">Klikněte na tlačítko **spustit** toovalidate hello kroky v modelu hello.</span><span class="sxs-lookup"><span data-stu-id="01927-131">Click **Run** toovalidate hello steps in hello model.</span></span> <span data-ttu-id="01927-132">Tento krok může trvat toocomplete 2 minut.</span><span class="sxs-lookup"><span data-stu-id="01927-132">This step might take 2 minutes toocomplete.</span></span>
   <span data-ttu-id="01927-133">![Model předpovědi počasí otevřete hello v Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="01927-133">![Open hello weather prediction model in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/3_open-weather-prediction-model-in-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="01927-134">Klikněte na tlačítko **nastavení webové služby** > **prediktivní webové služby**.</span><span class="sxs-lookup"><span data-stu-id="01927-134">Click **SET UP WEB SERVICE** > **Predictive Web Service**.</span></span>
   <span data-ttu-id="01927-135">![Nasazení modelu předpovědi počasí hello v Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="01927-135">![Deploy hello weather prediction model in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/4-deploy-weather-prediction-model-in-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="01927-136">V diagramu hello přetáhněte hello **webové služby vstup** modulu někde téměř hello **Score Model** modulu.</span><span class="sxs-lookup"><span data-stu-id="01927-136">In hello diagram, drag hello **Web service input** module somewhere near hello **Score Model** module.</span></span>
1. <span data-ttu-id="01927-137">Připojit hello **webové služby vstup** modulu toohello **Score Model** modulu.</span><span class="sxs-lookup"><span data-stu-id="01927-137">Connect hello **Web service input** module toohello **Score Model** module.</span></span>
   <span data-ttu-id="01927-138">![Připojení dvě moduly v nástroji Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)</span><span class="sxs-lookup"><span data-stu-id="01927-138">![Connect two modules in Azure Machine Learning Studio](media/iot-hub-weather-forecast-machine-learning/13_connect-modules-azure-machine-learning-studio.png)</span></span>
1. <span data-ttu-id="01927-139">Klikněte na tlačítko **spustit** toovalidate hello kroky v modelu hello.</span><span class="sxs-lookup"><span data-stu-id="01927-139">Click **RUN** toovalidate hello steps in hello model.</span></span>
1. <span data-ttu-id="01927-140">Klikněte na tlačítko **nasazení webové služby** toodeploy hello model jako webovou službu.</span><span class="sxs-lookup"><span data-stu-id="01927-140">Click **DEPLOY WEB SERVICE** toodeploy hello model as a web service.</span></span>
1. <span data-ttu-id="01927-141">Na řídicím panelu hello hello modelu, stáhněte si hello **Excel 2010 nebo starší sešitu** pro **požadavků a odpovědí**.</span><span class="sxs-lookup"><span data-stu-id="01927-141">On hello dashboard of hello model, download hello **Excel 2010 or earlier workbook** for **REQUEST/RESPONSE**.</span></span>

   > [!Note]
   > <span data-ttu-id="01927-142">Ujistěte se, že si stáhnout hello **Excel 2010 nebo starší sešitu** i v případě, že používáte novější verzi aplikace Excel ve vašem počítači.</span><span class="sxs-lookup"><span data-stu-id="01927-142">Ensure that you download hello **Excel 2010 or earlier workbook** even if you are running a later version of Excel on your computer.</span></span>

   ![Stáhnout hello aplikace Excel pro koncový bod REQUEST RESPONSE hello](media/iot-hub-weather-forecast-machine-learning/5_download-endpoint-app-excel-for-request-response.png)

1. <span data-ttu-id="01927-144">Otevřete sešit aplikace Excel hello, poznamenejte si hello **adresa URL webové služby** a **přístupový klíč**.</span><span class="sxs-lookup"><span data-stu-id="01927-144">Open hello Excel workbook, make a note of hello **WEB SERVICE URL** and **ACCESS KEY**.</span></span>

[!INCLUDE [iot-hub-get-started-create-consumer-group](../../includes/iot-hub-get-started-create-consumer-group.md)]

## <a name="create-configure-and-run-a-stream-analytics-job"></a><span data-ttu-id="01927-145">Vytvoření, konfigurace a spuštění úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="01927-145">Create, configure, and run a Stream Analytics job</span></span>

### <a name="create-a-stream-analytics-job"></a><span data-ttu-id="01927-146">Vytvoření úlohy Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="01927-146">Create a Stream Analytics job</span></span>

1. <span data-ttu-id="01927-147">V hello [portál Azure](https://ms.portal.azure.com/), klikněte na tlačítko **nový** > **Internet věcí** > **úlohy služby Stream Analytics**.</span><span class="sxs-lookup"><span data-stu-id="01927-147">In hello [Azure portal](https://ms.portal.azure.com/), click **New** > **Internet of Things** > **Stream Analytics job**.</span></span>
1. <span data-ttu-id="01927-148">Zadejte následující informace pro úlohu hello hello.</span><span class="sxs-lookup"><span data-stu-id="01927-148">Enter hello following information for hello job.</span></span>

   <span data-ttu-id="01927-149">**Název úlohy**: název hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="01927-149">**Job name**: hello name of hello job.</span></span> <span data-ttu-id="01927-150">Název Hello musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="01927-150">hello name must be globally unique.</span></span>

   <span data-ttu-id="01927-151">**Skupina prostředků**: použití hello stejné skupiny prostředků, která používá službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="01927-151">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="01927-152">**Umístění**: použití hello stejné umístění jako vaší skupiny prostředků.</span><span class="sxs-lookup"><span data-stu-id="01927-152">**Location**: Use hello same location as your resource group.</span></span>

   <span data-ttu-id="01927-153">**PIN kód toodashboard**: zaškrtnete tuto možnost pro Centrum IoT tooyour snadný přístup z řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="01927-153">**Pin toodashboard**: Check this option for easy access tooyour IoT hub from hello dashboard.</span></span>

   ![Vytvořit úlohu služby Stream Analytics v Azure](media/iot-hub-weather-forecast-machine-learning/7_create-stream-analytics-job-azure.png)

1. <span data-ttu-id="01927-155">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01927-155">Click **Create**.</span></span>

### <a name="add-an-input-toohello-stream-analytics-job"></a><span data-ttu-id="01927-156">Přidat úloha Stream Analytics vstupní toohello</span><span class="sxs-lookup"><span data-stu-id="01927-156">Add an input toohello Stream Analytics job</span></span>

1. <span data-ttu-id="01927-157">Úloha Stream Analytics otevřete hello.</span><span class="sxs-lookup"><span data-stu-id="01927-157">Open hello Stream Analytics job.</span></span>
1. <span data-ttu-id="01927-158">V části **úlohy topologie**, klikněte na tlačítko **vstupy**.</span><span class="sxs-lookup"><span data-stu-id="01927-158">Under **Job Topology**, click **Inputs**.</span></span>
1. <span data-ttu-id="01927-159">V hello **vstupy** podokně klikněte na tlačítko **přidat**a pak zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="01927-159">In hello **Inputs** pane, click **Add**, and then enter hello following information:</span></span>

   <span data-ttu-id="01927-160">**Vstupní alias**: hello jedinečný odkaz pro vstup hello.</span><span class="sxs-lookup"><span data-stu-id="01927-160">**Input alias**: hello unique alias for hello input.</span></span>

   <span data-ttu-id="01927-161">**Zdroj**: vyberte **služby IoT hub**.</span><span class="sxs-lookup"><span data-stu-id="01927-161">**Source**: Select **IoT hub**.</span></span>

   <span data-ttu-id="01927-162">**Skupiny příjemců**: Vyberte hello příjemce skupinu, kterou jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="01927-162">**Consumer group**: Select hello consumer group you created.</span></span>

   ![Přidat úloha Stream Analytics vstupní toohello v Azure](media/iot-hub-weather-forecast-machine-learning/8_add-input-stream-analytics-job-azure.png)

1. <span data-ttu-id="01927-164">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01927-164">Click **Create**.</span></span>

### <a name="add-an-output-toohello-stream-analytics-job"></a><span data-ttu-id="01927-165">Přidat úloha Stream Analytics toohello výstup</span><span class="sxs-lookup"><span data-stu-id="01927-165">Add an output toohello Stream Analytics job</span></span>

1. <span data-ttu-id="01927-166">V části **úlohy topologie**, klikněte na tlačítko **výstupy**.</span><span class="sxs-lookup"><span data-stu-id="01927-166">Under **Job Topology**, click **Outputs**.</span></span>
1. <span data-ttu-id="01927-167">V hello **výstupy** podokně klikněte na tlačítko **přidat**a pak zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="01927-167">In hello **Outputs** pane, click **Add**, and then enter hello following information:</span></span>

   <span data-ttu-id="01927-168">**Alias pro výstup**: hello jedinečný odkaz pro výstup hello.</span><span class="sxs-lookup"><span data-stu-id="01927-168">**Output alias**: hello unique alias for hello output.</span></span>

   <span data-ttu-id="01927-169">**Jímky**: vyberte **úložiště objektů Blob**.</span><span class="sxs-lookup"><span data-stu-id="01927-169">**Sink**: Select **Blob Storage**.</span></span>

   <span data-ttu-id="01927-170">**Účet úložiště**: hello účet úložiště pro úložiště objektů blob.</span><span class="sxs-lookup"><span data-stu-id="01927-170">**Storage account**: hello storage account for your blob storage.</span></span> <span data-ttu-id="01927-171">Můžete vytvořit účet úložiště nebo použijte existující.</span><span class="sxs-lookup"><span data-stu-id="01927-171">You can create a storage account or use an existing one.</span></span>

   <span data-ttu-id="01927-172">**Kontejner**: hello kontejneru uložení objektů blob hello.</span><span class="sxs-lookup"><span data-stu-id="01927-172">**Container**: hello container where hello blob is saved.</span></span> <span data-ttu-id="01927-173">Můžete vytvořit kontejner, nebo použijte existující.</span><span class="sxs-lookup"><span data-stu-id="01927-173">You can create a container or use an existing one.</span></span>

   <span data-ttu-id="01927-174">**Formát serializace událostí**: vyberte **CSV**.</span><span class="sxs-lookup"><span data-stu-id="01927-174">**Event serialization format**: Select **CSV**.</span></span>

   ![Přidat úloha Stream Analytics toohello výstupu v Azure](media/iot-hub-weather-forecast-machine-learning/9_add-output-stream-analytics-job-azure.png)

1. <span data-ttu-id="01927-176">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01927-176">Click **Create**.</span></span>

### <a name="add-a-function-toohello-stream-analytics-job-toocall-hello-web-service-you-deployed"></a><span data-ttu-id="01927-177">Přidání funkce toohello Stream Analytics úlohy toocall hello webové služby, které jste nasadili</span><span class="sxs-lookup"><span data-stu-id="01927-177">Add a function toohello Stream Analytics job toocall hello web service you deployed</span></span>

1. <span data-ttu-id="01927-178">V části **úlohy topologie**, klikněte na tlačítko **funkce** > **přidat**.</span><span class="sxs-lookup"><span data-stu-id="01927-178">Under **Job Topology**, click **Functions** > **Add**.</span></span>
1. <span data-ttu-id="01927-179">Zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="01927-179">Enter hello following information:</span></span>

   <span data-ttu-id="01927-180">**Funkce Alias**: Zadejte `machinelearning`.</span><span class="sxs-lookup"><span data-stu-id="01927-180">**Function Alias**: Enter `machinelearning`.</span></span>

   <span data-ttu-id="01927-181">**Typ funkce**: vyberte **Azure ML**.</span><span class="sxs-lookup"><span data-stu-id="01927-181">**Function Type**: Select **Azure ML**.</span></span>

   <span data-ttu-id="01927-182">**Import možnost**: vyberte **Import z jiného předplatného**.</span><span class="sxs-lookup"><span data-stu-id="01927-182">**Import option**: Select **Import from a different subscription**.</span></span>

   <span data-ttu-id="01927-183">**Adresa URL**: Zadejte hello adresa URL webové služby, kterou jste si poznamenali dolů ze sešitu aplikace Excel hello.</span><span class="sxs-lookup"><span data-stu-id="01927-183">**URL**: Enter hello WEB SERVICE URL that you noted down from hello Excel workbook.</span></span>

   <span data-ttu-id="01927-184">**Klíč**: Zadejte hello přístupový klíč, který jste si poznamenali dolů ze sešitu aplikace Excel hello.</span><span class="sxs-lookup"><span data-stu-id="01927-184">**Key**: Enter hello ACCESS KEY that you noted down from hello Excel workbook.</span></span>

   ![Přidat úloha Stream Analytics toohello funkce v Azure](media/iot-hub-weather-forecast-machine-learning/10_add-function-stream-analytics-job-azure.png)

1. <span data-ttu-id="01927-186">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="01927-186">Click **Create**.</span></span>

### <a name="configure-hello-query-of-hello-stream-analytics-job"></a><span data-ttu-id="01927-187">Konfigurace hello dotazu úlohy Stream Analytics hello</span><span class="sxs-lookup"><span data-stu-id="01927-187">Configure hello query of hello Stream Analytics job</span></span>

1. <span data-ttu-id="01927-188">V části **úlohy topologie**, klikněte na tlačítko **dotazu**.</span><span class="sxs-lookup"><span data-stu-id="01927-188">Under **Job Topology**, click **Query**.</span></span>
1. <span data-ttu-id="01927-189">Nahraďte stávající kód hello hello následující kód:</span><span class="sxs-lookup"><span data-stu-id="01927-189">Replace hello existing code with hello following code:</span></span>

   ```sql
   WITH machinelearning AS (
      SELECT EventEnqueuedUtcTime, temperature, humidity, machinelearning(temperature, humidity) as result from [YourInputAlias]
   )
   Select System.Timestamp time, CAST (result.[temperature] AS FLOAT) AS temperature, CAST (result.[humidity] AS FLOAT) AS humidity, CAST (result.[Scored Probabilities] AS FLOAT ) AS 'probabalities of rain'
   Into [YourOutputAlias]
   From machinelearning
   ```

   <span data-ttu-id="01927-190">Nahraďte `[YourInputAlias]` s aliasem hello vstupní hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="01927-190">Replace `[YourInputAlias]` with hello input alias of hello job.</span></span>

   <span data-ttu-id="01927-191">Nahraďte `[YourOutputAlias]` s alias pro výstup hello hello úlohy.</span><span class="sxs-lookup"><span data-stu-id="01927-191">Replace `[YourOutputAlias]` with hello output alias of hello job.</span></span>

1. <span data-ttu-id="01927-192">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="01927-192">Click **Save**.</span></span>

### <a name="run-hello-stream-analytics-job"></a><span data-ttu-id="01927-193">Spustit úlohu služby Stream Analytics hello</span><span class="sxs-lookup"><span data-stu-id="01927-193">Run hello Stream Analytics job</span></span>

<span data-ttu-id="01927-194">V úloze Stream Analytics hello, klikněte na tlačítko **spustit** > **nyní** > **spustit**.</span><span class="sxs-lookup"><span data-stu-id="01927-194">In hello Stream Analytics job, click **Start** > **Now** > **Start**.</span></span> <span data-ttu-id="01927-195">Jakmile hello úloha úspěšně spustí, změní se stav úlohy hello ze **Zastaveno** příliš**systémem**.</span><span class="sxs-lookup"><span data-stu-id="01927-195">Once hello job successfully starts, hello job status changes from **Stopped** too**Running**.</span></span>

![Spustit úlohu služby Stream Analytics hello](media/iot-hub-weather-forecast-machine-learning/11_run-stream-analytics-job-azure.png)

## <a name="use-microsoft-azure-storage-explorer-tooview-hello-weather-forecast"></a><span data-ttu-id="01927-197">Použití Microsoft Azure Storage Explorer tooview hello počasí prognózy</span><span class="sxs-lookup"><span data-stu-id="01927-197">Use Microsoft Azure Storage Explorer tooview hello weather forecast</span></span>

<span data-ttu-id="01927-198">Spusťte hello klienta aplikace toostart shromažďování a odesílání teploty a vlhkosti data tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="01927-198">Run hello client application toostart collecting and sending temperature and humidity data tooyour IoT hub.</span></span> <span data-ttu-id="01927-199">Pro každou zprávu, která přijímá služby IoT hub zavolá úlohy služby Stream Analytics hello hello předpověď počasí webové služby tooproduce hello riziko dešti.</span><span class="sxs-lookup"><span data-stu-id="01927-199">For each message that your IoT hub receives, hello Stream Analytics job calls hello weather forecast web service tooproduce hello chance of rain.</span></span> <span data-ttu-id="01927-200">výsledek Hello pak je uložena tooyour úložiště objektů blob Azure.</span><span class="sxs-lookup"><span data-stu-id="01927-200">hello result is then saved tooyour Azure blob storage.</span></span> <span data-ttu-id="01927-201">Azure Storage Explorer je nástroj, který můžete použít výsledek hello tooview.</span><span class="sxs-lookup"><span data-stu-id="01927-201">Azure Storage Explorer is a tool that you can use tooview hello result.</span></span>

1. <span data-ttu-id="01927-202">[Stáhněte a nainstalujte Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="01927-202">[Download and install Microsoft Azure Storage Explorer](http://storageexplorer.com/).</span></span>
1. <span data-ttu-id="01927-203">Otevřete Průzkumníka úložiště Azure.</span><span class="sxs-lookup"><span data-stu-id="01927-203">Open Azure Storage Explorer.</span></span>
1. <span data-ttu-id="01927-204">Přihlaste se tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="01927-204">Sign in tooyour Azure account.</span></span>
1. <span data-ttu-id="01927-205">Vyberte své předplatné.</span><span class="sxs-lookup"><span data-stu-id="01927-205">Select your subscription.</span></span>
1. <span data-ttu-id="01927-206">Klikněte na předplatné > **účty úložiště** > váš účet úložiště > **kontejnery objektů Blob** > vaše kontejneru.</span><span class="sxs-lookup"><span data-stu-id="01927-206">Click your subscription > **Storage Accounts** > your storage account > **Blob Containers** > your container.</span></span>
1. <span data-ttu-id="01927-207">Otevřete výsledek hello toosee soubor .csv.</span><span class="sxs-lookup"><span data-stu-id="01927-207">Open a .csv file toosee hello result.</span></span> <span data-ttu-id="01927-208">poslední sloupec záznamů Hello hello riziko dešti.</span><span class="sxs-lookup"><span data-stu-id="01927-208">hello last column records hello chance of rain.</span></span>

   ![Získání výsledku předpověď počasí pomocí Azure Machine Learning](media/iot-hub-weather-forecast-machine-learning/12_get-weather-forecast-result-azure-machine-learning.png)

## <a name="summary"></a><span data-ttu-id="01927-210">Souhrn</span><span class="sxs-lookup"><span data-stu-id="01927-210">Summary</span></span>

<span data-ttu-id="01927-211">Úspěšně jste použili Azure Machine Learning tooproduce hello riziko déšť na základě dat hello teploty a vlhkosti, která přijímá služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="01927-211">You’ve successfully used Azure Machine Learning tooproduce hello chance of rain based on hello temperature and humidity data that your IoT hub receives.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]