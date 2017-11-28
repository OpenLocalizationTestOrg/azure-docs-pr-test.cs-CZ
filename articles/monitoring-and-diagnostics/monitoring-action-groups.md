---
title: "aaaCreate a spravovat skupiny akce v hello portálu Azure | Microsoft Docs"
description: "Zjistěte, jak toocreate a spravovat skupiny akce v hello portálu Azure."
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
ms.openlocfilehash: 97e0b22bea7787fff6856f895a7e6256c177efd9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-action-groups-in-hello-azure-portal"></a><span data-ttu-id="140fb-103">Vytvoření a Správa skupin akce v hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="140fb-103">Create and manage action groups in hello Azure portal</span></span>
## <a name="overview"></a><span data-ttu-id="140fb-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="140fb-104">Overview</span></span> ##
<span data-ttu-id="140fb-105">Tento článek ukazuje, jak toocreate a spravovat skupiny akce v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="140fb-105">This article shows you how toocreate and manage action groups in hello Azure portal.</span></span>

<span data-ttu-id="140fb-106">Seznam akcí, můžete nakonfigurovat skupiny akcí.</span><span class="sxs-lookup"><span data-stu-id="140fb-106">You can configure a list of actions with action groups.</span></span> <span data-ttu-id="140fb-107">Tyto skupiny pak lze použít při definování aktivity protokolu výstrahy.</span><span class="sxs-lookup"><span data-stu-id="140fb-107">These groups then can be used when you define activity log alerts.</span></span> <span data-ttu-id="140fb-108">Tyto skupiny můžete použít znovu pak Každá výstraha aktivity protokolu, které definujete, zajistit, že hello pořízení stejné akce jsou pokaždé, když se aktivuje výstraha hello aktivity protokolu.</span><span class="sxs-lookup"><span data-stu-id="140fb-108">These groups can then be reused by each activity log alert you define, ensuring that hello same actions are taken each time hello activity log alert is triggered.</span></span>

<span data-ttu-id="140fb-109">Skupinu akce může mít až too10 každý typ akce.</span><span class="sxs-lookup"><span data-stu-id="140fb-109">An action group can have up too10 of each action type.</span></span> <span data-ttu-id="140fb-110">Každá akce se skládá z hello následující vlastnosti:</span><span class="sxs-lookup"><span data-stu-id="140fb-110">Each action is made up of hello following properties:</span></span>

* <span data-ttu-id="140fb-111">**Název**: Jedinečný identifikátor v rámci skupiny akce hello.</span><span class="sxs-lookup"><span data-stu-id="140fb-111">**Name**: A unique identifier within hello action group.</span></span>  
* <span data-ttu-id="140fb-112">**Typ akce**: Odeslat zprávu SMS, e-mailovou zprávu nebo volat webhook, jehož.</span><span class="sxs-lookup"><span data-stu-id="140fb-112">**Action type**: Send an SMS, send an email, or call a webhook.</span></span>  
* <span data-ttu-id="140fb-113">**Podrobnosti o**: hello odpovídající telefonní číslo, e-mailovou adresu nebo webhooku identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="140fb-113">**Details**: hello corresponding phone number, email address, or webhook URI.</span></span>

<span data-ttu-id="140fb-114">Informace o tom toouse Azure Resource Manager šablony tooconfigure akce skupinách naleznete v tématu [šablony správce prostředků skupiny akce](monitoring-create-action-group-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="140fb-114">For information on how toouse Azure Resource Manager templates tooconfigure action groups, see [Action group Resource Manager templates](monitoring-create-action-group-with-resource-manager-template.md).</span></span>

## <a name="create-an-action-group-by-using-hello-azure-portal"></a><span data-ttu-id="140fb-115">Vytvořit skupinu akce pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="140fb-115">Create an action group by using hello Azure portal</span></span> ##
1. <span data-ttu-id="140fb-116">V hello [portál](https://portal.azure.com), vyberte **monitorování**.</span><span class="sxs-lookup"><span data-stu-id="140fb-116">In hello [portal](https://portal.azure.com), select **Monitor**.</span></span> <span data-ttu-id="140fb-117">Hello **monitorování** slučuje okno veškeré monitorování nastavení a data v jednom zobrazení.</span><span class="sxs-lookup"><span data-stu-id="140fb-117">hello **Monitor** blade consolidates all your monitoring settings and data in one view.</span></span>

    ![Hello "Sledování" service](./media/monitoring-action-groups/home-monitor.png)
2. <span data-ttu-id="140fb-119">V hello **protokol aktivit** vyberte **skupiny akcí**.</span><span class="sxs-lookup"><span data-stu-id="140fb-119">In hello **Activity log** section, select **Action groups**.</span></span>

    ![Karta "Akce skupiny" Hello](./media/monitoring-action-groups/action-groups-blade.png)
3. <span data-ttu-id="140fb-121">Vyberte **přidat akci skupinu**a vyplňte pole hello.</span><span class="sxs-lookup"><span data-stu-id="140fb-121">Select **Add action group**, and fill in hello fields.</span></span>

    ![příkaz "Přidat skupinu akce" Hello](./media/monitoring-action-groups/add-action-group.png)
4. <span data-ttu-id="140fb-123">Zadejte název v hello **název skupiny akce** pole a zadejte název v hello **krátký název** pole.</span><span class="sxs-lookup"><span data-stu-id="140fb-123">Enter a name in hello **Action group name** box, and enter a name in hello **Short name** box.</span></span> <span data-ttu-id="140fb-124">krátký název Hello je použít místo názvu skupiny úplné akce při odesílání oznámení pomocí této skupiny.</span><span class="sxs-lookup"><span data-stu-id="140fb-124">hello short name is used in place of a full action group name when notifications are sent using this group.</span></span>

      ![Dialogové okno Hello přidat akci skupiny"](./media/monitoring-action-groups/action-group-define.png)

5. <span data-ttu-id="140fb-126">Hello **předplatné** pole autofills s vaším aktuálním předplatným.</span><span class="sxs-lookup"><span data-stu-id="140fb-126">hello **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="140fb-127">Toto předplatné je hello, jeden v které skupinu akce hello je uložit.</span><span class="sxs-lookup"><span data-stu-id="140fb-127">This subscription is hello one in which hello action group is saved.</span></span>

6. <span data-ttu-id="140fb-128">Vyberte hello **skupiny prostředků** v akci, která hello skupinu uložit.</span><span class="sxs-lookup"><span data-stu-id="140fb-128">Select hello **Resource group** in which hello action group is saved.</span></span>

7. <span data-ttu-id="140fb-129">Definujte seznam akcí, tím, že poskytuje každá akce:</span><span class="sxs-lookup"><span data-stu-id="140fb-129">Define a list of actions by providing each action's:</span></span>

    <span data-ttu-id="140fb-130">a.</span><span class="sxs-lookup"><span data-stu-id="140fb-130">a.</span></span> <span data-ttu-id="140fb-131">**Název**: Zadejte jedinečný identifikátor pro tuto akci.</span><span class="sxs-lookup"><span data-stu-id="140fb-131">**Name**: Enter a unique identifier for this action.</span></span>

    <span data-ttu-id="140fb-132">b.</span><span class="sxs-lookup"><span data-stu-id="140fb-132">b.</span></span> <span data-ttu-id="140fb-133">**Typ akce**: Vyberte SMS, e-mailu nebo webhooku.</span><span class="sxs-lookup"><span data-stu-id="140fb-133">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="140fb-134">c.</span><span class="sxs-lookup"><span data-stu-id="140fb-134">c.</span></span> <span data-ttu-id="140fb-135">**Podrobnosti o**: založený na typu akce hello, zadejte telefonní číslo, e-mailovou adresu nebo webhooku identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="140fb-135">**Details**: Based on hello action type, enter a phone number, email address, or webhook URI.</span></span>

8. <span data-ttu-id="140fb-136">Vyberte **OK** toocreate hello akce skupiny.</span><span class="sxs-lookup"><span data-stu-id="140fb-136">Select **OK** toocreate hello action group.</span></span>

## <a name="manage-your-action-groups"></a><span data-ttu-id="140fb-137">Správa skupin akce</span><span class="sxs-lookup"><span data-stu-id="140fb-137">Manage your action groups</span></span> ##
<span data-ttu-id="140fb-138">Jakmile vytvoříte skupinu akcí, se zobrazí na hello **skupiny akcí** části hello **monitorování** okno.</span><span class="sxs-lookup"><span data-stu-id="140fb-138">After you create an action group, it's visible in hello **Action groups** section of hello **Monitor** blade.</span></span> <span data-ttu-id="140fb-139">Vyberte skupinu hello akce, kterou chcete toomanage na:</span><span class="sxs-lookup"><span data-stu-id="140fb-139">Select hello action group you want toomanage to:</span></span>

* <span data-ttu-id="140fb-140">Přidat, upravit nebo odebrat akce.</span><span class="sxs-lookup"><span data-stu-id="140fb-140">Add, edit, or remove actions.</span></span>
* <span data-ttu-id="140fb-141">Odstraňte skupinu akce hello.</span><span class="sxs-lookup"><span data-stu-id="140fb-141">Delete hello action group.</span></span>

## <a name="next-steps"></a><span data-ttu-id="140fb-142">Další kroky</span><span class="sxs-lookup"><span data-stu-id="140fb-142">Next steps</span></span> ##
* <span data-ttu-id="140fb-143">Další informace o [SMS výstrahy chování](monitoring-sms-alert-behavior.md).</span><span class="sxs-lookup"><span data-stu-id="140fb-143">Learn more about [SMS alert behavior](monitoring-sms-alert-behavior.md).</span></span>  
* <span data-ttu-id="140fb-144">Získání [pochopení hello aktivity protokolu výstrahy webhooku schématu](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="140fb-144">Gain an [understanding of hello activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>  
* <span data-ttu-id="140fb-145">Další informace o [omezení rychlosti](monitoring-alerts-rate-limiting.md) výstrah.</span><span class="sxs-lookup"><span data-stu-id="140fb-145">Learn more about [rate limiting](monitoring-alerts-rate-limiting.md) on alerts.</span></span> 
* <span data-ttu-id="140fb-146">Získat [přehled výstrah aktivity protokolu](monitoring-overview-alerts.md)a zjistěte, jak tooreceive výstrahy.</span><span class="sxs-lookup"><span data-stu-id="140fb-146">Get an [overview of activity log alerts](monitoring-overview-alerts.md), and learn how tooreceive alerts.</span></span>  
* <span data-ttu-id="140fb-147">Zjistěte, jak příliš[Konfigurace upozornění pokaždé, když je odeslána oznámení o stavu služby](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="140fb-147">Learn how too[configure alerts whenever a service health notification is posted](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
