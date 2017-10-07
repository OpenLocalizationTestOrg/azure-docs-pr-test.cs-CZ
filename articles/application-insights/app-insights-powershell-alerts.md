---
title: "výstrahy tooset aaaUse prostředí Powershell ve službě Application Insights | Microsoft Docs"
description: "Automatické konfiguraci služby Application Insights tooget e-mailů o metriky změny."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 05d6a9e0-77a2-4a35-9052-a7768d23a196
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 10/31/2016
ms.author: bwren
ms.openlocfilehash: d68e5f9511bb4015f59175724bc1a4a04ecf43e1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooset-alerts-in-application-insights"></a>Pomocí prostředí PowerShell tooset výstrahy ve službě Application Insights
Můžete automatizovat konfiguraci hello [výstrahy](app-insights-alerts.md) v [Application Insights](app-insights-overview.md).

Kromě toho můžete [nastavit webhooky tooautomate upozornění tooan odpovědi](../monitoring-and-diagnostics/insights-webhooks-alerts.md).

> [!NOTE]
> Pokud chcete toocreate prostředky a výstrahy v hello stejný čas, zvažte [pomocí šablony Azure Resource Manager](app-insights-powershell.md).
>
>

## <a name="one-time-setup"></a>Jednorázové instalace
Pokud jste nepoužili prostředí PowerShell s předplatným Azure před:

Nainstalujte modul Azure Powershell hello na hello počítači, kde se má toorun hello skripty.

* Nainstalujte [instalačního programu webové platformy (verze 5 nebo novější)](http://www.microsoft.com/web/downloads/platform.aspx).
* Použít tooinstall Microsoft Azure Powershell

## <a name="connect-tooazure"></a>Připojit tooAzure
Otevřete prostředí Azure PowerShell a [připojení odběru tooyour](/powershell/azure/overview):

```PowerShell

    Add-AzureAccount
```


## <a name="get-alerts"></a>Získání výstrahy
    Get-AzureAlertRmRule -ResourceGroup "Fabrikam" [-Name "My rule"] [-DetailedOutput]

## <a name="add-alert"></a>Přidání oznámení
    Add-AlertRule  -Name "{ALERT NAME}" -Description "{TEXT}" `
     -ResourceGroup "{GROUP NAME}" `
     -ResourceId "/subscriptions/{SUBSCRIPTION ID}/resourcegroups/{GROUP NAME}/providers/microsoft.insights/components/{APP RESOURCE NAME}" `
     -MetricName "{METRIC NAME}" `
     -Operator GreaterThan  `
     -Threshold {NUMBER}   `
     -WindowSize {HH:MM:SS}  `
     [-SendEmailToServiceOwners] `
     [-CustomEmails "EMAIL1@X.COM","EMAIL2@Y.COM" ] `
     -Location "East US" // must be East US at present
     -RuleType Metric



## <a name="example-1"></a>Příklad 1
Poslat mi e-mail, pokud hello server odpovědi tooHTTP požadavků, která je průměrem více než 5 minut, je nižší než 1 sekunda. Moje prostředek Application Insights se nazývá IceCreamWebApp a je ve skupině prostředků společnosti Fabrikam. Jsem hello vlastník hello předplatného Azure.

Hello GUID je ID předplatného hello (ne hello klíč instrumentace aplikace hello).

    Add-AlertRule -Name "slow responses" `
     -Description "email me if hello server responds slowly" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "request.duration" `
     -Operator GreaterThan `
     -Threshold 1 `
     -WindowSize 00:05:00 `
     -SendEmailToServiceOwners `
     -Location "East US" -RuleType Metric

## <a name="example-2"></a>Příklad 2
Je nutné aplikaci, ve které používám [TrackMetric()](app-insights-api-custom-events-metrics.md#trackmetric) tooreport metriky s názvem "salesPerHour." Odešlete e-mail toomy kolegy Pokud "salesPerHour" klesne pod 100, průměr po dobu 24 hodin.

    Add-AlertRule -Name "poor sales" `
     -Description "slow sales alert" `
     -ResourceGroup "Fabrikam" `
     -ResourceId "/subscriptions/00000000-0000-0000-0000-000000000000/resourcegroups/Fabrikam/providers/microsoft.insights/components/IceCreamWebApp" `
     -MetricName "salesPerHour" `
     -Operator LessThan `
     -Threshold 100 `
     -WindowSize 24:00:00 `
     -CustomEmails "satish@fabrikam.com","lei@fabrikam.com" `
     -Location "East US" -RuleType Metric

Hello stejného pravidla lze použít pro metriku hello hlášené pomocí hello [měření parametr](app-insights-api-custom-events-metrics.md#properties) jiné sledování volání například TrackEvent nebo trackPageView.

## <a name="metric-names"></a>Metriky názvy
| Název metriky | Název obrazovky | Popis |
| --- | --- | --- |
| `basicExceptionBrowser.count` |Výjimky prohlížečů |Počet nezachycená výjimky vydané v prohlížeči hello. |
| `basicExceptionServer.count` |Výjimky serveru |Počet neošetřené výjimky vyvolané aplikace hello |
| `clientPerformance.clientProcess.value` |Doba zpracování klienta |Doba mezi přijetí hello posledního bajtu dokumentu, dokud je načtena hello DOM. Asynchronních připojení může být stále aktivní. |
| `clientPerformance.networkConnection.value` |Doba připojení sítě zatížení stránky |Čas hello prohlížeče trvá tooconnect toohello sítě. Může být 0, pokud do mezipaměti. |
| `clientPerformance.receiveRequest.value` |Přijetí doba odezvy |Doba mezi prohlížeče odeslání žádosti o toostarting tooreceive odpovědi. |
| `clientPerformance.sendRequest.value` |Odeslat doba požadavku |Doba, kterou požadavek toosend prohlížeče. |
| `clientPerformance.total.value` |Čas načítání stránky prohlížeče |Čas od požadavku uživatele, dokud se načtou DOM, předlohy se styly, skripty a obrázků. |
| `performanceCounter.available_bytes.value` |Dostupná paměť |Fyzické paměti k dispozici pro zpracování nebo pro použití systémem. |
| `performanceCounter.io_data_bytes_per_sec.value` |Rychlost zpracování vstupně-výstupní operace |Celkový počet bajtů na druhý pro čtení a napsané toofiles, sítě a zařízení. |
| `performanceCounter.number_of_exceps_thrown_per_sec.value` |míra výjimky |Výjimek vyvolaných za sekundu. |
| `performanceCounter.percentage_processor_time.value` |Proces procesoru |Hello procentuální hodnotu uplynulého času podprocesy procesu používané hello procesoru tooexecution pokyny pro proces aplikace hello. |
| `performanceCounter.percentage_processor_total.value` |Čas procesoru |Hello procento času, který hello procesor stráví v jiných než nečinných vláken. |
| `performanceCounter.process_private_bytes.value` |Proces nesdílených bajtů |Paměť výhradně přiřazená toohello sledovat procesy aplikace. |
| `performanceCounter.request_execution_time.value` |Čas provádění požadavku ASP.NET |Čas provedení posledního požadavku hello. |
| `performanceCounter.requests_in_application_queue.value` |ASP.NET požadavků ve frontě provádění |Délka fronty požadavků aplikace hello. |
| `performanceCounter.requests_per_sec.value` |Rychlost požadavků ASP.NET |Rychlost, jakou všechny požadavky aplikace toohello za sekundu z technologie ASP.NET. |
| `remoteDependencyFailed.durationMetric.count` |Selhání závislostí |Počet selhání volání zdrojů tooexternal hello serverových aplikací. |
| `request.duration` |Doba odezvy serveru |Doba mezi přijetí požadavku HTTP a dokončením odeslání odpovědi hello. |
| `request.rate` |Rychlost požadavků |Rychlost, jakou všechny požadavky aplikace toohello za sekundu. |
| `requestFailed.count` |Neúspěšné požadavky |Počet HTTP požadavků, které kód odpovědi > = 400 |
| `view.count` |Zobrazení stránky |Počet žádostí uživatele klienta pro webovou stránku. Syntetické provoz odfiltrována. |
| {váš vlastní název metriky} |{Název metriky} |Metriky hodnota hlášené [TrackMetric](app-insights-api-custom-events-metrics.md#trackmetric) nebo v hello [parametr měření volání sledování](app-insights-api-custom-events-metrics.md#properties). |

metriky Hello jsou odesílány moduly různých telemetrie:

| Metriky skupiny | Kolekce modulu |
| --- | --- |
| basicExceptionBrowser,<br/>clientPerformance,<br/>zobrazit |[JavaScript prohlížeče](app-insights-javascript.md) |
| performanceCounter |[Výkon](app-insights-configuration-with-applicationinsights-config.md) |
| remoteDependencyFailed |[Závislost](app-insights-configuration-with-applicationinsights-config.md) |
| žádost,<br/>requestFailed |[Požadavek na serveru](app-insights-configuration-with-applicationinsights-config.md) |

## <a name="webhooks"></a>Webhooky
Můžete [automatizovat upozornění tooan odpovědi](../monitoring-and-diagnostics/insights-webhooks-alerts.md). Azure bude volat webovou adresu podle vaší volby, když je vydána výstraha.

## <a name="see-also"></a>Viz také
* [Skript tooconfigure Application Insights](app-insights-powershell-script-create-resource.md)
* [Vytvoření služby Application Insights a web test prostředky ze šablon](app-insights-powershell.md)
* [Automatizovat spojovacích Statistika tooApplication diagnostické nástroje sady Microsoft Azure](app-insights-powershell-azure-diagnostics.md)
* [Automatizovat upozornění tooan odpovědi](../monitoring-and-diagnostics/insights-webhooks-alerts.md)
