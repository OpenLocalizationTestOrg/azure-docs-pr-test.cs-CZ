---
title: "Uložené hledání a upozornění v OMS řešení | Microsoft Docs"
description: "Řešení v OMS by měl obvykle zahrnovat uložená hledání v Log Analytics k analýze dat shromážděných řešením.  Mohou také definovat výstrahy, které uživatele nebo v reakci na kritický problém automaticky provést akci.  Tento článek popisuje, jak definovat analýzy protokolů, které jsou uložené hledání a upozornění v šablony ARM, můžou být součástí řešení pro správu."
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
ms.openlocfilehash: 21c42a747a08c5386c65d10190baf0054a7adef8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="adding-log-analytics-saved-searches-and-alerts-to-oms-management-solution-preview"></a><span data-ttu-id="26480-105">Přidání analýzy protokolů uložené hledání a výstrahy OMS řešení pro správu (Preview)</span><span class="sxs-lookup"><span data-stu-id="26480-105">Adding Log Analytics saved searches and alerts to OMS management solution (Preview)</span></span>

> [!NOTE]
> <span data-ttu-id="26480-106">Toto je předběžná dokumentace pro vytváření řešení pro správu v OMS, které jsou aktuálně ve verzi preview.</span><span class="sxs-lookup"><span data-stu-id="26480-106">This is preliminary documentation for creating management solutions in OMS which are currently in preview.</span></span> <span data-ttu-id="26480-107">Žádné schéma popsané níže se mohou změnit.</span><span class="sxs-lookup"><span data-stu-id="26480-107">Any schema described below is subject to change.</span></span>   


<span data-ttu-id="26480-108">[Řešení pro správu v OMS](operations-management-suite-solutions.md) by měl obvykle zahrnovat [uložená hledání](../log-analytics/log-analytics-log-searches.md) v Log Analytics k analýze dat shromážděných řešením.</span><span class="sxs-lookup"><span data-stu-id="26480-108">[Management solutions in OMS](operations-management-suite-solutions.md) will typically include [saved searches](../log-analytics/log-analytics-log-searches.md) in Log Analytics to analyze data collected by the solution.</span></span>  <span data-ttu-id="26480-109">Může také definovat [výstrahy](../log-analytics/log-analytics-alerts.md) upozornit uživatele, nebo v reakci na kritický problém automaticky provést akci.</span><span class="sxs-lookup"><span data-stu-id="26480-109">They may also define [alerts](../log-analytics/log-analytics-alerts.md) to notify the user or automatically take action in response to a critical issue.</span></span>  <span data-ttu-id="26480-110">Tento článek popisuje, jak definovat uložená hledání analýzy protokolů a výstrahy [Správa prostředků šablony](../resource-manager-template-walkthrough.md) tak můžou být je součástí [řešení pro správu](operations-management-suite-solutions-creating.md).</span><span class="sxs-lookup"><span data-stu-id="26480-110">This article describes how to define Log Analytics saved searches and alerts in a [Resource Management template](../resource-manager-template-walkthrough.md) so they can be included in [management solutions](operations-management-suite-solutions-creating.md).</span></span>

> [!NOTE]
> <span data-ttu-id="26480-111">Ukázky v tomto článku použít parametry a proměnné, které jsou nutné nebo společné pro řešení pro správu a jsou popsány v [vytváření řešení pro správu v Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span><span class="sxs-lookup"><span data-stu-id="26480-111">The samples in this article use parameters and variables that are either required or common to management solutions  and described in [Creating management solutions in Operations Management Suite (OMS)](operations-management-suite-solutions-creating.md)</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="26480-112">Požadavky</span><span class="sxs-lookup"><span data-stu-id="26480-112">Prerequisites</span></span>
<span data-ttu-id="26480-113">Tento článek předpokládá, že jste již obeznámeni s postupy [vytvoření řešení správy](operations-management-suite-solutions-creating.md) a struktura [šablony ARM](../resource-group-authoring-templates.md) a soubor řešení.</span><span class="sxs-lookup"><span data-stu-id="26480-113">This article assumes that you're already familiar with how to [create a management solution](operations-management-suite-solutions-creating.md) and the structure of an [ARM template](../resource-group-authoring-templates.md) and solution file.</span></span>


## <a name="log-analytics-workspace"></a><span data-ttu-id="26480-114">Pracovní prostor analýzy protokolů</span><span class="sxs-lookup"><span data-stu-id="26480-114">Log Analytics Workspace</span></span>
<span data-ttu-id="26480-115">Všechny prostředky v analýzy protokolů jsou součástí [prostoru](../log-analytics/log-analytics-manage-access.md).</span><span class="sxs-lookup"><span data-stu-id="26480-115">All resources in Log Analytics are contained in a [workspace](../log-analytics/log-analytics-manage-access.md).</span></span>  <span data-ttu-id="26480-116">Jak je popsáno v [OMS pracovní prostor a účet Automation](operations-management-suite-solutions.md#oms-workspace-and-automation-account) pracovním prostoru není zahrnutý v řešení pro správu, ale musí existovat před instalací řešení.</span><span class="sxs-lookup"><span data-stu-id="26480-116">As described in [OMS workspace and Automation account](operations-management-suite-solutions.md#oms-workspace-and-automation-account) the workspace isn't included in the management solution but must exist before the solution is installed.</span></span>  <span data-ttu-id="26480-117">Pokud není k dispozici, se nezdaří instalace řešení.</span><span class="sxs-lookup"><span data-stu-id="26480-117">If it isn't available, then the solution install will fail.</span></span>

<span data-ttu-id="26480-118">Název pracovního prostoru je názvu každého prostředku analýzy protokolů.</span><span class="sxs-lookup"><span data-stu-id="26480-118">The name of the workspace is in the name of each Log Analytics resource.</span></span>  <span data-ttu-id="26480-119">To se provádí v řešení s **prostoru** parametr jako v následujícím příkladu savedsearch prostředku.</span><span class="sxs-lookup"><span data-stu-id="26480-119">This is done in the solution with the **workspace** parameter as in the following example of a savedsearch resource.</span></span>

    "name": "[concat(parameters('workspaceName'), '/', variables('SavedSearchId'))]"


## <a name="saved-searches"></a><span data-ttu-id="26480-120">Uložená hledání</span><span class="sxs-lookup"><span data-stu-id="26480-120">Saved Searches</span></span>
<span data-ttu-id="26480-121">Zahrnout [uložená hledání](../log-analytics/log-analytics-log-searches.md) v řešení, aby uživatelé mohli dotaz na data shromažďovaná společností řešení.</span><span class="sxs-lookup"><span data-stu-id="26480-121">Include [saved searches](../log-analytics/log-analytics-log-searches.md) in a solution to allow users to query data collected by your solution.</span></span>  <span data-ttu-id="26480-122">Uložené hledání se zobrazí v části **Oblíbené** na portálu OMS a **uložená hledání** na portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="26480-122">Saved searches will appear under **Favorites** in the OMS portal and **Saved Searches** in the Azure portal .</span></span>  <span data-ttu-id="26480-123">Uložené hledání je také nutný pro každou výstrahu.</span><span class="sxs-lookup"><span data-stu-id="26480-123">A saved search is also required for each alert.</span></span>   

<span data-ttu-id="26480-124">[Analýzy protokolů uložené hledání](../log-analytics/log-analytics-log-searches.md) prostředky mít typ `Microsoft.OperationalInsights/workspaces/savedSearches` a mít následující strukturu.</span><span class="sxs-lookup"><span data-stu-id="26480-124">[Log Analytics saved search](../log-analytics/log-analytics-log-searches.md) resources have a type of `Microsoft.OperationalInsights/workspaces/savedSearches` and have the following structure.</span></span>  <span data-ttu-id="26480-125">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="26480-125">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 

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



<span data-ttu-id="26480-126">Ke každé z vlastností uloženého hledání jsou popsané v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="26480-126">Each of the properties of a saved search are described in the following table.</span></span> 

| <span data-ttu-id="26480-127">Vlastnost</span><span class="sxs-lookup"><span data-stu-id="26480-127">Property</span></span> | <span data-ttu-id="26480-128">Popis</span><span class="sxs-lookup"><span data-stu-id="26480-128">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="26480-129">category</span><span class="sxs-lookup"><span data-stu-id="26480-129">category</span></span> | <span data-ttu-id="26480-130">Kategorie pro uložené hledání.</span><span class="sxs-lookup"><span data-stu-id="26480-130">The category for the saved search.</span></span>  <span data-ttu-id="26480-131">Všechny uložená hledání ve stejném řešení, bude často sdílet jednu kategorii, jsou seskupeny dohromady v konzole.</span><span class="sxs-lookup"><span data-stu-id="26480-131">Any saved searches in the same solution will often share a single category so they are grouped together in the console.</span></span> |
| <span data-ttu-id="26480-132">DisplayName</span><span class="sxs-lookup"><span data-stu-id="26480-132">displayname</span></span> | <span data-ttu-id="26480-133">Název zobrazení pro uložené hledání na portálu.</span><span class="sxs-lookup"><span data-stu-id="26480-133">Name to display for the saved search in the portal.</span></span> |
| <span data-ttu-id="26480-134">query</span><span class="sxs-lookup"><span data-stu-id="26480-134">query</span></span> | <span data-ttu-id="26480-135">Dotaz spustit.</span><span class="sxs-lookup"><span data-stu-id="26480-135">Query to run.</span></span> |

> [!NOTE]
> <span data-ttu-id="26480-136">Musíte použít řídicí znaky v dotazu, pokud obsahuje znaky, které je možné interpretovat jako JSON.</span><span class="sxs-lookup"><span data-stu-id="26480-136">You may need to use escape characters in the query if it includes characters that could be interpreted as JSON.</span></span>  <span data-ttu-id="26480-137">Například, pokud se dotaz **typu: AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write"**, by měly být zapsány v souboru řešení jako **typu: AzureActivity OperationName:\"Microsoft.Compute/virtualMachines/write\"**.</span><span class="sxs-lookup"><span data-stu-id="26480-137">For example, if your query was **Type:AzureActivity OperationName:"Microsoft.Compute/virtualMachines/write"**, it should be written in the solution file as **Type:AzureActivity OperationName:\"Microsoft.Compute/virtualMachines/write\"**.</span></span>

## <a name="alerts"></a><span data-ttu-id="26480-138">Výstrahy</span><span class="sxs-lookup"><span data-stu-id="26480-138">Alerts</span></span>
<span data-ttu-id="26480-139">[Protokolu analýzy výstrahy](../log-analytics/log-analytics-alerts.md) jsou vytvořené pomocí pravidla výstrah, které běží uloženého hledání v pravidelných intervalech.</span><span class="sxs-lookup"><span data-stu-id="26480-139">[Log Analytics alerts](../log-analytics/log-analytics-alerts.md) are created by alert rules that run a saved search on a regular interval.</span></span>  <span data-ttu-id="26480-140">Pokud výsledky dotazu odpovídal zadaná kritéria, se vytvoří záznam výstrah a spouštějí jednu nebo více akcí.</span><span class="sxs-lookup"><span data-stu-id="26480-140">If the results of the query match specified criteria, an alert record is created and one or more actions are run.</span></span>  

<span data-ttu-id="26480-141">Pravidla výstrah v rámci řešení pro správu se skládá z těchto tří různých prostředků.</span><span class="sxs-lookup"><span data-stu-id="26480-141">Alert rules in a management solution are made up of the following three different resources.</span></span>

- <span data-ttu-id="26480-142">**Uložené hledání.**</span><span class="sxs-lookup"><span data-stu-id="26480-142">**Saved search.**</span></span>  <span data-ttu-id="26480-143">Definuje vyhledávání protokolu, který se má spustit.</span><span class="sxs-lookup"><span data-stu-id="26480-143">Defines the log search that will be run.</span></span>  <span data-ttu-id="26480-144">Více pravidla výstrah můžete sdílet jeden uloženého hledání.</span><span class="sxs-lookup"><span data-stu-id="26480-144">Multiple alert rules can share a single saved search.</span></span>
- <span data-ttu-id="26480-145">**Plán.**</span><span class="sxs-lookup"><span data-stu-id="26480-145">**Schedule.**</span></span>  <span data-ttu-id="26480-146">Definuje, jak často se spustí vyhledávání protokolu.</span><span class="sxs-lookup"><span data-stu-id="26480-146">Defines how often the log search will be run.</span></span>  <span data-ttu-id="26480-147">Každé pravidlo výstrahy budou mít pouze jeden plán.</span><span class="sxs-lookup"><span data-stu-id="26480-147">Each alert rule will have one and only one schedule.</span></span>
- <span data-ttu-id="26480-148">**Výstrahy akce.**</span><span class="sxs-lookup"><span data-stu-id="26480-148">**Alert action.**</span></span>  <span data-ttu-id="26480-149">Každé pravidlo výstrahy bude mít jeden prostředek akce s typem **výstraha** , který definuje podrobnosti výstrahy například kritéria pro vytvoření záznamu výstrah a závažnosti výstrah.</span><span class="sxs-lookup"><span data-stu-id="26480-149">Each alert rule will have one action resource with a type of **Alert** that defines the details of the alert such as the criteria for when an alert record will be created and the alert's severity.</span></span>  <span data-ttu-id="26480-150">Akce prostředku bude volitelně definovat odpověď e-mailu a sady runbook.</span><span class="sxs-lookup"><span data-stu-id="26480-150">The action resource will optionally define a mail and runbook response.</span></span>
- <span data-ttu-id="26480-151">**Akce Webhooku (volitelné).**</span><span class="sxs-lookup"><span data-stu-id="26480-151">**Webhook action (optional).**</span></span>  <span data-ttu-id="26480-152">Pokud pravidlo výstrahy bude volat webhook, jehož, pak vyžaduje prostředek další akce s typem **Webhooku**.</span><span class="sxs-lookup"><span data-stu-id="26480-152">If the alert rule will call a webhook, then it requires an additional action resource with a type of **Webhook**.</span></span>    

<span data-ttu-id="26480-153">Uložené hledání, které prostředky jsou popsané výše.</span><span class="sxs-lookup"><span data-stu-id="26480-153">Saved search resources are described above.</span></span>  <span data-ttu-id="26480-154">Dále jsou uvedená na další prostředky.</span><span class="sxs-lookup"><span data-stu-id="26480-154">The other resources are described below.</span></span>


### <a name="schedule-resource"></a><span data-ttu-id="26480-155">Plán prostředku</span><span class="sxs-lookup"><span data-stu-id="26480-155">Schedule resource</span></span>

<span data-ttu-id="26480-156">Uložené hledání může mít jeden nebo více plánů s každý plán reprezentující samostatné pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="26480-156">A saved search can have one or more schedules with each schedule representing a separate alert rule.</span></span> <span data-ttu-id="26480-157">Plán definuje, jak často hledání je spustit a časový interval, za které se načítají data.</span><span class="sxs-lookup"><span data-stu-id="26480-157">The schedule defines how often the search is run and the time interval over which the data is retrieved.</span></span>  <span data-ttu-id="26480-158">Plán prostředky mít typ `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` a mít následující strukturu.</span><span class="sxs-lookup"><span data-stu-id="26480-158">Schedule resources have a type of `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/` and have the following structure.</span></span> <span data-ttu-id="26480-159">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="26480-159">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 


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



<span data-ttu-id="26480-160">Vlastnosti pro plán prostředky jsou popsány v následující tabulce.</span><span class="sxs-lookup"><span data-stu-id="26480-160">The properties for schedule resources are described in the following table.</span></span>

| <span data-ttu-id="26480-161">Název elementu</span><span class="sxs-lookup"><span data-stu-id="26480-161">Element name</span></span> | <span data-ttu-id="26480-162">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="26480-162">Required</span></span> | <span data-ttu-id="26480-163">Popis</span><span class="sxs-lookup"><span data-stu-id="26480-163">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="26480-164">povoleno</span><span class="sxs-lookup"><span data-stu-id="26480-164">enabled</span></span>       | <span data-ttu-id="26480-165">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-165">Yes</span></span> | <span data-ttu-id="26480-166">Určuje, zda je povoleno výstrahu, když je vytvořeno.</span><span class="sxs-lookup"><span data-stu-id="26480-166">Specifies whether the alert is enabled when it's created.</span></span> |
| <span data-ttu-id="26480-167">Interval</span><span class="sxs-lookup"><span data-stu-id="26480-167">interval</span></span>      | <span data-ttu-id="26480-168">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-168">Yes</span></span> | <span data-ttu-id="26480-169">Jak často spuštění dotazu v minutách.</span><span class="sxs-lookup"><span data-stu-id="26480-169">How often the query runs in minutes.</span></span> |
| <span data-ttu-id="26480-170">QueryTimeSpan</span><span class="sxs-lookup"><span data-stu-id="26480-170">queryTimeSpan</span></span> | <span data-ttu-id="26480-171">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-171">Yes</span></span> | <span data-ttu-id="26480-172">Časový interval v minutách, přes který vyhodnotit výsledky.</span><span class="sxs-lookup"><span data-stu-id="26480-172">Length of time in minutes over which to evaluate results.</span></span> |

<span data-ttu-id="26480-173">Plán prostředku by měl závisí na uloženého hledání tak, aby se vytvoří před plán.</span><span class="sxs-lookup"><span data-stu-id="26480-173">The schedule resource should depend on the saved search so that it's created before the schedule.</span></span>


### <a name="actions"></a><span data-ttu-id="26480-174">Akce</span><span class="sxs-lookup"><span data-stu-id="26480-174">Actions</span></span>
<span data-ttu-id="26480-175">Existují dva typy prostředků akce určeného **typ** vlastnost.</span><span class="sxs-lookup"><span data-stu-id="26480-175">There are two types of action resource specified by the **Type** property.</span></span>  <span data-ttu-id="26480-176">Plán vyžaduje jednu **výstraha** akce, který definuje podrobnosti pravidlo výstrahy a jaké akce jsou provedeny, když se vytvoří výstraha.</span><span class="sxs-lookup"><span data-stu-id="26480-176">A schedule requires one **Alert** action which defines the details of the alert rule and what actions are taken when an alert is created.</span></span>  <span data-ttu-id="26480-177">Může také zahrnovat **Webhooku** akce, pokud by se měla volat webhook, jehož ve výstraze.</span><span class="sxs-lookup"><span data-stu-id="26480-177">It may also include a **Webhook** action if a webhook should be called from the alert.</span></span>  

<span data-ttu-id="26480-178">Akce prostředky mít typ `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.</span><span class="sxs-lookup"><span data-stu-id="26480-178">Action resources have a type of `Microsoft.OperationalInsights/workspaces/savedSearches/schedules/actions`.</span></span>  

#### <a name="alert-actions"></a><span data-ttu-id="26480-179">Akce upozornění</span><span class="sxs-lookup"><span data-stu-id="26480-179">Alert actions</span></span>

<span data-ttu-id="26480-180">Každý plán bude mít jeden **výstrahy** akce.</span><span class="sxs-lookup"><span data-stu-id="26480-180">Every schedule will have one **Alert** action.</span></span>  <span data-ttu-id="26480-181">Definuje podrobnosti výstrahy a volitelně oznámení a nápravné akce.</span><span class="sxs-lookup"><span data-stu-id="26480-181">This defines the details of the alert and optionally notification and remediation actions.</span></span>  <span data-ttu-id="26480-182">Oznámení se odešle e-mail na jeden nebo více adres.</span><span class="sxs-lookup"><span data-stu-id="26480-182">A notification sends an email to one or more addresses.</span></span>  <span data-ttu-id="26480-183">Nápravy spuštění sady runbook ve službě Azure Automation se pokusit o napravit zjištěné potíže.</span><span class="sxs-lookup"><span data-stu-id="26480-183">A remediation starts a runbook in Azure Automation to attempt to remediate the detected issue.</span></span>

<span data-ttu-id="26480-184">Akce výstrah mají následující strukturu.</span><span class="sxs-lookup"><span data-stu-id="26480-184">Alert actions have the following structure.</span></span>  <span data-ttu-id="26480-185">Jedná se o běžné parametry a proměnné tak, aby můžete zkopírovat a vložit tento fragment kódu do souboru řešení a změnit názvy parametrů.</span><span class="sxs-lookup"><span data-stu-id="26480-185">This includes common variables and parameters so that you can copy and paste this code snippet into your solution file and change the parameter names.</span></span> 



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

<span data-ttu-id="26480-186">Vlastnosti výstrah akce prostředky jsou popsané v následujících tabulkách.</span><span class="sxs-lookup"><span data-stu-id="26480-186">The properties for Alert action resources are described in the following tables.</span></span>

| <span data-ttu-id="26480-187">Název elementu</span><span class="sxs-lookup"><span data-stu-id="26480-187">Element name</span></span> | <span data-ttu-id="26480-188">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="26480-188">Required</span></span> | <span data-ttu-id="26480-189">Popis</span><span class="sxs-lookup"><span data-stu-id="26480-189">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="26480-190">Typ</span><span class="sxs-lookup"><span data-stu-id="26480-190">Type</span></span> | <span data-ttu-id="26480-191">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-191">Yes</span></span> | <span data-ttu-id="26480-192">Typ akce.</span><span class="sxs-lookup"><span data-stu-id="26480-192">Type of the action.</span></span>  <span data-ttu-id="26480-193">Bude jím **výstraha** pro akce výstrah.</span><span class="sxs-lookup"><span data-stu-id="26480-193">This will be **Alert** for alert actions.</span></span> |
| <span data-ttu-id="26480-194">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="26480-194">Name</span></span> | <span data-ttu-id="26480-195">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-195">Yes</span></span> | <span data-ttu-id="26480-196">Zobrazovaný název výstrahy.</span><span class="sxs-lookup"><span data-stu-id="26480-196">Display name for the alert.</span></span>  <span data-ttu-id="26480-197">Toto je název, který se zobrazí v konzole pro pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="26480-197">This is the name that's displayed in the console for the alert rule.</span></span> |
| <span data-ttu-id="26480-198">Popis</span><span class="sxs-lookup"><span data-stu-id="26480-198">Description</span></span> | <span data-ttu-id="26480-199">Ne</span><span class="sxs-lookup"><span data-stu-id="26480-199">No</span></span> | <span data-ttu-id="26480-200">Volitelný popis výstrahy.</span><span class="sxs-lookup"><span data-stu-id="26480-200">Optional description of the alert.</span></span> |
| <span data-ttu-id="26480-201">Závažnost</span><span class="sxs-lookup"><span data-stu-id="26480-201">Severity</span></span> | <span data-ttu-id="26480-202">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-202">Yes</span></span> | <span data-ttu-id="26480-203">Závažnost výstrahy záznam z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="26480-203">Severity of the alert record from the following values:</span></span><br><br> <span data-ttu-id="26480-204">**Kritické**</span><span class="sxs-lookup"><span data-stu-id="26480-204">**Critical**</span></span><br><span data-ttu-id="26480-205">**Upozornění**</span><span class="sxs-lookup"><span data-stu-id="26480-205">**Warning**</span></span><br><span data-ttu-id="26480-206">**Informační**</span><span class="sxs-lookup"><span data-stu-id="26480-206">**Informational**</span></span> |


##### <a name="threshold"></a><span data-ttu-id="26480-207">Prahová hodnota</span><span class="sxs-lookup"><span data-stu-id="26480-207">Threshold</span></span>
<span data-ttu-id="26480-208">Tento oddíl je povinný.</span><span class="sxs-lookup"><span data-stu-id="26480-208">This section is required.</span></span>  <span data-ttu-id="26480-209">Definuje vlastnosti prahové hodnoty pro výstrahu.</span><span class="sxs-lookup"><span data-stu-id="26480-209">It defines the properties for the alert threshold.</span></span>

| <span data-ttu-id="26480-210">Název elementu</span><span class="sxs-lookup"><span data-stu-id="26480-210">Element name</span></span> | <span data-ttu-id="26480-211">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="26480-211">Required</span></span> | <span data-ttu-id="26480-212">Popis</span><span class="sxs-lookup"><span data-stu-id="26480-212">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="26480-213">Operátor</span><span class="sxs-lookup"><span data-stu-id="26480-213">Operator</span></span> | <span data-ttu-id="26480-214">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-214">Yes</span></span> | <span data-ttu-id="26480-215">Operátor pro porovnání z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="26480-215">Operator for the comparison from the following values:</span></span><br><br><span data-ttu-id="26480-216">**gt = větší než<br>lt = menší než**</span><span class="sxs-lookup"><span data-stu-id="26480-216">**gt = greater than<br>lt = less than**</span></span> |
| <span data-ttu-id="26480-217">Hodnota</span><span class="sxs-lookup"><span data-stu-id="26480-217">Value</span></span> | <span data-ttu-id="26480-218">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-218">Yes</span></span> | <span data-ttu-id="26480-219">Hodnoty k porovnání výsledků.</span><span class="sxs-lookup"><span data-stu-id="26480-219">The value to compare the results.</span></span> |


##### <a name="metricstrigger"></a><span data-ttu-id="26480-220">MetricsTrigger</span><span class="sxs-lookup"><span data-stu-id="26480-220">MetricsTrigger</span></span>
<span data-ttu-id="26480-221">Tato část je volitelné.</span><span class="sxs-lookup"><span data-stu-id="26480-221">This section is optional.</span></span>  <span data-ttu-id="26480-222">Zahrňte pro upozornění na metriky měření.</span><span class="sxs-lookup"><span data-stu-id="26480-222">Include it for a metric measurement alert.</span></span>

> [!NOTE]
> <span data-ttu-id="26480-223">Metriky měření výstrahy jsou aktuálně ve verzi public preview.</span><span class="sxs-lookup"><span data-stu-id="26480-223">Metric measurement alerts are currently in public preview.</span></span> 

| <span data-ttu-id="26480-224">Název elementu</span><span class="sxs-lookup"><span data-stu-id="26480-224">Element name</span></span> | <span data-ttu-id="26480-225">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="26480-225">Required</span></span> | <span data-ttu-id="26480-226">Popis</span><span class="sxs-lookup"><span data-stu-id="26480-226">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="26480-227">TriggerCondition</span><span class="sxs-lookup"><span data-stu-id="26480-227">TriggerCondition</span></span> | <span data-ttu-id="26480-228">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-228">Yes</span></span> | <span data-ttu-id="26480-229">Určuje, zda prahová hodnota pro celkový počet narušení nebo po sobě jdoucích narušení z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="26480-229">Specifies whether the threshold is for total number of breaches or consecutive breaches from the following values:</span></span><br><br><span data-ttu-id="26480-230">**Celkový počet<br>po sobě jdoucích**</span><span class="sxs-lookup"><span data-stu-id="26480-230">**Total<br>Consecutive**</span></span> |
| <span data-ttu-id="26480-231">Operátor</span><span class="sxs-lookup"><span data-stu-id="26480-231">Operator</span></span> | <span data-ttu-id="26480-232">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-232">Yes</span></span> | <span data-ttu-id="26480-233">Operátor pro porovnání z následujících hodnot:</span><span class="sxs-lookup"><span data-stu-id="26480-233">Operator for the comparison from the following values:</span></span><br><br><span data-ttu-id="26480-234">**gt = větší než<br>lt = menší než**</span><span class="sxs-lookup"><span data-stu-id="26480-234">**gt = greater than<br>lt = less than**</span></span> |
| <span data-ttu-id="26480-235">Hodnota</span><span class="sxs-lookup"><span data-stu-id="26480-235">Value</span></span> | <span data-ttu-id="26480-236">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-236">Yes</span></span> | <span data-ttu-id="26480-237">Počet přístupů, které musí být splněna kritéria pro aktivování upozornění.</span><span class="sxs-lookup"><span data-stu-id="26480-237">Number of the times the criteria must be met to trigger the alert.</span></span> |

##### <a name="throttling"></a><span data-ttu-id="26480-238">Omezování</span><span class="sxs-lookup"><span data-stu-id="26480-238">Throttling</span></span>
<span data-ttu-id="26480-239">Tato část je volitelné.</span><span class="sxs-lookup"><span data-stu-id="26480-239">This section is optional.</span></span>  <span data-ttu-id="26480-240">Pokud chcete pro potlačení výstrahy ze stejného pravidla pro určitou dobu, po vytvoření výstrahy, zahrnují v této části.</span><span class="sxs-lookup"><span data-stu-id="26480-240">Include this section if you want to suppress alerts from the same rule for some amount of time after an alert is created.</span></span>

| <span data-ttu-id="26480-241">Název elementu</span><span class="sxs-lookup"><span data-stu-id="26480-241">Element name</span></span> | <span data-ttu-id="26480-242">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="26480-242">Required</span></span> | <span data-ttu-id="26480-243">Popis</span><span class="sxs-lookup"><span data-stu-id="26480-243">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="26480-244">Doba trvání v minutách</span><span class="sxs-lookup"><span data-stu-id="26480-244">DurationInMinutes</span></span> | <span data-ttu-id="26480-245">Ano, pokud omezení zahrnuté – element</span><span class="sxs-lookup"><span data-stu-id="26480-245">Yes if Throttling element included</span></span> | <span data-ttu-id="26480-246">Počet minut pro potlačení výstrahy po vytvoření jedné ze stejné pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="26480-246">Number of minutes to suppress alerts after one from the same alert rule is created.</span></span> |

##### <a name="emailnotification"></a><span data-ttu-id="26480-247">EmailNotification</span><span class="sxs-lookup"><span data-stu-id="26480-247">EmailNotification</span></span>
 <span data-ttu-id="26480-248">Tato část je volitelný ho zahrňte, pokud chcete výstrahu odesílat poštu jeden nebo více příjemcům.</span><span class="sxs-lookup"><span data-stu-id="26480-248">This section is optional  Include it if you want the alert to send mail to one or more recipients.</span></span>

| <span data-ttu-id="26480-249">Název elementu</span><span class="sxs-lookup"><span data-stu-id="26480-249">Element name</span></span> | <span data-ttu-id="26480-250">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="26480-250">Required</span></span> | <span data-ttu-id="26480-251">Popis</span><span class="sxs-lookup"><span data-stu-id="26480-251">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="26480-252">Příjemce</span><span class="sxs-lookup"><span data-stu-id="26480-252">Recipients</span></span> | <span data-ttu-id="26480-253">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-253">Yes</span></span> | <span data-ttu-id="26480-254">Čárkami oddělený seznam e-mailové adresy k odesílání oznámení, pokud je výstrahy vytvořeny jako v následujícím příkladu.</span><span class="sxs-lookup"><span data-stu-id="26480-254">Comma delimited list of email addresses to send notification when an alert is created such as in the following example.</span></span><br><br><span data-ttu-id="26480-255">**[ "recipient1@contoso.com", "recipient2@contoso.com" ]**</span><span class="sxs-lookup"><span data-stu-id="26480-255">**[ "recipient1@contoso.com", "recipient2@contoso.com" ]**</span></span> |
| <span data-ttu-id="26480-256">Předmět</span><span class="sxs-lookup"><span data-stu-id="26480-256">Subject</span></span> | <span data-ttu-id="26480-257">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-257">Yes</span></span> | <span data-ttu-id="26480-258">Řádek předmětu e-mailu.</span><span class="sxs-lookup"><span data-stu-id="26480-258">Subject line of the mail.</span></span> |
| <span data-ttu-id="26480-259">Přílohy</span><span class="sxs-lookup"><span data-stu-id="26480-259">Attachment</span></span> | <span data-ttu-id="26480-260">Ne</span><span class="sxs-lookup"><span data-stu-id="26480-260">No</span></span> | <span data-ttu-id="26480-261">Přílohy nejsou aktuálně podporovány.</span><span class="sxs-lookup"><span data-stu-id="26480-261">Attachments are not currently supported.</span></span>  <span data-ttu-id="26480-262">Pokud tento element je zahrnuto, by měla být **žádné**.</span><span class="sxs-lookup"><span data-stu-id="26480-262">If this element is included, it should be **None**.</span></span> |


##### <a name="remediation"></a><span data-ttu-id="26480-263">Nápravy</span><span class="sxs-lookup"><span data-stu-id="26480-263">Remediation</span></span>
<span data-ttu-id="26480-264">Tato část je volitelný ho zahrňte, pokud chcete sadu runbook spustit v reakci na výstrahy.</span><span class="sxs-lookup"><span data-stu-id="26480-264">This section is optional  Include it if you want a runbook to start in response to the alert.</span></span> |

| <span data-ttu-id="26480-265">Název elementu</span><span class="sxs-lookup"><span data-stu-id="26480-265">Element name</span></span> | <span data-ttu-id="26480-266">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="26480-266">Required</span></span> | <span data-ttu-id="26480-267">Popis</span><span class="sxs-lookup"><span data-stu-id="26480-267">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="26480-268">RunbookName</span><span class="sxs-lookup"><span data-stu-id="26480-268">RunbookName</span></span> | <span data-ttu-id="26480-269">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-269">Yes</span></span> | <span data-ttu-id="26480-270">Název sady runbook spustit.</span><span class="sxs-lookup"><span data-stu-id="26480-270">Name of the runbook to start.</span></span> |
| <span data-ttu-id="26480-271">WebhookUri</span><span class="sxs-lookup"><span data-stu-id="26480-271">WebhookUri</span></span> | <span data-ttu-id="26480-272">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-272">Yes</span></span> | <span data-ttu-id="26480-273">Identifikátor URI služby webhooku pro sadu runbook.</span><span class="sxs-lookup"><span data-stu-id="26480-273">Uri of the webhook for the runbook.</span></span> |
| <span data-ttu-id="26480-274">Vypršení platnosti</span><span class="sxs-lookup"><span data-stu-id="26480-274">Expiry</span></span> | <span data-ttu-id="26480-275">Ne</span><span class="sxs-lookup"><span data-stu-id="26480-275">No</span></span> | <span data-ttu-id="26480-276">Datum a čas, kdy vyprší platnost náprava.</span><span class="sxs-lookup"><span data-stu-id="26480-276">Date and time that the remediation expires.</span></span> |

#### <a name="webhook-actions"></a><span data-ttu-id="26480-277">Akce Webhooku</span><span class="sxs-lookup"><span data-stu-id="26480-277">Webhook actions</span></span>

<span data-ttu-id="26480-278">Akce Webhooku spuštění procesu voláním adresu URL a volitelně poskytuje datové části k odeslání.</span><span class="sxs-lookup"><span data-stu-id="26480-278">Webhook actions start a process by calling a URL and optionally providing a payload to be sent.</span></span> <span data-ttu-id="26480-279">Jsou podobná nápravné akce s výjimkou jsou určené pro webhooků, který může vyvolat procesy než Azure Automation runbook.</span><span class="sxs-lookup"><span data-stu-id="26480-279">They are similar to Remediation actions except they are meant for webhooks that may invoke processes other than Azure Automation runbooks.</span></span> <span data-ttu-id="26480-280">Obsahují taky další možnost poskytnout datové části který bude doručen do vzdálený proces.</span><span class="sxs-lookup"><span data-stu-id="26480-280">They also provide the additional option of providing a payload to be delivered to the remote process.</span></span>

<span data-ttu-id="26480-281">Pokud upozornění bude volat webhook, jehož, pak bude je nutné prostředek akce s typem **Webhooku** kromě **výstraha** akce prostředků.</span><span class="sxs-lookup"><span data-stu-id="26480-281">If your alert will call a webhook, then it will need an action resource with a type of **Webhook** in addition to the **Alert** action resource.</span></span>  

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

<span data-ttu-id="26480-282">Vlastnosti Webhooku akce prostředky jsou popsané v následujících tabulkách.</span><span class="sxs-lookup"><span data-stu-id="26480-282">The properties for Webhook action resources are described in the following tables.</span></span>

| <span data-ttu-id="26480-283">Název elementu</span><span class="sxs-lookup"><span data-stu-id="26480-283">Element name</span></span> | <span data-ttu-id="26480-284">Požaduje se</span><span class="sxs-lookup"><span data-stu-id="26480-284">Required</span></span> | <span data-ttu-id="26480-285">Popis</span><span class="sxs-lookup"><span data-stu-id="26480-285">Description</span></span> |
|:--|:--|:--|
| <span data-ttu-id="26480-286">type</span><span class="sxs-lookup"><span data-stu-id="26480-286">type</span></span> | <span data-ttu-id="26480-287">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-287">Yes</span></span> | <span data-ttu-id="26480-288">Typ akce.</span><span class="sxs-lookup"><span data-stu-id="26480-288">Type of the action.</span></span>  <span data-ttu-id="26480-289">Bude jím **Webhooku** pro akce webhooku.</span><span class="sxs-lookup"><span data-stu-id="26480-289">This will be **Webhook** for webhook actions.</span></span> |
| <span data-ttu-id="26480-290">jméno</span><span class="sxs-lookup"><span data-stu-id="26480-290">name</span></span> | <span data-ttu-id="26480-291">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-291">Yes</span></span> | <span data-ttu-id="26480-292">Zobrazovaný název akce.</span><span class="sxs-lookup"><span data-stu-id="26480-292">Display name for the action.</span></span>  <span data-ttu-id="26480-293">To se nezobrazí v konzole.</span><span class="sxs-lookup"><span data-stu-id="26480-293">This is not displayed in the console.</span></span> |
| <span data-ttu-id="26480-294">wehookUri</span><span class="sxs-lookup"><span data-stu-id="26480-294">wehookUri</span></span> | <span data-ttu-id="26480-295">Ano</span><span class="sxs-lookup"><span data-stu-id="26480-295">Yes</span></span> | <span data-ttu-id="26480-296">Identifikátor URI pro webhook.</span><span class="sxs-lookup"><span data-stu-id="26480-296">Uri for the webhook.</span></span> |
| <span data-ttu-id="26480-297">CustomPayload</span><span class="sxs-lookup"><span data-stu-id="26480-297">customPayload</span></span> | <span data-ttu-id="26480-298">Ne</span><span class="sxs-lookup"><span data-stu-id="26480-298">No</span></span> | <span data-ttu-id="26480-299">Vlastní datovou část k odeslání do webhooku.</span><span class="sxs-lookup"><span data-stu-id="26480-299">Custom payload to be sent to the webhook.</span></span> <span data-ttu-id="26480-300">Formát bude záviset na co webhooku očekává.</span><span class="sxs-lookup"><span data-stu-id="26480-300">The format will depend on what the webhook is expecting.</span></span> |




## <a name="sample"></a><span data-ttu-id="26480-301">Ukázka</span><span class="sxs-lookup"><span data-stu-id="26480-301">Sample</span></span>

<span data-ttu-id="26480-302">Tady je ukázka řešení, které zahrnují, který obsahuje následující zdroje:</span><span class="sxs-lookup"><span data-stu-id="26480-302">Following is a sample of a solution that include that includes the following resources:</span></span>

- <span data-ttu-id="26480-303">Uložené hledání</span><span class="sxs-lookup"><span data-stu-id="26480-303">Saved search</span></span>
- <span data-ttu-id="26480-304">Plán</span><span class="sxs-lookup"><span data-stu-id="26480-304">Schedule</span></span>
- <span data-ttu-id="26480-305">Akce oznámení</span><span class="sxs-lookup"><span data-stu-id="26480-305">Alert action</span></span>
- <span data-ttu-id="26480-306">Akce Webhooku</span><span class="sxs-lookup"><span data-stu-id="26480-306">Webhook action</span></span>

<span data-ttu-id="26480-307">Příklad používá [standardní řešení parametry](operations-management-suite-solutions-solution-file.md#parameters) proměnné, které by běžně používané v řešení oproti hardcoding hodnoty v definicích prostředků.</span><span class="sxs-lookup"><span data-stu-id="26480-307">The sample uses [standard solution parameters](operations-management-suite-solutions-solution-file.md#parameters) variables that would commonly be used in a solution as opposed to hardcoding values in the resource definitions.</span></span>

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
              "Description": "List of recipients for the email alert separated by semicolon"
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


<span data-ttu-id="26480-308">Následující soubor parametrů obsahuje hodnoty ukázky pro toto řešení.</span><span class="sxs-lookup"><span data-stu-id="26480-308">The following parameter file provides samples values for this solution.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="26480-309">Další kroky</span><span class="sxs-lookup"><span data-stu-id="26480-309">Next steps</span></span>
* <span data-ttu-id="26480-310">[Přidání zobrazení](operations-management-suite-solutions-resources-views.md) do řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="26480-310">[Add views](operations-management-suite-solutions-resources-views.md) to your management solution.</span></span>
* <span data-ttu-id="26480-311">[Přidat runbooků služeb automatizace a dalším prostředkům](operations-management-suite-solutions-resources-automation.md) do řešení pro správu.</span><span class="sxs-lookup"><span data-stu-id="26480-311">[Add Automation runbooks and other resources](operations-management-suite-solutions-resources-automation.md) to your management solution.</span></span>

