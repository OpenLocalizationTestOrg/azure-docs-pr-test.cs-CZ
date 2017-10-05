---
title: "Vzdálené sledování IoT a oznámení službou Azure Logic Apps | Microsoft Docs"
description: "Použít Azure Logic Apps pro IoT teploty sledování ve službě IoT hub a automaticky odesílat e-mailová oznámení do poštovní schránky pro případných zjištěných."
services: iot-hub
documentationcenter: 
author: shizn
manager: timlt
tags: 
keywords: "IOT iot oznámení monitorování monitorování teploty iot"
ms.assetid: 43043067-2e1f-42c9-953d-e2dce8fd86df
ms.service: iot-hub
ms.devlang: arduino
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: xshi
ms.openlocfilehash: 7a611912ae55eb22103539dbba9f1a06aaa543b7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a><span data-ttu-id="aab16-104">IoT pro vzdálené monitorování a oznámení službou Azure Logic Apps připojení služby IoT hub a poštovní schránky</span><span class="sxs-lookup"><span data-stu-id="aab16-104">IoT remote monitoring and notifications with Azure Logic Apps connecting your IoT hub and mailbox</span></span>

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/7.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="aab16-106">Azure Logic Apps poskytuje způsob, jak automatizovat procesy jako řadu kroků.</span><span class="sxs-lookup"><span data-stu-id="aab16-106">Azure Logic Apps provides a way to automate processes as a series of steps.</span></span> <span data-ttu-id="aab16-107">Aplikace logiky můžete připojit přes různé služby a protokoly.</span><span class="sxs-lookup"><span data-stu-id="aab16-107">A logic app can connect across various services and protocols.</span></span> <span data-ttu-id="aab16-108">Začíná aktivační událost, jako "Při přidání účtu" a a kombinace akcí, jako "odesílat nabízená oznámení".</span><span class="sxs-lookup"><span data-stu-id="aab16-108">It begins with a trigger such as 'When an account is added', and followed by a combination of actions, one like 'sending a push notification'.</span></span> <span data-ttu-id="aab16-109">Díky této funkci Logic Apps ideální řešení IoT pro IoT monitorování, jako je například zachování výstrahy anomálií mezi scénáře použití.</span><span class="sxs-lookup"><span data-stu-id="aab16-109">This feature makes Logic Apps a perfect IoT solution for IoT monitoring, such as staying alert for anomalies, among other usage scenarios.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="aab16-110">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="aab16-110">What you learn</span></span>

<span data-ttu-id="aab16-111">Zjistíte, jak vytvořit aplikaci logiky, která se připojuje služby IoT hub a poštovní schránky pro monitorování teploty a oznámení.</span><span class="sxs-lookup"><span data-stu-id="aab16-111">You learn how to create a logic app that connects your IoT hub and your mailbox for temperature monitoring and notifications.</span></span> <span data-ttu-id="aab16-112">Když je nad 30 C, klientská aplikace označí `temperatureAlert = "true"` ve zprávě, odešle do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="aab16-112">When the temperature is above 30 C, the client application marks `temperatureAlert = "true"` in the message it sends to your IoT hub.</span></span> <span data-ttu-id="aab16-113">Zpráva se aktivuje aplikaci logiky můžete odeslat e-mailové oznámení.</span><span class="sxs-lookup"><span data-stu-id="aab16-113">The message triggers the logic app to send you an email notification.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="aab16-114">Co dělat</span><span class="sxs-lookup"><span data-stu-id="aab16-114">What you do</span></span>

* <span data-ttu-id="aab16-115">Vytvořit obor názvů sběrnice služby a přidejte do ní fronty.</span><span class="sxs-lookup"><span data-stu-id="aab16-115">Create a service bus namespace and add a queue to it.</span></span>
* <span data-ttu-id="aab16-116">Přidáte koncový bod a pravidel směrování do služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="aab16-116">Add an endpoint and a routing rule to your IoT hub.</span></span>
* <span data-ttu-id="aab16-117">Vytvoření, konfiguraci a testování aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="aab16-117">Create, configure, and test a logic app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="aab16-118">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="aab16-118">What you need</span></span>

* <span data-ttu-id="aab16-119">Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="aab16-119">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers the following requirements:</span></span>
  * <span data-ttu-id="aab16-120">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="aab16-120">An active Azure subscription.</span></span>
  * <span data-ttu-id="aab16-121">V rámci svého předplatného služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="aab16-121">An Azure IoT hub under your subscription.</span></span>
  * <span data-ttu-id="aab16-122">Klientská aplikace, která odesílá zprávy do služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="aab16-122">A client application that sends messages to your Azure IoT hub.</span></span>

## <a name="create-service-bus-namespace-and-add-a-queue-to-it"></a><span data-ttu-id="aab16-123">Vytvoření oboru názvů service bus a přidejte do ní fronty</span><span class="sxs-lookup"><span data-stu-id="aab16-123">Create service bus namespace and add a queue to it</span></span>

### <a name="create-a-service-bus-namespace"></a><span data-ttu-id="aab16-124">Vytvořit obor názvů sběrnice služby</span><span class="sxs-lookup"><span data-stu-id="aab16-124">Create a service bus namespace</span></span>

1. <span data-ttu-id="aab16-125">Na [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **Enterprise integrace** > **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="aab16-125">On the [Azure portal](https://portal.azure.com/), click **New** > **Enterprise Integration** > **Service Bus**.</span></span>
1. <span data-ttu-id="aab16-126">Zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="aab16-126">Provide the following information:</span></span>

   <span data-ttu-id="aab16-127">**Název**: název služby service bus.</span><span class="sxs-lookup"><span data-stu-id="aab16-127">**Name**: The name of the service bus.</span></span>

   <span data-ttu-id="aab16-128">**Cenová úroveň**: klikněte na tlačítko **základní** > **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="aab16-128">**Pricing tier**: Click **Basic** > **Select**.</span></span> <span data-ttu-id="aab16-129">Úroveň Basic je dostačující pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="aab16-129">The Basic tier is sufficient for this tutorial.</span></span>

   <span data-ttu-id="aab16-130">**Skupina prostředků**: použijte stejnou skupinu prostředků, která používá službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="aab16-130">**Resource group**: Use the same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="aab16-131">**Umístění**: používalo stejné umístění, která používá službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="aab16-131">**Location**: Use the same location that your IoT hub uses.</span></span>
1. <span data-ttu-id="aab16-132">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="aab16-132">Click **Create**.</span></span>

   ![Vytvořit obor názvů sběrnice služby na portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a><span data-ttu-id="aab16-134">Přidat frontou service bus</span><span class="sxs-lookup"><span data-stu-id="aab16-134">Add a service bus queue</span></span>

1. <span data-ttu-id="aab16-135">Otevřete obor názvů sběrnice a pak klikněte na tlačítko **+ fronty**.</span><span class="sxs-lookup"><span data-stu-id="aab16-135">Open the service bus namespace, and then click **+ Queue**.</span></span>
1. <span data-ttu-id="aab16-136">Zadejte název pro frontu a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="aab16-136">Enter a name for the queue and then click **Create**.</span></span>
1. <span data-ttu-id="aab16-137">Otevřete frontou service bus a pak klikněte na tlačítko **zásady sdíleného přístupu** > **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="aab16-137">Open the service bus queue, and then click **Shared access policies** > **+ Add**.</span></span>
1. <span data-ttu-id="aab16-138">Zadejte název zásady, zkontrolujte **spravovat**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="aab16-138">Enter a name for the policy, check **Manage**, and then click **Create**.</span></span>

   ![Přidat frontou service bus na portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-rule-to-your-iot-hub"></a><span data-ttu-id="aab16-140">Přidat koncový bod a pravidel směrování do služby IoT hub</span><span class="sxs-lookup"><span data-stu-id="aab16-140">Add an endpoint and a routing rule to your IoT hub</span></span>

### <a name="add-an-endpoint"></a><span data-ttu-id="aab16-141">Přidání koncového bodu</span><span class="sxs-lookup"><span data-stu-id="aab16-141">Add an endpoint</span></span>

1. <span data-ttu-id="aab16-142">Otevřete službu IoT hub, klikněte na koncové body > + Přidat.</span><span class="sxs-lookup"><span data-stu-id="aab16-142">Open your IoT hub, click Endpoints > + Add.</span></span>
1. <span data-ttu-id="aab16-143">Zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="aab16-143">Enter the following information:</span></span>

   <span data-ttu-id="aab16-144">**Název**: název koncového bodu.</span><span class="sxs-lookup"><span data-stu-id="aab16-144">**Name**: The name of the endpoint.</span></span>

   <span data-ttu-id="aab16-145">**Typ koncového bodu**: vyberte **frontou Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="aab16-145">**Endpoint type**: Select **Service Bus Queue**.</span></span>

   <span data-ttu-id="aab16-146">**Obor názvů sběrnice**: Vyberte obor názvů, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="aab16-146">**Service Bus namespace**: Select the namespace you created.</span></span>

   <span data-ttu-id="aab16-147">**Fronty Service Bus**: vyberte fronty, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="aab16-147">**Service Bus queue**: Select the queue you created.</span></span>
1. <span data-ttu-id="aab16-148">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="aab16-148">Click **OK**.</span></span>

   ![Přidání koncového bodu do služby IoT hub v portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a><span data-ttu-id="aab16-150">Přidat pravidlo směrování</span><span class="sxs-lookup"><span data-stu-id="aab16-150">Add a routing rule</span></span>

1. <span data-ttu-id="aab16-151">Ve službě IoT hub, klikněte na tlačítko **trasy** > **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="aab16-151">In your IoT hub, click **Routes** > **+ Add**.</span></span>
1. <span data-ttu-id="aab16-152">Zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="aab16-152">Enter the following information:</span></span>

   <span data-ttu-id="aab16-153">**Název**: název pravidla směrování.</span><span class="sxs-lookup"><span data-stu-id="aab16-153">**Name**: The name of the routing rule.</span></span>

   <span data-ttu-id="aab16-154">**Zdroj dat**: vyberte **DeviceMessages**.</span><span class="sxs-lookup"><span data-stu-id="aab16-154">**Data source**: Select **DeviceMessages**.</span></span>

   <span data-ttu-id="aab16-155">**Koncový bod**: Vyberte koncový bod, který jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="aab16-155">**Endpoint**: Select the endpoint you created.</span></span>

   <span data-ttu-id="aab16-156">**Řetězec dotazu**: Zadejte `temperatureAlert = "true"`.</span><span class="sxs-lookup"><span data-stu-id="aab16-156">**Query string**: Enter `temperatureAlert = "true"`.</span></span>
1. <span data-ttu-id="aab16-157">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="aab16-157">Click **Save**.</span></span>

   ![Přidat pravidlo směrování na portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a><span data-ttu-id="aab16-159">Vytvoření a konfigurace aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="aab16-159">Create and configure a logic app</span></span>

### <a name="create-a-logic-app"></a><span data-ttu-id="aab16-160">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="aab16-160">Create a logic app</span></span>

1. <span data-ttu-id="aab16-161">V [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **Enterprise integrace** > **aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="aab16-161">In the [Azure portal](https://portal.azure.com/), click **New** > **Enterprise Integration** > **Logic App**.</span></span>
1. <span data-ttu-id="aab16-162">Zadejte následující informace:</span><span class="sxs-lookup"><span data-stu-id="aab16-162">Enter the following information:</span></span>

   <span data-ttu-id="aab16-163">**Název**: název aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="aab16-163">**Name**: The name of the logic app.</span></span>

   <span data-ttu-id="aab16-164">**Skupina prostředků**: použijte stejnou skupinu prostředků, která používá službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="aab16-164">**Resource group**: Use the same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="aab16-165">**Umístění**: používalo stejné umístění, která používá službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="aab16-165">**Location**: Use the same location that your IoT hub uses.</span></span>
1. <span data-ttu-id="aab16-166">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="aab16-166">Click **Create**.</span></span>

### <a name="configure-the-logic-app"></a><span data-ttu-id="aab16-167">Konfigurace aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="aab16-167">Configure the logic app</span></span>

1. <span data-ttu-id="aab16-168">Otevřete aplikaci logiky, které se otevře v Návrháři logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="aab16-168">Open the logic app that opens into the Logic Apps Designer.</span></span>
1. <span data-ttu-id="aab16-169">V návrháři aplikace logiky, klikněte na tlačítko **prázdné aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="aab16-169">In the Logic Apps Designer, click **Blank Logic App**.</span></span>

   ![Začít s prázdnou logiku aplikace na portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

1. <span data-ttu-id="aab16-171">Klikněte na tlačítko **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="aab16-171">Click **Service Bus**.</span></span>

   ![Vyberte služby Service Bus chcete začít vytvářet aplikaci logiky na portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. <span data-ttu-id="aab16-173">Klikněte na tlačítko **Service Bus – při obdržení jeden nebo více zpráv ve frontě (automatické dokončování)**.</span><span class="sxs-lookup"><span data-stu-id="aab16-173">Click **Service Bus – When one or more messages arrive in a queue (auto-complete)**.</span></span>
1. <span data-ttu-id="aab16-174">Umožňuje vytvořte připojení service bus.</span><span class="sxs-lookup"><span data-stu-id="aab16-174">Create a service bus connection.</span></span>
   1. <span data-ttu-id="aab16-175">Zadejte název připojení.</span><span class="sxs-lookup"><span data-stu-id="aab16-175">Enter a connection name.</span></span>
   1. <span data-ttu-id="aab16-176">Klikněte na obor názvů service bus > zásady service bus > **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="aab16-176">Click the service bus namespace > the service bus policy > **Create**.</span></span>

      ![Vytvořte připojení sběrnice služby pro svou aplikaci logiky na portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   1. <span data-ttu-id="aab16-178">Klikněte na tlačítko **pokračovat** po vytvoření připojení k službě sběrnice.</span><span class="sxs-lookup"><span data-stu-id="aab16-178">Click **Continue** after the service bus connection is created.</span></span>
   1. <span data-ttu-id="aab16-179">Vybrat fronty, kterou jste vytvořili a zadat `175` pro **maximální počet zpráv**</span><span class="sxs-lookup"><span data-stu-id="aab16-179">Select the queue that you created and enter `175` for **Maximum message count**</span></span>

      ![Zadejte počet maximální zpráv pro připojení služby service bus v aplikaci logiky](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)
   1. <span data-ttu-id="aab16-181">Klikněte na "tlačítko Uložit změny uložit".</span><span class="sxs-lookup"><span data-stu-id="aab16-181">Click "Save" button to save the changes.</span></span>

1. <span data-ttu-id="aab16-182">Vytvoření připojení služby SMTP.</span><span class="sxs-lookup"><span data-stu-id="aab16-182">Create an SMTP service connection.</span></span>
   1. <span data-ttu-id="aab16-183">Klikněte na tlačítko **nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="aab16-183">Click **New step** > **Add an action**.</span></span>
   1. <span data-ttu-id="aab16-184">Typ `SMTP`, klikněte **SMTP** služby ve výsledcích hledání a potom klikněte na **SMTP - odeslat E-mail**.</span><span class="sxs-lookup"><span data-stu-id="aab16-184">Type `SMTP`, click the **SMTP** service in the search result, and then click **SMTP - Send Email**.</span></span>

      ![Vytvoření připojení k SMTP ve vaší aplikaci logiky na portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   1. <span data-ttu-id="aab16-186">Zadejte informace SMTP poštovní schránky a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="aab16-186">Enter the SMTP information of your mailbox, and then click **Create**.</span></span>

      ![Zadejte informace o připojení protokolu SMTP ve vaší aplikaci logiky na portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      <span data-ttu-id="aab16-188">Získat informace o SMTP [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [z Gmailu](https://support.google.com/a/answer/176600?hl=en), a [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).</span><span class="sxs-lookup"><span data-stu-id="aab16-188">Get the SMTP information for [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), and [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).</span></span>
   1. <span data-ttu-id="aab16-189">Zadejte e-mailovou adresu pro **z** a **k**, a `High temperature detected` pro **subjektu** a **textu**.</span><span class="sxs-lookup"><span data-stu-id="aab16-189">Enter your email address for **From** and **To**, and `High temperature detected` for **Subject** and **Body**.</span></span>
   1. <span data-ttu-id="aab16-190">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="aab16-190">Click **Save**.</span></span>

<span data-ttu-id="aab16-191">Aplikace logiky je ve funkčním stavu, když jste jej uložili.</span><span class="sxs-lookup"><span data-stu-id="aab16-191">The logic app is in working order when you save it.</span></span>

## <a name="test-the-logic-app"></a><span data-ttu-id="aab16-192">Aplikaci logiky si otestujte</span><span class="sxs-lookup"><span data-stu-id="aab16-192">Test the logic app</span></span>

1. <span data-ttu-id="aab16-193">Spustit aplikaci klienta, který nasadíte do zařízení v [ESP8266 připojit ke službě Azure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="aab16-193">Start the client application that you deploy to your device in [Connect ESP8266 to Azure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md).</span></span>
1. <span data-ttu-id="aab16-194">Zvýšení teploty prostředí kolem SensorTag být vyšší než 30 C. Například light svíčkového grafu okolo vaší SensorTag.</span><span class="sxs-lookup"><span data-stu-id="aab16-194">Increase the environment temperature around the SensorTag to be above 30 C. For example, light a candle around your SensorTag.</span></span>
1. <span data-ttu-id="aab16-195">Měli byste obdržet e-mail s oznámením poslal aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="aab16-195">You should receive an email notification sent by the logic app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="aab16-196">Poskytovatel služeb e-mailu možná muset ověřit identitu odesílatele a ujistěte se, že se že jedná, kdo odešle e-mailu.</span><span class="sxs-lookup"><span data-stu-id="aab16-196">Your email service provider may need to verify the sender identity to make sure it is you who sends the email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="aab16-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="aab16-197">Next steps</span></span>

<span data-ttu-id="aab16-198">Úspěšně jste vytvořili aplikaci logiky, která se připojuje služby IoT hub a poštovní schránky pro monitorování teploty a oznámení.</span><span class="sxs-lookup"><span data-stu-id="aab16-198">You have successfully created a logic app that connects your IoT hub and your mailbox for temperature monitoring and notifications.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
