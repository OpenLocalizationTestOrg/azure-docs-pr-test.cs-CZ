---
title: "škálování aaaAutomatic a virtuálního počítače sady škálování | Microsoft Docs"
description: "Další informace o použití diagnostiky a škálování prostředky tooautomatically škálování virtuálních počítačů v sadě škálování."
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
ms.openlocfilehash: 25f54b693e3c991577238242008c262023ed479c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-automatic-scaling-and-virtual-machine-scale-sets"></a><span data-ttu-id="a016e-103">Jak toouse automatické škálování a sady škálování virtuálního počítače</span><span class="sxs-lookup"><span data-stu-id="a016e-103">How toouse automatic scaling and Virtual Machine Scale Sets</span></span>
<span data-ttu-id="a016e-104">Automatické škálování virtuálních počítačů v sadě škálování je hello vytvoření nebo odstranění počítačů v hello nastavit jako potřeby toomatch požadavky na výkon.</span><span class="sxs-lookup"><span data-stu-id="a016e-104">Automatic scaling of virtual machines in a scale set is hello creation or deletion of machines in hello set as needed toomatch performance requirements.</span></span> <span data-ttu-id="a016e-105">S růstem hello objem práce aplikace může vyžadovat další prostředky tooenable ho tooeffectively provádět úlohy.</span><span class="sxs-lookup"><span data-stu-id="a016e-105">As hello volume of work grows, an application may require additional resources tooenable it tooeffectively perform tasks.</span></span>

<span data-ttu-id="a016e-106">Automatické škálování je automatizovaný proces, pomáhá usnadňují režie na správu.</span><span class="sxs-lookup"><span data-stu-id="a016e-106">Automatic scaling is an automated process that helps ease management overhead.</span></span> <span data-ttu-id="a016e-107">Protože se sníží režie, není nutné toocontinually sledování výkonu systému nebo rozhodnout, jak toomanage prostředky.</span><span class="sxs-lookup"><span data-stu-id="a016e-107">By reducing overhead, you don't need toocontinually monitor system performance or decide how toomanage resources.</span></span> <span data-ttu-id="a016e-108">Škálování je elastické proces.</span><span class="sxs-lookup"><span data-stu-id="a016e-108">Scaling is an elastic process.</span></span> <span data-ttu-id="a016e-109">Další prostředky se dá přidat jako zvyšuje zatížení hello.</span><span class="sxs-lookup"><span data-stu-id="a016e-109">More resources can be added as hello load increases.</span></span> <span data-ttu-id="a016e-110">A jako vyžádání snížení prostředky můžete odebrané toominimize náklady a zachovat úrovně výkonu.</span><span class="sxs-lookup"><span data-stu-id="a016e-110">And as demand decreases, resources can be removed toominimize costs and maintain performance levels.</span></span>

<span data-ttu-id="a016e-111">Nastavte automatické škálování na škále nastavit pomocí šablony, prostředí Azure PowerShell, rozhraní příkazového řádku Azure nebo hello portál Azure Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="a016e-111">Set up automatic scaling on a scale set by using an Azure Resource Manager template, Azure PowerShell, Azure CLI, or hello Azure portal.</span></span>

## <a name="set-up-scaling-by-using-resource-manager-templates"></a><span data-ttu-id="a016e-112">Nastavení škálování pomocí šablony Resource Manageru</span><span class="sxs-lookup"><span data-stu-id="a016e-112">Set up scaling by using Resource Manager templates</span></span>
<span data-ttu-id="a016e-113">Namísto nasazení a samostatně spravovat každý prostředek vaší aplikace, použijte šablonu, která nasadí všechny prostředky v rámci jediné koordinované operace.</span><span class="sxs-lookup"><span data-stu-id="a016e-113">Rather than deploying and managing each resource of your application separately, use a template that deploys all resources in a single, coordinated operation.</span></span> <span data-ttu-id="a016e-114">V šabloně hello prostředky aplikace jsou definované a jsou zadány parametry nasazení pro různá prostředí.</span><span class="sxs-lookup"><span data-stu-id="a016e-114">In hello template, application resources are defined and deployment parameters are specified for different environments.</span></span> <span data-ttu-id="a016e-115">Šablona Hello se skládá z JSON a výrazy, můžete použít hodnoty tooconstruct pro vaše nasazení.</span><span class="sxs-lookup"><span data-stu-id="a016e-115">hello template consists of JSON and expressions that you can use tooconstruct values for your deployment.</span></span> <span data-ttu-id="a016e-116">toolearn víc, podívejte se na [šablon pro tvorbu Azure Resource Manageru](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="a016e-116">toolearn more, look at [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="a016e-117">V šabloně hello zadejte element kapacity hello:</span><span class="sxs-lookup"><span data-stu-id="a016e-117">In hello template, you specify hello capacity element:</span></span>

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 3
},
```

<span data-ttu-id="a016e-118">Kapacita identifikuje hello počet virtuálních počítačů v sadě hello.</span><span class="sxs-lookup"><span data-stu-id="a016e-118">Capacity identifies hello number of virtual machines in hello set.</span></span> <span data-ttu-id="a016e-119">Můžete ručně změnit hello kapacity nasazení šablony s jinou hodnotou.</span><span class="sxs-lookup"><span data-stu-id="a016e-119">You can manually change hello capacity by deploying a template with a different value.</span></span> <span data-ttu-id="a016e-120">Pokud nasazujete kapacitou hello tooonly změny šablony, můžete zahrnout pouze element SKU hello s kapacitou hello aktualizovat.</span><span class="sxs-lookup"><span data-stu-id="a016e-120">If you are deploying a template tooonly change hello capacity, you can include only hello SKU element with hello updated capacity.</span></span>

<span data-ttu-id="a016e-121">Hello kapacity škálovací sady můžete být automaticky upravována pomocí kombinace hello **autoscaleSettings** prostředků a hello rozšíření diagnostiky.</span><span class="sxs-lookup"><span data-stu-id="a016e-121">hello capacity of your scale set can be automatically adjusted by using a combination of hello **autoscaleSettings** resource and hello diagnostics extension.</span></span>

### <a name="configure-hello-azure-diagnostics-extension"></a><span data-ttu-id="a016e-122">Konfigurace rozšíření Azure Diagnostics hello</span><span class="sxs-lookup"><span data-stu-id="a016e-122">Configure hello Azure Diagnostics extension</span></span>
<span data-ttu-id="a016e-123">Automatické škálování provést pouze pokud se kolekce metriky úspěšné na každém virtuálním počítači v sadě škálování hello.</span><span class="sxs-lookup"><span data-stu-id="a016e-123">Automatic scaling can only be done if metrics collection is successful on each virtual machine in hello scale set.</span></span> <span data-ttu-id="a016e-124">Hello rozšíření diagnostiky Azure poskytuje možnosti pro monitorování a Diagnostika hello vyhovujících potřebám kolekce metriky hello hello škálování prostředku.</span><span class="sxs-lookup"><span data-stu-id="a016e-124">hello Azure Diagnostics Extension provides hello monitoring and diagnostics capabilities that meets hello metrics collection needs of hello autoscale resource.</span></span> <span data-ttu-id="a016e-125">Rozšíření hello můžete nainstalovat v rámci šablony Resource Manageru hello.</span><span class="sxs-lookup"><span data-stu-id="a016e-125">You can install hello extension as part of hello Resource Manager template.</span></span>

<span data-ttu-id="a016e-126">Tento příklad ukazuje hello proměnné, které se používají v rozšíření diagnostiky hello šablony tooconfigure hello:</span><span class="sxs-lookup"><span data-stu-id="a016e-126">This example shows hello variables that are used in hello template tooconfigure hello diagnostics extension:</span></span>

```json
"diagnosticsStorageAccountName": "[concat(parameters('resourcePrefix'), 'saa')]",
"accountid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/', resourceGroup().name,'/providers/', 'Microsoft.Storage/storageAccounts/', variables('diagnosticsStorageAccountName'))]",
"wadlogs": "<WadCfg> <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" /></WindowsEventLog>",
"wadperfcounter": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\"><PerformanceCounterConfiguration counterSpecifier=\"\\Processor(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Percent\"><annotation displayName=\"Thread Count\" locale=\"en-us\"/></PerformanceCounterConfiguration></PerformanceCounters>",
"wadcfgxstart": "[concat(variables('wadlogs'),variables('wadperfcounter'),'<Metrics resourceId=\"')]",
"wadmetricsresourceid": "[concat('/subscriptions/',subscription().subscriptionId,'/resourceGroups/',resourceGroup().name ,'/providers/','Microsoft.Compute/virtualMachineScaleSets/',parameters('vmssName'))]",
"wadcfgxend": "[concat('\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/><MetricAggregation scheduledTransferPeriod=\"PT1M\"/></Metrics></DiagnosticMonitorConfiguration></WadCfg>')]"
```

<span data-ttu-id="a016e-127">Parametry jsou k dispozici při nasazení šablony hello.</span><span class="sxs-lookup"><span data-stu-id="a016e-127">Parameters are provided when hello template is deployed.</span></span> <span data-ttu-id="a016e-128">V tomto příkladu, hello název účtu úložiště hello (které jsou uložená data) a hello je zadaný název sady škálování hello (kdy se shromažďují data).</span><span class="sxs-lookup"><span data-stu-id="a016e-128">In this example, hello name of hello storage account (where data is stored) and hello name of hello scale set (where data is collected) are provided.</span></span> <span data-ttu-id="a016e-129">Také v tomto příkladu systému Windows Server se shromažďují pouze hello čítače výkonu počtu vláken.</span><span class="sxs-lookup"><span data-stu-id="a016e-129">Also in this Windows Server example, only hello Thread Count performance counter is collected.</span></span> <span data-ttu-id="a016e-130">Všechny hello čítače výkonu k dispozici v systému Windows nebo Linux lze použít toocollect diagnostické informace a můžou být součástí konfigurace rozšíření hello.</span><span class="sxs-lookup"><span data-stu-id="a016e-130">All hello available performance counters in Windows or Linux can be used toocollect diagnostics information and can be included in hello extension configuration.</span></span>

<span data-ttu-id="a016e-131">Tento příklad ukazuje definici rozšíření hello hello v šabloně hello:</span><span class="sxs-lookup"><span data-stu-id="a016e-131">This example shows hello definition of hello extension in hello template:</span></span>

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

<span data-ttu-id="a016e-132">Po spuštění rozšíření diagnostiky hello hello data ukládají a shromažďují v tabulce v hello účet úložiště, který určíte.</span><span class="sxs-lookup"><span data-stu-id="a016e-132">When hello diagnostics extension runs, hello data is stored and collected in a table, in hello storage account that you specify.</span></span> <span data-ttu-id="a016e-133">V hello **WADPerformanceCounters** tabulky, můžete najít hello shromažďovaných dat:</span><span class="sxs-lookup"><span data-stu-id="a016e-133">In hello **WADPerformanceCounters** table, you can find hello collected data:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountBefore2.png)

### <a name="configure-hello-autoscalesettings-resource"></a><span data-ttu-id="a016e-134">Konfigurace prostředků autoScaleSettings hello</span><span class="sxs-lookup"><span data-stu-id="a016e-134">Configure hello autoScaleSettings resource</span></span>
<span data-ttu-id="a016e-135">prostředek autoscaleSettings Hello používá hello informace z toodecide rozšíření diagnostiky hello zda tooincrease nebo snížení hello počet virtuálních počítačů v hello škálování nastavit.</span><span class="sxs-lookup"><span data-stu-id="a016e-135">hello autoscaleSettings resource uses hello information from hello diagnostics extension toodecide whether tooincrease or decrease hello number of virtual machines in hello scale set.</span></span>

<span data-ttu-id="a016e-136">Tento příklad ukazuje hello konfigurace hello prostředku v šabloně hello:</span><span class="sxs-lookup"><span data-stu-id="a016e-136">This example shows hello configuration of hello resource in hello template:</span></span>

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

<span data-ttu-id="a016e-137">V předchozím příkladu hello vytvoří se dvě pravidla pro definování hello automatické škálování akce.</span><span class="sxs-lookup"><span data-stu-id="a016e-137">In hello example above, two rules are created for defining hello automatic scaling actions.</span></span> <span data-ttu-id="a016e-138">první pravidlo Hello definuje akce škálování hello a druhé pravidlo hello definuje hello škálování v akci.</span><span class="sxs-lookup"><span data-stu-id="a016e-138">hello first rule defines hello scale-out action and hello second rule defines hello scale-in action.</span></span> <span data-ttu-id="a016e-139">Tyto hodnoty jsou k dispozici v pravidlech hello:</span><span class="sxs-lookup"><span data-stu-id="a016e-139">These values are provided in hello rules:</span></span>

| <span data-ttu-id="a016e-140">Pravidlo</span><span class="sxs-lookup"><span data-stu-id="a016e-140">Rule</span></span> | <span data-ttu-id="a016e-141">Popis</span><span class="sxs-lookup"><span data-stu-id="a016e-141">Description</span></span> |
| ---- | ----------- |
| <span data-ttu-id="a016e-142">metricName</span><span class="sxs-lookup"><span data-stu-id="a016e-142">metricName</span></span>        | <span data-ttu-id="a016e-143">Tato hodnota je hello stejné jako čítače výkonu hello jste definovali v hello wadperfcounter proměnnou pro rozšíření diagnostiky hello.</span><span class="sxs-lookup"><span data-stu-id="a016e-143">This value is hello same as hello performance counter that you defined in hello wadperfcounter variable for hello diagnostics extension.</span></span> <span data-ttu-id="a016e-144">V předchozím příkladu hello se používá čítač počtu vláken hello.</span><span class="sxs-lookup"><span data-stu-id="a016e-144">In hello example above, hello Thread Count counter is used.</span></span>    |
| <span data-ttu-id="a016e-145">metricResourceUri</span><span class="sxs-lookup"><span data-stu-id="a016e-145">metricResourceUri</span></span> | <span data-ttu-id="a016e-146">Tato hodnota je identifikátor prostředku hello škálovací sadu virtuálních počítačů hello.</span><span class="sxs-lookup"><span data-stu-id="a016e-146">This value is hello resource identifier of hello virtual machine scale set.</span></span> <span data-ttu-id="a016e-147">Tento identifikátor obsahuje hello název skupiny prostředků hello hello název zprostředkovatele prostředků hello a hello název tooscale sada škálování hello.</span><span class="sxs-lookup"><span data-stu-id="a016e-147">This identifier contains hello name of hello resource group, hello name of hello resource provider, and hello name of hello scale set tooscale.</span></span> |
| <span data-ttu-id="a016e-148">Časovými úseky</span><span class="sxs-lookup"><span data-stu-id="a016e-148">timeGrain</span></span>         | <span data-ttu-id="a016e-149">Tato hodnota je hello členitost hello metriky, které jsou shromážděny.</span><span class="sxs-lookup"><span data-stu-id="a016e-149">This value is hello granularity of hello metrics that are collected.</span></span> <span data-ttu-id="a016e-150">V předchozím příkladu hello data jsou shromažďována v intervalu jedné minuty.</span><span class="sxs-lookup"><span data-stu-id="a016e-150">In hello preceding example, data is collected on an interval of one minute.</span></span> <span data-ttu-id="a016e-151">Tato hodnota se používá s hodnota timeWindow.</span><span class="sxs-lookup"><span data-stu-id="a016e-151">This value is used with timeWindow.</span></span> |
| <span data-ttu-id="a016e-152">statistiky</span><span class="sxs-lookup"><span data-stu-id="a016e-152">statistic</span></span>         | <span data-ttu-id="a016e-153">Tato hodnota určuje, jak hello metriky jsou kombinované tooaccommodate hello automatického škálování akce.</span><span class="sxs-lookup"><span data-stu-id="a016e-153">This value determines how hello metrics are combined tooaccommodate hello automatic scaling action.</span></span> <span data-ttu-id="a016e-154">Hello možné hodnoty jsou: průměr, minimum, maximum.</span><span class="sxs-lookup"><span data-stu-id="a016e-154">hello possible values are: Average, Min, Max.</span></span> |
| <span data-ttu-id="a016e-155">Hodnota timeWindow</span><span class="sxs-lookup"><span data-stu-id="a016e-155">timeWindow</span></span>        | <span data-ttu-id="a016e-156">Tato hodnota je rozsah hello času, ve kterém se shromažďují instance data.</span><span class="sxs-lookup"><span data-stu-id="a016e-156">This value is hello range of time in which instance data is collected.</span></span> <span data-ttu-id="a016e-157">Musí být mezi 5 minutami a 12 hodin.</span><span class="sxs-lookup"><span data-stu-id="a016e-157">It must be between 5 minutes and 12 hours.</span></span> |
| <span data-ttu-id="a016e-158">Agregace času</span><span class="sxs-lookup"><span data-stu-id="a016e-158">timeAggregation</span></span>   | <span data-ttu-id="a016e-159">Tato hodnota určuje, jak by měla být kombinovány hello data, která se shromažďují v čase.</span><span class="sxs-lookup"><span data-stu-id="a016e-159">This value determines how hello data that is collected should be combined over time.</span></span> <span data-ttu-id="a016e-160">Hello výchozí hodnota je průměr.</span><span class="sxs-lookup"><span data-stu-id="a016e-160">hello default value is Average.</span></span> <span data-ttu-id="a016e-161">Hello možné hodnoty jsou: průměr, minimální, maximální, poslední, celkový počet, počet.</span><span class="sxs-lookup"><span data-stu-id="a016e-161">hello possible values are: Average, Minimum, Maximum, Last, Total, Count.</span></span> |
| <span data-ttu-id="a016e-162">Operátor</span><span class="sxs-lookup"><span data-stu-id="a016e-162">operator</span></span>          | <span data-ttu-id="a016e-163">Tato hodnota je hello operátor, který je použité toocompare hello metriky dat a hello prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a016e-163">This value is hello operator that is used toocompare hello metric data and hello threshold.</span></span> <span data-ttu-id="a016e-164">Hello možné hodnoty jsou: rovná NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span><span class="sxs-lookup"><span data-stu-id="a016e-164">hello possible values are: Equals, NotEquals, GreaterThan, GreaterThanOrEqual, LessThan, LessThanOrEqual.</span></span> |
| <span data-ttu-id="a016e-165">Prahová hodnota</span><span class="sxs-lookup"><span data-stu-id="a016e-165">threshold</span></span>         | <span data-ttu-id="a016e-166">Tato hodnota je hello hodnotu, která aktivuje hello akce škálování.</span><span class="sxs-lookup"><span data-stu-id="a016e-166">This value is hello value that triggers hello scale action.</span></span> <span data-ttu-id="a016e-167">Být jisti tooprovide dostatečná rozdíl mezi hello prahové hodnoty pro hello **Škálováním na více systémů** a **škálování v** akce.</span><span class="sxs-lookup"><span data-stu-id="a016e-167">Be sure tooprovide a sufficient difference between hello threshold values for hello **scale-out** and **scale-in** actions.</span></span> <span data-ttu-id="a016e-168">Pokud jste nastavili hello stejné hodnoty pro obě akce, připravená na hello systému konstantní změna, která zabraňuje v jeho implementaci akce škálování.</span><span class="sxs-lookup"><span data-stu-id="a016e-168">If you set hello same values for both actions, hello system anticipates constant change, which prevents it from implementing a scaling action.</span></span> <span data-ttu-id="a016e-169">Například nebude fungovat nastavení obou too600 vláken v předchozím příkladu hello.</span><span class="sxs-lookup"><span data-stu-id="a016e-169">For example, setting both too600 threads in hello preceding example doesn't work.</span></span> |
| <span data-ttu-id="a016e-170">Směr</span><span class="sxs-lookup"><span data-stu-id="a016e-170">direction</span></span>         | <span data-ttu-id="a016e-171">Tato hodnota určuje hello akce, která se provede, když je dosaženo hello prahovou hodnotu.</span><span class="sxs-lookup"><span data-stu-id="a016e-171">This value determines hello action that is taken when hello threshold value is achieved.</span></span> <span data-ttu-id="a016e-172">Hello možné hodnoty jsou zvýšení nebo snížení.</span><span class="sxs-lookup"><span data-stu-id="a016e-172">hello possible values are Increase or Decrease.</span></span> |
| <span data-ttu-id="a016e-173">type</span><span class="sxs-lookup"><span data-stu-id="a016e-173">type</span></span>              | <span data-ttu-id="a016e-174">Tato hodnota je hello typ akce, který má vzniknout a musí být nastaven tooChangeCount.</span><span class="sxs-lookup"><span data-stu-id="a016e-174">This value is hello type of action that should occur and must be set tooChangeCount.</span></span> |
| <span data-ttu-id="a016e-175">hodnota</span><span class="sxs-lookup"><span data-stu-id="a016e-175">value</span></span>             | <span data-ttu-id="a016e-176">Tato hodnota je hello počet virtuálních počítačů, které jsou přidány tooor odebrána z hello škálovací sadu.</span><span class="sxs-lookup"><span data-stu-id="a016e-176">This value is hello number of virtual machines that are added tooor removed from hello scale set.</span></span> <span data-ttu-id="a016e-177">Tato hodnota musí být 1 nebo vyšší.</span><span class="sxs-lookup"><span data-stu-id="a016e-177">This value must be 1 or greater.</span></span> |
| <span data-ttu-id="a016e-178">cooldown</span><span class="sxs-lookup"><span data-stu-id="a016e-178">cooldown</span></span>          | <span data-ttu-id="a016e-179">Tato hodnota je hello množství toowait času od poslední akce škálování hello předtím, než dojde k hello další akce.</span><span class="sxs-lookup"><span data-stu-id="a016e-179">This value is hello amount of time toowait since hello last scaling action before hello next action occurs.</span></span> <span data-ttu-id="a016e-180">Tato hodnota musí být mezi minutu a jeden týden.</span><span class="sxs-lookup"><span data-stu-id="a016e-180">This value must be between one minute and one week.</span></span> |

<span data-ttu-id="a016e-181">V závislosti na hello čítače výkonu který používáte, některé hello elementů v konfiguraci šablony hello fungují jinak.</span><span class="sxs-lookup"><span data-stu-id="a016e-181">Depending on hello performance counter, you are using, some of hello elements in hello template configuration are used differently.</span></span> <span data-ttu-id="a016e-182">V předchozím příkladu hello čítače výkonu hello je počet vláken, hello prahová hodnota je 650 pro akci škálování a hello prahová hodnota je 550 pro hello škálování v akci.</span><span class="sxs-lookup"><span data-stu-id="a016e-182">In hello preceding example, hello performance counter is Thread Count, hello threshold value is 650 for a scale-out action, and hello threshold value is 550 for hello scale-in action.</span></span> <span data-ttu-id="a016e-183">Pokud používáte čítače, jako je % času procesoru, hello prahová hodnota nastavená toohello procento využití procesoru, který určuje škálování akce.</span><span class="sxs-lookup"><span data-stu-id="a016e-183">If you use a counter such as %Processor Time, hello threshold value is set toohello percentage of CPU usage that determines a scaling action.</span></span>

<span data-ttu-id="a016e-184">Když se aktivuje škálování akce, jako je například vysoké zatížení, je vyšší kapacita hello sady hello podle hello hodnotu v šabloně hello.</span><span class="sxs-lookup"><span data-stu-id="a016e-184">When a scaling action is triggered, such as a high load, hello capacity of hello set is increased based on hello value in hello template.</span></span> <span data-ttu-id="a016e-185">Například v škálování nastavit kde kapacity hello je nastavený too3 a hodnotu akce škálování hello too1:</span><span class="sxs-lookup"><span data-stu-id="a016e-185">For example, in a scale set where hello capacity is set too3 and hello scale action value is set too1:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerBefore.png)

<span data-ttu-id="a016e-186">Když hello aktuální zatížení příčiny hello vlákno průměrný počet toogo nad prahovou hodnotou hello 650:</span><span class="sxs-lookup"><span data-stu-id="a016e-186">When hello current load causes hello average thread count toogo above hello threshold of 650:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ThreadCountAfter.png)

<span data-ttu-id="a016e-187">A **Škálováním na více systémů** akce se aktivuje, že příčiny hello kapacitu toobe sadu hello zvýšenou o hodnotu jedna:</span><span class="sxs-lookup"><span data-stu-id="a016e-187">A **scale-out** action is triggered that causes hello capacity of hello set toobe increased by one:</span></span>

```json
"sku": {
  "name": "Standard_A0",
  "tier": "Standard",
  "capacity": 4
},
```

<span data-ttu-id="a016e-188">Hello výsledkem je, že virtuální počítač se přidá toohello škálovací sadu:</span><span class="sxs-lookup"><span data-stu-id="a016e-188">hello result is a virtual machine is added toohello scale set:</span></span>

![](./media/virtual-machine-scale-sets-autoscale-overview/ResourceExplorerAfter.png)

<span data-ttu-id="a016e-189">Po dobu cooldown pět minut je-li hello průměrný počet vláken na počítačích hello více než 600 jiný počítač přidání toohello sady.</span><span class="sxs-lookup"><span data-stu-id="a016e-189">After a cooldown period of five minutes, if hello average number of threads on hello machines stays over 600, another machine is added toohello set.</span></span> <span data-ttu-id="a016e-190">Je-li počet vláken průměrná hello níže 550, kapacity hello škálovací sady hello se sníží o jedna a počítač je odebrán ze sady hello.</span><span class="sxs-lookup"><span data-stu-id="a016e-190">If hello average thread count stays below 550, hello capacity of hello scale set is reduced by one and a machine is removed from hello set.</span></span>

## <a name="set-up-scaling-using-azure-powershell"></a><span data-ttu-id="a016e-191">Nastavení škálování pomocí Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="a016e-191">Set up scaling using Azure PowerShell</span></span>

<span data-ttu-id="a016e-192">Příklady toosee pomocí prostředí PowerShell tooset až automatické škálování, podívejte se na [Azure PowerShell monitorování rychlý start ukázky](../monitoring-and-diagnostics/insights-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="a016e-192">toosee examples of using PowerShell tooset up autoscaling, look at [Azure Monitor PowerShell quick start samples](../monitoring-and-diagnostics/insights-powershell-samples.md).</span></span>

## <a name="set-up-scaling-using-azure-cli"></a><span data-ttu-id="a016e-193">Nastavení škálování pomocí rozhraní příkazového řádku Azure</span><span class="sxs-lookup"><span data-stu-id="a016e-193">Set up scaling using Azure CLI</span></span>

<span data-ttu-id="a016e-194">toosee příklady použití Azure CLI tooset až automatické škálování, podívejte se na [Azure monitorování napříč platformami CLI rychlý start ukázky](../monitoring-and-diagnostics/insights-cli-samples.md).</span><span class="sxs-lookup"><span data-stu-id="a016e-194">toosee examples of using Azure CLI tooset up autoscaling, look at [Azure Monitor Cross-platform CLI quick start samples](../monitoring-and-diagnostics/insights-cli-samples.md).</span></span>

## <a name="set-up-scaling-using-hello-azure-portal"></a><span data-ttu-id="a016e-195">Nastavení škálování pomocí hello portálu Azure</span><span class="sxs-lookup"><span data-stu-id="a016e-195">Set up scaling using hello Azure portal</span></span>

<span data-ttu-id="a016e-196">Příklad použití toosee hello Azure portálu tooset až automatické škálování, vyhledejte v [vytvoření sady škálování virtuálního počítače pomocí portálu Azure hello](virtual-machine-scale-sets-portal-create.md).</span><span class="sxs-lookup"><span data-stu-id="a016e-196">toosee an example of using hello Azure portal tooset up autoscaling, look at [Create a Virtual Machine Scale Set using hello Azure portal](virtual-machine-scale-sets-portal-create.md).</span></span>

## <a name="investigate-scaling-actions"></a><span data-ttu-id="a016e-197">Prozkoumat škálování akce</span><span class="sxs-lookup"><span data-stu-id="a016e-197">Investigate scaling actions</span></span>

* <span data-ttu-id="a016e-198">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="a016e-198">**Azure portal**</span></span>  
<span data-ttu-id="a016e-199">Teď můžete získat omezené množství informací pomocí portálu hello.</span><span class="sxs-lookup"><span data-stu-id="a016e-199">You can currently get a limited amount of information using hello portal.</span></span>

* <span data-ttu-id="a016e-200">**Průzkumník prostředků Azure**</span><span class="sxs-lookup"><span data-stu-id="a016e-200">**Azure Resource Explorer**</span></span>  
<span data-ttu-id="a016e-201">Tento nástroj je nejlepší pro zkoumání hello aktuální stav škálovací sady hello.</span><span class="sxs-lookup"><span data-stu-id="a016e-201">This tool is hello best for exploring hello current state of your scale set.</span></span> <span data-ttu-id="a016e-202">Postupujte podle této cestě a měli byste vidět sadu zobrazení instance hello hello měřítka, kterou jste vytvořili:</span><span class="sxs-lookup"><span data-stu-id="a016e-202">Follow this path and you should see hello instance view of hello scale set that you created:</span></span>  
<span data-ttu-id="a016e-203">**Odběry > {vaše předplatné} > Skupinyprostředků > {vaší skupiny prostředků} > poskytovatelé > Microsoft.Compute > virtualMachineScaleSets > {škálovací sadu} > virtuálních počítačů**</span><span class="sxs-lookup"><span data-stu-id="a016e-203">**Subscriptions > {your subscription} > resourceGroups > {your resource group} > providers > Microsoft.Compute > virtualMachineScaleSets > {your scale set} > virtualMachines**</span></span>

* <span data-ttu-id="a016e-204">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="a016e-204">**Azure PowerShell**</span></span>  
<span data-ttu-id="a016e-205">Použijte tento příkaz tooget některé informace:</span><span class="sxs-lookup"><span data-stu-id="a016e-205">Use this command tooget some information:</span></span>

  ```powershell
  Get-AzureRmResource -name vmsstest1 -ResourceGroupName vmsstestrg1 -ResourceType Microsoft.Compute/virtualMachineScaleSets -ApiVersion 2015-06-15
  Get-Autoscalesetting -ResourceGroup rainvmss -DetailedOutput
  ```

* <span data-ttu-id="a016e-206">Připojte toohello jumpbox virtuálního počítače stejně, jako by všechny ostatní počítače a pak můžete vzdáleně přistupovat hello virtuálních počítačů v hello škálování sadu toomonitor jednotlivých procesů.</span><span class="sxs-lookup"><span data-stu-id="a016e-206">Connect toohello jumpbox virtual machine just like you would any other machine and then you can remotely access hello virtual machines in hello scale set toomonitor individual processes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a016e-207">Další kroky</span><span class="sxs-lookup"><span data-stu-id="a016e-207">Next Steps</span></span>
* <span data-ttu-id="a016e-208">Podívejte se na [automaticky škálovat počítače ve Škálovací sadě virtuálního počítače](virtual-machine-scale-sets-windows-autoscale.md) toosee příklad jak toocreate škálování nastavit s nakonfigurovat automatické škálování.</span><span class="sxs-lookup"><span data-stu-id="a016e-208">Look at [Automatically scale machines in a Virtual Machine Scale Set](virtual-machine-scale-sets-windows-autoscale.md) toosee an example of how toocreate a scale set with automatic scaling configured.</span></span>

* <span data-ttu-id="a016e-209">Najít příklady Azure monitorování monitorování funkcí v [Azure PowerShell monitorování rychlý start ukázky](../monitoring-and-diagnostics/insights-powershell-samples.md)</span><span class="sxs-lookup"><span data-stu-id="a016e-209">Find examples of Azure Monitor monitoring features in [Azure Monitor PowerShell quick start samples](../monitoring-and-diagnostics/insights-powershell-samples.md)</span></span>

* <span data-ttu-id="a016e-210">Další informace o funkcích oznámení v [použití automatického škálování akce toosend e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).</span><span class="sxs-lookup"><span data-stu-id="a016e-210">Learn about notification features in [Use autoscale actions toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md).</span></span>

* <span data-ttu-id="a016e-211">Další informace o tom příliš[protokoly auditu použití toosend e-mailu a webhooku oznámení výstrah v monitorování Azure](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span><span class="sxs-lookup"><span data-stu-id="a016e-211">Learn about how too[Use audit logs toosend email and webhook alert notifications in Azure Monitor](../monitoring-and-diagnostics/insights-auditlog-to-webhook-email.md)</span></span>

* <span data-ttu-id="a016e-212">Další informace o [pokročilé scénáře škálování](virtual-machine-scale-sets-advanced-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="a016e-212">Learn about [advanced autoscale scenarios](virtual-machine-scale-sets-advanced-autoscale.md).</span></span>
