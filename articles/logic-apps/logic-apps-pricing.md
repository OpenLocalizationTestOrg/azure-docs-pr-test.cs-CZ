---
title: Ceny a fakturace - Azure Logic Apps | Microsoft Docs
description: "Zjistěte, jak funguje pro Azure Logic Apps ceny a fakturace."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.openlocfilehash: 63784c5e3af360b2f3f8cb330a9df8b27a85d859
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="logic-apps-pricing-model"></a><span data-ttu-id="12a94-103">Cenový model Logic Apps</span><span class="sxs-lookup"><span data-stu-id="12a94-103">Logic Apps pricing model</span></span>
<span data-ttu-id="12a94-104">Azure Logic Apps umožňuje škálovat a spouštět pracovní postupy integrace v cloudu.</span><span class="sxs-lookup"><span data-stu-id="12a94-104">Azure Logic Apps allows you to scale and execute integration workflows in the cloud.</span></span>  <span data-ttu-id="12a94-105">Následují informace o fakturaci a cenách plány pro Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="12a94-105">Following are details on the billing and pricing plans for Logic Apps.</span></span>
## <a name="consumption-pricing"></a><span data-ttu-id="12a94-106">Spotřeba – ceny</span><span class="sxs-lookup"><span data-stu-id="12a94-106">Consumption pricing</span></span>
<span data-ttu-id="12a94-107">Nově vytvořený Logic Apps použití plánu spotřeby.</span><span class="sxs-lookup"><span data-stu-id="12a94-107">Newly created Logic Apps use a consumption plan.</span></span> <span data-ttu-id="12a94-108">S Logic Apps spotřeba cenový model platíte jenom se používá.</span><span class="sxs-lookup"><span data-stu-id="12a94-108">With the Logic Apps consumption pricing model, you only pay for what you use.</span></span>  <span data-ttu-id="12a94-109">Aplikace logiky nejsou omezeny při použití plánu spotřeby.</span><span class="sxs-lookup"><span data-stu-id="12a94-109">Logic Apps are not throttled when using a consumption plan.</span></span>
<span data-ttu-id="12a94-110">Všechny akce provést při spuštění instance aplikace logiky jsou měření podle objemu.</span><span class="sxs-lookup"><span data-stu-id="12a94-110">All actions executed in a run of a logic app instance are metered.</span></span>
### <a name="what-are-action-executions"></a><span data-ttu-id="12a94-111">Jaké jsou spuštěních akce?</span><span class="sxs-lookup"><span data-stu-id="12a94-111">What are action executions?</span></span>
<span data-ttu-id="12a94-112">Každý krok v definici aplikace logiky je akce, která zahrnuje aktivačních událostí, tok řízení kroky jako podmínky, oborů, pro každý smyčky provést až smyčky, volání konektory a volání na nativní akce.</span><span class="sxs-lookup"><span data-stu-id="12a94-112">Every step in a logic app definition is an action, which includes triggers, control flow steps like conditions, scopes, for each loops, do until loops, calls to connectors and calls to native actions.</span></span>
<span data-ttu-id="12a94-113">Aktivační události jsou zvláštní akce, které slouží k vytváření instancí novou instanci aplikace logiky, když dojde k určité události.</span><span class="sxs-lookup"><span data-stu-id="12a94-113">Triggers are special actions that are designed to instantiate a new instance of a logic app when a particular event occurs.</span></span>  <span data-ttu-id="12a94-114">Existuje několik různých chování pro aktivační události, které mohou ovlivnit jak monitorované aplikaci logiky.</span><span class="sxs-lookup"><span data-stu-id="12a94-114">There are several different behaviors for triggers, which could affect how the logic app is metered.</span></span>
* <span data-ttu-id="12a94-115">**Aktivační událost dotazování** – této aktivační události průběžně dotazuje koncový bod, dokud neobdrží zprávu, která splňuje kritéria pro vytvoření instance aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="12a94-115">**Polling trigger** – this trigger continually polls an endpoint until it receives a message that satisfies the criteria for creating an instance of a logic app.</span></span>  <span data-ttu-id="12a94-116">Interval dotazování se dá nakonfigurovat v aktivační události v Návrháři logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="12a94-116">The polling interval can be configured in the trigger in the Logic App Designer.</span></span>  <span data-ttu-id="12a94-117">Každý požadavek dotazování i v případě, že ji nebude vytvořit instanci aplikace logiky, počítá jako spuštění akce.</span><span class="sxs-lookup"><span data-stu-id="12a94-117">Each polling request, even if it doesn’t create an instance of a logic app, counts as an action execution.</span></span>
* <span data-ttu-id="12a94-118">**Aktivační události Webhooku** – této aktivační události čeká klienta k odeslání požadavku na konkrétní koncový bod.</span><span class="sxs-lookup"><span data-stu-id="12a94-118">**Webhook trigger** – this trigger waits for a client to send it a request on a particular endpoint.</span></span>  <span data-ttu-id="12a94-119">Každý požadavek odeslaný ke koncovému bodu webhooku se počítá jako spuštění akce.</span><span class="sxs-lookup"><span data-stu-id="12a94-119">Each request sent to the webhook endpoint counts as an action execution.</span></span> <span data-ttu-id="12a94-120">Žádost a aktivační události Webhooku protokolu HTTP jsou obě aktivační události webhooku.</span><span class="sxs-lookup"><span data-stu-id="12a94-120">The Request and the HTTP Webhook trigger are both webhook triggers.</span></span>
* <span data-ttu-id="12a94-121">**Aktivační událost opakování** – této aktivační události vytvoří instanci aplikace logiky na základě intervalu opakování nakonfigurované v aktivační události.</span><span class="sxs-lookup"><span data-stu-id="12a94-121">**Recurrence trigger** – this trigger creates an instance of the logic app based on the recurrence interval configured in the trigger.</span></span>  <span data-ttu-id="12a94-122">Opakování aktivační událost může být například nakonfigurován pro spouštění každé tři dny nebo i každou minutu.</span><span class="sxs-lookup"><span data-stu-id="12a94-122">For example, a recurrence trigger can be configured to run every three days or even every minute.</span></span>

<span data-ttu-id="12a94-123">Spuštění aktivační události se zobrazí v okně prostředků aplikace logiky v části historie aktivační události.</span><span class="sxs-lookup"><span data-stu-id="12a94-123">Trigger executions can be seen in the Logic Apps resource blade in the Trigger History part.</span></span>

<span data-ttu-id="12a94-124">Všechny akce, které byly provedeny, jestli byly provedené úspěšně nebo neúspěšně se měří jako spuštění akce.</span><span class="sxs-lookup"><span data-stu-id="12a94-124">All actions that were executed, whether they were successful or failed are metered as an action execution.</span></span>  <span data-ttu-id="12a94-125">Akce, které byly přeskočeny z důvodu nebyla splněna podmínka nebo akce, které nebylo provést, protože aplikace logiky ukončeno před dokončením se počítá jako spuštění akce.</span><span class="sxs-lookup"><span data-stu-id="12a94-125">Actions that were skipped due to a condition not being met or actions that didn’t execute because the logic app terminated before completion are not counted as action executions.</span></span>

<span data-ttu-id="12a94-126">Akce provést v rámci smyčky, se počítají za iteraci smyčky.</span><span class="sxs-lookup"><span data-stu-id="12a94-126">Actions executed within loops are counted per iteration of the loop.</span></span>  <span data-ttu-id="12a94-127">Například jedna akce v pro každou smyčku iterace v rámci seznam 10 položek se počítají jako počet položek v seznamu (10) vynásobená počet akcí ve smyčce (1) a jeden pro zahájení smyčky, kterou v tomto příkladu by (10 * 1) + 1 = 11 spuštění akce.</span><span class="sxs-lookup"><span data-stu-id="12a94-127">For example, a single action in a for each loop iterating through a list of 10 items are counted as the number of items in the list (10) multiplied by the number of actions in the loop (1) plus one for the initiation of the loop, which, in this example, would be (10 * 1) + 1 = 11 action executions.</span></span>
<span data-ttu-id="12a94-128">Zakázané Logic Apps nemohou mít nové instance vytvořené a proto při zakázáno, není účtován.</span><span class="sxs-lookup"><span data-stu-id="12a94-128">Disabled Logic Apps cannot have new instances instantiated and therefore, while disabled, are not charged.</span></span>  <span data-ttu-id="12a94-129">Mějte na paměti, že po zakázání aplikace logiky může trvat nějakou dobu pro instance k uvedení před úplně zakázaná.</span><span class="sxs-lookup"><span data-stu-id="12a94-129">Be mindful that after disabling a logic app it may take a little time for the instances to quiesce before being completely disabled.</span></span>
### <a name="integration-account-usage"></a><span data-ttu-id="12a94-130">Používání účtů integrace</span><span class="sxs-lookup"><span data-stu-id="12a94-130">Integration Account Usage</span></span>
<span data-ttu-id="12a94-131">Součástí na základě spotřeby využití je [integrace účet](logic-apps-enterprise-integration-create-integration-account.md) pro zkoumání, vývoj a budete moci použít pro účely testování [B2B/EDI](logic-apps-enterprise-integration-b2b.md) a [zpracování kódu XML](logic-apps-enterprise-integration-xml.md) funkce Logic Apps bez dalších poplatků.</span><span class="sxs-lookup"><span data-stu-id="12a94-131">Included in consumption-based usage is an [integration account](logic-apps-enterprise-integration-create-integration-account.md) for exploration, development, and testing purposes allowing you to use the [B2B/EDI](logic-apps-enterprise-integration-b2b.md) and [XML processing](logic-apps-enterprise-integration-xml.md) features of Logic Apps at no additional cost.</span></span> <span data-ttu-id="12a94-132">Budete moci vytvořit maximálně jeden účet na oblast a úložiště až 10 smluv a 25 mapy.</span><span class="sxs-lookup"><span data-stu-id="12a94-132">You are able to create a maximum of one account per region and store up to 10 Agreements and 25 maps.</span></span> <span data-ttu-id="12a94-133">Schémata, certifikáty a partnery mít žádná omezení a můžete nahrát tolik, jako je třeba.</span><span class="sxs-lookup"><span data-stu-id="12a94-133">Schemas, certificates, and partners have no limits and you can upload as many as you need.</span></span>

<span data-ttu-id="12a94-134">Kromě zahrnutí účty pro integraci s spotřebou můžete také vytvořit účty pro standardní integraci bez těchto omezení a s naše smlouva SLA standardní logiku aplikace.</span><span class="sxs-lookup"><span data-stu-id="12a94-134">In addition to the inclusion of integration accounts with consumption, you can also create standard integration accounts without these limits and with our standard Logic Apps SLA.</span></span> <span data-ttu-id="12a94-135">Další informace najdete v tématu [Azure ceny](https://azure.microsoft.com/pricing/details/logic-apps).</span><span class="sxs-lookup"><span data-stu-id="12a94-135">For more information, see [Azure pricing](https://azure.microsoft.com/pricing/details/logic-apps).</span></span>

## <a name="app-service-plans"></a><span data-ttu-id="12a94-136">Plány služby App Service</span><span class="sxs-lookup"><span data-stu-id="12a94-136">App Service plans</span></span>
<span data-ttu-id="12a94-137">Aplikace logiky vytvořili odkazující plán služby App Service bude nadále chovat jako předtím.</span><span class="sxs-lookup"><span data-stu-id="12a94-137">Logic apps previously created referencing an App Service Plan continues to behave as before.</span></span> <span data-ttu-id="12a94-138">V závislosti na plánu vybrali jsou omezené po předepsaných denní spuštěních se překročí ale fakturují pomocí měření provádění akce.</span><span class="sxs-lookup"><span data-stu-id="12a94-138">Depending on the plan chosen, are throttled after the prescribed daily executions are exceeded but are billed using the action execution meter.</span></span>
<span data-ttu-id="12a94-139">EA zákazníky, kteří mají plánu služby App Service v jejich odběr, který nemá být explicitně přidružená aplikace logiky, využívat výhody zahrnuté množství.</span><span class="sxs-lookup"><span data-stu-id="12a94-139">EA customers that have an App Service Plan in their subscription, which does not have to be explicitly associated with the Logic App, get the included quantities benefit.</span></span>  <span data-ttu-id="12a94-140">Například pokud máte v předplatného EA a aplikace logiky ve stejném předplatném standardní plán služby App Service pak vám není účtován 10 000 akce spuštěních za den (viz následující tabulka).</span><span class="sxs-lookup"><span data-stu-id="12a94-140">For example, if you have a Standard App Service Plan in your EA subscription and a Logic App in the same subscription then you aren't charged for 10,000 action executions per day (see following table).</span></span> 

<span data-ttu-id="12a94-141">Plány služby App Service a jejich denní spuštěních povolené akce:</span><span class="sxs-lookup"><span data-stu-id="12a94-141">App Service Plans and their daily allowed action executions:</span></span>
|  | <span data-ttu-id="12a94-142">Uvolněte a sdílet nebo základní</span><span class="sxs-lookup"><span data-stu-id="12a94-142">Free/Shared/Basic</span></span> | <span data-ttu-id="12a94-143">Standard</span><span class="sxs-lookup"><span data-stu-id="12a94-143">Standard</span></span> | <span data-ttu-id="12a94-144">Premium</span><span class="sxs-lookup"><span data-stu-id="12a94-144">Premium</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="12a94-145">Akce spuštěních za den</span><span class="sxs-lookup"><span data-stu-id="12a94-145">Action executions per day</span></span> |<span data-ttu-id="12a94-146">200</span><span class="sxs-lookup"><span data-stu-id="12a94-146">200</span></span> |<span data-ttu-id="12a94-147">10 000</span><span class="sxs-lookup"><span data-stu-id="12a94-147">10,000</span></span> |<span data-ttu-id="12a94-148">50,000</span><span class="sxs-lookup"><span data-stu-id="12a94-148">50,000</span></span> |
### <a name="convert-from-app-service-plan-pricing-to-consumption"></a><span data-ttu-id="12a94-149">Převod plánu služby App Service – ceny na spotřebu</span><span class="sxs-lookup"><span data-stu-id="12a94-149">Convert from App Service Plan pricing to Consumption</span></span>
<span data-ttu-id="12a94-150">Chcete-li změnit aplikaci logiky, který má plán služby App Service přidružené k využívání modelu, odeberte odkaz na plán aplikační služby v definici aplikace logiky.</span><span class="sxs-lookup"><span data-stu-id="12a94-150">To change a Logic App that has an App Service Plan associated with it to a consumption model, remove the reference to the App Service Plan in the Logic App definition.</span></span>  <span data-ttu-id="12a94-151">Tato změna se provádí pomocí volání rutiny prostředí PowerShell:`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`</span><span class="sxs-lookup"><span data-stu-id="12a94-151">This change can be done with a call to a PowerShell cmdlet: `Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`</span></span>
## <a name="pricing"></a><span data-ttu-id="12a94-152">Ceny</span><span class="sxs-lookup"><span data-stu-id="12a94-152">Pricing</span></span>
<span data-ttu-id="12a94-153">Podrobnosti o cenách najdete v části [ceny aplikace logiky](https://azure.microsoft.com/pricing/details/logic-apps).</span><span class="sxs-lookup"><span data-stu-id="12a94-153">For pricing details, see [Logic Apps Pricing](https://azure.microsoft.com/pricing/details/logic-apps).</span></span>

## <a name="next-steps"></a><span data-ttu-id="12a94-154">Další kroky</span><span class="sxs-lookup"><span data-stu-id="12a94-154">Next steps</span></span>
* <span data-ttu-id="12a94-155">[Přehled Logic Apps][whatis]</span><span class="sxs-lookup"><span data-stu-id="12a94-155">[An overview of Logic Apps][whatis]</span></span>
* <span data-ttu-id="12a94-156">[Vytvoření první aplikace logiky][create]</span><span class="sxs-lookup"><span data-stu-id="12a94-156">[Create your first logic app][create]</span></span>

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-what-are-logic-apps.md
[create]: logic-apps-create-a-logic-app.md
