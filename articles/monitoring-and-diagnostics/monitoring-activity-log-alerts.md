---
title: "aaaCreate aktivity protokolu výstrahy | Microsoft Docs"
description: "Informování prostřednictvím serveru SMS, webhooku a e-mailu při určité události v protokolu aktivit hello."
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
ms.openlocfilehash: ba0716cc12a0b3a0024ee5562a025f3f153f8982
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-activity-log-alerts"></a><span data-ttu-id="075fb-103">Vytvořit aktivitu protokolu výstrahy</span><span class="sxs-lookup"><span data-stu-id="075fb-103">Create activity log alerts</span></span>

## <a name="overview"></a><span data-ttu-id="075fb-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="075fb-104">Overview</span></span>
<span data-ttu-id="075fb-105">Výstrahy protokolu aktivit jsou výstrahy, které aktivovat při výskytu nové aktivity protokolu události, který odpovídá hello podmínky zadané v hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="075fb-105">Activity log alerts are alerts that activate when a new activity log event occurs that matches hello conditions specified in hello alert.</span></span> <span data-ttu-id="075fb-106">Jsou prostředky Azure, takže je lze vytvořit pomocí šablony Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="075fb-106">They are Azure resources, so they can be created by using an Azure Resource Manager template.</span></span> <span data-ttu-id="075fb-107">Také mohou být vytvořen, aktualizovat nebo odstranit v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="075fb-107">They also can be created, updated, or deleted in hello Azure portal.</span></span> <span data-ttu-id="075fb-108">Tento článek představuje koncepty hello za aktivitu protokolu výstrahy.</span><span class="sxs-lookup"><span data-stu-id="075fb-108">This article introduces hello concepts behind activity log alerts.</span></span> <span data-ttu-id="075fb-109">Poté vám ukáže, jak toouse hello Azure portálu tooset až výstrahu na aktivity protokolu události.</span><span class="sxs-lookup"><span data-stu-id="075fb-109">It then shows you how toouse hello Azure portal tooset up an alert on activity log events.</span></span>

<span data-ttu-id="075fb-110">Obvykle vytvoříte aktivity protokolu výstrahy, oznámení tooreceive při:</span><span class="sxs-lookup"><span data-stu-id="075fb-110">Typically, you create activity log alerts tooreceive notifications when:</span></span>

* <span data-ttu-id="075fb-111">Určité změny, ke kterým došlo u prostředků v Azure předplatného, často vymezená tooparticular skupiny prostředků nebo prostředky.</span><span class="sxs-lookup"><span data-stu-id="075fb-111">Specific changes occur on resources in your Azure subscription, often scoped tooparticular resource groups or resources.</span></span> <span data-ttu-id="075fb-112">Například můžete toobe upozorněni, když se odstraní všechny virtuální počítače v myProductionResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="075fb-112">For example, you might want toobe notified when any virtual machine in myProductionResourceGroup is deleted.</span></span> <span data-ttu-id="075fb-113">Nebo můžete chtít toobe oznámení, pokud žádné nové role přiřazené tooa uživatele v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="075fb-113">Or, you might want toobe notified if any new roles are assigned tooa user in your subscription.</span></span>
* <span data-ttu-id="075fb-114">Nastane událost stavu služby.</span><span class="sxs-lookup"><span data-stu-id="075fb-114">A service health event occurs.</span></span> <span data-ttu-id="075fb-115">Události stavu služby zahrnují upozornění incidenty a události údržby, které se vztahují tooresources v rámci vašeho předplatného.</span><span class="sxs-lookup"><span data-stu-id="075fb-115">Service health events include notification of incidents and maintenance events that apply tooresources in your subscription.</span></span>

<span data-ttu-id="075fb-116">V obou případech výstrahu protokolu aktivit monitoruje pouze pro události v hello předplatné, ve které hello se vytvoří výstraha.</span><span class="sxs-lookup"><span data-stu-id="075fb-116">In either case, an activity log alert monitors only for events in hello subscription in which hello alert is created.</span></span>

<span data-ttu-id="075fb-117">Můžete nakonfigurovat výstrahu protokolu aktivity podle libovolné vlastnosti nejvyšší úrovně v hello objekt JSON pro aktivitu protokolu události.</span><span class="sxs-lookup"><span data-stu-id="075fb-117">You can configure an activity log alert based on any top-level property in hello JSON object for an activity log event.</span></span> <span data-ttu-id="075fb-118">Portál hello však uvádí nejběžnější možnosti hello:</span><span class="sxs-lookup"><span data-stu-id="075fb-118">However, hello portal shows hello most common options:</span></span>

- <span data-ttu-id="075fb-119">**Kategorie**: pro správu, služba stavu, škálování a doporučení.</span><span class="sxs-lookup"><span data-stu-id="075fb-119">**Category**: Administrative, Service Health, Autoscale, and Recommendation.</span></span> <span data-ttu-id="075fb-120">Další informace najdete v tématu [přehled protokol činnosti Azure hello](./monitoring-overview-activity-logs.md#categories-in-the-activity-log).</span><span class="sxs-lookup"><span data-stu-id="075fb-120">For more information, see [Overview of hello Azure activity log](./monitoring-overview-activity-logs.md#categories-in-the-activity-log).</span></span> <span data-ttu-id="075fb-121">toolearn Další informace o události stavu služby, najdete v části [výstrahy v protokolu aktivit na oznámení o službách](./monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="075fb-121">toolearn more about service health events, see [Receive activity log alerts on service notifications](./monitoring-activity-log-alerts-on-service-notifications.md).</span></span>
- <span data-ttu-id="075fb-122">**Skupina prostředků**</span><span class="sxs-lookup"><span data-stu-id="075fb-122">**Resource group**</span></span>
- <span data-ttu-id="075fb-123">**Prostředek**</span><span class="sxs-lookup"><span data-stu-id="075fb-123">**Resource**</span></span>
- <span data-ttu-id="075fb-124">**Typ prostředku**</span><span class="sxs-lookup"><span data-stu-id="075fb-124">**Resource type**</span></span>
- <span data-ttu-id="075fb-125">**Název operace**: název operace hello řízení přístupu na základě Role Správce prostředků.</span><span class="sxs-lookup"><span data-stu-id="075fb-125">**Operation name**: hello Resource Manager Role-Based Access Control operation name.</span></span>
- <span data-ttu-id="075fb-126">**Úroveň**: hello úroveň závažnosti události hello (Verbose, informační, upozornění, chyby nebo kritický).</span><span class="sxs-lookup"><span data-stu-id="075fb-126">**Level**: hello severity level of hello event (Verbose, Informational, Warning, Error, or Critical).</span></span>
- <span data-ttu-id="075fb-127">**Stav**: stav hello hello události, obvykle spuštění, se nezdařilo nebo bylo úspěšné.</span><span class="sxs-lookup"><span data-stu-id="075fb-127">**Status**: hello status of hello event, typically Started, Failed, or Succeeded.</span></span>
- <span data-ttu-id="075fb-128">**Událost iniciovaná**: hello také známé jako "volající."</span><span class="sxs-lookup"><span data-stu-id="075fb-128">**Event initiated by**: Also known as hello "caller."</span></span> <span data-ttu-id="075fb-129">Hello e-mailovou adresu nebo identifikátor Azure Active Directory hello uživatele, který provedl operaci hello.</span><span class="sxs-lookup"><span data-stu-id="075fb-129">hello email address or Azure Active Directory identifier of hello user who performed hello operation.</span></span>

>[!NOTE]
><span data-ttu-id="075fb-130">Je nutné zadat alespoň dvě z hello před kritéria v výstrahy, s právě jednu kategorii hello.</span><span class="sxs-lookup"><span data-stu-id="075fb-130">You must specify at least two of hello preceding criteria in your alert, with one being hello category.</span></span> <span data-ttu-id="075fb-131">Nelze vytvořit výstrahu, která aktivuje pokaždé, když dojde k vytvoření události v protokolech aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="075fb-131">You may not create an alert that activates every time an event is created in hello activity logs.</span></span>
>
>

<span data-ttu-id="075fb-132">Když se aktivuje výstrahu protokolu aktivit, používá akci skupiny toogenerate akce nebo oznámení.</span><span class="sxs-lookup"><span data-stu-id="075fb-132">When an activity log alert is activated, it uses an action group toogenerate actions or notifications.</span></span> <span data-ttu-id="075fb-133">Skupinu akcí je opakovaně použitelné sadu příjemců oznámení, jako je například e-mailové adresy, adresy URL webhooku nebo SMS telefonních čísel.</span><span class="sxs-lookup"><span data-stu-id="075fb-133">An action group is a reusable set of notification receivers, such as email addresses, webhook URLs, or SMS phone numbers.</span></span> <span data-ttu-id="075fb-134">příjemci Hello můžete na něj odkazovat z více toocentralize výstrahy a skupiny vaší kanály oznámení.</span><span class="sxs-lookup"><span data-stu-id="075fb-134">hello receivers can be referenced from multiple alerts toocentralize and group your notification channels.</span></span> <span data-ttu-id="075fb-135">Když definujete výstrahu protokolu aktivit, máte dvě možnosti.</span><span class="sxs-lookup"><span data-stu-id="075fb-135">When you define your activity log alert, you have two options.</span></span> <span data-ttu-id="075fb-136">Můžete:</span><span class="sxs-lookup"><span data-stu-id="075fb-136">You can:</span></span>

* <span data-ttu-id="075fb-137">Použijte existující skupinu akce v protokolu upozornění aktivity.</span><span class="sxs-lookup"><span data-stu-id="075fb-137">Use an existing action group in your activity log alert.</span></span> 
* <span data-ttu-id="075fb-138">Vytvořte novou skupinu akce.</span><span class="sxs-lookup"><span data-stu-id="075fb-138">Create a new action group.</span></span> 

<span data-ttu-id="075fb-139">toolearn Další informace o akci skupiny, najdete v části [vytvořit a spravovat skupiny akce v hello portál Azure](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="075fb-139">toolearn more about action groups, see [Create and manage action groups in hello Azure portal](monitoring-action-groups.md).</span></span>

<span data-ttu-id="075fb-140">toolearn Další informace o oznámení o stavu služby, najdete v části [výstrahy v protokolu aktivit na oznámení o stavu služby](monitoring-activity-log-alerts-on-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="075fb-140">toolearn more about service health notifications, see [Receive activity log alerts on service health notifications](monitoring-activity-log-alerts-on-service-notifications.md).</span></span>

## <a name="create-an-alert-on-an-activity-log-event-with-a-new-action-group-by-using-hello-azure-portal"></a><span data-ttu-id="075fb-141">Vytvořit výstrahu na aktivity protokolu události s novou skupinu akce pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="075fb-141">Create an alert on an activity log event with a new action group by using hello Azure portal</span></span>
1. <span data-ttu-id="075fb-142">V hello [portál](https://portal.azure.com), vyberte **monitorování**.</span><span class="sxs-lookup"><span data-stu-id="075fb-142">In hello [portal](https://portal.azure.com), select **Monitor**.</span></span>

    ![Hello "Sledování" service](./media/monitoring-activity-log-alerts/home-monitor.png)
2. <span data-ttu-id="075fb-144">V hello **protokol aktivit** vyberte **výstrahy**.</span><span class="sxs-lookup"><span data-stu-id="075fb-144">In hello **Activity log** section, select **Alerts**.</span></span>

    ![Karta "Oznámení" Hello](./media/monitoring-activity-log-alerts/alerts-blades.png)
3. <span data-ttu-id="075fb-146">Vyberte **přidat aktivitu protokolu upozornění**a vyplňte pole hello.</span><span class="sxs-lookup"><span data-stu-id="075fb-146">Select **Add activity log alert**, and fill in hello fields.</span></span>

4. <span data-ttu-id="075fb-147">Zadejte název v hello **výstrahy název aktivity protokolu** a vyberte **popis**.</span><span class="sxs-lookup"><span data-stu-id="075fb-147">Enter a name in hello **Activity log alert name** box, and select a **Description**.</span></span>

    ![Hello příkaz "Přidat aktivitu protokolu upozornění"](./media/monitoring-activity-log-alerts/add-activity-log-alert.png)

5. <span data-ttu-id="075fb-149">Hello **předplatné** pole autofills s vaším aktuálním předplatným.</span><span class="sxs-lookup"><span data-stu-id="075fb-149">hello **Subscription** box autofills with your current subscription.</span></span> <span data-ttu-id="075fb-150">Toto předplatné je hello, jeden v které skupinu akce hello je uložit.</span><span class="sxs-lookup"><span data-stu-id="075fb-150">This subscription is hello one in which hello action group is saved.</span></span> <span data-ttu-id="075fb-151">výstrahy prostředek Hello je nasazené toothis předplatného a monitorování aktivity protokolu události z něj.</span><span class="sxs-lookup"><span data-stu-id="075fb-151">hello alert resource is deployed toothis subscription and monitors activity log events from it.</span></span>

    ![Hello "Přidat aktivitu protokolu výstraha" dialogové okno](./media/monitoring-activity-log-alerts/activity-log-alert-new-action-group.png)

6. <span data-ttu-id="075fb-153">Vyberte hello **skupiny prostředků** v které hello výstrahy prostředků vytvořili.</span><span class="sxs-lookup"><span data-stu-id="075fb-153">Select hello **Resource group** in which hello alert resource is created.</span></span> <span data-ttu-id="075fb-154">Toto není hello skupinu prostředků, který je monitorován hello výstraha.</span><span class="sxs-lookup"><span data-stu-id="075fb-154">This is not hello resource group that's monitored by hello alert.</span></span> <span data-ttu-id="075fb-155">Místo toho je skupina prostředků hello, kde je umístěný prostředek výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="075fb-155">Instead, it's hello resource group where hello alert resource is located.</span></span>

7. <span data-ttu-id="075fb-156">Volitelně vyberte **kategorie události** toomodify hello další filtry, které jsou zobrazeny.</span><span class="sxs-lookup"><span data-stu-id="075fb-156">Optionally, select an **Event category** toomodify hello additional filters that are shown.</span></span> <span data-ttu-id="075fb-157">Pro správu události hello filtry zahrnují **skupiny prostředků**, **prostředků**, **typ prostředku**, **název operace**, **Úroveň**, **stav**, a **iniciovaná událostí**.</span><span class="sxs-lookup"><span data-stu-id="075fb-157">For Administrative events, hello filters include **Resource group**, **Resource**, **Resource type**, **Operation name**, **Level**, **Status**, and **Event initiated by**.</span></span> <span data-ttu-id="075fb-158">Tyto hodnoty identifikovat události, které by měly monitorovat této výstrahy.</span><span class="sxs-lookup"><span data-stu-id="075fb-158">These values identify which events this alert should monitor.</span></span>

    >[!NOTE]
    ><span data-ttu-id="075fb-159">Musíte zadat alespoň jeden Dobrý den předcházející kritéria v upozornění.</span><span class="sxs-lookup"><span data-stu-id="075fb-159">You must specify at least one of hello preceding criteria in your alert.</span></span> <span data-ttu-id="075fb-160">Nelze vytvořit výstrahu, která aktivuje pokaždé, když dojde k vytvoření události v protokolech aktivity hello.</span><span class="sxs-lookup"><span data-stu-id="075fb-160">You may not create an alert that activates every time an event is created in hello activity logs.</span></span>
    >
    >

8. <span data-ttu-id="075fb-161">Zadejte název v hello **název skupiny akce** pole a zadejte název v hello **krátký název** pole.</span><span class="sxs-lookup"><span data-stu-id="075fb-161">Enter a name in hello **Action group name** box, and enter a name in hello **Short name** box.</span></span> <span data-ttu-id="075fb-162">krátký název Hello je použít místo názvu skupiny úplné akce při odesílání oznámení pomocí této skupiny.</span><span class="sxs-lookup"><span data-stu-id="075fb-162">hello short name is used in place of a full action group name when notifications are sent using this group.</span></span>

9.  <span data-ttu-id="075fb-163">Definujte seznam akcí, tím, že poskytuje hello akce:</span><span class="sxs-lookup"><span data-stu-id="075fb-163">Define a list of actions by providing hello action’s:</span></span>

    <span data-ttu-id="075fb-164">a.</span><span class="sxs-lookup"><span data-stu-id="075fb-164">a.</span></span> <span data-ttu-id="075fb-165">**Název**: Zadejte název, alias nebo identifikátor hello akce.</span><span class="sxs-lookup"><span data-stu-id="075fb-165">**Name**: Enter hello action’s name, alias, or identifier.</span></span>

    <span data-ttu-id="075fb-166">b.</span><span class="sxs-lookup"><span data-stu-id="075fb-166">b.</span></span> <span data-ttu-id="075fb-167">**Typ akce**: Vyberte SMS, e-mailu nebo webhooku.</span><span class="sxs-lookup"><span data-stu-id="075fb-167">**Action Type**: Select SMS, email, or webhook.</span></span>

    <span data-ttu-id="075fb-168">c.</span><span class="sxs-lookup"><span data-stu-id="075fb-168">c.</span></span> <span data-ttu-id="075fb-169">**Podrobnosti o**: založený na typu akce hello, zadejte telefonní číslo, e-mailovou adresu nebo webhooku identifikátor URI.</span><span class="sxs-lookup"><span data-stu-id="075fb-169">**Details**: Based on hello action type, enter a phone number, email address, or webhook URI.</span></span>

10. <span data-ttu-id="075fb-170">Vyberte **OK** toocreate hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="075fb-170">Select **OK** toocreate hello alert.</span></span>

<span data-ttu-id="075fb-171">Výstraha Hello trvá několik minut toofully rozšířit a pak se zobrazí jako aktivní.</span><span class="sxs-lookup"><span data-stu-id="075fb-171">hello alert takes a few minutes toofully propagate and then become active.</span></span> <span data-ttu-id="075fb-172">Aktivuje při nové události kritériím hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="075fb-172">It triggers when new events match hello alert's criteria.</span></span>

<span data-ttu-id="075fb-173">Další informace najdete v tématu [Rady pro pochopení hello webhooku schématu použít ve výstrahách aktivity protokolu](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="075fb-173">For more information, see [Understand hello webhook schema used in activity log alerts](monitoring-activity-log-alerts-webhook.md).</span></span>

>[!NOTE]
><span data-ttu-id="075fb-174">Skupina akce Hello definované v těchto kroků je opakovaně použitelné jako stávající skupina akce pro všechny budoucí výstrahy definice.</span><span class="sxs-lookup"><span data-stu-id="075fb-174">hello action group defined in these steps is reusable as an existing action group for all future alert definitions.</span></span>
>
>

## <a name="create-an-alert-on-an-activity-log-event-for-an-existing-action-group-by-using-hello-azure-portal"></a><span data-ttu-id="075fb-175">Vytvořit výstrahu na aktivity protokolu události pro stávající skupinu akce pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="075fb-175">Create an alert on an activity log event for an existing action group by using hello Azure portal</span></span>
1. <span data-ttu-id="075fb-176">Postupujte podle kroků 1 až 7 v předchozí části toocreate hello výstrahu protokolu aktivit.</span><span class="sxs-lookup"><span data-stu-id="075fb-176">Follow steps 1 through 7 in hello previous section toocreate your activity log alert.</span></span>

2. <span data-ttu-id="075fb-177">V části **oznámit prostřednictvím**, vyberte hello **existující** tlačítko akce skupiny.</span><span class="sxs-lookup"><span data-stu-id="075fb-177">Under **Notify via**, select hello **Existing** action group button.</span></span> <span data-ttu-id="075fb-178">Vyberte existující skupinu akce hello seznamu.</span><span class="sxs-lookup"><span data-stu-id="075fb-178">Select an existing action group from hello list.</span></span>

3. <span data-ttu-id="075fb-179">Vyberte **OK** toocreate hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="075fb-179">Select **OK** toocreate hello alert.</span></span>

<span data-ttu-id="075fb-180">Výstraha Hello trvá několik minut toofully rozšířit a pak se zobrazí jako aktivní.</span><span class="sxs-lookup"><span data-stu-id="075fb-180">hello alert takes a few minutes toofully propagate and then become active.</span></span> <span data-ttu-id="075fb-181">Aktivuje při nové události kritériím hello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="075fb-181">It triggers when new events match hello alert's criteria.</span></span>

## <a name="manage-your-alerts"></a><span data-ttu-id="075fb-182">Spravovat oznámení</span><span class="sxs-lookup"><span data-stu-id="075fb-182">Manage your alerts</span></span>

<span data-ttu-id="075fb-183">Po vytvoření výstrahy, je viditelný v části výstrahy hello hello monitorování okna.</span><span class="sxs-lookup"><span data-stu-id="075fb-183">After you create an alert, it's visible in hello Alerts section of hello Monitor blade.</span></span> <span data-ttu-id="075fb-184">Vyberte hello oznámení, které chcete toomanage na:</span><span class="sxs-lookup"><span data-stu-id="075fb-184">Select hello alert you want toomanage to:</span></span>

* <span data-ttu-id="075fb-185">Upravte.</span><span class="sxs-lookup"><span data-stu-id="075fb-185">Edit it.</span></span>
* <span data-ttu-id="075fb-186">Odstraňte ji.</span><span class="sxs-lookup"><span data-stu-id="075fb-186">Delete it.</span></span>
* <span data-ttu-id="075fb-187">Zakázat nebo povolit, pokud chcete zastavit tootemporarily nebo obnovit příjem oznámení pro výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="075fb-187">Disable or enable it, if you want tootemporarily stop or resume receiving notifications for hello alert.</span></span>

## <a name="next-steps"></a><span data-ttu-id="075fb-188">Další kroky</span><span class="sxs-lookup"><span data-stu-id="075fb-188">Next steps</span></span>
- <span data-ttu-id="075fb-189">Získat [přehled výstrah](monitoring-overview-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="075fb-189">Get an [overview of alerts](monitoring-overview-alerts.md).</span></span>
- <span data-ttu-id="075fb-190">Další informace o [omezení rychlosti oznámení](monitoring-alerts-rate-limiting.md).</span><span class="sxs-lookup"><span data-stu-id="075fb-190">Learn about [notification rate limiting](monitoring-alerts-rate-limiting.md).</span></span>
- <span data-ttu-id="075fb-191">Zkontrolujte hello [schéma výstrahy webhooku protokolu činnosti](monitoring-activity-log-alerts-webhook.md).</span><span class="sxs-lookup"><span data-stu-id="075fb-191">Review hello [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md).</span></span>
- <span data-ttu-id="075fb-192">Další informace o [skupiny akcí](monitoring-action-groups.md).</span><span class="sxs-lookup"><span data-stu-id="075fb-192">Learn more about [action groups](monitoring-action-groups.md).</span></span>  
- <span data-ttu-id="075fb-193">Další informace o [oznámení o stavu služby](monitoring-service-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="075fb-193">Learn about [service health notifications](monitoring-service-notifications.md).</span></span>
- <span data-ttu-id="075fb-194">Vytvoření [aktivity protokolu výstrahy toomonitor všechny operace škálování modul vaše předplatné](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span><span class="sxs-lookup"><span data-stu-id="075fb-194">Create an [activity log alert toomonitor all autoscale engine operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-alert).</span></span>
- <span data-ttu-id="075fb-195">Vytvoření [aktivity protokolu výstrahy toomonitor všechny operace škálování nebo škálovatelnou neúspěšné automatické škálování na vaše předplatné](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span><span class="sxs-lookup"><span data-stu-id="075fb-195">Create an [activity log alert toomonitor all failed autoscale scale-in/scale-out operations on your subscription](https://github.com/Azure/azure-quickstart-templates/tree/master/monitor-autoscale-failed-alert).</span></span>
