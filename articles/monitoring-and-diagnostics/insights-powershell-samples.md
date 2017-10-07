---
title: "Ukázky úvodní aaaAzure monitorování PowerShell. | Dokumentace Microsoftu"
description: "Pomocí prostředí PowerShell tooaccess Azure monitorování funkce, jako je automatické škálování, výstrahy, webhooky a hledání protokoly aktivity."
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
ms.openlocfilehash: 6eece0b0227e0bbf08225bd330d0601169911f55
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="azure-monitor-powershell-quick-start-samples"></a><span data-ttu-id="6c411-104">Ukázek Azure PowerShell monitorování rychlý start</span><span class="sxs-lookup"><span data-stu-id="6c411-104">Azure Monitor PowerShell quick start samples</span></span>
<span data-ttu-id="6c411-105">Tento článek ukazuje ukázkové toohelp příkazy prostředí PowerShell měli přístup k funkcím monitorování Azure.</span><span class="sxs-lookup"><span data-stu-id="6c411-105">This article shows you sample PowerShell commands toohelp you access Azure Monitor features.</span></span> <span data-ttu-id="6c411-106">Azure monitorování umožňuje tooAutoScale cloudové služby, virtuální počítače a webové aplikace a toosend oznámení výstrah nebo volání webové adresy URL založené na hodnotách nakonfigurované telemetrická data.</span><span class="sxs-lookup"><span data-stu-id="6c411-106">Azure Monitor allows you tooAutoScale Cloud Services, Virtual Machines, and Web Apps and toosend alert notifications or call web URLs based on values of configured telemetry data.</span></span>

> [!NOTE]
> <span data-ttu-id="6c411-107">Azure monitorování je hello nový název pro co byla volána "Statistika Azure" až 25 září 2016.</span><span class="sxs-lookup"><span data-stu-id="6c411-107">Azure Monitor is hello new name for what was called "Azure Insights" until Sept 25th, 2016.</span></span> <span data-ttu-id="6c411-108">Ale hello obory názvů a proto hello následující příkazy stále obsahovat hello "insights".</span><span class="sxs-lookup"><span data-stu-id="6c411-108">However, hello namespaces and thus hello following commands still contain hello "insights".</span></span>
> 
> 

## <a name="set-up-powershell"></a><span data-ttu-id="6c411-109">Nastavení prostředí PowerShell</span><span class="sxs-lookup"><span data-stu-id="6c411-109">Set up PowerShell</span></span>
<span data-ttu-id="6c411-110">Pokud jste to ještě neudělali, nastavte toorun prostředí PowerShell v počítači.</span><span class="sxs-lookup"><span data-stu-id="6c411-110">If you haven't already, set up PowerShell toorun on your computer.</span></span> <span data-ttu-id="6c411-111">Další informace najdete v tématu [jak tooInstall a konfigurace prostředí PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="6c411-111">For more information, see [How tooInstall and Configure PowerShell](/powershell/azure/overview).</span></span>

## <a name="examples-in-this-article"></a><span data-ttu-id="6c411-112">Příklady v tomto článku</span><span class="sxs-lookup"><span data-stu-id="6c411-112">Examples in this article</span></span>
<span data-ttu-id="6c411-113">Příklady Hello v článku hello ilustrují, jak můžete použít rutiny Azure monitorování.</span><span class="sxs-lookup"><span data-stu-id="6c411-113">hello examples in hello article illustrate how you can use Azure Monitor cmdlets.</span></span> <span data-ttu-id="6c411-114">Můžete také zkontrolovat hello celý seznam rutin Azure Powershellu monitorování v [rutiny Azure monitorování (přehled)](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).</span><span class="sxs-lookup"><span data-stu-id="6c411-114">You can also review hello entire list of Azure Monitor PowerShell cmdlets at [Azure Monitor (Insights) Cmdlets](https://msdn.microsoft.com/library/azure/mt282452#40v=azure.200#41.aspx).</span></span>

## <a name="sign-in-and-use-subscriptions"></a><span data-ttu-id="6c411-115">Přihlásit a používat odběrů</span><span class="sxs-lookup"><span data-stu-id="6c411-115">Sign in and use subscriptions</span></span>
<span data-ttu-id="6c411-116">Nejdřív přihlaste tooyour předplatného Azure.</span><span class="sxs-lookup"><span data-stu-id="6c411-116">First, log in tooyour Azure subscription.</span></span>

```PowerShell
Login-AzureRmAccount
```

<span data-ttu-id="6c411-117">To vyžaduje toosign v.</span><span class="sxs-lookup"><span data-stu-id="6c411-117">This requires you toosign in.</span></span> <span data-ttu-id="6c411-118">Až to uděláte, váš účet, zobrazí se TenantID a výchozí ID předplatného.</span><span class="sxs-lookup"><span data-stu-id="6c411-118">Once you do, your Account, TenantID and default Subscription ID are displayed.</span></span> <span data-ttu-id="6c411-119">Všechny hello pracovní rutiny Azure v kontextu hello předplatného výchozí.</span><span class="sxs-lookup"><span data-stu-id="6c411-119">All hello Azure cmdlets work in hello context of your default subscription.</span></span> <span data-ttu-id="6c411-120">tooview hello seznam předplatných, budete mít přístup k, použijte následující příkaz hello.</span><span class="sxs-lookup"><span data-stu-id="6c411-120">tooview hello list of subscriptions you have access to, use hello following command.</span></span>

```PowerShell
Get-AzureRmSubscription
```

<span data-ttu-id="6c411-121">toochange pracovní kontext tooa jiné předplatné, hello použijte následující příkaz.</span><span class="sxs-lookup"><span data-stu-id="6c411-121">toochange your working context tooa different subscription, use hello following command.</span></span>

```PowerShell
Set-AzureRmContext -SubscriptionId <subscriptionid>
```


## <a name="retrieve-activity-log-for-a-subscription"></a><span data-ttu-id="6c411-122">Načtení protokol aktivit pro předplatné</span><span class="sxs-lookup"><span data-stu-id="6c411-122">Retrieve Activity log for a subscription</span></span>
<span data-ttu-id="6c411-123">Použití hello `Get-AzureRmLog` rutiny.</span><span class="sxs-lookup"><span data-stu-id="6c411-123">Use hello `Get-AzureRmLog` cmdlet.</span></span>  <span data-ttu-id="6c411-124">Hello následuje několik běžných příkladů.</span><span class="sxs-lookup"><span data-stu-id="6c411-124">hello following are some common examples.</span></span>

<span data-ttu-id="6c411-125">Získání položky protokolu z této toopresent času a data:</span><span class="sxs-lookup"><span data-stu-id="6c411-125">Get log entries from this time/date toopresent:</span></span>

```PowerShell
Get-AzureRmLog -StartTime 2016-03-01T10:30
```

<span data-ttu-id="6c411-126">Získáte položky protokolu mezi rozsahem času a data:</span><span class="sxs-lookup"><span data-stu-id="6c411-126">Get log entries between a time/date range:</span></span>

```PowerShell
Get-AzureRmLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

<span data-ttu-id="6c411-127">Získání položky protokolu z určité skupiny zdrojů:</span><span class="sxs-lookup"><span data-stu-id="6c411-127">Get log entries from a specific resource group:</span></span>

```PowerShell
Get-AzureRmLog -ResourceGroup 'myrg1'
```

<span data-ttu-id="6c411-128">Získáte položky protokolu od zprostředkovatele konkrétní prostředků mezi rozsahem času a data:</span><span class="sxs-lookup"><span data-stu-id="6c411-128">Get log entries from a specific resource provider between a time/date range:</span></span>

```PowerShell
Get-AzureRmLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

<span data-ttu-id="6c411-129">Získání všech položek protokolů s konkrétní volající:</span><span class="sxs-lookup"><span data-stu-id="6c411-129">Get all log entries with a specific caller:</span></span>

```PowerShell
Get-AzureRmLog -Caller 'myname@company.com'
```

<span data-ttu-id="6c411-130">Následující příkaz načte Hello hello posledních 1000 události z protokolu aktivit hello:</span><span class="sxs-lookup"><span data-stu-id="6c411-130">hello following command retrieves hello last 1000 events from hello activity log:</span></span>

```PowerShell
Get-AzureRmLog -MaxEvents 1000
```

<span data-ttu-id="6c411-131">`Get-AzureRmLog`podporuje mnoho dalších parametrů.</span><span class="sxs-lookup"><span data-stu-id="6c411-131">`Get-AzureRmLog` supports many other parameters.</span></span> <span data-ttu-id="6c411-132">V tématu hello `Get-AzureRmLog` pro další informace.</span><span class="sxs-lookup"><span data-stu-id="6c411-132">See hello `Get-AzureRmLog` reference for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="6c411-133">`Get-AzureRmLog`poskytuje pouze 15 dní historie.</span><span class="sxs-lookup"><span data-stu-id="6c411-133">`Get-AzureRmLog` only provides 15 days of history.</span></span> <span data-ttu-id="6c411-134">Pomocí hello **- MaxEvents** parametr vám umožní tooquery hello posledních N události, nad rámec 15 dnů.</span><span class="sxs-lookup"><span data-stu-id="6c411-134">Using hello **-MaxEvents** parameter allows you tooquery hello last N events, beyond 15 days.</span></span> <span data-ttu-id="6c411-135">tooaccess události starší než 15 dny, použijte hello REST API nebo sady SDK (C# ukázku pomocí hello SDK).</span><span class="sxs-lookup"><span data-stu-id="6c411-135">tooaccess events older than 15 days, use hello REST API or SDK (C# sample using hello SDK).</span></span> <span data-ttu-id="6c411-136">Pokud neuvedete **StartTime**, pak hello výchozí hodnota je **EndTime** minus jednu hodinu.</span><span class="sxs-lookup"><span data-stu-id="6c411-136">If you do not include **StartTime**, then hello default value is **EndTime** minus one hour.</span></span> <span data-ttu-id="6c411-137">Pokud neuvedete **EndTime**, pak hello výchozí hodnota je aktuální čas.</span><span class="sxs-lookup"><span data-stu-id="6c411-137">If you do not include **EndTime**, then hello default value is current time.</span></span> <span data-ttu-id="6c411-138">Všechny časy jsou ve formátu UTC.</span><span class="sxs-lookup"><span data-stu-id="6c411-138">All times are in UTC.</span></span>
> 
> 

## <a name="retrieve-alerts-history"></a><span data-ttu-id="6c411-139">Načtení historie výstrah</span><span class="sxs-lookup"><span data-stu-id="6c411-139">Retrieve alerts history</span></span>
<span data-ttu-id="6c411-140">všechny výstrahy událostí, můžete dát dotaz na tooview hello protokolů Azure Resource Manager pomocí hello následující příklady.</span><span class="sxs-lookup"><span data-stu-id="6c411-140">tooview all alert events, you can query hello Azure Resource Manager logs using hello following examples.</span></span>

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/alertRules" -DetailedOutput -StartTime 2015-03-01
```

<span data-ttu-id="6c411-141">pravidlo tooview hello historie pro konkrétní výstrahu, můžete použít hello `Get-AzureRmAlertHistory` rutiny předávání v ID prostředku hello hello pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="6c411-141">tooview hello history for a specific alert rule, you can use hello `Get-AzureRmAlertHistory` cmdlet, passing in hello resource ID of hello alert rule.</span></span>

```PowerShell
Get-AzureRmAlertHistory -ResourceId /subscriptions/s1/resourceGroups/rg1/providers/microsoft.insights/alertrules/myalert -StartTime 2016-03-1 -Status Activated
```

<span data-ttu-id="6c411-142">Hello `Get-AzureRmAlertHistory` rutina podporuje různé parametry.</span><span class="sxs-lookup"><span data-stu-id="6c411-142">hello `Get-AzureRmAlertHistory` cmdlet supports various parameters.</span></span> <span data-ttu-id="6c411-143">Další informace najdete v tématu [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).</span><span class="sxs-lookup"><span data-stu-id="6c411-143">More information, see [Get-AlertHistory](https://msdn.microsoft.com/library/mt282453.aspx).</span></span>

## <a name="retrieve-information-on-alert-rules"></a><span data-ttu-id="6c411-144">Načíst informace o pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="6c411-144">Retrieve information on alert rules</span></span>
<span data-ttu-id="6c411-145">Všechny následující příkazy hello fungují na skupinu prostředků s názvem "montest".</span><span class="sxs-lookup"><span data-stu-id="6c411-145">All of hello following commands act on a Resource Group named "montest".</span></span>

<span data-ttu-id="6c411-146">Zobrazte všechny vlastnosti hello hello pravidlo výstrahy:</span><span class="sxs-lookup"><span data-stu-id="6c411-146">View all hello properties of hello alert rule:</span></span>

```PowerShell
Get-AzureRmAlertRule -Name simpletestCPU -ResourceGroup montest -DetailedOutput
```

<span data-ttu-id="6c411-147">Načtěte všechny výstrahy pro skupinu prostředků:</span><span class="sxs-lookup"><span data-stu-id="6c411-147">Retrieve all alerts on a resource group:</span></span>

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest
```

<span data-ttu-id="6c411-148">Načtěte všechny výstrahy pravidla nastavená pro cílový prostředek.</span><span class="sxs-lookup"><span data-stu-id="6c411-148">Retrieve all alert rules set for a target resource.</span></span> <span data-ttu-id="6c411-149">Všechna pravidla výstrah je třeba nastavit na virtuálním počítači.</span><span class="sxs-lookup"><span data-stu-id="6c411-149">For example, all alert rules set on a VM.</span></span>

```PowerShell
Get-AzureRmAlertRule -ResourceGroup montest -TargetResourceId /subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig
```

<span data-ttu-id="6c411-150">`Get-AzureRmAlertRule`podporuje další parametry.</span><span class="sxs-lookup"><span data-stu-id="6c411-150">`Get-AzureRmAlertRule` supports other parameters.</span></span> <span data-ttu-id="6c411-151">V tématu [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) Další informace.</span><span class="sxs-lookup"><span data-stu-id="6c411-151">See [Get-AlertRule](https://msdn.microsoft.com/library/mt282459.aspx) for more information.</span></span>

## <a name="create-metric-alerts"></a><span data-ttu-id="6c411-152">Vytvoření metriky výstrahy</span><span class="sxs-lookup"><span data-stu-id="6c411-152">Create metric alerts</span></span>
<span data-ttu-id="6c411-153">Můžete použít hello `Add-AlertRule` toocreate rutiny, aktualizovat nebo zakázat pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="6c411-153">You can use hello `Add-AlertRule` cmdlet toocreate, update or disable an alert rule.</span></span>

<span data-ttu-id="6c411-154">Můžete vytvořit e-mailu a webhooku vlastností pomocí `New-AzureRmAlertRuleEmail` a `New-AzureRmAlertRuleWebhook`, v uvedeném pořadí.</span><span class="sxs-lookup"><span data-stu-id="6c411-154">You can create email and webhook properties using  `New-AzureRmAlertRuleEmail` and `New-AzureRmAlertRuleWebhook`, respectively.</span></span> <span data-ttu-id="6c411-155">V rutině hello pravidlo výstrahy, přiřadit jako akce toohello **akce** vlastnost hello pravidlo výstrahy.</span><span class="sxs-lookup"><span data-stu-id="6c411-155">In hello Alert rule cmdlet, assign these as actions toohello **Actions** property of hello Alert Rule.</span></span>

<span data-ttu-id="6c411-156">Hello následující tabulka popisuje hello parametry a hodnoty používané toocreate výstrahu pomocí metriky.</span><span class="sxs-lookup"><span data-stu-id="6c411-156">hello following table describes hello parameters and values used toocreate an alert using a metric.</span></span>

| <span data-ttu-id="6c411-157">Parametr</span><span class="sxs-lookup"><span data-stu-id="6c411-157">parameter</span></span> | <span data-ttu-id="6c411-158">hodnota</span><span class="sxs-lookup"><span data-stu-id="6c411-158">value</span></span> |
| --- | --- |
| <span data-ttu-id="6c411-159">Name (Název)</span><span class="sxs-lookup"><span data-stu-id="6c411-159">Name</span></span> |<span data-ttu-id="6c411-160">simpletestdiskwrite</span><span class="sxs-lookup"><span data-stu-id="6c411-160">simpletestdiskwrite</span></span> |
| <span data-ttu-id="6c411-161">Umístění tohoto pravidla výstrah</span><span class="sxs-lookup"><span data-stu-id="6c411-161">Location of this alert rule</span></span> |<span data-ttu-id="6c411-162">Východ USA</span><span class="sxs-lookup"><span data-stu-id="6c411-162">East US</span></span> |
| <span data-ttu-id="6c411-163">ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="6c411-163">ResourceGroup</span></span> |<span data-ttu-id="6c411-164">montest</span><span class="sxs-lookup"><span data-stu-id="6c411-164">montest</span></span> |
| <span data-ttu-id="6c411-165">TargetResourceId</span><span class="sxs-lookup"><span data-stu-id="6c411-165">TargetResourceId</span></span> |<span data-ttu-id="6c411-166">/subscriptions/S1/resourceGroups/montest/providers/Microsoft.COMPUTE/virtualMachines/testconfig</span><span class="sxs-lookup"><span data-stu-id="6c411-166">/subscriptions/s1/resourceGroups/montest/providers/Microsoft.Compute/virtualMachines/testconfig</span></span> |
| <span data-ttu-id="6c411-167">MetricName hello výstrahy, která je vytvořena</span><span class="sxs-lookup"><span data-stu-id="6c411-167">MetricName of hello alert that is created</span></span> |<span data-ttu-id="6c411-168">\PhysicalDisk (_celkem) \Disk zápisů za sekundu. V tématu hello `Get-MetricDefinitions` rutiny o tom, jak tooretrieve hello přesný metriky názvy</span><span class="sxs-lookup"><span data-stu-id="6c411-168">\PhysicalDisk(_Total)\Disk Writes/sec. See hello `Get-MetricDefinitions` cmdlet about how tooretrieve hello exact metric names</span></span> |
| <span data-ttu-id="6c411-169">Operátor</span><span class="sxs-lookup"><span data-stu-id="6c411-169">operator</span></span> |<span data-ttu-id="6c411-170">GreaterThan</span><span class="sxs-lookup"><span data-stu-id="6c411-170">GreaterThan</span></span> |
| <span data-ttu-id="6c411-171">Prahová hodnota (počet za sekundu za pro tato metrika)</span><span class="sxs-lookup"><span data-stu-id="6c411-171">Threshold value (count/sec in for this metric)</span></span> |<span data-ttu-id="6c411-172">1</span><span class="sxs-lookup"><span data-stu-id="6c411-172">1</span></span> |
| <span data-ttu-id="6c411-173">Velikost_okna (ve formátu hh: mm:)</span><span class="sxs-lookup"><span data-stu-id="6c411-173">WindowSize (hh:mm:ss format)</span></span> |<span data-ttu-id="6c411-174">00:05:00</span><span class="sxs-lookup"><span data-stu-id="6c411-174">00:05:00</span></span> |
| <span data-ttu-id="6c411-175">agregátoru (statistiky hello metriku, která používá průměrný počet, v takovém případě)</span><span class="sxs-lookup"><span data-stu-id="6c411-175">aggregator (statistic of hello metric, which uses Average count, in this case)</span></span> |<span data-ttu-id="6c411-176">Průměr</span><span class="sxs-lookup"><span data-stu-id="6c411-176">Average</span></span> |
| <span data-ttu-id="6c411-177">vlastní e-mailů (řetězec pole)</span><span class="sxs-lookup"><span data-stu-id="6c411-177">custom emails (string array)</span></span> |<span data-ttu-id="6c411-178">'foo@example.com','bar@example.com'</span><span class="sxs-lookup"><span data-stu-id="6c411-178">'foo@example.com','bar@example.com'</span></span> |
| <span data-ttu-id="6c411-179">odesílání e-mailu tooowners, přispěvatelé a čtenáři</span><span class="sxs-lookup"><span data-stu-id="6c411-179">send email tooowners, contributors and readers</span></span> |<span data-ttu-id="6c411-180">-SendToServiceOwners</span><span class="sxs-lookup"><span data-stu-id="6c411-180">-SendToServiceOwners</span></span> |

<span data-ttu-id="6c411-181">Vytvoří akci, e-mailu</span><span class="sxs-lookup"><span data-stu-id="6c411-181">Create an Email action</span></span>

```PowerShell
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail myname@company.com
```

<span data-ttu-id="6c411-182">Vytvoření Webhooku akce</span><span class="sxs-lookup"><span data-stu-id="6c411-182">Create a Webhook action</span></span>

```PowerShell
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri https://example.com?token=mytoken
```

<span data-ttu-id="6c411-183">Vytvořit pravidlo výstrahy hello na hello procesoru % metriky na klasické virtuální počítač</span><span class="sxs-lookup"><span data-stu-id="6c411-183">Create hello alert rule on hello CPU% metric on a classic VM</span></span>

```PowerShell
Add-AzureRmMetricAlertRule -Name vmcpu_gt_1 -Location "East US" -ResourceGroup myrg1 -TargetResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.ClassicCompute/virtualMachines/my_vm1 -MetricName "Percentage CPU" -Operator GreaterThan -Threshold 1 -WindowSize 00:05:00 -TimeAggregationOperator Average -Actions $actionEmail, $actionWebhook -Description "alert on CPU > 1%"
```

<span data-ttu-id="6c411-184">Načtení pravidlo výstrahy hello</span><span class="sxs-lookup"><span data-stu-id="6c411-184">Retrieve hello alert rule</span></span>

```PowerShell
Get-AzureRmAlertRule -Name vmcpu_gt_1 -ResourceGroup myrg1 -DetailedOutput
```

<span data-ttu-id="6c411-185">výstrahy rutiny Hello přidat také aktualizuje pravidlo hello, pokud se pravidlo výstrahy pro hello zadané vlastnosti již existuje.</span><span class="sxs-lookup"><span data-stu-id="6c411-185">hello Add alert cmdlet also updates hello rule if an alert rule already exists for hello given properties.</span></span> <span data-ttu-id="6c411-186">pravidlo výstrahy toodisable zahrnout parametr hello **- DisableRule**.</span><span class="sxs-lookup"><span data-stu-id="6c411-186">toodisable an alert rule, include hello parameter **-DisableRule**.</span></span>

## <a name="get-a-list-of-available-metrics-for-alerts"></a><span data-ttu-id="6c411-187">Získat seznam dostupné metriky pro výstrahy</span><span class="sxs-lookup"><span data-stu-id="6c411-187">Get a list of available metrics for alerts</span></span>
<span data-ttu-id="6c411-188">Můžete použít hello `Get-AzureRmMetricDefinition` rutiny tooview hello seznamu všechny metriky pro konkrétní zdroje.</span><span class="sxs-lookup"><span data-stu-id="6c411-188">You can use hello `Get-AzureRmMetricDefinition` cmdlet tooview hello list of all metrics for a specific resource.</span></span>

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id>
```

<span data-ttu-id="6c411-189">Hello následující příklad vygeneruje tabulku s hello metrika název a hello jednotky pro ni.</span><span class="sxs-lookup"><span data-stu-id="6c411-189">hello following example generates a table with hello metric Name and hello Unit for it.</span></span>

```PowerShell
Get-AzureRmMetricDefinition -ResourceId <resource_id> | Format-Table -Property Name,Unit
```

<span data-ttu-id="6c411-190">Úplný seznam dostupných možností pro `Get-AzureRmMetricDefinition` je k dispozici na [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).</span><span class="sxs-lookup"><span data-stu-id="6c411-190">A full list of available options for `Get-AzureRmMetricDefinition` is available at [Get-MetricDefinitions](https://msdn.microsoft.com/library/mt282458.aspx).</span></span>

## <a name="create-and-manage-autoscale-settings"></a><span data-ttu-id="6c411-191">Vytvořit a spravovat nastavení automatického škálování</span><span class="sxs-lookup"><span data-stu-id="6c411-191">Create and manage AutoScale settings</span></span>
<span data-ttu-id="6c411-192">Prostředek, například webovou aplikaci, virtuální počítač, Cloudová služba nebo sadu škálování virtuálního počítače může mít pouze jeden nastavení automatického škálování pro něj nakonfigurovali.</span><span class="sxs-lookup"><span data-stu-id="6c411-192">A resource, such as a Web app, VM, Cloud Service or Virtual Machine Scale Set can have only one autoscale setting configured for it.</span></span>
<span data-ttu-id="6c411-193">Každé nastavení automatického škálování však může mít několik profilů.</span><span class="sxs-lookup"><span data-stu-id="6c411-193">However, each autoscale setting can have multiple profiles.</span></span> <span data-ttu-id="6c411-194">Například jeden pro profil na základě výkonu škálování a druhý pro profil na základě plánu.</span><span class="sxs-lookup"><span data-stu-id="6c411-194">For example, one for a performance-based scale profile and a second one for a schedule-based profile.</span></span> <span data-ttu-id="6c411-195">Každý profil může mít víc pravidel, které jsou nakonfigurované na něm.</span><span class="sxs-lookup"><span data-stu-id="6c411-195">Each profile can have multiple rules configured on it.</span></span> <span data-ttu-id="6c411-196">Další informace o škálování najdete v tématu [jak tooAutoscale aplikace](../cloud-services/cloud-services-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="6c411-196">For more information about Autoscale, see [How tooAutoscale an Application](../cloud-services/cloud-services-how-to-scale.md).</span></span>

<span data-ttu-id="6c411-197">Zde jsou hello kroky, které budeme používat:</span><span class="sxs-lookup"><span data-stu-id="6c411-197">Here are hello steps we will use:</span></span>

1. <span data-ttu-id="6c411-198">Vytvoření pravidel.</span><span class="sxs-lookup"><span data-stu-id="6c411-198">Create rule(s).</span></span>
2. <span data-ttu-id="6c411-199">Vytvořte profily mapování hello pravidla, které jste vytvořili dříve toohello profily.</span><span class="sxs-lookup"><span data-stu-id="6c411-199">Create profile(s) mapping hello rules that you created previously toohello profiles.</span></span>
3. <span data-ttu-id="6c411-200">Volitelné: Konfigurace vlastností webhooku a e-mailu vytvořte oznámení o škálování.</span><span class="sxs-lookup"><span data-stu-id="6c411-200">Optional: Create notifications for autoscale by configuring webhook and email properties.</span></span>
4. <span data-ttu-id="6c411-201">Vytvoření nastavení automatického škálování s názvem hello cílovému prostředku tak, že mapuje hello profily a oznámení, které jste vytvořili v předchozích krocích hello.</span><span class="sxs-lookup"><span data-stu-id="6c411-201">Create an autoscale setting with a name on hello target resource by mapping hello profiles and notifications that you created in hello previous steps.</span></span>

<span data-ttu-id="6c411-202">Hello následující příklady ukazují můžete jak můžete vytvořit na nastavení automatického škálování pro sadu škálování virtuálního počítače pro operační systém Windows na základě pomocí metriky využití hello procesoru.</span><span class="sxs-lookup"><span data-stu-id="6c411-202">hello following examples show you how you can create an Autoscale setting for a Virtual Machine Scale Set for a Windows operating system based by using hello CPU utilization metric.</span></span>

<span data-ttu-id="6c411-203">Nejprve vytvořte pravidlo tooscale-out, větší počet instancí.</span><span class="sxs-lookup"><span data-stu-id="6c411-203">First, create a rule tooscale-out, with an instance count increase.</span></span>

```PowerShell
$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Increase -ScaleActionValue 1
```        

<span data-ttu-id="6c411-204">Dále vytvořte pravidlo tooscale v s snížení počtu instancí.</span><span class="sxs-lookup"><span data-stu-id="6c411-204">Next, create a rule tooscale-in, with an instance count decrease.</span></span>

```PowerShell
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -Operator GreaterThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:10:00 -ScaleActionCooldown 00:10:00 -ScaleActionDirection Decrease -ScaleActionValue 1
```

<span data-ttu-id="6c411-205">Pak vytvořte profil pro pravidla hello.</span><span class="sxs-lookup"><span data-stu-id="6c411-205">Then, create a profile for hello rules.</span></span>

```PowerShell
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "My_Profile"
```

<span data-ttu-id="6c411-206">Vytvoření vlastnosti webhooku.</span><span class="sxs-lookup"><span data-stu-id="6c411-206">Create a webhook property.</span></span>

```PowerShell
$webhook_scale = New-AzureRmAutoscaleWebhook -ServiceUri "https://example.com?mytoken=mytokenvalue"
```

<span data-ttu-id="6c411-207">Vytvořit hello oznámení vlastnost pro nastavení automatického škálování hello, včetně e-mailů a hello webhooku, který jste vytvořili dříve.</span><span class="sxs-lookup"><span data-stu-id="6c411-207">Create hello notification property for hello autoscale setting, including email and hello webhook that you created previously.</span></span>

```PowerShell
$notification1= New-AzureRmAutoscaleNotification -CustomEmails ashwink@microsoft.com -SendEmailToSubscriptionAdministrators SendEmailToSubscriptionCoAdministrators -Webhooks $webhook_scale
```

<span data-ttu-id="6c411-208">Nakonec vytvořte hello škálování nastavení tooadd hello profil, který jste vytvořili výše.</span><span class="sxs-lookup"><span data-stu-id="6c411-208">Finally, create hello autoscale setting tooadd hello profile that you created above.</span></span>

```PowerShell
Add-AzureRmAutoscaleSetting -Location "East US" -Name "MyScaleVMSSSetting" -ResourceGroup big2 -TargetResourceId /subscriptions/s1/resourceGroups/big2/providers/Microsoft.Compute/virtualMachineScaleSets/big2 -AutoscaleProfiles $profile1 -Notifications $notification1
```

<span data-ttu-id="6c411-209">Další informace o správě nastavení automatického škálování najdete v tématu [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).</span><span class="sxs-lookup"><span data-stu-id="6c411-209">For more information about managing Autoscale settings, see [Get-AutoscaleSetting](https://msdn.microsoft.com/library/mt282461.aspx).</span></span>

## <a name="autoscale-history"></a><span data-ttu-id="6c411-210">Historie škálování</span><span class="sxs-lookup"><span data-stu-id="6c411-210">Autoscale history</span></span>
<span data-ttu-id="6c411-211">Hello následující příklad ukazuje, jak zobrazit nedávné škálování a výstraha události.</span><span class="sxs-lookup"><span data-stu-id="6c411-211">hello following example shows you how you can view recent autoscale and alert events.</span></span> <span data-ttu-id="6c411-212">Použijte automatické škálování historie hello vyhledávání tooview hello aktivity protokolu.</span><span class="sxs-lookup"><span data-stu-id="6c411-212">Use hello activity log search tooview hello autoscale history.</span></span>

```PowerShell
Get-AzureRmLog -Caller "Microsoft.Insights/autoscaleSettings" -DetailedOutput -StartTime 2015-03-01
```

<span data-ttu-id="6c411-213">Můžete použít hello `Get-AzureRmAutoScaleHistory` rutiny tooretrieve historie škálování.</span><span class="sxs-lookup"><span data-stu-id="6c411-213">You can use hello `Get-AzureRmAutoScaleHistory` cmdlet tooretrieve AutoScale history.</span></span>

```PowerShell
Get-AzureRmAutoScaleHistory -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/microsoft.insights/autoscalesettings/myScaleSetting -StartTime 2016-03-15 -DetailedOutput
```

<span data-ttu-id="6c411-214">Další informace najdete v tématu [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).</span><span class="sxs-lookup"><span data-stu-id="6c411-214">For more information, see [Get-AutoscaleHistory](https://msdn.microsoft.com/library/mt282464.aspx).</span></span>

### <a name="view-details-for-an-autoscale-setting"></a><span data-ttu-id="6c411-215">Zobrazit podrobnosti o nastavení automatického škálování</span><span class="sxs-lookup"><span data-stu-id="6c411-215">View details for an autoscale setting</span></span>
<span data-ttu-id="6c411-216">Můžete použít hello `Get-Autoscalesetting` rutiny tooretrieve Další informace o nastavení automatického škálování hello.</span><span class="sxs-lookup"><span data-stu-id="6c411-216">You can use hello `Get-Autoscalesetting` cmdlet tooretrieve more information about hello autoscale setting.</span></span>

<span data-ttu-id="6c411-217">Hello následující příklad ukazuje údaje o všechna nastavení automatického škálování v myrg1' hello prostředku skupiny".</span><span class="sxs-lookup"><span data-stu-id="6c411-217">hello following example shows details about all autoscale settings in hello resource group 'myrg1'.</span></span>

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -DetailedOutput
```

<span data-ttu-id="6c411-218">Hello následující příklad zobrazuje podrobnosti o všech nastaveních škálování v myrg1' hello prostředku skupiny"a konkrétně hello nastavení automatického škálování s názvem 'MyScaleVMSSSetting'.</span><span class="sxs-lookup"><span data-stu-id="6c411-218">hello following example shows details about all autoscale settings in hello resource group 'myrg1' and specifically hello autoscale setting named 'MyScaleVMSSSetting'.</span></span>

```PowerShell
Get-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting -DetailedOutput
```

### <a name="remove-an-autoscale-setting"></a><span data-ttu-id="6c411-219">Odebrat nastavení automatického škálování</span><span class="sxs-lookup"><span data-stu-id="6c411-219">Remove an autoscale setting</span></span>
<span data-ttu-id="6c411-220">Můžete použít hello `Remove-Autoscalesetting` toodelete rutiny nastavení automatického škálování.</span><span class="sxs-lookup"><span data-stu-id="6c411-220">You can use hello `Remove-Autoscalesetting` cmdlet toodelete an autoscale setting.</span></span>

```PowerShell
Remove-AzureRmAutoscalesetting -ResourceGroup myrg1 -Name MyScaleVMSSSetting
```

## <a name="manage-log-profiles-for-activity-log"></a><span data-ttu-id="6c411-221">Správa profilů protokolu pro protokol aktivit</span><span class="sxs-lookup"><span data-stu-id="6c411-221">Manage log profiles for activity log</span></span>
<span data-ttu-id="6c411-222">Můžete vytvořit *protokolu profil* a exportu dat z vašeho účtu úložiště tooa protokolu aktivity a vy můžete nakonfigurovat uchovávání dat pro ni.</span><span class="sxs-lookup"><span data-stu-id="6c411-222">You can create a *log profile* and export data from your activity log tooa storage account and you can configure data retention for it.</span></span> <span data-ttu-id="6c411-223">Volitelně můžete také stream hello data tooyour centra událostí.</span><span class="sxs-lookup"><span data-stu-id="6c411-223">Optionally, you can also stream hello data tooyour Event Hub.</span></span> <span data-ttu-id="6c411-224">Všimněte si, že tato funkce je aktuálně ve verzi Preview a je lze vytvořit pouze jeden profil protokolu podle předplatného.</span><span class="sxs-lookup"><span data-stu-id="6c411-224">Note that this feature is currently in Preview and you can only create one log profile per subscription.</span></span> <span data-ttu-id="6c411-225">Můžete použít následující rutiny s vaší aktuální předplatné toocreate hello a spravovat profily protokolu.</span><span class="sxs-lookup"><span data-stu-id="6c411-225">You can use hello following cmdlets with your current subscription toocreate and manage log profiles.</span></span> <span data-ttu-id="6c411-226">Můžete také určitý odběr.</span><span class="sxs-lookup"><span data-stu-id="6c411-226">You can also choose a particular subscription.</span></span> <span data-ttu-id="6c411-227">I když prostředí PowerShell výchozí toohello aktuální předplatné, můžete kdykoliv změnit této pomocí `Set-AzureRmContext`.</span><span class="sxs-lookup"><span data-stu-id="6c411-227">Although PowerShell defaults toohello current subscription, you can always change that using `Set-AzureRmContext`.</span></span> <span data-ttu-id="6c411-228">V rámci tohoto předplatného můžete nakonfigurovat účet úložiště aktivity protokolu tooroute data tooany nebo Centrum událostí.</span><span class="sxs-lookup"><span data-stu-id="6c411-228">You can configure activity log tooroute data tooany storage account or Event Hub within that subscription.</span></span> <span data-ttu-id="6c411-229">Data se zapisují jako objekt blob soubory ve formátu JSON.</span><span class="sxs-lookup"><span data-stu-id="6c411-229">Data is written as blob files in JSON format.</span></span>

### <a name="get-a-log-profile"></a><span data-ttu-id="6c411-230">Získat profil protokolu</span><span class="sxs-lookup"><span data-stu-id="6c411-230">Get a log profile</span></span>
<span data-ttu-id="6c411-231">toofetch existujících protokolu profilů, použijte hello `Get-AzureRmLogProfile` rutiny.</span><span class="sxs-lookup"><span data-stu-id="6c411-231">toofetch your existing log profiles, use hello `Get-AzureRmLogProfile` cmdlet.</span></span>

### <a name="add-a-log-profile-without-data-retention"></a><span data-ttu-id="6c411-232">Přidat profil protokolu bez uchovávání dat</span><span class="sxs-lookup"><span data-stu-id="6c411-232">Add a log profile without data retention</span></span>
```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia
```

### <a name="remove-a-log-profile"></a><span data-ttu-id="6c411-233">Odebrat profil protokolu</span><span class="sxs-lookup"><span data-stu-id="6c411-233">Remove a log profile</span></span>
```PowerShell
Remove-AzureRmLogProfile -name my_log_profile_s1
```

### <a name="add-a-log-profile-with-data-retention"></a><span data-ttu-id="6c411-234">Přidat profil protokolu s uchovávání dat</span><span class="sxs-lookup"><span data-stu-id="6c411-234">Add a log profile with data retention</span></span>
<span data-ttu-id="6c411-235">Můžete zadat hello **- RetentionInDays** vlastnost s hello počet dní, jako kladné celé číslo, které mají být uchována hello data.</span><span class="sxs-lookup"><span data-stu-id="6c411-235">You can specify hello **-RetentionInDays** property with hello number of days, as a positive integer, where hello data is retained.</span></span>

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

### <a name="add-log-profile-with-retention-and-eventhub"></a><span data-ttu-id="6c411-236">Přidat profil protokolu s uchovávání a EventHub</span><span class="sxs-lookup"><span data-stu-id="6c411-236">Add log profile with retention and EventHub</span></span>
<span data-ttu-id="6c411-237">V toorouting přidání účtu toostorage dat je můžete také Streamovat ho tooan centra událostí.</span><span class="sxs-lookup"><span data-stu-id="6c411-237">In addition toorouting your data toostorage account, you can also stream it tooan Event Hub.</span></span> <span data-ttu-id="6c411-238">Poznámka: v této verzi preview verze a hello konfigurace účtu úložiště je povinná však Konfigurace centra událostí je volitelné.</span><span class="sxs-lookup"><span data-stu-id="6c411-238">Note that in this preview release and hello storage account configuration is mandatory but Event Hub configuration is optional.</span></span>

```PowerShell
Add-AzureRmLogProfile -Name my_log_profile_s1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/my_storage -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Locations global,westus,eastus,northeurope,westeurope,eastasia,southeastasia,japaneast,japanwest,northcentralus,southcentralus,eastus2,centralus,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia -RetentionInDays 90
```

## <a name="configure-diagnostics-logs"></a><span data-ttu-id="6c411-239">Konfigurace protokolů diagnostiky</span><span class="sxs-lookup"><span data-stu-id="6c411-239">Configure diagnostics logs</span></span>
<span data-ttu-id="6c411-240">Mnoho služeb Azure poskytují další protokoly a telemetrii, kterou může být nakonfigurované toosave data ve vašem účtu úložiště Azure odeslat tooEvent centra nebo odeslat pracovní prostor analýzy protokolů OMS tooan.</span><span class="sxs-lookup"><span data-stu-id="6c411-240">Many Azure services provide additional logs and telemetry that can be configured toosave data in your Azure Storage account, send tooEvent Hubs, and/or sent tooan OMS Log Analytics workspace.</span></span> <span data-ttu-id="6c411-241">Tuto operaci může provést pouze na úrovni prostředků a hello úložiště účet nebo event hub by měl být součástí hello stejné oblasti jako hello cílový prostředek, kde je nakonfigurované nastavení diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="6c411-241">That operation can only be performed at a resource level and hello storage account or event hub should be present in hello same region as hello target resource where hello diagnostics setting is configured.</span></span>

### <a name="get-diagnostic-setting"></a><span data-ttu-id="6c411-242">Získat nastavení diagnostiky</span><span class="sxs-lookup"><span data-stu-id="6c411-242">Get diagnostic setting</span></span>
```PowerShell
Get-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp
```

<span data-ttu-id="6c411-243">Zakažte nastavení diagnostiky</span><span class="sxs-lookup"><span data-stu-id="6c411-243">Disable diagnostic setting</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $false
```

<span data-ttu-id="6c411-244">Povolit nastavení diagnostiky bez uchování</span><span class="sxs-lookup"><span data-stu-id="6c411-244">Enable diagnostic setting without retention</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true
```

<span data-ttu-id="6c411-245">Povolit nastavení diagnostiky s dobou uchování</span><span class="sxs-lookup"><span data-stu-id="6c411-245">Enable diagnostic setting with retention</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Logic/workflows/andy0315logicapp -StorageAccountId /subscriptions/s1/resourceGroups/Default-Storage-WestUS/providers/Microsoft.Storage/storageAccounts/mystorageaccount -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

<span data-ttu-id="6c411-246">Povolit nastavení diagnostiky se zachováním pro konkrétní kategorii</span><span class="sxs-lookup"><span data-stu-id="6c411-246">Enable diagnostic setting with retention for a specific log category</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -StorageAccountId /subscriptions/s1/resourceGroups/myrg1/providers/Microsoft.Storage/storageAccounts/sakteststorage -Categories NetworkSecurityGroupEvent -Enable $true -RetentionEnabled $true -RetentionInDays 90
```

<span data-ttu-id="6c411-247">Povolit nastavení diagnostiky pro službu Event Hubs</span><span class="sxs-lookup"><span data-stu-id="6c411-247">Enable diagnostic setting for Event Hubs</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -serviceBusRuleId /subscriptions/s1/resourceGroups/Default-ServiceBus-EastUS/providers/Microsoft.ServiceBus/namespaces/mytestSB/authorizationrules/RootManageSharedAccessKey -Enable $true
```

<span data-ttu-id="6c411-248">Povolit nastavení diagnostiky pro OMS</span><span class="sxs-lookup"><span data-stu-id="6c411-248">Enable diagnostic setting for OMS</span></span>

```PowerShell
Set-AzureRmDiagnosticSetting -ResourceId /subscriptions/s1/resourceGroups/insights-integration/providers/Microsoft.Network/networkSecurityGroups/viruela1 -WorkspaceId 76d785fd-d1ce-4f50-8ca3-858fc819ca0f -Enabled $true

```
