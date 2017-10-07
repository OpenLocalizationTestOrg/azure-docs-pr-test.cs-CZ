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
# <a name="migrate-azure-alerts-on-management-events-tooactivity-log-alerts"></a>Migrace Azure výstrahy na správu události tooActivity protokolu výstrahy


> [!WARNING]
> Výstrahy na události správy se vypne na nebo po říjen 1. Pokud máte tyto výstrahy a jejich migrací, pokud ano, použijte hello pokynů níže toounderstand.
>
> 

## <a name="what-is-changing"></a>Co je změna

Azure monitorování (dříve Statistika Azure) nabízí možnost toocreate výstrahu, která aktivuje z událostí správy a vygeneruje oznámení tooa webhooku adresu URL nebo e-mailové adresy. Jste vytvořili jednu z těchto výstrah některý z těchto způsobů:
* V hello portál Azure pro určité typy prostředků, v části monitorování -> Výstrahy -> Přidat výstrahy, kde "Výstrah na" je nastaven příliš "Události"
* Rutiny prostředí PowerShell přidat AzureRmLogAlertRule spuštěné hello
* Přímo pomocí [hello výstrahy REST API](http://docs.microsoft.com/rest/api/monitor/alertrules) s odata.type = "ManagementEventRuleCondition" a dataSource.odata.type = "RuleManagementEventDataSource"
 
Hello následující skript prostředí PowerShell vrátí seznam všech výstrah událostí správy, které máte v vaše předplatné, jakož i podmínky hello nastavit pro jednotlivé výstrahy.

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

Pokud máte žádné výstrahy týkající se správy událostí, bude výstup rutiny prostředí PowerShell hello výše řadu zprávy upozornění podobné následujícímu:

`WARNING: hello output of this cmdlet will be flattened, i.e. elimination of hello properties field, in a future release tooimprove hello user experience.`

Tyto zprávy upozornění můžete ignorovat. Pokud máte výstrahy na události správy, bude výstup hello tuto rutinu prostředí PowerShell vypadat takto:

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

Každá výstraha je oddělená na přerušovanou čáru a podrobnosti zahrnují ID prostředku hello hello výstrahy a hello konkrétní pravidlo monitorovány.

Tato funkce příliš přešla[Azure monitorování aktivity protokolu výstrah](monitoring-activity-log-alerts.md). Tyto nové výstrahy umožňují tooset podmínkou na aktivity protokolu události a přijímat oznámení, když o novou událost odpovídá hello podmínku. Také nabízí několik vylepšení z výstrahy na události správy:
* Můžete opakovaně použít skupině příjemců oznámení ("akce") napříč více výstrah pomocí [skupiny akcí](monitoring-action-groups.md), snižuje složitost hello změny příjemce výstrahu.
* Můžete obdržet oznámení přímo na váš telefon SMS pomocí akce skupiny.
* Můžete [vytvářet výstrahy, aktivity protokolu pomocí šablony Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md).
* Podmínky lze vytvořit s větší flexibilitu a složitost toomeet svých konkrétních potřeb.
* Oznámení se doručují rychleji.
 
## <a name="how-toomigrate"></a>Jak toomigrate
 
toocreate oznámení nové aktivity na protokolu, můžete buď:
* Postupujte podle [našem návodu na tom, jak hello toocreate výstraha v portálu Azure](monitoring-activity-log-alerts.md)
* Zjistěte, jak příliš[vytvořena výstraha pomocí šablony Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
 
Výstrahy na události správy, které jste předtím vytvořili nebudou výstrahy automaticky migrované tooActivity protokolu. Je nutné toouse hello předcházející výstrahy hello toolist skript prostředí PowerShell na správu události jste nakonfigurovali a ručně je znovu vytvořit jako aktivity protokolu výstrahy. To je třeba provést před říjen 1, po jejímž uplynutí výstrahy na události správy se už nebude zobrazovat ve vašem předplatném Azure. Jiné typy výstrah, Azure, včetně monitorování Azure metriky výstrah, Application Insights výstrahy a upozornění analýzy protokolů jsou tato změna nemá vliv. Pokud máte nějaké otázky, post v komentářích hello níže.


## <a name="next-steps"></a>Další kroky

* Další informace o [protokol aktivit](monitoring-overview-activity-logs.md)
* Konfigurace [aktivity protokolu výstrahy prostřednictvím portálu Azure](monitoring-activity-log-alerts.md)
* Konfigurace [aktivity protokolu výstrahy prostřednictvím Resource Manageru](monitoring-create-activity-log-alerts-with-resource-manager-template.md)
* Zkontrolujte hello [schématu výstrahy webhooku protokolu aktivit](monitoring-activity-log-alerts-webhook.md)
* Další informace o [oznámení o službách](monitoring-service-notifications.md)
* Další informace o [skupiny akcí](monitoring-action-groups.md)
