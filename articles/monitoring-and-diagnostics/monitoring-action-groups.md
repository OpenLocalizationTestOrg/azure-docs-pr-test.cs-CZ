---
title: "Vytvoření a Správa skupin akce na portálu Azure | Microsoft Docs"
description: "Naučte se vytvářet a spravovat skupiny akce na portálu Azure."
author: anirudhcavale
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
ms.date: 05/15/2017
ms.author: ancav
ms.openlocfilehash: ea15705bf02d9773507c6cb59f2da4c1dd0f9d77
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-action-groups-in-the-azure-portal"></a><span data-ttu-id="1b965-103">Vytvoření a Správa skupin akce na portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1b965-103">Create and manage action groups in the Azure portal</span></span>
## <a name="overview"></a><span data-ttu-id="1b965-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="1b965-104">Overview</span></span> ##
<span data-ttu-id="1b965-105">Tento článek ukazuje, jak vytvořit a spravovat skupiny akce na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="1b965-105">This article shows you how to create and manage action groups in the Azure portal.</span></span>

<span data-ttu-id="1b965-106">Seznam akcí, můžete nakonfigurovat skupiny akcí.</span><span class="sxs-lookup"><span data-stu-id="1b965-106">You can configure a list of actions with action groups.</span></span> <span data-ttu-id="1b965-107">Tyto skupiny pak lze použít při definování aktivity protokolu výstrahy.</span><span class="sxs-lookup"><span data-stu-id="1b965-107">These groups then can be used when you define activity log alerts.</span></span> <span data-ttu-id="1b965-108">Tyto skupiny můžete použít znovu pak Každá výstraha aktivity protokolu, které definujete, zajistíte, že jsou stejné akce trvá pokaždé, když se aktivuje výstraha aktivity protokolu.</span><span class="sxs-lookup"><span data-stu-id="1b965-108">These groups can then be reused by each activity log alert you define, ensuring that the same actions are taken each time the activity log alert is triggered.</span></span>

<span data-ttu-id="1b965-109">Skupinu akce může mít až 10 každý typ akce.</span><span class="sxs-lookup"><span data-stu-id="1b965-109">An action group can have up to 10 of each action type.</span></span> <span data-ttu-id="1b965-110">Každá akce se skládá z následujících vlastností:</span><span class="sxs-lookup"><span data-stu-id="1b965-110">Each action is made up of the following properties:</span></span>

* <span data-ttu-id="1b965-111">**Název**: Jedinečný identifikátor v rámci skupiny pro akce.</span><span class="sxs-lookup"><span data-stu-id="1b965-111">**Name**: A unique identifier within the action group.</span></span>  
* <span data-ttu-id="1b965-112">**Typ akce**: Odeslat zprávu SMS, e-mailovou zprávu nebo volat webhook, jehož.</span><span class="sxs-lookup"><span data-stu-id="1b965-112">**Action type**: Send an SMS, send an email, or call a webhook.</span></span>  
* <span data-ttu-id="1b965-113">**Podrobnosti o**: odpovídající telefonní číslo, e-mailovou adresu nebo webhooku identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="1b965-113">**Details**: The corresponding phone number, email address, or webhook URI.</span></span>

<span data-ttu-id="1b965-114">Informace o tom, jak pomocí šablony Azure Resource Manager můžete nakonfigurovat skupiny akcí najdete v tématu [šablony správce prostředků skupiny akce](monitoring-create-action-group-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="1b965-114">For information on how to use Azure Resource Manager templates to configure action groups, see [Action group Resource Manager templates](monitoring-create-action-group-with-resource-manager-template.md).</span></span>

## <a name="create-an-action-group-by-using-the-azure-portal"></a><span data-ttu-id="1b965-115">Vytvořit skupinu akce pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="1b965-115">Create an action group by using the Azure portal</span></span> ##
1. <span data-ttu-id="1b965-116">V [portál](https://portal.azure.com), vyberte **monitorování**.</span><span class="sxs-lookup"><span data-stu-id="1b965-116">In the [portal](https://portal.azure.com), select **Monitor**.</span></span> <span data-ttu-id="1b965-117">**Monitorování** slučuje okno veškeré monitorování nastavení a data v jednom zobrazení.</span><span class="sxs-lookup"><span data-stu-id="1b965-117">The **Monitor** blade consolidates all your monitoring settings and data in one view.</span></span>

    ![Službu "Sledování"](./media/monitoring-action-groups/home-monitor.png)
2. <span data-ttu-id="1b965-119">V **protokol aktivit** vyberte **skupiny akcí**.</span><span class="sxs-lookup"><span data-stu-id="1b965-119">In the **Activity log** section, select **Action groups**.</span></span>

    ![Na kartě "Akce skupiny"](./media/monitoring-action-groups/action-groups-blade.png)
3. <span data-ttu-id="1b965-121">Vyberte **přidat akci skupinu**a vyplňte příslušná pole.</span><span class="sxs-lookup"><span data-stu-id="1b965-121">Select **Add action group**, and fill in the fields.</span></span>

    ![Příkaz "Přidat skupinu akce"](./media/monitoring-action-groups/add-action-group.png)
4. <span data-ttu-id="1b965-123">Zadejte název do pole **název skupiny akce** pole a zadejte název do pole **krátký název** pole.</span><span class="sxs-lookup"><span data-stu-id="1b965-123">Enter a name in the **Action group name** box, and enter a name in the **Short name** box.</span></span> <span data-ttu-id="1b965-124">Krátký název se používá namísto názvu skupiny úplné akce při odesílání oznámení pomocí této skupiny.</span><span class="sxs-lookup"><span data-stu-id="1b965-124">The short name is used in place of a full action group name when notifications are sent using this group.</span></span>

      ![Dialogové okno Přidat skupinu akce"](./media/monitoring-action-groups/action-group-define.png)

5. <span data-ttu-id="1b965-126">**Předplatné** pole autofills s vaším aktuálním předplatným.</span><span class="sxs-lookup"><span data-stu-id="1b965-126">The **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="1b965-127">Toto předplatné je ten, ve kterém je akce skupinu uložit.</span><span class="sxs-lookup"><span data-stu-id="1b965-127">This subscription is the one in which the action group is saved.</span></span>

6. <span data-ttu-id="1b965-128">Vyberte **skupiny prostředků** ve skupině akce je uložen.</span><span class="sxs-lookup"><span data-stu-id="1b965-128">Select the **Resource group** in which the action group is saved.</span></span>

7. <span data-ttu-id="1b965-129">Definujte seznam akcí, tím, že poskytuje každá akce:</span><span class="sxs-lookup"><span data-stu-id="1b965-129">Define a list of actions by providing each action's:</span></span>

    <span data-ttu-id="1b965-130">a.</span><span class="sxs-lookup"><span data-stu-id="1b965-130">a.</span></span> <span data-ttu-id="1b965-131">**Název**: Zadejte jedinečný identifikátor pro tuto akci.</span><span class="sxs-lookup"><span data-stu-id="1b965-131">**Name**: Enter a unique identifier for this action.</span></span>

    <span data-ttu-id="1b965-132">b.</span><span class="sxs-lookup"><span data-stu-id="1b965-132">b.</span></span> <span data-ttu-id="1b965-133">**Typ akce**: Vyberte SMS, e-mailu nebo webhooku.</span><span class="sxs-lookup"><span data-stu-id="1b965-133">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="1b965-134">c.</span><span class="sxs-lookup"><span data-stu-id="1b965-134">c.</span></span> <span data-ttu-id="1b965-135">**Podrobnosti o**: v závislosti na typu akce, zadejte telefonní číslo, e-mailovou adresu nebo webhooku identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="1b965-135">**Details**: Based on the action type, enter a phone number, email address, or webhook URI.</span></span>

8. <span data-ttu-id="1b965-136">Vyberte **OK** vytvořit skupinu akce.</span><span class="sxs-lookup"><span data-stu-id="1b965-136">Select **OK** to create the action group.</span></span>

## <a name="manage-your-action-groups"></a><span data-ttu-id="1b965-137">Správa skupin akce</span><span class="sxs-lookup"><span data-stu-id="1b965-137">Manage your action groups</span></span> ##
<span data-ttu-id="1b965-138">Jakmile vytvoříte skupinu akcí, se zobrazí na **skupiny akcí** části **monitorování** okno.</span><span class="sxs-lookup"><span data-stu-id="1b965-138">After you create an action group, it's visible in the **Action groups** section of the **Monitor** blade.</span></span> <span data-ttu-id="1b965-139">Vyberte skupinu akce, kterou chcete spravovat:</span><span class="sxs-lookup"><span data-stu-id="1b965-139">Select the action group you want to manage to:</span></span>

* <span data-ttu-id="1b965-140">Přidat, upravit nebo odebrat akce.</span><span class="sxs-lookup"><span data-stu-id="1b965-140">Add, edit, or remove actions.</span></span>
* <span data-ttu-id="1b965-141">Odstraňte skupinu akce.</span><span class="sxs-lookup"><span data-stu-id="1b965-141">Delete the action group.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1b965-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="1b965-142">Next steps</span></span> ##
* <span data-ttu-id="1b965-143">Další informace o [SMS výstrahy chování](monitoring-sms-alert-behavior.md).</span><span class="sxs-lookup"><span data-stu-id="1b965-143">Learn more about [SMS alert behavior](monitoring-sms-alert-behavior.md).</span></span>  
* <span data-ttu-id="1b965-144">Získání [porozumět schéma výstrahy webhooku protokolu činnosti](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="1b965-144">Gain an [understanding of the activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>  
* <span data-ttu-id="1b965-145">Další informace o [omezení rychlosti](monitoring-alerts-rate-limiting.md) výstrah.</span><span class="sxs-lookup"><span data-stu-id="1b965-145">Learn more about [rate limiting](monitoring-alerts-rate-limiting.md) on alerts.</span></span> 
* <span data-ttu-id="1b965-146">Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md)a zjistěte, jak dostávat výstrahy.</span><span class="sxs-lookup"><span data-stu-id="1b965-146">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how to receive alerts.</span></span>  
* <span data-ttu-id="1b965-147">Zjistěte, jak [Konfigurace upozornění pokaždé, když je odeslána oznámení o stavu služby](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="1b965-147">Learn how to [configure alerts whenever a service health notification is posted](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
