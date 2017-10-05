---
title: "Vytvořit aktivitu protokolu výstrahy | Microsoft Docs"
description: "Informování prostřednictvím serveru SMS, webhooku a e-mailu při určité události v protokolu aktivit."
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
ms.date: 08/03/2017
ms.author: johnkem
ms.openlocfilehash: 3885469ec0e1fcc31386dd0ad7fe6cb5d03ab28e
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/29/2017
---
# <a name="create-activity-log-alerts"></a><span data-ttu-id="94d38-103">Vytvořit aktivitu protokolu výstrahy</span><span class="sxs-lookup"><span data-stu-id="94d38-103">Create activity log alerts</span></span>

## <a name="overview"></a><span data-ttu-id="94d38-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="94d38-104">Overview</span></span>
<span data-ttu-id="94d38-105">Výstrahy protokolu aktivit jsou výstrahy, které aktivovat při výskytu nové aktivity protokolu události, která odpovídá podmínkám uvedeném ve výstraze.</span><span class="sxs-lookup"><span data-stu-id="94d38-105">Activity log alerts are alerts that activate when a new activity log event occurs that matches the conditions specified in the alert.</span></span> <span data-ttu-id="94d38-106">Jsou prostředky Azure, takže je lze vytvořit pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="94d38-106">They are Azure resources, so they can be created by using an Azure Resource Manager template.</span></span> <span data-ttu-id="94d38-107">Také mohou být vytvořeny, aktualizovat nebo odstranit na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="94d38-107">They also can be created, updated, or deleted in the Azure portal.</span></span> <span data-ttu-id="94d38-108">Tento článek představuje koncepty za aktivitu protokolu výstrahy.</span><span class="sxs-lookup"><span data-stu-id="94d38-108">This article introduces the concepts behind activity log alerts.</span></span> <span data-ttu-id="94d38-109">Ji pak ukazuje, jak nastavit výstrahy na aktivity protokolu události pomocí portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="94d38-109">It then shows you how to use the Azure portal to set up an alert on activity log events.</span></span>

<span data-ttu-id="94d38-110">Obvykle můžete vytvořit aktivitu protokolu výstrahy pro příjem oznámení při:</span><span class="sxs-lookup"><span data-stu-id="94d38-110">Typically, you create activity log alerts to receive notifications when:</span></span>

* <span data-ttu-id="94d38-111">Určité změny, ke kterým došlo u prostředků ve vašem předplatném Azure často obor pro určitý prostředek skupiny nebo prostředky.</span><span class="sxs-lookup"><span data-stu-id="94d38-111">Specific changes occur on resources in your Azure subscription, often scoped to particular resource groups or resources.</span></span> <span data-ttu-id="94d38-112">Například můžete chtít být upozorněni, když se odstraní všechny virtuální počítače v myProductionResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="94d38-112">For example, you might want to be notified when any virtual machine in myProductionResourceGroup is deleted.</span></span> <span data-ttu-id="94d38-113">Nebo můžete chtít upozornění, pokud žádné nové role přiřazené uživateli v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="94d38-113">Or, you might want to be notified if any new roles are assigned to a user in your subscription.</span></span>
* <span data-ttu-id="94d38-114">Nastane událost stavu služby.</span><span class="sxs-lookup"><span data-stu-id="94d38-114">A service health event occurs.</span></span> <span data-ttu-id="94d38-115">Události stavu služby zahrnují upozornění incidenty a události údržby, které se vztahují k prostředkům ve vašem předplatném.</span><span class="sxs-lookup"><span data-stu-id="94d38-115">Service health events include notification of incidents and maintenance events that apply to resources in your subscription.</span></span>

<span data-ttu-id="94d38-116">V obou případech výstrahu protokolu aktivit monitoruje pouze pro události v rámci předplatného, ve kterém se vytvoří výstraha.</span><span class="sxs-lookup"><span data-stu-id="94d38-116">In either case, an activity log alert monitors only for events in the subscription in which the alert is created.</span></span>

<span data-ttu-id="94d38-117">Můžete nakonfigurovat výstrahu protokolu aktivity podle libovolné vlastnosti nejvyšší úrovně v objektu JSON pro aktivitu protokolu události.</span><span class="sxs-lookup"><span data-stu-id="94d38-117">You can configure an activity log alert based on any top-level property in the JSON object for an activity log event.</span></span> <span data-ttu-id="94d38-118">Na portálu však uvádí nejběžnější možnosti:</span><span class="sxs-lookup"><span data-stu-id="94d38-118">However, the portal shows the most common options:</span></span>

- <span data-ttu-id="94d38-119">**Kategorie**: pro správu, služba stavu, škálování a doporučení.</span><span class="sxs-lookup"><span data-stu-id="94d38-119">**Category**: Administrative, Service Health, Autoscale, and Recommendation.</span></span> <span data-ttu-id="94d38-120">Další informace najdete v tématu [přehled protokolu Azure činnosti](./monitoring-overview-activity-logs.md#categories-in-the-activity-log).</span><span class="sxs-lookup"><span data-stu-id="94d38-120">For more information, see [Overview of the Azure activity log](./monitoring-overview-activity-logs.md#categories-in-the-activity-log).</span></span> <span data-ttu-id="94d38-121">Další informace o události stavu služby najdete v tématu [výstrahy v protokolu aktivit na oznámení o službách](./monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="94d38-121">To learn more about service health events, see [Receive activity log alerts on service notifications](./monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
- <span data-ttu-id="94d38-122">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="94d38-122">**Resource group**</span></span>
- <span data-ttu-id="94d38-123">**Prostředek**</span><span class="sxs-lookup"><span data-stu-id="94d38-123">**Resource**</span></span>
- <span data-ttu-id="94d38-124">**Typ prostředku**</span><span class="sxs-lookup"><span data-stu-id="94d38-124">**Resource type**</span></span>
- <span data-ttu-id="94d38-125">**Název operace**: název operace The Resource Manager Role-Based řízení přístupu.</span><span class="sxs-lookup"><span data-stu-id="94d38-125">**Operation name**: The Resource Manager Role-Based Access Control operation name.</span></span>
- <span data-ttu-id="94d38-126">**Úroveň**: úroveň závažnosti události (Verbose, informační, upozornění, chyby nebo kritický).</span><span class="sxs-lookup"><span data-stu-id="94d38-126">**Level**: The severity level of the event (Verbose, Informational, Warning, Error, or Critical).</span></span>
- <span data-ttu-id="94d38-127">**Stav**: stav události, obvykle spuštění, se nezdařilo nebo bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="94d38-127">**Status**: The status of the event, typically Started, Failed, or Succeeded.</span></span>
- <span data-ttu-id="94d38-128">**Událost iniciovaná**: také známé jako "volající."</span><span class="sxs-lookup"><span data-stu-id="94d38-128">**Event initiated by**: Also known as the "caller."</span></span> <span data-ttu-id="94d38-129">E-mailovou adresu nebo identifikátor Azure Active Directory uživatele, který provádí operaci.</span><span class="sxs-lookup"><span data-stu-id="94d38-129">The email address or Azure Active Directory identifier of the user who performed the operation.</span></span>

>[!NOTE]
><span data-ttu-id="94d38-130">Zadejte alespoň dvě z předchozí kritéria v výstrahy, s jedním probíhá kategorii.</span><span class="sxs-lookup"><span data-stu-id="94d38-130">You must specify at least two of the preceding criteria in your alert, with one being the category.</span></span> <span data-ttu-id="94d38-131">Nelze vytvořit výstrahu, která aktivuje pokaždé, když dojde k vytvoření události v protokolech aktivity.</span><span class="sxs-lookup"><span data-stu-id="94d38-131">You may not create an alert that activates every time an event is created in the activity logs.</span></span>
>
>

<span data-ttu-id="94d38-132">Když se aktivuje výstrahu protokolu aktivit, používá skupinu akcí ke generování akcí nebo oznámení.</span><span class="sxs-lookup"><span data-stu-id="94d38-132">When an activity log alert is activated, it uses an action group to generate actions or notifications.</span></span> <span data-ttu-id="94d38-133">Skupinu akcí je opakovaně použitelné sadu příjemců oznámení, jako je například e-mailové adresy, adresy URL webhooku nebo SMS telefonních čísel.</span><span class="sxs-lookup"><span data-stu-id="94d38-133">An action group is a reusable set of notification receivers, such as email addresses, webhook URLs, or SMS phone numbers.</span></span> <span data-ttu-id="94d38-134">Příjemci lze odkazovat z více výstrah a centralizovat skupiny vaší kanály oznámení.</span><span class="sxs-lookup"><span data-stu-id="94d38-134">The receivers can be referenced from multiple alerts to centralize and group your notification channels.</span></span> <span data-ttu-id="94d38-135">Když definujete výstrahu protokolu aktivit, máte dvě možnosti.</span><span class="sxs-lookup"><span data-stu-id="94d38-135">When you define your activity log alert, you have two options.</span></span> <span data-ttu-id="94d38-136">Můžete:</span><span class="sxs-lookup"><span data-stu-id="94d38-136">You can:</span></span>

* <span data-ttu-id="94d38-137">Použijte existující skupinu akce v protokolu upozornění aktivity.</span><span class="sxs-lookup"><span data-stu-id="94d38-137">Use an existing action group in your activity log alert.</span></span> 
* <span data-ttu-id="94d38-138">Vytvořte novou skupinu akce.</span><span class="sxs-lookup"><span data-stu-id="94d38-138">Create a new action group.</span></span> 

<span data-ttu-id="94d38-139">Další informace o skupinách akce najdete v tématu [vytvořit a spravovat skupiny akce na portálu Azure](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="94d38-139">To learn more about action groups, see [Create and manage action groups in the Azure portal](monitoring-action-groups.md).</span></span>

<span data-ttu-id="94d38-140">Další informace o oznámení o stavu služby najdete v tématu [výstrahy v protokolu aktivit na oznámení o stavu služby](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="94d38-140">To learn more about service health notifications, see [Receive activity log alerts on service health notifications](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>

## <a name="create-an-alert-on-an-activity-log-event-with-a-new-action-group-by-using-the-azure-portal"></a><span data-ttu-id="94d38-141">Vytvořit výstrahu na aktivity protokolu události s novou skupinu akce pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="94d38-141">Create an alert on an activity log event with a new action group by using the Azure portal</span></span>
1. <span data-ttu-id="94d38-142">V [portál](https://portal.azure.com), vyberte **monitorování**.</span><span class="sxs-lookup"><span data-stu-id="94d38-142">In the [portal](https://portal.azure.com), select **Monitor**.</span></span>

    ![Službu "Sledování"](./media/monitoring-activity-log-alerts/home-monitor.png)
2. <span data-ttu-id="94d38-144">V **protokol aktivit** vyberte **výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="94d38-144">In the **Activity log** section, select **Alerts**.</span></span>

    ![Na kartě "Výstrahy"](./media/monitoring-activity-log-alerts/alerts-blades.png)
3. <span data-ttu-id="94d38-146">Vyberte **přidat aktivitu protokolu upozornění**a vyplňte příslušná pole.</span><span class="sxs-lookup"><span data-stu-id="94d38-146">Select **Add activity log alert**, and fill in the fields.</span></span>

4. <span data-ttu-id="94d38-147">Zadejte název do pole **výstrahy název aktivity protokolu** a vyberte **popis**.</span><span class="sxs-lookup"><span data-stu-id="94d38-147">Enter a name in the **Activity log alert name** box, and select a **Description**.</span></span>

    ![Příkaz "Přidat aktivitu protokolu upozornění"](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

5. <span data-ttu-id="94d38-149">**Předplatné** pole autofills s vaším aktuálním předplatným.</span><span class="sxs-lookup"><span data-stu-id="94d38-149">The **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="94d38-150">Toto předplatné je ten, ve kterém je akce skupinu uložit.</span><span class="sxs-lookup"><span data-stu-id="94d38-150">This subscription is the one in which the action group is saved.</span></span> <span data-ttu-id="94d38-151">Výstrahy prostředek je nasazen na toto předplatné a monitoruje aktivity protokolu události z něj.</span><span class="sxs-lookup"><span data-stu-id="94d38-151">The alert resource is deployed to this subscription and monitors activity log events from it.</span></span>

    ![Dialogové okno "Přidat aktivitu protokolu upozornění"](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

6. <span data-ttu-id="94d38-153">Vyberte **skupiny prostředků** v výstrahy prostředek je vytvořen.</span><span class="sxs-lookup"><span data-stu-id="94d38-153">Select the **Resource group** in which the alert resource is created.</span></span> <span data-ttu-id="94d38-154">Nejedná se o skupině prostředků, který je monitorován výstraha.</span><span class="sxs-lookup"><span data-stu-id="94d38-154">This is not the resource group that's monitored by the alert.</span></span> <span data-ttu-id="94d38-155">Místo toho je skupina prostředků, kde je umístěný prostředek výstrahy.</span><span class="sxs-lookup"><span data-stu-id="94d38-155">Instead, it's the resource group where the alert resource is located.</span></span>

7. <span data-ttu-id="94d38-156">Volitelně vyberte **kategorie události** upravit další filtry, které jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="94d38-156">Optionally, select an **Event category** to modify the additional filters that are shown.</span></span> <span data-ttu-id="94d38-157">Pro správu události filtry zahrnují **skupiny prostředků**, **prostředků**, **typ prostředku**, **název operace**, **Úroveň**, **stav**, a **iniciovaná událostí**.</span><span class="sxs-lookup"><span data-stu-id="94d38-157">For Administrative events, the filters include **Resource group**, **Resource**, **Resource type**, **Operation name**, **Level**, **Status**, and **Event initiated by**.</span></span> <span data-ttu-id="94d38-158">Tyto hodnoty identifikovat události, které by měly monitorovat této výstrahy.</span><span class="sxs-lookup"><span data-stu-id="94d38-158">These values identify which events this alert should monitor.</span></span>

    >[!NOTE]
    ><span data-ttu-id="94d38-159">Musíte zadat alespoň jeden z předchozí kritéria v upozornění.</span><span class="sxs-lookup"><span data-stu-id="94d38-159">You must specify at least one of the preceding criteria in your alert.</span></span> <span data-ttu-id="94d38-160">Nelze vytvořit výstrahu, která aktivuje pokaždé, když dojde k vytvoření události v protokolech aktivity.</span><span class="sxs-lookup"><span data-stu-id="94d38-160">You may not create an alert that activates every time an event is created in the activity logs.</span></span>
    >
    >

8. <span data-ttu-id="94d38-161">Zadejte název do pole **název skupiny akce** pole a zadejte název do pole **krátký název** pole.</span><span class="sxs-lookup"><span data-stu-id="94d38-161">Enter a name in the **Action group name** box, and enter a name in the **Short name** box.</span></span> <span data-ttu-id="94d38-162">Krátký název se používá namísto názvu skupiny úplné akce při odesílání oznámení pomocí této skupiny.</span><span class="sxs-lookup"><span data-stu-id="94d38-162">The short name is used in place of a full action group name when notifications are sent using this group.</span></span>

9.  <span data-ttu-id="94d38-163">Definujte seznam akcí, tím, že poskytuje akce:</span><span class="sxs-lookup"><span data-stu-id="94d38-163">Define a list of actions by providing the action’s:</span></span>

    <span data-ttu-id="94d38-164">a.</span><span class="sxs-lookup"><span data-stu-id="94d38-164">a.</span></span> <span data-ttu-id="94d38-165">**Název**: Zadejte název akce, alias nebo identifikátor.</span><span class="sxs-lookup"><span data-stu-id="94d38-165">**Name**: Enter the action’s name, alias, or identifier.</span></span>

    <span data-ttu-id="94d38-166">b.</span><span class="sxs-lookup"><span data-stu-id="94d38-166">b.</span></span> <span data-ttu-id="94d38-167">**Typ akce**: Vyberte SMS, e-mailu nebo webhooku.</span><span class="sxs-lookup"><span data-stu-id="94d38-167">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="94d38-168">c.</span><span class="sxs-lookup"><span data-stu-id="94d38-168">c.</span></span> <span data-ttu-id="94d38-169">**Podrobnosti o**: v závislosti na typu akce, zadejte telefonní číslo, e-mailovou adresu nebo webhooku identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="94d38-169">**Details**: Based on the action type, enter a phone number, email address, or webhook URI.</span></span>

10. <span data-ttu-id="94d38-170">Vyberte **OK** vytvořit výstrahu.</span><span class="sxs-lookup"><span data-stu-id="94d38-170">Select **OK** to create the alert.</span></span>

<span data-ttu-id="94d38-171">Výstraha trvá několik minut plně šíří a pak stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="94d38-171">The alert takes a few minutes to fully propagate and then become active.</span></span> <span data-ttu-id="94d38-172">Aktivuje při nové události kritériím výstrah.</span><span class="sxs-lookup"><span data-stu-id="94d38-172">It triggers when new events match the alert's criteria.</span></span>

<span data-ttu-id="94d38-173">Další informace najdete v tématu [pochopit schéma webhooku použít ve výstrahách aktivity protokolu](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="94d38-173">For more information, see [Understand the webhook schema used in activity log alerts](monitoring-activity-log-alerts-webhook.md).</span></span>

>[!NOTE]
><span data-ttu-id="94d38-174">Skupina akce definované v těchto kroků je opakovaně použitelné jako stávající skupina akce pro všechny budoucí výstrahy definice.</span><span class="sxs-lookup"><span data-stu-id="94d38-174">The action group defined in these steps is reusable as an existing action group for all future alert definitions.</span></span>
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-the-azure-portal"></a><span data-ttu-id="94d38-175">Vytvořit výstrahu na aktivity protokolu události pro stávající skupinu akce pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="94d38-175">Create an alert on an activity log event for an existing action group by using the Azure portal</span></span>
1. <span data-ttu-id="94d38-176">Postupujte podle kroků 1 až 7 v předchozí části Vytvořit výstrahu protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="94d38-176">Follow steps 1 through 7 in the previous section to create your activity log alert.</span></span>

2. <span data-ttu-id="94d38-177">V části **oznámit prostřednictvím**, vyberte **existující** tlačítko akce skupiny.</span><span class="sxs-lookup"><span data-stu-id="94d38-177">Under **Notify via**, select the **Existing** action group button.</span></span> <span data-ttu-id="94d38-178">Vyberte existující skupinu akce ze seznamu.</span><span class="sxs-lookup"><span data-stu-id="94d38-178">Select an existing action group from the list.</span></span>

3. <span data-ttu-id="94d38-179">Vyberte **OK** vytvořit výstrahu.</span><span class="sxs-lookup"><span data-stu-id="94d38-179">Select **OK** to create the alert.</span></span>

<span data-ttu-id="94d38-180">Výstraha trvá několik minut plně šíří a pak stane aktivní.</span><span class="sxs-lookup"><span data-stu-id="94d38-180">The alert takes a few minutes to fully propagate and then become active.</span></span> <span data-ttu-id="94d38-181">Aktivuje při nové události kritériím výstrah.</span><span class="sxs-lookup"><span data-stu-id="94d38-181">It triggers when new events match the alert's criteria.</span></span>

## <a name="manage-your-alerts"></a><span data-ttu-id="94d38-182">Spravovat oznámení</span><span class="sxs-lookup"><span data-stu-id="94d38-182">Manage your alerts</span></span>

<span data-ttu-id="94d38-183">Po vytvoření výstrahy, je viditelný v části výstrahy monitorování okna.</span><span class="sxs-lookup"><span data-stu-id="94d38-183">After you create an alert, it's visible in the Alerts section of the Monitor blade.</span></span> <span data-ttu-id="94d38-184">Vyberte výstrahy, kterou chcete spravovat:</span><span class="sxs-lookup"><span data-stu-id="94d38-184">Select the alert you want to manage to:</span></span>

* <span data-ttu-id="94d38-185">Upravte.</span><span class="sxs-lookup"><span data-stu-id="94d38-185">Edit it.</span></span>
* <span data-ttu-id="94d38-186">Odstraňte ji.</span><span class="sxs-lookup"><span data-stu-id="94d38-186">Delete it.</span></span>
* <span data-ttu-id="94d38-187">Zakázat nebo povolit, pokud chcete dočasně zastavit nebo obnovit příjem oznámení pro výstrahy.</span><span class="sxs-lookup"><span data-stu-id="94d38-187">Disable or enable it, if you want to temporarily stop or resume receiving notifications for the alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="94d38-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="94d38-188">Next steps</span></span>
- <span data-ttu-id="94d38-189">Získat [přehled výstrah](monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="94d38-189">Get an [overview of alerts](monitoring-overview-alerts.md).</span></span>
- <span data-ttu-id="94d38-190">Další informace o [omezení rychlosti oznámení](monitoring-alerts-rate-limiting.md).</span><span class="sxs-lookup"><span data-stu-id="94d38-190">Learn about [notification rate limiting](monitoring-alerts-rate-limiting.md).</span></span>
- <span data-ttu-id="94d38-191">Zkontrolujte [schéma výstrahy webhooku protokolu činnosti](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="94d38-191">Review the [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>
- <span data-ttu-id="94d38-192">Další informace o [skupiny akcí](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="94d38-192">Learn more about [action groups](monitoring-action-groups.md).</span></span>  
- <span data-ttu-id="94d38-193">Další informace o [oznámení o stavu služby](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="94d38-193">Learn about [service health notifications](monitoring-service-notifications.md).</span></span>
- <span data-ttu-id="94d38-194">Vytvoření [výstraha aktivity protokolu monitorovat všechny operace škálování modul vaše předplatné](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span><span class="sxs-lookup"><span data-stu-id="94d38-194">Create an [activity log alert to monitor all autoscale engine operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span></span>
- <span data-ttu-id="94d38-195">Vytvoření [výstraha aktivity protokolu monitorovat všechny operace škálování nebo škálovatelnou neúspěšné automatické škálování na vaše předplatné](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span><span class="sxs-lookup"><span data-stu-id="94d38-195">Create an [activity log alert to monitor all failed autoscale scale-in/scale-out operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span></span>
