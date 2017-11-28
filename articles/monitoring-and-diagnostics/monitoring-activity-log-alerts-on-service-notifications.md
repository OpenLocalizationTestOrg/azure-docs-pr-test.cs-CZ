---
title: "aaaReceive aktivity protokolu výstrahy na oznámení o službách | Microsoft Docs"
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
ms.openlocfilehash: dd35e8f39d2a522efdae4dfed20779c992c1dd27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-activity-log-alerts-on-service-notifications"></a><span data-ttu-id="7cec6-103">Vytvoření aktivity protokolu upozornění na oznámení o službách</span><span class="sxs-lookup"><span data-stu-id="7cec6-103">Create activity log alerts on service notifications</span></span>
## <a name="overview"></a><span data-ttu-id="7cec6-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="7cec6-104">Overview</span></span>
<span data-ttu-id="7cec6-105">Tento článek ukazuje, jak výstrahy tooset si protokol aktivit pro oznámení o stavu služby pomocí hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="7cec6-105">This article shows you how tooset up activity log alerts for service health notifications by using hello Azure portal.</span></span>  

<span data-ttu-id="7cec6-106">Můžete zobrazit upozornění, když Azure odesílá služba stavu oznámení tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="7cec6-106">You can receive an alert when Azure sends service health notifications tooyour Azure subscription.</span></span> <span data-ttu-id="7cec6-107">Můžete nakonfigurovat hello výstrahu založenou na:</span><span class="sxs-lookup"><span data-stu-id="7cec6-107">You can configure hello alert based on:</span></span>

- <span data-ttu-id="7cec6-108">Třída Hello oznámení o stavu služby (incident, údržby, informace, atd.).</span><span class="sxs-lookup"><span data-stu-id="7cec6-108">hello class of service health notification (incident, maintenance, information, etc.).</span></span>
- <span data-ttu-id="7cec6-109">Hello služeb vliv.</span><span class="sxs-lookup"><span data-stu-id="7cec6-109">hello service(s) affected.</span></span>
- <span data-ttu-id="7cec6-110">Hello oblast(i) vliv.</span><span class="sxs-lookup"><span data-stu-id="7cec6-110">hello region(s) affected.</span></span>
- <span data-ttu-id="7cec6-111">Stav Hello hello oznámení (aktivní oproti přeložit).</span><span class="sxs-lookup"><span data-stu-id="7cec6-111">hello status of hello notification (active vs. resolved).</span></span>
- <span data-ttu-id="7cec6-112">úroveň Hello hello oznámení (informační, upozornění, chyby).</span><span class="sxs-lookup"><span data-stu-id="7cec6-112">hello level of hello notifications (informational, warning, error).</span></span>

<span data-ttu-id="7cec6-113">Můžete také konfigurovat kdo hello výstraha by měly být odeslány na:</span><span class="sxs-lookup"><span data-stu-id="7cec6-113">You also can configure who hello alert should be sent to:</span></span>

- <span data-ttu-id="7cec6-114">Vyberte existující skupinu akce.</span><span class="sxs-lookup"><span data-stu-id="7cec6-114">Select an existing action group.</span></span>
- <span data-ttu-id="7cec6-115">Vytvořte novou skupinu akce (který můžete použít pro budoucí výstrahy).</span><span class="sxs-lookup"><span data-stu-id="7cec6-115">Create a new action group (that can be used for future alerts).</span></span>

<span data-ttu-id="7cec6-116">toolearn Další informace o akci skupiny, najdete v části [vytvořit a spravovat skupiny akce](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="7cec6-116">toolearn more about action groups, see [Create and manage action groups](monitoring-action-groups.md).</span></span>

<span data-ttu-id="7cec6-117">Informace o tom, jak tooconfigure služby stavu oznámení výstrahy pomocí šablon Azure Resource Manageru, najdete v části [šablony Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="7cec6-117">For information on how tooconfigure service health notification alerts by using Azure Resource Manager templates, see [Resource Manager templates](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span></span>

## <a name="create-an-alert-on-a-service-health-notification-for-a-new-action-group-by-using-hello-azure-portal"></a><span data-ttu-id="7cec6-118">Vytvořit výstrahu na oznámení stavu služby pro novou skupinu akce pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7cec6-118">Create an alert on a service health notification for a new action group by using hello Azure portal</span></span>
1. <span data-ttu-id="7cec6-119">V hello [portál](https://portal.azure.com), vyberte **monitorování**.</span><span class="sxs-lookup"><span data-stu-id="7cec6-119">In hello [portal](https://portal.azure.com), select **Monitor**.</span></span>

    ![Hello "Sledování" service](./media/monitoring-activity-log-alerts-on-service-notifications/home-monitor.png)

2. <span data-ttu-id="7cec6-121">V hello **protokol aktivit** vyberte **výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="7cec6-121">In hello **Activity log** section, select **Alerts**.</span></span>

    ![Karta "Oznámení" Hello](./media/monitoring-activity-log-alerts-on-service-notifications/alerts-blades.png)

3. <span data-ttu-id="7cec6-123">Vyberte **přidat aktivitu protokolu upozornění**a vyplňte pole hello.</span><span class="sxs-lookup"><span data-stu-id="7cec6-123">Select **Add activity log alert**, and fill in hello fields.</span></span>

    ![Hello příkaz "Přidat aktivitu protokolu upozornění"](./media/monitoring-activity-log-alerts-on-service-notifications/add-activity-log-alert.png)

4. <span data-ttu-id="7cec6-125">Zadejte název v hello **výstrahy název aktivity protokolu** pole a zadejte **popis**.</span><span class="sxs-lookup"><span data-stu-id="7cec6-125">Enter a name in hello **Activity log alert name** box, and provide a **Description**.</span></span>

    ![Hello "Přidat aktivitu protokolu výstraha" dialogové okno](./media/monitoring-activity-log-alerts-on-service-notifications/activity-log-alert-service-notification-new-action-group.png)

5. <span data-ttu-id="7cec6-127">Hello **předplatné** pole autofills s vaším aktuálním předplatným.</span><span class="sxs-lookup"><span data-stu-id="7cec6-127">hello **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="7cec6-128">Toto předplatné je výstraha použité toosave hello aktivity protokolu.</span><span class="sxs-lookup"><span data-stu-id="7cec6-128">This subscription is used toosave hello activity log alert.</span></span> <span data-ttu-id="7cec6-129">výstrahy prostředek Hello je nasazené toothis předplatného a monitorování události v protokolu hello aktivity pro ni.</span><span class="sxs-lookup"><span data-stu-id="7cec6-129">hello alert resource is deployed toothis subscription and monitors events in hello activity log for it.</span></span>

6. <span data-ttu-id="7cec6-130">Vyberte hello **skupiny prostředků** v které hello výstrahy prostředků vytvořili.</span><span class="sxs-lookup"><span data-stu-id="7cec6-130">Select hello **Resource group** in which hello alert resource is created.</span></span> <span data-ttu-id="7cec6-131">Tato akce není hello skupinu prostředků, který je monitorován hello výstraha.</span><span class="sxs-lookup"><span data-stu-id="7cec6-131">This isn't hello resource group that's monitored by hello alert.</span></span> <span data-ttu-id="7cec6-132">Místo toho je skupina prostředků hello, kde je umístěný prostředek výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="7cec6-132">Instead, it's hello resource group where hello alert resource is located.</span></span>

7. <span data-ttu-id="7cec6-133">V hello **kategorie události** vyberte **stav služby**.</span><span class="sxs-lookup"><span data-stu-id="7cec6-133">In hello **Event category** box, select **Service Health**.</span></span> <span data-ttu-id="7cec6-134">Volitelně vyberte hello **služby**, **oblast**, **typ**, **stav**, a **úroveň** služby oznámení o stavu, které chcete tooreceive.</span><span class="sxs-lookup"><span data-stu-id="7cec6-134">Optionally, select hello **Service**, **Region**, **Type**, **Status**, and **Level** of service health notifications that you want tooreceive.</span></span>

8. <span data-ttu-id="7cec6-135">V části **výstrahy prostřednictvím**, vyberte hello **nový** tlačítko akce skupiny.</span><span class="sxs-lookup"><span data-stu-id="7cec6-135">Under **Alert via**, select hello **New** action group button.</span></span> <span data-ttu-id="7cec6-136">Zadejte název v hello **název skupiny akce** pole a zadejte název v hello **krátký název** pole.</span><span class="sxs-lookup"><span data-stu-id="7cec6-136">Enter a name in hello **Action group name** box, and enter a name in hello **Short name** box.</span></span> <span data-ttu-id="7cec6-137">krátký název Hello se odkazuje v hello oznámení, která se posílají, když se aktivuje se tato výstraha.</span><span class="sxs-lookup"><span data-stu-id="7cec6-137">hello short name is referenced in hello notifications that are sent when this alert fires.</span></span>

9. <span data-ttu-id="7cec6-138">Definujte seznam příjemců tím, že poskytuje příjemce hello:</span><span class="sxs-lookup"><span data-stu-id="7cec6-138">Define a list of receivers by providing hello receiver's:</span></span>

    <span data-ttu-id="7cec6-139">a.</span><span class="sxs-lookup"><span data-stu-id="7cec6-139">a.</span></span> <span data-ttu-id="7cec6-140">**Název**: Zadejte název, alias nebo identifikátor hello příjemce.</span><span class="sxs-lookup"><span data-stu-id="7cec6-140">**Name**: Enter hello receiver’s name, alias, or identifier.</span></span>

    <span data-ttu-id="7cec6-141">b.</span><span class="sxs-lookup"><span data-stu-id="7cec6-141">b.</span></span> <span data-ttu-id="7cec6-142">**Typ akce**: Vyberte SMS, e-mailu nebo webhooku.</span><span class="sxs-lookup"><span data-stu-id="7cec6-142">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="7cec6-143">c.</span><span class="sxs-lookup"><span data-stu-id="7cec6-143">c.</span></span> <span data-ttu-id="7cec6-144">**Podrobnosti o**: založený na typu akce hello vybrali, zadejte telefonní číslo, e-mailovou adresu nebo webhooku identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="7cec6-144">**Details**: Based on hello action type chosen, enter a phone number, email address, or webhook URI.</span></span>

10. <span data-ttu-id="7cec6-145">Vyberte **OK** toocreate hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="7cec6-145">Select **OK** toocreate hello alert.</span></span>

<span data-ttu-id="7cec6-146">Během několika minut hello výstraha je aktivní a začne tootrigger na základě hello podmínek, které zadáte během vytváření.</span><span class="sxs-lookup"><span data-stu-id="7cec6-146">Within a few minutes, hello alert is active and begins tootrigger based on hello conditions you specified during creation.</span></span>

<span data-ttu-id="7cec6-147">Informace o schématu hello webhooku pro aktivitu protokolu výstrahy naleznete v tématu [Webhooky Azure aktivity protokolu výstrahy](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="7cec6-147">For information on hello webhook schema for activity log alerts, see [Webhooks for Azure activity log alerts](monitoring-activity-log-alerts-webhook.md).</span></span>

>[!NOTE]
><span data-ttu-id="7cec6-148">Skupina akce Hello definované v těchto kroků je opakovaně použitelné jako stávající skupina akce pro všechny budoucí výstrahy definice.</span><span class="sxs-lookup"><span data-stu-id="7cec6-148">hello action group defined in these steps is reusable as an existing action group for all future alert definitions.</span></span>
>
>

## <a name="create-an-alert-on-a-service-health-notification-for-an-existing-action-group-by-using-hello-azure-portal"></a><span data-ttu-id="7cec6-149">Vytvořit výstrahu na oznámení stavu služby pro stávající skupinu akce pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="7cec6-149">Create an alert on a service health notification for an existing action group by using hello Azure portal</span></span>

1. <span data-ttu-id="7cec6-150">Postupujte podle kroků 1 až 7 v předchozí části toocreate hello oznámení služby stavu.</span><span class="sxs-lookup"><span data-stu-id="7cec6-150">Follow steps 1 through 7 in hello previous section toocreate your service health notification.</span></span> 

2. <span data-ttu-id="7cec6-151">V části **výstrahy prostřednictvím**, vyberte hello **existující** tlačítko akce skupiny.</span><span class="sxs-lookup"><span data-stu-id="7cec6-151">Under **Alert via**, select hello **Existing** action group button.</span></span> <span data-ttu-id="7cec6-152">Vyberte skupinu hello příslušnou akci.</span><span class="sxs-lookup"><span data-stu-id="7cec6-152">Select hello appropriate action group.</span></span>

3. <span data-ttu-id="7cec6-153">Vyberte **OK** toocreate hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="7cec6-153">Select **OK** toocreate hello alert.</span></span>

<span data-ttu-id="7cec6-154">Během několika minut hello výstraha je aktivní a začne tootrigger na základě hello podmínek, které zadáte během vytváření.</span><span class="sxs-lookup"><span data-stu-id="7cec6-154">Within a few minutes, hello alert is active and begins tootrigger based on hello conditions you specified during creation.</span></span>

## <a name="manage-your-alerts"></a><span data-ttu-id="7cec6-155">Spravovat oznámení</span><span class="sxs-lookup"><span data-stu-id="7cec6-155">Manage your alerts</span></span>

<span data-ttu-id="7cec6-156">Po vytvoření výstrahy, se zobrazí na hello **výstrahy** části hello **monitorování** okno.</span><span class="sxs-lookup"><span data-stu-id="7cec6-156">After you create an alert, it's visible in hello **Alerts** section of hello **Monitor** blade.</span></span> <span data-ttu-id="7cec6-157">Vyberte hello oznámení, které chcete toomanage na:</span><span class="sxs-lookup"><span data-stu-id="7cec6-157">Select hello alert you want toomanage to:</span></span>

* <span data-ttu-id="7cec6-158">Upravte.</span><span class="sxs-lookup"><span data-stu-id="7cec6-158">Edit it.</span></span>
* <span data-ttu-id="7cec6-159">Odstraňte ji.</span><span class="sxs-lookup"><span data-stu-id="7cec6-159">Delete it.</span></span>
* <span data-ttu-id="7cec6-160">Zakázat nebo povolit, pokud chcete zastavit tootemporarily nebo obnovit příjem oznámení pro výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="7cec6-160">Disable or enable it, if you want tootemporarily stop or resume receiving notifications for hello alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7cec6-161">Další kroky</span><span class="sxs-lookup"><span data-stu-id="7cec6-161">Next steps</span></span>
- <span data-ttu-id="7cec6-162">Další informace o [oznámení o stavu služby](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="7cec6-162">Learn about [service health notifications](monitoring-service-notifications.md).</span></span>
- <span data-ttu-id="7cec6-163">Další informace o [omezení rychlosti oznámení](monitoring-alerts-rate-limiting.md).</span><span class="sxs-lookup"><span data-stu-id="7cec6-163">Learn about [notification rate limiting](monitoring-alerts-rate-limiting.md).</span></span>
- <span data-ttu-id="7cec6-164">Zkontrolujte hello [schéma výstrahy webhooku protokolu činnosti](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="7cec6-164">Review hello [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>
- <span data-ttu-id="7cec6-165">Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md)a zjistěte, jak tooreceive výstrahy.</span><span class="sxs-lookup"><span data-stu-id="7cec6-165">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how tooreceive alerts.</span></span> 
- <span data-ttu-id="7cec6-166">Další informace o [skupiny akcí](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="7cec6-166">Learn more about [action groups](monitoring-action-groups.md).</span></span>
