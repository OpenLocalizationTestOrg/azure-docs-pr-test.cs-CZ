---
title: "aaaIoT vzdálené monitorování a oznámení službou Azure Logic Apps | Microsoft Docs"
description: "Použití Azure Logic Apps pro IoT teploty monitorování ve službě IoT hub a automaticky odesílat e-mailové oznámení tooyour schránky pro případných zjištěných."
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
ms.openlocfilehash: 89396528ed63c37258e1b49f342f0723e686ecb3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="iot-remote-monitoring-and-notifications-with-azure-logic-apps-connecting-your-iot-hub-and-mailbox"></a><span data-ttu-id="9a8d0-104">IoT pro vzdálené monitorování a oznámení službou Azure Logic Apps připojení služby IoT hub a poštovní schránky</span><span class="sxs-lookup"><span data-stu-id="9a8d0-104">IoT remote monitoring and notifications with Azure Logic Apps connecting your IoT hub and mailbox</span></span>

![Diagram začátku do konce](media/iot-hub-get-started-e2e-diagram/7.png)

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

<span data-ttu-id="9a8d0-106">Azure Logic Apps poskytuje způsob tooautomate procesy jako řadu kroků.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-106">Azure Logic Apps provides a way tooautomate processes as a series of steps.</span></span> <span data-ttu-id="9a8d0-107">Aplikace logiky můžete připojit přes různé služby a protokoly.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-107">A logic app can connect across various services and protocols.</span></span> <span data-ttu-id="9a8d0-108">Začíná aktivační událost, jako "Při přidání účtu" a a kombinace akcí, jako "odesílat nabízená oznámení".</span><span class="sxs-lookup"><span data-stu-id="9a8d0-108">It begins with a trigger such as 'When an account is added', and followed by a combination of actions, one like 'sending a push notification'.</span></span> <span data-ttu-id="9a8d0-109">Díky této funkci Logic Apps ideální řešení IoT pro IoT monitorování, jako je například zachování výstrahy anomálií mezi scénáře použití.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-109">This feature makes Logic Apps a perfect IoT solution for IoT monitoring, such as staying alert for anomalies, among other usage scenarios.</span></span>

## <a name="what-you-learn"></a><span data-ttu-id="9a8d0-110">Co se naučíte</span><span class="sxs-lookup"><span data-stu-id="9a8d0-110">What you learn</span></span>

<span data-ttu-id="9a8d0-111">Zjistíte, jak toocreate aplikace logiky, která se připojuje služby IoT hub a poštovní schránky pro monitorování teploty a oznámení.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-111">You learn how toocreate a logic app that connects your IoT hub and your mailbox for temperature monitoring and notifications.</span></span> <span data-ttu-id="9a8d0-112">Když hello je nad 30 C, hello klienta aplikace značky `temperatureAlert = "true"` ve zprávě hello odešle tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-112">When hello temperature is above 30 C, hello client application marks `temperatureAlert = "true"` in hello message it sends tooyour IoT hub.</span></span> <span data-ttu-id="9a8d0-113">Hello zprávy aktivačních událostí hello toosend aplikace logiky můžete e-mailové oznámení.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-113">hello message triggers hello logic app toosend you an email notification.</span></span>

## <a name="what-you-do"></a><span data-ttu-id="9a8d0-114">Co dělat</span><span class="sxs-lookup"><span data-stu-id="9a8d0-114">What you do</span></span>

* <span data-ttu-id="9a8d0-115">Vytvořit obor názvů sběrnice služby a přidejte tooit fronty.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-115">Create a service bus namespace and add a queue tooit.</span></span>
* <span data-ttu-id="9a8d0-116">Přidáte koncový bod a směrování pravidlo tooyour IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-116">Add an endpoint and a routing rule tooyour IoT hub.</span></span>
* <span data-ttu-id="9a8d0-117">Vytvoření, konfiguraci a testování aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-117">Create, configure, and test a logic app.</span></span>

## <a name="what-you-need"></a><span data-ttu-id="9a8d0-118">Co potřebujete</span><span class="sxs-lookup"><span data-stu-id="9a8d0-118">What you need</span></span>

* <span data-ttu-id="9a8d0-119">Kurz [nastavit vaše zařízení](iot-hub-raspberry-pi-kit-node-get-started.md) dokončit, která zahrnuje hello následující požadavky:</span><span class="sxs-lookup"><span data-stu-id="9a8d0-119">Tutorial [Setup your device](iot-hub-raspberry-pi-kit-node-get-started.md) completed which covers hello following requirements:</span></span>
  * <span data-ttu-id="9a8d0-120">Aktivní předplatné Azure.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-120">An active Azure subscription.</span></span>
  * <span data-ttu-id="9a8d0-121">V rámci svého předplatného služby Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-121">An Azure IoT hub under your subscription.</span></span>
  * <span data-ttu-id="9a8d0-122">Klientská aplikace, která odesílá zprávy tooyour Azure IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-122">A client application that sends messages tooyour Azure IoT hub.</span></span>

## <a name="create-service-bus-namespace-and-add-a-queue-tooit"></a><span data-ttu-id="9a8d0-123">Vytvoření oboru názvů service bus a přidejte tooit fronty</span><span class="sxs-lookup"><span data-stu-id="9a8d0-123">Create service bus namespace and add a queue tooit</span></span>

### <a name="create-a-service-bus-namespace"></a><span data-ttu-id="9a8d0-124">Vytvořit obor názvů sběrnice služby</span><span class="sxs-lookup"><span data-stu-id="9a8d0-124">Create a service bus namespace</span></span>

1. <span data-ttu-id="9a8d0-125">Na hello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **Enterprise integrace** > **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-125">On hello [Azure portal](https://portal.azure.com/), click **New** > **Enterprise Integration** > **Service Bus**.</span></span>
1. <span data-ttu-id="9a8d0-126">Zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="9a8d0-126">Provide hello following information:</span></span>

   <span data-ttu-id="9a8d0-127">**Název**: název hello hello service Bus.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-127">**Name**: hello name of hello service bus.</span></span>

   <span data-ttu-id="9a8d0-128">**Cenová úroveň**: klikněte na tlačítko **základní** > **vyberte**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-128">**Pricing tier**: Click **Basic** > **Select**.</span></span> <span data-ttu-id="9a8d0-129">úroveň Basic Hello je dostačující pro účely tohoto kurzu.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-129">hello Basic tier is sufficient for this tutorial.</span></span>

   <span data-ttu-id="9a8d0-130">**Skupina prostředků**: použití hello stejné skupiny prostředků, která používá službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-130">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="9a8d0-131">**Umístění**: použití hello stejné umístění, které používá služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-131">**Location**: Use hello same location that your IoT hub uses.</span></span>
1. <span data-ttu-id="9a8d0-132">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-132">Click **Create**.</span></span>

   ![Vytvořit obor názvů sběrnice služby v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/1_create-service-bus-namespace-azure-portal.png)

### <a name="add-a-service-bus-queue"></a><span data-ttu-id="9a8d0-134">Přidat frontou service bus</span><span class="sxs-lookup"><span data-stu-id="9a8d0-134">Add a service bus queue</span></span>

1. <span data-ttu-id="9a8d0-135">Otevřete obor názvů sběrnice hello a pak klikněte na tlačítko **+ fronty**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-135">Open hello service bus namespace, and then click **+ Queue**.</span></span>
1. <span data-ttu-id="9a8d0-136">Zadejte název pro frontu hello a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-136">Enter a name for hello queue and then click **Create**.</span></span>
1. <span data-ttu-id="9a8d0-137">Otevřete hello frontou service bus a pak klikněte na tlačítko **zásady sdíleného přístupu** > **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-137">Open hello service bus queue, and then click **Shared access policies** > **+ Add**.</span></span>
1. <span data-ttu-id="9a8d0-138">Zadejte název zásady hello, zkontrolujte **spravovat**a potom klikněte na **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-138">Enter a name for hello policy, check **Manage**, and then click **Create**.</span></span>

   ![Přidání frontou service bus v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/2_add-service-bus-queue-azure-portal.png)

## <a name="add-an-endpoint-and-a-routing-rule-tooyour-iot-hub"></a><span data-ttu-id="9a8d0-140">Přidat koncový bod a směrování pravidlo tooyour IoT hub</span><span class="sxs-lookup"><span data-stu-id="9a8d0-140">Add an endpoint and a routing rule tooyour IoT hub</span></span>

### <a name="add-an-endpoint"></a><span data-ttu-id="9a8d0-141">Přidání koncového bodu</span><span class="sxs-lookup"><span data-stu-id="9a8d0-141">Add an endpoint</span></span>

1. <span data-ttu-id="9a8d0-142">Otevřete službu IoT hub, klikněte na koncové body > + Přidat.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-142">Open your IoT hub, click Endpoints > + Add.</span></span>
1. <span data-ttu-id="9a8d0-143">Zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="9a8d0-143">Enter hello following information:</span></span>

   <span data-ttu-id="9a8d0-144">**Název**: hello název koncového bodu hello.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-144">**Name**: hello name of hello endpoint.</span></span>

   <span data-ttu-id="9a8d0-145">**Typ koncového bodu**: vyberte **frontou Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-145">**Endpoint type**: Select **Service Bus Queue**.</span></span>

   <span data-ttu-id="9a8d0-146">**Obor názvů sběrnice**: Vyberte hello obor názvů, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-146">**Service Bus namespace**: Select hello namespace you created.</span></span>

   <span data-ttu-id="9a8d0-147">**Fronty Service Bus**: Vyberte hello fronty, které jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-147">**Service Bus queue**: Select hello queue you created.</span></span>
1. <span data-ttu-id="9a8d0-148">Klikněte na **OK**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-148">Click **OK**.</span></span>

   ![Přidání koncového bodu tooyour IoT hub v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/3_add-iot-hub-endpoint-azure-portal.png)

### <a name="add-a-routing-rule"></a><span data-ttu-id="9a8d0-150">Přidat pravidlo směrování</span><span class="sxs-lookup"><span data-stu-id="9a8d0-150">Add a routing rule</span></span>

1. <span data-ttu-id="9a8d0-151">Ve službě IoT hub, klikněte na tlačítko **trasy** > **+ přidat**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-151">In your IoT hub, click **Routes** > **+ Add**.</span></span>
1. <span data-ttu-id="9a8d0-152">Zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="9a8d0-152">Enter hello following information:</span></span>

   <span data-ttu-id="9a8d0-153">**Název**: hello název pravidla směrování hello.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-153">**Name**: hello name of hello routing rule.</span></span>

   <span data-ttu-id="9a8d0-154">**Zdroj dat**: vyberte **DeviceMessages**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-154">**Data source**: Select **DeviceMessages**.</span></span>

   <span data-ttu-id="9a8d0-155">**Koncový bod**: Vyberte koncový bod hello jste vytvořili.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-155">**Endpoint**: Select hello endpoint you created.</span></span>

   <span data-ttu-id="9a8d0-156">**Řetězec dotazu**: Zadejte `temperatureAlert = "true"`.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-156">**Query string**: Enter `temperatureAlert = "true"`.</span></span>
1. <span data-ttu-id="9a8d0-157">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-157">Click **Save**.</span></span>

   ![Přidat pravidlo směrování v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/4_add-routing-rule-azure-portal.png)

## <a name="create-and-configure-a-logic-app"></a><span data-ttu-id="9a8d0-159">Vytvoření a konfigurace aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="9a8d0-159">Create and configure a logic app</span></span>

### <a name="create-a-logic-app"></a><span data-ttu-id="9a8d0-160">Vytvoření aplikace logiky</span><span class="sxs-lookup"><span data-stu-id="9a8d0-160">Create a logic app</span></span>

1. <span data-ttu-id="9a8d0-161">V hello [portál Azure](https://portal.azure.com/), klikněte na tlačítko **nový** > **Enterprise integrace** > **aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-161">In hello [Azure portal](https://portal.azure.com/), click **New** > **Enterprise Integration** > **Logic App**.</span></span>
1. <span data-ttu-id="9a8d0-162">Zadejte hello následující informace:</span><span class="sxs-lookup"><span data-stu-id="9a8d0-162">Enter hello following information:</span></span>

   <span data-ttu-id="9a8d0-163">**Název**: název hello hello logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-163">**Name**: hello name of hello logic app.</span></span>

   <span data-ttu-id="9a8d0-164">**Skupina prostředků**: použití hello stejné skupiny prostředků, která používá službu IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-164">**Resource group**: Use hello same resource group that your IoT hub uses.</span></span>

   <span data-ttu-id="9a8d0-165">**Umístění**: použití hello stejné umístění, které používá služby IoT hub.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-165">**Location**: Use hello same location that your IoT hub uses.</span></span>
1. <span data-ttu-id="9a8d0-166">Klikněte na možnost **Vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-166">Click **Create**.</span></span>

### <a name="configure-hello-logic-app"></a><span data-ttu-id="9a8d0-167">Konfigurace aplikace logiky hello</span><span class="sxs-lookup"><span data-stu-id="9a8d0-167">Configure hello logic app</span></span>

1. <span data-ttu-id="9a8d0-168">Otevřete aplikaci logiky hello, které se otevře do hello návrhář aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-168">Open hello logic app that opens into hello Logic Apps Designer.</span></span>
1. <span data-ttu-id="9a8d0-169">V hello návrhář aplikace logiky, klikněte na **prázdné aplikace logiky**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-169">In hello Logic Apps Designer, click **Blank Logic App**.</span></span>

   ![Začít s prázdnou logiku aplikace v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/5_start-with-blank-logic-app-azure-portal.png)

1. <span data-ttu-id="9a8d0-171">Klikněte na tlačítko **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-171">Click **Service Bus**.</span></span>

   ![Vyberte služby Service Bus toostart vytvoření aplikace logiky v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/6_select-service-bus-when-creating-blank-logic-app-azure-portal.png)

1. <span data-ttu-id="9a8d0-173">Klikněte na tlačítko **Service Bus – při obdržení jeden nebo více zpráv ve frontě (automatické dokončování)**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-173">Click **Service Bus – When one or more messages arrive in a queue (auto-complete)**.</span></span>
1. <span data-ttu-id="9a8d0-174">Umožňuje vytvořte připojení service bus.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-174">Create a service bus connection.</span></span>
   1. <span data-ttu-id="9a8d0-175">Zadejte název připojení.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-175">Enter a connection name.</span></span>
   1. <span data-ttu-id="9a8d0-176">Klikněte na obor názvů sběrnice hello > hello service bus zásady > **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-176">Click hello service bus namespace > hello service bus policy > **Create**.</span></span>

      ![Vytvořte připojení sběrnice služby pro svou aplikaci logiky v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/7_create-service-bus-connection-in-logic-app-azure-portal.png)

   1. <span data-ttu-id="9a8d0-178">Klikněte na tlačítko **pokračovat** po vytvoření připojení sběrnice služby hello.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-178">Click **Continue** after hello service bus connection is created.</span></span>
   1. <span data-ttu-id="9a8d0-179">Vyberte hello fronty, kterou jste vytvořili a zadejte `175` pro **maximální počet zpráv**</span><span class="sxs-lookup"><span data-stu-id="9a8d0-179">Select hello queue that you created and enter `175` for **Maximum message count**</span></span>

      ![Zadejte počet hello maximální zpráv pro připojení sběrnice služby hello v aplikaci logiky](media/iot-hub-monitoring-notifications-with-azure-logic-apps/8_specify-maximum-message-count-for-service-bus-connection-logic-app-azure-portal.png)
   1. <span data-ttu-id="9a8d0-181">Klikněte na "tlačítko toosave hello změny uložit".</span><span class="sxs-lookup"><span data-stu-id="9a8d0-181">Click "Save" button toosave hello changes.</span></span>

1. <span data-ttu-id="9a8d0-182">Vytvoření připojení služby SMTP.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-182">Create an SMTP service connection.</span></span>
   1. <span data-ttu-id="9a8d0-183">Klikněte na tlačítko **nový krok** > **přidat akci**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-183">Click **New step** > **Add an action**.</span></span>
   1. <span data-ttu-id="9a8d0-184">Typ `SMTP`, klikněte na tlačítko hello **SMTP** služby ve výsledku hledání hello a pak klikněte na **SMTP - odeslat E-mail**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-184">Type `SMTP`, click hello **SMTP** service in hello search result, and then click **SMTP - Send Email**.</span></span>

      ![Vytvoření připojení k SMTP ve vaší aplikaci logiky v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/9_create-smtp-connection-logic-app-azure-portal.png)

   1. <span data-ttu-id="9a8d0-186">Zadejte informace hello SMTP poštovní schránky a pak klikněte na tlačítko **vytvořit**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-186">Enter hello SMTP information of your mailbox, and then click **Create**.</span></span>

      ![Zadejte informace o připojení protokolu SMTP ve vaší aplikaci logiky v hello portálu Azure](media/iot-hub-monitoring-notifications-with-azure-logic-apps/10_enter-smtp-connection-info-logic-app-azure-portal.png)

      <span data-ttu-id="9a8d0-188">Získání informací o hello SMTP pro [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [z Gmailu](https://support.google.com/a/answer/176600?hl=en), a [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).</span><span class="sxs-lookup"><span data-stu-id="9a8d0-188">Get hello SMTP information for [Hotmail/Outlook.com](https://support.office.com/en-us/article/Add-your-Outlook-com-account-to-another-mail-app-73f3b178-0009-41ae-aab1-87b80fa94970), [Gmail](https://support.google.com/a/answer/176600?hl=en), and [Yahoo Mail](https://help.yahoo.com/kb/SLN4075.html).</span></span>
   1. <span data-ttu-id="9a8d0-189">Zadejte e-mailovou adresu pro **z** a **k**, a `High temperature detected` pro **subjektu** a **textu**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-189">Enter your email address for **From** and **To**, and `High temperature detected` for **Subject** and **Body**.</span></span>
   1. <span data-ttu-id="9a8d0-190">Klikněte na **Uložit**.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-190">Click **Save**.</span></span>

<span data-ttu-id="9a8d0-191">aplikace logiky Hello je ve funkčním stavu, když jste jej uložili.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-191">hello logic app is in working order when you save it.</span></span>

## <a name="test-hello-logic-app"></a><span data-ttu-id="9a8d0-192">Testování aplikace logiky hello</span><span class="sxs-lookup"><span data-stu-id="9a8d0-192">Test hello logic app</span></span>

1. <span data-ttu-id="9a8d0-193">Spustit aplikaci hello klienta, který nasazujete tooyour zařízení v [připojit ESP8266 tooAzure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9a8d0-193">Start hello client application that you deploy tooyour device in [Connect ESP8266 tooAzure IoT Hub](iot-hub-arduino-huzzah-esp8266-get-started.md).</span></span>
1. <span data-ttu-id="9a8d0-194">Zvýšení teploty prostředí hello kolem hello SensorTag toobe nad 30 C. Například light svíčkového grafu okolo vaší SensorTag.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-194">Increase hello environment temperature around hello SensorTag toobe above 30 C. For example, light a candle around your SensorTag.</span></span>
1. <span data-ttu-id="9a8d0-195">Měli byste obdržet e-mail s oznámením poslal aplikace logiky hello.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-195">You should receive an email notification sent by hello logic app.</span></span>

   > [!NOTE]
   > <span data-ttu-id="9a8d0-196">Poskytovatel služeb e-mailu může být nutné tooverify hello odesílatele identity toomake se, že se že jedná o vás, který odešle e-mail hello.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-196">Your email service provider may need tooverify hello sender identity toomake sure it is you who sends hello email.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9a8d0-197">Další kroky</span><span class="sxs-lookup"><span data-stu-id="9a8d0-197">Next steps</span></span>

<span data-ttu-id="9a8d0-198">Úspěšně jste vytvořili aplikaci logiky, která se připojuje služby IoT hub a poštovní schránky pro monitorování teploty a oznámení.</span><span class="sxs-lookup"><span data-stu-id="9a8d0-198">You have successfully created a logic app that connects your IoT hub and your mailbox for temperature monitoring and notifications.</span></span>

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
