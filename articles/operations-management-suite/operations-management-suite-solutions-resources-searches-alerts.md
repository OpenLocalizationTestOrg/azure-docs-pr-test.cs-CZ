---
title: "aaaSaved hledání a upozornění v OMS řešení | Microsoft Docs"
description: "Řešení v OMS by měl obvykle zahrnovat uložená hledání v analýzy protokolů tooanalyze data shromažďovaná společností hello řešení.  Mohou také definovat výstrahy toonotify hello uživatele i automaticky akce v odpovědi tooa kritický problém.  Tento článek popisuje, jak toodefine analýzy protokolů uložené hledání a upozornění v šablony ARM, můžou být součástí řešení pro správu."
services: operations-management-suite
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: operations-management-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: bwren
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 93d7c5bbf061473833ca6c0a8e4d8e10d923f3ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="adding-log-analytics-saved-searches-and-alerts-toooms-management-solution-preview"></a><span data-ttu-id="fb69c-105">Přidání analýzy protokolů uložené hledání a výstrahy tooOMS řešení pro správu (Preview)</span><span class="sxs-lookup"><span data-stu-id="fb69c-105">Adding Log Analytics saved searches and alerts tooOMS management solution (Preview)</span></span>

> [!NOTE]
> <span data-ttu-id="fb69c-106">Toto je předběžná dokumentace pro vytváření řešení pro správu v OMS, které jsou aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="fb69c-106">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="fb69c-107">Žádné schéma níže popsané je toochange subjektu.</span><span class="sxs-lookup"><span data-stu-id="fb69c-107">Any schema described below is subject toochange.</span></span>   


<span data-ttu-id="fb69c-108">[Řešení pro správu v OMS](operations-management-suite-solutions.md) by měl obvykle zahrnovat [uložená hledání](../log-analytics/log-analytics-log-searches.md) v analýzy protokolů tooanalyze data shromažďovaná společností hello řešení.</span><span class="sxs-lookup"><span data-stu-id="fb69c-108">[Management solutions in OMS](operations-management-suite-solutions.md) will typically include [saved searches](../log-analytics/log-analytics-log-searches.md) in Log Analytics tooanalyze data collected by hello solution.</span></span>  <span data-ttu-id="fb69c-109">Může také definovat [výstrahy](../log-analytics/log-analytics-alerts.md) toonotify hello uživatele nebo automaticky provést akce v odpovědi tooa kritický problém.</span><span class="sxs-lookup"><span data-stu-id="fb69c-109">They may also define [alerts](../log-analytics/log-analytics-alerts.md) toonotify hello user or automatically take action in response tooa critical issue.</span></span>  <span data-ttu-id="fb69c-110">Tento článek popisuje, jak uložit toodefine analýzy protokolů hledání a upozornění v [Správa prostředků šablony](../resource-manager-template-walkthrough.md) tak můžou být je součástí [řešení pro správu](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="fb69c-110">This article describes how toodefine Log Analytics saved searches and alerts in a [Resource Management template](../resource-manager-template-walkthrough.md) so they can be included in [management solutions](operations-management-suite-solutions-creating.md).</span></span>

> [!NOTE]
> <span data-ttu-id="fb69c-111">Použijte parametry a proměnné, které jsou buď požadované, nebo běžné toomanagement řešení a popsané v zprostředkovatele Hello ukázky v tomto článku [vytváření řešení pro správu v Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span><span class="sxs-lookup"><span data-stu-id="fb69c-111">hello samples in this article use parameters and variables that are either required or common toomanagement solutions  and described in [Creating management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="fb69c-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="fb69c-112">Prerequisites</span></span>
<span data-ttu-id="fb69c-113">Tento článek předpokládá, že jste již obeznámeni s postupy příliš[vytvoření řešení správy](operations-management-suite-solutions-creating.md) a struktuře hello [šablony ARM](../resource-group-authoring-templates.md) a soubor řešení.</span><span class="sxs-lookup"><span data-stu-id="fb69c-113">This article assumes that you're already familiar with how too[create a management solution](operations-management-suite-solutions-creating.md) and hello structure of an [ARM template](../resource-group-authoring-templates.md) and solution file.</span></span>


## <a name="log-analytics-workspace"></a><span data-ttu-id="fb69c-114">Pracovní prostor analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="fb69c-114">Log Analytics Workspace</span></span>
<span data-ttu-id="fb69c-115">Všechny prostředky v analýzy protokolů jsou součástí [prostoru](../log-analytics/log-analytics-manage-access.md).</span><span class="sxs-lookup"><span data-stu-id="fb69c-115">All resources in Log Analytics are contained in a [workspace](../log-analytics/log-analytics-manage-access.md).</span></span>  <span data-ttu-id="fb69c-116">Jak je popsáno v [OMS pracovní prostor a účet Automation](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello prostoru není zahrnutý v řešení pro správu hello ale musí existovat před instalací hello řešení.</span><span class="sxs-lookup"><span data-stu-id="fb69c-116">As described in [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) hello workspace isn't included in hello management solution but must exist before hello solution is installed.</span></span>  <span data-ttu-id="fb69c-117">Pokud není k dispozici, se nezdaří instalace řešení hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-117">If it isn't available, then hello solution install will fail.</span></span>

<span data-ttu-id="fb69c-118">Název Hello hello pracovního prostoru se hello název každého prostředku analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="fb69c-118">hello name of hello workspace is in hello name of each Log Analytics resource.</span></span>  <span data-ttu-id="fb69c-119">To se provádí v hello řešení s hello **prostoru** parametr jako hello následující ukázka savedsearch prostředku.</span><span class="sxs-lookup"><span data-stu-id="fb69c-119">This is done in hello solution with hello **workspace** parameter as in hello following example of a savedsearch resource.</span></span>

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"


## <a name="saved-searches"></a><span data-ttu-id="fb69c-120">Uložená hledání</span><span class="sxs-lookup"><span data-stu-id="fb69c-120">Saved Searches</span></span>
<span data-ttu-id="fb69c-121">Zahrnout [uložená hledání](../log-analytics/log-analytics-log-searches.md) v řešení tooallow uživatelé tooquery dat shromážděných vaše řešení.</span><span class="sxs-lookup"><span data-stu-id="fb69c-121">Include [saved searches](../log-analytics/log-analytics-log-searches.md) in a solution tooallow users tooquery data collected by your solution.</span></span>  <span data-ttu-id="fb69c-122">Uložené hledání se zobrazí v části **Oblíbené** na portálu OMS hello a **uložená hledání** v hello portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="fb69c-122">Saved searches will appear under **Favorites** in hello OMS portal and **Saved Searches** in hello Azure portal .</span></span>  <span data-ttu-id="fb69c-123">Uložené hledání je také nutný pro každou výstrahu.</span><span class="sxs-lookup"><span data-stu-id="fb69c-123">A saved search is also required for each alert.</span></span>   

<span data-ttu-id="fb69c-124">[Analýzy protokolů uložené hledání](../log-analytics/log-analytics-log-searches.md) prostředky mít typ `Microsoft.OperationalInsights/workspaces/savedSearches` a mít hello strukturu.</span><span class="sxs-lookup"><span data-stu-id="fb69c-124">[Log Analytics saved search](../log-analytics/log-analytics-log-searches.md) resources have a type of `Microsoft.OperationalInsights/workspaces/savedSearches` and have hello following structure.</span></span>  <span data-ttu-id="fb69c-125">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-125">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 

    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
        ],
        "tags": { },
        "properties": {
            "etag": "*",
            "query": "[variables('SavedSearch').Query]",
            "displayName": "[variables('SavedSearch').DisplayName]",
            "category": "[variables('SavedSearch').Category]"
        }
    }



<span data-ttu-id="fb69c-126">Ke každé z vlastností hello uloženého hledání jsou popsané v následující tabulce hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-126">Each of hello properties of a saved search are described in hello following table.</span></span> 

| <span data-ttu-id="fb69c-127">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="fb69c-127">Property</span></span> | <span data-ttu-id="fb69c-128">Popis</span><span class="sxs-lookup"><span data-stu-id="fb69c-128">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="fb69c-129">category</span><span class="sxs-lookup"><span data-stu-id="fb69c-129">category</span></span> | <span data-ttu-id="fb69c-130">Hello kategorii pro uložené hledání hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-130">hello category for hello saved search.</span></span>  <span data-ttu-id="fb69c-131">Žádné uložená hledání v hello často budou sdílet stejnou řešení jedné kategorii, jsou seskupeny dohromady v konzole hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-131">Any saved searches in hello same solution will often share a single category so they are grouped together in hello console.</span></span> |
| <span data-ttu-id="fb69c-132">DisplayName</span><span class="sxs-lookup"><span data-stu-id="fb69c-132">displayname</span></span> | <span data-ttu-id="fb69c-133">Název toodisplay pro hello uložené hledání hello portálu.</span><span class="sxs-lookup"><span data-stu-id="fb69c-133">Name toodisplay for hello saved search in hello portal.</span></span> |
| <span data-ttu-id="fb69c-134">query</span><span class="sxs-lookup"><span data-stu-id="fb69c-134">query</span></span> | <span data-ttu-id="fb69c-135">Toorun dotazu.</span><span class="sxs-lookup"><span data-stu-id="fb69c-135">Query toorun.</span></span> |

> [!NOTE]
> <span data-ttu-id="fb69c-136">Toouse řídicí znaky v hello dotazu může být nutné, pokud obsahuje znaky, které je možné interpretovat jako JSON.</span><span class="sxs-lookup"><span data-stu-id="fb69c-136">You may need toouse escape characters in hello query if it includes characters that could be interpreted as JSON.</span></span>  <span data-ttu-id="fb69c-137">Například, pokud se dotaz **typu: AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write"**, by měly být zapsány v souboru hello řešení jako **typu: AzureActivity OperationName:\" Microsoft.Compute/virtualMachines/write\"**.</span><span class="sxs-lookup"><span data-stu-id="fb69c-137">For example, if your query was **Type:AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write"**, it should be written in hello solution file as **Type:AzureActivity OperationName:\"Microsoft.Compute/virtualMachines/write\"**.</span></span>

## <a name="alerts"></a><span data-ttu-id="fb69c-138">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="fb69c-138">Alerts</span></span>
<span data-ttu-id="fb69c-139">[Protokolu analýzy výstrahy](../log-analytics/log-analytics-alerts.md) jsou vytvořené pomocí pravidla výstrah, které běží uloženého hledání v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="fb69c-139">[Log Analytics alerts](../log-analytics/log-analytics-alerts.md) are created by alert rules that run a saved search on a regular interval.</span></span>  <span data-ttu-id="fb69c-140">Pokud hello výsledky dotazu hello odpovídají zadaným kritériím, se vytvoří záznam výstrah a spouštějí jednu nebo více akcí.</span><span class="sxs-lookup"><span data-stu-id="fb69c-140">If hello results of hello query match specified criteria, an alert record is created and one or more actions are run.</span></span>  

<span data-ttu-id="fb69c-141">Pravidla výstrah v rámci řešení pro správu jsou tvořeny hello následující tři různé prostředky.</span><span class="sxs-lookup"><span data-stu-id="fb69c-141">Alert rules in a management solution are made up of hello following three different resources.</span></span>

- <span data-ttu-id="fb69c-142">**Uložené hledání.**</span><span class="sxs-lookup"><span data-stu-id="fb69c-142">**Saved search.**</span></span>  <span data-ttu-id="fb69c-143">Definuje hello protokolu vyhledávání, který se má spustit.</span><span class="sxs-lookup"><span data-stu-id="fb69c-143">Defines hello log search that will be run.</span></span>  <span data-ttu-id="fb69c-144">Více pravidla výstrah můžete sdílet jeden uloženého hledání.</span><span class="sxs-lookup"><span data-stu-id="fb69c-144">Multiple alert rules can share a single saved search.</span></span>
- <span data-ttu-id="fb69c-145">**Plán.**</span><span class="sxs-lookup"><span data-stu-id="fb69c-145">**Schedule.**</span></span>  <span data-ttu-id="fb69c-146">Definuje, jak často hello protokol hledání se spustí.</span><span class="sxs-lookup"><span data-stu-id="fb69c-146">Defines how often hello log search will be run.</span></span>  <span data-ttu-id="fb69c-147">Každé pravidlo výstrahy budou mít pouze jeden plán.</span><span class="sxs-lookup"><span data-stu-id="fb69c-147">Each alert rule will have one and only one schedule.</span></span>
- <span data-ttu-id="fb69c-148">**Výstrahy akce.**</span><span class="sxs-lookup"><span data-stu-id="fb69c-148">**Alert action.**</span></span>  <span data-ttu-id="fb69c-149">Každé pravidlo výstrahy bude mít jeden prostředek akce s typem **výstraha** hello podrobnosti výstrahy hello například hello kritéria pro při záznamu výstrah se vytvoří a hello závažnost výstrahy, který definuje.</span><span class="sxs-lookup"><span data-stu-id="fb69c-149">Each alert rule will have one action resource with a type of **Alert** that defines hello details of hello alert such as hello criteria for when an alert record will be created and hello alert's severity.</span></span>  <span data-ttu-id="fb69c-150">Hello akce prostředků bude volitelně definovat odpověď e-mailu a sady runbook.</span><span class="sxs-lookup"><span data-stu-id="fb69c-150">hello action resource will optionally define a mail and runbook response.</span></span>
- <span data-ttu-id="fb69c-151">**Akce Webhooku (volitelné).**</span><span class="sxs-lookup"><span data-stu-id="fb69c-151">**Webhook action (optional).**</span></span>  <span data-ttu-id="fb69c-152">Pokud pravidlo výstrahy hello bude volat webhook, jehož, pak vyžaduje prostředek další akce s typem **Webhooku**.</span><span class="sxs-lookup"><span data-stu-id="fb69c-152">If hello alert rule will call a webhook, then it requires an additional action resource with a type of **Webhook**.</span></span>    

<span data-ttu-id="fb69c-153">Uložené hledání, které prostředky jsou popsané výše.</span><span class="sxs-lookup"><span data-stu-id="fb69c-153">Saved search resources are described above.</span></span>  <span data-ttu-id="fb69c-154">Hello další prostředky jsou popsané níže.</span><span class="sxs-lookup"><span data-stu-id="fb69c-154">hello other resources are described below.</span></span>


### <a name="schedule-resource"></a><span data-ttu-id="fb69c-155">Plán prostředku</span><span class="sxs-lookup"><span data-stu-id="fb69c-155">Schedule resource</span></span>

<span data-ttu-id="fb69c-156">Uložené hledání může mít jeden nebo více plánů s každý plán reprezentující samostatné pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fb69c-156">A saved search can have one or more schedules with each schedule representing a separate alert rule.</span></span> <span data-ttu-id="fb69c-157">Hello plán definuje, jak často hello vyhledávání běží a hello časový interval, přes které hello jsou načtena data.</span><span class="sxs-lookup"><span data-stu-id="fb69c-157">hello schedule defines how often hello search is run and hello time interval over which hello data is retrieved.</span></span>  <span data-ttu-id="fb69c-158">Plán prostředky mít typ `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` a mít hello strukturu.</span><span class="sxs-lookup"><span data-stu-id="fb69c-158">Schedule resources have a type of `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` and have hello following structure.</span></span> <span data-ttu-id="fb69c-159">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-159">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 


    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name)]"
        ],
        "properties": {
            "etag": "*",
            "interval": "[variables('Schedule').Interval]",
            "queryTimeSpan": "[variables('Schedule').TimeSpan]",
            "enabled": "[variables('Schedule').Enabled]"
        }
    }



<span data-ttu-id="fb69c-160">Hello vlastnosti pro plán prostředky jsou popsané v následující tabulka hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-160">hello properties for schedule resources are described in hello following table.</span></span>

| <span data-ttu-id="fb69c-161">Název elementu</span><span class="sxs-lookup"><span data-stu-id="fb69c-161">Element name</span></span> | <span data-ttu-id="fb69c-162">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="fb69c-162">Required</span></span> | <span data-ttu-id="fb69c-163">Popis</span><span class="sxs-lookup"><span data-stu-id="fb69c-163">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="fb69c-164">povoleno</span><span class="sxs-lookup"><span data-stu-id="fb69c-164">enabled</span></span>       | <span data-ttu-id="fb69c-165">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-165">Yes</span></span> | <span data-ttu-id="fb69c-166">Určuje, zda text hello upozornění je povolené, když je vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="fb69c-166">Specifies whether hello alert is enabled when it's created.</span></span> |
| <span data-ttu-id="fb69c-167">interval</span><span class="sxs-lookup"><span data-stu-id="fb69c-167">interval</span></span>      | <span data-ttu-id="fb69c-168">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-168">Yes</span></span> | <span data-ttu-id="fb69c-169">Jak často hello spuštění dotazu v minutách.</span><span class="sxs-lookup"><span data-stu-id="fb69c-169">How often hello query runs in minutes.</span></span> |
| <span data-ttu-id="fb69c-170">QueryTimeSpan</span><span class="sxs-lookup"><span data-stu-id="fb69c-170">queryTimeSpan</span></span> | <span data-ttu-id="fb69c-171">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-171">Yes</span></span> | <span data-ttu-id="fb69c-172">Časový interval v minutách, přes které tooevaluate výsledky.</span><span class="sxs-lookup"><span data-stu-id="fb69c-172">Length of time in minutes over which tooevaluate results.</span></span> |

<span data-ttu-id="fb69c-173">prostředek Hello plán by měl záviset na hello uložené hledání tak, aby se vytvoří před hello plán.</span><span class="sxs-lookup"><span data-stu-id="fb69c-173">hello schedule resource should depend on hello saved search so that it's created before hello schedule.</span></span>


### <a name="actions"></a><span data-ttu-id="fb69c-174">Akce</span><span class="sxs-lookup"><span data-stu-id="fb69c-174">Actions</span></span>
<span data-ttu-id="fb69c-175">Existují dva typy akce prostředku zadaného parametrem hello **typ** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="fb69c-175">There are two types of action resource specified by hello **Type** property.</span></span>  <span data-ttu-id="fb69c-176">Plán vyžaduje jednu **výstraha** akce, který definuje hello podrobnosti hello pravidlo výstrahy a jaké akce jsou provedeny, když se vytvoří výstraha.</span><span class="sxs-lookup"><span data-stu-id="fb69c-176">A schedule requires one **Alert** action which defines hello details of hello alert rule and what actions are taken when an alert is created.</span></span>  <span data-ttu-id="fb69c-177">Může také zahrnovat **Webhooku** akce, pokud by se měla volat webhook, jehož hello výstraze.</span><span class="sxs-lookup"><span data-stu-id="fb69c-177">It may also include a **Webhook** action if a webhook should be called from hello alert.</span></span>  

<span data-ttu-id="fb69c-178">Akce prostředky mít typ `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.</span><span class="sxs-lookup"><span data-stu-id="fb69c-178">Action resources have a type of `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.</span></span>  

#### <a name="alert-actions"></a><span data-ttu-id="fb69c-179">Akce upozornění</span><span class="sxs-lookup"><span data-stu-id="fb69c-179">Alert actions</span></span>

<span data-ttu-id="fb69c-180">Každý plán bude mít jeden **výstrahy** akce.</span><span class="sxs-lookup"><span data-stu-id="fb69c-180">Every schedule will have one **Alert** action.</span></span>  <span data-ttu-id="fb69c-181">Definuje hello podrobnosti výstrahy hello a volitelně oznámení a nápravné akce.</span><span class="sxs-lookup"><span data-stu-id="fb69c-181">This defines hello details of hello alert and optionally notification and remediation actions.</span></span>  <span data-ttu-id="fb69c-182">Odešle oznámení tooone e-mailu nebo víc adres.</span><span class="sxs-lookup"><span data-stu-id="fb69c-182">A notification sends an email tooone or more addresses.</span></span>  <span data-ttu-id="fb69c-183">Nápravy spustí sadu runbook v Azure Automation tooattempt tooremediate hello zjistil problém.</span><span class="sxs-lookup"><span data-stu-id="fb69c-183">A remediation starts a runbook in Azure Automation tooattempt tooremediate hello detected issue.</span></span>

<span data-ttu-id="fb69c-184">Akce výstrah mít hello strukturu.</span><span class="sxs-lookup"><span data-stu-id="fb69c-184">Alert actions have hello following structure.</span></span>  <span data-ttu-id="fb69c-185">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-185">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change hello parameter names.</span></span> 



    {
        "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Alert').Name)]",
        "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
        "apiVersion": "[variables('LogAnalyticsApiVersion')]",
        "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
        ],
        "properties": {
            "etag": "*",
            "type": "Alert",
            "name": "[variables('Alert').Name]",
            "description": "[variables('Alert').Description]",
            "severity": "[variables('Alert').Severity]",
            "threshold": {
                "operator": "[variables('Alert').Threshold.Operator]",
                "value": "[variables('Alert').Threshold.Value]",
                "metricsTrigger": {
                    "triggerCondition": "[variables('Alert').Threshold.Trigger.Condition]",
                    "operator": "[variables('Alert').Trigger.Operator]",
                    "value": "[variables('Alert').Trigger.Value]"
                },
            },
            "emailNotification": {
                "recipients": [
                    "[variables('Alert').Recipients]"
                ],
                "subject": "[variables('Alert').Subject]"
            },
            "remediation": {
                "runbookName": "[variables('Alert').Remedition.RunbookName]",
                "webhookUri": "[variables('Alert').Remedition.WebhookUri]"
            }
        }
    }

<span data-ttu-id="fb69c-186">Hello vlastnosti výstrahy akce prostředků jsou popsané v následující tabulky hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-186">hello properties for Alert action resources are described in hello following tables.</span></span>

| <span data-ttu-id="fb69c-187">Název elementu</span><span class="sxs-lookup"><span data-stu-id="fb69c-187">Element name</span></span> | <span data-ttu-id="fb69c-188">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="fb69c-188">Required</span></span> | <span data-ttu-id="fb69c-189">Popis</span><span class="sxs-lookup"><span data-stu-id="fb69c-189">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="fb69c-190">Typ</span><span class="sxs-lookup"><span data-stu-id="fb69c-190">Type</span></span> | <span data-ttu-id="fb69c-191">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-191">Yes</span></span> | <span data-ttu-id="fb69c-192">Typ akce hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-192">Type of hello action.</span></span>  <span data-ttu-id="fb69c-193">Bude jím **výstraha** pro akce výstrah.</span><span class="sxs-lookup"><span data-stu-id="fb69c-193">This will be **Alert** for alert actions.</span></span> |
| <span data-ttu-id="fb69c-194">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="fb69c-194">Name</span></span> | <span data-ttu-id="fb69c-195">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-195">Yes</span></span> | <span data-ttu-id="fb69c-196">Zobrazovaný název pro hello výstrahu.</span><span class="sxs-lookup"><span data-stu-id="fb69c-196">Display name for hello alert.</span></span>  <span data-ttu-id="fb69c-197">Toto je hello název, který se zobrazí v konzole hello pro pravidlo výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-197">This is hello name that's displayed in hello console for hello alert rule.</span></span> |
| <span data-ttu-id="fb69c-198">Popis</span><span class="sxs-lookup"><span data-stu-id="fb69c-198">Description</span></span> | <span data-ttu-id="fb69c-199">Ne</span><span class="sxs-lookup"><span data-stu-id="fb69c-199">No</span></span> | <span data-ttu-id="fb69c-200">Volitelný popis výstrahy hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-200">Optional description of hello alert.</span></span> |
| <span data-ttu-id="fb69c-201">Závažnost</span><span class="sxs-lookup"><span data-stu-id="fb69c-201">Severity</span></span> | <span data-ttu-id="fb69c-202">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-202">Yes</span></span> | <span data-ttu-id="fb69c-203">Závažnost výstrahy záznam hello z hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="fb69c-203">Severity of hello alert record from hello following values:</span></span><br><br> <span data-ttu-id="fb69c-204">**Kritické**</span><span class="sxs-lookup"><span data-stu-id="fb69c-204">**Critical**</span></span><br><span data-ttu-id="fb69c-205">**Upozornění**</span><span class="sxs-lookup"><span data-stu-id="fb69c-205">**Warning**</span></span><br><span data-ttu-id="fb69c-206">**Informační**</span><span class="sxs-lookup"><span data-stu-id="fb69c-206">**Informational**</span></span> |


##### <a name="threshold"></a><span data-ttu-id="fb69c-207">Prahová hodnota</span><span class="sxs-lookup"><span data-stu-id="fb69c-207">Threshold</span></span>
<span data-ttu-id="fb69c-208">Tento oddíl je povinný.</span><span class="sxs-lookup"><span data-stu-id="fb69c-208">This section is required.</span></span>  <span data-ttu-id="fb69c-209">Definuje vlastnosti hello hello prahové hodnoty pro výstrahu.</span><span class="sxs-lookup"><span data-stu-id="fb69c-209">It defines hello properties for hello alert threshold.</span></span>

| <span data-ttu-id="fb69c-210">Název elementu</span><span class="sxs-lookup"><span data-stu-id="fb69c-210">Element name</span></span> | <span data-ttu-id="fb69c-211">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="fb69c-211">Required</span></span> | <span data-ttu-id="fb69c-212">Popis</span><span class="sxs-lookup"><span data-stu-id="fb69c-212">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="fb69c-213">Operátor</span><span class="sxs-lookup"><span data-stu-id="fb69c-213">Operator</span></span> | <span data-ttu-id="fb69c-214">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-214">Yes</span></span> | <span data-ttu-id="fb69c-215">Operátor pro porovnání hello z hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="fb69c-215">Operator for hello comparison from hello following values:</span></span><br><br><span data-ttu-id="fb69c-216">**gt = větší než<br>lt = menší než**</span><span class="sxs-lookup"><span data-stu-id="fb69c-216">**gt = greater than<br>lt = less than**</span></span> |
| <span data-ttu-id="fb69c-217">Hodnota</span><span class="sxs-lookup"><span data-stu-id="fb69c-217">Value</span></span> | <span data-ttu-id="fb69c-218">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-218">Yes</span></span> | <span data-ttu-id="fb69c-219">Hello hodnota toocompare hello výsledky.</span><span class="sxs-lookup"><span data-stu-id="fb69c-219">hello value toocompare hello results.</span></span> |


##### <a name="metricstrigger"></a><span data-ttu-id="fb69c-220">MetricsTrigger</span><span class="sxs-lookup"><span data-stu-id="fb69c-220">MetricsTrigger</span></span>
<span data-ttu-id="fb69c-221">Tato část je volitelné.</span><span class="sxs-lookup"><span data-stu-id="fb69c-221">This section is optional.</span></span>  <span data-ttu-id="fb69c-222">Zahrňte pro upozornění na metriky měření.</span><span class="sxs-lookup"><span data-stu-id="fb69c-222">Include it for a metric measurement alert.</span></span>

> [!NOTE]
> <span data-ttu-id="fb69c-223">Metriky měření výstrahy jsou aktuálně ve verzi public preview.</span><span class="sxs-lookup"><span data-stu-id="fb69c-223">Metric measurement alerts are currently in public preview.</span></span> 

| <span data-ttu-id="fb69c-224">Název elementu</span><span class="sxs-lookup"><span data-stu-id="fb69c-224">Element name</span></span> | <span data-ttu-id="fb69c-225">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="fb69c-225">Required</span></span> | <span data-ttu-id="fb69c-226">Popis</span><span class="sxs-lookup"><span data-stu-id="fb69c-226">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="fb69c-227">TriggerCondition</span><span class="sxs-lookup"><span data-stu-id="fb69c-227">TriggerCondition</span></span> | <span data-ttu-id="fb69c-228">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-228">Yes</span></span> | <span data-ttu-id="fb69c-229">Určuje, zda text hello prahová hodnota pro celkový počet narušení nebo po sobě jdoucích narušení z hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="fb69c-229">Specifies whether hello threshold is for total number of breaches or consecutive breaches from hello following values:</span></span><br><br><span data-ttu-id="fb69c-230">**Celkový počet<br>po sobě jdoucích**</span><span class="sxs-lookup"><span data-stu-id="fb69c-230">**Total<br>Consecutive**</span></span> |
| <span data-ttu-id="fb69c-231">Operátor</span><span class="sxs-lookup"><span data-stu-id="fb69c-231">Operator</span></span> | <span data-ttu-id="fb69c-232">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-232">Yes</span></span> | <span data-ttu-id="fb69c-233">Operátor pro porovnání hello z hello následující hodnoty:</span><span class="sxs-lookup"><span data-stu-id="fb69c-233">Operator for hello comparison from hello following values:</span></span><br><br><span data-ttu-id="fb69c-234">**gt = větší než<br>lt = menší než**</span><span class="sxs-lookup"><span data-stu-id="fb69c-234">**gt = greater than<br>lt = less than**</span></span> |
| <span data-ttu-id="fb69c-235">Hodnota</span><span class="sxs-lookup"><span data-stu-id="fb69c-235">Value</span></span> | <span data-ttu-id="fb69c-236">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-236">Yes</span></span> | <span data-ttu-id="fb69c-237">Počet hello časy hello kritéria musí být výstraha nesplnění tootrigger hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-237">Number of hello times hello criteria must be met tootrigger hello alert.</span></span> |

##### <a name="throttling"></a><span data-ttu-id="fb69c-238">Omezování</span><span class="sxs-lookup"><span data-stu-id="fb69c-238">Throttling</span></span>
<span data-ttu-id="fb69c-239">Tato část je volitelné.</span><span class="sxs-lookup"><span data-stu-id="fb69c-239">This section is optional.</span></span>  <span data-ttu-id="fb69c-240">Pokud chcete, aby toosuppress výstrahy z hello stejná pravidla pro určitou dobu, po vytvoření výstrahy, zahrnují v této části.</span><span class="sxs-lookup"><span data-stu-id="fb69c-240">Include this section if you want toosuppress alerts from hello same rule for some amount of time after an alert is created.</span></span>

| <span data-ttu-id="fb69c-241">Název elementu</span><span class="sxs-lookup"><span data-stu-id="fb69c-241">Element name</span></span> | <span data-ttu-id="fb69c-242">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="fb69c-242">Required</span></span> | <span data-ttu-id="fb69c-243">Popis</span><span class="sxs-lookup"><span data-stu-id="fb69c-243">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="fb69c-244">Doba trvání v minutách</span><span class="sxs-lookup"><span data-stu-id="fb69c-244">DurationInMinutes</span></span> | <span data-ttu-id="fb69c-245">Ano, pokud omezení zahrnuté – element</span><span class="sxs-lookup"><span data-stu-id="fb69c-245">Yes if Throttling element included</span></span> | <span data-ttu-id="fb69c-246">Počet minut toosuppress výstrahy po jedné z hello, že se vytvoří stejné pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fb69c-246">Number of minutes toosuppress alerts after one from hello same alert rule is created.</span></span> |

##### <a name="emailnotification"></a><span data-ttu-id="fb69c-247">EmailNotification</span><span class="sxs-lookup"><span data-stu-id="fb69c-247">EmailNotification</span></span>
 <span data-ttu-id="fb69c-248">Tato část je volitelné zahrnout jej chcete-li hello tooone výstrahy toosend e-mailu nebo další příjemce.</span><span class="sxs-lookup"><span data-stu-id="fb69c-248">This section is optional  Include it if you want hello alert toosend mail tooone or more recipients.</span></span>

| <span data-ttu-id="fb69c-249">Název elementu</span><span class="sxs-lookup"><span data-stu-id="fb69c-249">Element name</span></span> | <span data-ttu-id="fb69c-250">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="fb69c-250">Required</span></span> | <span data-ttu-id="fb69c-251">Popis</span><span class="sxs-lookup"><span data-stu-id="fb69c-251">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="fb69c-252">Příjemce</span><span class="sxs-lookup"><span data-stu-id="fb69c-252">Recipients</span></span> | <span data-ttu-id="fb69c-253">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-253">Yes</span></span> | <span data-ttu-id="fb69c-254">Čárkami oddělený seznam e-mailové adresy toosend oznámení při výstrahu jako ve hello následující ukázka.</span><span class="sxs-lookup"><span data-stu-id="fb69c-254">Comma delimited list of email addresses toosend notification when an alert is created such as in hello following example.</span></span><br><br><span data-ttu-id="fb69c-255">**[ "recipient1@contoso.com", "recipient2@contoso.com" ]**</span><span class="sxs-lookup"><span data-stu-id="fb69c-255">**[ "recipient1@contoso.com", "recipient2@contoso.com" ]**</span></span> |
| <span data-ttu-id="fb69c-256">Předmět</span><span class="sxs-lookup"><span data-stu-id="fb69c-256">Subject</span></span> | <span data-ttu-id="fb69c-257">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-257">Yes</span></span> | <span data-ttu-id="fb69c-258">Řádek předmětu e-mailu hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-258">Subject line of hello mail.</span></span> |
| <span data-ttu-id="fb69c-259">Přílohy</span><span class="sxs-lookup"><span data-stu-id="fb69c-259">Attachment</span></span> | <span data-ttu-id="fb69c-260">Ne</span><span class="sxs-lookup"><span data-stu-id="fb69c-260">No</span></span> | <span data-ttu-id="fb69c-261">Přílohy nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="fb69c-261">Attachments are not currently supported.</span></span>  <span data-ttu-id="fb69c-262">Pokud tento element je zahrnuto, by měla být **žádné**.</span><span class="sxs-lookup"><span data-stu-id="fb69c-262">If this element is included, it should be **None**.</span></span> |


##### <a name="remediation"></a><span data-ttu-id="fb69c-263">Nápravy</span><span class="sxs-lookup"><span data-stu-id="fb69c-263">Remediation</span></span>
<span data-ttu-id="fb69c-264">Tato část je volitelný ho zahrňte, pokud chcete runbook toostart v odpovědi toohello výstrahy.</span><span class="sxs-lookup"><span data-stu-id="fb69c-264">This section is optional  Include it if you want a runbook toostart in response toohello alert.</span></span> |

| <span data-ttu-id="fb69c-265">Název elementu</span><span class="sxs-lookup"><span data-stu-id="fb69c-265">Element name</span></span> | <span data-ttu-id="fb69c-266">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="fb69c-266">Required</span></span> | <span data-ttu-id="fb69c-267">Popis</span><span class="sxs-lookup"><span data-stu-id="fb69c-267">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="fb69c-268">RunbookName</span><span class="sxs-lookup"><span data-stu-id="fb69c-268">RunbookName</span></span> | <span data-ttu-id="fb69c-269">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-269">Yes</span></span> | <span data-ttu-id="fb69c-270">Název sady runbook toostart hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-270">Name of hello runbook toostart.</span></span> |
| <span data-ttu-id="fb69c-271">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="fb69c-271">WebhookUri</span></span> | <span data-ttu-id="fb69c-272">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-272">Yes</span></span> | <span data-ttu-id="fb69c-273">Identifikátor URI hello webhooku pro sadu runbook hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-273">Uri of hello webhook for hello runbook.</span></span> |
| <span data-ttu-id="fb69c-274">Vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="fb69c-274">Expiry</span></span> | <span data-ttu-id="fb69c-275">Ne</span><span class="sxs-lookup"><span data-stu-id="fb69c-275">No</span></span> | <span data-ttu-id="fb69c-276">Datum a čas, který hello nápravy vyprší.</span><span class="sxs-lookup"><span data-stu-id="fb69c-276">Date and time that hello remediation expires.</span></span> |

#### <a name="webhook-actions"></a><span data-ttu-id="fb69c-277">Akce Webhooku</span><span class="sxs-lookup"><span data-stu-id="fb69c-277">Webhook actions</span></span>

<span data-ttu-id="fb69c-278">Akce Webhooku spuštění procesu voláním adresu URL a volitelně poskytuje datové části toobe, odeslána.</span><span class="sxs-lookup"><span data-stu-id="fb69c-278">Webhook actions start a process by calling a URL and optionally providing a payload toobe sent.</span></span> <span data-ttu-id="fb69c-279">Jsou podobné tooRemediation akce s výjimkou jsou určené pro webhooků, který může vyvolat procesy než Azure Automation runbook.</span><span class="sxs-lookup"><span data-stu-id="fb69c-279">They are similar tooRemediation actions except they are meant for webhooks that may invoke processes other than Azure Automation runbooks.</span></span> <span data-ttu-id="fb69c-280">Obsahují taky hello další možnost poskytnout vzdáleného procesu toohello toobe doručovat datové části.</span><span class="sxs-lookup"><span data-stu-id="fb69c-280">They also provide hello additional option of providing a payload toobe delivered toohello remote process.</span></span>

<span data-ttu-id="fb69c-281">Pokud upozornění bude volat webhook, jehož, pak bude je nutné prostředek akce s typem **Webhooku** v přidání toohello **výstraha** akce prostředků.</span><span class="sxs-lookup"><span data-stu-id="fb69c-281">If your alert will call a webhook, then it will need an action resource with a type of **Webhook** in addition toohello **Alert** action resource.</span></span>  

    {
      "name": "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearch').Name, '/', variables('Schedule').Name, '/', variables('Webhook').Name)]",
      "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions/",
      "apiVersion": "[variables('LogAnalyticsApiVersion')]",
      "dependsOn": [
            "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('SavedSearch').Name, '/schedules/', variables('Schedule').Name)]"
      ],
      "properties": {
        "etag": "*",
        "type": "[variables('Alert').Webhook.Type]",
        "name": "[variables('Alert').Webhook.Name]",
        "webhookUri": "[variables('Alert').Webhook.webhookUri]",
        "customPayload": "[variables('Alert').Webhook.CustomPayLoad]"
      }
    }

<span data-ttu-id="fb69c-282">Hello vlastnosti Webhooku akce prostředků jsou popsané v následujících tabulkách hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-282">hello properties for Webhook action resources are described in hello following tables.</span></span>

| <span data-ttu-id="fb69c-283">Název elementu</span><span class="sxs-lookup"><span data-stu-id="fb69c-283">Element name</span></span> | <span data-ttu-id="fb69c-284">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="fb69c-284">Required</span></span> | <span data-ttu-id="fb69c-285">Popis</span><span class="sxs-lookup"><span data-stu-id="fb69c-285">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="fb69c-286">type</span><span class="sxs-lookup"><span data-stu-id="fb69c-286">type</span></span> | <span data-ttu-id="fb69c-287">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-287">Yes</span></span> | <span data-ttu-id="fb69c-288">Typ akce hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-288">Type of hello action.</span></span>  <span data-ttu-id="fb69c-289">Bude jím **Webhooku** pro akce webhooku.</span><span class="sxs-lookup"><span data-stu-id="fb69c-289">This will be **Webhook** for webhook actions.</span></span> |
| <span data-ttu-id="fb69c-290">jméno</span><span class="sxs-lookup"><span data-stu-id="fb69c-290">name</span></span> | <span data-ttu-id="fb69c-291">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-291">Yes</span></span> | <span data-ttu-id="fb69c-292">Zobrazovaný název pro akci hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-292">Display name for hello action.</span></span>  <span data-ttu-id="fb69c-293">To se nezobrazí v konzole hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-293">This is not displayed in hello console.</span></span> |
| <span data-ttu-id="fb69c-294">wehookUri</span><span class="sxs-lookup"><span data-stu-id="fb69c-294">wehookUri</span></span> | <span data-ttu-id="fb69c-295">Ano</span><span class="sxs-lookup"><span data-stu-id="fb69c-295">Yes</span></span> | <span data-ttu-id="fb69c-296">Identifikátor URI pro hello webhook.</span><span class="sxs-lookup"><span data-stu-id="fb69c-296">Uri for hello webhook.</span></span> |
| <span data-ttu-id="fb69c-297">CustomPayload</span><span class="sxs-lookup"><span data-stu-id="fb69c-297">customPayload</span></span> | <span data-ttu-id="fb69c-298">Ne</span><span class="sxs-lookup"><span data-stu-id="fb69c-298">No</span></span> | <span data-ttu-id="fb69c-299">Webhooku toohello toobe odeslat vlastní datovou část.</span><span class="sxs-lookup"><span data-stu-id="fb69c-299">Custom payload toobe sent toohello webhook.</span></span> <span data-ttu-id="fb69c-300">Formát Hello bude záviset na jaké hello webhooku očekává.</span><span class="sxs-lookup"><span data-stu-id="fb69c-300">hello format will depend on what hello webhook is expecting.</span></span> |




## <a name="sample"></a><span data-ttu-id="fb69c-301">Ukázka</span><span class="sxs-lookup"><span data-stu-id="fb69c-301">Sample</span></span>

<span data-ttu-id="fb69c-302">Tady je ukázka řešení, které zahrnují zahrnující hello následující prostředky:</span><span class="sxs-lookup"><span data-stu-id="fb69c-302">Following is a sample of a solution that include that includes hello following resources:</span></span>

- <span data-ttu-id="fb69c-303">Uložené hledání</span><span class="sxs-lookup"><span data-stu-id="fb69c-303">Saved search</span></span>
- <span data-ttu-id="fb69c-304">Plán</span><span class="sxs-lookup"><span data-stu-id="fb69c-304">Schedule</span></span>
- <span data-ttu-id="fb69c-305">Akce oznámení</span><span class="sxs-lookup"><span data-stu-id="fb69c-305">Alert action</span></span>
- <span data-ttu-id="fb69c-306">Akce Webhooku</span><span class="sxs-lookup"><span data-stu-id="fb69c-306">Webhook action</span></span>

<span data-ttu-id="fb69c-307">Hello používá ukázka [standardní řešení parametry](operations-management-suite-solutions-solution-file.md#parameters) proměnné, které by běžně používané v řešení, jako byl proti toohardcoding hodnoty v definicích prostředků hello.</span><span class="sxs-lookup"><span data-stu-id="fb69c-307">hello sample uses [standard solution parameters](operations-management-suite-solutions-solution-file.md#parameters) variables that would commonly be used in a solution as opposed toohardcoding values in hello resource definitions.</span></span>

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0",
        "parameters": {
          "workspaceName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Log Analytics workspace"
            }
          },
          "accountName": {
            "type": "string",
            "metadata": {
              "Description": "Name of Automation account"
            }
          },
          "workspaceregionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Log Analytics workspace"
            }
          },
          "regionId": {
            "type": "string",
            "metadata": {
              "Description": "Region of Automation account"
            }
          },
          "pricingTier": {
            "type": "string",
            "metadata": {
              "Description": "Pricing tier of both Log Analytics workspace and Azure Automation account"
            }
          },
          "recipients": {
            "type": "string",
            "metadata": {
              "Description": "List of recipients for hello email alert separated by semicolon"
            }
          }
        },
        "variables": {
          "SolutionName": "MySolution",
          "SolutionVersion": "1.0",
          "SolutionPublisher": "Contoso",
          "ProductName": "SampleSolution",
    
          "LogAnalyticsApiVersion": "2015-11-01-preview",
    
          "MySearch": {
            "displayName": "Error records by hour",
            "query": "Type=MyRecord_CL | measure avg(Rating_d) by Instance_s interval 60minutes",
            "category": "Samples",
            "name": "Samples-Count of data"
          },
          "MyAlert": {
            "Name": "[toLower(concat('myalert-',uniqueString(resourceGroup().id, deployment().name)))]",
            "DisplayName": "My alert rule",
            "Description": "Sample alert.  Fires when 3 error records found over hour interval.",
            "Severity": "Critical",
            "ThresholdOperator": "gt",
            "ThresholdValue": 3,
            "Schedule": {
              "Name": "[toLower(concat('myschedule-',uniqueString(resourceGroup().id, deployment().name)))]",
              "Interval": 15,
              "TimeSpan": 60
            },
            "MetricsTrigger": {
              "TriggerCondition": "Consecutive",
              "Operator": "gt",
              "Value": 3
            },
            "ThrottleMinutes": 60,
            "Notification": {
              "Recipients": [
                "[parameters('recipients')]"
              ],
              "Subject": "Sample alert"
            },
            "Remediation": {
              "RunbookName": "MyRemediationRunbook",
              "WebhookUri": "https://s1events.azure-automation.net/webhooks?token=TluBFH3GpX4IEAnFoImoAWLTULkjD%2bTS0yscyrr7ogw%3d"
            },
            "Webhook": {
              "Name": "MyWebhook",
              "Uri": "https://MyService.com/webhook",
              "Payload": "{\"field1\":\"value1\",\"field2\":\"value2\"}"
            }
          }
        },
        "resources": [
          {
            "name": "[concat(variables('SolutionName'), '[' ,parameters('workspacename'), ']')]",
            "location": "[parameters('workspaceRegionId')]",
            "tags": { },
            "type": "Microsoft.OperationsManagement/solutions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
              "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
            ],
            "properties": {
              "workspaceResourceId": "[resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspacename'))]",
              "referencedResources": [
              ],
              "containedResources": [
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches', parameters('workspacename'), variables('MySearch').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Name)]",
                "[resourceId('Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions', parameters('workspacename'), variables('MySearch').Name, variables('MyAlert').Schedule.Name, variables('MyAlert').Webhook.Name)]"
              ]
            },
            "plan": {
              "name": "[concat(variables('SolutionName'), '[' ,parameters('workspaceName'), ']')]",
              "Version": "[variables('SolutionVersion')]",
              "product": "[variables('ProductName')]",
              "publisher": "[variables('SolutionPublisher')]",
              "promotionCode": ""
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [ ],
            "tags": { },
            "properties": {
              "etag": "*",
              "query": "[variables('MySearch').query]",
              "displayName": "[variables('MySearch').displayName]",
              "category": "[variables('MySearch').category]"
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name)]"
            ],
            "properties": {
              "etag": "*",
              "interval": "[variables('MyAlert').Schedule.Interval]",
              "queryTimeSpan": "[variables('MyAlert').Schedule.TimeSpan]",
              "enabled": true
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/',  variables('MyAlert').Schedule.Name, '/',  variables('MyAlert').Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/',  variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Alert",
              "Name": "[variables('MyAlert').DisplayName]",
              "Description": "[variables('MyAlert').Description]",
              "Severity": "[variables('MyAlert').Severity]",
              "Threshold": {
                "Operator": "[variables('MyAlert').ThresholdOperator]",
                "Value": "[variables('MyAlert').ThresholdValue]",
                "MetricsTrigger": {
                  "TriggerCondition": "[variables('MyAlert').MetricsTrigger.TriggerCondition]",
                  "Operator": "[variables('MyAlert').MetricsTrigger.Operator]",
                  "Value": "[variables('MyAlert').MetricsTrigger.Value]"
                }
              },
              "Throttling": {
                "DurationInMinutes": "[variables('MyAlert').ThrottleMinutes]"
              },
              "EmailNotification": {
                "Recipients": "[variables('MyAlert').Notification.Recipients]",
                "Subject": "[variables('MyAlert').Notification.Subject]",
                "Attachment": "None"
              },
              "Remediation": {
                "RunbookName": "[variables('MyAlert').Remediation.RunbookName]",
                "WebhookUri": "[variables('MyAlert').Remediation.WebhookUri]"
              }
            }
          },
          {
            "name": "[concat(parameters('workspaceName'), '/', variables('MySearch').Name, '/', variables('MyAlert').Schedule.Name, '/', variables('MyAlert').Webhook.Name)]",
            "type": "Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions",
            "apiVersion": "[variables('LogAnalyticsApiVersion')]",
            "dependsOn": [
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name)]",
              "[concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName'), '/savedSearches/', variables('MySearch').Name, '/schedules/', variables('MyAlert').Schedule.Name, '/actions/',variables('MyAlert').Name)]"
            ],
            "properties": {
              "etag": "*",
              "Type": "Webhook",
              "Name": "[variables('MyAlert').Webhook.Name]",
              "WebhookUri": "[variables('MyAlert').Webhook.Uri]",
              "CustomPayload": "[variables('MyAlert').Webhook.Payload]"
            }
          }
        ]
    }


<span data-ttu-id="fb69c-308">Hello následující soubor parametrů obsahuje hodnoty ukázky pro toto řešení.</span><span class="sxs-lookup"><span data-stu-id="fb69c-308">hello following parameter file provides samples values for this solution.</span></span>

    {
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
            "workspacename": {
                "value": "myWorkspace"
            },
            "accountName": {
                "value": "myAccount"
            },
            "workspaceregionId": {
                "value": "East US"
            },
            "regionId": {
                "value": "East US 2"
            },
            "pricingTier": {
                "value": "Free"
            },
            "recipients": {
                "value": "recipient1@contoso.com;recipient2@contoso.com"
            }
        }
    }


## <a name="next-steps"></a><span data-ttu-id="fb69c-309">Další kroky</span><span class="sxs-lookup"><span data-stu-id="fb69c-309">Next steps</span></span>
* <span data-ttu-id="fb69c-310">[Přidání zobrazení](operations-management-suite-solutions-resources-views.md) tooyour řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="fb69c-310">[Add views](operations-management-suite-solutions-resources-views.md) tooyour management solution.</span></span>
* <span data-ttu-id="fb69c-311">[Přidat runbooků služeb automatizace a dalším prostředkům](operations-management-suite-solutions-resources-automation.md) tooyour řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="fb69c-311">[Add Automation runbooks and other resources](operations-management-suite-solutions-resources-automation.md) tooyour management solution.</span></span>

