---
title: "Migrace Azure výstrahy na události správy do protokolu upozornění | Microsoft Docs"
description: "Výstrahy na události správy budou odebrány říjen 1. Připravte migraci existujícího výstrahy."
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
ms.date: 08/14/2017
ms.author: johnkem
ms.openlocfilehash: 08a457029d3721f5c38dbcd2d2aab7d09a241d8f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="migrate-azure-alerts-on-management-events-to-activity-log-alerts"></a><span data-ttu-id="dcd19-104">Migrace Azure výstrahy na události správy do protokolu činnosti výstrahy</span><span class="sxs-lookup"><span data-stu-id="dcd19-104">Migrate Azure alerts on management events to Activity Log alerts</span></span>


> [!WARNING]
> <span data-ttu-id="dcd19-105">Výstrahy na události správy se vypne na nebo po říjen 1.</span><span class="sxs-lookup"><span data-stu-id="dcd19-105">Alerts on management events will be turned off on or after October 1.</span></span> <span data-ttu-id="dcd19-106">Následující pokyny slouží k pochopení, pokud máte tyto výstrahy a migraci, je-li tomu tak.</span><span class="sxs-lookup"><span data-stu-id="dcd19-106">Use the directions below to understand if you have these alerts and migrate them if so.</span></span>
>
> 

## <a name="what-is-changing"></a><span data-ttu-id="dcd19-107">Co je změna</span><span class="sxs-lookup"><span data-stu-id="dcd19-107">What is changing</span></span>

<span data-ttu-id="dcd19-108">Azure monitorování (dříve Statistika Azure) nabízí možnost vytvořit výstrahu, která aktivuje z událostí správy a vygeneruje oznámení na webhooku adresu URL nebo e-mailové adresy.</span><span class="sxs-lookup"><span data-stu-id="dcd19-108">Azure Monitor (formerly Azure Insights) offered a capability to create an alert that triggered off of management events and generated notifications to a webhook URL or email addresses.</span></span> <span data-ttu-id="dcd19-109">Jste vytvořili jednu z těchto výstrah některý z těchto způsobů:</span><span class="sxs-lookup"><span data-stu-id="dcd19-109">You may have created one of these alerts any of these ways:</span></span>
* <span data-ttu-id="dcd19-110">Na portálu Azure pro určité typy prostředků v části monitorování -> Výstrahy -> Přidat výstrahy, kde "Výstrah na" je nastaven na "Události"</span><span class="sxs-lookup"><span data-stu-id="dcd19-110">In the Azure portal for certain resource types, under Monitoring -> Alerts -> Add Alert, where “Alert on” is set to “Events”</span></span>
* <span data-ttu-id="dcd19-111">Spuštěním rutiny Add-AzureRmLogAlertRule prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="dcd19-111">By running the Add-AzureRmLogAlertRule PowerShell cmdlet</span></span>
* <span data-ttu-id="dcd19-112">Přímo pomocí [výstrahy REST API](http://docs.microsoft.com/rest/api/monitor/alertrules) s odata.type = "ManagementEventRuleCondition" a dataSource.odata.type = "RuleManagementEventDataSource"</span><span class="sxs-lookup"><span data-stu-id="dcd19-112">By directly using [the alert REST API](http://docs.microsoft.com/rest/api/monitor/alertrules) with odata.type = “ManagementEventRuleCondition” and dataSource.odata.type = “RuleManagementEventDataSource”</span></span>
 
<span data-ttu-id="dcd19-113">Následující skript prostředí PowerShell vrátí seznam všech výstrah v události správy, které máte v vašeho předplatného, jakož i podmínky nastavit pro jednotlivé výstrahy.</span><span class="sxs-lookup"><span data-stu-id="dcd19-113">The following PowerShell script returns a list of all alerts on management events that you have in your subscription, as well as the conditions set on each alert.</span></span>

```powershell
Login-AzureRmAccount
$alerts = $null
foreach ($rg in Get-AzureRmResourceGroup ) {
  $alerts += Get-AzureRmAlertRule -ResourceGroup $rg.ResourceGroupName
}
foreach ($alert in $alerts) {
  if($alert.Properties.Condition.DataSource.GetType().Name.Equals("RuleManagementEventDataSource")) {
    "Alert Name: " + $alert.Name
    "Alert Resource ID: " + $alert.Id
    "Alert conditions:"
    $alert.Properties.Condition.DataSource
    "---------------------------------"
  }
} 
```

<span data-ttu-id="dcd19-114">Pokud máte žádné výstrahy týkající se správy událostí, výše uvedené rutiny prostředí PowerShell výstup řadu zprávy upozornění podobné následujícímu:</span><span class="sxs-lookup"><span data-stu-id="dcd19-114">If you have no alerts on management events, the PowerShell cmdlet above will output a series of warning messages like this one:</span></span>

`WARNING: The output of this cmdlet will be flattened, i.e. elimination of the properties field, in a future release to improve the user experience.`

<span data-ttu-id="dcd19-115">Tyto zprávy upozornění můžete ignorovat.</span><span class="sxs-lookup"><span data-stu-id="dcd19-115">These warning messages can be ignored.</span></span> <span data-ttu-id="dcd19-116">Pokud máte výstrahy na události správy, bude výstup této rutiny prostředí PowerShell vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="dcd19-116">If you do have alerts on management events, the output of this PowerShell cmdlet will look like this:</span></span>

```
Alert Name: webhookEvent1
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/webhookEvent1
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : microsoft.web/sites/start/action
ResourceGroupName    : 
ResourceProviderName : 
Status               : succeeded
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Web/sites/samplealertapp

---------------------------------
Alert Name: someclilogalert
Alert Resource ID: /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/microsoft.insights/alertrules/someclilogalert
Alert conditions:

EventName            : 
EventSource          : 
Level                : 
OperationName        : Start
ResourceGroupName    : 
ResourceProviderName : 
Status               : 
SubStatus            : 
Claims               : Microsoft.Azure.Management.Monitor.Management.Models.RuleManagementEventClaimsDataSource
ResourceUri          : /subscriptions/<subscription-id>/resourceGroups/<resourcegroup-name>/providers/Microsoft.Compute/virtualMachines/Seaofclouds

---------------------------------
```

<span data-ttu-id="dcd19-117">Každá výstraha je oddělená na přerušovanou čáru a podrobnosti zahrnují ID prostředku výstraha a konkrétní pravidlo, které jsou monitorovány.</span><span class="sxs-lookup"><span data-stu-id="dcd19-117">Each alert is separated by a dashed line and details include the resource ID of the alert and the specific rule being monitored.</span></span>

<span data-ttu-id="dcd19-118">Tato funkce přešla do [Azure monitorování aktivity protokolu výstrah](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="dcd19-118">This functionality has been transitioned to [Azure Monitor Activity Log Alerts](monitoring-activity-log-alerts.md).</span></span> <span data-ttu-id="dcd19-119">Tyto nové výstrahy umožňují nastavit stav aktivity protokolu událostí a přijímat oznámení, když o novou událost odpovídá podmínku.</span><span class="sxs-lookup"><span data-stu-id="dcd19-119">These new alerts enable you to set a condition on Activity Log events and receive a notification when a new event matches the condition.</span></span> <span data-ttu-id="dcd19-120">Také nabízí několik vylepšení z výstrahy na události správy:</span><span class="sxs-lookup"><span data-stu-id="dcd19-120">They also offer several improvements from alerts on management events:</span></span>
* <span data-ttu-id="dcd19-121">Můžete opakovaně použít skupině příjemců oznámení ("akce") napříč více výstrah pomocí [skupiny akcí](monitoring-action-groups.md), snižuje složitost změny příjemce výstrahu.</span><span class="sxs-lookup"><span data-stu-id="dcd19-121">You can reuse your group of notification recipients (“actions”) across many alerts using [Action Groups](monitoring-action-groups.md), reducing the complexity of changing who should receive an alert.</span></span>
* <span data-ttu-id="dcd19-122">Můžete obdržet oznámení přímo na váš telefon SMS pomocí akce skupiny.</span><span class="sxs-lookup"><span data-stu-id="dcd19-122">You can receive a notification directly on your phone using SMS with Action Groups.</span></span>
* <span data-ttu-id="dcd19-123">Můžete [vytvářet výstrahy, aktivity protokolu pomocí šablony Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="dcd19-123">You can [create Activity Log Alerts with Resource Manager templates](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span></span>
* <span data-ttu-id="dcd19-124">Podmínky lze vytvořit s větší flexibilitu a složitost tak, aby vyhovovala vašim konkrétním potřebám.</span><span class="sxs-lookup"><span data-stu-id="dcd19-124">You can create conditions with greater flexibility and complexity to meet your specific needs.</span></span>
* <span data-ttu-id="dcd19-125">Oznámení se doručují rychleji.</span><span class="sxs-lookup"><span data-stu-id="dcd19-125">Notifications are delivered more quickly.</span></span>
 
## <a name="how-to-migrate"></a><span data-ttu-id="dcd19-126">Postup migrace</span><span class="sxs-lookup"><span data-stu-id="dcd19-126">How to migrate</span></span>
 
<span data-ttu-id="dcd19-127">Pokud chcete vytvořit oznámení nové aktivity na protokolu, můžete buď:</span><span class="sxs-lookup"><span data-stu-id="dcd19-127">To create a new Activity Log Alert, you can either:</span></span>
* <span data-ttu-id="dcd19-128">Postupujte podle [našem návodu o tom, jak vytvořit upozornění na portálu Azure](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="dcd19-128">Follow [our guide on how to create an alert in the Azure portal](monitoring-activity-log-alerts.md)</span></span>
* <span data-ttu-id="dcd19-129">Zjistěte, jak [vytvořena výstraha pomocí šablony Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="dcd19-129">Learn how to [create an alert using a Resource Manager template](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span></span>
 
<span data-ttu-id="dcd19-130">Výstrahy na události správy, které jste předtím vytvořili, nebude se migrovat automaticky aktivity protokolu výstrah.</span><span class="sxs-lookup"><span data-stu-id="dcd19-130">Alerts on management events that you have previously created will not be automatically migrated to Activity Log Alerts.</span></span> <span data-ttu-id="dcd19-131">Budete muset použít předchozí skript prostředí PowerShell k zobrazení seznamu výstrahy na události správy jste nakonfigurovali a ručně je znovu vytvořit jako aktivity protokolu výstrahy.</span><span class="sxs-lookup"><span data-stu-id="dcd19-131">You need to use the preceding PowerShell script to list the alerts on management events that you currently have configured and manually recreate them as Activity Log Alerts.</span></span> <span data-ttu-id="dcd19-132">To je třeba provést před říjen 1, po jejímž uplynutí výstrahy na události správy se už nebude zobrazovat ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="dcd19-132">This must be done before October 1, after which alerts on management events will no longer be visible in your Azure subscription.</span></span> <span data-ttu-id="dcd19-133">Jiné typy výstrah, Azure, včetně monitorování Azure metriky výstrah, Application Insights výstrahy a upozornění analýzy protokolů jsou tato změna nemá vliv.</span><span class="sxs-lookup"><span data-stu-id="dcd19-133">Other types of Azure alerts, including Azure Monitor metric alerts, Application Insights alerts, and Log Analytics alerts are unaffected by this change.</span></span> <span data-ttu-id="dcd19-134">Pokud máte nějaké otázky, post v komentářích níže.</span><span class="sxs-lookup"><span data-stu-id="dcd19-134">If you have any questions, post in the comments below.</span></span>


## <a name="next-steps"></a><span data-ttu-id="dcd19-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="dcd19-135">Next steps</span></span>

* <span data-ttu-id="dcd19-136">Další informace o [protokol aktivit](monitoring-overview-activity-logs.md)</span><span class="sxs-lookup"><span data-stu-id="dcd19-136">Learn more about [Activity Log](monitoring-overview-activity-logs.md)</span></span>
* <span data-ttu-id="dcd19-137">Konfigurace [aktivity protokolu výstrahy prostřednictvím portálu Azure](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="dcd19-137">Configure [Activity Log Alerts via Azure portal](monitoring-activity-log-alerts.md)</span></span>
* <span data-ttu-id="dcd19-138">Konfigurace [aktivity protokolu výstrahy prostřednictvím Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="dcd19-138">Configure [Activity Log Alerts via Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span></span>
* <span data-ttu-id="dcd19-139">Zkontrolujte [schématu výstrahy webhooku protokolu aktivit](monitoring-activity-log-alerts-webhook.md)</span><span class="sxs-lookup"><span data-stu-id="dcd19-139">Review the [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md)</span></span>
* <span data-ttu-id="dcd19-140">Další informace o [oznámení o službách](monitoring-service-notifications.md)</span><span class="sxs-lookup"><span data-stu-id="dcd19-140">Learn more about [Service Notifications](monitoring-service-notifications.md)</span></span>
* <span data-ttu-id="dcd19-141">Další informace o [skupiny akcí](monitoring-action-groups.md)</span><span class="sxs-lookup"><span data-stu-id="dcd19-141">Learn more about [Action Groups](monitoring-action-groups.md)</span></span>
