---
title: "Automatické škálování a virtuálního počítače sady škálování | Microsoft Docs"
description: "Další informace o použití diagnostiky a škálování prostředky automaticky škálovat virtuální počítače ve škálovací sadě."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d29a3385-179e-4331-a315-daa7ea5701df
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: adegeo
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 06ff9d9ae1dd8256f0d22c1a60ed6a85554f1f17
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-automatic-scaling-and-virtual-machine-scale-sets"></a><span data-ttu-id="b00de-103">Jak používat automatické škálování a sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="b00de-103">How to use automatic scaling and Virtual Machine Scale Sets</span></span>
<span data-ttu-id="b00de-104">Automatické škálování virtuálních počítačů v sadě škálování je vytvoření nebo odstranění počítačů v sadě podle potřeby tak, aby odpovídaly požadavkům na výkon.</span><span class="sxs-lookup"><span data-stu-id="b00de-104">Automatic scaling of virtual machines in a scale set is the creation or deletion of machines in the set as needed to match performance requirements.</span></span> <span data-ttu-id="b00de-105">S růstem objem pracovní aplikace, může vyžadovat další zdroje, které umožní, aby efektivní provádění úloh.</span><span class="sxs-lookup"><span data-stu-id="b00de-105">As the volume of work grows, an application may require additional resources to enable it to effectively perform tasks.</span></span>

<span data-ttu-id="b00de-106">Automatické škálování je automatizovaný proces, pomáhá usnadňují režie na správu.</span><span class="sxs-lookup"><span data-stu-id="b00de-106">Automatic scaling is an automated process that helps ease management overhead.</span></span> <span data-ttu-id="b00de-107">Protože se sníží režie, nemusíte průběžně monitorovat výkon systému nebo rozhodnout, jak spravovat prostředky.</span><span class="sxs-lookup"><span data-stu-id="b00de-107">By reducing overhead, you don't need to continually monitor system performance or decide how to manage resources.</span></span> <span data-ttu-id="b00de-108">Škálování je elastické proces.</span><span class="sxs-lookup"><span data-stu-id="b00de-108">Scaling is an elastic process.</span></span> <span data-ttu-id="b00de-109">Při rostoucí zátěži, můžete přidat další zdroje.</span><span class="sxs-lookup"><span data-stu-id="b00de-109">More resources can be added as the load increases.</span></span> <span data-ttu-id="b00de-110">A jak vyžádání snižuje, minimalizovat náklady a Udržovat úrovně výkonu lze odebrat prostředky.</span><span class="sxs-lookup"><span data-stu-id="b00de-110">And as demand decreases, resources can be removed to minimize costs and maintain performance levels.</span></span>

<span data-ttu-id="b00de-111">Nastavte automatické škálování na škále nastavit pomocí šablonu Azure Resource Manager, Azure PowerShell, rozhraní příkazového řádku Azure nebo portálu Azure.</span><span class="sxs-lookup"><span data-stu-id="b00de-111">Set up automatic scaling on a scale set by using an Azure Resource Manager template, Azure PowerShell, Azure CLI, or the Azure portal.</span></span>

## <a name="set-up-scaling-by-using-resource-manager-templates"></a><span data-ttu-id="b00de-112">Nastavení škálování pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="b00de-112">Set up scaling by using Resource Manager templates</span></span>
<span data-ttu-id="b00de-113">Namísto nasazení a samostatně spravovat každý prostředek vaší aplikace, použijte šablonu, která nasadí všechny prostředky v rámci jediné koordinované operace.</span><span class="sxs-lookup"><span data-stu-id="b00de-113">Rather than deploying and managing each resource of your application separately, use a template that deploys all resources in a single, coordinated operation.</span></span> <span data-ttu-id="b00de-114">V šabloně jsou definovány prostředky aplikace a jsou zadány parametry nasazení pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="b00de-114">In the template, application resources are defined and deployment parameters are specified for different environments.</span></span> <span data-ttu-id="b00de-115">Šablona se skládá z JSON a výrazy, které můžete použít k vytvoření hodnot pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="b00de-115">The template consists of JSON and expressions that you can use to construct values for your deployment.</span></span> <span data-ttu-id="b00de-116">Další informace, podívejte se na [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b00de-116">To learn more, look at [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="b00de-117">V šabloně zadejte element kapacity:</span><span class="sxs-lookup"><span data-stu-id="b00de-117">In the template, you specify the capacity element:</span></span>

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 3
},
```

<span data-ttu-id="b00de-118">Kapacita identifikuje počet virtuálních počítačů v sadě.</span><span class="sxs-lookup"><span data-stu-id="b00de-118">Capacity identifies the number of virtual machines in the set.</span></span> <span data-ttu-id="b00de-119">Můžete ručně změnit kapacitu nasazení šablony s jinou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="b00de-119">You can manually change the capacity by deploying a template with a different value.</span></span> <span data-ttu-id="b00de-120">Pokud nasazujete šablonu, která má kapacitu, pouze změní, můžete zahrnout pouze element SKU s aktualizované kapacitu.</span><span class="sxs-lookup"><span data-stu-id="b00de-120">If you are deploying a template to only change the capacity, you can include only the SKU element with the updated capacity.</span></span>

<span data-ttu-id="b00de-121">Kapacitu škálovací sadu pomocí kombinace automaticky upravována **autoscaleSettings** prostředků a rozšíření diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="b00de-121">The capacity of your scale set can be automatically adjusted by using a combination of the **autoscaleSettings** resource and the diagnostics extension.</span></span>

### <a name="configure-the-azure-diagnostics-extension"></a><span data-ttu-id="b00de-122">Konfigurace rozšíření Azure Diagnostics</span><span class="sxs-lookup"><span data-stu-id="b00de-122">Configure the Azure Diagnostics extension</span></span>
<span data-ttu-id="b00de-123">Automatické škálování provést pouze pokud se kolekce metriky úspěšné na každém virtuálním počítači v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="b00de-123">Automatic scaling can only be done if metrics collection is successful on each virtual machine in the scale set.</span></span> <span data-ttu-id="b00de-124">Rozšíření diagnostiky Azure poskytuje možnosti monitorování a diagnostiky, které splňuje potřeby kolekce metriky automatického škálování prostředku.</span><span class="sxs-lookup"><span data-stu-id="b00de-124">The Azure Diagnostics Extension provides the monitoring and diagnostics capabilities that meets the metrics collection needs of the autoscale resource.</span></span> <span data-ttu-id="b00de-125">Rozšíření můžete nainstalovat v rámci šablony Resource Manageru.</span><span class="sxs-lookup"><span data-stu-id="b00de-125">You can install the extension as part of the Resource Manager template.</span></span>

<span data-ttu-id="b00de-126">Tento příklad ukazuje proměnné, které se používají v šabloně ke konfiguraci rozšíření diagnostiky:</span><span class="sxs-lookup"><span data-stu-id="b00de-126">This example shows the variables that are used in the template to configure the diagnostics extension:</span></span>

```json
"diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
"accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
"wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
"wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
"wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
"wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
"wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
```

<span data-ttu-id="b00de-127">Parametry jsou k dispozici při nasazení šablony.</span><span class="sxs-lookup"><span data-stu-id="b00de-127">Parameters are provided when the template is deployed.</span></span> <span data-ttu-id="b00de-128">V tomto příkladu jsou zadat název účtu úložiště (které jsou uložená data) a název sady škálování (kdy se shromažďují data).</span><span class="sxs-lookup"><span data-stu-id="b00de-128">In this example, the name of the storage account (where data is stored) and the name of the scale set (where data is collected) are provided.</span></span> <span data-ttu-id="b00de-129">Také v tomto příkladu systému Windows Server se shromažďují pouze počet vláken počítadla výkonu.</span><span class="sxs-lookup"><span data-stu-id="b00de-129">Also in this Windows Server example, only the Thread Count performance counter is collected.</span></span> <span data-ttu-id="b00de-130">Všechny čítače výkonu k dispozici v systému Windows nebo Linux umožňuje shromažďovat diagnostické informace a může být zahrnutý v konfiguraci rozšíření.</span><span class="sxs-lookup"><span data-stu-id="b00de-130">All the available performance counters in Windows or Linux can be used to collect diagnostics information and can be included in the extension configuration.</span></span>

<span data-ttu-id="b00de-131">Tento příklad ukazuje definici rozšíření v šabloně:</span><span class="sxs-lookup"><span data-stu-id="b00de-131">This example shows the definition of the extension in the template:</span></span>

```json
"extensionProfile": {
  "extensions": [
    {
      "name": "Microsoft.Insights.VMDiagnosticsSettings",
      "properties": {
        "publisher": "Microsoft.Azure.Diagnostics",
        "type": "IaaSDiagnostics",
        "typeHandlerVersion": "1.5",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "xmlCfg": "[base64(concat(variables('wadcfgxstart'),variables('wadmetricsresourceid'),variables('wadcfgxend')))]",
          "storageAccount": "[variables('diagnosticsStorageAccountName')]"
        },
        "protectedSettings": {
          "storageAccountName": "[variables('diagnosticsStorageAccountName')]",
          "storageAccountKey": "[listkeys(variables('accountid'), variables('apiVersion')).key1]",
          "storageAccountEndPoint": "https://core.windows.net"
        }
      }
    }
  ]
}
```

<span data-ttu-id="b00de-132">Při spuštění rozšíření diagnostiky, se data uložená a shromažďují v tabulce v účtu úložiště, který určíte.</span><span class="sxs-lookup"><span data-stu-id="b00de-132">When the diagnostics extension runs, the data is stored and collected in a table, in the storage account that you specify.</span></span> <span data-ttu-id="b00de-133">V **WADPerformanceCounters** tabulky, můžete najít na shromážděná data:</span><span class="sxs-lookup"><span data-stu-id="b00de-133">In the **WADPerformanceCounters** table, you can find the collected data:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-the-autoscalesettings-resource"></a><span data-ttu-id="b00de-134">Konfigurace prostředků autoScaleSettings</span><span class="sxs-lookup"><span data-stu-id="b00de-134">Configure the autoScaleSettings resource</span></span>
<span data-ttu-id="b00de-135">Prostředek autoscaleSettings používá informace z rozšíření diagnostiky se rozhodnout, jestli se má zvýšit nebo snížit počet virtuálních počítačů v sadě škálování.</span><span class="sxs-lookup"><span data-stu-id="b00de-135">The autoscaleSettings resource uses the information from the diagnostics extension to decide whether to increase or decrease the number of virtual machines in the scale set.</span></span>

<span data-ttu-id="b00de-136">Tento příklad ukazuje konfiguraci prostředku v šabloně:</span><span class="sxs-lookup"><span data-stu-id="b00de-136">This example shows the configuration of the resource in the template:</span></span>

```json
{
  "type": "Microsoft.Insights/autoscaleSettings",
  "apiVersion": "2015-04-01",
  "name": "[concat(parameters('resourcePrefix'),'as1')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachineScaleSets/',parameters('vmSSName'))]"
  ],
  "properties": {
    "enabled": true,
    "name": "[concat(parameters('resourcePrefix'),'as1')]",
    "profiles": [
      {
        "name": "Profile1",
        "capacity": {
          "minimum": "1",
          "maximum": "10",
          "default": "1"
        },
        "rules": [
          {
            "metricTrigger": {
              "metricName": "\\Processor(_Total)\\Thread Count",
              "metricNamespace": "",
              "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT5M",
              "timeAggregation": "Average",
              "operator": "GreaterThan",
              "threshold": 650
            },
            "scaleAction": {
              "direction": "Increase",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          },
          {
            "metricTrigger": {
              "metricName": "\\Processor(_Total)\\Thread Count",
              "metricNamespace": "",
              "metricResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
              "timeGrain": "PT1M",
              "statistic": "Average",
              "timeWindow": "PT5M",
              "timeAggregation": "Average",
              "operator": "LessThan",
              "threshold": 550
            },
            "scaleAction": {
              "direction": "Decrease",
              "type": "ChangeCount",
              "value": "1",
              "cooldown": "PT5M"
            }
          }
        ]
      }
    ],
    "targetResourceUri": "[concat('/subscriptions/', subscription().subscriptionId, '/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
  }
}
```

<span data-ttu-id="b00de-137">V příkladu nahoře vytvoří se dvě pravidla pro definování automatické škálování akce.</span><span class="sxs-lookup"><span data-stu-id="b00de-137">In the example above, two rules are created for defining the automatic scaling actions.</span></span> <span data-ttu-id="b00de-138">První pravidlo definuje akce škálování a druhé pravidlo definuje akce škálování v.</span><span class="sxs-lookup"><span data-stu-id="b00de-138">The first rule defines the scale-out action and the second rule defines the scale-in action.</span></span> <span data-ttu-id="b00de-139">Tyto hodnoty jsou k dispozici v pravidlech:</span><span class="sxs-lookup"><span data-stu-id="b00de-139">These values are provided in the rules:</span></span>

| <span data-ttu-id="b00de-140">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="b00de-140">Rule</span></span> | <span data-ttu-id="b00de-141">Popis</span><span class="sxs-lookup"><span data-stu-id="b00de-141">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="b00de-142">metricName</span><span class="sxs-lookup"><span data-stu-id="b00de-142">metricName</span></span>        | <span data-ttu-id="b00de-143">Tato hodnota je stejná jako čítače výkonu, který jste definovali v proměnné wadperfcounter pro rozšíření diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="b00de-143">This value is the same as the performance counter that you defined in the wadperfcounter variable for the diagnostics extension.</span></span> <span data-ttu-id="b00de-144">V předchozím příkladu se používá čítač počtu vláken.</span><span class="sxs-lookup"><span data-stu-id="b00de-144">In the example above, the Thread Count counter is used.</span></span>    |
| <span data-ttu-id="b00de-145">metricResourceUri</span><span class="sxs-lookup"><span data-stu-id="b00de-145">metricResourceUri</span></span> | <span data-ttu-id="b00de-146">Tato hodnota je identifikátor prostředku sady škálování virtuálního počítače.</span><span class="sxs-lookup"><span data-stu-id="b00de-146">This value is the resource identifier of the virtual machine scale set.</span></span> <span data-ttu-id="b00de-147">Tento identifikátor obsahuje název skupiny prostředků, název zprostředkovatele prostředků a názvu sad škálování škálování.</span><span class="sxs-lookup"><span data-stu-id="b00de-147">This identifier contains the name of the resource group, the name of the resource provider, and the name of the scale set to scale.</span></span> |
| <span data-ttu-id="b00de-148">Časovými úseky</span><span class="sxs-lookup"><span data-stu-id="b00de-148">timeGrain</span></span>         | <span data-ttu-id="b00de-149">Tato hodnota je členitost metriky, které se shromažďují.</span><span class="sxs-lookup"><span data-stu-id="b00de-149">This value is the granularity of the metrics that are collected.</span></span> <span data-ttu-id="b00de-150">V předchozím příkladu data jsou shromažďována v intervalu jedné minuty.</span><span class="sxs-lookup"><span data-stu-id="b00de-150">In the preceding example, data is collected on an interval of one minute.</span></span> <span data-ttu-id="b00de-151">Tato hodnota se používá s hodnota timeWindow.</span><span class="sxs-lookup"><span data-stu-id="b00de-151">This value is used with timeWindow.</span></span> |
| <span data-ttu-id="b00de-152">statistiky</span><span class="sxs-lookup"><span data-stu-id="b00de-152">statistic</span></span>         | <span data-ttu-id="b00de-153">Tato hodnota určuje, jak se zkombinují metriky pro přizpůsobení automatické škálování akci.</span><span class="sxs-lookup"><span data-stu-id="b00de-153">This value determines how the metrics are combined to accommodate the automatic scaling action.</span></span> <span data-ttu-id="b00de-154">Možné hodnoty jsou: průměr, minimum, maximum.</span><span class="sxs-lookup"><span data-stu-id="b00de-154">The possible values are: Average, Min, Max.</span></span> |
| <span data-ttu-id="b00de-155">Hodnota timeWindow</span><span class="sxs-lookup"><span data-stu-id="b00de-155">timeWindow</span></span>        | <span data-ttu-id="b00de-156">Tato hodnota je doba, ve kterém se shromažďují instance data.</span><span class="sxs-lookup"><span data-stu-id="b00de-156">This value is the range of time in which instance data is collected.</span></span> <span data-ttu-id="b00de-157">Musí být mezi 5 minutami a 12 hodin.</span><span class="sxs-lookup"><span data-stu-id="b00de-157">It must be between 5 minutes and 12 hours.</span></span> |
| <span data-ttu-id="b00de-158">Agregace času</span><span class="sxs-lookup"><span data-stu-id="b00de-158">timeAggregation</span></span>   | <span data-ttu-id="b00de-159">Tato hodnota určuje, jak by měla být kombinovány data, která se shromažďují v čase.</span><span class="sxs-lookup"><span data-stu-id="b00de-159">This value determines how the data that is collected should be combined over time.</span></span> <span data-ttu-id="b00de-160">Výchozí hodnota je průměr.</span><span class="sxs-lookup"><span data-stu-id="b00de-160">The default value is Average.</span></span> <span data-ttu-id="b00de-161">Možné hodnoty jsou: průměr, minimální, maximální, poslední, celkový počet, počet.</span><span class="sxs-lookup"><span data-stu-id="b00de-161">The possible values are: Average, Minimum, Maximum, Last, Total, Count.</span></span> |
| <span data-ttu-id="b00de-162">Operátor</span><span class="sxs-lookup"><span data-stu-id="b00de-162">operator</span></span>          | <span data-ttu-id="b00de-163">Tato hodnota je operátor, který se používá k porovnání data metriky a prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="b00de-163">This value is the operator that is used to compare the metric data and the threshold.</span></span> <span data-ttu-id="b00de-164">Možné hodnoty jsou: rovná NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span><span class="sxs-lookup"><span data-stu-id="b00de-164">The possible values are: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span></span> |
| <span data-ttu-id="b00de-165">Prahová hodnota</span><span class="sxs-lookup"><span data-stu-id="b00de-165">threshold</span></span>         | <span data-ttu-id="b00de-166">Tato hodnota je hodnota, která aktivuje akci škálování.</span><span class="sxs-lookup"><span data-stu-id="b00de-166">This value is the value that triggers the scale action.</span></span> <span data-ttu-id="b00de-167">Nezapomeňte poskytnout dostatečnou rozdíl mezi prahové hodnoty pro **Škálováním na více systémů** a **škálování v** akce.</span><span class="sxs-lookup"><span data-stu-id="b00de-167">Be sure to provide a sufficient difference between the threshold values for the **scale-out** and **scale-in** actions.</span></span> <span data-ttu-id="b00de-168">Pokud jste nastavili stejné hodnoty pro obě akce, připravená na systém konstantní změna, která zabraňuje v jeho implementaci akce škálování.</span><span class="sxs-lookup"><span data-stu-id="b00de-168">If you set the same values for both actions, the system anticipates constant change, which prevents it from implementing a scaling action.</span></span> <span data-ttu-id="b00de-169">Například nebude fungovat nastavení na 600 vláken v předchozím příkladu.</span><span class="sxs-lookup"><span data-stu-id="b00de-169">For example, setting both to 600 threads in the preceding example doesn't work.</span></span> |
| <span data-ttu-id="b00de-170">Směr</span><span class="sxs-lookup"><span data-stu-id="b00de-170">direction</span></span>         | <span data-ttu-id="b00de-171">Tato hodnota určuje akce, která se provede, když je dosaženo prahové hodnoty.</span><span class="sxs-lookup"><span data-stu-id="b00de-171">This value determines the action that is taken when the threshold value is achieved.</span></span> <span data-ttu-id="b00de-172">Možné hodnoty jsou zvýšení nebo snížení.</span><span class="sxs-lookup"><span data-stu-id="b00de-172">The possible values are Increase or Decrease.</span></span> |
| <span data-ttu-id="b00de-173">type</span><span class="sxs-lookup"><span data-stu-id="b00de-173">type</span></span>              | <span data-ttu-id="b00de-174">Tato hodnota je typ akce, který má vzniknout a musí být nastavena na ChangeCount.</span><span class="sxs-lookup"><span data-stu-id="b00de-174">This value is the type of action that should occur and must be set to ChangeCount.</span></span> |
| <span data-ttu-id="b00de-175">hodnota</span><span class="sxs-lookup"><span data-stu-id="b00de-175">value</span></span>             | <span data-ttu-id="b00de-176">Tato hodnota je počet virtuálních počítačů, které jsou přidat nebo odebrat ze sady škálování.</span><span class="sxs-lookup"><span data-stu-id="b00de-176">This value is the number of virtual machines that are added to or removed from the scale set.</span></span> <span data-ttu-id="b00de-177">Tato hodnota musí být 1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="b00de-177">This value must be 1 or greater.</span></span> |
| <span data-ttu-id="b00de-178">cooldown</span><span class="sxs-lookup"><span data-stu-id="b00de-178">cooldown</span></span>          | <span data-ttu-id="b00de-179">Tato hodnota je množství času má počkat od poslední akce škálování, než dojde k další akce.</span><span class="sxs-lookup"><span data-stu-id="b00de-179">This value is the amount of time to wait since the last scaling action before the next action occurs.</span></span> <span data-ttu-id="b00de-180">Tato hodnota musí být mezi minutu a jeden týden.</span><span class="sxs-lookup"><span data-stu-id="b00de-180">This value must be between one minute and one week.</span></span> |

<span data-ttu-id="b00de-181">V závislosti na čítače výkonu který používáte, některé prvky v konfiguraci šablony fungují jinak.</span><span class="sxs-lookup"><span data-stu-id="b00de-181">Depending on the performance counter, you are using, some of the elements in the template configuration are used differently.</span></span> <span data-ttu-id="b00de-182">V předchozím příkladu je hodnota čítače výkonu počtu vláken, prahová hodnota je 650 pro akci škálování a prahová hodnota je 550 pro akci škálování in.</span><span class="sxs-lookup"><span data-stu-id="b00de-182">In the preceding example, the performance counter is Thread Count, the threshold value is 650 for a scale-out action, and the threshold value is 550 for the scale-in action.</span></span> <span data-ttu-id="b00de-183">Pokud používáte čítače, jako je % času procesoru, prahová hodnota nastavená na procento využití procesoru, který určuje škálování akce.</span><span class="sxs-lookup"><span data-stu-id="b00de-183">If you use a counter such as %Processor Time, the threshold value is set to the percentage of CPU usage that determines a scaling action.</span></span>

<span data-ttu-id="b00de-184">Když se aktivuje škálování akce, jako je například vysoké zatížení, je vyšší kapacita sady závislosti na hodnotě v šabloně.</span><span class="sxs-lookup"><span data-stu-id="b00de-184">When a scaling action is triggered, such as a high load, the capacity of the set is increased based on the value in the template.</span></span> <span data-ttu-id="b00de-185">Například v škálování sady, kde je kapacitu nastaven na 3 a hodnotu akce škálování je nastaven na 1:</span><span class="sxs-lookup"><span data-stu-id="b00de-185">For example, in a scale set where the capacity is set to 3 and the scale action value is set to 1:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

<span data-ttu-id="b00de-186">Pokud aktuální zatížení způsobí, že počet průměrná vláken přejdete nad prahovou hodnotou 650:</span><span class="sxs-lookup"><span data-stu-id="b00de-186">When the current load causes the average thread count to go above the threshold of 650:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

<span data-ttu-id="b00de-187">A **Škálováním na více systémů** akce se aktivuje, který způsobuje, že kapacita sady do dá zvětšit o jeden:</span><span class="sxs-lookup"><span data-stu-id="b00de-187">A **scale-out** action is triggered that causes the capacity of the set to be increased by one:</span></span>

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 4
},
```

<span data-ttu-id="b00de-188">Výsledkem je, že virtuální počítač je přidat do sady škálování:</span><span class="sxs-lookup"><span data-stu-id="b00de-188">The result is a virtual machine is added to the scale set:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

<span data-ttu-id="b00de-189">Po určité době cooldown pět minut je-li průměrný počet vláken na počítačích více než 600 je jiný počítač přidat do sady.</span><span class="sxs-lookup"><span data-stu-id="b00de-189">After a cooldown period of five minutes, if the average number of threads on the machines stays over 600, another machine is added to the set.</span></span> <span data-ttu-id="b00de-190">Je-li počet průměrná vláken níže 550, kapacita sady škálování se sníží o jedna a počítač je odebrán ze sady.</span><span class="sxs-lookup"><span data-stu-id="b00de-190">If the average thread count stays below 550, the capacity of the scale set is reduced by one and a machine is removed from the set.</span></span>

## <a name="set-up-scaling-using-azure-powershell"></a><span data-ttu-id="b00de-191">Nastavení škálování pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="b00de-191">Set up scaling using Azure PowerShell</span></span>

<span data-ttu-id="b00de-192">Chcete-li zobrazit příklady použití prostředí PowerShell pro nastavení automatického škálování, podívejte se na [Azure PowerShell monitorování rychlý start ukázky](../monitoring-and-diagnostics/insights-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b00de-192">To see examples of using PowerShell to set up autoscaling, look at [Azure Monitor PowerShell quick start samples](../monitoring-and-diagnostics/insights-powershell-samples.md).</span></span>

## <a name="set-up-scaling-using-azure-cli"></a><span data-ttu-id="b00de-193">Nastavení škálování pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="b00de-193">Set up scaling using Azure CLI</span></span>

<span data-ttu-id="b00de-194">Pokud chcete zobrazit příklady použití Azure CLI pro nastavení automatického škálování, podívejte se na [Azure monitorování napříč platformami CLI rychlý start ukázky](../monitoring-and-diagnostics/insights-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="b00de-194">To see examples of using Azure CLI to set up autoscaling, look at [Azure Monitor Cross-platform CLI quick start samples](../monitoring-and-diagnostics/insights-cli-samples.md).</span></span>

## <a name="set-up-scaling-using-the-azure-portal"></a><span data-ttu-id="b00de-195">Nastavení škálování pomocí portálu Azure</span><span class="sxs-lookup"><span data-stu-id="b00de-195">Set up scaling using the Azure portal</span></span>

<span data-ttu-id="b00de-196">Pokud chcete zobrazit příklad nastavení automatického škálování pomocí portálu Azure, podívejte se na [vytvoření sady škálování virtuálního počítače pomocí portálu Azure](virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="b00de-196">To see an example of using the Azure portal to set up autoscaling, look at [Create a Virtual Machine Scale Set using the Azure portal](virtual-machine-scale-sets-portal-create.md).</span></span>

## <a name="investigate-scaling-actions"></a><span data-ttu-id="b00de-197">Prozkoumat škálování akce</span><span class="sxs-lookup"><span data-stu-id="b00de-197">Investigate scaling actions</span></span>

* <span data-ttu-id="b00de-198">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="b00de-198">**Azure portal**</span></span>  
<span data-ttu-id="b00de-199">Teď můžete získat omezené množství informací pomocí portálu.</span><span class="sxs-lookup"><span data-stu-id="b00de-199">You can currently get a limited amount of information using the portal.</span></span>

* <span data-ttu-id="b00de-200">**Průzkumník prostředků Azure**</span><span class="sxs-lookup"><span data-stu-id="b00de-200">**Azure Resource Explorer**</span></span>  
<span data-ttu-id="b00de-201">Tento nástroj je nejvhodnější pro zkoumání aktuálního stavu škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="b00de-201">This tool is the best for exploring the current state of your scale set.</span></span> <span data-ttu-id="b00de-202">Postupujte podle této cestě a měli byste vidět zobrazení instance škálovací sady, který jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="b00de-202">Follow this path and you should see the instance view of the scale set that you created:</span></span>  
<span data-ttu-id="b00de-203">**Odběry > {vaše předplatné} > Skupinyprostředků > {vaší skupiny prostředků} > poskytovatelé > Microsoft.Compute > virtualMachineScaleSets > {škálovací sadu} > virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="b00de-203">**Subscriptions > {your subscription} > resourceGroups > {your resource group} > providers > Microsoft.Compute > virtualMachineScaleSets > {your scale set} > virtualMachines**</span></span>

* <span data-ttu-id="b00de-204">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="b00de-204">**Azure PowerShell**</span></span>  
<span data-ttu-id="b00de-205">Určité informace, použijte tento příkaz:</span><span class="sxs-lookup"><span data-stu-id="b00de-205">Use this command to get some information:</span></span>

  ```powershell
  Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
  Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput
  ```

* <span data-ttu-id="b00de-206">Připojte k virtuálnímu počítači jumpbox stejně, jako by všechny ostatní počítače a potom můžete virtuální počítače v sad pro monitorování jednotlivých procesů škálování vzdálený přístup.</span><span class="sxs-lookup"><span data-stu-id="b00de-206">Connect to the jumpbox virtual machine just like you would any other machine and then you can remotely access the virtual machines in the scale set to monitor individual processes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b00de-207">Další kroky</span><span class="sxs-lookup"><span data-stu-id="b00de-207">Next Steps</span></span>
* <span data-ttu-id="b00de-208">Podívejte se na [automaticky škálovat počítače ve Škálovací sadě virtuálního počítače](virtual-machine-scale-sets-windows-autoscale.md) příklad toho, jak vytvořit měřítko nastavit automatické škálování nakonfigurované.</span><span class="sxs-lookup"><span data-stu-id="b00de-208">Look at [Automatically scale machines in a Virtual Machine Scale Set](virtual-machine-scale-sets-windows-autoscale.md) to see an example of how to create a scale set with automatic scaling configured.</span></span>

* <span data-ttu-id="b00de-209">Najít příklady Azure monitorování monitorování funkcí v [Azure PowerShell monitorování rychlý start ukázky](../monitoring-and-diagnostics/insights-powershell-samples.md)</span><span class="sxs-lookup"><span data-stu-id="b00de-209">Find examples of Azure Monitor monitoring features in [Azure Monitor PowerShell quick start samples](../monitoring-and-diagnostics/insights-powershell-samples.md)</span></span>

* <span data-ttu-id="b00de-210">Další informace o funkcích oznámení v [používat akce škálování k odesílání e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).</span><span class="sxs-lookup"><span data-stu-id="b00de-210">Learn about notification features in [Use autoscale actions to send email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).</span></span>

* <span data-ttu-id="b00de-211">Další informace o tom, jak [protokoly auditu použijte k odesílání e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="b00de-211">Learn about how to [Use audit logs to send email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span></span>

* <span data-ttu-id="b00de-212">Další informace o [pokročilé scénáře škálování](virtual-machine-scale-sets-advanced-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="b00de-212">Learn about [advanced autoscale scenarios](virtual-machine-scale-sets-advanced-autoscale.md).</span></span>
