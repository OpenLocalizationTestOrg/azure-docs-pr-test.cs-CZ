---
title: "Azure Privileged Identity Management schválení pracovních | Microsoft Docs"
description: "Další informace o schválení pracovních postupů v Privileged Identity Management (PIM)"
services: active-directory
documentationcenter: 
author: barclayn
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: barclayn
ms.custom: pim
ms.openlocfilehash: cf6a9213fa0a1cba8725aabb42abe51b805ece7a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="approvals-preview"></a><span data-ttu-id="42026-103">Schválení (Preview)</span><span class="sxs-lookup"><span data-stu-id="42026-103">Approvals (Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="42026-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="42026-104">Overview</span></span>

<span data-ttu-id="42026-105">Schválení Privileged Identity Management můžete nakonfigurovat role za účelem vyžadovat schválení pro aktivaci a zvolte jednu nebo víc uživatelů nebo skupin jako delegovaný schvalovatelů.</span><span class="sxs-lookup"><span data-stu-id="42026-105">With Approvals for Privileged Identity Management, you can configure roles to require approval for activation, and choose one or multiple users or groups as delegated approvers.</span></span> <span data-ttu-id="42026-106">Zachovat čtení Další informace o konfiguraci rolí a vyberte schvalovatelů.</span><span class="sxs-lookup"><span data-stu-id="42026-106">Keep reading to learn how to configure roles and select approvers.</span></span>

>[!NOTE]
<span data-ttu-id="42026-107">Mějte prosím na paměti, že tato funkce je stále ve vývoji a pravděpodobně narazíte na chyby.</span><span class="sxs-lookup"><span data-stu-id="42026-107">Please keep in mind this feature is still in development, and you may encounter bugs.</span></span> <span data-ttu-id="42026-108">Funkce, včetně textu a konvence vytváření názvů se mohou změnit a by se neměla považovat za poslední.</span><span class="sxs-lookup"><span data-stu-id="42026-108">The functionality, including text and naming conventions are subject to change, and should not be considered final.</span></span>


## <a name="key-terminology"></a><span data-ttu-id="42026-109">Terminologie klíče</span><span class="sxs-lookup"><span data-stu-id="42026-109">Key Terminology</span></span>

<span data-ttu-id="42026-110">*Oprávněný uživatel Role* – oprávněné role uživatel je uživatelem v rámci vaší organizace, který je přiřazený k roli služby Azure AD jako způsobilých (role vyžaduje aktivaci).</span><span class="sxs-lookup"><span data-stu-id="42026-110">*Eligible Role User* – An eligible role user is a user within your organization that’s been assigned to an Azure AD role as eligible (role requires activation).</span></span>

<span data-ttu-id="42026-111">*Delegovaná schvalovatel* – delegované schvalovatel je jeden nebo více jednotlivce nebo skupiny v rámci služby Azure AD, kteří jsou zodpovědní za schválení žádosti o aktivaci role.</span><span class="sxs-lookup"><span data-stu-id="42026-111">*Delegated Approver* – A delegated approver is one or multiple individuals or groups within your Azure AD who are responsible for approving requests for role activation.</span></span>

## <a name="scenarios"></a><span data-ttu-id="42026-112">Scénáře</span><span class="sxs-lookup"><span data-stu-id="42026-112">Scenarios</span></span>

<span data-ttu-id="42026-113">Privátní preview podporuje následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="42026-113">The private preview supports the following scenarios:</span></span>

<span data-ttu-id="42026-114">**Jako privilegované Role správce (PRA) můžete:**</span><span class="sxs-lookup"><span data-stu-id="42026-114">**As a Privileged Role Administrator (PRA) you can:**</span></span>

-   [<span data-ttu-id="42026-115">Povolit schválení pro konkrétní role</span><span class="sxs-lookup"><span data-stu-id="42026-115">enable approval for specific roles</span></span>](#enable-approval-for-specific-roles)

-   [<span data-ttu-id="42026-116">Zadejte schvalovatel uživatelé nebo skupiny ke schválení žádostí</span><span class="sxs-lookup"><span data-stu-id="42026-116">specify approver users and/or groups to approve requests</span></span>](#specify-approver-users-and/or-groups-to-approve-requests)

-   [<span data-ttu-id="42026-117">Zobrazit historii požadavku a schválení všech privilegovaných rolí</span><span class="sxs-lookup"><span data-stu-id="42026-117">view request and approval history for all privileged roles</span></span>](#view-request-and-approval-history-for-all-privileged-roles)

<span data-ttu-id="42026-118">**Jako určeného schvalovatele můžete:**</span><span class="sxs-lookup"><span data-stu-id="42026-118">**As a designated approver, you can:**</span></span>

-   [<span data-ttu-id="42026-119">Zobrazit čeká na schválení (počet požadavků)</span><span class="sxs-lookup"><span data-stu-id="42026-119">view pending approvals (requests)</span></span>](#view-pending-approvals-requests)

-   [<span data-ttu-id="42026-120">Schválit nebo odmítnout žádosti o zvýšení oprávnění role (jeden nebo hromadně)</span><span class="sxs-lookup"><span data-stu-id="42026-120">approve or reject requests for role elevation (single and/or bulk)</span></span>](#approve-or-reject-requests-for-role-elevation-single-and/or-bulk)

-   [<span data-ttu-id="42026-121">uveďte její odůvodnění moje schválení nebo zamítnutí</span><span class="sxs-lookup"><span data-stu-id="42026-121">provide justification for my approval/rejection</span></span>](#provide-justification-for-my-approval/rejection) 

<span data-ttu-id="42026-122">**Jako oprávněný uživatel roli můžete:**</span><span class="sxs-lookup"><span data-stu-id="42026-122">**As an Eligible Role User you can:**</span></span>

-   [<span data-ttu-id="42026-123">žádost o aktivaci role, který vyžaduje schválení</span><span class="sxs-lookup"><span data-stu-id="42026-123">request activation of a role that requires approval</span></span>](#request-activation-of-a-role-that-requires-approval)

-   [<span data-ttu-id="42026-124">Zobrazení stavu vaši žádost o aktivaci</span><span class="sxs-lookup"><span data-stu-id="42026-124">view the status of your request to activate</span></span>](#view-the-status-of-your-request-to-activate)

-   [<span data-ttu-id="42026-125">dokončení úkolu ve službě Azure AD, pokud byla schválena aktivace</span><span class="sxs-lookup"><span data-stu-id="42026-125">complete your task in Azure AD if activation was approved</span></span>](#complete-your-task-in-azure-ad-if-activation-was-approved)

### <a name="navigation"></a><span data-ttu-id="42026-126">Navigace</span><span class="sxs-lookup"><span data-stu-id="42026-126">Navigation</span></span>

<span data-ttu-id="42026-127">Aktualizovali jsme navigace pro podporu schválení</span><span class="sxs-lookup"><span data-stu-id="42026-127">We've updated the navigation to support approvals</span></span>

![](media/azure-ad-pim-approval-workflow/image001.png)

<span data-ttu-id="42026-128">Výchozí úvodní stránka poskytuje pohodlné přístup k informacím o PIM a nová dokumentace schválení.</span><span class="sxs-lookup"><span data-stu-id="42026-128">The default landing page provides convenient access to information about PIM and the new approvals documentation.</span></span>

![](media/azure-ad-pim-approval-workflow/image002.png)

<span data-ttu-id="42026-129">Přidali jsme také novou část pro všechny uživatele PIM, historie Moje auditu.</span><span class="sxs-lookup"><span data-stu-id="42026-129">We’ve also added a new section for all users of PIM, ‘My Audit History’.</span></span> <span data-ttu-id="42026-130">Tady najdete všechny informace relevantní pro vaši identitu.</span><span class="sxs-lookup"><span data-stu-id="42026-130">Here you can find all the information relevant to your identity.</span></span> <span data-ttu-id="42026-131">To zahrnuje všechny vaše požadavky na vyřízení a dokončený, všechny rozhodnutí, které jste udělali týkající se požadavků, které můžete vyřešit a všechny vaše posledních aktivace role z jednoho místa.</span><span class="sxs-lookup"><span data-stu-id="42026-131">This includes all your pending and completed requests, any decisions you’ve made about the requests you resolve, and all your past role activations in one convenient location.</span></span>

![](media/azure-ad-pim-approval-workflow/image003.png)

### <a name="enable-approval-for-specific-roles"></a><span data-ttu-id="42026-132">Povolit schválení pro konkrétní role</span><span class="sxs-lookup"><span data-stu-id="42026-132">Enable approval for specific roles</span></span>

<span data-ttu-id="42026-133">Pokud chcete povolit schválení pro určité role, nejprve vyberte role Directory z levé navigaci.</span><span class="sxs-lookup"><span data-stu-id="42026-133">To enable approval for a specific role, first select Directory Roles from the left navigation.</span></span>

![](media/azure-ad-pim-approval-workflow/image004.png)

<span data-ttu-id="42026-134">Najděte a vyberte nastavení v levé navigaci Directory role</span><span class="sxs-lookup"><span data-stu-id="42026-134">Find and select settings in the Directory Roles left navigation</span></span>

![](media/azure-ad-pim-approval-workflow/image006.png)

<span data-ttu-id="42026-135">Vyberte privilegované role:</span><span class="sxs-lookup"><span data-stu-id="42026-135">Select privileged Roles:</span></span>

![](media/azure-ad-pim-approval-workflow/image009.png)

<span data-ttu-id="42026-136">Vyberte možnost "Povolit" v vyžadují části:</span><span class="sxs-lookup"><span data-stu-id="42026-136">Select “Enable” in the Require approval section:</span></span>

![](media/azure-ad-pim-approval-workflow/image011.png)

<span data-ttu-id="42026-137">Jakmile bude povoleno, v okně se rozbalí a zobrazí následující podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="42026-137">Once enabled, the blade will expand to show the following details:</span></span>

![](media/azure-ad-pim-approval-workflow/image013.png)

>[!NOTE]
<span data-ttu-id="42026-138">Pokud je nesmí zadat všechny schvalovatelů, stanou se PRA(s) schvalovatelům výchozí.</span><span class="sxs-lookup"><span data-stu-id="42026-138">If you DO NOT specify any approvers, the PRA(s) become the default approver(s).</span></span> <span data-ttu-id="42026-139">PRA(s) by byly zapotřebí k schválit všechny žádosti o aktivaci pro tuto roli.</span><span class="sxs-lookup"><span data-stu-id="42026-139">PRA(s) would be required to approve ALL activation requests for this role.</span></span>

### <a name="specify-approver-users-andor-groups-to-approve-requests"></a><span data-ttu-id="42026-140">Zadejte schvalovatel uživatelé nebo skupiny ke schválení žádostí</span><span class="sxs-lookup"><span data-stu-id="42026-140">Specify approver users and/or groups to approve requests</span></span>

<span data-ttu-id="42026-141">Delegovat schválení, klikněte na možnost "Vyberte schvalovatelů":</span><span class="sxs-lookup"><span data-stu-id="42026-141">To delegate approval, click the option to “Select approvers”:</span></span>

![](media/azure-ad-pim-approval-workflow/image015.png)

<span data-ttu-id="42026-142">Až se načte v okně vyberte schvalovatelů, pravděpodobně vyhledávání pro konkrétního uživatele nebo skupiny pomocí panelu Hledat v horní části, nebo výběrem ze seznamu předem vyplněná a pak klikněte na tlačítko "Vyberte" po dokončení:</span><span class="sxs-lookup"><span data-stu-id="42026-142">When the Select approvers blade loads, you may search for a specific user or group using the search bar at the top, or selecting from the pre-populated list, then click “Select” when finished:</span></span>

![](media/azure-ad-pim-approval-workflow/image017.png)

<span data-ttu-id="42026-143">Poznámka: Může vybrat více uživatelů nebo skupin současně.</span><span class="sxs-lookup"><span data-stu-id="42026-143">Note: You may select multiple users or groups at a time.</span></span>

<span data-ttu-id="42026-144">Výběr se objeví v seznamu vybraný schvalovatelů, jak vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="42026-144">Your selection will appear in the list of selected approvers as seen below:</span></span>

![](media/azure-ad-pim-approval-workflow/image019.png)

<span data-ttu-id="42026-145">Odebrání schvalovatele, jednoduše kliknutím na tlačítko Odebrat vedle jména.</span><span class="sxs-lookup"><span data-stu-id="42026-145">To remove an approver, simply click the Remove button next to their name.</span></span>

<span data-ttu-id="42026-146">Pokud chcete přidat další schvalovatele, proces opakujte.</span><span class="sxs-lookup"><span data-stu-id="42026-146">To add additional approvers, repeat the process.</span></span>

## <a name="view-request-and-approval-history-for-all-privileged-roles"></a><span data-ttu-id="42026-147">Zobrazit historii požadavku a schválení všech privilegovaných rolí</span><span class="sxs-lookup"><span data-stu-id="42026-147">View request and approval history for all privileged roles</span></span>

<span data-ttu-id="42026-148">Chcete-li zobrazit historii požadavku a schválení všech privilegovaných rolí, vyberte historie auditu na řídicím panelu:</span><span class="sxs-lookup"><span data-stu-id="42026-148">To view request and approval history for all privileged roles, select Audit History from the dashboard:</span></span>

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
<span data-ttu-id="42026-149">Můžete řadit data akcí a vyhledejte "Aktivace schválené"</span><span class="sxs-lookup"><span data-stu-id="42026-149">You can sort the data by Action, and look for “Activation Approved”</span></span>

### <a name="view-pending-approvals-requests"></a><span data-ttu-id="42026-150">Zobrazit čeká na schválení (počet požadavků)</span><span class="sxs-lookup"><span data-stu-id="42026-150">View pending approvals (requests)</span></span>

<span data-ttu-id="42026-151">Jako delegovaný schvalovatel obdržíte e-mailová oznámení při žádost čeká na schválení.</span><span class="sxs-lookup"><span data-stu-id="42026-151">As a delegated approver, you’ll receive email notifications when a request is pending your approval.</span></span> <span data-ttu-id="42026-152">Chcete-li zobrazit tyto požadavky na portálu PIM, na řídicím panelu (v nové navigační) vyberte kartu "Čekající žádosti o schválení" v levém navigačním panelu.</span><span class="sxs-lookup"><span data-stu-id="42026-152">To view these requests in the PIM portal, from the dashboard (in the new navigation) select the “Pending Approval Requests” tab in the left navigation bar.</span></span>

![](media/azure-ad-pim-approval-workflow/image023.png)

<span data-ttu-id="42026-153">Z tohoto místa se zobrazí seznam žádosti čekající na schválení:</span><span class="sxs-lookup"><span data-stu-id="42026-153">From there, you’ll see a list of requests pending approval:</span></span>

![](media/azure-ad-pim-approval-workflow/image024.png)

### <a name="approve-or-reject-requests-for-role-elevation-single-andor-bulk"></a><span data-ttu-id="42026-154">Schválit nebo odmítnout žádosti o zvýšení oprávnění role (jeden nebo hromadně)</span><span class="sxs-lookup"><span data-stu-id="42026-154">Approve or reject requests for role elevation (single and/or bulk)</span></span>

<span data-ttu-id="42026-155">Vyberte požadavky, které chcete schválit nebo zamítnout a klikněte na tlačítko na panelu akcí, která odpovídá vaše rozhodnutí:</span><span class="sxs-lookup"><span data-stu-id="42026-155">Select the requests you wish to approve or deny, and click the button in the action bar that corresponds with your decision:</span></span>

![](media/azure-ad-pim-approval-workflow/image025.png)

### <a name="provide-justification-for-my-approvalrejection"></a><span data-ttu-id="42026-156">Uveďte její odůvodnění moje schválení nebo zamítnutí</span><span class="sxs-lookup"><span data-stu-id="42026-156">Provide justification for my approval/rejection</span></span>

<span data-ttu-id="42026-157">Otevře se nové okno schválí nebo zamítne více požadavků najednou.</span><span class="sxs-lookup"><span data-stu-id="42026-157">This will open a new blade to approve or deny multiple requests at once.</span></span> <span data-ttu-id="42026-158">Zadejte odůvodnění pro vaše rozhodnutí, a klikněte na tlačítko Schválit (nebo odepřít) v dolní části nebo v okně:</span><span class="sxs-lookup"><span data-stu-id="42026-158">Enter a justification for your decision, and click approve (or deny) at the bottom or the blade:</span></span>

![](media/azure-ad-pim-approval-workflow/image029.png)

<span data-ttu-id="42026-159">Po dokončení zpracování žádosti o symbolu stavu bude odrážet rozhodnutí, které jste nastavili (v tomto příkladu rozhodnutí je schválit):</span><span class="sxs-lookup"><span data-stu-id="42026-159">When the request process is complete, the status symbol will reflect the decision you made (in this example, the decision is approve):</span></span>

![](media/azure-ad-pim-approval-workflow/image031.png)

### <a name="request-activation-of-a-role-that-requires-approval"></a><span data-ttu-id="42026-160">Žádost o aktivaci role, který vyžaduje schválení</span><span class="sxs-lookup"><span data-stu-id="42026-160">Request activation of a role that requires approval</span></span>

<span data-ttu-id="42026-161">Žádají o aktivaci role, který vyžaduje schválení lze zahájit z původního navigační PIM nebo nové navigační jako proces pro aktivaci role zůstává stejná.</span><span class="sxs-lookup"><span data-stu-id="42026-161">Requesting activation of a role that requires approval may be initiated from either the old PIM navigation, or the new navigation, as the process for role activation remains the same.</span></span> <span data-ttu-id="42026-162">Jednoduše vyberte v seznamu rolí k aktivaci role:</span><span class="sxs-lookup"><span data-stu-id="42026-162">Simply select a role from the list of roles to activate:</span></span>

![](media/azure-ad-pim-approval-workflow/image033.png)

<span data-ttu-id="42026-163">Pokud privilegovaných rolí vyžaduje Vícefaktorové ověřování, budete vyzváni k dokončení této úlohy nejdřív:</span><span class="sxs-lookup"><span data-stu-id="42026-163">If a privileged role requires Multi-Factor Authentication, you’ll be prompted to complete that task first:</span></span>

![](media/azure-ad-pim-approval-workflow/image035.png)

<span data-ttu-id="42026-164">Po dokončení, kliknout na aktivovat a zadejte odůvodnění (v případě potřeby):</span><span class="sxs-lookup"><span data-stu-id="42026-164">Once complete, click Activate and provide a justification (if required):</span></span>

![](media/azure-ad-pim-approval-workflow/image037.png)

<span data-ttu-id="42026-165">Žadatel se zobrazí oznámení, že žádosti čekající na schválení:</span><span class="sxs-lookup"><span data-stu-id="42026-165">The requestor will see a notification that the request is pending approval:</span></span>

![](media/azure-ad-pim-approval-workflow/image039.png)

### <a name="view-the-status-of-your-request-to-activate"></a><span data-ttu-id="42026-166">Zobrazení stavu vaši žádost o aktivaci</span><span class="sxs-lookup"><span data-stu-id="42026-166">View the status of your request to activate</span></span>

<span data-ttu-id="42026-167">Zobrazení stavu čekající žádost o aktivaci, musí se přístup z nové navigace.</span><span class="sxs-lookup"><span data-stu-id="42026-167">Viewing the status of a pending request to activate must be accessed from the new navigation.</span></span> <span data-ttu-id="42026-168">V levém navigačním panelu vyberte kartu "Moje žádosti o":</span><span class="sxs-lookup"><span data-stu-id="42026-168">From the left navigation bar, select the “My Requests” tab:</span></span>

![](media/azure-ad-pim-approval-workflow/image041.png)

<span data-ttu-id="42026-169">Stav žádosti bude jako výchozí nastavena na "Čekající na vyřízení", ale můžete přepínat zobrazíte všechny nebo odepření požadavků.</span><span class="sxs-lookup"><span data-stu-id="42026-169">The request state defaults to “Pending”, but you can toggle to see all or denied requests.</span></span>

### <a name="complete-your-task-in-azure-ad-if-activation-was-approved"></a><span data-ttu-id="42026-170">Dokončení úkolu ve službě Azure AD, pokud byla schválena aktivace</span><span class="sxs-lookup"><span data-stu-id="42026-170">Complete your task in Azure AD if activation was approved</span></span>

<span data-ttu-id="42026-171">Po schválení žádosti role je aktivní a veškerou práci, kterou vyžaduje tato role může pokračovat.</span><span class="sxs-lookup"><span data-stu-id="42026-171">Once the request is approved, the role is active and you may proceed with any work that requires this role.</span></span>

![](media/azure-ad-pim-approval-workflow/image043.png)

## <a name="next-steps"></a><span data-ttu-id="42026-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="42026-172">Next steps</span></span>

<span data-ttu-id="42026-173">Je vhodné nám svůj názor.</span><span class="sxs-lookup"><span data-stu-id="42026-173">Your feedback is valuable to us.</span></span> <span data-ttu-id="42026-174">Prosím Nebojte se zde sdílet komentáře nebo zpětnou vazbu s námi!</span><span class="sxs-lookup"><span data-stu-id="42026-174">Please feel free to share comments or feedback with us here!</span></span>
