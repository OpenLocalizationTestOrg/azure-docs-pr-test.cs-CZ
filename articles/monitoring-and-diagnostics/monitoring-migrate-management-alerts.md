---
title: "aaaMigrate Azure výstrahy na události Management tooActivity protokolu výstrahy | Microsoft Docs"
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
ms.openlocfilehash: e00bc4f0bad4e8f97443310770c333d250e343ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-azure-alerts-on-management-events-tooactivity-log-alerts"></a><span data-ttu-id="6c5eb-104">Migrace Azure výstrahy na správu události tooActivity protokolu výstrahy</span><span class="sxs-lookup"><span data-stu-id="6c5eb-104">Migrate Azure alerts on management events tooActivity Log alerts</span></span>


> [!WARNING]
> <span data-ttu-id="6c5eb-105">Výstrahy na události správy se vypne na nebo po říjen 1.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-105">Alerts on management events will be turned off on or after October 1.</span></span> <span data-ttu-id="6c5eb-106">Pokud máte tyto výstrahy a jejich migrací, pokud ano, použijte hello pokynů níže toounderstand.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-106">Use hello directions below toounderstand if you have these alerts and migrate them if so.</span></span>
>
> 

## <a name="what-is-changing"></a><span data-ttu-id="6c5eb-107">Co je změna</span><span class="sxs-lookup"><span data-stu-id="6c5eb-107">What is changing</span></span>

<span data-ttu-id="6c5eb-108">Azure monitorování (dříve Statistika Azure) nabízí možnost toocreate výstrahu, která aktivuje z událostí správy a vygeneruje oznámení tooa webhooku adresu URL nebo e-mailové adresy.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-108">Azure Monitor (formerly Azure Insights) offered a capability toocreate an alert that triggered off of management events and generated notifications tooa webhook URL or email addresses.</span></span> <span data-ttu-id="6c5eb-109">Jste vytvořili jednu z těchto výstrah některý z těchto způsobů:</span><span class="sxs-lookup"><span data-stu-id="6c5eb-109">You may have created one of these alerts any of these ways:</span></span>
* <span data-ttu-id="6c5eb-110">V hello portál Azure pro určité typy prostředků, v části monitorování -> Výstrahy -> Přidat výstrahy, kde "Výstrah na" je nastaven příliš "Události"</span><span class="sxs-lookup"><span data-stu-id="6c5eb-110">In hello Azure portal for certain resource types, under Monitoring -> Alerts -> Add Alert, where “Alert on” is set too“Events”</span></span>
* <span data-ttu-id="6c5eb-111">Rutiny prostředí PowerShell přidat AzureRmLogAlertRule spuštěné hello</span><span class="sxs-lookup"><span data-stu-id="6c5eb-111">By running hello Add-AzureRmLogAlertRule PowerShell cmdlet</span></span>
* <span data-ttu-id="6c5eb-112">Přímo pomocí [hello výstrahy REST API](http://docs.microsoft.com/rest/api/monitor/alertrules) s odata.type = "ManagementEventRuleCondition" a dataSource.odata.type = "RuleManagementEventDataSource"</span><span class="sxs-lookup"><span data-stu-id="6c5eb-112">By directly using [hello alert REST API](http://docs.microsoft.com/rest/api/monitor/alertrules) with odata.type = “ManagementEventRuleCondition” and dataSource.odata.type = “RuleManagementEventDataSource”</span></span>
 
<span data-ttu-id="6c5eb-113">Hello následující skript prostředí PowerShell vrátí seznam všech výstrah událostí správy, které máte v vaše předplatné, jakož i podmínky hello nastavit pro jednotlivé výstrahy.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-113">hello following PowerShell script returns a list of all alerts on management events that you have in your subscription, as well as hello conditions set on each alert.</span></span>

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

<span data-ttu-id="6c5eb-114">Pokud máte žádné výstrahy týkající se správy událostí, bude výstup rutiny prostředí PowerShell hello výše řadu zprávy upozornění podobné následujícímu:</span><span class="sxs-lookup"><span data-stu-id="6c5eb-114">If you have no alerts on management events, hello PowerShell cmdlet above will output a series of warning messages like this one:</span></span>

`WARNING: hello output of this cmdlet will be flattened, i.e. elimination of hello properties field, in a future release tooimprove hello user experience.`

<span data-ttu-id="6c5eb-115">Tyto zprávy upozornění můžete ignorovat.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-115">These warning messages can be ignored.</span></span> <span data-ttu-id="6c5eb-116">Pokud máte výstrahy na události správy, bude výstup hello tuto rutinu prostředí PowerShell vypadat takto:</span><span class="sxs-lookup"><span data-stu-id="6c5eb-116">If you do have alerts on management events, hello output of this PowerShell cmdlet will look like this:</span></span>

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

<span data-ttu-id="6c5eb-117">Každá výstraha je oddělená na přerušovanou čáru a podrobnosti zahrnují ID prostředku hello hello výstrahy a hello konkrétní pravidlo monitorovány.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-117">Each alert is separated by a dashed line and details include hello resource ID of hello alert and hello specific rule being monitored.</span></span>

<span data-ttu-id="6c5eb-118">Tato funkce příliš přešla[Azure monitorování aktivity protokolu výstrah](monitoring-activity-log-alerts.md).</span><span class="sxs-lookup"><span data-stu-id="6c5eb-118">This functionality has been transitioned too[Azure Monitor Activity Log Alerts](monitoring-activity-log-alerts.md).</span></span> <span data-ttu-id="6c5eb-119">Tyto nové výstrahy umožňují tooset podmínkou na aktivity protokolu události a přijímat oznámení, když o novou událost odpovídá hello podmínku.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-119">These new alerts enable you tooset a condition on Activity Log events and receive a notification when a new event matches hello condition.</span></span> <span data-ttu-id="6c5eb-120">Také nabízí několik vylepšení z výstrahy na události správy:</span><span class="sxs-lookup"><span data-stu-id="6c5eb-120">They also offer several improvements from alerts on management events:</span></span>
* <span data-ttu-id="6c5eb-121">Můžete opakovaně použít skupině příjemců oznámení ("akce") napříč více výstrah pomocí [skupiny akcí](monitoring-action-groups.md), snižuje složitost hello změny příjemce výstrahu.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-121">You can reuse your group of notification recipients (“actions”) across many alerts using [Action Groups](monitoring-action-groups.md), reducing hello complexity of changing who should receive an alert.</span></span>
* <span data-ttu-id="6c5eb-122">Můžete obdržet oznámení přímo na váš telefon SMS pomocí akce skupiny.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-122">You can receive a notification directly on your phone using SMS with Action Groups.</span></span>
* <span data-ttu-id="6c5eb-123">Můžete [vytvářet výstrahy, aktivity protokolu pomocí šablony Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span><span class="sxs-lookup"><span data-stu-id="6c5eb-123">You can [create Activity Log Alerts with Resource Manager templates](monitoring-create-activity-log-alerts-with-resource-manager-template.md).</span></span>
* <span data-ttu-id="6c5eb-124">Podmínky lze vytvořit s větší flexibilitu a složitost toomeet svých konkrétních potřeb.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-124">You can create conditions with greater flexibility and complexity toomeet your specific needs.</span></span>
* <span data-ttu-id="6c5eb-125">Oznámení se doručují rychleji.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-125">Notifications are delivered more quickly.</span></span>
 
## <a name="how-toomigrate"></a><span data-ttu-id="6c5eb-126">Jak toomigrate</span><span class="sxs-lookup"><span data-stu-id="6c5eb-126">How toomigrate</span></span>
 
<span data-ttu-id="6c5eb-127">toocreate oznámení nové aktivity na protokolu, můžete buď:</span><span class="sxs-lookup"><span data-stu-id="6c5eb-127">toocreate a new Activity Log Alert, you can either:</span></span>
* <span data-ttu-id="6c5eb-128">Postupujte podle [našem návodu na tom, jak hello toocreate výstraha v portálu Azure](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="6c5eb-128">Follow [our guide on how toocreate an alert in hello Azure portal](monitoring-activity-log-alerts.md)</span></span>
* <span data-ttu-id="6c5eb-129">Zjistěte, jak příliš[vytvořena výstraha pomocí šablony Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="6c5eb-129">Learn how too[create an alert using a Resource Manager template](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span></span>
 
<span data-ttu-id="6c5eb-130">Výstrahy na události správy, které jste předtím vytvořili nebudou výstrahy automaticky migrované tooActivity protokolu.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-130">Alerts on management events that you have previously created will not be automatically migrated tooActivity Log Alerts.</span></span> <span data-ttu-id="6c5eb-131">Je nutné toouse hello předcházející výstrahy hello toolist skript prostředí PowerShell na správu události jste nakonfigurovali a ručně je znovu vytvořit jako aktivity protokolu výstrahy.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-131">You need toouse hello preceding PowerShell script toolist hello alerts on management events that you currently have configured and manually recreate them as Activity Log Alerts.</span></span> <span data-ttu-id="6c5eb-132">To je třeba provést před říjen 1, po jejímž uplynutí výstrahy na události správy se už nebude zobrazovat ve vašem předplatném Azure.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-132">This must be done before October 1, after which alerts on management events will no longer be visible in your Azure subscription.</span></span> <span data-ttu-id="6c5eb-133">Jiné typy výstrah, Azure, včetně monitorování Azure metriky výstrah, Application Insights výstrahy a upozornění analýzy protokolů jsou tato změna nemá vliv.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-133">Other types of Azure alerts, including Azure Monitor metric alerts, Application Insights alerts, and Log Analytics alerts are unaffected by this change.</span></span> <span data-ttu-id="6c5eb-134">Pokud máte nějaké otázky, post v komentářích hello níže.</span><span class="sxs-lookup"><span data-stu-id="6c5eb-134">If you have any questions, post in hello comments below.</span></span>


## <a name="next-steps"></a><span data-ttu-id="6c5eb-135">Další kroky</span><span class="sxs-lookup"><span data-stu-id="6c5eb-135">Next steps</span></span>

* <span data-ttu-id="6c5eb-136">Další informace o [protokol aktivit](monitoring-overview-activity-logs.md)</span><span class="sxs-lookup"><span data-stu-id="6c5eb-136">Learn more about [Activity Log](monitoring-overview-activity-logs.md)</span></span>
* <span data-ttu-id="6c5eb-137">Konfigurace [aktivity protokolu výstrahy prostřednictvím portálu Azure](monitoring-activity-log-alerts.md)</span><span class="sxs-lookup"><span data-stu-id="6c5eb-137">Configure [Activity Log Alerts via Azure portal](monitoring-activity-log-alerts.md)</span></span>
* <span data-ttu-id="6c5eb-138">Konfigurace [aktivity protokolu výstrahy prostřednictvím Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span><span class="sxs-lookup"><span data-stu-id="6c5eb-138">Configure [Activity Log Alerts via Resource Manager](monitoring-create-activity-log-alerts-with-resource-manager-template.md)</span></span>
* <span data-ttu-id="6c5eb-139">Zkontrolujte hello [schématu výstrahy webhooku protokolu aktivit](monitoring-activity-log-alerts-webhook.md)</span><span class="sxs-lookup"><span data-stu-id="6c5eb-139">Review hello [activity log alert webhook schema](monitoring-activity-log-alerts-webhook.md)</span></span>
* <span data-ttu-id="6c5eb-140">Další informace o [oznámení o službách](monitoring-service-notifications.md)</span><span class="sxs-lookup"><span data-stu-id="6c5eb-140">Learn more about [Service Notifications](monitoring-service-notifications.md)</span></span>
* <span data-ttu-id="6c5eb-141">Další informace o [skupiny akcí](monitoring-action-groups.md)</span><span class="sxs-lookup"><span data-stu-id="6c5eb-141">Learn more about [Action Groups](monitoring-action-groups.md)</span></span>
