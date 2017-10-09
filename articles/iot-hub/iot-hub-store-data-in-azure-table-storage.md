---
title: "aaaSave tooAzure datové úložiště zprávy služby IoT hub | Microsoft Docs"
description: "Pomocí služby Azure funkce aplikace toosave vaše IoT hub zprávy tooyour úložiště tabulek Azure. Hello IoT hub zprávy obsahují informace, například data snímačů, která je odeslána ze zařízení IoT."
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
ms.openlocfilehash: be72d9ba9a781822926364351b50f58f5d96e94a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="save-iot-hub-messages-that-contain-sensor-data-tooyour-azure-table-storage"></a><span data-ttu-id="55c77-105">Uložit IoT hub zprávy, které obsahují úložiště tabulek Azure tooyour data snímačů</span><span class="sxs-lookup"><span data-stu-id="55c77-105">Save IoT hub messages that contain sensor data tooyour Azure table storage</span></span>

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/3.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

## <a name="what-you-learn"></a><span data-ttu-id="55c77-107">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="55c77-107">What you learn</span></span>

<span data-ttu-id="55c77-108">Zjistíte, jak toocreate účet úložiště Azure a Azure funkce aplikace toostore IoT Centrum zpráv ve službě table storage.</span><span class="sxs-lookup"><span data-stu-id="55c77-108">You learn how toocreate an Azure storage account and an Azure function app toostore IoT hub messages in your table storage.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="55c77-109">Co dělat</span><span class="sxs-lookup"><span data-stu-id="55c77-109">What you do</span></span>

- <span data-ttu-id="55c77-110">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="55c77-110">Create an Azure storage account.</span></span>
- <span data-ttu-id="55c77-111">Příprava služby IoT hub zprávy tooread připojení.</span><span class="sxs-lookup"><span data-stu-id="55c77-111">Prepare your IoT hub connection tooread messages.</span></span>
- <span data-ttu-id="55c77-112">Vytvořte a nasaďte aplikaci Azure funkce.</span><span class="sxs-lookup"><span data-stu-id="55c77-112">Create and deploy an Azure function app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="55c77-113">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="55c77-113">What you need</span></span>

- <span data-ttu-id="55c77-114">[Nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) toocover hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="55c77-114">[Set up your device](iot-hub-raspberry-pi-kit-node-get-started.md) toocover hello following requirements:</span></span>
  - <span data-ttu-id="55c77-115">Aktivní předplatné Azure</span><span class="sxs-lookup"><span data-stu-id="55c77-115">An active Azure subscription</span></span>
  - <span data-ttu-id="55c77-116">Služby IoT hub v rámci svého předplatného</span><span class="sxs-lookup"><span data-stu-id="55c77-116">An IoT hub under your subscription</span></span> 
  - <span data-ttu-id="55c77-117">Spuštěné aplikace, která odesílá zprávy tooyour IoT hub</span><span class="sxs-lookup"><span data-stu-id="55c77-117">A running application that sends messages tooyour IoT hub</span></span>

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="55c77-118">Vytvoření účtu úložiště Azure</span><span class="sxs-lookup"><span data-stu-id="55c77-118">Create an Azure storage account</span></span>

1. <span data-ttu-id="55c77-119">V hello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **úložiště** > **účet úložiště**  >   **Vytvoření**.</span><span class="sxs-lookup"><span data-stu-id="55c77-119">In hello [Azure portal](https://portal.azure.com/), click **New** > **Storage** > **Storage account** > **Create**.</span></span>

2. <span data-ttu-id="55c77-120">Zadejte hello potřebné informace pro účet úložiště hello:</span><span class="sxs-lookup"><span data-stu-id="55c77-120">Enter hello necessary information for hello storage account:</span></span>

   ![Vytvořit účet úložiště v hello portálu Azure](media\iot-hub-store-data-in-azure-table-storage\1_azure-portal-create-storage-account.png)

   * <span data-ttu-id="55c77-122">**Název**: hello název účtu úložiště hello.</span><span class="sxs-lookup"><span data-stu-id="55c77-122">**Name**: hello name of hello storage account.</span></span> <span data-ttu-id="55c77-123">Název Hello musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="55c77-123">hello name must be globally unique.</span></span>

   * <span data-ttu-id="55c77-124">**Skupina prostředků**: použití hello stejné skupiny prostředků, která používá službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55c77-124">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   * <span data-ttu-id="55c77-125">**PIN kód toodashboard**: tuto možnost vyberte pro Centrum IoT tooyour snadný přístup z řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="55c77-125">**Pin toodashboard**: Select this option for easy access tooyour IoT hub from hello dashboard.</span></span>

3. <span data-ttu-id="55c77-126">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="55c77-126">Click **Create**.</span></span>

## <a name="prepare-your-iot-hub-connection-tooread-messages"></a><span data-ttu-id="55c77-127">Příprava tooread zprávy připojení ke službě IoT hub</span><span class="sxs-lookup"><span data-stu-id="55c77-127">Prepare your IoT hub connection tooread messages</span></span>

<span data-ttu-id="55c77-128">IoT hub zpřístupní předdefinovaný koncový bod kompatibilní s centrem tooenable aplikace tooread IoT hub zprávy o událostech.</span><span class="sxs-lookup"><span data-stu-id="55c77-128">IoT hub exposes a built-in event hub-compatible endpoint tooenable applications tooread IoT hub messages.</span></span> <span data-ttu-id="55c77-129">Mezitím aplikace využívat uživatelských skupin tooread dat ze služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55c77-129">Meanwhile, applications use consumer groups tooread data from your IoT hub.</span></span> <span data-ttu-id="55c77-130">Než vytvoříte model Azure funkce aplikace tooread data ze služby IoT hub, hello následující:</span><span class="sxs-lookup"><span data-stu-id="55c77-130">Before you create an Azure function app tooread data from your IoT hub, do hello following:</span></span>

- <span data-ttu-id="55c77-131">Získáte připojovací řetězec hello koncový bod centra IoT.</span><span class="sxs-lookup"><span data-stu-id="55c77-131">Get hello connection string of your IoT hub endpoint.</span></span>
- <span data-ttu-id="55c77-132">Vytvořte skupinu uživatelů pro službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55c77-132">Create a consumer group for your IoT hub.</span></span>

### <a name="get-hello-connection-string-of-your-iot-hub-endpoint"></a><span data-ttu-id="55c77-133">Získat připojovací řetězec hello koncový bod centra IoT</span><span class="sxs-lookup"><span data-stu-id="55c77-133">Get hello connection string of your IoT hub endpoint</span></span>

1. <span data-ttu-id="55c77-134">Otevřete službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55c77-134">Open your IoT hub.</span></span>

2. <span data-ttu-id="55c77-135">Na hello **IoT Hub** podokně v části **zasílání zpráv**, klikněte na tlačítko **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="55c77-135">On hello **IoT Hub** pane, under **Messaging**, click **Endpoints**.</span></span>

3. <span data-ttu-id="55c77-136">V hello pravým podokně v části **předdefinované koncové body**, klikněte na tlačítko **události**.</span><span class="sxs-lookup"><span data-stu-id="55c77-136">In hello right pane, under **Built-in endpoints**, click **Events**.</span></span>

4. <span data-ttu-id="55c77-137">V hello **vlastnosti** podokně, Poznámka hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="55c77-137">In hello **Properties** pane, note hello following values:</span></span>
   - <span data-ttu-id="55c77-138">Koncový bod kompatibilní s centrem událostí</span><span class="sxs-lookup"><span data-stu-id="55c77-138">Event hub-compatible endpoint</span></span>
   - <span data-ttu-id="55c77-139">Název kompatibilní s centrem událostí</span><span class="sxs-lookup"><span data-stu-id="55c77-139">Event hub-compatible name</span></span>

   ![Získat připojovací řetězec hello koncový bod centra IoT v hello portálu Azure](media\iot-hub-store-data-in-azure-table-storage\2_azure-portal-iot-hub-endpoint-connection-string.png)

5. <span data-ttu-id="55c77-141">V hello **IoT Hub** podokně v části **nastavení**, klikněte na tlačítko **zásady sdíleného přístupu**.</span><span class="sxs-lookup"><span data-stu-id="55c77-141">In hello **IoT Hub** pane, under **Settings**, click **Shared access policies**.</span></span>

6. <span data-ttu-id="55c77-142">Klikněte na tlačítko **iothubowner**.</span><span class="sxs-lookup"><span data-stu-id="55c77-142">Click **iothubowner**.</span></span>

7. <span data-ttu-id="55c77-143">Poznámka: hello **primární klíč** hodnotu.</span><span class="sxs-lookup"><span data-stu-id="55c77-143">Note hello **Primary key** value.</span></span>

8. <span data-ttu-id="55c77-144">Připojovací řetězec hello koncový bod centra IoT vytvořte takto:</span><span class="sxs-lookup"><span data-stu-id="55c77-144">Create hello connection string of your IoT hub endpoint as follows:</span></span>

   `Endpoint=<Event Hub-compatible endpoint>;SharedAccessKeyName=iothubowner;SharedAccessKey=<Primary key>`

   > [!NOTE]
   > <span data-ttu-id="55c77-145">Nahraďte `<Event Hub-compatible endpoint>` a `<Primary key>` hello hodnotami, které jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="55c77-145">Replace `<Event Hub-compatible endpoint>` and `<Primary key>` with hello values that you noted earlier.</span></span>

### <a name="create-a-consumer-group-for-your-iot-hub"></a><span data-ttu-id="55c77-146">Vytvořte skupinu uživatelů pro službu IoT hub</span><span class="sxs-lookup"><span data-stu-id="55c77-146">Create a consumer group for your IoT hub</span></span>

1. <span data-ttu-id="55c77-147">Otevřete službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55c77-147">Open your IoT hub.</span></span>

2. <span data-ttu-id="55c77-148">V hello **IoT Hub** podokně v části **zasílání zpráv**, klikněte na tlačítko **koncové body**.</span><span class="sxs-lookup"><span data-stu-id="55c77-148">In hello **IoT Hub** pane, under **Messaging**, click **Endpoints**.</span></span>

3. <span data-ttu-id="55c77-149">V hello pravým podokně v části **předdefinované koncové body**, klikněte na tlačítko **události**.</span><span class="sxs-lookup"><span data-stu-id="55c77-149">In hello right pane, under **Built-in endpoints**, click **Events**.</span></span>

4. <span data-ttu-id="55c77-150">V hello **vlastnosti** podokně v části **skupiny příjemců**, zadejte název a poznamenejte si ho.</span><span class="sxs-lookup"><span data-stu-id="55c77-150">In hello **Properties** pane, under **Consumer groups**, enter a name, and then make a note of it.</span></span>

5. <span data-ttu-id="55c77-151">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="55c77-151">Click **Save**.</span></span>

## <a name="create-and-deploy-an-azure-function-app"></a><span data-ttu-id="55c77-152">Vytvoření a nasazení aplikace Azure – funkce</span><span class="sxs-lookup"><span data-stu-id="55c77-152">Create and deploy an Azure function app</span></span>

1. <span data-ttu-id="55c77-153">V hello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **výpočetní** > **aplikaci funkce**  >   **Vytvoření**.</span><span class="sxs-lookup"><span data-stu-id="55c77-153">In hello [Azure portal](https://portal.azure.com/), click **New** > **Compute** > **Function App** > **Create**.</span></span>

2. <span data-ttu-id="55c77-154">Zadejte nezbytné informace hello hello funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="55c77-154">Enter hello necessary information for hello function app.</span></span>

   ![Vytvoření aplikace pro funkce v hello portálu Azure](media\iot-hub-store-data-in-azure-table-storage\3_azure-portal-create-function-app.png)

   * <span data-ttu-id="55c77-156">**Název aplikace**: název hello hello funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="55c77-156">**App name**: hello name of hello function app.</span></span> <span data-ttu-id="55c77-157">Název Hello musí být globálně jedinečný.</span><span class="sxs-lookup"><span data-stu-id="55c77-157">hello name must be globally unique.</span></span>

   * <span data-ttu-id="55c77-158">**Skupina prostředků**: použití hello stejné skupiny prostředků, která používá službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55c77-158">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   * <span data-ttu-id="55c77-159">**Účet úložiště**: hello účet úložiště, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="55c77-159">**Storage Account**: hello storage account that you created.</span></span>

   * <span data-ttu-id="55c77-160">**PIN kód toodashboard**: zaškrtnete tuto možnost pro aplikaci funkce toohello snadný přístup z řídicího panelu hello.</span><span class="sxs-lookup"><span data-stu-id="55c77-160">**Pin toodashboard**: Check this option for easy access toohello function app from hello dashboard.</span></span>

3. <span data-ttu-id="55c77-161">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="55c77-161">Click **Create**.</span></span>

4. <span data-ttu-id="55c77-162">Po vytvoření hello funkce aplikace, otevřete ho.</span><span class="sxs-lookup"><span data-stu-id="55c77-162">After hello function app has been created, open it.</span></span>

5. <span data-ttu-id="55c77-163">V aplikaci funkce hello vytvořte novou funkci pomocí tohoto postupu hello následující:</span><span class="sxs-lookup"><span data-stu-id="55c77-163">In hello function app, create a new function by doing hello following:</span></span>

   <span data-ttu-id="55c77-164">a.</span><span class="sxs-lookup"><span data-stu-id="55c77-164">a.</span></span> <span data-ttu-id="55c77-165">Klikněte na tlačítko **novou funkci**.</span><span class="sxs-lookup"><span data-stu-id="55c77-165">Click **New Function**.</span></span>

   <span data-ttu-id="55c77-166">b.</span><span class="sxs-lookup"><span data-stu-id="55c77-166">b.</span></span> <span data-ttu-id="55c77-167">Vyberte **JavaScript** pro **jazyk**, a **zpracování dat** pro **scénář**.</span><span class="sxs-lookup"><span data-stu-id="55c77-167">Select **JavaScript** for **Language**, and **Data Processing** for **Scenario**.</span></span>

   <span data-ttu-id="55c77-168">c.</span><span class="sxs-lookup"><span data-stu-id="55c77-168">c.</span></span> <span data-ttu-id="55c77-169">Klikněte na tlačítko **vytvořit tuto funkci**a potom klikněte na **novou funkci**.</span><span class="sxs-lookup"><span data-stu-id="55c77-169">Click **Create this function**, and then click **New Function**.</span></span>

   <span data-ttu-id="55c77-170">d.</span><span class="sxs-lookup"><span data-stu-id="55c77-170">d.</span></span> <span data-ttu-id="55c77-171">Vyberte **JavaScript** pro jazyk hello a **zpracování dat** pro scénář hello.</span><span class="sxs-lookup"><span data-stu-id="55c77-171">Select **JavaScript** for hello language, and **Data Processing** for hello scenario.</span></span>

   <span data-ttu-id="55c77-172">e.</span><span class="sxs-lookup"><span data-stu-id="55c77-172">e.</span></span> <span data-ttu-id="55c77-173">Klikněte na tlačítko hello **EventHubTrigger JavaScript** šablony.</span><span class="sxs-lookup"><span data-stu-id="55c77-173">Click hello **EventHubTrigger-JavaScript** template.</span></span>

   <span data-ttu-id="55c77-174">f.</span><span class="sxs-lookup"><span data-stu-id="55c77-174">f.</span></span> <span data-ttu-id="55c77-175">Zadejte nezbytné informace hello hello šablony.</span><span class="sxs-lookup"><span data-stu-id="55c77-175">Enter hello necessary information for hello template.</span></span>

      * <span data-ttu-id="55c77-176">**Název funkce**: hello název funkce hello.</span><span class="sxs-lookup"><span data-stu-id="55c77-176">**Name your function**: hello name of hello function.</span></span>

      * <span data-ttu-id="55c77-177">**Název centra událostí**: hello název kompatibilní s centrem událostí, který jste si předtím poznamenali.</span><span class="sxs-lookup"><span data-stu-id="55c77-177">**Event Hub name**: hello event hub-compatible name that you noted earlier.</span></span>

      * <span data-ttu-id="55c77-178">**Připojení rozbočovače událostí**: tooadd hello připojovací řetězec hello koncový bod centra IoT kterou jste vytvořili, klikněte na tlačítko **nový**.</span><span class="sxs-lookup"><span data-stu-id="55c77-178">**Event Hub connection**: tooadd hello connection string of hello IoT hub endpoint that you created, click **New**.</span></span>

   <span data-ttu-id="55c77-179">g.</span><span class="sxs-lookup"><span data-stu-id="55c77-179">g.</span></span> <span data-ttu-id="55c77-180">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="55c77-180">Click **Create**.</span></span>

6. <span data-ttu-id="55c77-181">Nakonfigurujte výstup hello funkce pomocí hello následující:</span><span class="sxs-lookup"><span data-stu-id="55c77-181">Configure an output of hello function by doing hello following:</span></span>

   <span data-ttu-id="55c77-182">a.</span><span class="sxs-lookup"><span data-stu-id="55c77-182">a.</span></span> <span data-ttu-id="55c77-183">Klikněte na tlačítko **integrovat** > **nový výstupní** > **Azure Table Storage** > **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="55c77-183">Click **Integrate** > **New Output** > **Azure Table Storage** > **Select**.</span></span>

      ![Přidání tabulky úložiště tooyour funkce aplikace v hello portálu Azure](media\iot-hub-store-data-in-azure-table-storage\4_azure-portal-function-app-add-output-table-storage.png)

   <span data-ttu-id="55c77-185">b.</span><span class="sxs-lookup"><span data-stu-id="55c77-185">b.</span></span> <span data-ttu-id="55c77-186">Zadejte nezbytné informace hello.</span><span class="sxs-lookup"><span data-stu-id="55c77-186">Enter hello necessary information.</span></span>

      * <span data-ttu-id="55c77-187">**Parametr název tabulky**: použití `outputTable`, který se používá v hello Azure kód funkce.</span><span class="sxs-lookup"><span data-stu-id="55c77-187">**Table parameter name**: Use `outputTable`, which will be used in hello Azure function's code.</span></span>
      
      * <span data-ttu-id="55c77-188">**Název tabulky**: použití `deviceData`.</span><span class="sxs-lookup"><span data-stu-id="55c77-188">**Table name**: Use `deviceData`.</span></span>

      * <span data-ttu-id="55c77-189">**Připojení účtu úložiště**: klikněte na tlačítko **nový**a potom vyberte nebo zadejte svůj účet úložiště.</span><span class="sxs-lookup"><span data-stu-id="55c77-189">**Storage account connection**: Click **New**, and then select or enter your storage account.</span></span> <span data-ttu-id="55c77-190">Pokud se nezobrazí hello účet úložiště, najdete v části [požadavky na účet úložiště](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).</span><span class="sxs-lookup"><span data-stu-id="55c77-190">If hello storage account is not displayed, see [Storage account requirements](https://docs.microsoft.com/azure/azure-functions/functions-create-function-app-portal#storage-account-requirements).</span></span>
      
   <span data-ttu-id="55c77-191">c.</span><span class="sxs-lookup"><span data-stu-id="55c77-191">c.</span></span> <span data-ttu-id="55c77-192">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="55c77-192">Click **Save**.</span></span>

7. <span data-ttu-id="55c77-193">V části **aktivační události**, klikněte na tlačítko **centra událostí Azure (eventHubMessages)**.</span><span class="sxs-lookup"><span data-stu-id="55c77-193">Under **Triggers**, click **Azure Event Hub (eventHubMessages)**.</span></span>

8. <span data-ttu-id="55c77-194">V části **skupiny příjemců centra událostí**, zadejte název hello skupiny hello příjemce, který jste vytvořili a pak klikněte na tlačítko **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="55c77-194">Under **Event Hub consumer group**, enter hello name of hello consumer group that you created, and then click **Save**.</span></span>

9. <span data-ttu-id="55c77-195">Klikněte na tlačítko hello funkce, které jste vytvořili na hello vlevo a pak klikněte na **zobrazit soubory** na hello správné.</span><span class="sxs-lookup"><span data-stu-id="55c77-195">Click hello function you've created on hello left and then click **View Files** on hello right.</span></span>

10. <span data-ttu-id="55c77-196">Nahraďte kód hello v `index.js` s hello následující:</span><span class="sxs-lookup"><span data-stu-id="55c77-196">Replace hello code in `index.js` with hello following:</span></span>

   ```javascript
   'use strict';

   // This function is triggered each time a message is received in hello IoT hub.
   // hello message payload is persisted in an Azure storage table
 
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

11. <span data-ttu-id="55c77-197">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="55c77-197">Click **Save**.</span></span>

<span data-ttu-id="55c77-198">Nyní jste vytvořili aplikaci funkce hello.</span><span class="sxs-lookup"><span data-stu-id="55c77-198">You have now created hello function app.</span></span> <span data-ttu-id="55c77-199">Ukládá zprávy, které IoT hub přijímá ve službě table storage.</span><span class="sxs-lookup"><span data-stu-id="55c77-199">It stores messages that your IoT hub receives in your table storage.</span></span>

> [!NOTE]
> <span data-ttu-id="55c77-200">Můžete použít hello **spustit** tlačítko tootest hello funkce aplikace.</span><span class="sxs-lookup"><span data-stu-id="55c77-200">You can use hello **Run** button tootest hello function app.</span></span> <span data-ttu-id="55c77-201">Když kliknete na tlačítko **spustit**, hello zkušební zprávu se odeslat tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55c77-201">When you click **Run**, hello test message is sent tooyour IoT hub.</span></span> <span data-ttu-id="55c77-202">Hello doručení zprávy hello by měly aktivovat hello funkce aplikace toostart a potom uložte hello zpráva tooyour tabulka úložiště.</span><span class="sxs-lookup"><span data-stu-id="55c77-202">hello arrival of hello message should trigger hello function app toostart and then save hello message tooyour table storage.</span></span> <span data-ttu-id="55c77-203">Hello **protokoly** podokně zaznamenává hello podrobnosti o procesu hello.</span><span class="sxs-lookup"><span data-stu-id="55c77-203">hello **Logs** pane records hello details of hello process.</span></span>

## <a name="verify-your-message-in-your-table-storage"></a><span data-ttu-id="55c77-204">Ověřit zprávu ve službě table storage</span><span class="sxs-lookup"><span data-stu-id="55c77-204">Verify your message in your table storage</span></span>

1. <span data-ttu-id="55c77-205">Spusťte hello ukázkovou aplikaci na zařízení toosend zprávy tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="55c77-205">Run hello sample application on your device toosend messages tooyour IoT hub.</span></span>

2. <span data-ttu-id="55c77-206">[Stáhněte a nainstalujte Azure Storage Explorer](http://storageexplorer.com/).</span><span class="sxs-lookup"><span data-stu-id="55c77-206">[Download and install Azure Storage Explorer](http://storageexplorer.com/).</span></span>

3. <span data-ttu-id="55c77-207">Otevřete Storage Explorer, klikněte na **přidat účet Azure** > **přihlášení**a potom se přihlaste tooyour účet Azure.</span><span class="sxs-lookup"><span data-stu-id="55c77-207">Open Storage Explorer, click **Add an Azure Account** > **Sign in**, and then sign in tooyour Azure account.</span></span>

4. <span data-ttu-id="55c77-208">Klikněte na předplatné Azure > **účty úložiště** > váš účet úložiště > **tabulky** > **deviceData**.</span><span class="sxs-lookup"><span data-stu-id="55c77-208">Click your Azure subscription > **Storage Accounts** > your storage account > **Tables** > **deviceData**.</span></span>

   <span data-ttu-id="55c77-209">Měli byste vidět zprávy odeslané ze zařízení IoT hub tooyour přihlášení hello `deviceData` tabulky.</span><span class="sxs-lookup"><span data-stu-id="55c77-209">You should see messages sent from your device tooyour IoT hub logged in hello `deviceData` table.</span></span>

## <a name="next-steps"></a><span data-ttu-id="55c77-210">Další kroky</span><span class="sxs-lookup"><span data-stu-id="55c77-210">Next steps</span></span>

<span data-ttu-id="55c77-211">Úspěšně jste vytvořili účet úložiště Azure a Azure funkce aplikace, která ukládá zprávy, které IoT hub přijímá ve službě table storage.</span><span class="sxs-lookup"><span data-stu-id="55c77-211">You’ve successfully created your Azure storage account and Azure function app, which stores messages that your IoT hub receives in your table storage.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
