---
title: "pracovní postupy Privileged Identity Management schválení aaaAzure | Microsoft Docs"
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
ms.openlocfilehash: 4afaf5c138798a803eb3d3b7905b9361d65792cd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="approvals-preview"></a><span data-ttu-id="b12ae-103">Schválení (Preview)</span><span class="sxs-lookup"><span data-stu-id="b12ae-103">Approvals (Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="b12ae-104">Přehled</span><span class="sxs-lookup"><span data-stu-id="b12ae-104">Overview</span></span>

<span data-ttu-id="b12ae-105">Schválení Privileged Identity Management můžete nakonfigurovat role toorequire schválení pro aktivaci a vyberte jeden nebo více uživatelů nebo skupin jako delegovaný schvalovatelů.</span><span class="sxs-lookup"><span data-stu-id="b12ae-105">With Approvals for Privileged Identity Management, you can configure roles toorequire approval for activation, and choose one or multiple users or groups as delegated approvers.</span></span> <span data-ttu-id="b12ae-106">Zachovat čtení toolearn jak tooconfigure role a vyberte schvalovatelů.</span><span class="sxs-lookup"><span data-stu-id="b12ae-106">Keep reading toolearn how tooconfigure roles and select approvers.</span></span>

>[!NOTE]
<span data-ttu-id="b12ae-107">Mějte prosím na paměti, že tato funkce je stále ve vývoji a pravděpodobně narazíte na chyby.</span><span class="sxs-lookup"><span data-stu-id="b12ae-107">Please keep in mind this feature is still in development, and you may encounter bugs.</span></span> <span data-ttu-id="b12ae-108">Hello funkce, včetně textu a konvence vytváření názvů se mohou změnit a by se neměla považovat za poslední.</span><span class="sxs-lookup"><span data-stu-id="b12ae-108">hello functionality, including text and naming conventions are subject to change, and should not be considered final.</span></span>


## <a name="key-terminology"></a><span data-ttu-id="b12ae-109">Terminologie klíče</span><span class="sxs-lookup"><span data-stu-id="b12ae-109">Key Terminology</span></span>

<span data-ttu-id="b12ae-110">*Oprávněný uživatel Role* – oprávněné role uživatel je uživatelem v rámci vaší organizace, která byla přiřazena role tooan Azure AD jako způsobilých (role vyžaduje aktivaci).</span><span class="sxs-lookup"><span data-stu-id="b12ae-110">*Eligible Role User* – An eligible role user is a user within your organization that’s been assigned tooan Azure AD role as eligible (role requires activation).</span></span>

<span data-ttu-id="b12ae-111">*Delegovaná schvalovatel* – delegované schvalovatel je jeden nebo více jednotlivce nebo skupiny v rámci služby Azure AD, kteří jsou zodpovědní za schválení žádosti o aktivaci role.</span><span class="sxs-lookup"><span data-stu-id="b12ae-111">*Delegated Approver* – A delegated approver is one or multiple individuals or groups within your Azure AD who are responsible for approving requests for role activation.</span></span>

## <a name="scenarios"></a><span data-ttu-id="b12ae-112">Scénáře</span><span class="sxs-lookup"><span data-stu-id="b12ae-112">Scenarios</span></span>

<span data-ttu-id="b12ae-113">privátní Preview verzi Hello podporuje hello následující scénáře:</span><span class="sxs-lookup"><span data-stu-id="b12ae-113">hello private preview supports hello following scenarios:</span></span>

<span data-ttu-id="b12ae-114">**Jako privilegované Role správce (PRA) můžete:**</span><span class="sxs-lookup"><span data-stu-id="b12ae-114">**As a Privileged Role Administrator (PRA) you can:**</span></span>

-   [<span data-ttu-id="b12ae-115">Povolit schválení pro konkrétní role</span><span class="sxs-lookup"><span data-stu-id="b12ae-115">enable approval for specific roles</span></span>](#enable-approval-for-specific-roles)

-   [<span data-ttu-id="b12ae-116">Zadejte schvalovatel uživatelů nebo skupin tooapprove požadavků</span><span class="sxs-lookup"><span data-stu-id="b12ae-116">specify approver users and/or groups tooapprove requests</span></span>](#specify-approver-users-and/or-groups-to-approve-requests)

-   [<span data-ttu-id="b12ae-117">Zobrazit historii požadavku a schválení všech privilegovaných rolí</span><span class="sxs-lookup"><span data-stu-id="b12ae-117">view request and approval history for all privileged roles</span></span>](#view-request-and-approval-history-for-all-privileged-roles)

<span data-ttu-id="b12ae-118">**Jako určeného schvalovatele můžete:**</span><span class="sxs-lookup"><span data-stu-id="b12ae-118">**As a designated approver, you can:**</span></span>

-   [<span data-ttu-id="b12ae-119">Zobrazit čeká na schválení (počet požadavků)</span><span class="sxs-lookup"><span data-stu-id="b12ae-119">view pending approvals (requests)</span></span>](#view-pending-approvals-requests)

-   [<span data-ttu-id="b12ae-120">Schválit nebo odmítnout žádosti o zvýšení oprávnění role (jeden nebo hromadně)</span><span class="sxs-lookup"><span data-stu-id="b12ae-120">approve or reject requests for role elevation (single and/or bulk)</span></span>](#approve-or-reject-requests-for-role-elevation-single-and/or-bulk)

-   [<span data-ttu-id="b12ae-121">uveďte její odůvodnění moje schválení nebo zamítnutí</span><span class="sxs-lookup"><span data-stu-id="b12ae-121">provide justification for my approval/rejection</span></span>](#provide-justification-for-my-approval/rejection) 

<span data-ttu-id="b12ae-122">**Jako oprávněný uživatel roli můžete:**</span><span class="sxs-lookup"><span data-stu-id="b12ae-122">**As an Eligible Role User you can:**</span></span>

-   [<span data-ttu-id="b12ae-123">žádost o aktivaci role, který vyžaduje schválení</span><span class="sxs-lookup"><span data-stu-id="b12ae-123">request activation of a role that requires approval</span></span>](#request-activation-of-a-role-that-requires-approval)

-   [<span data-ttu-id="b12ae-124">Zobrazit stav hello tooactivate vaší žádosti</span><span class="sxs-lookup"><span data-stu-id="b12ae-124">view hello status of your request tooactivate</span></span>](#view-the-status-of-your-request-to-activate)

-   [<span data-ttu-id="b12ae-125">dokončení úkolu ve službě Azure AD, pokud byla schválena aktivace</span><span class="sxs-lookup"><span data-stu-id="b12ae-125">complete your task in Azure AD if activation was approved</span></span>](#complete-your-task-in-azure-ad-if-activation-was-approved)

### <a name="navigation"></a><span data-ttu-id="b12ae-126">Navigace</span><span class="sxs-lookup"><span data-stu-id="b12ae-126">Navigation</span></span>

<span data-ttu-id="b12ae-127">Aktualizovali jsme hello navigační toosupport schválení</span><span class="sxs-lookup"><span data-stu-id="b12ae-127">We've updated hello navigation toosupport approvals</span></span>

![](media/azure-ad-pim-approval-workflow/image001.png)

<span data-ttu-id="b12ae-128">Hello výchozí cílová stránka poskytuje pohodlné přístup tooinformation o PIM a hello novou dokumentaci schválení.</span><span class="sxs-lookup"><span data-stu-id="b12ae-128">hello default landing page provides convenient access tooinformation about PIM and hello new approvals documentation.</span></span>

![](media/azure-ad-pim-approval-workflow/image002.png)

<span data-ttu-id="b12ae-129">Přidali jsme také novou část pro všechny uživatele PIM, historie Moje auditu.</span><span class="sxs-lookup"><span data-stu-id="b12ae-129">We’ve also added a new section for all users of PIM, ‘My Audit History’.</span></span> <span data-ttu-id="b12ae-130">Zde můžete najít všechny hello informace relevantní tooyour identity.</span><span class="sxs-lookup"><span data-stu-id="b12ae-130">Here you can find all hello information relevant tooyour identity.</span></span> <span data-ttu-id="b12ae-131">To zahrnuje všechny vaše požadavky na vyřízení a dokončený, všechny rozhodnutí, které jste udělali o hello požadavků, které vyřešíte a všechny vaše posledních aktivace role z jednoho místa.</span><span class="sxs-lookup"><span data-stu-id="b12ae-131">This includes all your pending and completed requests, any decisions you’ve made about hello requests you resolve, and all your past role activations in one convenient location.</span></span>

![](media/azure-ad-pim-approval-workflow/image003.png)

### <a name="enable-approval-for-specific-roles"></a><span data-ttu-id="b12ae-132">Povolit schválení pro konkrétní role</span><span class="sxs-lookup"><span data-stu-id="b12ae-132">Enable approval for specific roles</span></span>

<span data-ttu-id="b12ae-133">tooenable schválení pro určité role, nejprve vyberte adresář role z levého navigačního hello.</span><span class="sxs-lookup"><span data-stu-id="b12ae-133">tooenable approval for a specific role, first select Directory Roles from hello left navigation.</span></span>

![](media/azure-ad-pim-approval-workflow/image004.png)

<span data-ttu-id="b12ae-134">Najděte a vyberte nastavení v levém navigačním role Directory hello</span><span class="sxs-lookup"><span data-stu-id="b12ae-134">Find and select settings in hello Directory Roles left navigation</span></span>

![](media/azure-ad-pim-approval-workflow/image006.png)

<span data-ttu-id="b12ae-135">Vyberte privilegované role:</span><span class="sxs-lookup"><span data-stu-id="b12ae-135">Select privileged Roles:</span></span>

![](media/azure-ad-pim-approval-workflow/image009.png)

<span data-ttu-id="b12ae-136">Vyberte možnost "Povolit" v hello vyžadují části:</span><span class="sxs-lookup"><span data-stu-id="b12ae-136">Select “Enable” in hello Require approval section:</span></span>

![](media/azure-ad-pim-approval-workflow/image011.png)

<span data-ttu-id="b12ae-137">Po povolení bude rozbalte okno hello tooshow hello následující podrobnosti:</span><span class="sxs-lookup"><span data-stu-id="b12ae-137">Once enabled, hello blade will expand tooshow hello following details:</span></span>

![](media/azure-ad-pim-approval-workflow/image013.png)

>[!NOTE]
<span data-ttu-id="b12ae-138">Pokud je nesmí zadat všechny schvalovatelů, stanou se hello PRA(s) schvalujících výchozí hello.</span><span class="sxs-lookup"><span data-stu-id="b12ae-138">If you DO NOT specify any approvers, hello PRA(s) become hello default approver(s).</span></span> <span data-ttu-id="b12ae-139">PRA(s) by požadované tooapprove aktivace všechny požadavky pro tuto roli.</span><span class="sxs-lookup"><span data-stu-id="b12ae-139">PRA(s) would be required tooapprove ALL activation requests for this role.</span></span>

### <a name="specify-approver-users-andor-groups-tooapprove-requests"></a><span data-ttu-id="b12ae-140">Zadejte schvalovatel uživatelů nebo skupin tooapprove požadavků</span><span class="sxs-lookup"><span data-stu-id="b12ae-140">Specify approver users and/or groups tooapprove requests</span></span>

<span data-ttu-id="b12ae-141">schválení toodelegate, klikněte na možnost hello příliš "Vyberte schvalovatelů":</span><span class="sxs-lookup"><span data-stu-id="b12ae-141">toodelegate approval, click hello option too“Select approvers”:</span></span>

![](media/azure-ad-pim-approval-workflow/image015.png)

<span data-ttu-id="b12ae-142">Až okno Vyberte schvalovatelů hello načte, pravděpodobně vyhledávání pro konkrétního uživatele nebo skupiny pomocí hello panelu Hledat v horní části hello nebo výběrem ze seznamu předem vyplněná hello a pak klikněte na tlačítko "Vyberte" po dokončení:</span><span class="sxs-lookup"><span data-stu-id="b12ae-142">When hello Select approvers blade loads, you may search for a specific user or group using hello search bar at hello top, or selecting from hello pre-populated list, then click “Select” when finished:</span></span>

![](media/azure-ad-pim-approval-workflow/image017.png)

<span data-ttu-id="b12ae-143">Poznámka: Může vybrat více uživatelů nebo skupin současně.</span><span class="sxs-lookup"><span data-stu-id="b12ae-143">Note: You may select multiple users or groups at a time.</span></span>

<span data-ttu-id="b12ae-144">Výběr se zobrazí v seznamu hello vybrané schvalovatelů, jak vidíte níže:</span><span class="sxs-lookup"><span data-stu-id="b12ae-144">Your selection will appear in hello list of selected approvers as seen below:</span></span>

![](media/azure-ad-pim-approval-workflow/image019.png)

<span data-ttu-id="b12ae-145">tooremove schvalovatele, jednoduše klikněte hello odebrat tlačítko Další tootheir název.</span><span class="sxs-lookup"><span data-stu-id="b12ae-145">tooremove an approver, simply click hello Remove button next tootheir name.</span></span>

<span data-ttu-id="b12ae-146">tooadd další schvalovatele, proces opakování hello.</span><span class="sxs-lookup"><span data-stu-id="b12ae-146">tooadd additional approvers, repeat hello process.</span></span>

## <a name="view-request-and-approval-history-for-all-privileged-roles"></a><span data-ttu-id="b12ae-147">Zobrazit historii požadavku a schválení všech privilegovaných rolí</span><span class="sxs-lookup"><span data-stu-id="b12ae-147">View request and approval history for all privileged roles</span></span>

<span data-ttu-id="b12ae-148">Historie tooview požadavku a schválení všech privilegovaných rolí, vyberte historie auditu z řídicího panelu hello:</span><span class="sxs-lookup"><span data-stu-id="b12ae-148">tooview request and approval history for all privileged roles, select Audit History from hello dashboard:</span></span>

![](media/azure-ad-pim-approval-workflow/image021.png)

>[!NOTE]
<span data-ttu-id="b12ae-149">Můžete řadit data hello akcí a vyhledejte "Aktivace schválené"</span><span class="sxs-lookup"><span data-stu-id="b12ae-149">You can sort hello data by Action, and look for “Activation Approved”</span></span>

### <a name="view-pending-approvals-requests"></a><span data-ttu-id="b12ae-150">Zobrazit čeká na schválení (počet požadavků)</span><span class="sxs-lookup"><span data-stu-id="b12ae-150">View pending approvals (requests)</span></span>

<span data-ttu-id="b12ae-151">Jako delegovaný schvalovatel obdržíte e-mailová oznámení při žádost čeká na schválení.</span><span class="sxs-lookup"><span data-stu-id="b12ae-151">As a delegated approver, you’ll receive email notifications when a request is pending your approval.</span></span> <span data-ttu-id="b12ae-152">tooview tyto požadavky hello PIM portálu na kartě řídicí panel (v nové navigační hello) vyberte hello "čekající žádosti o schválení" hello levé navigační panel.</span><span class="sxs-lookup"><span data-stu-id="b12ae-152">tooview these requests in hello PIM portal, from the dashboard (in hello new navigation) select hello “Pending Approval Requests” tab in hello left navigation bar.</span></span>

![](media/azure-ad-pim-approval-workflow/image023.png)

<span data-ttu-id="b12ae-153">Z tohoto místa se zobrazí seznam žádosti čekající na schválení:</span><span class="sxs-lookup"><span data-stu-id="b12ae-153">From there, you’ll see a list of requests pending approval:</span></span>

![](media/azure-ad-pim-approval-workflow/image024.png)

### <a name="approve-or-reject-requests-for-role-elevation-single-andor-bulk"></a><span data-ttu-id="b12ae-154">Schválit nebo odmítnout žádosti o zvýšení oprávnění role (jeden nebo hromadně)</span><span class="sxs-lookup"><span data-stu-id="b12ae-154">Approve or reject requests for role elevation (single and/or bulk)</span></span>

<span data-ttu-id="b12ae-155">Vyberte hello požadavky chcete tooapprove nebo odepřít a klikněte na tlačítko hello na panelu akcí, která odpovídá vaše rozhodnutí:</span><span class="sxs-lookup"><span data-stu-id="b12ae-155">Select hello requests you wish tooapprove or deny, and click hello button in the action bar that corresponds with your decision:</span></span>

![](media/azure-ad-pim-approval-workflow/image025.png)

### <a name="provide-justification-for-my-approvalrejection"></a><span data-ttu-id="b12ae-156">Uveďte její odůvodnění moje schválení nebo zamítnutí</span><span class="sxs-lookup"><span data-stu-id="b12ae-156">Provide justification for my approval/rejection</span></span>

<span data-ttu-id="b12ae-157">To bude otevřete nové okno tooapprove nebo odepřít více požadavků najednou.</span><span class="sxs-lookup"><span data-stu-id="b12ae-157">This will open a new blade tooapprove or deny multiple requests at once.</span></span> <span data-ttu-id="b12ae-158">Zadejte odůvodnění pro vaše rozhodnutí, a klikněte na tlačítko Schválit (nebo odepřít) v dolní hello nebo hello okno:</span><span class="sxs-lookup"><span data-stu-id="b12ae-158">Enter a justification for your decision, and click approve (or deny) at hello bottom or hello blade:</span></span>

![](media/azure-ad-pim-approval-workflow/image029.png)

<span data-ttu-id="b12ae-159">Po dokončení zpracování žádosti o hello symbolu stavu hello bude odrážet rozhodnutí, které jste nastavili (v tomto příkladu rozhodnutí hello je schválit):</span><span class="sxs-lookup"><span data-stu-id="b12ae-159">When hello request process is complete, hello status symbol will reflect the decision you made (in this example, hello decision is approve):</span></span>

![](media/azure-ad-pim-approval-workflow/image031.png)

### <a name="request-activation-of-a-role-that-requires-approval"></a><span data-ttu-id="b12ae-160">Žádost o aktivaci role, který vyžaduje schválení</span><span class="sxs-lookup"><span data-stu-id="b12ae-160">Request activation of a role that requires approval</span></span>

<span data-ttu-id="b12ae-161">Žádají o aktivaci role, která vyžaduje schválení lze zahájit z původní navigační PIM hello nebo nové navigační hello, jako hello proces aktivace zůstane role hello stejné.</span><span class="sxs-lookup"><span data-stu-id="b12ae-161">Requesting activation of a role that requires approval may be initiated from either hello old PIM navigation, or hello new navigation, as hello process for role activation remains hello same.</span></span> <span data-ttu-id="b12ae-162">Jednoduše vyberte hello seznamu rolí k aktivaci role:</span><span class="sxs-lookup"><span data-stu-id="b12ae-162">Simply select a role from hello list of roles to activate:</span></span>

![](media/azure-ad-pim-approval-workflow/image033.png)

<span data-ttu-id="b12ae-163">Pokud privilegovaných rolí vyžaduje Vícefaktorové ověřování, budete vyzváni k dokončení této úlohy nejdřív:</span><span class="sxs-lookup"><span data-stu-id="b12ae-163">If a privileged role requires Multi-Factor Authentication, you’ll be prompted to complete that task first:</span></span>

![](media/azure-ad-pim-approval-workflow/image035.png)

<span data-ttu-id="b12ae-164">Po dokončení, kliknout na aktivovat a zadejte odůvodnění (v případě potřeby):</span><span class="sxs-lookup"><span data-stu-id="b12ae-164">Once complete, click Activate and provide a justification (if required):</span></span>

![](media/azure-ad-pim-approval-workflow/image037.png)

<span data-ttu-id="b12ae-165">žadatel Hello uvidí, že je oznámení, že hello žádosti čekající na schválení:</span><span class="sxs-lookup"><span data-stu-id="b12ae-165">hello requestor will see a notification that hello request is pending approval:</span></span>

![](media/azure-ad-pim-approval-workflow/image039.png)

### <a name="view-hello-status-of-your-request-tooactivate"></a><span data-ttu-id="b12ae-166">Zobrazení stavu hello tooactivate vaší žádosti</span><span class="sxs-lookup"><span data-stu-id="b12ae-166">View hello status of your request tooactivate</span></span>

<span data-ttu-id="b12ae-167">Zobrazení stavu hello tooactivate nevyřízenou žádost musí mít přístup nové navigace.</span><span class="sxs-lookup"><span data-stu-id="b12ae-167">Viewing hello status of a pending request tooactivate must be accessed from the new navigation.</span></span> <span data-ttu-id="b12ae-168">V levém navigačním panelu hello vyberte kartu "Moje žádosti o" hello:</span><span class="sxs-lookup"><span data-stu-id="b12ae-168">From hello left navigation bar, select hello “My Requests” tab:</span></span>

![](media/azure-ad-pim-approval-workflow/image041.png)

<span data-ttu-id="b12ae-169">stav žádosti Hello výchozí příliš "Čeká na vyřízení", ale můžete přepínat toosee všechny nebo odepření požadavků.</span><span class="sxs-lookup"><span data-stu-id="b12ae-169">hello request state defaults too“Pending”, but you can toggle toosee all or denied requests.</span></span>

### <a name="complete-your-task-in-azure-ad-if-activation-was-approved"></a><span data-ttu-id="b12ae-170">Dokončení úkolu ve službě Azure AD, pokud byla schválena aktivace</span><span class="sxs-lookup"><span data-stu-id="b12ae-170">Complete your task in Azure AD if activation was approved</span></span>

<span data-ttu-id="b12ae-171">Po schválení žádosti hello hello role je aktivní a veškerou práci, kterou vyžaduje tato role může pokračovat.</span><span class="sxs-lookup"><span data-stu-id="b12ae-171">Once hello request is approved, hello role is active and you may proceed with any work that requires this role.</span></span>

![](media/azure-ad-pim-approval-workflow/image043.png)

## <a name="next-steps"></a><span data-ttu-id="b12ae-172">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b12ae-172">Next steps</span></span>

<span data-ttu-id="b12ae-173">Váš názor se hodí v situaci toous.</span><span class="sxs-lookup"><span data-stu-id="b12ae-173">Your feedback is valuable toous.</span></span> <span data-ttu-id="b12ae-174">Můžete volné tooshare komentáře nebo zpětnou vazbu s námi zde!</span><span class="sxs-lookup"><span data-stu-id="b12ae-174">Please feel free tooshare comments or feedback with us here!</span></span>
