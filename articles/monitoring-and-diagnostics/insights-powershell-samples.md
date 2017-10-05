---
title: "Ukázek Azure PowerShell monitorování rychlý start. | Dokumentace Microsoftu"
description: "Pomocí prostředí PowerShell pro přístup k Azure monitorování funkce, jako je automatické škálování, výstrahy, webhooky a hledání protokoly aktivity."
author: kamathashwin
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: c0761814-7148-4ab5-8c27-a2c9fa4cfef5
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: ashwink
ms.openlocfilehash: 48f064884c2a6d0a55cc58a44169ed03c62de46d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/18/2017
---
# <a name="azure-monitor-powershell-quick-start-samples"></a><span data-ttu-id="09e1a-104">Ukázek Azure PowerShell monitorování rychlý start</span><span class="sxs-lookup"><span data-stu-id="09e1a-104">Azure Monitor PowerShell quick start samples</span></span>
<span data-ttu-id="09e1a-105">Tento článek ukazuje ukázkové příkazy prostředí PowerShell, abyste měli přístup k funkcím Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="09e1a-105">This article shows you sample PowerShell commands to help you access Azure Monitor features.</span></span> <span data-ttu-id="09e1a-106">Azure monitorování umožňuje škálování cloudové služby, virtuální počítače a webových aplikací a odesílat oznámení o výstrahách nebo volání webové adresy URL založené na hodnotách nakonfigurované telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="09e1a-106">Azure Monitor allows you to AutoScale Cloud Services, Virtual Machines, and Web Apps and to send alert notifications or call web URLs based on values of configured telemetry data.</span></span>

> [!NOTE]
> <span data-ttu-id="09e1a-107">Azure monitorování je nový název pro co byla volána "Statistika Azure" až 25 září 2016.</span><span class="sxs-lookup"><span data-stu-id="09e1a-107">Azure Monitor is the new name for what was called "Azure Insights" until Sept 25th, 2016.</span></span> <span data-ttu-id="09e1a-108">Však obory názvů a proto následující příkazy stále obsahovat "insights".</span><span class="sxs-lookup"><span data-stu-id="09e1a-108">However, the namespaces and thus the following commands still contain the "insights".</span></span>
> 
> 

## <a name="set-up-powershell"></a><span data-ttu-id="09e1a-109">Nastavení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="09e1a-109">Set up PowerShell</span></span>
<span data-ttu-id="09e1a-110">Pokud jste to ještě neudělali, nastavte prostředí PowerShell pro spouštění v počítači.</span><span class="sxs-lookup"><span data-stu-id="09e1a-110">If you haven't already, set up PowerShell to run on your computer.</span></span> <span data-ttu-id="09e1a-111">Další informace najdete v tématu [postup instalace a konfigurace prostředí PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="09e1a-111">For more information, see [How to Install and Configure PowerShell](/powershell/azure/overview).</span></span>

## <a name="examples-in-this-article"></a><span data-ttu-id="09e1a-112">Příklady v tomto článku</span><span class="sxs-lookup"><span data-stu-id="09e1a-112">Examples in this article</span></span>
<span data-ttu-id="09e1a-113">V příkladech v článku znázorňují, jak můžete použít rutiny Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="09e1a-113">The examples in the article illustrate how you can use Azure Monitor cmdlets.</span></span> <span data-ttu-id="09e1a-114">Můžete také zkontrolovat celý seznam rutin Azure Powershellu monitorování v [rutiny Azure monitorování (přehled)](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).</span><span class="sxs-lookup"><span data-stu-id="09e1a-114">You can also review the entire list of Azure Monitor PowerShell cmdlets at [Azure Monitor (Insights) Cmdlets](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).</span></span>

## <a name="sign-in-and-use-subscriptions"></a><span data-ttu-id="09e1a-115">Přihlásit a používat odběrů</span><span class="sxs-lookup"><span data-stu-id="09e1a-115">Sign in and use subscriptions</span></span>
<span data-ttu-id="09e1a-116">Nejdřív přihlaste k vašemu předplatnému Azure.</span><span class="sxs-lookup"><span data-stu-id="09e1a-116">First, log in to your Azure subscription.</span></span>

```PowerShell
Login-AzureRmAccount
```

<span data-ttu-id="09e1a-117">To vyžaduje, abyste se přihlásili.</span><span class="sxs-lookup"><span data-stu-id="09e1a-117">This requires you to sign in.</span></span> <span data-ttu-id="09e1a-118">Až to uděláte, váš účet, zobrazí se TenantID a výchozí ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="09e1a-118">Once you do, your Account, TenantID and default Subscription ID are displayed.</span></span> <span data-ttu-id="09e1a-119">Všechny rutiny Azure fungovat v rámci vašeho předplatného výchozí.</span><span class="sxs-lookup"><span data-stu-id="09e1a-119">All the Azure cmdlets work in the context of your default subscription.</span></span> <span data-ttu-id="09e1a-120">Chcete-li zobrazit seznam odběrů máte přístup, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="09e1a-120">To view the list of subscriptions you have access to, use the following command.</span></span>

```PowerShell
Get-AzureRmSubscription
```

<span data-ttu-id="09e1a-121">Chcete-li změnit váš pracovní kontext do jiného předplatného, použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="09e1a-121">To change your working context to a different subscription, use the following command.</span></span>

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-activity-log-for-a-subscription"></a><span data-ttu-id="09e1a-122">Načtení protokol aktivit pro předplatné</span><span class="sxs-lookup"><span data-stu-id="09e1a-122">Retrieve Activity log for a subscription</span></span>
<span data-ttu-id="09e1a-123">Použití `Get-AzureRmLog` rutiny.</span><span class="sxs-lookup"><span data-stu-id="09e1a-123">Use the `Get-AzureRmLog` cmdlet.</span></span>  <span data-ttu-id="09e1a-124">Následuje několik běžných příkladů.</span><span class="sxs-lookup"><span data-stu-id="09e1a-124">The following are some common examples.</span></span>

<span data-ttu-id="09e1a-125">Získání položky protokolu z tohoto času a data k dispozici:</span><span class="sxs-lookup"><span data-stu-id="09e1a-125">Get log entries from this time/date to present:</span></span>

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

<span data-ttu-id="09e1a-126">Získáte položky protokolu mezi rozsahem času a data:</span><span class="sxs-lookup"><span data-stu-id="09e1a-126">Get log entries between a time/date range:</span></span>

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

<span data-ttu-id="09e1a-127">Získání položky protokolu z určité skupiny zdrojů:</span><span class="sxs-lookup"><span data-stu-id="09e1a-127">Get log entries from a specific resource group:</span></span>

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

<span data-ttu-id="09e1a-128">Získáte položky protokolu od zprostředkovatele konkrétní prostředků mezi rozsahem času a data:</span><span class="sxs-lookup"><span data-stu-id="09e1a-128">Get log entries from a specific resource provider between a time/date range:</span></span>

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

<span data-ttu-id="09e1a-129">Získání všech položek protokolů s konkrétní volající:</span><span class="sxs-lookup"><span data-stu-id="09e1a-129">Get all log entries with a specific caller:</span></span>

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

<span data-ttu-id="09e1a-130">Tento příkaz načte posledních 1000 události z protokolu aktivit:</span><span class="sxs-lookup"><span data-stu-id="09e1a-130">The following command retrieves the last 1000 events from the activity log:</span></span>

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

<span data-ttu-id="09e1a-131">`Get-AzureRmLog`podporuje mnoho dalších parametrů.</span><span class="sxs-lookup"><span data-stu-id="09e1a-131">`Get-AzureRmLog` supports many other parameters.</span></span> <span data-ttu-id="09e1a-132">Najdete v článku `Get-AzureRmLog` pro další informace.</span><span class="sxs-lookup"><span data-stu-id="09e1a-132">See the `Get-AzureRmLog` reference for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="09e1a-133">`Get-AzureRmLog`poskytuje pouze 15 dní historie.</span><span class="sxs-lookup"><span data-stu-id="09e1a-133">`Get-AzureRmLog` only provides 15 days of history.</span></span> <span data-ttu-id="09e1a-134">Pomocí **- MaxEvents** parametr umožňuje dotazování poslední události N, nad rámec 15 dnů.</span><span class="sxs-lookup"><span data-stu-id="09e1a-134">Using the **-MaxEvents** parameter allows you to query the last N events, beyond 15 days.</span></span> <span data-ttu-id="09e1a-135">Pro přístup k události starší než 15 dní pomocí rozhraní REST API nebo SDK (C# ukázka pomocí sady SDK).</span><span class="sxs-lookup"><span data-stu-id="09e1a-135">To access events older than 15 days, use the REST API or SDK (C# sample using the SDK).</span></span> <span data-ttu-id="09e1a-136">Pokud neuvedete **StartTime**, pak výchozí hodnota je **EndTime** minus jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="09e1a-136">If you do not include **StartTime**, then the default value is **EndTime** minus one hour.</span></span> <span data-ttu-id="09e1a-137">Pokud neuvedete **EndTime**, pak výchozí hodnota je aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="09e1a-137">If you do not include **EndTime**, then the default value is current time.</span></span> <span data-ttu-id="09e1a-138">Všechny časy jsou ve formátu UTC.</span><span class="sxs-lookup"><span data-stu-id="09e1a-138">All times are in UTC.</span></span>
> 
> 

## <a name="retrieve-alerts-history"></a><span data-ttu-id="09e1a-139">Načtení historie výstrah</span><span class="sxs-lookup"><span data-stu-id="09e1a-139">Retrieve alerts history</span></span>
<span data-ttu-id="09e1a-140">Chcete-li zobrazit všechny výstrahy událostí, se můžete dotazovat protokolů Azure Resource Manager pomocí následující příklady.</span><span class="sxs-lookup"><span data-stu-id="09e1a-140">To view all alert events, you can query the Azure Resource Manager logs using the following examples.</span></span>

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

<span data-ttu-id="09e1a-141">Chcete-li zobrazit historii pro konkrétní pravidlo výstrahy, můžete použít `Get-AzureRmAlertHistory` rutiny předávání v ID prostředku pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="09e1a-141">To view the history for a specific alert rule, you can use the `Get-AzureRmAlertHistory` cmdlet, passing in the resource ID of the alert rule.</span></span>

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

<span data-ttu-id="09e1a-142">`Get-AzureRmAlertHistory` Rutina podporuje různé parametry.</span><span class="sxs-lookup"><span data-stu-id="09e1a-142">The `Get-AzureRmAlertHistory` cmdlet supports various parameters.</span></span> <span data-ttu-id="09e1a-143">Další informace najdete v tématu [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).</span><span class="sxs-lookup"><span data-stu-id="09e1a-143">More information, see [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).</span></span>

## <a name="retrieve-information-on-alert-rules"></a><span data-ttu-id="09e1a-144">Načíst informace o pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="09e1a-144">Retrieve information on alert rules</span></span>
<span data-ttu-id="09e1a-145">Všechny z následujících příkazů fungují na skupinu prostředků s názvem "montest".</span><span class="sxs-lookup"><span data-stu-id="09e1a-145">All of the following commands act on a Resource Group named "montest".</span></span>

<span data-ttu-id="09e1a-146">Zobrazte všechny vlastnosti pravidlo výstrahy:</span><span class="sxs-lookup"><span data-stu-id="09e1a-146">View all the properties of the alert rule:</span></span>

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

<span data-ttu-id="09e1a-147">Načtěte všechny výstrahy pro skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="09e1a-147">Retrieve all alerts on a resource group:</span></span>

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

<span data-ttu-id="09e1a-148">Načtěte všechny výstrahy pravidla nastavená pro cílový prostředek.</span><span class="sxs-lookup"><span data-stu-id="09e1a-148">Retrieve all alert rules set for a target resource.</span></span> <span data-ttu-id="09e1a-149">Všechna pravidla výstrah je třeba nastavit na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="09e1a-149">For example, all alert rules set on a VM.</span></span>

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

<span data-ttu-id="09e1a-150">`Get-AzureRmAlertRule`podporuje další parametry.</span><span class="sxs-lookup"><span data-stu-id="09e1a-150">`Get-AzureRmAlertRule` supports other parameters.</span></span> <span data-ttu-id="09e1a-151">V tématu [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="09e1a-151">See [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) for more information.</span></span>

## <a name="create-metric-alerts"></a><span data-ttu-id="09e1a-152">Vytvoření metriky výstrahy</span><span class="sxs-lookup"><span data-stu-id="09e1a-152">Create metric alerts</span></span>
<span data-ttu-id="09e1a-153">Můžete použít `Add-AlertRule` rutiny vytvářet, aktualizovat nebo zakázat pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="09e1a-153">You can use the `Add-AlertRule` cmdlet to create, update or disable an alert rule.</span></span>

<span data-ttu-id="09e1a-154">Můžete vytvořit e-mailu a webhooku vlastností pomocí `New-AzureRmAlertRuleEmail` a `New-AzureRmAlertRuleWebhook`, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="09e1a-154">You can create email and webhook properties using  `New-AzureRmAlertRuleEmail` and `New-AzureRmAlertRuleWebhook`, respectively.</span></span> <span data-ttu-id="09e1a-155">V rutině pravidlo výstrahy, přiřadit jako akce **akce** vlastnost pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="09e1a-155">In the Alert rule cmdlet, assign these as actions to the **Actions** property of the Alert Rule.</span></span>

<span data-ttu-id="09e1a-156">Následující tabulka popisuje parametry a hodnoty použité k vytvoření výstrahy pomocí metriky.</span><span class="sxs-lookup"><span data-stu-id="09e1a-156">The following table describes the parameters and values used to create an alert using a metric.</span></span>

| <span data-ttu-id="09e1a-157">Parametr</span><span class="sxs-lookup"><span data-stu-id="09e1a-157">parameter</span></span> | <span data-ttu-id="09e1a-158">hodnota</span><span class="sxs-lookup"><span data-stu-id="09e1a-158">value</span></span> |
| --- | --- |
| <span data-ttu-id="09e1a-159">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="09e1a-159">Name</span></span> |<span data-ttu-id="09e1a-160">simpletestdiskwrite</span><span class="sxs-lookup"><span data-stu-id="09e1a-160">simpletestdiskwrite</span></span> |
| <span data-ttu-id="09e1a-161">Umístění tohoto pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="09e1a-161">Location of this alert rule</span></span> |<span data-ttu-id="09e1a-162">Východ USA</span><span class="sxs-lookup"><span data-stu-id="09e1a-162">East US</span></span> |
| <span data-ttu-id="09e1a-163">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="09e1a-163">ResourceGroup</span></span> |<span data-ttu-id="09e1a-164">montest</span><span class="sxs-lookup"><span data-stu-id="09e1a-164">montest</span></span> |
| <span data-ttu-id="09e1a-165">TargetResourceId</span><span class="sxs-lookup"><span data-stu-id="09e1a-165">TargetResourceId</span></span> |<span data-ttu-id="09e1a-166">/subscriptions/S1/resourceGroups/montest/providers/Microsoft.COMPUTE/virtualMachines/testconfig</span><span class="sxs-lookup"><span data-stu-id="09e1a-166">/subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig</span></span> |
| <span data-ttu-id="09e1a-167">MetricName výstrahy, která je vytvořena</span><span class="sxs-lookup"><span data-stu-id="09e1a-167">MetricName of the alert that is created</span></span> |<span data-ttu-id="09e1a-168">\PhysicalDisk (_celkem) \Disk zápisů za sekundu. Najdete v článku `Get-MetricDefinitions` rutiny o tom, jak získat přesné metriky názvy</span><span class="sxs-lookup"><span data-stu-id="09e1a-168">\PhysicalDisk(_Total)\Disk Writes/sec. See the `Get-MetricDefinitions` cmdlet about how to retrieve the exact metric names</span></span> |
| <span data-ttu-id="09e1a-169">Operátor</span><span class="sxs-lookup"><span data-stu-id="09e1a-169">operator</span></span> |<span data-ttu-id="09e1a-170">GreaterThan</span><span class="sxs-lookup"><span data-stu-id="09e1a-170">GreaterThan</span></span> |
| <span data-ttu-id="09e1a-171">Prahová hodnota (počet za sekundu za pro tato metrika)</span><span class="sxs-lookup"><span data-stu-id="09e1a-171">Threshold value (count/sec in for this metric)</span></span> |<span data-ttu-id="09e1a-172">1</span><span class="sxs-lookup"><span data-stu-id="09e1a-172">1</span></span> |
| <span data-ttu-id="09e1a-173">Velikost_okna (ve formátu hh: mm:)</span><span class="sxs-lookup"><span data-stu-id="09e1a-173">WindowSize (hh:mm:ss format)</span></span> |<span data-ttu-id="09e1a-174">00:05:00</span><span class="sxs-lookup"><span data-stu-id="09e1a-174">00:05:00</span></span> |
| <span data-ttu-id="09e1a-175">agregátoru (statistiky metriky, které používá průměrný počet, v takovém případě)</span><span class="sxs-lookup"><span data-stu-id="09e1a-175">aggregator (statistic of the metric, which uses Average count, in this case)</span></span> |<span data-ttu-id="09e1a-176">Průměr</span><span class="sxs-lookup"><span data-stu-id="09e1a-176">Average</span></span> |
| <span data-ttu-id="09e1a-177">vlastní e-mailů (řetězec pole)</span><span class="sxs-lookup"><span data-stu-id="09e1a-177">custom emails (string array)</span></span> |<span data-ttu-id="09e1a-178">'foo@example.com','bar@example.com'</span><span class="sxs-lookup"><span data-stu-id="09e1a-178">'foo@example.com','bar@example.com'</span></span> |
| <span data-ttu-id="09e1a-179">poslat vlastníci, přispěvatelé a čtenáři e-mailu</span><span class="sxs-lookup"><span data-stu-id="09e1a-179">send email to owners, contributors and readers</span></span> |<span data-ttu-id="09e1a-180">-SendToServiceOwners</span><span class="sxs-lookup"><span data-stu-id="09e1a-180">-SendToServiceOwners</span></span> |

<span data-ttu-id="09e1a-181">Vytvoří akci, e-mailu</span><span class="sxs-lookup"><span data-stu-id="09e1a-181">Create an Email action</span></span>

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

<span data-ttu-id="09e1a-182">Vytvoření Webhooku akce</span><span class="sxs-lookup"><span data-stu-id="09e1a-182">Create a Webhook action</span></span>

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

<span data-ttu-id="09e1a-183">Vytvořit pravidlo výstrahy na metrika % využití procesoru na virtuálním počítači classic</span><span class="sxs-lookup"><span data-stu-id="09e1a-183">Create the alert rule on the CPU% metric on a classic VM</span></span>

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

<span data-ttu-id="09e1a-184">Načtení pravidlo výstrahy</span><span class="sxs-lookup"><span data-stu-id="09e1a-184">Retrieve the alert rule</span></span>

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

<span data-ttu-id="09e1a-185">Výstrahy rutinu přidat také aktualizuje pravidlo, pokud se pravidlo výstrahy již existuje pro dané vlastnosti.</span><span class="sxs-lookup"><span data-stu-id="09e1a-185">The Add alert cmdlet also updates the rule if an alert rule already exists for the given properties.</span></span> <span data-ttu-id="09e1a-186">Chcete-li zakázat pravidlo výstrahy, zahrňte parametr **- DisableRule**.</span><span class="sxs-lookup"><span data-stu-id="09e1a-186">To disable an alert rule, include the parameter **-DisableRule**.</span></span>

## <a name="get-a-list-of-available-metrics-for-alerts"></a><span data-ttu-id="09e1a-187">Získat seznam dostupné metriky pro výstrahy</span><span class="sxs-lookup"><span data-stu-id="09e1a-187">Get a list of available metrics for alerts</span></span>
<span data-ttu-id="09e1a-188">Můžete použít `Get-AzureRmMetricDefinition` rutiny zobrazíte seznam všechny metriky pro konkrétní zdroje.</span><span class="sxs-lookup"><span data-stu-id="09e1a-188">You can use the `Get-AzureRmMetricDefinition` cmdlet to view the list of all metrics for a specific resource.</span></span>

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

<span data-ttu-id="09e1a-189">Následující příklad vytvoří tabulku s metrika název a jednotky pro ni.</span><span class="sxs-lookup"><span data-stu-id="09e1a-189">The following example generates a table with the metric Name and the Unit for it.</span></span>

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

<span data-ttu-id="09e1a-190">Úplný seznam dostupných možností pro `Get-AzureRmMetricDefinition` je k dispozici na [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).</span><span class="sxs-lookup"><span data-stu-id="09e1a-190">A full list of available options for `Get-AzureRmMetricDefinition` is available at [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).</span></span>

## <a name="create-and-manage-autoscale-settings"></a><span data-ttu-id="09e1a-191">Vytvořit a spravovat nastavení automatického škálování</span><span class="sxs-lookup"><span data-stu-id="09e1a-191">Create and manage AutoScale settings</span></span>
<span data-ttu-id="09e1a-192">Prostředek, například webovou aplikaci, virtuální počítač, Cloudová služba nebo sadu škálování virtuálního počítače může mít pouze jeden nastavení automatického škálování pro něj nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="09e1a-192">A resource, such as a Web app, VM, Cloud Service or Virtual Machine Scale Set can have only one autoscale setting configured for it.</span></span>
<span data-ttu-id="09e1a-193">Každé nastavení automatického škálování však může mít několik profilů.</span><span class="sxs-lookup"><span data-stu-id="09e1a-193">However, each autoscale setting can have multiple profiles.</span></span> <span data-ttu-id="09e1a-194">Například jeden pro profil na základě výkonu škálování a druhý pro profil na základě plánu.</span><span class="sxs-lookup"><span data-stu-id="09e1a-194">For example, one for a performance-based scale profile and a second one for a schedule-based profile.</span></span> <span data-ttu-id="09e1a-195">Každý profil může mít víc pravidel, které jsou nakonfigurované na něm.</span><span class="sxs-lookup"><span data-stu-id="09e1a-195">Each profile can have multiple rules configured on it.</span></span> <span data-ttu-id="09e1a-196">Další informace o škálování najdete v tématu [postup škálování aplikace](../cloud-services/cloud-services-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="09e1a-196">For more information about Autoscale, see [How to Autoscale an Application](../cloud-services/cloud-services-how-to-scale.md).</span></span>

<span data-ttu-id="09e1a-197">Zde jsou kroky, které budeme používat:</span><span class="sxs-lookup"><span data-stu-id="09e1a-197">Here are the steps we will use:</span></span>

1. <span data-ttu-id="09e1a-198">Vytvoření pravidel.</span><span class="sxs-lookup"><span data-stu-id="09e1a-198">Create rule(s).</span></span>
2. <span data-ttu-id="09e1a-199">Vytvořte profily mapování pravidla, které jste vytvořili dříve na profily.</span><span class="sxs-lookup"><span data-stu-id="09e1a-199">Create profile(s) mapping the rules that you created previously to the profiles.</span></span>
3. <span data-ttu-id="09e1a-200">Volitelné: Konfigurace vlastností webhooku a e-mailu vytvořte oznámení o škálování.</span><span class="sxs-lookup"><span data-stu-id="09e1a-200">Optional: Create notifications for autoscale by configuring webhook and email properties.</span></span>
4. <span data-ttu-id="09e1a-201">Vytvoření nastavení automatického škálování s názvem na cílového prostředku tak, že mapuje profily a oznámení, které jste vytvořili v předchozích krocích.</span><span class="sxs-lookup"><span data-stu-id="09e1a-201">Create an autoscale setting with a name on the target resource by mapping the profiles and notifications that you created in the previous steps.</span></span>

<span data-ttu-id="09e1a-202">Následující příklady ukazují, jak můžete vytvořit na nastavení automatického škálování pro sadu škálování virtuálního počítače pro operační systém Windows na základě pomocí metriky využití procesoru.</span><span class="sxs-lookup"><span data-stu-id="09e1a-202">The following examples show you how you can create an Autoscale setting for a Virtual Machine Scale Set for a Windows operating system based by using the CPU utilization metric.</span></span>

<span data-ttu-id="09e1a-203">Nejprve vytvořte pravidlo pro škálovatelnou větší počet instancí.</span><span class="sxs-lookup"><span data-stu-id="09e1a-203">First, create a rule to scale-out, with an instance count increase.</span></span>

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionValue 1
```        

<span data-ttu-id="09e1a-204">Dále vytvořte pravidlo pro škálování v, s snížení počtu instancí.</span><span class="sxs-lookup"><span data-stu-id="09e1a-204">Next, create a rule to scale-in, with an instance count decrease.</span></span>

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionValue 1
```

<span data-ttu-id="09e1a-205">Pak vytvořte profil pro pravidla.</span><span class="sxs-lookup"><span data-stu-id="09e1a-205">Then, create a profile for the rules.</span></span>

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

<span data-ttu-id="09e1a-206">Vytvoření vlastnosti webhooku.</span><span class="sxs-lookup"><span data-stu-id="09e1a-206">Create a webhook property.</span></span>

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

<span data-ttu-id="09e1a-207">Vytvořte oznámení vlastnost pro nastavení automatického škálování, včetně e-mailu a webhooku, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="09e1a-207">Create the notification property for the autoscale setting, including email and the webhook that you created previously.</span></span>

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

<span data-ttu-id="09e1a-208">Nakonec vytvořte nastavení automatického škálování přidat profil, který jste vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="09e1a-208">Finally, create the autoscale setting to add the profile that you created above.</span></span>

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

<span data-ttu-id="09e1a-209">Další informace o správě nastavení automatického škálování najdete v tématu [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).</span><span class="sxs-lookup"><span data-stu-id="09e1a-209">For more information about managing Autoscale settings, see [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).</span></span>

## <a name="autoscale-history"></a><span data-ttu-id="09e1a-210">Historie škálování</span><span class="sxs-lookup"><span data-stu-id="09e1a-210">Autoscale history</span></span>
<span data-ttu-id="09e1a-211">Následující příklad ukazuje, jak zobrazit nedávné škálování a události výstrah.</span><span class="sxs-lookup"><span data-stu-id="09e1a-211">The following example shows you how you can view recent autoscale and alert events.</span></span> <span data-ttu-id="09e1a-212">Pomocí vyhledávání protokolu aktivity a zobrazte historii škálování.</span><span class="sxs-lookup"><span data-stu-id="09e1a-212">Use the activity log search to view the autoscale history.</span></span>

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

<span data-ttu-id="09e1a-213">Můžete použít `Get-AzureRmAutoScaleHistory` rutiny načíst historii škálování.</span><span class="sxs-lookup"><span data-stu-id="09e1a-213">You can use the `Get-AzureRmAutoScaleHistory` cmdlet to retrieve AutoScale history.</span></span>

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

<span data-ttu-id="09e1a-214">Další informace najdete v tématu [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).</span><span class="sxs-lookup"><span data-stu-id="09e1a-214">For more information, see [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).</span></span>

### <a name="view-details-for-an-autoscale-setting"></a><span data-ttu-id="09e1a-215">Zobrazit podrobnosti o nastavení automatického škálování</span><span class="sxs-lookup"><span data-stu-id="09e1a-215">View details for an autoscale setting</span></span>
<span data-ttu-id="09e1a-216">Můžete použít `Get-Autoscalesetting` rutiny získat další informace o nastavení automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="09e1a-216">You can use the `Get-Autoscalesetting` cmdlet to retrieve more information about the autoscale setting.</span></span>

<span data-ttu-id="09e1a-217">Následující příklad ukazuje údaje o všechna nastavení škálování v prostředku skupiny "myrg1'.</span><span class="sxs-lookup"><span data-stu-id="09e1a-217">The following example shows details about all autoscale settings in the resource group 'myrg1'.</span></span>

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

<span data-ttu-id="09e1a-218">Následující příklad ukazuje údaje o všech nastavení škálování v prostředku skupiny "myrg1' a konkrétně nastavení automatického škálování s názvem 'MyScaleVMSSSetting'.</span><span class="sxs-lookup"><span data-stu-id="09e1a-218">The following example shows details about all autoscale settings in the resource group 'myrg1' and specifically the autoscale setting named 'MyScaleVMSSSetting'.</span></span>

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a><span data-ttu-id="09e1a-219">Odebrat nastavení automatického škálování</span><span class="sxs-lookup"><span data-stu-id="09e1a-219">Remove an autoscale setting</span></span>
<span data-ttu-id="09e1a-220">Můžete použít `Remove-Autoscalesetting` rutiny odstranit nastavení automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="09e1a-220">You can use the `Remove-Autoscalesetting` cmdlet to delete an autoscale setting.</span></span>

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-activity-log"></a><span data-ttu-id="09e1a-221">Správa profilů protokolu pro protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="09e1a-221">Manage log profiles for activity log</span></span>
<span data-ttu-id="09e1a-222">Můžete vytvořit *protokolu profil* a exportu dat z aktivity protokolu do účtu úložiště a vy můžete nakonfigurovat uchovávání dat pro ni.</span><span class="sxs-lookup"><span data-stu-id="09e1a-222">You can create a *log profile* and export data from your activity log to a storage account and you can configure data retention for it.</span></span> <span data-ttu-id="09e1a-223">Volitelně můžete také Streamovat data do vašeho centra událostí.</span><span class="sxs-lookup"><span data-stu-id="09e1a-223">Optionally, you can also stream the data to your Event Hub.</span></span> <span data-ttu-id="09e1a-224">Všimněte si, že tato funkce je aktuálně ve verzi Preview a je lze vytvořit pouze jeden profil protokolu podle předplatného.</span><span class="sxs-lookup"><span data-stu-id="09e1a-224">Note that this feature is currently in Preview and you can only create one log profile per subscription.</span></span> <span data-ttu-id="09e1a-225">Následující rutiny s vaším aktuálním předplatným slouží k vytvoření a Správa profilů protokolu.</span><span class="sxs-lookup"><span data-stu-id="09e1a-225">You can use the following cmdlets with your current subscription to create and manage log profiles.</span></span> <span data-ttu-id="09e1a-226">Můžete také určitý odběr.</span><span class="sxs-lookup"><span data-stu-id="09e1a-226">You can also choose a particular subscription.</span></span> <span data-ttu-id="09e1a-227">I když k aktuálním předplatném výchozí nastavení prostředí PowerShell, můžete kdykoliv změnit této pomocí `Set-AzureRmContext`.</span><span class="sxs-lookup"><span data-stu-id="09e1a-227">Although PowerShell defaults to the current subscription, you can always change that using `Set-AzureRmContext`.</span></span> <span data-ttu-id="09e1a-228">Můžete nakonfigurovat protokolu aktivity a data trasy k žádnému účtu úložiště nebo Centrum událostí v rámci tohoto předplatného.</span><span class="sxs-lookup"><span data-stu-id="09e1a-228">You can configure activity log to route data to any storage account or Event Hub within that subscription.</span></span> <span data-ttu-id="09e1a-229">Data se zapisují jako objekt blob soubory ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="09e1a-229">Data is written as blob files in JSON format.</span></span>

### <a name="get-a-log-profile"></a><span data-ttu-id="09e1a-230">Získat profil protokolu</span><span class="sxs-lookup"><span data-stu-id="09e1a-230">Get a log profile</span></span>
<span data-ttu-id="09e1a-231">Chcete-li načíst vaše stávající protokolu profilů, použijte `Get-AzureRmLogProfile` rutiny.</span><span class="sxs-lookup"><span data-stu-id="09e1a-231">To fetch your existing log profiles, use the `Get-AzureRmLogProfile` cmdlet.</span></span>

### <a name="add-a-log-profile-without-data-retention"></a><span data-ttu-id="09e1a-232">Přidat profil protokolu bez uchovávání dat</span><span class="sxs-lookup"><span data-stu-id="09e1a-232">Add a log profile without data retention</span></span>
```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a><span data-ttu-id="09e1a-233">Odebrat profil protokolu</span><span class="sxs-lookup"><span data-stu-id="09e1a-233">Remove a log profile</span></span>
```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a><span data-ttu-id="09e1a-234">Přidat profil protokolu s uchovávání dat</span><span class="sxs-lookup"><span data-stu-id="09e1a-234">Add a log profile with data retention</span></span>
<span data-ttu-id="09e1a-235">Můžete zadat **- RetentionInDays** vlastnost s počet dní, jako kladné celé číslo, které se uchovávají data.</span><span class="sxs-lookup"><span data-stu-id="09e1a-235">You can specify the **-RetentionInDays** property with the number of days, as a positive integer, where the data is retained.</span></span>

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a><span data-ttu-id="09e1a-236">Přidat profil protokolu s uchovávání a EventHub</span><span class="sxs-lookup"><span data-stu-id="09e1a-236">Add log profile with retention and EventHub</span></span>
<span data-ttu-id="09e1a-237">Kromě směrování dat do účtu úložiště, můžete také Streamovat ho do centra událostí.</span><span class="sxs-lookup"><span data-stu-id="09e1a-237">In addition to routing your data to storage account, you can also stream it to an Event Hub.</span></span> <span data-ttu-id="09e1a-238">Nezapomeňte, že v této verzi preview a úložiště konfigurace účtu je povinná, ale konfigurace centra událostí je volitelné.</span><span class="sxs-lookup"><span data-stu-id="09e1a-238">Note that in this preview release and the storage account configuration is mandatory but Event Hub configuration is optional.</span></span>

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a><span data-ttu-id="09e1a-239">Konfigurace protokolů diagnostiky</span><span class="sxs-lookup"><span data-stu-id="09e1a-239">Configure diagnostics logs</span></span>
<span data-ttu-id="09e1a-240">Mnoho služeb Azure poskytují další protokoly a telemetrie, které můžete nakonfigurovat tak, aby uložení dat v účtu úložiště Azure, odesílat do centra událostí nebo odeslat k pracovnímu prostoru analýzy protokolů OMS.</span><span class="sxs-lookup"><span data-stu-id="09e1a-240">Many Azure services provide additional logs and telemetry that can be configured to save data in your Azure Storage account, send to Event Hubs, and/or sent to an OMS Log Analytics workspace.</span></span> <span data-ttu-id="09e1a-241">Tuto operaci může provést pouze na úrovni prostředků a úložiště účet nebo události rozbočovače musí být ve stejné oblasti jako cílový prostředek, kde je nakonfigurované nastavení diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="09e1a-241">That operation can only be performed at a resource level and the storage account or event hub should be present in the same region as the target resource where the diagnostics setting is configured.</span></span>

### <a name="get-diagnostic-setting"></a><span data-ttu-id="09e1a-242">Získat nastavení diagnostiky</span><span class="sxs-lookup"><span data-stu-id="09e1a-242">Get diagnostic setting</span></span>
```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

<span data-ttu-id="09e1a-243">Zakažte nastavení diagnostiky</span><span class="sxs-lookup"><span data-stu-id="09e1a-243">Disable diagnostic setting</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

<span data-ttu-id="09e1a-244">Povolit nastavení diagnostiky bez uchování</span><span class="sxs-lookup"><span data-stu-id="09e1a-244">Enable diagnostic setting without retention</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

<span data-ttu-id="09e1a-245">Povolit nastavení diagnostiky s dobou uchování</span><span class="sxs-lookup"><span data-stu-id="09e1a-245">Enable diagnostic setting with retention</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

<span data-ttu-id="09e1a-246">Povolit nastavení diagnostiky se zachováním pro konkrétní kategorii</span><span class="sxs-lookup"><span data-stu-id="09e1a-246">Enable diagnostic setting with retention for a specific log category</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

<span data-ttu-id="09e1a-247">Povolit nastavení diagnostiky pro službu Event Hubs</span><span class="sxs-lookup"><span data-stu-id="09e1a-247">Enable diagnostic setting for Event Hubs</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Enable $true
```

<span data-ttu-id="09e1a-248">Povolit nastavení diagnostiky pro OMS</span><span class="sxs-lookup"><span data-stu-id="09e1a-248">Enable diagnostic setting for OMS</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -WorkspaceId 76d785fd-d1ce-4f50-8ca3-858fc819ca0f -Enabled $true

```
