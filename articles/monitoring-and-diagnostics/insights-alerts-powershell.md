---
title: "aaaCreate výstrahy pro služby Azure – prostředí PowerShell | Microsoft Docs"
description: "Při splnění zadané podmínky hello aktivaci e-mailů, oznámení, adresy URL weby volání (webhooky) nebo automatizace."
author: rboucher
manager: carmonm
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d26ab15b-7b7e-42a9-81c8-3ce9ead5d252
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2016
ms.author: robb
ms.openlocfilehash: 80d3a3f194fc6a5a09a81d04206ea7a1640bddb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="create-metric-alerts-in-azure-monitor-for-azure-services---powershell"></a>Vytvoření metriky výstrah v monitorování Azure pro služby Azure – prostředí PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](insights-alerts-portal.md)
> * [PowerShell](insights-alerts-powershell.md)
> * [Rozhraní příkazového řádku](insights-alerts-command-line-interface.md)
>
>

## <a name="overview"></a>Přehled
Tento článek ukazuje, jak tooset si Azure metrika výstrah pomocí prostředí PowerShell.  

Můžete zobrazit upozornění na základě monitorování metriky pro nebo událostí na služeb Azure.

* **Metriky hodnoty** – hello aktivační události výstrahy, když hodnota hello zadaný metriky překračuje prahovou hodnotu přiřadíte v obou směrech. To znamená, aktivuje obě při první je splněna podmínka hello a pak později, pokud podmínky již probíhá není splněna.    
* **Události protokolu aktivit** -výstrahu můžete aktivovat pro *každých* události nebo pouze tehdy, když dojde k určité události. Další informace o protokolu upozornění toolearn [, klikněte sem](monitoring-activity-log-alerts.md)

Můžete nakonfigurovat následující hello metriky toodo výstrahy, když ji spustí:

* odesílat oznámení e-mailu, toohello Správce služeb a spolusprávci
* odesílání e-mailů tooadditional e-mailů, které zadáte.
* Volat webhook, jehož
* Spusťte provádění runbook služby Azure (pouze z hello portál Azure)

Můžete nakonfigurovat a získat informace o použití pravidla výstrah

* [Azure Portal](insights-alerts-portal.md)
* [PowerShell](insights-alerts-powershell.md)
* [rozhraní příkazového řádku (CLI)](insights-alerts-command-line-interface.md)
* [Rozhraní API REST Azure monitorování](https://msdn.microsoft.com/library/azure/dn931945.aspx)

Další informace najdete vždy zadáte ```Get-Help``` a pak hello příkaz prostředí PowerShell, který chcete zobrazit nápovědu.

## <a name="create-alert-rules-in-powershell"></a>Vytvořit pravidla výstrah v prostředí PowerShell
1. Přihlaste se tooAzure.   

    ```PowerShell
    Login-AzureRmAccount

    ```
2. Získání seznamu předplatných hello máte k dispozici. Ověřte, že pracujete s hello správné předplatné. Pokud ne, nastavte ji toohello doprava o jednu pomocí výstup hello `Get-AzureRmSubscription`.

    ```PowerShell
    Get-AzureRmSubscription
    Get-AzureRmContext
    Set-AzureRmContext -SubscriptionId <subscriptionid>
    ```
3. toolist existující pravidla na skupinu prostředků, hello použijte následující příkaz:

   ```PowerShell
   Get-AzureRmAlertRule -ResourceGroup <myresourcegroup> -DetailedOutput
   ```
4. toocreate pravidlo, budete potřebovat toohave několik důležitých informací nejdřív.

  * Hello **ID prostředku** pro prostředek hello chcete tooset výstrahu pro
  * Hello **definice metrik** k dispozici pro tento prostředek

     Jedním ze způsobů tooget hello ID prostředku je toouse hello portálu Azure. Za předpokladu, že již není vytvořen hello prostředků, vyberte ho v portálu hello. V dalším okně hello vyberte *vlastnosti* pod hello *nastavení* části. **ID prostředku** je pole v dalším okně hello. Další možností je toouse hello [Průzkumníka prostředků Azure](https://resources.azure.com/).

     Je například ID prostředku pro webovou aplikaci

     ```
     /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename
     ```

     Můžete použít `Get-AzureRmMetricDefinition` tooview hello seznam všechny definice metrik pro konkrétní zdroje.

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id>
     ```

     Hello následující příklad vygeneruje tabulku s hello metrika název a hello jednotky pro tuto metriku.

     ```PowerShell
     Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit

     ```
     Úplný seznam dostupných možností pro Get-AzureRmMetricDefinition je k dispozici spuštěním Get-MetricDefinitions.
5. Následující příklad sady si výstrahy na prostředku webu Hello. Hello výstrahy aktivuje vždy, když obdrží veškerou komunikaci konzistentně 5 minut a opakujte při přijetí žádný provoz 5 minut.

    ```PowerShell
    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Description "alert on any website activity"

    ```
6. toocreate webhooku nebo odesílat e-maily, když se aktivuje výstraha, nejprve vytvořit hello e-mailu nebo webhooky. Hned vytvořit pravidlo hello později s hello - akce značky a jak je znázorněno v následující ukázka hello. Nelze přidružit webhooku nebo e-mailů s již vytvořili pravidla pomocí prostředí PowerShell.

    ```PowerShell
    $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
    $actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://www.contoso.com?token=mytoken

    Add-AzureRmMetricAlertRule -Name myMetricRuleWithWebhookAndEmail -Location "East US" -ResourceGroup myresourcegroup -TargetResourceId /subscriptions/dededede-7aa0-407d-a6fb-eb20c8bd1192/resourceGroups/myresourcegroupname/providers/Microsoft.Web/sites/mywebsitename -MetricName "BytesReceived" -Operator GreaterThan -Threshold 2 -WindowSize 00:05:00 -TimeAggregationOperator Total -Actions $actionEmail, $actionWebhook -Description "alert on any website activity"
    ```

7. tooverify, že vaše upozornění byla vytvořena správně prohlížením hello jednotlivých pravidel.

    ```PowerShell
    Get-AzureRmAlertRule -Name myMetricRuleWithWebhookAndEmail -ResourceGroup myresourcegroup -DetailedOutput

    Get-AzureRmAlertRule -Name myLogAlertRule -ResourceGroup myresourcegroup -DetailedOutput
    ```
8. Oznámení odstraňte. Tyto příkazy odstranit pravidla hello vytvořili dříve v tomto článku.

    ```PowerShell
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myrule
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myMetricRuleWithWebhookAndEmail
    Remove-AzureRmAlertRule -ResourceGroup myresourcegroup -Name myLogAlertRule
    ```

## <a name="next-steps"></a>Další kroky
* [Získat přehled o Azure monitorování](monitoring-overview.md) včetně hello typy informací, můžete sledovat a shromažďovat.
* Další informace o [konfigurace webhooky ve výstrahách](insights-webhooks-alerts.md).
* Další informace o [konfigurace výstrah pro aktivitu protokolu události](monitoring-activity-log-alerts.md).
* Další informace o [sad Azure Automation Runbook](../automation/automation-starting-a-runbook.md).
* Získat [přehled shromažďování diagnostických protokolů](monitoring-overview-of-diagnostic-logs.md) toocollect podrobné vysoká frekvence metriky pro vaši službu.
* Získat [přehled metriky kolekce](insights-how-to-customize-monitoring.md) toomake, že je služba dostupná a dobře reagovaly.
