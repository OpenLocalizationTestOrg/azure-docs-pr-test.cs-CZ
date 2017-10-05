---
title: "Uložení zpráv IoT hub do úložiště dat Azure | Microsoft Docs"
description: "Pomocí aplikace Azure funkce IoT hub zprávy uložit Azure table Storage. IoT hub zprávy obsahují informace, například data snímačů, která je odeslána ze zařízení IoT."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "úložiště dat IOT, úložiště dat snímačů iot"
ms.assetid: 62fd14fd-aaaa-4b3d-8367-75c1111b6269
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2017
ms.author: xshi
ms.openlocfilehash: 06503f9564e00ef62587d02f2da4778974e246c5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-to-your-azure-table-storage"></a><span data-ttu-id="2bb5a-105">Uložit IoT hub zprávy, které obsahují data snímačů Azure table Storage</span><span class="sxs-lookup"><span data-stu-id="2bb5a-105">Save IoT hub messages that contain sensor data to your Azure table storage</span></span>

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/3.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="2bb5a-107">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="2bb5a-107">What you learn</span></span>

<span data-ttu-id="2bb5a-108">Zjistíte, jak vytvořit účet úložiště Azure a Azure funkce aplikace na ukládání IoT Centrum zpráv ve službě table storage.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-108">You learn how to create an Azure storage account and an Azure function app to store IoT hub messages in your table storage.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="2bb5a-109">Co dělat</span><span class="sxs-lookup"><span data-stu-id="2bb5a-109">What you do</span></span>

- <span data-ttu-id="2bb5a-110">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="2bb5a-110">Create an Azure storage account.</span></span>
- <span data-ttu-id="2bb5a-111">Příprava připojení centra IoT umožní číst zprávy.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-111">Prepare your IoT hub connection to read messages.</span></span>
- <span data-ttu-id="2bb5a-112">Vytvořte a nasaďte aplikaci Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-112">Create and deploy an Azure function app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="2bb5a-113">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="2bb5a-113">What you need</span></span>

- <span data-ttu-id="2bb5a-114">[Nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) tak, aby pokrývalo následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="2bb5a-114">[Set up your device](iot-hub-raspberry-pi-kit-node-get-started.md) to cover the following requirements:</span></span>
  - <span data-ttu-id="2bb5a-115">Aktivní předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="2bb5a-115">An active Azure subscription</span></span>
  - <span data-ttu-id="2bb5a-116">Služby IoT hub v rámci svého předplatného</span><span class="sxs-lookup"><span data-stu-id="2bb5a-116">An IoT hub under your subscription</span></span> 
  - <span data-ttu-id="2bb5a-117">Spuštěné aplikace, která odesílá zprávy do služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="2bb5a-117">A running application that sends messages to your IoT hub</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="2bb5a-118">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="2bb5a-118">Create an Azure storage account</span></span>

1. <span data-ttu-id="2bb5a-119">V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **úložiště** > **účet úložiště**  >   **Vytvoření**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-119">In the [Azure portal](https://portal.azure.com/), click **New** > **Storage** > **Storage account** > **Create**.</span></span>

2. <span data-ttu-id="2bb5a-120">Zadejte informace potřebné pro účet úložiště:</span><span class="sxs-lookup"><span data-stu-id="2bb5a-120">Enter the necessary information for the storage account:</span></span>

   ![Vytvoření účtu úložiště na portálu Azure](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * <span data-ttu-id="2bb5a-122">**Název**: název účtu úložiště.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-122">**Name**: The name of the storage account.</span></span> <span data-ttu-id="2bb5a-123">Název musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-123">The name must be globally unique.</span></span>

   * <span data-ttu-id="2bb5a-124">**Skupina prostředků**: použijte stejnou skupinu prostředků, která používá službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-124">**Resource group**: Use the same resource group that your IoT hub uses.</span></span>

   * <span data-ttu-id="2bb5a-125">**Připnout na řídicí panel:** Vyberte tuto možnost pro snadný přístup k centru IoT z řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-125">**Pin to dashboard**: Select this option for easy access to your IoT hub from the dashboard.</span></span>

3. <span data-ttu-id="2bb5a-126">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-126">Click **Create**.</span></span>

## <a name="prepare-your-iot-hub-connection-to-read-messages"></a><span data-ttu-id="2bb5a-127">Příprava připojení centra IoT umožní číst zprávy</span><span class="sxs-lookup"><span data-stu-id="2bb5a-127">Prepare your IoT hub connection to read messages</span></span>

<span data-ttu-id="2bb5a-128">IoT hub zpřístupní koncový bod předdefinované událostí kompatibilní s centrem umožnit aplikacím umožní číst zprávy typu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-128">IoT hub exposes a built-in event hub-compatible endpoint to enable applications to read IoT hub messages.</span></span> <span data-ttu-id="2bb5a-129">Mezitím aplikace používají skupiny uživatelů pro čtení dat ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-129">Meanwhile, applications use consumer groups to read data from your IoT hub.</span></span> <span data-ttu-id="2bb5a-130">Než vytvoříte aplikaci Azure funkce načíst data ze služby IoT hub, postupujte takto:</span><span class="sxs-lookup"><span data-stu-id="2bb5a-130">Before you create an Azure function app to read data from your IoT hub, do the following:</span></span>

- <span data-ttu-id="2bb5a-131">Získáte připojovací řetězec koncový bod centra IoT.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-131">Get the connection string of your IoT hub endpoint.</span></span>
- <span data-ttu-id="2bb5a-132">Vytvořte skupinu uživatelů pro službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-132">Create a consumer group for your IoT hub.</span></span>

### <a name="get-the-connection-string-of-your-iot-hub-endpoint"></a><span data-ttu-id="2bb5a-133">Získat připojovací řetězec koncový bod centra IoT</span><span class="sxs-lookup"><span data-stu-id="2bb5a-133">Get the connection string of your IoT hub endpoint</span></span>

1. <span data-ttu-id="2bb5a-134">Otevřete službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-134">Open your IoT hub.</span></span>

2. <span data-ttu-id="2bb5a-135">Na **IoT Hub** podokně v části **zasílání zpráv**, klikněte na tlačítko **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-135">On the **IoT Hub** pane, under **Messaging**, click **Endpoints**.</span></span>

3. <span data-ttu-id="2bb5a-136">V pravém podokně v části **předdefinované koncové body**, klikněte na tlačítko **události**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-136">In the right pane, under **Built-in endpoints**, click **Events**.</span></span>

4. <span data-ttu-id="2bb5a-137">V **vlastnosti** podokně, pamatujte na následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="2bb5a-137">In the **Properties** pane, note the following values:</span></span>
   - <span data-ttu-id="2bb5a-138">Koncový bod kompatibilní s centrem událostí</span><span class="sxs-lookup"><span data-stu-id="2bb5a-138">Event hub-compatible endpoint</span></span>
   - <span data-ttu-id="2bb5a-139">Název kompatibilní s centrem událostí</span><span class="sxs-lookup"><span data-stu-id="2bb5a-139">Event hub-compatible name</span></span>

   ![Získat připojovací řetězec koncový bod centra IoT na portálu Azure](media\iot-hub-store-data-in-azure-table-storage\2_azure-portal-iot-hub-endpoint-connection-string.png)

5. <span data-ttu-id="2bb5a-141">V **IoT Hub** podokně v části **nastavení**, klikněte na tlačítko **zásady sdíleného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-141">In the **IoT Hub** pane, under **Settings**, click **Shared access policies**.</span></span>

6. <span data-ttu-id="2bb5a-142">Klikněte na tlačítko **iothubowner**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-142">Click **iothubowner**.</span></span>

7. <span data-ttu-id="2bb5a-143">Poznámka: **primární klíč** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-143">Note the **Primary key** value.</span></span>

8. <span data-ttu-id="2bb5a-144">Připojovací řetězec koncový bod centra IoT vytvořte takto:</span><span class="sxs-lookup"><span data-stu-id="2bb5a-144">Create the connection string of your IoT hub endpoint as follows:</span></span>

   `Endpoint=<Event Hub-compatible endpoint>;SharedAccessKeyName=iothubowner;SharedAccessKey=<Primary key>`

   > [!NOTE]
   > <span data-ttu-id="2bb5a-145">Nahraďte `<Event Hub-compatible endpoint>` a `<Primary key>` hodnotami, které jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-145">Replace `<Event Hub-compatible endpoint>` and `<Primary key>` with the values that you noted earlier.</span></span>

### <a name="create-a-consumer-group-for-your-iot-hub"></a><span data-ttu-id="2bb5a-146">Vytvořte skupinu uživatelů pro službu IoT hub</span><span class="sxs-lookup"><span data-stu-id="2bb5a-146">Create a consumer group for your IoT hub</span></span>

1. <span data-ttu-id="2bb5a-147">Otevřete službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-147">Open your IoT hub.</span></span>

2. <span data-ttu-id="2bb5a-148">V **IoT Hub** podokně v části **zasílání zpráv**, klikněte na tlačítko **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-148">In the **IoT Hub** pane, under **Messaging**, click **Endpoints**.</span></span>

3. <span data-ttu-id="2bb5a-149">V pravém podokně v části **předdefinované koncové body**, klikněte na tlačítko **události**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-149">In the right pane, under **Built-in endpoints**, click **Events**.</span></span>

4. <span data-ttu-id="2bb5a-150">V **vlastnosti** podokně v části **skupiny příjemců**, zadejte název a poznamenejte si ho.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-150">In the **Properties** pane, under **Consumer groups**, enter a name, and then make a note of it.</span></span>

5. <span data-ttu-id="2bb5a-151">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-151">Click **Save**.</span></span>

## <a name="create-and-deploy-an-azure-function-app"></a><span data-ttu-id="2bb5a-152">Vytvoření a nasazení aplikace Azure – funkce</span><span class="sxs-lookup"><span data-stu-id="2bb5a-152">Create and deploy an Azure function app</span></span>

1. <span data-ttu-id="2bb5a-153">V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **výpočetní** > **aplikaci funkce**  >   **Vytvoření**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-153">In the [Azure portal](https://portal.azure.com/), click **New** > **Compute** > **Function App** > **Create**.</span></span>

2. <span data-ttu-id="2bb5a-154">Zadejte informace potřebné pro aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-154">Enter the necessary information for the function app.</span></span>

   ![Vytvoření aplikace pro funkce na portálu Azure](media\iot-hub-store-data-in-azure-table-storage\3_azure-portal-create-function-app.png)

   * <span data-ttu-id="2bb5a-156">**Název aplikace**: název aplikace funkce.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-156">**App name**: The name of the function app.</span></span> <span data-ttu-id="2bb5a-157">Název musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-157">The name must be globally unique.</span></span>

   * <span data-ttu-id="2bb5a-158">**Skupina prostředků**: použijte stejnou skupinu prostředků, která používá službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-158">**Resource group**: Use the same resource group that your IoT hub uses.</span></span>

   * <span data-ttu-id="2bb5a-159">**Účet úložiště**: účet úložiště, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-159">**Storage Account**: The storage account that you created.</span></span>

   * <span data-ttu-id="2bb5a-160">**Připnout na řídicí panel**: zaškrtnete tuto možnost pro snadný přístup k aplikaci funkce z řídicího panelu.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-160">**Pin to dashboard**: Check this option for easy access to the function app from the dashboard.</span></span>

3. <span data-ttu-id="2bb5a-161">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-161">Click **Create**.</span></span>

4. <span data-ttu-id="2bb5a-162">Po vytvoření funkce aplikace, otevřete ho.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-162">After the function app has been created, open it.</span></span>

5. <span data-ttu-id="2bb5a-163">V aplikaci funkce a vytvořte novou funkci následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2bb5a-163">In the function app, create a new function by doing the following:</span></span>

   <span data-ttu-id="2bb5a-164">a.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-164">a.</span></span> <span data-ttu-id="2bb5a-165">Klikněte na tlačítko **novou funkci**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-165">Click **New Function**.</span></span>

   <span data-ttu-id="2bb5a-166">b.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-166">b.</span></span> <span data-ttu-id="2bb5a-167">Vyberte **JavaScript** pro **jazyk**, a **zpracování dat** pro **scénář**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-167">Select **JavaScript** for **Language**, and **Data Processing** for **Scenario**.</span></span>

   <span data-ttu-id="2bb5a-168">c.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-168">c.</span></span> <span data-ttu-id="2bb5a-169">Klikněte na tlačítko **vytvořit tuto funkci**a potom klikněte na **novou funkci**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-169">Click **Create this function**, and then click **New Function**.</span></span>

   <span data-ttu-id="2bb5a-170">d.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-170">d.</span></span> <span data-ttu-id="2bb5a-171">Vyberte **JavaScript** pro daný jazyk, a **zpracování dat** pro scénář.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-171">Select **JavaScript** for the language, and **Data Processing** for the scenario.</span></span>

   <span data-ttu-id="2bb5a-172">e.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-172">e.</span></span> <span data-ttu-id="2bb5a-173">Klikněte **EventHubTrigger JavaScript** šablony.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-173">Click the **EventHubTrigger-JavaScript** template.</span></span>

   <span data-ttu-id="2bb5a-174">f.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-174">f.</span></span> <span data-ttu-id="2bb5a-175">Zadejte informace potřebné pro šablonu.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-175">Enter the necessary information for the template.</span></span>

      * <span data-ttu-id="2bb5a-176">**Název funkce**: název funkce.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-176">**Name your function**: The name of the function.</span></span>

      * <span data-ttu-id="2bb5a-177">**Název centra událostí**: název kompatibilní s centrem událostí, který jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-177">**Event Hub name**: The event hub-compatible name that you noted earlier.</span></span>

      * <span data-ttu-id="2bb5a-178">**Připojení rozbočovače událostí**: Chcete-li přidat připojovací řetězec koncový bod centra IoT, kterou jste vytvořili, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-178">**Event Hub connection**: To add the connection string of the IoT hub endpoint that you created, click **New**.</span></span>

   <span data-ttu-id="2bb5a-179">g.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-179">g.</span></span> <span data-ttu-id="2bb5a-180">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-180">Click **Create**.</span></span>

6. <span data-ttu-id="2bb5a-181">Výstup funkce nakonfigurujte následujícím způsobem:</span><span class="sxs-lookup"><span data-stu-id="2bb5a-181">Configure an output of the function by doing the following:</span></span>

   <span data-ttu-id="2bb5a-182">a.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-182">a.</span></span> <span data-ttu-id="2bb5a-183">Klikněte na tlačítko **integrovat** > **nový výstupní** > **Azure Table Storage** > **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-183">Click **Integrate** > **New Output** > **Azure Table Storage** > **Select**.</span></span>

      ![Přidání úložiště tabulek do funkce aplikace na portálu Azure](media\iot-hub-store-data-in-azure-table-storage\4_azure-portal-function-app-add-output-table-storage.png)

   <span data-ttu-id="2bb5a-185">b.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-185">b.</span></span> <span data-ttu-id="2bb5a-186">Zadejte potřebné informace.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-186">Enter the necessary information.</span></span>

      * <span data-ttu-id="2bb5a-187">**Parametr název tabulky**: použití `outputTable`, který se používá v kódu funkci Azure.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-187">**Table parameter name**: Use `outputTable`, which will be used in the Azure function's code.</span></span>
      
      * <span data-ttu-id="2bb5a-188">**Název tabulky**: použití `deviceData`.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-188">**Table name**: Use `deviceData`.</span></span>

      * <span data-ttu-id="2bb5a-189">**Připojení účtu úložiště**: klikněte na tlačítko **nový**a potom vyberte nebo zadejte svůj účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-189">**Storage account connection**: Click **New**, and then select or enter your storage account.</span></span> <span data-ttu-id="2bb5a-190">Pokud se nezobrazí, účet úložiště, najdete v části [požadavky na účet úložiště](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).</span><span class="sxs-lookup"><span data-stu-id="2bb5a-190">If the storage account is not displayed, see [Storage account requirements](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).</span></span>
      
   <span data-ttu-id="2bb5a-191">c.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-191">c.</span></span> <span data-ttu-id="2bb5a-192">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-192">Click **Save**.</span></span>

7. <span data-ttu-id="2bb5a-193">V části **aktivační události**, klikněte na tlačítko **centra událostí Azure (eventHubMessages)**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-193">Under **Triggers**, click **Azure Event Hub (eventHubMessages)**.</span></span>

8. <span data-ttu-id="2bb5a-194">V části **skupiny příjemců centra událostí**, zadejte název skupiny uživatelů, který jste vytvořili a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-194">Under **Event Hub consumer group**, enter the name of the consumer group that you created, and then click **Save**.</span></span>

9. <span data-ttu-id="2bb5a-195">Klikněte na funkce, které jste vytvořili na levé straně a pak klikněte na **zobrazit soubory** na pravé straně.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-195">Click the function you've created on the left and then click **View Files** on the right.</span></span>

10. <span data-ttu-id="2bb5a-196">Nahraďte kód v `index.js` následujícím kódem:</span><span class="sxs-lookup"><span data-stu-id="2bb5a-196">Replace the code in `index.js` with the following:</span></span>

   ```javascript
   'use strict';

   // This function is triggered each time a message is received in the IoT hub.
   // The message payload is persisted in an Azure storage table
 
   module.exports = function (context, iotHubMessage) {
    context.log('Message received: ' + JSON.stringify(iotHubMessage));
    var date = Date.now();
    var partitionKey = Math.floor(date / (24 * 60 * 60 * 1000)) + '';
    var rowKey = date + '';
    context.bindings.outputTable = {
     "partitionKey": partitionKey,
     "rowKey": rowKey,
     "message": JSON.stringify(iotHubMessage)
    };
    context.done();
   };
   ```

11. <span data-ttu-id="2bb5a-197">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-197">Click **Save**.</span></span>

<span data-ttu-id="2bb5a-198">Nyní jste vytvořili aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-198">You have now created the function app.</span></span> <span data-ttu-id="2bb5a-199">Ukládá zprávy, které IoT hub přijímá ve službě table storage.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-199">It stores messages that your IoT hub receives in your table storage.</span></span>

> [!NOTE]
> <span data-ttu-id="2bb5a-200">Můžete použít **spustit** tlačítko Testovat aplikaci funkce.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-200">You can use the **Run** button to test the function app.</span></span> <span data-ttu-id="2bb5a-201">Když kliknete na tlačítko **spustit**, je testovací zpráva odeslána do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-201">When you click **Run**, the test message is sent to your IoT hub.</span></span> <span data-ttu-id="2bb5a-202">Přijetí zprávy by měly aktivovat aplikaci funkce spuštění a potom uložte zprávu table Storage.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-202">The arrival of the message should trigger the function app to start and then save the message to your table storage.</span></span> <span data-ttu-id="2bb5a-203">**Protokoly** podokně zaznamenává informace o procesu.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-203">The **Logs** pane records the details of the process.</span></span>

## <a name="verify-your-message-in-your-table-storage"></a><span data-ttu-id="2bb5a-204">Ověřit zprávu ve službě table storage</span><span class="sxs-lookup"><span data-stu-id="2bb5a-204">Verify your message in your table storage</span></span>

1. <span data-ttu-id="2bb5a-205">Spuštění ukázkové aplikace na zařízení k odesílání zpráv do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-205">Run the sample application on your device to send messages to your IoT hub.</span></span>

2. <span data-ttu-id="2bb5a-206">[Stáhněte a nainstalujte Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="2bb5a-206">[Download and install Azure Storage Explorer](http://storageexplorer.com/).</span></span>

3. <span data-ttu-id="2bb5a-207">Otevřete Storage Explorer, klikněte na **přidat účet Azure** > **přihlášení**a potom se přihlaste k účtu Azure.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-207">Open Storage Explorer, click **Add an Azure Account** > **Sign in**, and then sign in to your Azure account.</span></span>

4. <span data-ttu-id="2bb5a-208">Klikněte na předplatné Azure > **účty úložiště** > váš účet úložiště > **tabulky** > **deviceData**.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-208">Click your Azure subscription > **Storage Accounts** > your storage account > **Tables** > **deviceData**.</span></span>

   <span data-ttu-id="2bb5a-209">Měli byste vidět zprávy odeslané ze zařízení do služby IoT hub přihlášení `deviceData` tabulky.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-209">You should see messages sent from your device to your IoT hub logged in the `deviceData` table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2bb5a-210">Další kroky</span><span class="sxs-lookup"><span data-stu-id="2bb5a-210">Next steps</span></span>

<span data-ttu-id="2bb5a-211">Úspěšně jste vytvořili účet úložiště Azure a Azure funkce aplikace, která ukládá zprávy, které IoT hub přijímá ve službě table storage.</span><span class="sxs-lookup"><span data-stu-id="2bb5a-211">You’ve successfully created your Azure storage account and Azure function app, which stores messages that your IoT hub receives in your table storage.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
