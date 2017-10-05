---
title: "Výstrahy v protokolu aktivit na oznámení o službách | Microsoft Docs"
description: "SMS, e-mailem nebo webhooku dostat upozornění, když dojde k služby Azure."
author: johnkemnetz
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: 
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: johnkem
ms.openlocfilehash: bf6a98fd7e7e11764bef174f9efd0635fa7efe9a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-activity-log-alerts-on-service-notifications"></a><span data-ttu-id="fa213-103">Vytvoření aktivity protokolu upozornění na oznámení o službách</span><span class="sxs-lookup"><span data-stu-id="fa213-103">Create activity log alerts on service notifications</span></span>
## <a name="overview"></a><span data-ttu-id="fa213-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="fa213-104">Overview</span></span>
<span data-ttu-id="fa213-105">Tento článek ukazuje, jak nastavit výstrahy protokolu aktivit pro oznámení o stavu služby pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fa213-105">This article shows you how to set up activity log alerts for service health notifications by using the Azure portal.</span></span>  

<span data-ttu-id="fa213-106">Můžete zobrazit upozornění, když Azure odesílá oznámení o stavu služby k předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="fa213-106">You can receive an alert when Azure sends service health notifications to your Azure subscription.</span></span> <span data-ttu-id="fa213-107">Můžete nakonfigurovat výstrahy na základě:</span><span class="sxs-lookup"><span data-stu-id="fa213-107">You can configure the alert based on:</span></span>

- <span data-ttu-id="fa213-108">Třída oznámení o stavu služby (incident, údržby, informace, atd.).</span><span class="sxs-lookup"><span data-stu-id="fa213-108">The class of service health notification (incident, maintenance, information, etc.).</span></span>
- <span data-ttu-id="fa213-109">Služeb vliv.</span><span class="sxs-lookup"><span data-stu-id="fa213-109">The service(s) affected.</span></span>
- <span data-ttu-id="fa213-110">Oblast(i) vliv.</span><span class="sxs-lookup"><span data-stu-id="fa213-110">The region(s) affected.</span></span>
- <span data-ttu-id="fa213-111">Stav oznámení (aktivní oproti přeložit).</span><span class="sxs-lookup"><span data-stu-id="fa213-111">The status of the notification (active vs. resolved).</span></span>
- <span data-ttu-id="fa213-112">Úroveň oznámení (informační, upozornění, chyby).</span><span class="sxs-lookup"><span data-stu-id="fa213-112">The level of the notifications (informational, warning, error).</span></span>

<span data-ttu-id="fa213-113">Můžete také konfigurovat kdo výstraha by měly být odeslány na:</span><span class="sxs-lookup"><span data-stu-id="fa213-113">You also can configure who the alert should be sent to:</span></span>

- <span data-ttu-id="fa213-114">Vyberte existující skupinu akce.</span><span class="sxs-lookup"><span data-stu-id="fa213-114">Select an existing action group.</span></span>
- <span data-ttu-id="fa213-115">Vytvořte novou skupinu akce (který můžete použít pro budoucí výstrahy).</span><span class="sxs-lookup"><span data-stu-id="fa213-115">Create a new action group (that can be used for future alerts).</span></span>

<span data-ttu-id="fa213-116">Další informace o skupinách akce najdete v tématu [vytvořit a spravovat skupiny akce](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="fa213-116">To learn more about action groups, see [Create and manage action groups](monitoring-action-groups.md).</span></span>

<span data-ttu-id="fa213-117">Informace o tom, jak nakonfigurovat služby stavu oznámení výstrah pomocí šablon Azure Resource Manageru najdete v tématu [šablony Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="fa213-117">For information on how to configure service health notification alerts by using Azure Resource Manager templates, see [Resource Manager templates](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span></span>

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-the-azure-portal"></a><span data-ttu-id="fa213-118">Vytvořit výstrahu na oznámení stavu služby pro novou skupinu akce pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fa213-118">Create an alert on a service health notification for a new action group by using the Azure portal</span></span>
1. <span data-ttu-id="fa213-119">V [portál](https://portal.azure.com), vyberte **monitorování**.</span><span class="sxs-lookup"><span data-stu-id="fa213-119">In the [portal](https://portal.azure.com), select **Monitor**.</span></span>

    ![Službu "Sledování"](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2. <span data-ttu-id="fa213-121">V **protokol aktivit** vyberte **výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="fa213-121">In the **Activity log** section, select **Alerts**.</span></span>

    ![Na kartě "Výstrahy"](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

3. <span data-ttu-id="fa213-123">Vyberte **přidat aktivitu protokolu upozornění**a vyplňte příslušná pole.</span><span class="sxs-lookup"><span data-stu-id="fa213-123">Select **Add activity log alert**, and fill in the fields.</span></span>

    ![Příkaz "Přidat aktivitu protokolu upozornění"](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

4. <span data-ttu-id="fa213-125">Zadejte název do pole **výstrahy název aktivity protokolu** pole a zadejte **popis**.</span><span class="sxs-lookup"><span data-stu-id="fa213-125">Enter a name in the **Activity log alert name** box, and provide a **Description**.</span></span>

    ![Dialogové okno "Přidat aktivitu protokolu upozornění"](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

5. <span data-ttu-id="fa213-127">**Předplatné** pole autofills s vaším aktuálním předplatným.</span><span class="sxs-lookup"><span data-stu-id="fa213-127">The **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="fa213-128">Toto předplatné se používá pro uložení výstraha aktivity protokolu.</span><span class="sxs-lookup"><span data-stu-id="fa213-128">This subscription is used to save the activity log alert.</span></span> <span data-ttu-id="fa213-129">Výstrahy prostředků je nasazen na toto předplatné a sleduje události v protokolu aktivit pro ni.</span><span class="sxs-lookup"><span data-stu-id="fa213-129">The alert resource is deployed to this subscription and monitors events in the activity log for it.</span></span>

6. <span data-ttu-id="fa213-130">Vyberte **skupiny prostředků** v výstrahy prostředek je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="fa213-130">Select the **Resource group** in which the alert resource is created.</span></span> <span data-ttu-id="fa213-131">Tato akce není skupině prostředků, který je monitorován výstraha.</span><span class="sxs-lookup"><span data-stu-id="fa213-131">This isn't the resource group that's monitored by the alert.</span></span> <span data-ttu-id="fa213-132">Místo toho je skupina prostředků, kde je umístěný prostředek výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fa213-132">Instead, it's the resource group where the alert resource is located.</span></span>

7. <span data-ttu-id="fa213-133">V **kategorie události** vyberte **stav služby**.</span><span class="sxs-lookup"><span data-stu-id="fa213-133">In the **Event category** box, select **Service Health**.</span></span> <span data-ttu-id="fa213-134">Volitelně vyberte **služby**, **oblast**, **typ**, **stav**, a **úroveň** o stavu služby oznámení, které chcete dostávat.</span><span class="sxs-lookup"><span data-stu-id="fa213-134">Optionally, select the **Service**, **Region**, **Type**, **Status**, and **Level** of service health notifications that you want to receive.</span></span>

8. <span data-ttu-id="fa213-135">V části **výstrahy prostřednictvím**, vyberte **nový** tlačítko akce skupiny.</span><span class="sxs-lookup"><span data-stu-id="fa213-135">Under **Alert via**, select the **New** action group button.</span></span> <span data-ttu-id="fa213-136">Zadejte název do pole **název skupiny akce** pole a zadejte název do pole **krátký název** pole.</span><span class="sxs-lookup"><span data-stu-id="fa213-136">Enter a name in the **Action group name** box, and enter a name in the **Short name** box.</span></span> <span data-ttu-id="fa213-137">Krátký název je odkazováno na oznámení, která se posílají, když se aktivuje se tato výstraha.</span><span class="sxs-lookup"><span data-stu-id="fa213-137">The short name is referenced in the notifications that are sent when this alert fires.</span></span>

9. <span data-ttu-id="fa213-138">Definujte seznam příjemců tím, že poskytuje příjemce:</span><span class="sxs-lookup"><span data-stu-id="fa213-138">Define a list of receivers by providing the receiver's:</span></span>

    <span data-ttu-id="fa213-139">a.</span><span class="sxs-lookup"><span data-stu-id="fa213-139">a.</span></span> <span data-ttu-id="fa213-140">**Název**: Zadejte název, alias nebo identifikátor příjemce.</span><span class="sxs-lookup"><span data-stu-id="fa213-140">**Name**: Enter the receiver’s name, alias, or identifier.</span></span>

    <span data-ttu-id="fa213-141">b.</span><span class="sxs-lookup"><span data-stu-id="fa213-141">b.</span></span> <span data-ttu-id="fa213-142">**Typ akce**: Vyberte SMS, e-mailu nebo webhooku.</span><span class="sxs-lookup"><span data-stu-id="fa213-142">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="fa213-143">c.</span><span class="sxs-lookup"><span data-stu-id="fa213-143">c.</span></span> <span data-ttu-id="fa213-144">**Podrobnosti o**: v závislosti na typu akce vybrali, zadejte telefonní číslo, e-mailovou adresu nebo webhooku identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="fa213-144">**Details**: Based on the action type chosen, enter a phone number, email address, or webhook URI.</span></span>

10. <span data-ttu-id="fa213-145">Vyberte **OK** vytvořit výstrahu.</span><span class="sxs-lookup"><span data-stu-id="fa213-145">Select **OK** to create the alert.</span></span>

<span data-ttu-id="fa213-146">Během několika minut výstraha je aktivní a začne k aktivaci na základě podmínek, které jste zadali při vytváření.</span><span class="sxs-lookup"><span data-stu-id="fa213-146">Within a few minutes, the alert is active and begins to trigger based on the conditions you specified during creation.</span></span>

<span data-ttu-id="fa213-147">Informace o schématu webhooku pro aktivitu protokolu výstrahy naleznete v tématu [Webhooky Azure aktivity protokolu výstrahy](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="fa213-147">For information on the webhook schema for activity log alerts, see [Webhooks for Azure activity log alerts](monitoring-activity-log-alerts-webhook.md).</span></span>

>[!NOTE]
><span data-ttu-id="fa213-148">Skupina akce definované v těchto kroků je opakovaně použitelné jako stávající skupina akce pro všechny budoucí výstrahy definice.</span><span class="sxs-lookup"><span data-stu-id="fa213-148">The action group defined in these steps is reusable as an existing action group for all future alert definitions.</span></span>
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-the-azure-portal"></a><span data-ttu-id="fa213-149">Vytvořit výstrahu na oznámení stavu služby pro stávající skupinu akce pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="fa213-149">Create an alert on a service health notification for an existing action group by using the Azure portal</span></span>

1. <span data-ttu-id="fa213-150">Postupujte podle kroků 1 až 7 v předchozí části, chcete-li vytvořit oznámení služby stavu.</span><span class="sxs-lookup"><span data-stu-id="fa213-150">Follow steps 1 through 7 in the previous section to create your service health notification.</span></span> 

2. <span data-ttu-id="fa213-151">V části **výstrahy prostřednictvím**, vyberte **existující** tlačítko akce skupiny.</span><span class="sxs-lookup"><span data-stu-id="fa213-151">Under **Alert via**, select the **Existing** action group button.</span></span> <span data-ttu-id="fa213-152">Vyberte skupinu příslušnou akci.</span><span class="sxs-lookup"><span data-stu-id="fa213-152">Select the appropriate action group.</span></span>

3. <span data-ttu-id="fa213-153">Vyberte **OK** vytvořit výstrahu.</span><span class="sxs-lookup"><span data-stu-id="fa213-153">Select **OK** to create the alert.</span></span>

<span data-ttu-id="fa213-154">Během několika minut výstraha je aktivní a začne k aktivaci na základě podmínek, které jste zadali při vytváření.</span><span class="sxs-lookup"><span data-stu-id="fa213-154">Within a few minutes, the alert is active and begins to trigger based on the conditions you specified during creation.</span></span>

## <a name="manage-your-alerts"></a><span data-ttu-id="fa213-155">Spravovat oznámení</span><span class="sxs-lookup"><span data-stu-id="fa213-155">Manage your alerts</span></span>

<span data-ttu-id="fa213-156">Po vytvoření výstrahy, se zobrazí na **výstrahy** části **monitorování** okno.</span><span class="sxs-lookup"><span data-stu-id="fa213-156">After you create an alert, it's visible in the **Alerts** section of the **Monitor** blade.</span></span> <span data-ttu-id="fa213-157">Vyberte výstrahy, kterou chcete spravovat:</span><span class="sxs-lookup"><span data-stu-id="fa213-157">Select the alert you want to manage to:</span></span>

* <span data-ttu-id="fa213-158">Upravte.</span><span class="sxs-lookup"><span data-stu-id="fa213-158">Edit it.</span></span>
* <span data-ttu-id="fa213-159">Odstraňte ji.</span><span class="sxs-lookup"><span data-stu-id="fa213-159">Delete it.</span></span>
* <span data-ttu-id="fa213-160">Zakázat nebo povolit, pokud chcete dočasně zastavit nebo obnovit příjem oznámení pro výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fa213-160">Disable or enable it, if you want to temporarily stop or resume receiving notifications for the alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fa213-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fa213-161">Next steps</span></span>
- <span data-ttu-id="fa213-162">Další informace o [oznámení o stavu služby](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="fa213-162">Learn about [service health notifications](monitoring-service-notifications.md).</span></span>
- <span data-ttu-id="fa213-163">Další informace o [omezení rychlosti oznámení](monitoring-alerts-rate-limiting.md).</span><span class="sxs-lookup"><span data-stu-id="fa213-163">Learn about [notification rate limiting](monitoring-alerts-rate-limiting.md).</span></span>
- <span data-ttu-id="fa213-164">Zkontrolujte [schéma výstrahy webhooku protokolu činnosti](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="fa213-164">Review the [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>
- <span data-ttu-id="fa213-165">Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md)a zjistěte, jak dostávat výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fa213-165">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how to receive alerts.</span></span> 
- <span data-ttu-id="fa213-166">Další informace o [skupiny akcí](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="fa213-166">Learn more about [action groups](monitoring-action-groups.md).</span></span>
